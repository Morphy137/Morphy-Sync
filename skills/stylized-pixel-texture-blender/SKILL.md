---
name: stylized-pixel-texture-blender
description: Create, unwrap, bake, and procedurally paint Low-Res Pixel-Art Hybrid textures (128x128 or 256x256 with Closest / Nearest Neighbor interpolation) and stepped cell-shaded palettes for Stylized Faceted Low-Poly / Mid-Poly 3D characters in Blender using Python (bpy/numpy) or MCP tools. Use when the user asks to texture, UV unwrap, paint skin/clothes/shoes, or generate pixel-art texture atlases for stylized 3D characters, supporting faceless mannequins by default and optional pixel-art facial features when requested.
---
# Stylized Pixel Texture Blender

Generate, UV-unwrap, and procedurally paint Low-Res Pixel-Art Hybrid texture atlases (`128x128` or `256x256` px) with `Closest` (Nearest Neighbor) filtering for Stylized Faceted Low-Poly and Mid-Poly 3D characters in Blender (`bpy`).

## When to Use

- Texturing a Stylized Faceted humanoid base mesh or clothed character in Blender via Python scripts or MCP (`execute_blender_code`, `look`).
- Setting up pixel-grid-aligned UV unwraps and `Principled BSDF` matte shaders with `Closest` texture interpolation.
- Painting stepped cell-shaded skin tones (ambient occlusion + joint blush), clothing seams, pockets, and sneaker laces directly into an image buffer (`bpy.data.images`).
- Optionally painting pixel-art facial features (eyes, eyebrows, glasses, mouth, blush) ONLY when explicitly requested by the user or when the target mesh includes facial detail.

## Core Texturing Rules

1. **Resolution & Nearest-Neighbor Filtering (`Closest`)**
   - Default atlas size: `128x128` pixels for single characters or base meshes; `256x256` pixels for characters with multiple detailed garments.
   - Every `ShaderNodeTexImage` node MUST set `node.interpolation = 'Closest'` so texels render as crisp square pixels without bilinear blurring.
   - Material surface settings (Blender 4.0 / 5.x+): `Principled BSDF` with `Roughness = 0.95` to `1.0` and `Specular IOR Level = 0.0` (or `Specular = 0.0` in older versions).

2. **Zone-Based Rectangular UV Layout (Pixel Grid Alignment)**
   - Avoid chaotic angled UV islands that cause jagged diagonal pixel stair-stepping on straight garment edges.
   - Partition the `128x128` (or `256x256`) atlas into clean rectangular zones and map mesh faces (via planar/cylinder projections or per-face UV assignment aligned to pixel coordinates `u = x / width`, `v = y / height`):
     - **Zone A - Skin & Body (Top-Left)**: Base skin ramp (4 stepped tones: highlight, midtone, warm joint blush for knees/elbows/ears, and dark desaturated AO shadow for neck/under-chin/groin/armpits).
     - **Zone B - Upper Clothing / Outerwear (Top-Right)**: Base fabric color, stepped fold shadows, 1px seam lines, collar/lapel shading, and pocket outlines/buttons.
     - **Zone C - Lower Clothing & Footwear (Bottom-Left)**: Pants tone with knee/ankle fold bands; high-top sneaker canvas, white rubber sole border, white toe cap, and 1px criss-cross lace pattern.
     - **Zone D - Hair & Optional Face (Bottom-Right)**: Dark hair gradient strips (root-to-tip stepped bands) and **Optional Face Canvas** (left blank/skin-colored by default).

3. **Faceless Default vs. Optional Facial Features**
   - **Default Mode (Faceless Mannequin)**: Fill the facial UV region with the clean base skin tone and subtle under-chin/jawline shadow only. Do NOT paint eyes, eyebrows, nose, or mouth unless asked.
   - **Optional Face Mode (Only when explicitly requested by user)**: Paint crisp pixel-art eyes (sclera, iris cluster, 1-2px white specular highlight, dark upper lash line), eyebrows, dark eye-bag pixels (for tired/depressed themes), and subtle lip/nose-shadow pixels on the dedicated Face UV zone.

## Multi-Phase MCP Texturing Workflow

When executing via Blender MCP (`execute_blender_code` + `look`), follow these steps sequentially:

1. **Phase 1 - Atlas Generation & Shader Setup**:
   - Create the `bpy.data.images.new(name="Char_Pixel_Atlas", width=256, height=256, alpha=False)` image in memory.
   - Write a Python pixel-drawing helper that fills the rectangular atlas zones with stepped color ramps, 1px garment seams, pocket borders, and sneaker laces, then pack the image (`image.pack()`) so it persists inside the `.blend` file.
   - Create the material, assign the `ShaderNodeTexImage` with `interpolation = 'Closest'`, and set the 3D Viewport to `MATERIAL` or `SOLID` (Texture color) mode.
2. **Phase 2 - UV Unwrapping & Pixel Snapping**:
   - Assign UV coordinates to each body part/material zone using `bmesh` UV layers (`bm.loops.layers.uv.verify()`), aligning straight edges (cuffs, soles, jacket opening, knees) horizontally/vertically to the pixel grid.
   - Snap UV coordinates to pixel centers or corners: `u = round(u * res) / res`.
3. **Phase 3 - Viewport Inspection (`look`) & Refinement**:
   - Call `look` from Front, 3/4, Side, and Back views to verify that pixel density (texel density) is uniform across torso, limbs, and shoes, and that seams/laces align cleanly with the 3D geometry.

## Reusable Python Template: Procedural Pixel Atlas & UV Mapper

```python
import bpy
import bmesh

def create_pixel_atlas(name="Character_Pixel_Atlas", size=256, include_face=False):
    img = bpy.data.images.get(name)
    if img:
        bpy.data.images.remove(img)
    img = bpy.data.images.new(name=name, width=size, height=size, alpha=True)

    # Initialize flat RGBA pixel buffer (size * size * 4)
    pixels = [0.15, 0.15, 0.17, 1.0] * (size * size)

    def set_rect(x0, y0, x1, y1, rgba):
        for y in range(max(0, y0), min(size, y1)):
            for x in range(max(0, x0), min(size, x1)):
                idx = (y * size + x) * 4
                pixels[idx:idx+4] = rgba

    # 1. Skin Zone (Top-Left: 0..128, 128..256) - Stepped tones
    set_rect(0, 128, 128, 256, (0.82, 0.71, 0.64, 1.0))   # Base skin
    set_rect(0, 128, 128, 148, (0.64, 0.52, 0.47, 1.0))   # Neck / AO shadow band
    set_rect(0, 148, 128, 164, (0.80, 0.62, 0.58, 1.0))   # Knee / elbow warm blush band

    # 2. Jacket & Shirt Zone (Top-Right: 128..256, 128..256)
    set_rect(128, 128, 256, 200, (0.31, 0.42, 0.53, 1.0)) # Denim base
    set_rect(128, 180, 256, 182, (0.22, 0.30, 0.39, 1.0)) # 2px seam line
    set_rect(128, 200, 256, 256, (0.38, 0.39, 0.41, 1.0)) # Gray undershirt

    # 3. Pants & High-Top Sneakers Zone (Bottom-Left: 0..128, 0..128)
    set_rect(0, 64, 128, 128, (0.14, 0.15, 0.17, 1.0))    # Dark pants
    set_rect(0, 0, 128, 64, (0.12, 0.12, 0.14, 1.0))      # Sneaker canvas
    set_rect(0, 0, 128, 12, (0.85, 0.85, 0.85, 1.0))      # White rubber sole
    # Criss-cross pixel laces
    for y in range(20, 52, 6):
        set_rect(48, y, 80, y+2, (0.88, 0.88, 0.88, 1.0))

    # 4. Hair & Optional Face Zone (Bottom-Right: 128..256, 0..128)
    set_rect(128, 0, 256, 64, (0.13, 0.14, 0.16, 1.0))    # Dark hair base
    set_rect(128, 64, 256, 128, (0.82, 0.71, 0.64, 1.0))  # Face shield (Faceless default)

    if include_face:
        # Optional pixel eyes + tired eye bags
        set_rect(156, 92, 176, 96, (0.48, 0.42, 0.46, 1.0))  # Left eye bag
        set_rect(208, 92, 228, 96, (0.48, 0.42, 0.46, 1.0))  # Right eye bag
        set_rect(160, 96, 172, 106, (0.92, 0.92, 0.95, 1.0)) # Left sclera
        set_rect(212, 96, 224, 106, (0.92, 0.92, 0.95, 1.0)) # Right sclera
        set_rect(164, 98, 170, 104, (0.20, 0.22, 0.28, 1.0)) # Left pupil
        set_rect(216, 98, 222, 104, (0.20, 0.22, 0.28, 1.0)) # Right pupil

    img.pixels = pixels
    img.update()
    img.pack()
    return img

def create_closest_pixel_material(mat_name, image):
    mat = bpy.data.materials.get(mat_name) or bpy.data.materials.new(name=mat_name)
    mat.use_nodes = True
    nodes = mat.node_tree.nodes
    links = mat.node_tree.links
    nodes.clear()

    out = nodes.new("ShaderNodeOutputMaterial")
    bsdf = nodes.new("ShaderNodeBsdfPrincipled")
    tex = nodes.new("ShaderNodeTexImage")
    tex.image = image
    tex.interpolation = 'Closest'  # CRITICAL: Nearest Neighbor pixel-art filter

    bsdf.inputs["Roughness"].default_value = 0.98
    if "Specular IOR Level" in bsdf.inputs:
        bsdf.inputs["Specular IOR Level"].default_value = 0.0
    elif "Specular" in bsdf.inputs:
        bsdf.inputs["Specular"].default_value = 0.0

    links.new(tex.outputs["Color"], bsdf.inputs["Base Color"])
    links.new(bsdf.outputs["BSDF"], out.inputs["Surface"])
    return mat
```

## Gotchas

- **Always Pack Generated Images (`img.pack()`)**: In Blender, images created via `bpy.data.images.new()` exist only in RAM and will vanish on file reload unless packed or saved to disk.
- **Keep Face Blank by Default**: Never set `include_face=True` or paint facial features unless the user's prompt explicitly asks for eyes, glasses, or a face.

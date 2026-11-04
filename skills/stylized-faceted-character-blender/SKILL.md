---
name: stylized-faceted-character-blender
description: Create, audit, and refine Stylized Mid-Poly, Faceted Low-Poly (Synty/Ashen style), and Low-Poly Pixel-Art Hybrid 3D characters in Blender using Python (bpy/bmesh) or MCP tools, including anatomically proportioned humanoid base bodies built from scratch. Use when the user asks to model, modify, review, or generate scripts/prompts for stylized low-poly/mid-poly 3D characters or humanoid mannequins with flat shading, pelvis and body anatomy, layered clothing, faceless mannequin heads, chunk hair, or pixel-art nearest-neighbor textures.
---
# Stylized Faceted Character Blender

Build and refine Stylized Mid-Poly and Faceted Low-Poly 3D characters in Blender that combine clean, organic silhouettes with crisp flat-shaded polygonal facets, physical clothing layers, and matte or pixel-art textures. A base body is about 1,300 to 1,800 triangles; a clothed character with hair is 3,000 to 6,000+.

## When to Use

- Modeling a humanoid base body from scratch. Follow [Build a Humanoid Body from Scratch](#build-a-humanoid-body-from-scratch).
- Generating `bpy` / `bmesh` Python scripts to create or modify stylized 3D characters in Blender.
- Controlling a Blender MCP instance (`execute_blender_code`, `look`, `get_scene_info`) to model or audit characters.
- Converting overly boxy retro PS1 meshes (<1,000 tris) into high-fidelity Stylized Mid-Poly / Faceted Low-Poly models.
- Setting up materials for flat-shaded palettes or pixel-art nearest-neighbor (`Closest`) textures.

## Core Aesthetic & Technical Standards

1. **Polygon Budget & Silhouette**
   - **Base body: 1,300 to 1,800 triangles.** Add clothing and hair on top for a 3,000 to 6,000+ total. Do not raise the body's density to gain detail: extra faces made the approved look worse.
   - Do not restrict meshes to retro console limits (<1,000 tris) unless explicitly requested.
   - Use mostly quads (about 95%). Triangulate at export, not while modelling: diagonals make the surface look busy.
   - Use sufficient geometry to form anatomical outer silhouettes while keeping every face flat-shaded (`shade_flat`).
   - **Cross-section vertex counts for a base body**:
     - Arms: 8 vertices per loop.
     - Legs: 8 to 10 vertices per loop.
     - Neck: 8 vertices per loop.
     - Torso and pelvis: 12 to 16 vertices per full loop.
     - Head/cranium: 8 to 12 vertices per loop with defined jaw and chin planes. Less is smoother here.

2. **Faceted Flat Shading**
   - Enforce `use_smooth = False` on all mesh polygons or apply `bpy.ops.object.shade_flat()`.
   - Avoid unapplied Subdivision Surface modifiers that melt crisp polygonal planes. If using Subdivision for initial organic volume, apply it at Level 1 and use Planar/Collapse Decimate or manual loop cleanup to restore distinct facets.

3. **Layered Clothing Geometry (No Painted-On Clothes)**
   - Model base layers (e.g., undershirt, ankles) first.
   - Construct outer garments (e.g., denim jackets, hoodies, collars, cuffs, pockets) as separate geometric shells or extruded manifold layers with real thickness (1.5 cm to 2.5 cm).
   - Define structural details geometrically: folded lapels, collar stand, rolled cuffs, breast pockets, and angular fabric folds at elbows, knees, and ankles.

4. **Head & Hair Construction**
   - **Faceless Mannequin Style (Default when no facial features are requested)**: Create a smooth, continuous front facial shield with distinct angular planes for the jawline, chin, and temples, omitting eye sockets, mouth crevices, and nose protrusions unless specified. Ears are optional: leave them off unless requested, because crude ears look worse than none.
   - **Detailed Pixel/Stylized Face (When requested)**: Use flat polygonal planes for the nose bridge and eye sockets, pairing with floating geometric eyewear (e.g., octagonal/round frames offset 1 cm from the face) and pixel-art texture mapping for eyes.
   - **Chunk Hair**: Never extrude hair directly from the scalp as a single boxy helmet. Build hair from 15 to 35 overlapping, curved pyramidal/tapered 3D strands (4 to 6 sides per strand, converging to a sharp tip) arranged in layered clusters (fringe, sideburns, crown, nape).

5. **Footwear & Hands**
   - **Hands**: Model 5-finger faceted hands: four fingers of three segments with a natural curl, a shorter separate thumb, and a palm with knuckle planes (avoid flat 4-vertex paddles and stick-like thumbs). Use segmented mitts only when the budget demands it.
   - **Shoes**: Separate sole border, toe cap, high-top ankle collar, and tongue geometry.

6. **Materials & Visual Effects (Blender 4.0 / 5.x+ Compatible)**
   - **Matte Stylized Shading**: Set `Principled BSDF` with `Roughness = 0.9` to `1.0` and `Specular IOR Level = 0.0` (or `Specular = 0.0` in older versions) so flat facets read clearly under directional lighting without plastic highlights.
   - **Pixel-Art Hybrid Textures**: When using pixel-art textures, set the Image Texture node interpolation to `'Closest'` (`Nearest Neighbor`) to preserve crisp pixel edges.

## Build a Humanoid Body from Scratch

Read [references/humanoid-anatomy.md](references/humanoid-anatomy.md) before placing the first vertex. It has the proportions, the pelvis construction, and the mistakes to avoid. Check every step against those numbers, not by feel.

Model half the body on +X with a Mirror modifier (X axis, clipping and merge on), feet at z = 0. Work in this order, and look at front, side, back and three-quarter views after each step:

1. **Block the landmarks.** Set the head, shoulder, waist, hip, crotch, knee and ankle heights first.
2. **Torso.** Build rings from the neck base down to the waist and hip.
3. **Pelvis.** Build it as its own block. Do this before the legs, following the pelvis rules below.
4. **Legs and feet.** Grow them from the thigh-root rings.
5. **Arms and hands.** Open a socket in the rib cage and grow the shoulder out of it, in a relaxed A-pose.
6. **Head.** Keep it simple and connect it through the neck.
7. **Clean up.** Merge helper diagonals back into quads, set all faces flat, and confirm no non-manifold edges or holes off the mirror axis.

**The pelvis is where bodies go wrong.** These rules are mandatory:

- Never hang the legs from the torso's last ring. That produces a skirt with tubes under it.
- Give each thigh its own complete ring, tilted so it runs from the crotch up to the outer hip.
- Between the abdomen and the crotch, use a V panel that narrows to a small point between the legs.
- Do not end the crotch on a single flat vertex fan or a wide square arch.

## Workflow for Auditing & Upgrading Existing Models (via MCP or Script)

When asked to review or upgrade an existing model in Blender:

1. **Inspect Scene & Topology**:
   - Query object names, triangle counts, vertex counts, and modifiers.
   - If MCP viewport capture (`look`) is available, capture Front, 3/4, Side, and Close-up views in Solid/Material mode with ` Cavity` (Ridge/Valley) enabled to clearly see polygon edges.
2. **Identify Boxy/Primitive Artifacts**:
   - Flag 4-to-6-sided cylindrical limbs, flat torso boxes lacking lapel/pocket thickness, helmet-like hair, or facial features that contradict the target reference.
3. **Execute Iterative Upgrades**:
   - **Step A (Head & Hair)**: Replace primitive heads with a faceted mannequin cranium and layered pyramidal hair chunks.
   - **Step B (Torso & Outerwear)**: Rebuild or refine the jacket/shirt with explicit collar, lapel, pocket, and cuff thickness.
   - **Step C (Limbs, Hands & Footwear)**: Increase loop resolution on pants for angular knee/ankle folds; refine hands and high-top sneakers.
   - **Step D (Shading & Viewport FX)**: Apply flat shading, matte materials, and enable Viewport Cavity (`type = 'BOTH'`) for crisp edge definition.

## Reusable BMesh Snippet: Flat Shading & Blender 5.x Matte Material

```python
import bpy

def setup_stylized_material(name, rgba):
    mat = bpy.data.materials.get(name) or bpy.data.materials.new(name=name)
    mat.use_nodes = True
    bsdf = mat.node_tree.nodes.get("Principled BSDF")
    if bsdf:
        bsdf.inputs["Base Color"].default_value = rgba
        bsdf.inputs["Roughness"].default_value = 0.95
        if "Specular IOR Level" in bsdf.inputs:
            bsdf.inputs["Specular IOR Level"].default_value = 0.0
        elif "Specular" in bsdf.inputs:
            bsdf.inputs["Specular"].default_value = 0.0
    return mat

def enforce_faceted_flat(obj):
    if obj.type == 'MESH':
        for poly in obj.data.polygons:
            poly.use_smooth = False
        obj.data.update()
```

## Gotchas

- **Blender 4.0+ / 5.x API Changes**: `Principled BSDF` renamed `"Specular"` to `"Specular IOR Level"`, and `mesh.use_auto_smooth` was removed in Blender 4.1+. Always check input names dynamically and set `poly.use_smooth = False` directly on polygons.
- **Avoid Single-Turn Monolithic Scripts**: When driving Blender via MCP, never rewrite the entire character in one massive script. Upgrade and verify one body region at a time using viewport captures.
- **Misleading Review Images**: A hole in a close-up is usually camera clipping; count boundary edges off the mirror axis before treating it as real. Limbs crossing the torso in a side view are usually perspective; test real intersections with a BVH overlap. Dark backs in Material preview come from its lighting, not the mesh.
- **Stale BMesh References**: Vertex indices change after deleting and adding, because freed slots get reused. Keep vertices in a list made before deleting, and do not use a set of BMesh wrappers to find new vertices after `subdivide_edges`; find them by position.
- **Licensed References**: Measure a reference model only if the user approves its licence, and never add its file or vertices to a skill or public repository.

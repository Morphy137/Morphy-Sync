# Humanoid Anatomy for Faceted Base Bodies

Proportions from the user-approved male base body: 1.756 m tall, in a relaxed A-pose, modelled on +X with a mirror. Values are rounded targets, in metres. Scale them all by `target height / 1.756`.

Axes: X is half-width from the centre line, Y is depth with the front negative, and Z is height from the floor. This body stands with a slight lean: the pelvis sits forward of the ankles, so depth values drift toward +Y going down the legs. Keep that drift instead of stacking everything on one vertical line.

## Landmarks

About 7.4 heads tall.

| Landmark | Height z | % of height | Half-width x | Front y | Back y |
|---|---|---|---|---|---|
| Top of head | 1.756 | 100 | - | - | - |
| Head, widest | 1.65 | 94 | 0.082 | -0.121 | 0.090 |
| Chin | 1.52 | 87 | - | -0.122 | - |
| Neck | 1.51 | 86 | 0.062 | -0.072 | 0.067 |
| Clavicle line | 1.43 | 81 | 0.185 | -0.064 | 0.128 |
| Chest | 1.30 | 74 | 0.187 | -0.108 | 0.140 |
| Under the pectorals | 1.25 | 71 | 0.170 | -0.111 | 0.125 |
| Waist, narrowest | 1.07 | 61 | 0.134 | -0.104 | 0.084 |
| Hip, widest | 0.87 | 50 | 0.180 | -0.094 | 0.140 |
| Crotch | 0.775 | 44 | 0 | - | - |

The arms reach a half-width of 0.254 at the shoulder, so shoulders are about 3 head-widths across. Shoulder-to-waist ratio is about 1.9 measured to the deltoid and 1.4 at the torso. A moderate V comes from this: a torso ratio near 2.0 was rejected as exaggerated.

## Legs

One leg, from its inner to its outer edge, measured from the centre line.

| Level | Height z | Inner x | Outer x | Front y | Back y |
|---|---|---|---|---|---|
| Upper thigh | 0.70 | 0.018 | 0.183 | -0.074 | 0.111 |
| Mid thigh | 0.60 | 0.040 | 0.179 | -0.058 | 0.107 |
| Above knee | 0.50 | 0.068 | 0.174 | -0.020 | 0.130 |
| Knee | 0.47 | 0.077 | 0.181 | -0.009 | 0.140 |
| Calf, fullest | 0.33 | 0.070 | 0.197 | 0.035 | 0.158 |
| Lower calf | 0.20 | 0.101 | 0.199 | 0.060 | 0.154 |
| Ankle | 0.10 | 0.125 | 0.198 | 0.070 | 0.142 |

- The legs open slightly toward the ankles. Do not make them straight parallel tubes.
- Thighs nearly touch just below the crotch and separate downward.
- Knees are narrower than the thigh and calf. Calves bulge at the back.
- Foot: about 0.24 long and 0.09 wide, flat on z = 0, toes pointing forward with a slight outward turn.

## Pelvis Construction

This is the hardest area, and it took several rejected attempts. Build it in this structure, with about 36 faces on the modelled half, all quads.

**Front**

- Lower abdomen ring at about z = 0.95, with the centre line at y = -0.097.
- V panel rim, from the top outer corner down to the crotch: `(0.067, -0.086, 0.905)`, `(0.050, -0.084, 0.856)`, `(0.027, -0.069, 0.819)`.
- Centre line of the panel: `(0, -0.096, 0.892)`, `(0, -0.092, 0.847)`, `(0, -0.078, 0.813)`.
- Crotch point, set back between the legs: `(0, 0.022, 0.775)`. This makes a small dark triangle from the front, not a square arch.
- Groin crease, running diagonally from the outer hip down to the panel rim: `(0.180, 0.013, 0.869)`, `(0.153, -0.033, 0.862)`, `(0.084, -0.085, 0.832)`, `(0.027, -0.069, 0.819)`.

**Side**

- The hip is the widest point of the lower body at about x = 0.18, z = 0.87.
- The line from hip to thigh must flow without a corner, shelf, or diamond-shaped facets.

**Back**

- Buttock peak at about `(0.058, 0.145, 0.897)`.
- Gluteal fold under the buttock: `(0.017, 0.130, 0.826)`, `(0.067, 0.131, 0.819)`, `(0.136, 0.096, 0.818)`.
- Vertical cleft near the centre: `x` about 0.006 and `y` about 0.125 from z = 0.83 to 0.96, with the centre line recessed about 1.2 to 2.5 cm behind it.

## Arms and Hands

| Part | Position or size |
|---|---|
| Shoulder pivot | about `(0.225, 0.06, 1.34)` |
| Elbow | about `(0.315, 0.05, 1.12)` |
| Wrist | about `(0.413, -0.025, 0.91)` |
| Fingertips | z = 0.75, about crotch level |
| Upper arm diameter | 9 to 11 cm |
| Forearm diameter, fullest | 8 to 9 cm |
| Wrist | 4 cm thick, 7 cm wide |

- The arm hangs about 25° from vertical. Keep a gap between arm and body.
- The shoulder grows out of the torso through a socket. The deltoid is a rounded cap that blends into the trapezius.
- The upper arm should not be much thicker than the forearm.
- Palm faces the thigh with the thumb forward. The thumb is short and close to the palm.

## Head and Neck

- Width 0.164, depth about 0.21, height from chin to crown about 0.236.
- The head sits forward of the neck, but stays upright and looks ahead. Tilting it down reads as hunched.
- Sharp jaw plane under the chin, narrowing toward the neck.
- Neck about 12 cm wide and 14 cm deep, rising to the skull behind the jaw.

## Triangle Budget

Full body with the mirror, 1,358 triangles and 694 faces (96% quads).

| Region | Triangles |
|---|---|
| Head and neck | 80 |
| Torso and pelvis | 374 |
| Arms | 232 |
| Hands | 324 |
| Legs | 164 |
| Feet | 184 |

## Adapting the Body

- **Female build:** at the same height, use a shoulder half-width near 0.18, a much narrower waist near 0.08, a hip near 0.17, and a higher crotch near z = 0.83.
- **Slimmer character:** reduce chest and hip width and arm diameters first, and keep the height landmarks.
- **More stylized V:** widen the chest and narrow the waist together, and move the arm with the shoulder. Keep the torso ratio below 2.0 unless requested.

## Rejected Results

Each of these was tried and rejected by the user. Do not repeat them.

| Result | Fix |
|---|---|
| Boxy PS1-style shapes | Use organic rings with the proportions above |
| Legs attached straight to the torso | Build the pelvis block with thigh-root rings |
| Wide horizontal band across the pelvis | Use a V panel narrowing to a point |
| Square or arch gap between the thighs | Bring the thighs together under a small crotch triangle |
| Corners or diamond facets on the hip | Keep the hip-to-thigh line continuous |
| Many diagonals on the surface | Merge into quads |
| Dense head and face | Keep head and neck under about 240 triangles |
| Thick upper arms with a shoulder stuck on | Blend the deltoid into the torso |
| Exaggerated V torso | Keep a moderate V |
| Hunched posture with the head dropped | Keep the head up over the shoulders |

## Review Checklist

Before showing a body to the user:

- [ ] Heights match the landmark table within about 2 cm, after scaling.
- [ ] From the front: V panel, small crotch triangle, and thighs nearly touching at the top.
- [ ] From the side: chest, waist and buttock curves, with the head upright.
- [ ] From the back: gluteal fold and cleft read clearly.
- [ ] From the three-quarter view: the shoulder flows from torso to arm.
- [ ] About 95% quads, no ngons, no non-manifold edges, and no holes off the mirror axis.
- [ ] All faces flat, feet at z = 0, and the triangle count within budget.

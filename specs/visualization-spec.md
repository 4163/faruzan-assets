# Presentation and visualization specification: Faruzan

Project: [`workspace/genshin-impact-faruzan`](workspace/genshin-impact-faruzan)  
Client target: Genshin Impact v7.1.0  
Entity: Faruzan (`AvatarId: 10000076`)  
Stage: Stage 4 Presentation and visualization specification

---

## 1. Specification scope and architectural objectives

This document defines the technical visualization blueprint for all 728 Category B compound assets established during Stage 3. It provides structural specifications for geometry, bone hierarchies, material bindings, cel-shading parameters, 2D skeletal puppets, and recommended runtime engines.

Standalone media (790 audio clips, 138 graphics, 4 text registries) operate independently in [`assets/audio/`](assets/audio), [`assets/textures/`](assets/textures), and [`assets/text/`](assets/text). In contrast, the compound assets detailed below depend on specific engine scene graphs, shader pipelines, or animation controllers to render correctly.

---

## 2. Compound asset clusters and technical bindings

### Cluster 1: Faruzan 3D character skeletal rig, meshes, and level of detail

The primary character assembly is located in [`assets/Animator/Avatar_Girl_Bow_Faruzan.fbx`](assets/Animator/Avatar_Girl_Bow_Faruzan.fbx) and [`assets/-2021872655/Avatar_Girl_Bow_Faruzan.fbx`](assets/-2021872655/Avatar_Girl_Bow_Faruzan.fbx).

#### Mesh structure
- Base body mesh: `Body` contains the torso, legs, arms, and footwear geometry.
- Facial and head mesh: `Face` contains facial surfaces. `Face_Eye` contains eye planes and expression blend shapes.
- Hair and accessories: Hair strands, twin pigtails, and ribbon bands.
- Decimation tiers: LOD0 for close-up inspection, LOD1 for field exploration, and LOD2 for distant rendering.
- Effect mesh: `EffectMesh` handles local transparency cards and boundary geometry.

#### Skeletal hierarchy
The model uses an 87-node bone rig mapped to Unity Humanoid standards:
- Root and spine chain: `Bip001` -> `Pelvis` -> `Spine` -> `Spine1` -> `Spine2` -> `Neck` -> `Head`.
- Upper limbs: `Clavicle` -> `UpperArm` -> `Forearm` -> `Hand` -> Finger bones (`Finger0` through `Finger4`).
- Lower limbs: `Thigh` -> `Calf` -> `Foot` -> `Toe0Nub`.
- Secondary and dynamic bones:
  - Hair ribbons and twin tails: `A42`, `A82`, `B01`, `B04` drive spring oscillations.
  - Accessory attachments: `PRIVATE_Pendant`, `PRIVATE_RHand`.
  - Camera tracking anchors: `CameraLook`, `MoveHead`, `FocusAnchor`, and `HitObject` locators.

---

### Cluster 2: Character 3D surface textures, lightmaps, and ramps

Surface definitions in [`assets/Texture2D/`](assets/Texture2D) require specialized multi-channel decoding:

#### Diffuse textures
- [`Avatar_Girl_Bow_Faruzan_Tex_Body_Diffuse.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Body_Diffuse.png): Base color map for clothing and skin.
- [`Avatar_Girl_Bow_Faruzan_Tex_Hair_Diffuse.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Hair_Diffuse.png): Base color map for teal hair.
- [`Avatar_Girl_Bow_Faruzan_Tex_Face_Diffuse.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Face_Diffuse.png): Base skin tone, lips, and facial details.

#### Multi-channel lightmaps and tone maps
The lightmap files pack auxiliary shading data into separate RGBA color channels:
- Red channel: Ambient occlusion and fixed shadow areas.
- Green channel: Specular intensity and gloss exponent.
- Blue channel: Metallic property mask and material zone identifiers.
- Alpha channel: Outline thickness scale or emissive boundary mask.

#### Stepped shadow ramps
- [`Avatar_Girl_Bow_Faruzan_Tex_Body_Shadow_Ramp.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Body_Shadow_Ramp.png)
- [`Avatar_Girl_Bow_Faruzan_Tex_Hair_Shadow_Ramp.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Hair_Shadow_Ramp.png)

These textures map the half-Lambert dot product to stepped color gradients. The vertical coordinate selects the material zone from the lightmap blue channel, while the horizontal coordinate indexes shadow transitions.

---

### Cluster 3: Character animation library

The animation collection comprises 514 `.anim` clips in [`assets/AnimationClip/`](assets/AnimationClip) and virtual hash folders:

- Locomotion tracks: Walk, jog, sprint, turn, jump, fall, plunge, roll, swim, and climb.
- Combat stances and attacks: Normal attack four-stage archery combo, charged shot draw, release, and recovery.
- Skill execution: Cast animations for *Wind Realm of Nasamjnin* (`Ani_Avatar_Girl_Bow_Faruzan_Channel01AS.anim`) and looping stances (`Ani_Avatar_Girl_Bow_Faruzan_Channel01Loop.anim`).
- Burst activation: Polyhedron deployment (`Ani_SkillObj_Faruzan_Pyramid_Born_Forward.anim`) and camera track bindings.
- Facial and narrative poses: Akimbo stances, sitting poses, thinking loops, and Hangout series cutscene motions.

Each clip targets the `Bip001` bone hierarchy or morph targets on the `Face_Eye` mesh.

---

### Cluster 4: Material graphs and shader parameter bindings

The material definitions in [`assets/Material/`](assets/Material) govern surface rendering:

#### Dress material: [`Avatar_Girl_Bow_Faruzan_Mat_Dress.json`](assets/Material/Avatar_Girl_Bow_Faruzan_Mat_Dress.json)
- `_MainTex` binds to [`Avatar_Girl_Bow_Faruzan_Tex_Body_Diffuse.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Body_Diffuse.png).
- `_LightMapTex` binds to body lightmap data.
- `_PackedShadowRampTex` binds to [`Avatar_Girl_Bow_Faruzan_Tex_Body_Shadow_Ramp.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Body_Shadow_Ramp.png).
- `_MTMap` binds to shared tone map data.
- Culling configuration: Double-sided rendering enabled on skirt hems.

#### Face material: [`Avatar_Girl_Bow_Faruzan_Mat_Face.json`](assets/Material/Avatar_Girl_Bow_Faruzan_Mat_Face.json)
- `_MainTex` binds to [`Avatar_Girl_Bow_Faruzan_Tex_Face_Diffuse.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Face_Diffuse.png).
- `_FaceMapTex` binds to face signed distance field (SDF) textures for smooth shadow line rotation.
- `_LightMapTex` supplies mouth and eye boundary masks.

#### Hair material: [`Avatar_Girl_Bow_Faruzan_Mat_Hair.json`](assets/Material/Avatar_Girl_Bow_Faruzan_Mat_Hair.json)
- `_MainTex` binds to [`Avatar_Girl_Bow_Faruzan_Tex_Hair_Diffuse.png`](assets/Texture2D/Avatar_Girl_Bow_Faruzan_Tex_Hair_Diffuse.png).
- `_NormalPackageMap` supplies anisotropic tangent normals for hair highlights.
- `_MTSpecularRamp` defines hair specular bands.
- `_TextureLinePackageMap` controls outline width variations.

---

### Cluster 5: Dynamic bone physics and cloth simulation

Physics configurations are stored in [`assets/TextAsset/`](assets/TextAsset) and [`assets/MonoBehaviour/`](assets/MonoBehaviour):
- Cloth meshes: Skirt cloth planes and sleeve ribbons calculate inertia using mass-spring particles.
- Collision shapes: Upper and lower thigh capsule colliders prevent skirt geometry from clipping through character limbs.
- Damping and stiffness: Hair spring bones use tuned angular limits to simulate gentle swaying while preventing excessive deformation during sprint turns.

---

### Cluster 6: Combat VFX polyhedron assembly

Faruzan's Elemental Burst (*The Wind's Secret Ways*) deploys a moving geometric polyhedron:
- Geometry files: [`assets/-350308942/SkillObj_Faruzan_Pyramid_Model.obj`](assets/-350308942/SkillObj_Faruzan_Pyramid_Model.obj), `SkillObj_Faruzan_Pyramid_Core_Model.obj`, and `SkillObj_Faruzan_Pyramid_Edge_Model.obj`.
- Materials: [`assets/Material/SkillObj_Faruzan_Pyramid03_Mat.json`](assets/Material/SkillObj_Faruzan_Pyramid03_Mat.json) and [`assets/Material/SkillObj_Faruzan_Pyramid_Edge_Mat.json`](assets/Material/SkillObj_Faruzan_Pyramid_Edge_Mat.json).
- Textures: Shading requires `_BaseTex`, `_NoiseTex`, and `_DissolveTex` for the energy pulse animations.
- Motion: Loops along triangular flight paths, expanding scale periodically on pulse releases.

---

### Cluster 7: Prop: Faruzan Chibi Fungus Doll Furnishing (Puzzlepal)

The Serenitea Pot furnishing item (ID 374636) includes geometry and textures:
- Visible mesh: [`assets/Mesh/Beyd_Object_Prop_RenOu_01_Vo.obj`](assets/Mesh/Beyd_Object_Prop_RenOu_01_Vo.obj) (1,830 lines).
- Collision boundary: [`assets/Mesh/Beyd_Object_Prop_RenOu_01_Col.obj`](assets/Mesh/Beyd_Object_Prop_RenOu_01_Col.obj).
- Material configuration: [`assets/Material/Beyd_Object_Prop_RenOu_01.json`](assets/Material/Beyd_Object_Prop_RenOu_01.json).
- Surface textures:
  - Diffuse: [`assets/Texture2D/Beyd_Object_Prop_RenOu_01_Diffuse.png`](assets/Texture2D/Beyd_Object_Prop_RenOu_01_Diffuse.png).
  - Normal map: [`assets/Texture2D/Beyd_Object_Prop_RenOu_01_Normal.png`](assets/Texture2D/Beyd_Object_Prop_RenOu_01_Normal.png).
  - Multi-mask: [`assets/Texture2D/Beyd_Object_Prop_RenOu_01_SMBE.png`](assets/Texture2D/Beyd_Object_Prop_RenOu_01_SMBE.png) (Specular, Metallic, Bump, Emission).

---

### Cluster 8: Prop: Version 3.6 Omni-Directional Astrolabe Furnishing

The Akademiya Extravaganza astrolabe assembly comprises:
- Geometry files: [`assets/Mesh/Homeworld_Interior_Xm_Decoration_Prop_XyjLuopan_01_Lod0.obj`](assets/Mesh/Homeworld_Interior_Xm_Decoration_Prop_XyjLuopan_01_Lod0.obj), `Lod1.obj`, and `Col.obj`.
- Skybox projection: [`assets/Mesh/Eff_Codex_Skybox_AkaFesAstrolabe_01_Vo.obj`](assets/Mesh/Eff_Codex_Skybox_AkaFesAstrolabe_01_Vo.obj).
- Textures: Sumeru brass surface shaders and skybox projection maps in `assets/Texture2D/`.

---

### Cluster 9: Prop: Archaeological radar detector probes

The Version 3.6 Haravatat minigame exploration probes include:
- Detection nodes: [`assets/Mesh/Property_Ani_Prop_AkaFesArchaeologyDetector_01_01_Vo.obj`](assets/Mesh/Property_Ani_Prop_AkaFesArchaeologyDetector_01_01_Vo.obj), `02_Vo.obj`, and `03_Vo.obj`.
- Checkpoint geometry: `Property_Ani_Prop_AkaFesArcCheckPoint_01_Col.obj` and LOD tiers.
- Surface textures: Diffuse, normal, and signal emissive pulse masks in `assets/Texture2D/`.

---

### Cluster 10: Prop: Haravatat Darshan uniform hat and scarf models

Academic attire accessories include:
- Hat geometry: [`assets/Mesh/NPC_ItemDynamic_HaravatatHat001_Model.obj`](assets/Mesh/NPC_ItemDynamic_HaravatatHat001_Model.obj) and LOD1.
- Scarf geometry: [`assets/Mesh/Body008_Standard_HaravatatScarf001.obj`](assets/Mesh/Body008_Standard_HaravatatScarf001.obj) and LOD1.
- Materials: [`assets/Material/NPC_ItemDynamic_HaravatatHat001_Mat.json`](assets/Material/NPC_ItemDynamic_HaravatatHat001_Mat.json) and [`assets/Material/NPC_ItemDynamic_HaravatatScarf001_MaleBody008_Mat.json`](assets/Material/NPC_ItemDynamic_HaravatatScarf001_MaleBody008_Mat.json).
- Cloth parameters: `assets/TextAsset/NPC_ItemDynamic_HaravatatHat001_01_Maozi01.cloth.txt` and `NPC_Male_Body008_Standard_HaravatatScarf001_Daizi01.cloth`.

---

### Cluster 11: Prop: Companion ruin machine (TMR-0001)

The companion ruin automaton model from Faruzan's character quest contains:
- Mesh and skeleton rig: [`assets/-1809554292/AvatarObj_Ani_Quest_FaruzanRobot_01.fbx`](assets/-1809554292/AvatarObj_Ani_Quest_FaruzanRobot_01.fbx).
- Materials: `assets/-1124695584/AvatarObj_Ani_Quest_FaruzanRobot_Mat.json`.
- Textures: Diffuse and lightmap textures in virtual hash folders `-1895050433/` and `-1686003093/`.
- Animations: `Ani_Quest_FaruzanRobot_01_Default.anim`, `Ani_Quest_FaruzanRobot_01_Sit.anim`, `Ani_Quest_FaruzanRobot_01_DropDown.anim`, and `Ani_Quest_FaruzanRobot_01_Disappear.anim`.

---

### Cluster 12: 2D Spine skeletal puppet: Genius Invokation TCG dynamic card

The dynamic character card uses Spine 2D skeletal animation:
- Binary skeleton files:
  - Base skeleton: [`assets/-1573907974/Gcg_CardFace_Char_Avatar_Faruzan_Spine.skel`](assets/-1573907974/Gcg_CardFace_Char_Avatar_Faruzan_Spine.skel) (Verified header version: Spine 4.0.47).
  - Sub skeleton: [`assets/-888158333/Gcg_CardFace_Char_Avatar_Faruzan_Sub_Spine.skel`](assets/-888158333/Gcg_CardFace_Char_Avatar_Faruzan_Sub_Spine.skel) (Verified header version: Spine 4.0.47).
- Atlas definitions: `assets/-1695415305/Gcg_CardFace_Char_Avatar_Faruzan_Spine_Atlas.json` and `assets/-1440570577/Gcg_CardFace_Char_Avatar_Faruzan_Sub_Spine_Atlas.json`.
- Visual components:
  - Base card face: [`assets/textures/tcg_cards/Gcg_CardFace_Char_Avatar_Faruzan_Spine_Bg.png`](assets/textures/tcg_cards/Gcg_CardFace_Char_Avatar_Faruzan_Spine_Bg.png).
  - Skeletal sprite sheet: [`assets/textures/tcg_cards/Gcg_CardFace_Char_Avatar_Faruzan_Spine.png`](assets/textures/tcg_cards/Gcg_CardFace_Char_Avatar_Faruzan_Spine.png).
- Animation tracks: `Faruzan_Spine_Idle.anim`, `Faruzan_Spine_Attack.anim`, `Faruzan_Spine_Bg_Idle.anim`, and `Faruzan_Spine_Bg_Attack.anim`.

---

### Cluster 13: Animator controllers and minigame state models

State control graphs govern interactive transitions:
- Character animator controllers: State transitions between idle, locomotion, skill casting, and bow attack strings.
- Minigame state models: State transitions for event minigames.

---

## 3. Recommended runtime and rendering environments

To render these compound assets accurately, runtimes must match the original engine formats:

### 3.1 3D Character and props (Clusters 1 through 11)

#### Option 1: Unity (2020.3 LTS or 2021.3 LTS)
- Direct drop-in compatibility for FBX geometry, Humanoid bone mappings, and `.anim` clip curves.
- Cel-shading implementation requires a custom Shader Graph reproducing:
  1. Lightmap decoding: Extract AO from Red, roughness from Green, and material zone from Blue.
  2. Stepped ramp lookup: Sample `_PackedShadowRampTex` using half-Lambert diffuse lighting and material zone coordinates.
  3. Face SDF shading: Evaluate light direction relative to head forward/right vectors against `_FaceMapTex` thresholds.
  4. Outline rendering: Add a second pass with vertex normal inverted hull extrusion tinted by diffuse tones.

#### Option 2: Godot Engine 4.x
- Import FBX models directly with automatic Humanoid retargeting.
- Implement a VisualShader with:
  1. Custom light function taking light vector dot product with transformed vertex normals.
  2. Texture sampler reading stepped ramp curves.
  3. Outline pass using next-pass material with front-face culling and grow offset.

#### Option 3: Web-based 3D (Three.js or Babylon.js)
- Convert FBX models to glTF/GLB with bone hierarchies intact.
- Apply a custom `RawShaderMaterial` or `NodeMaterial` implementing the stepped ramp lookup and inverted hull outline pass.
- WebGL 2.0 or WebGPU allows interactive inspection directly in web browsers.

---

### 3.2 2D Spine skeletal card (Cluster 12)

#### Recommended runtime: Spine Web Player or Spine-Pixi (Spine 4.0 runtime)
- Skeleton files require Spine runtime version 4.0.x matching binary header 4.0.47.
- Render stack architecture:
  1. Base background: Draw `Gcg_CardFace_Char_Avatar_Faruzan_Spine_Bg.png` to canvas.
  2. Spine skeleton player: Load `Gcg_CardFace_Char_Avatar_Faruzan_Spine.skel` with texture atlas sheet, playing the loop track.
  3. Foreground overlay: Draw card frame [`UI_Gcg_CardFace_Char_Avatar_Faruzan.png`](assets/textures/tcg_cards/UI_Gcg_CardFace_Char_Avatar_Faruzan.png) and skill status icons.

---

### 3.3 Audio integration (Standalone Category A)

#### Recommended audio player: Web Audio API with vgmstream decoding
- Convert extracted Wwise `.wem` files to `.wav` or `.ogg` via `vgmstream` for universal web playback.
- Route audio through category buses matching the 12 dialog domains documented in [`specs/mapping.md`](specs/mapping.md).

---

### 3.4 Transform-dependent standalone graphics: Genius Invokation TCG card face

Standard non-dynamic card face textures in `assets/textures/tcg_cards/` (such as `Gcg_CardFace_Char_Avatar_Faruzan_Layer00.png`) are stored in $1024 \times 1024$ power-of-two square textures with overscan margins for engine parallax. Viewing them raw displays a $1.5\times$ horizontal stretch.

#### Bounding box and aspect parameters
- Texture dimensions: $1024 \times 1024$ RGBA
- Active illustration rectangle: $x \in [158, 865]$ (width $707$ px), $y \in [108, 916]$ (height $808$ px)
- Overscan margins: $158$ px left and right ($\Delta u \approx 0.1544$), $108$ px top and bottom ($\Delta v \approx 0.1055$)
- Target display aspect ratio: $7:12$ ($420 \times 720$ px, matching `UI_Gcg_CardFace_Char_Avatar_Faruzan.png`)

#### 2D viewport reconstruction function

```python
from PIL import Image

def reconstruct_card_face(layer00_path, target_width=420, target_height=720):
    img = Image.open(layer00_path)
    # Crop out overscan margins
    cropped = img.crop((158, 108, 865, 916))  # 707 x 808
    # Correct 1.5x horizontal stretch to 7:12 card aspect ratio
    card_face = cropped.resize((target_width, target_height), Image.Resampling.LANCZOS)
    return card_face
```

#### In-engine UV mapping formula

The 3D card mesh quad has physical aspect ratio $7:12$. The vertex stage passes normalized UV coordinates across the active face:

$$\text{UV}_{\text{base}} = \begin{bmatrix} 0.1544 + \text{uv.x} \cdot 0.6903 \\ 0.1055 + \text{uv.y} \cdot 0.7890 \end{bmatrix}$$

$$\text{UV}_{\text{sampled}} = \text{UV}_{\text{base}} + \mathbf{V}_{xy} \cdot \text{ParallaxScale0} \cdot \text{ParallaxMultiplier}$$

Where $\mathbf{V}_{xy}$ is the tangent-space view direction, and $\text{ParallaxScale0} = 0.2$.

---

### 3.5 Specialized standalone graphic handling

A systematic audit across all 138 standalone texture files in `assets/textures/` identified the following presentation handling rules:

#### 1. Namecard dual-layer presentation
- **Profile card background:** `UI_NameCardPic_Faruzan_P.png` ($840 \times 400$, aspect ratio $2.1:1$). Rendered as the full-width background plate for user profile headers.
- **Chat and signature ribbon:** `UI_NameCardPic_Faruzan_Alpha.png` ($898 \times 140$, aspect ratio $6.41:1$). Not a grayscale alpha mask; rendered as a standalone horizontal banner strip across chat headers, multiplayer lobbies, and signature docks.

#### 2. Multi-layer 2.5D TCG parallax cards
- **Talent card:** `Gcg_CardFace_Modify_Talent_Faruzan_Layer00` through `Layer02` ($1024 \times 1024$).
- **Summon card:** `Gcg_CardFace_Summon_Faruzan_Layer00` through `Layer02` ($1024 \times 1024$).
- **Stacking order:** Layer 00 (background), Layer 01 (midground subject cutout), Layer 02 (foreground VFX overlay).
- **Parallax scales:** Layer 00 uses `_ParallaxScale0: 0.2`, Layer 01 uses `_ParallaxScale1: 0.1`, Layer 02 uses `_ParallaxScale2: 0.025`.
- **2D representation:** For flat 2D display, use the pre-composited UI sprites (`UI_Gcg_CardFace_Modify_Talent_Faruzan.png`, `UI_Gcg_CardFace_Summon_Faruzan.png`), or crop the active frame $[148, 102, 868, 919]$ and resample to $7:12$.

#### 3. Sprite atlas containers
- `SpriteAtlasTexture-ui_sprite_activity_akafes_reasoning-1024x1024-fmt25.png` and `512x512-fmt25.png`: Raw Unity SpriteAtlas sheets. All individual icons (`UI_Frm_AkaFes_Reasoning_*`, `UI_Icon_AkaFes_Reasoning_Bufficon.png`) are already cleanly unpacked into discrete PNG files in `assets/textures/event_ui_and_guides/`.

#### 4. UI shader flow and distortion maps
- `Eff_UI_AkaFesMainPage_StageYellow_UV02.png` ($512 \times 256$), `UV06.png` ($256 \times 512$), and `UVBall.png` ($256 \times 256$).
- Non-color vector maps: Red and Green channels drive tangent UV displacement and scrolling distortion in event UI shaders.

---

## 4. Standalone viewer repository blueprint

An independent viewer application for this project can be structured as follows:

```text
faruzan-asset-viewer/
├── public/
│   ├── assets/
│   │   ├── models/           # Converted glTF/GLB from Cluster 1, 6, 7, 8, 9, 10, 11
│   │   ├── spine/            # Spine 4.0 .skel and atlas files from Cluster 12
│   │   ├── textures/         # Category A graphics and 3D surface maps
│   │   └── audio/            # Decoded audio tracks across 12 dialog domains
│   └── data/
│       ├── transcripts.json  # Audio transcripts from faruzan_voiceline_transcripts.json
│       └── profile.json      # Lore and parameters from faruzan_profile_and_lore.json
├── src/
│   ├── components/
│   │   ├── ModelViewer3D/    # Three.js / WebGL cel-shaded character viewer
│   │   ├── SpineCardViewer/  # Spine 4.0 dynamic card player
│   │   ├── AudioJukebox/     # 12-domain voiceover soundboard
│   │   └── LoreReader/       # Character stories and event dialog browser
│   └── shaders/
│       ├── celShading.vert   # Inverted hull outline and bone skinning vertex shader
│       └── celShading.frag   # Stepped shadow ramp and lightmap fragment shader
├── package.json
└── vite.config.js
```

This structure consumes all categorized assets directly without altering the underlying extraction provenance.

---

## 5. Summary and checkpoint

- All 13 compound asset clusters are documented with exact internal paths, bone hierarchies, material bindings, and textures.
- The Spine skeletal card files are confirmed at Spine 4.0.47 binary format.
- Accurate runtime requirements have been defined for Unity, Godot, Three.js, and Spine Web Player.
- This specification provides the complete technical foundation for any downstream visualization implementation.

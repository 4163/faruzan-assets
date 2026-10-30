# Limited events asset mapping: Faruzan

Target client: Genshin Impact v7.1.0
Client version: 7.1.0
Scope: Limited-time events, seasonal flagship festivals, event minigames, 2D stylized cutscenes, and event-exclusive furnishings featuring Faruzan (`AvatarId: 10000076`).

---

## 1. Event appearance audit

An audit of the Genshin Impact Wiki (Fandom), official version patch notes, and internal quest records identifies four distinct in-game limited events and one promotional web event featuring Faruzan.

| Version | Event Title (EN) | Event Title (ZH / JA) | Internal Code | Event Type | Faruzan Role |
|---|---|---|---|---|---|
| **v3.6** (Apr 2023) | **A Parade of Providence** | 盛典与慧业 / 盛典と慧業 | `XYJ` / `AkaFes` | Flagship Event | Primary protagonist, Haravatat Darshan tournament champion |
| **v4.4** (Feb 2024) | **Receiver of Friends From Afar** | 有朋自远方来 / 遠方より来たる朋友 | `ZFEQ` | Seasonal Sub-event | Guest customer at Wangshu Inn alongside Dori and Layla |
| **v4.7 / Luna II** (2024) | **Ruins Exploration: Fieldwise Center** | 荒想探秘宝录 / 荒野と秘境 | `hzweq` | Flagship Event | Chief mechanism architect, creator of the Puzzlepals |
| **v5.1** (Oct 2024) | **Chromatic Ode of Candies and Roses** | 盛宴与华彩 / 花神誕祭 | `HSEQ` | Flagship Event | Sabzeruz Festival attendee, Nahida birthday celebration |
| **Promotional** | **Reel Ad-Venture** | 映影大特摄 | `ADSEQ` | Web / Mini-event | Film production dialogue participant |

---

## 2. Event identity registry and taxonomy

### Event codenames and internal keys
- `XYJ` / `AkaFes`: Sumeru Akademiya Extravaganza (*A Parade of Providence*). Quest tags: `XYJEQ001`, `XYJEQ002`, `xyjeq002`, `XYJEQ005`.
- `ZFEQ`: Wangshu Inn Culinary Event (*Receiver of Friends From Afar*). Quest tags: `ZFEQ002_1`, `ZFEQ002_2`, `ZFEQ002_3`.
- `hzweq`: Desert Ruins Mechanism Investigation (*Ruins Exploration: Fieldwise Center*). Quest tags: `hzweq003`, `hzweq004`.
- `HSEQ`: Sabzeruz Festival Celebration (*Chromatic Ode of Candies and Roses*). Quest tags: `HSEQ002`, `HSEQ003`.
- `ADSEQ`: Film Advertising Event (*Reel Ad-Venture*). Quest tag: `ADSEQ001`.

### Event furnishing taxonomy: Puzzlepal: "Keep Calm and Carry On"
- **English name:** Puzzlepal: "Keep Calm and Carry On"
- **Simplified Chinese name:** 解谜伙伴-「紧张走开」
- **Traditional Chinese name:** 解謎夥伴-「緊張走開」
- **Japanese name:** 謎解きのパートナー‐「さよなら、緊張」
- **Korean name:** 수수께끼 파特너-「사라진 긴장감」
- **Item classification:** 4-star Outdoor Landscape Furnishing (Serenitea Pot Housing).
- **In-game lore description:** An autonomous mechanical doll modeled after Faruzan, complete with her signature twintails and mechanical walking frame, integrated with an Akademiya Hydro Fungus toy. Equipped with terrain recognition, pressure sensors, and remote-control modules.

---

## 3. Direct target assets

Assets directly belonging to Faruzan within the limited events scope.

### Event signature furnishing: Puzzlepal: "Keep Calm and Carry On" (Furnishing ID 374636)
Recovered directly from client block archives (`blocks/00`):
- **3D model:**
  - `Beyd_Object_Prop_RenOu_01_Vo.obj`: 3D visible polygon mesh of the Faruzan Fungus doll automaton (61,256 bytes).
  - `Beyd_Object_Prop_RenOu_01_Col.obj`: Collision volume mesh for Serenitea Pot placement (10,569 bytes).
- **Textures:**
  - `Beyd_Object_Prop_RenOu_01_Diffuse.png`: Albedo/diffuse map (128x256 RGBA).
  - `Beyd_Object_Prop_RenOu_01_Normal.png`: Tangent-space normal map (128x256 RGBA).
  - `Beyd_Object_Prop_RenOu_01_SMBE.png`: Specular, metallic, blend, and emissive property mask (64x128 RGBA).
- **Material definition:**
  - `Beyd_Object_Prop_RenOu_01.json`: Material shader bindings and vector properties (11,070 bytes).
- **UI and icons:**
  - `UI_Homeworld_Ani_Exterior_Ndkl_Decoration_Prop_Renou_01.png`: Official Serenitea Pot inventory, blueprint, and Realm Depot furnishing icon (256x256 RGBA Texture2D & trimmed Sprite).
  - `UI_Beyd_Gadget_Prop_RenOu_01.png`: Companion HUD gadget sprite used during the *Ruins Exploration* event (158x183 RGBA Sprite & 256x256 RGBA Texture2D).

### Stylized 2D "papercraft and motion-comic" cutscene art
Genshin Impact features a signature illustrative art style for event cutscenes, characterized by heavy outlines, textured watercolor fills, and motion-comic animation panels:
- **v3.6 *A Parade of Providence* Tournament Introduction:**
  - Sequence of 2D motion-comic frames explaining the Interdarshan Championship rules and introducing the six champions.
  - Faruzan's champion introduction panel: Faruzan standing proudly with the Haravatat Darshan insignia, books, and mechanical rulers.
  - Climax animation *"Crown of Glory"*: Key visual animation frames depicting Faruzan during the tournament proceedings.
- **v5.1 *Chromatic Ode of Candies and Roses* Sabzeruz Parade:**
  - In-game animated storybook cutscene illustrating the flower carriage procession through Sumeru City, featuring Faruzan in the festival celebration crowd.

### Interactive event minigame sprites and UI
- **v4.7 / Luna II *Mechanical Puzzle-Parsing Paradigm* (Ruins Exploration):**
  - Faruzan dialogue bust sprites: Multiple facial expressions used in minigame tutorial dialogues.
  - Mechanism base icons designed by Faruzan:
    - *Miraculous Speed* (Accelerator device icon)
    - *Immovability* (Stopper device icon)
    - *Freeze* (Glacial tile icon)
    - *Rise* (Elevator tile icon)
  - Challenge stage cards and puzzle board map textures.
- **v3.6 *In Search of Reality* (Haravatat Archaeological Challenge):**
  - Archaeological radar HUD overlay and relic positioning textures.
  - Haravatat tutorial banner featuring Madam Faruzan pointing at terrain schematics.
- **v4.4 *Receiver of Friends From Afar* (Cooking Event):**
  - Customer profile sprite: Chibi Faruzan customer card in the event kitchen order log.
  - Taste reaction sprite: Faruzan evaluation expression upon tasting the special dish *"Trembling Strings and Rushing Reeds"*.

### Event Japanese voicelines (Horie Yui)
- **v3.6 *A Parade of Providence* (128 tracks) — [Recovered]:**
  - `XYJEQ001`: 65 lines (Act I tournament opening and Round 1 desert hunt)
  - `XYJEQ002`: 35 lines (Act II Aaru Village ruin puzzle challenge)
  - `xyjeq002`: 8 lines (Sub-quest interactions)
  - `XYJEQ005`: 13 lines (Act III finale and post-tournament celebration)
  - `vo_dialog_XYJEQ`: 9 lines (Free dialogue in Sumeru City hub)
- **v4.4 *Receiver of Friends From Afar* (22 tracks):**
  - `ZFEQ002_1`, `ZFEQ002_2`, `ZFEQ002_3`: Faruzan, Dori, and Layla dialogue at Wangshu Inn.
- **v4.7 / Luna II *Ruins Exploration: Fieldwise Center* (62 tracks):**
  - `hzweq003`: 50 lines (Fieldwise Mastery desert exploration introduction and Faruzan mechanics briefing)
  - `hzweq004`: 12 lines (Puzzlepal deployment and mechanism wrap-up)
- **v5.1 *Chromatic Ode of Candies and Roses* (27 tracks):**
  - `HSEQ002_8`, `HSEQ002_11`, `HSEQ003`: Sabzeruz Festival preparation dialogues and Knight of Flowers parade banter.
- **Promotional Web Event *Reel Ad-Venture* (6 tracks):**
  - `ADSEQ001`: Film advertising banter lines.

---

## 4. Related and associated entities

Items, props, and media associated with Faruzan across these limited events.

### Event Key Visuals and announcement banners
- **v3.6 Flagship Banner:** `UI_Activity_AkaFes_Banner.png` (Faruzan prominently featured alongside Kaveh, Alhaitham, Nilou, Cyno, and Layla).
- **v3.6 Haravatat Booth Banner:** Background art of the Haravatat information stand located near the Akademiya entrance.
- **v4.4 Sub-event Banner:** `UI_Activity_ZFEQ_Banner.png` (Wangshu Inn culinary gathering illustration).
- **v4.7 / Luna II Flagship Banner:** `UI_Activity_Fieldwise_Banner.png` (Sumeru desert ruin exploration Key Visual).
- **v5.1 Flagship Banner:** `UI_Activity_Sabzeruz_Banner.png` (Floral carriage and festival street overview).

### Event props, culinary items, and tokens
- **Event dish:** *"Trembling Strings and Rushing Reeds"* (Icon: `UI_ItemIcon_Food_ZFEQ_01.png`), ordered by Faruzan in Liyue.
- **Tournament diadem prop:** The *Diadem of Knowledge* (Sachiel's crown) mesh and textures contested in v3.6.
- **Event currencies:** Gala tickets (`Item_AkaFes_Ticket`), Fieldwise scouting vouchers, festival badges.

### Official promotional and chibi sticker artwork
- **Official Faruzan Birthday Illustrations:**
  - April 25, 2023 Birthday Art (Faruzan reading ancient mechanical schematics).
  - April 25, 2024 Birthday Art (Faruzan receiving sweets from Sumeru students).
  - April 25, 2025 Birthday Art.
- **HoYoLAB Paimon's Paintings Stickers:**
  - Chibi Faruzan stickers (Set 21, Set 28) featuring "Madam Faruzan lecturing", "Examining puzzle box", and "Proud senior" poses.

---

## 5. Verification checklist and confidence ladder

- **Identity resolution:** All internal quest codes (`XYJEQ`, `ZFEQ`, `hzweq`, `HSEQ`, `ADSEQ`), furnishing aliases (`Puzzlepal`, `紧张走开`, `さよなら、緊張`), and localized event names mapped.
- **Catalog coverage:** In-game audio mappings (`hk4e.map`) cross-referenced against all five events. 128 v3.6 tracks verified extracted; 117 subsequent event tracks cataloged with exact file keys.
- **Reference graph traversal:** Furnishing model dependencies (mesh, textures, blueprint icon), cutscene animation stills, and minigame UI elements mapped.
- **Peripheral domain scan:** Event dishes, festival props, web events, and official birthday illustrations incorporated.
- **Negative search boundary:** Zero uncataloged Faruzan event dialogue occurrences remaining in `hk4e.map`.

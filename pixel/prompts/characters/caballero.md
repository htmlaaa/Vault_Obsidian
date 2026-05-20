---
title: Prompts Gemini — Caballero
personaje: Caballero
canvas: 64×64 px
vista: top-down (low top-down, ligeramente angulada)
estilo: pixel art — convertir en LibreSprite
carpeta_juego: "characters/caballero/animations/{anim}/{dir}/frame_NNN.png"
dirs: south · east · north · west  (west = mirror de east en LibreSprite)
---

> **Instrucciones de uso:**
> 1. Abre un chat nuevo en Gemini para el Caballero.
> 2. Envía los prompts **en orden**, uno por sección.
> 3. Cada prompt tras el Diseño Principal dice "using the exact character design you created" — Gemini mantendrá el contexto del chat.
> 4. Convierte cada imagen resultado a pixel art en LibreSprite (64×64 canvas, pixel art filter o traza manual).
> 5. Guarda los frames como `frame_000.png`, `frame_001.png`, etc. en la carpeta correspondiente.
> 6. West es mirror de East — usa horizontal flip en LibreSprite para ahorrar trabajo.

---

## 1 · Diseño Principal

```
Create a pixel art character reference sheet for a dark fantasy knight for a top-down roguelite game called Kaosuarina, set in the "Vacío Kaótico" (Chaotic Void) — an arena of infinite chaos where warriors are trapped and fight endless waves of enemies.

Character: The Caballero (Knight) — a heavy melee fighter.

Visual design:
- Dark iron plate armor, slightly battle-worn with small dents and scratches
- Blue-teal glowing accents on the armor edges and visor (color #00E5CC — a chaotic energy glow)
- Broad, stocky silhouette — this character is slow but tanky
- Wields a longsword in the right hand (blade pointing forward or at side when idle)
- A tower shield strapped on the left arm (slightly glowing edge)
- Closed full helmet with a narrow visor slit
- Dark cape or tabard behind the torso with subtle teal rune markings
- Boots with metal greaves

Style:
- Pixel art, clean outlines, top-down perspective (low top-down: head visible, slight overhead angle)
- 64×64 pixel canvas — character fills approximately 60% of the canvas height
- Dark fantasy palette: deep grays, charcoal blacks, muted blues, with teal accent glow
- Simple but expressive silhouette (readable at small size)

Show 4 directional standing poses in a single reference sheet:
- South (facing toward viewer — front view, most detailed)
- East (facing right — side profile)
- North (facing away — back view)
- West (facing left — mirror of east, include if possible)

This is a sprite reference for manual pixel art conversion. Clean, bold linework.
```

---

## 2 · Idle (breathing)

```
Using the exact character design you created for the Caballero knight, create a pixel art idle animation sprite sheet.

Animation: IDLE — breathing/weight shift while standing still.
Direction: South (facing toward viewer).
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Standing pose, neutral — same as the south reference pose.
- Frame 1: Slight chest rise (inhale), sword lowers 1-2px, shoulders rise 1px.
- Frame 2: Peak of breath — chest slightly puffed, shield arm relaxed.
- Frame 3: Chest falls (exhale), back to near-neutral.

Style: 64×64px per frame, same pixel art style as the reference sheet.
Note: This animation will be used for all 4 directions — generate only South; the other directions will reuse these frames or be created later.
```

> **Nota carpeta:** `characters/caballero/animations/idle/south/frame_000.png` → `frame_003.png`
> Para simplificar, puedes usar los mismos 4 frames para las 4 direcciones (north/east/west/south).

---

## 3 · Walk — Sur (South)

```
Using the exact character design you created for the Caballero knight, create a pixel art walk cycle sprite sheet.

Animation: WALK — moving forward (toward the viewer, south direction).
Direction: South.
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Left foot forward, right foot back, sword swings slightly right.
- Frame 1: Feet together, transition — character slightly shorter (compression step).
- Frame 2: Right foot forward, left foot back, sword swings slightly left.
- Frame 3: Feet together, transition — slight bounce recovery.

The knight walks with a heavy, grounded gait. Armor clinks. Shield shifts slightly with each step.
Style: 64×64px per frame, pixel art, consistent with the reference sheet.
```

> **Carpeta:** `characters/caballero/animations/walk/south/`

---

## 4 · Walk — Norte (North)

```
Using the exact character design you created for the Caballero knight, create a pixel art walk cycle sprite sheet.

Animation: WALK — moving away from the viewer (north direction, back view).
Direction: North.
Frames: 4 frames, displayed left to right.

Same walk rhythm as the south direction but seen from behind:
- The dark cape flutters slightly with each step.
- The shield is visible on the left side.
- Helmet back is visible.

Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/caballero/animations/walk/north/`

---

## 5 · Walk — Este (East) [West = mirror]

```
Using the exact character design you created for the Caballero knight, create a pixel art walk cycle sprite sheet.

Animation: WALK — moving to the right (east direction, side view).
Direction: East.
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Left leg forward, sword at side.
- Frame 1: Stride transition — both legs near center.
- Frame 2: Right leg forward, sword slightly raised.
- Frame 3: Stride transition — recovery.

The side view shows the full silhouette of the armor profile, cape behind.
Style: 64×64px per frame, pixel art.
Note: The West direction will be created by horizontally flipping these frames in LibreSprite.
```

> **Carpeta:** `characters/caballero/animations/walk/east/`
> West = flip horizontal en LibreSprite → `characters/caballero/animations/walk/west/`

---

## 6 · Ataque Ligero — Swing Espada (South)

```
Using the exact character design you created for the Caballero knight, create a pixel art sword swing animation sprite sheet.

Animation: ATTACK LIGHT — quick sword arc swing (120° frontal arc, fast strike).
Direction: South (facing viewer).
Frames: 6 frames, displayed left to right.

Frame breakdown:
- Frame 0: Wind-up — sword arm pulls back to the right, shield raised slightly.
- Frame 1: Swing begins — sword starts sweeping left in a wide arc.
- Frame 2: Mid-swing — sword crosses center, motion blur trail (3-4px streak in yellow-white #FFFDE7).
- Frame 3: Peak strike — sword fully extended left, impact flash.
- Frame 4: Follow-through — sword continues slightly past, body rotation.
- Frame 5: Recovery — sword returns toward neutral, stance stabilizes.

The swing is fast and decisive. Yellow-white energy trail follows the blade (teal accent flash on impact).
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/caballero/animations/swing/south/`
> Repite para north/east/west si quieres animaciones direccionales completas.

---

## 7 · Ataque Pesado — Swing Grande (South)

```
Using the exact character design you created for the Caballero knight, create a pixel art heavy attack animation sprite sheet.

Animation: ATTACK HEAVY — powerful wide sword sweep (200° arc, slower and more dramatic).
Direction: South (facing viewer).
Frames: 6 frames, displayed left to right.

Frame breakdown:
- Frame 0: Full wind-up — both hands on sword (two-handed grip), raised high above head.
- Frame 1: Charge pose — brief hold at raised position, teal energy glow builds on the blade.
- Frame 2: Downswing begins — sword arcing down and wide, cape blows back.
- Frame 3: Mid-arc — sword sweeping across full width, bright blue-white (#B3FFFF) energy trail.
- Frame 4: Slam — sword hits low and wide, impact shockwave ripples (2-3 pixel ring).
- Frame 5: Recovery — knight heaves the sword back, hunched forward briefly.

This attack feels HEAVY. The ground should shake (small pixel debris or dust). Teal energy explosion flash.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/caballero/animations/swing/south/` (misma carpeta que ligero, pero frames 6-11 si se combina, o carpeta separada `swing_heavy`)
> **Nota código:** El juego usa la misma carpeta `swing` para el ataque — el Caballero solo tiene 1 anim ATTACK. Elige el más representativo (ligero) o créalos como variantes para futuro.

---

## 8 · Hurt (recibir daño)

```
Using the exact character design you created for the Caballero knight, create a pixel art hurt reaction animation.

Animation: HURT — recoiling from a hit, brief stagger.
Direction: South (facing viewer). Same 2 frames work for all directions.
Frames: 2 frames.

Frame breakdown:
- Frame 0: Impact frame — knight jerked back/sideways, head tilted, slight white flash overlay on the armor.
- Frame 1: Stagger — recovering, slightly hunched, armor shows new small crack or dent (optional).

The Caballero takes hits with grit — no dramatic falling, just a brief stagger. He keeps fighting.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Hurt no está implementado en el código actual. Guardar en `characters/caballero/animations/hurt/south/` para uso futuro (Sprint 6+).

---

## 9 · Death (muerte)

```
Using the exact character design you created for the Caballero knight, create a pixel art death animation sprite sheet.

Animation: DEATH — the knight falls after being defeated.
Direction: South (facing viewer).
Frames: 5 frames, displayed left to right.

Frame breakdown:
- Frame 0: Fatal blow reaction — arms spread wide, sword drops.
- Frame 1: Beginning to fall — knees buckle, leans forward.
- Frame 2: Mid-fall — body tilting toward the ground, cape spreads.
- Frame 3: Near-ground — almost horizontal, armor pieces rattling.
- Frame 4: Final — lying on the ground, motionless. Teal glow in the armor dims and fades.

Dramatic but pixel-art simple. The silence of a fallen warrior.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Death no está implementado en código actual. Guardar en `characters/caballero/animations/death/south/` para uso futuro.

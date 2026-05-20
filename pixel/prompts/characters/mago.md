---
title: Prompts Gemini — Mago
personaje: Mago
canvas: 64×64 px
vista: top-down (low top-down, ligeramente angulada)
estilo: pixel art — convertir en LibreSprite
carpeta_juego: "characters/mago/animations/{anim}/{dir}/frame_NNN.png"
dirs: south · east · north · west  (west = mirror de east en LibreSprite)
---

> **Instrucciones de uso:**
> 1. Abre un chat nuevo en Gemini para el Mago (chat separado del Caballero).
> 2. Envía los prompts en orden. Gemini mantiene contexto del chat.
> 3. Convierte a pixel art en LibreSprite (64×64 canvas).
> 4. Frames: `frame_000.png`, `frame_001.png`, etc.
> 5. West = flip horizontal de East en LibreSprite.

---

## 1 · Diseño Principal

```
Create a pixel art character reference sheet for a dark fantasy mage for a top-down roguelite game called Kaosuarina, set in the "Vacío Kaótico" (Chaotic Void) — a collapsing realm of chaos energy where mages channel forbidden magic to survive endless enemy waves.

Character: The Mago (Mage) — a fragile but powerful arcane caster.

Visual design:
- Tattered deep-purple hooded robes with ragged edges (the fabric barely holds together — low defense)
- Chaotic purple-violet energy (#9B30FF) crackling around the hands and the hem of the robes
- Slender, slightly hunched figure — physically weak but radiating magical power
- Holds a gnarled obsidian staff with a glowing purple orb at the top (or can float at side)
- Face partially hidden by the hood — only glowing violet eyes visible in the shadow
- Floating spell runes drift slowly around the character at rest
- Bare feet or ghostly foot silhouette (the mage barely touches the ground)
- A tome of chaos strapped to the belt

Style:
- Pixel art, clean outlines, top-down perspective (low top-down: head visible, slight overhead angle)
- 64×64 pixel canvas — character fills approximately 60% canvas height
- Dark palette: deep purples, blacks, with violet-magenta energy glow
- Flowing robes should read clearly in silhouette even at 64×64

Show 4 directional standing poses in a single reference sheet:
- South (facing toward viewer — front view, most detailed)
- East (facing right)
- North (facing away — show the back of the robe and staff)
- West (facing left — mirror of east if needed)

Sprite reference for pixel art conversion. Clean, expressive silhouette.
```

---

## 2 · Idle (floating runes)

```
Using the exact character design you created for the Mago mage, create a pixel art idle animation sprite sheet.

Animation: IDLE — the mage hovers/sways gently, magical energy pulsing.
Direction: South (facing viewer).
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Neutral standing pose — staff held, a small rune orbits slowly to the left.
- Frame 1: Slight upward sway (1-2px body rise), rune moves to top position, orb glows slightly brighter.
- Frame 2: Sway continues — robes drift slightly left, rune at right position.
- Frame 3: Returns near-neutral, rune at bottom position, orb dims back to base.

The idle conveys floating magical energy — the mage is never completely still.
Style: 64×64px per frame, pixel art.
Note: These 4 frames can be reused for all 4 directions.
```

> **Carpeta:** `characters/mago/animations/idle/south/` (reusar para las 4 dirs si conviene)

---

## 3 · Walk — Sur (South)

```
Using the exact character design you created for the Mago mage, create a pixel art walk cycle sprite sheet.

Animation: WALK — gliding forward (south, toward viewer). The mage doesn't really "walk" — they glide.
Direction: South.
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Robes trail behind slightly, staff angled forward, left side of robe flares.
- Frame 1: Glide transition — body slightly lower, robes catch up.
- Frame 2: Robes trail the other direction, staff slight back-angle.
- Frame 3: Recovery — robes settle, brief energy pulse from the orb.

The mage moves with an eerie smoothness. Barely any foot movement — the robe hides the feet. A faint purple energy trail 1-2px behind.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/mago/animations/walk/south/`

---

## 4 · Walk — Norte (North)

```
Using the exact character design you created for the Mago mage, create a pixel art walk cycle sprite sheet.

Animation: WALK — gliding away (north, back view).
Direction: North.
Frames: 4 frames.

The back of the robe flutters with each glide cycle. The staff glows from behind. Hood is seen from above/behind.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/mago/animations/walk/north/`

---

## 5 · Walk — Este (East) [West = mirror]

```
Using the exact character design you created for the Mago mage, create a pixel art walk cycle sprite sheet.

Animation: WALK — gliding right (east, side view).
Direction: East.
Frames: 4 frames.

Side profile shows the full robe silhouette trailing behind. The staff is held forward or at side.
Frame rhythm: robe billows left–center–right–center as the mage glides.
Style: 64×64px per frame, pixel art.
Note: West direction = horizontal flip of these frames in LibreSprite.
```

> **Carpeta:** `characters/mago/animations/walk/east/`
> West = flip → `characters/mago/animations/walk/west/`

---

## 6 · Ataque Ligero — Bolt Mágico (South)

```
Using the exact character design you created for the Mago mage, create a pixel art magic bolt cast animation.

Animation: ATTACK LIGHT (cast) — casting a focused magic bolt toward a target. Auto-aim bolt.
Direction: South (facing viewer).
Frames: 6 frames, displayed left to right.

Frame breakdown:
- Frame 0: Wind-up — both hands raise slightly, orb on staff begins glowing brighter.
- Frame 1: Charge — a small violet energy ball forms between the mage's palms.
- Frame 2: Release begins — the bolt launches forward (bright violet streak #9B30FF → white tip).
- Frame 3: Mid-cast — the bolt is mid-air (can be shown as a streak leaving the frame), hands recoil slightly.
- Frame 4: Follow-through — staff pulses with afterglow, mage leans forward slightly.
- Frame 5: Recovery — stance returns to neutral, small residual sparks dissipate.

The bolt is fast and precise. The orb on the staff pulses white on release.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/mago/animations/cast/south/`

---

## 7 · Ataque Pesado — Explosión AoE (South)

```
Using the exact character design you created for the Mago mage, create a pixel art area explosion cast animation.

Animation: ATTACK HEAVY (cast heavy) — casting a massive chaos explosion centered on the mage.
Direction: South (facing viewer).
Frames: 6 frames, displayed left to right.

Frame breakdown:
- Frame 0: Deep focus — mage raises staff high, eyes glow brighter, runes swirl fast.
- Frame 1: Energy buildup — chaotic dark-purple aura expands from the mage's core (+4px radius each frame).
- Frame 2: Peak charge — mage's silhouette briefly outlined by blinding violet-white flash, robe blows back.
- Frame 3: EXPLOSION release — chaos burst radiates outward in all directions, large pixel spark ring.
- Frame 4: Shockwave aftermath — screen shake implied (mage jostles 1px), robes blown back fully.
- Frame 5: Exhaustion — mage slumps slightly, staff planted into the ground, aura dims (high mana cost visible in pose).

The explosion covers a wide area. Dark purple (#3D0070) ring with bright violet (#9B30FF) core flash.
Style: 64×64px per frame, pixel art.
```

> **Nota código:** El juego usa carpeta `cast` para el Mago — elige el ataque ligero como anim principal ATTACK o combínalos para el futuro.

---

## 8 · Hurt

```
Using the exact character design you created for the Mago mage, create a pixel art hurt animation.

Animation: HURT — recoiling from a hit.
Direction: South (facing viewer). Reusable for all directions.
Frames: 2 frames.

Frame breakdown:
- Frame 0: Violent recoil — mage thrown back 2-3px, robes jolt forward, staff momentarily dropped or tilted, brief flash.
- Frame 1: Recovery attempt — mage stumbles forward trying to regain balance, energy sparks around the hit point.

The mage is fragile — this hit hurts. The reaction is dramatic but brief.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Guardar en `characters/mago/animations/hurt/south/` para uso futuro.

---

## 9 · Death

```
Using the exact character design you created for the Mago mage, create a pixel art death animation.

Animation: DEATH — the mage collapses as their magic fades.
Direction: South (facing viewer).
Frames: 5 frames.

Frame breakdown:
- Frame 0: Final hit — mage's body convulses, a burst of chaotic energy explodes outward.
- Frame 1: Falling — staff clatters away, robes fold as the body begins to drop.
- Frame 2: Collapsing — body folds, the hood falls forward, the violet glow in the eyes fades.
- Frame 3: Near ground — robe crumples, dark energy fizzles in small dying sparks.
- Frame 4: Final — crumpled robe on the ground, staff lying beside, a few last embers of magic dying out.

The mage's death is both dramatic and melancholic — the magic dies with them.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Guardar en `characters/mago/animations/death/south/` para uso futuro.

---
title: Prompts Gemini — Tirador
personaje: Tirador
canvas: 64×64 px
vista: top-down (low top-down, ligeramente angulada)
estilo: pixel art — convertir en LibreSprite
carpeta_juego: "characters/shooter/animations/{anim}/{dir}/frame_NNN.png"
dirs: south · east · north · west  (west = mirror de east en LibreSprite)
---

> **Instrucciones de uso:**
> 1. Abre un chat nuevo en Gemini para el Tirador (chat separado de los otros).
> 2. Envía los prompts en orden. Gemini mantiene contexto del chat.
> 3. Convierte a pixel art en LibreSprite (64×64 canvas).
> 4. Frames: `frame_000.png`, `frame_001.png`, etc.
> 5. West = flip horizontal de East en LibreSprite.
>
> **Nota sobre auto-shoot:** El Tirador dispara automáticamente cada 0.16s.
> En el código, la animación ATTACK (shoot) se activa con cada disparo y dura ~0.5s.
> En la práctica, el Tirador está SIEMPRE en animación de disparo durante el juego.
> Por eso la animación `shoot` es la más importante. Walk es un fallback cuando
> el Tirador está completamente quieto y sin disparar.
>
> **Sobre los frames anteriores:** Los frames de shoot existentes usaban un template
> de "fireball" que hacía que se viera como lanzamiento mágico. Los nuevos frames
> deben mostrar claramente pistolas con destellos de disparo (muzzle flash).

---

## 1 · Diseño Principal

```
Create a pixel art character reference sheet for a dark fantasy gunslinger for a top-down roguelite game called Kaosuarina, set in the "Vacío Kaótico" (Chaotic Void) — a collapsing arena of chaos where gunslingers survive by speed, accuracy, and relentless firepower.

Character: The Tirador (Gunslinger/Shooter) — a fast, light-armored dual-pistol fighter.

Visual design:
- Light leather armor with golden-amber (#FFB800) accents and metal shoulder plates
- Dual pistols — one in each hand (can be holstered at sides when idle/walking)
- Sleek, athletic build — fast and nimble (highest move speed of the 3 characters)
- A wide-brim hat (slightly damaged/worn) OR a bandana/headwrap (choose consistent look)
- Fingerless gloves, belt with ammo pouches and small holsters
- A long trench coat or duster that flows behind while moving
- Boots with metal spurs
- A "combo counter" visual element: small orange/amber energy marks along the coat that glow brighter as kills accumulate (represents the Momentum de Combate relic)

Style:
- Pixel art, clean outlines, top-down perspective (low top-down)
- 64×64 pixel canvas — character fills approximately 60% canvas height
- Warm dark palette: dark browns, blacks, with amber-gold (#FFB800) energy highlights
- Silhouette should clearly show dual pistols and the hat/coat for instant recognition

Show 4 directional standing poses in a single reference sheet:
- South (facing viewer — both pistols visible at sides)
- East (facing right — profile with coat and one pistol visible)
- North (facing away — coat and hat brim from behind)
- West (facing left — mirror of east)

Sprite reference for pixel art conversion. Fast, dangerous energy in the pose.
```

---

## 2 · Idle

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art idle animation sprite sheet.

Animation: IDLE — at rest between bursts, still alert. Guns at the ready.
Direction: South (facing viewer).
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Both pistols at sides (low ready), coat settles, neutral stance.
- Frame 1: Weight shifts slightly to one side, one pistol raised slightly as if scanning.
- Frame 2: Brief spin of one pistol (showman twirl — 1-2px rotation hint), coat sways.
- Frame 3: Both pistols back at sides, stance returns to neutral, amber glow pulses once.

The Tirador is never truly idle — there's always a subtle restless energy. Think a gunslinger about to draw.
Style: 64×64px per frame, pixel art.
Note: These 4 frames can be reused for all 4 directions.
```

> **Carpeta:** `characters/shooter/animations/idle/south/`

---

## 3 · Walk — Sur (South)

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art walk cycle sprite sheet.

Animation: WALK — moving forward (south, toward viewer), pistols in low-ready position.
Direction: South.
Frames: 4 frames, displayed left to right.

Frame breakdown:
- Frame 0: Left leg forward, right pistol slightly raised, coat swings right.
- Frame 1: Feet together (transition), pistols level, brief contact with ground.
- Frame 2: Right leg forward, left pistol slightly raised, coat swings left.
- Frame 3: Feet together (transition), recovery step, coat settles momentarily.

The Tirador moves quickly and efficiently — a hunter's gait, not a soldier's march. The coat flows behind.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/shooter/animations/walk/south/`

---

## 4 · Walk — Norte (North)

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art walk cycle sprite sheet.

Animation: WALK — moving away (north, back view).
Direction: North.
Frames: 4 frames.

Back view: the long coat flows behind, hat brim visible. Pistols at sides swinging gently with each stride.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/shooter/animations/walk/north/`

---

## 5 · Walk — Este (East) [West = mirror]

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art walk cycle sprite sheet.

Animation: WALK — moving right (east, side view).
Direction: East.
Frames: 4 frames.

Side profile: the coat trails behind, one pistol forward and one behind as arms swing with the stride.
The hat brim creates a clear silhouette. Long confident strides.
Style: 64×64px per frame, pixel art.
Note: West = horizontal flip of these frames in LibreSprite.
```

> **Carpeta:** `characters/shooter/animations/walk/east/`
> West = flip → `characters/shooter/animations/walk/west/`

---

## 6 · Shoot — Disparo Automático (South) ⭐ ANIMACIÓN PRINCIPAL

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art auto-fire shooting animation sprite sheet.

Animation: SHOOT (ATTACK) — rapid dual-pistol auto-fire. This is the most important animation.
Direction: South (facing viewer — shooting toward the viewer/mouse direction).
Frames: 6 frames, displayed left to right.

IMPORTANT: This must look like GUNSHOTS with pistols, NOT magic or fire throwing.
Each shot must have a clear muzzle flash at the barrel tip.

Frame breakdown:
- Frame 0: Ready stance — both arms slightly raised, pistols aimed south, feet planted.
- Frame 1: FIRE — right pistol fires: bright yellow-white muzzle flash (#FFFFFF + #FFCC00 glow) at the barrel tip. Right arm recoils 1-2px back. Shell casing ejects (tiny yellow pixel).
- Frame 2: Recovery — right arm returns, left pistol fires: same muzzle flash on left barrel. Second shell casing.
- Frame 3: Both pistols lower slightly (brief pause between bursts), ambient shell casings still in air.
- Frame 4: Right pistol fires again — slightly different angle (the Tirador adjusts aim). Stronger muzzle flash.
- Frame 5: Recoil settle — both arms back to ready position, smoke wisps from both barrels (2-3 dark gray pixels), ready for next burst.

The amber/gold energy accents on the coat glow brighter during firing (Momentum de Combate relic building up).
Key visual rule: MUZZLE FLASH only at the gun barrel tip. No magical aura, no fireball, no orb. Just pistol fire.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/shooter/animations/shoot/south/`
> ⚠️ Reemplaza los frames anteriores que usaban template "fireball".

---

## 7 · Shoot — Norte (North)

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art shoot animation for the north direction.

Animation: SHOOT — firing away from the viewer (north/back view).
Direction: North.
Frames: 6 frames.

Back view: the character fires forward (away), muzzle flashes visible on both sides of the body as the guns extend forward. The coat billows slightly from the recoil. Shell casings fly back toward the viewer.
Same timing and energy as the south direction.
Style: 64×64px per frame, pixel art.
```

> **Carpeta:** `characters/shooter/animations/shoot/north/`

---

## 8 · Shoot — Este (East) [West = mirror]

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art shoot animation for the east direction.

Animation: SHOOT — firing to the right (east, side view).
Direction: East.
Frames: 6 frames.

Side view: one arm (right) forward firing, one arm at side or slightly behind as secondary. Profile silhouette with muzzle flash extending to the right from the front barrel. Coat blows back slightly with each shot.
Style: 64×64px per frame, pixel art.
Note: West = horizontal flip in LibreSprite.
```

> **Carpeta:** `characters/shooter/animations/shoot/east/`
> West = flip → `characters/shooter/animations/shoot/west/`

---

## 9 · Walk + Shoot Combinada (bonus — uso futuro)

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art walk-and-shoot combined animation sprite sheet.

Animation: WALK+SHOOT — the Tirador moves while firing continuously. This is a bonus animation for future use.
Direction: South.
Frames: 4 frames.

Frame breakdown:
- Frame 0: Walking stride left, right pistol mid-fire (muzzle flash), coat flowing right.
- Frame 1: Stride transition, both pistols firing simultaneously (twin muzzle flash), brief shell casings.
- Frame 2: Walking stride right, left pistol mid-fire, coat flowing left.
- Frame 3: Stride transition, right pistol fires, shells scatter.

The Tirador never stops moving OR shooting. This animation blends both without interruption.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Esta animación es para uso futuro. El código actual separa WALK y ATTACK.
> Guardar en `characters/shooter/animations/walk_shoot/south/` como referencia.

---

## 10 · Hurt

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art hurt animation.

Animation: HURT — getting hit while in a firefight.
Direction: South (facing viewer). Reusable for all directions.
Frames: 2 frames.

Frame breakdown:
- Frame 0: Impact — the character jolts sideways, hat tilts dramatically, one pistol drops slightly, amber glow flickers.
- Frame 1: Recovery — the Tirador immediately straightens, pistol snaps back up, combo counter glow dims slightly (took a hit = combo interrupted).

The Tirador is fast and resilient — the hurt is brief and they immediately return to fighting stance.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Guardar en `characters/shooter/animations/hurt/south/` para uso futuro.

---

## 11 · Death

```
Using the exact character design you created for the Tirador gunslinger, create a pixel art death animation.

Animation: DEATH — the gunslinger goes down shooting.
Direction: South (facing viewer).
Frames: 5 frames.

Frame breakdown:
- Frame 0: Last shot — one final muzzle flash as the character fires even while falling.
- Frame 1: Falling back — both pistols still raised as the body tips backward.
- Frame 2: Mid-fall — coat spreads wide, hat flies off, amber glow begins to fade.
- Frame 3: Hitting the ground — body slams, one pistol clatters away, a shell casing bounces.
- Frame 4: Final — lying on the ground, one pistol still in hand, hat nearby, all amber glow gone. Smoke rising from the barrels.

The Tirador dies like a true gunslinger — never stops firing until the very end.
Style: 64×64px per frame, pixel art.
```

> **Nota:** Guardar en `characters/shooter/animations/death/south/` para uso futuro.

---
title: Prompts PixelLab — Enemigos Kaosuarina
scope: idle + move (4 direcciones)
canvas: 64×64 px (minions) · 98×98 px (elites) · 128×128 px (jefes)
style: top-down pixel art, dark fantasy, Vacío Kaótico
nota: NO pedir aún animaciones de ataque — esas se diseñan en sprint posterior
generado: 2026-06-07
actualizado: 2026-06-07
---

# Prompts PixelLab — Sprites de Enemigos

> **Cómo usar:** Cada sección es el `description` para la tool `create_character` de PixelLab.
> Parámetros por categoría:
>
> | Categoría | `size` | Canvas aprox | `n_directions` |
> |-----------|--------|-------------|----------------|
> | Minions (BASICO…SHIELDER) | **64** | 92×92px | 4 |
> | Elites (ELITE_CHARGE, ELITE_SUMMON, ELITE_ZONE) | **98** | ~137×137px | 4 |
> | Jefes (GUARDIAN, ARQUERO, FRAGMENTADO, DEVASTADOR) | **128** | ~180×180px | 4 |
>
> Parámetros comunes: `view: "low top-down"` · `shading: "basic shading"` · `outline: "single color black outline"` · `detail: "medium detail"` · `mode: "standard"`
>
> Animaciones a pedir: `walk` (4 dirs) para todos; `idle` como segunda prioridad.

---

## 1 · BASICO — Soldado Vacío

**Rol:** minion de presión numérica, pursuer básico
**PixelLab ID (64px):** `d05ffa56-bab7-4a3e-88b9-59c9f079b1b9` ✅ generado 2026-06-07
**Animación:** `walking` template (6 frames, 4 dirs)

```
Top-down pixel art enemy for a dark fantasy roguelite called Kaosuarina (Chaotic Void).
Character: Soldado Vacío — a basic humanoid minion made of dark void-matter.

Visual design:
- Small hunched humanoid figure, roughly soldier-shaped but corrupted
- Body is deep charcoal black with dark-grey highlights on armor scraps
- Eyes: two small red/orange glowing dots, the only color on the figure
- Tattered cloth wrapping on arms, jagged crude weapon (short sword or claw)
- Minimalist silhouette — readable at 64px

Animation mood: Idle — a slow menacing sway, weight shifting side to side.
Walk — a shuffling aggressive stride toward target.

Style: 64×64px pixel art, top-down low perspective, dark palette with minimal glow.
```

---

## 2 · RAPIDO — Velocista Kaótico

**Rol:** kamikaze de alta velocidad, frágil
**PixelLab ID (64px):** `3a64091a-5572-4e30-b62e-b8314105006d` ✅ generado 2026-06-07
**Animación:** `running-4-frames` template (4 frames, 4 dirs)

```
Top-down pixel art enemy for Kaosuarina.
Character: Velocista Kaótico — a small, extremely fast void creature.

Visual design:
- Very small lean body, almost skeletal — speed over mass
- Electric purple/magenta energy trails streaking back from its limbs
- Sharp angular body, almost insectoid silhouette
- No visible weapon — the creature IS the weapon (uses its body as a ram)
- Glowing magenta eyes, frantic pose

Animation mood: Idle — twitching with energy, feet barely still, vibrating.
Walk/Run — fast scurrying movement, body leaning far forward, nearly horizontal sprint pose.

Style: 64×64px pixel art, top-down, dark with magenta/purple energy accents.
```

---

## 3 · TANQUE — Brutal Acorazado

**Rol:** tanque de presión sostenida, resistente a físico
**PixelLab ID (64px):** `75974d01-5e41-4458-bc62-3cfd0ae495a5` ✅ generado 2026-06-07
**Animación:** `crouched-walking` template (6 frames, 4 dirs)

```
Top-down pixel art enemy for Kaosuarina.
Character: Brutal Acorazado — a massive heavily armored void soldier, double the size of normal enemies.

Visual design:
- Large stocky body, wider than tall — pure bulk, clearly larger than other minions
- Thick void-metal plating covering chest and shoulders, dark iron texture
- Dark green glowing cracks between armor plates (corrupted energy inside)
- Carries a massive blunt weapon (club, mace) or large shield
- Heavy boots, slow deliberate stance
- Small glowing green eyes barely visible under a domed helmet

Animation mood: Idle — barely moves, chest heaves slowly, armor creaks.
Walk — slow heavy stomping gait, shaking ground feel.

Style: 64×64px pixel art, top-down, dark iron + green void-energy accent.
```

---

## 4 · SHOOTER — Tirador Sigiloso

**Rol:** orbita al jugador, dispara a distancia

```
Top-down pixel art enemy for Kaosuarina.
Character: Tirador Sigiloso — a ranged void creature that keeps its distance.

Visual design:
- Medium build, hunched and wiry — built for mobility not tankiness
- Holds a void-energy bow or crystalline shard launcher
- Blue-white energy glow emanating from the weapon
- Eyes: cold blue glowing slits, calculating expression
- Wearing a dark hooded cloak, partially translucent at the edges
- Slightly crouched "ready to move" pose

Animation mood: Idle — slowly drifting/floating, scanning with its eyes, weapon slightly raised.
Walk — quick sidestep orbiting movement, weapon always aimed outward.

Style: 64×64px pixel art, top-down, dark with cold blue energy accents.
```

---

## 5 · MALDITO — Portador de la Plaga

**Rol:** explota al morir con veneno, cuerpo a cuerpo

```
Top-down pixel art enemy for Kaosuarina.
Character: Portador de la Plaga — a bloated void creature that detonates on death.

Visual design:
- Grotesquely bloated body, almost spherical torso — clearly filled with toxins
- Sickly green-black skin with visible veins pulsing green
- Dripping toxic fluid from hands and mouth
- Small eyes, pained or manic expression
- Cracks and seams on the body suggest internal pressure building
- Toxic green aura faintly visible around the body

Animation mood: Idle — body pulses/expands and contracts rhythmically, toxic drips fall.
Walk — shambling, unstable gait, body lurching side to side.

Style: 64×64px pixel art, top-down, sickly green-black toxic palette.
```

---

## 6 · ESPECTRAL — Sombra Inmaterial

**Rol:** inmune a daño físico, semi-transparente

```
Top-down pixel art enemy for Kaosuarina.
Character: Sombra Inmaterial — a ghost-like entity that phases through physical attacks.

Visual design:
- Translucent/ghostly body, approximately 45% opacity — can see through it
- Shifting between solid and wispy form at the edges
- Pure white-grey with faint teal inner glow (not solid, more like smoke)
- No distinct limbs — flows like a floating shroud or wraith shape
- Dark hollow eyes (black voids in a white mask)
- Small tendrils or wisps extend from the bottom instead of legs

Animation mood: Idle — gently phasing in and out, edges undulating like smoke.
Walk — gliding movement, body swaying ethereally, no walking legs — pure float.

Style: 64×64px pixel art, top-down, translucent white-grey with teal inner fire. Use alpha/transparency effect.
```

---

## 7 · BERSERKER — Frenético del Vacío

**Rol:** carga directa, velocidad ×3 al <20% HP

```
Top-down pixel art enemy for Kaosuarina.
Character: Frenético del Vacío — a berserker that explodes with speed when near death.

Visual design:
- Muscular aggressive humanoid, clearly built for destruction
- Deep red-crimson void energy crackling around its fists and eyes
- Battle-damaged body — visible wounds that glow red (the void energy leaking out)
- Double axes or large claws as weapons, raised in aggressive stance
- Cracked dark skin with ember-like red cracks throughout
- Enraged expression, mouth open in a scream

Animation mood (normal): Idle — pacing back and forth, aggressive but controlled.
Walk (normal) — heavy aggressive stride.
[NOTE: Rage mode sprites (×3 speed) to be created later — for now just base state]

Style: 64×64px pixel art, top-down, dark crimson with red energy accents.
```

---

## 8 · SPLITTER — Divisor Caótico

**Rol:** se divide en 2 copias al morir (40% HP cada una)

```
Top-down pixel art enemy for Kaosuarina.
Character: Divisor Caótico — an enemy that splits into two copies on death.

Visual design:
- Amorphous blob-like body with a visible seam/crack running down the middle
- The body looks like two halves barely held together by void energy
- Purple and violet unstable energy at the seam line, pulsing
- Small reaching appendages on each side
- Unstable wobbling appearance — the body never looks quite solid
- Eyes on each half independently (two pairs of eyes total, slightly misaligned)

Animation mood: Idle — the two halves slightly separate and reconnect rhythmically, wobbling.
Walk — shambling movement, the seam wobbling dangerously with each step.

Style: 64×64px pixel art, top-down, deep purple unstable palette, visible split seam effect.
```

---

## 9 · HEALER — Cuidador Abisal

**Rol:** huye del jugador, cura a aliados en radio 150u

```
Top-down pixel art enemy for Kaosuarina.
Character: Cuidador Abisal — a support creature that keeps allies alive.

Visual design:
- Small, frail, almost non-threatening body — low profile
- Emits a soft warm golden-yellow healing aura from its core
- Holds a staff or orb radiating healing energy
- Robes or wrappings in dark grey with golden trim
- Nervous/skittish body language — always slightly turned to flee
- Round eyes, concerned expression

Animation mood: Idle — the healing aura pulses gently, rocking nervously side to side.
Walk — quick shuffling retreat movement, looking back over its shoulder.

Style: 64×64px pixel art, top-down, dark grey + warm gold healing aura accent.
```

---

## 10 · SHIELDER — Bastión Sombrío

**Rol:** lento, 70% defensa base

```
Top-down pixel art enemy for Kaosuarina.
Character: Bastión Sombrío — an immovable wall of void-armor.

Visual design:
- Extremely wide and short body — all defense, no offense
- Carries a massive tower shield that takes up most of the sprite
- Shield is dark void-metal with a pulsing blue defensive rune at center
- Tiny visible legs barely visible under the shield
- Heavy full-body armor with no gaps
- Slowly advancing, shield-first posture

Animation mood: Idle — barely moves, occasional shield shuffle.
Walk — very slow deliberate shuffle forward, shield always raised.

Style: 64×64px pixel art, top-down, dark iron with blue defensive rune glow.
```

---

## 11 · ELITE_CHARGE — Embestidor de Élite

> [!info] **Canvas: ~137×137 px — usa `size: 98`** — elite, más detalle que minions

**Rol:** telegrafía 1.5s → carga recta devastadora

```
Top-down pixel art ELITE enemy for Kaosuarina.
Character: Embestidor de Élite — an elite that charges in a devastating straight line.

Visual design:
- Tall, aerodynamic body built for charging — tapered front, wide powerful legs
- Dark void-armor with an orange-gold charge energy building in the chest/horns
- Prominent horns or shoulder spikes for the charge impact
- Eyes glow bright orange when telegraphing a charge
- Orange energy lines streak along the body (indicating built-up speed)
- Combat stance: leaning forward, ready to launch
- Noticeably larger and more detailed than basic minions

Animation mood: Idle — constant slow shifting of weight, energy simmering.
Walk — purposeful stride, always scanning for charge opportunity.

Style: 98×98px pixel art, top-down, dark armor + orange charge-energy accents. Elite-tier visual weight.
```

---

## 12 · ELITE_SUMMON — Invocador de Élite

> [!info] **Canvas: ~137×137 px — usa `size: 98`** — elite

**Rol:** invoca 3 BASICO cada 8s, huye del jugador

```
Top-down pixel art ELITE enemy for Kaosuarina.
Character: Invocador de Élite — a cowardly elite that calls reinforcements.

Visual design:
- Tall, robed figure with multiple reaching arms (4 arms total — 2 holding staves, 2 gesturing)
- Dark robes with void runes glowing pale violet
- Summoning circle visible under its feet (faint floor projection)
- Cowering body language — back slightly hunched, always moving away
- Multiple glowing eyes (3-4) in a mask-like face
- Staff or totem that channels the summoning power
- Noticeably larger and more detailed than basic minions

Animation mood: Idle — arms weaving in slow summoning gestures, robes flowing.
Walk — hurried backward retreat while still gesturing forward.

Style: 98×98px pixel art, top-down, dark robes with pale violet summoning energy. Elite-tier visual weight.
```

---

## 13 · ELITE_ZONE — Señor del Territorio

> [!info] **Canvas: ~137×137 px — usa `size: 98`** — elite (estacionario)

**Rol:** estacionario, crea zona lenta radio 120u

```
Top-down pixel art ELITE enemy for Kaosuarina.
Character: Señor del Territorio — a stationary elite that controls an area.

Visual design:
- Root-like appendages anchoring it to the ground — clearly not moving
- Pulsing dark-teal zone emanating from its base (the slow field indicator)
- Crystalline or stone-like body, jagged and immovable
- No legs — it's a fixed territorial creature
- Arms/tendrils reaching upward or outward, projecting the zone
- Ancient, monolithic appearance — like a corrupted obelisk given life
- Noticeably larger and more imposing than basic minions

Animation mood: Idle — slowly rotating or pulsing, zone expands and contracts rhythmically.
[NOTE: Este enemigo no camina — idle-only sprite necesario. Walk frames = idle frames.]

Style: 98×98px pixel art, top-down, stone-dark with teal territory-glow aura. Elite-tier visual weight.
```

---

## 14 · GUARDIAN — Heraldo de Hierro (Minijefe, ola 10)

> [!warning] **Canvas: ~180×180 px — usa `size: 128` (máximo PixelLab)** — minijefe

**Rol:** boss de ola 10, shockwave de área

```
Top-down pixel art MINIBOSS enemy for Kaosuarina.
Character: Heraldo de Hierro — a massive iron guardian that patrols the Void.

Visual design:
- Significantly larger than all regular enemies — imposing, fills most of the canvas
- Full heavy void-iron plate armor, ancient and massive
- Shockwave energy visibly building up in its fists/chest (periodic attack tell)
- Dark iron grey with deep crimson glowing cracks in the armor
- Two large pauldrons (shoulder armor) with void energy emanating from the joints
- Carries a giant war maul or warhammer — clearly built for area damage
- Imposing authoritative stance — this creature IS in charge

Animation mood: Idle — heavy chest breathing, armor plates shifting, crimson energy pulses.
Walk — ground-shaking heavy stomp, deliberate and unstoppable.

Style: 128×128px pixel art, top-down, dark iron + deep crimson shockwave-energy accents. Boss-tier visual weight.
```

---

## 15 · ARQUERO — Sombra Errante (Minijefe, ola 20)

> [!warning] **Canvas: ~180×180 px — usa `size: 128` (máximo PixelLab)** — minijefe

**Rol:** teleporta para evitar melee, dispara ráfagas

```
Top-down pixel art MINIBOSS enemy for Kaosuarina.
Character: Sombra Errante — an elusive archer that teleports to avoid melee.

Visual design:
- Lean, hooded assassin-archer figure, clearly built for hit-and-run
- Dark cloak that leaves motion trails when teleporting (ghost afterimage effect)
- Holds a large void-bow made of dark energy, constantly slightly drawn
- Cold white-blue energy on the arrow nocked at all times
- Teleport residue: faint shadow copies that fade behind where it was standing
- Mask or hidden face — unknowable, no visible emotion

Animation mood: Idle — slowly floating/hovering slightly above ground, bow moving fluidly in aim.
Walk — careful, predatory movement, always keeping optimal distance.

Style: 128×128px pixel art, top-down, dark cloak with cold white-blue energy. Hint of teleport afterimages. Boss-tier visual weight.
```

---

## 16 · FRAGMENTADO — Prisionero Roto (Boss, ola 30)

> [!danger] **Canvas: ~180×180 px — usa `size: 128` (máximo PixelLab)** — boss (3 fases: Física → Mágica → Caos)

```
Top-down pixel art BOSS enemy for Kaosuarina.
Character: El Fragmentado — a boss entity shattered into three unstable phases.

Visual design:
- Large chaotic figure, clearly unstable — like a being forcibly held together
- Body is visually "fragmented" with cracks and seams of energy between the pieces
- Phase 1 (Physical): dark iron-grey, heavy armor fragments, physical cracks
- The sprite shows the Phase 1 appearance with hints that other forms are inside
- Visible inner energy: a core glowing with shifting color (grey-white for Phase 1)
- Multiple limbs or body fragments that don't quite connect properly
- Large imposing presence despite the fragmentation

Animation mood: Idle — fragments slowly rotating/shifting around the core, unstable orbiting pieces.
Walk — lurching, uneven movement as fragments try to stay together.

Style: 128×128px pixel art, top-down, fragmented aesthetic with visible inner core glow. Dark iron grey (Phase 1 base). Maximum boss-tier visual weight.
```

---

## 17 · DEVASTADOR — Devastador del Caos (Boss Final, ola 50)

> [!danger] **Canvas: ~180×180 px — usa `size: 128` (máximo PixelLab)** — boss final

```
Top-down pixel art FINAL BOSS enemy for Kaosuarina.
Character: El Devastador del Caos — the ultimate enemy and lord of the Chaotic Void.

Visual design:
- Massive, terrifying presence — the largest enemy in the game
- Phase 1: dark void-matter body, deep black-purple, with swirling chaos energy
- Phase 2 (at 50% HP): body becomes more aggressive, crimson tint replaces the purple
- Multiple large arms/appendages each crackling with different void energies
- Spiral energy patterns emanating from the body (telegraphs the spiral attack)
- Crown or halo of dark energy floating above the head
- Eyes: multiple bright void-energy eyes, each a different chaotic color
- True chaos incarnate — the visual should feel overwhelming

Animation mood: Idle — body slowly rotating, chaos energy swirling, arms shifting, completely unstoppable feel.
Walk — slow deliberate advance, each step warping the space around it slightly.

Style: 128×128px pixel art, top-down, deep black-purple with swirling multi-color void energy. Maximum visual impact — this is the final boss.
```

---

## Resumen de canvas por tipo

| Tipo | `size` PixelLab | Canvas aprox | Categoría |
|------|----------------|-------------|-----------|
| BASICO, RAPIDO, TANQUE | 64 | 92×92px | Minion básico ✅ generado |
| SHOOTER, MALDITO, ESPECTRAL | 64 | 92×92px | Minion básico ⏳ pendiente |
| BERSERKER, SPLITTER, HEALER, SHIELDER | 64 | 92×92px | Minion especial ⏳ pendiente |
| ELITE_CHARGE, ELITE_SUMMON, ELITE_ZONE | 98 | ~137×137px | Elite ⏳ pendiente |
| GUARDIAN, ARQUERO | 128 | ~180×180px | Minijefe ⏳ pendiente |
| FRAGMENTADO, DEVASTADOR | 128 | ~180×180px | Boss ⏳ pendiente |

## Animaciones a pedir por tipo

| Animación | Minions (64px) | Elites (98px) | Jefes (128px) |
|-----------|---------------|---------------|---------------|
| Walk (4 dirs) | ✅ Primera prioridad | ✅ Primera prioridad | ✅ Primera prioridad |
| Idle (4 dirs) | ✅ Segunda prioridad | ✅ Segunda prioridad | ✅ Segunda prioridad |
| Attack (pendiente) | ❌ Sprint posterior | ❌ Sprint posterior | ❌ Sprint posterior |
| Death (pendiente) | ❌ Sprint posterior | ❌ Sprint posterior | ❌ Sprint posterior |

> [!note] Notas especiales
> - **ELITE_ZONE:** no camina — idle estático suficiente; walk frames = idle frames.
> - **FRAGMENTADO:** diseñar sprite base para Fase 1. Fases 2 y 3 son recoloraciones del mismo sprite.
> - **Multiplicador visual en código:** sprites se dibujan a `size * 1.4f` en mundo, donde `size` es el campo Java del Enemy (no el `size` de PixelLab).

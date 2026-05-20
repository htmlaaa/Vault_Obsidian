---
title: Caballero del Kaos — Diseño de Personaje
tags:
  - personaje
  - diseño
  - pixel-art
  - caballero
  - rol
aliases:
  - Caballero Kaos
  - Void Knight
date: 2026-05-06
---

# Caballero del Kaos

Rol de tipo **Tank / Frontline** en el Vacío Kaótico. Contrapartida sólida al [[Mago-Kaos]]. Donde el mago canaliza el caos en energía arcana, el caballero lo absorbe con metal y armadura hasta que el propio metal se corrompe.

---

## Sprites

### Original (hecho a mano)
![[cABALLERO.png]]

### Versión PixelLab (`Caballero-Kaos`)
![[caballero-pixellab-south.png]] ![[caballero-pixellab-east.png]] ![[caballero-pixellab-north.png]] ![[caballero-pixellab-west.png]]

> [!note] Cuál usar
> El sprite original fue hecho a mano — úsalo como referencia de estilo. El de PixelLab tiene las 4 direcciones y sirve de base para animaciones.

---

## Paleta de colores

| Elemento | Color | Hex |
|----------|-------|-----|
| Base armadura | Negro void | `#0A0A12` |
| Sombras armadura | Acero oscuro | `#0D1A1A` |
| Acento principal | Void teal | `#00E5CC` |
| Glow exterior | Teal claro | `#66FFE8` |
| Escudo | Teal calcificado | `#00BBA8` |
| Ojos / visor | Teal puro | `#00E5CC` |

> [!tip] Diferenciación de roles
> Caballero usa **teal `#00E5CC`** — Mago usa **púrpura `#9B30FF`** — Shooter usa **ámbar `#FFB800`**. Misma luminosidad, distinto matiz.

---

## Diseño visual

```
┌─────────────────────────────────┐
│  Casco de visera completa       │
│  Rendija de visor: 2 puntos     │
│  teal brillantes — sin rostro   │
│                                 │
│  Hombreras anchas — silueta     │
│  imponente, masa pura           │
│                                 │
│  Peto con grietas radiales      │
│  teal filtrándose desde dentro  │
│                                 │
│  Brazo izquierdo: escudo kite   │
│  parte inferior reemplazada     │
│  por void-llama teal            │
│                                 │
│  Puño derecho: grietas en       │
│  nudillos con glow teal         │
│                                 │
│  Grebas con teal en la base     │
│  2-3 fragmentos teal flotando   │
│  alrededor de las piernas       │
└─────────────────────────────────┘
```

---

## Stats del rol

| Stat | Valor |
|------|-------|
| Vida | 150 HP |
| Velocidad | 250 px/s |
| Daño base | 15 |
| Core | Fortaleza Reactiva |
| Especialidad | Alta resistencia, builds defensivas |

---

## Core — Fortaleza Reactiva

Ver [[Cores-Sinergias]] para mecánica completa.

Al recibir daño → gana stack de Armadura temporal (max 5). Cada stack reduce el siguiente golpe 8%. Sin daño 4s → pierde 1 stack/s.

**Sinergias clave**: Vida Máxima (+1 stack máximo por 20 HP extra), Contraataque (stack consumido → siguiente bala +40% daño).

---

## Prompt PixelLab (para regenerar)

```
64x64 pixel art character, corrupted void knight, dark fantasy style, front-facing isometric view, no antialiasing, transparent background, hard pixel edges. Body: heavy full plate armor, near-black base with very dark steel shadows, broad pauldrons and thick leg guards, imposing silhouette. Corruption: fine hairline cracks across chest, shoulder and gauntlet plates filled with teal void energy glowing from within. Left arm: kite shield merged with forearm, lower portion replaced by calcified teal void-flame pixel clusters. Right arm: large gauntleted fist, cracks along knuckles leaking teal light. Head: full-visor helmet, visor slit shows two teal glowing eye-points only, no face visible. Particles: 2-3 tiny floating teal pixel shards orbiting low around legs. Color count: max 13 colors. No gradients, no antialiasing. Pixel art like Brotato or Vampire Survivors.
```

**Negativo**: `no background, no antialiasing, no gradients, no smooth edges, no 3D rendering, no warm colors, no face visible`

---

## Notas de diseño

- El caballero es lo opuesto al mago: donde el mago es **vacío y energía**, el caballero es **masa y metal corrompido**.
- La corrupción teal en las grietas de la armadura conecta con la narrativa: el Vacío Kaótico rompe todo desde dentro, incluso el acero más duro.
- El escudo parcialmente disuelto diferencia su silueta de cualquier otro personaje — reconocible a cualquier tamaño.

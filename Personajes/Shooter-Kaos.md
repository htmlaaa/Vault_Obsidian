---
title: Shooter del Kaos — Diseño de Personaje
tags:
  - personaje
  - diseño
  - pixel-art
  - shooter
  - rol
aliases:
  - Shooter Kaos
  - Void Gunslinger
date: 2026-05-06
---

# Shooter del Kaos

Rol de tipo **DPS Ranged / Ágil** en el Vacío Kaótico. El más rápido de los tres personajes. Donde el [[Caballero-Kaos]] resiste con metal y el [[Mago-Kaos]] canaliza energía arcana, el Shooter ha *weaponizado* su propia corrupción — una de sus pistolas ha sido consumida por el void y ahora dispara plasma ámbar.

---

## Sprites (PixelLab)

![[shooter-south.png]] ![[shooter-east.png]] ![[shooter-north.png]] ![[shooter-west.png]]

> [!note] Ubicación
> `PIXEL/characters/shooter/` — 4 direcciones generadas por PixelLab.

---

## Cómo se diseñó

### Proceso de decisión

El Shooter fue definido por el agente `art-director` en la sesión del 2026-05-06. El proceso fue:

1. **Personajes elegidos**: Caballero, Mago, Shooter — los 3 arquetipos más distintos del backlog
2. **Paleta del Shooter**: el agente eligió **ámbar `#FFB800`** porque completa una tríada perfecta en la rueda de color con el teal del Caballero y el púrpura del Mago. Ningún color compite con otro visualmente en pantalla.
3. **Concepto visual**: pistolero asimétrico — una pistola intacta (acero oscuro), otra consumida por el void (cristal ámbar). La asimetría es la firma visual del personaje.
4. **Corrupción**: se manifiesta como quemadura que sube desde el gatillo por el antebrazo derecho — el void se filtró *a través del arma*, no desde fuera.

### Regla de diseño aplicada

> [!info] Regla de asimetría (lenguaje visual compartido)
> Cada personaje muestra **un elemento intacto + uno consumido** en paralelo:
> - Caballero: pauldron intacto vs. brazo-escudo disuelto
> - Mago: staff oscuro vs. orbe de void
> - **Shooter: pistola de metal vs. pistola de cristal ámbar**

---

## Paleta de colores

| Elemento | Color | Hex |
|----------|-------|-----|
| Base chaqueta | Negro void | `#0A0A12` |
| Sombras chaqueta | Carbón oscuro | `#141418` |
| Acento principal | Ámbar tóxico | `#FFB800` |
| Glow exterior | Ámbar claro | `#FFD966` |
| Pistola ámbar — cristal | Ámbar sólido | `#FFB800` |
| Grietas en piel | Ámbar interior | `#CC9200` |
| Gafas rotas — glow | Ámbar tras cristal | `#FFB800` |

> [!tip] Por qué ámbar y no otro color
> Ámbar `#FFB800` está en la banda amarillo-naranja, equidistante de teal (cian-verde) y púrpura en la rueda de color. Lee como **combustible inestable** — caliente, peligroso, volátil. Ninguna connotación arcana ni mecánica: es puro daño explosivo.

---

## Descripción visual

```
┌─────────────────────────────────┐
│  Pelo corto irregular           │
│  Gafas rotas subidas a la       │
│  frente — lente izquierda       │
│  intacta (vidrio oscuro),       │
│  lente derecha astillada con    │
│  glow ámbar detrás              │
│                                 │
│  Chaqueta negra rasgada en      │
│  hombro derecho — piel          │
│  corrupta visible debajo        │
│                                 │
│  Mano izquierda: pistola de     │
│  acero oscuro — intacta,        │
│  sin glow, arma normal          │
│                                 │
│  Mano derecha: pistola          │
│  parcialmente consumida —       │
│  mitad inferior = cristal       │
│  ámbar solidificado, cañón      │
│  con glow caliente              │
│                                 │
│  Antebrazo derecho: piel        │
│  agrietada con luz ámbar        │
│  sangrando hacia el codo        │
│                                 │
│  Botas ligeras — marca de       │
│  chamusco ámbar bajo pie        │
│  derecho únicamente             │
│                                 │
│  Partículas: 3-4 brasas ámbar   │
│  flotando del cañón corrupto    │
└─────────────────────────────────┘
```

---

## Stats del rol

| Stat | Valor |
|------|-------|
| Vida | 80 HP |
| Velocidad | 320 px/s |
| Daño base | 25 |
| Core | Momentum de Combate |
| Especialidad | DPS sostenido, builds de cadencia |

---

## Core — Momentum de Combate

Ver [[Cores-Sinergias]] para mecánica completa.

Kills acumulan Combo (max 10). Sin kill en 2s → -1 combo/s. Cada punto de Combo = +3% cadencia (max +30% a combo 10).

**Sinergias clave**: Cadencia (se compone con el Combo → ×2.3 cadencia a máximo), Adrenalina (nuevo upgrade: burst +50% velocidad al alcanzar combo 10).

---

## Prompt PixelLab usado

```
64x64 pixel art character, corrupted void gunslinger, dark fantasy style, front-facing isometric view, no antialiasing, transparent background, hard pixel edges. Body: lean agile frame, near-black jacket with very dark charcoal shadows, torn at right shoulder exposing corrupted skin beneath, wide combat stance suggesting mobility. Left hand: intact dark-metal pistol, sleek angular barrel, no glow. Right hand: partially void-consumed pistol, lower half replaced by solidified amber void-crystal, barrel tip glowing hot amber, small ember sparks drifting off the muzzle. Corruption: right forearm and trigger hand show cracked skin with amber light bleeding through cracks spreading up toward elbow like a burn pattern. Head: short scrappy hair, cracked goggles pushed up on forehead, left lens intact dark glass, right lens shattered with amber glow behind the crack. Feet: light boots, amber void-scorch marks on ground beneath right foot only. Particles: 3-4 tiny amber pixel embers floating off the corrupted gun barrel. Color count: max 13 colors. No gradients, no antialiasing. Pixel art like Brotato or Vampire Survivors.
```

**Negativo**: `no background, no antialiasing, no gradients, no smooth edges, no 3D rendering, no blue or purple colors on main body`

---

## Notas de diseño

- El Shooter es el más frágil de los tres pero el más rápido — el jugador que lo elige prioriza matar antes de que le maten.
- La asimetría de las dos pistolas es deliberada: el arma corrupta (derecha) define quién es, el arma intacta (izquierda) recuerda quién era.
- Sin piernas visibles estilo mago: **no** — el Shooter necesita verse ágil con botas y postura baja, diferenciando claramente su silueta del Mago.

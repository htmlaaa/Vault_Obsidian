---
title: Mago del Kaos — Diseño de Personaje
tags:
  - personaje
  - diseño
  - pixel-art
  - mago
  - rol
aliases:
  - Mago Kaos
  - Chaos Mage
date: 2026-05-05
---

# Mago del Kaos

Rol de tipo **Control/Daño mágico** en el Vacío Kaótico. Contrapartida arcana al [[Kaosuarina#Roles jugables|Caballero]]. Donde el caballero resiste con armadura, el mago canaliza el caos del vacío directamente en proyectiles y distorsiones.

---

## Referencia visual — Caballero

![[cABALLERO.png]]

**Estilo base a mantener:**
- Fondo transparente, sin antialiasing
- Base negra/gris muy oscuro
- Acento brillante con efecto glow
- Perspectiva frontal ligeramente isométrica
- Silueta clara y reconocible a 64×64 px

---

## Diseño del Mago

### Paleta de colores

| Elemento | Color | Hex |
|----------|-------|-----|
| Base túnica | Negro vacío | `#0A0A12` |
| Sombras túnica | Púrpura muy oscuro | `#1A0A2E` |
| Acento principal | Púrpura eléctrico | `#9B30FF` |
| Glow exterior | Violeta claro | `#C87AFF` |
| Runes / detalles | Magenta void | `#FF44CC` |
| Ojos / orbe | Blanco puro | `#FFFFFF` |
| Staff madera | Gris carbón | `#2A2A3A` |

> [!note] Diferenciación de roles
> Caballero usa **teal `#00E5CC`** — Mago usa **púrpura `#9B30FF`**. Misma luminosidad, distinto matiz. Reconocibles de un vistazo en pantalla.

### Descripción visual

```
┌─────────────────────────────────┐
│  Capucha puntiaguda con         │
│  runa brillante en la frente    │
│                                 │
│  Ojos brillantes (blanco/       │
│  púrpura) sin rostro visible    │
│                                 │
│  Túnica larga con capas         │
│  desiguales — efecto rasgado    │
│  por el vacío caótico           │
│                                 │
│  Mano izquierda: orbe de        │
│  energía púrpura flotando       │
│                                 │
│  Mano derecha: staff corto      │
│  con cristal en la punta        │
│                                 │
│  Partículas de caos             │
│  orbitando el cuerpo (2-3 px)   │
│                                 │
│  Sin piernas visibles —         │
│  túnica cubre hasta el suelo    │
│  efecto levitación sutil        │
└─────────────────────────────────┘
```

### Tamaño y proporciones (64×64 px)

| Zona | Altura (px) |
|------|------------|
| Capucha | ~14 px |
| Cabeza/ojos | ~8 px |
| Torso + brazos | ~22 px |
| Túnica inferior | ~18 px |
| Margen | ~2 px top/bottom |

**Anchura cuerpo**: ~32-36 px centrado. Staff puede sobresalir al lado.

---

## Prompt para PixelLab AI

### Prompt principal (copiar tal cual)

```
64x64 pixel art character, chaos mage wizard, dark void fantasy style, front-facing isometric view, no antialiasing, transparent background, hard pixel edges.

Body: long dark flowing robes, black base (#0A0A12) with very dark purple shadows (#1A0A2E), torn ragged hem at bottom suggesting void corruption.

Accents: electric purple glow (#9B30FF) on hood rune, staff crystal, orb energy, and robe trim. Bright violet (#C87AFF) for outer glow bloom. Small magenta rune details on chest.

Head: pointed hood with a single glowing rune on forehead. Two glowing white-purple eyes visible in shadow, no visible face beneath.

Left hand: floating orb of swirling purple void energy, 8x8 px chaos sphere.

Right hand: short dark staff topped with a cracked purple crystal.

Particles: 3-4 tiny floating purple-magenta pixel sparks orbiting around the character body.

Silhouette: robes cover feet completely, slight levitation implied by no ground contact. Wide flowing silhouette at bottom, narrow at shoulders.

Color count: max 12 colors. No gradients, no antialiasing. Pixel art game sprite style similar to Brotato or Vampire Survivors characters.
```

### Prompt negativo

```
no background, no antialiasing, no gradients, no smooth edges, no 3D rendering, no realistic proportions, no brown or green colors, no weapon other than staff, no legs visible
```

### Variantes a generar

| Variante | Cambio |
|----------|--------|
| Base | Prompt principal — túnica estática |
| Activo | Orbe más grande, más partículas, runa más brillante |
| Dañado | Colores más oscuros, túnica más rasgada, ojos más rojos |
| Paleta alt | Swap púrpura → teal para versión corrupta/alternativa |

---

## Stats del rol

| Stat | Valor | Fuente |
|------|-------|--------|
| Vida | **85 HP** | `Role.mago()` |
| Velocidad | 270 px/s | `PlayerStats.baseSpeed` |
| Defensa física | 5 | `s.physicalDefense = 5f` |
| Resistencia mágica | 15 | `s.magicResistance = 15f` |
| Maná máx. | 120 | `MANA_MAX_MAGO_BASE` |
| Regen maná | **2.0 /s** | `MAGE_PASSIVE_REGEN` — único pasivo |
| Daño bolt (ligero) | 34 | `s.magicLightDamage` |
| Daño blast (pesado) | 85 | `s.magicHeavyDamage` |
| Coste bolt | 8 maná | `s.magicLightManaCost` |
| Coste blast | 35 maná | `s.magicHeavyManaCost` |
| CD ataque ligero | 0.28 s | `s.lightAttackCooldown` |
| CD ataque pesado | 1.5 s | `s.heavyAttackCooldown` |
| Reliquia | Resonancia Caótica | `ReliquiaMago` |
| Especialidad | Proyectiles de área, bolt redirigible | — |

> [!note] Actualizado 2026-06-06
> HP corregido de 75 → 85 para coincidir con `Role.mago()` actual. Añadidos stats de maná, regen, daño y costes.

> [!todo] Pendiente
> - [ ] Generar sprite en PixelLab con el prompt de arriba
> - [ ] Exportar como `mago-kaos.png` (64×64, transparente)
> - [ ] Añadir a `A_Game_Kaosuarina/assets/sprites/`

---

## Notas de diseño

- El mago es el opuesto visual del caballero: donde el caballero es **masa y metal**, el mago es **vacío y energía**.
- La túnica rasgada conecta con la narrativa del Vacío Kaótico — el caos corrompe y transforma todo lo que toca.
- Sin piernas visibles = diferenciación de silueta clara en pantalla top-down, fácil de leer entre decenas de enemigos.

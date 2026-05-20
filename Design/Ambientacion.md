---
title: Ambientación — Vacío Kaótico
tags:
  - design
  - ambientacion
  - mundo
  - arte
  - biomas
aliases:
  - Ambientación
  - Vacío Kaótico Visual
date: 2026-05-06
updated: 2026-05-20
---

# Ambientación — Vacío Kaótico

Identidad visual del mundo, la arena y los enemigos. Complementa [[Kaosuarina]] (mecánicas) y [[Mago-Kaos]] (personajes).

> [!abstract] Concepto central
> El Vacío Kaótico es una dimensión que corrompe y distorsiona todo lo que toca. Nada es liso — todo emerge de grietas. El caos no envuelve: **rompe desde dentro**.

---

## Arena / Campo de batalla

### Color y atmósfera

| Elemento | Color | Hex |
|----------|-------|-----|
| Fondo (void) | Negro azul-violeta | `#06060F` |
| Suelo base | Azul oscuro profundo | `#0D0D1E` |
| Grid suelo | Azul noche | `#1A1A3A` |
| Grietas — glow interior | Magenta void tenue | `#3D0035` |
| Borde push-back | Magenta límite | `#6B0057` |

> [!tip] Por qué el fondo es azul-violeta y no negro puro
> Los personajes tienen base `#0A0A12` (negro neutro). El fondo `#06060F` tiene un toque azul-violeta que crea contraste sutil — los sprites se leen encima del suelo sin necesitar outlines extra.

### Suelo

Grid de celdas ~32px dibujado en `#1A1A3A`. En intersecciones aleatorias: grietas radiales (3-5 líneas, 4-8px) con glow interior magenta `#3D0035` muy tenue. Las grietas se **concentran en el tercio exterior** de la arena — el centro parece más estable, los bordes más corrompidos.

### Borde de arena

Sin muro físico. Niebla-void que se intensifica desde el 80% del radio hasta opacidad total en el límite. En el radio exacto del push-back: anillo de 1px pulsante en `#6B0057` (~40% opacidad) — señal de umbral, no de pared.

### Elementos ambientales (todos procedurales)

- **Partículas drift**: 8-12 puntos 1-2px en `#8899FF` (30% opacidad) flotando lentamente — capa de fondo, debajo de entidades
- **Grietas que respiran**: pulso muy lento 2-3s, de tenue a ligeramente menos tenue — da vida al suelo sin animación compleja
- **Tentáculos void**: 4-6 líneas irregulares `#2A0028` alcanzando desde el borde hacia el interior — estáticos, refuerzan el límite sin muros

---

## Lenguaje visual de enemigos

> [!warning] Paleta de enemigos ≠ paleta de jugadores
> Jugadores usan teal / purple / amber. Enemigos usan verde / rojo / azul / oro. Ningún color de enemigo se acerca al de un jugador — el campo de batalla se lee de un vistazo.

| Tipo | Silueta | Color acento | Hex | Concepto |
|------|---------|--------------|-----|---------|
| BASICO | Ovoide jorobado asimétrico ~40px, grieta vertical central, extremidades cortas | Verde tóxico | `#39FF14` | Materia caótica básica — inestable, el void en su forma más cruda |
| RAPIDO | Lágrima horizontal ~48px ancho, inclinado 30°, grietas trailing como líneas de velocidad | Rojo-naranja | `#FF3D00` | Fragmento que se quema para cerrar distancia — sacrifica forma por velocidad |
| TANQUE | Fortaleza cuadrada ~56px, múltiples capas de grietas viejas sobre nuevas, cabeza pequeña | Azul hierro | `#4488FF` | Masa void antigua cristalizada — el daño acumulado nunca sana del todo |
| SHOOTER | Anillo/halo hueco ~36px diámetro con núcleo central, 4 radios en spoke | Oro pálido | `#FFE566` | Ojo void que alcanzó órbita — observa, calcula, escupe energía void concentrada |

---

## 5 Reglas de cohesión

> [!important] Art Bible — Reglas globales
> Estas reglas aplican a **todo**: jugadores, enemigos, arena, proyectiles, UI.

1. **Todo emerge de grietas** — nada es liso. Si una entidad no tiene grietas, no pertenece al Vacío Kaótico.
2. **Glow siempre desde dentro, nunca desde fuera** — no hay fuentes de luz externas. Todo color es auto-emitido a través de grietas y partículas.
3. **Un color acento por entidad — máximo** — ese color aparece en grietas, partículas y proyectiles propios. Sin mezclar.
4. **Base oscura universal, acento diferencia facción** — la base negra une todo; el acento identifica quién es quién en milisegundos.
5. **El void disuelve los bordes** — niebla consume el límite, partículas se disipan, proyectiles dejan trail breve. La silueta es el único borde duro permitido.

---

## 5 Biomas del Vacío

> [!abstract] Concepto de biomas
> El Vacío Kaótico no es homogéneo. A medida que el jugador profundiza, el entorno evoluciona: el caos adopta formas distintas — vacío puro, cristalización, putrefacción, calor extremo, y finalmente el núcleo donde el Devastador duerme. Cada bioma mantiene las 5 reglas de cohesión pero introduce su propia paleta de acento y sus propios materiales de suelo.

Los biomas se corresponden con zonas de profundidad de partidas. El Abismo Central es siempre el punto de inicio; los siguientes se desbloquean conforme el jugador avanza en el árbol de progresión (pendiente de implementación).

---

### Bioma 0 — Abismo Central

**Concepto:** El inicio. Vacío puro antes de que el caos tome forma. Tranquilizador y amenazante a partes iguales — el suelo parece estable pero las grietas ya palpitan bajo la superficie.

**Paleta:**

| Elemento | Color | Hex |
|----------|-------|-----|
| Suelo base | Azul oscuro profundo | `#0D0D1E` |
| Grietas | Magenta void | `#3D0035` |
| Acento glow | Magenta límite | `#6B0057` |

**Suelo (tiles base):**
- Losa plana oscura con grid sutil en `#1A1A3A`
- Grietas radiales en intersecciones (magenta tenue, glow pulsante)
- Ocasionalmente: placa de void liso sin grietas — contraste de "suelo intacto"

**Decoración estática:**
- Columnas rotas de piedra void (1-2 por cuadrante) — base `#0D0D1E`, línea de fractura magenta
- Fragmentos de losa levantados en ángulo bajo — sombra proyectada procedural
- Anillo de niebla perimetral que se espesa hacia el borde

**Atmósfera:**
- Partículas drift azul-blanco `#8899FF` (30% opacidad) flotando hacia arriba muy lentamente
- Niebla perimetral sólida, sin color propio — absorbe el fondo
- Sin efectos de calor ni partículas de color — zona más "limpia" del juego

**Prompt PixelLab sugerido (tiles 32×32):**
```
top-down tileset, dark void stone floor tiles, deep blue-black color #0D0D1E,
subtle grid lines, magenta glowing cracks emanating from center,
inner glow effect, pixel art 32x32, dark fantasy roguelite,
no bright colors, cohesive 8-tile set: solid floor, cracked floor,
broken slab, corner crack, edge crack
```

---

### Bioma 1 — Bosque de Espinas Cristalinas

**Concepto:** El caos ha cristalizado. Formaciones de cristal violeta oscuro emergen del suelo como espinas, algunas tan altas como el jugador. El suelo está cubierto de fragmentos rotos que brillan tenuemente. Zona de lectura visual media — los cristales son obstáculos estéticos (no físicos en v1).

**Paleta:**

| Elemento | Color | Hex |
|----------|-------|-----|
| Suelo base | Violeta tiza | `#1A0A2E` |
| Cristales | Violeta frío | `#4B0082` |
| Glow interior cristal | Lila pálido | `#9966CC` |

**Suelo (tiles base):**
- Suelo de tierra void compacta, color `#1A0A2E` con textura granular
- Fragmentos de cristal roto incrustados — puntos de glow `#9966CC` en 1-2px
- Musgo void negro (`#0A0014`) en bordes de tiles — transición entre zonas

**Decoración estática:**
- Cristales emergentes: columnas de 1-3 tiles de alto, base ancha, punta irregular
- Racimos de cristal bajo (medio tile) — actúan como "arbustos" del bioma
- Raíces cristalizadas horizontales que conectan formaciones cercanas

**Atmósfera:**
- Partículas de polvo de cristal: puntos blancos `#DDDDF0` (20% opacidad), caída lenta diagonal
- Niebla de fondo color lila muy oscuro `#1A0A2E` — más densa que el Abismo
- Sin calor ni fuego — temperatura visual fría

**Prompt PixelLab sugerido (tiles 32×32):**
```
top-down tileset, dark crystal forest floor, deep purple-black soil #1A0A2E,
shattered violet crystal fragments embedded in ground, inner glow #9966CC,
pixel art 32x32, dark fantasy, cold color palette, no warm colors,
tile set: soil base, crystal shard floor, dense crystal floor,
crystal cluster decoration, root decoration
```

---

### Bioma 2 — Pantano de Corrupción

**Concepto:** El void ha podrido. Charcos de líquido verde-negro cubren el suelo irregular; la niebla baja no deja ver los bordes. Es el bioma más opresivo visualmente — la paleta más sucia del juego. Los jugadores sienten que están luchando en un lugar que no quiere que estén.

**Paleta:**

| Elemento | Color | Hex |
|----------|-------|-----|
| Suelo base | Negro verdoso | `#070F07` |
| Charcos veneno | Verde corrupción | `#1A4A00` |
| Glow líquido | Verde tóxico | `#39FF14` |

**Suelo (tiles base):**
- Tierra pantanosa oscura `#070F07` con textura irregular (no cuadrícula perfecta)
- Charcos de líquido tóxico: tiles con superficie `#1A4A00`, borde con glow `#39FF14` en 1px
- Grietas llenas de líquido — las grietas magenta del art bible base se sustituyen por grietas verdes en este bioma

**Decoración estática:**
- Hongos void: cap negro, líneas de esporas verde-tóxico `#39FF14`
- Raíces podridas emergiendo del suelo — sinuosas, color `#0A1A00`
- Burbujas de gas congeladas en el suelo — círculos 4-6px con glow verde

**Atmósfera:**
- Niebla baja y densa: capa semitransparente `#070F07` que cubre los 16px inferiores del viewport
- Partículas de esporas: puntos verde-tóxico `#39FF14` (15% opacidad), movimiento flotante errático
- Sin cielo — la niebla bloquea cualquier referencia superior

**Prompt PixelLab sugerido (tiles 32×32):**
```
top-down swamp tileset, dark corrupt bog floor, black-green mud #070F07,
toxic green glowing puddles #39FF14 with inner glow, poison corruption aesthetic,
pixel art 32x32, dark fantasy roguelite, sickly palette,
tile set: mud base, toxic puddle, puddle edge, cracked bog,
mushroom decoration, root decoration
```

---

### Bioma 3 — Forja del Caos

**Concepto:** El calor del caos concentrado. El void arde sin combustible — es energía pura sin control. Suelo de magma void (naranja-negro, no naranja brillante) con grietas que pulsan como latidos rápidos. Temperatura visual alta. El bioma más dinámico antes del final.

**Paleta:**

| Elemento | Color | Hex |
|----------|-------|-----|
| Suelo base | Obsidiana caliente | `#1A0800` |
| Grietas de magma | Naranja void | `#CC3300` |
| Glow magma | Naranja caliente | `#FF6600` |

**Suelo (tiles base):**
- Obsidiana void `#1A0800` con textura de escoria — irregular, con protuberancias
- Grietas de magma: `#CC3300` → `#FF6600` en gradiente desde centro de grieta
- Zonas de suelo caliente: tiles con textura de calor radiante (degradado sutil hacia glow)

**Decoración estática:**
- Pilares de escoria: columnas bajas y anchas, color obsidiana con corona de glow naranja
- Charcos de magma void (no lava real): superficies `#CC3300`, menos fluidos que el pantano
- Fragmentos de metal void incrustados en el suelo — destellos `#FF6600` en edges

**Atmósfera:**
- Partículas de brasa: puntos naranja `#FF6600` (40% opacidad) ascendiendo rápido, se desvanecen arriba
- Ondas de calor: distorsión sutil del render (efecto shimmer, baja intensidad)
- Niebla de ceniza: capa superior muy tenue color gris oscuro `#1A1010`

**Prompt PixelLab sugerido (tiles 32×32):**
```
top-down forge tileset, dark obsidian floor with magma cracks,
black-orange void lava #CC3300 glowing fissures, inner glow #FF6600,
pixel art 32x32, dark fantasy, volcanic chaos aesthetic, warm dark palette,
tile set: obsidian base, magma crack floor, hot floor glow,
slag pillar decoration, ember pit decoration
```

---

### Bioma 4 — Núcleo del Devastador

**Concepto:** El corazón del Vacío Kaótico. Aquí duerme el Devastador. El suelo es carne void — orgánico, pulsante, profundamente perturbador. Rojo sangre oscuro, sin elementos naturales. Todo late. Es el único bioma donde el suelo parece vivo.

**Paleta:**

| Elemento | Color | Hex |
|----------|-------|-----|
| Suelo base | Rojo carne void | `#1A0005` |
| Venas pulsantes | Rojo sangre | `#8B0000` |
| Glow núcleo | Rojo carmesí | `#CC0020` |

**Suelo (tiles base):**
- Superficie orgánica `#1A0005` con textura de membrana — sin grid, curvas irregulares
- Venas: líneas gruesas `#8B0000` ramificadas, glow interior `#CC0020` pulsante (animación lenta)
- Zonas de tejido "sano": más oscuras `#0F0002` — contraste mínimo pero presente

**Decoración estática:**
- Garras void: estructuras óseas emergiendo, base `#1A0005` con punta `#CC0020`
- Quistes void: bultos redondeados en el suelo, superficie translúcida con núcleo carmesí
- Altares rotos: piedra void cubierta de venas, en proceso de ser absorbida por el suelo

**Atmósfera:**
- Sin partículas normales — solo "latidos": pulsos de color `#CC0020` que irradian desde el centro del mapa y se desvanecen
- Niebla de sangre: capa de niebla rojiza muy oscura `#1A0005` a altura media
- Ausencia de luz ambiental — el glow de las venas es la única fuente de iluminación

**Prompt PixelLab sugerido (tiles 32×32):**
```
top-down organic flesh floor tileset, dark void heart, deep blood-red #1A0005,
pulsing dark veins #8B0000 with inner red glow #CC0020, organic membrane texture,
pixel art 32x32, dark fantasy horror roguelite, body horror aesthetic,
no bright colors, tile set: flesh base, vein floor, dense vein floor,
bone claw decoration, void cyst decoration
```

---

## Regla de transición entre biomas

> [!important] Los biomas comparten base oscura — la transición es de acento, no de contraste
> Cuando el jugador transita de un bioma al siguiente, el suelo base (muy oscuro) persiste. Solo el color de acento de las grietas y las decoraciones cambian. Esto mantiene la legibilidad táctica — los enemigos y el jugador siempre se leen encima del fondo independientemente del bioma.

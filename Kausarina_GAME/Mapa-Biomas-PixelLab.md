---
title: Plan de Tilesets PixelLab — 5 Biomas del Vacío
tags:
  - assets
  - pixellab
  - tilesets
  - biomas
  - mapa
date: 2026-05-20
estado: planificacion
dependencias:
  - Ambientacion.md
  - Sprint 12 — Mapas y Biomas
---

# Plan de Tilesets PixelLab — 5 Biomas del Vacío

Guía de producción de assets para el sistema de mapas por tiles. Este documento define qué generar, cómo pedírselo a PixelLab, y en qué orden hacerlo.

---

## Especificación técnica global

### Tamaño de tile: 32×32 px

LibGDX renderiza tilemaps con `TiledMap` (formato TMX + libgdx-tiledmap). El tamaño estándar que equilibra legibilidad, rendimiento y facilidad de trabajo en Tiled editor es **32×32 px**. A 1280×720 caben 40×22.5 tiles en pantalla — suficiente para una arena circular de radio razonable sin tiles demasiado pequeños.

> [!note] No usar 16×16 px
> El juego tiene personajes de 64×64px (canvas PixelLab). Tiles de 16px quedarían muy pequeños relativos a los sprites. Con 32px los personajes ocupan ~2×2 tiles — proporción correcta para un top-down shooter.

### Formato esperado por LibGDX

LibGDX + Tiled usa **sprite sheets (tilesheets)** en PNG: una imagen donde cada tile ocupa 32×32px en una cuadrícula. El formato Tiled TMX referencia el tilesheet con su ruta y las dimensiones de cada tile.

**PixelLab genera imágenes individuales (PNG sueltos), no sprite sheets automáticamente.**
El flujo de trabajo es:
1. Generar cada tile como PNG 32×32 individual desde PixelLab
2. Combinar manualmente en un sprite sheet usando Piskel / Aseprite / Photoshop (o gdx-texture-packer)
3. Crear el TMX en Tiled editor referenciando el sprite sheet
4. Cargar en LibGDX con `TiledMap map = new TmxMapLoader().load("maps/bioma0.tmx")`

### Restricciones de PixelLab a tener en cuenta

- PixelLab maneja bien: suelo oscuro, texturas orgánicas, brillos de bordes, pixel art estilo oscuro/fantástico
- PixelLab puede fallar en: geometría muy precisa, simetría perfecta, detalles menores de 2px
- Pedir siempre **fondo transparente** en decoraciones — facilita composición
- Usar `top-down` explícitamente en el prompt — PixelLab tiene modos top-down tileset

---

## Orden de creación recomendado

```
Prioridad 1 (Sprint 12 fase 1 — desbloquea integración código):
  Bioma 0 — Abismo Central (zona de inicio, siempre visible)

Prioridad 2 (Sprint 12 fase 2 — completan el arco de juego):
  Bioma 4 — Núcleo del Devastador (zona final, más impactante visualmente)
  Bioma 3 — Forja del Caos (penúltima zona)

Prioridad 3 (Sprint 12 fase 3 — biomas intermedios):
  Bioma 1 — Bosque de Espinas Cristalinas
  Bioma 2 — Pantano de Corrupción
```

**Razonamiento:** El Abismo Central desbloquea el sistema de tiles en código y sirve de plantilla técnica. El Núcleo se hace segundo porque es la zona más icónica del juego y el boss fight ocurre ahí — tiene prioridad creativa. Los biomas intermedios pueden iterarse una vez que la pipeline técnica funciona.

---

## Bioma 0 — Abismo Central

**6 assets · Prioridad máxima**

### Tiles de suelo (4 assets)

| ID     | Nombre                       | Descripción                              | Prompt PixelLab                                                                                                                                                                                     |
| ------ | ---------------------------- | ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| B0-T01 | `abismo_suelo_base`          | Losa void limpia, sin grietas            | `pixel art 32x32 top-down tile, dark void stone floor, deep blue-black #0D0D1E, subtle texture, no cracks, seamless tileable, dark fantasy roguelite, transparent background`                       |
| B0-T02 | `abismo_suelo_grieta_leve`   | Losa con grieta fina magenta tenue       | `pixel art 32x32 top-down tile, dark void stone floor #0D0D1E, single thin magenta glowing crack #3D0035, inner glow effect, seamless edges, dark fantasy, transparent background`                  |
| B0-T03 | `abismo_suelo_grieta_radial` | Losa con grieta radial desde esquina     | `pixel art 32x32 top-down tile, dark stone floor #0D0D1E, radial magenta crack from corner, 3 crack lines with purple-magenta inner glow #6B0057, seamless, dark roguelite, transparent background` |
| B0-T04 | `abismo_suelo_placa`         | Losa más oscura, variación de suelo sano | `pixel art 32x32 top-down tile, very dark void stone plate #080810, slightly raised edges, no glow, solid dark, seamless tileable, dark fantasy, transparent background`                            |

### Decoraciones (2 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B0-D01 | `abismo_columna_rota` | Columna de piedra void rota, vista top-down | `pixel art 32x32 top-down view, broken stone pillar ruin, dark blue-black #0D0D1E, single magenta crack line on surface #3D0035, rubble at base, dark fantasy, transparent background, no white pixels` |
| B0-D02 | `abismo_fragmento_losa` | Fragmento de losa levantado en ángulo | `pixel art 32x32 top-down view, angled stone slab fragment, dark void stone #0D0D1E, slight drop shadow, magenta edge glow #6B0057 on fracture line, dark fantasy roguelite, transparent background` |

---

## Bioma 1 — Bosque de Espinas Cristalinas

**7 assets · Prioridad 3**

### Tiles de suelo (4 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B1-T01 | `cristal_suelo_tierra` | Tierra void compacta, base del bioma | `pixel art 32x32 top-down tile, dark purple-black void soil #1A0A2E, compact earth texture, seamless tileable, cold palette, dark fantasy roguelite, transparent background` |
| B1-T02 | `cristal_suelo_fragmentos` | Tierra con cristales rotos incrustados | `pixel art 32x32 top-down tile, dark purple soil #1A0A2E with shattered violet crystal shards embedded, crystal fragments glowing faintly #9966CC, seamless, dark fantasy, transparent background` |
| B1-T03 | `cristal_suelo_denso` | Suelo cubierto de fragmentos densos | `pixel art 32x32 top-down tile, dense shattered crystal floor, deep purple #4B0082 crystals over dark soil, lila inner glow #9966CC on crystal edges, seamless, cold dark palette, transparent background` |
| B1-T04 | `cristal_suelo_musgo` | Tierra con musgo void negro | `pixel art 32x32 top-down tile, dark purple void soil with black void moss patches #0A0014, cold palette, organic edge texture, seamless tileable, dark fantasy, transparent background` |

### Decoraciones (3 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B1-D01 | `cristal_espina_alta` | Cristal emergente alto, vista top-down | `pixel art 32x32 top-down view, tall violet crystal spike emerging from ground, dark purple #4B0082 body, lila inner glow #9966CC at tip, sharp irregular top, dark fantasy, transparent background` |
| B1-D02 | `cristal_racimo_bajo` | Racimo de cristales bajos | `pixel art 32x32 top-down view, cluster of small violet crystal spikes, dark purple #4B0082, subtle glow #9966CC, irregular heights, cold palette, dark fantasy roguelite, transparent background` |
| B1-D03 | `cristal_raiz_horizontal` | Raíz cristalizada horizontal | `pixel art 32x32 top-down view, horizontal crystallized root connecting two points, deep purple texture, faint violet glow on joints, organic meets mineral, dark fantasy, transparent background` |

---

## Bioma 2 — Pantano de Corrupción

**7 assets · Prioridad 3**

### Tiles de suelo (4 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B2-T01 | `pantano_suelo_barro` | Barro void base, textura irregular | `pixel art 32x32 top-down tile, dark corrupt swamp mud #070F07, irregular uneven surface texture, no grid pattern, organic, seamless tileable, dark fantasy roguelite, transparent background` |
| B2-T02 | `pantano_charco_borde` | Borde de charco tóxico | `pixel art 32x32 top-down tile, edge of toxic green puddle #1A4A00 on dark mud, toxic green glow border #39FF14 1px, seamless, dark poison aesthetic, transparent background` |
| B2-T03 | `pantano_charco_centro` | Centro de charco, superficie tóxica | `pixel art 32x32 top-down tile, center of toxic puddle #1A4A00, subtle surface texture, faint green inner glow #39FF14, seamless tileable, poison corrupt aesthetic, dark fantasy, transparent background` |
| B2-T04 | `pantano_grieta_verde` | Suelo con grieta llena de líquido tóxico | `pixel art 32x32 top-down tile, dark bog floor #070F07, crack filled with toxic green liquid #39FF14, thin bright edge glow, seamless, dark corrupt swamp, transparent background` |

### Decoraciones (3 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B2-D01 | `pantano_hongo_void` | Hongo void, cap negro con esporas verdes | `pixel art 32x32 top-down view, void mushroom top-down, black cap #050505, toxic green spore lines #39FF14 radiating from center, dark fantasy horror, transparent background` |
| B2-D02 | `pantano_raiz_podrida` | Raíz podrida sinuosa emergiendo | `pixel art 32x32 top-down view, rotting root emerging from swamp floor, dark green-black #0A1A00, irregular sinuous shape, subtle toxic edge #39FF14, dark fantasy, transparent background` |
| B2-D03 | `pantano_burbuja_gas` | Burbuja de gas congelada en suelo | `pixel art 32x32 top-down view, gas bubble frozen in bog floor, circular 6-8px, dark surface with faint toxic green glow #39FF14 center, dark fantasy, transparent background` |

---

## Bioma 3 — Forja del Caos

**7 assets · Prioridad 2**

### Tiles de suelo (4 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B3-T01 | `forja_suelo_obsidiana` | Obsidiana void base, textura de escoria | `pixel art 32x32 top-down tile, dark obsidian floor #1A0800, rough slag texture, no glow, seamless tileable, volcanic dark fantasy roguelite, transparent background` |
| B3-T02 | `forja_suelo_grieta_magma` | Obsidiana con grieta de magma void | `pixel art 32x32 top-down tile, dark obsidian #1A0800, magma crack #CC3300 with orange inner glow #FF6600, crack 2-3px wide, seamless, volcanic dark fantasy, transparent background` |
| B3-T03 | `forja_suelo_caliente` | Suelo radiante de calor sutil | `pixel art 32x32 top-down tile, hot obsidian floor #1A0800, subtle heat glow gradient from center, very faint orange-red #CC3300 bloom, seamless tileable, volcanic void, transparent background` |
| B3-T04 | `forja_suelo_escoria` | Suelo de escoria con protuberancias | `pixel art 32x32 top-down tile, rough slag and scoria floor #1A0800, small raised bumps and pits, very dark, seamless, volcanic chaos aesthetic, transparent background` |

### Decoraciones (3 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B3-D01 | `forja_pilar_escoria` | Pilar bajo de escoria con corona de glow | `pixel art 32x32 top-down view, short wide slag pillar top-down, dark obsidian #1A0800, orange-red corona glow #FF6600 around rim, volcanic dark fantasy, transparent background` |
| B3-D02 | `forja_charco_magma` | Charco de magma void estático | `pixel art 32x32 top-down view, static void magma pool, dark red-orange #CC3300, center glow #FF6600, no bright lava, muted volcanic palette, dark fantasy roguelite, transparent background` |
| B3-D03 | `forja_metal_incrustado` | Fragmento de metal void en suelo | `pixel art 32x32 top-down view, metal fragment embedded in obsidian floor, dark metallic surface #1A0800 with orange-red edge glint #FF6600, geometric irregular shape, volcanic, transparent background` |

---

## Bioma 4 — Núcleo del Devastador

**7 assets · Prioridad 2**

### Tiles de suelo (4 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B4-T01 | `nucleo_suelo_carne` | Membrana orgánica base | `pixel art 32x32 top-down tile, dark organic flesh floor #1A0005, membrane texture, curved irregular surface, no grid, seamless tileable, dark fantasy body horror roguelite, transparent background` |
| B4-T02 | `nucleo_suelo_vena` | Membrana con vena principal | `pixel art 32x32 top-down tile, dark flesh floor #1A0005, thick dark red vein #8B0000 crossing tile, crimson inner glow #CC0020, organic, seamless, body horror dark fantasy, transparent background` |
| B4-T03 | `nucleo_suelo_venas_densas` | Membrana con red de venas ramificadas | `pixel art 32x32 top-down tile, organic flesh #1A0005, dense branching vein network #8B0000, crimson glow #CC0020 on veins, seamless tileable, dark void horror, transparent background` |
| B4-T04 | `nucleo_suelo_tejido_sano` | Tejido más oscuro, mínimo contraste | `pixel art 32x32 top-down tile, very dark flesh void floor #0F0002, almost no texture, slightly organic, seamless, darker variation of void heart, transparent background` |

### Decoraciones (3 assets)

| ID | Nombre | Descripción | Prompt PixelLab |
|----|--------|-------------|-----------------|
| B4-D01 | `nucleo_garra_void` | Garra ósea void emergiendo | `pixel art 32x32 top-down view, bone-like void claw emerging from flesh floor, dark base #1A0005, crimson tip glow #CC0020, sharp irregular shape, dark fantasy body horror, transparent background` |
| B4-D02 | `nucleo_quiste_void` | Quiste bulboso con núcleo carmesí | `pixel art 32x32 top-down view, void cyst bulge on flesh floor, rounded bump, dark surface #1A0005, crimson inner light #CC0020 visible through skin, body horror roguelite, transparent background` |
| B4-D03 | `nucleo_altar_absorbido` | Altar de piedra siendo absorbido por el suelo | `pixel art 32x32 top-down view, stone altar being consumed by flesh floor, dark stone #0D0D1E half-submerged in #1A0005 flesh, crimson veins crawling over stone, dark horror fantasy, transparent background` |

---

## Resumen de produccion

| Bioma | Tiles suelo | Decoraciones | Total | Prioridad |
|-------|-------------|--------------|-------|-----------|
| Bioma 0 — Abismo Central | 4 | 2 | **6** | 1 — primero |
| Bioma 1 — Bosque Cristalino | 4 | 3 | **7** | 3 — último |
| Bioma 2 — Pantano Corrupción | 4 | 3 | **7** | 3 — último |
| Bioma 3 — Forja del Caos | 4 | 3 | **7** | 2 — segundo |
| Bioma 4 — Núcleo Devastador | 4 | 3 | **7** | 2 — segundo |
| **TOTAL** | **20** | **14** | **34** | |

---

## Flujo de trabajo por asset

1. Abre PixelLab MCP en terminal Claude
2. Usa `mcp__pixellab__create_topdown_tileset` o `mcp__pixellab__create_object` según el tipo
3. Descarga el PNG resultante (32×32)
4. Guarda en `assets/tiles/{bioma}/{nombre}.png`
5. Al terminar un bioma completo: combina tiles en sprite sheet con Aseprite o Piskel
6. Crea el mapa TMX en Tiled editor usando el sprite sheet
7. Carga en LibGDX: `TiledMap map = new TmxMapLoader().load("maps/bioma0.tmx")`

> [!warning] Los tiles de suelo deben ser seamless (tileable)
> Pedir siempre `seamless` o `seamless tileable` en el prompt. Un tile que no sea seamless crea líneas visibles en el mapa. Revisar antes de integrar.

> [!tip] Variantes de tile
> Con 4 tiles de suelo por bioma se puede crear variación visual suficiente usando distribución aleatoria ponderada (70% tile base, 20% tile variante, 10% tile detalle). No es necesario generar 20 variantes.

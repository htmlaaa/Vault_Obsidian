---
title: Sprint 7 — Assets de Diseño Pendientes
tags:
  - sprint-9
  - assets
  - pixellab
  - audio
  - pendiente
date: 2026-05-18
updated: 2026-05-19
status: parcial
sprint_target: sprint-9-design
sprint_stories: S9-01, S9-02
---

# Sprint 7 — Assets de Diseño Pendientes

> [!success] Animaciones completadas — 2026-05-19
> Todos los sprites de animación han sido reorganizados e implementados. Ver [[../Sesiones/Sesion-2026-05-19-Sprint9-Assets-Implementacion]].
> **Solo queda pendiente el audio.**

> [!abstract] Para ejecutar en otra terminal
> Este archivo es un **playbook autónomo** para una sesión separada de Claude Code. El código del juego está completo; solo faltan los **assets de audio**. Lee este documento de arriba abajo antes de empezar.

---

## Contexto rápido

| Campo | Valor |
|-------|-------|
| Proyecto | Kaosuarina — roguelite 2D (LibGDX Java, TFG) |
| Ruta base assets | `A_Game_Kaosuarina/assets/` |
| Estructura animaciones | `characters/{rol}/animations/{anim}/{dir}/frame_NNN.png` |
| Roles | `caballero`, `mago`, `shooter` |
| Dirs | `south`, `east`, `north`, `west` |
| Frame count IDLE | 5 frames (`frame_000` → `frame_004`) |
| Frame count WALK | 4 frames (ya existen — no tocar) |
| Frame count ATTACK | 6 frames |
| Nombre carpeta ATTACK | `swing` (caballero), `cast` (mago), `shoot` (shooter) |
| Tamaño canvas PixelLab | `size: 64` → sprites 64×64 px |
| Estilo visual | Pixel art, vista top-down, roguelite oscuro |

> [!warning] Diseños nuevos
> Los personajes tienen **diseños actualizados** respecto a las primeras generaciones de PixelLab. Consulta los prompts en [[../pixel/prompts/characters/caballero]], [[../pixel/prompts/characters/mago]] y [[../pixel/prompts/characters/tirador]] antes de generar. Usa esos prompts como referencia visual base.

> [!info] El código ya funciona
> `AnimationSheets.java` carga los frames automáticamente si están en la ruta correcta. Si falta un directorio, hace fallback a `SpriteSheets.getSprite()` sin crash. Solo necesitas colocar los PNGs en la ruta correcta.

---

## ~~Tarea 1 — Idle Caballero~~ ✅ Completado 2026-05-19

- [x] idle south, east, north, west — 4 frames cada dir — `caballero/animations/idle/`

---

## ~~Tarea 2 — Idle Mago~~ ✅ Completado 2026-05-19

- [x] idle south, east, north, west — 4 frames cada dir — `mago/animations/idle/`

---

## ~~Tarea 3 — Idle Shooter~~ ✅ Completado 2026-05-19

- [x] idle south, east, north, west — 4 frames cada dir — `shooter/animations/idle/`

---

## ~~Tarea 4 — Swing Caballero~~ ✅ Completado 2026-05-19

> [!success] Implementado con animación light (`swing/`) y heavy (`swing2/`)
> Ambas animaciones están en 4 dirs × 9 frames. El código usa `Anim.ATTACK` para light y `Anim.HEAVY_ATTACK` para heavy.

**Estado actual:** no existe ningún `swing/` para el Caballero. El juego usa fallback al sprite estático durante el ataque.

**Ruta destino:**
```
A_Game_Kaosuarina/assets/characters/caballero/animations/swing/{dir}/frame_000.png → frame_005.png
```
(6 frames, dirs: south, east, north, west)

**Descripción:** Swing de espada — levanta → golpe diagonal → recuperación. 6 frames.

### Checklist

- [ ] Verificar créditos PixelLab (`mcp__pixellab__get_balance`)
- [ ] Generar swing south (6 frames)
- [ ] Generar swing east, north, west (6 frames c/u)
- [ ] Guardar en rutas correctas

---

## Tarea 5 — Audio (BLOCKER para smoke test S7-06)

> [!danger] Bloqueante
> Sin estos archivos, los checks 12 y 13 del smoke test S7-06 fallan. El juego **no crashea** (AudioManager tiene try-catch), pero no habrá sonido.

**Ruta destino:** `A_Game_Kaosuarina/assets/audio/`

| Archivo | Evento | Fuente sugerida |
|---------|--------|-----------------|
| `shot.wav` | Disparo del jugador | Kenney Impact Sounds |
| `hit.wav` | Bala impacta enemigo | Kenney Impact Sounds |
| `death.wav` | Enemigo muere | Kenney Impact Sounds |
| `levelup.wav` | Jugador sube nivel | Kenney UI Audio |
| `boss.wav` | Boss aparece | Kenney Impact Sounds |
| `music.ogg` | Música de fondo (loop) | Kenney Music Jingles o OpenGameArt |

**Fuentes CC0 (sin licencia):**
- Kenney Impact Sounds: `kenney.nl/assets/impact-sounds`
- Kenney UI Audio: `kenney.nl/assets/ui-audio`
- Kenney Music Jingles: `kenney.nl/assets/music-jingles`
- OpenGameArt synthwave: `opengameart.org` (filtrar CC0)

### Checklist

- [ ] Descargar pack de SFX de Kenney
- [ ] Seleccionar y renombrar: `shot.wav`, `hit.wav`, `death.wav`, `levelup.wav`, `boss.wav`
- [ ] Obtener track de música loop y exportar como `music.ogg`
- [ ] Crear carpeta `A_Game_Kaosuarina/assets/audio/`
- [ ] Copiar todos los archivos a esa carpeta
- [ ] Ejecutar el juego y verificar que se escuchan
 
---

## Verificación final tras añadir assets

```bash
# Desde A_Game_Kaosuarina/
gradlew lwjgl3:run
```

### Checks manuales

- [ ] Caballero parado → idle animado (al menos en south)
- [ ] Caballero en movimiento → walk animado (4 dirs)
- [ ] Mago parado → idle animado (4 dirs)
- [ ] Shooter parado → idle animado (4 dirs)
- [ ] Música de fondo suena al iniciar partida
- [ ] Música para en Game Over
- [ ] Disparar produce `shot.wav`
- [ ] Enemigo muere produce `death.wav`
- [ ] Level up produce `levelup.wav`
- [ ] Boss spawn produce `boss.wav`
- [ ] No hay crash en ninguna dirección ni con ningún personaje

---

## Cómo usar PixelLab MCP en esta terminal

```
# Verificar balance antes de empezar
mcp__pixellab__get_balance

# Para generar animación de personaje:
mcp__pixellab__animate_character  (si el personaje ya existe en PixelLab)
mcp__pixellab__create_character_state  (para nuevos estados de animación)

# Si necesitas crear el personaje desde cero:
mcp__pixellab__create_character
```

> [!note] IDs de personajes
> Si los personajes ya fueron creados en sesiones anteriores, sus IDs están en [[../pixel/prompts/characters/caballero]], [[../pixel/prompts/characters/mago]] y [[../pixel/prompts/characters/tirador]]. Usa `mcp__pixellab__list_characters` para listar todos los disponibles.

---

## Referencias

- [[../Sesiones/Sesion-2026-05-17-Sprint7-Code]] — implementación de código Sprint 7
- [[../pixel/prompts/characters/caballero]] — prompt y diseño Caballero
- [[../pixel/prompts/characters/mago]] — prompt y diseño Mago
- [[../pixel/prompts/characters/tirador]] — prompt y diseño Tirador
- [[../Kausarina_GAME/Pendientes-Tecnicos]] — tracking de pendientes técnicos

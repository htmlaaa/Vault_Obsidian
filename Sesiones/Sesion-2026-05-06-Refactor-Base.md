---
title: Sesión 2026-05-06 — Refactor Base + Arte
tags:
  - sesion
  - refactor
  - arte
  - tecnico
date: 2026-05-06
---

# Sesión 2026-05-06 — Refactor Base + Arte

Dos bloques grandes: identidad visual de los 3 personajes y refactor técnico completo de la base del juego (pasos 1-6 de revisión Technical Director Tier 1).

---

## Bloque 1 — Arte y Diseño

### Personajes y paleta

Tres personajes definitivos con estética ==Verzerrt== — corrupción del void, cada personaje muestra **un elemento intacto + uno consumido**. Sprites 64×64 px generados con PixelLab MCP.

| Personaje | Color | Hex | Elemento corrupto |
|-----------|-------|-----|-------------------|
| Caballero | Teal | `#00E5CC` | Brazo-escudo disuelto |
| Mago | Púrpura | `#9B30FF` | Orbe de void |
| Shooter | Ámbar | `#FFB800` | Pistola de cristal ámbar |

> [!info] Regla de diseño compartida
> Tríada cromática perfecta en la rueda de color — ningún acento de personaje compite visualmente con otro en pantalla.

### Notas de diseño creadas

- [[Ambientacion]] — colores de arena, lenguaje visual de enemigos, 5 reglas de cohesión
- [[Cores-Sinergias]] — mecánica completa de los 3 cores y tabla de sinergias con upgrades
- [[Caballero-Kaos]] — diseño visual, paleta, prompt PixelLab usado
- [[Mago-Kaos]] — referencia base del estilo "Verzerrt" (preexistente)
- [[Shooter-Kaos]] — diseño asimétrico de dos pistolas, por qué ámbar, prompt usado
- [[Agentes-y-Skills]] — guía práctica: cuándo usar agentes vs inline, workflow PixelLab

---

## Bloque 2 — Refactor Técnico (Pasos 1-6)

> [!warning] Problema raíz
> 650+ instancias de GPU texture redundantes (una por entidad del pool), allocaciones `new Vector2` / `new Color` en hot path cada frame, `Gdx.input` acoplado dentro de `Player`, constantes mágicas `4f` y `5f` en `ColisionManager`.

### Archivos nuevos

| Archivo | Qué hace |
|---------|----------|
| `utils/SharedTextures.java` | 4 texturas compartidas. `load()` en `KaosuarinaGame.create()`, `dispose()` en `dispose()`. Elimina 650+ texturas redundantes. |
| `Systems/PlayerStats.java` | POJO de stats base por rol. Constructor default (Base neutral) + constructor paramétrico para roles específicos. |
| `roles/Role.java` | Stub con enum `Tipo` (BASE / CABALLERO / MAGO / SHOOTER) y factory methods `caballero()`, `mago()`, `shooter()` con sus `PlayerStats`. |
| `cores/CoreEffect.java` | Interfaz con hooks `onDamageReceived`, `onKill`, `onUpdate` — pendiente de implementar en [[Pasos-7-8-Pendientes]]. |

### Archivos modificados

#### `Bala.java` + `BalaEnemiga.java`
- Eliminado `Pixmap`/`Texture` propio de cada instancia
- `render()` usa `SharedTextures.getBala()` / `getBalaEnemiga()`
- `dispose()` convertido en no-op — textura la gestiona `SharedTextures`

#### `Enemy.java`
- Eliminado `Pixmap`/`Texture` propio
- `render()` usa `SharedTextures.getEnemyWhite()` + tinte por `batch.setColor()`
- `color` → `private final Color color = new Color()` + `color.set(r,g,b,a)` en `activate()` — elimina `new Color()` por spawn
- `tmp` → `private final Vector2 tmp = new Vector2()` — elimina 2 allocaciones por frame en tipo `SHOOTER` (órbita + disparo)

#### `Player.java` *(reescrito completo)*
- Eliminado `Gdx` — ya no lee mouse directamente
- Eliminado `Pixmap`/`Texture` → `SharedTextures.getPlayer()`
- Integrado `PlayerStats`: `stats.baseSpeed`, `stats.baseShootCooldown`, `stats.baseBulletCount`, `stats.bulletSpread`, `stats.maxHealth`
- `arenaDir` → `private final Vector2 arenaDir = new Vector2()` — elimina `new Vector2` en límite de arena
- `update()` acepta `float aimAngle` en lugar de leer `Gdx.input` internamente
- Constructor `Player(x, y)` delega en `Player(x, y, new PlayerStats())` — retrocompatible

#### `UpgradeManager.java`
- `aplicarUpgrade()` devuelve `int hpBonus` (20 si `VIDA_MAXIMA_UP`, 0 si no)
- `GameScreen` ya no necesita inspeccionar el tipo del upgrade para aplicar el bonus de vida

#### `Constants.java`
- Eliminado `PLAYER_SPEED` (duplicado obsoleto de `stats.baseSpeed`)
- Añadido `BALA_RADIO = 4f` y `BALA_ENEMIGA_RADIO = 5f`

#### `ColisionManager.java`
- Sustituidos magic numbers `4f` y `5f` por `Constants.BALA_RADIO` y `Constants.BALA_ENEMIGA_RADIO`

#### `GameScreen.java`
- Añadido campo `float aimAngle`
- `procesarInput()` computa `aimAngle` desde mouse y lo pasa a `player.update(delta, poolBalas, aimAngle)`
- `procesarLevelUp()` usa `int hpBonus = upgradeManager.aplicarUpgrade(seleccionado)` — sin check de tipo

#### `KaosuarinaGame.java`
- `create()` → `SharedTextures.load()` antes de `setScreen()`
- Añadido `dispose()` con `SharedTextures.dispose()`

### Resultado

> [!success] Build limpio
> `gradlew clean build` → exit 0. Todos los `.class` generados correctamente. Sin errores de compilación.

---

## Pendientes

Ver [[Pasos-7-8-Pendientes]] para los siguientes pasos:
- **Paso 7** — Implementar `CoreEffect` concretos + integrar `Role` en `Player` y `GameScreen`
- **Paso 8** — Pipeline de sprites reales: cargar PNGs de `PIXEL/characters/` en lugar del Pixmap rojo

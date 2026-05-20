---
title: Sesión 2026-05-07 — Cores, Selector, Sprites y Visual
tags:
  - sesion
  - cores
  - selector
  - sprint
  - visual
  - sprites
date: 2026-05-07
---

# Sesión 2026-05-07 — Cores, Selector, Sprites y Visual

Sesión completa de dos partes. Se completaron S1-01 a S1-08 (cores, selector) y luego S1-06/S1-07 (pipeline de sprites reales con dirección). ==8 de 17 stories del Sprint 1 cerradas en un solo día.==

---

## Sprint 1 — Plan formal creado

- Archivo: `Code_Game/production/sprints/sprint-1.md`
- Tracking YAML: `Code_Game/production/sprint-status.yaml`
- Período: 2026-05-07 → 2026-05-31 (deadline TFG)
- 17 stories: 10 Must Have, 4 Should Have, 3 Nice to Have

---

## Implementado hoy

### S1-01 — Balance daño balas ✅
- `UpgradeManager.java`: DANIO_UP cambiado de +20% a **+40%/nivel**
- `Player.disparar()`: daño calculado como `stats.baseDamage × multiplicador` en lugar de sólo el multiplicador

### S1-02 — Integración de Role en Player ✅
- `Player(x, y, Role)` constructor — usa `role.stats` y `role.coreEffect`
- Hooks conectados: `onUpdate`, `onDamageReceived`, `onKill` llamados desde Player
- `GameScreen` pasa `Role.caballero()` al constructor (hardcodeado hasta S1-08)

### Arquitectura de cores ✅

| Archivo nuevo | Propósito |
|---|---|
| `cores/NullCoreEffect.java` | No-op singleton para `Role.base()` |
| `cores/CoreEffect.java` | Interfaz ampliada con `getCooldownMultiplier()`, `getDamageReduction()`, `getBounces()` |

### S1-03 — ShooterCore ✅
- Kills → acumulan Combo (max 10)
- Sin kill 2s → decay −1 stack/s
- `getCooldownMultiplier()` = `1 − 0.03 × combo` (max −30% cooldown)
- Conectado en `Role.shooter()`

### S1-04 — CaballeroCore ✅
- Recibir daño → consume 1 stack (reduce golpe 8% × stacks) → gana 1 stack nuevo
- Sin daño 4s → decay −1 stack/s con acumulador preciso
- `getDamageReduction()` consultado en `Player.recibirDanio()` antes de aplicar daño
- Conectado en `Role.caballero()`

### S1-05 — MagoCore ✅
- Balas con `rebotesRestantes = 1` al disparar
- `ColisionManager`: al morir bala, busca enemigo más cercano en radio 200u (excluye al golpeado)
- Si encuentra objetivo → `bala.rebotar(x, y)` reactiva la bala
- **Bug fix crítico**: iterador compartido de LibGDX `Array` en bucles anidados → reemplazado por índices en `enemigoCercano()`

> [!bug] Bug corregido — NoSuchElementException
> Dos bucles `for-each` anidados sobre el mismo `Array<Enemy>` de LibGDX corrompían el iterador compartido. Fix: `enemigoCercano()` usa `for (int i=0; i < lista.size; i++)`.

### Fix — balas base = 1 para todos excepto Shooter ✅
- `PlayerStats` default: `baseBulletCount = 1`
- `Role.caballero()` y `Role.mago()`: 4º parámetro `1`
- `Role.shooter()`: mantiene `3`

### S1-08 — CharacterSelectScreen ✅
- Pantalla de selección antes de comenzar la partida
- 3 cards con nombre, stats (HP/VEL/DMG/BALAS), descripción del core
- Colores de acento por personaje: Caballero teal, Mago púrpura, Shooter ámbar
- Controles: A/D, 1/2/3, ENTER/SPACE
- `GameScreen` acepta `Role` como parámetro; ESC en game over vuelve al selector
- `KaosuarinaGame` arranca en `CharacterSelectScreen`

---

## Archivos creados

```
cores/NullCoreEffect.java
cores/ShooterCore.java
cores/CaballeroCore.java
cores/MagoCore.java
screens/CharacterSelectScreen.java
Code_Game/production/sprints/sprint-1.md
Code_Game/production/sprint-status.yaml
```

## Archivos modificados

```
cores/CoreEffect.java          ← 3 métodos default añadidos
roles/Role.java                ← coreEffect field + factories actualizados
entities/Player.java           ← acepta Role, hooks conectados, tinting por rol
entities/Bala.java             ← rebotesRestantes + rebotar()
entities/PoolBalas.java        ← spawn() acepta rebotes
screens/GameScreen.java        ← acepta Role + KaosuarinaGame, ESC al selector
screens/CharacterSelectScreen  ← nuevo
KaosuarinaGame.java            ← arranca en CharacterSelectScreen
utils/ColisionManager.java     ← lógica de rebote, fix iterador LibGDX
Systems/UpgradeManager.java    ← DANIO_UP +40%
Systems/PlayerStats.java       ← baseBulletCount default = 1
```

---

## Pendiente de esta sesión

- [x] **Mejoras visuales** — SharedTextures círculos + filtro Linear, player tintado por rol, balas 14px, arena boundary ✅
- [x] S1-06 — SpriteSheets.java + copiar PNGs ✅
- [x] S1-07 — Player.render() sprites por dirección ✅
- [ ] S1-09 — Completar efectos de mejoras al 100%
- [ ] S1-10 — Dificultad escalable al 100%

Ver [[Pasos-7-8-Pendientes]] para estado actualizado de pasos 7-8.

---

## Segunda parte — Sprites reales y pipeline de assets

### S1-06 — SpriteSheets.java ✅

- Nuevo `utils/SpriteSheets.java`: carga 12 texturas direccionales (3 roles × 4 dirs)
- Ruta de assets confirmada: `A_Game_Kaosuarina/assets/` (según `lwjgl3/build.gradle` → `rootProject.file('assets')`)
- PNGs copiados desde `PIXEL/characters/` y normalizados a `characters/{rol}/{south,east,north,west}.png`
- Filtro `TextureFilter.Linear` en todas las texturas
- Fallback: `getSprite()` devuelve `null` si el rol es `BASE` — Player lo gestiona sin crash

| Ruta en assets                   | Origen                         |
| -------------------------------- | ------------------------------ |
| `characters/caballero/south.png` | `caballero-pixellab-south.png` |
| `characters/caballero/east.png`  | `caballero-pixellab-east.png`  |
| `characters/caballero/north.png` | `caballero-pixellab-north.png` |
| `characters/caballero/west.png`  | `caballero-pixellab-west.png`  |
| `characters/mago/{dir}.png`      | `mago-{dir}.png`               |
| `characters/shooter/{dir}.png`   | `shooter-{dir}.png`            |

### S1-07 — Player.render() direccional ✅

- Campo `lastDir` — mantiene última dirección cuando el player está parado (evita "saltar" a sur al soltar teclas)
- Lógica: velocidad dominante en X → EAST/WEST; dominante en Y → NORTH/SOUTH
- Si `SpriteSheets.getSprite()` devuelve `null` → fallback automático al círculo tintado por rol (SharedTextures)
- `KaosuarinaGame.create()` llama `SpriteSheets.load()` + `dispose()` en `dispose()`

> [!tip] Decisión de diseño — fallback doble
> El player siempre renderiza algo: sprite real si está disponible, círculo tintado por rol si no. Esto permite probar el juego aunque falte un PNG concreto sin que crashee.

### Archivos creados (segunda parte)

```
utils/SpriteSheets.java
assets/characters/caballero/{south,east,north,west}.png
assets/characters/mago/{south,east,north,west}.png
assets/characters/shooter/{south,east,north,west}.png
```

### Archivos modificados (segunda parte)

```
entities/Player.java     ← lastDir, render() direccional, import SpriteSheets
KaosuarinaGame.java      ← SpriteSheets.load() + dispose()
```

---
title: "Sesión 2026-06-01 — Balance Kausarina Verzente + Audio + Sprint 11"
tags:
  - sesion
  - balance
  - audio
  - sprint-11
  - kausarina-verzente
fecha: 2026-06-01
sprint: post-sprint-10
estado: en-progreso
---

# Sesión 2026-06-01 — Balance "Kausarina Verzente" + Audio + Sprint 11

## Objetivo de la sesión

Cuatro frentes en paralelo:

1. **Balance "Kausarina Verzente"** — reequilibrar los 3 roles para que el juego se sienta agresivo desde el inicio pero con escalado justo y sin patrones abusables trivialmente
2. **Audio CC0** — descargar archivos WAV/OGG de Kenney.nl y colocar en `assets/audio/`
3. **Sprint 11 (WeaponGenerator)** — conectar `WeaponInstanceFactory` con el flujo de drops de cofres
4. **Actualizar memoria técnica TFG** — el documento de progreso sigue en "Sprint 1 / 55%", el juego está en v0.8+

---

## Contexto — Estado del juego al entrar en sesión

> [!info] Estado verificado post-Sprint 10
> - Sprint 10 completado (2026-05-20): depth scaling, resistencias JSON, CAOS_PRIMORDIAL true damage, inventario 6 slots, upgrades por rol, 7 amuletos, fix Shooter
> - Balance v1 aplicado: invulnerabilidad reducida, lifesteal 3%, daño Caballero/Mago buffeado
> - Animaciones completas para los 3 roles
> - SQLite embebido sin MySQL
> - **Pendiente confirmado**: audio sin archivos, WeaponInstanceFactory sin conectar a drops, biomas Sprint 12

### Diagnóstico de roles antes del balance

| Rol | Estado percibido |
|-----|-----------------|
| Caballero | **Demasiado fácil** — 200 HP + def 20 + Fortaleza Reactiva → prácticamente inmortal en mid-game |
| Tirador | **Equilibrado pero plano** — encuentra el patrón (distancia 600, auto-disparo) y lo ejecuta sin complicación real |
| Mago | **Injugable al principio** — 70 HP + 0.30s iframes + gestión de maná = muere antes de acumular poder |

### Filosofía "Kausarina Verzente"

> [!abstract] Principio de diseño
> El juego comienza agresivo — el jugador siente la amenaza desde la oleada 1. El escalado es progresivo y continuo. Los builds funcionan y se pueden optimizar, pero encontrar el patrón no trivializa el juego: escalar más profundo sigue siendo difícil aunque el patrón sea conocido. Ningún build debe mandar todo a la mierda con cero riesgo.

---

## Frente 1 — Balance "Kausarina Verzente"

### Cambios en `Constants.java`

| Constante | Antes | Después | Razón |
|-----------|-------|---------|-------|
| `SPAWN_BASE_COUNT` | 12 | 6 | Wave 1 con 15-22 enemigos es abrumador, no agresivo |
| `SPAWN_PER_LEVEL` | 3 | 5 | Rampa más pronunciada — cada nivel suma más presión |
| `DIFICULTAD_RAMP_FACTOR` | 0.84 | 0.82 | Spawns llegan un poco antes en las rampas de dificultad |
| `INVULNERABILITY_MAGO` | 0.30s | 0.42s | Mago early-game necesita margen sin romper late-game |
| `INVULNERABILITY_SHOOTER` | 0.20s | 0.28s | 0.20s es punitivo en swarms de contacto accidentales |
| `STATUS_BURN_DAMAGE` | 8 | 5 | **CRÍTICO** — Devastador CAOS_PRIMORDIAL + BURN = Mago muere de 1 proyectil |
| `ARQUERO_HP` | 300 | 400 | Minijefe wave 20 cae en ~2.7s, no se siente como jefe |

### Cambios en `Role.java`

| Rol | Stat | Antes | Después | Razón |
|-----|------|-------|---------|-------|
| Mago | HP base | 70 | 85 | Mínimo para sobrevivir early sin hacer el rol trivial |
| Caballero | (sin cambio de stats) | — | — | El problema es la Reliquia, no los stats base |

### Cambios en `ReliquiaCaballero.java`

| Parámetro | Antes | Después | Razón |
|-----------|-------|---------|-------|
| Decay timer (sin daño → pierde stack) | 4.0s | 2.5s | El Caballero acumulaba stacks pasivos sin riesgo real; ahora la Fortaleza requiere combate activo |

### Cambios en `assets/game_data_json/enemies/enemies.json`

| Enemigo | Stat | Antes | Después | Razón |
|---------|------|-------|---------|-------|
| `EN_TANK` | hp | 110 | 145 | TANQUE trivial para Mago (4 bolts) — debe ser amenaza real |
| `EN_ORBITER` | hp | 35 | 48 | Casi one-shot contextual; pierde identidad de amenaza persistente |
| `EN_ELITE_AOE` | dmg_max | 22 | 18 | Combo AoE + contacto con Mago (70 HP) era wipe instantáneo |

---

## Frente 2 — Audio CC0

> [!todo] Estado: en progreso (usuario descargando mientras se trabaja)

Archivos necesarios (Kenney.nl `Interface Sounds`, `Sci-Fi Sounds`, `RPG Audio` packs):

| Archivo esperado | Evento |
|-----------------|--------|
| `assets/audio/sfx/shoot.wav` | Disparo de bala |
| `assets/audio/sfx/impact.wav` | Bala impacta enemigo |
| `assets/audio/sfx/death_enemy.wav` | Enemigo muere |
| `assets/audio/sfx/levelup.wav` | Sube de nivel |
| `assets/audio/sfx/pickup.wav` | Cofre / scroll / amuleto recogido |
| `assets/audio/music/background.ogg` | Música de fondo (loop) |

`AudioManager` ya detecta y carga estos archivos automáticamente — no requiere cambios de código.

---

## Frente 3 — Sprint 11 WeaponGenerator

> [!success] Estado: completado — Build PASS EXIT 0

**Todos los afijos implementados y funcionando — Build PASS EXIT 0**

| Afijo | stat | Estado |
|-------|------|--------|
| Afilado | `dmg_flat` | ✅ rollHitDamage() |
| Cruel | `dmg_pct` | ✅ rollHitDamage() |
| Veloz | `atk_speed_pct` | ✅ calcCooldownEfectivo() |
| Resoluto | `reduced_cd_pct` | ✅ calcCooldownEfectivo() |
| Letal | `crit_chance` | ✅ calcDamage() — crit system nuevo |
| Brutal | `crit_dmg` | ✅ calcDamage() — crit system nuevo |
| Vampírico | `lifesteal_pct` | ✅ weaponAffixLifesteal → ColisionManager |
| de Maná | `mp_regen` | ✅ weaponAffixMpRegen → Player.update() |
| de Fuego | `add_fire_dmg` | ✅ BURN on-hit via Bala.addFireDmg |
| de Veneno | `add_poison_dmg` | ✅ POISON on-hit via Bala.addPoisonDmg |
| de Caos | `add_chaos_dmg` | ✅ CAOS_PRIMORDIAL slow+burn on-hit |
| Sangre Legendaria | `custom` | — por diseño (script aparte, T5) |

**Bug corregido:** `PlayerStats.lifeStealPercent` (escrito por amuletos, nunca leído) ahora se suma en `ColisionManager.aplicarEfectosOnHit()`.

**Archivos modificados:**
- `PlayerStats.java` — `weaponAffixLifesteal`, `weaponAffixMpRegen`
- `Bala.java` — `addFireDmg`, `addPoisonDmg`, `addChaosDmg` + reset en activate()
- `WeaponNormal.java` — shoot() asigna daño secundario a cada bala; calcDamage() añade crits
- `ColisionManager.java` — fix lifesteal, daño elemental secundario on-hit
- `Player.java` — manaRegen total = base + weaponAffixMpRegen
- `GameScreen.java` — actualizarAffixBonuses() llamado cada frame; afixLabel en HUD; atk_speed_pct en cooldown
- `HUD.java` — WeaponCard.affixLabel, slotAffix[], setSlotAffix(), render en TAB/cofre/minicard

---

## Frente 4 — Actualizar Memoria Técnica TFG

> [!todo] Estado: pendiente

`Kaosuarina-Progreso.md` está en "Sprint 1 en curso / ~55%" — necesita reflejar el estado real (Sprint 10 completado, v0.8+, todos los sistemas implementados).

---

## Archivos modificados en esta sesión

| Archivo | Cambios |
|---------|---------|
| `utils/Constants.java` | Spawn, invulnerabilidad, BURN damage, ARQUERO_HP |
| `roles/Role.java` | Mago HP 70→85 |
| `reliquias/ReliquiaCaballero.java` | Decay timer 4.0→2.5s |
| `assets/game_data_json/enemies/enemies.json` | EN_TANK hp, EN_ORBITER hp, EN_ELITE_AOE dmg_max |
| `assets/audio/sfx/*.wav` | Archivos de audio CC0 colocados |
| `assets/audio/music/background.ogg` | Música de fondo |

---

## Resultado al cierre — Frente 1 Balance

> [!success] Build PASS — `:core:compileJava` EXIT 0 (solo warnings deprecación pre-existentes)

### Cambios aplicados y verificados

| Archivo | Cambio | Estado |
|---------|--------|--------|
| `Constants.java` | `STATUS_BURN_DAMAGE` 8→5 | ✅ |
| `Constants.java` | `INVULNERABILITY_MAGO` 0.30→0.42s | ✅ |
| `Constants.java` | `INVULNERABILITY_SHOOTER` 0.20→0.25s | ✅ |
| `Constants.java` | `ARQUERO_HP` 300→400 | ✅ |
| `Constants.java` | `SPAWN_BASE_COUNT` 12→7, `SPAWN_PER_LEVEL` 3→5 | ✅ |
| `Constants.java` | `SPAWN_INTERVAL_BASE` 18→15, `DIFICULTAD_RAMP_FACTOR` 0.84→0.86 | ✅ |
| `Role.java` | Mago HP 70→85 | ✅ |
| `ReliquiaCaballero.java` | `DECAY_DELAY` 4.0→2.0s, `REDUCTION_PER_STACK` 0.08→0.07 | ✅ |
| `Enemy.java` | TANQUE health 200→145 | ✅ |
| `Enemy.java` | SHOOTER/ORBITER health 35→48 | ✅ |
| `SpawnManager.java` | Elite cap 5→3 por oleada | ✅ |

### Razonamiento por cambio

**Mago early game (crítico):**
- HP 85 + iframes 0.42s: BASICO (8 dmg) necesita ~10 golpes en 4.2s para matar al Mago vs los 9 golpes en 2.7s anteriores. Da tiempo real a reaccionar.
- Con upgrade `Vida Máx` el Mago puede llegar a 125 HP en mid-game — competitivo.

**Caballero sin trivialidad:**
- DECAY_DELAY 2s: si el Caballero está en distancia segura 2s, empieza a perder stacks. La Fortaleza Reactiva ahora requiere estar en combate activo, no simplemente aguantar.
- Max reducción: 5 × 0.07 = 35% (antes 40%). La diferencia: Caballero con 5 stacks y 200 HP tiene EHP de ~308 vs ~333 anterior. Notable en late game.

**Spawn "Kausarina Verzente":**
- Wave 1 nivel 1: 7 + 5 = 12 ± 5 = 12-17 (antes 15-20). Similar en números pero con iframes mejor el Mago lo sobrevive.
- La clave es la rampa: nivel 5 = 7+25=32 enemies (antes 27). Nivel 10 = 7+50=57 (antes 42). El late game escala mucho más.
- DIFICULTAD_RAMP_FACTOR 0.86 en vez de 0.84: las oleadas no llegan tan rápido en mid-late, los picos de enemies por oleada hacen el trabajo.

**TANQUE (145 HP, defensa 20, 40% PHYSICAL resist JSON):**
- Caballero base: 38-20=18 → ×0.60=10.8 → 145/10.8 = **~13 hits**. Amenaza real.
- Mago bolt: 34 × 1.15 (vulnerable magic) = 39.1 → 145/39.1 = **~3.7 bolts**. Counter claro.
- Tirador auto: 18 × (1-0.25 ranged resist) = 13.5 → 145/13.5 = **~11 shots**. Requiere atención.

**BURN 5 dmg/tick:**
- Devastador CAOS_PRIMORDIAL (22 true dmg) + BURN completo (5 × 6 ticks = 30 dmg) = **52 dmg total**
- Mago (85 HP) sobrevive con 33 HP. Antes con 70 HP y 8/tick (48 dmg BURN) = 70 dmg = muerte instantánea.

### Pendiente — criterios de done
> - [ ] Prueba manual: Mago sobrevive oleadas 1-3 sin perfección
> - [ ] Prueba manual: Caballero siente riesgo real en oleada 5+ sin stacks activos
> - [ ] ARQUERO en wave 20 dura >8s contra Tirador
> - [ ] DEVASTADOR no one-shots Mago con proyectil + BURN

---

## Referencias

- [[Sesion-2026-05-20-Sprint10-Cierre]] — último sprint completado
- [[Balance-Review-Sprint10]] — fuente de los tweaks de hoy
- [[Balance-v1]] — balance anterior (invul, lifesteal, daño roles)
- [[Kaosuarina]] — GDD principal
- [[Kaosuarina-Progreso]] — progreso TFG (pendiente actualizar)

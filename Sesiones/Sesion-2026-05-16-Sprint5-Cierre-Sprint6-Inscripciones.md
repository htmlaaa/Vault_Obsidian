---
title: Sesión 2026-05-16 — Cierre Sprint 5 + Plan Sprint 6 + S6-01 Inscripciones
tags:
  - sesion
  - sprint5
  - sprint6
  - inscripciones
  - armas
  - hud
date: 2026-05-16
---

# Sesión 2026-05-16 — Cierre Sprint 5 + Plan Sprint 6 + S6-01 Inscripciones

Sesión de continuación directa del Sprint 5. Se completaron las últimas stories (S5-06, S5-07, S5-08), se corrigió un bug visual de las skills de área, se planificó y documentó el Sprint 6 completo, y se implementó S6-01 (sistema de inscripciones base).

---

## Lo hecho hoy

### Sprint 5 — Cierre completo ✅

> [!success] Sprint 5 completado — 2026-05-16
> Sistema de armas operativo: 6 armas (3 Normal, 3 Skill), 2 slots equipables, cofres en mapa, HUD con cooldown arc, maná universal con regen diferenciada, WeaponPool singleton, stat baseline §14.

#### S5-06 — Sistema de cofres + UI de intercambio de arma ✅

Spawn de cofre en mapa cada 2-3 oleadas. Al recogerlo con el jugador cerca aparece un menú de intercambio con timeout de 5 segundos.

**Campos añadidos a `GameScreen.java`:**

| Campo | Tipo | Uso |
|-------|------|-----|
| `waveCount` | `int` | Contador de oleadas |
| `nextChestWave` | `int` | Oleada en la que aparece el siguiente cofre |
| `chestX/Y` | `float` | Posición del cofre activo |
| `chestActive` | `boolean` | Si hay cofre en el mapa |
| `pendingPickupWeapon` | `Weapon` | Arma del cofre recogido |
| `swapMenuActive` | `boolean` | Si el menú está abierto |
| `swapMenuTimer` | `float` | Countdown del timeout |

**Métodos nuevos:** `spawnChest()`, `checkPlayerChestCollision()`, `triggerWeaponPickup(Weapon)`, `updateSwapMenu(delta)`, `closeSwapMenu()`

**Constantes añadidas a `Constants.java`:**
```
CHEST_SPAWN_INTERVAL_MIN = 2
CHEST_SPAWN_INTERVAL_MAX = 3
WEAPON_PICKUP_RADIUS     = 40f
WEAPON_SWAP_TIMEOUT      = 5f
```

---

#### S5-07 — HUD slots de arma con overlay de cooldown ✅

Dos slots de 48×48 px centrados en la parte inferior del HUD, sobre la barra de XP.

**Renderizado:**
- Fondo gris oscuro `(0.22, 0.22, 0.22)`
- Arc de enfriamiento dibujado con `ShapeRenderer.arc(cx, cy, r, 90f - sweep, sweep)` → sentido horario desde las 12
- Borde **rojo** si `manaLocked` (maná insuficiente para la skill), gris si ready
- Nombre del arma en fuente escala 1.1f

**Campos añadidos a `HUD.java`:**
```java
String[]  slotName      = {"-", "-"}
boolean[] slotIsSkill
float[]   slotCdFraction
boolean[] slotManaLocked
```

**Métodos nuevos:** `setWeaponSlot(int, String, boolean, float, boolean)`, `renderWeaponSlots()`, `renderSwapMenu(...)`

---

#### Bug corregido — Visual de área MARTILLO_JUICIO / TOMO_CAOS ✅

> [!bug] Bug reportado por el usuario
> Las skills de área (MARTILLO_JUICIO y TOMO_CAOS) aplicaban daño correctamente pero no mostraban ningún indicador visual del radio de explosión. El jugador no podía ver dónde impactaba.

**Causa:** `WeaponSkill.activate()` llamaba a `ColisionManager.comprobarBlast()` pero no había código de renderizado del anillo.

**Solución:** Sistema `blastEffect` añadido a `GameScreen`:

| Campo nuevo | Tipo | Valor |
|-------------|------|-------|
| `blastEffectTimer` | `float` | 0.3s de duración |
| `blastEffectX/Y` | `float` | Centro del blast |
| `blastEffectRadius` | `float` | 200f Martillo / 250f Tomo |
| `blastEffectIsCaos` | `boolean` | Color morado vs dorado |

Renderizado en `renderizar()` con `ShapeRenderer.ShapeType.Line`, `glLineWidth(3)`:
- **MARTILLO_JUICIO** → anillo dorado `(1f, 0.85f, 0.1f)`
- **TOMO_CAOS** → anillo morado `(0.8f, 0.2f, 1f)`

---

#### S5-08 — Documentación + Stat Baseline §14 ✅

Archivo: `Code_Game/design/gdd/00-stat-baseline.md` — Sección §14 "Stats v0.5 — Post Sprint 5" añadida.

**Contenido de §14:**

| Tabla | Descripción |
|-------|-------------|
| Catálogo de armas | 6 armas con WeaponType, Cat, DamageType, baseDmg, cooldown, manaCost, skillCD, Afinidad |
| Stat bonuses por arma | hpBonus, defBonus, resMagBonus, manaMaxBonus, moveSpeedMult |
| Maná base por rol | Caballero=40, Mago=120 (+2.0/s), Tirador=30 |
| WeaponNormal patterns | speed, range por arma |
| WeaponSkill effects | blast radius, pierce |

Sprint 5 marcado `status: done` + `completed: 2026-05-16`.

---

### Sprint 6 — Planificado y documentado ✅

Plan completo en `Code_Game/production/sprints/sprint-6.md`.

**Goal**: Inscripciones de arma (8 modificadores), amuletos recogibles (2 MVP), economía de maná completa (Overkill + Cicatriz), primer minijefe Guardián (oleada 10), menú principal.

**Periodo**: 17 may – 31 may · Capacidad: 10 días · Estimado: 8.0 días

| Story | Descripción | Días | Deps |
|-------|-------------|-----:|------|
| S6-01 | `Inscription` interfaz + 8 implementaciones | 1.5 | — |
| S6-02 | `InscriptionPool` + scroll pickup + UI asignación (1/2/X) | 1.0 | S6-01 |
| S6-03 | `AmuletType` + `AmuletPool` + Sed de Sangre + Guardián de Arena | 1.0 | — |
| S6-04 | Overkill maná `floor(excess/20)` + Cicatriz de Combate (Caballero) | 0.5 | — |
| S6-05 | Minijefe Guardián (HP 800, shockwave 150u cada 3s, drop scroll) | 2.0 | S6-01, S6-02 |
| S6-06 | `MainMenuScreen.java` (ENTER→CharSelect, ESC→exit) | 0.5 | — |
| S6-07 | HUD nombre inscripción en slot + display core activo (ARM/CMB) | 0.5 | S6-01 |
| S6-08 | `run_arma` BD + smoke test 14 checks + stat baseline §15 | 1.0 | todas |

---

### S6-01 — Interfaz Inscription + 8 implementaciones ✅

Primera story del Sprint 6, implementada en esta sesión.

#### Archivos NUEVOS creados

| Archivo | Descripción |
|---------|-------------|
| `weapons/Inscription.java` | Interfaz base: `onHit`, `damageMult`, `extraPierce`, `bypassesDefense`, `getName` |
| `weapons/inscriptions/EchoQueue.java` | Cola estática para impactos retardados (ECO) |
| `weapons/inscriptions/InscripcionIgnea.java` | IGN — aplica BURN 3s al impactar |
| `weapons/inscriptions/InscripcionVampirica.java` | VAM — roba `max(1, dmg/10)` HP; damageMult 0.9 |
| `weapons/inscriptions/InscripcionDelEco.java` | ECO — retrasa 40% del daño 2 segundos |
| `weapons/inscriptions/InscripcionDeVacio.java` | VAC — ignora defensa total; damageMult 0.75 |
| `weapons/inscriptions/InscripcionSismica.java` | SIS — stun 0.3s al impactar |
| `weapons/inscriptions/InscripcionDelCaos.java` | CAO — efecto aleatorio: BURN / POISON / curar 5 / +5 maná |
| `weapons/inscriptions/InscripcionResonante.java` | RES — stacks por objetivo (max 10); damageMult 1.0 + 0.05×stacks |
| `weapons/inscriptions/InscripcionEspectral.java` | ESP — extraPierce +1 |

#### Archivos MODIFICADOS

| Archivo | Cambio |
|---------|--------|
| `weapons/Weapon.java` | Campo `public Inscription inscription = null` |
| `entities/Bala.java` | Campo `public boolean ignoresDefense = false`; reset en `activate()` |
| `entities/Enemy.java` | Campo `public float stunTimer = 0f`; skip movimiento si > 0 en `update()` |
| `utils/Constants.java` | `STUN_DURATION_SISMICA = 0.3f`, `CICATRIZ_DIVISOR = 4` |
| `entities/PoolBalas.java` | Nuevo `spawnReturning(...)` que devuelve `Bala` (para setear `ignoresDefense`) |
| `weapons/WeaponNormal.java` | Aplica `damageMult`, `extraPierce`, `bypassesDefense` de la inscripción al disparar |
| `weapons/WeaponSkill.java` | Mismo tratamiento de inscripción en `activate()` |
| `utils/ColisionManager.java` | `calcularDaño` acepta `ignoresDefense`; llama `inscription.onHit()` tras impacto |
| `entities/Player.java` | `getInscriptionForWeaponType(WeaponType)` — helper para ColisionManager |
| `screens/GameScreen.java` | `EchoQueue.tick(delta)` en `actualizarJuego`; `EchoQueue.clear()` en `inicializar` |

#### Tabla de inscripciones implementadas

| Inscripción | `getName()` | `damageMult()` | `extraPierce()` | `bypassesDefense()` | `onHit()` |
|------------|-------------|----------------|-----------------|---------------------|-----------|
| Ígnea | IGN | 1.0 | 0 | false | BURN 3s |
| Vampírica | VAM | 0.9 | 0 | false | curar dmg/10 |
| Del Eco | ECO | 1.0 | 0 | false | cola 40% dmg en 2s |
| De Vacío | VAC | 0.75 | 0 | **true** | — |
| Sísmica | SIS | 1.0 | 0 | false | stunTimer 0.3s |
| Del Caos | CAO | 1.0 | 0 | false | aleatorio ×4 |
| Resonante | RES | 1.0 + 0.05×stacks | 0 | false | acumula stacks |
| Espectral | ESP | 1.0 | **1** | false | — |

> [!note] Instanciación
> Las inscripciones **no son singletons**. `InscriptionPool.getRandom()` (S6-02) hace `new InscripcionX()` en cada llamada. `InscripcionResonante` y `InscripcionDelEco` tienen estado mutable por instancia.

---

## Errores corregidos

| Bug | Síntoma | Causa | Fix |
|-----|---------|-------|-----|
| Sin visual en skills de área | MARTILLO/TOMO dañaban pero no mostraban radio | No había renderizado del blast ring | `blastEffectTimer` + `ShapeRenderer.circle` en `renderizar()` |
| `calcularDaño` sin parámetro `ignoresDefense` | InscripcionDeVacio no podía bypassear defensa | Firma de método sin el flag | Overload con `boolean ignoresDefense` |

---

## Estado del proyecto al cierre de sesión

```
v0.1 Sprint 1  ✅  Movimiento, auto-fire, 4 enemigos, upgrades básicos, HUD, game over
v0.2 Sprint 2  ✅  Stats RPG, roles, cores, selector de personaje, reescalado de daño
v0.3 Sprint 3  ✅  StatusEffects (BURN/POISON), enemigos MALDITO/ESPECTRAL, regen Mago
v0.4 Sprint 4  ✅  Upgrades elementales, life steal, A_DISTANCIA pen., DB MySQL
v0.5 Sprint 5  ✅  Sistema de armas (6 armas, 2 slots), maná universal, cofres, HUD cooldown
v0.6 Sprint 6  🔄  S6-01 ✅ · S6-02→S6-08 pendientes
```

---

## Pendientes Sprint 6

> [!todo] S6-02 — Scroll de Inscripción
> `InscriptionPool.getRandom()` + spawn en mapa cada 3-4 oleadas + menú de asignación con teclas 1/2/X.

> [!todo] S6-03 — Amuletos
> `AmuletType` enum + `AmuletPool` + Sed de Sangre (+5 maná extra/kill) + Guardián de la Arena (escudo periódico).

> [!todo] S6-04 — Overkill + Cicatriz de Combate
> `floor(excess_damage / 20)` maná al matar con overkill. Cicatriz: Reliquia Caballero devuelve `floor(dmg_recibido / 4)` como maná.

> [!todo] S6-05 — Minijefe Guardián
> Nuevo tipo `GUARDIAN` en `Enemy.Tipo`. HP=800, speed=60, size=64, def=30. Shockwave 150u cada 3s reutilizando `blastEffectTimer`. Dropea scroll al morir. Aparece en oleada 10.

> [!todo] S6-06 — Menú Principal
> `MainMenuScreen.java` con ENTER→CharacterSelectScreen, ESC→exit. KaosuarinaGame.create() lo muestra primero.

> [!todo] S6-07 — HUD inscripción + core display
> Nombre inscripción (4 chars) en cada slot de arma. Core activo: "ARM X" para Caballero, "CMB X" para Tirador.

> [!todo] S6-08 — BD + smoke test + baseline §15
> Tabla `run_arma` (`run_id`, `slot`, `arma_tipo`, `inscripcion`). Smoke test 14 checks. `00-stat-baseline.md` §15.

> [!warning] SQL pendiente — ejecutar manualmente en MySQL
> ```sql
> CREATE TABLE IF NOT EXISTS run_arma (
>     id INT AUTO_INCREMENT PRIMARY KEY,
>     run_id INT NOT NULL,
>     slot TINYINT NOT NULL,
>     arma_tipo VARCHAR(30),
>     inscripcion VARCHAR(10),
>     FOREIGN KEY (run_id) REFERENCES run(id) ON DELETE CASCADE
> );
> ```

---

## Referencias de sesión

- [[Sesion-2026-05-13-Sprint4-Cierre-Sprint5-Plan]] — sesión anterior
- [[../Kausarina_GAME/Solucionadas/disenados/Inscripciones-Sistema]] — diseño original de inscripciones
- [[../Proyectos/Kaosuarina-Progreso]] — progreso general del proyecto

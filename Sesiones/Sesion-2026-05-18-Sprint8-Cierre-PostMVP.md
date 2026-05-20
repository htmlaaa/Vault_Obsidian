---
tags: [sesion, sprint-8, sprint-9, sprint-10, sprint-11, cierre, planificacion, post-mvp]
fecha: 2026-05-18
sprint: 8-cierre / 9-10-11-plan
estado: completado
---

# Sesión 2026-05-18 — Cierre Sprint 8 + Plan Post-MVP

## Resumen

Sesión de cierre del MVP y planificación de la fase post-MVP.
Sprint 8 quedó completamente terminado en código. Se analizaron los Excel de diseño
(exportados a JSON) y se generaron dos GDDs formales y tres sprints de trabajo futuro.

---

## Sprint 8 — Cierre definitivo ✅

> [!success] Sprint 8 completado — MVP jugable entregable
> Todas las stories implementadas y verificadas. El juego tiene condición de victoria,
> leaderboard, créditos TFG y persistencia de armas en BD.

| Story | Estado | Notas |
|-------|--------|-------|
| S8-01 Devastador del Caos | ✅ | Boss 2 fases, shockwave + espiral, spawn oleada 50 |
| S8-02 WinScreen | ✅ | Pantalla de victoria con stats y leaderboard |
| S8-03 Game Over mejorado | ✅ | Stats detallados + LeaderboardScreen |
| S8-04 Créditos TFG | ✅ | `CreditsScreen.java` accesible desde CharacterSelectScreen con [C] |
| S8-05 run_arma BD | ✅ | BD poblada con armas equipadas al final de la run |
| S8-06 Smoke test | ✅ | Build limpio, flujo completo verificado |

### Archivos clave implementados en S8

- `ui/HUD.java` — `renderVictoria()`, `renderGameOver()`, `setBossHealth(int,int,int)` con tipo 0/1/2
- `screens/GameScreen.java` — `triggerVictory()`, `buildLeaderboard()`, `renderVictoria()`, `blastEffectIsDevastador`
- `screens/CreditsScreen.java` — pantalla nueva
- `db/DBManager.java` — `getTop10()`, `guardarKills` con los 9 tipos de enemigo
- `utils/ParticlePool.java` — color DEVASTADOR negro/magenta
- `utils/ColisionManager.java` — `contactDamagePor(DEVASTADOR)`

---

## Análisis game_data_json ✅

Se leyeron los 18 JSON exportados de los Excel de diseño (`game_data_json/`).
Directorios: `enemies/` (9 archivos) y `weapons/` (10 archivos).

> [!info] Hallazgo principal
> El JSON representa un sistema de juego **mucho más completo** que el código actual.
> No es una corrección menor — es el diseño objetivo de la Fase Post-MVP.

### Enemigos — diferencias críticas vs código actual

| Enemigo | HP código | HP JSON | Cambio |
|---------|-----------|---------|--------|
| BASICO | 30 | 40 | +10 |
| TANQUE | 180 | 110 | −70 (más frágil, con resistencias) |
| ARQUERO | 500 | 90 | **−410** — deja de ser esponja |
| GUARDIAN | 800 | 900 | +100 |
| DEVASTADOR | 2000 | 2200 | +200 |

### Sistemas nuevos detectados (no existen en código)

| Sistema | JSON de origen | Prioridad |
|---------|---------------|-----------|
| Depth Scaling por nivel jugador | `depthscaling.json` | CRÍTICA |
| Resistencias/inmunidades por tipo | `enemyresistances.json` + `enemyimmunities.json` | CRÍTICA |
| Sistema de fases genérico | `bossphases.json` | ALTA |
| Tiers de arma T1-T5 | `tiers.json` | CRÍTICA |
| 6 tipos de daño | `damagetypes.json` | CRÍTICA |
| 12 afijos procedurales | `weaponaffixes.json` | ALTA |
| Stats base por clase | `characters.json` | ALTA |
| Skills activas por arma | `skills.json` | ALTA |
| Loot tables | `loottables.json` + `enemyloot.json` | MEDIA |

---

## GDDs generados ✅

### [[../../Code_Game/design/gdd/enemy-system-redesign|GDD-ENE-001 — Sistema de Enemigos Rediseñado]]

- Stats sincronizados con `enemies.json`
- Resistencias/vulnerabilidades por tipo de daño
- **Depth Scaling vinculado al nivel del jugador** — al subir de nivel, los enemigos también escalan
- BossPhaseManager genérico para GUARDIAN y DEVASTADOR
- Tabla de multiplicadores depth 1-10 completada (6-10 proyectados)

### [[../../Code_Game/design/gdd/weapon-system-redesign|GDD-WPN-001 — Sistema de Armas Rediseñado]]

- 5 tiers de rareza (Común → Legendario) con multiplicadores y colores
- 6 tipos de daño (PHYSICAL, RANGED, MAGIC, POISON, FIRE, CHAOS — este último ignora resistencias)
- 12 armas base con pasivas y habilidades activas
- 12 afijos procedurales (prefix + suffix)
- Stats base diferenciados por clase (CABALLERO 120 HP / TIRADOR 85 HP / MAGO 70 HP)
- Fórmulas de escalado: `dmg × (1 + stat × 0.05f)`

---

## Sprints planificados

### [[../../Code_Game/production/sprints/sprint-9-design|Sprint 9 — Post-MVP Assets, Balance y Refactor]]
`2026-06-15 → 2026-06-28` · 9.5 días estimados

Tres pistas paralelas:
- **Diseño**: idle animations (Mago/Shooter/Caballero dirs) + archivos audio WAV/OGG
- **Balance**: leer Excel + llevar valores a `Constants.java`
- **Código**: SQLite embebido (sin MySQL) + refactor `GameScreen` → `BossManager`/`SpawnManager`

### [[../../Code_Game/production/sprints/sprint-10-enemies|Sprint 10 — Rediseño Sistema Enemigos]]
`2026-06-29 → 2026-07-12` · 9 días estimados · depende de Sprint 9

- S10-01: Sincronizar stats base (7 enemigos)
- S10-02: Resistencias e inmunidades + enum `DamageType`
- S10-03: Depth Scaling por nivel + `BossPhaseManager`
- S10-04: Smoke test + playtesting balance

### [[../../Code_Game/production/sprints/sprint-11-weapons|Sprint 11 — Rediseño Sistema Armas]]
`2026-07-13 → 2026-07-26` · 9.5 días estimados · depende de Sprint 10

- S11-01: `WeaponDefinition`, `WeaponTier`, `WeaponAffix`, `DamageType`
- S11-02: `WeaponGenerator` generación procedural
- S11-03: Stats base por clase en `CharacterSelectScreen`
- S11-04: Drops con loot tables
- S11-05: Habilidades activas vinculadas al arma
- S11-06: HUD tier colors + smoke test

---

## Estado de los pendientes

| Pendiente | Estado |
|-----------|--------|
| [[Sprint7-Assets-Diseño]] | 🔄 Movido a Sprint 9 (S9-01 + S9-02) |
| [[Pasos-7-8-Pendientes]] | ✅ Completado (Sprint 7 cerrado) |
| [[Reverse-Document-GDDs-v01]] | ✅ Cerrado |

---

## Decisiones de diseño tomadas hoy

> [!important] Depth Scaling vinculado a nivel del jugador
> El escalado de dificultad NO es solo por tiempo — **cada level-up del jugador hace que
> los siguientes spawns sean más duros**. Los enemigos en pantalla no cambian.
> Sensación objetivo: "cuanto más fuerte me vuelvo, más peligrosos se vuelven ellos".

> [!important] ARQUERO rediseñado: esponja → élite ágil
> HP 500 → 90. El peligro viene del teletransporte y el daño, no de aguantar golpes.
> Sigue siendo una amenaza significativa en depth 3+ gracias al scaling.

> [!important] CHAOS como true damage
> El tipo de daño CHAOS ignora TODAS las resistencias. Hace que armas caóticas
> sean siempre relevantes independientemente del enemigo.

---

## Próximos pasos

1. **Sprint 9** (código + assets): empezar por SQLite — libera la dependencia de MySQL antes de entregar el JAR
2. **Sprint 9** (diseño): ejecutar el playbook de [[Sprint7-Assets-Diseño]] en la terminal con PixelLab MCP
3. **Sprint 10**: implementar enemy redesign según [[../../Code_Game/design/gdd/enemy-system-redesign|GDD-ENE-001]]
4. **Sprint 11**: implementar weapon redesign según [[../../Code_Game/design/gdd/weapon-system-redesign|GDD-WPN-001]]

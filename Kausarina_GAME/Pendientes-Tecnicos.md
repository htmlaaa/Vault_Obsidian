---
title: Pendientes Técnicos
tags:
  - kausarina-game
  - pendientes
date: 2026-05-12
updated: 2026-05-19
---

# Pendientes Técnicos

> [!abstract] Propósito
> Registro de tareas técnicas pendientes de implementar o mejorar. Actualizar cuando se resuelvan.

---

## 1. Animaciones

> [!success] Completado — 2026-05-19
> Todas las animaciones de personaje están implementadas. Ver [[../Sesiones/Sesion-2026-05-19-Sprint9-Assets-Implementacion]].

### Estado final

| Elemento | Estado |
|----------|--------|
| Walk 3 roles × 4 dirs × 4 frames | ✅ 2026-05-19 |
| Idle 3 roles × 4 dirs × 4 frames | ✅ 2026-05-19 |
| Caballero `swing` (ataque light, 9f) | ✅ 2026-05-19 |
| Caballero `swing2` (ataque heavy, 9f) — **nuevo** | ✅ 2026-05-19 |
| Mago `cast` (bolt light, 9f) | ✅ 2026-05-19 |
| Mago `cast2` (área heavy, 9f) — **nuevo** | ✅ 2026-05-19 |
| Shooter `shoot` (auto-fire, 9f) | ✅ 2026-05-19 |
| Rotaciones estáticas 3 roles × 4 dirs | ✅ 2026-05-19 |
| `AnimationSheets.Anim.HEAVY_ATTACK` | ✅ 2026-05-19 |
| Shooter disparo automático desde inicio | ✅ 2026-05-19 (bug fix) |

### Bugs resueltos

#### Bug resuelto — 2026-05-16

> [!bug]- ReliquiaCaballero — stacks de armadura bloqueados en 1 (nunca subían a 2-5)
> **Causa:** `onDamageReceived()` decrementaba el stack antes de incrementarlo, anulando la ganancia neta para cualquier valor > 0.
> ```java
> // BUGUEADO:
> if (armorStacks > 0) armorStacks--;  // cancelaba el +1 siguiente
> armorStacks = Math.min(armorStacks + 1, MAX_STACKS);
> // CORREGIDO: se elimina la línea del decremento
> armorStacks = Math.min(armorStacks + 1, MAX_STACKS);
> ```
> **Fix aplicado en `ReliquiaCaballero.java`** — 2026-05-16.

#### Bug resuelto — 2026-05-12

> [!bug]- Animación shoot del Tirador atascada en frame 0
> **Causa:** `disparar()` reseteaba `animFrame = 0` en cada disparo. La cadencia del Tirador (0.16 s) es menor que la duración de la animación (6 frames × 12 fps = 0.5 s), por lo que el frame nunca avanzaba.
>
> **Fix aplicado en `Player.java`:**
> ```java
> // Solo inicia si no está ya reproduciéndose
> if (animState != AnimationSheets.Anim.ATTACK
>         && AnimationSheets.frameCount(role.tipo, AnimationSheets.Anim.ATTACK) > 1) {
>     animState = AnimationSheets.Anim.ATTACK;
>     animFrame = 0;
>     animTimer = ATTACK_FRAME_DUR;
> }
> ```

---

## 2. Base de Datos — Integración Java

> [!success] Completado en Sprint 4 — 2026-05-13
> `DBManager.java` + `RunDAOImpl.java` implementados. La run se guarda al morir el jugador con transacción y rollback. Schema actualizado a `schema_v2.sql`. Ver [[../Sesiones/Sesion-2026-05-13-Sprint4-Cierre-Sprint5-Plan]].

### Implementado

| Componente | Archivo | Estado |
|-----------|---------|--------|
| `DBManager.guardarRun()` | `db/DBManager.java` | ✅ Sprint 4 |
| `RunDAOImpl.guardar()` | `db/dao/impl/RunDAOImpl.java` | ✅ Sprint 4 |
| `tiempoSupervivencia` en `GameScreen` | `screens/GameScreen.java` | ✅ Sprint 4 |
| `manaGastadoTotal` en `PlayerStats` | `systems/PlayerStats.java` | ✅ Sprint 4 |
| `killsByType[]` en `PoolEnemigos` | `entities/PoolEnemigos.java` | ✅ Sprint 4 |
| `upgradesAplicados` en `UpgradeManager` | `systems/UpgradeManager.java` | ✅ Sprint 4 |
| `schema_v2.sql` (con `causa_fin`, `nombre_display`, índices) | `Code_Game/docs/database/` | ✅ Sprint 4 |

### Pendiente — Sprint 6

> [!todo] `run_arma` en BD
> La tabla `run_arma` ya existe en `schema_v2.sql` como hook. Se puebla en Sprint 6 cuando el sistema de armas de Sprint 5 esté estable.

### Referencia

- Esquema completo: [[../../Code_Game/docs/database/README|Base de Datos README]]
- Schema v2: `Code_Game/docs/database/schema_v2.sql`

---

## Checklist de resolución

- [x] **Bug ReliquiaCaballero** — stacks bloqueados en 1 corregido *(2026-05-16)*
- [x] **Soporte IDLE en código** — `AnimationSheets.Anim.IDLE` + transiciones `Player.java` *(2026-05-16)*
- [x] **Todos los assets de animación** — reorganizados desde PixelLab, implementados *(2026-05-19)*
- [x] **`Anim.HEAVY_ATTACK`** — nuevo estado, `swing2`/`cast2` en 4 dirs × 9 frames *(2026-05-19)*
- [x] **Shooter disparo automático** — arranca con `PISTOLAS_GEMELAS` en slot 0 *(2026-05-19)*
- [x] `DBManager.java` — insertar run al morir *(Sprint 4)*
- [x] Timer de tiempo en `GameScreen` *(Sprint 4)*
- [x] Contador `mana_total_gastado` en `PlayerStats` *(Sprint 4)*
- [x] Contadores de kills por tipo en `PoolEnemigos` *(Sprint 4)*
- [x] Lista ordenada de upgrades en `UpgradeManager` *(Sprint 4)*
- [ ] `run_arma` en BD — cuando Sprint 5 esté estable *(Sprint 6)*

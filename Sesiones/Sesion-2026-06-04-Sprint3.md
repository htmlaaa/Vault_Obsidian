---
title: "Sesión 2026-06-04 — Sprint 3: Amuletos, Reroll, Sinergias, Upgrades, Contenido"
tags:
  - sesion
  - sprint-3
  - mec-01
  - mec-02
  - mec-05
  - cnt-03
  - cnt-04
fecha: 2026-06-04
sprint: sprint-plan-s3
estado: completado
---

# Sesión 2026-06-04 — Sprint 3

## Implementado

### MEC-01 — Sistema de 3 slots de amuletos

| Archivo | Cambio |
|---------|--------|
| `entities/Player.java` | `equippedAmulets[3]`, `equipAmulet()`, `swapAmulet()`, `getAmuletAtSlot()`, `isAmuletSlotsFull()`. `recalcStats()` llama a `recalcAmuletBonuses()` — efectos reversibles al hacer swap |
| `systems/PlayerStats.java` | `baseSpeedBase` (snapshot del rol), `amuletHpBonus/SpeedBonus/ManaBonus/RegenBonus/Lifesteal/CritBonus/DefBonus`, flags nuevos amuletos |
| `screens/GameScreen.java` | `pendingAmuletType`, `amuletSwapMenuActive/Timer/Selected`. Al recoger con slots llenos → menú A/D+ENTER, timeout 5s |
| `ui/HUD.java` | `amuletSlotName[3]`, `setAmuletSlots()`, `amuletShortName()`, `renderAmuletSlots()`, `renderAmuletLabels()` — 3 slots visibles con borde dorado |

### MEC-02 — Reroll de upgrades [R]

| Archivo | Cambio |
|---------|--------|
| `systems/UpgradeManager.java` | `rerollsLeft = 1`, `getRerollsLeft()`, `reroll(n)` |
| `screens/LevelUpScreen.java` | Tecla R → `rerollRequested`. `setRerollsLeft(n)`. Token `[R] Mezclar` visible (dorado) / `[R] Agotado` (gris) |
| `screens/GameScreen.java` | `procesarLevelUp()` detecta `isRerollRequested()` y llama `upgradeManager.reroll(3)` |

### MEC-05 — Sinergias implícitas

| Sinergia | Implementación |
|----------|---------------|
| FILO_ÍGNEO + DAÑO_UP ≥3 | `getMultiplicadorDanio()` añade +10% |
| VAMPIRISMO + VIDA_MÁXIMA_UP ≥3 | `hasVampirismoSinergia()` — hook para ColisionManager |
| BALA_EXTRA + PERFORACIÓN | `getSinergiaPierceBonus()` — pierce extra por bala adicional |
| CUCHILLA_VENENO + CADENCIA_UP ≥3 | `getPoisonStackMultiplier()` — veneno apila ×2 |

### CNT-03 — 14 nuevos upgrades

**Genéricos:** ESCUDO_TEMPORAL, REGENERACION (+1 HP/s/nivel), AURA_VENENO, CRITICO_ENCADENADO, PESO_MUERTO

**Caballero:** CONTRAGOLPE (×nivel), AURA_INTIMIDACION, GOLPE_TIERRA

**Mago:** MANA_SOBRECARGA, BARRERA_MAGICA (×nivel), HECHIZO_ESFERAS

**Tirador:** BALA_EXPLOSIVA, MUNICION_ENVENENADA, DISPARO_TENSO

Todos añadidos a `Upgrade.Tipo`, inicializados en `UpgradeManager.inicializarUpgrades()` y aplicados en `GameScreen.aplicarBonusUpgrade()`.

### CNT-04 — 5 nuevos amuletos

| Amuleto | Efecto | HUD label |
|---------|--------|-----------|
| AMULETO_CRITICO | +8% crit global | "+Crit" |
| AMULETO_EXPLOSION | Crítico letal → explosión radio 50u | "Explo" |
| AMULETO_ESPECTROS | Cada 10 kills → proyectil espectral | "Espect" |
| AMULETO_TIEMPO | Slow global 60% por 3s (1×/45s) | "Tiempo" |
| AMULETO_ARMADURA | +15 DEF, -10% velocidad | "+DEF" |

Integrados en `recalcAmuletBonuses()` y `amuletShortName()` del HUD.

---

## Build PASS — EXIT 0

---

## Referencias

- [[Sesion-2026-06-04-SprintPlan-Bugs-Dificultad]] — sesión anterior (S1+S2)
- [[../Proyectos/Kaosuarina-Progreso]] — tracking general
- `SPRINT_PLAN.md` — fuente de tareas

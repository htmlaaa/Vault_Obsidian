---
title: Sprint Plan v1.0 — Kaosuarina
tags:
  - sprint-plan
  - completado
  - roadmap
date: 2026-06-04
completado: 2026-06-04
estado: cerrado
aliases:
  - Sprint Plan
  - SPRINT_PLAN
---

# Sprint Plan v1.0 — Kaosuarina

> [!success] ✅ COMPLETADO — 2026-06-04
> Todos los sprints S1-S5 implementados en una sola sesión maratón. Build EXIT 0.
> Ver detalles en [[../Sesiones/Sesion-2026-06-04-Resumen-General]].

Unificación Plan A (bugs + inmediato) + Plan B (expansión de contenido) · Junio 2026.

---

## Bugs Críticos

### ~~BUG-01~~ — Inscripción por tipo, no por slot ✅
`Bullet.sourceSlot` propagado a `CollisionManager` — lee inscripción directamente del slot origen.

### ~~BUG-02~~ — Cooldown compartido en arma duplicada ✅
`Weapon.clonar()` abstracto; `WeaponPool.get()` devuelve clones con timers independientes.

### ~~BUG-03~~ — Inscripción loot sobrescribe slots existentes ✅
No aplica — `WeaponDropper` asigna inscripción al objeto nuevo antes de equiparlo.

---

## Decisiones de Diseño

### ~~DISEÑO-01~~ — 6 slots o 2 activos + 4 guardados ✅ → **Opción A**
2 slots activos + 4 storage. Los guardados sirven de reserva para swap.

### ~~DISEÑO-02~~ — Slots de amuletos en HUD ✅
3 slots visibles. Swap menu con timeout 5s al recoger con slots llenos. Ver [[../Personajes/Caballero-Kaos]] / [[../Personajes/Mago-Kaos]].

---

## Mejoras Visuales

### ~~VIS-01~~ — Ataques melee en arco, no en línea recta ✅
`CollisionManager.checkMelee()` usa geometría angular. Caballero no genera `Bullet` — el hitbox sigue el arco `shapeRenderer.arc()`.

### ~~VIS-02~~ — Proyectiles diferenciados por tipo de daño ✅
`Bullet.render()` switch por `DamageType`: mágico azul 22px · fuego naranja 18px · veneno verde 16px · caos violeta 20px (oscilación sinusoidal) · caos primordial índigo 24px.

### ~~VIS-03~~ — Armas SKILL con borde dorado en HUD ✅
`slotIsSkill[]` en HUD; color `(1.0f, 0.85f, 0.0f)` en línea 378 de `HUD.java`.

---

## Mejoras de Mecánicas

### ~~MEC-01~~ — 3 slots de amuleto con UI ✅
`Player.equippedAmulets[3]`, `equipAmulet()`, `swapAmulet()`. `recalcStats()` → `recalcAmuletBonuses()`. HUD muestra 3 slots con borde dorado cuando ocupados.

### ~~MEC-02~~ — Reroll de upgrades [R] ✅
`UpgradeManager.rerollsLeft = 1`. `LevelUpScreen`: tecla R + token visual dorado / gris agotado.

### ~~MEC-03~~ — Feedback visual combo Tirador ✅
Barra combo 10 segmentos. Floater `"+COMBO xN"` naranja durante 1.2s. Flash pulsante dorado al combo 10.

### ~~MEC-04~~ — Selector de dificultad antes del run ✅
`Difficulty.java` enum NORMAL/BRUTAL/CAOS. `CharacterSelectScreen` navega W/S. `SpawnManager` aplica `eliteWaveStart` e `intensityMult`.

| Dificultad | Depth inicial | Elites desde | Intensidad |
|------------|:------------:|:------------:|:----------:|
| Normal | 1 | Oleada 4 | ×1.0 |
| Brutal | 3 | Oleada 1 | ×1.0 |
| Caos | 5 | Oleada 1 | ×1.2 |

### ~~MEC-05~~ — Sinergias implícitas entre upgrades ✅

| Sinergia | Condición | Bonus |
|----------|-----------|-------|
| CRITICO_ENCADENADO + DANIO_UP≥3 | Ambos activos | +10% daño |
| VAMPIRISMO + VIDA_MAX≥3 | Ambos activos | Lifesteal en kill radial |
| BALA_EXTRA + PERFORACION | Ambos activos | Pierce extra = niveles de Bala Extra |
| CUCHILLA_VENENO + CADENCIA≥3 | Ambos activos | Poison stacks ×2 |

---

## Contenido Nuevo

### ~~CNT-01~~ — +12 armas (6 → 30 con evoluciones) ✅

**Melee:** Hacha de Guerra · Estoque · Guadaña de Guerra · Mayal
**Ranged:** Rifle de Francotirador · Escopeta de Combate · Lanzagranadas · Boleadoras
**Magic:** Vara del Rayo · Grimorio de Sangre · Bastón del Vacío · Orbe Glacial

### ~~CNT-02~~ — +8 enemigos + El Fragmentado (9 → 17) ✅

| Tipo | Mecánica clave |
|------|----------------|
| BERSERKER | <20% HP → rage speed 420f |
| SPLITTER | Al morir → 2 BASICO |
| HEALER | Cura aliados r=150u cada 1s |
| SHIELDER | Def 35f, muy lento |
| ELITE_CHARGE | Telegraph 1.5s → charge 800f |
| ELITE_SUMMON | Invoca 3 BASICO cada 8s |
| ELITE_ZONE | Estacionario, slow 45% en r=120u |
| **FRAGMENTADO** | Boss wave 30 · HP 1600 · 3 fases (67%/33%) |

### ~~CNT-03~~ — +14 upgrades (15 → 29) ✅

**Genéricos:** ESCUDO_TEMPORAL · REGENERACION · AURA_VENENO · CRITICO_ENCADENADO · PESO_MUERTO
**Caballero:** CONTRAGOLPE · AURA_INTIMIDACION · GOLPE_TIERRA
**Mago:** MANA_SOBRECARGA · BARRERA_MAGICA · HECHIZO_ESFERAS
**Tirador:** BALA_EXPLOSIVA · MUNICION_ENVENENADA · DISPARO_TENSO

### ~~CNT-04~~ — +5 amuletos (7 → 12) ✅

AMULETO_CRITICO · AMULETO_EXPLOSION · AMULETO_ESPECTROS · AMULETO_TIEMPO · AMULETO_ARMADURA

### ~~CNT-05~~ — Evolución de armas — 6 cadenas ✅

| Base (×2) | Resultado | Tipo |
|-----------|-----------|------|
| W_SHORTSWORD | W_LONGSWORD | FÍSICO |
| W_DUAL_PISTOLS | W_CUATRO_PISTOLAS | A_DISTANCIA |
| W_APP_STAFF | W_ARCANE_CANON | MÁGICO |
| W_FLAMEBLADE | W_INFERNO_BLADE | FUEGO |
| W_HUNTBOW | W_WARBOW | A_DISTANCIA |
| W_CHAOS_WAND | W_VOID_CANNON | CAOS_PRIMORDIAL |

La arma evolucionada hereda la inscripción. HUD muestra `¡EVOLUCIÓN! [nombre]` parpadeante 2.5s.

---

## Meta-progresión ~~[MEC-01 Plan A]~~ ✅

**Fórmula tokens:** `max(1, score/200 + level×2 + wavesCompleted)`

Tabla `meta_tokens` SQLite — fila única, acumulativa entre runs. Se muestra en `GameOverScreen` en dorado.

---

## Resumen de impacto

| Sprint | Tipo | Resultado |
|--------|------|-----------|
| S1 — Bugs + Fundamentos | Fix | Juego funciona como fue diseñado ✅ |
| S2 — Visuales + Feedback | Visual | Diferenciación visual por tipo de ataque ✅ |
| S3 — Amuletos + Upgrades | Mecánica | Amuletos estratégicos, 29 upgrades, sinergias ✅ |
| S4 — Armas y Enemigos | Contenido | 30 armas, 17 enemigos, boss Fragmentado ✅ |
| S5 — Sistemas Avanzados | Sistema | Evolución de armas + meta-progresión ✅ |

**Completado 2026-06-04 — Build final EXIT 0.**

---

## Referencias

- [[Kaosuarina-Progreso]] — estado general del proyecto
- [[../Sesiones/Sesion-2026-06-04-Resumen-General]] — resumen completo de la sesión
- [[../Sesiones/Sesion-2026-06-04-SprintPlan-Bugs-Dificultad]] — S1 + S2
- [[../Sesiones/Sesion-2026-06-04-Sprint3]] — S3
- [[../Sesiones/Sesion-2026-06-04-Sprint4]] — S4
- [[../Sesiones/Sesion-2026-06-04-Sprint5]] — S5
- [[Balance/Stat-Baseline-v1.0]] — valores exactos post-sprint

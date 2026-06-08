---
title: Kaosuarina — Objetivos y Progreso TFG
tags:
  - proyecto
  - tfg
  - progreso
  - planificacion
aliases:
  - Kaosuarina Progreso
  - TFG Progress
date: 2026-06-03
updated: 2026-06-04
---

# Kaosuarina — Objetivos y Progreso TFG

Estado del proyecto y objetivos del TFG. Complementa [[Kaosuarina]] (diseño) y [[Kaosuarina-Dev]] (arquitectura técnica).

---

## Objetivos generales

1. **Prototipo jugable completo** — ejecutable `.jar` funcional, partidas de 15-20 min sin errores críticos.
2. **Progresión y personalización** — 3 roles con builds distintas mediante armas tiered, inscripciones, afijos y amuletos.
3. **Rendimiento estable** — 60 FPS en gamas medias (2018-2020) con 300-500 entidades en pantalla.
4. **Documentación completa** — memoria técnica, código comentado, control de versiones Git.

## Objetivos específicos

1. Movimiento del jugador y disparo automático fluido en arena continua.
2. Sistema de roles con reliquia permanente — personalización mediante mejoras y builds distintas.
3. Sistema de armas tiered con afijos y sesgo de rol.
4. IA básica con 9 tipos de enemigos, 2 minijefes y boss final.
5. Sistema de pickups (cofres, scrolls, amuletos) y object pooling para rendimiento.
6. Efectos visuales (partículas, animaciones idle) y sonoros básicos.
7. Persistencia SQLite embebida — leaderboard top 10.
8. Ejecutable JAR autocontenido con instrucciones claras.

---

## Estado actual — Sprint Plan fase post-MVP (2026-06-04)

> [!success] v0.9+ — Sprint 11 cerrado · SPRINT_PLAN en curso

### SPRINT_PLAN — Tracking

> Plan unificado (Plan A inmediato + Plan B expansión). Fuente: `SPRINT_PLAN.md` en raíz del workspace.

| Item | Sprint | Estado |
|------|--------|--------|
| BUG-01 Inscripción por slot | S1 | ✅ Ya estaba corregido |
| BUG-02 Cooldown compartido WeaponSkill | S1 | ✅ Corregido 2026-06-04 |
| BUG-03 Inscripción loot no sobrescribe | S1 | ✅ No aplica (diseño ya correcto) |
| DISEÑO-01 Documentar Opción A (2 slots activos) | S1 | ✅ De facto en código |
| MEC-04 Selector de dificultad (Normal/Brutal/Caos) | S1 | ✅ Implementado 2026-06-04 |
| VIS-02 Proyectiles mágicos diferenciados por tipo | S2 | ✅ Ya estaba implementado |
| VIS-03 HUD borde dorado armas SKILL | S2 | ✅ Ya estaba implementado |
| VIS-01 Ataques melee en arco (hitbox) | S2 | ✅ Ya estaba implementado (comprobarMelee) |
| MEC-03 Barra combo + texto flotante Tirador | S2 | ✅ Completado 2026-06-04 |
| **BUG-LEVELUP** Crash upgrades agotados | — | ✅ Corregido 2026-06-04 |
| DISEÑO-02 Slots amuletos en HUD | S3 | ✅ Completado 2026-06-04 |
| MEC-01 Sistema 3 slots amuletos con swap | S3 | ✅ Completado 2026-06-04 |
| MEC-02 Reroll upgrades [R] | S3 | ✅ Completado 2026-06-04 |
| MEC-05 Sinergias implícitas upgrades | S3 | ✅ Completado 2026-06-04 |
| CNT-03 14 upgrades nuevos | S3 | ✅ Completado 2026-06-04 |
| CNT-04 5 amuletos nuevos | S3 | ✅ Completado 2026-06-04 |
| CNT-01 12 armas nuevas (24 total) | S4 | ✅ Completado 2026-06-04 |
| CNT-02 8 enemigos + boss Fragmentado | S4 | ✅ Completado 2026-06-04 |
| CNT-05 Sistema evolución armas | S5 | ✅ Completado 2026-06-04 |
| MEC-01 Plan A Meta-progresión tokens | S5 | ✅ Completado 2026-06-04 |

---

## Estado histórico — Sprint 11 completado (2026-06-03)

> [!success] v0.9+ — Sprint 11 cerrado

| Sistema | Estado | Sprint |
|---------|--------|--------|
| Loop básico WASD + auto-shoot + pooling | ✅ Completo | S1 |
| 3 roles (Caballero, Mago, Tirador) + CharacterSelectScreen | ✅ Completo | S1-S2 |
| Stats RPG (defensa, resMag, maná, regen) | ✅ Completo | S2 |
| 3 reliquias (Fortaleza Reactiva, Resonancia Caótica, Momentum) | ✅ Completo | S2 |
| 6 tipos de daño (DamageType) + ColisionManager tipado | ✅ Completo | S2-S3 |
| 9 tipos de enemigos incluyendo MALDITO y ESPECTRAL | ✅ Completo | S3 |
| StatusEffect BURN / POISON + veneno de contacto jugador | ✅ Completo | S3 |
| BD SQLite embebida (kaosuarina.db) — patrón DAO | ✅ Completo | S4 → S9 |
| 6 armas en 2 slots + afinidad por rol + WeaponPool | ✅ Completo | S5 |
| 9 inscripciones + scrolls + InscriptionPool | ✅ Completo | S6 |
| 7 amuletos + AmuletPool | ✅ Completo | S6 → S10 |
| Minijefe Guardián (HP 900, shockwave, fase 2) | ✅ Completo | S6 |
| Minijefe Arquero (HP 400, teleport, proyectil) | ✅ Completo | S7 |
| ParticlePool 400 unidades — muerte, impacto, explosión | ✅ Completo | S7 |
| AudioManager infraestructura — espera WAVs/OGG | ✅ Infraestructura | S7 |
| WinScreen + GameOverScreen + LeaderboardScreen | ✅ Completo | S8 |
| MainMenuScreen + CreditsScreen + navegación completa | ✅ Completo | S8 |
| Boss final Devastador del Caos (HP 2200, 2 fases, espiral 8 balas) | ✅ Completo | S8 |
| BD guardarRun() — kills, upgrades, reliquia, armas + transacción | ✅ Completo | S8 |
| SQLite migrado de MySQL + GameScreen refactor (737 líneas) | ✅ Completo | S9 |
| BossManager + SpawnManager en screens/managers/ | ✅ Completo | S9 |
| DataManager (Gson) + 18 POJOs + 17 catálogos JSON | ✅ Completo | S9 |
| WeaponInstanceFactory — armas tiered T1-T5 con afijos | ✅ Completo | S9-S11 |
| Depth scaling (depthscaling.json) + resistencias JSON | ✅ Completo | S10 |
| CAOS_PRIMORDIAL true damage + ralentización | ✅ Completo | S10 |
| Inventario 6 slots (armas + amuletos) | ✅ Completo | S10 |
| Upgrades por rol diferenciados | ✅ Completo | S10 |
| Animaciones idle 4 dirs — 3 roles (PixelLab) | ✅ Completo | S10 |
| WeaponDropper — 12 armas tiered+afijos sesgo por rol | ✅ Completo | S11 |
| Sistema de afijos (11 tipos: crit, lifesteal, elemental, etc.) | ✅ Completo | S11 |
| Balance "Kausarina Verzente" (spawn, roles, enemigos) | ✅ Completo | S11 |
| **Audio** (5 SFX WAV + 1 OGG música) | ⏳ Pendiente | S10-S11 |

---

## Pendiente — SPRINT_PLAN en curso

> [!todo] Pendiente confirmado

- [x] ~~Todos los ítems del SPRINT_PLAN~~ — **Completados 2026-06-04** ✅
- [ ] **SPRINT_PLAN S3** — Slots amuletos, reroll upgrades, sinergias, 15 upgrades nuevos, 5 amuletos
- [ ] **SPRINT_PLAN S4** — 12 armas nuevas, 8 enemigos nuevos, boss EN_BOSS_PHASE
- [ ] **SPRINT_PLAN S5** — Meta-progresión tokens, sistema evolución armas
- [ ] **Audio** — descargar 5 SFX WAV + 1 OGG de Kenney.nl CC0, colocar en `assets/audio/`
- [ ] **Pruebas manuales balance** — verificar: Mago sobrevive oleadas 1-3, Caballero siente riesgo en 5+, ARQUERO dura >8s, DEVASTADOR no one-shots Mago
- [ ] **Sprint 12 / Biomas** — mapa con biomas y zonas (planificado en [[Mapa-Biomas-PixelLab]])
- [ ] **Optimización final y testing** — JUnit 5, profiling 300-500 entidades

---

## Progreso global

| Métrica | Valor |
|---------|-------|
| Sprint activo | **SPRINT_PLAN COMPLETADO** (S1-S5 ✅) |
| Versión del juego | v1.0 candidate |
| Sistemas principales | ~38 completados |
| Progreso global estimado | ~97% |

---

## Timeline original vs real

| Fase | Período original | Estado |
|------|-----------------|--------|
| 1. Planificación y diseño | Enero | ✅ Completado (consolidado en Vault) |
| 2. Core loop y movimiento | Febrero | ✅ Completado (Sprint 1) |
| 3. Sistema de roles e ítems | Feb-Mar | ✅ Completado (Sprints 2-6) |
| 4. IA, dificultad, minijefes, boss | Mar-Abr | ✅ Completado (Sprints 7-9) |
| 5. Pulido, audio, leaderboard, balance | Abr-May | ✅ Casi completo (Sprints 8-11) — falta audio |
| 6. WeaponGenerator, afijos, animaciones | (no planificado) | ✅ Completado (Sprints 10-11) |

> [!note] Registro de sesiones principales
> - **2026-05-05** — Nota creada; setup inicial
> - **2026-05-06** — Refactor técnico completo ([[Sesion-2026-05-06-Refactor-Base]])
> - **2026-05-07** — Roles, CharacterSelectScreen, cores ([[Sesion-2026-05-07-Cores-Selector]])
> - **2026-05-11** — Rediseño RPG Día 1+2 ([[Sesion-2026-05-11-Rediseno-RPG-Dia1]], [[Sesion-2026-05-11-Rediseno-RPG-Dia2]])
> - **2026-05-12** — Sprint 2-3: stats RPG, reliquias, nuevos enemigos
> - **2026-05-13** — Sprint 4-5: BD, armas, cofres ([[Sesion-2026-05-13-Sprint4-Cierre-Sprint5-Plan]])
> - **2026-05-16** — Sprint 5-6: inscripciones, minijefe Guardián ([[Sesion-2026-05-16-Sprint5-Cierre-Sprint6-Inscripciones]])
> - **2026-05-17** — Sprint 7: partículas, minijefe Arquero, AudioManager ([[Sesion-2026-05-17-Sprint7-Code]])
> - **2026-05-18** — Sprint 8: WinScreen, GameOverScreen, Leaderboard, BD completa ([[Sesion-2026-05-18-Sprint7-Cierre-Sprint8-Plan]], [[Sesion-2026-05-18-Sprint8-Cierre-PostMVP]])
> - **2026-05-19** — Sprint 9: SQLite, refactor GameScreen, DataManager, JSON ([[Sesion-2026-05-19-Sprint9-Assets-Implementacion]])
> - **2026-05-20** — Sprint 10: depth scaling, animaciones, inventario 6 slots ([[Sesion-2026-05-20-Sprint10-Cierre]])
> - **2026-06-01** — Sprint 11: WeaponDropper, afijos, balance "Kausarina Verzente" ([[Sesion-2026-06-01-Balance-Audio-Sprint11]])
> - **2026-06-04** — Sesión maratón: SPRINT_PLAN S1-S5 completados. Ver [[Sesion-2026-06-04-Resumen-General]] para resumen completo y sesiones detalladas de cada sprint.

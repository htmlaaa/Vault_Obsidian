---
title: Sesión 2026-05-13 — Cierre Sprint 4 + Memoria TFG + Plan Sprint 5
tags:
  - sesion
  - sprint4
  - sprint5
  - base-de-datos
  - armas
  - mana
  - memoria-tfg
date: 2026-05-13
---

# Sesión 2026-05-13 — Cierre Sprint 4 + Memoria TFG + Plan Sprint 5

Sesión de recuperación de contexto (sesión anterior cerrada accidentalmente). Se cerró Sprint 4 al 100%, se generó la documentación para la reunión con el tutor y se planificó el Sprint 5.

---

## Lo hecho hoy

### Sprint 4 — Cierre (S4-07) ✅

Build limpio verificado con `.\gradlew.bat lwjgl3:run`. Sprint 4 cerrado al 100%.

> [!success] Sprint 4 completado — 2026-05-13
> Upgrades elementales (BURN/POISON on-hit), life steal (Vampirismo 15%), diferenciación A_DISTANCIA, `DBManager` + `RunDAOImpl`, `schema_v2.sql`.

#### Resumen de lo implementado en Sprint 4

| Story | Descripción | Estado |
|-------|------------|--------|
| S4-01 | Nuevos upgrades roguelite: FILO_IGNEO, CUCHILLA_VENENO, VAMPIRISMO | ✅ |
| S4-02 | `ColisionManager` refactorizado — firma con `Player`, `aplicarEfectosOnHit()` | ✅ |
| S4-03 | A_DISTANCIA diferenciado: `reduccion = defensa × 0.5f` (50% pen) | ✅ |
| S4-04 | Costes de maná Caballero/Tirador — **DESCARTADO** (van en Sprint 5 con Skill weapons) | — |
| S4-05 | `DBManager.guardarRun()` + `RunDAOImpl` + transacción con rollback | ✅ |
| S4-06 | `schema_v2.sql` — `nombre_display`, `causa_fin ENUM`, índices, vistas mejoradas | ✅ |
| S4-07 | Smoke test + `00-stat-baseline.md` §12 y §13 actualizados | ✅ |

---

### Base de datos — schema_v2.sql ✅

Nuevo script en `Code_Game/docs/database/schema_v2.sql` (el `schema.sql` original pasa a ser versión histórica v1).

**Mejoras respecto a v1:**

| Mejora | Detalle |
|--------|---------|
| `nombre_display` | En `personaje`, `tipo_enemigo`, `tipo_upgrade` — textos legibles para informes |
| `causa_fin ENUM` | `'MUERTE'` \| `'VICTORIA'` con default `'MUERTE'` en tabla `run` |
| Índices nuevos | `idx_run_personaje`, `idx_run_score`, `idx_run_tiempo` + índices de tablas de join |
| Vistas mejoradas | `vista_runs_completas`, `vista_top10_score` usan `nombre_display` y `ROUND(AVG(...))` |

> [!info] Conexión MySQL
> URL: `jdbc:mysql://192.168.1.101:3306/kaosuarina` (VirtualBox del alumno, se lleva en el mismo portátil a la presentación).

---

### Documentación para el tutor ✅

#### MEMORIA_KAOSUARINA_v1.md

Documento Word-compatible generado siguiendo el índice exacto de `Plantilla_Memoria_Proyecto-Fin-Ciclo_GDD.docx`. Cubre el estado post-Sprint 4 completo.

**Estructura (13 secciones + anexos):**

1. Introducción — descripción, motivación, beneficios
2. Objetivos generales y específicos (RF-01→RF-14, RNF-01→RNF-06)
3. Contexto actual — tabla estado del arte, conceptos clave
4. Planificación — tabla Sprints 1-6, metodología Scrum
5. Análisis de requisitos — 14 RF, 6 RNF, perfiles de usuario
6. Diseño — mockups ASCII, diagrama arquitectura, ER, diagrama de clases
7. Desarrollo — tabla tecnologías, flujos ColisionManager y guardado BD
8. Pruebas — smoke test Sprint 4, tabla de integración
9. Despliegue — `gradlew lwjgl3:jar`, setup VirtualBox MySQL
10. Conclusiones y trabajo futuro — Sprint 5 armas, Sprint 6 jefes/polish
11. Relación módulos DAM — tabla BD/Prog/AccesoDatos/etc.
12. Bibliografía
13. Anexos — tablas stats, upgrades, schema

> [!tip] Reunión con tutor — 2026-05-13
> **Tutor**: Isidoro Nevares Martín · **Centro**: IES Virgen de la Paloma · **Ciclo**: DAM

---

### Stat Baseline — §12 y §13 añadidos ✅

Archivo: `Code_Game/design/gdd/00-stat-baseline.md`

| Sección | Sprint | Contenido añadido |
|---------|--------|-------------------|
| §12 — Stats v0.3 | Sprint 3 | StatusEffect (BURN/POISON), MALDITO, ESPECTRAL |
| §13 — Stats v0.4 | Sprint 4 | Upgrades nuevos, fórmula A_DISTANCIA, constantes DB |

**Valores clave confirmados:**
- BURN: 8 dmg/tick · 0.5s intervalo · 3s duración
- POISON: 5 dmg/tick · 1s intervalo · 5s duración
- MALDITO: HP 45, speed 130, explosión radio 120, 25 dmg VENENO
- ESPECTRAL: HP 35, speed 160, resMag 10, inmune FISICO, +50% FUEGO
- Vampirismo: `Math.max(1, Math.round(dmg × 0.15f))` HP robado

---

### Sprint 5 — Planificado ✅

Plan completo en `Code_Game/production/sprints/sprint-5.md`.

**Goal**: Implementar el sistema de 2 slots de arma (6 armas, Normal + Skill), activar el maná universal con regen diferenciada, y reemplazar el disparo hardcoded.

**Periodo**: 28 may – 11 jun · Capacidad: 11 días · Estimado: 7.5 días

| Story | Descripción | Días |
|-------|------------|-----:|
| S5-01 | Jerarquía `Weapon`/`WeaponNormal`/`WeaponSkill` + `WeaponPool` (6 armas) | 1.5 |
| S5-02 | `Player`: 2 slots equipables + stat bonuses por arma | 1.0 |
| S5-03 | Maná: pools por rol (120/40/30), regen Mago +2/s, barra HUD | 0.5 |
| S5-04 | Auto-fire desde armas Normal — reemplaza disparo hardcoded | 1.0 |
| S5-05 | Skill weapons Q/E + consumo de maná + efectos área/piercing | 1.0 |
| S5-06 | Cofre cada 2-3 oleadas + UI intercambio al llenar slots | 1.5 |
| S5-07 | HUD slots de arma con overlay de cooldown y candado rojo | 0.5 |
| S5-08 | Smoke test 14 checks + stat baseline §14 | 0.5 |

**Pospuesto a Sprint 6**: Inscripciones (8 tipos), `run_arma` en BD, amuletos (Sed de Sangre, Guardián de la Arena), Cicatriz de Combate (Reliquia Caballero), drops de minijefe.

**Referencias GDDs**:
- [[../../Code_Game/design/gdd/02-weapon-system|02 — Sistema de Armas]]
- [[../../Code_Game/design/gdd/03-mana-system|03 — Sistema de Maná]]

---

## Pendientes identificados

> [!warning] Paso 8 — Sprites reales aún pendiente
> `SpriteSheets.java` + dirección de sprites desde `velocity` sigue sin implementar. Bloqueado por la animación `swing` del Caballero (límite plan gratuito PixelLab). No es crítico para el TFG si los procedurales se mantienen.

> [!todo] Sprint 5 — Empieza el 28 de mayo
> - S5-01 primero: arquitectura `Weapon` sin tocar el game loop existente
> - S5-03 puede ir en paralelo con S5-01 (no dependen entre sí en implementación inicial)
> - S5-04 es el cambio estructural más arriesgado — requiere que S5-01 y S5-02 estén completos

> [!note] Inscripciones — GDD listo, implementación Sprint 6
> `Kausarina_GAME/Solucionadas/disenados/Inscripciones-Sistema.md` ya existe. Las 8 inscripciones están diseñadas. La implementación Java espera a que `Weapon` base esté estable.

> [!note] `run_arma` en BD — Sprint 6
> La tabla `run_arma` ya está en `schema_v2.sql` como hook. Se puebla cuando el sistema de armas de Sprint 5 esté estable.

---

## Estado del proyecto al cierre de sesión

```
v0.1 Sprint 1  ✅  Movimiento, auto-fire, 4 enemigos, upgrades básicos, HUD, game over
v0.2 Sprint 2  ✅  Stats RPG, roles, cores, selector de personaje, reescalado de daño
v0.3 Sprint 3  ✅  StatusEffects (BURN/POISON), enemigos MALDITO/ESPECTRAL, regen Mago
v0.4 Sprint 4  ✅  Upgrades elementales, life steal, A_DISTANCIA pen., DB MySQL
v0.5 Sprint 5  🔲  Sistema de armas (6 armas, 2 slots), maná universal
v0.6 Sprint 6  🔲  Inscripciones, amuletos, mini-jefes, run_arma BD
```

---

## Referencias de sesión

- [[Sesion-2026-05-11-Rediseno-RPG-Dia2]] — sesión anterior (Sprint 2 cierre)
- [[../Kausarina_GAME/Sprint3-Efectos-Enemigos]] — contexto Sprint 3
- [[../Proyectos/Kaosuarina-Progreso]] — progreso general del proyecto

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
date: 2026-05-05
---

# Kaosuarina — Objetivos y Progreso TFG

Estado del proyecto y objetivos del TFG. Complementa [[Kaosuarina]] (diseño) y [[Kaosuarina-Dev]] (arquitectura técnica).

---

## Objetivos generales

1. **Prototipo jugable completo** — ejecutable `.jar` funcional, partidas de 15-20 min sin errores críticos.
2. **Progresión y personalización** — 4-6 builds diferentes mediante roles, cores y sinergias.
3. **Rendimiento estable** — 60 FPS en gamas medias (2018-2020) con 300-500 entidades en pantalla.
4. **Documentación completa** — memoria técnica, código comentado, control de versiones Git.

## Objetivos específicos

1. Movimiento del jugador y disparo automático fluido en arena continua.
2. Sistema de roles con core permanente — personalización mediante mejoras y builds híbridas.
3. Generación procedural de arenas con obstáculos y zonas dinámicas.
4. IA básica que modifique comportamiento según la build del jugador.
5. Sistema de ítems, sinergias y object pooling para rendimiento con muchas entidades.
6. Efectos visuales y sonoros básicos (inmersión y feedback).
7. Pruebas unitarias e integración en sistemas críticos (sinergias, IA).
8. Ejecutable JAR multiplataforma con instrucciones claras.

---

## Timeline

| Fase | Período | Tareas | Horas | Entregable |
|------|---------|--------|-------|-----------|
| 1. Planificación y diseño | Enero (sem. 1-4) | Análisis, wireframes, arquitectura, setup | 50h | Doc diseño + proyecto configurado |
| 2. Core loop y movimiento | Febrero (sem. 5-8) | WASD, disparo, cámara, colisiones, pooling balas | 70h | Prototipo jugable básico |
| 3. Sistema de roles e ítems | Feb-Mar (sem. 9-13) | Enemigos, HUD, nivel/XP, mejoras, efectos | 90h | Sistema de progresión funcional |
| 4. IA adaptativa y procedural | Mar-Abr (sem. 14-18) | IA, dificultad, arenas procedurales, oleadas, mini-jefes | 100h | Juego con desafío escalable |
| 5. Pulido y documentación | Abr-May (sem. 19-22) | Efectos, optimización, testing, bugs, memoria | 90h | Proyecto completo + Memoria |

**Total**: 400 horas estimadas · Duración: 5 meses (enero — mayo 2025)

### Milestones

| Fecha | Hito |
|-------|------|
| 31 Enero | Core loop MVP — jugador se mueve, dispara, enemigos persiguen |
| 28 Febrero | Sistema de progresión funcional — niveles, mejoras, HUD |
| 31 Marzo | IA adaptativa y enemigos variados operativos |
| 30 Abril | Prototipo completo con todos los sistemas integrados |
| 31 Mayo | Entrega final — JAR + memoria técnica |

---

## Estado actual

> [!success] Completado — Sprint 1 en curso (2026-05-07)

| Tarea | Sesión |
|-------|--------|
| Configuración proyecto LibGDX + IntelliJ | — |
| Movimiento WASD normalizado | — |
| Auto-shoot con pooling (500 balas simultáneas) | — |
| Sistema de enemigos — 4 tipos (Básico, Rápido, Tanque, Shooter) | — |
| Colisiones círculo-círculo | — |
| HUD completo (vida, XP, timer, score) | — |
| Sistema de level up (3 mejoras aleatorias) | — |
| Sistema de upgrades (7 mejoras con multiplicadores) | — |
| Cooldown de daño visual (invulnerabilidad 1s, parpadeo) | — |
| **Refactor técnico completo** (SharedTextures, PlayerStats, Constants, ColisionManager) | [[Sesion-2026-05-06-Refactor-Base]] |
| **Sistema de roles y cores** (CaballeroCore, MagoCore, ShooterCore) | [[Sesion-2026-05-07-Cores-Selector]] |
| **CharacterSelectScreen** — selector de personaje con 3 cards | [[Sesion-2026-05-07-Cores-Selector]] |
| **Balance balas** — DANIO_UP +40%, balas base=1 excepto Shooter | [[Sesion-2026-05-07-Cores-Selector]] |
| **Mejoras visuales** — círculos HD, tintado por rol, arena boundary | [[Sesion-2026-05-07-Cores-Selector]] |

> [!warning] Sprint 1 en progreso (2026-05-07 → 2026-05-31)

Ver `Code_Game/production/sprints/sprint-1.md` para el plan completo.

| Story | Estado |
|-------|--------|
| S1-01 Balance daño balas | ✅ |
| S1-02 Integración Role en Player | ✅ |
| S1-03 ShooterCore | ✅ |
| S1-04 CaballeroCore | ✅ |
| S1-05 MagoCore | ✅ |
| S1-06 SpriteSheets.java | ⏳ |
| S1-07 Player sprites por dirección | ⏳ |
| S1-08 CharacterSelectScreen | ✅ |
| S1-09 Efectos de mejoras completos | ⏳ |
| S1-10 Dificultad escalable | ⏳ |

> [!todo] Pendiente (post-Sprint 1)

- [ ] IA adaptativa con FSM
- [ ] Generación procedural
- [ ] Sistema de partículas
- [ ] Audio
- [ ] Mini-jefes

---

## Progreso global

| Métrica | Valor |
|---------|-------|
| Sprint activo | Sprint 1 (2026-05-07 → 2026-05-31) |
| Stories Sprint 1 completadas | 5/17 |
| Progreso global estimado | ~55% |

> [!note] Registro de sesiones
> - **2026-05-05** — Nota creada desde memoria técnica TFG
> - **2026-05-06** — Refactor técnico completo ([[Sesion-2026-05-06-Refactor-Base]])
> - **2026-05-07** — Sprint 1 creado, cores S1-01→S1-08 implementados ([[Sesion-2026-05-07-Cores-Selector]])

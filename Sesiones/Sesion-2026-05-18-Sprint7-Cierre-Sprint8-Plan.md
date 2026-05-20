---
tags: [sesion, sprint-7, sprint-8, cierre, planificacion]
fecha: 2026-05-18
sprint: 7-cierre / 8-plan
---

# Sesión 2026-05-18 — Cierre Sprint 7 + Plan Sprint 8

## Resumen

Sesión de revisión y planificación. Se actualizó Code_Game a v1.0.0, se verificó el estado
real del código del Sprint 7, se separó la pista de código de la pista de diseño/assets,
y se planificó Sprint 8 completo.

---

## Code_Game — Actualización v1.0.0

Merge limpio de la release v1.0.0 desde GitHub. Novedades relevantes:
- Nuevo skill `/vertical-slice` — para futura documentación de vertical slices
- Mejoras a `/prototype`, `/gate-check`, `/sprint-plan`, `/smoke-check`
- Nuevos templates: `prototype-report.md`, `vertical-slice-report.md`
- Nuevos archivos raíz: `CONTRIBUTING.md`, `SECURITY.md`, `UPGRADING.md`

**Impacto en el juego:** ninguno. La actualización es del framework de desarrollo, no del código de Kaosuarina.

---

## Verificación Sprint 7 — Estado real del código

### Todo el código de Sprint 7 está implementado y verificado ✅

| Story | Estado código | Notas |
|-------|--------------|-------|
| S7-02 Partículas | ✅ | `Particle.java` + `ParticlePool.java` — pool 400 partículas, spawnDeath/Impact/Explosion |
| S7-03 Arquero | ✅ | `Enemy.ARQUERO`, teleport + disparo + drop cofre, HUD barra azul |
| S7-04 Audio | ✅ código | `AudioManager.java` completo con try-catch. Archivos WAV/OGG pendientes en pista diseño |
| S7-05 InscripcionVampiricaMana | ✅ | Registrada en InscriptionPool |
| S7-01 AnimationSheets | ✅ código | Carga, fallback y no crashea en ningún caso |

### Assets de animación disponibles

| Rol | walk | idle | attack |
|-----|------|------|--------|
| Caballero | ✅ 4 dirs | ⚠️ solo south (5 frames) | ❌ sin swing |
| Mago | ✅ 4 dirs | ❌ | ✅ cast 4 dirs |
| Shooter | ✅ 4 dirs | ❌ | ✅ shoot 4 dirs |

---

## Separación código / diseño

**Decisión**: las pistas de código y de diseño/assets son independientes a partir de ahora.
Los assets visuales y de audio no bloquean el Sprint 8 — el código tiene fallbacks robustos.

### Pista de diseño (para otra terminal)
Ver [[../Pendientes/Sprint7-Assets-Diseño]] — playbook completo para generar:
- Idle animations (Mago + Shooter + Caballero dirs faltantes) via PixelLab MCP
- Audio WAV/OGG desde Kenney.nl (CC0)
- Tras añadir assets: ejecutar smoke test S7-06

---

## Sprint 8 — Plan

Ver `Code_Game/production/sprints/sprint-8.md` para el plan completo.

### Goal
> "Añadir el Boss Final (Devastador del Caos) como condición de victoria, pantalla de resultados con stats y leaderboard, créditos TFG, y poblar run_arma en BD — cerrando el MVP jugable y la entrega del TFG."

### Stories

| ID | Story | Días | Prioridad |
|----|-------|------|-----------|
| S8-01 | Enemy.DEVASTADOR + 2 fases | 3.0 | must-have |
| S8-02 | WinScreen (condición victoria) | 1.0 | must-have |
| S8-03 | Game Over stats + LeaderboardScreen | 1.5 | must-have |
| S8-04 | Créditos TFG en menú | 0.5 | must-have |
| S8-05 | Poblar run_arma en BD | 0.5 | should-have |
| S8-06 | Smoke test final + §17 | 0.5 | must-have |

**Total estimado:** 7.0 días / 10 de capacidad

### Boss — decisiones de diseño

El Devastador del Caos tiene **2 fases** (suficiente para TFG):
- Fase 1: shockwave cada 3s + espiral 8 balas cada 5s
- Fase 2 (<50% HP): velocidad ×1.7, timers acelerados, invoca 3 ESPECTRAL (una sola vez)

Para un Sprint 9 o post-TFG: mecánica "El Portador" (ver [[../Kausarina_GAME/Mecanica-Jefes-Brainstorm]]), arenas procedurales, 3+ fases.

---

## Próximos pasos

1. **Esta terminal**: Iniciar implementación S8-01 (Enemy.DEVASTADOR)
2. **Otra terminal**: Ejecutar playbook de assets de diseño ([[../Pendientes/Sprint7-Assets-Diseño]])
3. **Cuando assets listos**: Ejecutar smoke test S7-06 y cerrar Sprint 7 completamente

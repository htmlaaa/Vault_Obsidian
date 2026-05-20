---
title: Kausarina_GAME — Índice de Diseño
tags:
  - kausarina-game
  - indice
date: 2026-05-12
---

# Kausarina_GAME — Documentos de Diseño RPG

> [!abstract] Propósito
> Esta carpeta contiene los documentos de diseño del **rediseño RPG** de Kaosuarina (post-v0.1). La fuente de verdad técnica vive en `Code_Game/design/gdd/`.

---

## En progreso / Pendientes

| Archivo | Contenido | Estado |
|---------|-----------|--------|
| [[Pendientes-Tecnicos]] | Animaciones bloqueadas + integración BD Java | Lista de tareas pendientes |
| [[Sistema-Ataques-Roles]] | Caballero arco espada, Mago bolt+AoE, Tirador auto-shoot | Implementado — swing Caballero pendiente (PixelLab trial) |
| [[Sprint3-Efectos-Enemigos]] | StatusEffect (BURN/POISON), MALDITO, ESPECTRAL, life steal hook | Implementado (Sprint 3) |
| [[Sistema-Mana-Armas-Dual]] | Sistema de maná + armas duales (Normal vs Habilidad) | Parcialmente implementado |
| [[Mecanica-Jefes-Brainstorm]] | Jefes, El Portador, señal Presagio, Jefe Final | Diseñado — sin implementar |

---

## GDDs técnicos (Code_Game)

| Archivo | Contenido |
|---------|-----------|
| `Code_Game/design/gdd/00-stat-baseline.md` | Stats exactos del código v0.2 — fuente de verdad |
| `Code_Game/design/gdd/01-vision-rediseno-rpg.md` | Gap analysis, nuevos stats, plan de sprints |
| `Code_Game/design/gdd/02-weapon-system.md` | Sistema de armas completo (6 armas, inscripciones, afinidades) |
| `Code_Game/design/gdd/03-mana-system.md` | Sistema de maná universal con regen diferenciada |

---

## Solucionadas

### `Solucionadas/implementados/` — Código en el juego

| Archivo | Qué implementa |
|---------|----------------|
| [[Solucionadas/implementados/Vision-Rediseno-RPG]] | Stats RPG, roles reescalados, estructura general Sprint 2 |
| [[Solucionadas/implementados/Tipos-de-Daño]] | `DamageType.java` — FISICO/MAGICO/A_DISTANCIA/FUEGO/VENENO/CAOS |
| [[Solucionadas/implementados/Reliquias-Brainstorm]] | `reliquias/` package — ReliquiaCaballero/Mago/Tirador activas |

### `Solucionadas/disenados/` — Diseñados, código pendiente

| Archivo | Sprint objetivo |
|---------|----------------|
| [[Solucionadas/disenados/Inscripciones-Sistema]] | Sprint 4 (Sistema de Armas) |

---

## Decisiones confirmadas (todas resueltas)

- [x] **Maná**: Universal con regen diferenciada — solo Mago tiene regen pasiva (+2/s)
- [x] **Armas**: 2 equipables, 6 en total, Normal + Habilidad, con inscripción pasiva
- [x] **Mapa**: Arena circular + pickups dispersos (inmediato); explorable (futuro)
- [x] **Jefes**: Cada 10 oleadas, mecánica El Portador en ronda 9, señal Presagio ambiental

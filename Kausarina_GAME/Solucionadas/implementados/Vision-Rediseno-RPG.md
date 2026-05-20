---
title: Visión General — Rediseño RPG
tags:
  - kausarina-game
  - vision
  - rediseno
  - rpg
  - planificacion
date: 2026-05-11
status: en-proceso
---

# Visión General — Rediseño RPG

> [!abstract] Dirección
> Kaosuarina evoluciona de shooter básico con upgrades a un **roguelite RPG profundo** inspirado en Elden Ring: Nightreign. El jugador construye una build a través de roles, reliquias, armas, inscripciones, items y amuletos encontrados explorando.

---

## Nuevos sistemas a implementar

### 1. Sistema de Stats RPG
- **Combate** — stat base ofensivo que escala daño
- **Vida** — HP máximo
- **Defensa** — reduce daño físico recibido
- **Daño Base** — valor de ataque sin modificar
- **Velocidad** — movimiento y cadencia
- **Rango** — distancia máxima de proyectiles

### 2. Tipos de Daño
Ver [[Tipos-de-Daño]]

### 3. Sistema de Maná (Mago principalmente)
- El Mago tiene más maná que todos los demás
- Pendiente: mecánica de recarga (por tiempo / kills / ambas)

### 4. Ataques de rol
- **Caballero**: secuencias de movimiento repetitivas que simulan combate continuo (sin combo visible)
- **Mago**: proyectiles lentos, más daño, distintos patrones mágicos
- **Tirador**: alta cadencia, múltiples proyectiles, empieza con bala extra (sprite 2 pistolas)

### 5. Sistema de Armas
- Múltiples armas por personaje (número pendiente)
- Se recogen del mapa / drops de jefes
- Cada arma tiene 1 slot de [[Inscripciones-Sistema|Inscripción]]

### 6. Sistema de Reliquias
Ver [[Reliquias-Brainstorm]]
- 1 reliquia única por personaje, siempre activa
- Reemplaza el sistema de "cores"
- Los efectos de los cores actuales se convierten en pasivas o habilidades complementarias

### 7. Items y Amuletos
- **Items**: bonificadores de stats encontrables en el mapa
- **Amuletos**: accesorios equipables con efectos únicos (robo de vida, resistencias, etc.)
- Incluir: hechizos vampíricos, buffs de area, efectos malditos

### 8. Exploración en el mapa
- La arena circular se expande a un mapa explorable
- Items, amuletos y encuentros opcionales dispersos
- Encuentros de jefes: por tiempo / oleada / exploración (pendiente decisión)

### 9. Más enemigos + Jefes
- Nuevos tipos de enemigos (pendiente diseño)
- Minijefes: varios, aparecen en puntos específicos o por oleada
- Jefe final: 1 principal (Devastador del Caos, ya en backlog S1-17)
- Jefe opcional: encontrado por exploración / suerte

### 10. Sistema de Daño adicional
- Robo de vida (life steal)
- Hechizos vampíricos
- Efectos de estado: Quemadura, Envenenamiento, Conmoción, Lentitud

---

## Inspiración principal

**Elden Ring: Nightreign** — roguelite de exploración donde:
- El mapa se encoge progresivamente (o tiene zonas que abren/cierran)
- El jugador recoge armas y items explorando
- Encuentros opcionales de jefes por exploración
- Cada run es una build diferente
- Alta sinergia entre armas, inscripciones (Ashes of War) e items

---

## Decisiones confirmadas (2026-05-11)

### Maná
- Regen base: pasiva muy lenta
- Por kills: cada enemigo muerto da maná (varía por tipo)
- Por impacto: ataques que conectan recuperan pequeña cantidad
- Armas con regen de maná como stat
- Reliquias con regen pasiva de maná
- Mago: pool de maná más alto de todos

### Armas
- 6 armas en total, **2 equipables** simultáneamente
- Si 2 armas son iguales: ambas activas a la vez
- Las armas dan **bonificadores de stat** al personaje
- Cada arma tiene 1 slot de Inscripción pasiva
- Se obtienen del mapa (drops de minijefes, exploración)

### Mapa
- Objetivo: mapa grande con zonas explorables (Nightreign)
- Inmediato: arena circular + pickups dispersos

### Jefes
- Minijefes: **cada 10 rondas**, sorteado al inicio de la run
- Sin aviso hasta ronda 9 (feedback ambiental — Presagio)
- Mecánica de aparición: **El Portador** (ronda 9, el jugador decide cuándo convoca)
- Jefe final: ronda 50 (a confirmar)
- Juego: infinito, run termina al morir

---

## Estado del rediseño

> [!success] Diseño base completo — listo para Sprint 2
> Ver `Code_Game/design/gdd/00-stat-baseline.md` (fuente de verdad) y `01-vision-rediseno-rpg.md` (plan).

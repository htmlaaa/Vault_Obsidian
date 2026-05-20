---
title: Inscripciones de Arma — Sistema (equiv. Ashes of War)
tags:
  - kausarina-game
  - inscripciones
  - armas
  - sistema
date: 2026-05-11
status: brainstorm
---

# Inscripciones de Arma

> [!abstract] Concepto
> Las Inscripciones son modificadores **pasivos** que se equipan en el slot de inscripción de un arma. Cambian el comportamiento del arma sin añadir una habilidad activa. Inspirado en las Cenizas de Guerra de Elden Ring, pero como pasivo puro.

---

## Mecánica base

- Cada arma tiene **1 slot de inscripción**.
- Las inscripciones se encuentran en el mapa o se obtienen de jefes/minijefes.
- Se pueden intercambiar libremente fuera del combate (o con tiempo de cooldown).
- Una inscripción puede llevar **pros y contras** (no todas son puras ventajas).

---

## Catálogo de Inscripciones

| # | Nombre | Efecto | Restricción |
|---|--------|--------|-------------|
| 1 | **Inscripción Ígnea** | Impactos aplican Quemadura (daño fuego DoT 3s) | — |
| 2 | **Inscripción Vampírica** | Roba X HP por impacto | -10% daño base |
| 3 | **Inscripción del Eco** | El daño se repite al 40% en el mismo objetivo 2s después | — |
| 4 | **Inscripción de Vacío** | Penetra el 100% de la defensa del objetivo | -25% daño base |
| 5 | **Inscripción Sísmica** | Impactos aplican Conmoción (0.3s de stun al objetivo) | — |
| 6 | **Inscripción del Caos** | Efecto aleatorio en cada impacto (fuego / veneno / lentitud / robo de vida) | Impredecible |
| 7 | **Inscripción Resonante** | +5% daño por impacto consecutivo al mismo objetivo (máx +50%) | Pierde stacks si cambias de objetivo |
| 8 | **Inscripción Espectral** | Proyectiles atraviesan 1 enemigo adicional | — |

---

## Afinidades por rol

| Inscripción | Caballero | Mago | Tirador |
|-------------|-----------|------|---------|
| Ígnea | Bien | Bien | Bien |
| Vampírica | **Muy buena** | Regular | Regular |
| Del Eco | Regular | **Muy buena** | Regular |
| De Vacío | Regular | **Muy buena** | Bien |
| Sísmica | **Muy buena** | Regular | Regular |
| Del Caos | Regular | Bien | Regular |
| Resonante | Regular | Regular | **Muy buena** |
| Espectral | Regular | Regular | **Muy buena** |

---

## Notas de diseño

- La **Inscripción Resonante** tiene sinergia especial con el Tirador (monoobjetivo, alta cadencia).
- La **Inscripción de Vacío** es el counter a los enemigos tanque.
- La **Inscripción Vampírica** convierte al Caballero en un tanque auto-sostenible.
- Necesitan valor de `X` concreto para Vampírica (pendiente balanceo del sistema de stats).

---
title: Tipos de Daño — Sistema
tags:
  - kausarina-game
  - daño
  - sistema
  - combate
date: 2026-05-11
status: diseño
---

# Tipos de Daño

> [!abstract] Concepto
> Kaosuarina implementa múltiples tipos de daño para crear sinergia entre builds, items e inscripciones. Cada tipo tiene resistencias en los enemigos y escala con diferentes stats.

---

## Tipos comunes

| Tipo | Descripción | Reducido por | Efecto secundario |
|------|-------------|-------------|-------------------|
| **Físico** | Golpes directos, balas sin modificar | Defensa física | — |
| **Mágico** | Hechizos y proyectiles arcanos | Resistencia mágica | — |
| **A Distancia** | Proyectiles puros sin escalar cuerpo a cuerpo | — | Rango extendido |
| **Fuego** | Ataques ígneos e inscripción ígnea | Resistencia fuego | Aplica **Quemadura** (DoT 3s, acumulable) |
| **Veneno** | Toxinas y ataques de veneno | Resistencia veneno | Aplica **Envenenamiento** (DoT stacks independientes) |

---

## Tipo único de Kaosuarina

### Caos Primordial

> [!important] Exclusivo de Kaosuarina
> Un tipo de daño diseñado específicamente para este juego, sin equivalente directo en otros títulos.

**Mecánica:**
- Ignora el **50% de toda defensa** del objetivo (física + mágica)
- Escala con la barra de **Corrupción** del jugador
- La Corrupción sube al: recibir daño, usar habilidades oscuras, o encontrar ciertos ítems malditos
- A mayor Corrupción → mayor daño de Caos → pero también mayor daño recibido de enemigos

**Tabla de escala:**

| Corrupción | Bonus daño Caos | Penalización daño recibido |
|------------|----------------|---------------------------|
| 0-25% | +0% | +0% |
| 25-50% | +15% | +10% |
| 50-75% | +35% | +25% |
| 75-100% | +60% | +50% |

**Identidad visual:**
- Proyectiles de color cambiante (ciclan entre púrpura, rojo, negro)
- Efecto de distorsión en el punto de impacto
- La barra de Corrupción se muestra en el HUD junto a la barra de HP

**Notas:**
- El Caos Primordial es la recompensa de jugar agresivo / arriesgado
- Ciertos items y la Reliquia del Mago interactúan con la Corrupción

---

## Interacción daño / resistencias (pendiente balanceo)

Los enemigos tendrán **perfiles de resistencia** distintos:
- BASICO → sin resistencias especiales
- TANQUE → alta resistencia física, débil a mágico
- SHOOTER → sin resistencias, móvil
- RAPIDO → sin resistencias, difícil de alcanzar
- Minijefes/Jefes → resistencias específicas a diseñar

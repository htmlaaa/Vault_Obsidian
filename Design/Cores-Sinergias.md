---
title: Sistema de Cores y Sinergias
tags:
  - design
  - cores
  - sinergias
  - sistemas
  - roguelite
aliases:
  - Cores
  - Sinergias
date: 2026-05-06
---

# Sistema de Cores y Sinergias

Sistema de profundidad roguelite para [[Kaosuarina]]. Cada personaje tiene un core pasivo permanente que interactúa con los upgrades existentes.

---

## Definición de Core

> [!abstract] ¿Qué es un core?
> Pasiva permanente siempre activa — sin botones, sin recursos, sin activación manual. Opera en segundo plano y escala con los upgrades elegidos durante la partida.

**Regla de oro**: el core nunca se desactiva. Lo que cambia es su magnitud según el estado de la partida.

---

## Core — Caballero

**Nombre**: Fortaleza Reactiva
**Personaje**: [[Mago-Kaos|Caballero]] (teal `#00E5CC`)

Al recibir daño, gana 1 stack de Armadura temporal (max 5 stacks). Cada stack reduce el siguiente golpe en 8% flat. Los stacks se consumen uno a uno. Sin daño durante 4s → pierde 1 stack/s hasta 0.

**Implementación**: `armorStacks` (int) en `Player.java`. En método de daño: si stacks > 0, reducir daño × 0.08 × stacks, decrementar stacks. En `update(delta)`: acumular `timeSinceLastHit`; si > 4s, decrementar 1 stack/s.

> [!success] Complejidad: SIMPLE
> Dos variables + lógica condicional en el método de daño existente.

---

## Core — Mago

**Nombre**: Resonancia Caótica
**Personaje**: Mago-Kaos (purple `#9B30FF`)

Las balas no se destruyen al impactar — rebotan hacia el enemigo más cercano en radio 200u. Máximo 1 rebote base. Sin enemigo cercano → se destruye normalmente. El rebote no reduce daño.

**Implementación**: flag `rebotesRestantes` en `Bala.java`. En `ColisionManager` al impacto: si `rebotesRestantes > 0`, buscar enemigo activo más cercano (O(n) sobre pool), redirigir velocidad de bala, decrementar counter.

> [!warning] Complejidad: MEDIO
> Modifica `Bala.java`, `ColisionManager`. QA cuidadoso por interacción con perforación + multibala.

---

## Core — Shooter

**Nombre**: Momentum de Combate
**Personaje**: Shooter-Kaos (amber `#FFB800`)

Kills acumulan Combo (max 10). Sin kill en 2s → -1 combo/s. Cada punto de Combo = +3% cadencia de disparo (max +30% a combo 10).

**Implementación**: `comboCount` (int) + `timeSinceLastKill` (float) en `Player.java`. Al matar: `comboCount = min(comboCount+1, 10)`, reset timer. Cooldown de disparo = `baseCooldown * (1 - 0.03f * comboCount)`.

> [!success] Complejidad: SIMPLE
> Un entero, un float, una multiplicación en el cooldown existente.

---

## Sinergias con upgrades existentes

| Core | + Upgrade | Resultado |
|------|-----------|-----------|
| Caballero | Vida Máxima +20 | Cada 20 HP extra sobre base (100) → +1 stack máximo de Armadura (cap absoluto: 8) |
| Caballero | Velocidad | Moverse más rápido = menos hits = conservar stacks más tiempo. Sinergia emergente. |
| Caballero | Perforación | Balas perforantes activan stack-consumo una vez aunque continúen — protección garantizada |
| Mago | Bala Extra | Cada bala del abanico rebota independiente — caos geométrico sin código extra |
| Mago | Daño +20% | Rebote hereda daño completo → cada stack de daño vale ×2 efectivo |
| Mago | Perforación | Perfora primero → luego rebota: hasta 3 hits por bala (ajuste menor en ColisionManager) |
| Shooter | Cadencia +15% | Se compone con Combo: max cadencia + max combo → ×2.3 cadencia base |
| Shooter | Velocidad | Más velocidad = más kills seguidos = Combo más fácil de mantener |
| Shooter | Bala Extra | Más balas por disparo = más probabilidad de kill = Combo más consistente |

---

## Upgrades nuevos propuestos

### Para Caballero
- **Contraataque** — Al consumir stack de Armadura → siguiente bala +40% daño. Max nivel 2 (40%/80%). Flag `nextShotBoosted` en Player.
- **Voluntad de Hierro** — Con ≥3 stacks activos, regenerar 2 HP/s. Max nivel 2 (2/4 HP/s).

### Para Mago
- **Segunda Resonancia** — +1 rebote por bala (total: 2). Max nivel 1. Implementación: `maxRebotes++` en UpgradeManager.

### Para Shooter
- **Adrenalina** — Al alcanzar Combo 10 → burst +50% velocidad 1.5s. Solo activa una vez por llegada al máximo. Max nivel 2 (1.5s/2.5s).

---

## UI — concepto mínimo

- **HUD**: icono del core + número de estado (stacks / combo) debajo de barra XP
- **LevelUpScreen**: chispa visual en upgrades con sinergia conocida con el core activo
- **Sin tooltips durante gameplay** — el jugador aprende jugando

---

## Orden de implementación

> [!todo] Prioridad
> 1. **Shooter — Momentum** (más simple, valida arquitectura entera)
> 2. **Caballero — Fortaleza Reactiva** (simple, pero modifica path de daño)
> 3. **Mago — Resonancia Caótica** (medio, afecta ColisionManager — con margen de tiempo)

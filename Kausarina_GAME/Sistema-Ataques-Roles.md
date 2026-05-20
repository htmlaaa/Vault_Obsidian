---
title: Sistema de Ataques por Rol
tags:
  - kausarina-game
  - sistema-ataques
  - pre-sprint4
date: 2026-05-12
status: implementado
sprint: pre-sprint4
---

> [!danger]+ ⚠️ LEER OBLIGATORIAMENTE ANTES DE SPRINT 4
> Esta nota documenta trabajo adelantado del Sprint 4 (ataques diferenciados por rol + sistema de animaciones). Revisar en detalle al comenzar Sprint 4 para conectar con el sistema de armas e inscripciones. Ver también [[Pendientes-Tecnicos]] para el checklist de lo que falta.

# Sistema de Ataques por Rol

> [!abstract] Propósito
> Define cómo ataca cada personaje. El **Tirador** conserva el auto-shoot original. **Caballero** y **Mago** pasan a ataque manual con clic izquierdo (ligero) y clic derecho (pesado).

---

## Modos de ataque (`Role.AttackMode`)

| Rol | Modo | Trigger |
|-----|------|---------|
| Tirador | `AUTO_SHOOT` | Automático cada `baseShootCooldown` |
| Caballero | `MELEE` | Clic izq / Clic der |
| Mago | `MAGIC` | Clic izq / Clic der |

---

## Tirador — sin cambios

Auto-dispara un abanico de balas hacia el ratón cada `0.16 s` (con upgrades). No usa maná. Reliquia: Momentum de Combate (kills reducen CD).

---

## Caballero — Arco de Espada

Ataca en un sector circular en la dirección del ratón. Visual: arco relleno con `ShapeRenderer.arc()` que aparece y desvanece.

| Parámetro | Ligero (clic izq) | Pesado (clic der) |
|-----------|:-----------------:|:-----------------:|
| Ángulo del arco | 120° | 200° |
| Radio | 140 px | 210 px |
| Daño base | 22 | 45 |
| Cooldown | 0.35 s | 1.0 s |
| Tipo de daño | `FISICO` | `FISICO` |
| Color visual | Amarillo-blanco | Azul-blanco |
| Duración flash | 0.15 s | 0.25 s |

> [!tip] Mecánica
> El arco se centra en el ángulo del ratón. Los enemigos dentro del sector reciben daño inmediato (hit-scan, sin proyectil).

> [!warning] Pendiente
> Animación `swing` del Caballero bloqueada por límite del plan gratuito de PixelLab. Durante el ataque se muestra el sprite estático direccional como fallback.

---

## Mago — Bolt + Explosión

### Ligero (clic izq) — Bolt Mágico

Lanza un proyectil `MAGICO` usando el pool de balas existente. **Auto-aim**: busca el enemigo más cercano en radio 400 px y apunta hacia él; si no hay ninguno, usa la dirección del ratón.

| Parámetro | Valor |
|-----------|-------|
| Daño | 22 |
| Cooldown | 0.28 s |
| Coste maná | 8 |
| Radio auto-aim | 400 px |
| Tipo de daño | `MAGICO` |

### Pesado (clic der) — Explosión AoE

Explosión circular instantánea alrededor del jugador. Visual: círculo morado que se expande y desvanece.

| Parámetro | Valor |
|-----------|-------|
| Daño | 55 |
| Radio blast | 180 px |
| Cooldown | 1.5 s |
| Coste maná | 35 |
| Tipo de daño | `MAGICO` |
| Duración visual | 0.25 s |

> [!tip] Mecánica
> Si no hay maná suficiente, el ataque no se ejecuta (sin animación ni cooldown consumido).

---

## Arquitectura — ficheros modificados

```
utils/Constants.java        ← MELEE_LIGHT/HEAVY_RADIUS, MELEE_LIGHT/HEAVY_ARC_DEG,
                               MAGIC_LIGHT_RANGE, MAGIC_BLAST_RADIUS
Systems/PlayerStats.java    ← lightAttackCooldown, heavyAttackCooldown,
                               meleeLightDamage, meleeHeavyDamage,
                               magicLightDamage, magicHeavyDamage,
                               magicLightManaCost, magicHeavyManaCost
roles/Role.java             ← enum AttackMode { AUTO_SHOOT, MELEE, MAGIC }
                               campo attackMode derivado del tipo de rol
utils/ColisionManager.java  ← comprobarMelee(), comprobarBlast(), nearestEnemy()
entities/Player.java        ← triggerLightAttack(), triggerHeavyAttack(),
                               renderAttackEffect(ShapeRenderer),
                               auto-shoot solo si attackMode == AUTO_SHOOT
screens/GameScreen.java     ← leftClickJust/rightClickJust,
                               llamadas a trigger* en actualizarJuego(),
                               recompensarKills() helper,
                               ShapeRenderer Filled pass para efectos
```

---

## Flujo de kills manuales

```
clic izq/der
  → player.triggerLight/HeavyAttack(poolBalas, aimAngle, poolEnemigos)
     → ColisionManager.comprobarMelee / comprobarBlast
        → enemy.recibirDaño(...)
        → devuelve kills
  → GameScreen.recompensarKills(n)
     → hud.addScore / addExperience
     → player.onKill() × n
     → stats.añadirMana(5) × n
```

---

## Relación con otros sistemas

- [[Sistema-Mana-Armas-Dual]] — el maná consumido en ataques del Mago interacciona con la regen pasiva (2/s) de su rol
- [[Solucionadas/implementados/Reliquias-Brainstorm]] — ReliquiaCaballero (Fortaleza Reactiva) reduce daño recibido pero no afecta el ataque; ReliquiaMago (Resonancia Caótica) añade rebotes al bolt

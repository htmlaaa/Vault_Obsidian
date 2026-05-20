---
title: "Balance v1 — 2026-05-20"
date: 2026-05-20
tags:
  - balance
  - kaosuarina
---

# Balance v1

Primer pase de balance serio tras implementar los sistemas de inventario, upgrades por rol y amuletos completos. El objetivo fue eliminar builds rotas que hacían el juego trivial o imposible.

---

## Invulnerabilidad post-golpe

> [!bug] Problema detectado
> Con 0.6-0.7s de invulnerabilidad + lifesteal, el Caballero era prácticamente inmortal: recibía daño, se curaba más de lo que recibía durante el siguiente segundo, y el ciclo se repetía infinitamente sin ningún riesgo real.

| Rol | Antes | Después | Constante |
|---|---|---|---|
| Caballero | 0.60s | **0.30s** | `INVULNERABILITY_CABALLERO` |
| Mago | 0.70s | **0.30s** | `INVULNERABILITY_MAGO` |
| Tirador | 0.45s | **0.20s** | `INVULNERABILITY_SHOOTER` |

El Mago tenía la invulnerabilidad más alta (0.70s) por ser el rol de menor HP, pero era excesiva. El Tirador es el más ágil y puede evitar más golpes, así que tiene la más corta.

---

## Lifesteal

> [!bug] Problema detectado
> `LIFESTEAL_BASE_PERCENT = 0.15f` (15%) combinado con el alto daño de Caballero creaba un loop de curación infinita. Un hit de 38 daño = roba 5.7 HP, más que suficiente para compensar cualquier daño recibido entre golpes.

| Fuente | Antes | Después |
|---|---|---|
| Upgrade Vampirismo | 15% | **3%** |
| Amuleto Collar Vampírico | +12% (acumulable) | **+2.4%** |
| Amuleto Sed de Sangre | +12% | **+2.4%** |

Dividido aproximadamente por 5 en todos los casos. El lifesteal ahora es un complemento útil (Caballero con 38 dmg roba ~1.14 HP por hit) sin ser una mecánica rota.

---

## Daño base de roles

### Caballero

> [!bug] Problema detectado
> El Caballero con 22/45 de daño base era completamente inferior al Tirador que hace daño automático continuo. La fantasía de rol de "tanque pesado que golpea fuerte" no se cumplía.

| Ataque | Antes | Después |
|---|---|---|
| Ataque ligero | 22 | **38** |
| Ataque pesado | 45 | **72** |
| Cooldown ligero | 0.35s | 0.20s _(buff de sprint anterior)_ |

Con el upgrade `Golpe Letal` (×1.30 por nivel, hasta nivel 4), el Caballero puede llegar a 38×2.86 = **108 daño ligero** y 72×2.86 = **206 daño pesado** en late game.

### Mago

> [!bug] Problema detectado
> El Mago era "una fumada" según el jugador. Con 22/55 de daño base y la necesidad de consumir maná para cada ataque, el DPS era muy inferior al Tirador. El rol se sentía desventajoso sin ningún payoff.

| Ataque | Antes | Después |
|---|---|---|
| Bolt ligero | 22 | **34** |
| Explosión pesada (blast) | 55 | **85** |

Además se añadió el upgrade `Resonancia Amplificada` exclusivo del Mago (×1.40 radio de blast por nivel, hasta nivel 3), que convierte la explosión en un ataque AoE que puede limpiar una sala en mid-game.

---

## Armas por rol en drops

> [!bug] Problema detectado
> El Tirador recibía `PISTOLAS_GEMELAS` en cofres y drops. Como el Tirador ya tiene auto-disparo nativo (su mechanic core), el arma era inútil para él y desperdiciaría el slot.

**WeaponDropper** ahora usa tabla de distribución por rol:

| Rol | Distribución |
|---|---|
| CABALLERO | melee primario, ranged/magic secundario |
| MAGO | magic primario, melee/ranged secundario |
| SHOOTER | **75% melee, 25% magic** — nunca PISTOLAS_GEMELAS |

---

## Escalado de enemigos (depths 11-20)

> [!bug] Problema detectado
> El archivo `depthscaling.json` solo cubría hasta depth 10. A partir de ahí los multiplicadores no existían y los enemigos salían con stats base (hp×1, dmg×1), haciendo el juego trivial en una run larga.

| Depth | HP mult | DMG mult | Speed mult | Elite % |
|---|---|---|---|---|
| 10 | 3.6 | 2.25 | 1.25 | 38% |
| 11 | 4.9 | 2.60 | 1.27 | 42% |
| 12 | 5.6 | 2.75 | 1.28 | 44% |
| 13 | 6.6 | 3.00 | 1.30 | 46% |
| 14 | 7.6 | 3.20 | 1.31 | 48% |
| 15 | 8.8 | 3.45 | 1.32 | 50% |
| 16 | 10.2 | 3.70 | 1.33 | 52% |
| 17 | 11.8 | 4.00 | 1.34 | 54% |
| 18 | 13.8 | 4.30 | 1.35 | 56% |
| 19 | 15.8 | 4.65 | 1.36 | 58% |
| 20 | 18.0 | 5.00 | 1.38 | 60% |

A depth 20, los enemigos tienen 5× el daño y 18× la vida base. Una run completa de 20 niveles debería durar ~35-45 minutos con un build optimizado.

---

## Notas de diseño

- El balance actual es un **punto de partida funcional**, no el balance final. Faltan datos de playtest real con los 3 roles en builds completas.
- El Caballero sigue teniendo la ventaja de supervivencia (más HP, más defensa upgradeable) pero ahora el Mago tiene el mayor DPS de área potencial.
- El Tirador es el rol de menor complejidad cognitiva (auto-disparo) pero mayor skill expression en posicionamiento.
- Próximo pase de balance: revisar curva de XP, tiempo por depth, y si el Mago tiene suficiente maná regenerado para mantener spam de bolt.

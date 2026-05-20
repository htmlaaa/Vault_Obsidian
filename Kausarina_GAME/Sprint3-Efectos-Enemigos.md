---
title: Sprint 3 — Efectos de Estado y Nuevos Enemigos
tags:
  - kausarina-game
  - sprint3
  - implementado
date: 2026-05-12
status: implementado
sprint: sprint3
---

# Sprint 3 — Efectos de Estado y Nuevos Enemigos

> [!abstract] Propósito
> Documenta todo lo implementado en Sprint 3: sistema de efectos de estado (quemadura/veneno), robo de vida (hook), veneno de contacto del jugador, y dos nuevos tipos de enemigo (MALDITO y ESPECTRAL).

---

## Resumen de cambios

| Fichero | Qué se añadió |
|---------|--------------|
| `utils/StatusEffect.java` | Nueva clase — tipo BURN/POISON, duración, tick, daño |
| `utils/Constants.java` | Constantes Sprint 3 (contacto, explosión, estados) |
| `entities/Enemy.java` | StatusEffect, MALDITO, ESPECTRAL, `debeExplotar` flag |
| `entities/PoolEnemigos.java` | Pesos actualizados (incluye MALDITO 12%, ESPECTRAL 8%) |
| `Systems/PlayerStats.java` | `lifeStealPercent = 0f` (hook Sprint 4) |
| `entities/Player.java` | `aplicarVeneno()`, `curar()`, tick veneno, tinte verde |
| `utils/ColisionManager.java` | `comprobarExplosionesMALDITO`, `aplicarStatusPorDanio`, inmunidad ESPECTRAL |
| `screens/GameScreen.java` | Llama a `comprobarExplosionesMALDITO` en `procesarColisiones` |

---

## 1. Sistema de Efectos de Estado (`StatusEffect.java`)

Clase de datos que vive en cada `Enemy`. Aplicada desde `ColisionManager` cuando una bala con `DamageType.FUEGO` o `DamageType.VENENO` impacta.

| Efecto | Trigger | Duración | Tick | Daño/tick |
|--------|---------|----------|------|-----------|
| BURN (quemadura) | DamageType.FUEGO | 3 s | 0.5 s | 8 |
| POISON (veneno) | DamageType.VENENO | 5 s | 1.0 s | 5 |

**Prioridad:** BURN > POISON. Si el enemigo arde y recibe veneno, se ignora. El mismo tipo refresca/extiende la duración (toma el máximo).

**Visual:** lerp 50% del color base hacia naranja (BURN) o verde (POISON) en `Enemy.render()`.

> [!warning] Nota de activación
> Ningún arma actual usa DamageType.FUEGO ni VENENO, así que en Sprint 3 los estados son "latentes". Se activarán en Sprint 4 cuando se implementen las armas elementales. La explosión del MALDITO sí aplica POISON a los enemigos en rango.

---

## 2. Nuevos Enemigos

### MALDITO

Persigue al jugador como el BASICO. Peligroso por sus dos mecánicas especiales.

| Parámetro | Valor |
|-----------|-------|
| Velocidad | 130 px/s |
| Tamaño | 30 px |
| HP | 45 |
| Color | Morado oscuro |
| Daño contacto | 10 |
| Spawn weight | 12 % |

**Mecánicas:**
- **Veneno de contacto al jugador:** al tocar al jugador aplica `aplicarVeneno(4s, 5 dps)`. El jugador sufre 5 de daño cada segundo durante 4 s (tinte verde en el sprite).
- **Explosión al morir:** radio 120 px, 25 de daño VENENO. Aplica POISON a todos los enemigos en rango. Funciona tanto si muere por ataque como por DoT (vía flag `debeExplotar` procesado en `comprobarExplosionesMALDITO`).

### ESPECTRAL

Persigue al jugador. Semi-transparente, inmune a daño físico.

| Parámetro | Valor |
|-----------|-------|
| Velocidad | 160 px/s |
| Tamaño | 28 px |
| HP | 35 |
| Color | Cian, alpha 0.45 |
| Resistencia mágica | 10 |
| Daño contacto | 7 |
| Spawn weight | 8 % |

**Mecánicas:**
- **Inmune a FISICO:** `calcularDaño` devuelve 0 para `DamageType.FISICO` → el Caballero no puede dañarlo con la espada.
- **+50% daño de FUEGO:** multiplicador 1.5× para `DamageType.FUEGO`.
- **Semi-transparente:** color con `alpha = 0.45` que se multiplica al 70% cuando tiene < 50% HP.

> [!tip] Contra-juego
> El ESPECTRAL es el contador del Caballero. El Mago (daño MAGICO) y armas de fuego (Sprint 4) son efectivos. El Tirador (A_DISTANCIA) no aplica multiplicador pero sí puede dañarlo.

---

## 3. Pesos de spawn actualizados

| Tipo | Sprint 2 | Sprint 3 |
|------|:--------:|:--------:|
| BASICO | 50 % | 40 % |
| RAPIDO | 25 % | 20 % |
| TANQUE | 15 % | 10 % |
| SHOOTER | 10 % | 10 % |
| MALDITO | — | 12 % |
| ESPECTRAL | — | 8 % |

---

## 4. Robo de vida — hook (Sprint 4)

Campo añadido a `PlayerStats`:

```java
public float lifeStealPercent = 0f;
```

Valor `0` para todos los roles actuales. Las armas/reliquias de Sprint 4 activarán el robo de vida asignando un valor > 0. La lógica de curación está preparada en `Player.curar(int)`.

---

## 5. Flujo de explosión del MALDITO

```
Enemy.recibirDanio() / status tick
  → health <= 0 && tipo == MALDITO
     → debeExplotar = true; active = false

GameScreen.procesarColisiones()
  → ColisionManager.comprobarExplosionesMALDITO(pool)
     → for each enemy with debeExplotar == true
          debeExplotar = false
          explocionMaldito(pool, position)
            → blast radius 120px, 25 dmg VENENO
            → aplicarStatusPorDanio → POISON en supervivientes
            → devuelve kills adicionales
  → recompensarKills(kills)
```

---

## Limitaciones conocidas

- El MALDITO no encadena explosiones recursivamente (diseño intencional para evitar lag en grupos grandes). Si un MALDITO muere por la explosión de otro MALDITO en el mismo frame, su explosión se procesa en el **siguiente frame** (porque `debeExplotar` se revisa al inicio de `comprobarExplosionesMALDITO`).
- El veneno del jugador (de MALDITO) no tiene indicador en el HUD. Pendiente Sprint 4/UX.

---

## Relación con otros sistemas

- [[Sistema-Ataques-Roles]] — el Caballero no puede dañar al ESPECTRAL con ataques físicos
- [[Pendientes-Tecnicos]] — `lifeStealPercent` pendiente de activar en Sprint 4
- [[Sistema-Mana-Armas-Dual]] — armas elementales (Sprint 4) activarán BURN/POISON en enemigos

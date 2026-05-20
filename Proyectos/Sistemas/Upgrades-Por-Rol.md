---
title: "Sistema de Upgrades por Rol"
tags:
  - sistema
  - upgrades
  - kaosuarina
status: implementado
---

# Sistema de Upgrades por Rol

## Motivación

Antes todos los upgrades aparecían para todos los roles. El Mago podía coger "Bala Extra" (inútil: no dispara balas) y el Caballero podía coger "Perforación" (inútil: no tiene proyectiles). Ahora el pool de upgrades se filtra por rol activo, garantizando que las 3 opciones siempre sean relevantes.

## Upgrades genéricos (todos los roles)

| Upgrade | Tipo | Efecto por nivel | Niveles máx |
|---|---|---|---|
| Daño +40% | `DANIO_UP` | multiplicador ×1.4 acumulable | 5 |
| Cadencia +15% | `CADENCIA_UP` | multiplicador ×1.15 acumulable | 5 |
| Velocidad +10% | `VELOCIDAD_UP` | multiplicador ×1.10 acumulable | 5 |
| Vida Máx +20 | `VIDA_MAXIMA_UP` | +20 HP al momento | 3 |
| Filo Ígneo | `FILO_IGNEO` | aplica Quemadura en cada hit | 1 |
| Cuchilla Venenosa | `CUCHILLA_VENENO` | aplica Veneno en cada hit | 1 |
| Vampirismo | `VAMPIRISMO` | 3% lifesteal | 1 |

## Caballero

| Upgrade | Tipo | Efecto | Niveles máx |
|---|---|---|---|
| Golpe Letal | `GOLPE_PESADO` | ×1.30 daño melee light + heavy (acumulable) | 4 |
| Armadura Reforzada | `DEFENSA` | +8 defensa permanente (`defensa` y `defBase`) | 4 |

> [!tip] Sinergia Caballero
> Golpe Letal nivel 4 = ×2.86 daño melee sobre el base ya buffeado (38/72). Con Daño genérico stacked encima, el Caballero puede one-shot enemigos élite en mid-game.

## Mago

| Upgrade | Tipo | Efecto | Niveles máx |
|---|---|---|---|
| Pozo Arcano | `MANA_MAXIMO_UP` | +30 maná máximo (actualiza `mana` y `manaMaxBase`) | 4 |
| Resonancia Amplificada | `RESONANCIA` | ×1.40 radio de explosión mágica (`blastRadiusMult`) | 3 |
| Foco Arcano | `CADENCIA_MAGICA` | ×0.80 cooldown del bolt ligero (`lightAttackCooldown`) | 4 |

> [!tip] Sinergia Mago
> Resonancia nivel 3 = radio ×2.744. Combinado con el blast base, limpia grupos densos de un solo ataque pesado. Foco Arcano nivel 4 lleva el bolt de 0.40s → 0.164s (casi spam).

## Tirador

| Upgrade | Tipo | Efecto | Niveles máx |
|---|---|---|---|
| Perforación | `PERFORACION` | pierce +1 por nivel (atraviesa N enemigos extra) | 3 |
| Bala Extra | `BALA_EXTRA` | +1 bala por ráfaga | 3 |
| Recarga Veloz | `RECARGA_RAPIDA` | ×0.85 cooldown de disparo automático | 4 |

> [!tip] Sinergia Tirador
> Bala Extra nivel 3 + Recarga Veloz nivel 4 = 5 balas por ráfaga a cooldown mínimo. Con Perforación, cada bala puede golpear hasta 4 enemigos. DPS AoE masivo.

## Implementación técnica

### `Upgrade.java`
```java
public Role.Tipo[] roles;  // null = disponible para todos

public boolean isAvailableFor(Role.Tipo role) {
    if (roles == null || role == null) return true;
    for (Role.Tipo r : roles) if (r == role) return true;
    return false;
}

// Constructor varargs para roles específicos:
public Upgrade(Tipo tipo, String nombre, String desc, int nivelMax, Role.Tipo... roles) { ... }
```

### `UpgradeManager.java`
```java
private Role.Tipo currentRole = null;

public void setRole(Role.Tipo role) { this.currentRole = role; }

public Array<Upgrade> getUpgradesAleatorios(int cantidad) {
    Array<Upgrade> disponibles = new Array<>();
    for (Upgrade u : todosLosUpgrades) {
        if (u.puedeMejorar() && u.isAvailableFor(currentRole)) disponibles.add(u);
    }
    // ... selección aleatoria de `cantidad` de los disponibles
}
```

`setRole()` se llama en `GameScreen.inicializar()` justo después de crear el jugador con su rol.

### `GameScreen.aplicarBonusUpgrade()`

Aplica el efecto real de cada upgrade a `PlayerStats` cuando el jugador selecciona:

```java
case GOLPE_PESADO:
    stats.meleeLightDamage = (int)(stats.meleeLightDamage * 1.30f);
    stats.meleeHeavyDamage = (int)(stats.meleeHeavyDamage * 1.30f);
    break;
case DEFENSA:
    stats.defensa += 8f;
    stats.defBase += 8f;
    break;
case MANA_MAXIMO_UP:
    stats.maxMana += 30f;
    stats.manaMaxBase += 30f;
    stats.mana = Math.min(stats.mana + 30f, stats.maxMana);
    break;
case RESONANCIA:
    stats.blastRadiusMult *= 1.40f;
    break;
case CADENCIA_MAGICA:
    stats.lightAttackCooldown *= 0.80f;
    break;
case RECARGA_RAPIDA:
    stats.baseShootCooldown *= 0.85f;
    break;
```

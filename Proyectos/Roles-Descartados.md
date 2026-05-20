---
title: Roles Descartados — Kaosuarina
tags:
  - descartado
  - diseño
  - roles
aliases:
  - Roles Descartados
cssclasses: []
---

# Roles Descartados

Roles conceptuales definidos en la fase inicial de diseño de [[Kaosuarina]] que **no se implementarán** en el proyecto TFG.

El alcance definitivo incluye solo 3 roles: **Caballero**, **Mago** y **Tirador**. Ver [[Kaosuarina#Roles jugables]] para los stats y reliquias de los roles activos.

> [!info] Motivo del descarte
> Reducción de alcance para ajustarse al tiempo disponible del TFG. Los 3 roles implementados cubren los arquetipos esenciales (melee/tanque, caster/mágico, shooter/velocidad). Los roles aquí listados quedan como referencia de diseño por si el proyecto se retomara.

## Roles descartados

| Rol | Vida | Velocidad | Daño base | Core previsto | Estilo |
|-----|------|-----------|-----------|---------------|--------|
| Base | 100 | 300 px/s | 10 | Neutro | Flexible, cualquier build |
| Tanque | 150 | 250 px/s | 15 | Resistencia | Alta vida y defensa pasiva |
| Daño | 80 | 320 px/s | 25 | Fuego explosivo | Proyectiles explosivos, muy ofensivo |
| Control | 90 | 280 px/s | 15 | Gravedad/Slow | Ralentiza y atrae enemigos |
| Veneno | 100 | 300 px/s | 12 | DoT ácido | Daño progresivo, efectos encadenables |
| Velocidad | 70 | 400 px/s | 10 | Agilidad | Máxima movilidad y cadencia |
| Torretas | 110 | 270 px/s | 10 | Dispositivos | Coloca torretas/drones de apoyo |

## Notas de diseño

- El **rol Base** fue el punto de partida del diseño — sus stats genéricos sobreviven como los valores por defecto del constructor `PlayerStats()`.
- El **rol Tanque** (jugable) fue suplantado por el **Caballero**: misma fantasía (alta supervivencia), mecánica más interesante (armadura reactiva en lugar de HP plano).
- El **rol Daño** y el **rol Veneno** tienen su infraestructura parcialmente lista: `DamageType.FUEGO` y `DamageType.VENENO` existen en el engine y aplican `StatusEffect.BURN` / `POISON`. Si se quisieran rescatar, solo faltaría el factory method en `Role.java` y la `Reliquia` correspondiente.
- El **rol Control** y las **Torretas** requerirían sistemas sin precedente en el código actual: entidades de torreta en pool propio, mecánicas de atracción/slow en `ColisionManager`, etc. Son los más costosos de rescatar.
- El **rol Velocidad** sería el más sencillo de implementar (básicamente un Tirador con stats extremos y una `Reliquia` distinta).

> [!tip] Infraestructura reutilizable si se rescata algún rol
> `DamageType`, `StatusEffect`, `ColisionManager.aplicarStatusPorDanio()`, y el patrón `Role` + `Reliquia` están diseñados para ser extensibles. Añadir un nuevo rol no requiere tocar el engine — solo un nuevo factory method y una nueva `Reliquia`.

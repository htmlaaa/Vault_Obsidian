---
title: Sistema de Maná y Armas Dual — Diseño
tags:
  - kausarina-game
  - mana
  - armas
  - sistema
  - dia2
date: 2026-05-11
status: diseño-confirmado
---

# Sistema de Maná y Armas Dual

> [!abstract] Decisión confirmada 2026-05-11 (Día 2 Sprint 2)
> Se implementan dos tipos de arma y maná universal con reglas distintas por personaje.

---

## Dos tipos de arma

| Tipo | Descripción | Coste | Disparo |
|------|-------------|-------|---------|
| **Arma Normal** | Auto-disparo continuo | Sin maná | Automático |
| **Arma de Habilidad** | Efecto especial poderoso | Coste de maná | Manual o cooldown |

- El jugador puede llevar **2 armas simultáneas** (cualquier combinación de tipos)
- Ambas pueden ser normales, ambas de habilidad, o una de cada
- Cada arma tiene 1 slot de [[Inscripciones-Sistema|Inscripción]] pasiva

---

## Maná por personaje

| Rol | MaxManá base | Regen pasiva | Notas |
|-----|-------------|-------------|-------|
| **Mago** | 120 | +2/s (siempre) | Única regen pasiva del juego |
| **Caballero** | 50 | 0 | Solo recupera por mecánicas activas |
| **Tirador** | 30 | 0 | Solo recupera por mecánicas activas |

> [!important] Privilegio del Mago
> La regen pasiva de maná es **exclusiva del Mago**. Es una parte fundamental de su identidad de rol. Los otros personajes NUNCA tendrán regen pasiva base — solo a través de ítems/amuletos específicos (y escasos).

---

## Formas de recuperar maná (no-Mago)

Evaluación de las 5 ideas propuestas:

| # | Mecánica | Incluir | Valor |
|---|----------|---------|-------|
| 1 | **Sed de Sangre** — kills dan maná | ✅ Sí | +5 por kill |
| 2 | **Inscripción Vampírica de Maná** — impactos recuperan maná | ✅ Sí | +2 por impacto del arma |
| 3 | **Overkill** — daño excedente → maná | ✅ Sí | +1 por cada 10 daño excedente |
| 4 | **Guardián de la Arena** — oleada sin daño +20 maná | ⚠️ Opcional | Complejo de detectar; dejar para Sprint 4 |
| 5 | **Cicatriz de Combate** — fin de invul → maná | ⚠️ Opcional | Interesante pero puede ser confuso; Sprint 4 |

**Para Sprint 2/3**: solo Sed de Sangre. Las demás llegan con el sistema de armas completo.

---

## ¿Qué pasa sin maná?

- El **Arma de Habilidad** no se puede activar (bloqueada)
- Visual: el botón/indicador del arma de habilidad se oscurece / parpadea rojo
- El **Arma Normal** siempre funciona independientemente del maná
- Sin maná no hay "penalización" — solo pierdes acceso a la habilidad

---

## Ideas de recuperación del Mago

Además de la regen pasiva, el Mago también puede recuperar maná por:
- Kills mágicos: +5 maná (como todos los demás)
- Reliquia "Grimorio de Sangre": +3 maná por kill (en lugar de +5 universal)
- Inscripción Vampírica de Maná en su arma: +2 por impacto

---

## Ideas de Armas (6 en total, diseño preliminar)

*GDD completo en `Code_Game/design/gdd/02-weapon-system.md` (generado por agente Opus)*

### Caballero (afinidad melee/físico)
- **Espada del Juicio** — Normal. Golpes en área frontal, daño físico. Stat bonus: +Defensa
- **Lanza del Tormento** — Habilidad (coste 30 maná). Carga rectilínea que atraviesa enemigos

### Mago (afinidad mágico)
- **Orbe Arcano** — Normal. Proyectil lento que rebota, daño mágico. Stat bonus: +Maná máx.
- **Torbellino de Caos** — Habilidad (coste 40 maná). Explosión arcana en área alrededor del jugador

### Tirador (afinidad a distancia/velocidad)
- **Pistola Doble** — Normal. Alta cadencia, 2 proyectiles simultáneos, daño a distancia. Stat bonus: +Velocidad
- **Rifle de Sombra** — Habilidad (coste 25 maná). Bala única que perfora todos los enemigos en línea

---

## GDDs en Code_Game

| Archivo | Estado |
|---------|--------|
| `design/gdd/02-weapon-system.md` | Generado por Opus |
| `design/gdd/03-mana-system.md` | Generado por Opus |

---

## Implementación en código

**S2-05 (hecho)**: `PlayerStats.java` — campos `maxMana`, `mana`, `manaRegen`, `defensa`, `resistenciaMagica`, `rango` + métodos `añadirMana()` y `consumirMana()`

**S2-06 (pendiente)**: `Role.java` — valores confirmados por rol

**S2-10 (pendiente)**: regen en `Player.update()`, Sed de Sangre en `GameScreen.onKill()`, barra de maná en `HUD`

---
title: Pendiente — Reverse-Document GDDs v0.1
tags:
  - pendiente
  - documentacion
  - gdd
  - code-game-studio
date: 2026-05-08
status: cerrado
---

# Pendiente — `/reverse-document` GDDs del v0.1

> [!abstract] Contexto
> Se lanzó `/reverse-document` desde el Code Game Studio para generar los GDDs del código v0.1 ya implementado. El análisis del código está completo. Estamos en **Fase 4 del workflow** (Presentar Hallazgos) esperando respuestas a 5 preguntas de intención antes de redactar.

---

## Estado actual

- [x] Análisis completo del código fuente leído
- [x] Hallazgos presentados al usuario
- [x] **Responder las 5 preguntas de intención** ← bloqueante
- [x] Redactar los 3 GDDs
- [x] Aprobación del usuario
- [x] Escribir archivos a `Code_Game/design/gdd/`

---

## Documentos a generar (cuando se desbloquee)

| Archivo destino | Contenido |
|-----------------|-----------|
| `Code_Game/design/gdd/core-loop.md` | Movimiento, disparo, enemigos, XP/nivel, dificultad |
| `Code_Game/design/gdd/role-system.md` | Roles (Caballero/Mago/Shooter), PlayerStats, CoreEffects |
| `Code_Game/design/gdd/upgrade-system.md` | 6 upgrades, fórmulas, selección aleatoria |

---

## Las 5 preguntas sin responder

> [!question]- 1. ¿Daño +40% o +20%?
> El vault dice "+20%" pero el código tiene `0.4f * u.nivel` → **+40% por nivel**.
> ¿El código es la fuente verdad y el vault está desactualizado?

> [!question]- 2. ¿Todos los enemigos hacen el mismo daño de contacto (10)?
> BASICO, RAPIDO, TANQUE y SHOOTER todos aplican 10 HP al tocar al jugador.
> ¿Es diseño intencional (simplicidad) o placeholder que habrá que diferenciar (TANQUE más, RAPIDO menos)?

> [!question]- 3. ¿Qué pasa con el rol BASE?
> `Role.base()` existe en el código pero **no aparece en CharacterSelectScreen** — solo lo usa un constructor de `Player` que ya no se llama desde ningún sitio.
> ¿Está deprecated y se puede eliminar, o quieres reservarlo para algo (tutorial, modo especial)?

> [!bug]- 4. Bug en ShooterCore — el decay del combo nunca ocurre
> `comboCount -= (int)(DECAY_RATE * delta + 0.5f)` con delta normal (~0.016s) da siempre `(int)0.516 = 0`.
> El combo **nunca decrementa** a 60fps. Sólo decrecería en frame spikes graves (delta ≥ 0.5s).
> ¿Debo documentarlo como bug conocido o como comportamiento intencional?
> (Corrección obvia: usar un acumulador igual que `CaballeroCore`.)

> [!question]- 5. ¿Pierce + Bounce son mutuamente excluyentes por hit (Mago + upgrade Perforación)?
> `onHit()` sólo desactiva la bala cuando `pierceLeft ≤ 0`. El rebote sólo se activa cuando la bala se desactiva (`!sigueActiva && rebotesRestantes > 0`).
> Con PERFORACIÓN activa, la bala perfora **sin rebotar** hasta que se agota el pierce — entonces rebota.
> ¿Es la interacción intencionada (pierce y bounce excluyentes por hit) o es un gap de diseño?

---

## Hallazgos clave del análisis (resumen)

### Valores reales del código (algunos difieren del vault)

| Rol | Vel | Cooldown | Daño base | Balas | Spread | HP |
|-----|-----|----------|-----------|-------|--------|----|
| Base | 400 | 0.20s | 1.0 | 1 | 15° | 100 |
| Caballero | 250 | 0.25s | 1.5 | 1 | 15° | 150 |
| Mago | 270 | 0.22s | 1.0 | 1 | **20°** | 75 |
| Shooter | 320 | 0.18s | 1.2 | **3** | 12° | 80 |

### Fórmulas de upgrade (valores reales)

- `getMultiplicadorDanio()` = `1 + 0.4 × nivel` (no 0.2 como dice el vault)
- `getMultiplicadorCadencia()` = `1 + 0.15 × nivel`
- `getMultiplicadorVelocidad()` = `1 + 0.1 × nivel`
- Pierce: `pierceLeft = pierce + 1`, donde `pierce = getNivelPerforation()`
- Balas extra: `baseBulletCount + getBalasExtra()` (getBalasExtra suma los niveles)

### XP y dificultad

```
expToNextLevel₀ = 100
expToNextLevelₙ = expToNextLevelₙ₋₁ × 1.2
XP por kill = 25 (todos los tipos igual)
Oleada = 1 + (nivel / 3) enemigos
Spawn interval: empieza en 2s → × 0.85 cada 30s → mínimo 0.3s
Distancia de spawn: 800u desde el jugador
```

### Daño recibido

- Contacto con cualquier enemigo: **10 HP**
- BalaEnemiga (SHOOTER): **8 HP** (fijo, `BalaEnemiga.DAMAGE = 8`)

---

## Cómo retomar

Cuando tengas las respuestas, di algo como:

> "Retoma el reverse-document: [respuestas a las 5 preguntas]"

Y continuamos directamente desde Fase 5 (redactar los GDDs con los datos confirmados).

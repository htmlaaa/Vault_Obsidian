---
tags: [minijefe, enemigo, diseño, sprint-7]
estado: implementado
sprint: S7-03
fecha: 2026-05-17
---

# Arquero — Segundo Minijefe

## Concepto

El Arquero es el segundo minijefe del juego. A diferencia del Guardián (cuerpo a cuerpo, lento, golpe de onda), el Arquero es un tirador de larga distancia que **nunca se queda quieto**: se teletransporta cada 4 segundos para frustrar el tracking del jugador y castiga la falta de movimiento con proyectiles continuos.

Aparece en **oleada 20** y después cada 20 oleadas (40, 60…). El Guardián sigue apareciendo cada 10 oleadas, por lo que en la oleada 20 ambos coinciden — el pico de dificultad más alto de la partida hasta ese momento.

---

## Stats

| Stat | Valor |
|------|------:|
| HP | 500 |
| Velocidad | 90 u/s |
| Tamaño | 48 × 48 px |
| Color | Azul `(0.3, 0.5, 1.0)` |
| Defensa física | 0 |
| Resistencia Mágica | 15 |
| Daño de contacto | 15 |
| Daño proyectil | 18 |
| Intervalo disparo | 2 s |
| Intervalo teleporte | 4 s |
| Drop al morir | Cofre con arma aleatoria |

---

## Comportamiento

### Movimiento
- **Rango óptimo**: 350–600 u del jugador.
- Si está **demasiado cerca** (< 350 u): huye en línea recta.
- Si está **demasiado lejos** (> 600 u): se acerca al 60 % de velocidad.
- En rango óptimo: **strafea** perpendicularmente alrededor del jugador.

### Teleporte
- Cada **4 segundos** se reposiciona instantáneamente a ~480 u del jugador en ángulo aleatorio.
- El timer arranca con delay aleatorio (2–4 s) para que el primer teleporte no sea inmediato.
- Tras teleportarse, la velocidad se reinicia a 0 para evitar drift.

### Disparo
- Cada **2 segundos** lanza un `BalaEnemiga` hacia la posición actual del jugador.
- Daño: **18** (vs 8 del SHOOTER normal y 8 de la `BalaEnemiga` base).
- Velocidad del proyectil: 450 u/s (igual que las balas enemigas normales).

---

## Resistencias y Debilidades

| Tipo de daño | Efecto |
|--------------|--------|
| FISICO | Normal (sin defensa) |
| MAGICO | -15 reducción (resistenciaMagica) |
| CAOS / CAOS_PRIMORDIAL | -7.5 reducción (media entre defensa y resistencia) |
| FUEGO | Normal + quemadura 3 s |
| VENENO | Normal + veneno 5 s |

**Estrategia recomendada**: FUEGO o físico puro son los más efectivos. Magia es penalizada.

---

## Drop

Al morir dropea un **cofre de arma** en su posición (misma lógica que los cofres de ola, implementado con `spawnChestAt()`). Si ya hay un cofre activo en el mapa, el drop se cancela.

El Guardián sigue dropeando **scroll de inscripción**. Los dos minijefes tienen drops distintos para incentivar matar a ambos.

---

## HUD

Barra de vida **azul** centrada en la parte superior, posicionada igual que la del Guardián. El Guardián tiene prioridad: si ambos están vivos a la vez, se muestra la barra del Guardián.

---

## Constantes (`Constants.java`)

```java
ARQUERO_HP                = 500
ARQUERO_SPEED             = 90f
ARQUERO_CONTACT_DMG       = 15
ARQUERO_PROJECTILE_DMG    = 18
ARQUERO_SHOOT_INTERVAL    = 2f
ARQUERO_TELEPORT_INTERVAL = 4f
ARQUERO_MINIBOSS_WAVE     = 20
```

---

## Archivos implementados

- `entities/Enemy.java` — enum `Tipo.ARQUERO`, campo `arqueroTeleportTimer`, casos en `activate()` y `update()`
- `entities/PoolEnemigos.java` — `spawnArquero()`, `getActiveArquero()`
- `entities/BalaEnemiga.java` — soporte de daño configurable (`activate(…, int dmg)`)
- `entities/PoolBalasEnemigas.java` — `spawnWithDamage()`
- `utils/SharedTextures.java` — textura `arquero` 48 px
- `utils/ColisionManager.java` — `ARQUERO` en `contactDamagePor()`
- `utils/ParticlePool.java` — color azul en `colorForTipo(ARQUERO)`
- `ui/HUD.java` — barra azul con flag `bossIsArquero`
- `screens/GameScreen.java` — spawn en oleada 20, tracking `arqueroRef`, `spawnChestAt()` en muerte

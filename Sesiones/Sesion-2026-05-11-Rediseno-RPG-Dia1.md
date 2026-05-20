---
title: Sesión 2026-05-11 — Rediseño RPG (Día 1)
tags:
  - sesion
  - rediseno
  - rpg
  - sprint2
  - fixes
date: 2026-05-11
---

# Sesión 2026-05-11 — Rediseño RPG (Día 1)

Sesión de diseño completo del nuevo sistema RPG + implementación del Día 1 del Sprint 2 (fixes críticos). Compilación verificada.

---

## Lo hecho hoy

### Diseño — Sistema RPG completo (nuevo)

Se diseñó y documentó la dirección completa del rediseño desde v0.1 hacia RPG roguelite:

- **Reliquias** (reemplazan Cores): 1 única por personaje, siempre activa. Brainstorm de 4 opciones por personaje.
- **Inscripciones de Arma** (equiv. Ashes of War): 8 inscripciones pasivas con tabla de afinidades por rol.
- **Tipos de Daño**: Físico, Mágico, A Distancia, Fuego, Veneno + **Caos Primordial** (único del juego — ignora 50% defensa, escala con barra de Corrupción oculta).
- **Stats RPG confirmados** por rol: Caballero HP 200 / Def 20, Mago HP 70 / Maná 120, Tirador HP 80 / vel 340.
- **Maná**: regen pasiva lenta + por kills + por impactos. Mago tiene el mayor pool.
- **Armas**: 6 en total, 2 equipables. Si 2 iguales → ambas activas. Cada arma da stat bonuses + 1 slot inscripción.
- **Minijefes**: cada 10 rondas, sorteado al inicio. Mecánica **"El Portador + Presagio"**: en ronda 9, cambio ambiental (música/colores) + aparece El Portador. Si lo matas → minijefe ahora. Si no → llega en ronda 10 con -20% HP.
- **Mapa**: arena circular por ahora + pickups dispersos. Futuro: mapa explorable Nightreign.

### Documentos creados

**Vault `Kausarina_GAME/`** (nueva carpeta):

| Archivo | Contenido |
|---------|-----------|
| `Reliquias-Brainstorm.md` | 4 opciones por personaje |
| `Inscripciones-Sistema.md` | 8 inscripciones + tabla de afinidades |
| `Tipos-de-Daño.md` | 5 comunes + Caos Primordial con tabla de escalado |
| `Mecanica-Jefes-Brainstorm.md` | 5 ideas + propuesta combinada El Portador + Presagio |
| `Vision-Rediseno-RPG.md` | Visión general con todas las decisiones confirmadas |
| `README.md` | Índice de la carpeta |

**Code_Game:**

| Archivo | Contenido |
|---------|-----------|
| `design/gdd/00-stat-baseline.md` | Stats exactos del código v0.1 — fuente de verdad |
| `design/gdd/01-vision-rediseno-rpg.md` | Gap analysis + nuevos stats + plan de sprints |
| `production/sprints/sprint-2.md` | 11 stories detalladas con AC, archivos y pseudocódigo |
| `production/sprint-status.yaml` | Sprint 2 añadido (11 stories en backlog) |
| `production/stage.txt` | "Production" |
| `production/review-mode.txt` | "solo" |

### Estudio de código — Agente Opus

El agente Opus leyó los 15 archivos Java y detectó **7 bugs**, confirmó todos los stats exactos y generó el contenido de los GDDs.

Bugs clave detectados:
- **CRÍTICO**: ShooterCore decay nunca ocurre a 60fps → corregido hoy (S2-01)
- **MEDIO**: Multilevel pierde pantallas de upgrade → corregido hoy (S2-02)
- **BAJO**: `renderManaBar()` definido pero nunca llamado → se activa en S2-10
- **NOTA**: `Role.base()` activo cuando estaba deprecated → eliminado hoy (S2-03)

---

## Sprint 2 — Día 1 implementado ✅

### S2-01 — Fix ShooterCore decay ✅

**Bug**: `(int)(DECAY_RATE * delta + 0.5f)` = 0 a 60fps → combo permanente.  
**Fix**: Acumulador float `decayAccumulator` igual al patrón de `CaballeroCore`.

```java
// Antes (bug):
comboCount -= (int)(DECAY_RATE * delta + 0.5f);  // siempre 0

// Después (fix):
decayAccumulator += delta;
while (decayAccumulator >= 1f / DECAY_RATE) {
    comboCount = Math.max(0, comboCount - 1);
    decayAccumulator -= 1f / DECAY_RATE;
}
```

**Archivo**: `cores/ShooterCore.java`

---

### S2-02 — Fix multilevel: cola de upgrades ✅

**Bug**: Subir N niveles de golpe → solo aparece 1 pantalla de upgrade. Los N-1 restantes se perdían.  
**Fix**: Campo `pendingLevelUps` que acumula los niveles pendientes y los consume de uno en uno.

```java
// detectarLevelUp() — nuevo:
if (nivelActual > nivelAnterior) {
    pendingLevelUps += nivelActual - nivelAnterior;
    nivelAnterior = nivelActual;
}
if (pendingLevelUps > 0 && !levelUpScreen.isActive()) {
    pendingLevelUps--;
    levelUpScreen.show(upgradeManager.getUpgradesAleatorios(3));
}
```

**Archivo**: `screens/GameScreen.java`

---

### S2-03 — Eliminar Role.base() y constructor Player(x,y) ✅

- Eliminado `Tipo.BASE` del enum `Role.Tipo`
- Eliminado `Role.base()` factory method
- Eliminado import de `NullCoreEffect` de `Role.java`
- Eliminado constructor `Player(float x, float y)`
- `SpriteSheets.roleIndex()` usa `default: return -1` → seguro sin BASE

**Archivos**: `roles/Role.java`, `entities/Player.java`

---

### S2-04 — Centralizar daño de contacto ✅

Añadidos a `Constants.java`:
```java
public static final int CONTACT_DAMAGE_DEFAULT = 10;  // usado hasta S2-08
public static final int CONTACT_DAMAGE_BASICO  = 8;
public static final int CONTACT_DAMAGE_RAPIDO  = 6;
public static final int CONTACT_DAMAGE_TANQUE  = 15;
public static final int CONTACT_DAMAGE_SHOOTER = 8;
```

`GameScreen.procesarColisiones()`: `player.recibirDanio(10)` → `player.recibirDanio(Constants.CONTACT_DAMAGE_DEFAULT)`.

El daño diferenciado por tipo requiere refactor de `ColisionManager` — se implementa en **S2-08**.

**Archivos**: `utils/Constants.java`, `screens/GameScreen.java`

---

## Estado compilación

> [!success] `gradlew core:compileJava` — OK, sin errores

---

## Pendiente — Sprint 2 Día 2

| Story | Tarea |
|-------|-------|
| S2-05 | Extender `PlayerStats` con defensa, resMag, rango, maná |
| S2-06 | Reescalar HP y stats por rol (200/70/80) en `Role.java` |
| S2-07 | Reescalar HP enemigos + resistencias en `Enemy.java` |
| S2-10 | Maná básico Mago + activar `renderManaBar()` |

Ver [[Pasos-7-8-Pendientes]] para el contexto anterior del Sprint 1.

---

## Archivos modificados hoy

```
cores/ShooterCore.java              ← fix decay acumulador
screens/GameScreen.java             ← cola multilevel + CONTACT_DAMAGE_DEFAULT
roles/Role.java                     ← eliminado BASE + Role.base()
entities/Player.java                ← eliminado constructor 2-arg
utils/Constants.java                ← CONTACT_DAMAGE_* constantes
```

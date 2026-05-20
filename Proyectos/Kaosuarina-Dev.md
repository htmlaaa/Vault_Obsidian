---
title: Kaosuarina — Contexto de Desarrollo
tags:
  - proyecto
  - dev
  - java
  - libgdx
  - arquitectura
aliases:
  - Kaosuarina Dev
  - dev-context
date: 2026-05-05
---

# Kaosuarina — Contexto de Desarrollo

Nota viva de contexto técnico para el desarrollo del TFG. Complementa [[Kaosuarina]] (diseño de juego). Se actualiza con decisiones, reglas y notas de sesión.

---

## Workspace

| Directorio | Propósito |
|---|---|
| `A_Game_Kaosuarina/` | Juego — LibGDX/Java |
| `Code_Game/` | Framework multi-agente IA (49 agentes, 72 slash commands) |
| `Kausarina_Vault/` | Este vault — diseño + referencia técnica |
| `obsidian-skills/` | Skills de edición Obsidian |

El trabajo de código ocurre en `A_Game_Kaosuarina/`. El framework de agentes usa `Code_Game/`.

---

## Comandos de build

```bash
# Ejecutar juego (escritorio)
gradlew lwjgl3:run

# Compilar
gradlew build

# Empaquetar JAR
gradlew lwjgl3:jar   # output: lwjgl3/build/libs/

# Limpiar
gradlew clean
```

> [!tip] Windows
> Usar `gradlew.bat` si `gradlew` no está en PATH.

---

## Stack técnico

| Componente | Versión |
|---|---|
| Lenguaje | Java 8 |
| Framework | LibGDX 1.14.0 |
| Plataforma | LWJGL3 — 1280×720 |
| Física | Box2D 1.14.0 |
| ECS | Ashley 1.7.4 (disponible, **no usado aún**) |
| Build | Gradle + gdx-liftoff |
| Gráficos | Procedurales vía `Pixmap` — sin assets externos |

---

## Arquitectura

### Entry point

```
Lwjgl3Launcher
  → Lwjgl3Application(new KaosuarinaGame())
    → setScreen(new GameScreen())
```

### Estructura de código

Fuentes bajo `core/src/main/java/com/milwar/kaosuarina/`:

```
KaosuarinaGame.java          ← extiende Game, gestiona transiciones de pantalla
screens/
  GameScreen.java            ← loop principal (render, update, spawn, colisión)
  LevelUpScreen.java         ← overlay de selección de mejoras
entities/
  Player.java                ← posición, salud, disparo, upgrades
  Enemy.java                 ← 4 tipos: BASICO, RAPIDO, TANQUE, SHOOTER
  Bala.java / BalaEnemiga.java
  PoolBalas / PoolEnemigos / PoolBalasEnemigas  ← pools pre-asignados
Systems/
  Upgrade.java               ← definición de una mejora
  UpgradeManager.java        ← aplica y almacena upgrades activos
ui/
  HUD.java                   ← vida, XP, timer, score
utils/
  Constants.java             ← constantes globales de gameplay
  ColisionManager.java       ← toda detección de colisión (facade)
```

---

## Reglas de código

> [!warning] Invariantes a no romper

1. **Nunca** usar `new Entity()` en el game loop — usar los pools (`active` flag para reciclar).
2. **`ColisionManager`** es el único punto de detección de colisiones. Toda nueva colisión va ahí.
3. **`Constants.java`** contiene todos los valores de gameplay tuning — no inline en el código.
4. El patrón `Screen` de libGDX: `show()` inicializa, `render(delta)` es el loop, `dispose()` limpia `Disposable`s.

### Constantes clave

| Constante | Valor |
|---|---|
| `SCREEN_WIDTH` | 1280 |
| `SCREEN_HEIGHT` | 720 |
| `PLAYER_SPEED` | 400f |
| `ARENA_RADIUS` | 8000f |

---

## Rendering

- Cámara sigue al jugador
- `SpriteBatch` dibuja entidades en world-space
- `HUD` usa viewport separado de tamaño fijo
- Todos los gráficos son formas `Pixmap` procedurales — sin atlas ni carga de assets

---

## Notas de sesión

> [!note] Log de decisiones y contexto añadido en sesiones

<!-- Las notas de sesión se añaden debajo de esta línea -->

### 2026-05-05

- Nota creada desde CLAUDE.md como contexto vivo del proyecto.
- El vault ya tiene [[Kaosuarina]] con diseño de juego y mecánicas detalladas.
- Ashley ECS disponible como dependencia pero no integrado — candidato para refactor futuro.

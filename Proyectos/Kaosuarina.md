---
title: Kaosuarina — Proyecto de Juego
tags:
  - proyecto
  - videojuego
  - roguelite
  - rpg
  - java
  - libgdx
aliases:
  - Kaosuarina
  - A_Game_Kaosuarina
cssclasses: []
---

# Kaosuarina

Roguelite top-down shooter con toques RPG al estilo de **Vampire Survivors** y **Brotato**, desarrollado en [[LibGDX-Framework|LibGDX]] (Java) como proyecto TFG.

> [!abstract] Concepto rápido
> El jugador elige entre 3 roles distintos (Caballero, Mago, Tirador), sobrevive oleadas infinitas de enemigos con mecánicas de ataque únicas por rol, acumula experiencia, sube de nivel y elige mejoras acumulativas. Recoge armas con inscripciones y amuletos durante la run. En oleada 50 aparece el **Devastador del Caos** — derrotarlo es la condición de victoria.

## Stack tecnológico

| Componente | Tecnología |
|---|---|
| Framework | [[LibGDX-Framework\|LibGDX]] 1.14.0 |
| Lenguaje | Java 8 |
| Build | Gradle + gdx-liftoff |
| Plataforma objetivo | PC Desktop (LWJGL3) 1280×720 |
| Gráficos | Procedurales (Pixmap) + sprites animados por rol |
| Base de datos | SQLite embebido — patrón DAO + JDBC (`kaosuarina.db` junto al JAR, sin servidor) |
| Audio | AudioManager listo — requiere archivos WAV/OGG en `assets/audio/` |

## Estructura del proyecto

```
A_Game_Kaosuarina/
├── core/src/main/java/com/milwar/kaosuarina/
│   ├── KaosuarinaGame.java              ← Entry point; carga texturas, armas, audio → MainMenuScreen
│   ├── screens/
│   │   ├── MainMenuScreen.java          ← Menú (Jugar · Records · Créditos · Salir)
│   │   ├── CharacterSelectScreen.java   ← Selección de rol antes de partida
│   │   ├── GameScreen.java              ← Loop principal (>900 líneas)
│   │   ├── LevelUpScreen.java           ← Overlay de mejoras (cola pendingLevelUps)
│   │   ├── WinScreen.java               ← Victoria — stats + leaderboard
│   │   ├── GameOverScreen.java          ← Derrota — kills por tipo + top 3 + [L] records
│   │   ├── LeaderboardScreen.java       ← Top 10 desde BD (oro/plata/bronce)
│   │   └── CreditsScreen.java           ← Créditos TFG
│   ├── entities/
│   │   ├── Player.java
│   │   ├── Enemy.java                   ← 17 tipos: BASICO/RAPIDO/TANQUE/SHOOTER/MALDITO/ESPECTRAL/GUARDIAN/ARQUERO/DEVASTADOR
│   │   │                                             BERSERKER/SPLITTER/HEALER/SHIELDER/ELITE_CHARGE/ELITE_SUMMON/ELITE_ZONE/FRAGMENTADO
│   │   ├── Bullet.java / EnemyBullet.java
│   │   └── BulletPool / EnemyPool / EnemyBulletPool
│   ├── roles/
│   │   └── Role.java                    ← Factory: caballero(), mago(), shooter()
│   ├── reliquias/
│   │   ├── Reliquia.java                ← Interfaz
│   │   ├── ReliquiaCaballero.java       ← Fortaleza Reactiva
│   │   ├── ReliquiaMago.java            ← Resonancia Caótica
│   │   └── ReliquiaTirador.java         ← Momentum de Combate
│   ├── weapons/
│   │   ├── Weapon.java                  ← Clase base abstracta (type, category, dmg, CD, bonuses)
│   │   ├── WeaponNormal.java            ← Auto-fire / melee continuo por rol
│   │   ├── WeaponSkill.java             ← Habilidad activa (Q/E) con coste de maná
│   │   ├── WeaponPool.java              ← Pre-alloca 6 instancias en init(); nunca new en loop
│   │   ├── WeaponType.java              ← ESPADA_VERDUGO · MARTILLO_JUICIO · BACULO_ARCANO · TOMO_CAOS · PISTOLAS_GEMELAS · RIFLE_PRECISION
│   │   ├── WeaponCategory.java          ← NORMAL | SKILL
│   │   ├── WeaponDropper.java           ← genera armas tiered T1-T5 + afijos con sesgo de rol 60/25/15%
│   │   ├── Inscription.java             ← Interfaz: onHit(), damageMult(), extraPierce(), bypassesDefense()
│   │   ├── InscriptionPool.java         ← Pool de las 9 inscripciones
│   │   └── inscriptions/
│   │       ├── InscripcionIgnea.java        ← FUEGO on hit → aplica BURN
│   │       ├── InscripcionVampirica.java    ← Roba vida en cada impacto
│   │       ├── InscripcionDelEco.java       ← Proyectil rebota al enemigo más cercano (EchoQueue)
│   │       ├── InscripcionDeVacio.java      ← Daño CAOS (ignora resistencias)
│   │       ├── InscripcionSismica.java      ← Aturde 0.3s (stunTimer en Enemy)
│   │       ├── InscripcionResonante.java    ← Resonancia (amplifica daño acumulado)
│   │       ├── InscripcionEspectral.java    ← Efectiva contra ESPECTRAL
│   │       ├── InscripcionDelCaos.java      ← Daño CAOS alternativo
│   │       └── InscripcionVampiricaMana.java← Roba maná en cada impacto
│   ├── items/
│   │   ├── AmuletType.java              ← Enum de tipos de amuleto
│   │   └── AmuletPool.java              ← Pool de amuletos; spawn cada 6-8 oleadas
│   ├── db/
│   │   ├── DBConnection.java            ← Conexión MySQL (jdbc:mysql://localhost:3306/kaosuarina)
│   │   ├── DBManager.java               ← API pública estática + RunData DTO
│   │   ├── dao/RunDAO.java              ← Interfaz: guardar/kills/upgrades/reliquia/armas
│   │   ├── dao/impl/RunDAOImpl.java     ← Implementación JDBC de RunDAO
│   │   └── vo/RunVO.java                ← VO: id, personajeId, score, tiempo, nivel, mana, completada
│   ├── systems/
│   │   ├── PlayerStats.java             ← Stats RPG + maná + lifeStealPercent hook + manaGastadoTotal
│   │   ├── Upgrade.java
│   │   └── UpgradeManager.java
│   ├── ui/
│   │   └── HUD.java                     ← Vida, maná, XP, timer, score, slots arma, barra boss, core display
│   └── utils/
│       ├── Constants.java               ← Toda constante de tuning (armas, inscripciones, amuletos, bosses, partículas)
│       ├── CollisionManager.java        ← Facade de colisiones + cálculo de daño tipado + overkill maná
│       ├── DamageType.java              ← FISICO | MAGICO | A_DISTANCIA | FUEGO | VENENO | CAOS
│       ├── StatusEffect.java            ← BURN / POISON sobre enemigos
│       ├── Particle.java                ← Partícula individual (pos, vel, vida, color, tamaño)
│       ├── ParticlePool.java            ← Pool estático de 400 partículas; spawn/render/update
│       ├── AudioManager.java            ← SFX + música loop; espera WAV/OGG en assets/audio/
│       ├── AnimationSheets.java
│       ├── SharedTextures.java
│       └── SpriteSheets.java
└── lwjgl3/                              ← Launcher de escritorio
```

## Mecánicas implementadas

### Movimiento del jugador

- Control con **WASD**
- Velocidad base por rol (Caballero 250, Mago 270, Tirador 340 px/s)
- Arena circular de radio `8000f` — el borde empuja suavemente hacia el centro
- Invulnerabilidad de **1 segundo** tras recibir daño (parpadeo visual)

### Sistema de ataque por rol

| Rol | Modo | Ataque ligero (clic izquierdo) | Ataque pesado (clic derecho) |
|-----|------|-------------------------------|------------------------------|
| Caballero | Melee | Arco 120° / radio 140px — FISICO — CD 0.35s | Arco 200° / radio 210px — FISICO — CD 1.0s |
| Mago | Mágico | Bolt auto-aim, 8 maná — MAGICO — CD 0.28s | Explosión AOE radio 180px, 35 maná — MAGICO — CD 1.5s |
| Tirador | Auto-shoot | 2 balas spread 12° — A_DISTANCIA — CD 0.16s | — |

> [!tip] El Tirador dispara automáticamente. Caballero y Mago usan clic manual.
> El bolt del Mago tiene auto-aim: enemigo más cercano en radio 400u; si no hay, apunta al ratón.

### Tipos de daño

```
DamageType: FISICO | MAGICO | A_DISTANCIA | FUEGO | VENENO | CAOS
```

| Tipo | Usado por | Fórmula | Nota |
|------|-----------|---------|------|
| FISICO | Caballero melee, Espada Verdugo | `max(1, raw - defensa)` | — |
| MAGICO | Mago, Báculo Arcano | `max(1, raw × (1 - resMag/100))` | +50% vs TANQUE |
| A_DISTANCIA | Tirador, Pistolas Gemelas | igual que FISICO | diferenciado en S10 |
| FUEGO | InscripcionIgnea | `max(1, raw - defensa×0.5)` + BURN | +50% vs ESPECTRAL |
| VENENO | Explosión MALDITO | `max(1, raw - defensa×0.5)` + POISON | — |
| CAOS | Tomo del Caos, InscripcionDeVacio | `max(1, raw - defensa×0.5 - resMag×0.5)` | Ignora resistencias parcialmente |

### Efectos de estado (StatusEffect)

| Estado | Activación | Tick | Daño/tick | Duración base |
|--------|-----------|------|-----------|--------------|
| BURN | FUEGO / InscripcionIgnea | cada 0.5s | 8 | 3s |
| POISON | VENENO / explosión MALDITO | cada 1.0s | 5 | 5s |

- Mismo tipo dos veces extiende duración (`Math.max`)
- Render: tinte naranja (BURN) o verde (POISON) mediante `lerp 50%`

### Sistema de armas

Dos **slots de arma** equipables durante la run. Los cofres aparecen cada 2-3 oleadas.

| Arma | Categoría | Daño | Rol afín | Activación |
|------|-----------|------|----------|-----------|
| Espada Verdugo | NORMAL | FISICO | Caballero | Auto-melee |
| Martillo del Juicio | SKILL | FISICO | Caballero | Q / E (AoE golpe) |
| Báculo Arcano | NORMAL | MAGICO | Mago | Auto-bolt |
| Tomo del Caos | SKILL | CAOS | Mago | Q / E (explosión CAOS) |
| Pistolas Gemelas | NORMAL | A_DISTANCIA | Tirador | Auto-disparo |
| Rifle de Precisión | SKILL | A_DISTANCIA | Tirador | Q / E (disparo concentrado) |

> [!tip] Afinidad de rol
> Equipar un arma de tu rol: **+15% daño** (`WEAPON_AFFINITY_DMG_MULT`) y **-10% cooldown** (`WEAPON_AFFINITY_CD_MULT`). El cooldown nunca baja de `MIN_WEAPON_CD = 0.06s`.

**Swap menu**: si ambos slots están llenos al recoger un cofre, aparece un menú de 5s — `[1]` slot 1, `[2]` slot 2, `[X]` descartar.

### Inscripciones

Aplicadas a un arma via **scroll** (spawn cada 3-4 oleadas). El arma con inscripción muestra el nombre en el HUD.

| Inscripción | Efecto |
|-------------|--------|
| Ígneo | FUEGO on hit → aplica BURN al enemigo |
| Vampírica | Roba vida proporcionalmente al daño infligido |
| Del Eco | El proyectil rebota hacia el enemigo más cercano (EchoQueue) |
| De Vacío | Convierte el daño a CAOS (ignora resistencias) |
| Sísmica | Aturde al enemigo 0.3s (`stunTimer`) |
| Resonante | Amplifica el daño acumulado en el objetivo |
| Espectral | Daño extra contra ESPECTRAL |
| Del Caos | Daño de tipo CAOS alternativo |
| Vampírica de Maná | Roba maná en cada impacto |

**Interfaz `Inscription`**: `onHit()`, `damageMult()`, `extraPierce()`, `bypassesDefense()`, `getName()`

### Amuletos

Equipables durante la run; spawn cada 6-8 oleadas. Pool pre-allocado (`AmuletPool`). Los valores específicos de cada tipo están en `AmuletType.java`.

> [!note] El sistema de amuletos está implementado a nivel de infraestructura. Los efectos concretos de cada tipo están en `AmuletType.java` y se aplican via `GameScreen`.

### Tipos de enemigos

| Tipo | Vel | HP | Def | ResMág | Comportamiento especial |
|------|-----|----|-----|--------|------------------------|
| `BASICO` | 150 | 40 | 0 | 0 | Persigue |
| `RAPIDO` | 300 | 25 | 0 | 0 | Persigue, kamikaze frágil |
| `TANQUE` | 80 | 110 | 20 | 0 | Persigue; resistente a FISICO (40%); débil a MAGICO |
| `SHOOTER` | 100 | 35 | 5 | 5 | Orbita 300-500u; huye si cerca; dispara cada 2s |
| `MALDITO` | 130 | 45 | 0 | 0 | **Explota al morir** (r=120, 25 dmg VENENO + POISON); veneno de contacto jugador (4s, 5 dps) |
| `ESPECTRAL` | 160 | 35 | 0 | 10 | **Inmune a FISICO**; +50% FUEGO; alpha 0.45 |
| `BERSERKER` | 120 | 60 | 0 | 0 | Carga directa; al <20% HP: velocidad ×3 (modo rabia) |
| `SPLITTER` | 100 | 50 | 0 | 0 | Normal; **al morir: spawn de 2 copias al 40% HP** |
| `HEALER` | 80 | 35 | 0 | 0 | Huye del player; **cura 10 HP/s a aliados en radio 150u** |
| `SHIELDER` | 60 | 80 | 0 | 0 | Lento; **70% defensa base** |
| `ELITE_CHARGE` | 140 | 120 | 10 | 0 | Telegrafía 1.5s → **carga recta devastadora** |
| `ELITE_SUMMON` | 80 | 90 | 0 | 5 | Huye; **invoca 3 BASICO cada 8s** |
| `ELITE_ZONE` | 50 | 100 | 0 | 0 | Estacionario; **crea zona lenta radio 120u** (45% slowdown) |
| `GUARDIAN` | 60 | 900 | 0 | 0 | **Minijefe (ola 10)** — shockwave radial cada 3s (r=150, 20 dmg); fase 2 al 50% HP: vel ×1.2 + 2 BASICO |
| `ARQUERO` | 90 | 400 | 0 | 0 | **Minijefe (ola 20)** — teleporta cada 4s; dispara proyectil 18 dmg cada 2s; contacto 15 dmg |
| `FRAGMENTADO` | 80 | 1600 | 0 | 0 | **Boss (ola 30)** — 3 fases: Física → Mágica → Caos |
| `DEVASTADOR` | 60/100 | 2200 | 0 | 0 | **Boss final (ola 50)** — ver sección dedicada |

> [!tip] Probabilidades de spawn (pool normal — 6 tipos base)
> BASICO 40% · RAPIDO 20% · TANQUE 10% · SHOOTER 10% · MALDITO 12% · ESPECTRAL 8%
>
> BERSERKER, SPLITTER, HEALER, SHIELDER entran en el pool a partir de oleadas medias.
> ELITE_CHARGE, ELITE_SUMMON, ELITE_ZONE spawnean según `eliteChancePct` por dificultad.
> GUARDIAN, ARQUERO, FRAGMENTADO y DEVASTADOR se spawnean por eventos de oleada.

> [!warning] Anti-recursión MALDITO
> Explosión al morir procesada en el siguiente frame para evitar recursión en grupos.

### Minijefes

**Guardián** — aparece en oleadas múltiplo de `MINIBOSS_WAVE_INTERVAL = 10`:
- Fase 1: shockwave cada 3s (radio 150, 20 dmg). Barra HP roja en HUD.
- Fase 2 (≤50% HP): velocidad ×1.2, invoca 2 BASICO. Visual más agresivo.
- Al morir: spawnea un scroll de inscripción.

**Arquero** — aparece en oleada `ARQUERO_MINIBOSS_WAVE = 20` (y múltiplos):
- Teletransporta cada 4s fuera del rango del jugador.
- Dispara proyectil de 18 dmg cada 2s. Peligroso por movilidad, no por HP.

### Boss Final — Devastador del Caos

Aparece en **oleada 50** (y oleadas 100, 150…). Derrotarlo activa la condición de victoria.

| Stat | Fase 1 | Fase 2 (≤50% HP) |
|------|--------|-----------------|
| HP total | 2200 | — |
| Velocidad | 60 u/s | 100 u/s |
| Daño contacto | 25 | 25 |
| Daño proyectil | 22 | 22 |
| Shockwave (r=350) | cada 3s | cada 1.5s |
| Espiral 8 balas | cada 5s | cada 3s |

**Transición de fase** (única, al cruzar 50% HP):
- Velocidad aumenta, timers acelerados
- Invoca **3 ESPECTRAL**
- Tinte rojizo en el sprite

**Al morir**: navegación a `WinScreen` con stats de la run y top 5 scores.

### Veneno de contacto (jugador)

El jugador puede ser envenenado por el MALDITO al tocarlo:
- Tick cada 1s, **5 dps**, duración 4s
- **No respeta invulnerabilidad post-impacto** — es DoT, no impacto
- Segundo contacto estando envenenado: extiende al máximo
- Visual: tinte verde en el sprite del jugador

### Sistema de dificultad

Intervalo de spawn empieza en `SPAWN_INTERVAL_BASE` y se multiplica por `DIFICULTAD_RAMP_FACTOR` cada `DIFICULTAD_RAMP_INTERVAL` segundos, hasta un mínimo de `SPAWN_INTERVAL_MIN`.
La cantidad de enemigos por oleada: `1 + (nivel / 3)`.

| Constante | Valor producción | Valor modo test |
|---|---|---|
| `SPAWN_INTERVAL_BASE` | 90s | 25s |
| `SPAWN_INTERVAL_MIN` | 12s | 4s |
| `DIFICULTAD_RAMP_INTERVAL` | 150s | 50s |
| `DIFICULTAD_RAMP_FACTOR` | 0.92 | 0.88 |
| **Oleada 50** | **~34 min** | **~10 min** |

> [!tip] El código actual usa los valores de modo test para facilitar pruebas. Para producción cambiar las 4 constantes en `Constants.java`.

### Sistema de upgrades (roguelite)

3 mejoras aleatorias al subir de nivel. N niveles a la vez → N pantallas consecutivas (cola `pendingLevelUps`).

| Upgrade | Efecto | Máx. nivel |
|---------|--------|-----------|
| Daño +20% | Multiplicador acumulativo | 5 |
| Cadencia +15% | Reduce cooldown de disparo | 5 |
| Velocidad +10% | Aumenta velocidad de movimiento | 5 |
| Vida Máxima +20 | Aumenta HP tope | 3 |
| Perforación | Balas atraviesan enemigos adicionales | 3 |
| Bala Extra | +1 bala en abanico (Tirador) | 3 |

### Progresión de experiencia

```
expToNextLevel inicial = 100
Cada level: expToNextLevel × 1.2
XP por muerte = 25
Maná por kill = 5
Maná por impacto de bala = 1
Maná por overkill = daño_excedente ÷ 20 (OVERKILL_DIVISOR)
```

### Sistema de partículas

Pool estático de **400 partículas** (`ParticlePool`). Spawn en:
- Muerte de enemigo: 12 partículas, vel=120, vida=0.6s
- Impacto de bala: 4 partículas, vel=60, vida=0.25s
- Explosión (MALDITO, skills AoE): 20 partículas, vel mayor

Color de partículas determinado por `Enemy.Tipo`: DEVASTADOR → negro/magenta.

### Audio

`AudioManager` gestiona SFX y música de fondo. **Los archivos aún no están presentes** (pendiente S9-02):

| Archivo | Evento |
|---------|--------|
| `assets/audio/sfx/shoot.wav` | Disparo de bala |
| `assets/audio/sfx/impact.wav` | Bala impacta enemigo |
| `assets/audio/sfx/death_enemy.wav` | Enemigo muere |
| `assets/audio/sfx/levelup.wav` | Sube de nivel |
| `assets/audio/sfx/pickup.wav` | Cofre / scroll / amuleto recogido |
| `assets/audio/music/background.ogg` | Música de fondo (loop) |

> [!warning] El juego arranca aunque falten los archivos de audio — `AudioManager` los omite con log de error.

### HUD

- Barra de **vida** (arriba izquierda) — cambia de color según HP%
- Barra de **maná** (debajo de vida) — visible si `maxMana > 0`
- **Slots de arma** (centro inferior) — abbrev + Q/E + cooldown visual + inscripción
- **Core display** (arriba derecha) — ARM X (Caballero stacks) / CMB X (Tirador combo)
- Barra de **experiencia** (centro inferior)
- **Timer** de supervivencia (centro superior)
- **Score** (arriba derecha)
- **Barra HP boss** (centro superior) — roja con etiqueta; visible cuando hay minijefe o Devastador activo

### Pantallas del juego

| Pantalla | Descripción | Navegación |
|----------|-------------|-----------|
| `MainMenuScreen` | Menú principal | ENTER→Jugar · L→Records · C→Créditos · ESC→Salir |
| `CharacterSelectScreen` | Elegir rol | Flechas A/D · ENTER confirma |
| `GameScreen` | Loop principal | — |
| `LevelUpScreen` | Overlay mejoras | 1/2/3 o ENTER |
| `WinScreen` | Victoria (Devastador muerto) | R→CharacterSelect · ESC→MainMenu |
| `GameOverScreen` | Derrota | R→CharacterSelect · L→Records · ESC→MainMenu |
| `LeaderboardScreen` | Top 10 runs desde BD | ESC/ENTER→MainMenu |
| `CreditsScreen` | Créditos TFG | ESC/ENTER/SPACE→MainMenu |

### Base de datos (SQLite)

Patrón **DAO** con JDBC. Guardado transaccional al terminar cada run. BD embebida `kaosuarina.db` junto al JAR — sin servidor externo.

**Tablas**: `run` · `run_kill` (kills por tipo de enemigo) · `run_upgrade` · `run_reliquia` · `run_arma` (arma + inscripción por slot) · `meta_tokens` (tokens de meta-progresión persistentes entre runs)

> [!success] BD migrada de MySQL a **SQLite embebido** en Sprint 9 (`org.xerial:sqlite-jdbc:3.45.3.0`). El JAR es autocontenido — no requiere instalación de servidor.

## Roles jugables

> [!note]
> Solo 3 roles implementados. Los conceptos de diseño descartados están en [[Roles-Descartados]].

| Rol | HP | Defensa | Res.Mág | Vel | Maná | Estilo |
|-----|----|---------|---------|----|------|--------|
| **Caballero** | 200 | 20 | 0 | 250 px/s | 40 | Melee reactivo, alta supervivencia |
| **Mago** | 85 | 5 | 15 | 270 px/s | 120 + regen 2/s | Caster de área, gestión de maná |
| **Tirador** | 80 | 5 | 0 | 340 px/s | 30 | Auto-shoot veloz, combo de kills |

### Reliquias

| Rol | Reliquia | Mecánica |
|-----|----------|---------|
| Caballero | **Fortaleza Reactiva** | Al recibir daño: +1 stack armadura (max 5, -7%/stack). Sin daño 2s → pierde 1/s |
| Mago | **Resonancia Caótica** | Proyectiles rebotan 1 vez hacia enemigo más cercano (r=200u) |
| Tirador | **Momentum de Combate** | Kills acumulan Combo (max 10, -3% CD/stack). Sin kill 2s → -1/s |

## Narrativa

> [!abstract] Premisa — Vacío Kaótico
> El jugador controla a un **nómada errante** atrapado en arenas infinitas dentro del **Vacío Kaótico**. Estas arenas representan un espacio caótico donde todo se transforma continuamente.
>
> **Objetivo**: sobrevivir oleadas, potenciar el rol con upgrades + armas + inscripciones y derrotar al **Devastador del Caos** en oleada 50.
>
> **Motivación**: libertad absoluta y experimentación — cada partida crea una historia distinta.

## Arquitectura de código

### Patrones usados

| Patrón | Dónde |
|--------|-------|
| Object Pool | `BulletPool`, `EnemyPool`, `EnemyBulletPool`, `WeaponPool`, `InscriptionPool`, `AmuletPool`, `ParticlePool` |
| State flag | campo `active` en `Bullet`, `EnemyBullet`, `Enemy`; campo `active` en `Particle` |
| Factory Method | `Role.caballero()`, `Role.mago()`, `Role.shooter()` |
| Strategy | `Enemy.Tipo` (switch por tipo); `Reliquia` (interfaz por rol); `Inscription` (interfaz por inscripción) |
| Facade | `CollisionManager` — toda detección y cálculo de daño |
| DAO / VO | `db/` — `RunDAO` interfaz, `RunDAOImpl` implementación JDBC, `RunVO` value object |

### Reglas de arquitectura

- `new` entity **nunca** dentro del game loop — siempre usar pools
- Valores de tuning **solo** en `Constants.java` o `UpgradeManager`, nunca inline
- Toda colisión y cálculo de daño pasa **solo** por `CollisionManager`
- Un rol → un factory method → un `PlayerStats` configurado → una `Reliquia`
- Toda inscripción implementa `Inscription`; su lógica vive en su propia clase

### Colisión

Todas las colisiones son **círculo vs círculo** usando `Vector2.dst()`.

```
Bala (r=4)    vs Enemy (r=size/2)
Player (r=32) vs Enemy (r=size/2)
Player (r=32) vs BalaEnemiga (r=5)
Pickup (cofre/scroll/amuleto): radio configurable en Constants
```

## Controles

| Tecla / Input | Acción |
|---------------|--------|
| WASD | Mover jugador |
| Ratón | Apuntar |
| Clic izquierdo | Ataque ligero (Caballero/Mago) |
| Clic derecho | Ataque pesado (Caballero/Mago) |
| (automático) | Disparar (solo Tirador) |
| Q | Activar skill del arma en slot 1 |
| E | Activar skill del arma en slot 2 |
| 1 / 2 | Equipar arma en slot correspondiente (swap menu) |
| X | Descartar pickup de arma |
| A/D o ←/→ | Navegar opciones de level up |
| 1 / 2 / 3 | Seleccionar upgrade directamente |
| ENTER / SPACE | Confirmar upgrade |
| R | Jugar de nuevo (GameOverScreen / WinScreen) |
| L | Ver Records (MainMenu / GameOverScreen → LeaderboardScreen) |
| C | Créditos (desde MainMenu) |
| ESC | Menú principal (desde WinScreen / GameOverScreen / LeaderboardScreen / CreditsScreen) |

## Registro de cambios

> [!success] v0.8 — Sprint 8 (2026-06-14)
> - **WinScreen**: pantalla de victoria dedicada al derrotar al Devastador — rol, reliquia, score, oleada, tiempo, armas, top 5
> - **GameOverScreen**: kills por tipo, stats completos, top 3 inline, `[L]` → LeaderboardScreen
> - **LeaderboardScreen**: top 10 desde BD con medallas oro/plata/bronce
> - **MainMenuScreen**: `[ENTER]` Jugar · `[L]` Records · `[C]` Créditos · `[ESC]` Salir
> - **CreditsScreen**: corregida navegación de vuelta a MainMenuScreen (era CharacterSelectScreen)
> - **BD completa**: `guardarRun()` persiste kills por tipo, upgrades, reliquia, armas + inscripciones en transacción única
> - **run_arma** poblada: slot, arma_tipo, inscripcion guardados tras cada run

> [!success] v0.7 — Sprint 7
> - **Partículas**: `ParticlePool` 400 unidades — muerte, impacto, explosión con color por tipo de enemigo
> - **Minijefe Arquero** (GUARDIAN): HP 500, teletransporte cada 4s, proyectil 18 dmg cada 2s
> - **AudioManager**: infraestructura completa lista; requiere archivos WAV/OGG en `assets/audio/` (S9-02)

> [!success] v0.6 — Sprint 6
> - **Sistema de inscripciones**: 9 tipos via scroll; interfaz `Inscription` con `onHit()` / `damageMult()` / `extraPierce()` / `bypassesDefense()`
> - **Amuletos**: `AmuletType` / `AmuletPool`, spawn configurable cada 6-8 oleadas
> - **Minijefe Guardián**: HP ~~800~~ **900** (actualizado Sprint 10), shockwave radial cada 3s, fase 2 al 50% HP invoca 2 BASICO; drop de scroll al morir
> - **Scrolls de inscripción**: spawn y UI de selección de slot (timer 5s)
> - **Overkill maná**: el exceso de daño letal se convierte en maná (`daño_excedente ÷ 20`)
> - **HUD barra boss**: muestra HP del minijefe activo con etiqueta

> [!success] v0.5 — Sprint 5
> - **Sistema de armas**: 6 armas (2 slots), `WeaponCategory` NORMAL/SKILL, `WeaponPool` pre-allocado
> - **Afinidad por rol**: +15% daño / -10% CD cuando el arma es del rol del jugador
> - **Cofres**: spawn cada 2-3 oleadas, swap menu con timeout 5s
> - **WeaponSkill** activables con Q/E, con coste de maná y cooldown visual en HUD
> - **Maná extendido**: +5 por kill, +1 por impacto, overkill ÷20 hook

> [!success] v0.11 — Sprint 11 (2026-06-01)
> - **WeaponDropper**: `generate(depth, roleHint)` — 12 armas tiered T1-T5 con sesgo de rol 60/25/15%; inscripción 30% probabilidad
> - **Sistema de afijos completo**: 11 afijos rollados — Afilado (`dmg_flat`), Cruel (`dmg_pct`), Veloz (`atk_speed_pct`), Resoluto (`reduced_cd_pct`), Letal (`crit_chance`), Brutal (`crit_dmg`), Vampírico (`lifesteal_pct`), de Maná (`mp_regen`), de Fuego, de Veneno, de Caos — cada uno con efecto en `ColisionManager` / `Player`
> - **Fix lifesteal**: `PlayerStats.lifeStealPercent` (antes nunca leído) ahora sumado en `ColisionManager.aplicarEfectosOnHit()`
> - **HUD afijos**: `WeaponCard.affixLabel` — nombre del afijo visible en slot de arma
> - **Balance "Kausarina Verzente"**: `SPAWN_BASE_COUNT` 12→7, `SPAWN_PER_LEVEL` 3→5, `DIFICULTAD_RAMP_FACTOR` 0.84→0.86, `SPAWN_INTERVAL_BASE` 18→15, `INVULNERABILITY_MAGO` 0.30→0.42s, `INVULNERABILITY_SHOOTER` 0.20→0.25s, `STATUS_BURN_DAMAGE` 8→5, `ARQUERO_HP` 300→400
> - **Balance roles**: Mago HP 70→85; `ReliquiaCaballero` DECAY_DELAY 4→2s, reducción/stack 0.08→0.07

> [!success] v0.10 — Sprint 10 (2026-05-20)
> - **Depth scaling**: multiplicadores de HP/daño desde `depthscaling.json` — escala continua más allá de oleada 50
> - **Resistencias JSON**: resistencias por tipo de daño por tipo de enemigo desde `enemies.json` (ej. TANQUE 40% resist FISICO)
> - **CAOS_PRIMORDIAL**: daño verdadero (ignora defensa y resistencias) + ralentización 2s al 50% velocidad
> - **Inventario 6 slots**: armas (2) + amuletos (4) con interfaz unificada
> - **Upgrades por rol**: cada rol tiene su pool propio de mejoras diferenciadas
> - **7 amuletos**: todos con efectos activos implementados (`AmuletType`)
> - **Animaciones idle 4 dirs**: Caballero, Mago y Tirador con sprites animados (PixelLab, `AnimationSheets`)
> - **Fix Tirador**: auto-disparo corregido en todos los escenarios de edge-case

> [!success] v0.9 — Sprint 9 (2026-05-19)
> - **SQLite**: BD migrada de MySQL a SQLite embebido (`kaosuarina.db` junto al JAR, sin servidor)
> - **Refactor**: `GameScreen.java` reducido a 737 líneas; `BossManager` y `SpawnManager` en `screens/managers/`
> - **Balance**: BASICO hp 30→40, RAPIDO daño contacto 6→15; 4 constantes de dificultad en `Constants.java`
> - **DataManager**: 18 POJOs + `DataManager` singleton (Gson 2.10.1) carga 17 catálogos JSON al arrancar; `WeaponInstanceFactory` rueda instancias con afijos y multiplicadores de tier
> - **Modo test**: `SPAWN_INTERVAL_BASE=25s` → oleada 50 en ~10 min (producción: 90s/34 min)
> - **Pendiente**: animaciones idle (S9-01) y audio (S9-02) aplazados a Sprint 10

> [!success] v0.4 — Sprint 4
> - **Base de datos**: patrón DAO completo (RunDAO, RunDAOImpl, RunVO); tablas run/kill/upgrade/reliquia/arma
> - `DBManager.guardarRun()` envuelto en transacción; fallo silencioso (juego continúa sin BD)
> - `DBManager.getTop10()` para leaderboard

> [!success] v0.3 — Sprint 3 (2026-05-12)
> - **Nuevos enemigos**: MALDITO (explosión + veneno de contacto) y ESPECTRAL (inmune a FISICO, +50% FUEGO, alpha 0.45)
> - **StatusEffect**: BURN/POISON sobre enemigos con tick, prioridad y tinte visual
> - **Veneno del jugador**: DoT independiente de invulnerabilidad
> - **`lifeStealPercent` hook**: campo `0f` en `PlayerStats` listo
> - Pesos de spawn actualizados a 6 tipos

> [!success] v0.2 — Sprint 2 (2026-05-12)
> - **Stats RPG**: `defensa`, `resistenciaMagica`, `rango`, `maná/maxMana/manaRegen` en `PlayerStats`
> - **Roles reescalados**: Caballero 200HP/def20, Mago 70HP/resMag15/maná120, Tirador 80HP/vel340
> - **DamageType** integrado en pipeline de `ColisionManager`
> - **Reliquias**: refactor `cores/` → `reliquias/` (Fortaleza Reactiva, Resonancia Caótica, Momentum de Combate)
> - **Cola de level-ups**: `pendingLevelUps`
> - Enemigos reescalados: BASICO 30HP, RAPIDO 12HP, TANQUE 180HP/def20, SHOOTER 50HP

> [!success] v0.1 — Sprint 1 (2026-05-07)
> - Loop básico: WASD, auto-disparo abanico, 4 tipos de enemigos, arena circular
> - Sistema de upgrades roguelite, HUD completo, escalado de dificultad

## Backlog

> [!success] Sprint 9 — CERRADO (2026-05-19)
> - [x] **S9-03** Balance auditado vs `game_data_json`; BASICO hp 30→40, RAPIDO dmg 6→15
> - [x] **S9-04** Curva de dificultad; modo test ~10 min / producción ~34 min
> - [x] **S9-05a** BD migrada MySQL → SQLite (`org.xerial:sqlite-jdbc:3.45.3.0`); `initDB()` inline; sin servidor
> - [x] **S9-05b** `GameScreen.java` 916→737 líneas; `BossManager.java` + `SpawnManager.java` en `screens/managers/`
> - [x] **S9-XX** 18 POJOs + `DataManager` (Gson) + `WeaponInstanceFactory` — paquete `data/`
> - [ ] **S9-01** 🎨 Animaciones idle en 4 dirs → **Sprint 10**
> - [ ] **S9-02** 🎨 5 SFX WAV + 1 OGG música → **Sprint 10**

> [!success] Sprint 10 — CERRADO (2026-05-20)
> - [x] **S10-INF** Depth scaling desde `depthscaling.json` — HP/daño escalan con profundidad
> - [x] **S10-INF** Resistencias por tipo de daño desde `enemies.json`
> - [x] **S10-INF** CAOS_PRIMORDIAL true damage + ralentización
> - [x] **S10-D1** 🎨 Animaciones idle 4 dirs — Caballero, Mago, Tirador (PixelLab MCP)
> - [x] **S10-INV** Inventario 6 slots + upgrades por rol + 7 amuletos
> - [x] **S10-FIX** Fix Tirador auto-disparo
> - [ ] **S10-D2** 🎨 Archivos de audio (5 SFX WAV + 1 OGG música) — **pendiente Sprint 12**

> [!success] Sprint 11 — CERRADO (2026-06-01)
> - [x] **S11-WD** `WeaponDropper` — 12 armas tiered+afijos con sesgo de rol 60/25/15%
> - [x] **S11-AFX** 11 afijos completos con efectos en `ColisionManager` / `Player` / `Bala`
> - [x] **S11-HUD** `WeaponCard.affixLabel` — afijo visible en slot HUD
> - [x] **S11-FIX** Fix `lifeStealPercent` nunca leído → ahora activo en `aplicarEfectosOnHit()`
> - [x] **S11-BAL** Balance "Kausarina Verzente" — spawn, iframes, BURN, TANQUE/SHOOTER hp, Caballero reliquia

> [!todo] Backlog largo plazo
> - [ ] IA adaptativa con FSM (comportamiento reactivo a la build del jugador)
> - [ ] Arenas procedurales con Perlin Noise
> - [ ] Persistencia de configuración y progreso meta
> - [ ] Boss secreto: Devorador Abisal
> - [ ] Optimización final y testing exhaustivo (JUnit 5)

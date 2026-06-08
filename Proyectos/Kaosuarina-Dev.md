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
date: 2026-06-03
---

# Kaosuarina — Contexto de Desarrollo

Nota viva de contexto técnico para el desarrollo del TFG. Complementa [[Kaosuarina]] (diseño de juego). Se actualiza con decisiones, reglas y notas de sesión.

---

## Workspace

| Directorio | Propósito |
|---|---|
| `A_Game_Kaosuarina/` | Juego — LibGDX/Java |
| `Code_Game/` | Framework multi-agente IA (49 agentes) — ya no se usa activamente |
| `Kausarina_Vault/` | Este vault — diseño + referencia técnica |
| `obsidian-skills/` | Skills de edición Obsidian |

El trabajo de código ocurre en `A_Game_Kaosuarina/`. El flujo actual es diseño en Vault → código directo.

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

| Componente | Versión / Detalle |
|---|---|
| Lenguaje | Java 8 |
| Framework | LibGDX 1.14.0 |
| Plataforma | LWJGL3 — 1280×720 |
| Física | Box2D 1.14.0 |
| ECS | Ashley 1.7.4 (disponible como dependencia, **no usado**) |
| Build | Gradle + gdx-liftoff |
| Gráficos | Procedurales vía `Pixmap` + sprites animados (4 dirs, 3 roles) |
| Base de datos | SQLite embebido — `org.xerial:sqlite-jdbc:3.45.3.0` — `kaosuarina.db` junto al JAR |
| JSON | Gson 2.10.1 — 17 catálogos bajo `assets/game_data_json/` |
| Audio | AudioManager listo — requiere WAV/OGG en `assets/audio/` (pendiente) |

---

## Arquitectura

### Entry point

```
Lwjgl3Launcher
  → Lwjgl3Application(new KaosuarinaGame())
    → setScreen(new MainMenuScreen())
      → CharacterSelectScreen → GameScreen
```

### Estructura de paquetes

Fuentes bajo `core/src/main/java/com/milwar/kaosuarina/`:

```
KaosuarinaGame.java              ← extiende Game; carga texturas, armas, audio → MainMenuScreen
screens/
  MainMenuScreen.java            ← ENTER·L·C·ESC
  CharacterSelectScreen.java     ← selección de rol (A/D + ENTER)
  GameScreen.java                ← loop principal (~737 líneas)
  LevelUpScreen.java             ← overlay mejoras (cola pendingLevelUps)
  WinScreen.java                 ← victoria — stats + leaderboard
  GameOverScreen.java            ← derrota — kills por tipo + top 3 + [L]
  LeaderboardScreen.java         ← top 10 desde BD con medallas
  CreditsScreen.java             ← créditos TFG
  managers/
    BossManager.java             ← lógica de minijefes y Devastador
    SpawnManager.java            ← timing de oleadas y escalado de dificultad
entities/
  Player.java
  Enemy.java                     ← 17 tipos: BASICO/RAPIDO/TANQUE/SHOOTER/MALDITO/ESPECTRAL/BERSERKER/SPLITTER/HEALER/SHIELDER/ELITE_CHARGE/ELITE_SUMMON/ELITE_ZONE/FRAGMENTADO + GUARDIAN/ARQUERO/DEVASTADOR
  Bullet.java / EnemyBullet.java ← renombrados de Bala/BalaEnemiga (2026-06-06)
  BulletPool / EnemyPool / EnemyBulletPool ← renombrados de PoolBalas/PoolEnemigos/PoolBalasEnemigas
roles/
  Role.java                      ← factory: caballero() / mago() / shooter()
reliquias/
  Reliquia.java                  ← interfaz: onDamageReceived / onUpdate / onKill / getDamageReduction
  ReliquiaCaballero.java         ← Fortaleza Reactiva (stacks armadura, decay 2s)
  ReliquiaMago.java              ← Resonancia Caótica (rebote proyectil)
  ReliquiaTirador.java           ← Momentum de Combate (combo kills → -CD)
weapons/
  Weapon.java / WeaponNormal.java / WeaponSkill.java
  WeaponPool.java / WeaponDropper.java   ← WeaponDropper genera armas tiered+afijos con sesgo de rol
  WeaponType.java / WeaponCategory.java
  Inscription.java / InscriptionPool.java
  inscriptions/                  ← 9 clases (Ignea, Vampirica, DelEco, DeVacio, Sismica, Resonante, Espectral, DelCaos, VampiricaMana)
items/
  AmuletType.java / AmuletPool.java
data/
  DataManager.java               ← singleton; carga 17 catálogos JSON con Gson al arrancar
  WeaponInstanceFactory.java     ← genera WeaponInstance (tier T1-T5 + afijos rollados)
  WeaponInstance.java / RolledAffix.java
  model/                         ← 18 POJOs (EnemyData, WeaponData, WeaponAffixData, TierData, etc.)
db/
  DBManager.java                 ← API pública estática + RunData DTO
  DBConnection.java              ← SQLite JDBC
  dao/ RunDAO.java + impl/RunDAOImpl.java
  vo/ RunVO.java
systems/
  PlayerStats.java               ← stats RPG + maná + weaponAffixLifesteal + weaponAffixMpRegen
  Upgrade.java / UpgradeManager.java
ui/
  HUD.java                       ← vida, maná, XP, timer, score, slots arma+afijo, barra boss, core display
  FontManager.java
utils/
  Constants.java                 ← toda constante de tuning (armas, inscripciones, spawn, bosses, partículas)
  CollisionManager.java          ← facade estático; cálculo de daño tipado + lifesteal + on-hit; usa SpatialGrid
  SpatialGrid.java               ← broad-phase O(n); cell=200u; rebuildGrid() cada frame
  DamageType.java                ← FISICO | MAGICO | A_DISTANCIA | FUEGO | VENENO | CAOS
  StatusEffect.java              ← BURN / POISON sobre enemigos
  ParticlePool.java / Particle.java   ← pool estático 400 partículas
  AudioManager.java              ← SFX + música loop; espera WAV/OGG en assets/audio/
  AnimationSheets.java / SharedTextures.java / SpriteSheets.java
```

---

## Reglas de código

> [!warning] Invariantes a no romper

1. **Nunca** usar `new Entity()` en el game loop — usar los pools (`active` flag para reciclar).
2. **`CollisionManager`** es el único punto de detección de colisiones y cálculo de daño. Toda nueva colisión va ahí. Llamar `rebuildGrid(enemyPool)` al inicio del frame antes de cualquier check.
3. **`Constants.java`** contiene todos los valores de tuning — no inline en el código.
4. Un rol → un factory method en `Role.java` → un `PlayerStats` configurado → una `Reliquia`.
5. Toda inscripción implementa la interfaz `Inscription`; su lógica vive en su propia clase bajo `inscriptions/`.
6. El patrón Screen de libGDX: `show()` inicializa, `render(delta)` es el loop, `dispose()` limpia Disposables.

---

## Constantes clave (valores actuales post-balance "Kausarina Verzente")

| Constante | Valor | Nota |
|---|---|---|
| `SCREEN_WIDTH / HEIGHT` | 1280 / 720 | — |
| `ARENA_RADIUS` | 8000f | — |
| `SPAWN_INTERVAL_BASE` | 15f | modo test: ~10 min a oleada 50 |
| `SPAWN_INTERVAL_MIN` | 4.0f | — |
| `SPAWN_BASE_COUNT` | 7 | wave 1 controlada |
| `SPAWN_PER_LEVEL` | 5 | rampa pronunciada |
| `DIFICULTAD_RAMP_FACTOR` | 0.86f | gradual en mid-late |
| `INVULNERABILITY_MAGO` | 0.42f | buffer early-game |
| `INVULNERABILITY_SHOOTER` | 0.25f | — |
| `INVULNERABILITY_CABALLERO` | 0.30f | — |
| `STATUS_BURN_DAMAGE` | 5 | Mago sobrevive 1 proyectil CAOS + BURN completo |
| `DEVASTADOR_HP` | 2200 | boss final oleada 50 |

---

## Rendering

- Cámara sigue al jugador; `SpriteBatch` dibuja en world-space.
- `HUD` usa viewport separado de tamaño fijo (1280×720).
- Gráficos: formas `Pixmap` procedurales + sprites animados 4 dirs (AnimationSheets) para los 3 roles.
- Sin atlas de texturas ni carga de assets externos (excepto animaciones generadas con PixelLab).

---

## Notas técnicas de sesiones

### 2026-05-19 — Sprint 9

- BD migrada MySQL → SQLite embebido (`kaosuarina.db`); sin servidor, JAR autocontenido.
- `GameScreen.java` reducido 916 → 737 líneas; `BossManager` y `SpawnManager` extraídos a `screens/managers/`.
- `DataManager` singleton con Gson carga 17 catálogos JSON al arrancar. `WeaponInstanceFactory` crea instancias con afijos y multiplicadores de tier.

### 2026-05-20 — Sprint 10

- Depth scaling desde `depthscaling.json`; resistencias por tipo de enemigo desde JSON.
- `CAOS_PRIMORDIAL` implementado como true damage + ralentización (`CAOS_PRIMORDIAL_SLOW_DURATION=2s`).
- Inventario expandido a 6 slots; upgrades diferenciados por rol.
- Animaciones idle 4 dirs para los 3 roles integradas con `AnimationSheets`.

### 2026-06-01 — Sprint 11 (balance + WeaponGenerator)

- `WeaponDropper.generate(depth, roleHint)` genera armas tiered T1-T5 con sesgo de rol 60/25/15%.
- 11 afijos implementados (Afilado, Cruel, Veloz, Resoluto, Letal, Brutal, Vampírico, de Maná, de Fuego, de Veneno, de Caos).
- Fix: `PlayerStats.lifeStealPercent` ahora leído en `CollisionManager.applyOnHitEffects()` (renombrado 2026-06-06).
- Balance "Kausarina Verzente": spawn ramp, iframes, BURN damage, TANQUE/SHOOTER hp, ReliquiaCaballero decay 2s.

### 2026-06-06 — Refactor inglés + SpatialGrid

- Renombradas 6 clases: `Bala`→`Bullet`, `BalaEnemiga`→`EnemyBullet`, `PoolBalas`→`BulletPool`, `PoolBalasEnemigas`→`EnemyBulletPool`, `PoolEnemigos`→`EnemyPool`, `ColisionManager`→`CollisionManager`.
- Renombrados campos `PlayerStats`: `defensa`→`physicalDefense`, `resistenciaMagica`→`magicResistance`, `manaGastadoTotal`→`totalManaSpent`.
- Nueva clase `SpatialGrid.java`: broad-phase hash grid cell=200u; elimina O(n×m) en bullets vs enemies.
- 5 bugs corregidos: double lifesteal, ELITE_ZONE permanent slow, FRAGMENTADO sin contacto, anyOverlay incompleto, resource leak.
- Build: EXIT 0. Ver [[../../Sesiones/Sesion-2026-06-06-Refactor-Ingles]].

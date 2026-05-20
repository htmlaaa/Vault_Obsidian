---
tags: [sesion, sprint-7, codigo]
fecha: 2026-05-17
sprint: 7
---

# Sesión 2026-05-17 — Sprint 7: Código completo

## Resumen

Sesión de implementación del bloque de código del Sprint 7. Se implementaron S7-02 (partículas, sesión anterior), S7-03 (Arquero), S7-04 (audio) y S7-05 (InscripcionVampiricaMana). Todo verificado con build y smoke test rápido en ejecución.

---

## Implementado

### S7-02 — Partículas (sesión anterior)
- `utils/Particle.java` + `utils/ParticlePool.java` — pool estático 400 partículas
- Hooks en `ColisionManager` (impacto, muerte, explosión MALDITO)
- Hooks en `GameScreen` (update, render, clear en restart)

### S7-03 — Minijefe Arquero ✅
- `Enemy.Tipo.ARQUERO` añadido al enum
- Campo `arqueroTeleportTimer`, lógica de strafe + teleporte + disparo en `Enemy.update()`
- `PoolEnemigos.spawnArquero()` + `getActiveArquero()`
- `BalaEnemiga` actualizada con daño configurable por instancia
- `PoolBalasEnemigas.spawnWithDamage()`
- `SharedTextures.arquero` — círculo blanco 48 px
- Barra de boss **azul** en HUD con flag `bossIsArquero`
- `GameScreen`: spawn oleada 20, tracking `arqueroRef`, drop cofre con arma al morir
- `spawnChestAt(float x, float y)` añadido a `GameScreen`
- `ColisionManager.contactDamagePor()` — caso ARQUERO
- `ParticlePool.colorForTipo()` — color azul para ARQUERO

**Diseño doc**: [[Arquero-Minijefe]]

### S7-04 — Audio básico ✅ (infraestructura)
- `utils/AudioManager.java` — carga condicional, no-op si faltan archivos
- Hook en `KaosuarinaGame.create()` y `dispose()`
- `AudioManager.startMusic()` en `GameScreen.inicializar()`
- `AudioManager.playLevelUp()` en `GameScreen.detectarLevelUp()`
- `AudioManager.playBoss()` al spawn de Guardian y Arquero
- `AudioManager.playHit()` + `playDeath()` en `ColisionManager`

**Pendiente**: añadir archivos WAV/OGG en `assets/audio/`  
**Diseño doc**: [[Audio-Basico]]

### S7-05 — InscripcionVampiricaMana ✅
- `weapons/inscriptions/InscripcionVampiricaMana.java` — `getName()="VMN"`, `onHit()→addMana(2)`
- Registrada en `InscriptionPool` (índice 8, probabilidad igual a las demás)

---

## Corrección importante

La spec de S7-03 decía que el Arquero dropeara **arma** (cofre), no scroll. El Guardian ya dropea scroll. Se añadió `spawnChestAt()` y se corrigió el drop del Arquero para que use cofre.

---

## Pendiente Sprint 7

- **S7-01**: Sprites (animaciones PixelLab — pendiente generar walk/attack + integrar en AnimationSheets)
- **S7-04**: Assets de audio (WAV + OGG — buscar en freesound/kenney)
- **S7-06**: Smoke test completo (14 checks)
- `00-stat-baseline.md` — sección §16

---

## Próximos pasos

1. Conseguir/generar assets de audio y colocarlos en `assets/audio/`
2. Completar animaciones PixelLab para los 3 roles (walk + attack)
3. Integrar animaciones en `AnimationSheets.java` y `Player.render()`
4. Smoke test S7-06 cuando todo esté listo

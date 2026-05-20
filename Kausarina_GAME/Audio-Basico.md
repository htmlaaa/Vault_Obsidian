---
tags: [audio, diseño, sprint-7]
estado: infraestructura-lista — assets pendientes
sprint: S7-04
fecha: 2026-05-17
---

# Audio Básico

## Estado

La infraestructura (`AudioManager.java`) está implementada con carga condicional: si el archivo no existe el juego arranca sin audio y sin crash. Los **archivos de audio son lo único pendiente**.

---

## Arquitectura

Clase estática `utils/AudioManager.java`:
- `Sound` (LibGDX) para SFX cortos — se reproducen con `sound.play(volume)`.
- `Music` (LibGDX) para el track de fondo — streaming, loop automático.
- Todos los métodos son no-ops si el archivo no se cargó.

### Hooks implementados

| Evento | Método | Archivo de audio |
|--------|--------|-----------------|
| Disparo bala impacta enemigo | `AudioManager.playHit()` | `audio/hit.wav` |
| Enemigo muere | `AudioManager.playDeath()` | `audio/death.wav` |
| Minijefe aparece | `AudioManager.playBoss()` | `audio/boss.wav` |
| Level up | `AudioManager.playLevelUp()` | `audio/levelup.wav` |
| Iniciar partida | `AudioManager.startMusic()` | `audio/music.ogg` |

> Nota: `playShot()` (al disparar el jugador) está implementado pero **no está hookeado** todavía — requiere pasar AudioManager al flujo de disparo del Player. Pendiente para Sprint 8.

---

## Assets necesarios

Todos en la carpeta `assets/audio/` del proyecto LWJGL3.

| Archivo | Descripción | Duración ideal |
|---------|-------------|---------------|
| `hit.wav` | Impacto bala en enemigo — sonido seco, metálico | < 0.1 s |
| `death.wav` | Enemigo muere — pop corto o destello | < 0.2 s |
| `levelup.wav` | Subida de nivel — fanfarria breve ascendente | 0.5–1 s |
| `boss.wav` | Minijefe aparece — stinger dramático | 1–2 s |
| `shot.wav` | Jugador dispara — implementado pero sin hook | < 0.05 s |
| `music.ogg` | Música de fondo — synthwave o chiptune, loop | 2–4 min |

### Fuentes de audio libre de derechos

- **freesound.org** — buscar "8bit hit", "chiptune death", "level up"
- **itch.io** — packs de audio para roguelite/bullet hell
- **kenney.nl/assets** — "Interface Sounds", "Sci-fi Sounds"
- **opengameart.org** — filtrar por CC0

### Formato técnico

- **SFX**: WAV, 44100 Hz, 16-bit mono, < 100 KB cada uno.
- **Música**: OGG Vorbis, 128–192 kbps, stereo, loop sin click (editar puntos de loop en Audacity).

---

## Ruta de assets en el proyecto

Los archivos internos en LibGDX se cargan con `Gdx.files.internal("ruta")`. La raíz para LWJGL3 es la carpeta `assets/` del módulo:

```
A_Game_Kaosuarina/
  lwjgl3/
    src/
      main/
        resources/
          audio/          ← aquí van los archivos
            hit.wav
            death.wav
            levelup.wav
            boss.wav
            shot.wav
            music.ogg
```

> En algunos setups de gdx-liftoff la carpeta `assets/` está en la raíz del proyecto y se comparte con Android. Verifica en `build.gradle` del módulo `lwjgl3` la propiedad `sourceSets.main.resources.srcDirs`.

---

## Volumetría

| Canal | Volumen base |
|-------|-------------|
| Música | `masterVolume × 0.5` = 35% |
| SFX hit | `masterVolume × 0.4` = 28% |
| SFX death | `masterVolume × 0.5` = 35% |
| SFX levelup | `masterVolume × 0.8` = 56% |
| SFX boss | `masterVolume × 0.9` = 63% |

`masterVolume` = 0.7f (ajustable en `AudioManager`).

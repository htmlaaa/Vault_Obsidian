---
title: LibGDX — Framework de Desarrollo
tags:
  - tecnologia
  - framework
  - java
  - libgdx
  - gradle
aliases:
  - LibGDX
  - gdx
cssclasses: []
---

# LibGDX — Framework de Desarrollo

LibGDX es un framework de desarrollo de videojuegos multiplataforma escrito en **Java**, de código abierto y mantenido activamente por la comunidad.

> [!info] Versión usada en Kaosuarina
> El proyecto [[Kaosuarina]] fue generado con **gdx-liftoff**, la herramienta oficial de scaffolding de LibGDX.

## ¿Qué es LibGDX?

LibGDX proporciona una capa de abstracción unificada sobre OpenGL (desktop), WebGL (web) y OpenGL ES (móvil), permitiendo escribir el código del juego **una sola vez** y desplegarlo en múltiples plataformas.

## Módulos del proyecto

| Módulo | Descripción |
|--------|-------------|
| `core` | Lógica principal compartida por todas las plataformas |
| `lwjgl3` | Plataforma de escritorio usando LWJGL3 (antes llamado `desktop`) |

## Arquitectura de LibGDX

```
ApplicationListener
    └── Game (extiende ApplicationAdapter)
            └── Screen (GameScreen, LevelUpScreen...)
```

- El punto de entrada es una clase que implementa `ApplicationListener` o extiende `Game`.
- Las pantallas implementan la interfaz `Screen` y se gestionan con `game.setScreen()`.
- El loop principal llama a `render(float delta)` en cada fotograma.

## Sistema de coordenadas

- Origen `(0,0)` en la esquina **inferior izquierda**.
- Eje Y apunta hacia **arriba**.
- Importante para convertir coordenadas de ratón: `Gdx.graphics.getHeight() - Gdx.input.getY()`.

## Clases clave usadas en Kaosuarina

| Clase LibGDX | Uso |
|---|---|
| `SpriteBatch` | Renderizado en batch de sprites y texturas |
| `OrthographicCamera` | Cámara 2D con proyección ortográfica |
| `ShapeRenderer` | Dibujado de primitivas (rect, circle, line) |
| `BitmapFont` | Renderizado de texto con fuentes bitmap |
| `Pixmap` | Generación de texturas procedurales en CPU |
| `Vector2` | Vectores 2D para posición y velocidad |
| `MathUtils` | Utilidades matemáticas (atan2, cos, sin, random) |
| `Array<T>` | Array dinámico optimizado para GDX (evita boxing) |

> [!warning] Blending y transparencia
> `ShapeRenderer` no activa blending automáticamente. Para usar colores con alpha < 1 hay que llamar explícitamente a:
> ```java
> Gdx.gl.glEnable(GL20.GL_BLEND);
> Gdx.gl.glBlendFunc(GL20.GL_SRC_ALPHA, GL20.GL_ONE_MINUS_SRC_ALPHA);
> ```

## Gestión de memoria y GC

LibGDX corre en JVM pero los recursos gráficos (`Texture`, `SpriteBatch`, `ShapeRenderer`...) son **nativos** y deben liberarse manualmente con `.dispose()`.

> [!danger] Regla crítica
> Todo objeto que implemente `Disposable` DEBE ser liberado al destruir la pantalla. Si no, se producen leaks de memoria de GPU.

Los objetos que se crean cada fotograma (como `new Color()`) generan **presión de GC** que causa micro-stutters. Usar campos pre-alojados y reutilizarlos con `.set()`.

## Gradle — Sistema de build

El proyecto usa **Gradle** con wrapper incluido.

### Tareas más usadas

```bash
# Ejecutar el juego en escritorio
gradlew lwjgl3:run

# Compilar jar ejecutable
gradlew lwjgl3:jar
# Salida: lwjgl3/build/libs/

# Limpiar builds
gradlew clean

# Construir todo
gradlew build
```

### Flags útiles

| Flag | Descripción |
|------|-------------|
| `--daemon` | Usa el daemon de Gradle (arranque más rápido) |
| `--offline` | Usa dependencias en caché sin red |
| `--continue` | Continúa aunque fallen tareas anteriores |
| `--refresh-dependencies` | Fuerza revalidación de dependencias |

## Object Pooling en LibGDX

Para entidades que se crean y destruyen frecuentemente (balas, enemigos), LibGDX recomienda **object pooling**: crear un conjunto fijo de objetos al inicio y reutilizarlos activando/desactivando con un flag `active`.

```
Pool → spawn() activa un objeto inactivo
     → update() procesa solo activos
     → al morir: active = false (vuelve al pool)
```

[[Kaosuarina]] implementa este patrón con `PoolBalas`, `PoolEnemigos` y `PoolBalasEnemigas`.

## Referencias

- [Documentación oficial LibGDX](https://libgdx.com/dev/)
- [gdx-liftoff](https://github.com/libgdx/gdx-liftoff)
- [Wiki LibGDX en GitHub](https://libgdx.com/wiki/)

---
title: README — Kaosuarina
tags:
  - readme
  - entrega
  - compilacion
aliases:
  - README
date: 2026-05-21
---

# Kaosuarina — README de Entrega

> [!abstract] Descripción rápida
> **Kaosuarina** es un videojuego roguelite top-down shooter con mecánicas RPG desarrollado en **Java 8** con el framework **LibGDX 1.14.0**. El jugador elige entre tres roles (Caballero, Mago, Tirador) y debe sobrevivir oleadas de enemigos hasta derrotar al boss final en la oleada 50. No requiere instalación ni servidor externo.

---

## 1. Requisitos del sistema

| Componente | Mínimo requerido |
|---|---|
| Sistema operativo | Windows 10 / macOS 12 / Linux (Ubuntu 20.04+) |
| Java | **JRE 8** o superior (JDK si se compila desde fuente) |
| RAM | 512 MB libres |
| GPU | Tarjeta con soporte OpenGL 2.0 (cualquier equipo 2012+) |
| Espacio en disco | ~80 MB (JAR + assets + base de datos) |

> [!tip] Comprobar versión de Java instalada
> ```bash
> java -version
> ```
> Debe mostrar `java version "1.8.x"` o superior. Si no está instalado: https://adoptium.net

---

## 2. Tecnología utilizada

| Componente | Tecnología | Versión |
|---|---|---|
| Lenguaje | Java | **8** (source/target compatibility 1.8) |
| Framework de juego | **LibGDX** | **1.14.0** |
| Plataforma de escritorio | LWJGL3 (vía LibGDX) | 1.14.0 |
| Sistema de build | **Gradle** (wrapper incluido) | 8.x (wrapper) |
| Base de datos | **SQLite** embebido | — |
| Driver JDBC SQLite | org.xerial : sqlite-jdbc | **3.45.3.0** |
| Parsing JSON | Google Gson | **2.10.1** |
| Scaffolding inicial | gdx-liftoff | 1.14.0 |
| IDE recomendado | IntelliJ IDEA Community | 2023+ |
| Generación de sprites | PixelLab (herramienta online) | — |

> [!info] Sin plugins externos de Unity / Unreal
> El proyecto **no usa Unity ni Unreal Engine**. LibGDX es un framework Java puro. No hay complementos adicionales que instalar más allá de las dependencias Gradle declaradas en `build.gradle`.

---

## 3. Estructura del proyecto entregado

```
Kaosuarina_Entrega/
│
├── A_Game_Kaosuarina/          ← Proyecto fuente completo
│   ├── core/                   ← Módulo principal (toda la lógica del juego)
│   │   └── src/main/java/com/milwar/kaosuarina/
│   ├── lwjgl3/                 ← Módulo de escritorio (launcher)
│   ├── assets/                 ← Recursos: sprites, JSON de datos, audio (*)
│   │   ├── characters/         ← Sprites animados de los 3 personajes
│   │   ├── game_data_json/     ← 18 ficheros JSON de configuración del juego
│   │   └── audio/              ← Carpeta lista; ficheros WAV/OGG pendientes (*)
│   ├── build.gradle            ← Build raíz
│   ├── gradlew                 ← Wrapper Gradle para Linux/macOS
│   ├── gradlew.bat             ← Wrapper Gradle para Windows
│   └── settings.gradle
│
├── Kaosuarina.jar              ← Ejecutable precompilado (listo para usar)
├── kaosuarina.db               ← Base de datos SQLite (se crea automáticamente)
└── README_Kaosuarina.md        ← Este documento
```

> [!warning] Nota sobre audio
> La infraestructura de audio está implementada (`AudioManager.java`) pero los ficheros `.wav` y `.ogg` no se incluyen por limitaciones de licencia. El juego arranca y funciona completamente sin ellos — simplemente no habrá sonido. Para añadirlos: colocar en `assets/audio/sfx/` los ficheros `shoot.wav`, `hit.wav`, `death_enemy.wav`, `levelup.wav`, `pickup.wav` y en `assets/audio/music/` el fichero `background.ogg`.

---

## 4. Ejecución directa (sin compilar)

La forma más sencilla es usar el JAR precompilado incluido en la entrega:

### Windows
```bat
java -jar Kaosuarina.jar
```
O simplemente hacer **doble clic** en `Kaosuarina.jar` si Java está asociado a ficheros `.jar`.

### Linux / macOS
```bash
java -jar Kaosuarina.jar
```

> [!tip] Si al hacer doble clic no abre
> En Windows: clic derecho → "Abrir con" → Java Platform SE Binary.
> En Linux: `sudo apt install default-jre` si Java no está instalado.

---

## 5. Compilación desde código fuente

### 5.1 Prerrequisitos
- **JDK 8** o superior instalado y en el PATH
- Conexión a Internet (primera vez, para descargar dependencias Gradle)
- No se necesita instalar Gradle manualmente — el proyecto incluye el wrapper `gradlew`

### 5.2 Clonar / descomprimir el proyecto
```bash
# Desde el ZIP entregado: descomprimir y entrar en la carpeta
cd A_Game_Kaosuarina
```

### 5.3 Compilar y ejecutar directamente
```bat
# Windows
gradlew.bat lwjgl3:run

# Linux / macOS
./gradlew lwjgl3:run
```

La primera ejecución descargará las dependencias de Maven Central (~50 MB). Las siguientes serán inmediatas.

### 5.4 Generar el JAR ejecutable
```bat
# Windows
gradlew.bat lwjgl3:jar

# Linux / macOS
./gradlew lwjgl3:jar
```

El JAR resultante aparece en:
```
lwjgl3/build/libs/lwjgl3-1.0.0.jar
```

Renombrar a `Kaosuarina.jar` y colocarlo junto a la carpeta `assets/` para que los recursos sean accesibles.

### 5.5 Compilar sin ejecutar (solo verificar que compila)
```bat
gradlew.bat :core:compileJava
```

### 5.6 Limpiar builds anteriores
```bat
gradlew.bat clean
```

---

## 6. Dependencias declaradas en Gradle

Todas las dependencias se descargan automáticamente de **Maven Central** la primera vez. No hay que instalar nada manualmente.

### `core/build.gradle`
```gradle
dependencies {
    api "com.badlogicgames.gdx:gdx:1.14.0"
    api "org.xerial:sqlite-jdbc:3.45.3.0"
    api "com.google.code.gson:gson:2.10.1"
}
```

### `lwjgl3/build.gradle`
```gradle
dependencies {
    implementation "com.badlogicgames.gdx:gdx-backend-lwjgl3:1.14.0"
    implementation "com.badlogicgames.gdx:gdx-platform:1.14.0:natives-desktop"
}
```

> [!note] Sin dependencias de servidor
> No se usa MySQL ni ningún servidor externo. La base de datos **SQLite** (`kaosuarina.db`) se crea automáticamente en el directorio de ejecución al primer arranque.

---

## 7. Base de datos

| Parámetro | Valor |
|---|---|
| Motor | SQLite (embebido, sin servidor) |
| Driver | `org.xerial:sqlite-jdbc:3.45.3.0` |
| Fichero | `kaosuarina.db` (junto al JAR) |
| Creación | Automática al primer arranque (`initDB()`) |
| Tablas | `run`, `run_kill`, `run_upgrade`, `run_reliquia`, `run_arma` |

Si se borra `kaosuarina.db`, el juego crea uno nuevo vacío en el siguiente arranque (se pierden los registros del leaderboard).

---

## 8. Controles del juego

| Tecla | Acción |
|---|---|
| `WASD` | Mover personaje |
| `Ratón` | Apuntar |
| `Clic izquierdo` | Ataque ligero (Caballero / Mago) |
| `Clic derecho` | Ataque pesado (Caballero / Mago) |
| `Q / E` | Activar skill del arma en slot 1 / slot 2 |
| `TAB` | Abrir / cerrar inventario (6 ranuras) |
| `1 / 2` | Seleccionar slot en menú de intercambio |
| `X` | Descartar arma nueva |
| `1 / 2 / 3` | Seleccionar mejora al subir de nivel |
| `ENTER / SPACE` | Confirmar selección |
| `R` | Jugar de nuevo |
| `L` | Ver clasificación |
| `C` | Créditos |
| `ESC` | Volver al menú principal |

---

## 9. Posibles problemas y soluciones

> [!bug]- El juego no arranca / error `UnsatisfiedLinkError`
> Las librerías nativas de LWJGL3 están empaquetadas dentro del JAR. Si hay un error de este tipo, asegurarse de usar el JAR generado con `gradlew lwjgl3:jar` (fat JAR con nativos incluidos) y no el JAR del módulo `core`.

> [!bug]- `java.lang.UnsupportedClassVersionError`
> El JAR requiere JRE 8+. Actualizar Java desde https://adoptium.net

> [!bug]- Pantalla en negro al arrancar
> La GPU no soporta OpenGL 2.0. En equipos muy antiguos o máquinas virtuales sin aceleración gráfica, añadir al comando de ejecución: `java -jar Kaosuarina.jar -Dorg.lwjgl.opengl.Display.allowSoftwareOpenGL=true`

> [!bug]- Gradle descarga dependencias muy lento
> Es normal la primera vez (~50 MB). Usar `gradlew --offline` en ejecuciones posteriores si ya se descargaron.

> [!bug]- Error de base de datos al guardar la partida
> El juego continúa sin guardar si falla la BD (fallo silencioso). Comprobar que el directorio de ejecución tiene permisos de escritura para crear `kaosuarina.db`.

---

## 10. Información de versión y build

| Campo | Valor |
|---|---|
| Nombre del juego | Kaosuarina |
| Versión | 1.0.0 (MVP + Post-MVP) |
| Fecha de compilación | Mayo 2026 |
| Autor | [NOMBRE ALUMNO] |
| Centro | IES Virgen de la Paloma — DAM |
| Tutor | Isidoro Nevares Martín |
| Repositorio | GitHub (privado durante evaluación) |
| Licencia del código | Uso educativo |

---

## 11. Contenido de la entrega completa

La carpeta comprimida `.zip` de entrega debe contener:

- [ ] `Kaosuarina.jar` — ejecutable precompilado
- [ ] `A_Game_Kaosuarina/` — código fuente completo con Gradle wrapper
- [ ] `assets/` — recursos del juego (sprites, JSON, audio*)
- [ ] `Memoria_Kaosuarina_TFG.docx` — memoria técnica del proyecto
- [ ] `README_Kaosuarina.md` — este documento
- [ ] `kaosuarina.db` — base de datos de ejemplo con partidas de prueba (opcional)

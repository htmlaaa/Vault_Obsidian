---
title: Sesión 2026-05-11 — Rediseño RPG (Día 2)
tags:
  - sesion
  - rediseno
  - rpg
  - sprint2
  - mana
  - armas
  - damagetypes
date: 2026-05-11
---

# Sesión 2026-05-11 — Rediseño RPG (Día 2)

Implementación completa del núcleo RPG del Sprint 2 (S2-06 → S2-11). Compilación verificada. Sprint 2 cerrado.

---

## Lo hecho hoy

### S2-06 — Reescalar stats por rol ✅

Nuevos valores en `roles/Role.java` (factory methods):

| Rol | HP | Defensa | ResMag | MaxManá | Regen | Velocidad | CD | Balas |
|-----|---:|--------:|-------:|--------:|------:|----------:|----|------:|
| Caballero | 200 | 20 | 0 | 50 | 0 | 250 | 0.25s | 1 |
| Mago | 70 | 5 | 15 | 120 | 2/s | 270 | 0.22s | 1 |
| Tirador | 80 | 5 | 0 | 30 | 0 | 340 | 0.16s | 2 |

- Display name "Shooter" renombrado a "Tirador" (`nombre`)
- Maná empieza lleno en todos los roles

---

### S2-07 — Reescalar HP enemigos + resistencias ✅

Nuevos valores en `entities/Enemy.java`:

| Tipo | HP | Def | ResMag |
|------|----|-----|--------|
| BASICO | 30 | 0 | 0 |
| RAPIDO | 12 | 0 | 0 |
| TANQUE | 180 | 20 | 0 |
| SHOOTER | 50 | 0 | 0 |

Campos `defensa` y `resistenciaMagica` añadidos como `public float` — se resetean a 0 en cada `activate()`.

---

### S2-08 — Enum DamageType + ColisionManager ✅

Nuevo archivo: `utils/DamageType.java`

```java
public enum DamageType { FISICO, MAGICO, A_DISTANCIA, FUEGO, VENENO, CAOS }
```

Cambios en cadena:
- `Bala.java`: campo `public DamageType damageType`; `activate()` acepta tipo; overload retrocompatible defaultea a FISICO
- `PoolBalas.java`: `spawn()` pasa DamageType; overloads mantienen retrocompatibilidad
- `ColisionManager.java`: método privado `calcularDaño(raw, tipo, enemy)`:
  - FISICO → `max(1, raw - defensa)`
  - MAGICO → `max(1, raw - resistenciaMagica)`
  - CAOS → `max(1, raw - defensa*0.5 - resMag*0.5)` (ignora 50%)
  - Resto → sin reducción
- `Player.java`: método `tipoDanio()` → CABALLERO=FISICO, MAGO=MAGICO, SHOOTER=A_DISTANCIA

---

### S2-09 — Interfaz Reliquia + paquete reliquias/ ✅

Nuevo paquete `reliquias/`:

| Archivo | Descripción |
|---------|-------------|
| `Reliquia.java` | Interfaz (reemplaza CoreEffect) |
| `ReliquiaCaballero.java` | Fortaleza Reactiva (stacks armadura) |
| `ReliquiaMago.java` | Resonancia Caótica (getBounces=1) |
| `ReliquiaTirador.java` | Momentum de Combate (combo kills→cadencia) |

- `Role.java`: campo renombrado `coreEffect` → `reliquia`, tipo `Reliquia`
- `Player.java`: campo renombrado `coreEffect` → `reliquia`, import actualizado
- `cores/` queda como dead code — **borrar manualmente** la carpeta `cores/`

---

### S2-10 — Maná funcional + HUD ✅

**Player.java** — regen pasiva en `update()`:
```java
if (stats.manaRegen > 0) stats.añadirMana(stats.manaRegen * delta);
```

**GameScreen.java** — Sed de Sangre en `procesarColisiones()`:
```java
player.onKill();
player.getStats().añadirMana(5f); // +5 maná por kill
```

**HUD.java**:
- `currentMana`/`maxMana` convertidos a `float`
- `renderManaBar()` activado condicionalmente: `if (maxMana > 0) renderManaBar()`
- Ancho de barra dinámico: `min(200f, 50f + maxMana)` — Mago tendrá barra más ancha
- `setMana(float, float)` actualizado
- `actualizarJuego()` llama `hud.setMana(stats.mana, stats.maxMana)` cada frame

---

### S2-11 — Smoke test ✅

Compilación `gradlew core:compileJava` → OK sin errores. Los 3 roles compilados con nuevas stats, DamageType y Reliquia.

---

## GDDs generados por Opus (paralelo)

El agente Opus generó durante esta sesión:

| Archivo | Contenido |
|---------|-----------|
| `Code_Game/design/gdd/02-weapon-system.md` | Sistema de armas dual + 6 armas MVP + inscripciones |
| `Code_Game/design/gdd/03-mana-system.md` | Sistema de maná universal por rol + recuperación |

**Puntos de atención del agente:**
- Propone maxManá Caballero en 40 (nosotros lo pusimos en 50 — decisión confirmada)
- Inscripción Vampírica de Maná es nueva — añadir a `Inscripciones-Sistema.md` del Vault
- Cicatriz de Combate sugerida como Reliquia del Caballero (reemplaza Fortaleza Reactiva en sprint futuro)
- Overkill: +1 maná por cada 10 daño excedente → implementar en Sprint 3

---

## Estado del Sprint 2

| Story | Estado | Día |
|-------|--------|-----|
| S2-01 Fix ShooterCore decay | ✅ done | Día 1 |
| S2-02 Fix multilevel upgrades | ✅ done | Día 1 |
| S2-03 Eliminar Role.base() | ✅ done | Día 1 |
| S2-04 Centralizar daño contacto | ✅ done | Día 1 |
| S2-05 Extender PlayerStats RPG | ✅ done | Día 2 (inicio) |
| S2-06 Reescalar stats roles | ✅ done | Día 2 |
| S2-07 Reescalar HP enemigos | ✅ done | Día 2 |
| S2-08 DamageType + ColisionManager | ✅ done | Día 2 |
| S2-09 Reliquia + refactor cores/ | ✅ done | Día 2 |
| S2-10 Maná funcional + HUD | ✅ done | Día 2 |
| S2-11 Smoke test | ✅ done | Día 2 |

**Sprint 2 — COMPLETO ✅**

---

## Archivos modificados hoy

```
roles/Role.java                    ← stats reescalados, maná, Reliquia
entities/Enemy.java                ← HP reescalados, defensa/resMag campos
entities/Bala.java                 ← campo damageType
entities/PoolBalas.java            ← spawn() con DamageType
entities/Player.java               ← tipoDanio(), regen maná, Reliquia
screens/GameScreen.java            ← Sed de Sangre, hud.setMana()
ui/HUD.java                        ← renderManaBar() activado, float mana
utils/DamageType.java              ← NUEVO enum
utils/ColisionManager.java         ← calcularDaño() con resistencias
reliquias/Reliquia.java            ← NUEVO interfaz
reliquias/ReliquiaCaballero.java   ← NUEVO
reliquias/ReliquiaMago.java        ← NUEVO
reliquias/ReliquiaTirador.java     ← NUEVO
Code_Game/design/gdd/02-weapon-system.md  ← NUEVO (Opus)
Code_Game/design/gdd/03-mana-system.md    ← NUEVO (Opus)
```

---

## Pendiente Sprint 3

- Borrar `cores/` manualmente (sandbox bloqueó la eliminación)
- Implementar sistema de armas dual (2 slots, Arma Normal + Arma de Habilidad)
- Overkill: +1 maná por 10 daño excedente
- Inscripción Vampírica de Maná (+2 maná por impacto)
- Sistema de Reliquias completo (3 opciones, 1 activa)
- DamageType FUEGO y VENENO (DoT)

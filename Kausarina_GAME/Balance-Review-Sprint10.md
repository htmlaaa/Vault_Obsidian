---
title: Balance Review — Sprint 10
tags:
  - kausarina-game
  - balance
  - sprint10
  - combat
date: 2026-05-20
status: review
sprint: sprint10
---

> [!info]+ Scope del documento
> Evaluacion de balance post-cambios Sprint 10: nuevos HP de enemigos (TANQUE, RAPIDO, SHOOTER, ARQUERO, GUARDIAN, DEVASTADOR), tiempos de invulnerabilidad por rol, y CAOS_PRIMORDIAL con true damage + BURN. Referencia: `enemies.json`, `enemyresistances.json`, `Constants.java`.

# Balance Review — Sprint 10

---

## 1. Evaluacion por tipo de enemigo

### EN_NORMAL — Soldado (BASICO)

| Stat | Valor |
|------|-------|
| HP | 40 |
| Daño contacto | 8 |
| Velocidad | 3.5 |
| Tipo daño | DMG_PHYSICAL |

**Veredicto: JUSTO**

El Soldado sigue cumpliendo su rol de enemigo de presion numerico. Con el nuevo `INVULNERABILITY_CABALLERO = 0.6s` (antes 1.0s), el jugador toma hits mas frecuentes en swarms densos, lo que eleva la peligrosidad percibida sin cambiar el stat del enemigo. Para el Tirador (80 HP, iframes 0.45s) los swarms de Soldados en oleadas avanzadas pueden acumular 16 dmg/s de contacto si estan todos pegados — controlable con la movilidad del Tirador.

Sin ajuste recomendado.

---

### EN_TANK — Brutal (TANQUE)

| Stat | Valor actual | Valor anterior |
|------|-------------|----------------|
| HP | 110 | 180 |

> [!warning] REDUCCION SIGNIFICATIVA

**Veredicto: NECESITA REVISION**

El cambio de HP de 180 a 110 es una reduccion del 39%. Con las resistencias actuales:
- DMG_PHYSICAL: 40% resist → Caballero melee ligero efectivo = 22 × 0.60 = **13.2 dmg**
- HP efectivo vs Caballero: 110 / 13.2 = **~8 golpes ligeros** (antes ~14 golpes)
- HP efectivo vs Tirador auto: 18 × 0.75 (sin resist ranged base) = 13.5 dmg → 110 / 13.5 = **~8 disparos**

El TANQUE pasa de ser una amenaza de presion sostenida a caer en ~8 hits, lo que lo hace menos intimidante. El Mago lo devuelve de forma eficiente: 22 bolt con -15% resist (vulnerable) = 22 × 1.15 = **25.3 dmg efectivo**, cayendo en **~4-5 bolts**.

**Recomendacion:** Subir HP de 110 a **145** — esto lo mantiene por debajo del valor original (180) pero le devuelve peso tatico. El Caballero necesitara ~11 golpes ligeros; el Mago ~6 bolts. Sigue siendo el target prioritario del Mago.

---

### EN_KAMIKAZE — Velocista (RAPIDO)

| Stat | Valor actual | Valor anterior |
|------|-------------|----------------|
| HP | 25 | 12 |

> [!success] MEJORA CORRECTA

**Veredicto: JUSTO**

El Velocista con 12 HP moría de prácticamente cualquier hit incidental (el bolt del Mago hace 22 dmg). Con 25 HP necesita al menos 1 hit directo del Mago o 2 disparos del Tirador (18 dmg × 2 = 36 > 25), lo que da sensacion de amenaza real antes de morir. Su daño de contacto 18-25 (maximo 25) sigue siendo el mas alto entre minions, lo que refuerza su identidad de kamikaze.

Sin ajuste recomendado.

---

### EN_ORBITER — Tirador Sigiloso (SHOOTER-enemy)

| Stat | Valor actual | Valor anterior |
|------|-------------|----------------|
| HP | 35 | 50 |

> [!warning] REDUCCION — POSIBLE SOBREAJUSTE

**Veredicto: ALGO FACIL**

El Orbiter ahora cae en **2 disparos del Tirador** (18 × 2 = 36 > 35) o **1-2 bolts del Mago** (22 bolt, pero el EN_ORBITER no tiene resist magic declarada, asi que toma daño completo). El valor de 35 HP esta en el limite de ser un one-shot contextual para el Mago con upgrade de daño minimo.

Su daño ranged 5-9 sigue siendo menor que otros enemigos, pero orbita al jugador de forma persistente. Reducirlo demasiado lo convierte en un farmeable de XP sin tension real.

**Recomendacion:** Subir HP de 35 a **45** — sigue siendo fragil (rol de tirador) pero requiere 3 disparos del Tirador y aguanta 2 bolts del Mago, manteniendo la presion durante su orbita.

---

### EN_ELITE_TP — Sombra Errante

| Stat | Valor |
|------|-------|
| HP | 90 |
| Resistencias | DMG_MAGIC 30%, DMG_RANGED 15% |

**Veredicto: JUSTO**

La Sombra Errante tiene una resistencia problematica para el Mago (30% resist magic): bolt efectivo = 22 × 0.70 = **15.4 dmg**, cayendo en **~6 bolts**. Para el Tirador: 18 × 0.85 = **15.3 dmg** → **~6 disparos**. El Caballero melee es el counter natural (sin resist fisica declarada → dmg completo).

La reduccion de invulnerabilidad del Mago (0.7s) es relevante si la Sombra dispara mientras el Mago la ataca en melee — pero la IA orbiter la mantiene a distancia, asi que el Mago puede recibir varios proyectiles en secuencia rapida.

Sin ajuste recomendado en stats. Vigilar que su AI de teleport no la haga inaccesible para el Caballero (melee range limitado).

---

### EN_ELITE_AOE — Coloso Sismico

| Stat | Valor |
|------|-------|
| HP | 260 |
| Resistencias | DMG_PHYSICAL 50%, DMG_FIRE 30%, DMG_MAGIC -20% |

**Veredicto: JUSTO pero PELIGROSO para Mago**

El Coloso con 50% resist fisica es casi inmune al Caballero sin upgrades: 22 × 0.50 = **11 dmg** → **~24 golpes ligeros**. La inscripcion SISMICA (stun 0.3s) aplica aqui, pero el Caballero necesita invertir mucho tiempo.

El Mago es el counter correcto: -20% resist magic = 22 × 1.20 = **26.4 dmg** → **~10 bolts**. El area 55 dmg × 1.20 = **66 dmg** → **~4 hits de explosion**. Esto es correcto.

**Problema detectado:** El AoE sismica del Coloso (cada ~6s) vs el Mago (70 HP) — si el Mago recibe 1 shockwave AoE + 1 golpe de contacto durante el ataque en secuencia rapida, puede caer antes de que su invulnerabilidad 0.7s proteja el segundo hit si el primer hit fue justo antes del iframe. Con la nueva logica de INVULNERABILITY reducida, este combo es mas probable.

**Recomendacion:** Reducir `EN_ELITE_AOE dmg_max` de 22 a **18** (mantener dmg_min en 14). Esto da margen de supervivencia al Mago sin cambiar la identidad del Coloso como tanque amenazante.

---

### EN_MINIBOSS — Heraldo de Hierro (GUARDIAN)

| Stat | Valor actual | Valor anterior |
|------|-------------|----------------|
| HP | 900 | 800 |

> [!success] AJUSTE CORRECTO

**Veredicto: JUSTO**

El Heraldo con 900 HP (confirmado en `Constants.java: GUARDIAN_HP = 900`) representa una batalla de ~45-60 segundos para el Caballero en melee con upgrades moderados. Con 35% resist fisica → dmg melee ligero efectivo = 22 × 0.65 = **14.3 dmg** → 900 / 14.3 = **~63 golpes** — esta es una pelea larga pero el GUARDIAN_ATTACK_INTERVAL de 3s le da ventanas de respiro.

El Mago no tiene una resistencia magic declarada en el Heraldo — dmg completo. Bolt: 22 dmg × 63 hits equivalentes = 900 / 22 = **~41 bolts**. El Mago tiene que gestionar mana (120 max) cuidadosamente.

Sin ajuste recomendado.

---

### EN_BOSS_SECRET — Devorador Abisal (ARQUERO minijefe, wave 20)

> [!note]
> El Arquero minijefe usa `Constants.java: ARQUERO_HP = 300` (antes 500). Los stats de `enemies.json` para `EN_BOSS_SECRET` son del Devorador Abisal (boss oculto), no del Arquero de Sprint 7. Evaluacion basada en Constants.

| Stat | Valor actual | Valor anterior |
|------|-------------|----------------|
| ARQUERO_HP | 300 | 500 |

**Veredicto: POSIBLE SOBREAJUSTE**

Reducir el Arquero de 500 a 300 HP es una caida del 40%. El Arquero aparece en wave 20, donde el jugador tiene upgrades significativos. Con el Tirador: 18 dmg × n hits con posibles upgrades de daño (+10-20% comunes) → cae muy rapido para ser un minijefe en wave 20.

**Recomendacion:** Subir ARQUERO_HP a **380** en `Constants.java`. A 380 HP, con el Tirador base (18 dmg) = ~21 disparos antes de upgrades — todavia rapido, pero tematicamente correcto para un minijefe de fase media. **NOTA: Este cambio requiere editar Constants.java — fuera del scope de este documento. Registrar como tarea pendiente.**

---

### EN_BOSS_FINAL — Devastador del Caos

| Stat | Valor actual | Valor anterior |
|------|-------------|----------------|
| DEVASTADOR_HP | 2200 | 2000 |

**Veredicto: JUSTO**

El incremento de 200 HP (+10%) sobre el boss final es conservador y correcto. El Devastador ya tiene mecanicas complejas (fases, CAOS_PRIMORDIAL true damage, BURN). El +10% HP extiende la pelea ~15-20% en fase 1 (ya que el jugador llega con upgrades maximos a wave 50) — es tension adicional sin hacerlo imposible.

**CAOS_PRIMORDIAL — True Damage + BURN:** Con la nueva implementacion de true damage (ignora resistencias) y aplicacion de BURN como secuela:
- BURN: 8 dmg/tick cada 0.5s × duracion 3s = **48 dmg de BURN total**
- DEVASTADOR_PROJECTILE_DMG = 22 (true damage) + 48 BURN = **70 dmg efectivo por proyectil con BURN**
- Para el Caballero (200 HP): 3 proyectiles con BURN = 210 dmg → letales sin invulnerabilidad
- Para el Mago (70 HP): 1 proyectil con BURN = 70 dmg → muerte potencial

**Problema critico detectado:** Un solo proyectil del Devastador mas el BURN completo puede matar al Mago si el iframe (0.7s) no interrumpe los ticks de BURN. Si BURN aplica aun durante iframes, el Mago es practicamente inviable en el boss final.

**Recomendacion urgente:** Confirmar que el BURN se interrumpe o pausa durante el periodo de invulnerabilidad. Si no, reducir `STATUS_BURN_DAMAGE` de 8 a **5 dmg/tick** para que el BURN total sea 30 dmg — permitiendo al Mago sobrevivir 1 proyectil CAOS (22 + 30 = 52 dmg, sobrevive con 18 HP).

---

## 2. Evaluacion de roles vs enemigos

### Caballero (200 HP, Def 20, melee 22/45 dmg, iframes 0.6s)

> [!success] ROL SOLIDO

**Fortalezas:**
- Counter natural de BASICO, KAMIKAZE y GUARDIAN (sin resist fisica relevante en estos)
- Melee pesado (45 dmg) permite eliminar ELITE_AOE en ~7 hits contra -20% resist magic del Coloso si se usa ataque alternado con Mago — pero el Caballero solo hace dmg fisico, asique necesita ~20 golpes pesados contra el Coloso (45 × 0.50 = 22.5 dmg)
- 200 HP con 0.6s iframes: aguanta swarms mejor que antes (1.0s era demasiado largo, acumulaba hits bloqueados)

**Debilidades:**
- TANQUE (40% resist fisica) es un muro en late game sin upgrades de penetracion
- COLOSO (50% resist fisica) requiere muchos golpes — poco practible en oleadas mixtas

**Veredicto:** BALANCEADO. La reduccion de iframes de 1.0s a 0.6s aumenta la presion en swarms pero es consecuente con un personaje de alto HP. No hay rol roto.

---

### Mago (70 HP, Res Magica 15, bolt 22 dmg / area 55 dmg, mana 120, iframes 0.7s)

> [!warning] ROL EN RIESGO — ALTA PRESION

**Fortalezas:**
- Counter definitivo de TANQUE (-15% resist magic = vulnerable)
- Counter definitivo de COLOSO (-20% resist magic = vulnerable)
- Area 55 dmg excelente para swarms de NORMAL y KAMIKAZE

**Debilidades criticas:**
- 70 HP es el menor del juego. Con iframes reducidos a 0.7s (antes 1.0s), el Mago toma hits mas frecuentes en melee
- KAMIKAZE depth 8 con dmg_mult 2.0: dmg_max = 25 × 2.0 = **50 dmg** → Mago queda en 20 HP de un golpe
- DEVASTADOR CAOS_PRIMORDIAL: 22 true + BURN potencial = riesgo de muerte por 1 proyectil
- ELITE_TP con 30% resist magic convierte al Mago en un rol ineficiente contra este elite

**Veredicto:** NEEDS_TUNING. El Mago funciona si el jugador prioriza objetivos y gestiona distancia, pero en profundidades 7-8 con KAMIKAZE a velocidad 1.17x y BURN del boss, el margen de error es casi nulo. Ver recomendacion de BURN en seccion boss.

---

### Tirador (80 HP, auto-fire 18 dmg, CD 0.16s, rango 600px, iframes 0.45s)

> [!warning] IFRAMES MUY CORTOS

**Fortalezas:**
- DPS automatico: 18 dmg / 0.16s = **112.5 dmg/s** contra enemigos sin resist
- Rango 600px permite eliminar KAMIKAZE antes de que alcance (move_speed 6.5 vs alcance del Tirador)
- Versatil — puede matar ORBITER a distancia antes de que orbite

**Debilidades:**
- 0.45s de iframes es el menor del juego. En swarms densos de BASICO (8 dmg contacto × varios) puede acumular 16-24 dmg/s efectivo si los enemigos estan agrupados
- TANQUE con 25% resist ranged → 18 × 0.75 = **13.5 dmg** → 110 HP / 13.5 = **~8 disparos** — razonable, pero lento para el DPS continuo
- Sin capacidad de escapada en melee (dependiente del movimiento del jugador)

**Veredicto:** NEEDS_TUNING menor. Los 0.45s de iframes crean una penalizacion muy severa en encuentros de contacto involuntarios. Considerar subir a **0.55s** para dar al Tirador un respiro minimo sin eliminar la tension de su bajo HP.

---

## 3. Problemas potenciales detectados

### PROBLEMA 1 — BURN durante invulnerabilidad (CRITICO)

> [!danger] Prioridad Alta

Si `STATUS_BURN_DAMAGE` aplica ticks durante el periodo de invulnerabilidad post-hit, el Mago muere de 1 proyectil del Devastador (22 true + hasta 48 BURN = 70 dmg = HP total del Mago). Confirmar comportamiento en `ColisionManager` o el sistema de efectos de estado.

**Fix:** O pausar BURN durante iframes, o reducir `STATUS_BURN_DAMAGE` de 8 a **5 dmg/tick** (total BURN = 30 dmg).

---

### PROBLEMA 2 — TANQUE demasiado blando para el Mago

> [!warning] Prioridad Media

Con 110 HP y -15% resist magic (vulnerable), el Mago elimina al TANQUE en **~4 bolts** (22 × 1.15 = 25.3 × 4 = 101 dmg, mas el 5to bolt para los 9 HP restantes). Esto hace al TANQUE trivial para el Mago, lo que rompe el diseño de "el TANQUE es el enemigo mas duro". Un jugador Mago no siente la amenaza del TANQUE.

**Fix:** Subir HP del TANQUE a **145** (recomendado en seccion 1) y considerar agregar `DMG_MAGIC resist_pct: 0` (neutral, removiendo la vulnerabilidad) — dejando la vulnerabilidad solo como ventaja tactica para el Mago avanzado con upgrades.

---

### PROBLEMA 3 — ORBITER a 35 HP es casi un one-shot contextual

> [!warning] Prioridad Baja

El Orbiter a 35 HP puede caer de 1 bolt del Mago con minimo upgrade de daño (+15% → 22 × 1.15 = 25.3, casi 35). Pierde identidad como "amenaza persistente" y se convierte en farmeable.

**Fix:** Subir HP de 35 a **45** (recomendado en seccion 1).

---

### PROBLEMA 4 — ARQUERO minijefe (wave 20) trivializado

> [!warning] Prioridad Media

ARQUERO_HP = 300 en wave 20 con jugadores ya empoderados es insuficiente para sentir el minijefe. El Tirador tiene potencial DPS de 112 dmg/s → el Arquero cae en **~2.7 segundos** de DPS sostenido. Un minijefe no deberia durar menos de 8-10 segundos incluso con el mejor rol.

**Fix:** Subir `ARQUERO_HP` a **380** en `Constants.java` (pendiente de implementar).

---

### PROBLEMA 5 — Elite chance depth 8 (32%) con swarms grandes

> [!note] Prioridad Baja — Monitorear

En depth 8 con elite_chance_pct = 32%, en oleadas de 15+ enemigos, estadisticamente habra **~5 elites simultaneos**. Dos COLOSOS juntos (5 AoE sismicas coordindas) pueden ser un wipe instantaneo para Mago y Tirador. No es un bug pero puede crear picos de dificultad no intencionales.

**Fix:** Considerar un techo de 2-3 elites simultaneos por oleada (logica de spawn cap, no cambio de stats).

---

## 4. Recomendaciones de ajuste — valores concretos

| # | Archivo | Campo | Valor actual | Valor propuesto | Prioridad |
|---|---------|-------|-------------|-----------------|-----------|
| 1 | `Constants.java` | `STATUS_BURN_DAMAGE` | 8 | **5** (si BURN no respeta iframes) | CRITICA |
| 2 | `enemies.json` | `EN_TANK.hp` | 110 | **145** | ALTA |
| 3 | `enemies.json` | `EN_ORBITER.hp` | 35 | **45** | BAJA |
| 4 | `enemies.json` | `EN_ELITE_AOE.dmg_max` | 22 | **18** | MEDIA |
| 5 | `Constants.java` | `ARQUERO_HP` | 300 | **380** | MEDIA |
| 6 | `Constants.java` | `INVULNERABILITY_SHOOTER` | 0.45 | **0.55** | BAJA |
| 7 | SpawnManager | elite_cap_per_wave | (ninguno) | **3 max** | BAJA |

> [!note] Nota sobre el alcance
> Los ajustes 1, 5, 6 requieren editar `Constants.java`. Los ajustes 2, 3, 4 requieren editar `enemies.json`. El ajuste 7 es logica nueva en SpawnManager. Ninguno de estos cambios esta dentro del scope de este documento — registrar como tareas en el backlog de Sprint 11.

---

## 5. Veredicto overall

> [!warning] NEEDS_TUNING

El sistema de combate es **funcionalmente jugable** para Caballero y Tirador. El Mago presenta riesgo de romper la experiencia en late game (depth 7-8) si BURN no respeta iframes — este es el unico punto critico real. Los demas ajustes son refinamientos de balance que mejoran la consistencia pero no bloquean el progreso.

**Prioridades inmediatas:**
1. Confirmar comportamiento de BURN durante invulnerabilidad — si no respeta iframes, aplicar fix de `STATUS_BURN_DAMAGE = 5`
2. Subir `EN_TANK.hp` de 110 a 145 — restaura la identidad del TANQUE como amenaza real
3. Subir `ARQUERO_HP` de 300 a 380 — el minijefe de wave 20 necesita duracion digna

**El juego NO esta en estado CRITICAL_ISSUES** — el loop principal funciona y todos los roles son viables con el conocimiento correcto. Los problemas son de refinamiento, no de diseño roto.

---

*Generado: 2026-05-20 | Reviewer: Game Designer | Sprint 10 post-cambios*
*Referencias: [[enemies.json]] · [[enemyresistances.json]] · [[Constants.java]] · [[depthscaling.json]]*

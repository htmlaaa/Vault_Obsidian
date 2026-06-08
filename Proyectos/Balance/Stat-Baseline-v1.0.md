---
title: Stat Baseline v1.0 — Valores Exactos del Código
tags:
  - balance
  - stats
  - fuente-verdad
  - v1-0
date: 2026-06-06
status: verificado
aliases:
  - Stat Baseline
  - Valores exactos
---

# Stat Baseline v1.0 — Kaosuarina

> [!important] Fuente de verdad
> Valores extraídos directamente de `Role.java`, `Constants.java` y `Enemy.java` el 2026-06-06.
> Cualquier discrepancia con documentos anteriores se resuelve a favor de este archivo.

---

## Roles (fuente: `roles/Role.java`)

| Rol | HP | Def | ResMag | Maná | Regen | Vel | Modo ataque |
|-----|---:|----:|-------:|-----:|------:|----:|-------------|
| **Caballero** | 200 | 20 | 0 | 40 | 0/s | 250 px/s | Melee manual (clic) |
| **Mago** | 85 | 5 | 15 | 120 | **2/s** | 270 px/s | Magia manual (clic) |
| **Tirador** | 80 | 5 | 0 | 30 | 0/s | 340 px/s | Auto-disparo |

### Ataques manuales — Caballero

| Ataque | Daño | Coste | Cooldown |
|--------|-----:|------:|---------:|
| Ligero (melee arco) | 38 | 0 | 0.20 s |
| Pesado (melee arco amplio) | 72 | 0 | 1.0 s |

### Ataques manuales — Mago

| Ataque | Daño | Coste | Cooldown |
|--------|-----:|------:|---------:|
| Bolt ligero (auto-aim) | 34 | 8 maná | 0.28 s |
| Blast pesado (AoE) | 85 | 35 maná | 1.5 s |

### Tirador — auto-shoot

| Parámetro | Valor |
|-----------|------:|
| Cooldown base | 0.16 s |
| Balas base | 2 (spread ±12°) |
| Damage type | A_DISTANCIA |

---

## Enemigos base (fuente: `entities/Enemy.java`, `utils/Constants.java`)

### Tipos básicos (Sprints 1-3)

| Tipo | HP | Vel | Def | ResMag | Contacto | XP | Especial |
|------|---:|----:|----:|-------:|---------:|---:|---------|
| BASICO | 30 | 150 | 0 | 0 | 10 | 25 | Persecución directa |
| RAPIDO | 12 | 300 | 0 | 0 | 10 | 25 | Persecución directa veloz |
| TANQUE | 180 | 80 | 20 | 0 | 10 | 25 | Persecución lenta, alta def |
| SHOOTER | 50 | 100 | 0 | 0 | 10 | 25 | Kite; dispara cada 2s |
| MALDITO | 45 | 130 | 0 | 0 | 10 | 25 | Explosión veneno al morir |
| ESPECTRAL | 35 | 160 | 0 | 10 | 7 | 25 | Inmune a FISICO; +50% dmg FUEGO |

### Tipos Sprint 4

| Tipo | HP | Vel | Def | Contacto | Especial |
|------|---:|----:|----:|---------:|---------|
| BERSERKER | ~60 | 160→420 | 0 | 10 | <20% HP → rage (vel 420) |
| SPLITTER | ~30 | 140 | 0 | 10 | Al morir → 2 BASICO |
| HEALER | ~40 | 90 | 0 | 8 | Cura aliados r=150u cada 1s |
| SHIELDER | ~70 | 70 | 35 | 12 | Alta def, muy lento |
| ELITE_CHARGE | ~80 | variable | 0 | 18 | Orbit → telegraph 1.5s → charge 800f |
| ELITE_SUMMON | ~90 | 80 | 0 | 10 | Invoca 3 BASICO cada 8s |
| ELITE_ZONE | ~100 | 0 | 0 | 10 | Estacionario; slow 45% en r=120u |

### Bosses (fuente: `utils/Constants.java`)

| Boss | HP | Vel | Def | ResMag | Contacto | Wave | Especial |
|------|---:|----:|----:|-------:|---------:|-----:|---------|
| **Guardián** | **900** | 60 | 30 | 10 | 25 | 10, 20, 30… | Shockwave r=150 dmg=20 cada 3s; fase 2 al 50% HP |
| **Fragmentado** | 1600 | 80/110/140 | 0 | 0 | 22 | 30 | 3 fases (67%/33% HP); vel escala por fase |
| Devastador | 2200 | 60/100 | 0 | 0 | — | 50 | Boss final — condición de victoria |

> [!warning] Corrección histórica
> El Guardián apareció con HP=800 en Sprint 6. **Sprint 10 lo subió a 900** (`GUARDIAN_HP` en `Constants.java`).
> Los documentos de sesiones anteriores a Sprint 10 que mencionan "HP 800" son históricamente correctos para su momento.

---

## Proyectiles (fuente: `entities/Bullet.java`, `entities/EnemyBullet.java`)

| Tipo | Velocidad | Rango | Radio | Daño |
|------|----------:|------:|------:|-----:|
| Bullet (jugador) | 600 u/s | 1500 u | 7 | según arma |
| EnemyBullet | 200 u/s | — | 6 | 8 |

---

## Upgrades (fuente: `systems/UpgradeManager.java`)

### Multiplicadores globales

| Upgrade | Fórmula | Nivel máx |
|---------|---------|:---------:|
| Daño +40% | `1 + 0.40 × nivel` | 5 |
| Cadencia +15% | `1 + 0.15 × nivel` | 5 |
| Velocidad +10% | `1 + 0.10 × nivel` | 5 |
| Vida Máx +20 | +20 HP por nivel | 3 |
| Vampirismo | 3% lifesteal (`LIFESTEAL_BASE_PERCENT`) | 1 |

### Sinergias activas (4)

| Sinergia | Condición | Efecto |
|----------|-----------|--------|
| Crítico Encadenado + Daño≥3 | Ambos activos | +10% daño adicional |
| Vampirismo + Vida Máx≥3 | Ambos activos | Lifesteal en kill radial |
| Bala Extra + Perforación | Ambos activos | Pierce bonus = niveles de Bala Extra |
| Cuchilla Venenosa + Cadencia≥3 | Ambos activos | Poison stacks ×2 |

Total upgrades: **29** (7 genéricos, 3 Tirador, 2 Caballero, 3 Mago + 14 CNT-03).

---

## Dificultad (fuente: `screens/Difficulty.java`)

| Dificultad | Depth inicial | Elites desde | Intensidad spawn |
|------------|:------------:|:------------:|:----------------:|
| NORMAL | 1 | Oleada 4 | ×1.0 |
| BRUTAL | 3 | Oleada 1 | ×1.0 |
| CAOS | 5 | Oleada 1 | ×1.2 |

---

## Economía de maná (todas las fuentes)

| Fuente | Cantidad | Quién | Condición |
|--------|----------:|-------|-----------|
| Kill base | +5 | Todos | Cualquier kill |
| Sed de Sangre (amuleto) | +5 extra | Todos | Si equipado |
| Overkill | `floor(exceso / 20)` | Todos | Daño > HP restante |
| Cicatriz de Combate | `floor(dmgRecibido / 4)` | Solo Caballero | Al expirar i-frame |
| Regen pasiva | 2/s | Solo Mago | Pasivo siempre |
| Guardián de la Arena (amuleto) | +20 | Todos | Oleada sin daño |

---

## Evolución de armas (fuente: `weapons/WeaponEvolutionCatalog.java`)

| Arma base (×2) | Evolución | Tipo |
|----------------|-----------|------|
| W_SHORTSWORD | W_LONGSWORD | FÍSICO |
| W_DUAL_PISTOLS | W_CUATRO_PISTOLAS | A_DISTANCIA |
| W_APP_STAFF | W_ARCANE_CANON | MÁGICO |
| W_FLAMEBLADE | W_INFERNO_BLADE | FUEGO |
| W_HUNTBOW | W_WARBOW | A_DISTANCIA |
| W_CHAOS_WAND | W_VOID_CANNON | CAOS_PRIMORDIAL |

Mecánica: recoger el mismo `weapon_id` ya equipado → evolución automática. La arma hereda la inscripción.

---

## Meta-progresión (fuente: `db/DBManager.java`)

**Fórmula tokens por run:** `max(1, score/200 + level×2 + wavesCompleted)`

Almacenados en tabla `meta_tokens` SQLite — fila única, acumulativa entre runs.

---

## Amuletos (12 tipos, 3 slots)

| Amuleto | Efecto |
|---------|--------|
| Collar Vampírico | +lifesteal |
| Sed de Sangre | +5 maná por kill extra |
| Guardián de la Arena | +20 maná al cerrar oleada sin daño |
| Tótem de Regen | +HP regen/s |
| Amuleto de Velocidad | +speed bonus |
| Amuleto de Maná | +maxMana bonus |
| Amuleto Crítico | +8% crit |
| Amuleto de Explosión | Crítico letal → explosión AoE |
| Amuleto de Espectros | Cada 10 kills → espectro aliado |
| Amuleto del Tiempo | Slow global 1×/45s |
| Amuleto de Armadura | +15 DEF, −10% vel |
| Amuleto de Daño | +daño flat/pct |

---

## Inscripciones (8 tipos, fuente: `weapons/inscriptions/`)

| Código | Clase | Efecto |
|--------|-------|--------|
| IGN | InscripcionIgnea | Aplica BURN 3s al impactar |
| VAM | InscripcionVampirica | Roba HP = max(1, rawDmg/10) |
| ECO | InscripcionDelEco | Echo 40% dmg con delay 2s |
| VAC | InscripcionDeVacio | Bala ignora defensa |
| SIS | InscripcionSismica | Stun 0.3s al impactar |
| CAO | InscripcionDelCaos | Random: BURN / POISON / +5 HP / +5 maná |
| RES | InscripcionResonante | Stacks (max 10) en mismo objetivo → dmg +5%/stack |
| ESP | InscripcionEspectral | +1 pierce sin efecto onHit |

---

## Resumen del contenido v1.0

| Métrica | Valor |
|---------|------:|
| Roles jugables | 3 |
| Armas en pool de drops | 30 (24 base + 6 evolucionadas) |
| Tipos de enemigo | 17 |
| Bosses | 2 activos (Guardián oleada 10, Fragmentado oleada 30) + Devastador (oleada 50, pendiente) |
| Upgrades | 29 |
| Amuletos | 12 |
| Inscripciones | 8 |
| Cadenas de evolución | 6 |
| Dificultades | 3 (Normal / Brutal / Caos) |
| Slots amuleto | 3 |
| Slots arma | 2 activos + 4 storage |

---

## Referencias

- [[../Kaosuarina]] — GDD principal con historial de versiones
- [[../../../Kausarina_GAME/Balance-Review-Sprint10]] — análisis de balance sprint 10
- [[../../Proyectos/Balance/Balance-v1]] — primer pase de balance (invulnerabilidad, lifesteal, daños de rol)
- [[../../Sesiones/Sesion-2026-06-06-Refactor-Ingles]] — sesión que originó este documento

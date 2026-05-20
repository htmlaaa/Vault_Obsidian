---
title: Guía — Agentes, Skills y PixelLab en Kaosuarina
tags:
  - guia
  - workflow
  - agentes
  - pixellab
aliases:
  - Guía Agentes
  - Workflow IA
date: 2026-05-06
---

# Agentes, Skills y PixelLab — Guía Práctica

Cómo usar el framework Code_Game y PixelLab MCP de forma efectiva en este proyecto.

---

## Cuándo usar agentes vs. hacerlo directo

| Situación | Acción |
|---|---|
| Tarea puntual (borrar dir, lanzar build, fix pequeño) | Directo — sin agente |
| Decisión de diseño (paleta, mecánica, sistema) | Spawnar agente → pedir conclusiones → confirmar |
| Generar sprites (PixelLab) | Directo con MCP tools, pero con descripción validada por art-director |
| Implementación Java/LibGDX | Directo o `gameplay-programmer` si son varios archivos |
| GDD de sistema nuevo | `game-designer` + skill `/design-system` |

> [!warning] Protocolo de confirmación
> Siempre pedir conclusiones del agente **antes** de generar sprites o escribir código. El agente propone → tú confirmas o cambias → se ejecuta.

---

## Agentes más útiles para Kaosuarina

### `art-director`
Decide paletas, conceptos visuales, estilo pixel art. Úsalo cuando:
- Hay un personaje nuevo o enemigo a diseñar
- No sabes qué colores usar
- Quieres coherencia visual entre assets

### `game-designer` / `systems-designer`
Diseña sistemas completos (cores, sinergias, roles). Úsalo cuando:
- Hay un sistema nuevo que necesita GDD
- Quieres explorar opciones de mecánica antes de implementar

### `gameplay-programmer`
Implementa en Java/LibGDX siguiendo la arquitectura del proyecto. Úsalo cuando:
- El cambio afecta a 2+ archivos
- Necesitas que respete los patrones (object pool, ColisionManager, Constants.java)

> [!tip] Tier de modelos
> - **Haiku** → checks rápidos, status, formateo
> - **Sonnet** → diseño, implementación (default)
> - **Opus** → revisiones cross-sistema, GDDs múltiples

---

## Skills más usadas

| Skill | Cuándo usarla |
|---|---|
| `/brainstorm` | Explorar ideas desde cero para un sistema |
| `/design-system` | Escribir el GDD completo de un sistema (cores, sinergias, etc.) |
| `/art-bible` | Definir identidad visual completa del juego |
| `/asset-spec` | Generar specs y prompts para assets desde el GDD |
| `/dev-story` | Implementar una historia de usuario concreta |
| `/obsidian-markdown` | Crear/editar notas en el Vault con formato correcto |

> [!example] Flujo diseño → implementación
> `/brainstorm` (ideas) → `/design-system` (GDD) → `/dev-story` (código)

---

## Flujo PixelLab MCP

```
1. Validar descripción visual con art-director (agente)
2. Llamar create_character / create_object con los params correctos
3. Esperar 2-5 minutos (async — el job sigue en background)
4. Llamar get_character / get_object para ver estado
5. Descargar PNG desde URL del resultado
6. Guardar en A_Game_Kaosuarina/PIXEL/characters/[nombre]/
```

### Parámetros clave — personajes

| Param | Valor para Kaosuarina |
|---|---|
| `size` | **64** (64×64 px) |
| `n_directions` | 4 (south/west/east/north) |
| `view` | `low top-down` |
| `outline` | `single color black outline` |
| `shading` | `medium shading` |
| `proportions` | `chibi` (legibilidad en pantalla) |

> [!danger] Tamaño fijo: 64px
> Siempre `size: 64`. Los personajes actuales son 64×64. No cambiar sin actualizar también el código de renderizado en `Player.java`.

### Variaciones con `vary_object`
Para generar variantes de un sprite ya existente (estado dañado, alternativa de color) usa `vary_object` pasando el ID del original + `edit_description` describiendo el cambio.

---

## El Vault como fuente de verdad

Antes de diseñar cualquier cosa, revisar:

- [[Kaosuarina]] — GDD principal (mecánicas, roles, enemigos, backlog)
- [[Kaosuarina-Progreso]] — estado actual, tareas en progreso, pendientes
- `Kausarina_Vault/Personajes/` — diseños de personaje ya definidos
- `Kausarina_Vault/Pendiente/` — notas de sesiones anteriores con bugs y tareas abiertas

> [!note] Referencia cruzada
> El Mago ya tiene diseño completo en [[Mago-Kaos]]. Antes de generar su sprite, usa ese prompt. No reinventar lo que ya está diseñado.

---

## Resumen rápido

```
Nueva mecánica    → /brainstorm → /design-system → /dev-story
Nuevo sprite      → art-director (conclusiones) → confirmar → PixelLab MCP
Fix de código     → directo en Java (gameplay-programmer si son 3+ archivos)
Duda de diseño    → leer Vault primero, luego agente si hace falta
```

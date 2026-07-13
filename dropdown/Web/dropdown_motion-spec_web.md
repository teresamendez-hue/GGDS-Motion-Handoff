---
component: Dropdown
component_slug: dropdown
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
---

## Descripción

Panel flotante anclado a Button o Icon Button. Lista de acciones (List) o slot custom. Abre con fade + translateY; cierra con motion-exit o instantáneo al seleccionar. El panel crece en ancho según el contenido — sin scroll horizontal. Submenú / Second Level excluidos del handoff.

## Layout

- width: max-content (panel crece con el contenido)
- min-width: según diseño (ej. 160px)
- overflow-x: visible — sin scroll horizontal
- overflow-y: auto solo en listas largas — sin animación de scroll
- items: white-space: nowrap

## Tokens

### Entrada panel
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida suave
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-100
- duration_ms: 100

### Salida forzada (selección ítem)
- semantic: none
- duration_ms: 0

### Estados ítem
- semantic: motion-curve-sm
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Panel — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |
| translateY | -4px | 0 | motion-curve-sm |

### Panel — Salida suave
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0 | -4px | motion-exit |

### Panel — Salida forzada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | none (instantáneo) |
| translateY | 0 | -4px | none (instantáneo) |

### Ítem — Hover / pressed / focus
| propiedad | de | a | token |
|---|---|---|---|
| background-color | enabled | hover/pressed | motion-curve-sm |
| opacity (ring) | 0 | 1 | motion-curve-sm |

## Triggers

### Apertura panel
- evento: click en trigger (toggle)
- tipo: user-action
- delay_ms: 0

### Cierre suave
- evento: click fuera, Escape, segundo click en trigger
- tipo: user-action
- auto_delay_ms: null

### Cierre forzado
- evento: click en ítem (selección)
- tipo: user-action
- duration_ms: 0

## Variantes

- trigger: [Icon Button, Button]
- alignment: [Left, Center, Right]
- dropdown: [List, Slot]
- item_type: [Item, Header]
- excluido: [Second Level, Submenú con Action]
- nota: variantes no alteran tokens; trigger usa handoffs Button / Icon Button

## Estados intermedios

Omitir.

## Coherencia sistémica

- componentes_relacionados: [Button, Icon Button, Tooltip]
- token_mismo_que: [Pagination estados ítem, Tooltip salida forzada]
- razon: panel flotante Default con salida forzada en selección; ítems comparten motion-curve-sm con controles de lista

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica

## Referencias

- material_design: https://m3.material.io/components/menus/overview
- apple_hig: no aplica
- tokens_file: references/tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=10025-9193

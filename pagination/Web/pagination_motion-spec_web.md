---
component: Pagination
component_slug: pagination
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Nehuén Benitez
date: 2026-07-02
design_system: GGDS
status: draft
preview_type: internal_state
---

## Descripción

Control de paginación Web compuesto por Navigation (prev, next, ellipsis) y Number (celdas). Motion de estado interno: hover/pressed/focus en Navigation; transición Selected en Number. Rearmado de ventana +4 instantáneo. Sin viewport enter/exit. Sin skeleton.

## Tokens

### Estados internos / selección
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida viewport
- semantic: no aplica

## Propiedades animadas

### Navigation — Hover / Pressed / Focus
| propiedad | de | a | token |
|---|---|---|---|
| background-color | enabled / hover | hover / pressed | motion-curve-sm |
| opacity (focus ring) | 0 | 1 | motion-curve-sm |

### Number — Cambio Selected
| propiedad | de | a | token |
|---|---|---|---|
| background-color | enabled / selected | selected / enabled | motion-curve-sm |
| color | on-control / on-control-selected | inverso | motion-curve-sm |

### Ventana +4 — Rearmado
| propiedad | de | a | token |
|---|---|---|---|
| contenido visible | set anterior | set nuevo | sin animación |

## Triggers

### Hover / Pressed / Focus Navigation
- evento: pointer enter/leave, pointer down/up, focus-visible
- tipo: user-action
- delay_ms: 0

### Cambio de página (Number)
- evento: click en número
- tipo: user-action
- delay_ms: 0

### Rearmado ventana
- evento: prev, next, ellipsis en modo +4
- tipo: user-action
- animacion: ninguna (swap instantáneo)

### Disabled
- evento: primera / última página
- tipo: programmatic
- animacion: ninguna

## Estados intermedios

### Navigation
- valores: Enabled, Hovered, Pressed, Focused, Disabled

### Number
- valores: Enabled, Selected

### Pagination (variante)
- items: 1, 2, 3, 4, +4
- position: Start, Middle, End

## Coherencia sistémica

- componentes_relacionados: [Breadcrumb, Icon Button, Link]
- token_mismo_que: [Breadcrumb estados, Button estados]
- razon: microinteracción de control de navegación; motion-curve-sm alineado con componentes Default

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición
- aria: aria-current en página activa; labels en prev/next

## Referencias

- material_design: https://m3.material.io/components/pagination/overview
- apple_hig: no aplica
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=7959-66

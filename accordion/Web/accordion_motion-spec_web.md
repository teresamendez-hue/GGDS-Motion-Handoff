---
component: Accordion
component_slug: accordion
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Nehuén Benitez
date: 2026-06-30
design_system: GGDS
status: draft
preview_type: internal_state
---

## Descripción

Ítem de acordeón único en flujo de layout. Expand/collapse del slot con **grid 0fr↔1fr + opacity inner + chevron rotate 180deg**. Sin entrada/salida del viewport. Header con estados hover/pressed/focus. Cambio Type Default/Container sin animación.

## Tokens

### Expand / collapse
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Header estados
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida viewport
- semantic: no aplica
- nota: sin motion-exit

## Propiedades animadas

### Slot — Expand
| propiedad | de | a | token |
|---|---|---|---|
| grid-template-rows (body) | 0fr | 1fr | motion-curve-sm |
| opacity (body inner) | 0 | 1 | motion-curve-sm |
| transform (chevron) | 0deg | 180deg | motion-curve-sm |

### Slot — Collapse
| propiedad | de | a | token |
|---|---|---|---|
| grid-template-rows (body) | 1fr | 0fr | motion-curve-sm |
| opacity (body inner) | 1 | 0 | motion-curve-sm |
| transform (chevron) | 180deg | 0deg | motion-curve-sm |

### Header — Estados
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hovered | motion-curve-sm |
| transform / opacity | 1, 1 | pressed scale | motion-curve-sm |

## Triggers

### Expand
- evento: tap en header (collapsed → expanded)
- tipo: user-action
- delay_ms: 0

### Collapse
- evento: tap en header (expanded → collapsed)
- tipo: user-action
- delay_ms: 0

### Cambio Type
- evento: swap Default ↔ Container
- tipo: programmatic
- animacion: ninguna

## Estados intermedios

### Expanded
- trigger: tap header o programmatic
- propiedades: grid 1fr, opacity 1, chevron 180deg
- token: motion-curve-sm

### Collapsed
- trigger: tap header o programmatic
- propiedades: grid 0fr, opacity 0, chevron 0deg
- token: motion-curve-sm

## Coherencia sistémica

- componentes_relacionados: [Inline Message (variante Expand), Left Menu]
- token_mismo_que: [Inline Message expand, Button estados, Switch]
- razon: microinteracción de reveal/collapse; categoría Default con motion-curve-sm

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica

## Referencias

- material_design: https://m3.material.io/components/lists/guidelines#expandable-lists
- apple_hig: no aplica
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=10180-45

---
component: Button
component_slug: button
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-05
design_system: GGDS
status: draft
---

## Descripción

Botón con estados internos animados para feedback de interacción. No tiene transición de entrada/salida de viewport.

## Tokens

### Estados internos
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Button — hover
| propiedad | de | a | token |
|---|---|---|---|
| background-color | default | hover | motion-curve-sm |

### Button — pressed
| propiedad | de | a | token |
|---|---|---|---|
| transform | scale(1) | scale(0.98) | motion-curve-sm |

### Focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

### Loading / Skeleton
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0/1 | 1/0 | motion-curve-sm |

## Triggers

### hover
- evento: mouseenter / mouseleave
- tipo: user-action

### pressed
- evento: mousedown / mouseup
- tipo: user-action

### focus
- evento: focus-visible
- tipo: user-action

### loading
- evento: set is-loading
- tipo: programmatic

### skeleton
- evento: set is-skeleton / resolve
- tipo: programmatic

### disabled
- evento: disabled true
- tipo: programmatic
- note: sin transición

## Coherencia sistémica

- componentes_relacionados: [Action Link]
- token_mismo_que: [Action Link estados internos]
- razon: Default microinteractions en controles de acción

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- material_design: https://m3.material.io/
- tokens_file: references/tokens.md

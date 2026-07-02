---
component: Tabs
component_slug: tabs
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
semantic_token_indicator: motion-curve-md
owner: Ezequiel Arguello
date: 2026-07-02
design_system: GGDS
status: draft
---

## Descripción

Control de navegación/selección entre grupos de contenido del mismo nivel jerárquico, con variantes Segmented Item y Line Item. Usa motion de estados internos, sin enter/exit de viewport.

## Tokens

### Estados internos (Hovered, Pressed, Focused, fade de color)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Indicador de selección (movimiento espacial)
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

## Propiedades animadas

### Tab Item (Segmented / Line) — Hovered
| propiedad | de | a | token |
|---|---|---|---|
| border-color | Enabled | Hovered | motion-curve-sm |
| background-color | Enabled | Hovered | motion-curve-sm |

### Tab Item — Pressed
| propiedad | de | a | token |
|---|---|---|---|
| background-color | Hovered | Pressed | motion-curve-sm |

### Focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

### Indicador de selección — Segmented (fondo relleno)
| propiedad | de | a | token |
|---|---|---|---|
| transform (translateX) | posición anterior | posición del tab activo | motion-curve-md |
| width | ancho anterior | ancho del tab activo | motion-curve-md |

### Indicador de selección — Line (línea inferior)
| propiedad | de | a | token |
|---|---|---|---|
| transform (translateX) | posición anterior | posición del tab activo | motion-curve-md |
| width | ancho anterior | ancho del tab activo | motion-curve-md |

### Label / ícono — cambio de selección
| propiedad | de | a | token |
|---|---|---|---|
| color | control | control-selected | motion-curve-sm |
| font-weight | Regular | Semi Bold | motion-curve-sm |

## Triggers

### hover
- evento: pointer enter/leave
- tipo: user-action

### pressed
- evento: pointer down/up
- tipo: user-action

### focus
- evento: focus-visible
- tipo: user-action

### selected
- evento: click/tap en tab o navegación por teclado (flechas + Enter)
- tipo: user-action
- delay_ms: 0

### disabled
- evento: disabled true
- tipo: programmatic
- note: sin transición

## Estados intermedios

Ninguno — Tabs no tiene loading/error propios; el contenido asociado maneja sus propios estados.

## Coherencia sistémica

- componentes_relacionados: [Button, Box Selector]
- token_mismo_que: [Button (estados internos)]
- razon: Hovered/Pressed/Focused usan el mismo token de microinteracción que Button para coherencia entre controles frecuentes; el indicador usa un token de categoría distinta (guía) por ser desplazamiento espacial, no color plano.

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición, incluyendo el indicador

## Referencias

- material_design: https://m3.material.io/components/tabs
- apple_hig: no aplica
- tokens_file: references/tokens-motion.md

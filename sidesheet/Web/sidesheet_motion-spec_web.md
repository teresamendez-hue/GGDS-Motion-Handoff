---
component: Sidesheet
component_slug: sidesheet
platform: web
category: communication
semantic_token_enter: motion-curve-lg
semantic_token_exit: motion-exit
owner: Teresa Mendez
date: 2026-06-05
design_system: GGDS
status: draft
---

## Descripción

Panel lateral que aparece desde el borde derecho con scrim. Incluye variantes overlay desktop y full-screen mobile web.

## Tokens

### Entrada
- semantic: motion-curve-lg
- primitive_curve: easing-emphasis
- css_value: cubic-bezier(0.2, 0, 0, 1)
- primitive_duration: duration-500
- duration_ms: 500

### Salida
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-300
- duration_ms: 300

## Propiedades animadas

### Sidesheet — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| transform | translateX(100%) | translateX(0) | motion-curve-lg |
| opacity | 0 | 1 | motion-curve-lg |

### Scrim — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-lg |

### Sidesheet — Salida
| propiedad | de | a | token |
|---|---|---|---|
| transform | translateX(0) | translateX(100%) | motion-exit |
| opacity | 1 | 0 | motion-exit |

### Scrim — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |

## Triggers

### Apertura
- evento: apertura del sidesheet
- tipo: user-action
- delay_ms: 0

### Cierre
- evento: cierre del sidesheet
- tipo: user-action
- auto_delay_ms: null

## Coherencia sistémica

- componentes_relacionados: [Drawer, Modal]
- token_mismo_que: [Drawer]
- razon: componente de navegación/panel lateral en categoría guide

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- material_design: https://m3.material.io/
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/animation
- tokens_file: references/tokens.md

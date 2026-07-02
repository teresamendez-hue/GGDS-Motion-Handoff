---
component: Carousel
component_slug: carousel
platform: web
category: guide
semantic_token_enter: motion-curve-md
semantic_token_exit: null
owner: Ezequiel Arguello
date: 2026-07-02
design_system: GGDS
status: draft
---

## Descripción

Carousel navega contenido relacionado en un espacio horizontal limitado mediante un track de slides. No tiene entrada/salida de viewport; el motion es reposicionamiento interno del track más microinteracciones de nav reveal y dot indicator.

## Tokens

### Slide transition
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Microinteracciones (nav reveal / dot indicator)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Carousel track — slide siguiente
| propiedad | de | a | token |
|---|---|---|---|
| transform | translateX(-n·100%) | translateX(-(n+1)·100%) | motion-curve-md |

### Carousel track — slide anterior
| propiedad | de | a | token |
|---|---|---|---|
| transform | translateX(-n·100%) | translateX(-(n-1)·100%) | motion-curve-md |

### Nav — reveal
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

### Dot indicator — activo
| propiedad | de | a | token |
|---|---|---|---|
| background-color | indicator | indicator-active | motion-curve-sm |

## Triggers

### Cambio de slide
- evento: click en flecha o dot
- tipo: user-action
- delay_ms: 0

### Nav reveal
- evento: mouseenter / mouseleave sobre el carousel
- tipo: user-action
- delay_ms: 0

### Disabled (Normal)
- evento: index alcanza 0 o total-1
- tipo: programmatic
- auto_delay_ms: null

## Estados intermedios

### Disabled (solo Behavior Normal)
- trigger: llegada al primer o último slide
- propiedades: pointer-events, background-color (estado final, sin transición)
- token: ninguno (instantáneo)

## Coherencia sistémica

- componentes_relacionados: [Tabs, Drawer, Sidesheet]
- token_mismo_que: [Drawer, Sidesheet]
- razon: el slide es navegación guiada dentro de un espacio contenido, misma categoría que Drawer/Sidesheet (motion-curve-md); el reveal de nav y el dot indicator son microinteracciones de estado, mismo criterio que Button (motion-curve-sm)

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: "https://m3.material.io/"
- apple_hig: "no aplica"
- tokens_file: "references/tokens-motion.md"

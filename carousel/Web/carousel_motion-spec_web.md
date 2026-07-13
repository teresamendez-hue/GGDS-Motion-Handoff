---
component: Carousel
component_slug: carousel
platform: web
category: guide
semantic_token_enter: motion-curve-md
semantic_token_exit: null
owner: Ezequiel Arguello
date: 2026-07-08
design_system: GGDS
status: draft
preview_type: internal_state
variants: [infinite, finite]
---

## Descripción

Carousel horizontal en modo peek con dos variantes: infinite (loop continuo) y finite (paginación con extremos). El track se desplaza por swipe, flecha o dot. Sin autoplay ni enter/exit de viewport. Motion de flechas documentado en Icon Button.

## Tokens

### Cambio de slide
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0.0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Dots (indicador activo)
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

## Propiedades animadas

### Track — slide change (infinite y finite)
| propiedad | de | a | token |
|---|---|---|---|
| transform | translateX(slide N) | translateX(slide N±1) | motion-curve-md |

### Track — clone reset (solo infinite)
| propiedad | de | a | token |
|---|---|---|---|
| transform | clone position | real slide equivalent | ninguno |
| transition | — | none | — |

### Dot — active
| propiedad | de | a | token |
|---|---|---|---|
| transform | scale(1) | scale(1.25) | motion-curve-sm |
| opacity | 0.45 | 1 | motion-curve-sm |

## Triggers

### slide_change
- evento: swipe, click flecha, click dot
- tipo: user-action
- variantes: [infinite, finite]

### dot_active
- evento: cambio de slide activo
- tipo: programmatic

### infinite_loop_reset
- evento: transición termina en slide clonado (primero o último duplicado)
- tipo: programmatic
- variantes: [infinite]
- note: aplicar transform al slide real con transition none

## Estados intermedios

### peek
- trigger: render inicial
- propiedades: slides adyacentes parcialmente visibles
- token: ninguno

### finite_boundary
- trigger: primer o último slide alcanzado
- propiedades: flechas disabled (Icon Button)
- token: ninguno en carousel
- variantes: [finite]

## Coherencia sistémica

- componentes_relacionados: [Sidesheet, Tabs, Drawer, Icon Button]
- token_mismo_que: [Sidesheet — navegación espacial]
- razon: transición guiada entre vistas usa categoría Guía; dots usan Default como Button

## Accesibilidad

- prefers_reduced_motion: salto instantáneo al slide destino sin transición del track ni dots

## Referencias

- material_design: https://m3.material.io/components/carousel
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/carousel-views
- tokens_file: references/tokens-motion.md
- icon_button_handoff: icon-button (motion de flechas)

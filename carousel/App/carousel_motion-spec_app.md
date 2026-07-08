---
component: Carousel
component_slug: carousel
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Ezequiel Arguello
date: 2026-07-08
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: carousel_motion-handoff_web.html
web_parity_token: motion-curve-md
variants: [infinite, finite]
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onDotTapped
---

## Descripción

Carousel horizontal en modo peek con variantes infinite y finite. Navegación por swipe y tap en dots — sin flechas. Sin autoplay.

## Tokens

### Cambio de slide
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: Mismo token en entrada y salida

### Dots (indicador activo)
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Track — slide change (infinite y finite)
| propiedad | de | a | token |
|---|---|---|---|
| offset | slide N | slide N±1 | motion-spring-md |

### Track — clone reset (solo infinite)
| propiedad | de | a | token |
|---|---|---|---|
| offset | clone position | real slide | instantáneo |

### Dot — active
| propiedad | de | a | token |
|---|---|---|---|
| scale | 1 | 1.25 | motion-spring-sm |
| opacity | 0.45 | 1 | motion-spring-sm |

## Triggers

### slide_change
- evento: swipe, tap dot
- tipo: user-action
- variantes: [infinite, finite]

### dot_active
- evento: cambio de slide activo
- tipo: programmatic

### infinite_loop_reset
- evento: animación termina en slide clonado
- tipo: programmatic
- variantes: [infinite]
- note: jumpToPage al slide real sin animación

### dot_tap_haptic
- evento: onDotTapped con índice distinto al actual
- tipo: user-action

## Estados intermedios

### peek
- trigger: render inicial
- propiedades: slides adyacentes parcialmente visibles
- token: ninguno

### finite_boundary
- trigger: primer o último slide alcanzado por swipe
- propiedades: swipe no avanza más allá del extremo
- variantes: [finite]

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onDotTapped
- swipe_haptic: none
- override_allowed: false

## Coherencia sistémica

- componentes_relacionados: [Sidesheet, Tabs, Bottom Sheet]
- token_mismo_que: [Sidesheet — navegación espacial]
- razon: transición entre vistas usa Guía; dots usan Default como Button

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- material_design: https://m3.material.io/components/carousel
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/carousel-views
- tokens_file: references/tokens-motion.md

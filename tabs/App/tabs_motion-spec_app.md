---
component: Tab
component_slug: tabs
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
companion_web_handoff: tabs_motion-handoff_web.html
web_parity_token: motion-curve-md
variants: [line, segmented]
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onTabSelected
---

## Descripción

Control de tabs con indicador Line o Segmented. Indicador y panel se desplazan horizontalmente al cambiar selección. Scroll horizontal en overflow con reveal del tab activo. Sin hover en App.

## Tokens

### Indicador y panel
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: Mismo token en entrada y salida

### Tab item
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Indicador — tab change
| propiedad | de | a | token |
|---|---|---|---|
| offset | tab N | tab N±1 | motion-spring-md |
| width | ancho tab N | ancho tab N±1 | motion-spring-md |

### Panel — tab change
| propiedad | de | a | token |
|---|---|---|---|
| offset | panel N | panel N±1 | motion-spring-md |

### Tab item — pressed
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0.85 | motion-spring-sm |

### Tab item — focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-sm |

## Triggers

### tab_select
- evento: tap o teclado en tab
- tipo: user-action

### scroll_manual
- evento: drag o fling en lista de tabs
- tipo: user-action
- note: scroll nativo sin token; sync indicador en listener

### scroll_reveal_active
- evento: tab seleccionado fuera del viewport
- tipo: programmatic
- flutter_api: ScrollController.animateTo
- disable_animations_api: ScrollController.jumpTo

## Estados intermedios

### scroll_overflow
- trigger: tabs exceden ancho
- propiedades: scroll horizontal nativo
- token: ninguno

### selected
- trigger: tab activo
- propiedades: estado selected en label
- token: ninguno en label

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onTabSelected
- override_allowed: false

## Coherencia sistémica

- componentes_relacionados: [Carousel, Sidesheet, Segmented Control]
- token_mismo_que: [Carousel — navegación con motion-spring-md]
- razon: cambio de vista guiado usa Guía; items usan Default

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- material_design: https://m3.material.io/components/tabs
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/tab-bars
- tokens_file: references/tokens-motion.md

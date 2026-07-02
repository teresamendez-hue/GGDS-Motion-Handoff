---
component: Carousel
component_slug: carousel
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Ezequiel Arguello
date: 2026-07-02
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: carousel_motion-handoff_web.html
web_parity_token: motion-curve-md
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onSlideSettled
---

## Descripción

Carousel de App navega por gesto (drag/swipe) o tap en dot. No tiene enter/exit de viewport ni flechas de navegación — el reveal-on-hover de Web no tiene equivalente en touch.

## Tokens

### Slide transition (drag / swipe / dot tap)
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en asentamiento hacia adelante, hacia atrás y en el retorno por resistencia"

### Dot indicator
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Track — asentamiento tras gesto
| propiedad | de | a | token |
|---|---|---|---|
| posición X | posición del gesto | posición del slide destino | motion-spring-md |

### Track — retorno por resistencia (límite, Normal)
| propiedad | de | a | token |
|---|---|---|---|
| posición X | posición de resistencia | posición del slide límite | motion-spring-md |

### Dot indicator — activo
| propiedad | de | a | token |
|---|---|---|---|
| color | indicator | indicator-active | motion-spring-sm |

## Triggers

### Cambio de slide (swipe)
- evento: onPanEnd con delta sobre el umbral
- tipo: user-action
- delay_ms: 0

### Cambio de slide (dot)
- evento: tap en dot
- tipo: user-action
- delay_ms: 0

### Resistencia en límite
- evento: onPanUpdate con currentIndex en extremo (solo Behavior Normal)
- tipo: user-action

## Estados intermedios

### Resistencia (rubber-band, solo Behavior Normal)
- trigger: drag más allá del primer o último slide
- propiedades: posición X con factor de resistencia ~0.35
- token: motion-spring-md (en el retorno al soltar)

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onSlideSettled (al completarse el cambio de índice)
- override_allowed: false

## Coherencia sistémica

- componentes_relacionados: [Tabs, Segmented Control]
- token_mismo_que: [Drawer/Bottom Sheet App para navegación guiada (motion-spring-md)]
- razon: el slide es navegación guiada por gesto, misma categoría que Bottom Sheet (motion-spring-md); el dot indicator es una microinteracción de estado, mismo criterio que Button App (motion-spring-sm); el haptic de selección sigue el mismo criterio que Tabs/Segmented Control

## Accesibilidad

- prefers_reduced_motion: "no aplica directamente en Flutter"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations aplica el estado final sin animación"

## Referencias

- material_design: "no aplica"
- apple_hig: "https://developer.apple.com/design/human-interface-guidelines/playing-haptics"
- tokens_file: "references/tokens-motion.md"

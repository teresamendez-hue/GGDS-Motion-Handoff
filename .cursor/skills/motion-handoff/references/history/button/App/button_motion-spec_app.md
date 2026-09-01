---
component: Button
component_slug: button
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-05
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: button_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-action-press
haptic_override_allowed: true
haptic_trigger: onPressed
---

## Descripción

Botón de app con estados internos animados usando spring. No tiene enter/exit de viewport.

## Tokens

### Estados internos
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

### Button — highlight
| propiedad | de | a | token |
|---|---|---|---|

### Button — pressed
| propiedad | de | a | token |
|---|---|---|---|
| scale | 1.0 | 0.98 | motion-spring-sm |

### Focus ring
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0.0 | 1.0 | motion-spring-sm |

### Loading / Skeleton
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0/1 | 1/0 | motion-spring-sm |

## Triggers

### highlight
- tipo: user-action

### pressed
- evento: onTapDown / onTapUp
- tipo: user-action

### focus
- evento: focus-visible
- tipo: user-action

### loading
- evento: set loading true/false
- tipo: programmatic

### skeleton
- evento: resolve skeleton
- tipo: programmatic

### disabled
- evento: disabled true
- tipo: programmatic
- note: instantáneo

## Haptics

- semantic_default: haptic-action-press
- primitive: haptic-impact-light
- flutter_api: HapticFeedback.lightImpact()
- trigger: onPressed
- override_allowed: true

## Coherencia sistémica

- componentes_relacionados: [Action Link]
- token_mismo_que: [Action Link app estados internos]
- razon: categoría default de microinteracciones

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- apple_hig: https://developer.apple.com/design/human-interface-guidelines/animation
- tokens_file: references/tokens-motion.md

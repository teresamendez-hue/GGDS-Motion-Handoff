---
component: Radio Button
component_slug: radio-button
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-08
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: radio-button_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onChanged
---

## Descripción

Componente con estados internos y selección en App usando spring.

## Tokens

### Estados internos
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

| propiedad | uso |
|---|---|
| scale | pressed |
| opacity | focus ring / indicador |
| fill | selected |

## Triggers

- pressed: onTapDown/onTapUp
- focus: focus-visible
- selected: toggle control
- disabled: estado programático instantáneo

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onChanged
- override_allowed: false

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- tokens_file: references/tokens-motion.md

---
component: Switch
component_slug: switch
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-11
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: switch_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-selection-change
haptic_override_allowed: false
haptic_trigger: onChanged
---

## Descripción

Control atomico para alternar entre encendido y apagado con estado interno.

## Tokens

### App
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: Mismo token en todos los cambios de estado internos

## Propiedades animadas

| propiedad | uso |
|---|---|
| position | desplazamiento del thumb |
| color | cambio de track off/on |
| scale | feedback pressed |
| opacity | focus ring |

## Triggers

- active_toggle: onTap / onChanged
- state_variant: Enabled/Pressed/Focused/Disabled
- skeleton_variant: true/false
- exit_trigger: no aplica

## Estados intermedios

### State
- valores: Enabled, Pressed, Focused, Disabled

### Active
- valores: False, True

### Skeleton
- valores: False, True

## Haptics

- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onChanged
- override_allowed: false

## Coherencia sistémica

- componentes_relacionados: [button, checkbox, radio-button, box-selector]
- token_mismo_que: [button]
- razon: microinteraccion de estado interno del mismo nivel de feedback

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- material_design: https://m3.material.io/components/switch/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/toggles
- tokens_file: references/tokens-motion.md

---
component: Switch
component_slug: switch
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Teresa Mendez
date: 2026-06-11
design_system: GGDS
status: draft
---

## Descripción

Control atomico para alternar entre encendido y apagado con estado interno.

## Tokens

### Entrada
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida
- semantic: null
- primitive_curve: null
- css_value: null
- primitive_duration: null
- duration_ms: null

## Propiedades animadas

| propiedad | uso |
|---|---|
| transform | desplazamiento del thumb |
| background-color | cambio de track off/on y hover |
| opacity | focus ring |

## Triggers

- active_toggle: click/tap
- state_variant: Enabled/Hovered/Pressed/Focused/Disabled
- skeleton_variant: true/false
- exit_trigger: no aplica

## Estados intermedios

### State
- valores: Enabled, Hovered, Pressed, Focused, Disabled

### Active
- valores: False, True

### Skeleton
- valores: False, True

## Coherencia sistémica

- componentes_relacionados: [button, checkbox, radio-button, box-selector]
- token_mismo_que: [button]
- razon: microinteraccion de estado interno del mismo nivel de feedback

## Accesibilidad

- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- material_design: https://m3.material.io/components/switch/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/toggles
- tokens_file: references/tokens-motion.md

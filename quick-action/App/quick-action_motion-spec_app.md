---
component: Quick Action
component_slug: quick-action
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-17
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: quick-action_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-action-press
haptic_override_allowed: true
haptic_trigger: onPressed
---

## Descripción

Componente de acceso rápido a acciones frecuentes dentro de una pantalla.
Usa motion solo para cambios de estado internos en App.

## Tokens

### Entrada
- semantic: motion-spring-sm
- primitive_curve: null
- css_value: null
- primitive_duration: null
- duration_ms: null

### Salida
- semantic: motion-spring-sm
- primitive_curve: null
- css_value: null
- primitive_duration: null
- duration_ms: null

### App (si aplica)
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: "Mismo token en entrada y salida"

## Propiedades animadas

### QuickAction — Estados
| propiedad | de | a | token |
|---|---|---|---|
| scale | 1 | 0.985 (pressed) | motion-spring-sm |
| opacity | 1 | 0.92 (pressed) | motion-spring-sm |
| outline-opacity | 0 | 1 (focused) | motion-spring-sm |

### QuickAction — Disabled
| propiedad | de | a | token |
|---|---|---|---|
| opacity | estado actual | 0.45 | none |

## Triggers

### Apertura
- evento: no aplica (componente persistente)
- tipo: user-action
- delay_ms: 0

### Cierre
- evento: no aplica
- tipo: user-action
- auto_delay_ms: null

## Estados intermedios

### State
- trigger: press/focus/toggles de soporte
- propiedades: pressed, focused, disabled, skeleton
- token: motion-spring-sm

## Haptics

- semantic_default: haptic-action-press
- primitive: haptic-impact-light
- flutter_api: HapticFeedback.lightImpact()
- trigger: onPressed
- override_allowed: true

## Coherencia sistémica

- componentes_relacionados: [Button, Icon Button]
- token_mismo_que: [Button, Icon Button]
- razon: mismo feedback tactil y jerarquia de interaccion

## Accesibilidad

- prefers_reduced_motion: no aplica
- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- material_design: https://m3.material.io/components/buttons/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/buttons
- tokens_file: references/tokens-motion.md

---
component: Link
component_slug: link
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
companion_web_handoff: link_motion-handoff_web.html
web_parity_token: motion-curve-sm
---

## Descripción

Componente de navegación textual para acciones o rutas internas.
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

### Link — Estados
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0.85 (pressed) | motion-spring-sm |
| translateY | 0px | 1px (pressed) | motion-spring-sm |
| outline-opacity | 0 | 1 (focused) | motion-spring-sm |

### Link — Visited/Disabled
| propiedad | de | a | token |
|---|---|---|---|
| color | enabled | visited | none |
| opacity | estado actual | 0.45 (disabled) | none |

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
- propiedades: enabled, focused, pressed, disabled, visited
- token: motion-spring-sm

## Coherencia sistémica

- componentes_relacionados: [Button, Breadcrumb]
- token_mismo_que: [Button]
- razon: misma semántica de feedback táctil en texto interactivo

## Accesibilidad

- prefers_reduced_motion: no aplica
- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- material_design: https://m3.material.io/components/buttons/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/buttons
- tokens_file: references/tokens-motion.md

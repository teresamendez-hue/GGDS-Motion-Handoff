---
component: Icon Button
component_slug: icon-button
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-10
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: icon-button_motion-handoff_web.html
web_parity_token: motion-curve-sm
---

## Descripción

Componente de acción compacta con estados internos y feedback táctil spring.

## Tokens

### Estados internos
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

## Propiedades animadas

- corner_radius: 4px

| propiedad | uso |
|---|---|
| scale | pressed + rebound |
| opacity | focus ring / loading / skeleton |

## Triggers

- pressed: onTapDown/onTapUp
- focus: focus-visible
- loading: estado programático
- skeleton: estado programático
- disabled: estado programático instantáneo

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations

## Referencias

- tokens_file: references/tokens.md

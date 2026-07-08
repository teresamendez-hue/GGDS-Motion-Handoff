---
component: Multiple Select
component_slug: multiple-select
platform: app
category: default
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: multiple-select_motion-handoff_web.html
web_parity_token: motion-curve-md
haptic_semantic_default: none
haptic_override_allowed: false
haptic_trigger: none
---

## Descripcion

Field de seleccion multiple en App. Solo modela estados internos del field y feedback del trigger; tap abre Bottom Sheet con Option Items. Chips en el field — handoff Chip.

## Tokens

### App
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28

## Propiedades animadas

### MultipleSelectContainer
| propiedad | de | a | token |
|---|---|---|---|
| borderColor | neutral | focus/error | motion-spring-md |
| opacity | 1 | 0.45 (disabled) | none |

### MultipleSelectRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/error | motion-spring-md |

### FieldTrigger
| propiedad | de | a | token |
|---|---|---|---|
| opacity | default | pressed | motion-spring-md |

## Haptics

- semantic_default: none
- primitive: none
- flutter_api: none
- trigger: none
- override_allowed: false
- note: seleccion en Option Item usa haptic-selection-change

## Coherencia sistemica

- componentes_relacionados: [Field Text, Field Phone Number, Bottom Sheet, Option Item]
- token_mismo_que: [Field Text, Field Phone Number]
- razon: misma familia fields.

## Accesibilidad

- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- tokens_file: tokens-motion.md
- related_handoffs: [Bottom Sheet, Option Item, Chip]

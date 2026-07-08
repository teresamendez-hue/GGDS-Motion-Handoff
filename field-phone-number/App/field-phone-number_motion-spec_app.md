---
component: Field Phone Number
component_slug: field-phone-number
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
companion_web_handoff: field-phone-number_motion-handoff_web.html
web_parity_token: motion-curve-md
haptic_semantic_default: none
haptic_override_allowed: false
haptic_trigger: none
---

## Descripcion

Campo compuesto para numero telefonico con selector de pais en App. Solo modela estados internos del field y feedback del trigger; tap en el prefix abre Bottom Sheet con Option Items (handoffs enlazados).

## Tokens

### App
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28

## Propiedades animadas

### FieldPhoneNumberContainer
| propiedad | de | a | token |
|---|---|---|---|
| borderColor | neutral | focus/error | motion-spring-md |
| opacity | 1 | 0.45 (disabled) | none |

### FieldPhoneNumberRing
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| borderColor | neutral | focus/error | motion-spring-md |

### CountryTrigger
| propiedad | de | a | token |
|---|---|---|---|
| opacity | default | pressed | motion-spring-md |

## Haptics

- semantic_default: none
- primitive: none
- flutter_api: none
- trigger: none
- override_allowed: false

## Coherencia sistemica

- componentes_relacionados: [Field Prefix, Field Text, Field Password, Text Area]
- token_mismo_que: [Field Prefix, Field Text, Field Password, Text Area]
- razon: misma familia fields.

## Accesibilidad

- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- tokens_file: tokens-motion.md
- related_handoffs: [Bottom Sheet, Option Item]

---
component: Chip
component_slug: chip
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Teresa Mendez
date: 2026-06-23
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: chip_motion-handoff_web.html
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-action-press
haptic_override_allowed: true
haptic_trigger: onDismiss
variants: [informational, removable, disabled]
---

## Descripcion

Pill compacto en App. Enter al montar, exit al dismiss, estados pressed en dismiss. Variantes informativo, removible y disabled.

## Tokens

### App
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: mismo token en enter, estados y exit

## Propiedades animadas

### Chip — Enter
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-sm |
| scale | 0.96 | 1 | motion-spring-sm |

### Chip — Exit
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-sm |
| scale | 1 | 0.96 | motion-spring-sm |

### ChipDismiss — Pressed
| propiedad | de | a | token |
|---|---|---|---|
| scale | 1 | 0.92 | motion-spring-sm |

## Haptics

- semantic_default: haptic-action-press
- primitive: haptic-impact-light
- flutter_api: HapticFeedback.lightImpact()
- trigger: onDismiss
- override_allowed: true
- note: solo variante removible

## Coherencia sistemica

- componentes_relacionados: [Multiple Select, Icon Button, Option Item]
- token_mismo_que: [Option Item, Button]
- razon: microinteraccion compacta.

## Accesibilidad

- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- tokens_file: tokens-motion.md
- related_handoffs: [Multiple Select]

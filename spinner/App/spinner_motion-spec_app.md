---
component: Spinner
component_slug: spinner
platform: app
category: default
semantic_token_enter: motion-curve-spin
semantic_token_exit: null
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-curve-spin
companion_web_handoff: spinner_motion-handoff_web.html
web_parity_token: motion-curve-spin
haptic_semantic_default: none
haptic_override_allowed: false
---

## Descripción

Spinner indeterminado en Flutter. Única animación: rotación continua con `motion-curve-spin` (`Curves.linear` + 800ms vía `AnimationController.repeat()`). Sin spring. Montaje, desmontaje y cambios de color/size instantáneos. Sin haptics. Skeleton excluido.

## Tokens

### Rotación continua
- semantic: motion-curve-spin
- curve: Curves.linear
- controller_duration_ms: 800
- repeat: true
- note: "No usar motion-spring-*; rotación lineal continua"

## Propiedades animadas

### Arco — Rotación (visible)
| propiedad | de | a | token |
|---|---|---|---|
| rotation (turns) | 0 | 1 (loop) | motion-curve-spin |

### Variante — Cambio color / size
| propiedad | de | a | token |
|---|---|---|---|
| color / size | anterior | nuevo | sin animación |

## Triggers

### Visible en árbol
- evento: initState / insert en widget tree
- tipo: programmatic
- delay_ms: 0

### Cambio color / size
- evento: actualización de props
- tipo: programmatic
- animacion: ninguna

### Desmontaje
- evento: dispose
- tipo: programmatic
- animacion: ninguna

## Haptics

| acción | token |
|---|---|
| Rotación / visible | none |
| Cambio variante | none |

## Coherencia sistémica

- componentes_relacionados: [Button, Icon Button, Progress Indicator]
- token_mismo_que: [Button loading spinner, Icon Button loading spinner]
- razon: período 800ms linear alineado con Web; Progress Indicator es determinado (spring en cambio de %)

## Accesibilidad

- flutter_disable_animations: controller.stop(), widget visible con Semantics loading
- semantics: liveRegion o label Loading

## Referencias

- material_design: https://m3.material.io/components/progress-indicators/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/progress-indicators
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=5358-336

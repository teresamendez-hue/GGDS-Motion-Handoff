---
component: Spinner
component_slug: spinner
platform: web
category: default
semantic_token_enter: motion-curve-spin
semantic_token_exit: null
owner: Nehuén Benitez
date: 2026-06-29
design_system: GGDS
status: draft
preview_type: internal_state
web_parity_token: motion-curve-spin
---

## Descripción

Spinner indeterminado pasivo. Única animación: rotación continua del arco SVG con `motion-curve-spin` (`easing-linear` + `duration-800`, 800ms por vuelta). Montaje, desmontaje y cambios de color/size son instantáneos. Skeleton excluido.

## Tokens

### Rotación continua
- semantic: motion-curve-spin
- primitive_curve: easing-linear
- css_value: cubic-bezier(0, 0, 1, 1)
- primitive_duration: duration-800
- duration_ms: 800
- iteration: infinite
- note: "Sin token semántico; misma cadena que spinner embebido en Button / Icon Button"

## Propiedades animadas

### Arco SVG — Rotación (visible)
| propiedad | de | a | token |
|---|---|---|---|
| transform (rotate) | 0deg | 360deg (loop) | motion-curve-spin |

### Variante — Cambio color / size
| propiedad | de | a | token |
|---|---|---|---|
| color / width / height | anterior | nuevo | sin animación |

## Triggers

### Visible en DOM
- evento: montaje o `display` no oculto
- tipo: programmatic
- delay_ms: 0
- rotacion: inicia inmediatamente (sin fade-in)

### Cambio color / size
- evento: actualización de props
- tipo: programmatic
- animacion: ninguna

### Desmontaje
- evento: unmount o oculto
- tipo: programmatic
- animacion: ninguna

## Coherencia sistémica

- componentes_relacionados: [Button, Icon Button, Progress Indicator]
- token_mismo_que: [Button loading spinner, Icon Button loading spinner]
- razon: loop lineal 800ms compartido; Progress Indicator es determinado (distinto patrón)

## Accesibilidad

- prefers_reduced_motion: pausar rotación (`animation: none`), mantener indicador visible
- aria: role status o aria-busy true en contenedor de carga

## Referencias

- material_design: https://m3.material.io/components/progress-indicators/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/progress-indicators
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=6055-995

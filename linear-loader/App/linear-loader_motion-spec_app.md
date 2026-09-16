---
component: Linear Loader
component_slug: linear-loader
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: null
owner: Juan Carlos De La Vía
date: 2026-09-15
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-md
companion_web_handoff: null
web_parity_token: null
haptic_semantic_default: none
haptic_override_allowed: false
---

## Descripción

Linear loader indeterminado en Flutter. Shape de ancho constante (120px @ W=360) se traslada solo hacia la derecha, sale por el borde derecho y reentra por la izquierda. Gap 4px entre shape y track inactivo en extremos del corredor. AnimationController.repeat() 2000ms. Sin haptics.

## Tokens

### Loop continuo
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- cycle_ms: 2000
- motor: AnimationController.repeat() + left(t) lineal
- gap_px: 4
- note: "Barrido unidireccional; no cos() ni springs por fase"

## Propiedades animadas

### Fill activo — Loop (t 0→1, repeat)
| propiedad | de | a | token |
|---|---|---|---|
| left | −segWidth | trackWidth (lineal en t) | motion-spring-md |
| width | segWidth | segWidth (constante) | — |

### Track inactivo (layout Figma)
| propiedad | fórmula | token |
|---|---|---|
| leading width | max(0, left − gap) | — |
| trailing width | max(0, W − left − segWidth − gap) | — |
| gap shape ↔ track | 4px en left=gap y left=W−gap−segWidth | — |

### Variante — Cambio color
| propiedad | de | a | token |
|---|---|---|---|
| color tokens | anterior | nuevo | sin animación |

## Triggers

### Visible en árbol
- evento: initState
- tipo: programmatic
- accion: controller.repeat()

## Haptics

| acción | token |
|---|---|
| Loop | none |

## Coherencia sistémica

- componentes_relacionados: [Spinner, Progress Indicator]
- razon: indeterminado continuo unidireccional; gap 4 alineado a flex gap Figma

## Accesibilidad

- flutter_disable_animations: stop en corredor inicial (left=gap)

## Referencias

- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/0HYJJ9VrKoturiGYzEIh50/Core---App-Components?node-id=13663-2236
- tokens_file: tokens-motion.md

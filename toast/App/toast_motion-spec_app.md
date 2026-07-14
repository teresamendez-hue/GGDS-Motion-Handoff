---
component: Toast
component_slug: toast
platform: app
category: communication
semantic_token_enter: motion-spring-lg
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-24
design_system: GGDS
status: draft
preview_type: viewport
spring_token: motion-spring-lg
companion_web_handoff: toast_motion-handoff_web.html
web_parity_token: motion-curve-lg
haptic_semantic_mapping: per-variant
haptic_trigger: onToastShown
---

## Descripción

Feedback flotante inferior derecho en Flutter (`right: 24px`, `bottom: 24px`, ancho 320px). Entrada con `motion-spring-lg` (`opacity` + `translateY`). Salida estándar con paridad `motion-exit` (`Curves.easeIn` · 300ms). Salida swipe con `motion-spring-lg` (`translateX`). Soporta auto-dismiss, X, swipe y cola de un elemento.

## Tokens

### Entrada
- semantic: motion-spring-lg
- mass: 1.0
- stiffness: 200
- damping: 30

### Salida estándar (X / auto-dismiss)
- semantic: motion-exit
- curve: Curves.easeIn
- duration_ms: 300
- note: "Paridad Web con motion-exit; no reutilizar spring de entrada"

### Salida swipe
- semantic: motion-spring-lg
- mass: 1.0
- stiffness: 200
- damping: 30

## Propiedades animadas

### Toast — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-lg |
| translateY | 100% | 0% | motion-spring-lg |

### Toast — Salida estándar
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0% | 100% | motion-exit |

### Toast — Salida (swipe)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-lg |
| translateX | offset gesto | fuera viewport lateral | motion-spring-lg |
| translateY | 0 | 0 | — (sin movimiento vertical) |

## Triggers

### Apertura
- evento: sistema encola y muestra toast
- tipo: programmatic
- delay_ms: 0

### Cierre — auto-dismiss
- evento: 6s después de finalizar entrada
- tipo: automatic
- auto_delay_ms: 6000

### Cierre — X
- evento: tap botón cerrar
- tipo: user-action
- auto_delay_ms: null

### Cierre — swipe
- evento: swipe lateral completado
- tipo: user-action
- auto_delay_ms: null
- nota: salida por costado (translateX + opacity), no translateY

### Ciclo de vida — Action Link
- evento: toast con Action Link
- tipo: programmatic
- auto_delay_ms: null

### Ciclo de vida — long press
- evento: pausa timer auto-dismiss
- tipo: user-action
- auto_delay_ms: null

### Cola
- evento: siguiente toast tras cierre del activo
- tipo: programmatic
- delay_ms: 0

## Estados intermedios

Omitir.

## Coherencia sistémica

- componentes_relacionados: [Snackbar, Modal, Banner]
- token_mismo_que: [Snackbar]
- razon: categoría Comunicación; paridad Web con motion-curve-lg

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Haptics

| variante | token | flutter_api | criterio |
|---|---|---|---|
| information | none | — | default, sin haptic |
| success | haptic-feedback-success | HapticFeedback.lightImpact() | opcional |
| warning | haptic-feedback-warning | HapticFeedback.mediumImpact() | opcional |
| error | haptic-feedback-error | HapticFeedback.heavyImpact() | solo crítico |

- trigger: onToastShown
- note: "Information no dispara haptic; Error solo en errores críticos"

## Referencias

- material_design: https://m3.material.io/components/snackbar/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/feedback
- tokens_file: references/tokens-motion.md
- figma_handoff: https://www.figma.com/design/FvgtR93pPCj97CQbNjUAqu/Hand-Off---Core-App?node-id=554-18152

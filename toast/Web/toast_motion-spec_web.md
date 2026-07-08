---
component: Toast
component_slug: toast
platform: web
category: communication
semantic_token_enter: motion-curve-lg
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-24
design_system: GGDS
status: draft
---

## Descripción

Feedback flotante no bloqueante en la **parte inferior derecha** de la pantalla (`right: 24px`, `bottom: 24px`, ancho 320px). Entra y sale del viewport con slide vertical + fade. Un toast visible a la vez.

## Tokens

### Entrada
- semantic: motion-curve-lg
- primitive_curve: easing-emphasis
- css_value: cubic-bezier(0.2, 0, 0, 1)
- primitive_duration: duration-500
- duration_ms: 500

### Salida
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-300
- duration_ms: 300

## Propiedades animadas

### Toast — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-lg |
| translateY | 100% | 0% | motion-curve-lg |

### Toast — Salida estándar
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0% | 100% | motion-exit |

### Toast — Salida (swipe)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateX | offset gesto | fuera viewport lateral | motion-exit |
| translateY | 0% | 0% | — (sin movimiento vertical) |

## Triggers

### Apertura
- evento: sistema encola y muestra toast tras evento de negocio
- tipo: programmatic
- delay_ms: 0

### Cierre — auto-dismiss
- evento: timer 6s después de finalizar animación de entrada
- tipo: automatic
- auto_delay_ms: 6000

### Cierre — manual X
- evento: tap en botón cerrar
- tipo: user-action
- auto_delay_ms: null

### Cierre — swipe
- evento: swipe lateral completado
- tipo: user-action
- auto_delay_ms: null
- nota: salida por costado (translateX + opacity), no translateY

### Ciclo de vida — Action Link
- evento: toast incluye Action Link
- tipo: programmatic
- auto_delay_ms: null
- nota: sin timer 6s; solo cierre manual

### Ciclo de vida — long press
- evento: long press pausa timer; release retoma
- tipo: user-action
- auto_delay_ms: null

### Cola
- evento: toast siguiente espera cierre del activo
- tipo: programmatic
- delay_ms: 0

## Estados intermedios

Omitir — variantes semánticas (Info, Success, Warning, Error) no tienen transición de motion propia.

## Coherencia sistémica

- componentes_relacionados: [Snackbar, Modal, Banner]
- token_mismo_que: [Snackbar]
- razon: feedback de comunicación no bloqueante; mismo peso visual que Snackbar en GGDS

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica

## Haptics

- no aplica en Web
- app_reference: haptic-feedback-success (opcional, ver handoff App)

## Referencias

- material_design: https://m3.material.io/components/snackbar/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/feedback
- tokens_file: references/tokens-motion.md
- figma_handoff: https://www.figma.com/design/FvgtR93pPCj97CQbNjUAqu/Hand-Off---Core-App?node-id=554-18152

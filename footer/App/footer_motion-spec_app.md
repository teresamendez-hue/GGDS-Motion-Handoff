---
component: Footer
component_slug: footer
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Juan Carlos De La Via Levy
date: 2026-09-01
design_system: GGDS
status: draft
preview_type: viewport
spring_token: motion-spring-md
companion_web_handoff: null
web_parity_token: null
overlay_spring_token: null
haptic_semantic_default: null
haptic_override_allowed: false
haptic_trigger: null
---

## Descripción

Contenedor de acciones fijo al fondo de pantalla en App. Se reposiciona verticalmente cuando aparece el teclado nativo para permanecer visible. Puede transicionar de Overlay/False a Overlay/True al abrir el teclado.

## Tokens

### Reposicionamiento (teclado)
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en apertura y cierre de teclado. Preferir viewInsets nativo en implementación."

### Overlay (superficie)
- semantic: none
- note: "Cambio instantáneo en entrada y salida. Sin spring ni duración."

## Propiedades animadas

### Footer — Teclado aparece
| propiedad | de | a | token | nota |
|---|---|---|---|---|
| padding-bottom / translateY | 0 | viewInsets.bottom | motion-spring-md | |
| backgroundColor | transparente | #FFFFFF | none | instantáneo con apertura de teclado |
| boxShadow | none | shadow-m | none | instantáneo con apertura de teclado |
| borderRadius top | 0 | 20px | none | instantáneo con apertura de teclado |

### Footer — Teclado desaparece
| propiedad | de | a | token | nota |
|---|---|---|---|---|
| padding-bottom / translateY | viewInsets.bottom | 0 | motion-spring-md | primero |
| backgroundColor | #FFFFFF | transparente | none | instantáneo con cierre de teclado |
| boxShadow | shadow-m | none | none | instantáneo con cierre de teclado |
| borderRadius top | 20px | 0 | none | instantáneo con cierre de teclado |

## Triggers

### Teclado aparece
- evento: focus en input de pantalla + teclado nativo visible
- tipo: automatic
- delay_ms: 0

### Teclado desaparece
- evento: blur del input o dismiss del teclado nativo
- tipo: automatic
- delay_ms: 0

## Estados intermedios

### Overlay forzado por teclado
- trigger: teclado aparece con overlay inicial false
- propiedades: backgroundColor, boxShadow, borderRadius
- token: none
- note: activación y reversión instantáneas con apertura/cierre de teclado

## Coherencia sistémica

- componentes_relacionados: [Bottom Sheet, Button, Icon Button, Checkbox, Field Text]
- token_mismo_que: [Bottom Sheet para reposicionamiento vertical]
- razon: Footer comparte desplazamiento vertical con paneles de guía (spring-md). Overlay es cambio de estado instantáneo sin token de motion.

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Referencias

- material_design: "https://m3.material.io/components/bottom-app-bar/overview"
- apple_hig: "https://developer.apple.com/design/human-interface-guidelines/keyboards"
- tokens_file: "references/tokens-motion.md"
- figma_handoff: "https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/iGq4p5mk5BFIa9fJE6aXov/Core---App-Components?node-id=13011-27835"

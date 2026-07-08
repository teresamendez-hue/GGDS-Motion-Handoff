---
component: Modal
component_slug: modal
platform: app
category: interactive
semantic_token_enter: motion-spring-xl
semantic_token_exit: motion-spring-xl
owner: Nehuén Benitez
date: 2026-07-01
design_system: GGDS
status: draft
preview_type: viewport
spring_token: motion-spring-xl
companion_web_handoff: null
web_parity_token: motion-curve-xl
---

## Descripción

Overlay bloqueante en Flutter. Default/Illustration: scrim + panel opacity, panel scale 0.92→1 con motion-spring-xl. Full Screen: panel translateY 100%→0 + opacity con motion-spring-md; scrim solo fade. Mismo spring en entrada y salida por variante. Skeleton visual sin swap. Sin haptics propios del modal.

## Tokens

### Default / Illustration — entrada y salida
- semantic: motion-spring-xl
- mass: 1.0
- stiffness: 100
- damping: 20
- note: "Mismo token en entrada y salida"

### Full Screen — entrada y salida
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en entrada y salida"

## Propiedades animadas

### Scrim — Entrada (Default / Illustration)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-xl |

### Panel — Entrada (Default / Illustration)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-xl |
| scale | 0.92 | 1 | motion-spring-xl |

### Scrim — Salida (Default / Illustration)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-xl |

### Panel — Salida (Default / Illustration)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-xl |
| scale | 1 | 0.92 | motion-spring-xl |

### Scrim — Entrada (Full Screen)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |

### Panel — Entrada (Full Screen)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| translateY | 100% | 0% | motion-spring-md |

### Panel — Salida (Full Screen)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-md |
| translateY | 0% | 100% | motion-spring-md |

## Triggers

### Apertura
- evento: showModal / Navigator.push / programmatic
- tipo: user-action
- delay_ms: 0

### Cierre
- evento: tap X, tap scrim, Navigator.pop/back, botón de acción que cierra
- tipo: user-action
- auto_delay_ms: null

## Estados intermedios

### Skeleton
- trigger: modal abre con skeleton=true
- propiedades: placeholders visibles; sin crossfade al cargar contenido
- token: ninguno

### Scroll
- trigger: variante Scroll=true
- propiedades: scroll nativo del cuerpo
- token: ninguno

## Coherencia sistémica

- componentes_relacionados: [Bottom Sheet, Sidesheet, Toast]
- token_mismo_que: [Bottom Sheet — motion-spring-md para navegación vertical]
- razon: Default centrado usa motion-spring-xl (Interactivos); Full Screen usa motion-spring-md (Guía) por deslizamiento espacial

## Accesibilidad

- prefers_reduced_motion: no aplica (Web)
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations → estado final sin spring"

## Referencias

- material_design: https://m3.material.io/styles/motion/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/animation
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/Core---App-Components?node-id=9209-462

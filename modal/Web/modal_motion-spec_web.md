---
component: Modal
component_slug: modal
platform: web
category: interactive
semantic_token_enter: motion-curve-xl
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-07-01
design_system: GGDS
status: draft
preview_type: viewport
---

## Descripción

Overlay bloqueante centrado (Default / Illustration) con scrim y panel. Entrada: fade scrim 0→0.7 + panel opacity 0→1 + translateY 8%→0. Salida: motion-exit. Sin scale en Web. Skeleton es estado visual sin animación de swap. Scroll interno sin motion propio.

## Tokens

### Entrada
- semantic: motion-curve-xl
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-700
- duration_ms: 700

### Salida
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-400
- duration_ms: 400

## Propiedades animadas

### Scrim — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 0.7 | motion-curve-xl |

### Panel — Entrada (Default / Illustration)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-xl |
| translateY | 8% | 0% | motion-curve-xl |

### Scrim — Salida
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0.7 | 0 | motion-exit |

### Panel — Salida (Default / Illustration)
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translateY | 0% | 8% | motion-exit |

## Triggers

### Apertura
- evento: click/tap en acción que abre el modal (programmatic o user-action)
- tipo: user-action
- delay_ms: 0

### Cierre
- evento: click en X, click en scrim, tecla Esc, click en botón de acción que cierra
- tipo: user-action
- auto_delay_ms: null

## Estados intermedios

### Skeleton
- trigger: modal abre con prop skeleton=true
- propiedades: placeholders visibles dentro del panel ya abierto
- token: ninguno (sin animación de swap)

### Scroll
- trigger: variante Scroll=true
- propiedades: scroll nativo del cuerpo
- token: ninguno

## Coherencia sistémica

- componentes_relacionados: [Sidesheet, Drawer, Toast]
- token_mismo_que: [Modal Web library — translateY 8%]
- razon: categoría Interactivos; overlay bloqueante con ritmo pausado motion-curve-xl

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica

## Referencias

- material_design: https://m3.material.io/styles/motion/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/animation
- tokens_file: tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=9341-3731

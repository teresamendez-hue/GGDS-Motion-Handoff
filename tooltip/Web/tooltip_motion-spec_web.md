---
component: Tooltip
component_slug: tooltip
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-25
design_system: GGDS
status: draft
---

## Descripción

Información contextual breve anclada a un trigger. Aparece en hover o focus; desaparece en mouseleave/blur (fade) o de forma instantánea en Esc, scroll, click o Tab. No interactivo.

## Tokens

### Entrada
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida suave
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-100
- duration_ms: 100

### Salida forzada
- semantic: none
- note: "Instantáneo — Esc, scroll, click trigger, Tab"
- duration_ms: 0

## Propiedades animadas

### Tooltip — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-sm |

### Tooltip — Salida suave
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |

### Tooltip — Salida forzada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | none (instantáneo) |

## Triggers

### Apertura
- evento: mouseenter o focus-visible en trigger
- tipo: user-action
- delay_ms: 0

### Cierre suave
- evento: mouseleave o blur del trigger
- tipo: user-action
- auto_delay_ms: null

### Cierre forzado — Escape
- evento: tecla Escape
- tipo: user-action
- auto_delay_ms: null
- nota: instantáneo

### Cierre forzado — scroll
- evento: scroll significativo
- tipo: user-action
- auto_delay_ms: null
- nota: instantáneo

### Cierre forzado — click
- evento: click en trigger
- tipo: user-action
- auto_delay_ms: null
- nota: instantáneo

### Cierre forzado — Tab
- evento: foco pasa a otro elemento
- tipo: user-action
- auto_delay_ms: null
- nota: instantáneo

## Variantes de posicionamiento

- vertical: [Top, Bottom]
- alignment: [Left, Center, Right]
- nota: no alteran tokens de motion

## Estados intermedios

Omitir.

## Coherencia sistémica

- componentes_relacionados: [Popover, Modal]
- token_mismo_que: [Link estados, Button estados]
- razon: micro-feedback Default; el patrón más sutil del sistema (solo opacity)

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica
- pointer_events: "none en tooltip; interacción solo en trigger"

## Haptics

- no aplica en Web

## Referencias

- material_design: https://m3.material.io/components/tooltips/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/tooltips
- tokens_file: references/tokens-motion.md
- figma_handoff: https://www.figma.com/design/7zqh3EGXWDVhqvY6Fwk1tH/Core---Web-Components?node-id=9401-1800

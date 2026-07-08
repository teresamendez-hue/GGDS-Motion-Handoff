---
component: Coachmark
component_slug: coachmark
platform: web
category: guide
semantic_token_enter: motion-curve-md
semantic_token_exit: motion-exit
owner: Nehuén Benitez
date: 2026-06-25
design_system: GGDS
status: draft
---

## Descripción

Tour contextual anclado a un elemento target con puntero triangular. Entra y sale del viewport con fade + translate 8px en eje del puntero. Avance de step crossfadea contenido interno. Sin scrim animado.

## Tokens

### Entrada
- semantic: motion-curve-md
- primitive_curve: easing-decelerate
- css_value: cubic-bezier(0, 0, 0.2, 1)
- primitive_duration: duration-300
- duration_ms: 300

### Salida suave
- semantic: motion-exit
- primitive_curve: easing-accelerate
- css_value: cubic-bezier(0.4, 0, 1, 1)
- primitive_duration: duration-200
- duration_ms: 200

### Cambio de step
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- css_value: cubic-bezier(0.4, 0, 0.2, 1)
- primitive_duration: duration-150
- duration_ms: 150

### Salida forzada
- semantic: none
- duration_ms: 0
- note: "Escape, target off-screen — transition: none"

## Propiedades animadas

### Coachmark — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-curve-md |
| translate (eje puntero) | 8px | 0 | motion-curve-md |

### Coachmark — Salida suave
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-exit |
| translate (eje puntero) | 0 | 8px | motion-exit |

### Coachmark — Salida forzada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | none |
| translate | visible | oculto | none |

### Contenido step — Cambio
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 → 1 | motion-curve-sm |

## Triggers

### Apertura
- evento: tour inicia o step se muestra programáticamente
- tipo: programmatic
- delay_ms: 0

### Cierre suave — X
- evento: tap en botón cerrar (X)
- tipo: user-action
- auto_delay_ms: null

### Cierre suave — Omitir
- evento: tap en botón secondary (Omitir)
- tipo: user-action
- auto_delay_ms: null

### Cierre suave — Finalizar
- evento: tap en primary en último step
- tipo: user-action
- auto_delay_ms: null

### Cierre forzado — Escape
- evento: tecla Escape
- tipo: user-action
- auto_delay_ms: null

### Cierre forzado — target off-screen
- evento: target sale del viewport por scroll
- tipo: automatic
- auto_delay_ms: null

### Avance de step
- evento: tap en primary (no último step)
- tipo: user-action
- delay_ms: 0

## Estados intermedios

### Cambio de step
- trigger: primary en paso intermedio
- propiedades: opacity contenido (crossfade)
- token: motion-curve-sm
- duration_ms: 150

### Reposicionamiento (salto grande)
- trigger: siguiente target con distancia superior a umbral
- propiedades: opacity, translate panel completo
- token: motion-curve-md
- duration_ms: 300

## Coherencia sistémica

- componentes_relacionados: [Tooltip, Modal, Drawer, Sidesheet]
- token_mismo_que: [Drawer, Sidesheet]
- razon: categoría Guía; onboarding y tours guiados comparten peso motion-curve-md / motion-spring-md

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: no aplica

## Referencias

- material_design: https://m3.material.io/components/tooltips/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/onboarding
- tokens_file: references/tokens-motion.md
- figma_handoff: https://www.figma.com/design/KXyagGEUmiS08WO7TV8vVM/Hand-Off---Core-Web?node-id=5003-63630

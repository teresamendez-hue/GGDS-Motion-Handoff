---
component: Coachmark
component_slug: coachmark
platform: app
category: guide
semantic_token_enter: motion-spring-md
semantic_token_exit: motion-spring-md
owner: Nehuén Benitez
date: 2026-06-25
design_system: GGDS
status: draft
preview_type: viewport
spring_token: motion-spring-md
companion_web_handoff: coachmark_motion-handoff_web.html
web_parity_token: motion-curve-md
haptic_semantic_default: none
haptic_override_allowed: true
haptic_trigger: none
---

## Descripción

Tour contextual anclado a target en Flutter. Entra y sale con spring motion-spring-md (opacity + translate 8px eje puntero). Crossfade de contenido con motion-spring-sm. Sin scrim animado. Salida forzada instantánea.

## Tokens

### Entrada
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28

### Salida suave
- semantic: motion-spring-md
- mass: 1.0
- stiffness: 300
- damping: 28
- note: "Mismo token en entrada y salida"

### Cambio de step
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35

### Salida forzada
- semantic: none
- duration_ms: 0
- note: "Escape, target off-screen — setHidden instantáneo"

## Propiedades animadas

### Coachmark — Entrada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 0 | 1 | motion-spring-md |
| translate (eje puntero) | 8px | 0 | motion-spring-md |

### Coachmark — Salida suave
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | motion-spring-md |
| translate (eje puntero) | 0 | 8px | motion-spring-md |

### Coachmark — Salida forzada
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 | none |
| translate | visible | oculto | none |

### Contenido step — Cambio
| propiedad | de | a | token |
|---|---|---|---|
| opacity | 1 | 0 → 1 | motion-spring-sm |

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
- evento: tecla Escape o gesto equivalente
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
- token: motion-spring-sm

### Reposicionamiento (salto grande)
- trigger: siguiente target con distancia superior a umbral
- propiedades: opacity, translate panel completo
- token: motion-spring-md

## Coherencia sistémica

- componentes_relacionados: [Tooltip, Modal, Drawer, Bottom Sheet, Button, Icon Button]
- token_mismo_que: [Drawer, Bottom Sheet]
- razon: categoría Guía; paridad Web con motion-curve-md

## Accesibilidad

- prefers_reduced_motion: "aplicar estado final sin transición"
- flutter_disable_animations: "MediaQuery.of(context).disableAnimations"

## Haptics

- requiere: false
- appear: none
- composicion: [Button, Icon Button]
- override_coachmark: "primary de pasos intermedios puede usar none (criterio de producto)"
- nota: "Sin haptic en aparición; press en acciones compuestas según handoffs enlazados"

## Referencias

- material_design: https://m3.material.io/components/tooltips/overview
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/onboarding
- tokens_file: references/tokens-motion.md
- figma_handoff: https://www.figma.com/design/FvgtR93pPCj97CQbNjUAqu/Hand-Off---Core-App?node-id=1146-21696

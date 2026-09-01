---
component: BottomNav
component_slug: bottom-nav
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Juan Carlos De La Via Levy
date: 2026-09-01
design_system: GGDS
status: draft
preview_type: internal_state
spring_token: motion-spring-sm
companion_web_handoff: null
web_parity_token: null
variants: [fixed, floating]
haptic_semantic_default_nav_item: haptic-selection-change
haptic_semantic_default_fab: haptic-action-press
haptic_override_allowed_fab: true
haptic_trigger_nav_item: onNavItemTap
haptic_trigger_fab: onPressed
---

## Descripción

Barra de navegación inferior persistente para cambiar de sección en App. Tap en item navega y actualiza estado Selected con transición de color. FAB central opcional dispara acción independiente. Variantes Fixed (ancho completo) y Floating (pill elevada). Sin entrada/salida de viewport.

## Tokens

### Nav item y FAB
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: Mismo token en entrada y salida de cada transición de estado

## Propiedades animadas

### Nav item — selección (icono)
| propiedad | de | a | token |
|---|---|---|---|
| color (icono) | color/text/action-enabled | color/icon/accent | motion-spring-sm |

### Nav item — selección (label)
| propiedad | de | a | token |
|---|---|---|---|
| color (label) | color/text/action-enabled | color/text/accent | motion-spring-sm |

### Nav item — pressed
| propiedad | de | a | token |
|---|---|---|---|
| backgroundColor (contenedor icono) | transparent | action-3-pressed | motion-spring-sm |

### FAB — pressed
| propiedad | de | a | token |
|---|---|---|---|
| backgroundColor | action-accent-1-enabled | action-accent-1-pressed | motion-spring-sm |

### FAB — focused
| propiedad | de | a | token |
|---|---|---|---|
| backgroundColor | — | action-accent-1-enabled | sin ring |

## Triggers

### nav_item_select
- evento: tap en item distinto al activo
- tipo: user-action
- delay_ms: 0

### nav_item_re_tap
- evento: tap en item ya seleccionado
- tipo: user-action
- note: solo pressed visual; sin cambio de sección ni haptic

### fab_action
- evento: tap en FAB
- tipo: user-action
- delay_ms: 0

### disabled
- evento: item o FAB en estado disabled
- tipo: programmatic

## Estados intermedios

### pressed
- trigger: pointer down en item o FAB
- propiedades: backgroundColor contenedor
- token: motion-spring-sm

### selected
- trigger: item activo de la sección visible
- propiedades: icono `color/icon/accent` (#797985) · label `color/text/accent` (#797985)
- token: motion-spring-sm al cambiar

### colores_estado (referencia Figma)
- item_enabled: icono/label #252529 (`color/icon/on-action-3`, `color/text/action-enabled`)
- item_pressed: contenedor #B4B4BF (`color/bg/action-3-pressed`); icono/label sin cambio
- item_selected: icono/label #797985 (`color/icon/accent`, `color/text/accent`)
- fab_enabled: background #8F8F9C (`color/bg/action-accent-1-enabled`); icono #FFFFFF
- fab_pressed: background #404047 (`color/bg/action-accent-1-pressed`); icono #FFFFFF
- fab_focused: background #8F8F9C (`color/bg/action-accent-1-enabled`); sin focus ring

## Haptics

### Nav item
- semantic_default: haptic-selection-change
- primitive: haptic-selection-click
- flutter_api: HapticFeedback.selectionClick()
- trigger: onNavItemTap cuando index != currentIndex
- override_allowed: false

### FAB
- semantic_default: haptic-action-press
- primitive: haptic-impact-light
- flutter_api: HapticFeedback.lightImpact()
- trigger: onPressed
- override_allowed: true
- override_values: [none, default]

## Coherencia sistémica

- componentes_relacionados: [Tab, Icon Button, Quick Action]
- token_mismo_que: [Tab item, Icon Button, Quick Action — motion-spring-sm]
- razon: microinteracción de cambio de estado y feedback táctil en navegación primaria

## Accesibilidad

- flutter_disable_animations: MediaQuery.of(context).disableAnimations
- prefers_reduced_motion: aplicar estado final sin transición

## Referencias

- material_design: https://m3.material.io/components/navigation-bar
- apple_hig: https://developer.apple.com/design/human-interface-guidelines/tab-bars
- tokens_file: references/tokens-motion.md
- figma_component: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/ahhZnEMvRu7ZvVQ8GlSFbh/Core---App-Components?node-id=13132-1272
- figma_handoff: https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/ahhZnEMvRu7ZvVQ8GlSFbh/Core---App-Components?node-id=13211-6646

---
component: Interactive Card Data
component_slug: interactive-card-data
platform: app
category: default
semantic_token_enter: motion-spring-sm
semantic_token_exit: motion-spring-sm
owner: Juan Carlos De La Via Levy
date: 2026-08-18
design_system: GGDS
status: approved
preview_type: internal_state
spring_token: motion-spring-sm
web_parity_token: motion-curve-sm
haptic_semantic_default: haptic-action-press
haptic_override_allowed: true
haptic_trigger: onPressed
---

## Description

Persistent component with internal state transitions in app

## Tokens

### Enter
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- perceived_settle_ms: 150
- web_parity_token: motion-curve-sm

### Exit
- semantic: motion-spring-sm
- mass: 1.0
- stiffness: 400
- damping: 35
- note: no motion-exit for this component

## Animated Properties

### Card Root
| property | from | to | token | notes |
|---|---|---|---|---|
| scale | 1.0 | 0.98 | motion-spring-sm | pressed |
| focus ring opacity | 0 | 1 | motion-spring-sm | outline 2px rgba(115,115,229,0.65) offset 2px |
| box-shadow | 0 8px 24px rgba(37,37,41,0.08) | none | motion-spring-sm | enabled visible, pressed hidden |
| transition | active | none in disabled | none | instant |

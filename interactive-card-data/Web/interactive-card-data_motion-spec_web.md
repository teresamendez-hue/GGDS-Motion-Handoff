---
component: Interactive Card Data
component_slug: interactive-card-data
platform: web
category: default
semantic_token_enter: motion-curve-sm
semantic_token_exit: null
owner: Juan Carlos De La Via Levy
date: 2026-08-18
design_system: GGDS
status: approved
---

## Description

Persistent component with internal state transitions

## Tokens

### Enter
- semantic: motion-curve-sm
- primitive_curve: easing-standard
- primitive_duration: duration-150
- duration_ms: 150
- perceived_settle_ms: 150
- app_parity_token: motion-spring-sm

### Exit
- semantic: null
- primitive_curve: null
- primitive_duration: null
- duration_ms: null

## Animated Properties

### Card Root
| property | from | to | token | notes |
|---|---|---|---|---|
| scale | 1.0 | 0.98 | motion-curve-sm | pressed |
| box-shadow | none | 0 8px 24px rgba(37,37,41,0.08) | motion-curve-sm | hover only |
| focus ring opacity | 0 | 1 | motion-curve-sm | outline 2px rgba(115,115,229,0.65) offset 2px |
| transition | active | none in disabled | none | instant |

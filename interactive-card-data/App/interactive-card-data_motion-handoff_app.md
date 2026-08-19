# Motion Handoff - Interactive Card Data App

| key | value |
|---|---|
| component | Interactive Card Data |
| platform | App Flutter |
| owner | Juan Carlos De La Via Levy |
| design system | GGDS |
| token spring | motion-spring-sm |
| category | default |
| date | 2026-08-18 |

## Visual Reference

| preview | use |
|---|---|
| Preview App HTML | referencia visual con simulación de spring |

## Web ↔ App Parity

| aspect | web | app |
|---|---|---|
| semantic token | motion-curve-sm | motion-spring-sm |
| primitive | easing-standard · duration-150 (150ms) | mass 1.0 · stiffness 400 · damping 35 |
| perceived timing | ~150ms settle | ~150ms settle (spring-sm) |
| internal states | hover · pressed · focus · disabled | pressed · focus · disabled |
| scale pressed | 1.0 → 0.98 | 1.0 → 0.98 |
| focus ring | outline 2px rgba(115,115,229,0.65) · offset 2px | mismo target · spring-sm |
| elevation shadow | hover only | enabled: 0 8px 24px rgba(37,37,41,0.08), pressed: none |
| disabled | sin transición | sin transición |

## Motion Specification

Estados internos únicamente. Sin enter o exit de viewport. Sin hover en app. Skeleton fuera de alcance.

## Timeline

| step | event | from | to | property | from value | to value | token |
|---|---|---|---|---|---|---|---|
| 1 | tap down | enabled | pressed | scale + shadow | 1.0 + 0 8px 24px rgba(37,37,41,0.08) | 0.98 + none | motion-spring-sm |
| 2 | tap up | pressed | enabled | scale + shadow | 0.98 + none | 1.0 + 0 8px 24px rgba(37,37,41,0.08) | motion-spring-sm |
| 3 | keyboard focus | enabled | focused | focus ring opacity | 0 | 1 | motion-spring-sm |
| 4 | blur | focused | enabled | focus ring opacity | 1 | 0 | motion-spring-sm |
| 5 | disabled true | active | disabled | all motion | active | none | none |

## Token Mapping

- motion-spring-sm → mass 1.0 · stiffness 400 · damping 35
- Paridad perceptual con motion-curve-sm (150ms) en Web
- Aplica a scale, focus ring opacity y sombra de elevación (enabled ↔ pressed)
- Disabled: sin animación

## Haptics Specification

- required: yes
- semantic token: haptic-action-press
- trigger: onPressed
- override: none | default

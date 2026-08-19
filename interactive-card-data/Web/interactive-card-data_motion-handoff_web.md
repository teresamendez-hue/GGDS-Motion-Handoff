# Motion Handoff - Interactive Card Data Web

| key | value |
|---|---|
| component | Interactive Card Data |
| platform | Web |
| owner | Juan Carlos De La Via Levy |
| design system | GGDS |
| token enter | motion-curve-sm |
| token exit | none |
| category | default |
| date | 2026-08-18 |

## Web ↔ App Parity

| aspect | web | app |
|---|---|---|
| semantic token | motion-curve-sm | motion-spring-sm |
| primitive | easing-standard · duration-150 (150ms) | mass 1.0 · stiffness 400 · damping 35 |
| perceived timing | ~150ms settle | ~150ms settle (spring-sm) |
| scale pressed | 1.0 → 0.98 | 1.0 → 0.98 |
| focus ring | outline 2px rgba(115,115,229,0.65) · offset 2px · opacity 0 → 1 | mismo target visual · spring-sm |
| elevation shadow | 0 8px 24px rgba(37,37,41,0.08) en hover | sin hover |
| disabled | sin transición | sin transición |
| reduced motion | estado final instantáneo | disableAnimations → estado final instantáneo |

## Motion Specification

Componente con estados internos únicamente. Sin enter o exit de viewport. Skeleton fuera de alcance.

## Timeline

| step | event | from | to | property | from value | to value | token |
|---|---|---|---|---|---|---|---|
| 1 | mouseenter | enabled | hovered | box-shadow | none | 0 8px 24px rgba(37,37,41,0.08) | motion-curve-sm |
| 2 | mouseleave | hovered | enabled | box-shadow | elevated | none | motion-curve-sm |
| 3 | mousedown | enabled / hovered | pressed | scale | 1.0 | 0.98 | motion-curve-sm |
| 4 | mouseup | pressed | enabled / hovered | scale | 0.98 | 1.0 | motion-curve-sm |
| 5 | focus-visible (keyboard) | enabled | focused | outline opacity | 0 | 1 | motion-curve-sm |
| 6 | blur | focused | enabled | outline opacity | 1 | 0 | motion-curve-sm |
| 7 | disabled true | active | disabled | all motion | active | none | none |

## Token Mapping

- motion-curve-sm → easing-standard → cubic-bezier(0.4, 0, 0.2, 1) → duration-150 (150ms)
- Aplica a scale, box-shadow (hover) y focus ring (outline-color y outline-offset)
- Disabled: transition none
- prefers-reduced-motion: estado final sin transición

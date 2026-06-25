# Motion Handoff — Switch (App)

| | |
|---|---|
| **Componente** | Switch |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-11 |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./switch_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/switch_motion-handoff_web.html) | Misma intención de motion. En App implementar con token spring, no curvas CSS. |

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms | mass 1.0 · stiffness 400 · damping 35 |
| Estados internos | Enabled, Pressed, Focused, Disabled + Active + Skeleton | Enabled, Pressed, Focused, Disabled + Active + Skeleton |
| Exit | no aplica | no aplica |

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** referencia de intención, no copiar CSS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription`.

---

## Motion Specification

Switch en App usa estados internos animados con `motion-spring-sm`.

Incluye variantes de Figma: `State` (`Enabled`, `Pressed`, `Focused`, `Disabled`), `Active` (`False/True`) y `Skeleton` (`False/True`).

`Disabled` es instantáneo y `disableAnimations` aplica estado final sin transición.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | De | A | Token spring |
|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tap/click | Switch | — | — | — | — |
| 2 | Response | Active toggle | Thumb | position | x=3 | x=21 | motion-spring-sm |
| 3 | Response | Active toggle | Track | color | off | on | motion-spring-sm |
| 6 | Trigger | Pressed state | Switch | — | — | — | — |
| 7 | Response | Pressed | Switch | scale | 1 | 0.98 | motion-spring-sm |
| 8 | Trigger | Focused state | Ring | — | — | — | — |
| 9 | Response | Focus ring | Ring | opacity | 0 | 1 | motion-spring-sm |
| 10 | Trigger | Skeleton state | Switch | — | — | — | — |
| 11 | Response | Skeleton | Track/thumb | opacity/visibility | normal | skeleton | motion-spring-sm |
| 12 | Trigger | Disabled state | Switch | — | — | — | — |
| 13 | Response | Disabled | todas | — | final | — | instantáneo |

---

## Token Mapping

| Token semántico | mass | stiffness | damping |
|---|---|---|---|
| `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Dart

```dart
final motion = context.motionTokens;
final spring = motion.springSm; // motion-spring-sm

// Usar `spring` en cambio de thumb, track y focus ring.
```

---

## Haptics Specification

Switch dispara haptic en cambio de estado on/off usando `haptic-selection-change`.

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `cambio de estado/selección` |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onChanged(bool value) {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange); // haptic-selection-change
}
```

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-sm` para estados internos |
| Active | thumb y track sincronizados |
| Exit | no aplica |
| Disabled | instantáneo |
| A11y | `MediaQuery.disableAnimations` |

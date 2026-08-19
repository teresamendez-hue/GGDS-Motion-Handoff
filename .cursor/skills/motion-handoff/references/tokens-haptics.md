# Haptics System GGDS — Tokens (App)

Fuente de verdad para handoffs de haptics en App (Flutter). **Nunca usar valores de memoria** — leer este archivo antes de documentar o implementar.

> Si un token no aparece aquí, no existe en el sistema. Notificar al equipo de Design Ops antes de generar el handoff.

---

## Arquitectura de capas

```
Componente (Button, Switch, etc.)
        ↓ consume
Token semántico (intención UX)
        ↓ resuelve a
Token primitivo (capacidad técnica)
        ↓ ejecuta
Flutter API (HapticFeedback.*)
```

| Capa | Qué representa | Quién lo define | Ejemplo |
|---|---|---|---|
| **Semántico** | Intención de experiencia | Design System (Ops + dev) | `haptic-action-press` |
| **Primitivo** | Capacidad técnica disponible en la plataforma | Design System (mapeo a Flutter) | `haptic-impact-light` |
| **Implementación** | API concreta del framework | Flutter SDK | `HapticFeedback.lightImpact()` |

Los **tokens primitivos** se nutren de las **APIs nativas de haptics expuestas por Flutter** (`services.dart`). No inventan comportamiento nuevo: nombran y estandarizan lo que la plataforma ya ofrece.

Los **tokens semánticos** traducen intención de producto/UX a un primitivo. El componente nunca llama `HapticFeedback` directo — consume el semántico (o `none`).

---

## Tokens primitivos — Haptics (App / Flutter)

| Token primitivo | Flutter API | Descripción técnica |
|---|---|---|
| `haptic-selection-click` | `HapticFeedback.selectionClick()` | Feedback corto para cambio de selección |
| `haptic-impact-light` | `HapticFeedback.lightImpact()` | Impacto suave de confirmación táctil |
| `haptic-impact-medium` | `HapticFeedback.mediumImpact()` | Impacto medio para énfasis |
| `haptic-impact-heavy` | `HapticFeedback.heavyImpact()` | Impacto fuerte para errores o alertas críticas |
| `haptic-vibrate` | `HapticFeedback.vibrate()` | Vibración genérica (fallback; usar con criterio) |

---

## Tokens semánticos — Haptics (App / Flutter)

| Categoría | Token semántico | Token primitivo | Flutter API resultante | Uso recomendado |
|---|---|---|---|---|
| Acción | `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` | Press en CTA primaria |
| Selección | `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` | Cambio de estado en controles de selección |
| Feedback | `haptic-feedback-success` | `haptic-impact-light` | `HapticFeedback.lightImpact()` | Confirmación exitosa |
| Feedback | `haptic-feedback-warning` | `haptic-impact-medium` | `HapticFeedback.mediumImpact()` | Advertencia con énfasis moderado |
| Feedback | `haptic-feedback-error` | `haptic-impact-heavy` | `HapticFeedback.heavyImpact()` | Error crítico |

> `haptic-selection-change` aplica a **cualquier** control de selección (Switch, Checkbox, Radio, Box Selector, Tabs, Segmented Control). No es exclusivo de Switch.

---

## Resolución rápida — App

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |
| `haptic-feedback-success` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |
| `haptic-feedback-warning` | `haptic-impact-medium` | `HapticFeedback.mediumImpact()` |
| `haptic-feedback-error` | `haptic-impact-heavy` | `HapticFeedback.heavyImpact()` |

---

## Mapeo componente → token semántico (default)

| Componente | Token default | Override por instancia | Notas |
|---|---|---|---|
| Button (primary) | `haptic-action-press` | `none` \| `default` | Contextual: onboarding intermedio puede usar `none` |
| Button (secondary) | `haptic-action-press` | `none` \| `default` | Misma regla que primary |
| Button (danger) | `haptic-feedback-warning` | `none` \| `default` | Acciones destructivas |
| Icon Button | `haptic-action-press` | `none` \| `default` | — |
| Quick Action | `haptic-action-press` | `none` \| `default` | — |
| Switch | `haptic-selection-change` | no recomendado | Siempre al cambiar estado |
| Checkbox | `haptic-selection-change` | no recomendado | — |
| Radio Button | `haptic-selection-change` | no recomendado | — |
| Box Selector | `haptic-selection-change` | no recomendado | — |
| Tabs / Segmented Control | `haptic-selection-change` | no recomendado | Al cambiar selección |
| Field Pin Code | `haptic-selection-change` | — | Por dígito o celda completada |
| Toast / Snackbar (opcional) | `haptic-feedback-success` | `none` | Solo feedback importante |
| Error crítico / validación fuerte | `haptic-feedback-error` | — | Evento puntual, no repetitivo |

### Componentes sin haptic por default

| Componente | Motivo |
|---|---|
| Field Text / Field Prefix / Text Area | No haptic por tecla; evita spam |
| Link / Breadcrumb | Navegación liviana |
| Tooltip / Skeleton / Loading | Sin acción táctil relevante |

---

## Override por instancia (Button y acciones)

El diseñador puede desactivar haptic en casos puntuales sin romper el sistema:

| Valor | Comportamiento |
|---|---|
| `default` | Usa el token semántico del variant/componente |
| `none` | No dispara haptic |

Ejemplo de criterio:
- Onboarding paso intermedio “Continuar” → `none`
- Onboarding paso final “Ingresar” → `default` (`haptic-action-press`)

---

## Implementación de referencia (Flutter)

```dart
enum GgdsHapticSemantic {
  none,
  actionPress,
  selectionChange,
  feedbackSuccess,
  feedbackWarning,
  feedbackError,
}

void triggerGgdsHaptic(GgdsHapticSemantic semantic) {
  switch (semantic) {
    case GgdsHapticSemantic.none:
      return;
    case GgdsHapticSemantic.actionPress:
    case GgdsHapticSemantic.feedbackSuccess:
      HapticFeedback.lightImpact();
      return;
    case GgdsHapticSemantic.selectionChange:
      HapticFeedback.selectionClick();
      return;
    case GgdsHapticSemantic.feedbackWarning:
      HapticFeedback.mediumImpact();
      return;
    case GgdsHapticSemantic.feedbackError:
      HapticFeedback.heavyImpact();
      return;
  }
}
```

> En producción, el mapping semántico → primitivo → API debe vivir en la capa de tokens del design system, no en cada componente.

---

## Restricciones del sistema

- **Nunca** llamar `HapticFeedback.*` directo desde componentes de UI — usar token semántico.
- **Nunca** disparar haptic en typing continuo (text fields, text area).
- **Nunca** usar haptic en cada frame de scroll, drag o animación.
- **Un evento = un haptic** (evitar spam sensorial).
- Respetar preferencias de accesibilidad del sistema (`MediaQuery.disableAnimations` y equivalentes de plataforma cuando aplique).
- `haptic-vibrate` solo como fallback documentado; no como default de componentes.

---

## Referencias

| Fuente | Link |
|---|---|
| GGDS tokens haptics | `tokens-haptics.md` |
| Flutter API | [HapticFeedback class](https://api.flutter.dev/flutter/services/HapticFeedback-class.html) |
| Apple HIG | [Playing haptics](https://developer.apple.com/design/human-interface-guidelines/playing-haptics) |
| Material M3 | [Búsqueda haptics](https://m3.material.io/search.html?q=haptics) |
| Android | [Haptics principles](https://developer.android.com/develop/ui/views/haptics/haptics-principles) |

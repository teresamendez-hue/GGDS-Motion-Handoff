# Motion Handoff — Chip (App)

| | |
|---|---|
| **Componente** | Chip |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-sm` |
| **Categoría** | Default |
| **Variantes** | Informativo, Removible, Disabled |
| **Fecha** | 2026-06-23 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./chip_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/chip_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-sm` / `motion-exit` | `motion-spring-sm` |
| Curva / física | easing-standard · 150ms / motion-exit · 100ms | mass 1.0 · stiffness 400 · damping 35 |
| Enter | opacity + scale | opacity + scale (spring) |
| Exit | motion-exit | motion-spring-sm |
| Variantes | informativo, removible, disabled | informativo, removible, disabled |
| Haptics | no aplica | `haptic-action-press` en dismiss |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Enter / exit:** el consumidor monta/desmonta; Chip define el contrato de animación.
- **Variante informativa:** sin dismiss ni haptic.
- **QA final:** validar en dispositivo con `SpringDescription` y `disableAnimations`.

---

## Motion Specification

Chip en App anima enter, exit y estados internos con `motion-spring-sm`. Sin hover. Sin skeleton.

Enter al montar en el consumidor. Exit al dismiss en variante removible. Disabled instantáneo.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros |
|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Chip mounted | chip | — | — | — | — | — |
| 2 | Response | Enter | chip | `opacity`, `scale` | `0`, `0.96` | `1`, `1` | motion-spring-sm | m:1.0, k:400, d:35 |
| 3 | Trigger | Tap down dismiss | dismiss | — | — | — | — | — |
| 4 | Response | Dismiss pressed | dismiss | `scale` | `1` | `0.92` | motion-spring-sm | m:1.0, k:400, d:35 |
| 5 | Trigger | Dismiss tap | dismiss | — | — | — | — | — |
| 6 | Response | Exit | chip | `opacity`, `scale` | `1`, `1` | `0`, `0.96` | motion-spring-sm | m:1.0, k:400, d:35 |
| 7 | Trigger | Disabled | chip | — | — | — | — | — |
| 8 | Response | Disabled | chip | `opacity` | `1` | `0.45` | none | instantáneo |

---

## Token Mapping (App)

| Token semántico | mass | stiffness | damping |
|---|---|---:|---:|
| `motion-spring-sm` | 1.0 | 400 | 35 |

---

## Implementación Flutter (Dart)

```dart
const _chipSpring = SpringDescription(
  mass: 1.0,
  stiffness: 400.0,
  damping: 35.0,
);

void animateChipPresence({
  required AnimationController controller,
  required double target,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = target;
    return;
  }
  controller.animateWith(
    SpringSimulation(_chipSpring, controller.value, target, 0.0),
  );
}
```

---

## Haptics Specification

| Campo | Valor |
|---|---|
| **Token default** | `haptic-action-press` |
| **Trigger** | tap en dismiss (solo variante removible) |
| **Variante informativa** | none |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |

---

## Implementación Haptics

```dart
void onChipDismissed(BuildContext context) {
  context.haptics.trigger(GgdsHapticSemantic.actionPress);
}
```

---

## Composición / Consumidores

| Consumidor | Uso |
|---|---|
| [Multiple Select](../../multiple-select/App/multiple-select_motion-handoff_app.html) | Chips de selección en el field |

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Token | `motion-spring-sm` enter, estados y exit |
| Hover | no en App |
| Dismiss | solo removible |
| Haptics | dismiss únicamente |
| Disabled | sin transición |
| A11y | `disableAnimations` |

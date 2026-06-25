# Motion Handoff — Field Pin Code (App)

| | |
|---|---|
| **Componente** | Field Pin Code |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-18 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./field-pin-code_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/field-pin-code_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

---

## Motion Specification

Field Pin Code en App usa motion de estado interno con spring. No aplica animación de entrada/salida de pantalla.

Estados cubiertos: `Enabled`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

Se prioriza feedback claro en foco y validación para flujo de OTP/PIN sin generar rebotes agresivos.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | `FieldPinCode` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | `AnimatedContainer` ring | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1, k:300, d:28 | 0 | 220* |
| 3 | Trigger | Digit typed | `PinCell` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Cell settle | `AnimatedScale` + color | `scale`, `surface` | `1`, default | `1.02`, active | `motion-spring-md` | m:1, k:300, d:28 | 0 | 220* |
| 5 | Trigger | Validation failed | `FieldPinCode` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Error emphasize | ring/container | `borderColor`, `opacity` | focus | error | `motion-spring-md` | m:1, k:300, d:28 | 0 | 220* |
| 7 | Trigger | Disabled | `FieldPinCode` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

\*Tiempo perceptual aproximado para equiparar `duration-300` de Web.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _fieldPinSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animatePinFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(_fieldPinSpring, focused ? 0.0 : 1.0, focused ? 1.0 : 0.0, 0.0),
  );
}
```

---

## Haptics Specification

Field Pin Code dispara haptic en dígito completado usando `haptic-selection-change`.

| Campo | Valor |
|---|---|
| **Token default** | `haptic-selection-change` |
| **Trigger** | `dígito completado` |

---

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-selection-change` | `haptic-selection-click` | `HapticFeedback.selectionClick()` |

---

## Implementación Haptics

```dart
void onDigitFilled(int index) {
  context.haptics.trigger(GgdsHapticSemantic.selectionChange); // haptic-selection-change
}
```

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo estados internos |
| Hover | no aplica en App |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |
| Coherencia | paridad de intención con Web (`motion-curve-md`) |

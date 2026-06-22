# Motion Handoff — Field Text (App)

| | |
|---|---|
| **Componente** | Field Text |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-17 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./field-text_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/field-text_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

---

## Motion Specification

Field Text en App usa motion de estado interno con spring y no aplica animaciones de entrada/salida de pantalla.

Estados cubiertos: `Enabled`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

La prioridad del motion está en legibilidad de foco y validación; por eso se usa `motion-spring-md` en lugar de `sm`.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | `FieldText` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | `AnimatedContainer` ring | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1, k:300, d:28 | 0 | 220* |
| 3 | Trigger | Input changed | `TextField` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Container settle | container | `borderColor`, `surface` | default | active | `motion-spring-md` | m:1, k:300, d:28 | 0 | 220* |
| 5 | Trigger | Validation failed | `FieldText` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Error emphasize | ring/container | `borderColor`, `opacity` | focus | error | `motion-spring-md` | m:1, k:300, d:28 | 0 | 220* |
| 7 | Trigger | Disabled | `FieldText` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

\*Tiempo perceptual aproximado para equiparar `duration-300` en Web.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _fieldTextSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateFieldFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(_fieldTextSpring, focused ? 0.0 : 1.0, focused ? 1.0 : 0.0, 0.0),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo estados internos |
| Hover | no aplica en App |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |
| Coherencia | paridad de intención con Web (`motion-curve-md`) |

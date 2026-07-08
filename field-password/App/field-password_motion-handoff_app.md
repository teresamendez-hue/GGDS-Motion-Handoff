# Motion Handoff — Field Password (App)

| | |
|---|---|
| **Componente** | Field Password |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./field-password_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/field-password_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-md` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Estados internos | hover, focus, typing, filled, error | focus, typing, filled, error |
| Toggle visibility | transición sutil de opacidad | transición sutil de opacidad |
| Haptics | no aplica | no aplica |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Haptics:** no aplica para Field Password (sin haptic por tecla).
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Field Password en App usa motion de estado interno con spring. No aplica animación de entrada/salida de pantalla.

Estados cubiertos: `Enabled`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El toggle de visibilidad usa el mismo token para feedback visual sin haptics de campo.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | `FieldPassword` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | ring wrapper | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Input changed | `TextField` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Container settle | root container | `borderColor` | default | active | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 5 | Trigger | Toggle visibility | visibility action | — | — | — | — | — | 0 | 0 |
| 6 | Response | Icon settle | toggle icon | `opacity` | default | active | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 7 | Trigger | Validation failed | `FieldPassword` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Error feedback | ring/helper | `borderColor`, `opacity` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 9 | Trigger | Disabled | `FieldPassword` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

*Asentamiento perceptual definido por el spring.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _fieldPasswordSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateFieldPasswordFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(_fieldPasswordSpring, focused ? 0.0 : 1.0, focused ? 1.0 : 0.0, 0.0),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo estados internos |
| Hover | no aplica en App |
| Haptics | no aplica |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |
| Coherencia | paridad de intención con Web (`motion-curve-md`) |

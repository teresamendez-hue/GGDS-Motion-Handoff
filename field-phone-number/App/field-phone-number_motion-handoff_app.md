# Motion Handoff — Field Phone Number (App)

| | |
|---|---|
| **Componente** | Field Phone Number |
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
| [Preview App (HTML)](./field-phone-number_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/field-phone-number_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-md` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Estados internos | hover, focus, typing, filled, error | focus, typing, filled, error |
| Country picker | dropdown del prefix (este handoff) + [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) + [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |
| Haptics | no aplica | no aplica |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Composición:** tap en el prefix abre [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html); el body del sheet usa [Option Items](../../option-item/App/option-item_motion-handoff_app.html).
- **Haptics:** no aplica para Field Phone Number.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Field Phone Number en App usa motion de estado interno con spring. No aplica animación de entrada/salida de pantalla.

Estados cubiertos: `Enabled`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El selector de país abre un Bottom Sheet al hacer tap en el prefix. Este handoff cubre los estados del field y el feedback del trigger; el motion del sheet y de las filas vive en sus handoffs enlazados.

---

## Composición

Al hacer tap en el prefix del phone number se despliega un [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) que dentro contiene [Option Items](../../option-item/App/option-item_motion-handoff_app.html) con motion propio en sus handoffs.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | input o country trigger | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | ring wrapper | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Country trigger press | country action | — | — | — | — | — | 0 | 0 |
| 4 | Response | Trigger feedback | country trigger | `opacity` | default | pressed | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 5 | Trigger | Input changed | `TextField` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Container settle | root container | `borderColor` | default | active | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 7 | Trigger | Validation failed | `FieldPhoneNumber` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 9 | Trigger | Disabled | `FieldPhoneNumber` | — | — | — | — | — | 0 | 0 |
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
const _fieldPhoneNumberSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateFieldPhoneNumberFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(_fieldPhoneNumberSpring, focused ? 0.0 : 1.0, focused ? 1.0 : 0.0, 0.0),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | estados internos del field + feedback del trigger prefix |
| Hover | no aplica en App |
| Composición | Bottom Sheet + Option Items — handoffs enlazados |
| Haptics | no aplica |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |

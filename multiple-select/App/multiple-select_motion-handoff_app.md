# Motion Handoff — Multiple Select (App)

| | |
|---|---|
| **Componente** | Multiple Select |
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
| [Preview App (HTML)](./multiple-select_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/multiple-select_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-md` / `motion-exit` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms / `motion-exit` · 200ms | mass 1.0 · stiffness 300 · damping 28 |
| Estados internos | hover, focus, filled, error | focus, filled, error |
| Lista de opciones | dropdown integrado + [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) + [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |
| Chips | [Chip](../../chip/App/chip_motion-handoff_app.html) | [Chip](../../chip/Web/chip_motion-handoff_web.html) |
| Haptics | no aplica | no aplica en el field |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Composición:** tap en el field abre [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html); el body del sheet usa [Option Items](../../option-item/App/option-item_motion-handoff_app.html).
- **Persistencia:** selecciones visibles como [Chips](../../chip/App/chip_motion-handoff_app.html) en el field — motion en handoff de Chip.
- **Haptics:** no aplica para Multiple Select field; la selección dispara haptic en Option Item.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Multiple Select en App usa motion de estado interno con spring. No aplica animación de entrada/salida de pantalla.

Estados cubiertos: `Enabled`, `Focused`, `Filled` (con chips), `Error`, `Disabled`.

Al hacer tap en el field se abre un Bottom Sheet. Este handoff cubre los estados del field y el feedback del trigger; el motion del sheet, de las filas y de los chips vive en sus handoffs enlazados.

---

## Composición

Al hacer tap en el field se despliega un [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) con [Option Items](../../option-item/App/option-item_motion-handoff_app.html) en su interior. Las opciones seleccionadas se muestran en el field como [Chips](../../chip/App/chip_motion-handoff_app.html).

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | field trigger | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | ring wrapper | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Field tap down | field trigger | — | — | — | — | — | 0 | 0 |
| 4 | Response | Trigger feedback | field trigger | `opacity` | default | pressed | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 5 | Trigger | Validation failed | `MultipleSelect` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 7 | Trigger | Disabled | `MultipleSelect` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

*Asentamiento perceptual definido por el spring.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _multipleSelectSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateMultipleSelectFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(
      _multipleSelectSpring,
      focused ? 0.0 : 1.0,
      focused ? 1.0 : 0.0,
      0.0,
    ),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | estados internos del field + feedback del trigger |
| Hover | no aplica en App |
| Composición | Bottom Sheet + Option Items — handoffs enlazados |
| Chips | [Chip](../../chip/App/chip_motion-handoff_app.html) — handoff aparte |
| Haptics | no en el field; selección en Option Item |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |

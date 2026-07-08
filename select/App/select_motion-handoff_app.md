# Motion Handoff — Select (App)

| | |
|---|---|
| **Componente** | Select |
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
| [Preview App (HTML)](./select_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../Web/select_motion-handoff_web.html) | Misma intención de motion. En App implementar con spring. |

> El preview Web muestra **cómo debe sentirse** el componente.  
> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-md` / `motion-exit` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms / `motion-exit` · 200ms | mass 1.0 · stiffness 300 · damping 28 |
| Trigger | focus o click en field | **tap** en field colapsado |
| Panel de opciones | dropdown integrado | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Búsqueda / teclado | no aplica | no aplica (lista cerrada) |
| Filas | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |
| Persistencia | valor en trigger | valor en field colapsado (sin Chip) |
| Haptics | no aplica | no aplica en el field |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Flujo:** tap en el field colapsado → abre [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) con lista de [Option Items](../../option-item/App/option-item_motion-handoff_app.html). **Sin** input de búsqueda ni teclado de filtro (diferencia vs Combobox).
- **Selección:** al elegir una opción, el sheet cierra y el valor persiste en el field.
- **Re-apertura:** la lista respeta la opción actual seleccionada y hace scroll para mantenerla visible.
- **Composición:** enter/exit del sheet → handoff Bottom Sheet; filas → Option Item.
- **Haptics:** no aplica en el field; la selección dispara haptic en Option Item.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Select en App usa motion de estado interno con spring en el **field colapsado**. No documenta la animación del Bottom Sheet ni de las filas.

Estados cubiertos: `Enabled`, `Focused`, `Pressed`, `Filled`, `Error`, `Disabled`.

Al hacer **tap** en el field se abre un Bottom Sheet con la lista de opciones predefinidas. Al seleccionar, el sheet cierra y el valor queda en el field.

Sin `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field colapsado (trigger en pantalla) | este documento |
| Apertura / cierre del sheet | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Filas de opciones | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |

Al hacer tap en el field se despliega un [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) con [Option Items](../../option-item/App/option-item_motion-handoff_app.html) en su interior. Selección única; sin Chip.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | field trigger | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | ring wrapper | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Field tap down | field trigger | — | — | — | — | — | 0 | 0 |
| 4 | Response | Trigger feedback | field trigger | `opacity` | default | pressed | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 5 | Trigger | Field tap up / sheet open | field trigger | — | — | — | — | — | 0 | 0 |
| 6 | Response | Sheet present | Bottom Sheet | `translateY`, `opacity` | off-screen | visible | `motion-spring-md` | ver handoff Bottom Sheet | 0 | settle* |
| 7 | Trigger | Option selected | Option Item | — | — | — | — | — | 0 | 0 |
| 8 | Response | Field filled | field trigger | — | placeholder | valor elegido | none | instantáneo tras cierre | 0 | 0 |
| 9 | Trigger | Validation failed | `Select` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 11 | Trigger | Disabled | `Select` | — | — | — | — | — | 0 | 0 |
| 12 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

*Asentamiento perceptual definido por el spring.

Filas 6–8: motion del sheet y de la selección en handoffs enlazados.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _selectFieldSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateSelectFieldFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(
      _selectFieldSpring,
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
| Scope | estados del field colapsado + feedback del trigger |
| Hover | no aplica en App |
| vs Combobox | sin buscador ni teclado de filtro |
| vs Multiple Select | selección única; sin Chips |
| Composición | Bottom Sheet + Option Items — handoffs enlazados |
| Re-apertura | scroll a opción actual seleccionada |
| Haptics | no en el field; selección en Option Item |
| Disabled | sin transición; no abre sheet |
| A11y | `disableAnimations` aplica estado final |

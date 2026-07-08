# Motion Handoff — Combobox (App)

| | |
|---|---|
| **Componente** | Combobox |
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
| [Preview App (HTML)](./combobox_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. |
| [Preview Web companion](../../autocomplete/Web/autocomplete_motion-handoff_web.html) | Misma intención de selección con búsqueda. En App implementar con spring + Bottom Sheet. |

> El preview Web muestra **cómo debe sentirse** la selección con filtrado.  
> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

---

## Paridad Web ↔ App

| Aspecto | Web ([Autocomplete](../../autocomplete/Web/autocomplete_motion-handoff_web.html)) | App (Combobox) |
|---------|-----|-----|
| Token semántico | `motion-curve-md` / `motion-exit` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms / `motion-exit` · 200ms | mass 1.0 · stiffness 300 · damping 28 |
| Trigger | escribir en el field de la página | **tap** en el field colapsado |
| Panel de resultados | dropdown integrado | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Input de búsqueda | en el field de la página | en la parte superior del sheet (focused + teclado) |
| Apertura de resultados | al **1.er carácter** | sheet al tap; filtrado al **1.er carácter** dentro del sheet |
| Filas | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |
| Persistencia | valor en el input | valor en el field colapsado (sin Chip) |
| Haptics | no aplica | no aplica en el field |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Flujo:** tap en el field colapsado → abre [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) con input de búsqueda en focus y teclado activo.
- **Filtrado:** a partir del **1.er carácter** tipeado en el input del sheet (el componente Search filtra desde el 3.er — no confundir).
- **Altura del sheet:** debe garantizar visibilidad mínima de **al menos 2 Option Items** en la lista.
- **Composición:** filas del sheet → [Option Items](../../option-item/App/option-item_motion-handoff_app.html); enter/exit del sheet → handoff Bottom Sheet.
- **Filtrado / sincronización:** actualización funcional de la lista; sin re-animar el sheet ni stagger en filas.
- **Haptics:** no aplica en el field; la selección dispara haptic en Option Item.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Combobox en App usa motion de estado interno con spring en el **field colapsado** de la pantalla. No documenta la animación del Bottom Sheet ni de las filas.

Estados cubiertos: `Enabled`, `Focused`, `Pressed`, `Filled`, `Error`, `Disabled`.

Al hacer **tap** en el field se abre un Bottom Sheet con el input de búsqueda en la parte superior, ya enfocado, y el teclado disparado. El filtrado de resultados comienza al escribir el primer carácter. Al seleccionar una opción, el sheet cierra y el valor persiste en el field.

Sin `Skeleton`, `Loading` ni `Readonly`.

En error: stroke rojo siempre; ring rojo solo en focus.

> En Web el equivalente es **Autocomplete** — handoff aparte.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field colapsado (trigger en pantalla) | este documento |
| Apertura / cierre del sheet | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Input de búsqueda + lista filtrada dentro del sheet | layout del sheet; motion del input interno hereda estados de field |
| Filas de resultados | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |

Al hacer tap en el field se despliega un [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html). El header del sheet incluye el input de búsqueda (focused + teclado). El body lista [Option Items](../../option-item/App/option-item_motion-handoff_app.html) filtrados desde el 1.er carácter. La altura inicial del sheet debe mostrar al menos **dos opciones** visibles.

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
| 8 | Response | Field filled | field trigger label/value | — | placeholder | valor elegido | none | instantáneo tras cierre | 0 | 0 |
| 9 | Trigger | Validation failed | `Combobox` | — | — | — | — | — | 0 | 0 |
| 10 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 11 | Trigger | Disabled | `Combobox` | — | — | — | — | — | 0 | 0 |
| 12 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

*Asentamiento perceptual definido por el spring.

Filas 6–8: el motion del sheet y de la selección vive en los handoffs enlazados. Este documento cubre el field colapsado y el feedback del trigger.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _comboboxFieldSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateComboboxFieldFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(
      _comboboxFieldSpring,
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
| Apertura | tap abre Bottom Sheet; no filtrar antes del 1.er carácter |
| Altura sheet | mínimo 2 Option Items visibles |
| vs Search | Search filtra desde el 3.er carácter; Combobox desde el 1.er |
| Composición | Bottom Sheet + Option Items — handoffs enlazados |
| Filtrado | sin re-animar sheet ni filas |
| Haptics | no en el field; selección en Option Item |
| Disabled | sin transición; no abre sheet |
| A11y | `disableAnimations` aplica estado final |

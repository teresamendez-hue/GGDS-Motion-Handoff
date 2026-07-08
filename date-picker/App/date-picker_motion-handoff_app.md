# Motion Handoff — Date Picker (App)

| | |
|---|---|
| **Componente** | Date Picker |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Default |
| **Fecha** | 2026-07-03 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./date-picker_motion-handoff_app.html) | Field + sheet con flujo borrador. Spring simulado en JS. |
| [Calendar companion](../calendar/App/calendar_motion-handoff_app.html) | Motion del grid, mes y Day Item dentro del sheet. |

> En App la selección requiere **confirmación explícita** (Aceptar). En Web el popover puede cerrar al elegir — handoff Web aparte.

---

## Paridad Web ↔ App

| Aspecto | Web (pendiente) | App (Date Picker) |
|---------|-----------------|-------------------|
| Token field | `motion-curve-md` | `motion-spring-md` |
| Contenedor | popover / dropdown | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Confirmación | selección puede cerrar sola (single) | **Aceptar** obligatorio |
| Variantes | Single / Range | Single / Range |
| Calendar interno | handoff Web aparte | [Calendar](../calendar/App/calendar_motion-handoff_app.html) |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Flujo:** tap en field → abre Bottom Sheet con [Calendar](../calendar/App/calendar_motion-handoff_app.html) + footer Borrar / Aceptar.
- **Borrador:** tap en día actualiza selección en borrador; no cierra sheet ni input hasta Aceptar.
- **Dismiss:** backdrop o Atrás del sistema descarta borrador; input sin cambios.
- **Borrar:** limpia borrador y vacía input si había fecha guardada.
- **Aceptar:** consolida borrador → valor en field → cierra sheet (motion del sheet en handoff Bottom Sheet).
- **Range:** Aceptar bloqueado si solo hay fecha de inicio — validación funcional, sin motion extra.
- **Composición:** sheet → Bottom Sheet · grid → Calendar · footer → [Button](../../button/App/button_motion-handoff_app.html).
- **Haptics:** no en field; día → Calendar; Aceptar/Borrar → Button.
- **Sin** Skeleton, Loading ni Readonly en scope motion.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Date Picker en App orquesta el **field colapsado** y el flujo de confirmación sobre un Bottom Sheet. No documenta enter/exit del sheet ni motion del grid — handoffs enlazados.

Estados del field: `Enabled`, `Focused`, `Pressed`, `Filled`, `Error`, `Disabled`. Variantes `Type=Single` y `Type=Range`.

Al hacer **tap** en el field se abre un Bottom Sheet con Calendar en el slot. La persona elige fecha(s) en **borrador** dentro del calendario. Solo **Aceptar** persiste el valor en el input y dispara el cierre del sheet. **Borrar** limpia borrador y el valor guardado. Dismiss sin confirmar restaura el estado previo del input.

Sin Skeleton, Loading ni Readonly.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field trigger en pantalla | este documento |
| Bottom Sheet enter/exit + scrim | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Calendar en el slot | [Calendar](../calendar/App/calendar_motion-handoff_app.html) |
| Footer Borrar / Aceptar | [Button](../../button/App/button_motion-handoff_app.html) |

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
| 7 | Trigger | Day tapped (borrador) | Calendar | — | — | — | — | — | 0 | 0 |
| 8 | Response | Draft updated | Calendar Day Item | contenido | anterior | nuevo día | none | instantáneo | 0 | 0 |
| 9 | Trigger | Borrar pressed | footer action | — | — | — | — | — | 0 | 0 |
| 10 | Response | Field cleared | field value | — | filled | empty | none | instantáneo | 0 | 0 |
| 11 | Trigger | Aceptar pressed | footer action | — | — | — | — | — | 0 | 0 |
| 12 | Response | Field filled | field value | — | empty/borrador | fecha confirmada | none | instantáneo | 0 | 0 |
| 13 | Response | Sheet dismiss | Bottom Sheet | `translateY`, `opacity` | visible | off-screen | `motion-spring-md` | ver handoff Bottom Sheet | 0 | settle* |
| 14 | Trigger | Dismiss (backdrop / Atrás) | — | — | — | — | — | — | 0 | 0 |
| 15 | Response | Sheet exit sin persistir | Bottom Sheet | `translateY`, `opacity` | visible | off-screen | `motion-spring-md` | ver handoff Bottom Sheet | 0 | settle* |
| 16 | Trigger | Validation error | field | — | — | — | — | — | 0 | 0 |
| 17 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 18 | Trigger | Disabled | field | — | — | — | — | — | 0 | 0 |
| 19 | Response | Disabled applied | root | `opacity` | actual | `0.45` | none | instantáneo | 0 | 0 |

Filas 7–8: motion de celda en [Calendar](../calendar/App/calendar_motion-handoff_app.html).

---

## Token Mapping

| Uso | Token semántico | mass | stiffness | damping |
|---|---|---:|---:|---:|
| Estados del field (focus, pressed, error) | `motion-spring-md` | 1.0 | 300 | 28 |
| Bottom Sheet enter/exit | `motion-spring-md` | 1.0 | 300 | 28 |

Mismo token spring en entrada y salida (regla App). Sheet: ver handoff Bottom Sheet.

---

## Implementación Flutter (Dart)

```dart
const _datePickerFieldSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateDatePickerFieldFocus({
  required AnimationController ringController,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    ringController.value = focused ? 1.0 : 0.0;
    return;
  }
  ringController.animateWith(
    SpringSimulation(
      _datePickerFieldSpring,
      ringController.value,
      focused ? 1.0 : 0.0,
      0.0,
    ),
  );
}

// Sheet: reutilizar animateBottomSheetVisibility del handoff Bottom Sheet.
// Calendar: ver calendar_motion-handoff_app.md
```

---

## Haptics Specification

| Acción | Token | Trigger |
|---|---|---|
| Field trigger | no aplica | — |
| Tap día (borrador) | `haptic-selection-change` | en Calendar |
| Aceptar / Borrar | `haptic-action-press` | onPressed en Button |

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | field + orquestación borrador/confirmar |
| Sheet | handoff Bottom Sheet — no duplicar |
| Calendar | handoff Calendar — no duplicar grid |
| App vs Web | confirmación explícita en App |
| Range | Aceptar requiere inicio y fin |
| Dismiss | descarta borrador |
| Skeleton | fuera de scope |
| Disabled | sin transición; no abre sheet |

# Motion Handoff — Time Picker (App)

| | |
|---|---|
| **Componente** | Time Picker |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Default |
| **Fecha** | 2026-07-06 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./time-picker_motion-handoff_app.html) | Field + sheet con lista de horarios. Spring simulado en JS. |
| [Time Picker Web companion](../Web/time-picker_motion-handoff_web.html) | Paridad de flujo — Web usa popover integrado en lugar de Bottom Sheet. |

> En App la selección confirma en **un solo tap** — sin footer Cancelar ni Aceptar.

---

## Paridad Web ↔ App

| Aspecto | Web (Time Picker) | App (Time Picker) |
|---------|-------------------|-------------------|
| Token field | `motion-curve-md` | `motion-spring-md` |
| Contenedor | popover integrado | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Confirmación | un clic en Option Item | un tap en Option Item |
| Footer Cancelar/Aceptar | no | no |
| Smart default | visible en field desde inicio | al abrir sheet (scroll al sugerido) |
| Formato hora | 12h AM/PM (Figma) | 12h AM/PM (Figma) |
| Filas | [Option Item](../../option-item/Web/option-item_motion-handoff_web.html) | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Flujo:** tap en field → abre [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) con lista de slots de **15 min** como [Option Items](../../option-item/App/option-item_motion-handoff_app.html).
- **Smart default:** input vacío → hora actual redondeada al **próximo bloque de 15 min**.
- **Auto-scroll:** al terminar enter del sheet, posicionar instantáneamente el valor sugerido o el guardado — **sin motion de scroll**.
- **Selección:** un tap en fila → valor formateado en field + cierre del sheet. Sin confirmación explícita.
- **Dismiss:** backdrop, swipe down o Atrás del sistema → input sin cambios.
- **Composición:** sheet → Bottom Sheet · filas → Option Item. Sin duplicar motion de compos.
- **Haptics:** no en field; selección → `haptic-selection-change` en Option Item.
- **Sin** Skeleton, Loading ni Readonly en scope motion.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Time Picker en App orquesta el **field colapsado** y la selección directa sobre un Bottom Sheet con lista de horarios. No documenta enter/exit del sheet ni motion de filas — handoffs enlazados.

Estados del field: `Enabled`, `Focused`, `Pressed`, `Filled`, `Error`, `Disabled`. Formato **12h AM/PM** según Figma.

Al hacer **tap** en el field se abre un Bottom Sheet con Option Items (slots cada 15 min). Si el input está vacío, el sistema calcula smart default y hace scroll instantáneo a esa fila en estado Selected. Al elegir una fila, el valor persiste en el field y el sheet cierra.

Sin Skeleton, Loading ni Readonly.

En error: stroke rojo siempre; ring rojo solo en focus.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field trigger en pantalla | este documento |
| Bottom Sheet enter/exit + scrim | [Bottom Sheet](../../bottom-sheet/App/bottom-sheet_motion-handoff_app.html) |
| Filas de horarios (slots 15 min) | [Option Item](../../option-item/App/option-item_motion-handoff_app.html) |

Sin footer Borrar/Aceptar. Sin buscador ni teclado.

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
| 7 | Response | Auto-scroll a sugerido | lista interna | `scrollOffset` | `0` | target | none | instantáneo post-enter | 0 | 0 |
| 8 | Trigger | Option Item tapped | Option Item | — | — | — | — | — | 0 | 0 |
| 9 | Response | Selected + field filled | Option Item, field | contenido | previo | hora elegida | none | instantáneo | 0 | 0 |
| 10 | Response | Sheet dismiss | Bottom Sheet | `translateY`, `opacity` | visible | off-screen | `motion-spring-md` | ver handoff Bottom Sheet | 0 | settle* |
| 11 | Trigger | Dismiss (backdrop / swipe / Atrás) | — | — | — | — | — | — | 0 | 0 |
| 12 | Response | Sheet exit sin persistir | Bottom Sheet | `translateY`, `opacity` | visible | off-screen | `motion-spring-md` | ver handoff Bottom Sheet | 0 | settle* |
| 13 | Trigger | Validation error | field | — | — | — | — | — | 0 | 0 |
| 14 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 15 | Trigger | Disabled | field | — | — | — | — | — | 0 | 0 |
| 16 | Response | Disabled applied | root | `opacity` | actual | `0.45` | none | instantáneo | 0 | 0 |

Filas 6–10: motion de sheet y fila en handoffs enlazados.

---

## Token Mapping

| Uso | Token semántico | mass | stiffness | damping |
|---|---|---:|---:|---:|
| Estados del field (focus, pressed, error) | `motion-spring-md` | 1.0 | 300 | 28 |
| Bottom Sheet enter/exit | `motion-spring-md` | 1.0 | 300 | 28 |

Mismo token spring en entrada y salida (regla App). Sheet y filas: ver handoffs enlazados.

---

## Implementación Flutter (Dart)

```dart
const _timePickerFieldSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateTimePickerFieldFocus({
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
      _timePickerFieldSpring,
      ringController.value,
      focused ? 1.0 : 0.0,
      0.0,
    ),
  );
}

// Sheet: reutilizar animateBottomSheetVisibility del handoff Bottom Sheet.
// Filas: ver option-item_motion-handoff_app.md
// Auto-scroll inicial: jumpTo sin animación tras onSheetOpened.
```

---

## Haptics Specification

| Acción | Token | Trigger |
|---|---|---|
| Field trigger | no aplica | — |
| Tap en Option Item (hora) | `haptic-selection-change` | onTap en Option Item |

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | field + orquestación selección directa |
| vs Date Picker App | sin borrador ni Aceptar/Borrar |
| vs Select App | misma confirmación en un tap; slots de tiempo |
| Smart default | redondeo 15 min hacia adelante |
| Auto-scroll | instantáneo; sin animar scroll |
| Sheet / filas | handoffs enlazados — no duplicar |
| Skeleton | fuera de scope |
| Disabled | sin transición; no abre sheet |

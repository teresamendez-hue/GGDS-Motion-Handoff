# Motion Handoff — Calendar (App)

| | |
|---|---|
| **Componente** | Calendar |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` / `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-07-03 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./calendar_motion-handoff_app.html) | Grid, mes y selección. Spring simulado en JS. |
| [Date Picker companion](../date-picker/App/date-picker_motion-handoff_app.html) | Orquestación field + sheet + footer. |

> Calendar vive dentro del Bottom Sheet de [Date Picker](../date-picker/App/date-picker_motion-handoff_app.html). También puede consumirse en otros flujos de fecha.

---

## Paridad Web ↔ App

| Aspecto | Web (pendiente) | App (Calendar) |
|---------|-----------------|----------------|
| Token mes / vista | `motion-curve-md` | `motion-spring-md` |
| Token celda | `motion-curve-sm` | `motion-spring-sm` |
| Day Item | átomo interno | átomo interno |
| Vistas | Month / Year | Month / Year |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Scope:** header, cambio de mes/vista, Day Item. No documenta enter/exit del Bottom Sheet.
- **Day Item:** átomo `_Calendar/Day Item` — estados Enabled, Focused, Active, Disabled; `Present` (hoy); `Range` None/Start/Middle/End.
- **Cambio de mes:** slide horizontal + fade con `motion-spring-md`.
- **Relleno de rango:** actualización visual instantánea — sin re-animar el grid.
- **Icon Buttons** del header: [Icon Button](../../icon-button/App/icon-button_motion-handoff_app.html).
- **Haptics:** `haptic-selection-change` al tap en día.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Calendar en App es el componente de selección de fechas dentro de un Bottom Sheet. Incluye vistas **Month** y **Year**, header con mes/año y navegación, y grid de **Day Items**.

Al tap en un día, la celda transiciona a estado Active/Focused con `motion-spring-sm`. El marcador de borrador se mueve si la persona elige otro día — sin cerrar el contenedor. En **Range**, el relleno entre inicio y fin se actualiza de forma funcional al elegir el límite o al paginar entre meses (estado cross-month persistido sin re-enter del panel).

Al cambiar de mes con los Icon Buttons del header, el grid hace slide horizontal con `motion-spring-md`.

Sin Skeleton, Loading ni Readonly.

---

## Calendar Day Item (átomo)

Átomo `_Calendar/Day Item`. No es un componente DS independiente.

| Variante | Uso |
|---|---|
| **Present** | Marca el día actual |
| **Range: Start / Middle / End** | Selección de rango |
| **Active / Focused** | Día en borrador o seleccionado |

Estados: `Enabled`, `Focused`, `Active`, `Active Focused`, `Disabled`. Motion de celda: `motion-spring-sm` en `backgroundColor`. Relleno de rango entre celdas: sin motion.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Mes anterior / siguiente | Icon Button header | — | — | — | — | — | 0 | 0 |
| 2 | Response | Month slide | grid container | `translateX`, `opacity` | offset | `0`, `1` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Cambio Month ↔ Year | header control | — | — | — | — | — | 0 | 0 |
| 4 | Response | Vista swap | Month / Year panel | contenido | mes | año | none | instantáneo | 0 | 0 |
| 5 | Trigger | Pointer down | Day Item | — | — | — | — | — | 0 | 0 |
| 6 | Response | Pressed | Day Item | `backgroundColor` | default | pressed | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle* |
| 7 | Trigger | Day selected (borrador) | Day Item | — | — | — | — | — | 0 | 0 |
| 8 | Response | Active state | Day Item | `backgroundColor` | default | active | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle* |
| 9 | Trigger | Range end selected | Day Item | — | — | — | — | — | 0 | 0 |
| 10 | Response | Range fill | celdas intermedias | `backgroundColor` | none | middle | none | instantáneo | 0 | 0 |
| 11 | Trigger | Cross-month navigation | Icon Button | — | — | — | — | — | 0 | 0 |
| 12 | Response | Persist range state | grid | contenido | rango previo | rango visible | none | instantáneo | 0 | 0 |

---

## Token Mapping

| Uso | Token semántico | mass | stiffness | damping |
|---|---|---:|---:|---:|
| Cambio de mes (slide) | `motion-spring-md` | 1.0 | 300 | 28 |
| Day Item (pressed, active, focus) | `motion-spring-sm` | 1.0 | 400 | 35 |

Mismo token en entrada y salida de cada capa (regla App).

---

## Implementación Flutter (Dart)

```dart
const _calendarMonthSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

const _calendarDaySpring = SpringDescription(
  mass: 1.0,
  stiffness: 400.0,
  damping: 35.0,
);

void animateCalendarMonth({
  required AnimationController controller,
  required bool forward,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = 1.0;
    return;
  }
  controller.value = 0.0;
  controller.animateWith(
    SpringSimulation(_calendarMonthSpring, 0.0, 1.0, 0.0),
  );
}
```

---

## Haptics Specification

| Acción | Token | Trigger |
|---|---|---|
| Tap en Day Item | `haptic-selection-change` | onDaySelected |
| Icon Button navegación | no aplica (Icon Button handoff) | — |

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | grid + header + Day Item |
| Day Item | átomo interno — no handoff aparte |
| Rango | relleno instantáneo; persistencia cross-month |
| Mes | slide horizontal; sin stagger en celdas |
| Vista Year | swap de contenido sin re-animar sheet |
| Consumidor | Date Picker enlaza este handoff |

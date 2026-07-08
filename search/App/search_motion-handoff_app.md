# Motion Handoff — Search (App)

| | |
|---|---|
| **Componente** | Search |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` / `motion-spring-sm` |
| **Categoría** | Default |
| **Fecha** | 2026-06-23 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./search_motion-handoff_app.html) | Spring simulado en JS. Tabs **Full-screen** e **In-page**. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/search_motion-handoff_web.html) | Misma intención de field + overlay. En App implementar con tokens **spring**. |

> El preview Web muestra **cómo debe sentirse** el field y el overlay.  
> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token field / overlay | `motion-curve-md` / `motion-exit` | `motion-spring-md` (mismo en salida overlay y modal) |
| Token clear | `motion-curve-sm` · 150ms | `motion-spring-sm` |
| Curva / física | `easing-decelerate` · 300ms / `motion-exit` · 200ms | mass 1.0 · stiffness 300 · damping 28 |
| Modos App | — | `fullscreen` (modal) · `inpage` (overlay) |
| Umbral panel | ≥ 3 chars o `showMenuOnFocus` | igual |
| Filas de lista | sin motion en este handoff |
| Haptics field | no aplica | no aplica |
| Disabled | sin transición | instantáneo (`disableAnimations`) |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Variante:** prop `interactionMode` → `fullscreen` | `inpage`.
- **Full-screen:** tap en Search colapsado → modal pantalla completa; Search en header; Atrás/Cancelar cierra sin cambios.
- **In-page:** tap → focus en sitio + overlay; cierre por tap fuera o Back del sistema.
- **Umbral:** < 3 chars historial/sugerencias; ≥ 3 chars resultados activos.
- **Clear (X):** vuelve a estado inicial de lista / contenido de pantalla.
- **Composición:** filas de resultados fuera de scope de motion Search.
- **Haptics:** no en el field.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Search en App documenta dos modos de interacción en un solo handoff. Ambos comparten motion del **input** (estados, clear) y difieren en el **contenedor de resultados**.

### Modo Full-screen

Para búsqueda como acción principal (home e-commerce, buscador global). Tap en el Search colapsado dispara transición a vista modal de pantalla completa (`motion-spring-md`). El Search se posiciona en el header del modal con foco y teclado automáticos. Cierre por Atrás/Cancelar (sin cambios) o por selección (ejecuta búsqueda y navega).

### Modo In-page

Para búsqueda secundaria (filtrar contenido en pantalla). Tap mantiene el field en su posición; overlay de resultados entra con `motion-spring-md`. Cierre: tap fuera, Back del sistema o selección. Sin botón Cancelar.

En ambos modos: clear visible solo con texto; al borrar vuelve al estado inicial de lista. Filtrado y swap historial → dinámico: **sin motion adicional**.

Sin `Skeleton`, `Loading` ni `Readonly`.

---

## Composición

| Pieza | Handoff |
|---|---|
| Field + clear (ambos modos) | este documento |
| Modal full-screen (modo `fullscreen`) | enter/exit documentados aquí · `motion-spring-md` |
| Overlay in-page (modo `inpage`) | enter/exit documentados aquí · `motion-spring-md` |
| Filas de resultados | fuera de scope — motion en handoff del componente de lista |

---

## Timeline de interacción (App)

### Field + clear (ambos modos)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | input Search | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | ring wrapper | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Input filled | input Search | — | — | — | — | — | 0 | 0 |
| 4 | Response | Clear in | clear button | `opacity` | `0` | `1` | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle* |
| 5 | Trigger | Clear pressed | clear button | — | — | — | — | — | 0 | 0 |
| 6 | Response | Clear out | clear button | `opacity` | `1` | `0` | `motion-spring-sm` | m:1.0, k:400, d:35 | 0 | settle* |
| 7 | Trigger | Disabled | Search root | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled applied | root | `opacity` | actual | `0.45` | none | instantáneo | 0 | 0 |

### Modo Full-screen — modal

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 9 | Trigger | Tap Search colapsado | field trigger | — | — | — | — | — | 0 | 0 |
| 10 | Response | Modal enter | full-screen view | `translateY`, `opacity` | off-screen / `0` | `0` / `1` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 11 | Trigger | Atrás / Cancelar | header action | — | — | — | — | — | 0 | 0 |
| 12 | Response | Modal exit | full-screen view | `translateY`, `opacity` | `0` / `1` | off-screen / `0` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 13 | Trigger | Result selected | fila de resultados | — | — | — | — | — | 0 | 0 |
| 14 | Response | Modal exit + navegación | full-screen view | `translateY`, `opacity` | visible | off-screen | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |

### Modo In-page — overlay

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 15 | Trigger | Tap Search | field in-page | — | — | — | — | — | 0 | 0 |
| 16 | Response | Overlay enter | results panel | `opacity`, `translateY` | `0`, offset | `1`, `0` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 17 | Trigger | Tap fuera / Back / selección | — | — | — | — | — | — | 0 | 0 |
| 18 | Response | Overlay exit | results panel | `opacity`, `translateY` | `1`, `0` | `0`, offset | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |

\* *settle* = asentamiento del spring según mass/stiffness/damping.

Filas 9–18: swap de contenido de lista (< 3 vs ≥ 3 chars) sin filas de motion adicionales.

---

## Token Mapping

| Uso | Token semántico | mass | stiffness | damping |
|---|---|---|---|---|
| Estados del field (focus, error, typing) | `motion-spring-md` | 1.0 | 300 | 28 |
| Clear button enter/exit | `motion-spring-sm` | 1.0 | 400 | 35 |
| Modal full-screen enter/exit | `motion-spring-md` | 1.0 | 300 | 28 |
| Overlay in-page enter/exit | `motion-spring-md` | 1.0 | 300 | 28 |

Mismo token spring en entrada y salida de cada capa (regla App).

---

## Implementación Flutter (Dart)

```dart
const _searchFieldSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

const _searchClearSpring = SpringDescription(
  mass: 1.0,
  stiffness: 400.0,
  damping: 35.0,
);

void animateSearchOverlay({
  required AnimationController controller,
  required bool visible,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) {
    controller.value = visible ? 1.0 : 0.0;
    return;
  }
  controller.animateWith(
    SpringSimulation(
      _searchFieldSpring,
      controller.value,
      visible ? 1.0 : 0.0,
      0.0,
    ),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Dos modos | un handoff; prop `interactionMode` |
| Full-screen | acción principal; modal + Atrás |
| In-page | tarea secundaria; overlay + tap fuera |
| Umbral 3 chars | historial < 3 · resultados ≥ 3 |
| Clear | restaura estado inicial de lista |
| vs Combobox | Combobox usa Bottom Sheet; Search fullscreen usa modal |
| Haptics field | no |
| Lista | fuera de scope Search |

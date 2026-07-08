# Motion Handoff — Bottom Sheet (App)

| | |
|---|---|
| **Componente** | Bottom Sheet |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-23 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./bottom-sheet_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |

> En Web, selecciones equivalentes usan **Dropdown** (handoff aparte). Este documento cubre solo App.

---

## Paridad Web ↔ App

| Aspecto | Web (Dropdown) | App (Bottom Sheet) |
|---------|----------------|---------------------|
| Token semántico | handoff aparte | `motion-spring-md` |
| Física | curva CSS | mass 1.0 · stiffness 300 · damping 28 |
| Entrada / salida | viewport del panel | `translateY` + `opacity` + scrim |
| Altura | según contenido | adaptativa o con scroll (~80% viewport) |
| Cierre gestual | no aplica | swipe down en handle |

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Altura:** dos variantes de **layout** en Figma (mismo motion):
  - **Adaptativa:** el sheet crece con el contenido.
  - **Con scroll:** si el contenido alcanza **~80% del viewport**, usar `max-height` ~80% y scroll en el body interno.
- **Contenido interno** (Option Item, busqueda): motion documentado por separado.
- **Haptics:** no aplica al abrir/cerrar el sheet.
- **QA final:** validar en dispositivo con `SpringDescription` del design system.

---

## Motion Specification

Bottom Sheet en App es un overlay de viewport que entra desde abajo. Combina desplazamiento vertical del panel y fade del scrim para mantener jerarquia espacial.

Cierre programatico (scrim tap, accion primaria, seleccion que cierra) y cierre por **swipe down** en el handle comparten `motion-spring-md`.

Sin scale en entrada. Sin variantes half/full fijas.

**Variantes de layout:** adaptativa (contenido corto) o con scroll interno cuando el contenido requiere ~80% del viewport. El token y la animacion del contenedor no cambian.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Open requested | `BottomSheet` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Sheet enter | panel | `translateY`, `opacity` | `100%`, `0` | `0`, `1` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Response | Scrim enter | scrim | `opacity` | `0` | `1` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 4 | Trigger | Close requested | `BottomSheet` | — | — | — | — | — | 0 | 0 |
| 5 | Response | Sheet exit | panel | `translateY`, `opacity` | `0`, `1` | `100%`, `0` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 6 | Response | Scrim exit | scrim | `opacity` | `1` | `0` | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 7 | Trigger | Swipe drag | handle / header | — | — | — | — | — | 0 | 0 |
| 8 | Response | Follow finger | panel | `translateY` | posicion actual | offset drag | none | 1:1 con gesto | 0 | 0 |
| 9 | Trigger | Swipe release | handle / header | — | — | — | — | — | 0 | 0 |
| 10 | Response | Dismiss or snap back | panel | `translateY` | offset | `100%` o `0` | `motion-spring-md` | umbral ~35% altura | 0 | settle* |

*Asentamiento perceptual definido por el spring.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _bottomSheetSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animateBottomSheetVisibility({
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
      _bottomSheetSpring,
      controller.value,
      visible ? 1.0 : 0.0,
      0.0,
    ),
  );
}

// Panel: translateY = lerp(1.0, 0.0, controller.value)
// Scrim: opacity = controller.value
// Swipe: actualizar offset durante drag; al soltar, spring a 0 o 1 segun umbral
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | viewport del sheet + scrim |
| Altura adaptativa | contenido corto: sheet crece con el hijo |
| Variante scroll | si contenido >= ~80% viewport: max-height ~80% + scroll interno en body |
| Header | superficie clara (sin header oscuro); handle + title + description |
| Scale | no usar |
| Swipe | solo en zona handle/header |
| Haptics | no en open/close |
| A11y | `disableAnimations` aplica estado final |
| Coherencia | mismo token que drawers/navegacion guia en GGDS |

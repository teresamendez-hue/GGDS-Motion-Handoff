# Motion Handoff — Field Prefix (App)

| | |
|---|---|
| **Componente** | Field Prefix |
| **Plataforma** | App (Flutter) |
| **Owner** | Teresa Mendez |
| **Design system** | GGDS |
| **Token semántico** | `motion-spring-md` |
| **Categoría** | Default |
| **Fecha** | 2026-06-18 |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./field-prefix_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/field-prefix_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra como debe sentirse el componente.  
> El preview App y el Token Mapping muestran como implementarlo en Flutter.

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-md` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Entrada / salida (viewport) | no aplica (componente interno) | mismo spring para todos los cambios internos |
| Estados internos | hover, focus, typing, filled, error | focus, typing, filled, error |
| Disabled | sin transición | instantáneo (`disableAnimations`) |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

---

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

Field Prefix en App usa motion de estado interno con spring. No aplica animación de entrada/salida de pantalla.

Estados cubiertos: `Enabled`, `Focused`, `Typing`, `Filled`, `Error`, `Disabled`.

El prefijo se mantiene estable; el feedback de motion se concentra en anillo externo, contenedor y helper/status.

---

## Timeline de interacción (App)

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Focus gained | `FieldPrefix` | — | — | — | — | — | 0 | 0 |
| 2 | Response | Ring activate | ring wrapper | `opacity`, `borderColor` | `0`, neutral | `1`, focus | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 3 | Trigger | Input changed | `TextField` | — | — | — | — | — | 0 | 0 |
| 4 | Response | Container settle | root container | `borderColor`, `surface` | default | active | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 5 | Trigger | Validation failed | `FieldPrefix` | — | — | — | — | — | 0 | 0 |
| 6 | Response | Error feedback | ring/helper | `borderColor`, `color` | focus/default | error | `motion-spring-md` | m:1.0, k:300, d:28 | 0 | settle* |
| 7 | Trigger | Disabled | `FieldPrefix` | — | — | — | — | — | 0 | 0 |
| 8 | Response | Disabled applied | root | `opacity` | estado actual | `0.45` | none | instantáneo | 0 | 0 |

\*Asentamiento perceptual definido por el spring.

---

## Token Mapping (App)

| Token semántico | Spring primitivo | mass | stiffness | damping |
|---|---|---:|---:|---:|
| `motion-spring-md` | `spring-standard-md` | 1.0 | 300 | 28 |

---

## Implementación Flutter (Dart)

```dart
const _fieldPrefixSpring = SpringDescription(
  mass: 1.0,
  stiffness: 300.0,
  damping: 28.0,
);

void animatePrefixFocus({
  required AnimationController controller,
  required bool focused,
  required BuildContext context,
}) {
  if (MediaQuery.of(context).disableAnimations) return;
  controller.animateWith(
    SpringSimulation(_fieldPrefixSpring, focused ? 0.0 : 1.0, focused ? 1.0 : 0.0, 0.0),
  );
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Scope | solo estados internos |
| Hover | no aplica en App |
| Prefix | estable, sin animación independiente |
| Disabled | sin transición |
| A11y | `disableAnimations` aplica estado final |

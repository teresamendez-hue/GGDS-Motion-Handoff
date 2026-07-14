# Motion Handoff — Coachmark (App)

| | |
|---|---|
| **Componente** | Coachmark |
| **Plataforma** | App (Flutter) |
| **Owner** | Nehuén Benitez |
| **Design system** | GGDS |
| **Token entrada/salida** | `motion-spring-md` |
| **Token cambio de step** | `motion-spring-sm` |
| **Categoría** | Guía |
| **Fecha** | 2026-06-25 |
| **Figma** | [Hand-Off Core App](https://www.figma.com/design/FvgtR93pPCj97CQbNjUAqu/Hand-Off---Core-App?node-id=1146-21696) |

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./coachmark_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/coachmark_motion-handoff_web.html) | Misma **intención** de motion. En App implementar con tokens **spring**, no curvas CSS. |

> El preview Web muestra **cómo debe sentirse** el componente.  
> El preview App y el Token Mapping muestran **cómo implementarlo** en Flutter.

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token entrada | `motion-curve-md` | `motion-spring-md` |
| Curva / física | `easing-decelerate` · 300ms | mass 1.0 · stiffness 300 · damping 28 |
| Token salida suave | `motion-exit` · 200ms | `motion-spring-md` (mismo spring invertido) |
| Salida forzada | instantánea | instantánea |
| Cambio de step | `motion-curve-sm` · 150ms | `motion-spring-sm` |
| Propiedades | opacity + translate 8px eje puntero | opacity + translate 8px eje puntero |
| Puntero | Top/Bottom/Left/Right + auto-flip | Top/Bottom + Left/Center/Right |
| Scrim | sin animación | sin animación |

Regla: no traducir ms de Web a duración fija en Flutter. El spring define el asentamiento.

**Paridad perceptual (default):** entrada, salida y cambio de step deben **sentirse igual** en Web y App — mismas propiedades animadas, mismos desplazamientos (p. ej. 8px), misma duración percibida en crossfade de step (~150ms × 2 en coachmark). Los tokens difieren (curva vs spring) pero la intención de motion es una sola. Excepciones solo si el handoff lo documenta explícitamente (p. ej. swipe dismiss solo App).

**Preview App:** el cambio de step usa 150ms × 2 en JS para alinear la percepción con Web (`motion-curve-sm`); en Flutter implementar con `motion-spring-sm` según Token Mapping.

## Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS; parámetros idénticos al token.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo o simulador con `flutter_animate` / `SpringDescription` del design system.

---

## Motion Specification

El Coachmark en App guía en un tour contextual anclado a un elemento target. Panel de 320px, fondo `accent-1` (#d9e1b8), puntero triangular, título, descripción, contador de pasos y acciones Primary/Secondary + X. **No anima scrim ni overlay** — el contenido subyacente permanece visible.

Entrada y salida suave usan `motion-spring-md` (mass 1.0, stiffness 300, damping 28): el **mismo token** en ambas direcciones, animando `opacity` y `translate` 8px en el eje del puntero. Salida forzada (Escape, target off-screen) es instantánea — sin spring ni curva alternativa.

El avance entre pasos crossfadea el contenido interno con `motion-spring-sm` (mass 1.0, stiffness 400, damping 35). En saltos grandes de posición, el panel completo puede re-entrar con `motion-spring-md`.

Variantes Top/Bottom + Left/Center/Right afectan layout, no tokens. Sin haptic en aparición; botones disparan `haptic-action-press` con override `none` opcional en pasos intermedios.

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración/Parámetros | Inicio (ms) | Fin (ms) |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Tour inicia / step se muestra (programmatic) | — | — | — | — | — | — | 0 | — |
| 2 | Response | Coachmark entra | `.coachmark` | `opacity` | 0 | 1 | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | 0 | settle |
| 3 | Response | Coachmark entra | `.coachmark` | `translate` (eje puntero) | 8px | 0 | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | 0 | settle |
| 4 | Trigger | Tap en X | — | — | — | — | — | — | * | — |
| 5 | Response | Coachmark sale (suave) | `.coachmark` | `opacity` | 1 | 0 | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | * | settle |
| 6 | Response | Coachmark sale (suave) | `.coachmark` | `translate` (eje puntero) | 0 | 8px | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | * | settle |
| 7 | Trigger | Tap en Secondary (Omitir) | — | — | — | — | — | — | * | — |
| 8 | Response | Mismo spring que filas 5–6 | `.coachmark` | `opacity`, `translate` | visible | oculto | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | * | settle |
| 9 | Trigger | Tap en Primary en último step (Finalizar) | — | — | — | — | — | — | * | — |
| 10 | Response | Mismo spring que filas 5–6 | `.coachmark` | `opacity`, `translate` | visible | oculto | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | * | settle |
| 11 | Trigger | Tap en Primary (no último step) | — | — | — | — | — | — | * | — |
| 12 | Response | Crossfade contenido | `.coachmark-body` | `opacity` | 1 | 0 → 1 | `motion-spring-sm` | mass 1.0 · stiffness 400 · damping 35 | * | settle |
| 13 | Response | Reposicionar panel (salto grande) | `.coachmark` | `opacity`, `translate` | oculto | visible | `motion-spring-md` | mass 1.0 · stiffness 300 · damping 28 | * | settle |
| 14 | Trigger | Escape / target off-screen | — | — | — | — | — | — | * | — |
| 15 | Response | Cierre forzado | `.coachmark` | todas | visible | oculto | none | 0ms | * | * |

> `*` = momento variable según interacción.

---

## Token Mapping

| Momento | Token semántico | mass | stiffness | damping |
|---|---|---:|---:|---:|
| Entrada / salida suave | `motion-spring-md` | 1.0 | 300 | 28 |
| Cambio de step | `motion-spring-sm` | 1.0 | 400 | 35 |
| Salida forzada | — | — | — | instantáneo |

---

## Implementación Dart

```dart
const motionSpringMd = SpringDescription(
  mass: 1.0,
  stiffness: 300,
  damping: 28,
);

const motionSpringSm = SpringDescription(
  mass: 1.0,
  stiffness: 400,
  damping: 35,
);

final disableAnimations = MediaQuery.of(context).disableAnimations;

Future<void> showCoachmark(CoachmarkStep step) async {
  if (disableAnimations) {
    coachmarkController.setVisible();
    return;
  }
  await coachmarkController.animateIn(
    spring: motionSpringMd,
    opacity: const Tuple2(0.0, 1.0),
    translateAxis: const Tuple2(8.0, 0.0),
  );
}

Future<void> dismissCoachmarkSoft() async {
  if (disableAnimations) {
    coachmarkController.setHidden();
    return;
  }
  await coachmarkController.animateOut(
    spring: motionSpringMd,
    opacity: const Tuple2(1.0, 0.0),
    translateAxis: const Tuple2(0.0, 8.0),
  );
}

void dismissCoachmarkForced() {
  coachmarkController.setHidden();
}

Future<void> advanceStep(CoachmarkStep next) async {
  if (disableAnimations) {
    coachmarkController.setContent(next);
    return;
  }
  await coachmarkController.crossfadeContent(
    spring: motionSpringSm,
    opacity: const Tuple2(1.0, 0.0),
    onMidpoint: () => coachmarkController.setContent(next),
    opacityIn: const Tuple2(0.0, 1.0),
  );
}
```

---

## Haptics Specification

| Evento | Token | Override |
|---|---|---|
| Aparición del coachmark | ninguno | — |
| Primary / Secondary / X | `haptic-action-press` | `none` \| `default` en primary de pasos intermedios |

## Token Mapping Haptics

| Token semántico | Token primitivo | Flutter API |
|---|---|---|
| `haptic-action-press` | `haptic-impact-light` | `HapticFeedback.lightImpact()` |

## Implementación Haptics

```dart
final primaryHaptic = step.primaryHaptic ?? GgdsHapticSemantic.actionPress;

void onPrimaryPressed() {
  if (primaryHaptic != GgdsHapticSemantic.none) {
    context.haptics.trigger(primaryHaptic);
  }
  advanceStep();
}

void onSecondaryPressed() {
  context.haptics.trigger(GgdsHapticSemantic.actionPress);
  dismissCoachmarkSoft();
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Springs | `motion-spring-md` entrada/salida; `motion-spring-sm` cambio de step |
| Salida forzada | Instantánea — no usar spring |
| Scrim | Sin animación de overlay |
| Haptics | Sin haptic en appear; `haptic-action-press` en botones |
| Override | `none` en primary de pasos intermedios si el tour es largo |
| Scroll | Coachmark sigue target vía layout; auto-close off-screen |
| Persistencia | X/Omitir dismiss permanente (lógica de negocio) |
| Accesibilidad | `MediaQuery.disableAnimations`: estado final sin spring |
| QA | Validar en dispositivo; preview HTML no sustituye Flutter |

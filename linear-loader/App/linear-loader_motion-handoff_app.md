# Motion Handoff — Linear Loader (App)

| | |
|---|---|
| **Componente** | Linear Loader |
| **Plataforma** | App (Flutter) |
| **Owner** | Juan Carlos De La Vía |
| **Design system** | GGDS |
| **Token loop** | `motion-spring-md` |
| **Categoría** | Guía |
| **Fecha** | 2026-09-15 |
| **Figma** | [Loaders / Linear Loader](https://www.figma.com/design/7rFqT5ZUPPXd40dKQVAd7N/branch/0HYJJ9VrKoturiGYzEIh50/Core---App-Components?node-id=13663-2236) |

---

## Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./linear-loader_motion-handoff_app.html) | Loop **continuo** (2000ms / ciclo) con las mismas funciones `left(t)` / `width(t)` que Flutter. No sustituye QA en dispositivo. |

> El preview HTML muestra **cómo implementarlo** en Flutter.  
> La UI final (color, track, proporciones) está en Figma.

---

## Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Handoff en repo | no documentado | `linear-loader/App/` |
| Token semántico | — | `motion-spring-md` |
| Motor del loop | — | `AnimationController.repeat()` + muestreo continuo |
| Loop indeterminado | — | barrido **solo hacia la derecha**; sale por el borde derecho y **reentra por la izquierda** |
| Gap shape ↔ track | — | **4px** (`spacing` / gap Figma) en el corredor visible |

**Nota:** referencia visual externa (p. ej. barra indeterminada tipo Atlaskit) solo orienta la **intención** del barrido; la fuente de verdad de motion es este documento y Figma.

---

## Nota para desarrollo

- **Fuente de verdad:** funciones periódicas + Token Mapping + snippet Dart de este documento.
- **No encadenar** `SpringSimulation` fase a fase con settle: eso produce “pasos” con frenos. El indeterminado debe ser **un solo loop continuo**.
- **QA final:** validar fluidez en dispositivo; ajustar solo `cycle_ms` si hace falta, no agregar paradas.

---

## Motion Specification

Indicador lineal **indeterminado**. Variante **Color**: accent, neutral, white. Track **4px** alto; shape activa **120px** de ancho (referencia Figma sobre **W = 360px**). **Gap fijo 4px** entre shape y tramos de track inactivo (layout Figma: `gap: 4px` entre pills).

**Única animación documentada:** la shape se desplaza **solo hacia la derecha** a velocidad constante, **sale por el borde derecho** del track (clip) y **vuelve a entrar por la izquierda** en loop infinito. **No** va y vuelve dentro del track.

- **Token (categoría Guía):** `motion-spring-md` — ritmo del ciclo; motor **`AnimationController.repeat()`** + muestreo lineal de `t` (sin springs encadenados con settle).
- **Duración de ciclo:** **2000ms** por vuelta completa (entrada izquierda → salida derecha → reentrada).
- **Ancho de shape en barrido:** constante `segWidth` (120px @ 360); no pulsa de lado a lado.
- **Gap 4px:** en el tramo **visible** dentro del track, `left` va de **`gap`** a **`W − gap − segWidth`**. Ahí se cumple:
  - `gapLeading = left − trackLeading` → **4px** al inicio del corredor (`left = gap`).
  - `gapTrailing = W − left − segWidth` → **4px** al final del corredor (`left = W − gap − segWidth`).
  - Entre ambos extremos, el track inactivo crece a la izquierda y se reduce a la derecha (como en la captura de referencia).

**Sin animación** en: montaje/desmontaje, cambio de `color`, skeleton (excluido).

Sin viewport enter/exit. Sin stagger. Sin haptics.

> **Nota Figma:** node `Loaders / Linear Loader` — fila flex con track + **gap 4** + shape + **gap 4** + track; la animación mueve la shape y redistribuye los tramos de track manteniendo el gap.

---

## Funciones del ciclo (referencia W = 360px)

Escalar si `trackWidth ≠ 360`:

- `gap = W × 4/360`
- `segWidth = W × 120/360`
- `t ∈ [0, 1)` — avance **lineal** del ciclo

```text
left(t)   = −segWidth + (W + segWidth) × t
width(t)  = segWidth   // constante durante el barrido
```

**Corredor visible** (shape dentro del track, recortada por overflow):

| Condición | left | gap leading | gap trailing |
|---|---|---|---|
| Entrada al corredor | `gap` | 4px | > 4px |
| Salida del corredor | `W − gap − segWidth` | > 4px | 4px |
| Fuera de pantalla | `< 0` o `left + segWidth > W` | — | — |

**Instantes de referencia** (W = 360, gap = 4, segWidth = 120):

| t (aprox.) | left | Comportamiento |
|---|---|---|
| 0.00 | −120 | reentrada (fuera a la izquierda) |
| 0.33 | 4 | entra al corredor; **gap leading = 4** |
| 0.67 | 236 | aún visible |
| 0.94 | 236 (= 360−4−120) | **gap trailing = 4** |
| 1.00 | 360 | sale por la derecha; t=0 reinicia |

---

## Timeline de interacción

| # | Tipo | Evento | Elemento | Propiedad | Valor inicial | Valor final | Token | Duración / notas | Inicio | Fin |
|---|---|---|---|---|---|---|---|---|---|---|
| 1 | Trigger | Montaje / visible | — | — | — | — | — | — | 0 | — |
| 2 | Response | Aparición | `LinearLoader` | — | — | — | **sin animación** | — | 0 | 0 |
| 3 | Response | Loop continuo (→ derecha) | fill activo | `left` | `−segWidth` → `W` | lineal en t | `motion-spring-md` | ciclo **2000ms**, `repeat()` | 0 | ∞ |
| 3b | Response | Ancho shape | fill activo | `width` | `segWidth` | constante | — | — | 0 | ∞ |
| 4 | Trigger | Cambio `color` | — | — | — | — | — | — | * | — |
| 5 | Response | Swap variante | `LinearLoader` | color | anterior | nuevo | **sin animación** | — | — | — |
| 6 | Trigger | Desmontaje | — | — | — | — | — | — | * | — |
| 7 | Response | Salida | `LinearLoader` | — | — | — | **sin animación** | — | — | — |
| 8 | Trigger | `MediaQuery.disableAnimations` | — | — | — | — | — | — | * | — |
| 9 | Response | Pausa loop | `AnimationController` | `stop()` | running | paused | — | frame t=0 | * | — |

> `*` = momento variable.

---

## Token Mapping

| Momento | Token semántico | Implementación Flutter | Valor |
|---|---|---|---|
| Loop continuo | `motion-spring-md` | `AnimationController(duration: 2000ms)..repeat()` + `left(t)` / `width(t)` | mass 1 · stiffness 300 · damping 28 (categoría); ciclo **2000ms** |
| Montaje / desmontaje | — | — | instantáneo |
| Cambio `color` | — | — | instantáneo |
| `disableAnimations` | — | `controller.stop()`; segmento en t=0 | pausa |

---

## Implementación Dart (Flutter)

```dart
/// Categoría Guía — motion-spring-md (no usar SpringSimulation encadenada).
const linearLoaderCycle = Duration(milliseconds: 2000);

class LinearLoaderSegment {
  const LinearLoaderSegment({required this.left, required this.width});
  final double left;
  final double width;
}

LinearLoaderSegment linearLoaderSegmentAt(double t, {required double trackWidth}) {
  assert(t >= 0 && t < 1);
  const figmaRef = 360.0;
  const gapPx = 4.0;
  const segPx = 120.0;
  final gap = trackWidth * gapPx / figmaRef;
  final segWidth = trackWidth * segPx / figmaRef;
  final left = -segWidth + (trackWidth + segWidth) * t;
  return LinearLoaderSegment(left: left, width: segWidth);
}

/// Track inactivo a la izquierda (visual Figma), cuando la shape ya entró al corredor.
double linearLoaderLeadingTrackWidth(double left, {required double trackWidth, required double gap}) {
  return max(0, left - gap);
}

/// Track inactivo a la derecha, mientras la shape no salió del corredor.
double linearLoaderTrailingTrackWidth(
  double left, {
  required double trackWidth,
  required double gap,
  required double segWidth,
}) {
  return max(0, trackWidth - left - segWidth - gap);
}

class GgdsLinearLoader extends StatefulWidget {
  const GgdsLinearLoader({super.key});

  @override
  State<GgdsLinearLoader> createState() => _GgdsLinearLoaderState();
}

class _GgdsLinearLoaderState extends State<GgdsLinearLoader>
    with SingleTickerProviderStateMixin {
  late final AnimationController _controller;

  @override
  void initState() {
    super.initState();
    _controller = AnimationController(vsync: this, duration: linearLoaderCycle);
    _syncMotion();
  }

  void _syncMotion() {
    if (MediaQuery.disableAnimationsOf(context)) {
      _controller.stop();
      _controller.value = 0;
      return;
    }
    if (!_controller.isAnimating) _controller.repeat();
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    _syncMotion();
  }

  @override
  void dispose() {
    _controller.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return AnimatedBuilder(
      animation: _controller,
      builder: (context, child) {
        final trackWidth = 360.0; // sustituir por layout real
        final seg = linearLoaderSegmentAt(_controller.value, trackWidth: trackWidth);
        return /* track + Positioned fill: left seg.left, width seg.width */;
      },
    );
  }
}
```

---

## Recomendaciones

| Tema | Criterio |
|---|---|
| Continuidad | Un solo driver `t` lineal; sin ida y vuelta dentro del track |
| Gap 4px | Respetar en extremos del corredor; no pegar shape al borde del track |
| Progress Indicator | Anima **% determinado**; no aplicar ese patrón acá |
| Spinner | Rotación `repeat()`; Linear Loader = traslación unidireccional + wrap |
| Cambio de variante | `color`: rebuild instantáneo |
| Skeleton | Excluido |
| Accesibilidad | `disableAnimations`: pausar en t=0; `Semantics(label: 'Loading')` |
| Haptics | Ninguno |

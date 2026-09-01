# App Handoff — Motion GGDS (Flutter)

Guía para documentar handoffs de plataforma **App** con `flutter_animate` y springs.

## Bloques adicionales obligatorios en MD

Insertar **después del encabezado**, antes de Motion Specification:

### 1. Referencia visual

| Preview | Uso |
|---------|-----|
| [Preview App (HTML)](./[componente]_motion-handoff_app.html) | Spring simulado en JS con los mismos parámetros del Token Mapping. No sustituye QA en dispositivo. |
| [Preview Web companion](../Web/[componente]_motion-handoff_web.html) | Misma intención de motion. En App implementar con tokens spring, no curvas CSS. |

> El preview Web muestra cómo debe sentirse el componente.  
> El preview App y el Token Mapping muestran cómo implementarlo en Flutter.

Si no existe handoff Web del mismo componente, omitir la fila companion y el link.

### 2. Paridad Web ↔ App

| Aspecto | Web | App |
|---------|-----|-----|
| Token semántico | `motion-curve-*` | `motion-spring-*` |
| Curva / física | easing + duración ms | mass · stiffness · damping |
| Estados internos | hover, pressed, focus, loading | highlight, pressed, focus, loading |
| Disabled | sin transición | instantáneo |

Regla: **no traducir ms de Web a duración fija en Flutter**.

### 3. Nota para desarrollo

- **Fuente de verdad:** Token Mapping + snippet Dart de este documento.
- **Preview HTML App:** representación visual con spring en JS.
- **Preview Web companion:** solo referencia de intención; no copiar CSS.
- **QA final:** validar en dispositivo con `flutter_animate` / `SpringDescription`.

## Reglas de tokens App

- Entrada y salida usan **siempre el mismo token spring**
- Nunca mezclar con curvas bezier en salida
- Leer parámetros exactos en `references/tokens-motion.md`

## HTML App — elementos obligatorios

1. Callout de representación (clase `callout`, borde violeta) bajo controles del preview
2. Spring simulado en JS con parámetros del Token Mapping (`springTo()`)
3. Readout: token fijo + estado/acción dinámico
4. Link al **preview Web companion** si existe en `../Web/`

## Estados interno App — reglas específicas

- **No usar estado `hover`** en preview ni en documentación App
- En **checkbox/radio**, la selección cambia por interacción directa (click/tap o teclado)
- Mantener solo controles auxiliares necesarios (`Disabled`, `Loading`, `Skeleton`, `disableAnimations`)

## Implementación Dart — solo motion

**Permitido:**
- `SpringDescription`, `SpringSimulation`, `flutter_animate`
- Tokens de motion (`motion-spring-*`)
- Gate de accesibilidad (`MediaQuery.of(context).disableAnimations`)

**Prohibido:**
- Tokens visuales no-motion (`--color-*`, spacing, radius, typography)
- Reglas funcionales del componente salvo mención mínima de "disabled sin transición"

## Haptics App

Leer `references/tokens-haptics.md`. Ver matriz componente → token.

Orden en HTML App con haptics:
1. Preview de motion
2. Implementación Dart (Flutter)
3. **Haptics** (divider + `h2`)
4. Footer

## Referencias canónicas

| Patrón | Referencia |
|---|---|
| Cambio de estado | `references/history/button/App/` |
| Entrada/salida | `references/history/sidesheet/Web/` (Web; App similar con spring) |
| Selección | `references/history/box-selector/App/` |

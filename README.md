# Motion Handoff GGDS

Guia operativa del proyecto de handoffs de motion para componentes GGDS en Web y App.

## Objetivo

- Estandarizar handoffs de motion entre Diseno Ops y Desarrollo.
- Documentar comportamiento real de componentes con tokens semanticos.
- Entregar previews y especificaciones tecnicas consistentes por plataforma.

## Que cubre este proyecto

- Estados internos de componentes (ej. button, checkbox, icon button).
- Componentes de entrada/salida de viewport (ej. sidesheet, modal, toast).
- Paridad Web/App cuando aplique.
- Haptics en App (cuando el componente lo requiere).
- Accesibilidad (`prefers-reduced-motion` y `disableAnimations`).

## Estructura de carpetas

```text
motion/
├── [componente-kebab-case]/
│   ├── Web/
│   │   ├── [componente]_motion-handoff_web.html
│   │   ├── [componente]_motion-handoff_web.md
│   │   └── [componente]_motion-spec_web.md
│   └── App/
│       ├── [componente]_motion-handoff_app.html
│       ├── [componente]_motion-handoff_app.md
│       └── [componente]_motion-spec_app.md
├── README.md
├── motion-handoff.skill
├── tokens-motion.md
├── tokens-haptics.md
└── guia-ops-motion-handoff.md
```

## Entregables por componente

- `*_motion-handoff_[web|app].html`: preview interactivo de motion.
- `*_motion-handoff_[web|app].md`: documento de handoff para ticket/repo.
- `*_motion-spec_[web|app].md`: spec estructurada para trazabilidad/pipeline.

## Convenciones clave

- Consumir siempre tokens semanticos (no valores crudos en decisiones de handoff).
- Incluir `Token Mapping` completo: semantico -> primitivo -> valor final.
- Mantener naming consistente por slug de componente.
- Reflejar solo estados reales de Figma y del componente.
- No usar este README para listar componentes uno por uno.

## Historial y trazabilidad

- La fuente de verdad de entregables es `[componente]/` en la **raíz del workspace** (repo `GGDS-Motion-Handoff`).
- La skill de equipo vive en `.cursor/skills/motion-handoff/SKILL.md` (misma definición que `motion-handoff.skill`).
- El historial curado del skill vive en `.cursor/skills/motion-handoff/references/history/` (o `~/.cursor/skills/motion-handoff/references/history/` si usás skill personal).
- En ese historial se mantienen solo referencias canonicas de patrones (no todo componente nuevo).
- Si hay cambios de criterio, documentarlos en el handoff/spec del componente afectado.

## Uso en equipo

1. Clonar el repo y abrirlo como workspace en Cursor.
2. Usar la skill `motion-handoff` (detecta el repo por `motion-handoff.skill` en la raíz).
3. Los entregables se generan en `[componente]/Web/` y/o `[componente]/App/` — rutas relativas, sin paths de usuario.
4. `git add`, commit y push del componente documentado.

## Mantenimiento de este README

Actualizar este archivo solo cuando cambie:

- la estructura de carpetas,
- el flujo de trabajo,
- las convenciones del sistema,
- o la politica de trazabilidad.

No actualizarlo por cada componente nuevo.


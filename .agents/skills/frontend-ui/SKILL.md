---
name: frontend-ui
description: Resolver trabajo de frontend Angular en NeoGaming cuando la tarea afecte pantallas, componentes, servicios HTTP, guards, interceptors, formularios, estados de carga/error, rutas protegidas o alineacion visual con contratos backend. Activar ante pedidos como "corrige esta pantalla", "alinea este servicio con el backend", "revisa loading/error", "haz responsive esta vista", "arregla el guard" o "audita este flujo UI". No usar para backend puro, SQL o tareas sin impacto en la interfaz.
---

# Frontend / UI Skill

## En NeoGaming
- Revisar juntos template, componente, tipos y servicio HTTP.
- Confirmar URL, metodo y payload exactos contra el backend antes de asumir shapes.
- Evitar `as any`, `Observable<any>` y `Observable<unknown>` si esconden contratos rotos.
- Verificar estados `loading`, `error`, vacio y permisos/rutas.
- No mover el trabajo a React o Tailwind si el repo usa Angular.

## Philosophy
- Prioritize **visual quality and user experience** above all. Code should look great out of the box.
- Use **semantic HTML** and **accessible** markup (ARIA roles, keyboard nav, color contrast).
- Default stack: **React + Tailwind CSS**. Use shadcn/ui components when available.
- Mobile-first responsive design. Every layout must work on 320px–1440px.

## Component Structure
1. One component per file, named with PascalCase.
2. Co-locate styles with the component (Tailwind classes inline, no separate CSS files unless necessary).
3. Extract reusable primitives (Button, Card, Badge) before building page-level components.
4. Use TypeScript interfaces for all props.

## Design Defaults
- Spacing scale: 4px base unit (Tailwind: p-1=4px, p-2=8px, p-4=16px).
- Typography: `font-sans` base, `text-sm` for UI labels, `text-base` for body, `text-2xl+` for headings.
- Colors: use CSS variables or Tailwind's design tokens — never hardcode hex unless matching a brand.
- Shadows: prefer `shadow-sm` or `shadow-md`. Avoid heavy shadows.
- Border radius: `rounded-lg` for cards/modals, `rounded-full` for avatars/badges.

## Workflow
1. **Understand the layout** — identify regions: header, sidebar, main, footer.
2. **Scaffold the structure** in HTML/JSX before adding styles.
3. **Apply Tailwind** classes from outside-in (container → section → element).
4. **Add interactivity** (hover states, focus rings, transitions) last.
5. **Check responsiveness** at sm (640px), md (768px), lg (1024px) breakpoints.
6. **Verify accessibility**: alt text on images, labels on inputs, logical tab order.

## Animation Guidelines
- Use `transition-all duration-200` for hover/focus state changes.
- Use `animate-fade-in` or framer-motion for page-level transitions.
- Never animate layout properties (width/height) — prefer opacity and transform.

## Common Patterns
- **Loading states**: skeleton screens over spinners for content areas.
- **Empty states**: always provide an illustration or helpful message.
- **Error states**: show the error inline, never just console.log it.
- **Forms**: use controlled components, validate on blur, show errors below each field.

## Do NOT
- Use inline `style={{}}` unless Tailwind cannot handle it.
- Mix CSS-in-JS and Tailwind in the same project.
- Ship components without at least one responsive breakpoint check.
- Hardcode pixel values when a Tailwind token exists.

---
name: trabajo-equipo
description: Resolver tareas de colaboracion y entrega en NeoGaming: mensajes de commit, textos de PR, code review, respuesta a comentarios, documentacion operativa, cambios de README, higiene Git y coordinacion entre frontend y backend. Activar ante pedidos como "escribeme el commit", "armame el PR", "revisa estos cambios", "contesta review comments", "ajusta el README" o "que va en backend y que va en frontend". No usar para implementar logica de producto cuando otra skill del dominio sea mas precisa.
---

# Trabajo en Equipo / Código Limpio Skill

## En NeoGaming
- Confirmar primero en que subrepo viven los cambios: `backend` o `frontend`.
- Leer `git status`, diff y archivos relevantes antes de redactar commits o PRs.
- Separar backend y frontend cuando el cambio real tambien lo este.
- Si un subrepo esta limpio, decirlo; no inventar commits ni PRs.
- Ajustar README y comandos al entorno real de Windows y PowerShell.

## Pull Request Workflow
1. **Before opening a PR**:
   - Run linter and tests locally — zero failures.
   - Self-review the diff: remove debug logs, dead code, and unrelated changes.
   - Squash or organize commits into a logical sequence.
2. **PR Title**: `type(scope): short imperative description` (e.g., `feat(auth): add OAuth2 login`).
3. **PR Description template**:
   ```
   ## What
   One paragraph describing what changed and why.

   ## How
   Key implementation decisions and trade-offs.

   ## Testing
   How to test this change manually or which tests cover it.

   ## Screenshots (if UI)
   Before / After screenshots or video.
   ```

## Code Review — Giving Feedback
- Be specific: reference the exact line and explain the concern.
- Distinguish blocking issues from suggestions: prefix with `[blocking]` or `[nit]`.
- Suggest, don't dictate: "Consider X because Y" not "Do X".
- Approve only when you'd be comfortable maintaining this code.

## Code Review — Receiving Feedback
- Respond to every comment, even if just "Done" or "Disagree — here's why".
- Do not silently close review comments.
- If disagreeing, explain the trade-off clearly and ask for a second opinion if unresolved.

## Coding Standards
- **Naming**: variables and functions in camelCase, classes in PascalCase, constants in UPPER_SNAKE_CASE.
- **Functions**: max 30 lines. If longer, extract helpers.
- **Files**: max 300 lines. If longer, split by responsibility.
- **Imports**: sorted — stdlib → third-party → local. No unused imports.
- **Comments**: explain *why*, not *what*. Code explains what; comments explain intent.

## Documentation
- Every public function/class needs a JSDoc/docstring with: description, params, return value, example.
- Every new feature needs a README section or updated docs.
- Changelogs follow [Keep a Changelog](https://keepachangelog.com): `Added`, `Changed`, `Fixed`, `Removed`.

## Merge Conflict Resolution
1. Understand **both** sides of the conflict before choosing.
2. Never blindly accept "ours" or "theirs" — combine intentionally.
3. After resolving, run tests to confirm the merged result is correct.
4. Leave a comment in the PR describing how you resolved complex conflicts.

## Git Best Practices
- Branch naming: `type/short-description` (e.g., `feat/user-auth`, `fix/login-crash`).
- Commit early and often on feature branches; clean up before merging.
- Never force-push to `main` or `develop`.
- Tag releases with semantic versioning: `v1.2.3`.

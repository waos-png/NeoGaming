---
name: desarrollo-general
description: Coordinar trabajo tecnico general en NeoGaming cuando la tarea requiera entender primero y ejecutar despues. Usar para debugging sistematico, planes cortos de implementacion, escritura o ajuste de pruebas, refactors pequenos, analisis de impacto, auditorias tecnicas y tareas ambiguas que crucen backend y frontend. Activar ante pedidos como "esto esta roto", "como implemento X", "haz una auditoria", "revisa riesgos", "agrega pruebas" o "explicame por donde atacar". No usar para tareas puramente visuales ni para SQL/backend aislado cuando otra skill sea mas precisa.
---

# Desarrollo General Skill

## En NeoGaming
- Leer archivos y ejecutar comandos concretos antes de concluir.
- Para auditorias, priorizar hallazgos por severidad con evidencia de archivo o comando.
- Para debugging, formular hipotesis concretas y validarlas antes de reescribir codigo.
- Aplicar cambios pequenos y verificables; no mezclar una correccion puntual con un refactor grande sin permiso.
- No inventar estado Git, endpoints ni artefactos que no existan.

## Golden Rule
**Understand before you write.** Read the existing code, understand the contract, then implement.

## Planning a Feature
1. Restate the requirement in one sentence to confirm understanding.
2. Identify affected files and modules.
3. List edge cases and failure modes before writing any code.
4. Write a short implementation plan (bullet list) and confirm with the user if ambiguous.
5. Implement in small, reviewable chunks.

## Debugging Workflow (Systematic)
1. **Reproduce** the bug with the minimal case.
2. **Hypothesize** — list 2–3 likely causes ranked by probability.
3. **Verify** each hypothesis with a targeted test or log, not by rewriting code.
4. **Fix** only the confirmed root cause. Do not refactor while fixing a bug.
5. **Confirm** the fix with a test that would have caught the bug.

## Writing Tests
- Unit tests: test one function/module in isolation. Mock all external dependencies.
- Integration tests: test the interaction between 2+ real modules.
- Name tests: `it("should [behavior] when [condition]")`.
- Arrange → Act → Assert structure always.
- Cover: happy path, edge cases, and at least one error/failure path.

## Refactoring Rules
- **Never refactor and fix a bug in the same commit.**
- Make one change at a time; run tests after each change.
- Rename for clarity, extract for reuse, simplify for readability.
- Leave the code better than you found it, but don't over-engineer.

## Code Quality Checklist
- [ ] Function does one thing (Single Responsibility).
- [ ] No magic numbers — use named constants.
- [ ] Error handling at every I/O boundary (API calls, file reads, DB queries).
- [ ] No commented-out code left behind.
- [ ] All new code has at least one test.

## Commit Message Format
```
type(scope): short description

- bullet of what changed
- bullet of why
```
Types: `feat`, `fix`, `refactor`, `test`, `chore`, `docs`.

## When Stuck
1. Re-read the error message word by word.
2. Check the docs for the library version in use (not latest if pinned).
3. Isolate the problem to the smallest possible reproduction.
4. Ask: "What assumption am I making that could be wrong?"

# Implementation Plan: [FEATURE]

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]

**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: Preencha a seção Technical Context e Project Structure com os defaults
do **seu** projeto (abaixo já há placeholders tipados). Remova opções não usadas.

## Summary

[Extract from feature spec: primary requirement + technical approach from research]

## Technical Context

<!-- DEFAULTS DO PROJETO — edite uma vez no template do repo -->

**Language/Version**: [ex.: TypeScript 5.x / Node 20]

**Primary Dependencies**: [ex.: Express, React, Prisma]

**Storage**: [ex.: PostgreSQL]

**Testing**: [comando exato, ex.: `npm test` ou `docker compose exec -T app php artisan test`]

**Target Platform**: [ex.: Linux server / Web]

**Project Type**: [ex.: web-service]

**Performance Goals**: [ex.: p95 &lt; 500ms nas APIs da feature]

**Constraints**: [ex.: sem novos serviços externos no P1]

**Scale/Scope**: [ex.: feature isolada em um módulo]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

[Gates derived from constitution]

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
└── tasks.md
```

### Source Code (repository)

<!-- Substitua pela árvore REAL. Não deixe Option 1/2/3 no plan entregue. -->

```text
src/
├── domain/
├── application/
├── infrastructure/
└── interfaces/
tests/
```

**Structure Decision**: [Uma frase: onde esta feature encaixa na árvore acima]

## Complexity Tracking

> Preencha SÓ se o Constitution Check tiver violações justificadas

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| | | |

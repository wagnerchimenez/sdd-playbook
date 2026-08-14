# Constitution — App Exemplo

**Version**: 1.0.0 | **Ratified**: 2026-08-14 | **Last Amended**: 2026-08-14

## Core Principles

### I. Idioma

Texto de produto e documentação em português brasileiro. Código de domínio em
inglês neste exemplo (`User`, `ConfirmationToken`) para parecer um app Node típico.

### II. Arquitetura

API Express fina → `src/application` (casos de uso) → `src/domain` →
`src/infrastructure` (e-mail, banco). UI não acessa repositórios.

### III. Qualidade e escopo

Testes em `tests/`. YAGNI. Não expandir além da spec ativa.

### IV. Git e commits

PR obrigatório. Conventional Commits. Sem atribuição de IA. Sem segredos.

## Fluxo Spec Kit

1. specify → 2. plan → 3. tasks → 4. implement → 5. converge (opcional)

Um chat por feature; revisão humana entre fases.

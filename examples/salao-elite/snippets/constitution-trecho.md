# Constitution (trecho) — Salão Elite

## Core Principles (resumo)

### I. Idioma e nomenclatura de domínio

Texto de produto em português brasileiro. Domínio em português (`Salao`,
`Agendamento`, `buscarPorSlug`). Inglês só no técnico Laravel (`Controller`,
`FormRequest`, `users`, `timestamps`).

### II. Arquitetura em camadas

App em `site/` (Laravel + Docker Compose). Controllers HTTP finos. Negócio em
`site/src/{Contexto}/` (`Aplicacao`, `Dominio`, `Infraestrutura`). Handler por
ação. UI (React/Inertia) não acessa repositórios.

### III. Qualidade e escopo

Testes via Docker. YAGNI. Em produção nunca `migrate:fresh` / `db:wipe`.

### IV. Git Flow e commits

PR para `main`. Branches `feature|bugfix|hotfix/<slug>`. Conventional Commits
em português. Sem atribuição de IA. Sem segredos.

## Fluxo Spec Kit

1. `/speckit-specify` — especificação
2. `/speckit-clarify` — só se necessário
3. `/speckit-plan` — plano técnico
4. `/speckit-tasks` — tarefas
5. `/speckit-implement` — implementação
6. `/speckit-converge` — gaps

Skills opcionais: `analyze`, `checklist`. Preferir um chat por feature e revisão
humana entre fases (editar markdown em vez de rerodar comando).

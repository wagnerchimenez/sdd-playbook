# Playbook SDD — consumo mínimo de requests

Guia passo a passo de **Spec-Driven Development** (Spec Kit) para Cursor e GitHub Copilot. Foco: **menos requests**, mais aproveitamento do contexto, artefatos que o agente realmente segue.

Leia na ordem. Cada documento cabe em uma sessão.

| # | Documento | Para que serve |
|---|-----------|----------------|
| 1 | [Guia completo](docs/01-guia-completo.md) | Por que SDD economiza requests; fluxo de 4 prompts |
| 2 | [Cursor vs Copilot](docs/02-cursor-vs-copilot.md) | Skills, rules, instruções em cada ferramenta |
| 3 | [Rules e skills](docs/03-rules-e-skills.md) | O que criar, o que não criar, templates |
| 4 | [Setup de projeto do zero](docs/04-setup-projeto-zero.md) | Instalar Spec Kit e configurar o repo |
| 5 | [Prompts prontos](docs/05-prompts-prontos.md) | Copy-paste na ordem, com exemplo preenchido |
| 6 | [Quando não usar SDD](docs/06-quando-nao-usar-sdd.md) | Bugfix, copy, refactor, spike |

## Quick start (5 minutos)

1. Instale o Spec Kit e rode `specify init` no projeto ([setup](docs/04-setup-projeto-zero.md)).
2. Copie as rules de [`templates/`](templates/) para `.cursor/rules/` (Cursor) ou adapte para `.github/copilot-instructions.md` (Copilot).
3. Preencha a constitution e o `plan-template` com **a stack real** do projeto.
4. Em **um único chat**, rode: specify → (revisar) → plan → (revisar) → tasks → (revisar) → implement.
5. Edite `spec.md` / `plan.md` / `tasks.md` no editor. Não rerode o comando para um ajuste pequeno.

## Exemplos e templates

- [`examples/minimal/`](examples/minimal/) — feature fictícia completa (`spec`, `plan`, `tasks`) + rules.
- [`examples/salao-elite/`](examples/salao-elite/) — trechos de um projeto Laravel real (constitution, arquitetura, prompt).
- [`templates/`](templates/) — arquivos para copiar no `specify init`.

## Ideia central

Cada prompt no Cursor/Copilot **reinicia ou infla contexto**. SDD só vale a pena se os artefatos (`spec.md`, `plan.md`, `tasks.md`) carregarem o contexto e o agente **não precisar explorar o repositório** a cada fase.

**Regra de ouro:** investir no primeiro prompt e nos arquivos; economizar repetindo fases e abrindo chats novos.

## Origem

Este playbook é genérico. O exemplo Laravel veio do projeto Salão Elite e foi adaptado para reuso — sem segredos, credenciais ou código de produto.

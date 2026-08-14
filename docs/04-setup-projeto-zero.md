# 04 — Setup de projeto do zero

Tempo alvo: **≤ 30 minutos** até a primeira feature piloto.

## Pré-requisitos

- Git
- Python 3.11+
- [uv](https://docs.astral.sh/uv/) (recomendado) ou pipx
- Cursor **ou** GitHub Copilot
- Conta no GitHub (opcional, para o remoto)

## Passo 1 — Criar o repositório do produto

```bash
mkdir meu-projeto && cd meu-projeto
git init
```

Ou clone o repo já existente e `cd` nele.

## Passo 2 — Instalar o Spec Kit CLI

Opção A — PyPI (simples):

```bash
uv tool install specify-cli
specify version
```

Opção B — pin em release do GitHub (reproduzível):

```bash
# Troque vX.Y.Z pela tag em https://github.com/github/spec-kit/releases
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
specify version
```

Upgrade depois: `specify self check` / `specify self upgrade`.

## Passo 3 — Inicializar no projeto

**Cursor:**

```bash
specify init . --ai cursor-agent
# Em versões recentes também: --integration cursor-agent
```

**Copilot:**

```bash
specify init . --integration copilot
```

Confirme que existem:

- `.specify/`
- Skills ou comandos Spec Kit no agente
- `.specify/init-options.json` (versão, integração)

Liste integrações: `specify integration list`.

## Passo 4 — Constitution

No agente:

```text
/speckit-constitution Princípios: idioma PT-BR; testes obrigatórios em mudanças de domínio;
Git Flow feature/bugfix/hotfix; YAGNI; commits Conventional em português sem atribuição de IA.
Incluir seção Fluxo Spec Kit com specify → plan → tasks → implement → converge.
```

Ou copie e edite [`templates/constitution-template.md`](../templates/constitution-template.md) para `.specify/memory/constitution.md`.

## Passo 5 — Copiar rules / instructions

**Cursor:**

```bash
cp /caminho/para/sdd-playbook/templates/rule-sdd-playbook.mdc .cursor/rules/sdd-playbook.mdc
cp /caminho/para/sdd-playbook/templates/rule-feature-ativa.mdc .cursor/rules/feature-ativa.mdc
cp /caminho/para/sdd-playbook/templates/rule-padroes-projeto.mdc .cursor/rules/padroes-projeto.mdc
# Ajuste globs e conteúdo:
cp /caminho/para/sdd-playbook/templates/rule-arquitetura.mdc .cursor/rules/arquitetura.mdc
cp /caminho/para/sdd-playbook/templates/rule-testes.mdc .cursor/rules/testes.mdc
```

**Copilot:** crie `.github/copilot-instructions.md` com o conteúdo resumido de playbook + feature-ativa + padrões (ver [02](02-cursor-vs-copilot.md)).

## Passo 6 — Customizar o plan template

Edite `.specify/templates/plan-template.md` (ou use override em `.specify/templates/overrides/` se a versão do Spec Kit preferir).

Substitua placeholders por:

| Campo | Exemplo |
|-------|---------|
| Language/Version | PHP 8.3 / Laravel 11 |
| Primary Dependencies | Inertia, React, Docker Compose |
| Storage | MySQL + Eloquent |
| Testing | `docker compose exec -T app php artisan test` |
| Project Structure | Árvore real do repo |

Use [`templates/plan-template-generic.md`](../templates/plan-template-generic.md) como base e delete as “Option 1/2/3”.

## Passo 7 — Pasta specs

```bash
mkdir -p specs
touch specs/.gitkeep
git add specs/.gitkeep
```

O próximo `/speckit-specify` cria `specs/NNN-slug/` e atualiza `.specify/feature.json`.

## Passo 8 — Feature piloto

Escolha algo **pequeno** (1 user story P1). Use os prompts de [05](05-prompts-prontos.md).

Checklist piloto:

- [ ] Um único chat
- [ ] 4 prompts (+ revisões manuais)
- [ ] `tasks.md` com paths corretos
- [ ] Implement marca `[X]`
- [ ] Testes passam

Se o piloto falhar por exploração demais: volte ao passo 6 (plan template) e à rule de arquitetura.

## `init-options.json` (referência)

Exemplo típico após init no Cursor:

```json
{
  "ai": "cursor-agent",
  "ai_skills": true,
  "feature_numbering": "sequential",
  "here": true,
  "integration": "cursor-agent",
  "script": "sh",
  "speckit_version": "0.16.0"
}
```

`feature_numbering: sequential` → pastas `001-slug`, `002-slug`, …

## Branch Git Flow (recomendado)

Antes ou depois do specify:

```bash
git checkout main
git pull
git checkout -b feature/notificacao-email
```

O diretório `specs/001-notificacao-email` e a branch `feature/notificacao-email` podem (e devem) usar o **mesmo slug**, mas são independentes no Spec Kit.

## Troubleshooting rápido

| Sintoma | Ação |
|---------|------|
| Comando Spec Kit não aparece | Reabrir o projeto; conferir skills em `.cursor/skills/` |
| `feature.json` aponta pasta inexistente | Rodar novo specify ou corrigir o path |
| Plan inventa pastas | Corrigir plan-template e re-gerar plan **uma vez** |
| Implement explora demais | Rule arquitetura + tasks com paths absolutos relativos ao repo |

## Próximo

- [05 — Prompts prontos](05-prompts-prontos.md)
- [06 — Quando não usar SDD](06-quando-nao-usar-sdd.md)

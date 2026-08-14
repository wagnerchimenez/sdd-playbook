# 02 — Cursor vs Copilot

O Spec Kit é o mesmo. O que muda é **onde** ficam skills/comandos e **como** o contexto persistente (rules/instructions) entra no agente.

## Visão rápida

| Aspecto | Cursor | GitHub Copilot |
|---------|--------|----------------|
| Init Spec Kit | `specify init . --ai cursor-agent` (ou integração `cursor-agent`) | `specify init . --integration copilot` |
| Comandos | Skills em `.cursor/skills/speckit-*` → invocar `/speckit-specify` (ou skill equivalente) | Slash `/speckit.specify` / agentes Spec Kit |
| Contexto persistente | `.cursor/rules/*.mdc` (`alwaysApply`, `globs`) | `.github/copilot-instructions.md` e/ou custom instructions |
| Feature ativa | `.specify/feature.json` + rule `feature-ativa.mdc` | Mesmo `feature.json`; instruction apontando para ele |
| Economia | 1 thread por feature; @ nos artefatos | Idem; evitar chats paralelos na mesma feature |

**Igual nos dois:** constitution (`.specify/memory/constitution.md`), templates (`.specify/templates/`), pasta `specs/`, ordem specify → plan → tasks → implement.

## Cursor

### O que o `specify init` cria

- `.specify/` — memory, templates, scripts, `feature.json`, integrações
- `.cursor/skills/speckit-*` — skills que o agente segue ao você pedir o comando

### Rules que você adiciona (manual)

Copie de [`templates/`](../templates/):

- `rule-sdd-playbook.mdc` → `.cursor/rules/sdd-playbook.mdc`
- `rule-feature-ativa.mdc` → `.cursor/rules/feature-ativa.mdc`
- Plus: `padroes-projeto`, `arquitetura`, `testes` (ver [03](03-rules-e-skills.md))

Rules com `alwaysApply: true` entram em **quase todo** prompt — mantenha-as **curtas**. Detalhe de arquitetura usa `globs` (só carrega quando o agente toca nesses arquivos).

### Como invocar

No Agent chat:

```text
/speckit-specify Usuário compartilha lista de tarefas com um colega...
```

Se a UI listar skills, use a skill `speckit-specify` com o mesmo texto.

### Dicas de consumo

- Não abra Agent + Chat paralelo na mesma feature.
- Se o contexto esfriar: `@specs/001-notificacao-email/tasks.md` + “continue a partir de T010”.
- Prefira Agent mode para implement; Ask/Plan só quando for revisão de artefato (e aí edite o arquivo você).

## GitHub Copilot

### O que o `specify init` cria

- Mesma árvore `.specify/`
- Integração Copilot (comandos/agents conforme a versão do Spec Kit)

### Instruções persistentes

Crie ou edite `.github/copilot-instructions.md` com o equivalente curto das rules:

```markdown
# Projeto

- Idioma: português brasileiro.
- Feature ativa: ler `.specify/feature.json` → pasta em `specs/`.
- Fluxo: specify → plan → tasks → implement (um chat por feature).
- Entre fases: humano edita markdown; não rerodar comando por nitpick.
- Implementação: seguir `tasks.md`; marcar [X]; não expandir escopo.
- Stack e pastas: ver constitution e plan-template.
```

Para regras que só importam em certas pastas, use custom instructions do Copilot com path filters, se disponíveis na sua versão, ou deixe no `plan.md` (o implement lê o plan).

### Como invocar

Use os slash commands Spec Kit documentados na sua versão, por exemplo:

```text
/speckit.specify ...
/speckit.plan
/speckit.tasks
/speckit.implement
```

(Em algumas UIs o ponto vira hífen; o nome da skill/comando é o que importa.)

## Tabela de comandos (núcleo)

| Fase | Cursor (skill) | Copilot (slash típico) |
|------|----------------|------------------------|
| Constituição | `speckit-constitution` | `/speckit.constitution` |
| Spec | `speckit-specify` | `/speckit.specify` |
| Plano | `speckit-plan` | `/speckit.plan` |
| Tarefas | `speckit-tasks` | `/speckit.tasks` |
| Implementar | `speckit-implement` | `/speckit.implement` |
| Convergir | `speckit-converge` | `/speckit.converge` |

Opcionais: `clarify`, `analyze`, `checklist` — ver [01](01-guia-completo.md).

## Erros comuns que gastam requests

1. Rodar `specify` de novo porque faltou um cenário → **edite** `spec.md`.
2. Chat novo por user story → **mesmo chat**, Phase por Phase.
3. Plan genérico (“Option 1 Single project”) → customizar `plan-template` **antes** da primeira feature.
4. Duplicar a constitution inteira em toda rule → constitution no Spec Kit; rules só apontam e reforçam o mínimo.

## Próximo

- [03 — Rules e skills](03-rules-e-skills.md)
- [04 — Setup do zero](04-setup-projeto-zero.md)

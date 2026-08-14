# 03 — Rules e skills

## Skills (Spec Kit) — não invente do zero

O `specify init` instala as skills/comandos. Você **usa**; não reescreve o fluxo core.

| Skill / comando | Obrigatório no fluxo mínimo? | Quando usar |
|-----------------|------------------------------|-------------|
| `speckit-constitution` | Uma vez (e ao mudar princípios) | Princípios do projeto |
| `speckit-specify` | Sim | Toda feature relevante |
| `speckit-plan` | Sim | Após spec aprovada |
| `speckit-tasks` | Sim | Após plan aprovado |
| `speckit-implement` | Sim | Execução |
| `speckit-converge` | Recomendado | Gaps pós-implement |
| `speckit-clarify` | Não | Spec ambígua |
| `speckit-analyze` | Não | Feature grande / multi-módulo |
| `speckit-checklist` | Não | Domínio sensível |
| `speckit-taskstoissues` | Não | Integração com Issues |

Atualizar Spec Kit: `specify self upgrade` (ou pin de versão — ver [docs oficiais](https://github.com/github/spec-kit)).

## Rules (você cria) — 3 a 5 no máximo

Rules demais = contexto sempre cheio = requests mais caros e respostas genéricas. Prefira **poucas e afiadas**.

| Arquivo | `alwaysApply` | Propósito |
|---------|---------------|-----------|
| `sdd-playbook.mdc` | true | Fluxo mínimo, o que pular, 1 chat/feature |
| `feature-ativa.mdc` | true | Aponta `.specify/feature.json` e artefatos |
| `padroes-projeto.mdc` | true | Idioma, Git Flow, commits |
| `arquitetura.mdc` | **false** + `globs` | Onde colocar código (reduz exploração) |
| `testes.mdc` | **false** + `globs` | Comando exato de teste |

Templates prontos:

- [`templates/rule-sdd-playbook.mdc`](../templates/rule-sdd-playbook.mdc)
- [`templates/rule-feature-ativa.mdc`](../templates/rule-feature-ativa.mdc)
- [`templates/rule-padroes-projeto.mdc`](../templates/rule-padroes-projeto.mdc)
- [`templates/rule-arquitetura.mdc`](../templates/rule-arquitetura.mdc)
- [`templates/rule-testes.mdc`](../templates/rule-testes.mdc)

### Como escrever uma boa rule

- **Curta** (ideal &lt; 80 linhas; alwaysApply &lt; 40).
- **Imperativa** (“MUST”, “não faça X”).
- **Aponta** para constitution/plan em vez de copiar.
- **Globs** para detalhe técnico; alwaysApply só para processo global.

### Exemplo mínimo `feature-ativa.mdc`

```markdown
---
description: Feature Spec Kit ativa
alwaysApply: true
---

# Feature ativa

1. Ler `.specify/feature.json` → `feature_directory`.
2. Artefatos: `spec.md`, `plan.md`, `tasks.md` nessa pasta.
3. Implementar só o que está em `tasks.md`; marcar `[X]` ao concluir.
4. Não expandir escopo além de `spec.md`.
```

## Constitution vs rules

| Artefato | Papel |
|----------|-------|
| `.specify/memory/constitution.md` | Princípios do produto/engenharia; Spec Kit consulta nos comandos |
| `.cursor/rules/*.mdc` | Instruções contínuas do **Cursor** |
| `.github/copilot-instructions.md` | Instruções contínuas do **Copilot** |

A constitution deve ter uma seção **Fluxo Spec Kit** listando a ordem mínima (ver [`templates/constitution-template.md`](../templates/constitution-template.md)).

## Plan template = a rule mais barata no implement

Customizar [`.specify/templates/plan-template.md`](../templates/plan-template-generic.md) com:

- Language, Storage, Testing **já preenchidos**
- Árvore de pastas **real** do repo
- Sem “Option 1 / Option 2 / Option 3”

Cada feature herda isso no `/speckit-plan`. Um template bom economiza exploração em **todas** as features.

## Checklist de setup de contexto

- [ ] Constitution preenchida (não placeholder)
- [ ] Plan template com stack e paths reais
- [ ] 2–3 alwaysApply rules (playbook + feature-ativa + padrões)
- [ ] 1–2 rules com glob (arquitetura + testes)
- [ ] Pasta `specs/` versionada (`.gitkeep`)
- [ ] `feature.json` aponta para a feature em curso (ou será reescrito no próximo specify)

## Próximo

- [04 — Setup do zero](04-setup-projeto-zero.md)
- Exemplo montado: [`examples/minimal/`](../examples/minimal/)

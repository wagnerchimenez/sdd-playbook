# Exemplo minimal — notificação por e-mail

Feature fictícia **001-notificacao-email** em app Node/Express genérico.

Use para **ler** o formato dos artefatos e o que revisar entre prompts — não é um app executável.

## Artefatos

| Arquivo | Papel |
|---------|--------|
| [`.specify/memory/constitution.md`](.specify/memory/constitution.md) | Princípios |
| [`.cursor/rules/`](.cursor/rules/) | Playbook + feature ativa |
| [`specs/001-notificacao-email/spec.md`](specs/001-notificacao-email/spec.md) | Produto |
| [`specs/001-notificacao-email/plan.md`](specs/001-notificacao-email/plan.md) | Técnico |
| [`specs/001-notificacao-email/tasks.md`](specs/001-notificacao-email/tasks.md) | Execução |

## O que revisar manualmente (entre prompts)

### Após specify → antes de plan

1. P1 entrega valor sozinho?
2. Fora de escopo está explícito?
3. Cenários Given/When/Then testáveis?

### Após plan → antes de tasks

1. Paths batem com a árvore do repo?
2. Comando de teste está escrito?
3. Modelo de dados mínimo (sem over-engineering)?

### Após tasks → antes de implement

1. Toda tarefa tem path?
2. Phase 3 = MVP?
3. Dá para mergear só até o fim da Phase 3?

## Prompt que gerou este exemplo (ilustrativo)

```text
/speckit-specify Após criar uma conta, o usuário recebe e-mail de confirmação.
P1: enviar e-mail com link de confirmação; ao clicar, conta fica confirmada.
P2: reenviar e-mail com rate limit.
Fora de escopo: SMS; provedor específico no P1 (abstrair porta); templates HTML ricos.
```

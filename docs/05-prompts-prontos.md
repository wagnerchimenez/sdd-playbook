# 05 — Prompts prontos

Use **na ordem**. Entre cada bloco: revise o arquivo no editor (sem IA).

Convenção Cursor abaixo. No Copilot, troque por `/speckit.specify` etc. (ver [02](02-cursor-vs-copilot.md)).

---

## Prompt 1 — Specify (descrição rica)

**Modelo:**

```text
/speckit-specify [ator] precisa [objetivo].

P1: [MVP que sozinho entrega valor].
P2: [nice-to-have; pode ficar de fora do primeiro merge].

Fora de escopo: [lista explícita].

Cenários:
- Given [estado], When [ação], Then [resultado]
- Given [estado], When [ação], Then [resultado]
- Given [erro/borda], When [ação], Then [comportamento]

Aceite: [2–4 critérios verificáveis sem stack].
```

**Exemplo preenchido (lista de tarefas com compartilhamento):**

```text
/speckit-specify Dono da lista precisa compartilhar uma lista de tarefas com um colega por e-mail.

P1: criar lista, adicionar itens, convidar um colaborador por e-mail; colaborador vê e marca item como feito.
P2: histórico de quem concluiu cada item.

Fora de escopo: app mobile nativo; SSO; compartilhar com mais de 5 pessoas; notificações push.

Cenários:
- Given usuário autenticado, When cria lista "Compras" e adiciona 2 itens, Then a lista aparece na home com 2 itens abertos.
- Given lista existente, When convida colega@empresa.com, Then o colega recebe e-mail e, ao aceitar, vê a lista em modo colaborador.
- Given colaborador com acesso, When marca item como feito, Then o item aparece concluído para o dono sem refresh manual longo (atualização em poucos segundos ou no próximo carregamento).
- Given e-mail inválido no convite, When envia convite, Then vê mensagem de erro clara e nenhum convite é criado.

Aceite: P1 utilizável sem P2; convite só por e-mail; dono pode revogar acesso.
```

**Depois (humano):** abrir `specs/NNN-*/spec.md` — ajustar prioridades, remover ambiguidade, garantir fora de escopo.

---

## Prompt 2 — Plan

```text
/speckit-plan
```

Só acrescente stack se o plan-template ainda estiver genérico:

```text
/speckit-plan Use a estrutura e stack já definidas no plan-template e na constitution. Não inventar pastas novas fora do layout do projeto.
```

**Depois (humano):** `plan.md`, `data-model.md`, `research.md` — paths reais, decisões ok.

---

## Prompt 3 — Tasks

```text
/speckit-tasks
```

**Depois (humano):** cada tarefa com path; Phase 3 = P1/MVP; cortar tarefas de P2 se quiser merge menor.

---

## Prompt 4 — Implement

```text
/speckit-implement
```

**Fatia (feature média, mesmo chat):**

```text
/speckit-implement — execute somente Phase 1, Phase 2 e Phase 3 (User Story 1 / P1). Marque [X] em tasks.md. Pare antes de P2.
```

**Continuar:**

```text
Continue — execute Phase 4 (User Story 2).
```

**Retomada (contexto frio):**

```text
Continue a feature em specs/001-lista-compartilhada — leia tasks.md e implemente a partir da tarefa T007.
```

---

## Opcional — Converge

Só se faltou requisito ou implement parou no meio:

```text
/speckit-converge
```

```text
/speckit-implement
```

No máximo **um** ciclo converge + implement antes de reabrir o escopo com novo specify.

---

## Opcionais (evitar no fluxo mínimo)

```text
/speckit-clarify
```

```text
/speckit-analyze
```

```text
/speckit-checklist foco em segurança do convite por e-mail
```

---

## Script mental de uma feature

1. Escrever o Prompt 1 no bloco de notas (2 minutos).
2. Colar no Agent (1 request).
3. Editar `spec.md` (0 request).
4. Prompt 2 → editar `plan.md`.
5. Prompt 3 → editar `tasks.md`.
6. Prompt 4 → testar localmente.
7. PR com “O que foi feito” / “Como testar”.

---

## Exemplo visual dos artefatos

Veja a feature completa em [`examples/minimal/specs/001-notificacao-email/`](../examples/minimal/specs/001-notificacao-email/) e o roteiro de revisão em [`examples/minimal/README.md`](../examples/minimal/README.md).

# Playbook SDD — economia máxima

Receita de bolo: Spec-Driven Development (Spec Kit + Cursor) do **projeto zero** até a **feature no código**, gastando o mínimo de IA.

Este repositório é um **kit de cópia**. Depois de copiar as rules para o produto, elas são a fonte da verdade — o playbook não precisa ficar aberto.

Rules prontas: pasta [`rules/`](rules/).

---

## Para que serve

Cada prompt no Cursor **infla ou reinicia contexto**. SDD vale a pena quando:

1. Você fecha o *o quê* num texto curto (`/speckit-specify`).
2. Os artefatos (`spec.md`, `plan.md`, `tasks.md`) carregam o contexto.
3. O implement **não** precisa reexplorar o repo a cada frase.

**Regra de ouro:** investir no prompt do specify e nos arquivos; economizar relendo fases e abrindo chats demais.

---

## Etapa 0 — Uma vez no projeto (do zero)

Faça **antes** da primeira feature. Não repita a cada feature.

### 0.1 Criar o repositório do produto

```bash
mkdir meu-projeto && cd meu-projeto
git init
# ou: clone o remoto e cd nele
```

### 0.2 Instalar o Spec Kit CLI

Pré-requisitos: Git, Python 3.11+, [uv](https://docs.astral.sh/uv/).

```bash
uv tool install specify-cli
specify version
```

Pin em release (opcional):

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
```

### 0.3 Inicializar Spec Kit no projeto (Cursor)

```bash
specify init . --ai cursor-agent
```

Isso cria, entre outros:

| Artefato | Função |
|----------|--------|
| `.specify/` | Templates, scripts, memory, `feature.json` |
| Skills `speckit-*` em `.cursor/skills/` | Comandos `/speckit-specify`, `plan`, `tasks`, `implement`, … |
| `.specify/memory/constitution.md` | Placeholder — você preenche no passo seguinte |
| `.specify/feature.json` | Aponta a feature ativa (`feature_directory`) |

Confira: `specify integration list` e se as skills aparecem no Cursor.

### 0.4 Constitution

No agente (chat novo):

```text
/speckit-constitution
Princípios: idioma PT-BR; testes em mudanças de domínio; Git Flow feature/bugfix/hotfix;
YAGNI; Conventional Commits em português sem atribuição de IA.
Incluir seção Fluxo Spec Kit: specify → plan → tasks → implement; converge para gaps.
```

Resultado: `.specify/memory/constitution.md` alinhado ao produto. Ajuste no editor se precisar.

### 0.5 Copiar as rules fundamentais

Deste kit para o **produto**:

```bash
mkdir -p .cursor/rules
cp /caminho/para/sdd-playbook/rules/*.mdc .cursor/rules/
```

| Arquivo | alwaysApply | Função |
|---------|-------------|--------|
| `sdd-playbook.mdc` | sim | Fluxo curto + economia |
| `feature-ativa-sdd.mdc` | sim | Lê `feature.json`; segue `tasks.md` |
| `padroes-gerais.mdc` | sim | Idioma, Git Flow, commits |
| `padroes-stack.mdc` | sim | Layout e stack — **preencha** |
| `sdd-economia-receita.mdc` | não | Receita completa sob demanda (não infla todo chat) |

Edite `padroes-stack.mdc`: troque os `<!-- PREENCHER -->` pela stack real. Só edite `padroes-gerais.mdc` se quiser mudar idioma ou Git Flow.

Após a cópia, as rules no produto bastam — não dependem deste repositório.

### 0.6 Plan template

Edite `.specify/templates/plan-template.md` (Language, Dependencies, Storage, Testing, Target Platform) com a stack do produto. O `/speckit-plan` usa isso.

### 0.7 Skill de integração (opcional)

Só se houver API externa:

- Crie `.cursor/skills/<nome>/SKILL.md`
- Description com “when to use”
- Links oficiais + checklist curto
- **Não** copie OpenAPI inteira — o agent consulta a doc sob demanda

### 0.8 Checklist — pronto para a 1ª feature

- [ ] `specify init` feito; skills Spec Kit ok
- [ ] Constitution preenchida
- [ ] Rules copiadas; `padroes-stack` preenchido
- [ ] Plan template com a stack
- [ ] (Opcional) skill de API externa

---

## Etapa 1 — Texto do specify (chat em modo Plan)

**Objetivo:** sair com um prompt fechado. **Ainda não** rode `/speckit-specify`.

1. Chat novo em **modo Plan**.
2. Descreva a feature em linguagem de produto.
3. Responda dúvidas do agente.
4. Diagrama/mock só se achar lacuna (1x).
5. Feche o texto (escopo, aceite, fora de escopo).
6. Não implemente neste chat.

**Texto bom tem:** quem; o quê o usuário faz/vê; persistência em linguagem de negócio; aceite testável; fora de escopo; “detalhe de API → skill X” (se houver).

Feche o chat de Plan (histórico grande = caro).

---

## Etapa 2 — `/speckit-specify`

Chat **novo** (Agent):

```text
/speckit-specify
<cole o texto final da etapa 1>
```

**O que acontece:** `specs/00N-slug/spec.md`, atualiza `.specify/feature.json`, costuma alinhar branch `feature/...`.

**Você:** lê `spec.md` (2–5 min). Ajuste no **editor**. Só re-specify se o escopo mudou de verdade.

**Pular por padrão:** `/speckit-clarify` (exceto pagamento / permissão / risco alto com spec ambígua).

---

## Etapa 3 — `/speckit-plan`

Mesmo chat da feature:

```text
/speckit-plan
```

Opcional (1 linha): `Usar skill <nome-api>.`

**O que acontece:** `plan.md`, `research.md`, `data-model.md`, contratos, `quickstart.md`.

**Você:** skim. Ajuste no editor. Não re-plan por cosmético.

---

## Etapa 4 — `/speckit-tasks`

```text
/speckit-tasks
```

**O que acontece:** `tasks.md` (T001…).

**Você:** corte o que não é MVP **antes** do implement. Pular analyze/checklist salvo risco alto.

---

## Etapa 5 — `/speckit-implement` (por etapas, sequencial)

Mesmo chat da feature.

### Primeira mensagem

```text
/speckit-implement
Só Phase 1–2 e US1 (até o checkpoint da US1).
Sequencial: uma task por vez, sem paralelo.
Ao terminar o checkpoint, PARE e espere eu autorizar a próxima etapa.
Marque [X] em tasks.md ao concluir cada task.
Não reexplore o que já está [X]. Não expanda além de spec.md.
```

### Próximas (curtas)

```text
Autorizado: US2. Continue a partir de T016.
Sequencial, sem paralelo. Pare no checkpoint US2.
```

**Se já decidiu fechar a feature inteira:**

```text
/speckit-implement
Tudo em ordem T001…fim, sequencial, sem paralelo.
Não pare entre US. Não reexplore [X].
```

(Menos overhead que N autorizações; você não corta MVP no meio.)

**Gaps vs spec:** uma vez `/speckit-converge`, depois implement — não novo specify.

---

## Etapa 6 — Depois do código

1. Smoke / testes conforme `quickstart.md` e aceite da spec.
2. PR no Git Flow → `main`.
3. Deploy se a validação exigir produção.
4. Próxima feature = **chat novo** + etapa 1 (Plan) de novo.

---

## Exemplo de texto para `/speckit-specify`

(Feature genérica — adapte ao seu domínio.)

```text
Permitir que o usuário autenticado receba notificação por e-mail quando um pedido mudar para o status "enviado".

Quem: usuário dono do pedido (conta com e-mail verificado).

O quê:
- Ao transição do pedido para "enviado", enfileirar o envio de um e-mail.
- Conteúdo mínimo: número do pedido, data/hora do envio, link para acompanhar.
- Se o e-mail falhar após retries definidos no plan, registrar falha auditável; não alterar o status do pedido.
- Preferências: se o usuário desativou "e-mails de status de pedido", não enviar (e não enfileirar).

Persistência:
- Registrar tentativa/resultado do envio ligado ao pedido (sucesso, falha, timestamp).
- Não duplicar e-mail para a mesma transição (idempotência por pedido + status).

Critérios de aceite:
- Pedido vai para "enviado" → usuário com preferência ativa recebe o e-mail com os campos mínimos.
- Preferência desativada → nenhum e-mail enfileirado nem enviado.
- Falha de provedor após retries → falha registrada; status do pedido permanece "enviado".
- Mesma transição não gera segundo e-mail.

Fora de escopo: SMS/push, outros status, template visual rico, painel de marketing, alteração do fluxo de checkout.
Detalhe de provedor de e-mail: skill email-provider (se existir no projeto).
```

---

## Faça / Evite

| Faça | Evite |
|------|--------|
| Chat Plan só para o texto do specify | Specify no mesmo chat interminável de brainstorm |
| 1 chat por feature no ciclo SDD | Novo chat a cada task |
| Implement por US + “PARE” | Implement tudo sem querer MVP |
| Sequencial | Paralelo “para economizar” (quase não corta token) |
| Editar `spec` / `plan` / `tasks` | Re-specify / re-plan por nitpick |
| Skill de API sob demanda | Colar OpenAPI no prompt do specify |
| `sdd-economia-receita` sob demanda | Colar a receita inteira em every turn |

---

## Ordem mínima (copiar)

1. Etapa 0 (Spec Kit + constitution + rules + plan-template).  
2. Chat Plan → fechar texto specify.  
3. Chat novo → `/speckit-specify` + texto.  
4. Revisar `spec.md` (editor).  
5. `/speckit-plan` → revisar (editor).  
6. `/speckit-tasks` → cortar MVP (editor).  
7. `/speckit-implement` → US1 → autorizar → US2 → …  
8. Testar → PR → (deploy) → próxima feature em chat novo.

---

## Mapa mental

```text
[Chat Plan]     conversa + texto specify     ← joga fora depois
      ↓
[Chat Feature]  specify → plan → tasks → implement por US
      ↓
editar markdown no editor (não re-rodar comando)
      ↓
PR / deploy
```

# Playbook SDD — economia máxima

Receita de bolo: Spec-Driven Development (Spec Kit + Cursor) do **projeto zero** até a **feature no código**, gastando o mínimo de IA.

Rules prontas para copiar: pasta [`rules/`](rules/).

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

Deste playbook para o **produto**:

```bash
mkdir -p .cursor/rules
cp /caminho/para/sdd-playbook/rules/*.mdc .cursor/rules/
```

| Arquivo | alwaysApply | Função |
|---------|-------------|--------|
| `sdd-playbook.mdc` | sim | Fluxo curto + economia |
| `feature-ativa-sdd.mdc` | sim | Lê `feature.json`; segue `tasks.md` |
| `padroes-gerais.mdc` | sim | Idioma, Git Flow, commits — **edite** a stack |
| `sdd-economia-receita.mdc` | não | Receita completa sob demanda (não infla todo chat) |

Edite `padroes-gerais.mdc`: troque os `<!-- AJUSTE -->` pela stack real (Laravel, pasta `site/`, etc.).

### 0.6 Plan template

Edite `.specify/templates/plan-template.md` (Language, Dependencies, Storage, Testing, Target Platform) com a stack do produto. O `/speckit-plan` usa isso.

### 0.7 Skill de integração (opcional)

Só se houver API externa (PagBank, Stripe, …):

- Crie `.cursor/skills/<nome>/SKILL.md`
- Description com “when to use”
- Links oficiais + checklist curto
- **Não** copie OpenAPI inteira — o agent consulta a doc sob demanda

### 0.8 Checklist — pronto para a 1ª feature

- [ ] `specify init` feito; skills Spec Kit ok
- [ ] Constitution preenchida
- [ ] Rules copiadas e `padroes-gerais` ajustado
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

**Texto bom tem:** quem; o quê o usuário faz/vê; persistência em linguagem de negócio; aceite testável; fora de escopo; “detalhe de API → skill X”.

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

(Feature real de validação PagBank PIX no admin — adaptável.)

```text
Integrar PagBank (conta pessoa física) no admin da plataforma Salão Elite, para o administrador validar recebimento PIX após deploy em produção.

Criar página de configuração PagBank no admin (/admin) com:
- Credenciais por ambiente (sandbox e produção). Dados de credencial/API: skill pagbank-api.
- Token persistido com segurança: mascarar na UI; não expor em logs nem nas respostas Inertia.
- Seletor/ambiente ativo: a geração do PIX usa as credenciais do ambiente selecionado (obrigatório estar salvas antes de gerar).

Na mesma página:
- Campos para gerar o PIX: CPF e nome do pagador (obrigatórios na tela). Demais dados de customer exigidos pela API: conforme skill pagbank-api (campos mínimos extras na tela ou valores de teste fixos definidos no plan — sem inventar fora da doc).
- Ao criar a cobrança, persistir o CPF (e o nome) na cobrança; na UI mascarar CPF; não logar CPF/token em claro.
- Gerar PIX fixo de R$ 1,00 via API de Pedidos (QR Code); exibir QR e copia-e-cola.
- Região A — cobrança atual: a última gerada com sucesso (valor, ids PagBank, status, timestamps, CPF mascarado, nome). Status mínimos: pendente e pago.
- Região B — notificações de webhook da cobrança atual.
- Tempo real (sem F5): ao criar cobrança, Região A atualiza; ao chegar webhook, Região B e o status na Região A atualizam.
- Erros da API/fluxo: mostrar a mensagem recebida na página.
- Consulta/histórico: listar cobranças anteriores e as notificações de cada uma (a visão principal continua na cobrança atual).

Persistência:
- Gerar PIX → gravar cobrança (valor, ids PagBank, status pendente, CPF, nome, timestamps).
- Webhook → gravar notificação; atualizar cobrança vinculada de forma idempotente; notificar o admin em tempo real.

Validação: apenas em produção após deploy (sem exigir teste de webhook em local).

Critérios de aceite:
- Admin salva credenciais do ambiente (token mascarado, sem vazamento).
- Sem credencial do ambiente selecionado, não gera PIX (feedback claro).
- Admin informa CPF e nome, gera PIX de R$ 1,00, vê QR/copia-e-cola; Região A mostra a cobrança atual em tempo real.
- Webhook persiste notificação, atualiza cobrança (ex.: para pago) e a página atualiza Região B e status em tempo real.
- Histórico permite consultar cobranças e notificações; principal = cobrança atual.

Fora de escopo: mensalidade/assinatura, alterar salão, cartão/boleto, painel do salão, valor livre, teste de webhook em local.
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

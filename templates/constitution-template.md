# Constitution do Projeto

**Version**: 1.0.0 | **Ratified**: [DATA] | **Last Amended**: [DATA]

## Core Principles

### I. Idioma e comunicação

Todo texto de produto, mensagens de erro, rótulos de UI e documentação MUST
ser em português brasileiro (ou o idioma do produto). Código de domínio MUST
seguir a convenção de nomenclatura do time (documentar aqui).

**Rationale**: reduz atrito entre negócio, UI e código.

### II. Arquitetura

[Descrever onde mora a regra de negócio, o que controllers/UI podem e não podem
fazer, pastas principais.]

**Rationale**: evita lógica espalhada e exploração desnecessária pelo agente.

### III. Qualidade e escopo

Mudanças MUST manter ou acrescentar testes adequados. Preferir YAGNI: não
expandir escopo além da feature/spec em curso. [Regras de banco/produção.]

**Rationale**: regressões caras; escopo creep atrasa entrega.

### IV. Git e commits

Integração na branch principal só via PR. Branches significativas
(`feature/<slug>`, `bugfix/<slug>`, `hotfix/<slug>`). Conventional Commits.
Commits e PRs MUST NOT incluir atribuição de ferramenta/IA. Não commitar
segredos. Só criar commit quando pedido explicitamente.

**Rationale**: histórico legível e revisão humana objetiva.

## Fluxo Spec Kit

Funcionalidades **relevantes** MUST seguir Spec-Driven Development:

1. `/speckit-specify` — especificação de produto
2. `/speckit-clarify` — só se ambiguidade crítica
3. `/speckit-plan` — plano técnico
4. `/speckit-tasks` — tarefas acionáveis
5. `/speckit-implement` — implementação
6. `/speckit-converge` — gaps pós-implementação (opcional)

Skills opcionais: `analyze`, `checklist`. Preferir **um chat por feature** e
revisão humana dos artefatos entre fases.

## Governance

Esta constitution prevalece sobre hábito ad hoc. Em conflito com uma rule do
agente, preferir a orientação **mais específica** ao arquivo/contexto; em
princípios de Git e qualidade, prevalece a constitution.

Emendas: atualizar este arquivo, incrementar versão, registrar **Last Amended**.

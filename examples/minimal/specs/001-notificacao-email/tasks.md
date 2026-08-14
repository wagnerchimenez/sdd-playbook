# Tasks: Notificação de confirmação por e-mail

**Input**: Design documents from `/specs/001-notificacao-email/`

**Prerequisites**: plan.md, spec.md

**Organization**: por user story; Phase 3 = MVP (P1)

## Format: `[ID] [P?] [Story] Description`

## Phase 1: Setup

**Purpose**: Estrutura mínima do módulo

- [ ] T001 Criar pastas `src/domain/auth`, `src/application/auth`, `src/infrastructure/email` conforme plan.md
- [ ] T002 [P] Adicionar config de expiração de token e URL base do link em arquivo de config do app

## Phase 2: Foundational

**Purpose**: Porta de e-mail e modelo de token

- [ ] T003 Definir entidade/VO de ConfirmationToken em `src/domain/auth/`
- [ ] T004 Definir porta `EmailNotifier` em `src/domain/auth/` (ou `application`)
- [ ] T005 [P] Implementar adapter de e-mail em `src/infrastructure/email/` (console ou SMTP fake em dev)
- [ ] T006 Migration/tabela de tokens em `src/infrastructure/persistence/`

**Checkpoint**: base pronta para P1

## Phase 3: User Story 1 - Confirmar conta (P1) — MVP

**Goal**: Cadastro dispara e-mail; link confirma a conta

- [ ] T007 [US1] Caso de uso `SendConfirmationEmail` em `src/application/auth/` chamado após cadastro
- [ ] T008 [US1] Caso de uso `ConfirmAccount` em `src/application/auth/`
- [ ] T009 [US1] Rotas HTTP POST/GET de confirmação em `src/interfaces/http/`
- [ ] T010 [P] [US1] Testes de integração do fluxo feliz e token inválido em `tests/integration/`
- [ ] T011 [US1] Invalidar token após uso e rejeitar reuso

**Checkpoint**: P1 demonstrável

## Phase 4: User Story 2 - Reenvio (P2)

**Goal**: Reenvio com rate limit

- [ ] T012 [US2] Caso de uso `ResendConfirmationEmail` com rate limit em `src/application/auth/`
- [ ] T013 [US2] Endpoint HTTP de reenvio em `src/interfaces/http/`
- [ ] T014 [P] [US2] Testes de rate limit em `tests/integration/`

## Phase 5: Polish

- [ ] T015 Revisar mensagens de erro em português
- [ ] T016 Atualizar quickstart da feature (se existir) com passos de teste manual

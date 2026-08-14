# Implementation Plan: Notificação de confirmação por e-mail

**Branch**: `001-notificacao-email` | **Date**: 2026-08-14 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `/specs/001-notificacao-email/spec.md`

## Summary

Após o cadastro, gerar token, enviar e-mail via porta de notificação e expor endpoint de confirmação. P2 adiciona reenvio com rate limit.

## Technical Context

**Language/Version**: TypeScript 5.x / Node 20

**Primary Dependencies**: Express, driver SQL (Prisma ou similar), fila opcional

**Storage**: PostgreSQL

**Testing**: `npm test`

**Target Platform**: Linux server / Web API

**Project Type**: web-service

**Performance Goals**: confirmação &lt; 300ms p95 no endpoint

**Constraints**: não acoplar a um provedor de e-mail no domínio; P1 sem UI rica de e-mail

**Scale/Scope**: módulo de autenticação/cadastro existente

## Constitution Check

- Camadas respeitadas (application/domain/infrastructure): OK
- Escopo limitado a P1+P2 descritos: OK
- Testes previstos nas tasks: OK

## Project Structure

### Documentation (this feature)

```text
specs/001-notificacao-email/
├── plan.md
├── spec.md
└── tasks.md
```

### Source Code (repository)

```text
src/
├── domain/
│   └── auth/
├── application/
│   └── auth/
├── infrastructure/
│   ├── email/
│   └── persistence/
└── interfaces/
    └── http/
tests/
├── unit/
└── integration/
```

**Structure Decision**: tokens e regras em `domain/auth`; casos de uso em `application/auth`; SMTP/API de e-mail em `infrastructure/email`.

## Complexity Tracking

| Violation | Why Needed | Simpler Alternative Rejected Because |
|-----------|------------|-------------------------------------|
| — | — | — |

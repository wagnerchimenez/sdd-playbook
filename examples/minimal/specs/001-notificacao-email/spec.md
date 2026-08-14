# Feature Specification: Notificação de confirmação por e-mail

**Feature Branch**: `001-notificacao-email`

**Created**: 2026-08-14

**Status**: Draft

**Input**: Após criar uma conta, o usuário recebe e-mail de confirmação…

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Confirmar conta pelo e-mail (Priority: P1)

Após o cadastro, o usuário recebe um e-mail com link. Ao abrir o link válido, a conta passa a confirmada e ele consegue entrar nas áreas que exigem confirmação.

**Why this priority**: Sem confirmação, o produto não confia no e-mail do usuário; é o MVP.

**Independent Test**: Cadastrar usuário de teste, abrir o link do e-mail (ou token em ambiente de teste) e verificar status confirmado.

**Acceptance Scenarios**:

1. **Given** cadastro válido concluído, **When** o sistema processa o cadastro, **Then** um e-mail de confirmação é enfileirado/enviado com link único.
2. **Given** token válido e não expirado, **When** o usuário abre o link, **Then** a conta fica confirmada e o token é invalidado.
3. **Given** token expirado ou já usado, **When** o usuário abre o link, **Then** vê mensagem clara de falha e a conta permanece não confirmada.

---

### User Story 2 - Reenviar e-mail (Priority: P2)

Usuário não confirmado pode pedir reenvio, com limite de frequência.

**Why this priority**: Melhora suporte, mas o P1 já entrega o fluxo principal.

**Independent Test**: Com conta não confirmada, solicitar reenvio duas vezes rápidas e verificar rate limit.

**Acceptance Scenarios**:

1. **Given** conta não confirmada, **When** solicita reenvio e está dentro do limite, **Then** novo e-mail é enviado e tokens anteriores deixam de valer.
2. **Given** reenvio recente, **When** solicita de novo cedo demais, **Then** recebe aviso de espera e nenhum e-mail extra é enviado.

### Edge Cases

- E-mail do provedor falha: cadastro não deve corromper estado; permitir reenvio (P2) ou fila com retry.
- Usuário já confirmado abre link antigo: mensagem neutra, sem erro assustador.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Sistema MUST enviar e-mail de confirmação após cadastro bem-sucedido.
- **FR-002**: Link MUST usar token de uso único com expiração definida.
- **FR-003**: Confirmação bem-sucedida MUST marcar a conta como confirmada.
- **FR-004**: Sistema MUST rejeitar tokens inválidos, expirados ou reutilizados com mensagem clara.
- **FR-005**: (P2) Sistema MUST permitir reenvio com rate limit configurável.

### Key Entities

- **User**: conta com status de confirmação e e-mail.
- **ConfirmationToken**: token, expiração, vínculo ao usuário, estado usado/ativo.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Usuário completa confirmação em menos de 2 minutos após receber o e-mail (fluxo feliz).
- **SC-002**: 100% dos tokens usados uma vez não podem confirmar de novo.
- **SC-003**: P1 utilizável em staging sem depender de P2.

## Assumptions

- Já existe cadastro e login básicos.
- Há (ou será criada) porta de envio de e-mail abstrata; provedor concreto é detalhe de infra.
- Idioma do e-mail: português brasileiro.

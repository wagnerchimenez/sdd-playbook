# 06 — Quando não usar SDD

SDD completo (4 prompts + artefatos) tem custo fixo. Use prompt direto quando o custo de especificar for maior que o risco de errar.

## Use prompt direto (sem Spec Kit)

| Situação | Abordagem |
|----------|-----------|
| Bugfix pontual | Branch `bugfix/slug` + prompt com sintoma, esperado, arquivo suspeito |
| Hotfix em produção | Branch `hotfix/slug` + mudança mínima + teste de regressão |
| Copy / rótulo / CSS | Rule de UI + prompt curto; sem `spec.md` |
| Refactor sem mudança de comportamento | Prompt focado + testes verdes; citar arquivos |
| Typo, config, CI one-liner | Você mesmo ou prompt de 1 frase |
| Spike / PoC descartável | Branch throwaway; spec leve opcional |

### Exemplo bugfix

```text
No endpoint de login, senha com espaços no fim falha. Esperado: trim antes de validar.
Arquivo suspeito: app/Http/Requests/LoginRequest.php
Adicione teste de regressão. Não refatore fora desse escopo.
```

## Use SDD completo

- Nova capacidade de produto (user-facing)
- Mudança que toca vários módulos
- Contrato de API novo
- Cobrança, permissões, dados sensíveis
- Qualquer coisa que você precisaria explicar duas vezes para outra pessoa

## Meio-termo (spec leve)

Para features pequenas mas novas:

1. Só `/speckit-specify` + editar `spec.md`
2. Pular plan formal se o plan-template e a arquitetura já bastam
3. Escrever `tasks.md` **você** (5–10 bullets) e pedir implement

Isso ainda versiona o “o quê”, com menos requests.

## Anti-padrões caros

1. **Specify para tudo** — inclusive rename de variável.
2. **Analyze em loop** — um analyze; corrija artefatos; implement.
3. **Novo specify porque o implement falhou** — use `converge` ou edite `tasks.md`.
4. **Uma spec com 8 user stories** — fatie em 2 features.
5. **Chat novo por Phase** — mesmo chat; “continue Phase N”.

## Decisão em 10 segundos

```text
A mudança altera comportamento de produto ou contrato?
  NÃO → prompt direto
  SIM → cabe em 1 user story clara?
           SIM → SDD mínimo (4 prompts) ou spec leve
           NÃO → SDD + possível 2ª feature para P2
```

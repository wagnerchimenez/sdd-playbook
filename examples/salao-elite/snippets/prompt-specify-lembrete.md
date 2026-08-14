# Prompt specify — exemplo (lembrete)

```text
/speckit-specify Dono do salão envia lembrete WhatsApp 24h antes do agendamento.

P1: lembrete automático configurável por salão (ligar/desligar + antecedência padrão 24h).
P2: cliente pode optar por não receber lembrete.

Fora de escopo: integração Meta Business API oficial; templates aprovados pela Meta;
envio SMS; campanhas de marketing.

Cenários:
- Given salão com lembrete ativo e agendamento em 24h, When o job diário roda, Then um lembrete é enfileirado para o telefone do cliente.
- Given agendamento cancelado antes do envio, When o job roda, Then nenhum lembrete é enviado.
- Given salão com fuso America/Manaus, When calcula a janela de 24h, Then usa o fuso do salão — não o do servidor UTC cego.

Aceite: respeitar fuso do salão; não enviar se cancelado; P1 utilizável sem P2.
```

## Por que este prompt economiza requests

- Ator e P1/P2 claros → menos `clarify`
- Fora de escopo → plan não inventa Meta API
- Cenários com fuso e cancelamento → tasks já nascem com edge cases
- Aceite mensurável → implement sabe quando parar

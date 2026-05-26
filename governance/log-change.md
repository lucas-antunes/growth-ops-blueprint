---
name: log-change
description: >
  Rode APÓS qualquer escrita em plataforma de ads (Tier 2 ou Tier 3).
  Registra a mudança no log compartilhado para que outras sessões
  saibam o que foi feito, por quem e quando.
---

# Log Change

<!--
  ANOTAÇÃO PEDAGÓGICA:
  
  Se o /pre-write-check é a verificação antes de agir,
  o /log-change é o registro depois de agir.
  
  Por que registrar? Porque o Claude que roda amanhã não sabe
  o que o Claude de hoje fez. Sem o log, cada sessão age
  como se fosse a primeira — e repete erros ou desfaz decisões.
  
  O log também serve para você: em 3 semanas, você vai querer saber
  por que aquela campanha foi pausada. O log tem a resposta.
-->

## Quando Usar

Rode **imediatamente após** qualquer ação Tier 2 ou Tier 3 bem-sucedida em ads.
Sem exceções — inclusive para mudanças pequenas.

## Processo

### Passo 1 — Coletar os Detalhes da Mudança

| Campo | Descrição | Exemplo |
|-------|-----------|---------|
| `plataforma` | Qual plataforma de ads | Google Ads |
| `conta` | Nome e ID da conta | Brasil ({{ACCOUNT_ID}}) |
| `campanha_id` | ID(s) da campanha afetada | 12345678 |
| `campanha_nome` | Nome(s) da campanha | [BR] PMax \| Desktop |
| `tier` | Tier da ação (2 ou 3) | 3 |
| `ação` | O que foi alterado | Aumento de orçamento |
| `valor_anterior` | Estado antes | $500/dia |
| `valor_novo` | Estado depois | $650/dia |
| `racional` | Por que a mudança foi feita | ROAS 3.2x com 70% de impression share perdido por orçamento |
| `rollback_trigger` | O que causaria reversão (Tier 3) | ROAS < 1.0x por 2 dias consecutivos |

### Passo 2 — Calcular Datas

- **Data da mudança:** agora (UTC)
- **Data de revisão:** mínimo 7 dias a partir de hoje. Estender para:
  - 14 dias: mudanças de tROAS/tCPA (período de aprendizado)
  - 28 dias: novas campanhas em ramp-up
  - 7 dias: a maioria dos casos

### Passo 3 — Determinar Status

| Situação | Status |
|----------|--------|
| Mudança normal Tier 2 ou 3 | `ACTIVE` |
| Mudança com período de monitoramento estendido | `MONITORING` |
| Pausa de emergência de mudança de outra sessão | `EMERGENCY_PAUSED` |

### Passo 4 — Registrar

Adicione uma entrada em `memory/ads-change-log.md` (ou no Google Sheet configurado):

```markdown
### [YYYY-MM-DD HH:MM UTC] — {{OPERATOR_NAME}}

**Plataforma:** {{ADS_PLATFORM}}
**Conta:** {{MARKET}} ({{ACCOUNT_ID}})
**Campanha:** [nome] (ID: [id])
**Tier:** [2 ou 3]
**Ação:** [o que foi feito]
**Antes:** [valor anterior]
**Depois:** [valor novo]
**Racional:** [por que, com dados]
**Revisão:** [data]
**Rollback se:** [condição — Tier 3 apenas]
**Status:** ACTIVE / MONITORING / EMERGENCY_PAUSED
```

### Passo 5 — Confirmar

Output para o operador:

```
## Mudança Registrada

**Ação:** [o que foi alterado]
**Campanha:** [nome] ([id])
**Status:** ACTIVE
**Ownership expira:** [data]
**Data de revisão:** [data]

Registrado no log. Futuras sessões verão esta mudança antes de alterar a mesma campanha.
```

## Múltiplas Campanhas na Mesma Sessão

Se várias campanhas foram alteradas na mesma ação (ex: escalar orçamentos em 3 campanhas), registre cada campanha como uma **entrada separada** com o mesmo timestamp. Isso permite rastreamento de ownership por campanha.

## Pausa de Emergência

Ao registrar uma `EMERGENCY_PAUSED`:
1. Anote o **owner original** no racional (ex: "Pausa de emergência da mudança de [sessão anterior] de [data]")
2. Especifique qual **threshold do circuit breaker** foi atingido
3. Não modifique configurações — só registre a pausa

---
name: pre-write-check
description: >
  Rode ANTES de qualquer escrita Tier 3 em plataforma de ads.
  Verifica o changelog da plataforma e o log compartilhado por conflitos.
  Retorna PROCEED, BLOCKED ou EMERGENCY.
---

# Pre-Write Check

<!--
  ANOTAÇÃO PEDAGÓGICA:
  
  Este skill existe porque o Claude não tem memória entre sessões.
  Sem ele, uma sessão de terça pode desfazer o experimento que
  a sessão de segunda começou — sem saber que ele existia.
  
  O pre-write check força o sistema a perguntar:
  "Alguém (eu mesmo, em outra sessão) tocou nessa campanha recentemente?"
  
  É chato? Um pouco. Vale a pena? Sempre que você evitar um conflito, sim.
-->

## Quando Usar

**Obrigatório** antes de qualquer ação Tier 3.
**Recomendado** antes de ações Tier 2 como boa prática.

## Processo

### Passo 1 — Identificar a Ação

Determine:
- **Plataforma:** Google Ads / Meta Ads / outro
- **Conta:** qual conta e ID
- **Campanha(s):** quais campanhas serão afetadas
- **Ação:** o que vai mudar (orçamento, status, lance, etc.)
- **Tier:** 2 ou 3 (se Tier 1, não precisa de check)

### Passo 2 — Consultar Histórico da Plataforma

**Google Ads — query GAQL por conta:**
```sql
SELECT
  change_event.change_date_time,
  change_event.user_email,
  change_event.resource_change_operation,
  change_event.client_type,
  campaign.name,
  campaign.id
FROM change_event
WHERE change_event.change_date_time DURING LAST_14_DAYS
  AND change_event.change_resource_type IN (
    'CAMPAIGN', 'CAMPAIGN_BUDGET', 'AD_GROUP',
    'AD_GROUP_AD', 'BIDDING_STRATEGY'
  )
LIMIT 100
```

**Meta Ads:** consultar activity log da conta.

**Importante:** use `DURING LAST_14_DAYS` (não ultrapasse 14 dias). Não inclua `changed_fields` — causa erros.

### Passo 3 — Ler o Log Compartilhado

Abra `memory/ads-change-log.md` e filtre por:
- Mesma plataforma e mesma conta
- Últimos 14 dias
- Status `ACTIVE` ou `MONITORING` (não `COMPLETED` ou `REVERTED`)

### Passo 4 — Verificar Conflito

Para **cada campanha** que você quer alterar:

**Pergunta 1:** Outra sessão fez uma mudança Tier 3 nesta campanha nos últimos 7 dias?

**Pergunta 2:** Existe um experimento ativo nesta campanha com data de revisão futura?

### Passo 5 — Decisão

**Sem conflito:**
```
RESULTADO: PROCEED
- Sem ownership ativa na campanha alvo
- Você pode executar a mudança
- Após executar, rode /log-change para registrar
```

**Com conflito (mudança de outra sessão nos últimos 7 dias ou experimento ativo):**
```
RESULTADO: BLOCKED
- Campanha [nome] foi alterada em [data]
- Mudança: [o que foi alterado]
- Ownership expira: [data + 7 dias, ou data de revisão se posterior]
- Aguarde até [data] antes de modificar esta campanha
```

**Com conflito + threshold de emergência atingido:**
```
RESULTADO: EMERGENCY
- Threshold atingido: [qual condição]
- Você pode PAUSAR a campanha apenas — não modifique configurações
- Após pausar, rode /log-change com status EMERGENCY_PAUSED
```

### Formato de Output

```
## Resultado do Pre-Write Check

**Ação:** [o que você quer fazer]
**Alvo:** [plataforma] / [conta] / [campanha]
**Tier:** [2 ou 3]

### Histórico Recente (últimos 14 dias)
[Tabela de mudanças recentes nesta campanha]

### Verificação de Conflito
[PROCEED / BLOCKED / EMERGENCY com detalhes]

### Próximos Passos
[O que fazer — executar + logar, aguardar, ou pausa de emergência]
```

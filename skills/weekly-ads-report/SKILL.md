---
name: weekly-ads-report
description: >
  Use quando o usuário pedir análise de performance de ads, relatório semanal
  de ads, checar ROAS, ou "como estão os ads de {{COMPANY_NAME}}".
---

# Relatório Semanal de Ads

<!--
  ANOTAÇÃO PEDAGÓGICA:
  
  Esta skill é focada exclusivamente em ads — mais fundo do que a seção 2
  do weekly-protocol. Use quando quiser drill down nos anúncios sem rodar
  o protocolo completo.
  
  A estrutura em 3 tiers de mercado (principal, médio, pequeno) é proposital:
  você não analisa Kuwait com o mesmo nível de detalhe que India ou UAE.
  Calibre os seus próprios tiers pelo volume de spend.
  
  O mais importante deste relatório: o changelog de campanhas.
  Performance caiu? O changelog mostra se foi uma mudança sua ou
  uma decisão da plataforma. Sem isso, você analisa os números no escuro.
-->

## Visão Geral

Análise padronizada de performance de {{ADS_PLATFORM}} para {{COMPANY_NAME}}.
Produz comparação semanal com tendências WoW, análise de ROAS e alertas.

## Workflow

### Passo 1 — Definir Janelas de Data

Use semanas de segunda a domingo:
- **W3** (semana completa mais recente): seg-dom anterior
- **W2**: a semana antes de W3
- **W1**: a semana antes de W2
- **Parcial**: segunda-feira desta semana até ontem

### Passo 2 — Puxar Dados (queries em paralelo)

Rode uma query por conta ativa em paralelo. Todas as contas de uma vez:

**Google Ads — query por conta:**
```sql
SELECT
  segments.date,
  metrics.impressions,
  metrics.clicks,
  metrics.cost_micros,
  metrics.conversions,
  metrics.conversions_value
FROM customer
WHERE segments.date BETWEEN '{data_inicio}' AND '{data_fim}'
ORDER BY segments.date ASC
```

**Contas ativas:**
{{ADS_ACCOUNTS_TABLE}}

**Nota de moeda:** {{CURRENCY_CONVERSION_NOTES}}

### Passo 3 — Agregar por Semana

Para cada mercado, some os dados diários em buckets semanais (W1, W2, W3, Parcial).

Calcule por semana:
- **Spend** = `cost_micros ÷ 1.000.000`
- **Conversões** = soma de conversões
- **Valor Convertido** = soma de conversions_value
- **ROAS** = Valor Convertido ÷ Spend
- **CPA** = Spend ÷ Conversões
- **ROAS Real** = Receita `{{DB_NAME}}` do mercado ÷ Spend

### Passo 4 — Calcular Variações WoW

Para cada mercado, calcule W2 vs W1 e W3 vs W2:
- % de variação em spend
- % de variação em conversões
- Variação absoluta de ROAS (ex: 1.75x → 1.36x)

### Passo 5 — Sinalizar Anomalias

Sinalize automaticamente:
- **ROAS < `{{MIN_ROAS_THRESHOLD}}`** em qualquer semana → não lucrativo
- **ROAS caindo por 3+ semanas** → problema estrutural
- **Variação de spend >30%** WoW → mudança de orçamento ou algoritmo
- **Outliers de valor** (um dia > 3x a média diária) → verificar pedidos anômalos
- **Dias com zero conversões** em mercados principais
- **Conta pausada** (sem dados nas datas recentes)

### Passo 6 — Puxar Changelog de Campanhas

Query de mudanças recentes por conta:
```sql
SELECT
  change_status.resource_type,
  change_status.resource_status,
  change_status.last_change_date_time,
  campaign.name,
  campaign.status
FROM change_status
WHERE change_status.last_change_date_time DURING LAST_14_DAYS
  AND change_status.change_resource_type = 'CAMPAIGN'
LIMIT 50
```

**Importante:**
- Use `change_status` (não `change_event` — pode não funcionar via MCP)
- Não selecione `changed_fields` — causa erros
- Rate limit: não rode todas as contas em paralelo — máx. `{{MAX_PARALLEL_CHANGELOG}}` de uma vez
- Cruze as datas do changelog com as quedas de performance

### Passo 7 — Performance por Campanha (mercados com variação significativa)

Para mercados com mudança WoW relevante, puxe dados por campanha:

```sql
SELECT campaign.name, campaign.status,
  metrics.impressions, metrics.clicks,
  metrics.cost_micros, metrics.conversions, metrics.conversions_value
FROM campaign
WHERE segments.date BETWEEN '{inicio}' AND '{fim}'
  AND metrics.impressions > 0
ORDER BY metrics.cost_micros DESC
```

Compare campanha a campanha para identificar:
- Campanhas pausadas que antes gastavam
- Shifts de orçamento entre campanhas
- Novas campanhas que apareceram
- Mudanças de status

### Passo 8 — Estruturar o Relatório

Apresente nesta ordem:

1. **Totais Globais** — spend total (moeda única) e conversões por semana
2. **Mercados Principais** (`{{TIER_1_MARKETS}}`) — tabela semanal completa
3. **Mercados Médios** (`{{TIER_2_MARKETS}}`) — tabela semanal resumida
4. **Mercados Pequenos** (`{{TIER_3_MARKETS}}`) — tabela semanal resumida
5. **Resumo do Changelog** — quem mudou o quê e quando
6. **Impacto por Campanha** — comparativo W3 vs Parcial para mercados com variação
7. **Alertas & Tendências** — lista numerada de achados com referências ao changelog
8. **Recomendações** — o que fazer sobre os itens sinalizados

### Passo 9 — Atualizar Decisions Log

Registre novos achados ou recomendações em `memory/decisions-log.md`.

---

## Referência Rápida: Ranges Saudáveis

Ver `memory/baselines.md` — Seção 2 para targets por mercado.

---

## Erros Comuns

- Comparar números de semana parcial com semana completa sem notar a diferença de dias
- Não verificar outliers de conversão de alto valor que inflam ROAS de um mercado
- Usar dados de mercados/contas pausadas como se fossem ativos
- Não cruzar quedas de performance com o changelog — performance caiu sem mudança = problema da plataforma; caiu com mudança = investigar a mudança

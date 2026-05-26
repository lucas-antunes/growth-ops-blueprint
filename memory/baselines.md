# Baselines — {{COMPANY_NAME}}

<!--
  ANOTAÇÃO PEDAGÓGICA — POR QUE ESTE ARQUIVO EXISTE:
  
  Sem baselines, o Claude não sabe o que é "normal" para o seu negócio.
  Ele pode reportar uma queda de 15% como catastrófica — mas se outubro
  sempre cai 15% por sazonalidade, isso é completamente esperado.
  
  Este arquivo define os ranges saudáveis para cada métrica chave.
  O sistema usa esses números para classificar automaticamente:
  - Verde: dentro do range
  - Amarelo: borderline — monitorar
  - Vermelho: fora do range — investigar
  
  COMO CALIBRAR:
  - Use pelo menos 90 dias de dados históricos
  - Exclua períodos anômalos (promoções, crises, lançamentos, feriados atípicos)
  - Revise trimestralmente — o negócio evolui, os baselines também
  
  QUAL SEÇÃO PREENCHER:
  Preencha a Seção 1 de acordo com o seu tipo de negócio.
  As Seções 2 (Ads) e 3 (Funil) têm variantes por tipo nas anotações.
  As Seções 4 (Sazonalidade) e 5 (Anomalias) são universais.
  
  Última atualização: {{BASELINE_DATE}}
  Período de referência: {{BASELINE_PERIOD}}
-->

---

## Seção 1 — Fonte de Verdade do Negócio

<!--
  ADAPTAR: preencha a variante do seu tipo de negócio e apague as outras.
  
  ──────────────────────────────────────────────────
  [E-COMMERCE] → Receita & Pedidos
  ──────────────────────────────────────────────────
-->

### [E-COMMERCE] Receita & Pedidos

| Métrica | Range Saudável | Alerta | Crítico |
|---------|---------------|--------|---------|
| Pedidos/dia | {{ORDERS_DAY_HEALTHY}} | {{ORDERS_DAY_ALERT}} | {{ORDERS_DAY_CRITICAL}} |
| Receita/dia | {{REVENUE_DAY_HEALTHY}} | {{REVENUE_DAY_ALERT}} | {{REVENUE_DAY_CRITICAL}} |
| AOV (ticket médio) | {{AOV_HEALTHY}} | {{AOV_ALERT}} | {{AOV_CRITICAL}} |
| Taxa de reembolso | <{{REFUND_HEALTHY}} | {{REFUND_ALERT}} | >{{REFUND_CRITICAL}} |

| Mercado | Pedidos/semana | Receita/semana | AOV |
|---------|---------------|----------------|-----|
| {{MARKET_1}} | {{M1_ORDERS}} | {{M1_REVENUE}} | {{M1_AOV}} |
| {{MARKET_2}} | {{M2_ORDERS}} | {{M2_REVENUE}} | {{M2_AOV}} |

<!--
  ──────────────────────────────────────────────────
  [SAAS] → Receita Recorrente (MRR)
  ──────────────────────────────────────────────────
-->

### [SAAS] Receita Recorrente

| Métrica | Range Saudável | Alerta | Crítico |
|---------|---------------|--------|---------|
| MRR total | {{MRR_HEALTHY}} | {{MRR_ALERT}} | {{MRR_CRITICAL}} |
| MRR growth MoM | >{{MRR_GROWTH_HEALTHY}} | {{MRR_GROWTH_ALERT}} | <{{MRR_GROWTH_CRITICAL}} |
| Churn rate (MRR) | <{{CHURN_HEALTHY}} | {{CHURN_ALERT}} | >{{CHURN_CRITICAL}} |
| Net Revenue Retention | >{{NRR_HEALTHY}} | {{NRR_ALERT}} | <{{NRR_CRITICAL}} |
| Trial → Paid CVR | >{{TRIAL_CVR_HEALTHY}} | {{TRIAL_CVR_ALERT}} | <{{TRIAL_CVR_CRITICAL}} |
| ARPU | {{ARPU_HEALTHY}} | {{ARPU_ALERT}} | {{ARPU_CRITICAL}} |

<!--
  ──────────────────────────────────────────────────
  [B2B/LEADS] → Pipeline & Revenue
  ──────────────────────────────────────────────────
-->

### [B2B/LEADS] Pipeline & Revenue

| Métrica | Range Saudável | Alerta | Crítico |
|---------|---------------|--------|---------|
| Novos MQLs/semana | {{MQL_HEALTHY}} | {{MQL_ALERT}} | {{MQL_CRITICAL}} |
| MQL → SQL CVR | >{{MQL_SQL_HEALTHY}} | {{MQL_SQL_ALERT}} | <{{MQL_SQL_CRITICAL}} |
| Taxa de fechamento | >{{CLOSE_RATE_HEALTHY}} | {{CLOSE_RATE_ALERT}} | <{{CLOSE_RATE_CRITICAL}} |
| Ciclo de vendas (dias) | <{{SALES_CYCLE_HEALTHY}} | {{SALES_CYCLE_ALERT}} | >{{SALES_CYCLE_CRITICAL}} |
| Pipeline múltiplo | >{{PIPELINE_MULTIPLE_HEALTHY}}x meta | {{PIPELINE_MULTIPLE_ALERT}}x | <{{PIPELINE_MULTIPLE_CRITICAL}}x |
| Ticket médio | {{TICKET_HEALTHY}} | {{TICKET_ALERT}} | {{TICKET_CRITICAL}} |

<!--
  ──────────────────────────────────────────────────
  [INFOPRODUTO] → Matrículas & Receita
  ──────────────────────────────────────────────────
-->

### [INFOPRODUTO] Matrículas & Receita

| Métrica | Range Saudável | Alerta | Crítico |
|---------|---------------|--------|---------|
| Novas matrículas/semana | {{ENROLLMENTS_HEALTHY}} | {{ENROLLMENTS_ALERT}} | {{ENROLLMENTS_CRITICAL}} |
| CPV (custo por venda) | <{{CPV_HEALTHY}} | {{CPV_ALERT}} | >{{CPV_CRITICAL}} |
| ROI de ads | >{{ROI_HEALTHY}}x | {{ROI_ALERT}}x | <{{ROI_CRITICAL}}x |
| Taxa de reembolso | <{{REFUND_HEALTHY}} | {{REFUND_ALERT}} | >{{REFUND_CRITICAL}} |
| Ticket médio | {{TICKET_HEALTHY}} | {{TICKET_ALERT}} | {{TICKET_CRITICAL}} |

---

## Seção 2 — Performance de Ads

<!--
  UNIVERSAL: mesma estrutura para todos os tipos de negócio.
  A métrica de eficiência muda — veja a anotação.
-->

**Métrica de eficiência usada:** `{{ADS_EFFICIENCY_METRIC}}`

| Segmento / Conta | Target {{ADS_EFFICIENCY_METRIC}} | CPA Típico | Spend Semanal Típico |
|------------------|----------------------------------|------------|---------------------|
| {{SEGMENT_1}} | {{S1_EFFICIENCY_TARGET}} | {{S1_CPA}} | {{S1_WEEKLY_SPEND}} |
| {{SEGMENT_2}} | {{S2_EFFICIENCY_TARGET}} | {{S2_CPA}} | {{S2_WEEKLY_SPEND}} |

**Alertas automáticos de ads:**
- `{{ADS_EFFICIENCY_METRIC}}` abaixo de `{{MIN_EFFICIENCY}}` → flag imediato
- Eficiência caindo por 3+ períodos → problema estrutural
- Variação de spend >30% sem mudança de performance → checar changelog
- Zero conversões por `{{ZERO_CONV_THRESHOLD}}` dias → alerta crítico

---

## Seção 3 — Funil de Aquisição

<!--
  ADAPTAR: as métricas do funil mudam por tipo de negócio.
  Mantenha apenas as linhas relevantes para o seu perfil.
-->

| Métrica | Range Saudável | Alerta | Crítico | Tipo |
|---------|---------------|--------|---------|------|
| Taxa de conversão compra | >{{ECOM_CVR_HEALTHY}} | — | <{{ECOM_CVR_CRITICAL}} | [E-COMMERCE] |
| Taxa de carrinho | >{{ATC_HEALTHY}} | — | <{{ATC_CRITICAL}} | [E-COMMERCE] |
| Trial activation | >{{TRIAL_ACT_HEALTHY}} | — | <{{TRIAL_ACT_CRITICAL}} | [SAAS] |
| Lead → MQL CVR | >{{LEAD_MQL_HEALTHY}} | — | <{{LEAD_MQL_CRITICAL}} | [B2B/LEADS] |
| Show-up rate (webinar) | >{{SHOWUP_HEALTHY}} | — | <{{SHOWUP_CRITICAL}} | [INFOPRODUTO] |
| Lead → matrícula CVR | >{{LEAD_SALE_HEALTHY}} | — | <{{LEAD_SALE_CRITICAL}} | [INFOPRODUTO] |
| CPC como % de receita | >{{CPC_SHARE_HEALTHY}} | — | <{{CPC_SHARE_CRITICAL}} | Universal |

---

## Seção 4 — Sazonalidade

<!--
  UNIVERSAL: todo negócio tem períodos previsíveis de alta e baixa.
  Registre aqui para evitar falsos alertas.
-->

| Período | Impacto Esperado | Nota |
|---------|-----------------|------|
| {{SEASON_1}} | {{SEASON_1_IMPACT}} | {{SEASON_1_NOTE}} |
| {{SEASON_2}} | {{SEASON_2_IMPACT}} | {{SEASON_2_NOTE}} |

---

## Seção 5 — Anomalias Históricas Conhecidas

<!--
  UNIVERSAL: datas ou períodos que distorcem análises.
  Registre para que o sistema contextualize automaticamente.
-->

- **{{ANOMALY_DATE_1}}** — {{ANOMALY_DESCRIPTION_1}}. Não usar como comparação.
- **{{ANOMALY_DATE_2}}** — {{ANOMALY_DESCRIPTION_2}}. Excluir de médias.

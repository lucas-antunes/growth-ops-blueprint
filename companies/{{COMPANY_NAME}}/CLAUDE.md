# {{COMPANY_NAME}} — Contexto da Empresa

<!--
  ANOTAÇÃO: Este arquivo é carregado automaticamente pelo Claude sempre que
  você trabalha nesta pasta. É a memória permanente da empresa — não de uma
  sessão, mas do negócio inteiro. Tudo que o Claude precisa saber antes de
  analisar qualquer coisa fica aqui.
  
  COMO PREENCHER:
  Consulte guia-adaptacao.md para exemplos de ferramentas e fontes de dados
  por tipo de negócio antes de preencher os placeholders abaixo.
-->

## Visão Geral

**Empresa:** {{COMPANY_NAME}}
**Tipo de negócio:** `{{BUSINESS_TYPE}}`
*(e-commerce / saas / b2b-leads / infoproduto)*

{{COMPANY_DESCRIPTION}}

**Mercados:** {{OPERATING_MARKETS}}

---

## Fontes de Dados

<!--
  ANOTAÇÃO: Liste cada fonte com a ferramenta exata que o Claude usa para
  acessá-la. Sem isso, o Claude vai tentar acessar dados que não existem
  ou usar a ferramenta errada.
  
  Dica: você não precisa de todas as fontes abaixo.
  Mantenha apenas as que você realmente usa.
-->

### {{PRIMARY_DB_NAME}} — Fonte de Verdade

<!--
  UNIVERSAL: toda empresa tem uma "fonte de verdade".
  
  [E-COMMERCE]  → banco de pedidos (Shopify, Metabase, plataforma própria)
  [SAAS]        → sistema de billing (Stripe, Chargebee, banco de dados)
  [B2B/LEADS]   → CRM (HubSpot, Salesforce, Pipedrive)
  [INFOPRODUTO] → plataforma de vendas (Hotmart, Kiwify, Stripe)
-->

- **Ferramenta:** `{{DB_MCP_TOOL}}`
- **O que cobre:** `{{DB_COVERAGE}}`
- **Regras de filtro:**
  - `{{FILTER_RULE_1}}` — {{FILTER_REASON_1}}
  - `{{FILTER_RULE_2}}` — {{FILTER_REASON_2}}

### {{ADS_PLATFORM}} — Plataforma de Ads

<!--
  UNIVERSAL: qualquer plataforma de anúncios.
  Escrita requer aprovação — veja governance/ads-collaboration-protocol.md
-->

- **Ferramenta:** `{{ADS_MCP_TOOL}}` (leitura) / `{{ADS_WRITE_MCP_TOOL}}` (escrita — requer aprovação)
- **Contas:**
  | Segmento / Mercado | Account ID | Moeda |
  |--------------------|------------|-------|
  | {{SEGMENT_1}} | {{ACCOUNT_ID_1}} | {{CURRENCY_1}} |
  | {{SEGMENT_2}} | {{ACCOUNT_ID_2}} | {{CURRENCY_2}} |

### {{ANALYTICS_TOOL}} — Analytics & Funil

<!--
  UNIVERSAL: ferramenta de analytics.
  [E-COMMERCE / INFOPRODUTO] → GA4
  [SAAS]                     → Mixpanel, Amplitude, PostHog, GA4
  [B2B/LEADS]                → HubSpot Analytics, GA4, ou CRM reports
-->

- **Ferramenta:** `{{ANALYTICS_MCP_TOOL}}`
- **O que cobre:** `{{ANALYTICS_COVERAGE}}`

### {{CRM_OR_EMAIL_TOOL}} — CRM / E-mail / Comunicação

<!--
  Opcional — inclua se for relevante para o seu negócio.
  [E-COMMERCE]  → Klaviyo, Braze (email + push)
  [SAAS]        → Intercom, Customer.io
  [B2B/LEADS]   → HubSpot CRM, Salesforce
  [INFOPRODUTO] → ActiveCampaign, Mailchimp, plataforma do curso
-->

- **Ferramenta:** `{{CRM_MCP_TOOL}}`
- **O que cobre:** `{{CRM_COVERAGE}}`

---

## Metodologia de Eficiência em Ads

<!--
  UNIVERSAL: toda empresa precisa definir como mede o retorno de ads.
  Plataformas de ads têm atribuição própria que geralmente superestima.
  A sua fonte de verdade é o banco de dados, não o painel do Google.
  
  [E-COMMERCE / INFOPRODUTO] → ROAS (retorno sobre gasto)
  [SAAS]                     → CAC (custo de aquisição) vs LTV
  [B2B/LEADS]                → CPL (custo por lead) ou CPO (custo por oportunidade)
-->

| Métrica | Cálculo | Quando usar |
|---------|---------|-------------|
| **Eficiência da Plataforma** | Valor atribuído pela plataforma ÷ gasto | Comparação interna entre campanhas |
| **Eficiência Real** | `{{REAL_EFFICIENCY_FORMULA}}` | Decisões de orçamento e escala |

**Regra:** nunca use a eficiência da plataforma como único critério para decisões de escala.

---

## Guardrails

<!--
  UNIVERSAL: regras que não mudam. Coisas que já queimaram você no passado
  e que o sistema nunca deve repetir.
  
  Exemplos por tipo:
  [E-COMMERCE]  → "Nunca comparar semanas com promoções ativas vs semanas normais"
  [SAAS]        → "India não tem trial — nunca alarmar trial activation rate = 0% para India"
  [B2B/LEADS]   → "Pipeline do Q4 é inflado por renovações — ajustar baseline"
  [INFOPRODUTO] → "Semana de lançamento distorce todas as métricas — excluir de médias"
-->

- **{{GUARDRAIL_1}}** — {{GUARDRAIL_REASON_1}}
- **{{GUARDRAIL_2}}** — {{GUARDRAIL_REASON_2}}
- **Nunca misture dados de empresas diferentes** — cada empresa tem suas próprias fontes

---

## Protocolos

- Queries detalhadas: `protocol/analysis-protocol.md`
- Baselines: `../../memory/baselines.md`
- Decisions log: `../../memory/decisions-log.md`
- Governança de ads: `../../governance/ads-collaboration-protocol.md`

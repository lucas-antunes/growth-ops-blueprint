# Guia de Adaptação por Tipo de Negócio

<!--
  Este guia mapeia cada tipo de negócio para as configurações corretas
  do sistema. Leia antes de preencher os placeholders.
  
  O sistema tem duas camadas:
  
  UNIVERSAL (não muda entre negócios):
  - Estrutura de skills (trigger → contexto → pulls → análise → memória)
  - Protocolo de governança (3 tiers, regra dos 7 dias, circuit breaker)
  - Decisions-log (status de recomendações)
  - Ritmo operacional (check diário + análise semanal)
  - Seção de Ads (spend, ROAS/eficiência, CPA)
  - Seção de Decisões
  
  ESPECÍFICA POR TIPO (adaptar):
  - Seção 1: "fonte de verdade" do negócio
  - Seção 3: funil de aquisição
  - Seção 4: produto/serviço/conteúdo
  - Baselines: métricas de referência
  - CLAUDE.md da empresa: fontes de dados
-->

---

## Perfil 1 — E-commerce

**Exemplos:** loja Shopify, marketplace, D2C, multi-mercado

### Fonte de Verdade
Banco de dados de pedidos (Shopify, plataforma própria, Metabase, etc.)

### Seção 1 — Receita & Pedidos
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S1_TITLE}}` | Receita & Pedidos |
| `{{S1_PRIMARY_METRIC}}` | Pedidos |
| `{{S1_SECONDARY_METRIC}}` | Receita |
| `{{S1_UNIT_METRIC}}` | AOV (ticket médio) |
| `{{S1_HEALTH_METRIC}}` | Taxa de reembolso |
| `{{S1_BREAKDOWN}}` | Por mercado / canal |

### Seção 3 — Tráfego & Funil
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S3_TITLE}}` | Tráfego & Funil de Compra |
| `{{S3_VOLUME_METRIC}}` | Sessões |
| `{{S3_CONVERSION_METRIC}}` | Taxa de conversão (compras ÷ sessões) |
| `{{S3_MID_FUNNEL}}` | Taxa de carrinho (add-to-cart ÷ visualizações) |
| `{{S3_CHANNEL_MIX}}` | CPC, orgânico, direto, push, email |

### Seção 4 — Produto
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S4_TITLE}}` | Performance de Produto |
| `{{S4_PRIMARY}}` | SKUs únicos vendidos |
| `{{S4_SECONDARY}}` | Concentração de receita (top 10) |
| `{{S4_HEALTH}}` | Produtos com ruptura de estoque |

### Ferramentas Comuns
- `{{DB_MCP_TOOL}}` → Shopify MCP, Metabase skill, ou query direta ao banco
- `{{ANALYTICS_MCP_TOOL}}` → GA4 MCP
- `{{ADS_MCP_TOOL}}` → Google Ads MCP, Meta Ads MCP

---

## Perfil 2 — SaaS / Assinatura

**Exemplos:** produto SaaS B2B, app de assinatura, clube de membros

### Fonte de Verdade
Banco de receita recorrente (Stripe, Chargebee, banco de dados próprio)

### Seção 1 — Receita Recorrente
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S1_TITLE}}` | Receita Recorrente (MRR) |
| `{{S1_PRIMARY_METRIC}}` | MRR total |
| `{{S1_SECONDARY_METRIC}}` | Novos MRR + Expansão MRR − Churn MRR |
| `{{S1_UNIT_METRIC}}` | ARPU (receita média por usuário) |
| `{{S1_HEALTH_METRIC}}` | Churn rate (% de MRR perdido) |
| `{{S1_BREAKDOWN}}` | Por plano / segmento de cliente |

**Red flags específicos:**
- MRR churn > `{{CHURN_THRESHOLD}}` → problema de retenção
- Net Revenue Retention < 100% → base encolhendo mesmo com novos clientes
- Trial-to-paid conversion abaixo de `{{TRIAL_CVR_THRESHOLD}}`

### Seção 3 — Funil de Aquisição
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S3_TITLE}}` | Funil de Aquisição & Ativação |
| `{{S3_VOLUME_METRIC}}` | Trials iniciados / Cadastros |
| `{{S3_CONVERSION_METRIC}}` | Taxa de conversão trial → paid |
| `{{S3_MID_FUNNEL}}` | Taxa de ativação (completou onboarding / atingiu ação-chave) |
| `{{S3_CHANNEL_MIX}}` | Paid, orgânico, product-led, indicação |

### Seção 4 — Saúde do Produto
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S4_TITLE}}` | Engajamento & Saúde do Produto |
| `{{S4_PRIMARY}}` | DAU/MAU (ratio de engajamento diário) |
| `{{S4_SECONDARY}}` | Adoção de features principais |
| `{{S4_HEALTH}}` | NPS ou CSAT recente |

### Ferramentas Comuns
- `{{DB_MCP_TOOL}}` → Stripe MCP, query ao banco de dados
- `{{ANALYTICS_MCP_TOOL}}` → Mixpanel, Amplitude, GA4
- `{{ADS_MCP_TOOL}}` → Google Ads MCP, LinkedIn Ads MCP

---

## Perfil 3 — Lead Generation / B2B

**Exemplos:** agência, consultoria, empresa B2B com ciclo de vendas, serviço profissional

### Fonte de Verdade
CRM (HubSpot, Salesforce, Pipedrive, etc.)

### Seção 1 — Pipeline & Revenue
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S1_TITLE}}` | Pipeline & Revenue |
| `{{S1_PRIMARY_METRIC}}` | Novos leads qualificados (MQL/SQL) |
| `{{S1_SECONDARY_METRIC}}` | Deals fechados / Receita gerada |
| `{{S1_UNIT_METRIC}}` | Ticket médio dos deals |
| `{{S1_HEALTH_METRIC}}` | Taxa de fechamento (deals won ÷ deals em negociação) |
| `{{S1_BREAKDOWN}}` | Por canal de origem / SDR / segmento |

**Red flags específicos:**
- Pipeline abaixo de `{{MIN_PIPELINE_MULTIPLE}}x` da meta mensal → risco de não bater
- Ciclo de vendas aumentando → problema de qualificação ou proposta
- Taxa de fechamento abaixo de `{{MIN_CLOSE_RATE}}` → problema de sales ou oferta

### Seção 3 — Funil de Aquisição
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S3_TITLE}}` | Funil de Captação de Leads |
| `{{S3_VOLUME_METRIC}}` | Visitantes / Impressões |
| `{{S3_CONVERSION_METRIC}}` | Taxa de conversão para lead (form fill, call booking) |
| `{{S3_MID_FUNNEL}}` | Taxa lead → MQL → SQL |
| `{{S3_CHANNEL_MIX}}` | Paid, orgânico, indicação, outbound, eventos |

### Seção 4 — Qualidade do Pipeline
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S4_TITLE}}` | Qualidade & Saúde do Pipeline |
| `{{S4_PRIMARY}}` | Deals por etapa do funil |
| `{{S4_SECONDARY}}` | Deals com follow-up atrasado (>X dias sem atividade) |
| `{{S4_HEALTH}}` | Deals perto do prazo de fechamento |

### Ferramentas Comuns
- `{{DB_MCP_TOOL}}` → HubSpot MCP, Salesforce MCP, Pipedrive MCP
- `{{ANALYTICS_MCP_TOOL}}` → GA4, HubSpot analytics
- `{{ADS_MCP_TOOL}}` → Google Ads MCP, LinkedIn Ads MCP, Meta Ads MCP

---

## Perfil 4 — Infoproduto / Educação

**Exemplos:** curso online, mentoria, comunidade paga, produto digital

### Fonte de Verdade
Plataforma de vendas (Hotmart, Eduzz, Kiwify, Stripe, etc.)

### Seção 1 — Matrículas & Receita
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S1_TITLE}}` | Matrículas & Receita |
| `{{S1_PRIMARY_METRIC}}` | Novas matrículas |
| `{{S1_SECONDARY_METRIC}}` | Receita bruta / líquida |
| `{{S1_UNIT_METRIC}}` | Ticket médio |
| `{{S1_HEALTH_METRIC}}` | Taxa de reembolso / chargeback |
| `{{S1_BREAKDOWN}}` | Por oferta / plataforma de tráfego |

**Red flags específicos:**
- Taxa de reembolso acima de `{{REFUND_THRESHOLD}}` → problema de entrega ou expectativa
- Custo por matrícula (CPV) acima de `{{MAX_CPV}}` → margem comprometida
- Queda de ROI em lançamento → saturação de audiência

### Seção 3 — Funil de Captação
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S3_TITLE}}` | Funil de Captação |
| `{{S3_VOLUME_METRIC}}` | Leads captados |
| `{{S3_CONVERSION_METRIC}}` | Taxa de conversão lead → aluno |
| `{{S3_MID_FUNNEL}}` | Taxa de show-up (webinar/evento) ou taxa de abertura (email) |
| `{{S3_CHANNEL_MIX}}` | Paid (Meta, Google, YouTube), orgânico, lista própria |

### Seção 4 — Engajamento do Aluno
| Placeholder | Preencher com |
|-------------|--------------|
| `{{S4_TITLE}}` | Engajamento & Conclusão |
| `{{S4_PRIMARY}}` | Taxa de conclusão do curso |
| `{{S4_SECONDARY}}` | NPS / avaliação média |
| `{{S4_HEALTH}}` | Alunos inativos há >X dias (risco de reembolso) |

### Ferramentas Comuns
- `{{DB_MCP_TOOL}}` → Hotmart API, query direta, planilha de vendas
- `{{ANALYTICS_MCP_TOOL}}` → GA4, Meta Pixel events
- `{{ADS_MCP_TOOL}}` → Meta Ads MCP, Google Ads MCP, YouTube Ads

---

## O que é Universal (não adaptar)

| Componente | Por que é universal |
|-----------|-------------------|
| Protocolo de governança (3 tiers, 7 dias) | Qualquer plataforma de ads tem risco de conflito entre sessões |
| Decisions-log | Toda recomendação precisa de rastreamento, independente do negócio |
| Baselines | Todo negócio tem "normal" — só as métricas mudam |
| Estrutura da skill (6 passos) | O loop trigger → contexto → pull → análise → decisão → memória é universal |
| Modo diário + semanal | Todo negócio tem ritmo diário e semanal |
| Seção de Ads (Seção 2) | Spend, ROAS/eficiência, CPA — funcionam para qualquer plataforma |
| Seção de Decisões (Seção 5) | Universal |
| Agent protocol | Universal |

---

## Checklist de Configuração Inicial

Antes de usar o sistema pela primeira vez:

- [ ] Definir `{{BUSINESS_TYPE}}` — qual dos 4 perfis acima?
- [ ] Preencher `CLAUDE.md` da empresa com fontes de dados reais
- [ ] Mapear cada fonte para o MCP correspondente
- [ ] Configurar MCPs no `~/.claude.json` ou `~/.mcp.json`
- [ ] Preencher `memory/baselines.md` com dados históricos reais (mín. 90 dias)
- [ ] Adaptar Seção 1, 3 e 4 do `weekly-protocol` usando este guia
- [ ] Fazer o primeiro check manual para validar que os dados chegam
- [ ] Configurar `memory/decisions-log.md` com o contexto histórico do negócio

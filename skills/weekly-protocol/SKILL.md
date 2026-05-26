---
name: weekly-protocol
description: >
  Use quando o usuário pedir uma análise completa de performance, check semanal,
  check diário, ou "como está {{COMPANY_NAME}}". Cobre todas as seções do negócio
  em modo diário ou semanal.
---

# Protocolo de Performance Semanal

<!--
  ANOTAÇÃO PEDAGÓGICA — O QUE É ESTA SKILL:
  
  Esta é a skill central do sistema. Ela responde "como está o negócio hoje?"
  de forma padronizada, toda vez que você pede.
  
  Por que padronizar?
  - A mesma pergunta sempre retorna o mesmo formato
  - Você lê o output em 5 minutos porque sabe onde cada número está
  - Anomalias ficam visíveis porque o formato não muda — o número muda
  - O decisions-log acumula histórico porque a skill escreve nele toda vez

  COMO ADAPTAR PARA O SEU TIPO DE NEGÓCIO:
  As Seções 1, 3 e 4 mudam conforme o negócio.
  A Seção 2 (Ads) e a Seção 5 (Decisões) são universais.
  
  Consulte o guia-adaptacao.md para configurar cada seção
  de acordo com o seu perfil:
  - [E-COMMERCE]   → pedidos, AOV, funil de compra, produto
  - [SAAS]         → MRR, churn, trial activation, engajamento
  - [B2B/LEADS]    → pipeline, MQLs, taxa de fechamento, qualidade
  - [INFOPRODUTO]  → matrículas, CPV, show-up, conclusão
  
  Regra: toda seção precisa responder uma pergunta de negócio específica.
  Se você não sabe por que uma seção existe, tire-a.
-->

## Visão Geral

Análise completa de performance para {{COMPANY_NAME}} cobrindo todos os canais.
Roda em modo **diário** (ontem vs anteontem) ou **semanal** (esta semana vs semana passada).

**Tipo de negócio configurado:** `{{BUSINESS_TYPE}}`
*(e-commerce / saas / b2b-leads / infoproduto — define as Seções 1, 3 e 4)*

## Modos de Comparação

| Modo | Comparação Principal | Baseline |
|------|---------------------|----------|
| **Diário** | Ontem vs anteontem | Ontem vs mesmo dia semana passada |
| **Semanal** | Esta semana vs semana passada | Esta semana vs média 3 meses |

Ambos os modos rodam **todas as seções**. A diferença é só a janela de comparação.

---

## Documentos de Referência

Leia antes de executar:
- `companies/{{COMPANY_NAME}}/CLAUDE.md` — contexto e guardrails da empresa
- `companies/{{COMPANY_NAME}}/protocol/analysis-protocol.md` — queries detalhadas
- `memory/baselines.md` — métricas de referência
- `memory/decisions-log.md` — recomendações pendentes

---

## Seções

<!--
  Seções 1, 3 e 4 variam por tipo de negócio.
  Seções 2 e 5 são universais.
-->

1. **{{S1_TITLE}}** — fonte de verdade do negócio *(adaptar por tipo)*
2. **Performance de Ads** — spend, eficiência, CPA por conta *(universal)*
3. **{{S3_TITLE}}** — funil de aquisição *(adaptar por tipo)*
4. **{{S4_TITLE}}** — produto, serviço ou conteúdo *(adaptar por tipo)*
5. **Decisões & Ações** — leitura e atualização do decisions-log *(universal)*

---

## Workflow

### Passo 1 — Determinar Modo

- "daily", "check diário", "como está" → modo diário
- "weekly", "semanal", "semana" → modo semanal

### Passo 2 — Calcular Janelas de Data

**Modo Diário:**
- `ontem` = hoje - 1
- `anteontem` = hoje - 2
- `mesmo_dia_semana_passada` = hoje - 8
- Puxe 14 dias de contexto

**Modo Semanal:**
- `esta_semana` = segunda a domingo mais recente
- `semana_passada` = a segunda a domingo anterior
- `baseline_3_meses` = média das últimas 12 semanas
- Puxe 4 semanas de contexto

### Passo 3 — Executar Todas as Seções

Maximize queries em paralelo:
- **Seções 1, 3, 4:** queries na fonte de verdade + analytics em paralelo
- **Seção 2:** queries de ads (máx. `{{MAX_PARALLEL_ADS_QUERIES}}` contas em paralelo)
- **Seção 5:** leia e atualize `memory/decisions-log.md`

### Passo 4 — Comparar e Reportar

Para cada métrica:
- Valor atual
- Valor do período de comparação
- % de variação
- Flag se fora do range saudável (ver `memory/baselines.md`)

### Passo 5 — Atualizar Memória

Antes de encerrar:
1. Adicione novos achados ao `memory/decisions-log.md` com status `NOT ACTIONED`
2. Atualize status de itens existentes que mudaram
3. Feche itens `MONITORING` que se resolveram ou escalaram

---

## Seção 1 — {{S1_TITLE}}

<!--
  UNIVERSAL: Esta seção usa a sua "fonte de verdade" — não o painel de ads,
  não o analytics, mas o banco de dados do seu produto ou CRM.
  
  Por quê? Plataformas de ads e analytics têm discrepâncias de atribuição.
  Sua fonte de verdade não. Sempre comece aqui.
  
  ADAPTAR: O que é "verdade" muda por tipo de negócio.
  Veja guia-adaptacao.md para a configuração do seu perfil.
  
  [E-COMMERCE]  → banco de pedidos: orders, revenue, AOV, refund rate
  [SAAS]        → billing: MRR novo, expansão, churn, NRR
  [B2B/LEADS]   → CRM: MQLs, SQLs, deals won, pipeline value
  [INFOPRODUTO] → plataforma de vendas: matrículas, receita, CPV
-->

**Ferramenta:** `{{DB_MCP_TOOL}}`
**Fonte:** `{{PRIMARY_DATA_SOURCE}}` em `{{DB_NAME}}`

**Métricas principais:**
- `{{S1_PRIMARY_METRIC}}` vs período de comparação
- `{{S1_SECONDARY_METRIC}}` vs período de comparação
- `{{S1_UNIT_METRIC}}` — mudanças bruscas indicam outliers ou mudança de mix
- `{{S1_HEALTH_METRIC}}` — indicador de saúde operacional

**Breakdown por:** `{{S1_BREAKDOWN}}`

**Red flags:**
- `{{S1_PRIMARY_METRIC}}` cai >20% vs comparação → investigar imediatamente
- `{{S1_HEALTH_METRIC}}` acima de `{{S1_HEALTH_THRESHOLD}}` → problema operacional
- Qualquer segmento com zero `{{S1_PRIMARY_METRIC}}` → verificar plataforma

---

## Seção 2 — Performance de Ads

<!--
  UNIVERSAL: Esta seção é idêntica para todos os tipos de negócio.
  Spend, eficiência e CPA funcionam para Google Ads, Meta Ads,
  LinkedIn Ads, TikTok Ads — qualquer plataforma.
  
  A única diferença é a métrica de "eficiência":
  - E-commerce e Infoproduto → ROAS (retorno sobre gasto em ads)
  - SaaS e B2B → CAC (custo de aquisição de cliente) ou CPL (custo por lead)
  
  Defina em {{ADS_EFFICIENCY_METRIC}} qual métrica representa retorno para você.
-->

**Ferramenta:** `{{ADS_MCP_TOOL}}`
**Métrica de eficiência:** `{{ADS_EFFICIENCY_METRIC}}`
*(ROAS para e-commerce/infoproduto | CAC ou CPL para SaaS/B2B)*

**Contas ativas:**
`{{ADS_ACCOUNTS_TABLE}}`

**Métricas por conta:**
- Spend, Conversões, `{{ADS_EFFICIENCY_METRIC}}`, CPA
- Eficiência real = `{{REAL_EFFICIENCY_CALCULATION}}`
*(ex: ROAS real = receita do banco ÷ spend | CAC real = LTV primeiro mês ÷ deals won)*

**Red flags:**
- `{{ADS_EFFICIENCY_METRIC}}` abaixo de `{{MIN_EFFICIENCY_THRESHOLD}}` → flag para análise
- Eficiência caindo por 3+ períodos → problema estrutural
- Variação de spend >30% sem mudança de performance → checar changelog
- Zero conversões por `{{ZERO_CONV_DAYS_THRESHOLD}}` dias → alerta imediato

---

## Seção 3 — {{S3_TITLE}}

<!--
  ADAPTAR: O funil de aquisição muda completamente por tipo de negócio.
  A pergunta universal é: "onde os usuários estão caindo antes de converter?"
  
  [E-COMMERCE]  → sessões → carrinho → checkout → compra
  [SAAS]        → visitantes → cadastro → ativação → trial → paid
  [B2B/LEADS]   → visitantes → leads → MQL → SQL → proposta → fechamento
  [INFOPRODUTO] → alcance → leads → lista → show-up/clique → matrícula
  
  Se o volume de topo do funil caiu: problema de aquisição (ads, SEO).
  Se a conversão no meio caiu: problema de produto ou proposta.
  As duas perguntas têm respostas completamente diferentes.
-->

**Ferramenta:** `{{ANALYTICS_MCP_TOOL}}`

**Métricas de volume:** `{{S3_VOLUME_METRIC}}`
**Métrica de conversão principal:** `{{S3_CONVERSION_METRIC}}`
**Métrica de meio de funil:** `{{S3_MID_FUNNEL}}`
**Mix de canais:** `{{S3_CHANNEL_MIX}}`

**O que checar:**
- `{{S3_CONVERSION_METRIC}}` vs período de comparação
- `{{S3_MID_FUNNEL}}` vs período de comparação
- Canal com maior queda de volume
- Canais pagos vs orgânico — qual está crescendo?

**Red flags:**
- `{{S3_CONVERSION_METRIC}}` cai abaixo de `{{S3_MIN_CVR}}` → funil quebrado
- Spike de volume sem aumento de conversão → tráfego de baixa qualidade
- Canal pago perdendo share → checar campanhas
- Canal orgânico caindo → verificar SEO ou saúde do conteúdo

---

## Seção 4 — {{S4_TITLE}}

<!--
  ADAPTAR: Esta seção responde "o que está performando?" dentro do seu produto.
  
  [E-COMMERCE]  → quais produtos estão vendendo / com estoque OK
  [SAAS]        → quais features são usadas / quais usuários estão engajados
  [B2B/LEADS]   → qualidade do pipeline / deals travados / follow-ups atrasados
  [INFOPRODUTO] → taxa de conclusão / alunos em risco de reembolso
  
  É a diferença entre um problema de marketing (menos volume no topo)
  e um problema de produto/serviço (o que chega não converte ou não fica).
-->

**Ferramenta:** `{{DB_MCP_TOOL}}`
**Fonte:** `{{S4_DATA_SOURCE}}`

**O que checar:**
- `{{S4_PRIMARY}}` vs período de comparação
- `{{S4_SECONDARY}}` — concentração ou distribuição
- `{{S4_HEALTH}}` — indicador de saúde do produto/serviço

---

## Seção 5 — Decisões & Ações

<!--
  UNIVERSAL: Esta seção é idêntica para todos os tipos de negócio.
  
  O decisions-log é a memória do sistema. Sem ele:
  - A mesma recomendação aparece toda semana
  - Você não sabe se algo foi acionado ou não
  - Insights acumulados se perdem entre sessões
  
  Com ele:
  - Cada recomendação tem um status claro
  - Itens NOT ACTIONED por 2+ semanas ficam visíveis = escalação automática
  - O histórico mostra padrões que uma sessão única não vê
-->

**Processo:**
1. Leia `memory/decisions-log.md`
2. Cheque status de todos os itens `NOT ACTIONED` — ainda pendentes?
3. Cheque itens `MONITORING` — a situação mudou?
4. Adicione novos achados desta análise com status `NOT ACTIONED`
5. Atualize o arquivo com as mudanças de status

**O que checar:**
- Recomendações `NOT ACTIONED` por mais de 2 semanas → escalar
- Itens `MONITORING` que pioraram → promover para recomendação ativa
- Novas anomalias desta análise → adicionar ao log

---

## Formato de Output

```
## {{COMPANY_NAME}} — Performance [Data] (Diário/Semanal)

**Comparação:** [Ontem vs anteontem / Esta semana vs semana passada]

### 1. {{S1_TITLE}}
- {{S1_PRIMARY_METRIC}}: X (vs X: +/-% | vs mesmo período: +/-%)
- {{S1_SECONDARY_METRIC}}: X (vs X: +/-%)
- {{S1_UNIT_METRIC}}: X (vs X: +/-%)
- Por {{S1_BREAKDOWN}}: [breakdown]
- Flags: [alertas]

### 2. Performance de Ads
- Spend total: $X (vs $X: +/-%)
- {{ADS_EFFICIENCY_METRIC}} plataforma: X | Real: X
- Por conta: [conta] $X spend / X {{ADS_EFFICIENCY_METRIC}} (vs X)
- Flags: [alertas]

### 3. {{S3_TITLE}}
- {{S3_VOLUME_METRIC}}: X (vs X: +/-%)
- {{S3_CONVERSION_METRIC}}: X% (vs X%)
- {{S3_MID_FUNNEL}}: X% (vs X%)
- Mix de canais: [canal principal] X%, orgânico X%, direto X%

### 4. {{S4_TITLE}}
- {{S4_PRIMARY}}: X (vs X)
- {{S4_SECONDARY}}: [análise]
- {{S4_HEALTH}}: [status]

### 5. Decisions Log
- NOT ACTIONED: [lista com dias em aberto]
- MONITORING: [mudanças de status]
- Novos achados: [adicionados nesta sessão]

### Itens de Ação
1. [Ação específica — urgência: imediata / esta semana / monitorar]
```

---

## Erros Comuns

<!--
  Os dois primeiros são universais.
  Os dois últimos são mais comuns em e-commerce — adapte para o seu negócio.
-->

- Comparar semana parcial com semana completa sem notar a diferença de dias — sempre note o número de dias no output
- Esquecer de atualizar o decisions-log ao final — o próximo check não terá contexto
- Usar a eficiência da plataforma de ads como único critério de decisão — sempre cruze com a fonte de verdade do negócio
- Não filtrar tráfego bot / spam do analytics — pode inflar métricas de volume artificialmente

# MEMORY — {{COMPANY_NAME}}

<!--
  ANOTAÇÃO PEDAGÓGICA — POR QUE ESTE ARQUIVO EXISTE:

  Este é o arquivo de contexto raiz do sistema. Ele responde uma
  pergunta simples: "o que o Claude precisa saber antes de qualquer
  análise, sem que eu precise repetir toda vez?"

  Diferença entre este arquivo e os outros arquivos de memória:

  | Arquivo            | Conteúdo                                      |
  |--------------------|-----------------------------------------------|
  | MEMORY.md          | Contexto imutável ou quase-imutável do negócio |
  | decisions-log.md   | Status de recomendações ativas                |
  | ads-analysis.md    | Padrões e anomalias de ads                    |
  | experiments.md     | Experimentos em andamento                     |
  | baselines.md       | Benchmarks de performance por métrica         |

  MEMORY.md é o que não muda de semana para semana.
  Se você se pega repetindo algo para o Claude toda sessão,
  esse algo pertence aqui.

  COMO PREENCHER:
  Preencha as seções abaixo com o que é estruturalmente verdadeiro
  sobre o seu negócio. Revise trimestralmente.

  Última revisão: {{MEMORY_DATE}}
-->

---

## Identidade do Negócio

**Empresa:** {{COMPANY_NAME}}
**Tipo:** `{{BUSINESS_TYPE}}` *(e-commerce / saas / b2b-leads / infoproduto)*
**Mercados:** {{OPERATING_MARKETS}}
**Moeda principal de reporte:** {{REPORTING_CURRENCY}}

**O que fazemos:**
{{COMPANY_DESCRIPTION}}

**Nosso cliente principal:**
{{ICP_DESCRIPTION}}

---

## Fonte de Verdade

<!--
  Onde ficam os números reais — não o painel de ads, não o analytics.
  Quando houver discrepância entre plataformas, esta fonte ganha.
-->

**Fonte:** {{PRIMARY_DATA_SOURCE}}
**Ferramenta de acesso:** `{{DB_MCP_TOOL}}`
**O que cobre:** {{DB_COVERAGE}}

**Regra:** nunca use dados de plataformas de ads como fonte primária de revenue ou conversões. Confirme sempre na fonte de verdade antes de tomar decisões de orçamento.

---

## Guardrails Permanentes

<!--
  Regras que não mudam. Coisas que já queimaram o negócio no passado
  e que o sistema nunca deve repetir.
  Seja específico: não "cuidado com sazonalidade", mas
  "não compare outubro com setembro — outubro tem feriado nacional em AE".
-->

- **{{GUARDRAIL_1}}** — {{GUARDRAIL_REASON_1}}
- **{{GUARDRAIL_2}}** — {{GUARDRAIL_REASON_2}}
- **Nunca misture dados de empresas diferentes** — cada empresa tem contexto e filtros próprios

---

## Anomalias Históricas Conhecidas

<!--
  Períodos que distorcem análises. O Claude consulta esta seção
  antes de declarar qualquer variação como anomalia.
-->

| Período | Impacto | Não usar para |
|---------|---------|---------------|
| {{ANOMALY_DATE_1}} | {{ANOMALY_IMPACT_1}} | Comparações de baseline |
| {{ANOMALY_DATE_2}} | {{ANOMALY_IMPACT_2}} | Médias mensais |

---

## Contexto de Canais

<!--
  O que é estruturalmente verdadeiro sobre cada canal.
  Não é performance — é arquitetura e comportamento esperado.
-->

| Canal | Papel | Particularidade |
|-------|-------|-----------------|
| {{CHANNEL_1}} | {{CHANNEL_1_ROLE}} | {{CHANNEL_1_NOTE}} |
| {{CHANNEL_2}} | {{CHANNEL_2_ROLE}} | {{CHANNEL_2_NOTE}} |

---

## Decisões Estratégicas em Vigor

<!--
  Decisões tomadas pela liderança que moldam como o sistema opera.
  Não são recomendações do Claude — são diretrizes do negócio.
  Exemplo: "Índia não recebe mais de $X/dia até Q3" ou
  "SEO é prioridade zero este trimestre — foco total em paid".
-->

- **{{STRATEGIC_DECISION_1}}** — vigente desde {{DECISION_DATE_1}}
- **{{STRATEGIC_DECISION_2}}** — vigente desde {{DECISION_DATE_2}}

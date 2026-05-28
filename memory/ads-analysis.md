# Ads Analysis — Contexto Acumulado — {{COMPANY_NAME}}

<!--
  ANOTAÇÃO PEDAGÓGICA — POR QUE ESTE ARQUIVO EXISTE:

  Os relatórios de ads que o sistema gera toda semana são descartáveis.
  O que NÃO é descartável é o contexto estratégico acumulado:
  - Uma anomalia que aparece toda vez que o mercado X tem alta de CPC
  - Um padrão de sazonalidade que distorce o ROAS em outubro
  - Uma hipótese que ficou aberta por 3 semanas sem validação

  Este arquivo não é o relatório. É o que sobrevive do relatório.
  O Claude lê este arquivo antes de toda análise de ads para não
  perder contexto entre sessões.

  COMO USAR:
  - Ao final de cada análise de ads, adicione aqui os insights que
    merecem ser lembrados na próxima sessão.
  - Remova entradas quando se tornarem irrelevantes ou resolvidas.
  - Seção 1 (anomalias ativas) é revisada toda semana.
  - Seção 2 (padrões confirmados) é revisada mensalmente.
  - Seção 3 (hipóteses abertas) é revisada a cada 2 semanas.

  Última atualização: {{ADS_ANALYSIS_DATE}}
-->

---

## Seção 1 — Anomalias Ativas

<!--
  Anomalias que estão acontecendo agora e precisam de monitoramento.
  Formato: data de início, descrição, status atual, próximo check.

  Quando resolver: mover para Seção 2 (se virou padrão) ou deletar.
-->

| Data início | Conta/Mercado | Anomalia | Status | Próximo check |
|-------------|--------------|----------|--------|---------------|
| {{ANOMALY_DATE_1}} | {{ANOMALY_ACCOUNT_1}} | {{ANOMALY_DESC_1}} | Monitorando | {{ANOMALY_CHECK_1}} |

---

## Seção 2 — Padrões Confirmados

<!--
  Comportamentos que se repetem e já estão validados.
  Não são alertas — são contexto para não alarmar erroneamente.

  Exemplos:
  - "India tem ROAS baixo toda a última semana do mês por causa de ciclo de pagamento"
  - "UAE Shopping colapsa 20-30% toda vez que há um feriado nacional"
  - "Meta satura audiência em ~3 semanas no mercado de lançamento"
-->

- **{{PATTERN_1}}** — {{PATTERN_1_CONTEXT}}
- **{{PATTERN_2}}** — {{PATTERN_2_CONTEXT}}

---

## Seção 3 — Hipóteses Abertas

<!--
  Hipóteses levantadas em análises anteriores que ainda não foram
  testadas ou validadas. Cada uma tem uma decisão pendente.
-->

| Hipótese | Levantada em | Decisão pendente |
|----------|-------------|-----------------|
| {{HYPOTHESIS_1}} | {{HYPOTHESIS_DATE_1}} | {{HYPOTHESIS_ACTION_1}} |

---

## Seção 4 — Contexto de Eficiência por Conta

<!--
  Baseline de eficiência por conta/mercado — atualizar quando os
  números mudarem estruturalmente (não por flutuações normais).
  Usado para contextualizar variações sem disparar falsos alertas.
-->

| Conta / Mercado | ROAS baseline | CPA baseline | Notas |
|----------------|--------------|-------------|-------|
| {{ACCOUNT_1}} | {{ACCOUNT_1_ROAS}} | {{ACCOUNT_1_CPA}} | {{ACCOUNT_1_NOTE}} |
| {{ACCOUNT_2}} | {{ACCOUNT_2_ROAS}} | {{ACCOUNT_2_CPA}} | {{ACCOUNT_2_NOTE}} |

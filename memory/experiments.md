# Experiments & Scaling — {{COMPANY_NAME}}

<!--
  ANOTAÇÃO PEDAGÓGICA — POR QUE ESTE ARQUIVO EXISTE:

  Toda decisão de escalar ou pausar um canal de marketing é um experimento
  implícito. O problema é que, sem estrutura, experimentos implícitos
  nunca têm resultados claros:
  - "Subimos o budget da Índia no mês passado... melhorou?"
  - "Aquela campanha que pausamos, era mesmo ruim ou era sazonalidade?"
  - "Testamos criativos novos mas não sabemos se foi isso que moveu o ROAS."

  Este arquivo transforma experimentos implícitos em experimentos explícitos:
  hipótese clara, controle definido, critério de sucesso, janela de tempo,
  e guardrails de rollback.

  O Claude monitora experimentos ativos neste arquivo toda semana e
  avisa quando uma decisão precisa ser tomada.

  TIPOS DE EXPERIMENTO:
  - Budget scaling: aumentar/diminuir gasto em conta ou campanha
  - Bidding strategy: mudar de manual para automático ou vice-versa
  - Creative rotation: testar novo criativo vs incumbente
  - Audience expansion: ampliar segmentação
  - Channel test: adicionar/remover plataforma

  GUARDRAIL UNIVERSAL:
  Nenhum experimento fica aberto por mais de {{MAX_EXPERIMENT_DAYS}} dias
  sem uma decisão. Se a janela expirou sem resultado: ENCERRAR e registrar.
-->

---

## Experimentos Ativos

<!--
  Preencha um bloco por experimento ativo.
  Quando concluir: mover para "Experimentos Encerrados" abaixo.
-->

### Experimento: {{EXPERIMENT_NAME_1}}

**Tipo:** `{{EXPERIMENT_TYPE_1}}`
*(budget scaling / bidding strategy / creative / audience / channel)*

**Hipótese:** {{EXPERIMENT_HYPOTHESIS_1}}
*(ex: "Aumentar o budget da Austrália em 30% vai manter ROAS acima de 3.0x")*

**Setup:**
- Conta / campanha: `{{EXPERIMENT_ACCOUNT_1}}`
- Variante: `{{EXPERIMENT_VARIANT_1}}` *(o que mudou)*
- Controle: `{{EXPERIMENT_CONTROL_1}}` *(o que ficou igual)*
- Data de início: `{{EXPERIMENT_START_1}}`
- Janela: `{{EXPERIMENT_WINDOW_1}}` dias
- Data de decisão: `{{EXPERIMENT_DECISION_DATE_1}}`

**Critério de sucesso:**
- ✅ ESCALAR se: `{{EXPERIMENT_SUCCESS_1}}`
- ❌ ROLLBACK se: `{{EXPERIMENT_ROLLBACK_1}}`

**Guardrails:**
- Rollback automático se `{{ADS_EFFICIENCY_METRIC}}` cair abaixo de `{{EXPERIMENT_GUARDRAIL_1}}` por 2 dias consecutivos
- Não modificar outras variáveis durante a janela do experimento
- Registrar toda mudança no changelog de ads

**Status atual:** `{{EXPERIMENT_STATUS_1}}`
*(ACTIVE / PAUSED / WAITING_DECISION)*

**Últimas leituras:**
- `{{EXPERIMENT_DATE_READ_1}}`: `{{EXPERIMENT_READING_1}}`

---

## Experimentos Encerrados

<!--
  Registro histórico. Não apagar — é memória de decisões tomadas.
  Útil para não repetir experimentos já testados.
-->

| Nome | Tipo | Hipótese | Resultado | Decisão | Data |
|------|------|----------|-----------|---------|------|
| {{CLOSED_EXP_1}} | {{CLOSED_TYPE_1}} | {{CLOSED_HYPOTHESIS_1}} | {{CLOSED_RESULT_1}} | {{CLOSED_DECISION_1}} | {{CLOSED_DATE_1}} |

---

## Fila de Experimentos

<!--
  Experimentos identificados mas ainda não iniciados.
  Priorize por impacto esperado vs risco.
-->

| Experimento | Tipo | Impacto esperado | Risco | Prioridade |
|-------------|------|-----------------|-------|------------|
| {{QUEUED_EXP_1}} | {{QUEUED_TYPE_1}} | {{QUEUED_IMPACT_1}} | {{QUEUED_RISK_1}} | Alta / Média / Baixa |

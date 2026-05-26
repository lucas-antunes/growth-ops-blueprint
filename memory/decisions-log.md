# Decisions & Recommendations Log — {{COMPANY_NAME}}

<!--
  ANOTAÇÃO PEDAGÓGICA — POR QUE ESTE ARQUIVO EXISTE:
  
  Este é o arquivo mais importante do sistema de memória.
  
  Sem ele: cada sessão do Claude começa do zero. Você recebe a mesma
  recomendação toda semana e não sabe se alguém a acionou.
  
  Com ele: o sistema acumula inteligência. Uma recomendação feita em
  fevereiro ainda está visível em abril, com seu status atual. O Claude
  sabe o que já foi tentado, o que funcionou, o que está monitorando.
  
  Como usar:
  1. O Claude adiciona recomendações aqui ao final de cada análise
  2. Você atualiza o status quando acionar (ou decidir não acionar)
  3. O Claude lê este arquivo no início de cada sessão
  4. Itens NOT ACTIONED por 2+ semanas aparecem como alerta na próxima análise
  
  Regra: nenhuma recomendação some sem status registrado.
-->

## Legenda de Status

- **NOT ACTIONED** — recomendação feita, ainda não implementada
- **ACTIONED** — implementada. Registrar data e resultado esperado.
- **MONITORING** — aguardando dados suficientes para avaliar impacto
- **REJECTED** — revisada e explicitamente rejeitada. Registrar motivo.

---

## Recomendações Ativas

<!--
  FORMATO DE ENTRADA:
  
  ### [YYYY-MM-DD] Título da Recomendação
  
  **Contexto:** O que os dados mostraram que gerou esta recomendação.
  **Ação recomendada:** O que fazer especificamente.
  **Impacto esperado:** O que deve mudar se a ação for tomada.
  **Status:** NOT ACTIONED / ACTIONED / MONITORING / REJECTED
  **Atualização:** [data] — O que mudou desde a recomendação original.
-->

### [YYYY-MM-DD] Exemplo — Campanha {{MARKET_1}} com ROAS abaixo do threshold

**Contexto:** ROAS de {{MARKET_1}} caiu para X.XXx por 3 semanas consecutivas. Spend de $X/semana gerando prejuízo líquido ao custo de aquisição atual.
**Ação recomendada:** Reduzir orçamento diário de $X para $X ou pausar e revisar estrutura de campanha.
**Impacto esperado:** Redução de $X/semana em gasto ineficiente. ROAS deve normalizar para X.XXx+ em 2 semanas.
**Status:** NOT ACTIONED
**Atualização:** — 

---

## Investigações

<!--
  FORMATO:
  
  ### [YYYY-MM-DD] Título da Investigação
  
  **Achado:** O que foi encontrado.
  **Impacto:** O que isso afeta.
  **Próxima ação:** O que precisa ser feito.
  **Status:** BLOCKED / IN PROGRESS / RESOLVED
-->

---

## Contexto Histórico & Guardrails

<!--
  ANOTAÇÃO: Esta seção registra fatos permanentes sobre o negócio que
  o Claude deve carregar em toda análise — anomalias históricas, datas
  de referência, decisões que não mudam.
  
  Exemplo: "Janeiro tem sazonalidade atípica — não comparar com dezembro".
  Sem isso registrado aqui, o sistema vai alarmar toda vez que ver a queda
  de janeiro, mesmo que seja completamente esperada.
-->

- **{{HISTORICAL_CONTEXT_1}}** — {{CONTEXT_REASON_1}}
- **{{HISTORICAL_CONTEXT_2}}** — {{CONTEXT_REASON_2}}

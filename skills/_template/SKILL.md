---
name: {{SKILL_NAME}}
description: >
  Use quando o usuário pedir {{TRIGGER_DESCRIPTION}}.
  Exemplos: "{{EXAMPLE_TRIGGER_1}}", "{{EXAMPLE_TRIGGER_2}}".
---

# {{SKILL_TITLE}}

<!--
  ANOTAÇÃO PEDAGÓGICA:
  Este é o template base para qualquer skill do seu sistema.
  A estrutura é sempre a mesma: trigger → contexto → pulls de dados →
  análise → decisão → escrita na memória.
  
  O que diferencia uma skill de um prompt avançado:
  1. Tem frontmatter com trigger definido (o Claude sabe quando usar)
  2. Tem fontes de dados explícitas (não depende de memória do Claude)
  3. Tem formato de output definido (o output é previsível)
  4. Escreve na memória ao final (a próxima sessão tem contexto)
-->

## Visão Geral

<!-- Uma frase descrevendo o que essa skill faz e quando usar. -->
{{SKILL_DESCRIPTION}}

## Quando Usar

- {{TRIGGER_CONDITION_1}}
- {{TRIGGER_CONDITION_2}}

---

## Passo 1 — Carregar Contexto

<!--
  ANOTAÇÃO: Antes de qualquer análise, o Claude precisa ler o estado atual.
  Isso garante que a sessão de segunda-feira sabe o que aconteceu na sexta.
-->

Antes de executar, leia:
- `companies/{{COMPANY_NAME}}/CLAUDE.md` — contexto da empresa
- `memory/decisions-log.md` — recomendações pendentes e histórico recente
- `memory/baselines.md` — métricas de referência para comparação

---

## Passo 2 — Calcular Janelas de Tempo

<!--
  ANOTAÇÃO: Defina as janelas antes de puxar dados. Nunca deixe o Claude
  inferir o período — datas explícitas eliminam erros silenciosos.
-->

| Modo | Comparação Principal | Baseline |
|------|---------------------|----------|
| **Diário** | Ontem vs anteontem | Ontem vs mesmo dia semana passada |
| **Semanal** | Esta semana vs semana passada | Esta semana vs média 3 meses |

Calcule: `ontem`, `anteontem`, `mesmo_dia_semana_passada`, `inicio_semana`, `fim_semana`.

---

## Passo 3 — Pulls de Dados (paralelo quando possível)

<!--
  ANOTAÇÃO: Liste cada fonte de dados explicitamente.
  Queries paralelas economizam tempo — marque quais podem rodar juntas.
-->

Execute em paralelo:

### Fonte A — {{DATA_SOURCE_A}}
**Ferramenta:** `{{MCP_TOOL_A}}`
**O que responde:** {{WHAT_IT_ANSWERS_A}}
```
{{QUERY_TEMPLATE_A}}
```

### Fonte B — {{DATA_SOURCE_B}}
**Ferramenta:** `{{MCP_TOOL_B}}`
**O que responde:** {{WHAT_IT_ANSWERS_B}}
```
{{QUERY_TEMPLATE_B}}
```

---

## Passo 4 — Análise

<!--
  ANOTAÇÃO: Defina o que comparar e o que sinalizar como alerta.
  Sem isso, o Claude compara qualquer coisa contra qualquer coisa.
-->

Para cada métrica, mostre:
- Valor atual
- Valor do período de comparação
- % de variação
- Flag se fora do range saudável (ver `memory/baselines.md`)

**Red flags automáticos:**
- {{RED_FLAG_1}} → sinalizar como ALERTA
- {{RED_FLAG_2}} → sinalizar como CRÍTICO

---

## Passo 5 — Formato do Output

<!--
  ANOTAÇÃO: Output previsível é o que transforma análise em operação.
  O mesmo formato toda semana significa que você lê em 3 minutos, não 30.
-->

```
## {{COMPANY_NAME}} — Performance [Data] (Diário/Semanal)

### 1. {{SECTION_1_NAME}}
- {{METRIC_1}}: X (vs X: +/-%)
- {{METRIC_2}}: X (vs X: +/-%)
- Flags: [qualquer alerta]

### 2. {{SECTION_2_NAME}}
...

### Itens de Ação
1. [Ação específica — urgência: imediata / esta semana / monitorar]
```

---

## Passo 6 — Escrever na Memória

<!--
  ANOTAÇÃO: Este é o passo que a maioria pula — e é o mais importante.
  Sem ele, cada sessão começa do zero. Com ele, você constrói inteligência acumulada.
-->

Após a análise:
1. Abra `memory/decisions-log.md`
2. Adicione qualquer nova recomendação com status `NOT ACTIONED`
3. Atualize o status de recomendações existentes que mudaram
4. Atualize items `MONITORING` com os novos dados

---

## Erros Comuns

<!--
  ANOTAÇÃO: Documente os erros que já aconteceram. Isso previne regressões
  quando o sistema evolui ou quando outra pessoa usar.
-->

- {{COMMON_MISTAKE_1}}
- {{COMMON_MISTAKE_2}}

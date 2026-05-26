# Estratégia White-Label — Sistema Growth Ops

## Princípio central

O white-label não é uma versão "simplificada". É a **mesma arquitetura**, com três trocas:
1. Nomes de empresa/produto → placeholders genéricos
2. Dados específicos (account IDs, queries SQL calibradas) → templates com instruções
3. Referências à equipe Desertcart → referências ao "operador" ou "time"

O aluno deve conseguir pegar o white-label, trocar os placeholders, e ter um sistema funcional.

---

## O que entra no white-label do curso

### Camada 0 — Infraestrutura (material adicional, não as aulas)
O repositório de MCPs fica público no GitHub como material complementar.
O aluno aprende o CONCEITO nas aulas; quem quiser implementar vai ao repo.

**Conteúdo do repo público:**
- `google-ads-mcp-write` — com README de instalação
- `clevertap-mcp`, `merchant-center-mcp`, etc. — com exemplos de uso
- Guia: "Como construir seu próprio MCP para qualquer plataforma"

### Camada 1 — Stack de Dados (template)
**Arquivo:** `white-label/01-data-stack.md`
Substitui as fontes específicas por um framework de mapeamento:
```
EMPRESA: {{COMPANY_NAME}}
FONTE DE VERDADE (transações): {{DB_TOOL}} — ex: Metabase, Redshift, BigQuery
PLATAFORMA DE ADS: {{ADS_PLATFORM}} — ex: Google Ads, Meta Ads, TikTok
ANALYTICS: {{ANALYTICS_TOOL}} — ex: GA4, Mixpanel, Amplitude
CRM/EMAIL: {{CRM_TOOL}} — ex: Klaviyo, Braze, HubSpot
PRODUTO: {{PRODUCT_PLATFORM}} — ex: Shopify, WooCommerce, plataforma própria
OUTPUT: {{OUTPUT_CHANNEL}} — ex: Google Sheets, Notion, Slack
```

### Camada 2 — Agentes (templates de perfil)
**Arquivos:**
- `white-label/agents/data-analyst.md` — perfil genérico do Analista
- `white-label/agents/ads-manager.md` — perfil genérico do Gestor de Ads
- `white-label/agents/orchestrator.md` — perfil genérico do Orquestrador

Cada um tem seções de placeholder claramente marcadas:
```
## Data Sources
- {{PRIMARY_DB}} via {{DB_TOOL}}
- {{ADS_PLATFORM}} via {{ADS_MCP}}
- {{ANALYTICS}} via {{ANALYTICS_MCP}}
```

### Camada 3 — Skills / Slash Commands (os mais importantes para o aluno)

**3a. Weekly Protocol (o carro-chefe)**
`white-label/skills/weekly-protocol/SKILL.md`
- Mesma estrutura de 7 seções (ou quantas o negócio tiver)
- Placeholders para fontes de dados e métricas
- Comentários explicando PORQUÊ cada seção existe

**3b. Template de skill em branco**
`white-label/skills/_template/SKILL.md`
- A estrutura trigger → context → data pulls → analysis → decision → memory write
- Com anotações pedagógicas em cada bloco

**3c. Weekly Ads Report**
`white-label/skills/weekly-ads-report/SKILL.md`
- Versão genérica para N contas/mercados
- Placeholders para account IDs e benchmarks

### Camada 4 — Governança Multi-Agente (o diferencial)

**4a. Protocolo de Colaboração**
`white-label/governance/ads-collaboration-protocol.md`
- A regra dos 7 dias, os 3 tiers, o circuit breaker
- Adaptado para times pequenos (1-5 pessoas)
- Com seção: "Solo operator? Ainda assim use os tiers — você vai mudar de ideia sobre uma campanha e vai querer saber por quê"

**4b. Pre-Write Check**
`white-label/governance/pre-write-check.md`
- Versão genérica sem referências específicas ao Google Sheets ID

**4c. Log Change**
`white-label/governance/log-change.md`
- Template de change log adaptável

**4d. Agent Protocol**
`white-label/governance/agent-protocol.md`
- As regras universais (read free, propose free, write com aprovação)

### Camada 5 — Memory Layer (o que o aluno mais precisa aprender)

**5a. Decisions Log**
`white-label/memory/decisions-log.md`
- Template com status legend e estrutura de entrada
- Exemplos genéricos de recomendações NOT ACTIONED → ACTIONED

**5b. Baselines**
`white-label/memory/baselines.md`
- Template para o aluno preencher com seus próprios números
- Com guia: "Quais métricas registrar no baseline e por quê"

**5c. CLAUDE.md Hierárquico**
- `white-label/CLAUDE.md` — nível agência/operação
- `white-label/companies/{{COMPANY_NAME}}/CLAUDE.md` — nível empresa

---

## O que NÃO entra no white-label

- Queries SQL específicas do Desertcart (marketplace_data, canonical_orders) — substituídas por templates
- Account IDs reais (Google Ads, Meta, Metabase)
- A `decisions-log.md` com histórico real de decisões
- Os relatórios gerados (reports/)
- A `baselines.md` com números reais do Desertcart
- Referências a Rahul, Debo, Vignes, Cemre (substituídas por "Membro do Time A/B/C")
- Qualquer credencial ou .env

---

## Convenção de placeholders

```
{{COMPANY_NAME}}          — nome da empresa do aluno
{{ADS_PLATFORM}}          — Google Ads, Meta Ads, etc.
{{ADS_MCP}}               — o MCP usado para acessar
{{PRIMARY_DB}}            — fonte de verdade transacional
{{DB_TOOL}}               — Metabase, Redshift, BigQuery, etc.
{{ANALYTICS_TOOL}}        — GA4, Mixpanel, etc.
{{CRM_TOOL}}              — Klaviyo, HubSpot, etc.
{{PRODUCT_PLATFORM}}      — Shopify, plataforma própria, etc.
{{ACCOUNT_ID}}            — ID da conta de ads
{{TARGET_ROAS}}           — ROAS target do negócio
{{ALERT_THRESHOLD}}       — threshold de alerta
{{TEAM_MEMBER_1}}         — nome do primeiro membro do time
{{OPERATOR}}              — quem aprova (em sistemas solo = o próprio)
```

---

## Próximos passos de execução

1. **Criar estrutura de pastas** no `white-label/`
2. **Começar pela Camada 3** (skills) — é o que o aluno vai usar primeiro
3. **Depois a Camada 4** (governança) — o diferencial pedagógico
4. **Depois a Camada 5** (memory) — o que fecha o loop
5. **Por último a Camada 0** (MCPs) — material adicional, vai para o GitHub

**Decisão pendente com Lucas:**
- O white-label inclui o framework multi-agente completo (com Orchestrator + agentes separados), ou começa com versão solo-operator que o aluno pode escalar?
- Sugestão: começar com solo-operator + o protocolo de governança (para quando escalar). O aluno que quiser o multi-agente completo vai ao material avançado.

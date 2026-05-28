# Growth Ops Blueprint

Sistema operacional de marketing construído com Claude Code.
Desenvolvido e operado por [Lucas Antunes](https://github.com/lucas-antunes).

---

## O que é isso

Um sistema completo para operar marketing com Claude Code — desde análise de performance até escrita em plataformas de ads com governança multi-agente.

Cobre 5 camadas:

| Camada | O que faz |
|--------|-----------|
| **CLAUDE.md hierárquico** | Contexto permanente da empresa, carregado automaticamente |
| **Skills** | Slash commands versionados que o Claude executa de forma consistente |
| **Governança** | Protocolo de 3 tiers para escrita em plataformas de ads sem conflito |
| **Memória** | 5 arquivos que acumulam conhecimento entre sessões: decisions-log, baselines, ads-analysis, experiments, MEMORY |
| **Agentes** | Perfis de Data Analyst e Ads Manager configuráveis para a sua empresa |
| **Guia de adaptação** | Configuração por tipo de negócio (e-commerce, SaaS, B2B, infoproduto) |

---

## Estrutura

```
CLAUDE.md                              ← raiz do sistema
guia-adaptacao.md                      ← leia antes de preencher qualquer placeholder

companies/{{COMPANY_NAME}}/
└── CLAUDE.md                          ← contexto da empresa

skills/
├── _template/SKILL.md                 ← template base comentado
├── weekly-protocol/SKILL.md           ← análise completa multi-canal
└── weekly-ads-report/SKILL.md         ← análise de ads cross-conta

governance/
├── ads-collaboration-protocol.md      ← os 3 tiers + regra dos 7 dias
├── pre-write-check.md                 ← verificação antes de escrever
├── log-change.md                      ← registro após escrever
└── agent-protocol.md                  ← regras universais

memory/
├── decisions-log.md                   ← rastreador de recomendações
├── baselines.md                       ← benchmarks por mercado
├── ads-analysis.md                    ← padrões e anomalias acumulados de ads
├── experiments.md                     ← experimentos e decisões de escala
└── MEMORY.md                          ← contexto cross-session do negócio

agents/
├── data-analyst.md                    ← perfil do agente analista de dados
└── ads-manager.md                     ← perfil do agente de gestão de ads
```

---

## Como usar

### 1. Clone o repositório

```bash
git clone https://github.com/lucas-antunes/growth-ops-blueprint.git
cd growth-ops-blueprint
```

### 2. Leia o guia de adaptação

Abra `guia-adaptacao.md` e identifique o seu tipo de negócio:
- **E-commerce** — loja, marketplace, D2C
- **SaaS** — produto de assinatura, app
- **B2B/Leads** — agência, consultoria, serviço
- **Infoproduto** — curso, mentoria, comunidade

### 3. Preencha os placeholders

Substitua todos os `{{PLACEHOLDERS}}` pelos seus dados reais:

```bash
# Para ver todos os placeholders de uma vez
grep -r "{{" . --include="*.md" | grep -v ".git"
```

**Ordem recomendada:**
1. `companies/{{COMPANY_NAME}}/CLAUDE.md` — fontes de dados e contexto
2. `memory/baselines.md` — métricas de referência (use 90 dias de histórico)
3. `skills/weekly-protocol/SKILL.md` — adaptar seções 1, 3 e 4 ao seu negócio
4. `memory/decisions-log.md` — adicionar contexto histórico inicial

### 4. Configure os MCPs

Cada fonte de dados listada no `CLAUDE.md` precisa de um MCP configurado.
Veja a documentação de MCPs do Claude Code para instalação.

### 5. Primeiro check

```
como está [NOME DA EMPRESA]?
```

---

## Tipos de negócio suportados

O sistema é universal na estrutura e adaptável nas métricas.

| Tipo | Seção 1 (fonte de verdade) | Eficiência de Ads |
|------|---------------------------|------------------|
| E-commerce | Pedidos + Receita | ROAS |
| SaaS | MRR + Churn | CAC vs LTV |
| B2B/Leads | Pipeline + MQLs | CPL / CPO |
| Infoproduto | Matrículas + Receita | CPV / ROI |

O protocolo de governança, o decisions-log e a estrutura de skills são idênticos para todos.

---

## O que é universal vs. o que adaptar

**Universal (não muda):**
- Protocolo de governança (`governance/`)
- Decisions-log
- Estrutura de skills (6 passos)
- Ritmo diário + semanal
- Seção de Ads (Seção 2)
- Seção de Decisões (Seção 5)

**Adaptar por tipo de negócio:**
- Seção 1 (fonte de verdade)
- Seção 3 (funil de aquisição)
- Seção 4 (produto/serviço/conteúdo)
- Baselines (métricas de referência)
- Fontes de dados no `CLAUDE.md`

---

## Skills Recomendadas

Suites de skills da comunidade que complementam bem este sistema:

| Suite | O que faz | Repositório |
|-------|-----------|-------------|
| **claude-seo** | SEO completo — audit técnico, conteúdo, schema, sitemap, GEO, programático. Inclui `/ads-meta` e `/ads-google` (audits com scoring ponderado por categoria). | [AgriciDaniel](https://github.com/AgriciDaniel/AgriciDaniel) |
| **ui-ux-pro-max** | Design intelligence para Claude Code: 161 regras de raciocínio, 67 estilos UI, geração de design systems completos para qualquer plataforma ou framework. | [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) |
| **superpowers** | Metodologia de desenvolvimento com agentes: spec → plano → subagent-driven development com TDD real. Disponível no marketplace oficial do Claude Code. | [obra/superpowers](https://github.com/obra/superpowers) |

## Créditos

**Sistema original e arquitetura:** Lucas Antunes

---

## Licença

MIT — use, adapte e distribua livremente.

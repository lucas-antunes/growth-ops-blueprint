# {{OPERATOR_NAME}} — Growth Ops System

Este repositório é o sistema operacional de marketing de {{OPERATOR_NAME}}.
Cada diretório tem seu próprio CLAUDE.md com contexto específico.

## Estrutura

```
companies/{{COMPANY_NAME}}/     ← contexto, protocolos e decisões por empresa
skills/                         ← slash commands (carregados automaticamente)
governance/                     ← regras de escrita em plataformas de ads
memory/                         ← baselines, decisões e histórico
```

## Regras Universais

1. **Leitura é sempre livre** — queries, análises e relatórios não precisam de aprovação
2. **Escrita em plataforma de ads segue os 3 tiers** — leia `governance/ads-collaboration-protocol.md` antes de qualquer write
3. **Toda recomendação vai para o decisions-log** — nenhuma recomendação some sem status registrado
4. **Contexto da empresa é obrigatório** — leia `companies/{{COMPANY_NAME}}/CLAUDE.md` antes de qualquer análise

## Empresas Configuradas

| Empresa | Diretório | Plataforma Principal |
|---------|-----------|---------------------|
| {{COMPANY_NAME}} | `companies/{{COMPANY_NAME}}/` | {{PRODUCT_PLATFORM}} |

## Skills Disponíveis

| Skill | Trigger | O que faz |
|-------|---------|-----------|
| `/weekly-protocol` | "como está {{COMPANY_NAME}}", "check semanal", "daily check" | Análise completa multi-canal |
| `/weekly-ads-report` | "como estão os ads", "relatório de ads" | Performance de ads cross-conta |

## Operador

**{{OPERATOR_NAME}}** — único aprovador de writes em plataformas externas.

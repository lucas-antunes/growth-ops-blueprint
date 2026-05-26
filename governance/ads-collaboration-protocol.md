# Protocolo de Colaboração em Ads

<!--
  ANOTAÇÃO PEDAGÓGICA — POR QUE ESTE PROTOCOLO EXISTE:
  
  Quando você dá acesso de escrita ao Claude nas suas plataformas de ads,
  surge um problema sutil: o Claude não sabe o que o Claude fez ontem.
  
  Cada sessão começa do zero. Sem um protocolo, você pode ter:
  - Campanhas pausadas e reativadas na mesma semana por sessões diferentes
  - Experimentos cancelados antes de ter dados suficientes
  - Mudanças de orçamento contraditórias sem histórico de por quê
  
  Este protocolo resolve isso em 3 regras:
  1. Classifique qualquer mudança em um tier antes de fazer
  2. Verifique o changelog antes de escrever (especialmente Tier 3)
  3. Registre tudo no log depois de escrever
  
  Para operadores solo: o protocolo ainda se aplica.
  Você vai mudar de ideia sobre uma campanha e vai querer saber
  qual era a lógica da mudança anterior e quando foi feita.
  
  Para times: o protocolo previne conflitos entre múltiplos agentes
  e múltiplas pessoas com acesso às mesmas contas.
-->

## Regras Fundamentais

1. **Verifique antes de escrever.** Leia o changelog da plataforma E o log compartilhado antes de qualquer mudança.
2. **Regra dos 7 dias.** Se você ou outro agente fez uma mudança Tier 3 em uma campanha nos últimos 7 dias, não modifique — verifique o resultado primeiro.
3. **Registre tudo.** Toda escrita em plataforma → entrada no log.
4. **Emergência = pausa apenas.** Se precisar agir em cima de uma mudança recente de outra sessão, só pause. Nunca modifique.
5. **Experimentos precisam de tempo.** Mínimo 7 dias de observação antes de reverter qualquer mudança Tier 3.

---

## Os 3 Tiers de Escrita

<!--
  ANOTAÇÃO: A classificação em tiers existe porque nem toda mudança
  tem o mesmo risco. Pausar uma campanha de $500/dia é diferente de
  adicionar uma keyword negativa. O tier determina o nível de cautela.
-->

### Tier 1 — Leitura & Análise (sempre livre)

Nenhuma regra se aplica. Execute sem restrições:
- Puxar dados de performance
- Gerar relatórios e análises
- Ler changelogs
- Ler o log de mudanças

### Tier 2 — Escrita de Baixo Risco (execute + registre)

Pode ser executado sem verificar ownership. **Obrigatório: registrar no log após.**

Exemplos:
- Adicionar/remover keywords negativas
- Atualizar copy de anúncios, assets, extensões
- Ajustar lances em menos de 20%
- Ajustes de bid por dispositivo/horário/localização
- Corrigir disapprovals no Merchant Center
- Atualizar atributos de feed (título, descrição, imagens)
- Trocar criativos em Meta Ads

**Fluxo:** execute → rode `/log-change`

### Tier 3 — Escrita de Alto Risco (verifique + execute + registre)

**Obrigatório: rode `/pre-write-check` ANTES de executar.** Sujeito à regra dos 7 dias.

Exemplos:
- Pausar ou ativar campanhas
- Mudanças de orçamento acima de 20%
- Mudar estratégia de lance (ex: tROAS → Max Conversions)
- Alterar metas de tROAS / tCPA
- Criar ou deletar campanhas
- Pausar/ativar contas inteiras
- Alterar configurações de conversão
- Mudar países-alvo no Google Ads ou Merchant Center
- Alterar objetivo ou otimização de campanha no Meta Ads

**Fluxo:** rode `/pre-write-check` → se PROCEED: execute → rode `/log-change`

---

## Fluxo de Decisão

```
Quero fazer uma mudança?
    │
    ├── É Tier 1 (leitura)? → Faça
    │
    ├── É Tier 2 (baixo risco)?
    │       → Faça → rode /log-change
    │
    └── É Tier 3 (alto risco)?
            → rode /pre-write-check PRIMEIRO
            → Se PROCEED: faça → rode /log-change
            → Se BLOCKED: espere o período de ownership expirar
            → Se EMERGÊNCIA: pause apenas → rode /log-change com status EMERGENCY_PAUSED
```

---

## A Regra dos 7 Dias

### Como Funciona

1. Quando você faz uma mudança Tier 3, você é o **owner** daquela mudança por 7 dias
2. Durante os 7 dias: só você pode modificar, ajustar ou reverter
3. Após 7 dias: a mudança vira "patrimônio comum" — qualquer sessão pode modificar
4. Você pode estender o período (ex: 14 dias para tROAS entrar em aprendizado)

### Escopo

O ownership cobre as **campanhas específicas** que você mudou, não a conta inteira.

---

## Circuit Breaker de Emergência

Qualquer sessão pode sobrescrever a regra dos 7 dias — mas **só pausar, nunca modificar**:

| Condição | Threshold |
|----------|-----------|
| ROAS em colapso | ROAS < `{{EMERGENCY_ROAS_THRESHOLD}}` por 2 dias consecutivos, spend > `{{EMERGENCY_MIN_SPEND}}` |
| Zero conversões | 0 conversões por `{{EMERGENCY_ZERO_CONV_DAYS}}` dias, spend > `{{EMERGENCY_ZERO_SPEND}}` |
| Violação de política | Aviso de suspensão ou flag de política detectado |
| Erro óbvio de orçamento | Budget claramente errado (ex: $15.000/dia em vez de $1.500/dia) |

**Após uma pausa de emergência:**
1. Registre no log com status `EMERGENCY_PAUSED`
2. Anote qual sessão/pessoa fez a mudança original
3. O owner original decide os próximos passos

---

## Log de Mudanças

**Localização:** `memory/ads-change-log.md` (ou Google Sheets — configure em `CLAUDE.md`)

Cada entrada de mudança deve conter:

| Campo | Descrição |
|-------|-----------|
| `data` | Quando foi feito |
| `plataforma` | Google Ads, Meta Ads, etc. |
| `conta` | Qual conta/mercado |
| `campanha` | Nome e ID da campanha |
| `tier` | 2 ou 3 |
| `ação` | O que foi mudado |
| `valor_anterior` | Estado antes da mudança |
| `valor_novo` | Estado após a mudança |
| `racional` | Por que foi feito (com dados) |
| `data_revisão` | Quando revisar o resultado |
| `rollback_trigger` | O que causaria reversão (Tier 3 apenas) |

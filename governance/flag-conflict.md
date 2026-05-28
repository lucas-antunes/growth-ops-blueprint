---
name: flag-conflict
description: >
  Rode quando pre-write-check retornar BLOCKED ou quando uma ação de escrita
  colidir com um experimento ativo, mudança recente de outra sessão, ou
  ownership em vigor. Documenta o conflito e apresenta opções ao operador.
---

# Flag Conflict

<!--
  ANOTAÇÃO PEDAGÓGICA:

  Este skill é o que acontece quando o pre-write-check diz "não".

  Sem ele, o sistema teria dois caminhos ruins:
  1. Ignorar o BLOCKED e escrever mesmo assim — desfaz trabalho anterior
  2. Silenciosamente abandonar a mudança — você não sabe que ela não aconteceu

  Com o flag-conflict, o resultado é sempre o mesmo: o conflito fica visível,
  as opções ficam claras, e a decisão fica com o operador.

  Nenhum conflito é resolvido silenciosamente.
-->

## Quando Usar

- `pre-write-check` retornou `BLOCKED`
- Tentativa de modificar campanha em experimento ativo
- Dois agentes ou sessões tentam modificar a mesma campanha

---

## Processo

### Passo 1 — Capturar o Contexto do Conflito

Registre:
- **Ação bloqueada:** o que você tentava fazer (plataforma, conta, campanha, mudança)
- **Causa do bloqueio:** ownership recente? experimento ativo?
- **Origem da mudança anterior:** quando foi feita e por qual sessão/agente
- **Expiração do bloqueio:** quando a ownership expira ou o experimento tem revisão

### Passo 2 — Consultar o Experimento (se aplicável)

Se o bloqueio for por experimento ativo, abra `memory/experiments.md` e leia:
- Hipótese do experimento
- Critério de sucesso definido
- Data de revisão agendada
- Status atual

Isso informa se a mudança bloqueada colocaria o experimento em risco.

### Passo 3 — Apresentar ao Operador

Apresente o conflito no formato abaixo e **aguarde decisão explícita**.

```
## Conflito Detectado

**Ação bloqueada:** [plataforma] / [conta] / [campanha]
**Mudança pretendida:** [o que mudaria]
**Causa:** [ownership em vigor / experimento ativo]

**Detalhes do bloqueio:**
- Última mudança: [data] — [o que foi alterado]
- Ownership expira: [data]
[Se experimento ativo:]
- Experimento: [nome]
- Hipótese: [hipótese]
- Revisão agendada: [data]

**Opções:**

A) Aguardar — esperar até [data de expiração] e re-executar a mudança
B) Substituir — executar agora com aprovação explícita (ownership é transferida)
   Risco: interrompe [nome do experimento / sequência anterior]
C) Cancelar — abandonar a mudança, registrar no decisions-log como REJECTED

Qual opção? (A / B / C)
```

### Passo 4 — Executar Conforme Decisão

**Se A (Aguardar):**
- Registrar no `decisions-log.md` com status `NOT ACTIONED`
- Incluir data de expiração e contexto do bloqueio
- Não executar nenhuma mudança

**Se B (Substituir com aprovação):**
- Confirmar que `{{OPERATOR_NAME}}` digitou "B" explicitamente
- Executar a mudança via ferramenta de escrita
- Executar `/log-change` imediatamente após
- Registrar no `decisions-log.md` com status `ACTIONED` + nota "conflito substituído com aprovação"
- Se havia experimento ativo: atualizar `memory/experiments.md` com status `ENCERRADO` e motivo

**Se C (Cancelar):**
- Registrar no `decisions-log.md` com status `REJECTED`
- Incluir motivo: conflito com [ownership / experimento] não resolvido

---

## Formato de Registro no Decisions-Log

```markdown
### [YYYY-MM-DD] Conflito — [Campanha] em [Conta]

**Contexto:** Tentativa de [mudança] bloqueada por [causa].
**Ação pretendida:** [detalhe da mudança]
**Conflito com:** [sessão anterior de data X / experimento Y]
**Resolução:** [Aguardando expiração em DATA / Substituído com aprovação / Cancelado]
**Status:** NOT ACTIONED / ACTIONED / REJECTED
```

---

## Regras

- Nunca resolver um conflito sem apresentar ao operador
- Nunca assumir que "B" é a escolha certa — o risco de interromper um experimento é do operador, não do sistema
- Toda resolução — incluindo cancelamentos — fica registrada no decisions-log
- Se o operador não responder: tratar como opção A (aguardar)

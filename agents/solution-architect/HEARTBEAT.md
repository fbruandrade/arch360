# HEARTBEAT.md — Checklist de heartbeat do Solution Architect

Siga este checklist toda vez que for acordado. Use a skill `paperclip` para toda
coordenação; ela detalha cada chamada da API.

## 1. Identidade e contexto

- `GET /api/agents/me` — confirme seu id, cargo, orçamento e cadeia de comando.
- Verifique o contexto do despertar: `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`,
  `PAPERCLIP_WAKE_COMMENT_ID` e `PAPERCLIP_APPROVAL_ID`.
- Releia `./SOUL.md`.

## 2. Memória

- Leia a nota do dia em `$AGENT_HOME/memory/AAAA-MM-DD.md`.
- Carregue o `summary.md` das entidades relevantes em `$AGENT_HOME/life/` — decisões
  anteriores, premissas sobre o parque do banco, lacunas regulatórias já identificadas.

## 3. Aprovações

Se `PAPERCLIP_APPROVAL_ID` estiver definido, trate a aprovação primeiro: revise-a junto com
as issues vinculadas, feche as que ela resolve e comente nas que continuam abertas.

## 4. Caixa de entrada

- `GET /api/companies/{companyId}/issues?assigneeAgentId={seu-id}&status=todo,in_progress,in_review,blocked`
- Prioridade: `in_progress` primeiro; depois `in_review`, se você foi acordado por um
  comentário nela; depois `todo`. Pule `blocked`, a menos que consiga desbloquear.
- Se `PAPERCLIP_TASK_ID` estiver definido e for sua, comece por ela.
- Se foi acordado por menção, leia a thread do comentário antes de qualquer outra coisa.

## 5. Checkout e entendimento

- Se o despertar já fez checkout da issue, siga em frente. Só chame
  `POST /api/issues/{id}/checkout` ao trocar de tarefa por conta própria.
- **Nunca repita uma chamada que retornou 409** — a tarefa pertence a outro agente.
- Leia a issue, os comentários e as issues ancestrais para entender por que ela existe.

## 6. Trabalho do seu domínio

- Antes de desenhar, liste os padrões de plataforma que se aplicam à iniciativa e confirme
  que estão vigentes no registro de decisões.
- Desenhe primeiro o fluxo de dados, marcando onde há dados pessoais e dados de cartão.
- Aplique os critérios de posicionamento do Enterprise Architect a cada workload e mostre a
  resposta.
- Onde um padrão não atender, abra uma issue para o Principal Architect como ADR candidato
  ou waiver; não siga desenhando em volta do problema.
- Peça parecer do Security Architect quando o fluxo tocar o CDE ou dados pessoais sensíveis.

## 7. Status e passagem

- Toda mudança de estado leva o header `X-Paperclip-Run-Id`.
- Pronto para revisão: `in_review` + menção ao Principal Architect com link e resumo.
- Bloqueado: `blocked`, dizendo o que bloqueia, por quê e quem desbloqueia; use
  `blockedByIssueIds` quando o bloqueio for outra issue.
- Precisa de decisão de sim/não: crie uma interação `request_confirmation` na issue, em vez
  de perguntar em texto solto.
- Trabalho de outro domínio: crie uma issue filha (com `parentId` e `goalId`) e atribua ao
  architect dono. Nunca cancele tarefas de outro domínio — reatribua ao Principal Architect
  com um comentário.

## 8. Memória e saída

- Registre fatos duráveis em `$AGENT_HOME/life/` e a linha do tempo do dia em
  `$AGENT_HOME/memory/AAAA-MM-DD.md`.
- Comente em todo trabalho `in_progress` antes de sair, com a próxima ação explícita.
- Sem atribuições e sem menção válida: saia. Não procure trabalho não atribuído.

## Regras

- Se a issue é acionável, avance algo concreto nesta execução; não saia só com um plano,
  a menos que a tarefa peça um plano.
- Comentários em pt-BR e em markdown conciso: linha de status, bullets, links.
- Acima de 80% do orçamento, trabalhe só no que for crítico e avise o Principal Architect.

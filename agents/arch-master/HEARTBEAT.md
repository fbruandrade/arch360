# HEARTBEAT.md — Checklist de heartbeat do Arch Master

Você é acordado periodicamente (a cada 5 minutos, quando há trabalho) e quando o board
interage. Siga este checklist em todo heartbeat. Use a skill `paperclip` para toda
coordenação e a skill `paperclip-board` para operações de gestão da company.

## 1. Identidade e contexto

- `GET /api/agents/me` — confirme seu id, orçamento e cadeia de comando.
- Verifique o contexto do despertar: `PAPERCLIP_TASK_ID`, `PAPERCLIP_WAKE_REASON`,
  `PAPERCLIP_WAKE_COMMENT_ID` e `PAPERCLIP_APPROVAL_ID`.

## 2. Plano do dia

1. Leia o plano do dia em `$AGENT_HOME/memory/AAAA-MM-DD.md`, na seção "## Plano de hoje".
2. Revise cada item: o que está concluído, o que está bloqueado, o que vem a seguir.
3. Resolva bloqueios que são seus; leve ao board os que só ele resolve.
4. Registre o progresso nas notas do dia.

## 3. Aprovações

Se `PAPERCLIP_APPROVAL_ID` estiver definido, revise a aprovação e as issues vinculadas;
feche as resolvidas e comente nas que continuam abertas.

## 4. Caixa de entrada

- `GET /api/companies/{companyId}/issues?assigneeAgentId={seu-id}&status=todo,in_progress,in_review,blocked`
- Prioridade: `in_progress`; depois `in_review`, se acordado por comentário nela; depois
  `todo`. Pule `blocked`, a menos que consiga desbloquear.
- Se `PAPERCLIP_TASK_ID` estiver definido e for sua, comece por ela.
- Se a descrição invocar `/first-task`, siga a skill `first-task`.

## 5. Encaminhamento

- Pedido de arquitetura do board: esclareça o objetivo (com `ask_user_questions` se
  faltar algo essencial), crie a issue com `parentId` e `goalId` e atribua ao Principal
  Architect. Não atribua diretamente a architects de domínio.
- Plano que precisa da aprovação do board: atualize o documento `plan`, crie um
  `request_confirmation` vinculado à revisão mais recente e coloque a issue em `in_review`.
  Não crie issues de execução antes do aceite.
- Falta de capacidade: proponha a contratação com a skill `paperclip-create-agent` e peça
  confirmação ao board.

## 6. Acompanhamento

- Verifique as issues-pai que você criou: o que avançou, o que bloqueou, o que o
  Principal Architect liberou como pronto para o comitê.
- Informe o board sobre artefatos prontos para o comitê e bloqueios que dependem dele.
- Nunca cancele tarefas de outro agente; reatribua ao Principal Architect com um
  comentário.

## 7. Memória

1. Extraia fatos duráveis das conversas desde a última extração para `$AGENT_HOME/life/`
   (skill `para-memory-files`): decisões do board, preferências, premissas do banco.
2. Atualize a linha do tempo em `$AGENT_HOME/memory/AAAA-MM-DD.md`.

## 8. Saída

- Comente em todo trabalho `in_progress` antes de sair.
- Sem atribuições e sem menção válida: saia. Não procure trabalho não atribuído.

## Regras

- Sempre inclua o header `X-Paperclip-Run-Id` em chamadas que alteram estado.
- Comentários em pt-BR e em markdown conciso: linha de status, bullets, links.
- Acima de 80% do orçamento, trabalhe só no que for crítico e avise o board.
- Faça checkout por conta própria só quando for mencionado explicitamente.

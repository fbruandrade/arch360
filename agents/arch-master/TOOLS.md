# TOOLS.md — Ferramentas do Arch Master

## Skills disponíveis

- `paperclip` — tarefas, heartbeats, comentários, documentos de issue, delegação e interações.
- `paperclip-board` — gestão da company como board: onboarding, agentes, aprovações,
  monitoramento de tarefas, custos e revisão de entregas.
- `paperclip-converting-plans-to-tasks` — transforma um plano em um grafo de issues com
  donos, dependências e paralelismo. Para trabalho de arquitetura, prefira deixar a
  decomposição com o Principal Architect.
- `paperclip-create-agent` — contratação de novos agentes, com revisão de configuração e
  pedido de aprovação.
- `para-memory-files` — memória persistente em `$AGENT_HOME` (PARA).
- `first-task` — conduz a primeira tarefa do usuário quando a descrição invoca `/first-task`.
- `stakeholder-update` — atualizações de status por público e cadência — use para os relatos ao board. (`./skills/stakeholder-update/SKILL.md`)
- `architecture` — análise de opções e trade-offs para ADRs e revisão de desenhos. O formato de saída é sempre o template em pt-BR do Governance Architect, nunca o formato próprio da skill. (`./skills/architecture/SKILL.md`)
- `code-review` — revisão de código quanto a segurança, desempenho e correção — use em implementações de referência das ARs e em provas de conceito. (`./skills/code-review/SKILL.md`)
- `debug` — sessão estruturada de depuração: reproduzir, isolar, diagnosticar e corrigir. (`./skills/debug/SKILL.md`)
- `deploy-checklist` — checklist de pré-implantação: migrações, feature flags, aprovações e gatilhos de rollback. (`./skills/deploy-checklist/SKILL.md`)
- `documentation` — documentação técnica: guias, runbooks, documentação de API e de arquitetura. (`./skills/documentation/SKILL.md`)
- `incident-response` — triagem, comunicação e postmortem de incidentes — use os postmortems para alimentar padrões de DR e domínios de falha. (`./skills/incident-response/SKILL.md`)
- `standup` — resumo de atividade no formato ontem/hoje/bloqueios. No Paperclip, o comentário do heartbeat já cumpre esse papel; use só quando pedirem um resumo. (`./skills/standup/SKILL.md`)
- `system-design` — desenho de sistemas e serviços: requisitos, fronteiras de serviço, modelagem de dados, APIs e trade-offs de escala. (`./skills/system-design/SKILL.md`)
- `tech-debt` — identificação e priorização de dívida técnica — use na racionalização do portfólio e no radar de tecnologia. (`./skills/tech-debt/SKILL.md`)
- `testing-strategy` — estratégia de testes — use para definir como provar NFRs, SLOs e padrões de resiliência. (`./skills/testing-strategy/SKILL.md`)

## Notas de uso

(Anote aqui o que aprender sobre ferramentas, APIs e atalhos que valham para as próximas
execuções. Mantenha curto e datado.)

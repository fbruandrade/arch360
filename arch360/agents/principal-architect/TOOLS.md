# TOOLS.md — Ferramentas e referências do Principal Architect

## Skills disponíveis

- `paperclip` — tarefas, heartbeats, comentários, documentos de issue, delegação, aprovações e interações com o board. Use para toda coordenação com a API do Paperclip.
- `para-memory-files` — memória persistente em `$AGENT_HOME` (PARA): fatos duráveis em `life/`, notas diárias em `memory/`.
- `paperclip-converting-plans-to-tasks` — transforma um plano em um grafo de issues com donos, dependências, bloqueios e paralelismo.
- `fable-judge` — verificação adversarial de trabalho dado como pronto: trata o "pronto" como um conjunto de afirmações e as checa com evidência. (`./skills/fable-judge/SKILL.md`)
- `roadmap-update` — priorização de roadmap em Now/Next/Later — use para ordenar e repriorizar o backlog de artefatos. (`./skills/roadmap-update/SKILL.md`)
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

## Referências regulatórias comuns ao Escritório

Confirme a versão vigente antes de citar — normas são alteradas e consolidadas. Se não
conseguir confirmar, cite como lacuna em vez de afirmar.

- **Resolução CMN nº 4.893/2021** (e alterações) — política de segurança cibernética e
  requisitos para contratação de serviços de processamento e armazenamento de dados e de
  computação em nuvem por instituições autorizadas pelo Banco Central. Pontos que mais
  aparecem em decisões: comunicação prévia ao BCB na contratação de serviços relevantes de
  nuvem, contratação no exterior, plano de continuidade e de saída do fornecedor, gestão de
  incidentes.
- **Resolução BCB nº 85/2021** — equivalente para instituições de pagamento, quando aplicável.
- **Resolução CMN nº 4.557/2017** — gerenciamento de riscos (inclui risco operacional e
  continuidade de negócios).
- **Lei nº 13.709/2018 (LGPD)** e regulamentos da ANPD — bases legais, minimização,
  segurança, comunicação de incidentes, transferência internacional de dados.
- **PCI DSS v4.0.1** (PCI SSC) — escopo do CDE, segmentação, proteção de dados de conta,
  gestão de acesso, logging e monitoramento. Use também o guia do PCI SSC sobre escopo e
  segmentação de rede.

## Referências do seu domínio

- Todas as referências de domínio listadas nos TOOLS.md dos architects — você precisa conhecê-las para revisar, não para escrevê-las.
- ISO/IEC/IEEE 42010 — o que torna uma descrição de arquitetura completa.
- Formato de ADR de Michael Nygard — a régua mínima de contexto, decisão e consequências.

## Notas de uso

(Anote aqui o que aprender sobre ferramentas, APIs, fontes e atalhos que valham para as
próximas execuções. Mantenha curto e datado.)

---
name: registro-adr
key: "arch360/governance-architect/registro-adr"
description: Mantém o registro de decisões do Escritório de Arquitetura — ADRs, ARs e waivers — com status, cadeia de substituição e vencimentos. Use para registrar um novo artefato, mudar status (proposta, aceita, substituída, descontinuada), substituir uma decisão por outra, registrar ou renovar um waiver, checar waivers vencidos ou responder "qual decisão rege X, desde quando e por quê?".
---

# Registro de decisões

O registro é a fonte única de verdade sobre o que está decidido no banco. Um auditor tem de
conseguir, só com ele, reconstruir qual decisão rege um assunto, desde quando, quem a
propôs e o que ela substituiu.

## Onde fica

Enquanto o Escritório não decidir um local definitivo (por ADR), o registro é o documento
`registro` de uma issue fixa chamada **"Registro de decisões"**, atribuída ao Governance
Architect no projeto do Escritório. Se essa issue não existir, crie-a. Cada atualização do
registro é uma nova revisão do documento, com um comentário dizendo o que mudou.

## Identificadores e chaves

Identificadores e chaves de metadados são independentes de idioma, para que as ferramentas
não dependam do texto. Títulos e valores de status são em pt-BR.

- ADR: `ADR-0001`, `ADR-0002`, … (sequencial, nunca reutilizado)
- AR: `AR-0001`, …
- Waiver: `WVR-0001`, …

## Estrutura de uma entrada

```yaml
- id: ADR-0007
  title: "Padrão de landing zone na OCI"
  status: proposta          # proposta | aceita | substituída | descontinuada
  domain: cloud             # enterprise | solution | technical | cloud | infrastructure | security | governance
  owner: cloud-architect
  created: 2026-10-05
  status_changed: 2026-10-05
  committee_ref: null       # referência da decisão do comitê da guilda, quando houver
  supersedes: []            # ids que esta decisão substitui
  superseded_by: null
  waivers: []               # ids de waivers contra esta decisão
  link: <link do documento>
```

Waiver:

```yaml
- id: WVR-0003
  against: ADR-0007
  requester: <time ou iniciativa>
  scope: "<o que fica fora do padrão>"
  risk_accepted: "<exposição em termos claros>"
  remediation: "<plano para voltar ao padrão>"
  owner: <responsável pela remediação>
  expires: 2027-03-31
  status: vigente           # vigente | vencido | encerrado
```

## Transições permitidas

| De | Para | Condição |
|----|------|----------|
| — | proposta | Artefato criado no template do Governance |
| proposta | aceita | Decisão do comitê da guilda registrada em `committee_ref` |
| proposta | descontinuada | Retirado antes de ir ao comitê, ou rejeitado; registre o motivo |
| aceita | substituída | Existe um ADR **aceito** que a substitui; preencha `superseded_by` e o `supersedes` do novo |
| aceita | descontinuada | Deixa de valer sem substituto; registre o motivo |

Nenhuma outra transição existe. Em particular: o Escritório nunca move nada para "aceita"
sem decisão do comitê, e uma decisão só fica "substituída" quando a nova já está "aceita".

## Operações

### Registrar novo artefato
Atribua o próximo id, crie a entrada com `status: proposta` e comente o id na issue do
artefato para que o autor o use no documento.

### Mudar status
Confira a tabela de transições. Atualize `status`, `status_changed` e, se for o caso,
`committee_ref`, `supersedes` e `superseded_by` dos dois lados.

### Responder "qual decisão rege X?"
Busque entradas com `status: aceita` cujo título ou domínio cubra X. Siga `superseded_by`
até o fim da cadeia. Responda com o id, o título, a data de aceite e os waivers vigentes
contra ela.

### Checar waivers
Liste waivers `vigente` com `expires` nos próximos 30 dias ou já passado. Para os vencidos,
mude para `vencido` e abra uma issue para o `owner`. Para os que vencem em breve, avise o
`owner` na issue de origem.

## Regras

- Nunca apague uma entrada. Decisões encerradas mudam de status; não somem.
- Nunca reutilize um id.
- Toda entrada aponta para o documento do artefato.
- Waiver sem `expires`, `owner` e `risk_accepted` não é registrado — devolva ao solicitante.

---
name: regulatorio-bacen
key: "arch360/security-architect/regulatorio-bacen"
description: Checklist regulatório de arquitetura para o banco — Resolução CMN 4.893/2021 (segurança cibernética e contratação de processamento, armazenamento e computação em nuvem), LGPD e PCI-DSS — que gera a seção de conformidade de um ADR ou AR. Use quando uma decisão envolver contratar ou mudar serviço de nuvem, armazenar ou processar dados fora do data center, transferir dados para o exterior, tratar dados pessoais ou de cartão, ou quando pedirem "conformidade regulatória", "o que o BACEN exige aqui?", "isso precisa ser comunicado ao Banco Central?" ou "mapear obrigação para controle".
---

# Checklist regulatório de arquitetura

Gera a seção de conformidade regulatória de um artefato. O objetivo não é parecer jurídico:
é mostrar ao comitê da guilda — e, mais tarde, a um auditor — que a decisão considerou as
obrigações, onde atende, onde não atende e o que ficou como lacuna.

## Antes de começar: confirme as versões

Normas são alteradas e consolidadas. Antes de citar, confirme a versão vigente de cada uma
na fonte oficial (site do Banco Central, Planalto, ANPD, PCI SSC). Se não conseguir
confirmar nesta execução, cite como está abaixo e marque **"versão não confirmada"** na
seção de lacunas. Nunca invente número de artigo, prazo ou resolução.

## Passo 1 — Gatilhos

Marque quais se aplicam à decisão. Cada gatilho marcado abre um bloco do checklist.

- [ ] **A.** Contrata, amplia ou muda serviço de processamento, armazenamento de dados ou
  computação em nuvem (inclui trocar de região, de fornecedor ou de modelo de serviço).
- [ ] **B.** Dados passam a ser armazenados ou processados fora do Brasil.
- [ ] **C.** Trata dados pessoais (LGPD).
- [ ] **D.** Toca dados de portador de cartão ou componentes conectados ao CDE (PCI-DSS).
- [ ] **E.** Altera controles de segurança cibernética: prevenção, detecção, resposta a
  incidentes, rastreabilidade, segmentação, criptografia, gestão de acesso.
- [ ] **F.** Afeta continuidade de negócios: RTO/RPO, DR, dependência de fornecedor único.

## Passo 2 — Checklist por gatilho

### A. Contratação de nuvem e de processamento de dados (Res. CMN 4.893/2021)

- O serviço é **relevante** para a instituição? Registre o critério usado. Serviços
  relevantes atraem as exigências mais fortes da norma.
- Due diligence do fornecedor: capacidade técnica, certificações, controles de segurança,
  capacidade de atender à regulamentação.
- O contrato prevê: acesso do Banco Central a dados, informações e instalações; acesso da
  instituição às informações; continuidade em caso de extinção do contrato, com
  transferência dos dados e remoção pelo fornecedor; notificação de subcontratação;
  comunicação de incidentes.
- **Comunicação ao Banco Central:** a contratação de serviço relevante exige comunicação ao
  BCB. Confirme o prazo e o conteúdo vigentes e registre quem é o responsável pelo envio.
- **Plano de saída:** como o banco migra para outro fornecedor ou de volta para o data
  center, e em quanto tempo. Sem plano de saída, registre como lacuna.
- Monitoramento contínuo do fornecedor e do serviço.

### B. Dados no exterior

- Países e regiões onde os dados serão armazenados e processados, declarados
  explicitamente (não apenas "região padrão do fornecedor").
- Res. CMN 4.893: verifique as condições para contratação no exterior, incluindo a
  existência de convênio para troca de informações entre o Banco Central e as autoridades
  supervisoras dos países envolvidos, e a garantia de acesso do BCB.
- LGPD: base para transferência internacional (arts. 33 a 36) e o regulamento da ANPD sobre
  transferência internacional de dados (cláusulas-padrão contratuais ou outro mecanismo).

### C. Dados pessoais (LGPD — Lei nº 13.709/2018)

- Base legal do tratamento e finalidade.
- Minimização: o desenho coleta e retém só o necessário? Prazo de retenção definido?
- Segurança (art. 46): medidas técnicas e administrativas proporcionais.
- Direitos dos titulares: o desenho permite localizar, corrigir, anonimizar e eliminar os
  dados de um titular?
- Incidentes: o desenho dá rastreabilidade suficiente para comunicar à ANPD e aos titulares
  dentro do prazo regulamentar vigente?
- Operadores e suboperadores identificados.

### D. Cartões (PCI DSS v4.0.1)

Use a skill `threat-model-pci`, se disponível, e traga para cá o resultado:
o efeito sobre o escopo do CDE (reduz / mantém / amplia) e as famílias de requisitos
afetadas.

### E. Segurança cibernética (Res. CMN 4.893/2021)

- A decisão é coerente com a política de segurança cibernética do banco?
- Prevenção, detecção e resposta: o que muda em logging, monitoramento, rastreabilidade e
  resposta a incidentes?
- Classificação dos dados por relevância e os controles proporcionais a ela.
- Incidentes relevantes: o desenho permite registrar, analisar causa e impacto e alimentar
  o relatório anual de segurança cibernética?

### F. Continuidade (Res. CMN 4.893/2021 e Res. CMN 4.557/2017)

- RTO/RPO declarados e coerentes com a criticidade do serviço.
- Cenários de incidente que afetam a continuidade, incluindo a indisponibilidade do
  fornecedor de nuvem.
- Teste periódico previsto.
- Concentração: a decisão aumenta a dependência de um único fornecedor?

## Passo 3 — Seção de saída

```markdown
## Conformidade regulatória

**Gatilhos aplicáveis:** A, C, D (ex.)

| Obrigação | Norma | Situação | Evidência no desenho | Dono |
|---|---|---|---|---|
| Comunicação ao BCB da contratação | Res. CMN 4.893/2021 | Atende / Não atende / Lacuna / N/A | <onde o artefato trata disso> | <quem> |

### Lacunas
- <obrigação> — <o que falta confirmar ou decidir> — <de quem depende>

### Versões consultadas
- Res. CMN 4.893/2021 — <versão/data confirmada ou "versão não confirmada">
```

## Regras

- **Situação** só pode ser *Atende*, *Não atende*, *Lacuna* ou *N/A*. *N/A* exige uma
  frase dizendo por quê.
- Nunca escreva "em conformidade" para o artefato como um todo. Conformidade é declarada
  obrigação por obrigação.
- O que um controle de segurança precisa ser é decisão do Security Architect; esta skill
  registra a obrigação e a evidência, não inventa o controle.
- "Não atende" sem plano vira candidato a waiver: encaminhe ao Governance Architect.

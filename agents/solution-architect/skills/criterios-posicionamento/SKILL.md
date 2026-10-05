---
name: criterios-posicionamento
key: "arch360/solution-architect/criterios-posicionamento"
description: Aplica os critérios de posicionamento de workload do banco para decidir onde um workload deve rodar — data center active-active, Azure, OCI ou AWS — e registra a decisão de forma rastreável. Use quando pedirem "onde esse sistema deve rodar?", "on-prem ou nuvem?", "qual nuvem?", "posicionamento de workload", "migrar para a nuvem", ou ao desenhar um HLD ou AR que precise justificar o ambiente de cada componente. O Enterprise Architect é o dono dos critérios; os demais architects os aplicam.
---

# Critérios de posicionamento de workload

Quem define os critérios é o Enterprise Architect. Quem dá a resposta por ambiente são o
Cloud Architect (Azure, OCI, AWS) e o Infrastructure Architect (data center). Esta skill é
o procedimento comum para que todos apliquem os mesmos critérios do mesmo jeito.

## Passo 0 — Use a matriz vigente

A matriz oficial é a do ADR **aceito** mais recente sobre posicionamento de workload, no
registro de decisões. Se ela existir, use os critérios e pesos dela e ignore a matriz
inicial abaixo.

Se ainda não existir ADR aceito, use a matriz inicial abaixo e escreva no resultado:
**"Aplicada a matriz inicial — critérios ainda não aceitos pelo comitê."** Se você é o
Enterprise Architect, transformar a matriz inicial em ADR é trabalho seu.

## Passo 1 — Caracterize o workload

| Campo | Pergunta |
|-------|----------|
| Criticidade | Qual o impacto de negócio se parar? RTO/RPO exigidos? |
| Dados | Classes de dados: dados de cartão (PCI-DSS), dados pessoais (LGPD), outros |
| Dependências | Com quais sistemas conversa, com qual latência e volume? Onde eles estão hoje? |
| Perfil de carga | Estável, sazonal, em picos? |
| Arquitetura | VM, contêiner (OpenShift/Kubernetes), serviço gerenciado, pacote de mercado |
| Estado | Novo, existente estável, existente em modernização, em descontinuação |
| Exposição | Interna, parceiros, pública; qual API gateway |

## Passo 2 — Critérios eliminatórios

Aplique primeiro. Cada critério eliminatório retira ambientes da disputa; registre qual
critério eliminou qual ambiente.

1. **Regulatório** — a contratação de nuvem atende à Res. CMN 4.893 para este workload
   (serviço relevante, localização dos dados, contrato, plano de saída)? Se a localização
   exigida não está disponível em uma nuvem, ela sai.
2. **Latência e acoplamento** — o workload tem dependência síncrona de baixa latência e
   alto volume com sistemas que ficam no data center? Se sim, a nuvem só segue se a
   conectividade híbrida comprovadamente atender.
3. **Escopo PCI-DSS** — o posicionamento amplia o escopo do CDE sem que haja controle de
   segmentação viável no ambiente? Se sim, o ambiente sai.
4. **Capacidade da plataforma** — o ambiente tem a landing zone, o serviço e os controles
   prontos? Se não, ele só segue com um plano e prazo, registrados.

## Passo 3 — Critérios classificatórios (matriz inicial)

Pontue de 1 a 5 cada ambiente que sobrou. Pesos iniciais sugeridos — a matriz aceita
prevalece:

| Critério | Peso | O que pontua alto |
|----------|------|-------------------|
| Aderência a serviços gerenciados diferenciados | 3 | O ambiente oferece serviço gerenciado que elimina operação relevante |
| Elasticidade | 2 | A carga varia e o ambiente escala sob demanda |
| Resiliência | 3 | O ambiente atinge o RTO/RPO exigido com menos complexidade |
| Custo total de 3 anos | 3 | Menor TCO, incluindo egress, licenças e operação |
| Proximidade de dados e dependências | 3 | Menos tráfego entre ambientes e menor latência |
| Concentração de fornecedor | 2 | Não aumenta a dependência de um único fornecedor |
| Maturidade operacional | 2 | Os times já operam bem esse ambiente |
| Alinhamento ao estado-alvo | 2 | Aproxima o banco do estado-alvo do Enterprise Architect |

## Passo 4 — Desempate e sensibilidade

Se a diferença entre os dois primeiros for menor que 10% do total, a decisão é sensível:
diga quais critérios a decidem e o que mudaria o resultado. Decisão sensível vai para o
Principal Architect com as duas opções.

## Formato de saída

```markdown
## Posicionamento de workload

**Workload:** <nome>
**Matriz aplicada:** <ADR-xxxx aceito> | Matriz inicial — critérios ainda não aceitos pelo comitê
**Resultado:** <Data center | Azure | OCI | AWS>

### Caracterização
<tabela do passo 1>

### Eliminatórios
| Ambiente | Eliminado? | Critério | Motivo |
|---|---|---|---|

### Pontuação
| Critério | Peso | DC | Azure | OCI | AWS |
|---|---|---|---|---|---|
| **Total ponderado** | | | | | |

### Sensibilidade
<o que mudaria o resultado>
```

## Regras

- O critério decide, não a preferência. Se você discorda do resultado, discorde do
  critério e leve ao Enterprise Architect; não ajuste a nota para chegar à resposta.
- Não pontue um ambiente que foi eliminado.
- Custo sem número é "não estimado", não "baixo".

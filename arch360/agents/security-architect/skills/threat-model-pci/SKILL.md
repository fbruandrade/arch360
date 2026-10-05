---
name: threat-model-pci
key: "arch360/security-architect/threat-model-pci"
description: Modelagem de ameaças com classificação de escopo PCI-DSS para ADRs, ARs e HLDs do Escritório. Use quando uma decisão ou desenho tocar dados de portador de cartão, dados pessoais (LGPD) ou credenciais; quando pedirem "modelo de ameaças", "STRIDE", "análise de escopo PCI", "isso entra no CDE?" ou "parecer de segurança"; ou antes de liberar qualquer artefato que mude fluxos de dados entre zonas de confiança.
---

# Modelagem de ameaças com escopo PCI-DSS

Produz duas coisas que andam juntas: o modelo de ameaças do desenho e o efeito dele sobre o
escopo do CDE. Uma sem a outra não serve ao Escritório — segmentação que não reduz escopo
custou esforço sem reduzir exposição.

## Passo a passo

### 1. Delimite o objeto

Escreva em uma frase o que está sendo analisado (o ADR, a AR ou o HLD) e em quais ambientes
ele existe: data center active-active, Azure, OCI, AWS. Se a fronteira não estiver clara,
pare e pergunte ao autor antes de seguir.

### 2. Desenhe o fluxo de dados com zonas de confiança

Liste os elementos: atores externos, processos, armazenamentos, fluxos e as fronteiras de
confiança que cada fluxo cruza (internet → borda, borda → aplicação, aplicação → dados,
on-prem ↔ nuvem, nuvem ↔ nuvem, banco ↔ parceiro). Se houver a skill `archify`, gere o
diagrama de data flow; caso contrário, uma tabela basta.

### 3. Classifique os dados em cada fluxo e armazenamento

| Classe | Exemplos | Regime |
|--------|----------|--------|
| Dados de conta — PAN | número do cartão | PCI-DSS |
| Dados sensíveis de autenticação (SAD) | trilha, CVV, PIN/PIN block | PCI-DSS — não podem ser armazenados após a autorização |
| Dados pessoais | CPF, nome, endereço, contato | LGPD |
| Dados pessoais sensíveis | biometria, saúde | LGPD (regime reforçado) |
| Credenciais e segredos | senhas, tokens, chaves | controle de segurança |

Marque onde cada classe **passa**, **fica** e é **alcançável**.

### 4. Classifique cada componente quanto ao escopo PCI

Use as categorias do guia do PCI SSC sobre escopo e segmentação:

- **CDE** — armazena, processa ou transmite dados de conta, ou está no mesmo segmento sem
  controle que o isole.
- **Conectado ao CDE ou com impacto na segurança dele** — não toca dados de conta, mas
  se conecta ao CDE ou pode afetar sua segurança (identidade, DNS, logging, jump hosts,
  pipelines de deploy, ferramentas de gestão).
- **Fora de escopo** — sem conexão com o CDE, comprovada pelos controles de segmentação.

Um componente só fica fora de escopo se a segmentação que o isola for **verificável**
(regras explícitas, testadas periodicamente). Na dúvida, ele está em escopo; diga isso.

### 5. Aplique STRIDE por elemento e por fluxo

Para cada elemento e cada fluxo que cruza fronteira de confiança, avalie:

| Ameaça | Pergunta |
|--------|----------|
| **S**poofing | Alguém pode se passar por este ator ou serviço? |
| **T**ampering | Os dados podem ser alterados em trânsito ou em repouso? |
| **R**epudiation | Uma ação pode ser negada por falta de registro confiável? |
| **I**nformation disclosure | Os dados podem vazar — por log, cache, backup, réplica, erro? |
| **D**enial of service | O componente pode ser derrubado ou degradado? |
| **E**levation of privilege | Alguém pode ganhar mais acesso do que deveria? |

Nomeie o atacante concreto (insider com acesso administrativo, parceiro comprometido,
atacante externo via API pública, workload vizinho no mesmo cluster) e o caminho. Sem
ameaças genéricas.

### 6. Mapeie para os requisitos do PCI DSS v4.0.1

| Req. | Família |
|------|---------|
| 1 | Controles de segurança de rede |
| 2 | Configurações seguras |
| 3 | Proteção dos dados de conta armazenados |
| 4 | Criptografia forte na transmissão por redes abertas e públicas |
| 5 | Proteção contra software malicioso |
| 6 | Sistemas e software seguros |
| 7 | Restrição de acesso por necessidade de negócio |
| 8 | Identificação de usuários e autenticação |
| 9 | Restrição de acesso físico |
| 10 | Registro e monitoramento de acessos |
| 11 | Testes regulares de segurança |
| 12 | Políticas e programas de segurança |

Cite o requisito específico quando souber; quando não souber, cite a família e marque a
lacuna. Confirme a numeração no documento oficial do PCI SSC antes de liberar.

### 7. Declare o efeito sobre o escopo

Compare com o estado atual e responda uma das três opções, sempre com o porquê:

- **Reduz** o escopo — quais componentes saem e qual controle garante isso.
- **Mantém** o escopo.
- **Amplia** o escopo — quais componentes entram e qual o custo de conformidade disso.

### 8. Controles e exposições remanescentes

Para cada ameaça relevante: o controle exigido (escrito com "deve"), quem implementa
(Cloud Architect na nuvem, Infrastructure Architect on-prem, Technical Architect no
serviço) e, se o controle não puder ser atendido hoje, a exposição remanescente em termos
claros, com dono. Exposição sem controle vai para o Governance Architect como candidata a
waiver.

## Formato de saída

Entregue como seção do artefato ou como documento da issue, em pt-BR:

```markdown
## Modelo de ameaças e escopo PCI-DSS

**Objeto:** <ADR/AR/HLD e ambientes>
**Efeito sobre o escopo do CDE:** Reduz | Mantém | Amplia — <motivo em uma frase>

### Classificação de dados e componentes
| Componente | Ambiente | Dados (passa/fica/alcançável) | Categoria PCI |
|---|---|---|---|

### Ameaças
| # | Elemento/fluxo | STRIDE | Atacante e caminho | Controle exigido | Req. PCI | Dono |
|---|---|---|---|---|---|---|

### Exposições remanescentes
| # | Exposição | Por que não é atendida hoje | Dono | Encaminhamento |
|---|---|---|---|---|

### Lacunas
- <o que não foi possível confirmar e de quem depende>
```

## Regras

- Nunca diga que algo está "fora de escopo" sem nomear o controle de segmentação que
  garante isso.
- Nunca suavize uma exposição. Descreva o que um atacante conseguiria fazer.
- Não reescreva o artefato do outro architect; entregue o parecer e os requisitos.

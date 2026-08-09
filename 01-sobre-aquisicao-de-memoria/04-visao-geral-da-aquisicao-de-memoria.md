# Aquisição de Memória

A fase de aquisição da memória é uma das etapas mais importantes no processo de forense de memória, pois o sucesso de uma análise depende frequentemente de uma aquisição corretamente planejada e executada. Durante o processo de aquisição o investigador tomará decisões importantes sobre quais devem ser coletados e qual será a melhor forma de realizar essa coleta.

O objetivo principal dessa etapa é preservar, tanto quanto possível, o estado do ambiente digital minimizando alterações nas fontes de dados, de forma que o investigador possa realizar inferências confiáveis por meio da análise posterior dos dados coletados. Esse é um princípio amplamente reconhecido na área forense. O Scientific Working Group on Digital Evidence (SWGDE) estabelece como princípio orientador da aquisição forense a minimização, na maior extensão possível, de qualquer alteração nos dados de origem, preservando sua integridade e utilidade para a investigação.

> “O princípio orientador para aquisições forenses computacionais é minimizar, na maior extensão possível, alterações nos dados de origem.” (SWGDE – Best Practices for Computer Forensic Acquisitions, SWGDE 17-F-002-2.1, tradução minha)

Especialmente no que se refere aos ambientes modernos, compostos por diversas tecnologias, como virtualização, computação em nuvem, contêineres, entre outras. O processo geral de aquisição, embora apoiado por um guia conceitual, não deve ser encarado como um procedimento único e imutável que o investigador aplicará a todos os cenários. Durante uma investigação, cada procedimento de aquisição pode ser direcionado de forma completamente diferente, dependendo do ambiente encontrado em campo.

A estratégia de aquisição será conduzida levando em consideração diversos fatores, como o tipo de ambiente (estações de trabalho, servidores, máquinas virtuais, nuvem ou ambientes híbridos), o estado do sistema (em execução, hibernado ou desligado), o nível de acesso disponível ao investigador, as políticas da organização, as tecnologias empregadas (como virtualização e contêineres), o impacto operacional aceitável e os objetivos da investigação. Em muitos casos, parte das evidências de interesse pode já estar disponível por meio de soluções de segurança, monitoramento ou ferramentas de análise, reduzindo ou até eliminando a necessidade de realizar novas aquisições.

Essa diversidade de possíveis cenários evidencia a importância e, muitas vezes, a complexidade que essa fase pode apresentar, além da necessidade de planejamento, tanto do ponto de vista do investigador, que deve possuir a capacidade de se adaptar a diferentes situações, quanto das organizações em geral, no que diz respeito à criação de procedimentos que visem facilitar o planejamento e a coleta. Dessa forma, é possível fornecer maior capacidade aos profissionais para atingir o objetivo principal, que é preservar, tanto quanto possível, a integridade e a utilidade forense das evidências.

O DHS, órgão norte-americano responsável pela segurança interna do país, destaca que a diversidade tecnológica presente nos ambientes modernos representa um desafio significativo para a coleta de evidências. Um exemplo dessa dificuldade é o fato de que diferentes tecnologias frequentemente exigem conhecimentos específicos, mecanismos de coleta distintos e, em alguns casos, podem demandar o apoio dos próprios fabricantes.

> “A diversidade das tecnologias utilizadas nos ambientes modernos de sistemas de controle também apresenta desafios significativos.” (DHS, Recommended Practice: Creating Cyber Forensics Plans for Control Systems, seção 1 — Traditional Forensics and Challenges to Control Systems, subseção 1.1 — Challenges with Collection, tradução minha).

Da mesma forma, o NIST, agência norte-americana responsável pelo desenvolvimento de padrões e diretrizes técnicas amplamente utilizados em segurança da informação e computação forense, destaca a dificuldade inerente ao processo de priorização dos dados para coleta, além da necessidade de as organizações desenvolverem planos que auxiliem e tornem esse processo mais ágil.

> “Em alguns casos, existem tantas fontes de dados possíveis que não é viável realizar a aquisição de todas elas. As organizações devem considerar cuidadosamente as complexidades envolvidas na priorização da aquisição de fontes de dados e desenvolver planos, diretrizes e procedimentos documentados que possam auxiliar os analistas a realizar essa priorização de forma eficaz.” (NIST, Special Publication 800-86, seção 3, subseção 3.1 — Data Collection, subseção 3.1.2 — Acquiring the Data, tradução minha).

> “As políticas, diretrizes e procedimentos da organização devem indicar quaisquer variações em relação ao procedimento padrão.” (NIST, Special Publication 800-86, seção 3 — Performing the Forensic Process, tradução minha).

<br>

Para ilustrar a frequente necessidade de adaptação, Ligh et al., em seu livro “The Art of Memory Forensics”, publicado em 2014, apresentam em seu capítulo “Memory Acquisition Overview” uma árvore de decisão composta por perguntas simples, cujas respostas direcionam o investigador para possíveis caminhos que o procedimento de aquisição pode seguir, demonstrando que pequenas mudanças nas características do ambiente podem alterar significativamente a abordagem adotada durante a coleta.

<br>

```text
Memory
│
├── VM?
│   │
│   ├── Yes
│   │   ├── Snapshot / Pause
│   │   ├── Clone
│   │   ├── Host disk
│   │   └── Introspection
│   │
│   └── No
│       │
│       ├── Running?
│       │   │
│       │   ├── No
│       │   │   ├── Hibernation file
│       │   │   ├── Page file(s)
│       │   │   └── Crash dumps
│       │   │
│       │   └── Yes
│       │       │
│       │       ├── Got root?
│       │       │   │
│       │       │   ├── Yes
│       │       │   │   └── Software
│       │       │   │       ├── Remote?
│       │       │   │       ├── Cost?
│       │       │   │       ├── Format
│       │       │   │       ├── CLI?
│       │       │   │       └── Real-time?
│       │       │   │
│       │       │   └── No
│       │       │       └── Hardware
│       │       │           (*may require pre-installation)
│       │       │
│       │       │           ├── RAM > 4 GB?
│       │       │           │   │
│       │       │           │   ├── Yes → PCI (**very expensive)
│       │       │           │   └── No  → FireWire
```
<br>

Cada escolha direciona o procedimento para um caminho específico de aquisição. 

No caso de um sistema bare-metal, por exemplo, essa árvore simplificada poderia ser apresentada da seguinte forma, considerando apenas o estado do sistema e a disponibilidade de privilégios administrativos:

<br>

```text
Bare-metal
│
├── Sistema em execução?
│   │
│   ├── Não
│   │   ├── Hibernation file
│   │   ├── Page file(s)
│   │   └── Crash dumps
│   │
│   └── Sim
│       │
│       ├── Possui privilégios administrativos?
│       │   │
│       │   ├── Sim → Aquisição por software
│       │   │
│       │   └── Não → Aquisição por hardware
│       │                ├── FireWire
│       │                └── PCIe (casos específicos)
```

<br>

Como mencionado, o contexto atual da aquisição de memória (2026) agrega diversas novas tecnologias que passaram a fazer parte do cotidiano de muitas organizações, ampliando tanto as possibilidades quanto a complexidade envolvida nesse processo. Adaptando os modelos apresentados anteriormente ao contexto moderno, o processo poderia ser representado de forma simplificada, conforme a figura abaixo.

<br>

```text
Aquisição de Memória (DFIR Moderno)

↓

Identificar o ambiente operacional
│
├── Bare-metal
│   │
│   ├── Sistema desligado
│   │   ├── Hiberfil.sys
│   │   ├── Pagefile / Swap
│   │   ├── Crash Dumps
│   │   └── Outros artefatos persistentes
│   │
│   └── Sistema em execução
│       │
│       ├── Aquisição por software
│       └── Aquisição assistida por hardware (casos específicos)
│
├── Máquina Virtual
│   │
│   ├── Guest
│   │   └── Memory Dump
│   │
│   └── Hypervisor
│       │
│       ├── Aquisição
│       │   ├── VM Memory Dump
│       │   └── Snapshot com memória
│       │
│       └── Virtual Machine Introspection (VMI)
│
├── Cloud
│   │
│   ├── Guest
│   │   └── Memory Dump
│   │
│   ├── Plataforma Cloud
│   │   └── Serviços nativos
│   │
│   └── Artefatos da plataforma
│
└── Containers
│
├── Host
│   └── Memory Dump
│
├── Container
│   ├── Checkpoint (CRIU)
│   └── Runtime artifacts
│
└── Orquestrador
```
<br>

O propósito principal deste capítulo é demonstrar que a aquisição é um processo orientado por decisões e, portanto, possui maior probabilidade de alcançar seu objetivo quando é devidamente planejado. As árvores de decisão apresentadas acima demonstram que cada resposta pode modificar, entre outros aspectos, quais evidências devem ser coletadas, a estratégia de coleta e quais ferramentas devem ser empregadas.

Diante disso, a aquisição não deve começar pela execução de uma ferramenta, mas pelo planejamento do processo de coleta. O NIST, por exemplo, sugere que o processo de coleta seja conduzido em três etapas principais: inicialmente, o planejamento da aquisição; em seguida, a execução da coleta propriamente dita; e, por fim, a verificação da integridade dos dados.

O planejamento consiste em identificar e priorizar as possíveis fontes de evidência, considerando principalmente:

- o valor potencial das evidências;
- a volatilidade das informações;
- o esforço necessário para sua aquisição.

Após isso, o investigador deve executar a aquisição propriamente dita, seja localmente, de forma remota ou por outros meios, de acordo com o cenário em análise. A etapa de verificação da integridade dos dados adquiridos deve ser realizada imediatamente após a coleta, normalmente por meio de funções criptográficas de hash, garantindo que as evidências não sofreram alterações durante o processo.

> “Após identificar as possíveis fontes de dados, o analista precisa adquirir os dados dessas fontes. A aquisição de dados deve ser realizada utilizando um processo composto por três etapas: desenvolver um plano para aquisição dos dados, realizar a aquisição dos dados e verificar a integridade dos dados adquiridos.” (NIST, Special Publication 800-86, seção 3 — Performing the Forensic Process, subseção 3.1 — Data Collection, subseção 3.1.2 — Acquiring the Data, tradução minha).

> “Verificar a integridade dos dados. Após a aquisição dos dados, sua integridade deve ser verificada. É particularmente importante que o analista consiga demonstrar que os dados não foram adulterados caso eles possam ser necessários para fins legais. A verificação da integridade dos dados normalmente consiste na utilização de ferramentas para calcular o resumo criptográfico (message digest) dos dados originais e dos dados copiados, comparando posteriormente os valores obtidos para garantir que sejam iguais.” (NIST, Special Publication 800-86, seção 3 — Performing the Forensic Process, subseção 3.1 — Data Collection, subseção 3.1.2 — Acquiring the Data, tópico 3 — Verify the Integrity of the Data, tradução minha).

<br>

Sob uma perspectiva moderna, esse processo de planejamento pode ser visualizado como um conjunto de perguntas que orientam a estratégia de aquisição:

Qual é o ambiente? O sistema é físico, virtual, em nuvem ou containerizado? De que forma ele está estruturado em um cenário híbrido?

Existem políticas ou restrições da organização que podem influenciar a aquisição?

Qual é o impacto operacional aceitável?

Quais artefatos precisam ser coletados?

A aquisição será realizada local? remota?

Qual é o estado atual do sistema? O sistema está ligado, desligado, suspenso ou em processo de inicialização/desligamento?

Quais ferramentas estão disponíveis e são mais adequadas em relação ao ambiente?

<br>

A aquisição de memória é uma atividade orientada por decisões, por isso, o investigador deve compreender o ambiente, identificar as restrições existentes, selecionar os métodos mais adequados e preservar a integridade das evidências. Não existe um procedimento universal, mas sim um processo de adaptação fundamentado em princípios forenses e nas características específicas de cada investigação.

## Porque adquirir a memória?

Por muitos anos, a computação forense concentrou-se principalmente na preservação do conteúdo persistente dos dispositivos de armazenamento. O procedimento de aquisição de evidências digitais normalmente aceito envolvia desligar o sistema e realizar uma cópia duplicada dos dados armazenados em disco para posterior análise. Esse procedimento baseava-se no entendimento de que o armazenamento persistente era uma fonte suficiente e também a mais confiável para a maioria das investigações. Além disso, naquele contexto, as técnicas de aquisição e análise de memória eram relativamente menos sofisticadas do que são atualmente.

Apoiados no entendimento de que qualquer interação com um sistema em execução altera, em algum grau, seu estado, os investigadores priorizavam a aquisição do armazenamento não volátil (disco rígido), abrindo mão da preservação de outras fontes de evidência, como a memória física (RAM). Assumia-se que essa era a melhor maneira de preservar a integridade das evidências, isto é, minimizar as alterações no armazenamento persistente, mesmo que isso significasse abrir mão dos dados disponíveis apenas na memória volátil e em outros componentes dependentes do estado de execução do sistema.

Com o tempo, tornou-se evidente que a análise de artefatos que não são gravados no armazenamento permanente e que permanecem disponíveis apenas enquanto o sistema está em funcionamento ou em determinadas condições de execução era essencial para aumentar a confiabilidade das inferências acerca do ocorrido em um sistema. Isso se tornou ainda mais importante em razão da evolução dos sistemas operacionais, de tecnologias como virtualização e criptografia e, principalmente, das técnicas utilizadas por atacantes, cujos ataques tornaram-se cada vez mais sofisticados.

> "Com o tempo, tornou-se evidente que preservar seletivamente determinadas evidências em detrimento de outras comprometia a confiabilidade das inferências produzidas durante a investigação."
>
> — Ligh et al. (2014, tradução minha)

A abordagem atual, portanto, objetiva obter uma visão mais completa do estado do sistema no momento da coleta por meio da aquisição e correlação de artefatos provenientes de múltiplas fontes, capacidade que a abordagem anterior, baseada na preservação e análise apenas do disco rígido, fornecia de forma limitada.

Como mencionado, o processo de preservação inevitavelmente altera, em algum grau, o estado do sistema, visto que o próprio software utilizado para capturar essas evidências consome recursos e modifica pequenas porções da memória durante o processo. Esse impacto torna-se ainda mais evidente quando a abordagem deixa de consistir apenas no desligamento do sistema para preservar o armazenamento não volátil e passa a mantê-lo em execução para realizar a aquisição de evidências voláteis. Apesar disso, o benefício obtido pela preservação dessas evidências normalmente supera, em muito, o pequeno impacto introduzido pelo processo de aquisição, tornando esse custo inevitável amplamente aceito pela comunidade forense. A partir desse entendimento, a principal preocupação passa a ser conhecer e documentar essas alterações, utilizando procedimentos e ferramentas adequados para minimizar ao máximo seu impacto.

Os benefícios desse processo de aquisição, realizado enquanto o sistema permanece em execução, consistem na preservação de informações extremamente relevantes que seriam perdidas após um desligamento ou reinicialização. A obtenção e a correlação de artefatos provenientes de diferentes fontes de evidência permitem reconstruir os eventos ocorridos de forma mais completa do que seria possível por meio da análise exclusiva do armazenamento persistente.

> "Ao correlacionar dados provenientes de múltiplas fontes (disco, rede, memória etc.) dentro do ambiente digital, é possível obter uma compreensão mais abrangente do que ocorreu no sistema do que a perspectiva limitada fornecida apenas pelo conteúdo do armazenamento em disco."
>
> — Ligh et al. (2014, tradução minha)

Algumas dessas informações incluem:

- processos em execução e suas relações;
- código malicioso residente apenas em memória (*fileless malware* e diversos tipos de *rootkits*);
- módulos, DLLs e drivers carregados;
- conexões de rede ativas e seus respectivos processos;
- comandos executados recentemente;
- chaves criptográficas e material criptográfico em uso;
- credenciais, *tokens* de autenticação e sessões autenticadas;
- conteúdo descriptografado de arquivos ou aplicações protegidas por criptografia;
- regiões de memória injetadas por malware;
- objetos do kernel e outras estruturas internas do sistema operacional;
- informações úteis para reconstrução da linha do tempo do incidente.

A preservação dessas informações amplia de forma significativa a quantidade e a qualidade das evidências disponíveis ao investigador, permitindo inferências mais confiáveis sobre o ocorrido no sistema, os processos envolvidos, os recursos acessados e as ações executadas no momento da aquisição.

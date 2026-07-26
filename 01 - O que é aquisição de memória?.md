## O que é aquisição de memória?

"Fundamentalmente, a aquisição de memória é o processo de copiar o conteúdo da memória física para outro dispositivo de armazenamento, com o objetivo de preservá-lo" (Ligh et al., 2014, tradução minha).

Em uma investigação forense digital, o investigador busca preservar o estado do ambiente digital analisado, tomando decisões importantes sobre quais dados devem ser coletados e sobre a melhor forma de realizar essa coleta, de maneira que a evidência não seja corrompida, sua integridade e validade sejam preservadas e a análise posterior permita chegar a conclusões confiáveis.

É importante destacar que, no contexto moderno da aquisição de memória, o termo não se refere apenas à aquisição da memória física (RAM), mas também a outros artefatos produzidos pelo sistema operacional, mecanismos de virtualização ou eventos de falha que podem preservar completa ou parcialmente o conteúdo da memória física.

Esses artefatos são frequentemente fontes relevantes de evidências e, dependendo do cenário investigado, podem representar a única forma viável de recuperar informações que estavam presentes na memória em determinado momento. Alguns dos principais são:

- **Memória física (RAM):** fonte primária e geralmente mais completa do estado da memória do sistema.
- **Arquivos de paginação (pagefile.sys / swap):** armazenam páginas de memória que foram movidas da memória física para o armazenamento persistente.
- **Arquivos de hibernação (hiberfil.sys):** preservam informações da memória durante o processo de hibernação do sistema.
- **Crash dumps:** registram parte ou todo o conteúdo da memória após uma falha do sistema.
- **Snapshots de máquinas virtuais:** capturam o estado de uma máquina virtual, incluindo informações presentes na memória.
- **Memory dumps produzidos pelo hipervisor:** capturam diretamente a memória de uma máquina virtual, independentemente do sistema operacional convidado.

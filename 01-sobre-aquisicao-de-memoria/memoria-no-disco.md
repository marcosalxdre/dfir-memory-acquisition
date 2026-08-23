# Artefatos de Memória no Disco na Análise Forense

O livro *The Art of Memory Forensics*, de Ligh et al., escrito em 2014, em seu capítulo sobre aquisição forense de memória, destaca a importância da aquisição de fontes que podem conter conteúdo da memória armazenado no disco, resumidamente da seguinte forma:

> **Memória volátil no disco:** É necessário que o investigador esteja ciente da possibilidade de coletar dados voláteis alternativos gravados no armazenamento persistente, os quais são artefatos produzidos como resultado do funcionamento normal do sistema. Em alguns casos, esses registros da memória salvos no armazenamento não volátil podem ser a única fonte de dados sobre determinado caso disponível.
> 
> Além disso, esses artefatos podem atribuir mais confiabilidade às inferências derivadas por meio de sua correlação com a imagem coletada, pois podem conter evidências registradas em períodos de tempo além do que foi possível capturar com o dump da RAM.

Embora o livro tenha sido escrito há mais de dez anos — um período longo no que se refere à tecnologia —, a importância desses artefatos para a investigação forense continua relevante. É claro que o comportamento dos sistemas e a forma como esses artefatos são gerados e armazenados evoluíram ao longo do tempo. Por isso, este material busca apresentar uma visão geral e atualizada do tema, além de reunir fontes e referências para consulta e aprofundamento.

Esses artefatos incluem:

* `Hiberfil.sys`
* `Pagefile.sys`
* `Swapfile.sys`
* `Crash dumps`

A seguir, cada um deles será abordado brevemente, com foco em sua função e relevância para a análise forense.

---

## Hiberfil.sys

O `hiberfil.sys` é um arquivo oculto reservado pelo Windows para dar suporte à infraestrutura de hibernação do sistema e, a partir do Windows 8, também ao **Fast Startup / Hybrid Boot**. Se a funcionalidade estiver habilitada, o arquivo de hibernação existirá na raiz da unidade onde o sistema operacional está instalado: `\hiberfil.sys`.

Dependendo de qual dessas funcionalidades é acionada e também da configuração/versão do sistema, o Windows grava no `hiberfil.sys` os dados necessários para preservar e restaurar o estado do sistema de acordo com o objetivo desejado. Para cada uma dessas funcionalidades, o conteúdo salvo pode ser:

* **Hibernation (Hibernação):** O sistema hiberna, comprimindo e gravando no `hiberfil.sys` as páginas e o estado de memória necessários para restaurar a sessão ao estado em que estava antes da hibernação.
* **Fast Startup (Inicialização Rápida / Hybrid Boot / Hybrid Shutdown):** O Fast Startup utiliza um *hiberfile* reduzido, encerrando as aplicações e as sessões dos usuários e gravando a imagem do kernel e os drivers carregados para acelerar a próxima inicialização.

### Coleta
A aquisição de uma imagem de hibernação a partir de um sistema em execução pode ser complexa e pouco prática. Em geral, a abordagem mais comum e confiável é realizar a aquisição com o sistema desligado (*"dead box"*), preservando o arquivo `hiberfil.sys` armazenado em disco. A escolha do método e da ordem de aquisição é, naturalmente, singular a cada procedimento.

Mecanismos de criptografia do disco, como o BitLocker, e o bloqueio do sistema operacional podem impedir ou limitar a análise offline do arquivo sem acesso às respectivas chaves ou mecanismos de recuperação. Além disso, o Windows não mantém um histórico de versões do `hiberfil.sys`; novos ciclos podem substituir o conteúdo anterior.

### Valor Forense
Dependendo do modo utilizado, da versão do Windows e dos dados que estejam presentes no arquivo no momento da coleta, a preservação e análise do `hiberfil.sys` pode fornecer ao investigador informações como:

* Processos em execução;
* Estruturas do kernel;
* Drivers e módulos carregados;
* Conexões de rede;
* Objetos e handles;
* Comandos e dados presentes na memória;
* Conteúdo de documentos ou aplicativos que estava residente na memória;
* Credenciais ou material criptográfico eventualmente presente na memória;
* Malware residente na memória;
* Artefatos relacionados à atividade do usuário.

---

## Arquivos de Paginação (pagefile.sys / swap)

O `pagefile.sys` é um arquivo oculto do Windows usado como apoio aos mecanismos de gerenciamento de memória virtual, sustentação de memória comprometida e também pode servir como apoio à gravação de *crash dumps*. Embora seu uso esteja a cargo do *Memory Manager*, sua utilização mais comum (ou pelo menos mais conhecida) ocorre quando o Windows o utiliza como uma "extensão" da memória RAM.

Esse processo envolve mover páginas da memória física para o disco, permitindo que o sistema utilize a memória física de forma mais eficiente para manter em RAM as páginas acessadas com maior frequência. Esse processo é conhecido como **paging**. Embora o arquivo não represente uma captura completa nem ordenada da RAM, ele pode conter páginas ou fragmentos de memória virtual anteriormente presentes na RAM, preservando dados sensíveis mesmo após a perda da memória volátil.

A localização padrão do arquivo é `%SystemDrive%\pagefile.sys`, embora o Windows possa utilizar mais de um arquivo ou armazená-lo em outra unidade. Como o `pagefile.sys` é armazenado em disco, seu conteúdo pode persistir após desligamentos ou reinicializações, embora essa persistência não seja garantida. Isso dependerá inteiramente do contexto do sistema em questão. A política *"Shutdown: Clear virtual memory pagefile"*, por exemplo, pode fazer com que o Windows limpe o `pagefile.sys` durante o desligamento normal, reduzindo a possibilidade de existirem dados históricos no arquivo.

### Coleta
Diferentemente de uma imagem convencional da RAM "limpa", o `pagefile.sys` não fornece diretamente uma representação clara e completa do estado da memória. A análise do arquivo pode envolver técnicas como *carving*, busca por *strings*, reconstrução de estruturas e correlação com *dumps* de memória e outros artefatos. Além disso, assim como o arquivo `hiberfil.sys`, o volume que contém o `pagefile.sys` também pode estar protegido por mecanismos de criptografia, como o BitLocker.

### Valor Forense
* Fragmentos de chats;
* Credenciais;
* Documentos não salvos;
* E-mails;
* Arquivos de mídia;
* Payloads e artefatos de malware;
* Dados descriptografados que estavam presentes na memória.

---

## Swapfile.sys (Windows 8+)

A partir do Windows 8, o arquivo `swapfile.sys` foi introduzido para apoiar o gerenciamento de memória das aplicações Windows. Especialmente em cenários de suspensão e retomada de aplicações, o Windows pode utilizá-lo como um arquivo de troca adicional, salvando nele dados privados que o aplicativo mantém na RAM, liberando essa memória para outros usos. Quando o aplicativo volta a ser executado, seus dados podem ser recuperados.

O arquivo não necessariamente possui uma captura completa do processo. Seu conteúdo dependerá da versão do Windows, das aplicações utilizadas e do gerenciamento realizado pelo *Memory Manager*. O *swapfile*, portanto, pode complementar, mas não substituir o *pagefile* ou um *dump* da RAM.

O arquivo existe, por padrão, no diretório raiz onde o sistema operacional está instalado: `\swapfile.sys`.

---

## Crash Dump Files

Os *Crash Dumps* são "snapshots" de memória gerados pelo Windows quando uma falha inesperada faz o sistema parar de funcionar (*bug check/Stop error*). A Microsoft reconhece cinco tipos principais, cuja principal diferença está na quantidade e no tipo de informação armazenada:

* **Complete Memory Dump**
* **Kernel Memory Dump**
* **Small Memory Dump**
* **Automatic Memory Dump**
* **Active Memory Dump**

Embora esses *dumps* tenham como finalidade principal o diagnóstico de falhas do Windows, eles também podem ser configurados ou gerados deliberadamente para fins de diagnóstico. Sua disponibilidade é oportunística, mas, quando presentes, podem fornecer informações relevantes para uma investigação forense.

### Localização Padrão
* **Complete, Kernel, Automatic e Active Memory Dump:** `%SystemRoot%\Memory.dmp`
* **Small Memory Dump:** `%SystemRoot%\Minidump`

Esses caminhos, assim como o tipo de *dump* configurado, podem ser alterados por meio das configurações do sistema. Essas configurações são armazenadas no Registro, principalmente na chave:
`HKLM\SYSTEM\CurrentControlSet\Control\CrashControl`

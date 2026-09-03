# COMO ADQUIRIR MEMÓRIA?

**(2026)**

## 1. Aquisição baseada em software

- A memória é adquirida por uma ferramenta executada no próprio sistema operacional, normalmente com os privilégios necessários para acessar a memória física e, dependendo da implementação, utilizando componentes executados em modo kernel.
- É uma das abordagens mais utilizadas em investigações forenses e resposta a incidentes.
- Exemplos: WinPmem, DumpIt e AVML.

### A aquisição pode ser realizada:

- **Localmente:** a ferramenta é executada diretamente no sistema, gravando o dump preferencialmente em uma mídia removível ou compartilhamento de rede.
- **Remotamente:** a ferramenta é acionada por um investigador ou plataforma de gerenciamento remoto (por exemplo, EDR, SSH ou PowerShell Remoting, via PsExec ou SMB), mas continua sendo executada no sistema alvo. Apenas o mecanismo de controle é diferente, o princípio técnico é o mesmo da aquisição local.

### 1.1 Aquisição por mecanismos nativos do sistema operacional

- O próprio sistema operacional pode gerar artefatos que contenham informações presentes na memória, sem a necessidade de uma ferramenta forense de aquisição de terceiros.
- São resultados de mecanismos normalmente destinados a diagnóstico, recuperação de falhas ou gerenciamento do sistema, mas seus artefatos podem possuir valor forense.
- A aquisição pode ocorrer como consequência de eventos específicos, como falhas do sistema, hibernação ou procedimentos de diagnóstico.

**Exemplos:**

- Windows: kernel e complete crash dumps e hibernation files (hiberfil.sys).
- Linux: kdump/vmcore e mecanismos de crash dump do kernel.
- Outros sistemas operacionais podem disponibilizar mecanismos equivalentes.

## 2. Aquisição por hipervisor (Virtual Machine Acquisition)

- Em hypervisors que fornecem essa funcionalidade, a memória é obtida diretamente através da infraestrutura de virtualização, sem a necessidade de executar uma ferramenta dentro da máquina virtual.
- Exemplos incluem mecanismos de saved state, suspend state, snapshots que preservam o estado de execução e dumps de memória disponibilizados pela plataforma de virtualização.

### A aquisição pode ser realizada:

- **Localmente:** a operação é realizada diretamente no host que executa a máquina virtual, utilizando os mecanismos de aquisição disponibilizados pelo hypervisor.
- **Remotamente:** a operação é acionada pelo investigador por meio das interfaces de gerenciamento remoto da infraestrutura de virtualização, apenas o mecanismo de controle é diferente.

**Exemplos:** mecanismos de saved state/suspend state e dumps de memória disponibilizados por VMware, Hyper-V e KVM.

## 3. Aquisição em ambientes de nuvem (Cloud Memory Acquisition)

- O provedor pode disponibilizar mecanismos de gerenciamento, interrupção, diagnóstico, snapshot de armazenamento e APIs que auxiliam a aquisição.
- A disponibilidade de uma aquisição direta da RAM pelo provedor/hypervisor depende da plataforma e não deve ser presumida.
- A execução de ferramentas de aquisição diretamente na própria instância é uma das abordagens mais comuns para aquisição de memória em ambientes de nuvem, além dos mecanismos de aquisição ou diagnóstico fornecidos pela própria plataforma. 
- Pode envolver ferramentas executadas na própria instância, mecanismos de crash dump, interrupções de diagnóstico, snapshots de armazenamento (permitindo recuperar um Memory.dmp que já tenha sido produzido pelo sistema, por exemplo), recursos de hibernação e APIs ou serviços específicos da plataforma.
- O método disponível depende da arquitetura e dos recursos oferecidos pelo provedor (AWS, Azure, GCP).

## 4. Aquisição baseada em hardware (Hardware-Based Acquisition)

- A tentativa de aquisição é realizada por um mecanismo externo ao sistema operacional, utilizando interfaces como DMA, FPGA ou mecanismos de depuração (JTAG, DCI).
- Atualmente é uma técnica especializada, empregada principalmente em pesquisa, análise de malware avançado e alguns laboratórios forenses.
- Seu uso tornou-se mais restrito devido às proteções presentes nos sistemas modernos, como IOMMU, Kernel DMA Protection e tecnologias de criptografia de memória.

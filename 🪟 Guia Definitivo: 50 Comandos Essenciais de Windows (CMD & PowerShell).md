# 🪟 Guia Definitivo: 50 Comandos Essenciais de Windows (CMD & PowerShell)

Este guia divide os comandos entre navegação clássica, administração de sistema e ferramentas exclusivas do PowerShell.

---

## 📂 1. Navegação e Gestão de Arquivos (CMD/PS)
| Comando | Descrição |
| :--- | :--- |
| `dir` | Lista arquivos e pastas no diretório atual |
| `cd [caminho]` | Altera o diretório (Ex: `cd ..` para voltar) |
| `mkdir [nome]` | Cria uma nova pasta |
| `type [arquivo]` | Exibe o conteúdo de um arquivo de texto no terminal |
| `copy [origem] [destino]` | Copia arquivos para outro local |
| `move [origem] [destino]` | Move ou renomeia arquivos/pastas |
| `del [arquivo]` | Exclui um ou mais arquivos |
| `rmdir /s [pasta]` | Remove uma pasta e todo o seu conteúdo |
| `ren [antigo] [novo]` | Renomeia um arquivo ou diretório |
| `attrib` | Exibe ou altera atributos de arquivos (Oculto, Somente Leitura) |

---

## ⚙️ 2. Informações e Gestão do Sistema
| Comando | Descrição |
| :--- | :--- |
| `systeminfo` | Exibe detalhes do hardware, BIOS e versão do Windows |
| `tasklist` | Lista todos os processos e serviços em execução |
| `taskkill /F /PID [id]` | Força o encerramento de um processo pelo ID |
| `taskkill /IM "chrome.exe"` | Encerra um processo pelo nome da imagem |
| `wmic cpu get name` | Consulta informações específicas via WMI (Ex: modelo da CPU) |
| `shutdown /s /t 0` | Desliga o computador imediatamente |
| `shutdown /r /t 0` | Reinicia o computador imediatamente |
| `ver` | Exibe a versão exata do Windows instalada |
| `driverquery` | Lista todos os drivers instalados e seu status |

---

## 🌐 3. Rede e Conectividade
| Comando | Descrição |
| :--- | :--- |
| `ipconfig` | Exibe endereços IP, Máscara e Gateway |
| `ipconfig /flushdns` | Limpa o cache do resolvedor DNS |
| `ping [host]` | Testa a conectividade com um servidor/site |
| `tracert [host]` | Rastreia a rota de pacotes até o destino |
| `netstat -an` | Exibe todas as conexões de rede e portas abertas |
| `nslookup [dominio]` | Consulta servidores DNS para obter o IP de um domínio |
| `getmac` | Exibe o endereço físico (MAC) das placas de rede |
| `net share` | Lista todos os recursos compartilhados na rede local |

---

## 🛠️ 4. Ferramentas de Disco e Diagnóstico
| Comando | Descrição |
| :--- | :--- |
| `chkdsk /f` | Verifica e corrige erros no disco rígido |
| `sfc /scannow` | Verifica a integridade de todos os arquivos do sistema |
| `diskpart` | Abre o utilitário de particionamento de disco (Poderoso!) |
| `format [unidade]:` | Formata uma unidade de disco (Ex: `format d:`) |
| `defrag [unidade]:` | Otimiza e desfragmenta o disco rígido |

---

## 🚀 5. Exclusivos do PowerShell (Cmdlets)
| Comando | Descrição |
| :--- | :--- |
| `Get-Service` | Lista todos os serviços do Windows e seu status |
| `Start-Service [nome]` | Inicia um serviço parado |
| `Stop-Service [nome]` | Para um serviço em execução |
| `Get-Process` | Versão avançada do tasklist (permite filtros) |
| `Get-Content [arquivo]` | Similar ao `type`, mas com suporte a objetos |
| `Set-ExecutionPolicy` | Define permissões para rodar scripts .ps1 |
| `Invoke-WebRequest [url]` | Faz requisições HTTP (similar ao cURL) |
| `Expand-Archive` | Descompacta arquivos .zip via comando |
| `Compress-Archive` | Compacta arquivos em um .zip |
| `Clear-History` | Limpa o histórico de comandos do terminal |

---

## 📦 6. Gestão de Pacotes e Atalhos
| Comando | Descrição |
| :--- | :--- |
| `winget install [nome]` | Gerenciador de pacotes oficial (Instala programas via terminal) |
| `winget upgrade --all` | Atualiza todos os programas instalados de uma vez |
| `cls` | Limpa a tela do terminal |
| `exit` | Fecha o terminal |
| `help [comando]` | Exibe ajuda para um comando específico do CMD |
| `Get-Help [comando]` | Exibe ajuda detalhada para Cmdlets do PowerShell |
| `echo %VAR%` | Exibe o valor de uma variável de ambiente (no CMD) |
| `echo $env:VAR` | Exibe o valor de uma variável de ambiente (no PS) |
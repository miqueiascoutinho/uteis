# 🐧 Guia Definitivo: 50 Comandos Essenciais de Linux & Bash

O terminal é a ferramenta mais poderosa do Linux. Este guia cobre desde a navegação básica até a automação com Shell Script.

---

## 📂 1. Navegação e Gestão de Arquivos
| Comando | Descrição |
| :--- | :--- |
| `ls -la` | Lista todos os arquivos (incluindo ocultos) com detalhes |
| `cd [diretorio]` | Entra em uma pasta específica |
| `pwd` | Exibe o caminho completo do diretório atual |
| `mkdir [nome]` | Cria uma nova pasta |
| `rm [arquivo]` | Remove um arquivo |
| `rm -rf [pasta]` | **CUIDADO:** Remove uma pasta e tudo dentro dela à força |
| `cp [origem] [destino]` | Copia arquivos ou pastas |
| `mv [origem] [destino]` | Move ou renomeia arquivos ou pastas |
| `touch [arquivo]` | Cria um arquivo vazio ou atualiza a data de acesso |
| `ln -s [alvo] [nome]` | Cria um link simbólico (atalho) |

---

## 📝 2. Visualização e Edição de Texto
| Comando | Descrição |
| :--- | :--- |
| `cat [arquivo]` | Exibe todo o conteúdo do arquivo no terminal |
| `less [arquivo]` | Abre o arquivo para leitura permitindo rolar (melhor para arquivos grandes) |
| `head -n 10` | Exibe as primeiras 10 linhas de um arquivo |
| `tail -f [arquivo]` | Exibe as últimas linhas e acompanha novas adições em tempo real (logs) |
| `grep "termo" [arq]` | Busca por um texto específico dentro de um arquivo |
| `nano` / `vim` | Editores de texto direto no terminal |
| `sed -i 's/velho/novo/g' [arq]` | Substitui texto dentro de um arquivo via linha de comando |
| `awk '{print $1}' [arq]` | Processa e extrai colunas específicas de um texto |
| `sort` / `uniq` | Ordena linhas e remove duplicatas |

---

## ⚙️ 3. Gestão do Sistema e Processos
| Comando | Descrição |
| :--- | :--- |
| `top` / `htop` | Monitor de processos e recursos do sistema (CPU/RAM) |
| `ps aux` | Lista todos os processos rodando no sistema |
| `kill -9 [PID]` | Força o encerramento de um processo pelo seu ID |
| `df -h` | Exibe o uso de espaço em disco em formato legível (GB/MB) |
| `du -sh [pasta]` | Exibe o tamanho total ocupado por uma pasta específica |
| `free -m` | Exibe o uso de memória RAM em Megabytes |
| `uptime` | Mostra há quanto tempo o servidor está ligado |
| `uname -a` | Exibe informações do Kernel e da versão do Linux |

---

## 🔐 4. Permissões e Usuários
| Comando | Descrição |
| :--- | :--- |
| `chmod +x [arquivo]` | Torna um arquivo executável (essencial para scripts) |
| `chmod 777 [arquivo]` | Dá permissão total (leitura, escrita, execução) a todos |
| `chown user:group [arq]` | Altera o dono e o grupo de um arquivo ou pasta |
| `sudo [comando]` | Executa um comando com privilégios de administrador (root) |
| `whoami` | Exibe o nome do usuário logado no momento |
| `passwd` | Altera a senha do usuário atual |

[Image of Linux file permissions diagram rwx]

---

## 🌐 5. Rede e Conectividade
| Comando | Descrição |
| :--- | :--- |
| `ip addr` | Exibe os endereços IP das
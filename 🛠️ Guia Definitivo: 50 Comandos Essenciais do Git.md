# 🛠️ Guia Definitivo: 50 Comandos Essenciais do Git

Um guia de referência rápida para dominar o controle de versão, do básico ao avançado.

---

## 🏁 1. Configuração e Início
| Comando | Descrição |
| :--- | :--- |
| `git init` | Inicializa um novo repositório Git local |
| `git clone [url]` | Baixa um projeto existente e seu histórico de versões |
| `git config --global user.name "[nome]"` | Define o nome que será exibido nos seus commits |
| `git config --global user.email "[email]"` | Define o e-mail que será vinculado aos seus commits |
| `git config --global core.editor [editor]` | Define o editor de texto padrão (ex: code, vim) |
| `git help [comando]` | Exibe a documentação detalhada de um comando |

---

## 📝 2. Fluxo de Trabalho (Stage & Commit)
| Comando | Descrição |
| :--- | :--- |
| `git status` | Lista os arquivos alterados e os que estão no Stage |
| `git add [arquivo]` | Adiciona um arquivo específico ao Stage (prepara para commit) |
| `git add .` | Adiciona TODOS os arquivos alterados ao Stage |
| `git commit -m "[mensagem]"` | Grava o snapshot do Stage permanentemente no histórico |
| `git commit --amend` | Altera o último commit (mensagem ou arquivos esquecidos) |
| `git restore --staged [arquivo]` | Remove um arquivo do Stage, mas mantém as alterações |
| `git rm [arquivo]` | Remove o arquivo do disco e do controle do Git |
| `git mv [origem] [destino]` | Renomeia ou move um arquivo e já prepara o stage |

---

## 🌿 3. Branches (Ramificações)
| Comando | Descrição |
| :--- | :--- |
| `git branch` | Lista todas as branches locais |
| `git branch -a` | Lista todas as branches (locais e remotas) |
| `git branch [nome]` | Cria uma nova branch |
| `git checkout [nome]` | Muda para a branch especificada |
| `git checkout -b [nome]` | Cria uma nova branch e já muda para ela |
| `git merge [nome]` | Une o histórico da branch especificada à branch atual |
| `git branch -d [nome]` | Exclui a branch (se já foi feito o merge) |
| `git branch -D [nome]` | Força a exclusão de uma branch, mesmo sem merge |
| `git switch [nome]` | Alterna entre branches (forma moderna do checkout) |

---

## 🌍 4. Sincronização Remota
| Comando | Descrição |
| :--- | :--- |
| `git remote add origin [url]` | Conecta seu repositório local a um servidor remoto |
| `git remote -v` | Lista os servidores remotos configurados |
| `git fetch` | Baixa o histórico do remoto, mas não altera seus arquivos |
| `git pull` | Baixa as novidades e já faz o merge na branch atual |
| `git push origin [branch]` | Envia seus commits locais para o servidor remoto |
| `git push -u origin [branch]` | Envia e define a branch como padrão para futuros pushes |
| `git remote show origin` | Exibe informações detalhadas sobre o repositório remoto |

---

## 🔍 5. Inspeção e Histórico
| Comando | Descrição |
| :--- | :--- |
| `git log` | Exibe o histórico de commits da branch atual |
| `git log --oneline` | Exibe o histórico resumido em uma única linha por commit |
| `git log --graph --all` | Exibe o histórico com uma visualização gráfica de branches |
| `git diff` | Mostra as alterações nos arquivos que ainda não foram para o stage |
| `git diff --staged` | Mostra as alterações entre o Stage e o último commit |
| `git show [commit]` | Exibe os detalhes e mudanças de um commit específico |
| `git blame [arquivo]` | Mostra quem alterou cada linha de um arquivo e quando |
| `git reflog` | Mostra o histórico de todas as ações (útil para recuperar deletados) |

---

## 🧹 6. Limpeza e Desfazendo Alterações
| Comando | Descrição |
| :--- | :--- |
| `git checkout -- [arquivo]` | Descarta as alterações locais em um arquivo (volta ao último commit) |
| `git reset [arquivo]` | Remove o arquivo do Stage sem alterar o código |
| `git reset --soft [commit]` | Volta o histórico para um commit, mantendo as alterações no Stage |
| `git reset --hard [commit]` | **CUIDADO:** Volta para um commit apagando todas as mudanças locais |
| `git revert [commit]` | Cria um novo commit que reverte as mudanças de um commit anterior |
| `git clean -df` | Remove arquivos e pastas não rastreados (que não estão no .gitignore) |

---

## 📦 7. Comandos Avançados (Pro)
| Comando | Descrição |
| :--- | :--- |
| `git stash` | Salva alterações temporariamente em uma pilha para limpar o diretório |
| `git stash pop` | Recupera as alterações salvas no stash e as remove da pilha |
| `git stash list` | Lista todos os stashes armazenados |
| `git cherry-pick [commit]` | Aplica um commit específico de outra branch na branch atual |
| `git rebase [branch]` | Reaplica os commits da branch atual sobre outra branch |
| `git tag [nome]` | Cria uma tag (marcador) para um ponto específico do histórico |
| `git bisect` | Usa busca binária para encontrar qual commit introduziu um bug |
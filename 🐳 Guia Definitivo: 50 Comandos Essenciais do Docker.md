# 🐳 Guia Definitivo: 50 Comandos Essenciais do Docker

Este guia cobre desde a manipulação de imagens até a orquestração básica e limpeza do sistema.

---

## 🏗️ 1. Gestão de Imagens
| Comando | Descrição |
| :--- | :--- |
| `docker build -t [nome] .` | Cria uma imagem a partir de um Dockerfile no diretório atual |
| `docker images` | Lista todas as imagens baixadas ou criadas localmente |
| `docker pull [imagem]` | Baixa uma imagem do Docker Hub para a sua máquina |
| `docker push [imagem]` | Envia uma imagem local para um registro (Docker Hub) |
| `docker rmi [id_imagem]` | Remove uma imagem específica do computador |
| `docker rmi $(docker images -q)` | Remove todas as imagens locais de uma vez |
| `docker tag [id] [nome:tag]` | Atribui um nome e uma tag a uma imagem existente |
| `docker history [imagem]` | Exibe o histórico de camadas de uma imagem |
| `docker save -o [arquivo.tar] [imagem]` | Salva uma imagem em um arquivo comprimido |
| `docker load -i [arquivo.tar]` | Carrega uma imagem a partir de um arquivo tar |

---

## 🚀 2. Ciclo de Vida de Containers
| Comando | Descrição |
| :--- | :--- |
| `docker run [imagem]` | Cria e inicia um novo container a partir de uma imagem |
| `docker run -d [imagem]` | Inicia o container em modo "detached" (em segundo plano) |
| `docker run -p [porta_host]:[porta_cont] [img]` | Mapeia uma porta do computador para o container |
| `docker ps` | Lista apenas os containers que estão rodando no momento |
| `docker ps -a` | Lista todos os containers (rodando e parados) |
| `docker stop [id_container]` | Para a execução de um container rodando |
| `docker start [id_container]` | Inicia um container que estava parado |
| `docker restart [id_container]` | Para e inicia novamente um container |
| `docker pause [id_container]` | Pausa todos os processos dentro de um container |
| `docker unpause [id_container]` | Retoma os processos de um container pausado |
| `docker rm [id_container]` | Remove um container parado |
| `docker rm -f [id_container]` | Força a remoção de um container (mesmo rodando) |
| `docker wait [id_container]` | Bloqueia o terminal até que o container pare |

---

## 🔍 3. Execução e Depuração
| Comando | Descrição |
| :--- | :--- |
| `docker exec -it [id] [comando]` | Executa um comando dentro de um container rodando |
| `docker exec -it [id] /bin/bash` | Abre um terminal interativo dentro do container |
| `docker logs [id_container]` | Exibe os logs de saída do container |
| `docker logs -f [id_container]` | Acompanha os logs em tempo real (follow) |
| `docker top [id_container]` | Exibe os processos ativos dentro do container |
| `docker inspect [id_objeto]` | Retorna informações detalhadas (JSON) de um container/imagem |
| `docker stats` | Exibe o consumo de CPU, memória e rede em tempo real |
| `docker diff [id_container]` | Mostra arquivos alterados no container desde a criação |
| `docker port [id_container]` | Lista os mapeamentos de portas do container |

---

## 💾 4. Volumes e Redes
| Comando | Descrição |
| :--- | :--- |
| `docker volume ls` | Lista todos os volumes de dados criados |
| `docker volume create [nome]` | Cria um novo volume persistente |
| `docker volume rm [nome]` | Remove um volume específico |
| `docker volume prune` | Remove todos os volumes não utilizados |
| `docker network ls` | Lista todas as redes configuradas no Docker |
| `docker network create [nome]` | Cria uma nova rede personalizada |
| `docker network connect [rede] [cont]` | Conecta um container a uma rede específica |
| `docker network disconnect [rede] [cont]` | Desconecta um container de uma rede |

---

## 🧹 5. Limpeza do Sistema (Housekeeping)
| Comando | Descrição |
| :--- | :--- |
| `docker system df` | Exibe o uso de disco pelo Docker |
| `docker system prune` | Remove containers parados, redes inúteis e cache |
| `docker system prune -a` | Limpa TUDO que não está sendo usado (incluindo imagens) |
| `docker container prune` | Remove todos os containers parados |
| `docker image prune` | Remove apenas imagens "dangling" (sem nome/tag) |

---

## 🐙 6. Docker Compose (Orquestração Local)
| Comando | Descrição |
| :--- | :--- |
| `docker-compose up` | Cria e inicia todos os serviços do docker-compose.yml |
| `docker-compose up -d` | Inicia os serviços em segundo plano |
| `docker-compose down` | Para e remove containers, redes e imagens do compose |
| `docker-compose ps` | Lista o status dos serviços do compose atual |
| `docker-compose logs -f` | Exibe os logs de todos os serviços simultaneamente |
| `docker-compose exec [servico] [comando]` | Executa comandos em um serviço específico |
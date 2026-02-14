# ☸️ Guia Definitivo: 50 Comandos Essenciais de Kubernetes (kubectl)

O Kubernetes é o orquestrador de containers mais utilizado. Dominar o `kubectl` é fundamental para gerenciar clusters, pods e serviços.

---

## 🏗️ 1. Informações do Cluster e Configuração
| Comando | Descrição |
| :--- | :--- |
| `kubectl cluster-info` | Exibe os endereços do Master e serviços do cluster |
| `kubectl version` | Exibe a versão do cliente e do servidor K8s |
| `kubectl config view` | Mostra as configurações do arquivo kubeconfig |
| `kubectl config get-contexts` | Lista todos os contextos (clusters/usuários) disponíveis |
| `kubectl config use-context [nome]` | Altera o contexto ativo para outro cluster |
| `kubectl get nodes` | Lista todos os nós do cluster e seu status |
| `kubectl describe node [nome]` | Exibe detalhes detalhados de um nó específico |
| `kubectl api-resources` | Lista todos os tipos de recursos suportados (pods, svc, etc.) |

---

## 🚀 2. Criação e Gerenciamento de Recursos
| Comando | Descrição |
| :--- | :--- |
| `kubectl apply -f [arquivo.yaml]` | Cria ou atualiza recursos definidos em um arquivo |
| `kubectl delete -f [arquivo.yaml]` | Remove os recursos definidos no arquivo |
| `kubectl run [nome] --image=[imagem]` | Cria e roda um pod de forma imperativa |
| `kubectl create deployment [nome] --image=[img]` | Cria um novo Deployment |
| `kubectl expose deployment [nome] --port=[80]` | Cria um Service para expor um Deployment |
| `kubectl edit [recurso] [nome]` | Edita a configuração de um recurso "ao vivo" no editor |
| `kubectl scale deployment [nome] --replicas=[n]` | Aumenta ou diminui o número de instâncias (pods) |
| `kubectl autoscale deployment [nome] --min=2 --max=5` | Configura o Horizontal Pod Autoscaler (HPA) |

---

## 🔍 3. Visualização e Inspeção (Get & Describe)
| Comando | Descrição |
| :--- | :--- |
| `kubectl get pods` | Lista os pods no namespace atual |
| `kubectl get pods -A` | Lista todos os pods de todos os namespaces |
| `kubectl get pods -o wide` | Lista pods com informações extras (como o IP do nó) |
| `kubectl get services` | Lista os serviços (SVCs) e seus IPs |
| `kubectl get deployments` | Lista os deployments e seu estado de prontidão |
| `kubectl get namespaces` | Lista os namespaces do cluster |
| `kubectl describe pod [nome]` | Mostra eventos e detalhes técnicos de um pod |
| `kubectl describe service [nome]` | Mostra detalhes do balanceamento e portas do serviço |
| `kubectl get events --sort-by='.lastTimestamp'` | Lista os eventos recentes do cluster por ordem de tempo |

---

## 🛠️ 4. Debugging e Troubleshooting
| Comando | Descrição |
| :--- | :--- |
| `kubectl logs [pod]` | Exibe os logs de um pod |
| `kubectl logs -f [pod]` | Acompanha os logs em tempo real |
| `kubectl logs [pod] -c [container]` | Exibe logs de um container específico dentro de um pod |
| `kubectl exec -it [pod] -- /bin/bash` | Abre um terminal interativo dentro de um pod |
| `kubectl port-forward [pod] [local]:[remote]` | Mapeia uma porta local para uma porta do pod |
| `kubectl proxy` | Cria um proxy entre sua máquina e o servidor de API do K8s |
| `kubectl top pod` | Exibe consumo de CPU e Memória dos pods (requer Metrics Server) |
| `kubectl top node` | Exibe consumo de CPU e Memória dos nós do cluster |
| `kubectl cp [arquivo] [pod]:[caminho]` | Copia arquivos da máquina local para dentro do pod |

---

## 🔄 5. Atualizações e Rollouts
| Comando | Descrição |
| :--- | :--- |
| `kubectl rollout status deployment/[nome]` | Acompanha o progresso de uma atualização |
| `kubectl rollout history deployment/[nome]` | Exibe o histórico de revisões (versões) |
| `kubectl rollout undo deployment/[nome]` | Reverte para a versão anterior (Rollback) |
| `kubectl rollout restart deployment/[nome]` | Reinicia todos os pods de um deployment |
| `kubectl set image deployment/[nome] [c]=[img:v2]` | Atualiza a imagem de um container no deployment |

---

## 🧹 6. Limpeza e Administração Avançada
| Comando | Descrição |
| :--- | :--- |
| `kubectl delete pod [nome]` | Remove um pod específico |
| `kubectl delete pod --all` | Remove todos os pods do namespace atual |
| `kubectl delete namespace [nome]` | Remove um namespace e TUDO que houver dentro dele |
| `kubectl label pods [nome] [chave]=[valor]` | Adiciona ou atualiza uma etiqueta em um recurso |
| `kubectl annotate pods [nome] [desc]` | Adiciona uma anotação informativa a um recurso |
| `kubectl cordon [nó]` | Marca um nó como não agendável (impede novos pods) |
| `kubectl drain [nó]` | Esvazia o nó, movendo os pods para outros nós |
| `kubectl uncordon [nó]` | Reabilita um nó para receber novos pods |
| `kubectl get all` | Mostra todos os recursos do namespace (pods, svc, deploy, etc.) |
# 📨 Guia de Hacks: Kafka & RabbitMQ (CLI & Admin)

Este guia reúne comandos práticos para monitoramento, depuração e gerenciamento de mensagens em sistemas de mensageria e streaming.

---

## 🎡 PARTE 1: APACHE KAFKA (Streaming de Eventos)

O Kafka é focado em logs distribuídos. Os comandos abaixo assumem o uso dos scripts padrão da pasta `/bin`.

### Tópicos e Estrutura
1. **Listar todos os tópicos:** `kafka-topics.sh --list --bootstrap-server localhost:9092`
2. **Criar tópico com partições e replicação:** `kafka-topics.sh --create --topic meu-topico --partitions 3 --replication-factor 2 --bootstrap-server localhost:9092`
3. **Ver detalhes de um tópico (Partições/ISR):** `kafka-topics.sh --describe --topic meu-topico --bootstrap-server localhost:9092`
4. **Alterar número de partições (apenas aumentar):** `kafka-topics.sh --alter --topic meu-topico --partitions 5 --bootstrap-server localhost:9092`
5. **Deletar um tópico:** `kafka-topics.sh --delete --topic meu-topico --bootstrap-server localhost:9092`

### Produção e Consumo (Debug)
6. **Consumir mensagens desde o início:** `kafka-console-consumer.sh --topic meu-topico --from-beginning --bootstrap-server localhost:9092`
7. **Consumir apenas as últimas 10 mensagens:** `kafka-console-consumer.sh --topic meu-topico --max-messages 10 --bootstrap-server localhost:9092`
8. **Produzir mensagens via terminal:** `kafka-console-producer.sh --topic meu-topico --bootstrap-server localhost:9092`
9. **Consumir com Chave e Valor visíveis:** `kafka-console-consumer.sh --topic meu-topico --property print.key=true --property key.separator="-" --from-beginning --bootstrap-server localhost:9092`

### Gestão de Grupos e Offsets
10. **Listar grupos de consumidores:** `kafka-consumer-groups.sh --list --bootstrap-server localhost:9092`
11. **Ver o "Lag" (atraso) de um grupo:** `kafka-consumer-groups.sh --describe --group meu-grupo --bootstrap-server localhost:9092`
12. **Resetar offsets para o início (Dry Run):** `kafka-consumer-groups.sh --group meu-grupo --topic meu-topico --reset-offsets --to-earliest --dry-run --bootstrap-server localhost:9092`
13. **Resetar offsets (Executar de verdade):** `kafka-consumer-groups.sh --group meu-grupo --topic meu-topico --reset-offsets --to-earliest --execute --bootstrap-server localhost:9092`

### Performance e Configuração
14. **Ver configurações de retenção do tópico:** `kafka-configs.sh --describe --entity-type topics --entity-name meu-topico --bootstrap-server localhost:9092`
15. **Alterar tempo de retenção (ex: 1 hora):** `kafka-configs.sh --alter --entity-type topics --entity-name meu-topico --add-config retention.ms=3600000 --bootstrap-server localhost:9092`

---

## 🐇 PARTE 2: RABBITMQ (Mensageria Tradicional)

O RabbitMQ usa o protocolo AMQP. O foco aqui é o comando `rabbitmqctl` e `rabbitmqadmin`.

### Gestão de Filas (Queues)
16. **Listar todas as filas e contagem de mensagens:** `rabbitmqctl list_queues name messages_ready messages_unacknowledged`
17. **Ver detalhes de uma fila específica:** `rabbitmqctl list_queues name consumers memory`
18. **Apagar todas as mensagens de uma fila (Purge):** `rabbitmqctl purge_queue nome_da_fila`
19. **Deletar uma fila:** `rabbitmqctl delete_queue nome_da_fila`
20. **Listar consumidores ativos:** `rabbitmqctl list_consumers`

### Usuários e Permissões
21. **Adicionar novo usuário:** `rabbitmqctl add_user meu_user minha_senha`
22. **Dar permissão de administrador:** `rabbitmqctl set_user_tags meu_user administrator`
23. **Definir permissões de acesso (vhost):** `rabbitmqctl set_permissions -p / meu_user ".*" ".*" ".*"`
24. **Alterar senha de usuário:** `rabbitmqctl change_password meu_user nova_senha`

### Exchanges e Bindings
25. **Listar exchanges:** `rabbitmqctl list_exchanges`
26. **Listar bindings (ligações entre exchange e fila):** `rabbitmqctl list_bindings`
27. **Criar uma exchange via CLI:** `rabbitmqadmin declare exchange name=minha_exchange type=direct`

### Saúde do Cluster e Sistema
28. **Ver status do nó:** `rabbitmqctl status`
29. **Verificar saúde (Health check):** `rabbitmqctl node_health_check`
30. **Listar conexões abertas:** `rabbitmqctl list_connections`
31. **Forçar fechamento de conexão:** `rabbitmqctl close_connection "ID_CONEXAO" "Motivo do fechamento"`
32. **Ver uso de memória por categoria:** `rabbitmqctl report`

### Hacks de Produtividade (rabbitmqadmin)
33. **Exportar todas as definições (Backup JSON):** `rabbitmqadmin export rabbit_backup.json`
34. **Importar definições de arquivo:** `rabbitmqadmin import rabbit_backup.json`
35. **Publicar uma mensagem via CLI:** `rabbitmqadmin publish exchange=amq.default routing_key=minha_fila payload="Olá Mundo"`
36. **Ler uma mensagem da fila (Get):** `rabbitmqadmin get queue=minha_fila ackmode=ack_requeue_false`

---

## 💡 Diferença Visual: Kafka vs RabbitMQ



### Quando usar cada um?
* **Use Kafka** se precisar de reprocessamento de dados antigos, alta retenção (dias/meses) e análise de logs em tempo real.
* **Use RabbitMQ** se precisar de roteamento complexo, confirmações de entrega granulares (ACK/NACK) e filas de tarefas simples.

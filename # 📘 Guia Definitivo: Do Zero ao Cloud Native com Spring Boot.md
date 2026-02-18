# 📘 Guia Definitivo: Do Zero ao Cloud Native com Spring Boot

O Spring Boot não é apenas um framework; é um ecossistema que abstrai a complexidade do Java Enterprise (Jakarta EE) através de Inversão de Controle (IoC) e Injeção de Dependência (DI). Este manual percorre desde a criação do Bean até a entrega resiliente em ambientes distribuídos.

---

## 1. Fundamentos e Injeção de Dependência

O coração do Spring é o ApplicationContext, que gerencia o ciclo de vida dos objetos (Beans). Para a injeção de dependência, prefira sempre a injeção via construtor (final) em vez de `@Autowired` em campos, garantindo imutabilidade e facilidade nos testes:

```java
@Service
public class OrdemService {
    private final PagamentoClient pagamentoClient; 

    public OrdemService(PagamentoClient pagamentoClient) {
        this.pagamentoClient = pagamentoClient;
    }
}
```

---

## 2. Persistência de Dados (Spring Data JPA e Jakarta)

O Spring Data utiliza o Hibernate por baixo dos panos, abstraindo o boilerplate, mas o mapeamento correto via Jakarta Persistence é o que garante a performance.

### Mapeamento de Entidades Complexas
Utilize UUIDs para microsserviços e evite problemas de performance (N+1) com o `FetchType.LAZY`.

```java
@Entity
@Table(name = "tb_pedido")
public class Pedido {

    @Id
    @GeneratedValue(strategy = GenerationType.UUID)
    private UUID id;

    @Column(nullable = false)
    private LocalDateTime dataCriacao = LocalDateTime.now();

    @Enumerated(EnumType.STRING)
    private StatusPedido status;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "cliente_id")
    private Cliente cliente;

    @OneToMany(mappedBy = "pedido", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<ItemPedido> itens = new ArrayList<>();
}
```

### O Poder das Query Methods e Projeções
Você não precisa escrever SQL para operações simples. Para relatórios ou listagens pesadas, evite carregar a entidade inteira utilizando Interfaces de Projeção.

```java
public interface PedidoRepository extends JpaRepository<Pedido, UUID> {
    // Spring gera o SQL automaticamente
    Optional<Pedido> findByStatus(StatusPedido status);
}

// Projeção para carregar apenas o necessário
public interface PedidoResumo {
    UUID getId();
    StatusPedido getStatus();
    @Value("#{target.cliente.nome}") // SpEL para acessar dados relacionados
    String getNomeCliente();
}
```

---

## 3. Segurança: Autenticação Stateless (JWT)

O modelo moderno (Spring Security 6+) removeu o `WebSecurityConfigurerAdapter` em favor de uma configuração baseada em Beans. O fluxo de segurança intercepta a requisição antes de chegar ao `@RestController`.

### Configuração de Filtros e Cadeia de Segurança

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        return http
            .csrf(csrf -> csrf.disable()) // Desabilitar para APIs Stateless
            .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/auth/**").permitAll()
                .requestMatchers("/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .addFilterBefore(new JwtSecurityFilter(), UsernamePasswordAuthenticationFilter.class)
            .build();
    }
}
```

---

## 4. Comunicação Síncrona e Assíncrona

### Síncrona: OpenFeign
Permite criar clientes HTTP através de interfaces, integrando-se nativamente com Load Balancers e Circuit Breakers.

```java
@FeignClient(name = "estoque-service", url = "${urls.estoque}")
public interface EstoqueClient {
    @GetMapping("/produtos/{id}/disponibilidade")
    @CircuitBreaker(name = "estoqueCB", fallbackMethod = "fallbackDisponibilidade")
    boolean verificarEstoque(@PathVariable("id") Long id);

    default boolean fallbackDisponibilidade(Long id, Throwable t) {
        return true; // Lógica de contingência
    }
}
```

### Assíncrona: Spring Cloud Stream
Abstrai o uso de mensageria (Kafka, RabbitMQ), permitindo trocar o provedor apenas alterando o `application.yml`.

```java
@Service
public class NotificacaoProducer {
    private final StreamBridge streamBridge;

    public void enviarEvento(Pedido pedido) {
        streamBridge.send("pedidoOutput-out-0", pedido);
    }
}
```

---

## 5. Resiliência de Sistemas (Resilience4j)

Para sistemas distribuídos, falhas são inevitáveis. Se um serviço cair, o outro não pode travar.

* Retry: Tenta novamente falhas transientes (ex: timeout de rede).
* Circuit Breaker: Interrompe chamadas a serviços instáveis (ex: banco fora do ar).
* Rate Limiter: Limita o número de chamadas por tempo (evita abusos).
* Bulkhead: Isola recursos/threads para diferentes serviços.

Configuração via `application.yml`:
```yaml
resilience4j.circuitbreaker:
  instances:
    backendExterno:
      slidingWindowSize: 10
      permittedNumberOfCallsInHalfOpenState: 3
      failureRateThreshold: 50
      waitDurationInOpenState: 10s
```

---

## 6. Performance: Cache e Agendamento

### Cache Distribuído (Redis/Caffeine)
O uso do `@Cacheable` reduz drasticamente a latência e idas ao banco de dados para informações como cotações financeiras ou catálogos.

```java
@Service
public class CotacaoService {

    @Cacheable(value = "cotacoes", key = "#moeda")
    public BigDecimal getCotacaoAtual(String moeda) {
        return apiEconomia.getRate(moeda);
    }

    @CacheEvict(value = "cotacoes", allEntries = true)
    @Scheduled(fixedRate = 3600000) // Limpa o cache a cada 1 hora
    public void limparCache() { }
}
```

### Tarefas Agendadas
Ideal para conciliações, limpezas de banco de dados ou envios de relatórios diários (`@Scheduled(cron = "0 0 0 * * *")`).

---

## 7. Qualidade: Testes Automatizados

No ecossistema Spring, a qualidade é garantida com JUnit 5, Mockito e Testcontainers.

* Testes Unitários: Focados na regra de negócio da `@Service` usando `@InjectMocks` e fazendo o mock dos repositórios para isolar o banco de dados.
* Testes de Integração (Testcontainers): Sobem bancos de dados reais (como PostgreSQL ou MySQL) em containers Docker descartáveis durante a esteira de CI/CD para validar queries e o mapeamento JPA na prática.

---

## 8. Observabilidade e Cloud Native (Docker & Kubernetes)

Um projeto Spring moderno requer métricas e empacotamento eficiente.

### Observabilidade (Actuator)
Adicione o `Spring Boot Actuator` e o `Micrometer` para expor o estado da aplicação:
* `/actuator/health`: Prontidão e vitalidade (Liveness/Readiness).
* `/actuator/prometheus`: Métricas no formato consumível pelo Prometheus/Grafana.

### Docker e Kubernetes (K8s)
Para ser verdadeiramente "Cloud Native", a aplicação deve rodar em containers orquestrados. 
* No Docker, utilize builds de múltiplos estágios (multi-stage builds) ou ferramentas nativas do Spring (`./mvnw spring-boot:build-image`) para gerar imagens OCI otimizadas.
* No Kubernetes, exponha os endpoints do Actuator nas sondas de `livenessProbe` e `readinessProbe` do seu `Deployment` para que o cluster saiba gerenciar o ciclo de vida do seu microsserviço com segurança, reiniciando pods travados automaticamente.

---

## 9. Boas Práticas e Anti-Padrões (Pitfalls)

1. Entity vs DTO: Nunca exponha sua `@Entity` diretamente no Controller. Use DTOs para evitar que mudanças no banco quebrem o contrato da API e para prevenir ataques de Mass Assignment.
2. Lazy Initialization Exception: Cuidado ao acessar coleções `@OneToMany` fora de uma transação (`@Transactional`).
3. Dependência Circular: Evite que o ServiceA injete o ServiceB e vice-versa. Resolva movendo a lógica comum para um terceiro componente.

### Tabela de Padrões de Resposta HTTP
| Código | Significado | Exemplo Prático |
| :--- | :--- | :--- |
| 200 OK | Sucesso | Retorno de um GET para listar recursos. |
| 201 Created | Criado | Retorno de um POST após salvar no banco. |
| 204 No Content | Sem Conteúdo | DELETE com sucesso ou PUT sem retorno. |
| 400 Bad Request | Erro do Cliente | Falha na validação (`@Valid`). |
| 401 Unauthorized | Não Autenticado | Token JWT ausente ou expirado. |
| 403 Forbidden | Sem Permissão | Usuário logado, mas sem Role necessária. |
| 404 Not Found | Não Encontrado | Registro não existe no banco de dados. |
| 500 Internal Error | Erro no Servidor | Exceção não tratada ou banco fora do ar. |

---

## 10. Guia de Referência de Anotações

### Estereótipos e Arquitetura
| Anotação | Propósito | Quando usar? |
| :--- | :--- | :--- |
| `@Component` | Genérico | Classes de utilidade ou lógica geral. |
| `@Service` | Negócio | Onde residem as regras de negócio e orquestração. |
| `@Repository` | Dados | Traduz exceções de banco para exceções do Spring. |
| `@RestController`| Exposição | Define uma API REST (`@Controller` + `@ResponseBody`). |
| `@Configuration` | Setup | Define métodos `@Bean` para instanciar objetos de terceiros. |

### Spring vs Jakarta e Terceiros
Para evitar confusão nos imports, consulte a referência de pacotes:

| Contexto | Anotação | Pacote/Fonte | Função |
| :--- | :--- | :--- | :--- |
| Injeção | `@Autowired` | `org.springframework...` | Injeção padrão do Spring. |
| Injeção | `@Inject` | `jakarta.inject` | Injeção padrão Java (JSR-330). |
| Persistência | `@Entity` | `jakarta.persistence` | Define classe como tabela. |
| Persistência | `@OneToMany` | `jakarta.persistence` | Relacionamento de 1 para muitos. |
| Transação | `@Transactional`| `org.springframework...` | Gerencia a transação do banco. |
| Web | `@RequestBody` | `org.springframework...` | Converte o payload JSON para Objeto Java. |
| Validação | `@Valid` | `jakarta.validation` | Dispara a validação do objeto na requisição. |
| Validação | `@NotBlank` | `jakarta.validation` | Impede envio de strings vazias no DTO. |
| Validação | `@Min(x)` | `jakarta.validation` | Valida valor mínimo numérico. |
| JSON | `@JsonProperty` | Jackson | Mapeia o nome do campo customizado no JSON. |
| Utilitário | `@Data` | Lombok | Gera código repetitivo (Getters, Setters) em tempo de compilação. |

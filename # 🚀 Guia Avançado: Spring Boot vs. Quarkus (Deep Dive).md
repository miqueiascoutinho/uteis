# 🚀 Guia Avançado: Spring Boot vs. Quarkus (Deep Dive)

Este guia é destinado a desenvolvedores que já dominam o ecossistema Java e precisam entender a equivalência técnica e as capacidades de infraestrutura de ambas as frameworks.

---

## 1. Mapeamento de Anotações (Injeção e Escopo)

O Spring utiliza um modelo próprio de Beans, enquanto o Quarkus baseia-se no **CDI (Contexts and Dependency Injection)**.

| Recurso | Spring Boot | Quarkus (ArC/CDI) | Notas |
| --- | --- | --- | --- |
| **Componente Genérico** | `@Component` | `@ApplicationScoped` | O Quarkus prefere escopos explícitos. |
| **Serviço/Lógica** | `@Service` | `@ApplicationScoped` | No Quarkus, não há uma anotação "Service" dedicada, usa-se o escopo. |
| **Singleton** | `@Scope("singleton")` | `@Singleton` | `@Singleton` no CDI é mais performático que `@ApplicationScoped` (sem proxy). |
| **Configuração** | `@Configuration` | Não existe uma direta | Usa-se métodos anotados com `@Produces` em qualquer bean. |
| **Criação de Bean** | `@Bean` | `@Produces` | Usado para integrar libs de terceiros. |
| **Injeção** | `@Autowired` | `@Inject` | No Quarkus, a injeção via construtor é recomendada e dispensa anotação. |
| **Post-Construct** | `@PostConstruct` | `@PostConstruct` | Comportamento idêntico (JSR-250). |
| **Valor/Config** | `@Value("${prop}")` | `@ConfigProperty(name="prop")` | Quarkus usa o padrão Eclipse MicroProfile Config. |

---

## 2. Persistência e Dados (Data Access)

### Spring Data JPA vs. Quarkus Panache

O Spring Data é baseado em interfaces e geração de proxies. O Quarkus introduziu o **Hibernate Reactive** e o **Panache**, que simplifica drasticamente o padrão Repository ou Active Record.

**Spring (Repository Pattern):**

```java
public interface ProdutoRepository extends JpaRepository<Produto, Long> {
    List<Produto> findByName(String name);
}

```

**Quarkus (Active Record Pattern - Panache):**

```java
@Entity
public class Produto extends PanacheEntity {
    public String name;

    public static List<Produto> findByName(String name) {
        return list("name", name);
    }
}
// Uso: Produto.findByName("Cerveja");

```

*O Quarkus também suporta o Repository Pattern com `PanacheRepository`, caso prefira manter a separação.*

---

## 3. Web e Reatividade

| Recurso | Spring Boot | Quarkus |
| --- | --- | --- |
| **Engine Principal** | Tomcat / Jetty (Imperativo) | Netty / Vert.x (Reativo por padrão) |
| **Stack Reativa** | Spring WebFlux (Project Reactor) | RESTEasy Reactive (Mutiny) |
| **Programação** | `Mono<T>` / `Flux<T>` | `Uni<T>` / `Multi<T>` |

> **Diferencial Quarkus:** No Quarkus, você pode misturar código bloqueante e não-bloqueante no mesmo controller. Ele gerencia as threads de forma inteligente (Worker Threads vs IO Threads) sem exigir que você mude toda a arquitetura para WebFlux.

---

## 4. Gerenciamento de Configurações Avançado

### Profiles e Runtime

* **Spring:** Usa `application-{profile}.properties` e a anotação `@Profile("dev")`.
* **Quarkus:** Usa um único `application.properties` com prefixos:
* `%dev.quarkus.datasource.jdbc.url=...`
* `%prod.quarkus.datasource.jdbc.url=...`
* Isso evita a fragmentação de arquivos de configuração.



---

## 5. Recursos de Infraestrutura (Cloud Native)

### Dev Services (O "Pulo do Gato" do Quarkus)

No Spring, você precisa configurar um Docker Compose ou instalar o banco localmente. No Quarkus:

* Se você adicionar a extensão `quarkus-jdbc-postgresql` e **não** configurar a URL de conexão, o Quarkus detecta o Docker rodando e **sobe automaticamente** um container Postgres para você durante o desenvolvimento.
* Isso vale para Redis, Kafka, RabbitMQ, Keycloak, etc.

### Observabilidade

* **Spring:** Spring Boot Actuator. Oferece `/actuator/health`, `/metrics`.
* **Quarkus:** Extensões de Health e Metrics (MicroProfile). Oferece `/q/health`, `/q/metrics` e uma **Dev UI** ( `/q/dev`) fantástica para inspecionar beans, rotas e configurações em tempo real.

---

## 6. Compilação Nativa (GraalVM)

Este é o campo onde o Quarkus brilha.

* **Spring Native:** Evoluiu muito no Spring Boot 3, mas ainda enfrenta desafios com bibliotecas que usam muita reflexão dinâmica.
* **Quarkus:** Foi desenhado para isso. Ele remove classes não utilizadas e resolve toda a reflexão durante o build. O resultado é um executável binário que não precisa de JVM para rodar.

---

## Resumo de Comandos CLI

| Ação | Spring (Maven) | Quarkus CLI |
| --- | --- | --- |
| Criar Projeto | Start.spring.io | `quarkus create app` |
| Modo Dev | `./mvnw spring-boot:run` | `quarkus dev` |
| Add Extensão | Editar pom.xml manualmente | `quarkus ext add hibernate-validator` |
| Build Nativo | `./mvnw native:compile` | `quarkus build --native` |

---

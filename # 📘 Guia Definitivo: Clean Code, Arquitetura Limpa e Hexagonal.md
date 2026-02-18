# 📘 Guia Definitivo: Clean Code, Arquitetura Limpa e Hexagonal

A construção de software sustentável exige disciplina em múltiplas escalas. O "Clean Code" cuida do nível micro (linhas, métodos e classes), enquanto a "Arquitetura Limpa" e a "Arquitetura Hexagonal" cuidam do nível macro (módulos, componentes e integrações). O objetivo de todas é o mesmo: criar sistemas fáceis de entender, manter e evoluir.

---

## PARTE 1: CLEAN CODE (O Nível Micro)

Popularizado por Robert C. Martin (Uncle Bob), o Código Limpo foca na legibilidade. O código é lido muito mais vezes do que é escrito, portanto, otimize para a leitura humana, não apenas para o compilador.

### 1.1 Nomes Significativos
Nomes de variáveis, métodos e classes devem revelar intenção. Evite abreviações obscuras ou nomes genéricos.

* Ruim: `int d; // tempo percorrido em dias`
* Ruim: `List<User> userList;` (Não coloque o tipo no nome da variável)
* Bom: `int elapsedTimeInDays;`
* Bom: `List<User> activeUsers;`

### 1.2 Funções Pequenas e Focadas
A regra de ouro para funções é: elas devem ser pequenas. A segunda regra é: elas devem ser menores ainda.
* Single Responsibility Principle (SRP): Uma função deve fazer apenas uma coisa, fazê-la bem e fazer apenas ela.
* Nível de Abstração: Todos os comandos de uma função devem estar no mesmo nível de abstração.
* Parâmetros: O número ideal de parâmetros é zero (niládica), depois um (monádica) e dois (diádica). Três ou mais exigem justificação pesada (encapsule-os em um objeto/DTO).

### 1.3 Comentários
"Não comente o código ruim, reescreva-o." Comentários mentem com o tempo porque o código muda e os comentários são frequentemente esquecidos.
* Quando usar: Para expressar intenções de negócio obscuras, alertar sobre consequências ou TODOs temporários.
* Quando evitar: Comentários que apenas repetem o que o código faz ou código comentado (apague-o, o Git existe para isso).

### 1.4 Tratamento de Erros
* Use Exceções em vez de códigos de retorno. Códigos de retorno poluem o fluxo de controle.
* Não retorne `null`. Retornar `null` força quem chama a fazer verificações defensivas (`if (obj != null)`), poluindo o código. Retorne coleções vazias ou lance exceções (como `EntityNotFoundException`).

### 1.5 A Regra do Escoteiro (Boy Scout Rule)
"Deixe a área do acampamento um pouco mais limpa do que você a encontrou." Sempre que abrir um arquivo para alterar algo, renomeie uma variável confusa, extraia um método longo ou apague um comentário inútil.

---

## PARTE 2: ARQUITETURA LIMPA (O Nível Macro Concêntrico)

Também descrita por Uncle Bob, a Clean Architecture propõe a divisão do sistema em camadas concêntricas, cujo princípio mais importante é a Regra da Dependência.

### 2.1 A Regra da Dependência
As dependências do código-fonte só podem apontar para dentro (em direção às políticas de alto nível). O núcleo do sistema não pode saber nada sobre banco de dados, frameworks web ou UI.

### 2.2 As Camadas (De dentro para fora)

1.  Entidades (Entities / Enterprise Business Rules):
    * Encapsulam as regras de negócio cruciais e independentes da aplicação.
    * Exemplo: Uma classe `Emprestimo` que calcula juros não importa se a aplicação é web, mobile ou terminal.

2.  Casos de Uso (Use Cases / Application Business Rules):
    * Orquestram o fluxo de dados para e das entidades e direcionam as entidades a usarem suas regras de negócio.
    * Exemplo: `RealizarEmprestimoUseCase`, que valida saldo, chama a entidade para calcular juros e pede para salvar o resultado.

3.  Adaptadores de Interface (Interface Adapters):
    * Convertem os dados do formato mais conveniente para os Casos de Uso/Entidades para o formato mais conveniente para a camada externa (DB, Web).
    * Exemplo: Controllers (REST), Presenters, DTOs e Implementações de Repositórios.

4.  Frameworks e Drivers (Camada mais externa):
    * Onde vivem os detalhes de infraestrutura: Banco de Dados, Frameworks (Spring, Hibernate), Bibliotecas de UI. Tudo aqui é tratado como um "detalhe" que pode ser substituído.

---

## PARTE 3: ARQUITETURA HEXAGONAL (Ports and Adapters)

Criada por Alistair Cockburn, a Arquitetura Hexagonal tem exatamente o mesmo objetivo da Arquitetura Limpa: isolar o domínio das tecnologias externas. A diferença está na forma de visualizar e na terminologia.

### 3.1 O Hexágono (O Domínio)
O centro do hexágono contém o modelo de domínio e as regras de negócio. O formato de hexágono não significa que existem exatamente seis lados, mas sim que existem múltiplos pontos de entrada e saída.

### 3.2 Ports (Portas)
São as "tomadas" do hexágono. Definem os contratos (interfaces) de como o mundo exterior pode interagir com o domínio e vice-versa.
* Inbound Ports (Portas de Entrada / Driving): Interfaces que definem o que a aplicação pode fazer (ex: `AprovarPedidoUseCase`). O lado esquerdo do hexágono.
* Outbound Ports (Portas de Saída / Driven): Interfaces que definem o que a aplicação precisa do mundo exterior para funcionar (ex: `PedidoRepository` ou `NotificacaoGateway`). O lado direito do hexágono.

### 3.3 Adapters (Adaptadores)
São os "plugues" que se conectam às portas, convertendo a comunicação do mundo exterior para o domínio.
* Driving Adapters (Adaptadores Diretores): Quem "inicia" a ação. Ex: Um `PedidoRestController` (Web) ou um `PedidoKafkaListener` (Mensageria). Eles chamam os Inbound Ports.
* Driven Adapters (Adaptadores Dirigidos): Quem "reage" à ação iniciada pelo domínio. Ex: `PedidoJpaAdapter` (implementa o Outbound Port do repositório) ou `EmailSenderAdapter` (implementa o Outbound Port de notificação).

---

## PARTE 4: CONVERGÊNCIA E O SEGREDO MÁGICO (DIP)

Como tanto a Clean Architecture quanto a Hexagonal conseguem fazer com que o Domínio (no centro) mande salvar dados no Banco de Dados (na borda) sem conhecer a tecnologia de banco de dados?

O segredo é o Princípio da Inversão de Dependência (Letra "D" do SOLID).

Sem DIP: Caso de Uso -> chama -> Classe Concreta do Banco de Dados (Viola a Regra da Dependência).
Com DIP:
1. O Caso de Uso (no centro) define uma Interface (Porta): `interface UsuarioRepository { void salvar(Usuario u); }`.
2. O Caso de Uso chama essa interface. Ele não sabe quem a implementa.
3. A camada externa (Adaptador) cria a classe `UsuarioRepositoryImpl` que implementa a interface do centro e usa SQL/JPA.
4. O framework de Injeção de Dependência (como Spring) "injeta" a implementação na interface em tempo de execução.

---

## PARTE 5: EXEMPLO PRÁTICO INTEGRADO (Java)

Aplicando Clean Code, Clean Architecture e Hexagonal na mesma estrutura:

### 1. O Domínio (Entidade - Clean Code: Focada)
```java
// domain/model/Conta.java
public class Conta {
    private UUID id;
    private BigDecimal saldo;

    public void sacar(BigDecimal valor) {
        if (valor.compareTo(saldo) > 0) {
            throw new SaldoInsuficienteException();
        }
        this.saldo = this.saldo.subtract(valor);
    }
}
```

### 2. O Port de Saída (Outbound Port)
```java
// application/port/out/ContaRepository.java
public interface ContaRepository {
    Optional<Conta> buscarPorId(UUID id);
    void salvar(Conta conta);
}
```

### 3. O Port de Entrada e Caso de Uso (Inbound Port / Use Case)
```java
// application/port/in/EfetuarSaqueUseCase.java
public interface EfetuarSaqueUseCase {
    void executar(UUID contaId, BigDecimal valor);
}

// application/service/EfetuarSaqueService.java (Aplica a Regra do Escoteiro, limpo e direto)
@Service
public class EfetuarSaqueService implements EfetuarSaqueUseCase {
    
    private final ContaRepository repository;

    // Injeção de dependência via construtor
    public EfetuarSaqueService(ContaRepository repository) {
        this.repository = repository;
    }

    @Override
    public void executar(UUID contaId, BigDecimal valor) {
        Conta conta = repository.buscarPorId(contaId)
            .orElseThrow(() -> new ContaNaoEncontradaException());
        
        conta.sacar(valor);
        repository.salvar(conta);
    }
}
```

### 4. O Adaptador de Entrada (Driving Adapter / Interface Adapter)
```java
// adapter/in/web/SaqueController.java
@RestController
@RequestMapping("/contas")
public class SaqueController {
    
    private final EfetuarSaqueUseCase saqueUseCase;

    public SaqueController(EfetuarSaqueUseCase saqueUseCase) {
        this.saqueUseCase = saqueUseCase;
    }

    @PostMapping("/{id}/saque")
    public ResponseEntity<Void> sacar(@PathVariable UUID id, @RequestBody SaqueRequestDTO request) {
        saqueUseCase.executar(id, request.getValor());
        return ResponseEntity.noContent().build();
    }
}
```

### 5. O Adaptador de Saída (Driven Adapter / Frameworks & Drivers)
```java
// adapter/out/persistence/ContaRepositoryAdapter.java
@Component
public class ContaRepositoryAdapter implements ContaRepository {

    private final SpringDataJpaContaRepository jpaRepository; // Repositório real do Spring

    public ContaRepositoryAdapter(SpringDataJpaContaRepository jpaRepository) {
        this.jpaRepository = jpaRepository;
    }

    @Override
    public Optional<Conta> buscarPorId(UUID id) {
        return jpaRepository.findById(id).map(ContaEntityMapper::toDomain);
    }

    @Override
    public void salvar(Conta conta) {
        jpaRepository.save(ContaEntityMapper.toEntity(conta));
    }
}
```

### Resumo do Fluxo no Exemplo:
O Controller Web (Adaptador) recebe o JSON, chama a Interface do Caso de Uso (Porta Inbound). A implementação do Caso de Uso carrega a Entidade de Negócio pura usando a Interface do Repositório (Porta Outbound). A entidade aplica as regras. O Caso de Uso manda salvar. O Adaptador de Banco de Dados executa o SQL usando o framework JPA. Tudo de fora para dentro, isolando as regras do negócio perfeitamente.

# ☕ Gerenciamento Java & Maven (Build & Dependency)

O Maven é o padrão para projetos Java, e o `mvnw` (Wrapper) garante que o projeto rode em qualquer máquina sem precisar instalar o Maven manualmente.

| Função | Comando (Global) | Comando (Wrapper) |
| :--- | :--- | :--- |
| Limpar pasta /target | `mvn clean` | `./mvnw clean` |
| Compilar classes | `mvn compile` | `./mvnw compile` |
| Rodar testes | `mvn test` | `./mvnw test` |
| Gerar JAR/WAR | `mvn package` | `./mvnw package` |
| Build + Instalar no Repo Local | `mvn clean install` | `./mvnw clean install` |
| Rodar App Spring Boot | `mvn spring-boot:run` | `./mvnw spring-boot:run` |
| Ver Árvore de Dependências | `mvn dependency:tree` | `./mvnw dependency:tree` |
| Pular testes no Build | `mvn package -DskipTests` | `./mvnw package -DskipTests` |
| Baixar Sources/Javadocs | `mvn dependency:sources` | `./mvnw dependency:sources` |
| Atualizar dependências | `mvn clean install -U` | `./mvnw clean install -U` |

---

# 🟢 Ecossistema Node.js & Gerenciadores (NPM / Yarn)

NPM e Yarn são os gestores de pacotes para JavaScript/TypeScript. O `npx` permite rodar ferramentas sem instalá-las globalmente.

| Função | NPM | Yarn |
| :--- | :--- | :--- |
| Iniciar Projeto | `npm init -y` | `yarn init -y` |
| Instalar tudo (package.json) | `npm install` | `yarn install` |
| Adicionar Pacote | `npm install [pkg]` | `yarn add [pkg]` |
| Adicionar como Dev (-D) | `npm install [pkg] -D` | `yarn add [pkg] --dev` |
| Instalar Globalmente | `npm install -g [pkg]` | `yarn global add [pkg]` |
| Remover Pacote | `npm uninstall [pkg]` | `yarn remove [pkg]` |
| Executar Script (start/dev) | `npm run [nome]` | `yarn [nome]` |
| Auditar vulnerabilidades | `npm audit fix` | `yarn audit` |
| Rodar s/ instalar (ex: React) | `npx create-react-app` | - |
| Limpar Cache | `npm cache clean --force` | `yarn cache clean` |

---

# 🔄 NVM (Node Version Manager)

Essencial para desenvolvedores que trabalham em vários projetos que exigem versões diferentes do Node.js.

| Comando | Descrição |
| :--- | :--- |
| `nvm install [versão]` | Instala uma versão específica (ex: `nvm install 18.16.0`) |
| `nvm use [versão]` | Ativa a versão informada para o terminal atual |
| `nvm ls` | Lista todas as versões instaladas localmente |
| `nvm ls-remote` | Lista todas as versões do Node disponíveis para download |
| `nvm alias default [v]` | Define qual versão inicia automaticamente com o terminal |
| `nvm current` | Mostra qual versão está ativa agora |
| `nvm uninstall [v]` | Remove uma versão instalada |

---

# 📝 Variáveis de Ambiente Comuns (Setup)

No Java e Node, é comum configurar variáveis de sistema para que os comandos funcionem globalmente:

* **JAVA_HOME**: Caminho da pasta de instalação do JDK.
* **MAVEN_HOME**: Caminho da pasta do Maven.
* **NODE_ENV**: Define o ambiente (`development` ou `production`).
* **PATH**: Deve conter `%JAVA_HOME%\bin` e o caminho dos binários do Node.
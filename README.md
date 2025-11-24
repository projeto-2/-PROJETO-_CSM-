# Projeto CSM – TaskManager

Aplicação web para gerenciamento de tarefas pessoais com login, CRUD de tarefas, relacionamento com categorias e controle de acesso por perfis.

O projeto do grupo será o desenvolvimento de uma aplicação web para gerenciamento de tarefas pessoais. Público alvo: estudantes, profissionais e qualquer pessoa que deseje organizar suas atividades do dia a dia. Objetivo: oferecer uma ferramenta simples e intuitiva para criar, editar e excluir tarefas, além de marcar atividades concluídas, permitindo maior organização, produtividade e controle das rotinas pessoais

## 👥 Equipe

| Nome                         | Formação                                 | Linguagens/Ferramentas   |
|------------------------------|------------------------------------------|---------------------------|
| Gabriel Cacique Silva      | Análise e Desenvolvimento de Sistemas    | Java, HTML|
| João Pedro Duque de Souza    | Análise e Desenvolvimento de Sistemas    | Java, HTML   |

## Como executar

- Pré-requisitos: Java 17+, Maven
- Comandos:
  - `mvn -q -DskipTests package`
  - `mvn -q spring-boot:run`
- Acesse:
  - `http://localhost:8080/` (Home)
  - `http://localhost:8080/login` (Login)
  - `http://localhost:8080/welcome` (após login)
  - `http://localhost:8080/tasks` (CRUD de Tarefas)
  - `http://localhost:8080/categories` (Categorias)
  - `http://localhost:8080/h2-console` (Console do H2)

Credenciais iniciais:
- admin / admin123
- user / user123

## Entidade escolhida e justificativa

- Entidade central: `Tarefas`
- Justificativa: é o núcleo funcional do projeto de organização pessoal. Permite listar, criar, visualizar, editar e excluir atividades do dia a dia.

## Tecnologias/Linguagens utilizadas

- Backend: Java 17, Spring Boot 3
- Frontend: HTML
- Banco: H2
- Build: Maven

## Páginas implementadas

- Home com acesso ao login
- Login com feedback de erro
- Bem vindo após autenticação
- CRUD de Tarefas: listar, criar, detalhes, editar, deletar
- Categorias: listar e criar

## Modelagem do Banco

- Tabela `users`
  - `id` bigint auto
  - `username` varchar único
  - `email` varchar único
  - `password` varchar (BCrypt)
  - `role` varchar (`ADMIN`, `EDITOR`, `VIEWER`)
  - `created_at` timestamp
- Tabela `categories`
  - `id` bigint auto
  - `name` varchar único
- Tabela `tasks`
  - `id` bigint auto
  - `title` varchar
  - `description` varchar(2000)
  - `due_date` date
  - `completed` boolean
  - `category_id` fk `categories(id)`
  - `owner_id` fk `users(id)`

Arquivos de referência:
- `src/main/java/com/csm/taskmanager/model/User.java`
- `src/main/java/com/csm/taskmanager/model/Category.java`
- `src/main/java/com/csm/taskmanager/model/Task.java`

## Integração de Login

- Autenticação de formulário (`/login`)
- Checagem de usuário e senha via `UserDetailsService`
- Feedback de erro em `login?error=true`
- Redireciona para `/welcome` após sucesso

Referências de código:
- `src/main/java/com/csm/taskmanager/config/SecurityConfig.java`
- `src/main/java/com/csm/taskmanager/service/CustomUserDetailsService.java`
- `src/main/java/com/csm/taskmanager/controller/AuthController.java`

## Controle de Acesso por Perfis

- `ADMIN` pode excluir tarefas
- `EDITOR` e `ADMIN` podem criar/editar
- Leitura liberada após login

Referência:
- `src/main/java/com/csm/taskmanager/controller/TaskController.java`

## Validações e Segurança

- Validações de campos com Bean Validation
- Senhas armazenadas com BCrypt
- Sem concatenação de SQL; uso de JPA

## Desafios e como foram superados

- Integração de autenticação: uso de Spring Security com `UserDetailsService`, encoder configurado e seeds automáticos.
- Relacionamento e seleção de categoria: modelagem ManyToOne e dropdown em formulário Thymeleaf.
- Restrição por perfil: `@PreAuthorize` e habilitação de segurança em métodos.

## Deploy

- Escolha sugerida: Render
- Passos:
  - Criar serviço Web no Render
  - Build: `./mvnw -DskipTests package` ou `mvn -q -DskipTests package`
  - Start: `java -jar target/taskmanager-0.0.1-SNAPSHOT.jar`
  - Variáveis de ambiente:
    - `SPRING_DATASOURCE_URL` com Postgres, ex: `jdbc:postgresql://HOST:PORT/DB`
    - `SPRING_DATASOURCE_USERNAME`
    - `SPRING_DATASOURCE_PASSWORD`
    - `SPRING_JPA_HIBERNATE_DDL_AUTO=update`

Para Postgres, adicionar dependência no `pom.xml` em produção e configurar as variáveis acima.

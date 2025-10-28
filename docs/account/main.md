# 🗃️ Módulo de Contas (Account API)

O serviço **Account API** é o pilar para o gerenciamento de contas de usuário dentro do domínio de e-commerce (`store`).

Este microsserviço realiza todas as **operações CRUD** (Criar, Ler, Atualizar, Excluir) essenciais para a **gestão de contas**, estabelecendo a fundação necessária para a **autenticação** e o relacionamento com outros serviços do ecossistema, como `auth`, `order` e `product`.

* **Criação** (Cadastro)
* **Busca** (Consulta)
* **Atualização**
* **Exclusão**

> 🔒 **Camada de Confiança (*Trusted Layer*) e Roteamento Seguro**
>
> Todas as comunicações externas passam obrigatoriamente pelo **Gateway** da aplicação. As rotas sob `/account/**` são **protegidas por *token***, exigindo o envio do cabeçalho de autenticação: `Authorization: Bearer <jwt>`.

---

## 🏛️ Componentes e Estrutura

A Account API é dividida em dois submódulos principais, garantindo a separação de responsabilidades:

| Módulo | Responsabilidade | Tecnologias Chave |
| :--- | :--- | :--- |
| **`account`** | **Interface/Contrato** - Define o contrato (DTOs e Interfaces Feign) que será consumido por outros serviços e aplicações *front-end*. | DTOs, Feign |
| **`account-service`** | **Implementação Principal** - Contém a lógica de negócio, a camada REST, persistência de dados e *scripts* de migração de banco de dados. | REST, JPA, Flyway |

```mermaid
flowchart TD
    A[account: Contrato e DTOs] --> B{account-service: Lógica de Negócio};
    B --> C[JPA: Persistência];
    B --> D[Flyway: Migrações de DB];
    E[Outros Módulos] --> A;
    A -- Contrato (DTOs/Feign) --> B;
````

-----

## 🌐 Fluxo de Comunicação

A comunicação com a API segue um fluxo de segurança e processamento em camadas:

```mermaid
graph TD
    internet[🌍 Internet] --> gateway{🛡️ Gateway};
    gateway --> accountService[⚙️ account-service];
    accountService --> db[💾 Database (JPA/Flyway)];
```

## 📂 Módulo de Contrato (account)

Este módulo expõe a interface e os Data Transfer Objects (DTOs) para os consumidores externos.

```tree
api/
    account/
        src/
            main/
                java/
                    store/
                        account/
                            AccountController.java
                            AccountIn.java
                            AccountOut.java
        pom.xml
        Jenkinsfile
```

??? info "Source"
\=== "pom.xml"

```{ .xml .copy .select linenums='1' title="pom.xml" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account/refs/heads/main/pom.xml"
```

\=== "Jenkinsfile"

```{ .jenkinsfile .copy .select linenums='1' title="Jenkinsfile" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account/refs/heads/main/Jenkinsfile"
```

\=== "AccountController"

```{ .java .copy .select linenums='1' title='AccountController.java' }
--8<-- "https://raw.githubusercontent.com/Lagoass/account/refs/heads/main/src/main/java/store/account/AccountController.java"
```

\=== "AccountIn"

```{ .java .copy .select linenums='1' title='AccountIn.java' }
--8<-- "https://raw.githubusercontent.com/Lagoass/account/refs/heads/main/src/main/java/store/account/AccountIn.java"
```

\=== "AccountOut"

```{ .java .copy .select linenums='1' title='AccountOut.java' }
--8<-- "https://raw.githubusercontent.com/Lagoass/account/refs/heads/main/src/main/java/store/account/AccountOut.java"
```

```{ bash }
> mvn clean install
```

## ⚙️ Módulo de Serviço (account-service)

Onde a lógica de negócio e a persistência de dados são implementadas.

```tree
api/
    account-service/
        k8s/
            k8s.yaml
        src/
            main/
                java/
                    store/
                        account/
                            Account.java
                            AccountApplication.java
                            AccountModel.java
                            AccountParser.java
                            AccountRepository.java
                            AccountResource.java
                            AccountService.java
                resources/
                    application.yaml
                    db/
                        migration/
                            V2025.08.29.001__create_schema.sql
                            V2025.08.29.002__create_table_account.sql
                            V2025.09.02.001__create_index_email.sql
        pom.xml
        Dockerfile
        Jenkinsfile
```

??? info "Source"

\=== "pom.xml"

```{ .xml .copy .select linenums='1' title="pom.xml" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/pom.xml"
```

\=== "Dockerfile"

```{ .dockerfile .copy .select linenums='1' title="Dockerfile" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/DockerFile"
```

\=== "Jenkinsfile"

```{ .jenkinsfile .copy .select linenums='1' title="Jenkinsfile" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/Jenkinsfile"
```

\=== "k8s.yaml"

```{ .yaml .copy .select linenums='1' title="k8s.yaml" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/k8s/k8s.yaml"
```

\=== "application.yaml"

```{ .yaml .copy .select linenums='1' title="application.yaml" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/resources/application.yaml"
```

\=== "Account.java"

```{ .java .copy .select linenums='1' title="Account.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/Account.java"
```

\=== "AccountApplication.java"

```{ .java .copy .select linenums='1' title="AccountApplication.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/AccountApplication.java"
```

\=== "AccountModel.java"

```{ .java .copy .select linenums='1' title="AccountModel.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/AccountModel.java"
```

\=== "AccountParser.java"

```{ .java .copy .select linenums='1' title="AccountParser.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/AccountParser.java"
```

\=== "AccountRepository.java"

```{ .java .copy .select linenums='1' title="AccountRepository.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/AccountRepository.java"
```

\=== "AccountResource.java"

```{ .java .copy .select linenums='1' title="AccountResource.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/AccountResource.java"
```

\=== "AccountService.java"

```{ .java .copy .select linenums='1' title="AccountService.java" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/java/store/account/AccountService.java"
```

\=== "V2025.08.29.001\_\_create\_schema.sql"

```{ .sql .copy .select linenums='1' title="V2025.08.29.001__create_schema.sql" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/resources/db/migration/V2025.08.29.001__create_schema.sql"
```

\=== "V2025.08.29.002\_\_create\_table\_account.sql"

```{ .sql .copy .select linenums='1' title="V2025.08.29.002__create_table_account.sql" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/resources/db/migration/V2025.08.29.002__create_table_account.sql"
```

\=== "V2025.09.02.001\_\_create\_index\_email.sql"

```{ .sql .copy .select linenums='1' title="V2025.09.02.001__create_index_email.sql" }
--8<-- "https://raw.githubusercontent.com/Lagoass/account-service/refs/heads/main/src/main/resources/db/migration/V2025.09.02.001__create_index_email.sql"
```

```{ bash }
> mvn clean package spring-boot:run
```
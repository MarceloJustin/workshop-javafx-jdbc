# Workshop JavaFX JDBC

![Java](https://img.shields.io/badge/Java-11%2B-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![JavaFX](https://img.shields.io/badge/JavaFX-11%2B-2196F3?style=flat-square)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat-square&logo=mysql&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

## 📌 Sobre o Projeto

Aplicação desktop de gerenciamento de **Vendedores** e **Departamentos**, desenvolvida como exercício prático do curso **Java COMPLETO** do professor Nélio Alves. O projeto aplica na prática os conceitos de JavaFX para interface gráfica e JDBC puro para persistência em banco de dados relacional, sem uso de frameworks ORM ou Maven.

## 🚀 Tecnologias Utilizadas

- **Java 11+**
- **JavaFX 11+** — interface gráfica (FXML + controllers)
- **JDBC** — acesso ao banco de dados sem ORM
- **MySQL 8.0** — banco de dados relacional
- **Eclipse IDE** com plugin **e(fx)clipse**

## 🏗️ Arquitetura

O projeto segue uma arquitetura em camadas bem definida:

```
src/
├── application/       # Ponto de entrada (Main.java)
├── gui/               # Controllers JavaFX + arquivos FXML
│   ├── listeners/     # Interface DataChangeListener (padrão Observer)
│   └── util/          # Utilitários de UI (Alerts, Constraints, Utils)
├── model/
│   ├── entities/      # Entidades de domínio (Seller, Department)
│   ├── dao/           # Interfaces DAO + DaoFactory
│   │   └── impl/      # Implementações JDBC (SellerDaoJDBC, DepartmentDaoJDBC)
│   ├── services/      # Camada de serviço (SellerService, DepartmentService)
│   └── exceptions/    # Exceções de domínio (ValidationException)
└── db/                # Utilitário de conexão com o banco (DB.java)
```

**Fluxo:** `gui (Controller)` → `model/services` → `model/dao/impl` → `db (JDBC/MySQL)`

A comunicação entre formulários e listas usa o padrão **Observer** via `DataChangeListener`, evitando acoplamento direto entre controllers.

## 📋 Funcionalidades

**Departamentos**
- Listagem em TableView com colunas ID e Nome
- Cadastro e edição via formulário modal
- Exclusão com confirmação de diálogo

**Vendedores**
- Listagem em TableView com ID, Nome, E-mail, Data de Nascimento e Salário Base
- Cadastro e edição via formulário modal com seleção de Departamento (ComboBox)
- Data formatada como `dd/MM/yyyy` e salário com 2 casas decimais
- Exclusão com confirmação de diálogo

## 📦 Regras de Negócio

| Regra | Detalhe |
|---|---|
| Campos obrigatórios (Vendedor) | Nome, E-mail, Data de Nascimento e Salário Base são obrigatórios |
| Campo obrigatório (Departamento) | Nome é obrigatório |
| Confirmação de exclusão | Exibe diálogo de confirmação antes de deletar qualquer registro |
| Integridade referencial | Ao tentar excluir um Departamento com Vendedores vinculados, a operação é bloqueada e uma mensagem de erro é exibida |
| Novo vs. edição | `saveOrUpdate` determina INSERT (id nulo) ou UPDATE (id preenchido) automaticamente |

## ⚠️ Tratamento de Erros

| Exceção | Origem | Comportamento na UI |
|---|---|---|
| `ValidationException` | Campos inválidos no formulário | Exibe mensagem de erro inline abaixo de cada campo inválido |
| `DbException` | Falha na conexão ou na query SQL | Alerta de erro (Alert JavaFX) |
| `DbIntegrityException` | Violação de chave estrangeira ao excluir | Alerta de erro informando que o registro está em uso |

## 🧪 Testes

O projeto não possui testes automatizados — trata-se de um exercício de curso com foco em aprendizado de JavaFX e JDBC.

## ▶️ Como Executar

### Pré-requisitos

- **JDK 11+** instalado
- **JavaFX SDK 11+** baixado separadamente ([gluonhq.com/products/javafx](https://gluonhq.com/products/javafx/))
- **MySQL 8.0** rodando localmente
- **Eclipse IDE** com plugin e(fx)clipse (para compilar o projeto)

### 1. Banco de dados

Crie o banco e as tabelas no MySQL:

```sql
CREATE DATABASE coursejdbc;
USE coursejdbc;

CREATE TABLE department (
  Id   INT PRIMARY KEY AUTO_INCREMENT,
  Name VARCHAR(60)
);

CREATE TABLE seller (
  Id           INT PRIMARY KEY AUTO_INCREMENT,
  Name         VARCHAR(60) NOT NULL,
  Email        VARCHAR(60) NOT NULL,
  BirthDate    DATETIME    NOT NULL,
  BaseSalary   DOUBLE      NOT NULL,
  DepartmentId INT         NOT NULL,
  FOREIGN KEY (DepartmentId) REFERENCES department(Id)
);
```

### 2. Configuração

Edite o arquivo `db.properties` na raiz do projeto com suas credenciais:

```properties
user=seu_usuario
password=sua_senha
dburl=jdbc:mysql://localhost:3306/coursejdbc
useSSL=false
```

### 3. Compilar

Abra o projeto no **Eclipse IDE** e execute **Build Project** (`Ctrl+B`). O Eclipse gerará os `.class` na pasta `bin/`.

### 4. Executar

Defina a variável de ambiente `PATH_TO_FX` apontando para a pasta `lib` do seu JavaFX SDK:

```bat
set PATH_TO_FX=C:\caminho\para\javafx-sdk-11\lib
```

Em seguida, execute o script na raiz do projeto:

```bat
myapp.bat
```

Ou rode manualmente:

```bat
java --module-path %PATH_TO_FX% --add-modules javafx.controls,javafx.fxml -cp bin application.Main
```

## 👨‍💻 Autor

**Marcelo Justin**

[![GitHub](https://img.shields.io/badge/GitHub-MarceloJustin-181717?style=flat-square&logo=github)](https://github.com/MarceloJustin)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-marcelojustin-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/marcelojustin)

## 📄 Licença

Este projeto está licenciado sob a licença MIT. Consulte o arquivo [LICENSE](LICENSE) para mais detalhes.

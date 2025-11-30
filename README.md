
# 🤖 ATVIII-Autobots (Automanager)

O **Automanager Enterprise** é uma evolução do sistema de gestão de clientes, expandido para atuar como um micro-serviço completo de gestão de oficinas e concessionárias. Desenvolvido em Java com Spring Boot, este projeto permite a administração de **Empresas**, gerindo os seus colaboradores, fornecedores, clientes, inventário (mercadorias), serviços prestados e vendas realizadas.

Este projeto continua a implementar o **Nível 3 do Modelo de Maturidade de Richardson (HATEOAS)** e utiliza **Lombok** para simplificação das classes de domínio.

## ✨ Funcionalidades Principais

O sistema gere as seguintes entidades principais através de uma API REST:

  * **🏢 Empresas:** Entidade raiz que agrupa todos os recursos.
  * **👥 Utilizadores:** Sistema polimórfico que gere diferentes perfis:
      * **Funcionários:** Equipa interna.
      * **Clientes:** Consumidores finais.
      * **Fornecedores:** Parceiros de negócio.
  * **📦 Mercadorias:** Gestão de stock de peças e produtos.
  * **🛠️ Serviços:** Catálogo de serviços oferecidos (ex: Alinhamento, Troca de Rodas).
  * **💰 Vendas:** Registo de transações que vinculam Clientes, Funcionários, Veículos, Mercadorias e Serviços.
  * **🚗 Veículos:** Gestão da frota dos clientes.

## 🛠️ Tecnologias Utilizadas

  * **Java 21:** Versão LTS mais recente.
  * **Spring Boot 3.2.0:** Framework principal.
  * **Spring Data JPA:** Persistência de dados com mapeamento relacional complexo (`@OneToMany`, `@ManyToOne`, `@Inheritance`).
  * **Spring HATEOAS:** Navegabilidade da API via links (`_links`).
  * **Project Lombok:** Biblioteca para gerar automaticamente Getters, Setters, Construtores e métodos `equals`/`hashCode`.
  * **H2 Database:** Banco de dados em memória.
  * **Maven:** Gestão de dependências.

## 🚀 Como Executar o Projeto

### Pré-requisitos

1.  **Java JDK 21** instalado e variável `JAVA_HOME` configurada.
2.  **Git** instalado.
3.  **(Opcional)** O seu IDE (Eclipse/IntelliJ/VSCode) deve ter suporte ao **Lombok** instalado para reconhecer as anotações.

### Passo a Passo

1.  **Clonar o repositório:**

    ```bash
    git clone https://github.com/lucasguerra12/ATVIII-Autobots.git
    cd ATVIII-Autobots
    ```

2.  **Compilar e Executar:**
    Utilize o Maven Wrapper incluído para garantir a compatibilidade.

    **No Windows:**

    ```cmd
    .\mvnw.cmd spring-boot:run
    ```

    **No Linux/macOS:**

    ```bash
    ./mvnw spring-boot:run
    ```

3.  **Aceder à Aplicação:**

      * API Base: `http://localhost:8080`
      * Consola H2: `http://localhost:8080/h2-console`
          * **JDBC URL:** `jdbc:h2:mem:testdb`
          * **User:** `sa`
          * **Password:** `password`

## 📡 Endpoints da API

Abaixo estão os principais pontos de entrada. Graças ao HATEOAS, podes navegar pelos recursos através dos links retornados.

### 🏢 Empresa (`/empresa`)

| Método | Rota | Descrição |
| :--- | :--- | :--- |
| `GET` | `/empresa` | Lista todas as empresas e os seus dados (usuarios, stock, etc). |
| `GET` | `/empresa/{id}` | Obtém os detalhes de uma empresa específica. |
| `POST` | `/empresa/cadastro` | Cria uma nova empresa. |
| `PUT` | `/empresa/atualizar/{id}` | Atualiza dados da empresa (Razão Social, Nome Fantasia). |
| `DELETE` | `/empresa/excluir/{id}` | Remove uma empresa e todos os dados associados (cascade). |

### 👥 Utilizador (`/usuario`)

| Método | Rota | Descrição |
| :--- | :--- | :--- |
| `GET` | `/usuario/usuarios` | Lista todos os utilizadores (independente da empresa). |
| `GET` | `/usuario/{id}` | Obtém um utilizador específico e os seus detalhes. |
| `PUT` | `/usuario/atualizar/{id}` | Atualiza dados de um utilizador (Nome, Perfis, etc). |
| `DELETE` | `/usuario/excluir/{id}` | Remove um utilizador e desassocia-o das empresas. |

## 🧪 Dados Iniciais (Seed Data)

Ao iniciar a aplicação, a classe `AutomanagerApplication` carrega automaticamente um cenário de teste completo no banco de dados:

1.  **Empresa:** "Car service toyota ltda".
2.  **Funcionário:** "Dom Pedro" (com endereço, telefone, email, credenciais).
3.  **Fornecedor:** "Componentes varejo...".
4.  **Mercadoria:** "Roda de liga leve".
5.  **Serviços:** Troca de rodas e Alinhamento.
6.  **Cliente:** "Dom Pedro Cliente" (com veículo Corolla Cross).
7.  **Vendas:** Duas vendas registadas vinculando todas as entidades acima.

## 📦 Exemplo de JSON

### Atualizar Empresa (`PUT /empresa/atualizar/{id}`)

```json
{
  "razaoSocial": "Nova Razão Social Ltda",
  "nomeFantasia": "Auto Center Premium"
}
```

### Estrutura de Resposta (HATEOAS)

```json
{
  "id": 1,
  "nome": "Pedro Alcântara",
  "perfis": ["FUNCIONARIO"],
  "_links": {
    "self": {
      "href": "http://localhost:8080/usuario/1"
    },
    "usuarios": {
      "href": "http://localhost:8080/usuario/usuarios"
    }
  }
}
```

-----

**Desenvolvido como atividade académica.**

---
marp: true
footer: 'out - 2023 - Educafro Tech - C# - por Roberto de Oliveira Santos'
---
<style>
section {
    justify-content: start;
}

img[alt$="<"] {
    float: left;
    margin-right: 2em;
    }

img[alt$="center"] {
    display: block;
    margin: 0 auto;
    }
</style>

<style scoped>section { justify-content: center; }</style>

# Educafro Tech
## Curso C# - Do Básico ao MVC
### Aula 11
---
# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. **Banco de Dados e Entity Framework**
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 5. Banco de Dados e Entity Framework
#### Introdução a banco de Dados

---

#### Conceitos Básicos

> Bancos de dados ou bases de dados são conjuntos de arquivos relacionados entre si, podendo conter registros sobre pessoas, lugares ou informações em geral. São coleções organizadas de dados que se relacionam ou não, de forma a armazenar informações.

Ou seja, nossos funcionários, cargos e avaliações precisam ser guardados em algum lugar. Esse lugar será um banco de dados SQLite. 

---

#### SQLite

> SQLite é uma biblioteca em linguagem C que implementa uma base de dados SQL embutida. Programas que usem a biblioteca SQLite podem ter acesso a banco de dados SQL sem executar um processo SGBD separado.


Existem muitos outros bancos de dados, mas vamos utilizar o SQLite por sua simplicidade.

Para conhecer outros bancos de dados, acesse: https://awari.com.br/sistemas-de-banco-de-dados/

---

#### SQL

> Structured Query Language, lit. "linguagem de consulta estruturada", é uma linguagem de domínio específico desenvolvida para gerenciar dados relacionais em um sistema de gerenciamento de banco de dados, ou para processamento de fluxo de dados em um sistema de gerenciamento de fluxo de dados.


Para efeito de conhecimento existe uma linguagem específica para conversar com bancos de dados. E ela é o SQL.

Porém, para sua sorte, que está iniciando seus estudos nesse momento, o .Net Framework tem um recurso que vai te permitir ignorar o SQL por um tempo e, mesmo assim, conseguir conversar com muitos bancos de dados.

---

#### Entity Framework

> O Entity Framework é um mapeador moderno de relação de objetos que permite criar uma camada de acesso a dados limpa, portátil e de alto nível com o .NET (C#) em vários bancos de dados, incluindo o Banco de Dados SQL (local e o Azure), SQLite, MySQL, PostgreSQL e Azure Cosmos DB. Ele dá suporte a consultas LINQ, controle de alterações, atualizações e migrações de esquema.

Isso mesmo, o .Net possui o Entity Framework. Ele permite conversar com o banco de dados usando classes C#. E a forma de fazer consultas é com o LINQ, que na prática é um conjunto de métodos que fazem funções como: Consultar, Filtrar, Ordenar...

---

#### Entity Framework

Para configurar o Entity Framework vamos usar as classes que criamos na ```Model``` do nosso projeto MVC.

Como nós criamos um projeto MVC com autenticação, lembra?

```dotnet new mvc -au Individual```

O .Net já incluiu um banco de dados SQLite no nosso projeto e já temos a estrutura base do Entity Framework.

---

#### Entity Framework

Na pasta, ```Data```, abra o arquivo ```ApplicationDbContext.cs```.

Antes do último ```}```, insira as seguintes propriedades.

Cada uma delas, com exceção de ```Gerente``` vão ser uma tabela no nosso banco de dados. ```Gerente``` vai ser apenas um campo na tabela ```Funcionarios``` para indicar que tipo aquele registro é.

```csharp
    public DbSet<Funcionario> Funcionarios { get; set; }
    public DbSet<Avaliacao> Avaliacoes { get; set; }
    public DbSet<Cargo> Cargos { get; set; }
    public DbSet<Gerente> Gerentes { get; set; }
```

---

#### Entity Framework

Estamos usando o EF no formato ***Code-First***. Ou seja, primeiro estamos construindo o código, para depois atualizar nosso banco de dados.

Primeiramente instale a ferramenta, executando o comando abaixo no terminal:

```
dotnet tool install --global dotnet-ef --version 6.0.27
```

Agora vamos criar uma ***migration***. Que basicamente são as instruções de atualização do banco de dados.

```
dotnet ef migrations add VersaoInicial
```

---

#### Entity Framework

```VersaoInicial``` é o nome dessa atualização. Poderia ser qualquer nome.

E depois vamos executar o comando que atualiza o banco de dados:

```
dotnet ef database update
```

No terminal, o resultado que você está vendo, é o código SQL que o próprio EntityFramework gerou para você, através das suas classes.

---

#### Entity Framework

Ex.:

```
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      SELECT "MigrationId", "ProductVersion"
      FROM "__EFMigrationsHistory"
      ORDER BY "MigrationId";
info: Microsoft.EntityFrameworkCore.Migrations[20402]
      Applying migration '20240124190402_VersaoInicial'.
Applying migration '20240124190402_VersaoInicial'.
info: Microsoft.EntityFrameworkCore.Database.Command[20101]
      Executed DbCommand (0ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE "Cargos" (
          "Id" INTEGER NOT NULL CONSTRAINT "PK_Cargos" PRIMARY KEY AUTOINCREMENT,
          "Nome" TEXT NOT NULL,
          "Nivel" INTEGER NOT NULL
      );
```

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





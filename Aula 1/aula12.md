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
### Aula 12
---
# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados e Entity Framework
6. **Construção de um Aplicativo Web MVC**
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 6. Construção de um Aplicativo Web MVC
#### Desenvolvendo a lógica de negócio

---

#### Introdução

Como já falei, na vida real, uma área da empresa ou um cliente trazem uma série de regras e funcionalidades que sua aplicação deve ter.

Essas regras e funcionalidades são conhecidas como : lógica de negócio ou regras de negócio.

Lembra dos métodos que criamos com:

```csharp
throw new NotImplementedException();
```

É hora de retirar esse código e construir um real.

Vamos começar pela classe ```Funcionario```.

---

#### Listar

A primeira parte é muito simples.

> 6. Listagens: Todos podem consultar funcionários ativos, porém, somente o gerente pode ver funcionários demitidos.

Isso significa que, na classe ```Funcionario```, o método público ```Listar``` vai simplesmente chamar o método ```protected``` de mesmo nome.

Isso é uma prática muito normal. Deixar público a versão mais simples e controlada de um método e ter internamente uma versão mais complexa, para evitar erros de desenvolvimento que possam quebrar regras de negócio.

O nome disso é **sobrecarga**. Um mesmo método com encapsulamentos e/ou parâmetros diferentes.

---

#### Listar

Então temos:

```csharp
public virtual List<Funcionario> Listar(){
    return Listar(true);
}

protected List<Funcionario> Listar(bool somenteAtivos){
    throw new NotImplementedException();
}

```

Onde o primeiro método ```Listar``` chama o segundo, sempre passando ```true``` como parâmetro.

Mas e o segundo?

---

#### Listar

Agora vem o EF e o LINQ. Para conversarmos com o banco de dados, vamos precisar de um ```DbContext```. Ou seja, um contexto de banco de dados. (Db vem de *database* que significa banco de dados.)

Para isso, vamos usar um outro recurso do C# e da OOP. O **Construtor**.

O construtor nada mais é que um método, sem retorno (nem mesmo **void**) com o mesmo nome que a classe.

---

#### Listar

Nesse caso:

```csharp
// Variável interna para armazenar o nosso contexto de banco de dados
private IdentityDbContext _db;

// Construtor recebendo o contexto como parâmetro
public Funcionario(IdentityDbContext db) {
    _db = db;
}
```

Agora que temos um contexto dentro da nossa classe, podemos seguir com o desenvolvimento da função ```protected Listar```.

---

#### Listar

Ela fica assim:

```csharp

```

---

#### Entity Framework

Estamos usando o EF no formato ***Code-First***. Ou seja, primeiro estamos construindo o código, para depois atualizar nosso banco de dados.

Primeiramente instale a ferramenta, executando o comando abaixo no terminal:

```
dotnet tool install --global dotnet-ef
```

Se você receber o erro abaixo:

<font size="5">

```
You must install or update .NET to run this application.

App: /home/vscode/.dotnet/tools/dotnet-ef
Architecture: x64
Framework: 'Microsoft.NETCore.App', version '8.0.1' (x64)
.NET location: /usr/share/dotnet

The following frameworks were found:
  8.0.0 at [/usr/share/dotnet/shared/Microsoft.NETCore.App]
```

</font>

---

#### Entity Framework

Desinstale a ferramenta

```
dotnet tool uninstall --global dotnet-ef
```

E execute novamente com a mesma versão que aparece na última linha. Nesse exemplo: 8.0.0

```
dotnet tool install --global dotnet-ef --version 8.0.0
```

Agora vamos criar uma ***migration**. Que basicamente são as instruções de atualização do banco de dados.

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





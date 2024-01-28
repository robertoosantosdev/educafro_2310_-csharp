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
// Variável compartilhada com filhas para armazenar o nosso contexto de banco de dados
protected IdentityDbContext _db;

// Construtor recebendo o contexto como parâmetro
public Funcionario(IdentityDbContext db) {
    _db = db;
}
```

Como a classe ```Funcionario``` possui filhas. Precisamos criar o construtor nelas também. O construtor da classe ```Gerente``` fica assim:

```csharp
public Gerente(ApplicationDbContext db) : base(db)
    { }
```

Apenas chamando o construtor da classe mãe.

---

#### Listar

Agora que temos um contexto dentro da nossa classe, podemos seguir com o desenvolvimento da função ```protected Listar```.

```csharp
protected List<Funcionario> Listar(bool somenteAtivos)
{
    // Variável do tipo lista de funcionários que vai armazenar o retorno do método
    List<Funcionario> funcionarios;
    // Verificação se devemos filtrar somente funcionários ativos
    if (somenteAtivos)
        // Filtro de funcionários ativos
        funcionarios = _db.Funcionarios.Where(funcionario => funcionario.Ativo == true).ToList();
    else
        // Listagem sem filtro
        funcionarios = _db.Funcionarios.ToList();
    // Retorno da função
    return funcionarios;
}
```

---

#### Listar

Vamos por partes.

A consulta ao banco de dados é feita através da variável ```_db```. Graças ao EF, ela possui todas as nossas classes da **Model** como propriedade.

Como queremos listar ```Funcionarios```, podemos usar ```_db.Funcionarios```.

Para filtrar usamos a função ```Where```, que significa **onde**. Então vamos dizer algo do tipo, me traga todos os ```Funcionarios```, **ONDE** a propriedade ```Ativo``` seja igual a ```true``` ou seja, **verdeiro**.

Já quando a função for chamada com ```false``` ou seja, **falso**, vamos apenas pedir ao banco, todos os ```Funcionarios```.

Ao final, usamos a função ```ToList()``` para trasformar o resultado do banco numa lista.

---

#### Listar

Hora de testar!

No projeto de testes, mude o nome do arquivo ```UnitTest1.cs``` para ```FuncionarioTest.cs```. Mude também o nome da classe de ```UnitTest``` para ```FuncionarioTest```.
`
Quando vamos fazer testes de funções com banco de dados, alguns pontos são importantes:

1. Ao invés de usar um banco de dados de verdade, use um ***mock***, ou seja, um banco falso ou simulado.
2. Para garantir que você vai saber o resultado esperado dos seus testes, faça um cenário antes de iniciar os testes usando um **construtor**.

---

#### Listar
<font size="2">

```csharp
// Variável interna que compartilha o banco entre os testes.
    protected ApplicationDbContext _db;
    // Construtor que constroi nosso cenario.
    public FuncionarioTest()
    {
        // Aqui, criamos um banco falso (mock) em memória. Ou seja,
        // assim que o teste termina, ele desaparece.
        var connection = new SqliteConnection("Filename=:memory:");
        // Essa função abre a conexão com o banco.
        connection.Open();
        // O EF precisa de algumas opções de contexto para ser criado.
        // Usamos este Builder para nos ajudar no processo.
        var contextOptions = new DbContextOptionsBuilder<ApplicationDbContext>()
            .UseSqlite(connection)
            .Options;
        // Aqui iniciamos o EF com nosso banco de testes
        _db = new ApplicationDbContext(contextOptions);
        // Esta função garante que nossas tabelas existam no banco
        _db.Database.EnsureCreated();
        // Criamos um cargo de Vendedor somente para estes testes
        var cargoVendedor = new Cargo();
        cargoVendedor.Nome = "Vendedor";
        cargoVendedor.Nivel = 0;
        // Criamos um funcionário fictício
        var juarez = new Funcionario(_db);
        juarez.Nome = "Juarez Soares";
        juarez.Salario = 1000;
        juarez.Cargo = cargoVendedor;
        juarez.Email = "a@a.com.br";
        // Criamos um funcionário INATIVO fictício
        var inativo = new Funcionario(_db);
        inativo.Nome = "Jorge Matos";
        inativo.Salario = 900;
        inativo.Ativo = false;
        inativo.Cargo = cargoVendedor;
        inativo.Email = "j@m.com.br";
        // Adicionamos o funcionário ao banco de dados
        _db.Funcionarios.Add(juarez);
        _db.Funcionarios.Add(inativo);
        // Salvamos as alterações
        _db.SaveChanges();
    }
```

</font>


---


#### Listar

Já no nosso teste fica assim:

```csharp
public void ListarTest()
{
    var expected = 1;

    Funcionario funcionario = new Funcionario(_db);

    List<Funcionario> funcionarios = funcionario.Listar();

    var actual = funcionarios.Count;

    Assert.Equal(expected, actual);
}
``` 

---

#### Listar

Como nosso cenário feito no **construtor** tem 2 ```Funcionarios```, porém, somente 1 ativo, esperamos que a contagem (**Count**) resultado da função ```Listar``` seja igual a 1.

Vamos executar nossos testes via **Terminal** acessando a pasta de testes com ```cd src/abantu.tests/``` e depois ```dotnet watch test```.

**ATENÇÃO**: Em algumas versões recentes do .Net podem haver problemas na execução do ```dotnet watch```. Caso o resultado do teste não seja exibido, use ```dotnet test```.

O seu resultado deve ter sido esse:

---

#### Listar

```
[xUnit.net 00:00:01.75]     abantu.tests.FuncionarioTest.ListarTest [FAIL]
  Failed abantu.tests.FuncionarioTest.ListarTest [990 ms]
  Error Message:
   Assert.Equal() Failure
Expected: 1
Actual:   0
```

Veja, esperávamos que tivéssemos 1 funcionário ativo, porém, tivemos 0. O que pode ter acontecido? Sim! Esquecemos de definir a propriedade ```Ativo``` com o valor ```true``` para o ```juarez```.

Você poderia fazer isso no momento que cria o funcionário ```juarez```. Mas **sempre** que um novo funcionário é criado, ele deveria estar ativo. Então o melhor lugar para fazer isso é no **construtor** da classe ```Funcionario```.

---

#### Listar

O construtor atualizado fica assim:

 ```csharp
// Construtor recebendo o contexto como parâmetro
public Funcionario(ApplicationDbContext db)
{
    _db = db;
    // Sempre que um novo funcionário for criado, ele será ativo.
    Ativo = true;
}
 ```

---

#### Listar - Gerente

A função ```Listar``` do ```Gerente``` fica assim:

```csharp
public override List<Funcionario> Listar()
{
    return Listar(false);
}
```

Simples, não é mesmo? Ela apenas retorna o resultado da nossa função ```protected``` que está na classe mãe ```Funcionario``` com o parâmetro ```false```, pois um ```Gerente``` pode ver ```Funcionarios``` ativos e inativos.

---

#### Listar - Gerente

Agora, crie o arquivo ```GerenteTest.cs``` e assim como a classe ```Gerente``` herda de ```Funcionario```, a classe ```GerenteTest``` vai herdar de ```FuncionarioTest```.

Ela fica assim::

<font size="4">

```csharp
public class GerenteTest : FuncionarioTest
{
    public GerenteTest() : base()
    {
        // Criamos o cargo fictício de gerente
        var cargoGerente = new Cargo();
        cargoGerente.Nome = "Gerente";
        cargoGerente.Nivel = 1;
        // Criamos um gerente fictício
        var maria = new Funcionario(_db);
        maria.Nome = "Maria Antunes";
        maria.Salario = 2000;
        maria.Cargo = cargoGerente;
        maria.Email = "m@a.com.br";
        // Adicionamos ao banco criado na classe mãe
        _db.Funcionarios.Add(maria);
        // Salvamos
        _db.SaveChanges();
    }
```

</font>

---

#### Listar - Gerente

<font size="4">

```csharp
    [Fact]
    public override void ListarTest()
    {
        var expected = 3;

        Gerente gerente = new Gerente(_db);

        List<Funcionario> funcionarios = gerente.Listar();

        var actual = funcionarios.Count;

        Assert.Equal(expected, actual);
    }
}
```

</font>

O resultado dos testes fica assim:

```
Starting test execution, please wait...
A total of 1 test files matched the specified pattern.

Passed!  - Failed:     0, Passed:     2, Skipped:     0, Total:     2, Duration: 4 ms - abantu.tests.dll (net7.0)
```

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





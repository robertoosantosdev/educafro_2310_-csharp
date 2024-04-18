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
### Aula 13
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

#### Contratar

Agora vamos construir a função ```Contratar```. Já fizemos ela nos nossos testes. Basicamente precisamos:

1. Adicionar o ```novoFuncionario``` ao banco
2. Salvar as alterações

Vamos lá! Experimente criar essa função.

---

#### Contratar

Ela deve ter ficado assim:

```csharp
public Funcionario Contratar(Funcionario novoFuncionario)
{
    _db.Add(novoFuncionario);
    _db.SaveChanges();
    return novoFuncionario
}
```

Agora construa o teste dessa função!

O ```expected``` é que o ```Id``` do novo ```Funcionario``` seja 4.

---

#### Listar

Ele deve ter ficado assim:

<font size="4">

```csharp
[Fact]
public void ContratarTest() {
    var expected = 4;

    // Procuramos pelo primeiro gerente
    Gerente gerente = _db.Gerentes.First();
    // Procuramos pelo cargo de Vendedor.
    Cargo cargoVendedor = _db.Cargos.First(c => c.Nome == "Vendedor");
    
    var novoFuncionario = new Funcionario(_db);
    novoFuncionario.Nome = "Roberto Lira";
    novoFuncionario.Cargo = cargoVendedor;
    novoFuncionario.Salario = 1000;

    gerente.Contratar(novoFuncionario);

    var actual = novoFuncionario.Id;
    
    Assert.Equal(expected, actual);
}
```

</font>

---

#### Demitir

Agora vamos fazer a função ```Demitir```. Mas lembre-se:

> 2. Demissão: Somente um gerente pode demitir. Um funcionário só pode ser demitido se a média das suas avaliações for menor que 5.

Para calcular a média simples de algo, devemos:

1. Somar os valores
2. Dividir pela quantidade de valores

---

#### Demitir

Para separar as responsabilidades, podemos fazer uma função para calcular média:

<font size=5>

```csharp
private decimal CalcularMediaAvaliacoes(List<Avaliacao> avaliacoes)
{
    var quantidade = avaliacoes.Count;
    decimal total = 0;
    decimal media = 0;

    for (int i = 0; i < quantidade; i++)
    {
        // += significa pegar o valor de total e acrescentar o valor da nota da avaliação
        // na primeira execução, total é 0. Então teremos 0 + nota.
        // Ex.: total = 0 + 3
        // já na segunda execução do for, se a nota for 7
        // total = 3 + 7
        total += avaliacoes[i].Nota;
    }

    media = total / quantidade; 

    return media;
}
```

</font>

---

#### Demitir

Então, a função ```Demitir``` fica assim:

```csharp
public Funcionario Demitir(Funcionario funcionario)
{
    var mediaMinima = 5;
    
    var funcionarioDb = _db.Funcionarios.Single(f => f.Id == funcionario.Id);

    if (CalcularMediaAvaliacoes(funcionarioDb.Avaliacoes) < mediaMinima)
    {
        funcionarioDb.Ativo = false;

        _db.SaveChanges();
    }

    return funcionarioDb;
}
```

---

#### Demitir

Já os testes ficam assim:

```csharp
[Fact]
public void DemitirTest()
{
    var expected = false;

    Gerente gerente = _db.Gerentes.First();

    Funcionario funcionario = _db.Funcionarios.First(f => f.Ativo);

    var demitido = gerente.Demitir(funcionario);

    var actual = demitido.Ativo;

    Assert.Equal(expected, actual);
}
```

---

#### Demitir

Nesse teste, simplesmente pegamos o primeiro funcionário (que no nosso caso é o Juarez) e tentamos fazer sua demissão.

Mas nosso teste falha com o erro:

<font size="3">

```
System.NullReferenceException : Object reference not set to an instance of an object.
```
</font>

⚠️ Esse é um erro muito comum, lembre-se dele: ***Null*** (nula) ***Reference*** (referência) significa que você tentou usar algo de um objeto, mas esse objeto é nulo. Ou seja, não existe.

No nosso caso, tentamos calcular a média das avaliações, mas o Juarez não tem nenhuma avaliação.

<font size="3">

```csharp
var quantidade = avaliacoes.Count;
```

</font>

---

#### Demitir

Outro ponto muito comum é: Não recebemos todas as regras no começo do projeto.

Ex.: Se um funcionário for contratado e com 2 meses de trabalho, sem nenhuma validação o gerente quiser demiti-lo, ele pode?

Então, para nosso sistema, vamos usar a seguinte regra. Se o ```Funcionario``` não tiver ```Avaliacoes```, ele não pode ser demitido.

Consegue fazer essa regra?

Você vai precisar usar: ```if```, ```.Count``` e uma verificação para saber se as ```Avaliacoes``` são nulas: ```.Avaliacoes == null```. E por último, vai precisar usar o operador ```||``` ou. Ele significa que seu ```if``` vai avaliar duas condições. Se o funcionário não tiver avaliações (nulo) ou a quantidade de avaliações for zero, retornar uma mensagem de erro.

---

#### Demitir

Então ela fica assim:

```csharp
if (funcionario.Avaliacoes == null || funcionario.Avaliacoes.Count == 0)
            throw new ApplicationException("Funcionário não possui avaliações. É necessário uma média inferior a 5 para realizar uma demissão");
```

Então, para testar a demissão do Juarez, esperamos que o resultado seja uma exceção do tipo: ```ApplicationException```. Mude o nome o conteúdo da função para:

```csharp
[Fact]
public void NaoDemitirTest()
{
    Gerente gerente = _db.Gerentes.First();

    Funcionario funcionario = _db.Funcionarios.First(f => f.Ativo);

    Assert.Throws<ApplicationException>(() => gerente.Demitir(funcionario));
}
```

---

#### Demitir

Mas e uma demissão que realmente deveria acontecer?

Crie um novo teste: ```DemitirTest``` e faça você esse teste.

Adicione 2 avaliações ao Juarez. Uma com nota 6 e outra com nota 3.

Para definir a propriedade ```RealizadaEm```, use a função ```.RealizadaEm = DateTime.Now```

Para adicionar uma avaliação use:

```csharp
funcionario.Avaliacoes = new List<Avaliacoes()>
funcionario.Avaliacoes.Add(avaliacao6);
funcionario.Avaliacoes.Add(avaliacao3);
```

---

#### Demitir

<font size="2">

```csharp
[Fact]
    public void DemitirTest()
    {
        var expected = false;

        Gerente gerente = _db.Gerentes.First();
        Funcionario funcionario = _db.Funcionarios.First(f => f.Ativo == true);

        Avaliacao avaliacao6 = new Avaliacao();
        avaliacao6.Avaliado = funcionario;
        avaliacao6.Avaliador = gerente;
        avaliacao6.Nota = 6;
        avaliacao6.RealizadaEm = DateTime.Now;
        avaliacao6.Comentario = "Razoável";

        Avaliacao avaliacao3 = new Avaliacao();
        avaliacao3.Avaliado = funcionario;
        avaliacao3.Avaliador = gerente;
        avaliacao3.Nota = 3;
        avaliacao3.RealizadaEm = DateTime.Now;
        avaliacao3.Comentario = "Ruim";

        funcionario.Avaliacoes = new List<Avaliacao>();
        funcionario.Avaliacoes.Add(avaliacao6);
        funcionario.Avaliacoes.Add(avaliacao3);

        _db.SaveChanges();

        funcionario = gerente.Demitir(funcionario);

        var actual = funcionario.Ativo;

        Assert.Equal(expected, actual);
    }
```

</font>

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





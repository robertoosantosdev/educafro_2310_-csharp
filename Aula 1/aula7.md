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
### Aula 7
---
# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. **Programação Orientada a Objetos (POO)**
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados e Entity Framework
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 3. Programação Orientada a Objetos (POO)
#### Herança e Polimorfismo

---

#### Herança

>Herança é um princípio de orientação a objetos, que permite que classes compartilhem atributos e métodos, através de "heranças". Ela é usada na intenção de reaproveitar código ou comportamento generalizado ou especializar operações ou atributos.

Por enquanto, nossos ingredientes são apenas objetos do tipo ```string```, ou seja, texto.

Vamos mudar isso. Crie uma **classe pública** com o nome Ingrediente.

Essa classe vai ter propriedades (ou atributos)!

As propriedades serão: ```Nome``` e ```Marca```

---

#### Vamos criar propriedades

Para criar uma propriedade, usamos a seguinte estrutura:

```csharp
    private string _nome;
   
    public string Nome
    {
        get { return _nome; }
        set { _nome = value; }
    }
```
Primeiro temos uma **variável privada** sem valor inicial. No nosso caso: ```private string nome```.

E depois temos a forma **pública** de acessar essa variável com o ```get``` e o ```set```.

---

#### Vamos criar propriedades

Então, no final, sua classe deve estar assim:

```csharp
public class Ingredente
{
    private string _nome;
    public string Nome
    {
        get { return _nome; }
        set { _nome = value; }
    }

    private string _marca;
    public string Marca
    {
        get { return _marca; }
        set { _marca = value; }
    }
}
```

---

#### Vamos criar um método

Agora vamos incluir um método para preparar nosso ingrediente.

Ele pode ser algo como:

```csharp
    public string Preparar() {
        string preparo = string.Format("Ingrediente {0} preparado!", _nome);
        return preparo;
    }
```

---

#### Vamos usar nossa classe

No método ```PegarFatia``` vamos substituir o tipo atributo ```ingrediente``` de ```string``` por ```Ingrediente```.

Ao salvar, nossos testes vão mostrar o erro:

```
cannot convert from 'string' to 'Ingredente'
```

Isso significa que vamos ter que atualizar nossos testes substituindo as ```string``` por **objetos** do tipo ```Ingrediente```.

Ou seja, crie uma variável e use ```new Ingrediente()``` para criar um novo objeto do tipo ```Ingrediente```.

---

#### Vamos usar nossa classe

Então seu teste deve ficar parecido com:

```csharp
Ingredente mortadela = new Ingredente();
...
roboMortadelaComQueijo.PegarFatia(mortadela, 1);
```
Depois de criar as outras variáveis e salvar suas alterações seus testes vão apontar que:

```
Expected:    Peguei uma fatia de mortadela.

Actual:   ···eguei uma fatia de Ingredente.
```

---

#### Ajustando nosso código

Isso acontece por que, o método ```PegarFatia``` usa o atributo ```ingrediente``` assim:

```csharp
fatias = fatias + string.Format("Peguei uma fatia de {0}.\n", ingrediente);
```

Um objeto do tipo ```string``` sabe que ao ser utilizado assim, deve retornar seu valor.

Ex.:

```csharp
string nome = "Roberto";
```

Vai retornar ```Roberto```

Então vamos informar que no nosso caso, queremos o valor da propriedade ```Nome``` da classe ```Ingrediente```, que é do tipo ```string```.


---

#### Ajustando nosso teste

Ficando assim:

```csharp
fatias = fatias + string.Format("Peguei uma fatia de {0}.\n", ingrediente.Nome);
```

Mas ao salvar, os testes informam que:

```
Expected: Peguei uma fatia de mortadela.

Actual:   Peguei uma fatia de .
```

Isso acontece por que não informamos o valor da propriedade ```Nome``` nos nossos testes.

---

#### Ajustando nosso teste

Para isso, basta, depois de criar o objeto, informar o valor da propriedade nome:

```csharp
        Ingredente mortadela = new Ingredente();
        mortadela.Nome = "mortadela";
        
        Ingredente queijo = new Ingredente();
        queijo.Nome = "queijo";
```

Ao salvar, nossos testes informam que:

```
Passed!  - Failed:     0, Passed:     3, Skipped:     0, Total:     3, Duration: 4 ms
```

---

#### Melhora isso aí!

Mas e o preparo do ingrediente?

Realmente, uma coisa é um lanche frio de mortadela com queijo e outra coisa é um lanche com mortadela quente e queijo derretido.

Então vamos testar a função ```Preparar``` dos nossos ingredientes!

Até agora todos os nossos testes foram colocados no arquivo ```ProgramTest.cs```. Mas assim como nosso código, nossos testes também precisam estar organizados. Crie um arquivo ```RoboTest.cs``` e recorte os testes ```PegarFatiaTest``` e ```PegarFatiaNegativaTest``` para ele.

---

#### Melhora isso aí

Agora crie um arquivo ```IngredenteTest.cs``` e construa dois testes para ```Preparar```. Um que espera receber o resultado: ```"queijo derretido"``` e outro que espera o resultado: ```"mortadela quente"```.

Cada teste deve criar um objeto do tipo ```Ingrediente```, mas o primeiro teste tem ```Nome = "queijo"``` e outro tem ```Nome = "mortadela"```.

---

#### Melhora isso aí

Seus testes devem ficar parecidos com:

```csharp
    [Fact]
    public void PrepararMortadelaTest()
    {
        string expected = "mortadela quente";

        Ingredente queijo = new Ingredente();
        queijo.Nome = "mortadela";

        string actual = queijo.Preparar();

        Assert.Equal(expected, actual);
    }
```

---

#### Melhora isso aí

Mas os testes vão nos avisar que tem algo errado.

```bash
  Failed Sanduiche.Test.IngredienteTest.PrepararMortadelaTest [< 1 ms]
  Error Message:
   Assert.Equal() Failure
          ↓ (pos 0)
Expected: mortadela quente
Actual:   Ingrediente mortadela preparado!
```

E agora?

---

#### Melhora isso aí

Isso acontece por que a classe ingrediente só sabe preparar ingredientes de uma forma genérica.

Ela não sabe se o ingrediente é mortadela, queijo ou tomate, maçã...

🤔 Você pode estar pensando em usar o ```if``` e sim, seria possível resolver o problema colocando um ```if``` para cada ingrediente que possa ser usado num sanduíche.

⚠️ Mas não faça isso! O método ```Preparar``` ficaria enorme. Com muitas linhas de código e difícil de ler e de manter.

Para isso existe o conceito de **herança**!

--- 

#### Voltando à Herança

No projeto ```Sanduiche```, crie dois novos arquivos. Um com o nome ```Queijo.cs``` e outro com o nome ```Mortadela.cs```. Em cada arquivo crie uma classe pública chamada ```Queijo``` e outra chamada ```Mortadela```.

Vou usar a classe ```Queijo``` como exemplo. Depois do nome da classe, digite ```: Ingrediente```

```csharp
public class Queijo : Ingredente
{
    
}
```

--- 

#### Voltando à Herança

Isso indica para o C# que a classe queijo herda as características da classe Ingrediente. Nesse momento elas são idênticas. Só o nome é diferente.

Experimente trocar no teste ```PrepararQueijoTest``` a classe ```Ingrediente``` por ```Queijo```

```csharp
    [Fact]
    public void PrepararQueijoTest()
    {
        string expected = "queijo derretido";

        Ingrediente queijo = new Queijo();
        queijo.Nome = "queijo";

        string actual = queijo.Preparar();

        Assert.Equal(expected, actual);
    }
```

--- 

#### Voltando à Herança

O resultado vai ser o mesmo, por que a classe ```Queijo``` tem o mesmo método ```Preparar``` que a classe ```Ingrediente```.

Para que a classe ```Queijo``` tenha sua própria versão de ```Preparar``` precisamos criar um método com mesmo nome e acrescentar a palavra ```override``` depois do ```public```.

A instrução ```override``` significa que, a versão do método da classe filha (Ex.: ```Mortadela```), sobrescreve a versão do método da classe mãe (Ex.: ```Ingrediente```).

```csharp
public class Queijo : Ingredente
{
    public override string Preparar(){
        return "queijo derretido";
    }   
}
```

--- 

#### Voltando à Herança

Seus testes vão apontar um erro.

```bash
cannot override inherited member 'Ingredente.Preparar()' because it is not marked virtual, abstract, or override
```

O que o erro está dizendo é que o método ```Preparar``` em ```Ingrediente``` não está marcado como ```virtual```, ```abstract``` ou ```override```.

Essas opções indicam que o método da classe mãe ```Ingrediente``` pode ser substituído pela versão mais atualizada da classe filha ```Queijo```. Então vamos alterar o método para em ```Ingrediente``` para ficar assim:

```csharp
    public virtual string Preparar() {
```

--- 

#### Polimorfismo

> Na programação orientada a objetos, o polimorfismo permite que referências de tipos de classes mais abstratas representem o comportamento das classes concretas que referenciam. Assim, é possível tratar vários tipos de maneira homogênea. O termo polimorfismo é originário do grego e significa "muitas formas".

No nosso caso, note que usamos a classe ```Ingrediente``` mas criamos um objeto do tipo ```Queijo```. Isso é possível graças à **herança**:

```csharp
...
Ingrediente queijo = new Queijo();
...
```

--- 

#### Polimorfismo

E na chamada do método ```Preparar``` temos o benefício do **polimorfismo**:

```csharp
...
string actual = queijo.Preparar();
...
```

Pois, agora, o ```Ingrediente``` queijo, na verdade executa o código da classe ```Queijo```.

Tudo isso, graças à marcação ```virtual``` no método da classe mãe e ```override``` no método da classe filha.

---

<style scoped>section { justify-content: center; }</style>

# 🎈 Parabéns!
## Você chegou ao final do módulo básico!

Todos os códigos que desenvolvemos até aqui estão disponíveis em: https://github.com/robertoosantos/aula-sanduiche-educafro-csharp

## E nos vemos na próxima aula! 👋





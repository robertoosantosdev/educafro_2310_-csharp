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
### Aula 5
---
# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. **Fundamentos da Programação em C#**
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados SQL Server e Entity Framework
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 2. Fundamentos da Programação em C#
#### Criando uma aplicação .Net em C#

---

#### Removendo o desnecessário

Vamos abrir novamente o Visual Studio e o nosso projeto ```Sanduiche```.

Vamos novamente colocar o projeto de testes para executar com ```dotnet watch test```.

Perceba que no ponto atual, não precisamos mais do teste e da função ```PegarFatiaDeQueijo```.

Podemos excluir ambas.

---

#### Vamos falar de variáveis

Na função ```PegarFatia```, criamos uma variável ```ingrediente``` para armazenar o **nome do ingrediente** que queríamos pegar uma fatia.

Antes do nome da variável, definimos seu tipo. Ou seja, uma variável do tipo ```string```, pode armazenar textos!

Mas existem outros tipos de variáveis. Saiba mais em: https://learn.microsoft.com/pt-br/dotnet/csharp/fundamentals/types/

Vamos supor que você queira um sanduíche com três fatias de queijo. Como você faria?

Você teria que chamar a função ```PegarFatia``` 2 vezes idênticas?

---

#### Vamos falar de variáveis

Claro que não!

Para isso vamos usar 3 coisas:
1. Uma variável para armazenar números
2. Uma variável para acumular o retorno das fatias
3. Uma estrutura para fazer uma função mais de uma vez

Na função ```PegarFatia```, acrescente uma variável do tipo ```int``` com o nome ```quantidade```.

``` c#
    public static string PegarFatia(string ingrediente, int quantidade)
```

---

#### Vamos falar de variáveis

Ainda na função ```PegarFatia``` vamos criar uma variável do tipo ```string``` chamada ```fatias```.

Vamos envolver a função ```string.Format``` com a estrutura ```for```.

O ```for``` executa um trecho de código, quantas vezes forem necessárias.

``` c#
    for (int i = 0; i < quantidade; i++)
        {
            return string.Format("Peguei uma fatia de {0}.\n", ingrediente);
        }
```

Agora vamos substituir o ```return``` por ```fatias = fatias + ``` e, ao final, vamos retornar o a variável ```fatias```.

---

#### Ixi! Os testes falharam

```csharp
        for (int i = 0; i < quantidade; i++)
        {
            fatias = fatias + string.Format("Peguei uma fatia de {0}.\n", ingrediente.Nome);
        }

        return fatias;
```

Como esperado, se você salvou o arquivo, seu terminal deve mostrar o seguinte resultado:

```
There is no argument given that corresponds to the required parameter 'quantidade' of 'Program.
PegarFatia(string, int)'
```

Isso indica que nos testes, não estamos informando o valor do parâmetro quantidade.

---

#### Atualizando os testes

Vamos no arquivo ```ProgramTest.cs``` no nosso projeto de testes.

Primeiro ponto, agora não esperamos mais que a saída da função seja apenas ```"Peguei uma fatia de queijo.\n"``` mas que essa mensagem seja exibida 2 vezes. Ou seja:

```csharp
        string expectedQueijo = "Peguei uma fatia de queijo.\nPeguei uma fatia de queijo.\n";
```

E vamos informar quantas fatias de cada ingrediente queremos. 1 de mortadela e 2 de queijo.

```csharp
            Sanduiche.Program.PegarFatia("mortadela", 1);
            ...
            Sanduiche.Program.PegarFatia("queijo", 2);
```

---

#### Mas e se...

E se for informado um número negativo? 🤔

Aí vem algo que é chamado regra de negócio.

> Uma regra de negócio é um critério que, quem solicitou o desenvolvimento do sistema deve informar.
Ex.: 
Não podemos vender mais produtos do que o que tivermos em estoque.
O vendedor não pode dar um desconto maior que 10%.
No ano novo só fazemos reservas de no mínimo, 5 dias.

Isso significa que você vai ter que programar algo que trate essas situações.

---

#### Atualizando nossos testes

No nosso caso, vamos pensar que não é possível pegar -3 fatias e que se isso acontecer, o sistema deve retornar um erro.

Então crie um novo teste com o nome ```PegarFatiaNegativaTest```.

Na chamada da função ```PegarFatia``` informe um número negativo.

Para testar uma mensagem de erro, precisamos envolver a chamada da função com um ```Assert``` diferente.

---

#### Atualizando nossos testes

Ao invés de usarmos o ```Assert.Equal``` que testa se um texto é igual a um texto esperado. Vamos usar o ```Assert.Throws<>``` onde dentro do ```<>``` vamos colocar o tipo do erro que esperamos.

No nosso caso, ficará:

```csharp
            Assert.Throws<ArgumentOutOfRangeException>(() => Sanduiche.Program.PegarFatia("presunto", -5));
```

Ao salvar o arquivo, vamos ver que nosso teste falou com a mensagem:

```
Expected: typeof(System.ApplicationException)
Actual:   (No exception was thrown)
```

---


#### Atualizando nossa função

Isso nos avisa que a função ```PegarFatia``` não está atendendo a regra de negócio.

Vamos atualizar a função ```PegarFatia``` incluindo, na primeira linha dela, a verificação se a quantidade é menor que zero.

```csharp
    public static string PegarFatia(string ingrediente, int quantidade)
    {
        if (quantidade < 0)
        {
            throw new ArgumentOutOfRangeException();
        }
            ...
```

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋



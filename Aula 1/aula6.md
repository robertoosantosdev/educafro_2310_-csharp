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
### Aula 6
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
#### Classes e Objetos

---

#### Objetos

>Programação orientada a objetos (POO, ou OOP segundo as suas siglas em inglês) é um paradigma de programação baseado no conceito de "objetos", que podem conter dados na forma de campos, também conhecidos como atributos, e códigos, na forma de procedimentos, também conhecidos como métodos.

Em C#, tudo é ou é tratado como objeto. Ou seja, suas variáveis podem ser acessadas como atributos ou propriedades e suas funções podem ser acessadas como métodos.

---

#### Classes

Pra que isso funcione, todo objeto é a representação real de uma classe. Ela funciona como um contrato.

Uma classe ```Sanduiche``` pode ter as propriedades ```Peso``` e ```Ingredientes```.

Isso garante que todo sanduíche vai ter um peso e uma lista de ingredientes mesmo que ele seja de mortadela, atum ou hambúrguer.

Basicamente, essa forma de programar, ajuda a organizar o código de uma maneira mais intuitiva.

Vamos supor que seu amigo seja alérgico a leite. É intuitivo imaginar que a propriedade ingredientes vai te responder se o sanduíche tem ou não leite ou derivados.

---

#### Vamos criar nossa primeira classe

Assim como cada função deve ter uma finalidade específica, cada classe também deve ter um papel bem definido.

Então, ao invés de fazer todas as nossas funções na classe ```Program```, vamos criar uma classe chamada ```Robo``` que vai ser responsável por preparar nossos sanduíches.

Para criar uma classe basta criar um arquivo com o nome da classe e que termine com ```.cs```.

Então, no projeto ```Sanduiche```, crie um arquivo ```Sanduiche.cs```.

---

#### Vamos criar nossa primeira classe

A estrutura mais simples para uma classe é:

``` c#
class Robo
{
    
}
```

Onde ```class``` define que estamos criar uma classe e ```Robo``` é o seu nome.

Agora vamos copiar a função ```PegarFatia``` do arquivo ```Program.cs``` e colocá-la no arquivo ```Robo.cs```.

**⚠️ Importante:** Deixe a classe ```Program``` somente com o método ```Main```.



---

#### Ixi! Os testes falharam

Ao salvar o arquivo, seus testes devem mostrar o erro:

```
'Program' does not contain a definition for 'PegarFatia'
```

Indicando que precisamos atualizar nossos testes.

Assim, ao invés de chamarmos: ```Sanduiche.Program.PegarFatia("mortadela", 1);```.

Vamos chamar: ```Robo.PegarFatia("mortadela", 1);```

Ao salvar o arquivo, seus testes devem mostrar o erro:

```
'Robo' is inaccessible due to its protection level
```

---

#### Vamos falar sobre visibilidade

Nossa classe ```Robo``` e a classe ```Program``` tem uma diferença muito sutil.

Program.cs
```csharp
public class Program
```

Robo.cs
```csharp
class Robo
```

Isso mesmo, o ```public```. Uma classe pode ter diferentes níveis de visibilidade.

Isso ajuda a deixar seu sistema mais seguro e padronizado.

---

#### Vamos falar sobre visibilidade

Então se a gente colocar a palavra ```public``` antes do ```class``` na classe ```Robo```, nossos testes vão funcionar!

Existem diferentes tipos de visibilidade. Para saber mais, acesse: https://learn.microsoft.com/pt-br/dotnet/csharp/language-reference/keywords/accessibility-levels

Mas e o objeto para a classe ```Robo```? Nos nossos testes não criamos um objeto antes de chamar o método ```PegarFatia```. Isso é por que o método ```PegarFatia``` tem um pequeno detalhe:

```csharp
public static void PegarFatia
```

---

#### Nosso primeiro objeto

Isso mesmo, o ```static```. Isso indica que esse método não depende de um objeto, mas que a própria classe pode ser usada para chamá-lo.

Pense na classe ```Pessoa``` definindo que toda pessoa tem um ```Nome```, uma ```Idade``` e um método ```Correr```. Cada pessoa corre a uma velocidade diferente, por isso, vamos precisar de objetos para chamar a função  ```Correr``` de cada pessoa.

Mas e se quisermos saber a quantidade de pessoas que temos? Nesse caso podemos ter o seguinte método estático: ```static int CalcularPopulacao```. Assim, podemos chamar o método através da classe ```Pessoa.CalcularPopulacao``` sem depender de um objeto em específico.

---

#### Nosso primeiro objeto

Então, no nosso caso, a gente poderia ter 10 robôs para fazendo vários sanduíches ao mesmo tempo. Sendo assim, cada robô vai precisar pegar uma fatia específica. Ou seja, nosso método não pode ser ```static```.

Vamos tirar essa palavrinha ```static``` do método ```PegarFatia``` na classe ```Robo```. Ao salvar, nossos testes vão apontar um outro problema:

```
An object reference is required for the non-static field, method, or property
```

Essa mensagem está nos dizendo que precisamos criar um objeto para usar o método ```PegarFatia```. Exatamente como esperado!

---

#### Nosso primeiro objeto

A gente já criou alguns objetos, mas agora vamos prestar mais atenção dessa vez.

No teste ```PegarFatiaTest```, estamos utilizando a classe ```Robo``` para chamar a função ```PegarFatia```.

Para criar um objeto, vamos criar uma variável do tipo Robo. 

Se uma variável que guarda um texto é criada assim:

```csharp
string nome = "Roberto";
```

---

#### Nosso primeiro objeto

Uma variável que representa um objeto do tipo ```Robo```, vai ser criada assim:

```csharp
Robo roboMortadela = new Robo();
```

Onde, ```roboMortadela``` é o nome do objeto e ```new Robo()``` é a forma de criar um novo objeto.

E substituir:

```csharp
Robo.PegarFatia(...;
```

Por

```csharp
roboMortadela.PegarFatia(...
```

---

#### Uma pequena pausa

C# é uma linguagem *case-sensitive*, que significa que maiúsculas e minúsculas fazem diferença.

Por isso existem convenções para facilitar a leitura de um código em qualquer lugar que você esteja trabalhando.

Classes: *PascalCase*, ou seja, a primeira letra de cada palavra é maiúscula. Ex.: Empresa de construção, vira: ```EmpresaDeConstrucao```.

Métodos: também são *PascalCase*, mas a primeira palavra é um verbo no infinitivo. Ex.: Construção de parede, vira: ```ConstruirParede```.

Variáveis: *camelCase*, ou seja, a primeira letra da primeira palavra é minúscula. Ex.: Materiais de construção, vira: ```materiaisDeConstrucao```

---

#### Uma pequena pausa

Para saber mais, acesse: https://learn.microsoft.com/pt-br/dotnet/csharp/fundamentals/coding-style/identifier-names

---

#### Nosso primeiro objeto

Repita o processo para o teste: ```PegarFatiaNegativaTest``` e ao salvar, você terá seus testes funcionando corretamente e seu código muito mais organizado e fácil de trabalhar.

```
Starting test execution, please wait...
A total of 1 test files matched the specified pattern.

Passed!  - Failed:     0, Passed:     3, Skipped:     0, Total:     3, Duration: 3 ms - Sanduiche.Test.dll
```

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋




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
### Aula 16
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
#### Construindo Views e Controllers

---

#### Construindo Views e Controllers

Agora use o ```aspnet-codegenerator``` para gerar o cadastro de ```Funcionarios```.

Novamente é interessante adicionar o : ```@Html.ValidationSummary()```.

E fazer o procedimento de criar ```@Html.DropDownList``` para a propriedade ```Cargo```.

Além disso, há um detalhe. A propriedade:

```csharp
/// Um funcionário pode ter várias avaliações
public List<Avaliacao> Avaliacoes { get; set; }
```

Do jeito que ela está, estamos dizendo que um funcionário obrigatoriamente precisa de avaliações. Mas isso não é verdade. Quando um funcionário começa a trabalhar, ele vai precisar de algum tempo para ser avaliado.

---

#### Construindo Views e Controllers

Para ajustar isso é muito simples. Basta adicionar um ```?``` no tipo da propriedade. Ficando assim:

```csharp
/// Um funcionário pode ter várias avaliações
public List<Avaliacao>? Avaliacoes { get; set; }
```

Onde a gente diz que essa propriedade pode ser nula.

No **Controller** também precisamos fazer uma alteração.

Por padrão nós temos esse método:

```csharp
public async Task<IActionResult> Create([Bind("Id,Nome,Email,Ativo,Salario")] Funcionario funcionario)
```

Mas ele não recebe o ```Cargo``` que foi preenchido na **View**.

---

#### Construindo Views e Controllers

Mas isso é muito simples de ajustar. Basta adicionar um parâmetro de nome ```Cargo``` e tipo ```string```.

Depois disso, vamos usar esse parâmetro para consultar o ```Cargo``` no banco de dados.

Veja, a consulta que vamos fazer é através do ```Id``` que é do tipo ```int```, mas recebemos o cargo selecionado como ```string```. Mas o C# nos permite fazer a conversão de tipos nesses casos. Esse processo se chama ***parse*** e é feito assim: ```int.Parse(Cargo)```.

⚠️ Importante: Nesse caso, vamos alterar o valor do objeto ```funcionario```. E ele que é utilizado para definir se ```ModelState.IsValid```. Então, alguns passos adicionais vão ser necessários para atualizar as validações.

---

#### Construindo Views e Controllers

Outro ponto: Se a **model** estiver inválida, é retornada a **view** com o formulário preenchido para o usuário.

Mas, assim como no método ```Create()```, é necessário carregar a lista de cargos para a ```ViewBag```.

---

#### Construindo Views e Controllers

Então, a versão final fica assim:

```csharp
public async Task<IActionResult> Create([Bind("Id,Nome,Email,Ativo,Salario")] Funcionario funcionario, string Cargo)
{
    // Limpa as validações padrões
    ModelState.Clear();
    // Consulta o cargo no banco de dados
    var cargo = _context.Cargos.Single(c => c.Id == int.Parse(Cargo));
    // Preenche o cargo do funcionário
    funcionario.Cargo = cargo;
    // Atualiza a validação do modelo
    var validacao = TryValidateModel(funcionario);

    // Se for válida, seguimos com o cadastro
    if (validacao)
    {
        _context.Add(funcionario);
        await _context.SaveChangesAsync();
        return RedirectToAction(nameof(Index));
    }

    ViewBag.Cargo = CargosParaLista(_context.Cargos.ToList());
    return View(funcionario);
}
```

---

#### Construindo Views e Controllers


Experimente executar a aplicação novamente e criar um novo funcionário.

Acesse: https://localhost:7081/Funcionarios/Create



---

#### Construindo nosso menu

Atualmente nosso menu tem apenas dois itens: **Home** e **Privacy**. E quando queremos ir para outras páginas temos que digitar o endereço manualmente.

Para melhorar isso, vamos modificar o arquivo: Views/Shared/_Layout.cshtml

Veja que o menu atual está representado por:

```html
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
</li>
```

Note que no elemento ```<a>```temos duas propriedades ```asp-controller``` com o nome do **Controller** que se deseja acessar e ```asp-action``` para o método que queremos.

---

#### Construindo nosso menu

Então experimente criar os outros items do menu e mude os nomes *Privacy* e *Home* para **Política de Privacidade** e **Início** respectivamente.

---

#### Construindo nosso menu

Seu deve ter ficado assim:

```html
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Início</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Cargos" asp-action="Index">Cargos</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Funcionarios" asp-action="Index">Funcionários</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Avaliacoes" asp-action="Index">Avaliações</a>
</li>
<li class="nav-item">
    <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Política de Privacidade</a>
</li>
```

---

#### Traduzindo

Nossas páginas criadas pelo ```dotnet aspnet-codegenerator``` tem uma série de mensagens em inglês. Agora, vamos traduzi-las.

Veja a página: Views/Cargos/Create.cshtml

Para mudar o título da página, altere:

```
ViewData["Title"] = "Create";
```

e:

```
<h1>Create</h1>
```

Para **Criar**

---

#### Traduzindo

Mude também a propriedade ***value*** do botão:

```
<input type="submit" value="Create" class="btn btn-primary" />
```

Para **Criar**

E por fim, mude o texto do link:

```
<a asp-action="Index">Back to List</a>
```

Para: **Voltar para lista**


**⚠️IMPORTANTE⚠️**: Agora repita esse processo para as demais Views

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





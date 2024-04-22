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
### Aula 17
---
# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados e Entity Framework
6. Construção de um Aplicativo Web MVC
7. **Implementando Recursos Avançados**
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 7. Implementando Recursos Avançados
#### Autenticação e Autorização

---

#### Autorização e Autenticação

Faça o seguinte teste. Execute a aplicação com ```dotnet run```e experimente criar um novo cargo, por exemplo, "Diretor".

Faz sentido que qualquer pessoa possa criar um cargo? Não.

Para que, somente usuários logados no sistema possa criar um cargo, podemos fazer o seguinte:

No ***controller*** Cargos, inclua o atributo ```[Authorize]``` na classe Cargos:

```csharp
[Authorize]
public class CargosController : Controller
```

---

#### Autorização e Autenticação

Agora, pare de rodar a aplicação com ```CTRL+C``` e execute novamente com ```dotnet watch run```. E tente novamente cadastrar um cargo.

Assim que você clica em "Cargos" no menu, ou tenta acessar o caminho **/Cargos** pela barra de endereços do navegador, você é enviado para a tela de login para a **autenticação** do usuário.

Inclua ```[Authorize]``` também nos ***controllers*** **Funcionarios** e **Avaliacoes**.

---

#### Autorização e Autenticação

Se o cliente, ou seu chefe pedir que: 

> Se o usuário não estiver logado, não mostre os menus que ele não tem acesso.

O que você precisa fazer é alterar o arquivo **/Views/Shared/_Layout.cshtml** incluindo ```if``` para verificar se o usuário está **Autenticado**:

<font size=3>

```html
@if (User.Identity.IsAuthenticated)
{
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Cargos"
            asp-action="Index">Cargos</a>
    </li>

    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Funcionarios"
            asp-action="Index">Funcionários</a>
    </li>
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Avaliacoes"
            asp-action="Index">Avaliações</a>
    </li>
}
```

</font>

---

#### Autorização e Autenticação

Veja que agora o menu é um quando você está logado. E outro para quando não está.

Experimente ocultar os itens: **Início** e **Política de Privacidade** 

---

#### Autorização e Autenticação

Deve ter ficado assim:

<font size=3>

```html
@if (User.Identity.IsAuthenticated)
{
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Cargos"
            asp-action="Index">Cargos</a>
    </li>

    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Funcionarios"
            asp-action="Index">Funcionários</a>
    </li>
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Avaliacoes"
            asp-action="Index">Avaliações</a>
    </li>
}
else
{
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Home"
            asp-action="Index">Início</a>
    </li>
    <li class="nav-item">
        <a class="nav-link text-dark" asp-area="" asp-controller="Home"
            asp-action="Privacy">Política de Privacidade</a>
    </li>
}
```

</font>

---

### Autorização e Autenticação

Agora, nos vamos para a parte mais complicada do nosso projeto. As funções do gerente.

Veja o método ```Create``` do ```FuncionariosController```. Ele não faz muito sentido. Porque, cadastrar um novo funcionário significa, contratar alguém, e somente um gerente pode contratar um funcionário.

Para tratar isso, precisamos de uma série de alterações no nosso código.

Primeiramente, vamos proteger os métodos ```Create POST``` e ```GET``` somente para gerentes. 

---

### Autorização e Autenticação

Um conceito muito importante em Autorização é a ***role*** que em português seria algo como **papel** que o usuário desempenha no sistema. Alguns usuários são funcionários, e podem fazer algumas coisas. Outros são gerentes e podem fazer outras coisas. Mas poderíamos ainda ter funcionários administradores com outros acessos.

Nos métodos ```CREATE``` , inclua o seguinte atributo:

```csharp
[Authorize(Roles ="GERENTES")]
```

E depois, tente criar um novo funcionário. Você deverá receber o erro:

<font color="red">
Access denied
</font>

Que significa, **Acesso Negado**.

---

### Autorização e Autenticação

Agora vamos usar novamente o objeto ```User``` que já usamos para controlar os itens do menu, para consultar quem é o usuário logado e se o cargo dele é gerente. Mas temos um problema. Quando o usuário se cadastra, ele informa apenas e-mail e senha. E não temos o e-mail dos nossos funcionários.

Então faça as seguintes mudanças:

1. Inclua a propriedade ```Email``` do tipo ```string``` na classe ```Model.Funcionario```.
2. Faça uma nova ```migration```
3. Atualize o banco de dados.


---

### Autorização e Autenticação

Substitua as linhas que usavam o _context diretamente, sem nossas regras de negócio:

```csharp
_context.Add(funcionario);
await _context.SaveChangesAsync();
```

Por nosso ```Gerente``` que é quem pode contratar.

```csharp
var gerente = _context.Gerentes.Single(g => g.Email == User.Identity.Name);
gerente.Contratar(funcionario);
```

---

### Autorização e Autenticação

Substitua também o ```_context.Funcionarios.Remove(funcionario);``` no método ```DeleteConfirmed```, pelo método ```Gerente.Demitir```.

---

### Autorização e Autenticação

E por último, substitua também o ```_context.Update(funcionario);``` no método ```Edit POST```, pelo método ```Gerente.AumentarSalario```, usando ```funcionario.Salario``` como parâmetro ```novoSalario``` pois, ```funcionario``` tem os valores que o usuário preencheu no formulário.

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





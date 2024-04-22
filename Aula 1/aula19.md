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
### Aula 19

---

# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados e Entity Framework
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. **Melhores Práticas e Testes**
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 8. Melhores Práticas e Testes
#### Database Seeding

---

#### Database Seeding

> A propagação do banco de dados é preencher um banco de dados com um conjunto inicial de dados. É comum carregar dados iniciais, como contas de usuário iniciais ou dados fictícios, na configuração inicial de um aplicativo.

---

#### Database Seeding

Na primeira vez que nossa aplicação for executada, não existirá nenhum cargo e nenhum funcionário.

Isso significa que ninguém vai poder contratar, avaliar ou criar cargos.

Para resolver isso, é muito comum que um sistema já nasça com algumas informações salvas em banco de dados.

No nosso caso vamos precisar criar:
1. Um cargo
2. Um usuário
3. Um funcionário nível gerente.

---

#### Database Seeding

Vamos criar uma classe ```ConfiguraAdministradorSistema``` na raiz do projeto ```abantu.mvc```.

Essa classe vai precisar de 6 variáveis privadas internas:

<font size=3>

```csharp
private readonly ApplicationDbContext _db;
private readonly UserManager<IdentityUser> _userManager;
private readonly RoleManager<IdentityRole> _roleManager;
private readonly Cargo _cargoAdmin = new Cargo
{
    Nome = "SysAdmin",
    Nivel = 2
};

private readonly IdentityRole _perfilGerente = new IdentityRole("GERENTES");

private readonly Gerente _admin = new Gerente(null)
{
    Nome = "Administrador do Sistema",
    Salario = 0,
    Email = "admin@admin.com.br"
};
```

</font>

---

#### Database Seeding

O construtor deve ser: 

```csharp
public ConfiguraAdministradorSistema(ApplicationDbContext db, UserManager<IdentityUser> userManager, RoleManager<IdentityRole> roleManager)
    {
        _db = db;
        _userManager = userManager;
        _roleManager = roleManager;
    }
```

Os métodos serão:

<font size=3>


```csharp
public async Task Configurar()
{
    try
    {
        ConfigurarBancoDeDados();
        await ConfigurarAutenticacao();
    }
    catch (System.Exception exception)
    {
        Console.Write(exception);
    }
}
```

</font>

---

#### Database Seeding

```csharp
private async Task<bool> ConfigurarAutenticacao()
{
    IdentityResult usuarioConfigurado;

    var user = new IdentityUser
    {
        UserName = _admin.Email,
        Email = _admin.Email,
        EmailConfirmed = true
    };

    var userAdmin = await _userManager.FindByEmailAsync(user.Email);

    if (userAdmin == null)
    {
        usuarioConfigurado = await _userManager.CreateAsync(user, "Sys@dm1n");

        if (!usuarioConfigurado.Succeeded)
            return false;
    }
    else
    {
        user = userAdmin;
    }

    var perfilGerentes = await _roleManager.FindByNameAsync(_perfilGerente.Name);

    if (perfilGerentes == null)
    {
        usuarioConfigurado = await _roleManager.CreateAsync(_perfilGerente);

        if (!usuarioConfigurado.Succeeded)
            return false;
    }

    if (!await _userManager.IsInRoleAsync(user, _perfilGerente.Name))
    {
        usuarioConfigurado = await _userManager.AddToRoleAsync(user, _perfilGerente.Name);

        if (!usuarioConfigurado.Succeeded)
            return false;
    }
    return true;
}
```

---

#### Database Seeding

```csharp
private int ConfigurarBancoDeDados()
{
    var admin = _db.Gerentes.SingleOrDefault(g => g.Email == _admin.Email);
    var quantidadeGerentes = _db.Gerentes.Where(g => g.Ativo).Count();

    if (admin == null)
    {
        var cargoAdmin = _db.Cargos.SingleOrDefault(c => c.Nome == _cargoAdmin.Nome);

        admin = _admin;
        admin.Cargo = cargoAdmin == null ? _cargoAdmin : cargoAdmin;
        admin.Ativo = quantidadeGerentes == 0;

        _db.Add(admin);
    }
    else
    {
        if (quantidadeGerentes >= 1)
        {
            admin.Ativo = false;
        }
        else
        {
            admin.Ativo = true;
        }
    }

    return _db.SaveChanges();
}
```

---

#### Database Seeding

Por fim, no ```Program.cs``` precisamos incluir a chamada para essa configuração que criamos.

```csharp
var app = builder.Build();

using (var scope = app.Services.CreateScope())
{
    var services = scope.ServiceProvider;
    var db = services.GetRequiredService<ApplicationDbContext>();
    var um = services.GetRequiredService<UserManager<IdentityUser>>();
    var rm = services.GetRequiredService<RoleManager<IdentityRole>>();
    var configurarDb = new ConfiguraAdministradorSistema(db, um, rm);

    await configurarDb.Configurar();
}

// Configure the HTTP request pipeline.
```

---

#### Database Seeding

Agora experimente executar a aplicação novamente e acessá-la com: 

Usuário: **admin@admin.com.br**
Senha: **Sys@dm1n**

Agora você conseguirá ter acesso a todas as outras funções da aplicação e contratar os primeiros funcionários.

**⚠️ Essa é uma forma simplificada de carregar nossa aplicação com o primeiro usuário. O problema dessa solução é que, qualquer pessoa que saiba como nossa aplicação funciona, saberá como acessar essa aplicação em qualquer cliente que a utilize.**

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





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
### Aula 18
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
#### User Experience

---

#### User Experience

> Experiência do usuário (EU), do inglês user experience (UX), é o conjunto de elementos e fatores relativos à interação do usuário com um determinado produto, sistema ou serviço cujo resultado gera uma percepção positiva ou negativa. O termo foi utilizado pela primeira vez por Donald Norman na década de 1990.

---

#### User Experience

Para melhorar a experiência do usuário da nossa aplicação, mude texto dos botões na ***View*** **Funcionarios/Index.cshtml** para: 

- Dar Aumento
- Ver Detalhes
- Demitir

Ficando mais claro o que cada opção faz.

Em cada uma das ***Views*** de Funcionários, altere também seus títulos e botões.

---

#### User Experience

Nessa mesma ***View*** notamos que Email e Salario poderiam estar escritos como E-mail e Salário.

Para fazer isso, adicione o atributo ```[Display(Name = "XXX")]``` na ***Model*** ```Funcionario``` para alterar qual nome queremos exibir para essas propriedades.

---

#### User Experience

Repita o processo para as propriedades:

- Cargos
    - Nivel

- Avaliacoes
    - RealizadaEm
    - Comentarios

---

#### User Experience

Como já observamos, algumas mensagens de validação do nosso sistema estão em inglês.

Para ajustar isso, use o atributo: ```[Required(ErrorMessage ="MENSAGEM DE ERRO")]```

Nas propriedades:

- Cargos
    - Nome
    - Nivel

---

- Funcionario
    - Nome
    - Email
    - Salario
    - Cargo

- Avaliacao
    - Avaliado
    - Avaliador
    - RealiadaEm
    - Comentario

---

<style scoped>section { justify-content: center; }</style>

# Muito obrigado
## E nos vemos na próxima aula! 👋





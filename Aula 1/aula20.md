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
### Aula 20

---

# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados e Entity Framework
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. **Projetos e Aplicações Futuras**

---
<style scoped>section { justify-content: center; }</style>

### 9. Projetos e Aplicações Futuras

---

#### Aprofundando conhecimentos

##### 🎈 Parabéns 🥳

Chegamos ao final das nossas aulas e eu espero que vocês tenham se divertido e aprendido C# de uma forma simples e prática.

Apesar de termos visto muitos conceitos e recurso do C# e do MVC, ainda existe muito para se aprender.

Vou deixar aqui duas sugestões de guias de estudos:

https://techguide.sh/pt-BR/path/csharp/

---

#### Evoluindo este projeto

Minha versão final desse projeto está disponível em: https://github.com/robertoosantos/abantu/tree/realease.1.0

Atualmente o projeto está funcional mas ainda faltam alguns recursos interessantes. Tente:

1. Criar a função ```Promover``` para criar novos ```gerentes``` além do usuário administrador.
2. Criar o conceito de ```Departamentos```. Uma empresa tem vários departamentos, cada departamento tem um gerente e vários funcionários.
3. O ```funcionário``` só poderia ver o histórico das suas próprias avaliações mas somente ```gerentes``` podem ver as avaliações de qualquer funcionário
4. Avaliações só podem ser alteradas pelo avaliador que a criou

---

#### Projeto de estoque

Tente criar um novo projeto.

Um sistema de controle de estoque com:

1. Dois tipos de usuários. Estoque e Vendas.
2. Vendas podem consultar produtos e reduzir quantidade disponível
3. Estoque pode cadastrar e excluir produtos
4. Não é possível reduzir a quantidade disponível para menos que zero.
5. Quando a quantidade for zero, o sistema deve mostrar o produto como indisponível

---

#### Projeto para escola

Tente construir um novo projeto para uma escola com:

1. Dois perfis de usuário, Alunos e Professores
2. Cada materia possui um professor e vários alunos
3. Cada professor só pode avaliar alunos da materia que eles são professores e somente se a turma estiver ativa
4. Cada aluno pode consultar suas notas, mas somente o professor pode consultar as notas de todos os alunos
5. O sistema deve calcular a média do aluno em cada materia e uma média geral

---

<style scoped>section { justify-content: center; }</style>

# Parabéns
## Muito obrigado por ter chegado até aqui! 
### Muito sucesso nas suas carreiras
### Contem comigo para o que precisarem! 👋





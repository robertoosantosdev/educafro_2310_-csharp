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
---
# Agenda
1. **Introdução à Programação e Ambiente de Desenvolvimento**
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados SQL Server e Entity Framework
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
<style scoped>section { justify-content: center; }</style>

### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### Quem sou eu?

---

#### Quem sou eu?

![image alt <](https://media.licdn.com/dms/image/D4D03AQGV6tXPqdKwtg/profile-displayphoto-shrink_200_200/0/1689793670219?e=1703721600&v=beta&t=RXZWE-7NvGH473xOlc4bARqd4x_L6WN9qTy29gIoH-A) Roberto de Oliveira Santos
37 anos
Gerente @ Itaú Unibanco
Homem negro, apaixonado por tecnologia e por agilidade

##### Formação
Tecnologia da Informação @ Fatec Jundiaí (2007)
Arquitetura e Sistemas .Net @ FIAP (2013)
MBA Executivo @ Insper (2021) 

> Linkedin: https://www.linkedin.com/in/robertoosantos/

---
<style scoped>section { justify-content: center; }</style>

### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### O que é programação?

---
#### O que é programação?

> Programação é o processo de escrita, teste e manutenção de um programa de computador. O programa é escrito em uma linguagem de programação, embora seja possível, com alguma dificuldade, o escrever diretamente em linguagem de máquina. Diferentes partes de um programa podem ser escritas em diferentes linguagens.
**Wikipedia*

---

#### O que é programação?

Para facilitar o entendimento, imagine um programa de computador como um conjunto de funções que permite que as pessoas façam algo sem entender detalhadamente como um computador funciona.

##### Vamos praticar! ✏️

Numa folha de papel escreva as funções que você imagina que um computador e/ou celular deve fazer para mostrar as últimas publicações do Instagram para você.

---

#### O que é programação?

##### Muito bem! ✅

Apesar de eu não saber exatamente todas as funções que o Instagram realiza, essa é a minha lista:

- Solicitar usuário e senha
- Validar usuário e senha
- Consultar notificações
- Consultar últimas publicações das pessoas que sigo
- Filtrar publicações que eu já vi
- Consultar quantidade de curtidas de cada publicação
- Consultar quantidade de comentários de cada publicação

---

#### O que é programação?

- Consultar meu perfil
- Consultar publicidades que são relevantes para mim
- Consultar perfis que poderia começar a seguir
- Exibir publicações
- Exibir botões em cada publicação
- Exibir menu de navegação
- Exibir texto da publicação
- Consultar últimos *stories* de pessoas que eu sigo
- Ordenar *stories* não vistos primeiro
...

---

#### O que é programação?

Como eu disse, eu não tenho certeza sobre todas as funções Instagram realiza, mas espero que a sua lista esteja parecida com a minha.

Cada item dessa lista foi desenvolvido através de programação!

---
<style scoped>section { justify-content: center; }</style>

### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### Funções

---

#### Funções

Como falamos anteriormente, uma função realizar uma tarefa para uma pessoa no computador.

##### Vamos praticar! ✏️

Para ajudar a entender esse conceito, vamos pensar na seguinte atividade.

Numa folha de papel anote o passo-a-passo que eu devo seguir para fazer um sanduíche de mortadela com requeijão.

---

#### Funções

##### Muito bem! ✅

Agora, vamos imaginar que eu sou um computador. E você escreveu as seguintes instruções:

...
* Pegue o requeijão
* Passe o requeijão no pão
...

Uma pessoa entenderia o que fazer, mas um computador poderia entender que deve pegar o pote de requeijão e passar o pote no pão.

Nada bom, não é mesmo?

---

#### Funções

Esse vídeo exemplifica um pouco da dificuldade programar e do quanto é importante ser específico nas instruções.


https://www.youtube.com/watch?v=pdhqwbUWf4U



---

#### Funções

##### 💡 Dicas!

1. Tenha funções com finalidades específicas.
2. Teste cada uma das suas funções individualmente.



---

#### Funções

Outro ponto interessante. Vamos pensar nessas 2 funções:

> Pegue uma fatia de mortadela
> Pegue uma fatia de queijo


Um computador ou smartphone não tem braços. 
Mas vamos supor que você conseguiu construir um braço mecânico.

![image w:200px center](../assets//images/imagem_8_braco_robo.png)

<font size="5">Imagem por <a href="https://icons8.com/illustrations/author/zD2oqC8lLBBA">Icons 8</a> em <a href="https://icons8.com/illustrations">Ouch!</a></font>


---

#### Funções

Ainda assim, o computador precisaria dar uma série de instruções para o braço conseguir completar a função "Pegue uma fatia de mortadela"

Ex.:

* Abrir mão
* Esticar braço até a **mortadela**
* Posicionar um dos dedos abaixo de uma fatia de mortadela
* Fechar a mão
* Contrair o braço

---

#### Funções

Ufa! Se tudo deu certo, o braço do robô deveria estar com uma fatia de mortadela na mão.

Porém, agora você você quer que o computador pegue uma fatia de queijo.
Seria muito ruim ter que escrever de novo todas as funções que o braço deve executar simplesmente trocando a **mortadela** por **queijo**.

---

#### Funções

Por sorte, você não precisa fazer isso. Um dos princípios da programação é:

> DRY: Do inglês, **D**on't **R**epeat **Y**ourself.

Ou seja, **Não Se Repita!**

Então, você pode criar funções personalizadas.

Assim, você pode fazer criar uma função com o nome "Pegar uma fatia" com todas as instruções que o braço mecânico deve fazer.

E a partir desse ponto, o computador vai conseguir executar as duas funções:

> Pegue uma fatia de **mortadela**
> Pegue uma fatia de **queijo**

---

#### Funções

É importante observar que as duas chamadas da sua função "Pegar uma fatia" só tem uma diferença.

Uma pega uma fatia de **mortadela** e a outra de **queijo**

Nesse caso mortadela e queijo são os **parâmetros** da função.

---

#### Funções

##### 💡 Dica!

Vale apena relembrar das aulas de Português. Todo verbo transitivo precisa de um **objeto**.

> Quem **pega**, pega alguma coisa
> Quem **empresta**, empresta algo para alguém
> Quem **reserva**, reserva alguma coisa

Ou seja, se você estiver escrevendo essas funções é melhor passar o objeto como parâmetro. Assim, seu programa de computador vai estar pronto para emprestar **lápis**, mas no futuro também pode emprestar um **caderno** ou uma **caneta**.
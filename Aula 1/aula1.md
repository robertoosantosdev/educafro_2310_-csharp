---
marp: true
footer: 'out - 2023 - Educafro Tech - C# - por Roberto de Oliveira Santos'
---
<style>
img[alt$="<"] {
    float: left;
    margin-right: 2em;
    }
</style>
# Educafro Tech
## Curso C# - Do Básico ao MVC
---
# Agenda
1. Introdução à Programação e Ambiente de Desenvolvimento
2. Fundamentos da Programação em C#
3. Programação Orientada a Objetos (POO)
4. Desenvolvimento Web com ASP.NET MVC
5. Banco de Dados SQL Server e Entity Framework
6. Construção de um Aplicativo Web MVC
7. Implementando Recursos Avançados
8. Melhores Práticas e Testes
9. Projetos e Aplicações Futuras

---
### 1. Introdução à Programação e Ambiente de Desenvolvimento

---
### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### 1.1. Quem sou eu?

---
<style scoped>section { justify-content: start; }</style>

#### 1.1. Quem sou eu?

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
### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### 1.1. GitHub
##### 1.1.1. Criando sua conta

---
<style scoped>section { justify-content: start; }</style>

### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### 1.1. GitHub
> A plataforma de desenvolvedores baseada em IA para criar, dimensionar e fornecer software seguro.

Link: https://github.com/

---
### 1. Introdução à Programação e Ambiente de Desenvolvimento
##### 1.1.1. Criando sua conta
1. Clique em ***Sign Up*** no canto superior direito
![Botões de acesso ao Github](../assets/images/imagem_1_sign_up_github.png "Botão Sign Up")

---
<style scoped>section { justify-content: start; }</style>

##### 1.1.1. Criando sua conta

2. Será solicitado seu **e-mail** e a criação de uma **senha**.
> Se você não tiver um e-mail, você pode criar um em: https://gmail.com
3. Será solicitada a criação de um **nome de usuário**.
> Futuros contratantes verão seu nome de usuário. Sugiro utilizar algo similar ao seu nome. Ex.: rafa-silva-dev, devmariasouza, anasilva-dev
4. Se você quer notícias sobre o github por e-mail responda ***y*** para **sim**, ou ***n*** para **não**.

---
<style scoped>section { justify-content: start; }</style>

##### 1.1.1. Criando sua conta

5. Ao final, você terá que resolver um desafio para confirmar que você é um usuário e não uma máquina. Ex.:
![Desafio para verificar conta no Github](../assets/images/imagem_2_verify_github.png "Verificar conta")

---
<style scoped>section { justify-content: start; }</style>

##### 1.1.1. Criando sua conta

6. Clique em **Create account** para criar sua conta.
7. Um código com 8 números vai ser enviado para seu **e-mail**. Acesse seu e-mail numa nova aba ou janela e digite nessa tela o **código enviado por e-mail**.

---
<style scoped>section { justify-content: start; }</style>

##### 1.1.1. Criando sua conta

8. Agora você deve configurar sua conta.
Recomendo que você selecione:
* ***Just me*** (apenas eu)
* ***Student*** (estudante)
![w:800px Configuração da conta no Github](../assets/images/imagem_3_configuracao_github.png "Verificar conta")

---
<style scoped>section { justify-content: start; }</style>

##### 1.1.1. Criando sua conta

9. Selecione quais recursos pretende utilizar. Recomendo selecionar somente **'Collaborative coding'** e, no final da página, selecione **Continue**. 
![width:500px Configuração da conta no Github](../assets/images/imagem_4_funcionalidades_github.png "Verificar conta")

---

##### 1.1.1. Criando sua conta
<style scoped>section { justify-content: start; }</style>

10. Finalmente, serão oferecidos os tipos de conta que você deseja. Selecione ***Free*** (Grátis) e no final da página clique em **Continue for free**.
![width:500px Configuração da conta no Github](../assets/images/imagem_5_conta_github.png "Verificar conta")

---
<style scoped>section { justify-content: start; }</style>

##### 1.1.1. Criando sua conta

#### Parabéns! 👏
Agora você possui uma conta na maior plataforma de desenvolvimento do mundo!

---
### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### 1.2. Visual Studio Code
#### 1.2.1. Instalação
#### 1.2.2. Criando um novo projeto

---
<style scoped>section { justify-content: start; }</style>

### 1. Introdução à Programação e Ambiente de Desenvolvimento
#### 1.2. Visual Studio Code
> Edição de código. Redefinido. Grátis. Construído em open source. Roda em qualquer lugar.


Link: https://code.visualstudio.com/download

---
<style scoped>section { justify-content: start; }</style>

##### 1.2.1 Instalação
1. Selecione a versão desejada. Se o seu computador for 32 ou 64 bits e para qual Sistema Operacional você deseja. (Windows, Linux, MacOS...)
> No meu caso estou usando o Linux Ubuntu 22.04 64bits

![h:350px Configuração da conta no Github](../assets/images/imagem_6_download_visual_studio.png "Verificar conta")

---
<style scoped>section { justify-content: start; }</style>

##### 1.2.1 Instalação
2. Clique duas vezes no arquivo baixado e siga as instruções para instalação.

3. **Pronto**, você tem uma das ferramentas mais populares de programação, disponível para utilização.

---
<style scoped>section { justify-content: start; }</style>

##### 1.2.2 Configuração

No lado esquerdo você encontrará as funcionalidades do visual studio.

1. Clique em **Extensões** e pesquise por C#
2. Instale a extensão com a descrição ```Base language support for C#```.

![h:350px Configuração da conta no Github](../assets/images/imagem_7_extensoes_visual_studio.png "Verificar conta")



---
<style scoped>section { justify-content: start; }</style>

##### 1.2.2 Configuração

Agora vamos instalar o .Net SDK*
> Crie. Teste. Implante. O .NET é a estrutura gratuita, de software livre e multiplataforma para compilar aplicativos modernos e serviços de nuvem poderosos.

> \* SDK significa *Software Development Kit*, ou seja, kit para desenvolvimento de software. Esse é um conceito que existe para diversas tecnologias.

Link: https://dotnet.microsoft.com/pt-br/download

---
<style scoped>section { justify-content: start; }</style>

##### 1.2.2 Configuração

#### Parabéns! 👏
Agora você está pronto para começar a desenvolver em C#!
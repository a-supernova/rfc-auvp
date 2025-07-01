### AUVP - A Única Verdade Possível
### Request for comments: 1
### Categoria: Desenvolvimento de Software

### Autores:
#### Marcelo Machado Fleury
#### Alyf Mendonça

# RFC1 - Como desenvolver software na AUVP

# Resumo

Esta RFC tem como objetivo descrever os processos, as práticas e as tecnologias relativas ao desenvolvimento de software na AUVP. É importante que este documento seja lido e compreendido por todos os desenvolvedores da empresa e que todos o mantenham sempre vivo, surgerindo melhorias e o adequando às melhores práticas da atualidade. 

# Sumário

## 1 - O Manifesto
## 2 - As tecnologias
## 3 - O processo de desenvolvimento de software
### 3.1 - A proposta de desenvolvimento
### 3.2 - A implementação de uma nova funcionalidade
### 3.3 - A correção de bugs
## 4 - O versionamento de código
### 4.1 - O Fluxo
### 4.2 - Monorepo
### 4.3 - Multirepo
## 5 - A arquitetura do software
## 6 - Os testes no software
## 7 - A política de releases
### 7.1 - O que é uma release
### 7.2 - Integração contínua
### 7.3 - Entrega contínua
## 8 - A infraestrutura
### 8.1 - A nuvem
### 8.2 - Datacenter próprio


# 1 - O Manifesto

Desenvolver software é uma atividade secular, tendo o seu primeiro algoritimo público em meados de ~1840 por Ada Lovelace. Nesses mais de 180 anos novas tecnolgias de hardware e software surgiram e junto vieram muitas metodologias, políticas e as ditas "boas práticas". Nosso objetivo é continuar desenvolvendo software de forma simples, objetiva e escalável (nas pespectivas técnicas e de negócio).

Quando se pensa em criar um produto, na pespectiva de negócio existem práticas como: Balanced Scorecard, Plano de Negócios, Planejamento Estratégico, Canvas, StartUP Exuta, MVP, Errar rápido/Corrigir rápido e por vai. Pensando em tecnologias, existem centenas de linguagens, dezenas de bancos de dados, arquiteturas, metodologias, provedores de infra e etc.

É realmente um overhead de informação e as nossas escolhas são cruciais para o sucesso dos nossos projetos. Aqui, daremos liberdade criativa com responsabilidade, objetivando um software de alta qualidade ao menor custo possível. Responsabilidade pode ser vista como limites a serem respeitados, restrições impostas por nós mesmos, visando o bem comum.

Desenvolvemos softwares não somente para os clientes, mas para outros desenvolvedores que irão manter os nossos códigos. Ter isso em mente é essencial para o trabalho em equipe. Entendemos a importância de aprender metodologias, conceitos e tecnologias como clean code, clean architecture, DDD, arquitetura hexagonal, event-driven architecture, G0F, GRASP, SOLID, linguagens funcionais, scrum, entre outros. Entretanto, não acreditamos em uma solução "by the book" para todas as nossas necessidades. As diferentes formas de se desenvolver software, existem porque, em algum momento, resolveram de forma satisfatória um determinado problema. Os problemas são distintos e o momento muda, portanto, cabe a nós, a cada projeto, definirmos a arquitetura e as boas práticas a serem utilizadas.

Por padrão, iremos sempre desenvolver software da forma mais idiomática e canônica possível relativa a linguagem e/ou framework escolhido. Esta decisão torna o projeto mais fácil de ser mantido por diferentes profissionais, em diferentes momentos.

Todo projeto de software deve ter como documentação mínima os seguintes documentos:
1. ARCH.md - Documento de arquitetura e tecnologias escolhidas.
2. README.md - Documento que ensina a subir o software em ambientes de desenvolvimento (localmente), homologação e produção.

Sabemos que sempre irá existir trade-offs em nossas escolhas e, por essa razão, prezamos pela transparência e aceite dos mesmos.
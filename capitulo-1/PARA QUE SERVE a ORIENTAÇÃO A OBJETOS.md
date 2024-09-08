05-02-24 

referencia para um artigo 

The Principles of OOD (MARTIN, 2005)

utilização de OO
- **criar um modelo de domínio**
- **gerenciar as dependências do seu código**

## OO como ferramenta de domínio
OO consegue representar em código, os conceitos do problema que estamos resolvendo. 

**criar um domain model que traduza para uma linguagem de programação o problema a ser resolvido**


#### metodologia e tecnicas
- class-responsibility-collaboraiton(CRC) - proposta por ward cunnigham e kent beck para explora os possíveis objetos de um sistema e suas **reponsabilidades**
- Feature-driven development (FDD) - utilizar as funcionalidade do sistema como guia para a modelagem dos objetos
- Domain-driven design (DDD) - descrita por eric evans, em que a modelagem é focada em representa , em código, a linguagem dos especialista em negócio.

## OO como ferramenta de gerenciamento

Um bom código OO, com as dependências administradas com cuidado, leva a mais flexibilidade, robustez e** possibilidade de reúso**.

### Técnicas de gerenciamento de dependências.

-  Grasp patterns/principles
- dependency injection
- design patterns
- princípios SOLID - foco do livro

**Os princípios Solid focam bastante no gerenciamento de dependências.**

## Os Princípios SOLID

- single responsibility principle
- open/closed principle
- liskov substituition principle
- interface segregation principle
- dependncy inversion principle

artigo de refencia: Getting a SOLID start (MARTIN, 2009)

## Os princípios de coesão e acoplamento de módulos

##### princípios sobre coesão

- release reuse equivalency principle - componentes de modulo devem ser reutilizados de forma independente
- common closure principle - classes que mudam juntas devem esta no mesmo modulo
- common reuse principle - classes reutilizadas juntas devem está no mesmo modulo

#### Princípios de acoplament de módulos

- acyclic dependencies principle 
- stable dependencies principle
- stable abstractions principle

## Os dez mandamentos de OO

1. Entidades de software devem ser abertas para extensão, mas fechadas para modificação
2. classes derivas devem ser usáveis de através da interface da classe base sem a necessidade de o usuários saber a diferença.
3. detalhes devem depender de abstrações. Abstrações não devem depender de detalhes
4. A granularidade do reúso é a mesma que a granularidade da entrega? 
5. as classes de um componente entrege devem compartilha um fechamento comum
6. as Classes de um componente entregue devem ser reusadas juntas.
7. a estrutura de dependências dos componentes entregues deve ser um grafo acíclico direcionado(DAG), não pode have cíclicos
8. Dependências entre componentes entregues devem ser na direção da estabilidades?
9. quanto mais estável um componente entregue é, mais deve consistir de classes abstratas. um componente completamente estável deve consistir de nada mais que classes abstratas.
10. Onde possível, use patterns comprovados para resolver prblemas de design.
11. Ao atravessa dosi paradigmas diferentes, constura uma camada de interfaces que os separe. Não polua uma lado com paradigma dos outro

### Os princípios toman forma



*Uma classe tem um bom design, quando a classe só tem um  interessado*

*uma classes com muitos interessados, por diversas mudanças*

*uma classes tem muitas responsabilidades quando tem vários interessados*

muitas responsabilidades significa muitas razões pra ser modificada

### Single responsbility principle (SRP)
Uma classe deve ter um, e apenas um, motivo para ser modificada

### SRP E CLASSES COESAS

#### Adesão 
Quando algo externo, gruda coisas relacionadas - composição de objeto - acoplamento

#### Coesão
quando a união é natural

classes coesa têm uma característica semelhante: os conceitos que essas classes representam estariam relacionado e separá-los  seria pouco natural

### como identificar classes pouco coesas
- Possuem muitos métodos diferentes
- sao modificadas com frequência
- não para nunca de crescer

### Métrica: LCOM 

LCOM (Lack of Cohesion inMethods). A LCOM mede o grau em que os métodos de uma
classe acessam todos os atributos. De acordo com essa métrica,uma classe totalmente coesa (cujo LCOM é 0) teria todos os métodos acessando todos os atributos.

### Responsabilidades e srp no cotuba

### Peguntas boas SRP
- quais as responsabilidades dessa classe
- quais os motivo para essa clase ser modificada

### Cotuba
- Lê as opções da linha de comando
- Renderiza arquivos .md para HTML
- gera um ebook no formato .pdf
* gerar um ebbok no formato .epub

Pensando em motivos para mudanças, cada detalhe relacionado a essas responsabilidades poderia levar a uma necessidade de modificação da classe main.
#### imports
Os imports podem ser um bom indicador de muitas responsabilidades, classes que utilizam muitos pacotes diferentes tem grande chancer de precisaram ser modificadas por vários motivos


### Utilizando os principio SRP na pratica

- Criar uma classe para ler as opções de linha de comando
- Uma classe para gerar pdf
- Uma classe para gerar epub

Depois de das refatorações , foram extraidas três novas classes da main
- LeitorOpcoesCLI
- GeradorPDF
- GeradorEPUB
![[classes_reponsabilidades_definidas.png]]

==Refatoração: uma alteração feita na estrutura interna do software para torná-lo mais fácil de ser entendido e menos custoso de ser modificado sem altera seu comportamento observável.==

*Modificá-lo em pequenos passos até chegar a um código extensível e com responsabilidades bem definidas é uma maneira de ver a verdadeira utilidade de OO.*

## Duplicação

*No fim das contas, quando temos repetição de código, há algo novo pedindo para nascer.*

Pragmatic Programmer (HUNT; THOMAS,1999)

Dont't Repeat Yourself (não se repita)

*Todo bloco de conhecimento deve ter uma representação única, sem ambiguidades e dominante num sistema*

*Dependência é o principal problema em desenvolvimento de software. Duplicação é o sintoma. Mas, ao contrário da maioria dos problemas na vida, nos quais eliminar os sintomas*
*faz com que um problema mais grave apareça em outro lugar,eliminar duplicação nos programas elimina a dependência.*
*Kent Beck, no livro TDD by Example (BECK, 2002)*

*A primeira vez que você faz algo, apenas faça. Na segunda vez que você faz algo parecido, incomode-se com a duplicação, mas duplique de qualquer forma. Na terceira vez que você faz algo semelhante, refatore.*


**Nessa parte do livro ele refatora com a criação de uma nova classe responsável por transforma o md em html**

- RenderizadorMDParaHTML

![[main_coordena_classes.png]]

### Avaliando opções de design do código

Uma questão importante é definir quem chamar quem

Main chama rederizadorMdParaHtml, que, por sua vez, chamar GeradorPDF e GeradorEPUB
![[main_chma_apenas_renderizador.png]]

 main chamar RenderizadorMDParaHtml, GeradorPDF e GeradorEPUB
![[main_coordena_classes.png]]

Pensando em termos de distribuição de responsabilidades de responsabilidades, ter a lasse main como "coordenadora" é uma opção melhor. 


### Um Domain Model Simples

Uma opção para resolve como o html vai ser passado para os gerado de pdf e epub é criar um modelo de dominio.
* tem um formato (PDF ou EPUB), um arquivo de saída e uma lista de capítulos
* tem um título e um conteúdo HTML

apos a criação do domínio basicamente ele refatora as classes para utilizarem o novo domínio.

### Para saber mais: designe simples
Extreme Programming Explained (BECK, 2000)

- passa em todos os teste automatizados
- não contém nenhuma duplicação
- expressa as ideias com clareza
- minimizar o número de classes e métodos


È uma maneira subjetiva, porém interessante, de guiar o designe do código em direção a uma boa distribuição de responsabilidades e á alta coesão. 



### MVC, ENTIDADES E CASOS DE USO


#### View 
contém a lógica que monta a UI, e que trata a entrada de dados, recendo eventos do usuário

#### Controller
meio de campo recebe interações do usuário de view e colabora com o model para enviar e obter dados, que são apresentados para view

#### Model
o resto do código, que não tem a ver com UI, realiza cálculas, regras de negócio, persisência, integrações com outros sistema


### separação da apresentação do modelo

- apresentação e modelo têm a ver com diferentes preocupações
- **o mesmo modelo pode ter diferente apresentações**
- **é mais fácil de criar teste automatizados para a lógica de domínio do que pra o código de ui**


#### SOLID para Ninjas
- buscam dados do BD
- implementam regras de negócio
- enviam emails
- chamam Web Services
- enviam resultados para a View (o que realmente deveriam fazer).

### Entidades e casos de uso

*Uncle Bob, no livro Clean Architecture (MARTIN, 2017), cita o trabalho de Ivar jacobson para argumentar que regras de negócio são composta por dois tipos de objetos: entidades e os casos de uso*

As entidades são objetos que contém dados e funções que disponibilizam regras de negócio críticas . Devem ser totalmente independentes de como os dados são armazenados em banco de dados, de como são apresentado em uma UI e dos frameworks utilizados.

Casos de uso não descrevem como o sistema vai ser para o usuário nem como os dados chegam ou saem do sistema. São focados na interação com as entidades.


### GRASP General Responsibility Assignment Software Patterns/Principles

* Alta coesão
* Baixo acoplamento
* polimorfismo
* indireção um intermediário que media as interações entre
outros objetos, evitando acoplamento direto
* Variações protegidas: uma interface estável em volta de
pontos de variação do design.

### Patterns específicos

* Information expert: uma classe que tem a maior quantidade de informação para cumprir uma responsabilidade
* Controller: um "ponto de entrada" do sistema que receber "eventos" do usuário ou uma representação de um "caso de uso" no código.
* Creator : encapsula a criação de outros objetos
* Pure fabrication: classe que não existe no domínio de negócio mas promove baixo acoplamento e alta coesão.

Modularity and testability (BROWN, 2014) - 

***devemos fazer uma doação para a caridade toda vez que digitarmos public class sem pensar se essa classe realmente precisa ser pública.***


### Responsabilidades no nível de métodos
O SRP é focado no nível de classes e, mais especificamente, em ter poucos motivos para mudá-las.

porém um método deveria fazer apenas uma coisa.

### Contra Ponto: críticas ao SRP

Uma reflexão sobre o Princípio da responsabilidade única (SOUZA, 2020), Alberto Souza critica a subjetividade do SRP

CDD (Cognitive Driven Development), que sugere um design de código focado em
torno da facilidade de entendimento do código.

#### SOLID is not solid (COPELAND, 2019)

David Copeland também critica o SRP por ser vago na definição do que é
"responsabilidade" e no que seriam as razões para modificar uma classe.

### CUPID - the back story (NORTH, 2021), 
Dan North hama o SRP de Princípio Inutilmente Vago.
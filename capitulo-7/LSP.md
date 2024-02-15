
Paulo Silveira, no artigo Como não aprender orientação a objetos: Herança (SILVEIRA, 2006)

*Se um gato possui raça e patas, e um cachorro possui raça, patas e tipoDoPelo, logo Cachorro extends Gato ? Pode parecer engraçado, mas é (...) herança por preguiça, por comodismo, porque vai dar uma ajudinha. A relação “é um” não se encaixa aqui, e vai nos gerar problemas.*



*A ideia intuitiva de um subtipo é aquela cujos objetos fornecem todo o comportamento de objetos de outro tipo (o supertipo) mais algo extra. Barbara Liskov*


Um Set não é um subtipo de uma lista, nem o contrário. Em um conjunto (como uma implementação de Set do Java) um mesmo elemento só pode ser adicionado uma vez. Já em uma lista (como um List do Java) é possível repetir elementos.

Um detalhe importante é destacado pela autora: não basta que a assinatura dos métodos seja a mesma, as operações de um subtipo devem ter o mesmo comportamento do supertipo.


### subtipos
Arrays, sequências (como um LinkedList do Java) e conjuntos indexados (como um LinkedHashSet do Java) são subtipos de uma coleção indexada, já que possuem uma
operação de busca por um elemento de uma determinada posição.

Podemos fazer mais coisas que a classe mãe e até alterar alguns comportamentos, mas não podemos fazer nada a menos.

## Favorecendo composição à herança

- Favoreça composição à herança. - Design Patterns (GAMMA et al., 1994)
- Effective Java (BLOCH, 2001): "Item 14: Prefira composiçãoà herança."

**Multiset da biblioteca Google Guava**

## CONTRAPONTO: CRÍTICAS AO LSP

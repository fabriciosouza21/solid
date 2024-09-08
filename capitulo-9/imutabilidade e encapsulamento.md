``` java
// desde o Java 1.2
this.movimentacoes = Collections.unmodifiableList(movimentacoes);
// desde o Java 9
this.movimentacoes = List.of(movimentacoes.toArray());
// desde o Java 16
this.movimentacoes = movimentacoes.stream().toList();
```

### classes imutáveis

- String
- classes wrappers de tipos primitivos como Integer e Double
- BigDecimal e BigInteger
- java.time e LocalTime

Vantagens
- São simples: só há um estado possível.
- São thread-safe
- Podem ser compartilhados e reutilizados livremente.


*Escolher entre idealismo e pragmatismo é uma decisão que deve ser tomada de acordo com o contexto do projeto, da equipe e da empresa.*


*Use o Builder pattern quando:*
- *o algoritmo para criar um objeto complexo deve ser independente das partes que compõem o objeto e como elas são montadas.*
- o processo de construção deve permitir diferentes representações para o objeto que é construído.*

*Imutabilidade deve ser pensada quando tratamos com api publicas*


## Record
* um atributo private final , imutável, para cada componente, com o mesmo nome e tipo;
- um método de acesso public para cada componente, com o mesmo nome do componente e com retorno domesmo tipo;
- um construtor "canônico", que recebe cada componente e atribui ao atributos privados;
- implementações de equals e hashcode , que verificam cada componente;
- uma implementação de toString que mostra o nome de cada componente com o seu respectivo valor.
Fora isso, um record pode implementar interfaces, declararmembros estáticos, declarar métodos, usar modificadores de acesso e uma série de outros detalhes descritos na JEP 395: Records

## Design Pattern: Iterator

Iterator, uma maneira de acessar sequencialmente um objeto agregado, como uma lista, sem expor sua representação interna.

- interface java.util.Iterator
- hasNext , que retorna um boolean indicando se há mais elementos
- next , que retorna o próximo elemento da iteração

``` java
class ContagemDePalavras {
	static record Contagem(String palavra, int ocorrencias) {
		}
		// código omitido...
		}
	}
```

``` java
class ContagemDePalavras implements Iterable<ContagemDePalavras.Contagem>{
	public Iterator<Contagem> iterator() {
	Iterator<Map.Entry<String, Integer>> iterator = this.map.entrySet().iterator();
	return new Iterator<>() {
		@Override
		public boolean hasNext() {
			return iterator.hasNext();
		}
		@Override
		public Contagem next() {
			Map.Entry<String, Integer> entry = iterator.next();
			String palavra = entry.getKey();
			int ocorrencias = entry.getValue();
			return new Contagem(palavra, ocorrencias);
		}
	}
}

for (ContagemDePalavras.Contagem contagem : contagemDePalavras) {
	System.out.println(contagem.palavra() + ": " +
	contagem.ocorrencias());
}
```
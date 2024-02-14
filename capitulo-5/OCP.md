13-02-2024
## Em busca da abstração perfeita

```java
public class EmissorNotaFiscal {
	private RegrasDeTributacao tributacao;
	private LegislacaoFiscal legislacao;
	private NotaFiscalDAO notas;
	private EnviadorEmail email;
	private EnviadorSMS sms;
	//...
	public NotaFiscal gera(Fatura fatura) {
	List<Imposto> impostos = tributacao.verifica(fatura);
	List<Isencao> isencoes = legislacao.analisa(fatura);
	// método auxiliar
	NotaFiscal nota = aplica(impostos, isencoes);
	notas.salva(nota);
	String mensagemNota = String.format(
	"Sua nota fiscal foi gerada: %s",
	nota.getNumero());
	email.envia(mensagemNota, fatura.getEmail());
	sms.envia(mensagemNota, fatura.getTelefone());
	return nota;
	}
}
``` 

![[acaoPosEmissao.png]]o código se torna extremamente flexível, resistindo a novas necessidades de negócio.

*Código mais genérico é mais flexível mas, por outro lado, pode ser mais difícil de entender.*

```java
public class EmissorNotaFiscal {
	private RegrasDeTributacao tributacao;
	private LegislacaoFiscal legislacao;
	private List<AcaoPosEmissao> acoes;
	//...
	public NotaFiscal gera(Fatura fatura) {
		List<Imposto> impostos = tributacao.verifica(fatura);
		List<Isencao> isencoes = legislacao.analisa(fatura);
		// método auxiliar
		NotaFiscal nota = aplica(impostos, isencoes);
		// modificado
		for (AcaoPosEmissao acao: acoes) {
			acao.faz(nota);
		}
		return nota;
	}
}
```


### Comand
Às vezes, é necessário fazer solicitações para objetos sem saber nada sobre a
operação solicitada ou o destinatário da solicitação.

Protected Variation (LARMAN, 2001): Identifique pontos previstos de
variação e crie uma interface estável em torno deles.

### OPEN/CLOSED PRINCIPLE (OCP)
Entidades de sfotware devem ser abertas para extensão, mas fechadas para modifcação

### Strategy
![[geracao-ebooks.png]]

### Strategy vs Command

- No Command, queremos abstrair uma operação genérica,sem definir o que é feito.
- No Strategy, abstraímos um procedimento específico que faz coisas parecidas de jeitos diferentes.

## Revisitando Abstrações
Seguir o Dip á risca pode levar a códigos com muitas interfaces com apenas uma implementação.

Não vale a pena manter uma abstração com apena uma implementação, mesmo violando o DIP.


Designer pattern 

## Uma Factory Inteligente

``` java
public interface GeradorEbook {
	void gera(Ebook ebook);
	static GeradorEbook cria(String formato) { // inserido
	}
}
```

![[capitulo-5/dependencias-cotuba.png]]

### Condiconais ao polimosrfismo

*Uma boa abstração aliada a polimorfismo nos permite criar geradores de ebook em diferentes formatos sem modificar a classe que usa esses geradores.*

*Quando diferentes comportamentos são agrupados em uma classe, é difícil evitar o uso de declarações condicionais para selecionar o comportamento correto. Encapsular o comportamento em classes de estratégia separadas elimina essas declarações condicionais.*

O polimorfismo é uma ótima ferramenta para remover ifs 

*Objetos têm um mecanismo fabuloso, mensagens polimórficas,que permitem expressar lógica condicional de maneira flexível mas clara.*

*Um bom design OO vai ter poucas condicionais como if-else ou switch-case .*

#### Mapeando formatos para geradores

Maps podem nos ajudar a organizar melhor o código 

``` java
public interface GeradorEbook {
	Map<String, GeradorEbook> GERADORES =
	new HashMap<String, GeradorEbook>() {{
	put("pdf", new GeradorPDF());
	put("epub", new GeradorEPUB());
	}};

	static GeradorEbook cria(String formato) {
	GeradorEbook gerador = GERADORES.get(formato);
		if (gerador == null) {
			throw new IllegalArgumentException(
			"Formato do ebook inválido: " + formato);
		}
		return gerador;
	}

// código omitido...
}
 ```

#### utilizando um enum

``` java
public enum FormatoEbook {
	PDF(new GeradorPDF()),
	EPUB(new GeradorEPUB());
	private GeradorEbook gerador;
		FormatoEbook(GeradorEbook gerador) {
		this.gerador = gerador;
	}
public GeradorEbook getGerador() {
	return gerador;
}
```


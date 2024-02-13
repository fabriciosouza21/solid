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
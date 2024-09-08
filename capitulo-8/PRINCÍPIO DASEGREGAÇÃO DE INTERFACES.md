

## O PRINCÍPIO DA SEGREGAÇÃO DE INTERFACES

- o ISP trata de coesão para interfaces

*Clientes não devem ser obrigados a depender de métodos que eles não usam*
**um bom indentificador são implementações que simplesmente retorna null** 

basicamente ele separar as interfaces

![[interfaces-segregadas.png]]

## SEPARANDO CLASSES, NÃO APENAS INTERFACES

```java
public class NotaFiscal {
	private Cliente cliente;
	private Endereco entrega;
	private Endereco cobranca;
	private List<Item> itens = new ArrayList<>();
	private List<Desconto> descontos = new ArrayList<>();
	private FormaDePagamento pagamento;
	private BigDecimal valorTotal;
	// getters...
// restante do código...
}
```

CalculadoraDeImposto usa apenas o método getItens da NotaFiscal

separar melhor a entidade e torna encapsulando o valor, para que calculardora possar utilizar somente o atributo que ele precisar

```
public interface Tributavel {
List<Item> itensASeremTributados();
}
```

``` java
public class CalculadoraDeImposto {
	// código omitido...
	// modificado
	public BigDecimal calcula(Tributavel tributavel) {
	BigDecimal total = BigDecimal.ZERO;
	// modificado
		for (Item item : tributavel.itensASeremTributados()) {
		// código omitido...
		}
	}
}
```

*Se você tiver uma classe que tenha vários clientes, em vez de carregar a classe com todos os métodos de que os clientes precisam, crie interfaces específicas para cada cliente e*
*implemente-as na classe.*

### Para saber mais: Atitude limitante vs. permissiva

##### Directing attitude
uma atitude limitante, controladora, direcionadora, restritiva. É uma mentalidade preocupada
em prevenir desenvolvedores ruins de causar danos. Leva a designs robustos, mas que limitam as possibilidades.

#### Enabling attitude
uma atitude permissiva, habilitante,tolerante, capacitadora. É uma mentalidade que dá
liberdade aos desenvolvedores, sem prevenir que façam coisas ruins. Leva a designs flexíveis, mas que podem ser usados de maneira errada.

## Contraponto: criticas ao ISP

Por isso, North diz que o ISP não é um princípio, mas um pattern: uma estratégia que funciona em um determinado contexto e traz vantagens e desvantagens.

*Copeland diz ainda que a necessidade do ISP **só surge quando há classes pouco coesas** e que o ISP levado ao extremo leva a uma proliferação de interfaces de apenas um*
*método*.


*Essa segunda interpretação, para Kaminski, não é um princípio para ser adotado inicialmente, mas uma ferramenta para lidar com as fronteiras de um sistema.*

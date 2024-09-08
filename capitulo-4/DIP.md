12-02-2024

``` java  
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
	net.sargue.mailgun.Configuration configuration =
	new net.sargue.mailgun.Configuration()
	.domain("empresa.com")
	.apiKey(System.getenv("MAILGUN_API_KEY"))
	.from("Empresa", "contato@empresa.com");
	net.sargue.mailgun.Mail mail =
	net.sargue.mailgun.Mail.using(configuration)
	.to(fatura.getEmail())
	.subject("Sua nota fiscal")
	.text(mensagemNota)
	.build();
	email.envia(mail);
	com.twilio.rest.api.v2010.account.Message smsMessage =
	com.twilio.rest.api.v2010.account.Message.creator(
	new PhoneNumber("+14159352345"),
	new PhoneNumber(fatura.getTelefone()),
	mensagemNota)
	.create();
	sms.envia(smsMessage);
	return nota;
	}
}
```

EmissorNotaFiscal tem muitas dependências :

* RegrasDeTributacao,
* LegislacaoFiscal,
* NotaFiscalDAO, 
* EnviadorEmail
* EnviadorSMS

## Acoplamento, Estabilidade e volatilidade

**A falta de cuidados com as dependências do nosso código faz com que a flexibilidade e o reúso fiquem prejudicados.**

### Acoplamento bom x acoplamento ruim
Classes estáveis podem ser utilizadas por que dificilmente são alteradas é o caso do pacote java.lang, por isso podemos depender delas tranquilamente.

classes voláteis são classes que podem ser mudadas com frequência, bibliotecas para envio de email frequentemente são substituitas, nesse caso vão ser trocadas com frequencia.

## Abstrações e inversão da dependências
As abstrações minimizam os impactos de mudanças em dependências voláteis. **Abstrações são estáveis**: mudam muito menos que implementações.

**Usando classes abstratas ou interfaces, o código não depende mais diretamente da dependência volátil e sim da abstração**. **E a dependência volátil, por sua vez, também depende da abstração**, implementando-a. Por isso, podemos dizer que a dependência é invertida.

#### Evitando vazamento de detalhes de implementação

É importante **evitar que detalhes da dependência vazem nas nossas abstrações**, evitar que as abstração recebam dependências que podem ser alterados, os detalhes de implementação devem está na classe que implementa o a abstração

#### Abstrações conceitualmente abstratas

Precisamos evitar vinculá-las conceitualmente a tarefas muito particulares, no caso abaixo o nome do parâmetro fica vinculado a uma nota fiscal.


``` java
public interface EnviadorSMS {
void enviaNota(String mensagemNota,
String telefoneClienteNota); // abstração ruim
}
```


## Regras de negócio x Detalhes

A parte mais importante do nosso código é implementação da regras de negocio, as coisas técnicas são detalhes de implementação UI Web e/ou Mobile, persistência e Banco de Dados,integração com outros sistemas, frameworks, protocolos.
**Os detalhes técnicos são  importantes, são os mecanismo de entrega das regras de negócio para os usuários.**

## Código de alto nível e baixo nível

- códigos de **alto nível** seriam os código que implementam regras de negócio.

- Códigos de **baixo nível** seriam os mecanismo de entrega, os detalhes de implementação mais técnicos.

### Alto/baixo nível e entradas/saídas

Uncle bob diz que código de alto **nível é aquele mais distante das entradas ou saídas do sistema e**, por isso, muda menos frequentemente e por razões mais importantes, relacionadas ao negócio.

Já o código de baixo nível, **mais próximo das entradas ou saídas,** muda mais frequentemente e com mais urgência.

*Porém, para quem desenvolve o Hibernate, gerar código SQLcomum entre os bancos de dados pode ser considerado alto nível, já que está relacionado ao problema que uma biblioteca de ORM resolve. Variações entre os bancos de dados, como AUTO_INCREMENT , SEQUENCE ou IDENTITY na geração de PKs, seriam detalhes de implementação de baixo nível.*

## O PRINCÍPIO DA DEPENDÊNCIAS (DIP)
*Abstrações não devem depender de detalhes. Detalhes devem depender de abstrações.*

Em outras palavras, regras de negócio não devem depender de **mecanismo de entregas**, mas de abstrações desses detalhes de implementação.

### DIP, camadas e regra da dependência
-  código de alto nível (Negócio) depende de código de baixo nível (Persistência) - errado
- fazendo com que Persistência dependa de Negócio e não o contrário.
![[tres-camadsxdependenciasinvertidas.png]]

*Dependências devem apontar apenas para dentro, em direção às*
*regras de negócio.*

13-02-2024
## Dip No cotuba

- Alto nível: as classes Cotuba , Ebook e Capitulo , que são relativas ao domínio do problema.
- Baixo nível: as classes relativas a UI, Main e LeitorOpcoesCLI , e as classes que implementam detalhes técnicos como RenderizadorMDParaHTML , GeradorPDF e GeradorEPUB
### Problema na classe cotuba 
é classificada como alto nivel, mas depende de varias abstrações de baixo nivel!
![[capitulo-4/dependencias-cotuba.png]]

### invertendo as dependências da classe Cotuba
as dependencias do cotuba vão ser invertidas

### Design pattern: Factory

*Observação: como todo método de uma interface é público, mesmo sendo estático, podemos omitir o modificador de acesso public .*

``` java
public interface GeradorPDF {
	void gera(Ebook ebook);
	// inserido
	// fabricar
	static GeradorPDF cria() {
	return new GeradorPDFImpl();
	}
}
```

### código final 
![[dependecias-invertidas.png]]


##  CONTRAPONTO: CRÍTICAS AO DIP

### SOLID is not solid (COPELAND, 2019),

*Há ainda uma crítica à promessa de flexibilidade e reúso que, de acordo com Copeland, leva a códigos desnecessariamente complexos.*

*argumenta que a obsessão com inversão de dependências causou*
*bilhões de dólares em custos desnecessários.*

*Também há a argumentação de que a promessa de poder trocar uma dependência complexa como um banco de dados evapora assim que tentamos realizar essa tarefa na prática.*

**A inversão de dependência é uma ferramenta que deve ser usada com sabedoria, validar a real necessidade de criar a inversão.**
# Testes unitários com Quarkus

## Requisitos

* Java 8 ou 11 (desejavel 11)
* Maven 3.6.3 ou >
* Quarkus
* JUnit 5
* Mokito
* Quarkus
* Curiosidade com TDD

## TDD

Os testes unitários são muito utilizados no TDD (Test-Driven Development), uma metodologia de desenvolvimento que se baseia nos testes para implementar as funcionalidades (ao invés de fazer o contrário, que é mais comum)

Boas práticas na práticas

Quando falamos em TDD é bom destacar que estamos falando em utilizar o Teste Unitário diretamente como ferramenta de desenvolvimento, portanto algumas práticas citadas aqui são aplicáveis como boas práticas direcionadas ao uso do teste unitário.

Para escrever testes de qualidade, precisamos ter em mente três ideias centrais:

1. Testes precisam ser confiáveis. Seus testes precisam dar a certeza de que eles não possuem bugs

2. Testes precisam ser legíveis. Os testes devem indicar claramente o que está acontecendo à primeira vista. Um teste que não dá pra saber o que está sendo testado não serve para nada.

3. Testes precisam ser sustentáveis Os testes precisam seguir a escalabilidade do software que eles testam. Idealmente os testes devem ser imutáveis, portanto é importante garantir que eles se comportem da forma intencionada conforme o código é escalado.

É importante ter sempre esse tripé em mente quando estiver escrevendo testes unitários, especialmente no caso do Desenvolvimento Guiado por Testes.

1 - Apenas escrever código em resposta a um teste falhando
Isso é a base de trabalhar com Desenvolvimento Guiado por Testes: escrever um teste que falha e fazê-lo ter sucesso com o código de produção, então os seus testes sempre devem ser escritos antes dos métodos de produção. Pode parecer um pouco óbvio, mas é muito comum perder o foco e "furar" o TDD quando a implementação é algo muito simples ou que o programador tem certeza que "não tem como dar errado". Mas a verdade é que "furar" o TDD é uma má prática e acaba deixando o software vulnerável a erros bobos que poderiam ter sido evitados. Isso auxilia a criação de testes confiáveis já que você sempre vai ver seu teste falhar e ter sucesso

2 - Nomeie os seus testes de forma autoexplicativa
Código de testes deve ser mais legível que código de produção, então não há necessidade de nomear os métodos de teste de forma minimalista. Prefira nomear os testes da forma que fique o mais claro possível o que o teste está fazendo, mesmo que o nome fique grande e/ou não siga convenções nominais de métodos.

3 - Rodar os testes diversas vezes
Teste antes de codificar, após codificar e teste mais uma vez após refatorar o código, não importa o quão pequena for a alteração. Adotando esse tipo de prática, você assegura que seu código não vai quebrar em nenhum momento e passar despercebido.

4 - Teste apenas uma coisa por vez
Existem ferramentas de Teste Unitário que permitem que os métodos de teste tenham mais de um assert em si, como por exemplo o JUnit. Apesar de ser permitido pela ferramenta, isso é considerado uma má prática porque prejudica a clareza do teste sendo feito, aumenta a chance de ter bugs nos seus testes e torna debugar mais trabalhoso. Teste uma coisa só por vez.

5 - Não ponha lógica nos seus testes (Pelo menos tente)
Testes não devem conter lógica. Se o seu teste possui um if ou switch é porque você, provavelmente, está testando mais de uma coisa, e aumenta muito a chance de ter bugs no seu código de teste.

6 - Testes existentes devem passar antes de criar um novo teste
As vezes é tentador ignorar um teste que falha e partir pra criação do outro. Ou ainda escrever múltiplos testes de uma vez antes de implementar o código de produção. Isso deve ser evitado porque prejudica a clareza dos seus testes. Um dos objetivos do TDD é que a implementação do código de produção deve funcionar como o esperado sempre e quebrar essa regra geralmente se torna adiar o inevitável, já que o programador terá que implementar o código de produção eventualmente.

7 - Faça o teste passar da forma mais simples possível
A ideia é que quanto mais simples for a implementação mais fácil e melhor será de manter em produção ela será. Isso quer dizer que a maioria das aplicações funcionam melhor quando são mantidas simples ao invés de desnecessariamente complexas.

8 - Não use dependência entre testes
Cada teste deve executar independentemente de outros, portanto, programadores devem ser capazes de executar cada teste individualmente, grupos de teste ou todos os testes de uma vez. Haver dependências entre os testes os tornam mais propícios a bugs com a introdução de novos testes.

9 - Não remova nem altere testes antigos
Evite ao máximo possível alterar ou remover qualquer teste que já esteja passando. A grande vantagem de utilizar o Teste Unitário, e por consequência o TDD, é a manutenção do código de testes que é executado após cada alteração no código de produção. Alterar ou remover testes que funcionam faz perder totalmente o propósito dos testes que foram construídos.

As únicas exceções seriam no caso da implementação ser removida ou ter seu propósito alterado.

10 - Tente sempre utilizar o padrao Given (Dado) / When (quando) / Then (entao) 

Isso auxilia os demais desenvolvedores a entenderem o teste onde no **Given** (dado) é produzido ou realizado o mock de tudo que você precisa para testar aquele cenário. Ao chamar o método ou a classe **When**, eu **Then** (então) você obtem o seguinte resultado. 

```java
// codigo abastraido

		/** 
  		* If an item is loaded from the repository, the name of that item should 
  		* be transformed into uppercase. */ 
  	  	@Test 
	  	  	public void shouldReturnItemNameInUpperCase() { 
		  	// 
		  	// Given //
		  	// fornecido valores
		  	// para java 8 
		  	// Item mockedItem = new Item("it1", "Item 1", "Este e o primeiro item", 2000, true); 
		  	// para java 11
		  	var mockedItem = new Item("it1", "Item 1", "Este e o primeiro item", 2000, true); 
		  	when(itemRepository.findById("it1")).thenReturn(mockedItem); 
		  
		  
		  	// When //
		  	// Quando eu executo algo
		  	// para java 8
		  	// String result = itemService.getItemNameUpperCase("it1"); 
		  	// para java 11 
		  	var result = itemService.getItemNameUpperCase("it1"); 
		  
		  	// Then //
		  	// então eu obtenho asassertivas
		
		  	verify(itemRepository, times(1)).findById("it1"); assertThat(result, is("ITEM 1")); 
	 	}
```

## Maven dependencias

```xml
<!-- omitido o começo do pom.xml -->
<dependency>
	<groupId>io.quarkus</groupId>
	<artifactId>quarkus-junit5</artifactId>
	<scope>test</scope>
</dependency>
<dependency>
	<groupId>io.quarkus</groupId>
	<artifactId>quarkus-junit5-internal</artifactId>
	<scope>test</scope>
</dependency>
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-junit5-mockito</artifactId>
    <scope>test</scope>
</dependency>
<!-- opctional mas deixa o seu código mais legivel -->
<dependency>
	<groupId>io.rest-assured</groupId>
	<artifactId>rest-assured</artifactId>
	<scope>test</scope>
</dependency>
<!-- em caso de teste com repositorio utilize o banco de dados h2 -->
<dependency>
    <groupId>io.quarkus</groupId>
    <artifactId>quarkus-jdbc-h2</artifactId>
</dependency>
<!-- continua -->
```

## import static

Utilize import static pois eles facilitam a leitura dos testes.

Exemplo:

```java
// importes
import static io.restassured.RestAssured.given;
import static org.hamcrest.CoreMatchers.is;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;


	//exemplo de códigos	

 	// implementacao omitida
	// use dessa forma 
	when(mock.findAllUsuariosByDataSource()).thenReturn(usuarios);
	// ao inves de usar com Mockito.*
	//Mockito.when(mock.findAllUsuariosByDataSource()).thenReturn(usuarios);

	// use dessa forma
	verify(itemRepository, times(1)).findById("it1"); assertThat(result, is("ITEM 1"));
	// ao inves de usar com Mockito.*
	//Mockito.verify(itemRepository, times(1)).findById("it1"); assertThat(result, is("ITEM 1"));

	// use dessa forma
	given().when().get("/config/query").then().statusCode(200).body(is(H2Profile.QUERY));
	// ao inves de usar
	//RestAssured.given()..when().get("/config/query").then().statusCode(200).body(CoreMatchers.is(H2Profile.QUERY));

```

## Teste unitario sobrescrevendo o profile com Quarkus

Se voce desejar sobrescrever alguma propriedade do application.properties para algum test em especifico pode utilizar esta forma

```java
import org.eclipse.microprofile.config.inject.ConfigProperty;

import javax.inject.Inject;
import javax.ws.rs.GET;
import javax.ws.rs.Path;
import javax.ws.rs.Produces;
import javax.ws.rs.core.MediaType;
import javax.ws.rs.core.Response;
import java.util.logging.Logger;

@Path("/config")
public class SgnResource {

    private static Logger LOGGER = Logger.getLogger(SgnResource.class.getName());

    @ConfigProperty(name = "query.pix")
    private String query;

   
    @GET
    @Path("/query")
    @Produces(MediaType.TEXT_PLAIN)
    public Response getQuery() {
        return Response.ok(query).build();
    }
}
```

Crie um profile de test

```java
import io.quarkus.test.junit.QuarkusTestProfile;

import java.util.Map;

public class H2Profile implements QuarkusTestProfile {

    public static final String QUERY = "uma query diferente";

    @Override
    public Map<String, String> getConfigOverrides() {
        return Map.of(
                "quarkus.datasource.url", "jdbc:h2:tcp://localhost/mem:test",
                "quarkus.datasource.driver", "org.h2.Driver",
                "quarkus.hibernate-orm.database.generation", "drop-and-create",
                "quarkus.hibernate-orm.log.sql", "true",
                "query.pix", QUERY
        );
    }

    @Override
    public String getConfigProfile() {
        return "h2";
    }
```

Teste

```java
import io.quarkus.test.junit.QuarkusTest;
import io.quarkus.test.junit.TestProfile;
import org.junit.jupiter.api.Test;

import javax.inject.Inject;

import java.text.MessageFormat;

import static io.restassured.RestAssured.given;
import static org.hamcrest.CoreMatchers.is;

@QuarkusTest
@TestProfile(H2Profile.class) //injeta o profile de teste customizado
class SgnResourceTest {

    @Inject
    SgnResource api;

    @Test
    public void testLoadQuery() {
        given()
                .when().get("/config/query")
                .then()
                .statusCode(200)
                .body(is(H2Profile.QUERY));
    }
}
```


## Teste unitario com JUnit5 e Mockito utilizando Quarkus e injeção de depedencias

Classe de repositorio

```java
@ApplicationScoped
public class UsuarioRepository implements PanacheRepository<Usuario> {
    @Inject
    private AgroalDataSource dataSource;

    public List<Usuario> findAllUsuariosByDataSource() {
        List<Usuario> dados = new ArrayList<>();
        try (PreparedStatement preparedStatement = dataSource.getConnection().prepareStatement("SELECT id, nome, sobrenome, idade from Usuario ")) {
            ResultSet rs = preparedStatement.executeQuery();
            Usuario usuario;
            while (rs.next()) {
                usuario = new Usuario();
                usuario.setId(rs.getLong("id"));
                usuario.setNome(rs.getString("nome"));
                usuario.setSobrenome(rs.getString("sobrenome"));
                usuario.setIdade(rs.getInt("idade"));

                dados.add(usuario);
            }
        } catch (SQLException e) {
            LOGGER.severe(e.getMessage());
        }
        return dados;
    }


}
```

Classe de servico

```java
@ApplicationScoped
public class SgnService {

    @Inject
    private UsuarioRepository repository;

    private static final Logger LOGGER = Logger.getLogger(SgnService.class.getName());

    public List<Usuario> findAllUsuariosByDataSource() {
        return repository.findAllUsuariosByDataSource();
    }
}
```

Classe de teste

```java
@QuarkusTest
public class SgnServiceTest {

    @Inject
    SgnService service;

    @Test
    public void testFindAllUsuariosByDataSource() {
		//given
        UsuarioRepository mock = Mockito.mock(UsuarioRepository.class);
        List<Usuario> usuarios = new ArrayList<>();
        usuarios.add(new Usuario());
        
		// mock com mockito
		when(mock.findAllUsuariosByDataSource()).thenReturn(usuarios);

        // faz a substituição da injeção de dependencia 
        QuarkusMock.installMockForType(mock, UsuarioRepository.class);
		 
		// when 
		int size = service.findAllUsuariosByDataSource().size();

		// then
        assertEquals(1, size);
    }
}
```

[link externo referencia quarkus com mockito](https://quarkus.io/blog/mocking/)


## Utilização de override de metodos anonimos para fugir de implementações com metodos static

- Alguns metodos static são difices de testar, mas podem ser contornados por um override de metodo de classe que o invocar no seu teste.
- Outra solução que você pode buscar é a utilização do mockito-inline para fugir do PowerMockito e testar metodos static

```java
	public class OutraClasseDeNegocio {

	// este metdo ficará coberto só o da integração 'enviaMsgParaMQSViaSgnMensageria' não ficará
	public boolean hasMensagemRetornoAsString() {
		String msg this.enviaMsgParaMQSViaSgnMensageria();

		return msg.contains("RETORNO");
	}

	// tente isolar a chamada dentro de um metodo seu
	// você não terá ele na sua coverage, mas desviará dele não fazendo a sua chamada
	// mantenha ele public ou protected para que possa ser acessivel no mesmo packege pelo teste
	public String enviaMsgParaMQSViaSgnMensageria(String fila, String mensagem) {
		return Grpc.publicarMensagemGrpc(fila, mensagem);
	}
}
```

Classe de teste 

```java

class OutraClasseDeNegocioTest {

	private OutraClasseDeNegocio negocio;

	@Test
	public void testFazAlgoComMsg() {
		// given
		// importante faça o override do metodo e de o retorno adequado no seu objeto 
		negocio = new OutraClasseDeNegocio() {
			@Override
			public String enviaMsgParaMQSViaSgnMensageria(String fila, String mensagem) {
				return "RETORNO_MSQ"; // coloque o retorno que você aguarda da chamada no caminho feliz
			}
		}

		// When / Then
		//desta forma você consegue isolar e testar o seu negocio, sem se preocupar com a integracao com o Grpc
		assertTrue(negocio.hasMensagemRetornoAsString());
	}
}
```

## Criando um teste que utiliza a chamada de uma classe que implementa o padrão Singleton compativel com java 11 sem PowerMockito

- Não recomendo a utilização deste padrão de projeto, porém como há varios subprojetos que utilizam estou colocando como resolver isso.


Parte do corpo da classe ConfiguradorService antes da alteração

```java
public class ConfiguradorService {

	private static final Logger LOGGER = Logger.getLogger(ConfiguradorService.class.getName());

	private static ConfiguradorService instance;

	public ConfiguradorService() {
		super();
	}

	public static synchronized ConfiguradorService getInstance() {
		if (instance == null) {
			instance = new ConfiguradorService();
		}
		return instance;
	}

    /// ..... código continua
```

A solução foi criar uma interface que continha todas as assinaturas dos metodos da classe ConfiguradorService.

Código da interface

```java
package org.todeschini.service;

import java.util.List;

import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorBase;
import org.todeschini.configuracao.utils.enums.TipoServicoEnum;

public interface InterfaceConfigurador {

	public <T extends ComponenteConfiguradorBase> T obter(TipoServicoEnum tipoServico);
	public <T extends ComponenteConfiguradorBase> List<T> obterLista(TipoServicoEnum tipoServico);
	public String obterFinalidade(String finalidade);
	public String obterFuncao(String nomeFuncao);
	public String obterConfiguracaoIndicioFraudePorTipo(String tipoIndicioFraude);
	public String obterConfiguracaoIndicioFraudePorId(String id);
	public String obterBloqueio();
	public String obterConfiguracao(String tipo, String dado);
	public String obterTriagem(String tipo);
}
```

Volte na classe ConfiguradorService e faça com que ela implemente a nova interface

```java
                                 // adicionando isso a classe original 
public class ConfiguradorService implements InterfaceConfigurador {
    // codigo abstraido...
```

* Note que dessa forma não foi mechido em nada que afete a classe ConfiguradorService mantendo-a como toda sua coesão que o padrão Singleton lhe oferece.

Crie uma camada para abstrair a integração

* nota utilize os metodos get e set com o modificador synchronized especialmente devido aos testes quando houver concorrencia

```java
package org.todeschini.integracao;

import org.todeschini.service.ConfiguradorService;
import org.todeschini.service.InterfaceConfigurador;

public class Integrador {
	
	private static volatile Integrador instance;
    private InterfaceConfigurador configurador;
	
	public static synchronized Integrador getInstance() {
		if (instance == null) {
			instance = new Integrador();
		}
		return instance;
	}
	
	public synchronized InterfaceConfigurador getConfigurador() {
		if (configurador == null) {
			setConfigurador(ConfiguradorService.getInstance());
		}
		return configurador;
	}

	public synchronized void setConfigurador(InterfaceConfigurador configurador) {
		this.configurador = configurador;
	}
}

```

Em todas as classe que você gostaria de chamar o Configurador agora utilize da seguinte forma

```java
    String finalidade = Integrador.getInstance().getConfigurador().obterFinalidade("teste");
```

Já para os testes você pode injetar na classe uma outra implementação de InterfaceConfigurador que seja Fake  

Como a configuração é algo muito especifico de cada projeto mantenha o Fake no no seu projeto no diretorio de testes

```java
package org.todeschini.integracao;

import java.util.List;

import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorBase;
import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorMapeamentoEntrada;
import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorMapeamentoSaida;
import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorPesquisaMainframe;
import org.todeschini.configuracao.utils.enums.TipoServicoEnum;
import org.todeschini.service.InterfaceConfigurador;

public class ConfiguradorFake implements InterfaceConfigurador {

    // JDBC driver name and database URL
	public static final String H2_URL = "jdbc:h2:mem:sample;";
	public static final String JDBC_DRIVER = "org.h2.Driver";

	// Database credentials
	public static final String USER = "sa";
	public static final String PASS = "sa";

	public static final String NOME_FINALIDADE = "TESTE";

	public static final String FINALIDADE = "{\"finalidade\" : \"" + NOME_FINALIDADE + "\","
			+ "\"tipo\" : \"BANCO_DADOS\",\"dados\" : {\"tipoConexao\" : \"H@\", \"driver\" : \"" + JDBC_DRIVER + "\""
			+ ",\"url\" : \"" + H2_URL + "\",\"usuario\" : \"" + USER + "\",\"senha\" : \"" + PASS
			+ "\"},\"ativo\" : true}";
	
	// crie os componentes que você deseje utilizar e faça o emcapsulamento dele
	private ComponenteConfiguradorMapeamentoEntrada mapaEntrada = null;
	private ComponenteConfiguradorPesquisaMainframe mapaPesquisa = null;;
	private ComponenteConfiguradorMapeamentoSaida mapaSaida = null;
	private String bloqueio = null;
	
	@SuppressWarnings("unchecked")
	@Override
	public <T extends ComponenteConfiguradorBase> T obter(TipoServicoEnum tipoServico) {
		//adicione todos os tipos de voce desejar, mas lembre-se de criar um atributo e encapsular para faciliar o envio da configuracao
		if (tipoServico.name().equalsIgnoreCase(TipoServicoEnum.MAPEAMENTO_ENTRADA.name())) {
			return (T) mapaEntrada;
		} else if (tipoServico.name().equalsIgnoreCase(TipoServicoEnum.MAPEAMENTO_SAIDA.name())) {
			return (T) mapaSaida;
		} else if (tipoServico.name().equalsIgnoreCase(TipoServicoEnum.PESQUISA_MAINFRAME.name())) {
			return (T) mapaPesquisa;
		}
		return null;
	}

	@Override
	public String obterFinalidade(String finalidade) {
		return FINALIDADE;
	}

	//metodos omiticos sem implementação consideravel

	// encapsulamento dos Configuracoes
	public void setMapaEntrada(ComponenteConfiguradorMapeamentoEntrada mapaEntrada) {
		this.mapaEntrada = mapaEntrada;
	}

	public void setMapaPesquisa(ComponenteConfiguradorPesquisaMainframe mapaPesquisa) {
		this.mapaPesquisa = mapaPesquisa;
	}

	public void setMapaSaida(ComponenteConfiguradorMapeamentoSaida mapaSaida) {
		this.mapaSaida = mapaSaida;
	}

	public void setBloqueio(String bloqueio) {
		this.bloqueio = bloqueio;
	}
}

```

Teste da classe utilitaria Integrador como exemplo de uso do configurador

```java
package org.todeschini.integracao;

import static org.junit.jupiter.api.Assertions.assertNotSame;
import static org.junit.jupiter.api.Assertions.assertSame;

import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.MethodOrderer;
import org.junit.jupiter.api.Order;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.TestMethodOrder;

import org.todeschini.integracao.fake.ConfiguradorFake;
import org.todeschini.service.ConfiguradorService;

@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
public class IntegradorTest {

	@Test
	@Order(1)
	public void isSame() {
		// given / when
		var i1 = Integrador.getInstance();
		var i2 = Integrador.getInstance();

		// then 
		//vai obter dois objetos de Integrador que serão: 
		//a mesma instancias de Integrador 
		assertSame(i1, i2);
	}

	@Test
	@Order(2)
	public void getConfiguradorServiceAsInterfaceConfigurador() {
		// given / when
		var service = ConfiguradorService.getInstance();
		var configurador = Integrador.getInstance().getConfigurador();
		
		// then 
		// vai demostra que as duas instancias obtidas
		// sao de ConfiguradorService sao a mesma
		assertSame(service, configurador);
	}
	
	@Test
	@Order(3)
	public void changeConfiguradorBySetConfigurador() {
		/* given */

		var service = ConfiguradorService.getInstance();
		var fake = new ConfiguradorFake();
		
		Integrador.getInstance().setConfigurador(fake);
		
		/* when / then */

		// vai demostrar que se alterar a o valor da instancia de Integrador não será
		// uma insntancia real de ConfiguradorService 
		// e sim uma implementação da interface InterfaceConfigurador
		assertNotSame(service, Integrador.getInstance().getConfigurador());
	}

	// Nota importante !!!!! 
	// lembre-se de utilizar a reflexao e remover do atributo instance
	// o valor do fake After/ Before cada teste
	@AfterEach
	public void tearDown() {
		try {
			Field i = Integrador.class.getDeclaredField("instance");
			i.setAccessible(true);
			i.set(null, null);

		} catch (Exception e) {
//			e.printStackTrace();
			fail(e.getMessage());
		}
	}
}
```

Exemplo de utilização em classes de negocios ...  

Classe de Negocio 

```java 
package org.todeschini.servico;

import org.todeschini.integracao.Integrador;

import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorMapeamentoEntrada;

public class ClasseDeNegocio {

	private ComponenteConfiguradorMapeamentoEntrada entrada;

	public ClasseDeNegocio() {
		this.entrada = Integrador.getInstance().getConfigurador().obter(TipoServicoEnum.MAPEAMENTO_ENTRADA);
	}
	
	public ComponenteConfiguradorMapeamentoEntrada getEntrada() {
		return entrada;
	}
}
```

Teste da classe de Negocio


```java
package org.todeschini.servico;

public class ClasseDeNegocioTest {

import org.todeschini.integracao.ConfiguradorFake;
import org.todeschini.modelo.Propriedade;

import org.todeschini.configuracao.utils.componente.ComponenteConfiguradorMapeamentoEntrada;

	@BeforeEach
	public void setUp() {
		this.tearDown();
		configurador = new ConfiguradorFake();
		//injeta o seu fake no integrador
		Integrador.getInstance().setConfigurador(configurador);
	}

	// importante !!!!!
	// lembre-se de utilizar a reflexao 
	// e remover do atributo instance o valor do fake After/Before cada teste
	@AfterEach
	public void tearDown() {
		try {
			Field i = Integrador.class.getDeclaredField("instance");
			i.setAccessible(true);
			i.set(null, null);

		} catch (Exception e) {
//			e.printStackTrace();
			fail(e.getMessage());
		}
	}

	@Test
	public void testMeuNegocioComComponenteConfiguradorMapeamentoEntrada() {
		/* Given */

		//para java 8
		//ComponenteConfiguradorMapeamentoEntrada entrada = new ComponenteConfiguradorMapeamentoEntrada();
		//se java 11
		var entrada = new ComponenteConfiguradorMapeamentoEntrada();
		entrada.setCampos(new ArrayList<Propriedade>());
		
		// se java 11
		var p1 = new Propriedade();
		// se java 8
		// Propriedade p1 = new Propriedade();
		p1.setId(1);
		p1.setNome("minhaPropriedade");
		p1.setFixo(true);
		p1.setTipo("Numerico");
		p1.setValor("1")

		entrada.getCampos().add(p1);

		//injeta no mapemaneto de entrada no seu configurador
		configurador.setMapaEntrada(entrada);

		// ... chamada da sua classe de negocio com o fake
		// neste caso o construtor de ClasseDeNegocio faz a chamada para o Integrador e o Configurador
		
		/* When */
		// java 8
		// ClasseDeNegocio negocio = new ClasseDeNegocio();
		// java 11
		var negocio = new ClasseDeNegocio();


		// then 
		// validacoes com assertEquals etc ...
		assertNotNull(negocio.getEntrada());
		assertNotNull(negocio.getEntrada().getCampos());
		assertTrue(negocio.getEntrada().getCampos().size() > 0);

		//se java 8
		//Propriedade prop = negocio.getEntrada().getCampos().get(0);
		// se java 11
		var prop = negocio.getEntrada().getCampos().get(0);
		
		assertEquals(1, prop.getId());
		assertEquals("minhaPropriedade", prop.getNome());
		assertTrue(prop.getFixo());
		assertEquals("Numerico", prop.getTipo());
		assertEquals(1, Integer.parseInt(prop.getValor()));
	}
}

```

## Teste unisários sem concorrencia

Se não quiser concorrencia nos seus testes, cole isso no seu pom.xml na configuração para maven-surefire-plugin 

```xml
<!-- inicio abstraido -->

<properties>
	<compiler-plugin.version>3.8.1</compiler-plugin.version>
	<maven.compiler.parameters>true</maven.compiler.parameters>
	<maven.compiler.source>11</maven.compiler.source>
	<maven.compiler.target>11</maven.compiler.target>
	<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	<project.reporting.outputEncoding>UTF-8
	</project.reporting.outputEncoding>
	<surefire-plugin.version>2.22.1</surefire-plugin.version>
	<jacoco.version>0.8.4</jacoco.version>
	<dbiq.skip>true</dbiq.skip>
</properties>

<!--  abstraido -->

<build>
	<plugins>
		<plugin>
			<artifactId>maven-compiler-plugin</artifactId>
			<version>${compiler-plugin.version}</version>
			<configuration>
				<useIncrementalCompilation>false</useIncrementalCompilation>
				<!-- configura se o codigo é java 11-->
				<source>11</source>
				<target>11</target>
			</configuration>
		</plugin>
		<plugin>
			<artifactId>maven-surefire-plugin</artifactId>
			<version>${surefire-plugin.version}</version>
			<configuration>
				<printSummary>true</printSummary>
				<!-- no parallelos execute nos teste unitatios uma thread de cada vez-->
				<threadCount>1</threadCount>
				<useUnlimitedThreads>false</useUnlimitedThreads>
				<perCoreThreadCount>false</perCoreThreadCount>
				<threadCountSuites>1</threadCountSuites>
				<threadCountClasses>1</threadCountClasses>
				<threadCountMethods>1</threadCountMethods>
			</configuration>
			<executions>
				<execution>
					<id>tests</id>
					<phase>test</phase>
					<goals>
						<goal>test</goal>
					</goals>
				</execution>
			</executions>
		</plugin>
		<!-- abstaido a utilizacao do jacoco -->
	</plugins>
</build>
<!-- abstraido restante do pom -->
```

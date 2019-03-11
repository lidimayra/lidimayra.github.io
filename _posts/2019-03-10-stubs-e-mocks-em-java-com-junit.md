---
layout: post
title:  "Stubs e Mocks em Java com JUnit"
date:   2019-03-10 22:50:00 -0300
categories: jekyll update
---

De uns tempos pra cá, passei a trabalhar com alguns projetos Java. Da primeira
vez que precisei implementar testes que cobrissem métodos que dependiam
de requisições externas, fiquei um pouco perdida sobre qual seria a forma ideal
para trabalhar com mocks e stubs no JUnit. Por conta disso, consultei o livro
[Pragmatic Unit Testing in Java 8 with JUnit](https://pragprog.com/book/utj2/pragmatic-unit-testing-in-java-8-with-junit).
Este post reúne o que aprendi com ele.

Para fins de demonstração, usarei um exemplo que executa uma requisição para a
[Star Wars API](https://swapi.co/).

A partir do nome de um determinado planeta, o método
`getClimate()`, busca as informações referentes ao planeta através da API e
retorna o seu clima.

{% highlight java %}
public class Planet {

    public String getClimate(String name) throws IOException {
        String uri = "https://swapi.co/api/planets/?search=%s" + name;

        String response = new HttpImpl().get(uri);

        JSONArray jsonArray = new JSONObject(response).getJSONArray("results");
        String climate = jsonArray.getJSONObject(0).get("climate").toString();

        return climate;
    }
}

{% endhighlight %}

Criamos um objeto _HttpImpl_ que através do método `get()` se encarrega de
passar o endereço de uma URL e retorna as informações fornecidas
pela API. Em seguida, extrai do resultado obtido apenas o dado desejado (o clima
do planeta) e finalmente, o retorna.

Indo mais a fundo, segue o código na íntegra de _HttpImpl_:


{% highlight java %}
public class HttpImpl implements Http {
    public String get(String uri) throws IOException {

        CloseableHttpClient client = HttpClients.createDefault();
        HttpGet request = new HttpGet(uri);
        CloseableHttpResponse response = client.execute(request);

        try {
            HttpEntity entity = response.getEntity();
            String entityStr = EntityUtils.toString(entity);
            return entityStr;
        } finally {
            response.close();
        }

    }
}
{% endhighlight %}

Podemos perceber que _HttpImpl_ é uma implementação que performa uma requisição
GET para uma API qualquer e retorna o seu conteúdo.

Tendo conhecimento do código deste projeto e partindo do princípio que _HttpImpl_
já possui os testes unitários que cobrem o seu funcionamento, como poderíamos
escrever um teste automatizado para o método `getClimate()`?

Uma primeira tentativa:

{% highlight java %}
public class PlanetTest {

    @Test
    public void testGetClimate() throws IOException {
        Planet planet = new Planet();
        String climate = planet.getClimate("Tatooine");

        Assert.assertEquals("arid", climate);
    }
}
{% endhighlight %}

Aqui realizamos a chamada para `getClimate()` passando a string _Tatooine_ como
nome do planeta a ser buscado. Então, checamos se o resultado é igual à "arid",
pois é o valor contido em `climate` ao buscar pelo planeta Tatooine fazendo uso
da API.

**Problema:** Ao rodar a linha `String climate = planet.getClimate("Tatooine");`,
este teste realiza uma requisição real para a API. Esta prática é considerada
ruim por 2 motivos:
  - Realizar uma chamada HTTP faz com que a execução dos testes se torne mais
    lenta.
  - Não existe garantia de que a API sempre retorne o mesmo valor. Não queremos
que mudanças feitas na API levem o teste a falhar.

## Uso de _stubs_
Para evitar a requisição real para a API, especificaremos uma string _hardcoded_
nos testes. Desta forma, ao chamar o método `get()` no objeto _HttpImpl_, iremos
substituir o seu valor real pelo valor da string especificada. Este tipo de
implementação é chamada de _stub_.


{% highlight java %}
Http http = (String url) ->
    "{\"results\":" +
        "[" +
            "{" +
                "\"name\": \"Tatooine\"," +
                "\"climate\": \"arid\"" +
            "}" +
        "]" +
    "}";
{% endhighlight %}

O stub acima define uma string muito similar ao [retorno fornecido pela
API](https://swapi.co/api/planets/?search=Tatooine&format=json),
entretanto contém apenas os dados necessários neste contexto (nome do Planeta
e clima).

Tendo o stub definido, é necessário que a classe _Planet_ faça uso dele ao invés
da real implementação definida em _HttpImpl_ ao ser executada no ambiente de
testes.

Uma das formas de fazer isso é através da técnica de injeção de dependência.

Podemos injetar o objeto Http como um parâmetro de um método construtor em
_Planet_ e aplicá-lo a um atributo _http_.

{% highlight java %}
public class Planet {
    // Declara o atributo do tipo Http
    private Http http;

    // Cria um construtor que recebe http como um parâmetro e o aplica ao
    // atributo http
    public Planet(Http http) {
        this.http = http;
    }

    public String getClimate(String name) throws IOException {
        String uri = "https://swapi.co/api/planets/?search=%s" + name;

        // Substitui a criação da nova instância de HttpImpl pela chamada ao
        // atributo http
        String response = http.get(uri);

        JSONArray jsonArray = new JSONObject(response).getJSONArray("results");
        String climate = jsonArray.getJSONObject(0).get("climate").toString();

        return climate;
    }
}
{% endhighlight %}

Agora é possível utilizar a injeção de dependência nos testes passando o stub
como objeto Http:

{% highlight java %}
public class PlanetTest {

    @Test
    public void testGetClimate() throws IOException {
        // Cria o stub
        Http http = (String url) ->
            "{\"results\":" +
                "[" +
                    "{" +
                        "\"name\": \"Tatooine\"," +
                        "\"climate\": \"arid\"" +
                    "}" +
                "]" +
            "}";

        // Passa o stub no construtor de Planet
        Planet planet = new Planet(http);

        String climate = planet.getClimate("Tatooine");

        Assert.assertEquals("arid", climate);
    }
}
{% endhighlight %}

Pronto! Temos um teste automatizado que checa a nossa implementação sem performar
requisições externas. Entretanto, ainda pode ser melhorado. É importante ter em
mente que o funcionamento de `getClimate()` depende da passagem de um parâmetro
_name_, que é acoplado à query string enviada para a API. O nosso teste atual
não cobre esse funcionamento.

## Verificação de parâmetros passados para a URL

Podemos adicionar uma condição no início do teste que o força a falhar caso o
parâmetro passado para a URL não contenha a informação esperada:


{% highlight java %}
public class PlanetTest {

    @Test
    public void testGetClimate() throws IOException {
        Http http = (String url) -> {
            if(!url.contains("search=Tatooine"))
                Assert.fail(url + " does not contain expected param!");
            return "{\"results\":" +
                        "[" +
                            "{" +
                                "\"name\": \"Tatooine\"," +
                                "\"climate\": \"arid\"" +
                            "}" +
                        "]" +
                    "}";
        };

        Planet planet = new Planet(http);
        String climate = planet.getClimate("Tatooine");

        Assert.assertEquals("arid", climate);
    }
}
{% endhighlight %}

Nosso _stub_ se tornou um pouco mais parecido com uma implementação real do
elemento que ele substitui, pois verifica também os parâmetros passados.
Perceba, porém, que o teste se tornou bastante verboso. Em um projeto real,
diversas implementações similares a essa sendo implementadas manualmente
apresentam um esforço de manutenção considerável.

Para facilitar este cenário, podemos utilizar uma ferramenta chamada
[Mockito](https://site.mockito.org/).

## Mockito

Mockito é um framework de testes unitários utilizado para a criação de
**mocks**.

Um mock pode ser considerado a evolução de um _stub_. É um objeto que simula o
comportamento de um determinado elemento de forma controlada. É possível
definir o retorno que queremos que um mock produza em certas condições.

{% highlight java %}
public class PlanetTest {

    @Test
    public void run() throws IOException {
        // Cria um objeto que simula uma instância de Http
        Http http = Mockito.mock(Http.class);

        // Define a resposta a ser providenciada quando o nosso mock http
        // receber uma chamada para get() contendo o parâmetro "search=Tatooine"
        Mockito.when(http.get(contains("search=Tatooine"))).thenReturn(
                "{\"results\":" +
                    "[" +
                        "{" +
                            "\"name\": \"Tatooine\"," +
                            "\"climate\": \"arid\"" +
                        "}" +
                    "]" +
                "}"
        );

        Planet planet = new Planet(http);
        String climate = planet.getClimate("Tatooine");

        Assert.assertEquals("arid", climate);
    }
}
{% endhighlight %}

## Ferramentas de injeção de dependência do Mockito
O framework Mockito fornece algumas ferramentas de injeção de dependência
interessantes que permitem refatorar os testes, de forma a torná-los mais
legíveis. Facilita também a  verificação de erros, dado que o mock é
automaticamente identificado pelo nome do atributo.

{% highlight java %}
public class PlanetTest {
    // Cria um mock de um objeto Http
    @Mock private Http http;

    // Injeta o mock acima em uma instância de Planet
    @InjectMocks private Planet planet;

    // O uso da anotação @Before faz com que setupPlanet() seja executado antes
    // de qualquer teste dentro de PlanetTest
    @Before
    public void setupPlanet() {
        // Inicializa todos os atributos que contenham Mockito Annotations
        // Neste exemplo: @Mock e @InjectMocks
        MockitoAnnotations.initMocks(this);
    }

    @Test
    public void testGetClimate() throws IOException {
        Mockito.when(http.get(contains("search=Tatooine"))).thenReturn(
                "{\"results\":" +
                    "[" +
                        "{" +
                            "\"name\": \"Tatooine\"," +
                            "\"climate\": \"arid\"" +
                        "}" +
                    "]" +
                "}"
        );

        String climate = planet.getClimate("Tatooine");

        Assert.assertEquals("arid", climate);
    }
}
{% endhighlight %}

Este refactor também permite que o construtor de _Planet_ seja removido (lembre
que este construtor nunca foi necessário para o funcionamento de Planet, havia
sido adicionado exclusivamente por conta dos testes):


{% highlight java %}
public class Planet {
    private Http http = new HttpImpl();

    public String getClimate(String name) throws IOException {
        String uri = String.format("https://swapi.co/api/planets/?search=%s",
name);

        String response = http.get(uri);

        JSONArray jsonArray = new JSONObject(response).getJSONArray("results");
        String climate = jsonArray.getJSONObject(0).get("climate").toString();

        return climate;
    }
}
{% endhighlight %}

Como o mocking agora está sendo feito no nível dos atributos, o construtor não é
mais necessário.

## Para ver mais
- Todos os exemplos utilizados neste post estão contidos [neste
  repositório](https://github.com/lidimayra/junit-samples).
- A base para estes exemplos foi retirada do livro [Pragmatic Unit Testing in Java 8 with JUnit](https://pragprog.com/book/utj2/pragmatic-unit-testing-in-java-8-with-junit).

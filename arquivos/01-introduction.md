![Jasmine Br](assets/img/jasmine-logo.png)

**Tradução da documentação do framework de testes JavaScript Jasmine, versão 2.0.0.**

## Introdução.js

Jasmine é um framework BDD (behavior-driven development | desenvolvimento orientado a comportamento) para testar código JavaScript. Ele não depende de nenhum outro framework JavaScript. Também não requer DOM, além de ter uma sintaxe limpa e óbvia, então você pode facilmente escrever testes. [Este guia](http://jasmine.github.io/2.0/introduction.html) funciona com a versão 2.0.0 do Jasmine.

<pre>
<code>
describe("Um conjunto", function () {
	it("contém uma especulação com uma expectativa", function () {
		expect(true).toBe(true);
	});
});
</code>
</pre>

## Distribuição Independente

A [distribuição independente](https://github.com/pivotal/jasmine/tree/master/dist) contém tudo que você precisa para rodar o Jasmine. Abrir `SpecRunner.html` irá rodar as especulações incluídas (`specs`, de agora em diante). Você irá notar que ambos os arquivos fonte e suas respectivas *specs* serão linkadas no `head` de `SpecRunner.html`. Para começar a usar o Jasmine, troque os arquivos em `source/spec` pelos seus próprios arquivos.

## Conjuntos (suites): `describe` seus testes

Um conjunto de testes começa com a chamada da função global do Jasmine `describe` com dois parâmetros: uma string e uma função. A string é o nome ou título para um conjunto de especulações - geralmente o que está sendo testado. A função é um bloco de código que implementa o conjunto.

## Specs (Especulações)

*Especulações* são definidas chamando a função global do Jasmine `it`, que, como `describe`, pega uma string e uma função. A string é o título da especulação e a função é a especulação, ou o teste. Uma especulação contém uma ou mais expectativas que testam o estado do código. Uma expectativa para o Jasmine é uma asserção, uma afirmação que é ou verdadeira ou falsa. Uma especulação com todas as expectativas verdadeiras é uma especulação aprovada. Uma especulação com uma ou mais expectativas falsas, é uma especulação falha.

### São apenas funções

Sendo os blocos `describe` e `it` funções, eles podem conter qualquer código executável necessário para implementar o teste. As [regras de escopo](https://github.com/eoop/traduz-ai/blob/master/javascript/003-escopo-de-variavel-js-e-hoisting-explicado.md#escopo-de-vari%C3%A1vel-javascript-e-hoisting-explicado) do JavaScript são aplicadas, então variáveis declaradas em um `describe` estão disponíveis para qualquer bloco `it` dentro do conjunto.

<pre>
<code>
describe("Um conjunto é apenas uma função", function () {
	var a;

	it("e isto é uma especulação", function () {
		a = true;

		expect(a).toBe(true);
	});
});
</code>
</pre>

## Expectativas

Expectativas são contruídas com a função `expect` que assume um valor, chamado de atual. Ele é ligado a uma função comparadora, que recebe o valor esperado.

## Matchers (comparador)

Cada *matcher* (comparador) implementa uma comparação booleana entre o atual valor e o valor esperado. Ele é responsável por reportar ao Jasmine se a expectativa é verdadeira ou falsa. O Jasmine então vai aprovar ou reprovar a especulação.

Qualquer comparador pode avaliar uma asserção negativa encadeando a chamada `expect` com um `not`antes da chamada do comparador.

<pre>
<code>
describe("O comparador 'toBe' compara com ===", function () {
	
	it("e tem um caso positivo", function () {
		expect(true).toBe(true);
	});

	it("e também um caso negativo", function () {
		expect(false).not.toBe(true);
	});
});
</code>
</pre>

## Incluindo Comparadores

O Jasmine tem um conjunto rico de comparador incluído. Há também a possibilidade de escrever [comparadores personalizados](custom-matcher.html), para quando as afirmações específicas necessárias para as chamadas de seu projeto não estiverem inclusas abaixo.

<pre>
<code>
describe("Comparadores inclusos", function () {
	
	it("O comparador 'toBe' compara com ===", function () {
		var a = 12;
		var b = a;

		expect(a).toBe(b);
		expect(a).not.toBe(null);
	});

	describe("O comparador 'toEqual' ", function () {

		it("trabalha com simples literais e variáveis", function () {
			var a = 12;
			expect(a).toEqual(12);
		});

		it("pode trabalhar com objetos", function () {
			var foo = {
				a: 12,
				b: 34
			};

			var bar = {
				a: 12,
				b: 34
			};
			expect(foo).toEqual(bar);
		});
	});

	it("O comparador 'toMatch' é para expressões regulares", function () {
		var message = "foo bar baz";

		expect(message).toMatch(/bar/);
		expect(message).toMatch("bar");
		expect(message).not.toMatch(/quux/);
	});

	it("O comparador 'toBeDefined' compara com 'undefined'", function () {
		var a = {
			foo: "foo"
		};

		expect(a.foo).toBeDefined();
		expect(a.bar).not.toBeDefined();
	});

	it("O comparador 'toBeUndefined' compara com 'undefined'", function () {
		var a = {
			foo: "foo"
		};

		expect(a.foo).not.toUndefined();
		expect(a.bar).toBeUndefined();
	});

	it("O comparador 'toBeNull' compara com null", function () {
		var a = null;
		var foo = "foo";

		expect(null).toBeNull();
		expect(a).toBeNull();
		expect(foo).not.toBeNull();
	});

	it("O comparador 'toBeTruthy' é para testar o operador booleano", function () {
		var a, foo = "foo";

		expect(foo).toBeTruthy();
		expect(a).not.ToBeTruthy();
	});

	it("O comparador 'toBeFalsy' é para testar o operador booleano", function () {
		var a, foo = "foo";

		expect(a).toBeFalsy();
		expect(foo).not.toBeFalsy();
	});

	it("O comparador 'toContain' é para encontrar um item em um Array", function () {
		var a = ["foo", "bar", "baz"];

		expect(a).toContain("bar");
		expect(a).not.toContain("quux");
	});

	it("O comparador 'toBeLessThan' é para comparações matemáticas", function () {
		var pi = 3.1415926,
			e  = 2.78;

		expect(e).toBeLessThan(pi);
		expect(pi).not.toBeLessThan(e);
	});

	it("O comparador 'toBeGreaterThan' é para comparações matemáticas", function () {
		var pi = 3.1415926,
			e  = 2.78;

		expect(pi).toBeGreaterThan(e);
		expect(e).not.toBeGreaterThan(pi);
	});

	it("O comparador 'toBeCloseTo' é para comparações matemáticas precisas", function () {
		var pi = 3.1415926,
			e  = 2.78;

		expected(pi).not.toBeCloseTo(e, 2);
		expected(pi).toBeCloseTo(e, 0);
	});

	it("O comparador 'toThrow' é para testar se uma função lançou uma exceção", function () {
		var foo = function () {
			return 1 + 2;
		};
		var bar = function () {
			a + 1;
		};

		expect(foo).not.toThrow();
		expect(bar).toThrow();
	});
});
</code>
</pre>

## Agrupamento de Especulações Relacionadas com `describe`

A função `describe` é para o agrupamento de especulações relacionadas. O parâmetro string é para nomear a coleção de especulações, e vai ser concatenado com estas para fazer o nome completo da especulação. Isso ajuda a encontrar especulações em um grande conjunto. Se você as nomear bem, suas especulações serão lidas como sentenças tradicionais do estilo [BDD](http://en.wikipedia.org/wiki/Behavior-driven_development).

<pre>
<code>
describe("Uma especulação", function () {
	it("é apenas uma função, então ela pode conter qualquer código", function () {
		var foo = 0;
		foo += 1;

		expect(foo).toEqual(1);
	});

	it("pode ter mais que uma expectativa", function () {
		var foo = 0;
		foo += 1;

		expect(foo).toEqual(1);
		expect(true).toEqual(true);
	});
});
</code>
</pre>

### Configuração e Subdivisão

Para ajudar a manter seu conjunto de testes nos padrões DRY (Don't Repeat Yourself - Não se repita), e então não duplicar seus códigos de configuração e subdivisão, o Jasmine fornece as funções globais `beforeEach` e `afterEach`. Como o nome sugere, a função `beforeEach` é chamada uma vez antes de cada especulação quando `describe`é rodado, e a função `afterEach` é chamada depois de cada especulação. Aqui temos o mesmo conjunto de especulações escritas um pouco diferente. A variável em teste é definida no escopo de nível superior - o bloco `describe` - e o código de inicialização é movido para dentro da função `beforeEach`. A função `afterEach` reinicia a variável antes de continuar.

<pre>
<code>
describe("Uma especulação (com configuração e subdivisão)", function () {
	var foo;

	beforeEach(function () {
		foo = 0;
		foo += 1;
	});

	afterEach(function () {
		foo = 0;
	});

	it("é somente uma função, então pode conter qualquer código", function () {
		expect(foo).toEqual(1);
	});

	it("pode ter mais de uma expectativa", function () {
		expect(too).toEqual(1);
		expect(true).toEqual(true);
	});
});
</code>
</pre>

### Aninhando Blocos `describe`

Chamadas a função `describe` podem ser aninhadas, com especulações definidas em cada nível. Isto permite a composição de conjuntos como árvores de funções. Antes de uma especulação ser executada, o Jasmine percorre a árvore executando cada função `beforeEach` na ordem. Depois que a especulação é executada, o Jasmine percore pelas funções `afterEach` similarmente.

<pre>
<code>
describe("Uma especulação", function () {
	var foo;

	beforeEach(function () {
		foo = 0;
		foo += 1;
	});

	afterEach(function () {
		foo = 0;
	});

	it("é apenas uma função, então pode conter qualquer código", function () {
		expect(foo).toEqual(1);
	});

	it("pode ter mais de uma expectativa", function () {
		expect(foo).toEqual(1);
		expect(true).toEqual(true);
	});

	describe("aninhando um segundo describe", function () {
		var bar;

		beforeEach(function () {
			bar = 1;
		});

		it("se pode referenciar a ambos os escopos se necessário", function () {
			expect(foo).toEqual(bar);
		});
	});
});
</code>
</pre>

## Desabilitando Conjuntos

Conjuntos e especulações podem ser desabilitados com as funções `xdescribe` e `xit`, respectivamente. Estes conjuntos e quaisquer especulações dentro deles serão puladas quando rodar a verificação e seus resultados também não irão aparecer.

<pre>
<code>
xdescribe("Uma especulação", function () {
	var foo;

	beforeEach(function () {
		foo = 0;
		foo += 1;
	});

	it("é apenas uma função, então pode conter qualquer código", function () {
		expect(foo).toEqual(1);
	});
});
</code>
</pre>

## Especulações Pendentes

Especulações Pendentes não rodam, mas seus nomes vão aparecer nos resultados como `pending`.

Qualquer especulação declarada com `xit` é marcada como pendente.

Qualquer especulação declarada sem um corpo de função também vai ser marcada como resultado pendente.

E se você chamar a função `pending` em qualquer lugar do corpo da especulação, não importa as expectativas, a especulação vai ser marcada como pendente.

<pre>
<code>
describe("Especulações pendentes", function () {
	
	xit("podem ser declaradas 'xit'", function () {
		expect(true).toBe(false);
	});

	it("pode ser declarado com 'it' mas sem uma função");

	it("pode ser declarado chamando 'pending' no corpo da especulação", function () {
		expect(true).toBe(false);
		pending();
	});
});
</code>
</pre>

## Spies (espiões)

Jasmine tem duas funções de teste chamadas 'spies' (espiões). Um *spy* (espião) pode rastrear qualquer função e chamadas a ela seus argumentos. Há comparadores especiais para interagir com os spies. **Esta sintaxe foi alterada para o Jasmine 2.0**. O comparador `toHaveBeenCalled` vai retornar `true` se o spy tiver sido chamado. O comparador `toHaveBeenCalledWith` vai retornar `true` se a lista de argumentos encontrar qualquer registro chamado com o spy.

<pre>
<code>
describe("Um spy (espião)", function () {
	var foo, bar = null;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			}
		};

		spyOn(foo, 'setBar');

		foo.setBar(123);
		foo.setBar(456, 'another param');
	});

	it("rastreia se o spy foi chamado", function () {
		expect(foo.setBar).toHaveBeenCalled();
	});

	it("rastreia todos os argumentos de chamadas", function () {
		expect(foo.setBar).toHaveBeenCalledWith(123);
		expect(foo.setBar).toHaveBeenCalledWith(456, 'another param');
	});

	it("para toda a execução em uma função", function () {
		expect(bar).toBeNull();
	});
});
</code>
</pre>

### Spies: `and.callThrough`

Encadeando o spy com `and.callThrough`, o spy vai continuar rastreando todas as chamadas a ele mas em adição, ele vai delegar a implementação atual.

<pre>
<code>
describe("Um spy, quando configurado para 'call through' (chamar através)", function () {
	var foo, bar, fetchedBar;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			},
			getBar: function () {
				return bar;
			}
		};

		spyOn(foo, 'getBar').and.callThrough();

		foo.setBar(123);
		fetchedBar = foo.getBar();
	});

	it("rastreia se o spy foi chamado", function () {
		expect(foo.getBar).toHaveBeenCalled();
	});

	it("não deve afetar outras funções", function () {
		expect(bar).toEqual(123);
	});

	it("quando chamado retorna o valor requisitado", function () {
		expect(fetchedBar).toEqual(123);
	});
});
</code>
</pre>

### Spies: `and.returnValue`

Encadeando o spy com `and.returnValue`, todas as chamadas à função vão retornar um valor específico.

<pre>
<code>
describe("Um spy, quando configurado para 'falsificar' um valor de retorno", function () {
	var foo, bar, fetchedBar;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			},
			getBar: function () {
				return bar;
			}
		};

		spyOn(foo, "getBar").and.returnValue(745);

		foo.setBar(123);
		fetchedBar = foo.getBar();
	});

	it("rastreia se o spy foi chamado", functio () {
		expect(foo.getBar).toHaveBeenCalled();
	});

	it("não deve afetar outras funções", function () {
		expect(bar).toEqual(123);
	});

	it("quando chamado retorna o valor requisitado", function () {
		expect(fetchedBar).toEqual(745);
	})
});
</code>
</pre>

### Spies: `and.callFake`

Encadeando o spy com `and.callFake`, todas as chamadas ao spy vão ser delegadas a função fornecida.

<pre>
<code>
describe("Um spy, quando configurado com uma implementação alternativa", function () {
	var foo, bar, fetchedBar;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			},
			getBar: function () {
				return bar;
			}
		};

		spyOn(foo, "getBar").and.callFake(function () {
			return 1001;
		});

		foo.setBar(123);
		fetchedBar = foo.getBar();
	});

	it("rastreia se o spy foi chamado", function () {
		expect(foo.getBar).toHaveBeenCalled();
	});

	it("não deve afetar outras funções", function () {
		expect(bar).toEqual(123);
	});

	it("quando chamado retorna o valor requisitado", function () {
		expect(fetchedBar).toEqual(1001);
	});
});
</code>
</pre>

### Spies: `and.throwError`

Encadeando o spy com `and.throwError`, todas as chamadas ao spy vão `throw` (lançar) o valor especificado como um erro.

<pre>
<code>
describe("Um spy, quando configurado para lançar um erro", function () {
	var foo, bar;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			}
		};

		spyOn(foo, "setBar").and.throwError("quux");
	});

	it("lança o valor", function () {
		expect(function () {
			foo.setBar(123)
		}).toThrowError("quux");
	});
});
</code>
</pre>

### Spies: `and.stub`

Quando uma chamada estratégica é usada por um spy, o comportamento original *stubbing* pode ser retornado a qualquer momento com `and.stub`.

<pre>
<code>
describe("Um spy," function () {
	var foo, bar = null;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			}
		};

		spyOn(foo, 'setBar')and.callThrough();
	});

	it("pode chamar através e então *stub* na mesma especulação", function () {
		foo.setBar(123);
		expect(bar).toEqual(123);

		foo.setBar.and.stub();
		bar = null;

		foo.setBar(123);
		expect(bar).toBe(null);
	});
});
</code>
</pre>

## Outras Propriedades de Rastreamento

Toda chamada a um spy é rastreada e exposta em uma propriedade `calls`.

<pre>
<code>
describe("Um spy", function () {
	var foo, bar = null;

	beforeEach(function () {
		foo = {
			setBar: function (value) {
				bar = value;
			}
		};

		spyOn(foo, 'setBar');
	});
}); // esse describe fecha originalmente após
	// a demonstração de todos métodos .calls, 
	// mais exatamente no .calls.reset
</code>
</pre>

`.calls.any()`: retorna `false` se o spy não tiver sido chamado ainda, e `true` quando a chamada acontece pelo menos uma vez.

<pre>
<code>
it("rastreia se foi chamado alguma vez", function () {
	expect(foo.setBar.calls.any()).toEqual(false);

	foo.setBar();

	expect(foo.setBar.calls.any()).toEqual(true);
});
</code>
</pre>

`.calls.count()`: retorna o número de vezes que o spy foi chamado.

<pre>
<code>
it("rastreia o número de vezes que isso foi chamado", function () {
	expect(foo.setBar.calls.count()).toEqual(0);

	foo.setBar();
	foo.setBar();

	expect(foo.setBar.calls.count()).toEqual(2);
});
</code>
</pre>

`.calls.argsFor(index)`: retorna os argumentos passados chamando pelo seu `index`.

<pre>
<code>
it("rastreia os argumentos de cada chamada", function () {
	foo.setBar(123);
	foo.setBar(456, "baz");

	expect(foo.setBar.calls.argsFor(0)).toEqual([123]);
	expect(foo.setBar.calls.argsFor(1)).toEqual([456, "baz"]);
});
</code>
</pre>

`.calls.allArgs()`: retorna os argumentos de todas as chamadas.

<pre>
<code>
it("rastreia os argumentos de todas as chamadas", function () {
	foo.setBar(123);
	foo.setBar(456, "baz");

	expect(foo.setBar.calls.allArgs()).toEqual([123],[456, "baz"]);
});
</code>
</pre>

`.calls.all()`: retorna o contexto (o `this`) e os argumentos passados em todas chamadas.

<pre>
<code>
it("pode fornecer o contexto e argumentos para todas chamadas", function () {
	foo.setBar(123);

	expect(foo.setBar.calls.all()).toEqual([{object: foo, args: [123]}]);
});
</code>
</pre>

`.calls.mosRecent()`: retorna o contexto (o `this`) e os argumentos para a chamada mais recente.

<pre>
<code>
it("um atalho para a chamada mais recente", function () {
	foo.setBar(123);
	foo.setBar(456, "baz");

	expect(foo.setBar.calls.mostRecent()).toEqual({object: foo, args: [456, "baz"]});
});
</code>
</pre>

`.calls.first()`: retorna o contexto (o `this`) e os argumentos da primeira chamada.

<pre>
<code>
it("atalho para a primeira chamada", function () {
	foo.setBar(123);
	foo.setBar(456, "baz");

	expect(foo.setBar.calls.first()).toEqual({object: foo, args: [123]});
});
</code>
</pre>

`.calls.reset()`: limpa todo o rastreio do spy.

<pre>
<code>
it("pode ser resetado", function () {
	foo.setBar(123);
	foo.setBar(456, "baz");

	expect(foo.setBar.calls.any()).toBe(true);

	foo.setBar.calls.reset();

	expect(foo.setBar.calls.any()).toBe(false);
});
</code>
</pre>

### Spies: `createSpy`

Quando não existe uma função para ser especionada, podemos usar `jasmine.createSpy` para criar um spy. Este spy atua como qualquer outro spy - rastreando chamadas, argumentos e etc. Mas não há uma implementação por trás dele. Spies são objetos JavaScript e pode sem usados desta forma.

<pre>
<code>
describe("Um spy, quando criado manualmente", function () {
	var whatAmI;

	beforeEach(function () {
		whatAmI = jasmine.createSpy('whatAmI');

		whatAmI("I", "am", "a", "spy");
	});

	it("é nomeado, e ajuda no relatório de erro", function () {
		expect(whatAmI.and.identify()).toEqual('whatAmI');
	});

	it("rastreia se o spy foi chamado", function () {
		expect(whatAmI).toHaveBeenCalled();
	});

	it("rastreia o número de chamadas", function () {
		expect(whatAmI.calls.count()).toEqual(1);
	});

	it("rastreia todos os argumentos desta chamada", function () {
		expect(whatAmI).toHaveBeenCalledWith("I", "am", "a", "spy");
	});

	it("permite o acesso a chamada mais recente", function () {
		expect(whatAmI.calls.mostRecent().args[0].toEqual("I"));
	});
});
</code>
</pre>

### Spies: `createSpyObj`

A fim de criar uma simulação com múltiplos spies, use `jasmine.createSpyObj` e passe um aray de strings. Isso retorna um objeto que tem uma propriedade para cada string que é um spy.

<pre>
<code>
describe("Múltiplos spies, quando criados manualmente", function () {
	var tape;

	beforeEach(function () {
		tape = jasmine.createSpyObj('tape', ['play', 'pause', 'stop', 'rewind']);

		tape.play();
		tape.pause();
		tape.rewind();
	});

	it("cria spies para cada função requisitada", function () {
		expect(tape.play).toBeDefined();
		expect(tape.pause).toBeDefined();
		expect(tape.stop).toBeDefined();
		expect(tape.rewind).toBeDefined();
	});

	it("rastreia se os spies foram chamados", function () {
		expect(tape.play).toHaveBeenCalled();
		expect(tape.pause).toHaveBeenCalled();
		expect(tape.rewind).toHaveBeenCalled();
		expect(tape.stop).not.toHaveBeenCalled();
	});

	it("rastreia todos os argumentos desta chamada", function () {
		expect(tape.rewind).toHaveBeenCalledWith(0);
	});
});
</code>
</pre>

## Correspondendo tudo com `jasmine.any`

`jasmine.any` pega um nome de construtor ou de uma "classe" como um valor experado. Ele retorna `true` se o construtor encontra o construtor do valor real.

<pre>
<code>
describe("jasmine.any", function () {
	it("encontra qualquer valor", function () {
		expect({}).toEqual(jasmine.any(Object));
		expect(12).toEqual(jasmine.any(Number));
	});

	describe("quando usado com um spy", function () {
		it("é útil para comparar argumentos", function () {
			var foo = jasmine.createSpy('foo');
			foo(12, function () {
				return true;
			});

			expect(foo).toHaveBeenCalledWidth(jasmine.any(Number), jasmine.any(function));
		});
	});
});
</code>
</pre>

## Combinação parcial com `jasmine.objectContaining`

`jasmine.objectContaining` é para quando uma expectativa somente importa sobre certos pares chave/valor serem verdadeiros.

<pre>
<code>
describe("jasmine.objectContaining", function () {
	var foo;

	beforeEach(function () {
		foo = {
			a: 1,
			b: 2,
			bar: "baz"
		};
	});

	it("encontra objetos com os pares chave/valor experado", function () {
		expect(foo).toEqual(jasmine.objectContaining({
			bar: "baz"
		}));
		expect(foo).not.toEqual(jasmine.objectContaining({
			c: 37
		}));
	});

	describe("quando usado com um spy", function () {
		it("é útil para comparar argumentos", function () {
			var callback = jasmine.createSpy('callback');

			callback({
				bar: "baz"
			});

			expect(callback).toHaveBeenCalledWith(jasmine.objectContaining({
				bar: "baz"
			}));
			expect(callback).not.toHaveBeenCalledWith(jasmine.objectContaining({
				c: 37
			}));
		});
	});
});
</code>
</pre>

## Simulando Funções Timeout do JavaScript

**Esta sintaxe mudou no Jasmine 2.0**. O *Jasmine Clock* está disponível para um conjunto de testes que precisam da habilidade de usar callbacks `setTimeout` ou `setInterval`. Isto torna os callbacks síncronos, executando as funções registradas somente se o tempo estiver passando. Isto torna o código relacionado ao temporizador muito mais fácil de ser testado.

Ele é instalado com a chamada `jasmine.clock().install` em uma especulação ou conjunto que precisa de chamar funções temporizadoras.

Assegure-se de desinstalar o clock depois que você tiver feito terminado para restaurar o temporizador original das funções.

Chamadas para qualquer callback registrado é disparada quando o relógio é iniciado por via da função `jasmine.clock().tick`, que pega o número em milisegundos.

<pre>
<code>
describe("Iniciando manualmente o relógio do Jasmine", function () {
	var timeCallback;

	beforeEach(function () {
		timerCallback = jasmine.createSpy("timerCallback");
		jasmine.clock().install();
	});

	afterEach(function () {
		jasmine.clock().uninstall();
	});

	it("faz com que o timeout seja chamado síncronamente", function () {
		setTimeout(function () {
			timerCallback();
		}, 100);

		expect(timerCallback).not.toHaveBeenCalled();

		jasmine.clock().tick(101);

		expect(timerCallback).toHaveBeenCalled();
	});

	it("faz com que um intervalo seja chamado síncronamente", function () {
		setInterval(function () {
			timerCallback();
		}, 100);

		expect(timerCallback).not.toHaveBeenCalled();

		jasmine.clock().tick(101);
		expect(timerCallback.calls.count()).toEqual(1);

		jasmine.clock().tick(50);
		expect(timerCallback.calls.count()).toEqual(1);

		jasmine.clock().tick(50);
		expect(timerCallback.calls.count()).toEqual(2);
	});
});
</code>
</pre>

## Suporte Assíncrono

**Esta sintaxe mudou no Jasmine 2.0**: O Jasmine também suporta rodar especulações que necessitam de operações de testes assíncronos.

Chamadas à `beforeEach`, `it` e `afterEach` podem ter um simples argumento opcional que deve ser chamado quando o trabalho assíncrono está completo.

Essa especulação não vai iniciar até que a função `done` seja chamada pela chamada `beforeEach`. E esta especulação não vai estar completa até que `done` seja chamado.

<pre>
<code>
describe("Especulações Assíncronas", function () {
	var value;

	beforeEach(function () {
		setTimeout(function () {
			value = 0;
			done();
		}, 1);
	});

	it("deve suportar a execução assíncrona dos testes - preparação e expectativas", function (done) {
		value++;
		expect(value).toBeGreaterThan(0);
		done();
	});
});
</code>
</pre>

## Downloads

* O [lançamento independente](https://github.com/pivotal/jasmine/tree/master/dist) é para página do navegador, ou console de projetos.
* O [Jasmine Ruby Gem](http://github.com/pivotal/jasmine-gem) é para Rails, Ruby, ou desenvolvimento amigável ao Ruby.
* [Outros ambientes](http://github.com/pivotal/jasmine/wiki) também são suportados.

## Suporte

* [Lista de Email](http://groups.google.com/group/jasmine-js) no Google Groups - uma grande primeira parada para fazer perguntas, propor coisas, ou discutir *pull requests*
* [Relate questões](http://github.com/pivotal/jasmine/issues) no Github.
* O [Backlog](http://www.pivotaltracker.com/projects/10606) no [Pivotal Tracker](http://www.pivotaltracker.com/)
* Siga [@JasmineBDD](http://twitter.com/jasminebdd) no Twitter

## Obrigado

*Documentação inspirada por [@mjackson](http://twitter.com/mjackson) na Fluent Summit em 2012*.
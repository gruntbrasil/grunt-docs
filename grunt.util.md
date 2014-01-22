Diversos utilitários para seu Gruntfile e suas tarefas.

### grunt.util.kindOf
Retorna o "tipo" do valor. Como o `typeof` mas retorna o valor interno da `[[Classe]]`. Resultados possíveis são `"number"`, `"string"`, `"boolean"`, `"function"`, `"regexp"`, `"array"`, `"date"`, `"error"`, `"null"`, `"undefined"` e o vasto `"object"`.

```js
grunt.util.kindOf(value)
```

### grunt.util.error
Retorna uma nova instância Error (que pode ser lançada) com a mensagem adequada. Se um objeto Error for especificado em vez de uma `message`, este objeto será retornado.
Além disso, se um objeto Error for especificado para `origError` e o Grunt tiver sido executado com a opção `--debug 9`, o stack original Error vai ser retornado.

```js
grunt.util.error(message [, origError])
```

### grunt.util.linefeed
O caractere para avanço de linha, normalizado para o padrão atual do sistema operacional. (`\r\n` no Windows, `\n` em outros sistemas)

### grunt.util.normalizelf
Fornecido uma string, retorna uma nova string com todos os avanços de linhas normalizados para o padrão atual do sistema operacional. (`\r\n` no Windows, `\n` em outros sistemas)

```js
grunt.util.normalizelf(string)
```

### grunt.util.recurse
Percorre recursivamente objetos e arrays, executando `callbackFunction` para cada valor que não for um objeto. Se `continueFunction` retornar `false`, o objeto ou valor fornecido será ignorado.

```js
grunt.util.recurse(object, callbackFunction, continueFunction)
```

### grunt.util.repeat
Retorna a string `str` repetida `n` vezes.

```js
grunt.util.repeat(n, str)
```

### grunt.util.pluralize
Recebido `str` de `"a/b"`, Se `n` for `1`, retorna `"a"` no lugar de `"b"`. Você pode especificar um separador customizado se '/' não funcionar para você.

```js
grunt.util.pluralize(n, str, separator)
```

### grunt.util.spawn
Gera um processo filho, mantendo o controle dos seus stdout, stderr e códigos de saída. O método retorna uma referência ao filho gerado. Quando o filho existir, a função `doneFunction` é chamada.

```js
grunt.util.spawn(options, doneFunction)
```

O objeto `options` possui as possíveis propriedades:

```js
var options = {
  // O comando a ser executado. Ele deve ser um caminho do sistema.
  cmd: commandToExecute,
  // Se especificado, o mesmo grunt bin que está em execução será gerado 
  // como um comando filho, no lugar da opção "cmd". O padrão é false.
  grunt: boolean,
  // Um array de argumentos para passar ao comando.
  args: arrayOfArguments,
  // Opções adicionais para o processo de criação do child_process Node.js.
  opts: nodeSpawnOptions,
  // Se o valor for definido e um erro ocorrer, ele vai ser utilizado como valor 
  // e null será passado como o valor do erro.
  fallback: fallbackValue
};
```

A função `doneFunction` aceita os possíveis argumentos:

```js
function doneFunction(error, result, code) {

  // Se o código de saída foi diferente de zero e um retorno não foi especificado, um objeto de erro,
  // caso contrário, nulo.
  error
  // O objeto resultante é um objeto com as propriedades .stdout, .stderr,
  // e .code (código de saída).
  result
  // Quando resultado é forçada para uma string, o valor é stdout se o código de saída for zero, 
  // fallback se o código de saída foi diferente de zero e um fallback foi especificado, ou stderr 
  // se o código de saída foi diferente de zero e um fallback for especificado.
  String(result)
  // O código de saída numérico.
  code
}
```

### grunt.util.toArray
Dado um array ou objeto array-like, retorna um array. Ótimo para converter objetos `arguments` para arrays.

```js
grunt.util.toArray(arrayLikeObject)
```

### grunt.util.callbackify
Normaliza ambas as funções que "retorna um valor" e "passa o resultado para um callback" para sempre passar o resultado a um callback especificado. Se a função original retornar um valor, esse valor será agora passado ao callback, depois de todos os argumentos predefinidos. Se a função original passou um valor para o callback, ela continuará a fazê-lo.

```js
grunt.util.callbackify(syncOrAsyncFunction)
```

Este exemplo pode ilustar melhor:

```js
function add1(a, b) {
  return a + b;
}
function add2(a, b, callback) {
  callback(a + b);
}

var fn1 = grunt.util.callbackify(add1);
var fn2 = grunt.util.callbackify(add2);

fn1(1, 2, function(result) {
  console.log('1 plus 2 equals ' + result);
});
fn2(1, 2, function(result) {
  console.log('1 plus 2 equals ' + result);
});
```

## Bibliotecas internas

### grunt.util.namespace
Uma biblioteca interna para resolver propriedades profundamente aninhadas em objetos.

### grunt.util.task
Uma biblioteca interna para tarefas em execução.

## Bibliotecas externas
*Obsoleto*

__Todas as bibliotecas externas que estão listadas abaixo estão obsoletas.__

Por favor, use o __npm__ para gerenciar estas bibliotecas externas nas dependências do seu projeto.

Por exemplo, se você quiser usar a biblioteca [Lo-Dash](https://npmjs.org/package/lodash), instale-a primeiro `npm install lodash`, 
então use no seu `Gruntfile`: `var _ = require('lodash');`

#### grunt.util._
*Obsoleto*

[Lo-Dash](http://lodash.com/) e [Underscore.string](https://github.com/epeli/underscore.string)

`grunt.util._.str` está disponível para os métodos que estão em conflito com os métodos Lo-Dash existentes.

#### grunt.util.async
*Obsoleto*

[Async](https://github.com/caolan/async) - Utilitários Async para o node e browser.

#### grunt.util.hooker
*Obsoleto*

[JavaScript Hooker](https://github.com/cowboy/javascript-hooker) - Fuções Monkey-patch (hook) para depuração e outras coisas.

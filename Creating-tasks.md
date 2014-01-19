As tarefas são o pão com manteiga do Grunt. As coisas que você mais faz, como `jshint` ou `nodeunit`. Toda vez que o Grunt é executado, você especifica uma ou mais tarefas a serem executadas, que diz ao Grunt o que você gostaria de fazer.

Se você não especificar uma tarefa, mas a tarefa nomeada "default" foi definida, esta tarefa vai ser executada (obviamente) por padrão.

## Atalhos de Tarefas
Se uma lista de tarefas foi especificada, a nova tarefa vai ser um atalho para uma ou mais tarefas. Sempre que este "atalho de tarefa" é executado, todas as tarefas especificadas na `taskList` vão ser executadas, em suas respectivas ordens. O argumento `taskList` deve ser um array de tarefas.

```javascript
grunt.registerTask(taskName, [description, ] taskList)
```

Este exemplo de atalho de uma tarefa define uma tarefa "default" por onde as tarefas "jshint", "qunit", "concat" e "uglify" serão sempre executadas se o Grunt é executado sem nenhuma tarefa especificada.

```javascript
grunt.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify']);
```

Os argumentos das tarefas também podem ser especificadas. Neste exemplo, o atalho "dist" executa tanto a tarefa "concat" como a "uglify", cada um com o argumento "dist".

```javascript
grunt.registerTask('dist', ['concat:dist', 'uglify:dist']);
```

## Multitarefas
Quando uma multitarefa é executada, o Grunt procura por uma propriedade de mesmo nome em sua configuração. As multitarefas podem ter múltiplas configurações, definidas arbitrariamente usando "targets".

Especificando ambas tarefas e "targets" como `grunt concat:foo` ou `grunt concat:bar` vai processar somente as configurações dos targets especificados. Observe que se uma tarefa for renomeada com [grunt.task.renameTask](grunt.task.md#grunt.task.renameTask), o Grunt vai procurar por uma propriedade com o novo nome da tarefa no objeto da configuração.

A maioria das tarefas oficiais (contrib), incluindo a [tarefa do plugin jshint grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint), [concat task](https://github.com/gruntjs/grunt-contrib-concat) e a [tarefa do plugin concat grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat) são multitarefas.

```javascript
grunt.registerMultiTask(taskName, [description, ] taskFunction)
```

Dada a configuração especificada, este exemplo de uma multitarefa deve registrar `foo: 1,2,3` se o Grunt for executado através de `grunt log:foo`, ou deve registrar `bar: hello world` se o Grunt for executado através de `grunt log:bar`. Porém ainda se o Grunt for executado através de `grunt log` , a multitarefa deve registrar `foo: 1,2,3` depois `bar: hello world` e depois `baz: false`.

```javascript
grunt.initConfig({
  log: {
    foo: [1, 2, 3],
    bar: 'hello world',
    baz: false
  }
});

grunt.registerMultiTask('log', 'Log stuff.', function() {
  grunt.log.writeln(this.target + ': ' + this.data);
});
```


## Tarefas "básicas"
Quando uma tarefa básica é executada, o Grunt não olha a configuração ou ambiente, ele só executa a função da tarefa especificada, passando qualquer argumento seperado por dois pontos como argumentos de função.

```javascript
grunt.registerTask(taskName, [description, ] taskFunction)
```

Neste exemplo, a tarefa registra `foo, testando 123` se o Grunt foi executado através de `grunt foo:testando:123`. Se a tarefa é executada sem argumentos, como `grunt foo`, a tarefa registra `foo, sem args`.

```javascript
grunt.registerTask('foo', 'Uma simples tarefa que registra algumas coisas.', function(arg1, arg2) {
  if (arguments.length === 0) {
    grunt.log.writeln(this.name + ", sem args");
  } else {
    grunt.log.writeln(this.name + ", " + arg1 + " " + arg2);
  }
});
```

## Tarefas customizadas
Você pode fazer loucuras com as tarefas. Se você não quiser seguir a estrutura "multitarefas", use uma tarefa customizada.

```javascript
grunt.registerTask('default', 'Minha descrição para a tarefa "default".', function() {
  grunt.log.writeln('Atualmente executando a tarefa "default".');
});
```

Dentro de uma tarefa, você pode executar outras tarefas.

```javascript
grunt.registerTask('foo', 'Minha tarefa "foo".', function() {
  // Enfileirando as tarefas "bar" e "baz", para executar a tarefa "foo" quando terminar, em ordem.
  grunt.task.run('bar', 'baz');
  // Ou:
  grunt.task.run(['bar', 'baz']);
});
```

Tarefas podem ser assíncronas.

```javascript
grunt.registerTask('asyncfoo', 'Minha tarefa "asyncfoo".', function() {
  // Força a tarefa para o modo async e associa à função "done"
  var done = this.async();
  // Executa algum sync
  grunt.log.writeln('Processando a tarefa...');
  // E algum async
  setTimeout(function() {
    grunt.log.writeln('Tudo certo!');
    done();
  }, 1000);
});
```
Tarefas podem acessar os próprios nomes e argumentos.

```javascript
grunt.registerTask('foo', 'Minha tarefa "foo".', function(a, b) {
  grunt.log.writeln(this.name, a, b);
});

// Uso:
// grunt foo foo:bar
//   registra: "foo", undefined, undefined
//   registra: "foo", "bar", undefined
// grunt foo:bar:baz
//   registra: "foo", "bar", "baz"
```

As tarefas podem falhar se quaisquer erros foram registrados

```javascript
grunt.registerTask('foo', 'Minha tarefa "foo".', function() {
  if (algumErroQualquer) {
    grunt.log.error('Esta é uma mensagem de erro.');
  }

  // Falha retornando false, se a tarefa obteve erros.
  if (ifErrors) { return false; }

  grunt.log.writeln('Esta é uma mensagem de sucesso.');
});
```

Quando tarefas falham, todas as tarefas subsequentes são interrompidas se a opção `--force` foi especificada.

```javascript
grunt.registerTask('foo', 'My "foo" task.', function() {
  // Falha de forma síncrona.
  return false;
});

grunt.registerTask('bar', 'My "bar" task.', function() {
  var done = this.async();
  setTimeout(function() {
    // Falha de forma assíncrona.
    done(false);
  }, 1000);
});
```

As tarefas podem ser dependentes da execução bem sucedida de outras tarefas. Note que o `grunt.task.requires` não vai executar outra(s) tarefa(s). Só vai verificar se a tarefa foi executada e não falhou.

```javascript
grunt.registerTask('foo', 'Minha tarefa "foo".', function() {
  return false;
});

grunt.registerTask('bar', 'Minha tarefa "foo".', function() {
  // Falha a tarefa se a tarefa "foo" falhou ou nunca foi executada.
  grunt.task.requires('foo');
  // Este código executa se a tarefa "foo" foi executada com sucesso.
  grunt.log.writeln('Hello, world.');
});

// Uso:
// grunt foo bar
//   não registra nada, pois "foo" falhou.
// grunt bar
//   não registra nada, pois a tarefa "foo" nunca foi executada.
```

Tarefas podem falhar se a configuração de propriedades requeridas não existam.

```javascript
grunt.registerTask('foo', 'Minha tarefa "foo".', function() {
  // Falha se a propriedade de configuração "meta.name" estiver faltando.
  grunt.config.requires('meta.name');
  // Também falha se a propriedade de configuração "meta.name" estiver faltando.
  grunt.config.requires(['meta', 'name']);
  // Registra... Condicionalmente.
  grunt.log.writeln('Só haverá o registro, se "meta.name" estiver definida na configuração.');
});
```

Tarefas podem acessar propriedades de configurações.

```javascript
grunt.registerTask('foo', 'Minha tarefa "foo".', function() {
  // Registra o valor da propriedade. Retorna null se a propriedade é indefinida.
  grunt.log.writeln('A propriedade meta.name é: ' + grunt.config('meta.name'));
  // Também registra o valor da propriedade. Retorna null se a propriedade é indefinida.
  grunt.log.writeln('A propriedade meta.name é: ' + grunt.config(['meta', 'name']));
});
```

Dê uma olhada em [tarefas contrib](https://github.com/gruntjs/) para mais exemplos.

## Opções do CLI / ambiente
_TODO_
(pull do FAQ, recomendado: process.env)

## Por que minhas tarefas assíncronas não concluem?

Há chances de isso estar acontecendo porque você esqueceu de chamar o método [this.async](grunt.task#wiki-this-async) para dizer ao Grunt que a sua tarefa é assíncrona. Por causa da simplicidade, o Grunt usa um estilo de código síncrono, que pode ser alternado para assíncrono chamando `this.async()` dentro da tarefa.

Observe que passando `false` para a função `done()` diz ao Grunt que a tarefa falhou.

Por exemplo:

```javascript
grunt.registerTask('asyncme', 'Minha tarefa assíncrona.', function() {
  var done = this.async();
  doSomethingAsync(done);
});
```

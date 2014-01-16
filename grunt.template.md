Strings de template podem ser processadas manualmente usando as funções de template fornecidas. Além disso, o método config.get (usado por muitas tarefas) automaticamente expande as strings de template `<% %>` especificadas como dados de configuração no `Gruntfile`.

### grunt.template.process
Processa uma string [Lo-Dash template](http://lodash.com/docs/#template). O argumento `template` será processado recursivamente até não existir mais templates para processar.

O objeto de dados padrão é o objeto de configuração inteiro, mas se `options.data` está configurado, este objeto será usado no lugar. O delimitadores padrão de template são `<% %>` mas se `options.delimiters` está configurado com um delimitador customizado, esses delimitadores de template serão usados no lugar.

```js
grunt.template.process(template [, options])
```

Nos templates, o objeto `grunt` é exposto para que seja possível fazer coisas como `<%= grunt.template.today('yyyy') %>`. _Perceba que se um objeto de dados já tem uma propriedade `grunt`, a API do `grunt` não ficará acessível nesses templates._

Nesse exemplo, a propriedade `baz` é processada recursivamente até que não existam mais templates `<% %>` para processar.

```js
var obj = {
  foo: 'c',
  bar: 'b<%= foo %>d',
  baz: 'a<%= bar %>e'
};
grunt.template.process('<%= baz %>', {data: obj}) // 'abcde'
```

### grunt.template.setDelimiters
Configura os delimitadores [Lo-Dash template](http://lodash.com/docs/#template) para um conjunto pré-definido no caso de seu template `grunt.util._.template` precisar ser chamado manualmente. O delimitador `config` `<% %>` é incluído por padrão.

_Você provavelmente não precisará usar esse método pois estará usando `grunt.template.process` que usa este método internamente._

```js
grunt.template.setDelimiters(name)
```

### grunt.template.addDelimiters
Adiciona um conjunto de delimitadores [Lo-Dash template](http://lodash.com/docs/#template). Você provavelmente não precisará usar esse método, pois os delimitadores embutidos devem ser suficientes, entretanto você tem a opção de adicionar delimitadores no estilo `{% %}` ou `[% %]`

```js
grunt.template.addDelimiters(name, opener, closer)
```

## Helpers

### grunt.template.date
Formata uma data usando a [biblioteca dateformat](https://github.com/felixge/node-dateformat).

```js
grunt.template.date(date, format)
```

Nesse exemplo, uma data especificada é formatada como mês/dia/ano.

```js
grunt.template.date(847602000000, 'yyyy-mm-dd') // '1996-11-10'
```

### grunt.template.today
Formata a data de hoje usando a [biblioteca dateformat](https://github.com/felixge/node-dateformat).

```js
grunt.template.today(format)
```

Nesse exemplo, a data de hoje é formatada como um ano de 4 dígitos.

```js
grunt.template.today('yyyy') // '2014'
```

_(somebody remind me to update this date every year so the docs appear current)_
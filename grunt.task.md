Registra, executa e carrega tarefas externas.

Veja o [task lib source](https://github.com/gruntjs/grunt/blob/master/lib/grunt/task.js) e [task util lib source](https://github.com/gruntjs/grunt/blob/master/lib/util/task.js) para mais informações.

## A API de tarefas
Enquanto uma tarefa está sendo executada, o Grunt exibe diversas propriedades úteis específicas para tarefas e métodos dentro da função da tarefa através do objeto `this`. Veja o guia [[Por dentro de todas as tarefas]](Inside-Tasks.md) para uma lista destas propriedades e métodos.
 
Muitas propriedades úteis e métodos estão disponíveis dentro das tarefas através do objeto `this`.

Note que qualquer método marcado com um ☃ (unicode boneco de neve) também está disponível no objeto `grunt`. Só para você saber. Veja [página principal da API](grunt) para mais informações de uso.

## Criando tarefas

### grunt.task.registerTask ☃
Registra um "alias de tarefa" ou uma função de tarefa. Este método suporta as duas formas de escrever a seguir:

**Alias de tarefa**

Se uma lista de tarefas é especificada, a nova tarefa será um alias para uma ou mais tarefas. Sempre que este "alias de tarefa" é executado, todas as tarefas especificadas em `taskList` serão executadas, na ordem especificada. O argumento `taskList` deve ser um array de tarefas.

```javascript
grunt.task.registerTask(taskName, taskList)
```

Este exemplo de alias de tarefa define uma tarefa "default", e através desta as tarefas "jshint", "qunit", "concat" e "uglify" serão executadas automaticamente se o Grunt for executado sem especificar qualquer tarefa:

```javascript
task.registerTask('default', ['jshint', 'qunit', 'concat', 'uglify']);
```

Argumentos para tarefas podem ser especificados. Neste exemplo, a alias "dist" executa tanto a tarefa "concat" quanto a "uglify", cada uma com o argumento "dist":

```javascript
task.registerTask('dist', ['concat:dist', 'uglify:dist']);
```

**Função de tarefa**

Se uma `description` e `taskFunction` são passados, a função especificada será executada sempre que a tarefa for executada. Além disso, a descrição especificada será apresentada quando `grunt --help` for executado. Propriedades específicas de tarefas e métodos estão disponíveis dentro da função de tarefa do objecto `this`. A função de tarefa pode retornar `false` para indicar que a tarefa falhou.

Perceba que o método `grunt.task.registerMultiTask`, explicado abaixo, pode ser usado para definir um tipo especial de tarefa conhecido como "multitarefa".

```javascript
grunt.task.registerTask(taskName, description, taskFunction)
```

Esta tarefa de exemplo loga `foo, testing 123` se o Grunt for executado via `grunt foo:testing:123`. Se a tarefa for executada sem argumentos como `grunt foo`, a tarefa loga `foo, no args`.

```javascript
grunt.task.registerTask('foo', 'A sample task that logs stuff.', function(arg1, arg2) {
  if (arguments.length === 0) {
    grunt.log.writeln(this.name + ", no args");
  } else {
    grunt.log.writeln(this.name + ", " + arg1 + " " + arg2);
  }
});
```

Veja a documentação [Criando tarefas](Creating-tasks.md) para mais exemplos de tarefas e aliass de tarefas.

_Este método também está disponível como [grunt.registerTask](grunt)_

### grunt.task.registerMultiTask ☃
Registra uma "multitarefa". A multitarefa é uma tarefa que implicitamente itera por todos suas subpropriedades nomeadas (também conhecido como alvos) se nenhum alvo for especificado. Além das propriedades e métodos padrão, propriedades especificas para multitarefas estão disponíveis dentro da função da tarefa como propriedades do objeto `this`.

Muitas das tarefas contrib, incluindo as tarefas [jshint](https://github.com/gruntjs/grunt-contrib-jshint), [concat](https://github.com/gruntjs/grunt-contrib-concat) e [uglify](https://github.com/gruntjs/grunt-contrib-uglify) são multitarefas.

```javascript
grunt.task.registerMultiTask(taskName, description, taskFunction)
```

Dada a configuração especificada, este exemplo de multitarefa registraria `foo: 1,2,3` se o Grunt fosse executado via `grunt log:foo`, ou registraria `bar: hello world` se o Grunt fosse executado via `grunt log:bar`. Contudo, se o Grunt for executado como `grunt log`, seria registrado `foo: 1,2,3`, depois `bar: hello world` e depois `baz: false`.

```javascript
grunt.initConfig({
  log: {
    foo: [1, 2, 3],
    bar: 'hello world',
    baz: false
  }
});

grunt.task.registerMultiTask('log', 'Log stuff.', function() {
  grunt.log.writeln(this.target + ': ' + this.data);
});
```

Veja a documentação [Criando tarefas](Creating-tasks.md) para mais exemplos de multitarefas.

_Este método também está disponível como [grunt.registerMultiTask](grunt)._

### grunt.task.renameTask ☃
Renomeia uma tarefa. Isto pode ser útil se você quer sobrescrever o comportamento padrão de uma tarefa, enquanto retém o antigo nome.

_Note que se uma tarefa for renomeada, as propriedades [this.name](inside-tasks#this.name) e [this.nameArgs](inside-tasks#this.nameArgs) serão alteradas consequentemente._

```javascript
grunt.task.renameTask(oldname, newname)
```

_Este método também está disponível como [grunt.renameTask](grunt)._

## Carregando tarefas definidas externamente
Para a maioria dos projetos, as tarefas serão definidas no [Gruntfile](Getting-started.md). Para projetos maiores, ou em casos onde as tarefas precisam ser compartilhadas entre diversos projetos, as tarefas podem ser carregadas de um ou mais diretórios externos ou plugins do Grunt instaladas via Npm.

### grunt.task.loadTasks ☃
Carrega arquivos relacionados a tarefas do diretório especificado, relativo ao [Gruntfile](Getting-started.md). Este método pode ser usado para carregar arquivos relacionados a uma tarefa de um plugin do Grunt instalado localmente especificando o caminho para o subdiretório do plugin da tarefa.

```javascript
grunt.task.loadTasks(tasksPath)
```

_Este método também está disponível como [grunt.loadTasks](grunt)._

### grunt.task.loadNpmTasks ☃
Carrega tarefas a partir do plugin do Grunt especificado. Este plugin precisa estar instalado localmente via npm, e deve ser relativo ao [Gruntfile](Getting-started.md). Plugins do Grunt podem ser criados usando os [modelos de grunt-init para plugin do Grunt](https://github.com/gruntjs/grunt-init): `grunt init:gruntplugin`.

```javascript
grunt.task.loadNpmTasks(pluginName)
```

_Este método também está disponível como [grunt.loadNpmTasks](grunt)._


## Fila de tarefas
O Grunt automaticamente enfileira e executa todas as tarefas especificadas na linha de comando, mas tarefas individuais podem enfileirar a execução de tarefas adicionais.

### grunt.task.run
Enfileira uma ou mais tarefas. Qualquer tarefa especificada na `taskList` será executada imediatamente depois que a tarefa atual for completa, na ordem especificada. A lista de tarefas pode ser um array de tarefas ou um argumento com uma tarefa individual.

```javascript
grunt.task.run(taskList)
```

### grunt.task.clearQueue
Esvazia a fila de tarefas completamente. A menos que novas tarefas sejam adicionadas à fila, nenhuma outra tarefa será executada.

```javascript
grunt.task.clearQueue()
```

### grunt.task.normalizeMultiTaskFiles
Normaliza um objeto de configuração de uma tarefa alvo para um array de mapeamento de arquivo fonte-destino. Este método é utilizado internamente pela propriedade [this.files / grunt.task.current.files](grunt.task#wiki-this-files) do sistema multitarefas.

```javascript
grunt.task.normalizeMultiTaskFiles(data [, targetname])
```
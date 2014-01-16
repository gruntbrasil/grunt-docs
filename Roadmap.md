## grunt 0.5

1. Tarefas são módulos npm que podem ser requeridos e executam independentemente de qualquer executor de tarefa (se você quiser construir manualmente um objeto de configuração compatível para executa-lo).  Pode canalizar dados entre múltiplas tarefas (pense em um coffescript transpilation + uglify em um único passo). Toda saída de tarefa emite um evento.  Veja: https://github.com/tkellen/node-task

2. Uma biblioteca para a expansão glob que lida com arrays de glob, negação, etc.  Veja https://github.com/cowboy/node-globule

3.Uma biblioteca para a analisar a configuração (mesclar opções, expandir de template, expansão de glob (usando a lib do item #2) do formato do Gruntfile atual, de forma válida a rodar módulos compatíveis com node-task.  Irá suportar middleware definido pelo usuário para controlar a configuração de saída.

4. Um executor de tarefa que usa a biblioteca de análise de configuração do item #3 para executar módulos compatíveis com node-task (pode ser usado programaticamente, ou via cli).  Suporta definir tarefas "pseudônimas" que compilam um conjunto de tarefas que podem ser executadas em paralelo  Veja: http://github.com/gruntjs/grunt

5. Um logger para ouvir eventos e envia-las para o console.  Lida com stderr/stdout, ou o próprio Grunt tem isto embutido. Veja: https://github.com/cowboy/node-prolog

**0.5 Idéias para o Gruntfile**
```js
var grunt = require('grunt');

grunt.initConfig({
  // defaults for cli
  grunt: {
    dryRun: true,
    stack: true,
    verbose: true,
    // O que sobre definir loggers específicos para a tarefa?
    // Isto é obrigatório no seu Gruntfile ou está por padrão?
    logger: [require('grunt-logger')] 
  },
  jshint: {
    // ...
  },
  concat: {
    // ...
  },
  min: {
    // ...
  }
});

grunt.registerTask(require('grunt-contrib-jshint'));
grunt.registerTask(require('grunt-contrib-concat'));
grunt.registerTask(require('grunt-contrib-uglify'), 'min'); // Renomeações opcionais no segundo parâmetro

// Gera um objeto compatível com node-task e executa grunt.registerTask em que
grunt.registerTask('name','description', function (config) {
  //...
});

// Carregar um conjunto de tarefas para ser rodado em paralelo
grunt.registerTask('name', ['jshint', 'concat'], { parallel:true });

// eu penso que o cli deveria chamar isso, mas colocando aqui por você mencionar estar pensando em como isso deveria ser.
grunt.run();
```
***Por favor, ignore a seção abaixo.  É uma confusa bagunça/trabalho em andamento e não deve ser considerado qualquer coisa parecida com um roteiro.***

527 - execução paralela de tarefas
545 - compilação condicional (provavelmente pertence à tarefas assistidas)
493 - Manuseio de cwd

* mais códigos de erros específicos
  * Task not found
  * Task failed
  * Task requirement not met
  * Config requirement not met

## grunt-log
* Registro do stderr/stdout. [#586](https://github.com/gruntjs/grunt/issues/586) [#570](https://github.com/gruntjs/grunt/issues/570) [#120](https://github.com/gruntjs/grunt/issues/120)
* https://github.com/tkellen/grunt-decoupled/tree/master/grunt-log

## grunt-file
* https://github.com/tkellen/grunt-decoupled/tree/master/grunt-file

## grunt-util
* https://github.com/tkellen/grunt-decoupled/tree/master/grunt-util
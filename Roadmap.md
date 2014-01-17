## grunt 0.5

1. Tarefas como módulos npm, os quais podem ser requeridos e executados independente de qual for o task runner (se você quiser construir manualmente um objeto de configuração compatível para executá-lo). Pode distribuir dados entre múltiplas tarefas (pense em um coffescript transpilation + uglify em um único passo). Toda saída tarefa emitida como eventos. Veja: https://github.com/tkellen/node-task

2. Uma biblioteca para a expansão glob que lida com arrays de glob, negação, etc.  Veja https://github.com/cowboy/node-globule

3.Uma biblioteca para analisar configurações (mesclar opções, expansão de template, expansão de glob (usando a lib do item #2)) a partir do formato atual do Gruntfile, em uma forma válida para a execução de módulos compatíveis com node-task. Irá suportar middleware definido pelo usuário para controlar a configuração de saída.

4. Um task runner que usa a biblioteca de análise de configuração do item #3 para executar módulos compatíveis com node-task (pode ser usado programaticamente, ou via CLI). Suportar a definição de tarefas "atalhos" que compila um conjunto de tarefas que podem ser executadas em paralelo. Veja: http://github.com/gruntjs/grunt

5. Um histórico de logs para observar e enviar eventos para o console. Lidar com stderr/stdout, ou o Grunt ter o seu próprio embutido. Veja: https://github.com/cowboy/node-prolog

**Idéias para o Gruntfile**
```js
var grunt = require('grunt');

grunt.initConfig({
  // defaults for cli
  grunt: {
    dryRun: true,
    stack: true,
    verbose: true,
    // que tal definir históricos de logs específicos para a tarefa?
    // seria necessário no seu gruntfile ou um padrão?
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
grunt.registerTask(require('grunt-contrib-uglify'), 'min'); // renomeações opcionais no segundo parâmetro

// gera um objeto "node-task" compatível e nele, executa grunt.registerTask
grunt.registerTask('name','description', function (config) {
  //...
});

// Carrega um conjunto de tarefas para serem executadas em paralelo
grunt.registerTask('name', ['jshint', 'concat'], { parallel:true });

// eu acho que isso deveria ser chamado pelo CLI, mas estou colocando aqui pois você mencionou que poderia vir aqui.
grunt.run();
```
***Por favor, ignore a seção abaixo.  É uma confusa bagunça/trabalho em andamento e não deve ser considerado qualquer coisa parecida com um roteiro.***

527 - execução paralela de tarefas
545 - compilação condicional (provavelmente pertence à tarefas assistidas)
493 - tratamento de cwd

* mais códigos de erros específicos
  * Task not found
  * Task failed
  * Task requirement not met
  * Config requirement not met

## grunt-log
* Registrar para stderr/stdout. [#586](https://github.com/gruntjs/grunt/issues/586) [#570](https://github.com/gruntjs/grunt/issues/570) [#120](https://github.com/gruntjs/grunt/issues/120)
* https://github.com/tkellen/grunt-decoupled/tree/master/grunt-log

## grunt-file
* https://github.com/tkellen/grunt-decoupled/tree/master/grunt-file

## grunt-util
* https://github.com/tkellen/grunt-decoupled/tree/master/grunt-util
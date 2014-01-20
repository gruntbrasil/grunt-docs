## Como eu instalo o grunt?
Para instruções gerais de instalação, por favor leia o guia [[Getting Started]]. Se após a leitura desse guia, você precisar de informações mais específicas, leia o guia abrangente [[Installing grunt]]. 

## Quando poderei usar a funcionalidade em desenvolvimento 'X'?
Instalação tanto de versões publicadas e não publicadas do Grunt é coberta no guia [[Installing grunt]].

## O Grunt funciona no Windows?
O Grunt funciona bem no Windows pois o [Node.js](http://nodejs.org/) e o [npm](http://npmjs.org/) também funcionam bem no Windows. Geralmente a parte problemática é o [Cygwin](http://www.cygwin.com/) já que ele vem com uma versão desatualizada do Node.js.

A melhor maneira de evitar esse problema é usar o [Instalador msysGit](http://msysgit.github.com/) para instalar os binários do `git` e o [Instalador do Node.js](http://nodejs.org/#download) para instalar os binários do `node` e do `npm`, e usar o [prompt de comando do Windows](http://www.cs.princeton.edu/courses/archive/spr05/cos126/cmd-prompt.html) ou [PowerShell](http://support.microsoft.com/kb/968929) embutidos em vez do Cygwin.

## Porque minhas tarefas assíncronas não completam?
Provavelmente isto está acontencendo pois você esqueceu de chamar o método [this.async](grunt.task#wiki-this-async) que diz para o Grunt que sua tarefa é assíncrona. Para simplificar, o Grunt usa um estilo de codificação síncrono, que pode ser chaveado para o assíncrono chamando `this.async()` dentro do corpo da tarefa.

Perceba que passar `false` para a função `done()` diz ao Grunt que a tarefa falhou.

Por exemplo:

```javascript
grunt.registerTask('asyncme', 'My asynchronous task.', function() {
  var done = this.async();
  doSomethingAsync(done);
});
```

## Como eu habilito o auto-completar usando tab no shell?
Para habilitar o auto-completar usando tab para grunt, adicione a seguinte linha no seu arquivo `~/.bashrc`:

```bash
eval "$(grunt --completion=bash)"
```

Isso presume que o Grunt foi instalado globalmente com `npm install -g grunt`. Atualmente, o único shell suportado é o bash.

## Como posso compartilhar parâmetros entre múltiplas tarefas?
Enquanto cada tarefa pode aceitar seus próprios parâmetros, existem algumas poucas opções disponíveis para compartilhar parâmetros entre múltiplas tarefas.

### Tarefas com pseudônimo "Dynamic"
**Esse é o método sugerido para compartilhar parâmetros entre múltiplas tarefas.**

Enquanto tarefas [alias tasks](grunt#wiki-grunt-registerTask) são necessariamente simples, uma tarefa normal pode usar [grunt.task.run](grunt.task#wiki-grunt-task-run) para fazê-la efetivamente funcionar como uma tarefa pseudônima "dynamic". Nesse exemplo, executar `grunt build:001` na linha de comando resultaria na execução das tarefas `foo:001`, `bar:001` e `baz:001`.

```javascript
grunt.registerTask('build', 'Run all my build tasks.', function(n) {
  if (n == null) {
    grunt.warn('Build num must be specified, like build:001.');
  }
  grunt.task.run('foo:' + n, 'bar:' + n, 'baz:' + n);
});
```

### -- options

Outro jeito de compartilhar parâmetros entre múltiplas tarefas é usar [grunt.option](grunt#wiki-grunt-option). Nesse exemplo, executar `grunt deploy --target=staging` na linha de comando faria `grunt.option('target')` retornar `"staging"`.

```javascript
grunt.registerTask('upload', 'Upload code to specified target.', function(n) {
  var target = grunt.option('target');
  // fazer algo útil com target aqui
});
grunt.registerTask('deploy', ['validate', 'upload']);
```

_Perceba que opções booleanas podem ser especificadas usando apenas a chave sem o valor. Por exemplo, executar `grunt deploy --staging` na linha de comando faria `grunt.option('staging')` retornar `true`._

### Globals e configs

Em outros casos, você pode desejar expor uma configuração ou valores globais. Nesses casos, registre uma tarefa que configura seus argumentos como um valor global ou de configuração.

Nesse exemplo, executar `grunt set_global:name:peter set_config:target:staging deploy` na linha de comando faria `global.name` ser `"peter"` e `grunt.config('target')` retornar `"staging"`.
Pode-se presumir que a tarefa `deploy` irá retornar esses valores.

```javascript
grunt.registerTask('set_global', 'Set a global variable.', function(name, val) {
  global[name] = val;
});

grunt.registerTask('set_config', 'Set a config property.', function(name, val) {
  grunt.config.set(name, val);
});
```


***


## Perguntas relativas ao grunt 0.3

## No Windows com Grunt 0.3, porque meu editor JS abre quando eu tento executar o grunt?
Se você está no mesmo diretório do [Gruntfile](Getting-started), o Windows tenta executar _esse arquivo_ quando você digita grunt. Portanto você precisa digitar `grunt.cmd` no lugar.

Uma alternativa é usar o comando `DOSKEY` para criar uma macro Grunt, seguindo [essas instruções](http://devblog.point2.com/2010/05/14/setup-persistent-aliases-macros-in-windows-command-prompt-cmd-exe-using-doskey/). Isso permitirá usar `grunt` em vez de `grunt.cmd`.

Esse é o comando `DOSKEY` que você pode usar:

```
DOSKEY grunt=grunt.cmd $*
```
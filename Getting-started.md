Grunt e os plugins para Grunt são instalados e gerenciados via [npm](https://npmjs.org/), o gerenciador de pacotes do [Node.js](http://nodejs.org/).


_Grunt 0.4.x requer uma versão estável do Node.js `>= 0.8.0`. Versões ímpares do Node.js são consideradas versões de desenvolvimento (instáveis)._

## Instalando a CLI
**Se você está fazendo o upgrade do Grunt 0.3, por favor veja as [notas de atualização do Grunt 0.3](upgrading-from-0.3-to-0.4#grunt-0.3-notes)**

Para começar, você precisa instalar a interface de linha de comando (CLI) globalmente. Talvez você precise usar sudo (para OSX, *nix, BSD etc) ou executar o prompt de comando como Administrador (Windows) para realizar a instalação.

```shell
npm install -g grunt-cli
```

Isto irá adicionar o comando `grunt` ao path do seu sistema operacional, permitindo que você o execute a partir de qualquer diretório.

Perceba que instalar o `grunt-cli` não instala o Grunt em si! O trabalho do Grunt CLI é simples: rodar a versão do Grunt que está no `Gruntfile`.
Isto permite que múltiplas versões do Grunt sejam instaladas na mesma máquina.

## Como a CLI funciona

Toda vez que o `grunt` é executado, ele procura por um Grunt instalado localmente através do sistema `require()` do node. Assim, você pode executar o `grunt` de qualquer subdiretório do seu projeto.

Se uma instalação local do Grunt é encontrada, a CLI carrega a biblioteca Grunt instalada localmente, aplica a configuração presente no seu `Gruntfile`, e executa as tarefas que você solicitou que fossem executadas.

*Para entender de verdade o que acontece, [veja o código](https://github.com/gruntjs/grunt-cli/blob/master/bin/grunt). É bem pequeno.*

## Trabalhando com um projeto Grunt já existente
Assumindo que você já possui o Grunt CLI instalado e que o projeto já está configurado com um `package.json` e um `Gruntfile`, é muito fácil começar a trabalhar com Grunt:

1. Vá até o diretório raiz do projeto. 
2. Instale as dependências do projeto com `npm install`.
3. Execute o Grunt com `grunt`.

Isso é todo o necessário. As tarefas Grunt instaladas podem ser listadas executando `grunt --help`, mas é sempre uma ótima idéia começar com a documentação do projeto.

## Preparando um novo projeto Grunt
Uma configuração inicial típica irá adicionar dois arquivo ao seu projeto: `package.json` e um `Gruntfile`.

**package.json**: Este arquivo é usado pelo [npm] para guardar metadados para projetos publicados como módulos npm. Você irá listar o Grunt e os plugins do Grunt que seu projeto precisa como [devDependencies] neste arquivo.
 
**Gruntfile**: Este arquivo é chamado `Gruntfile.js` ou `Gruntfile.coffee` e é utilizado para configurar ou definir tarefas e carregar os plugins do Grunt.

## package.json

O arquivo `package.json` fica no diretório raiz do seu projeto, junto com o `Gruntfile`, e deve ser commitado com o código fonte do seu projeto. Executando `npm install` no mesmo diretório que um arquivo `package.json` irá instalar a versão correta de cada dependência listada neste.

Existem algumas maneiras de criar um arquivo `package.json` para seu projeto:

* A maioria dos modelos de [grunt-init] irá criar um `package.json` específico para o projeto.
* O comando [npm init] irá criar um `package.json` simples.
* Começar com o exemplo abaixo, expandindo conforme necessário, seguindo esta [especificação][json]

```js
{
  "name": "my-project-name",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.2",
    "grunt-contrib-jshint": "~0.6.3",
    "grunt-contrib-nodeunit": "~0.2.0",
    "grunt-contrib-uglify": "~0.2.2"
  }
}
```

### Instalando Grunt e plugins do Grunt
A maneira mais fácil de adicionar o Grunt e plugins do Grunt para um `package.json` existente é executando o comando `npm install <module> --save-dev`. Isto não só irá instalar o `<module>` localmente, como irá adicioná-lo para a seção [devDependencies] automaticamente, usando [intervalo de versão com til][tilde version range].

Por exemplo, isto irá instalar a última versão do Grunt no diretório do seu projeto e adicioná-lo ao seu devDepencencies:

```shell
npm install grunt --save-dev
```

O mesmo pode ser feito para os plugins do Grunt e outros módulos node. Certifique-se de commitar o `package.json` atualizado com seu projeto quando terminar!

## O Gruntfile
 O arquivo `Gruntfile.js` ou `Gruntfile.coffee` é um JavaScript válido ou arquivo CoffeeScript que pertence ao diretório raiz do seu projeto, junto com o arquivo `package.json`, e deve ser commitado com o código fonte do seu projeto.

Um `Gruntfile` é composto pelas seguintes partes:

* A função "wrapper"
* Configurações do projeto e tarefas
* Carregar plugins do Grunt e tarefas
* Tarefas customizadas

### Um exemplo de Gruntfile
No `Gruntfile` a seguir, os metadados são importados do arquivo `package.json` do projeto para as configurações do Grunt e a tarefa `uglify` do plugin [grunt-contrib-uglify] é configurada para minificar o arquivo fonte e gerar um banner de comentário dinamicamente usando estes metadados. Quando `grunt` é executado na linha de comando, a tarefa `uglify` será executada por padrão.

```js
module.exports = function(grunt) {

  // Configuração do projeto.
  grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    uglify: {
      options: {
        banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
      },
      build: {
        src: 'src/<%= pkg.name %>.js',
        dest: 'build/<%= pkg.name %>.min.js'
      }
    }
  });

  // Carrega o plugin que disponibiliza a tarefa "uglify"
  grunt.loadNpmTasks('grunt-contrib-uglify');

  // Tarefas padrão.
  grunt.registerTask('default', ['uglify']);

};
```

Agora que você viu todo o `Gruntfile`, vamos ver as partes que o compõe.

### A função "wrapper"
Todo `Gruntfile` (e gruntplugin) usa este formato básico, e todo seu código Grunt deve ser especificado dentro da função:

```js
module.exports = function(grunt) {
  // coisas do Grunt aqui.
};
```

### Configuração de projeto e tarefas
A maior parte das tarefas Grunt depende dos dados de configuração definidos num objeto passado para o método [[grunt.initConfig|grunt#grunt.initconfig]].

Neste exemplo, `grunt.file.readJSON('package.json')` importa os metadados do JSON armazenado no `package.json` nas configurações do grunt. O modelo de strings `<% %>` devem referenciar todas as propriedade de configuração, e dados de configuração como caminhos e listas de arquivos deve ser especificados desta forma para reduzir repetição.

Desde que não conflite com propriedades que suas tarefas requisitem, você pode guardar qualquer dado arbitrário dentro do objeto de configuração, caso contrário estes dados serão ignorados. E como isto é JavaScript, você não está limitado ao JSON; você pode usar qualquer JS válido aqui. Você pode até gerar a configuração programaticamente se necessário.

Como a maioria das tarefas, a `uglify` do plugin [grunt-contrib-uglify] espera que sua configuração esteja na propriedade com mesmo nome. Aqui, a opção `banner` é especificada, junto com um único alvo chamado `build` que minifica um único arquivo fonte para um único arquivo de destino.

```js
// Configuração do projeto
grunt.initConfig({
  pkg: grunt.file.readJSON('package.json'),
  uglify: {
    options: {
      banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
    },
    build: {
      src: 'src/<%= pkg.name %>.js',
      dest: 'build/<%= pkg.name %>.min.js'
    }
  }
});
```

### Carregando plugins do Grunt e tarefas
Muitas tarefas comumente usadas como [concatenation], [minification][grunt-contrib-uglify] e [linting] estão disponíveis como [plugins do Grunt](https://github.com/gruntjs). Desde que um plugin esteja especificado no `package.json` como uma dependência, e foi instalado via `npm install`, este pode ser habilitado no seu `Gruntfile` com um simples comando:

```js
// Carrega o plugin que disponibiliza a tarefa "uglify".
grunt.loadNpmTasks('grunt-contrib-uglify');
```

**Nota:** o comando `grunt --help` irá listar todas as tarefas disponíveis.

### Tarefas customizadas
Você pode configurar o Grunt para executar uma ou mais tarefas por padrão definindo uma tarefa `default`. No exemplo a seguir, executando `grunt` na linha de comando sem especificar uma tarefa irá executar a tarefa `uglify`. Isso é o mesmo que explicitamente executar `grunt uglify` ou até `grunt default`. Qualquer quantidade de tarefas (com ou sem argumentos) podem ser especificadas em uma array.

```js
// Tarefas padrão.
grunt.registerTask('default', ['uglify']);
```

Se o seu projeto requer tarefas não disponibilizadas através de plugins do Grunt, você pode definir tarefas customizadas diretamente no `Gruntfile`. Por exemplo, este `Gruntfile` define uma tarefa `default` completamente customizada que sequer utiliza configuração de tarefas:

```js
module.exports = function(grunt) {

  // Uma tarefa bem simples.
  grunt.registerTask('default', 'Log some stuff.', function() {
    grunt.log.write('Logging some stuff...').ok();
  });

};
```

Tarefas customizadas para projetos específicos não precisam ser definidas no `Gruntfile`; elas podem ser definidas externamente em arquivos `.js` e carregadas pelo método [[grunt.loadTasks|grunt#grunt.loadtasks]].

## Leitura Adicional

* O guia [[Installing grunt]] tem informações detalhadas sobre como instalar versões específicas, para produção ou para desenvolvimento, do Grunt e grunt-cli.
* O guia [[Configuring Tasks]] tem uma explicação aprofundada de como configurar tarefas, destinos, opções e arquivos dentro do `Gruntfile`, junto com uma explicação sobre modelos, globbing patterns e importação de dados externos.
* O guia [[Creating Tasks]] lista as diferenças entre os tipos de tarefas Grunt e mostra diversos exemplos de tarefas e configurações.
* Para mais informações sobre como desenvolver tarefas customizadas ou plugins do Grunt, veja a [[developer documentation|grunt]].

[npm]: https://npmjs.org/
[devDependencies]: https://npmjs.org/doc/json.html#devDependencies
[json]: https://npmjs.org/doc/json.html
[npm init]: https://npmjs.org/doc/init.html
[grunt-init]: Project-Scaffolding
[tilde version range]: https://npmjs.org/doc/misc/semver.html#Ranges
[grunt-contrib-uglify]: http://github.com/gruntjs/grunt-contrib-uglify
[concatenation]: https://github.com/gruntjs/grunt-contrib-concat
[linting]: https://github.com/gruntjs/grunt-contrib-jshint
[grunt.loadTasks]: https://github.com/gruntjs/grunt/wiki/grunt.task
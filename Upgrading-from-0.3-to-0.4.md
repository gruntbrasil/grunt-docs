_Perceba que mesmo que você esteja familiarizado com o grunt, vale a pena ler o novo guia [[Iniciando]]._

Grunt está dividido em três partes agora: `grunt`, `grunt-cli` e `grunt-init`.

1. O módulo npm `grunt` deve ser instalado localmente no seu projeto. Ele contém o código e a lógica para executar as tarefas, carregar os plugins, etc.
2. O módulo npm `grunt-cli` deve ser instalado globalmente. Ele disponibiliza o comando `grunt` no seu _PATH_, o qual pode ser executado em qualquer local do seu sistema. Por sí só, ele não faz nada; o seu trabalho é carregar e executar o grunt que foi instalado localmente no seu projeto, independentemente da versão. Para mais informações do porquê desta mudança, por favor leia [npm 1.0: Instalação Global vs Local (em inglês)](http://blog.nodejs.org/2011/03/23/npm-1-0-global-vs-local-installation).
3. A tarefa `init` foi "quebrada" em seu próprio módulo npm, `grunt-init`.  Deverá ser instalado globalmente com `npm install -g grunt-init` e executada com o comando `grunt-init`. Nos próximos meses, o [Yeoman](http://yeoman.io/) vai substituir por completo o _grunt-init_.  Veja a [página do projeto grunt-init (em inglês)](https://github.com/gruntjs/grunt-init) para mais informações.


## Notas do Grunt 0.3

Se você está realizando a atualização a partir do Grunt 0.3, certifique-se de remover o `grunt`:

```shell
npm uninstall -g grunt
```

_Perceba que para a versão 0.3.x, os nomes dos plugins e as configurações das opções nas tarefas podem ser diferentes daquelas que foram apresentadas na seção "O Gruntfile"._

_Este arquivo era nomeado como `grunt.js` na versão 0.3.x do Grunt._

## Tarefas e plugins existentes
Todos os plugins `grunt-contrib-*` estão aptos para o Grunt 0.4. Entretanto, é muito provável que plugins de terceiros escritos para o Grunt 0.3, não funcionem com a versão 0.4 até que tenham sido atualizados. Estamos trabalhando ativamente com os autores de plugins para garantir que este funcionamento aconteça o mais breve possível.

_A próxima versão lançada do Grunt será focada na dissociação da arquitetura do grunt para que os plugins não sejam afetados por conta de futuras atualizações._

## Requerimentos
- O Grunt necessita do Node.js em sua versão `>= 0.8.0`.

## O Gruntfile
- O "Gruntfile" mudou de `grunt.js` para `Gruntfile.js`.
- O "Gruntfile" possui suporte para CoffeeScript no arquivo `Gruntfile.coffee` ou em tarefas com a extensão `*.coffee` (a compilação para JS acontece automaticamente).

Confira a seção "O Gruntfile" do guia [[Iniciando]] para mais informações.

## Tarefas nativas são plugins do grunt agora
As oito tarefas nativas que foram incluídas no Grunt 0.3 agora fazem parte de plugins separados. Cada um é um módulo npm discreto que deve ser instalado como um plugin através da seção "Carregando plugins e tarefas do Grunt" no guia [[Iniciando]].

- concat → [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat) plugin
- init → [grunt-init] utilitário isolado
- lint → [grunt-contrib-jshint](https://github.com/gruntjs/grunt-contrib-jshint) plugin
- min → [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify) plugin
- qunit → [grunt-contrib-qunit](https://github.com/gruntjs/grunt-contrib-qunit) plugin
- server → [grunt-contrib-connect](https://github.com/gruntjs/grunt-contrib-connect) plugin
- test → [grunt-contrib-nodeunit](https://github.com/gruntjs/grunt-contrib-nodeunit) plugin
- watch → [grunt-contrib-watch](https://github.com/gruntjs/grunt-contrib-watch) plugin

Alguns nomes das tarefas e suas opções foram alteradas. Certifique-se de verificar a documentação de cada plugin mencionado acima para obter os detalhes das configurações mais recentes.

## Configuração
O formato de configuração para as tarefas no Grunt 0.4 foram padronizadas e melhoradas consideravelmente. Veja o guia [[Configurando tarefas]], assim como a documentação de cada plugin para mais informações.

- É possível utilizar _globbing patterns_ (coringas) para negar a exclusão dos arquivos relacionados.
- Tarefas suportam o objeto padrão `options`.
- Tarefas suportam o objeto padrão `files`.

`<% %>` as strings de template especificadas como dados de configuração dentro do `Gruntfile` são automaticamente compilados, confira a documentação [[grunt.template]] para mais informações.

**Diretivas foram removidas**, mas as suas funcionalidades foram mantidas. Estas substituições podem ser feitas:

- `'<config:prop.subprop>'` → `'<%= prop.subprop %>'`
- `'<json:file.json>'` → `grunt.file.readJSON('file.json')`
- `'<file_template:file.js>'` → `grunt.template.process(grunt.file.read('file.js'))`

Ao invés de especificar um banner em uma lista de arquivo com `'<banner>'` ou `'<banner:prop.subprop>'`, é possível incluir com os plugins [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat) e [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify) utilizando a opção `banner`.

Ao invés de remover individulamnte os banners de cada arquivo com `'<file_strip_banner:file.js>'`, o plugin [grunt-contrib-concat](https://github.com/gruntjs/grunt-contrib-concat) e [grunt-contrib-uglify](https://github.com/gruntjs/grunt-contrib-uglify) possui uma opção para remover/preservar banners.

## Mudanças nas tarefas customizadas
Quando especificar uma tarefa customizada, a lista de tarefas deverá ser especificada como um array de strings.

```js
// v0.3.x (formato antigo)
grunt.registerTask('default', 'jshint nodeunit concat');
// v0.4.x (formato novo)
grunt.registerTask('default', ['jshint', 'nodeunit', 'concat']);
```

## Argumentos de tarefas podem conter espaços agora
As mudanças nas tarefas customizadas (a lista de tarefas deve ser declarada em um array) mencionadas anteriormente permitem isso. Apenas certifique-se de envolver os espaços contido entre aspas quando especificados na linha de comando, para que sejam parseados de forma apropriada.

```shell
grunt my-task:argumento-sem-espacos "other-task:argumento com espacos"
```

## Encoding de caracteres
O método [file.defaultEncoding](grunt.file#wiki-grunt-file-defaultEncoding) foi adicionado para normalizar o enconding de caracteres, e todos os métodos da `grunt.file` foram atualizados para suportar o enconding específico.

## Helpers
O sistema de ajuda do Grunt foi removido em favor ao `require` do node. Para um exemplo conciso de como compartilhar funcionalidades entre plugins do Grunt, por favor confira [grunt-lib-legacyhelpers](https://github.com/gruntjs/grunt-lib-legacyhelpers). Os autores de plugins são encorajados a atualizar os seus plugins.

## API
A API do Grunt percorreu mudanças substanciais, da versão 0.3 para a 0.4.

- [grunt](grunt)
    - Removido os métodos `grunt.registerHelper` e `grunt.renameHelper`.
- [grunt.config](grunt.config)
    - Modificado o método [config.get](grunt.config#wiki-grunt-config-get) para expandir de maneira recursiva os templates `<% %>`  automaticamente.
    - Adicionado o método [config.getRaw](grunt.config#wiki-grunt-config-getRaw) que fará a solicitação do raw (sem expansão) dos dados de configuração.
    - Modificado o método [config.process](grunt.config#wiki-grunt-config-process) para processar agora um valor de acordo com a sua configuração, expandindo os templates recursivamente. Este método é chamado dentro do `config.get`, mas **não** dentro do `config.getRaw`.
- [grunt.event](grunt.event) adicionado para que as tarefas possam emitir eventos.
- [grunt.fail](grunt.fail)
    - Não emite um _beep_ se a opção `--no-color` estiver especificada.
    - Adicionado o alerta para código de saída `fail.code`.
    - Removido o método `fail.warnAlternate`.
- [grunt.file](grunt.file)
    - As tarefas não são mais carregadas automaticamente no diretório `~/.grunt/tasks/` (instale-as localmente no seu projeto!).
    - Adicionado o método [file.defaultEncoding](grunt.file#wiki-grunt-file-defaultEncoding) para normalizar o enconding de caracteres através de todos os métodos do `grunt.file`.
    - Adicionado o método [file.delete](grunt.file#wiki-grunt-file-delete).
    - Adicionado métodos de testes [file.exists](grunt.file#wiki-grunt-file-exists), [file.isDir](grunt.file#wiki-grunt-file-isDir), [file.isFile](grunt.file#wiki-grunt-file-isFile), [file.isLink](grunt.file#wiki-grunt-file-isLink), [file.isPathCwd](grunt.file#wiki-grunt-file-isPathCwd), [file.isPathInCwd](grunt.file#wiki-grunt-file-isPathInCwd), [file.doesPathContain](grunt.file#wiki-grunt-file-doesPathContain), [file.arePathsEquivalent](grunt.file#wiki-grunt-file-arePathsEquivalent) relativamente auto-explicativos.
    - Adicionado os métodos [file.match](grunt.file#wiki-grunt-file-match) e [file.isMatch](grunt.file#wiki-grunt-file-isMatch) para facilitar a comparação de _wildcard patterns_ com caminho de arquivos.
    - Adicionado o método [file.expandMapping](grunt.file#wiki-grunt-file-expandMapping) para usar na geração de um-para-um de mapeamentos de arquivo origem-destino.
    - Adicionado o método [file.readYAML](grunt.file#wiki-grunt-file-readYAML).
    - Modificado [file.findup](grunt.file#wiki-grunt-file-findup) para utilizar o módulo [findup-sync](https://github.com/cowboy/node-findup-sync).
    - Modificado [file.glob](grunt.file#wiki-grunt-file-glob) para utilizar o módulo [glob](https://github.com/isaacs/node-glob).
    - Adicionado [file.minimatch](grunt.file#wiki-grunt-file-minimatch) que expõe o módulo [minimatch](https://github.com/isaacs/minimatch).
    - Removido o método `file.userDir` (movido para [grunt-init]).
    - Removido o método `file.clearRequireCache`.
    - Removido os métodos `file.expandFiles` e `file.expandDirs`, para utilizar a opção `filter` do `file.expand`.
    - Removido o método `file.expandFileURLs`. Não especificar URLs onde arquivos devem ser especificados (ex.: a tarefa do qunit agora possibilita a opção de `urls`).
- [grunt.task](grunt#wiki-grunt-task)
    - As tarefas registradas, ambos com o [task.registerTask](grunt.task#wiki-grunt-task-registerTask) e [task.registerMultiTask](grunt.task#wiki-grunt-task-registerMultiTask) possuem o método `this.options`.
    - Adicionado o método [task.normalizeMultiTaskFiles](grunt.task#wiki-grunt-task-normalizeMultiTaskFiles) para facilitar a normalização de objetos `files` em multi-tarefas na propriedade `this.file`.
    - Removido os métodos `task.registerHelper` e `task.renameHelper`.
    - Removida a propriedade `task.searchDirs`.
    - Removido os métodos `task.expand` `task.expandDirs` `task.expandFiles` `task.getFile` `task.readDefaults` (movido para o [grunt-init]).
- [grunt.package](grunt#wiki-grunt-package) reflete os metadados armazenados no manifesto `package.json`.
- [grunt.version](grunt#wiki-grunt-version) é a versão atual do grunt passada como uma string.
- [grunt.template](grunt.template)
    - Adicionado o método [template.addDelimiters](grunt.template#wiki-grunt-template-addDelimiters) para novos delimitadores de template.
    - Adicionado o método [template.setDelimiters](grunt.template#wiki-grunt-template-setDelimiters) para selecionar os delimitadores de template.
    - Os delimitadores e template `init` e `user` foram removidos, mas você pode adicioná-los novamente, se necessário, com `template.addDelimiters` ([grunt-init] utiliza isto para ativar os delimitadores de template `{% %}`).
- [grunt.util](grunt.util) replaces the now-removed `grunt.utils`.
    - Modificado `util._` para uso do [Lo-Dash](http://lodash.com/)
    - Adicionado o método [util.callbackify](grunt.util#wiki-grunt-util-callbackify).
    - Modificado o método [util.spawn](grunt.util#wiki-grunt-util-spawn) para um comportamento muito melhor e passando argumentos mais consistentes no seu callback.

## Tarefas / Autores de plugins
**Autores de plugins, por favor indiquem claramente no README do seu repositório qual a versão do seu plugin não é compatível com a versão 0.3 do Grunt.**

### Tarefas
- Multi-tarefas
    - Múltiplos mapeamentos de arquivos origem-destino podem ser especificados por alvo no objeto `files` (isto é opcional).
- [this.files / grunt.task.current.files](grunt.task#wiki-this-files)
    - A propriedade `this.files` é um array de mapeamento de objetos para arquivo origem-destino que deve ser iterado através da sua multi-tarefa. Sempre será um array, e você sempre deve iterar sobre ele, mesmo que o uso mais comum seja para especificar um arquivo único.
    - Cada mapeamento de objeto de arquivo origem-destino possui a propriedade `src` e `dest` (e possivelmente outros, dependendo no que o usuário especificar). A propriedade `src` encontra-se expandido de qualquer _glob pattern_ que o usuário tenha especificado.
- [this.filesSrc / grunt.task.current.filesSrc](grunt.task#wiki-this-filesSrc)
    - A proprieade `this.filesSrc` é um array reduzido e único de todos os arquivos encontrados a partir de todas as propriedades `src`. Útil para tarefas somente com permissão de leitura.
- [this.options / grunt.task.current.options](grunt.task#wiki-this-options)
    - O método `this.options` deve ser utilizado com tarefas para normalizar opções. Dentro de uma tarefa, você deve especificar opções padrão como: `var options = this.options({option: 'defaultvalue', ...});`

### Plugins
* Um template atualizado do `gruntplugin` foi criado para compatibilidade de plugin e Grunt 0.4, e está disponível no modo standalone [grunt-init].

## Troubleshooting
* Se você instalou previamente uma versão de desenvolvimento do Grunt 0.4 ou qualquer plugin grunt-contrib, certifique-se de esvaziar o cache do npm com `npm cache clean` primeiramente para assegurar-se de que estará rodando as últimas versões do Grunt e plugins do grunt-contrib.

[grunt-init]: https://github.com/gruntjs/grunt-init

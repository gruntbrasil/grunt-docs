# Grunt-init

O grunt-init é uma ferramenta capaz de automatizar a criação de um projeto. Ele constrói uma estrutura completa com base no ambiente atual e em algumas respostas. Os arquivos e conteúdos a serem criados, dependem das escolhas feitas durante as perguntas realizadas.

_Obseravação: este utilitário costumava ser embutido no Grunt como uma tarefa de nome "init". Veja o guia [Atualizando do 0.3 para 0.4](https://github.com/gruntjs/grunt/wiki/Upgrading-from-0.3-to-0.4) do Grunt para mais informações a respeito desta mudança._

## Instalação

Para utilizar o grunt-init, é necessário realizar a sua instalação de forma global.

```shell
npm install -g grunt-init
```

Isso fará com que o comando `grunt-init` seja habilitado no seu sistema, sendo possível executá-lo em qualquer local.

_Obseravação: provavelmente, você precise utilizar `sudo` (para OSX, ***nix, BSD, etc) ou rodar este comando como administrador (Windows)._


## Como utilizar

* Para ajuda e lista de templates disponíveis, execute: `grunt-init --help`
* Criar um projeto baseado em um template: `grunt-init TEMPLATE`
* Criar um projeto baseado em um template em um determinado local: `grunt-init /path/to/TEMPLATE`

Perceba que os templates geram seus arquivos no diretório atual, então certifique-se de estar em um novo diretório para que os seus arquivos anteriores não sejam substituídos.


## Instalando Templates

Uma vez que os templates estiverem instalados dentro do seu diretório `~/.grunt-init/` (`%USERPROFILE%\.grunt-init\` no Windows), eles estarão disponíveis para a utilização com o grunt-init. É recomendável que você utilize o git para clonar um template para este diretório. Por exemplo, o template [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery) pode ser instalado da seguinte forma:

```bash
git clone git@github.com:gruntjs/grunt-init-jquery.git ~/.grunt-init/jquery
```
_Observação: se você quiser que o template fique disponível localmente como "foobarbaz", você pode especificar `~/.grunt-init/foobarbaz` enquanto realiza o clonagem. O grunt-init utilizará o nome do diretório atual do template, uma vez que exista dentro do diretório `~/.grunt-init/`_

Alguns templates que são mantidos oficialmente:

* [grunt-init-commonjs](https://github.com/gruntjs/grunt-init-commonjs) - Cria um módulo commonjs, incluindo testes unitários com o Nodeunit. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-commonjs-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-commonjs-sample#project-creation-transcript))
* [grunt-init-gruntfile](https://github.com/gruntjs/grunt-init-gruntfile) - Cria um Gruntfile básico. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-gruntfile-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-gruntfile-sample#project-creation-transcript))
* [grunt-init-gruntplugin](https://github.com/gruntjs/grunt-init-gruntplugin) - Cria um plugin Grunt, incluindo testes unitários com o Nodeunit. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-gruntplugin-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-gruntplugin-sample#project-creation-transcript))
* [grunt-init-jquery](https://github.com/gruntjs/grunt-init-jquery) - Cria um plugin para jQuery, incluindo testes unitários com o QUnit. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-jquery-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-jquery-sample#project-creation-transcript))
* [grunt-init-node](https://github.com/gruntjs/grunt-init-node) - Cria um módulo Node.js, incluindo testes unitários com o Nodeunit. ([sample "generated" repo](https://github.com/gruntjs/grunt-init-node-sample/tree/generated) | [creation transcript](https://github.com/gruntjs/grunt-init-node-sample#project-creation-transcript))


## Templates Customizados

Você pode criar e usar templates personalizados. O seu template deve seguir a mesma estrutura que os templates acima mencionados.

Um template de exemplo chamado `meu-template` deve seguir a seguinte estrutura geral para os arquivos:

* `meu-template/template.js` - o arquivo de template principal
* `meu-template/rename.json` - regras especificas para renomear templates
* `meu-template/root/` - arquivos à serem copiados no local desejado

Na certeza de que estes arquivos constam no `/caminho/para/meu-template`, o comando grunt-init será utilizado para processar o template. Vários templates de nomes exclusivos podem existir em um mesmo diretório.

Além disso, se você colocar este template customizado no seu diretório `~/.grunt-init/` (`%USERPROFILE%\.grunt-init\` no Windows) ele será automaticamente habilitado para ser utilizado somente com `grunt-init meu-template`.


### Copiando os arquivos

Enquanto o template utilizar os métodos `init.filesToCopy` e `init.copyAndProcess`, qualquer arquivo no sub-diretório `/root` será copiado para o diretório atual quando a inicialização do template for executada.

Note que todos os arquivos copiados serão processados como templates, com qualquer delimitador `{% %}` ques estiverem sendo processados com os dados dos objetos `props` coletados, a menos que a opção `noProcess` esteja definida. Veja o [jquery template](https://github.com/gruntjs/grunt-init-jquery), por exemplo.

### Renomeando ou excluíndo arquivos do template

O `rename.json` descreve o mapeamento do `sourcepath` para o `destpath`. O `sourcepath` precisa ser o caminho relativo do arquivo que será copiado para o diretório `root/`, mas o valor do `destpah` pode conter delimitadores `{% %}`, descrevendo como será o caminho de destino.

Caso `destpath` esteja definido como `false`, o arquivo não será copiado. Além disso, padrões globais (glob patterns) são suportados em `srcpath`.

## Especificando respostas padrões

Cada prompt de inicialização terá um valor padrão ou fará uma busca no ambiente atual para determinar um valor padrão. Se você quiser sobrescrever um valor padrão em particular, você pode fazê-lo com o arquivo `defaults.json`.

Por exemplo, meu arquivo `defaults.json` se parece com isso, pois eu quero usar um nome ligeiramente diferente do nome padrão, eu quero excluir o meu endereço de e-mail, e eu quero especificar uma url para o autor automaticamente.

```json
{
  "author_name": "\"Cowboy\" Ben Alman",
  "author_email": "none",
  "author_url": "http://benalman.com/"
}
```

_Note: até que todas as solicitações internas tenham sido documentadas, você pode encontrar os seus nomes e os valores padrões no [código fonte](https://github.com/gruntjs/grunt-init/blob/master/tasks/init.js)._

## Definindo um template inicial

### exports.description

Esta breve descrição do template será exibida junto ao nome do template quando o usuário executar o comando `grunt init` ou `grunt-init` para listar todos os templates disponíveis.

```js
exports.description = "A descrição vai aqui.";
```

### exports.notes

Se for especificado, esta descrição estendida opcional será exibida antes que quaisquer prompts forem exibidos. Este é um bom lugar para dar ao usuário uma pequena ajuda explicando convenções de nomenclatura, que podem ser obrigatórios ou opcionais, etc.

```js
exports.notes = "Uma nota adicional qualquer"
```

### exports.warnOn

Se este opcional (mas recomendado) padrão *willdcard* ou array de padrões willdcards se relacionarem, o Grunt será abortado com um aviso de que o usuário poderá continuar com `--force`. Isto é muito útil nos casos em que o init template venha a substituir arquivos existentes.

```js
exports.warnOn = wildcardPattern;
```

Enquanto o valor mais comum será `'*'`, sendo equivalente a qualquer arquivo ou diretório, a sintaxe padrão do curinga [minimatch](https://github.com/isaacs/minimatch) utilizado permitirá uma grande flexibilidade. Por exemplo:

```js
// Avisar no arquivo Gruntfile.js.
exports.warnOn = 'Gruntfile.js';

// Avisar em qualquer arquivo .js.
exports.warnOn = '*.js';

// Avisar em qualquer arquivo ou diretório que não seja "sem ponto" (dotfile/dotdir).
exports.warnOn = '*';

// Avisar em qualquer arquivo ou diretório "sem ponto" (dotfile/dotdir).
exports.warnOn = '.*';

// Avisar em qualquer arquivo ou diretório "sem ponto" ou "com ponto".
exports.warnOn = '{.*,*}';

// Avisar em qualquer arquivo (ignorando diretórios).
exports.warnOn = '!*/**';

// Avisar em qualquer arquivo de imagem.
exports.warnOn = '*.{png,gif,jpg}';

// Esta é outra maneira para descrever o exemplo anterior.
exports.warnOn = ['*.png', '*.gif', '*.jpg'];
```

### exports.template

Enquanto as propriedades `exports` são definidas fora dessa função, todo o código de inicialização atual é especificado dentro. Três argumentos são passados nesta função. O argumento `grunt` é uma referência ao grunt, contendo todos os métodos e bibliotecas do grunt. O argumento `init` é um objeto que contém métodos e propriedades específicas para este template. O argumento `done` é uma função que deve ser chamada quando o template finalizar a sua execução.

```js
exports.template = function(grunt, init, done) {
  // Veja a seção "Dentro de um init template"
};
```

## Por dentro do init template

### init.addLicenseFiles

Adiciona os nomes apropriados para os arquivos de licenças ao objeto *files*.

```js
var files = {};
var licenses = ['MIT'];
init.addLicenseFiles(files, licenses);
// files === {'LICENSE-MIT': 'licenses/LICENSE-MIT'}
```

### init.availableLicenses

Retorna um array com as licenças disponíveis.

```js
var licenses = init.availableLicenses();
// licenses === [ 'Apache-2.0', 'GPL-2.0', 'MIT', 'MPL-2.0' ]
```

### init.copy

Através de um caminho de origem absoluto ou relativo, e um caminho de destino opcional, copia o arquivo, podendo ser processado opcionalmente através de um *callback*.

```js
init.copy(srcpath[, destpath], options)
```

### init.copyAndProcess

Itera todos os arquivos no objeto passado, copiando o arquivo da fonte para o destino, processando os conteúdos.

```js
init.copyAndProcess(files, props[, options])
```

### init.defaults

Valores padrões de inicialização que foram especificados pelo usuário no arquivo `defaults.json`.

```js
init.defaults
```

### init.destpath

Destino absoluto do arquivo.

```js
init.destpath()
```

### init.expand

O mesmo que [grunt.file.expand](https://github.com/gruntjs/grunt/wiki/grunt.file#wiki-grunt-file-expand).

Retorna um array único com todos os arquivos ou caminhos que correspondam a determinado padrão `wildcard`. Este método aceita a sepração por vírgulas ou um array de padrões `wildcard`. Caminhos que correspondam a padrões començando com `!`, serão excluídos do array retornado. Os padrões são processados em ordem, então a ordem de inclusão e exclusão é significativa.

```js
init.expand([options, ] patterns)
```

### init.filesToCopy

Retorna um objeto contendo arquivo a serem copiados com os seus caminhos de origem absolutos e caminhos de destino relativos, renomeando (ou omitindo) de acordo com as regras em `rename.json` (caso exista).

```js
var files = init.filesToCopy(props);
/* files === { '.gitignore': 'template/root/.gitignore',
  '.jshintrc': 'template/root/.jshintrc',
  'Gruntfile.js': 'template/root/Gruntfile.js',
  'README.md': 'template/root/README.md',
  'test/test_test.js': 'template/root/test/name_test.js' } */
```

### init.getFile

Pega o caminho do arquivo de uma tarefa.

```js
init.getFile(filepath[, ...])
```

### init.getTemplates

Retorna um objeto de todos os templates existentes.

```js
init.getTemplates()
```

### init.initSearchDirs

Inicializa os diretórios em busca de init templates. `template` é a localização de um template. Também incluirá `~/.grunt-init/` e a inicialização principal das tarefas através do grunt-init.

```js
init.initSearchDirs([filename])
```

### init.process

Inicia o processo para começar a solicitação de entrada de dados.

```js
init.process(options, prompts, done)
init.process({}, [
  // Prompt para estes valores
  init.prompt('name'),
  init.prompt('description'),
  init.prompt('version')
], function(err, props) {
  // Finalizado, faça algo com as propriedades
});
```

### init.prompt

Solicita uma entrada de valor para o usuário.

```js
init.prompt(name[, default])
```

### init.prompts

Um objeto de todos os *prompts*

```js
var prompts = init.prompts;
```

### init.readDefaults

Faz a leitura dos valores padrões do JSON a partir dos arquivos de tarefas (se existirem), mesclando-os em um objeto de dados.

```js
init.readDefaults(filepath[, ...])
```

### init.renames

As regras para renomear o template.

```js
var renames = init.renames;
// renames === { 'test/name_test.js': 'test/{%= name %}_test.js' }
```

### init.searchDirs

Um array dos diretórios a serem vasculhados no template.

```js
var dirs = init.searchDirs;
/* dirs === [ '/Users/shama/.grunt-init',
  '/usr/local/lib/node_modules/grunt-init/templates' ] */
```

### init.srcpath

Busca por um arquivo no template init e retorna um caminho absoluto.

```js
init.srcpath(filepath[, ...])
```

### init.userDir

Retorna o caminho absoluto para o diretório template do usuário.

```js
var dir = init.userDir();
// dir === '/Users/shama/.grunt-init'
```

### init.writePackageJSON

Salva um arquivo `package.json` no diretório de destino. O retorno pode ser usado para propriedades de pós-processamento a fim de adicionar/remover/o que for.

```js
init.writePackageJSON(filename, props[, callback])
```

## Prompts embutidos

### author_email
Endereço de email do autor para ser utilizado no `package.json`. Tentará encontrar um valor padrão nas configurações git do usuário.

### author_name
Nome completo do autor para ser utilizado no `package.json`. Tentará encontrar um valor padrão nas configurações git do usuário.

### author_url
Uma URL pública do autor para ser utilizada no `package.json`.

### bin
Um caminho relativo do projeto para o script CLI.

### bugs
A url do projeto para relatar problemas. Será utilizada a url do repositório no Github, caso exista.

### description
Uma descrição do projeto. Usada no `package.json` e nos arquivos `README`.

### grunt_version
A versão semanticamente válida do grunt no projeto.

### homepage
A url para a home page do projeto. Será utilizada a url do repositório no Github, caso exista.

### jquery_version
Se for um projeto integrado ao jQuery, define a versão necessária para o jQuery. A versão deve ser semanticamente correta para ser validada.

### License
A licença do projeto. Licenças múltiplas serão separadas por espaço. As licenças embutidas são: `MIT`, `MPL-2.0`, `GPL-2.0` e `Apache-2.0`. O padrão é `MIT`. Adicione licenças customizadas com [init.addLicenseFiles](#initaddlicensefiles).

### main
O principal ponto de entrada do projeto. O padrão é o nome do projeto dentro da pasta `lib`.

### name
O nome do projeto. Será muito usado em todo o template do projeto. O padrão é o diretório de trabalho atual.

### node_version
A versão semanticamente válida do Node.js no projeto.

### npm_test
Comando para rodar testes nos eu projeto. O padrão é `grunt`.

### repository
Respoitório git do projeto. O padrão é um palpite de uma url github.

### title
O nome do projeto legível. O padrão é o nome real do projeto alterado para ser mais legível.

### version
A versão do projeto. O padrão é, 0.1.0.

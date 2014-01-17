Este documento explica como instalar uma versão específica do Grunt e de plugins do Grunt. Se você não leu o guia [[Introdução]](Getting-started.md), deve dar uma olhada nele antes.

## Visão geral
Grunt e plugins do Grunt devem ser definidos como [devDependencies](https://npmjs.org/doc/json.html#devDependencies) no [package.json](https://npmjs.org/doc/json.html) do seu projeto. Isto irá permitir que você instale todas as dependências do seu projeto com um único comando: `npm install`. As versões estáveis e de desenvolvimento do Grunt são sempre listadas na [página da wiki](https://github.com/gruntjs/grunt/wiki/).

## Instalando uma versão específica
Se você precisar de uma versão específica do Grunt ou de um plugin do Grunt, execute `npm install grunt@VERSAO --save-dev` onde `VERSAO` é a versão que você deseja. Isto irá instalar a versão especificada, adicionando-a no devDependencies do package.json.

Perceba que um [intervalo de versão com til][tilde version range] será usado no seu `package.json` quando você adicionar a flag `--save-dev` ao `npm install`. Isto é bom, pois lançamentos de patches da versão especificada serão automaticamente atualizados enquanto o desenvolvimento continua, por [versionamento semântico][semver].

[tilde version range]: https://npmjs.org/doc/json.html#Tilde-Version-Ranges
[semver]: http://semver.org

## Instalando uma versão de desenvolvimento publicada
De tempos em tempos, enquanto uma nova funcionalidade está sendo desenvolvida, versões do Grunt podem ser publicadas para o npm. Estas versões _nunca_ serão instaláveis sem especificar explicitamente o número da versão, e terá um número de versão ou designação de alpha/beta/release candidate.

Da mesma forma que se instala uma versão específica do grunt, execute `npm install grunt@VERSAO --save-dev` onde `VERSAO` é a versão que você deseja, e o npm irá instalar esta versão do Grunt no diretório do seu projeto, adicionando-a ao devDependencies do seu `package.json`.

Note que independente da versão que você especifica, um [intervalo de versão com til][tilde version range] será inserido no `package.json`. **Isto é muito ruim**, já que novos, possivelmente incompatíveis, lançamentos de patches da versão de desenvolvimento especificada podem ser instalados pelo npm, quebrando sua versão.

_Neste caso é **muito importante** que você manualmente edite seu `package json` e remova o ~ (til) do número de versão. Isto irá fechar seu Grunt na versão de desenvolvimento exata que especificou._

O mesmo processo deve ser usado para instalar uma versão de desenvolvimento publicada de um plugin do Grunt.

## Instalando diretamente do Github
Se você quiser instalar uma versão muito recente, ainda não publicada do Grunt ou de um plugin do Grunt, siga as instruções para especificar uma [URL git como dependência](https://npmjs.org/doc/json.html#Git-URLs-as-Dependencies) e certifique-se de especificar um SHA commit atual (não um nome de branch) como o `commit-ish`. Isto irá garantir que seu projeto sempre use esta exata versão do Grunt.

A versão especificada pela URL git deve ser do repositório oficial do Grunt ou de um fork.
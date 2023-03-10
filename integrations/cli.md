<Logo name="windi" class="logo-float-xl"/>

# Interface da Linha de Comando da Windi CSS

<PackageInfo name="windicss" author="voorjaar" />

## Instalar

Adicionar o pacote:

```bash
npm i -g windicss
```

## Uso

### Imprimir a Mensagem de Ajuda

```bash
windicss --help  // windicss -h
```

deve imprimir a mensagem de ajuda como abaixo.

```bash
Generate css from text files that containing windi classes.
By default, it will use interpretation mode to generate a single css file.

Usage:
  windicss [filenames]
  windicss [filenames] -c -m -d
  windicss [filenames] -c -s -m -d
  windicss [filenames] [-c | -i] [-a] [-b | -s] [-m] [-d] [-p <prefix:string>] [-o <path:string>] [--args arguments]

Options:
  -h, --help            Print this help message and exit.
  -v, --version         Print windicss current version and exit.

  -i, --interpret       Interpretation mode, generate class selectors. This is the default behavior.
  -c, --compile         Compilation mode, combine the class name in each row into a single class.
  -a, --attributify     Attributify mode, generate attribute selectors. Attributify mode can be mixed with the other two modes.
  -t, --preflight       Add preflights, default is false.

  -b, --combine         Combine all css into one single file. This is the default behavior.
  -s, --separate        Generate a separate css file for each input file.

  -d, --dev             Enable hot reload and watch mode.
  -m, --minify          Generate minimized css file.
  -z, --fuzzy           Enable fuzzy match, only works in interpration mode.
  -p, --prefix PREFIX   Set the css class name prefix, only valid in compilation mode. The default prefix is 'windi-'.
  -o, --output PATH     Set output css file path.
  -f, --config PATH     Set config file path.

  --style               Parse and transform windi style block.
  --init PATH           Start a new project on the path.
```

### Projeto Inicial de Teste

```bash
windicss --init <project>  // windicss --init .
windicss --init <project> --compile  // windicss --init hello_world --compile
```

### Nomes de Ficheiro

O par??metro `[filenames]` pode incluir caminhos de ficheiro e padr??es glob (alimentado pelo [node-glob](https://github.com/isaacs/node-glob)).

```bash
windicss './hello.html' './world.html'
windicss './**/*.html'
windicss './src/**/*.html'
windicss './hello.html' './world.html', './src/**/*.svelte'
```

### Compilar o Ficheiro de CSS

#### Gerar o CSS Normal

Use o par??metro `-o` para especificar o nome do ficheiro de CSS gerado, e o par??metro `-t` para especificar se adiciona o preflight (estilos de base).

```bash
windicss './**/*.html'
windicss './**/*.html' -to windi.css
windicss './test.html' -to windi.css
windicss './test.html' --preflight --output windi.css
```

#### Minimizar a Constru????o

Use `-m` ou `--minify` para gerar o ficheiro de CSS minimizado. Este par??metro ?? maioritariamente usado para o momento da constru????o.

```bash
windicss './**/*.html' -mto windi.min.css
windicss './**/*.html' -to windi.css --minify
```

#### Usando o Modo de Compila????o

O modo de compila????o combinar?? todos os utilit??rios da windi em uma nova classe, que podes especificar com `-p` ou `--prefix`

```bash
windicss './**/*.html' -cto windi.css
windicss './**/*.html' -ctom windi.min.css
windicss './**/*.html' -cto windi.css --minify
windicss './**/*.html' -cto windi.css --minify --prefix 'tw-'
windicss './test.html' --compile --preflight --output windi.css
```

Dar um exemplo

```html
<div class="windi-15wa4me">
  <img class="windi-1q7lotv" src="/img/erin-lindford.jpg" alt="Woman's Face">
  <div class="windi-7831z4">
    <div class="windi-x3f008">
      <p class="windi-2lluw6">
        Erin Lindford
      </p>
      <p class="windi-1caa1b7">
        Product Engineer
      </p>
    </div>
    <button class="windi-d2pog2">Message</button>
  </div>
</div>
```

#### Usando o Modo `attributify`

Tu podes combinar o modo `attributify` com o modo de interpreta????o ou modo de compila????o.

```bash
windicss './**/*.html' -ato windi.css
windicss './**/*.html' -atom windi.min.css
windicss './**/*.html' -ato windi.css --minify
windicss './test.html' --attributify --preflight --output windi.css
windicss './test.html' --attributify --compile --preflight --output windi.css
```

Dar um exemplo

```html
<button
  bg="blue-400 hover:blue-500 dark:blue-500 dark:hover:blue-600"
  text="sm white"
  font="mono light"
  p="y-2 x-4"
  border="2 rounded blue-200"
>
  Button
</button>
```

#### Passar um Ficheiro de Configura????o

Passe um ficheiro de configura????o com o par??metro `-f` ou `--config`, atualmente apenas os ficheiros de configura????o de JavaScript s??o suportados.

```bash
windicss './**/*.html' -to windi.css --config windi.config.js
```

Dar um exemplo

```js windi.config.js
module.exports = {
  // ...
  theme: {
    extend: {
      colors: {
        primary: '#1c1c1e',
      },
      // ...
    },
  },
}
```

#### Modo de Desenvolvimento

O modo de desenvolvimento liga o recarregamento instant??neo e observar?? as mudan??as do teu ficheiro para atualizar o teu ficheiro em tempo real.

```bash
windicss './**/*.html' -to windi.css --dev
```

> Nota: Para uma experi??ncia melhor de recarregamento instant??neo (~5ms) n??o removemos o CSS constru??do no momento do desenvolvimento, assim ??s esperado para reconstru??-lo uma vez no momento do lan??amento usando o comando de minimizar para conseguires a melhor experi??ncia para ambos desenvolvimento e constru????o. Tal como `windicss './**/*.html' -mto windi.css`

#### Modo `fuzzy`

Por padr??o a windi corresponde a `class/className='...'` nos ficheiros de chegada, se o tipo do teu ficheiro n??o corresponder, podes ligar esta op????o. Ela corresponder?? todos utilit??rios poss??veis contidos no ficheiro

```bash
windicss './**/*.html' -to windi.css --dev --fuzzy
```

Tu podes tamb??m configurar o `extractors` para tipos de ficheiro espec??ficos

```js windi.config.js
module.exports = {
  // ...
  extract: {
    extractors: [
      {
        extractor: (content) => {
          return { classes: content.match(/(?<=class:)[!@\w-]+/g) ?? [] }
        },
        extensions: ['svelte'],
      },
    ],
  },
  // ...
}
```

#### Bloco `style`

Para ativar o bloco de estilo, precisas usar a op????o `--style`.

```bash
windicss './**/*.html' -to windi.css --dev --style
```

Defina o bloco de estilo como este:

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>cli</title>
  <link rel="stylesheet" href="windi.css">
  <style lang='windi'>
    .btn-blue {
      @apply bg-blue-500 hover:bg-blue-700 text-white;
      padding-top: 1rem;
    }
  </style>
</head>
```
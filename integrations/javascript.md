<Logo name="javascript" class="logo-float-xl"/>

# API de JavaScript da Windi CSS

<PackageInfo name="windicss" author="voorjaar" />

## Sobre

Se o uso da interface da linha de comando não se ajusta ao teu fluxo de trabalho, poderias usar diretamente a API da Windi CSS.

## Instalar

Adicionar o pacote:

```bash
npm i -D windicss
```

## Uso

### Configurar com o modo de interpretação


```js
const { Processor } = require('windicss/lib')
const { HTMLParser } = require('windicss/utils/parser')

export function generateStyles(html) {
  // Receber o processador da windi
  const processor = new Processor()

  // Analisar todas as classes e colocá-las em uma linha para simplificar as operações
  const htmlClasses = new HTMLParser(html)
    .parseClasses()
    .map(i => i.result)
    .join(' ')

  // Gerar o preflight baseada na HTML que introduzimos
  const preflightSheet = processor.preflight(html)

  // Processar as classes da HTML para uma folha de estilo interpretada
  const interpretedSheet = processor.interpret(htmlClasses).styleSheet

  // Construir os estilos
  const APPEND = false
  const MINIFY = false
  const styles = interpretedSheet.extend(preflightSheet, APPEND).build(MINIFY)

  return styles
}
```

### Configurar com o modo de compilação

O modo de compilação exige um pouco mais de trabalho de perna para trocar os nomes de classe compilados para os atuais.

Leia mais sobre o [modo de compilação](/posts/modes.html).

```js
const { Processor } = require('windicss/lib')
const { HTMLParser } = require('windicss/utils/parser')

export function generateStyles(html) {
  // Receber o processador da windi
  const processor = new Processor()

  // Analisar o HTML para receber o arranjo das correspondências de classe com a localização
  const parser = new HTMLParser(html)

  // Gerar o preflight baseado no HTML que introduzimos
  const preflightSheet = processor.preflight(html)

  const PREFIX = 'windi-'
  const outputCSS = []
  let outputHTML = ''
  let indexStart = 0

  parser.parseClasses().forEach((p) => {
    // adicionar subsequência de caracteres do índice para corresponder o início
    outputHTML += html.substring(indexStart, p.start)

    // gerar a folha de estilo
    const style = processor.compile(p.result, PREFIX)

    // adicionar a folha de estilo à pilha de estilos
    outputCSS.push(style.styleSheet)

    // anexar as classes ignoradas e empurrar para a saída
    outputHTML += [style.className, ...style.ignored].join(' ')

    // marcar o final como nosso novo início para a próxima iteração
    indexStart = p.end
  })

  // anexar o resto do html
  outputHTML += html.substring(indexStart)

  // Construir os estilos
  const MINIFY = false
  const styles = outputCSS
    // estender a folha de preflight com cada folha da pilha
    .reduce((acc, curr) => acc.extend(curr), preflightSheet)
    .build(MINIFY)

  return {
    styles,
    outputHTML,
  }
}
```

### Configurar com o modo `attributify`

O modo `attributify` exige um pouco mais de trabalho de perna para analisar cada atributo individual.

Leia mais sobre o [modo `attributify`](/posts/v30.html#attributify-mode)
E podes encontrar a [sintaxe nesta ligação](/posts/attributify.html)

```js
const { Processor } = require('windicss/lib')
const { HTMLParser } = require('windicss/utils/parser')

export function generateStyles(html) {
  // Receber o processador da windi
  const processor = new Processor()

  // Analisar o HTML para receber o arranjo das correspondências de classe com a localização
  const parser = new HTMLParser(html)

  // Gerar o preflight baseado no HTML que introduzimos
  const preflightSheet = processor.preflight(html)

  // Sempre retornar um arranjo
  const castArray = val => (Array.isArray(val) ? val : [val])

  const attrs = parser.parseAttrs().reduceRight((acc, curr) => {
    // receber a chave da correspondência atual
    const attrKey = curr.key

    // ignorar os atributos `class` ou `className`
    if (attrKey === 'class' || attrKey === 'className') return acc

    // receber o valor da correspondência atual como arranjo
    const attrValue = castArray(curr.value)

    // se a chave da correspondência atual já estiver no acumulador
    if (attrKey in acc) {
      // receber o valor da chave do atributo atual como arranjo
      const attrKeyValue = castArray(acc[attrKey])

      // anexar o valor atual ao valor do acumulador
      acc[attrKey] = [...attrKeyValue, ...attrValue]
    } else {
      // ou então adicionar o arranjo do valor do atributo ao acumulador
      acc[attrKey] = attrValue
    }

    return acc
  }, {})

  // Construir os estilos
  const MINIFY = false
  const styles = processor
    .attributify(attrs)
    .styleSheet.extend(preflightSheet)
    .build(MINIFY)

  return styles
}
```

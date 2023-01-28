# Extrações

A Windi CSS depende da **exames e extrações estáticas** dos teus ficheiros de código-fonte para encontrar o teu uso de utilitário e gerar o CSS equivalente sobre demanda. Similar a [limitação de Purgação da Tailwind](https://tailwindcss.com/docs/optimizing-for-production#writing-purgeable-html), precisarás usar os nomes estáticos completos dos utilitários para a Windi CSS detetá-los corretamente. Por exemplo, concatenações de sequências de caracteres (String, em Inglês) NÃO PODEM SER extraídas estaticamente:

```html
<div class="text-${ active ? 'green' : 'orange' }-400"></div>
```

No lugar de exemplo de cima use os nomes completos dos utilitários:

```html
<div class="${ active ? 'text-green-400' : 'text-orange-400' }"></div>
```

## Safelist

Algumas vezes terás de usar concatenações dinâmicas:

```html
<div class="p-${size}"></div>
```

Para isto, precisarás de especificar as possíveis combinações ma opção `safelist` do ficheiro `windi.config.js`.

```ts windi.config.js
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  safelist: 'p-1 p-2 p-3 p-4',
})
```

Ou ser mais flexível:

```ts windi.config.js
import { defineConfig } from 'windicss/helpers'

function range(size, startAt = 1) {
  return Array.from(Array(size).keys()).map(i => i + startAt)
}

export default defineConfig({
  safelist: [
    range(3).map(i => `p-${i}`), // p-1 to p-3
    range(10).map(i => `mt-${i}`), // mt-1 to mt-10
  ],
})
```

## Exame de Recursos

Quando o processo do servidor de desenvolvimento ou construção começar, a Windi CSS examinará o teu código-fonte e extrairá os usos de utilitários. Por padrão, ela examinará os ficheiros sob `src/` com as extensões `vue, html, mdx, pug, jsx, tsx`.

Se quiseres ativar ou desativar o exame para os outros tipos de ficheiro ou localizações, podes configurar isto usando as opções `include` e `exclude`:

```ts windi.config.js
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  extract: {
    // aceita globais e caminhos de ficheiro relativo à raiz do projeto
    include: [
      'index.html',
      'src/**/*.{vue,html,jsx,tsx}',
    ],
    exclude: [
      'node_modules/**/*',
      '.git/**/*',
    ],
  },
})
```

### Preflight

Preflight (redefinição de estilo) também é ativada sobre demanda com o exame.

Tu podes desativá-lo completamente na configuração:

```ts windi.config.js
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  preflight: false,
})
```

Ou explicitamente ativá-lo com alistamento seguro (`safelist`):

```ts windi.config.js
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  preflight: {
    safelist: 'h1 h2 h3 p img',
  },
})
```

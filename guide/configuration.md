[windi css]: https://github.com/windicss/windicss
[tailwind css]: https://tailwindcss.com/docs
[guia de migração]: /guide/migration

# Configurar a Windi CSS

A configuração na [Windi CSS] é parecida com o que esperarias na [Tailwind CSS] mas com otimizações e funcionalidades adicionais.

Se estiveres a migrar da Tailwind, consulte primeiro o [guia de migração].

## Ficheiro de Configuração

Por padrão, a Windi CSS procurará pelo ficheiro de configuração sob raiz do teu projeto. Os nomes de ficheiro válidos são:

- `windi.config.ts`
- `windi.config.js`
- `tailwind.config.ts`
- `tailwind.config.js`

O **Módulo de ECMAScript nativo e TypeScript são suportados fora da caixa**, alimentado pelo [sucrase](https://github.com/alangpierce/sucrase).

Para conseguires a verificação de tipo para as tuas configurações, podes importar a função `defineConfig` de `windicss/helpers`:

```ts windi.config.ts
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  /* configurações... */
})
```

```js windi.config.js
// @ts-check - ativa verificação de TS para ficheiro JS
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  /* configurações... */
})
```

A `defineConfig` é uma função de contorno com sugestões de tipo, o que significa que podes omiti-lo se não precisares da conclusão automática de código / verificação de tipo.

```js windi.config.js
export default {
  /* configurações... */
}
```

Tu podes usar a conclusão automática de código a partir do teu editor para ver os possíveis campos de configuração. A personalização para as funcionalidades serão descritas nas páginas correspondentes.

## Exemplo de Configuração

```js windi.config.js
import { defineConfig } from 'windicss/helpers'
import colors from 'windicss/colors'
import plugin from 'windicss/plugin'

export default defineConfig({
  darkMode: 'class', // ou 'media'
  theme: {
    extend: {
      screens: {
        'sm': '640px',
        'md': '768px',
        'lg': '1024px',
        'xl': '1280px',
        '2xl': '1536px',
      },
      colors: {
        blue: colors.sky,
        red: colors.rose,
        pink: colors.fuchsia,
      },
      fontFamily: {
        sans: ['Graphik', 'sans-serif'],
        serif: ['Merriweather', 'serif'],
      },
      spacing: {
        128: '32rem',
        144: '36rem',
      },
      borderRadius: {
        '4xl': '2rem',
      },
    },
  },
  plugins: [
    plugin(({ addUtilities }) => {
      const newUtilities = {
        '.skew-10deg': {
          transform: 'skewY(-10deg)',
        },
        '.skew-15deg': {
          transform: 'skewY(-15deg)',
        },
      }
      addUtilities(newUtilities)
    }),
    plugin(({ addComponents }) => {
      const buttons = {
        '.btn': {
          padding: '.5rem 1rem',
          borderRadius: '.25rem',
          fontWeight: '600',
        },
        '.btn-blue': {
          'backgroundColor': '#3490dc',
          'color': '#fff',
          '&:hover': {
            backgroundColor: '#2779bd',
          },
        },
        '.btn-red': {
          'backgroundColor': '#e3342f',
          'color': '#fff',
          '&:hover': {
            backgroundColor: '#cc1f1a',
          },
        },
      }
      addComponents(buttons)
    }),
    plugin(({ addDynamic, variants }) => {
      addDynamic('skew', ({ Utility, Style }) => {
        return Utility.handler
          .handleStatic(Style('skew'))
          .handleNumber(0, 360, 'int', number => `skewY(-${number}deg)`)
          .createProperty('transform')
      }, variants('skew'))
    }),
    require('windicss/plugin/filters'),
    require('windicss/plugin/forms'),
    require('windicss/plugin/aspect-ratio'),
    require('windicss/plugin/line-clamp'),
    require('windicss/plugin/typography')({
      modifiers: ['DEFAULT', 'sm', 'lg', 'red'],
    }),
  ],
})
```

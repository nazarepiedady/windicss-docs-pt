<Logo name="gridsome" class="logo-float-xl"/>

# Integração para a [Gridsome](https://gridsome.org/)

<PackageInfo name="gridsome-plugin-windicss" author="harlan-zw" />

## Instalar

```bash
yarn add gridsome-plugin-windicss -D
# npm i gridsome-plugin-windicss -D
```

:warning: Este módulo é um pré-lançamento, reporte qualquer [problemas](https://github.com/windicss/gridsome-plugin-windicss/issues) que encontrares.

## Uso

Dentro do teu ficheiro `gridsome.config.js` adicione o seguinte.

```js gridsome.config.js
export default {
  // ...
  plugins: [
    {
      use: 'gridsome-plugin-windicss',
      options: {
        // see https://github.com/windicss/vite-plugin-windicss/blob/main/packages/plugin-utils/src/options.ts
      },
    },
  ],
}
```

Este módulo não funcionará com a extensão `gridsome-plugin-tailwindcss`, precisarás de removê-lo.

```diff
 plugins: [
    {
-      use: 'gridsome-plugin-tailwindcss',
-      options: {
-        // ...
-      }
    },
  ],
```

Se tiveres um ficheiro `tailwind.config.js`, renomeie-o para `windi.config.js` ou `windi.config.ts`.

Consulte [esta ligação](https://windicss.netlify.app/guide/configuration.html) para mais detalhes de configuração.

## Migração

Se estavas anteriormente a usar a `gridsome-plugin-tailwindcss`, consulte a [documentação](https://windicss.netlify.app/guide/migration.html) sobre a migração.

## Configuração

- Padrão:

```js
export default {
  scan: {
    dirs: ['./'],
    exclude: [
      'node_modules',
      '.git',
      'dist',
      '.cache',
      '*.template.html',
      'app.html',
    ],
    include: [],
  },
  transformCSS: 'pre',
  preflight: {
    alias: {
      // adicionar pseudónimos da gridsome
      'g-link': 'a',
      'g-image': 'img',
    },
  },
}
```

- Consulte o [options.ts](https://github.com/windicss/vite-plugin-windicss/blob/main/packages/plugin-utils/src/options.ts) pela referência de configuração.

### Exemplos

#### Desativar o Preflight

_gridsome.config.js_

```js
export default {
  // ...
  plugins: [
    {
      use: 'gridsome-plugin-windicss',
      options: {
        preflight: false,
      },
    },
  ],
}
```

## Advertências

### Estilo Limitado

A diretiva `@media` com o estilo limitado pode **apenas funcionar** com `css`, `postcss`, `scss` mas não `sass`, `less` nem com a `stylus`.

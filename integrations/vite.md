[comparação de velocidade]: https://twitter.com/antfu7/status/1361398324587163648
[diretivas de CSS]: /features/directives
[utilitários de classes]: /utilities/
[migração]: /guide/migration

<Logo name="vite" class="logo-float-xl"/>

# Integração para a [Vite](https://vitejs.dev)

<PackageInfo name="vite-plugin-windicss" author="antfu" />

## Funcionalidades

- ⚡️ **É RÁPIDA** - 20~100x vezes mais rápida do que a Tailwind sobre a Vite
- 🧩 Utilitários de CSS sobre demanda (Completamente compatível com a Tailwind CSS v2)
- 📦 Redefinição de estilo de elementos nativos sobre demanda (preflight)
- 🔥 Substituição de módulo instantânea (HMR, sigla em Inglês)
- 🍃 Carrega as configurações a partir do `tailwind.config.js`
- 🤝 Agnóstica de abstração - Vue, React, Svelte e Vanilla!
- 📄 Transformações de diretivas `@apply` / `@screen` de CSS (também funcionam para `<style>` dos Componentes de Ficheiro Único de Vue)
- 🎳 Suporta para grupos de variante - por exemplo, `bg-gray-200 hover:(bg-gray-100 text-red-300)`
- 😎 ["Desenhe nas Ferramentas de Programação"](#desenhe-nas-ferramentas-de-programação) - se trabalhas desta maneira na Tailwind tradicional, não existe razão de não podermos!

> Consulte a [comparação de velocidade] entre a Windi CSS e a Tailwind CSS sobre a Vite.

## Instalar

Instale o pacote:

```bash
npm i -D vite-plugin-windicss windicss
```

Depois, instale a extensão na tua configuração da Vite:

```ts vite.config.js
import WindiCSS from 'vite-plugin-windicss'

export default {
  plugins: [
    WindiCSS(),
  ],
}
```

E finalmente, importe `virtual:windi.css` nas tuas entradas de Vite:

```js main.js
import 'virtual:windi.css'
```

E é isto! Comece a usar os [utilitários de classes] ou [diretivas de CSS] na tua aplicação, e desfrute da velocidade! ⚡️

> Se estiveres a migrar da Tailwind CSS, consulte também a [_seção de Migração_][migração]

## Suportes

### TypeScript

Ativar a TypeScript para o teu `windi.config.js`? Claro, porquê não?

Mude o seu nome para `windi.config.ts` e as coisas já funcionam!

```ts windi.config.ts
import { defineConfig } from 'windicss/helpers'
import formsPlugin from 'windicss/plugin/forms'

export default defineConfig({
  darkMode: 'class',
  safelist: 'p-3 p-4 p-5',
  theme: {
    extend: {
      colors: {
        teal: {
          100: '#096',
        },
      },
    },
  },
  plugins: [formsPlugin],
})
```

### Suporte ao Pug

Ela ativará automaticamente o suporte ao Pug para `.pug` e `.vue` quando a dependência `pug` for encontrada na área de trabalho. 

### "Desenhe nas Ferramentas de Programação"

It might be a common practice when you use the purge-based Tailwind where you have all the classes in your browser and you can try how things work by directly changing the classes in DevTools. While you might think this is some kind of limitation of "on-demand" where the DevTools don't know those you haven't used in your source code yet.

But unfortunately, **we are here to BREAK the limitation** 😎 See the [video demo](https://twitter.com/antfu7/status/1372244287975387145).

Just add the following line to your main entry

```js
import 'virtual:windi-devtools'
```

It will be enabled automatically for you, have fun!

Oh, and don't worry about the final bundle, in production build `virtual:windi-devtools` will be an empty module and you don't have to do anything about it :)

> ⚠️ Please use it with caution, under the hood we use [MutationObserver](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) to detect the class changes. Which means not only your manual changes but also the changes made by your scripts will be detected and included in the stylesheet. This could cause some misalignment between dev and the production build when **using dynamically constructed classes** (false-positive). We recommended adding your dynamic parts to the `safelist` or setup UI regression tests for your production build if possible.

## Configuration

### Preflight (style resetting)

Preflight is enabled on-demand. If you'd like to completely disable it, you can configure it as below

```ts windi.config.ts
import { defineConfig } from 'vite-plugin-windicss'

export default defineConfig({
  preflight: false,
})
```

### Safelist

By default, we scan your source code statically and find all the usages of the utilities then generated corresponding CSS on-demand. However, there is some limitation that utilities that decided in the runtime can not be matched efficiently, for example

```html
<!-- will not be detected -->
<div className={`p-${size}`}>
```

For that, you will need to specify the possible combinations in the `safelist` options of `windi.config.ts`.

```ts windi.config.ts
import { defineConfig } from 'vite-plugin-windicss'

export default defineConfig({
  safelist: 'p-1 p-2 p-3 p-4',
})
```

Or you can do it this way

```ts windi.config.ts
import { defineConfig } from 'vite-plugin-windicss'

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

### Scanning

On server start, `vite-plugin-windicss` will scan your source code and extract the utility usages. By default,
only files under `src/` with extensions `vue, html, mdx, pug, jsx, tsx` will be included. If you want to enable scanning for other file types of locations, you can configure it via:

```ts windi.config.js
import { defineConfig } from 'windicss/helpers'

export default defineConfig({
  extract: {
    include: ['src/**/*.{vue,html,jsx,tsx}'],
    exclude: ['node_modules', '.git'],
  },
})
```

Or in plugin options:

```ts vite.config.js
import { defineConfig } from 'vite'

export default defineConfig({
  plugins: [
    WindiCSS({
      scan: {
        dirs: ['.'], // all files in the cwd
        fileExtensions: ['vue', 'js', 'ts'], // also enabled scanning for js/ts
      },
    }),
  ],
})
```


### [Attributify Mode](https://windicss.org/posts/v30.html#attributify-mode)

Enabled it by

```ts windi.config.ts
export default {
  attributify: true,
}
```

And use them as you would like:

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

#### Prefix

If you are concerned about naming confliction, you can add custom prefix to attributify mode by:

```ts windi.config.ts
export default {
  attributify: {
    prefix: 'w:',
  },
}
```

```html
<button
  w:bg="blue-400 hover:blue-500 dark:blue-500 dark:hover:blue-600"
  w:text="sm white"
  w:font="mono light"
  w:p="y-2 x-4"
  w:border="2 rounded blue-200"
>
  Button
</button>
```

### Alias Config

Be aware, alias entries need to be prefixed with * when used, eg:
```html
<div class="*hstack">
```
See [this release post](https://windicss.org/posts/v30.html#alias-config) for the difference between shortcuts and alias.

```ts windi.config.ts
export default {
  alias: {
    'hstack': 'flex items-center',
    'vstack': 'flex flex-col',
    'icon': 'w-6 h-6 fill-current',
    'app': 'text-red',
    'app-border': 'border-gray-200 dark:border-dark-300',
  },
}
```

### Layers Ordering

> Supported from v0.14.x

By default, importing `virtual:windi.css` will import all the three layers with the order `base - components - utilities`. If you want to have better controls over the orders, you can separate them by:

```diff
- import 'virtual:windi.css'
+ import 'virtual:windi-base.css'
+ import 'virtual:windi-components.css'
+ import 'virtual:windi-utilities.css'
```

You can also make your custom css been able to be overridden by certain layers:

```diff
  import 'virtual:windi-base.css'
  import 'virtual:windi-components.css'
+ import './my-style.css'
  import 'virtual:windi-utilities.css'
```

### More

See [options.ts](https://github.com/windicss/vite-plugin-windicss/blob/main/packages/plugin-utils/src/options.ts) for more configuration reference.

## Caveats

### Scoped Style

You will need to **set `transformCSS: 'pre'` to get Scoped Style work**.

`@media` directive with scoped style can **only works** with `css` `postcss` `scss` but not `sass`, `less` nor `stylus`

## Example

See [examples](https://github.com/windicss/vite-plugin-windicss/blob/main/examples) for *react*, *vue* and *vue with pug* sample projects, or [`Vitesse`](https://github.com/antfu/vitesse)

---

## SvelteKit (as of 1.0.1)

Install plugin with `npm i -D vite-plugin-windicss` and edit the `vite.config.js` file:

```diff
import { sveltekit } from '@sveltejs/kit/vite';
+ import WindiCSS from 'vite-plugin-windicss'

/** @type {import('vite').UserConfig} */
const config = {
  plugins: [
    sveltekit(),
+   WindiCSS()
  ]
};

export default config;
```

Add `import "virtual:windi.css"` to the top of your `+layout.svelte` file:

```html +layout.svelte
<script>
  import "virtual:windi.css"

  // if you want to enable windi devtools
  import { browser } from "$app/env";
  if (browser) import("virtual:windi-devtools")
  // ...
</script>
<!-- ...rest of +layout.svelte -->
```

[auto]: /features/value-auto-infer
[design]: /posts/story

# Migrar da Tailwind CSS

## Pacotes

Algumas das tuas dependências já não são obrigatórias. Tu podes removê-las do teu `package.json` se elas eram apenas necessárias para a Tailwind CSS.

```diff package.json
- "tailwindcss": "*",
- "postcss": "*",
- "autoprefixer": "*",
+ "windicss": "*"
```

## Estilos de Base

Tu podes agora remover as importações da Tailwind CSS do teu ficheiro CSS de entrada.

```diff
- @import 'tailwindcss/base';
- @import 'tailwindcss/components';
- @import 'tailwindcss/utilities';
```

(Opcional) Baseado nas ferramentas de integração que estiveres a usar, podes precisar de importar a entrada `virtual:windi.css` explicitamente. Consulte a documentação das ferramentas para mais detalhes.

```js main.js
import 'virtual:windi.css'
```

## Configurações

Já que todas as variantes são [ativadas automaticamente][auto], os campos `variant` e `purge` já não são necessários.

Os `colors` e `plugins` precisam ser importados a partir de `windicss`.

Nós somos compatíveis com ambos `windi.config.js` e `tailwind.config.js`.

```diff windi.config.js
-const colors = require('tailwindcss/colors')
+const colors = require('windicss/colors')
-const typography = require('@tailwindcss/typography')
+const typography = require('windicss/plugin/typography')

export default {
- purge: {
-   content: [
-     './**/*.html',
-   ],
-   options: {
-     safelist: ['prose', 'prose-sm', 'm-auto'],
-   },
- },
- variants: {
-   extend: {
-     cursor: ['disabled'],
-   }
- },
+ extract: {
+   include: ['./**/*.html'],
+ },
+ safelist: ['prose', 'prose-sm', 'm-auto'],
  darkMode: 'class',
  plugins: [typography],
  theme: {
    extend: {
      colors: {
        teal: colors.teal,
      },
    }
  },
}
```

## Limpeza (opcional)

Os seguintes ficheiros podem ser removidos se não usas outras das suas funcionalidades.

```diff
- postcss.config.js
```

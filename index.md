---
home: true

heroText: Windi CSS

actionText: Começar 
actionLink: /guide/

altActionText: Saiba Mais
altActionLink: /features/

heroImage: /assets/logo.svg
heroAlt: Logótipo da Windi CSS

newsTitle: 👉 Consulte as novas funcionalidades da Windi CSS v3.4
newsLink: /posts/v34.html

footer: Licenciada sob MIT | Copyright © 2020-2021 Colaboradores da Windi CSS
---

<Sponsors />

<InlinePlayground
  :input="`bg-gradient-to-r from-green-400 to-blue-500
text-white text-center italic
px-4 py-2 rounded cursor-default
transition-all duration-400
hover:rounded-2xl
dark:\(from-teal-400 to-yellow-500)`"
  :showCSS="true"
  :showMode="true"
  :showTabs="true"
  :showConfig="false"
  :enableConfig="true"
  :config="{
  shortcuts: {
    btn: 'py-2 px-4 font-semibold rounded-lg shadow-md',
    'btn-green': 'text-white bg-green-500 hover:bg-green-700',
  },
  theme: {
    extend: {
      colors: {
        primary: '#0ea5e9'
      }
    }
  }
}"
/>

<p class="flex justify-center opacity-75 mt-12">
  <a href="https://www.netlify.com" rel="noreferrer" target="_blank">
    <img src="/assets/netlify.svg" alt="Implementações em Produção pela Netlify">
  </a>
</p>

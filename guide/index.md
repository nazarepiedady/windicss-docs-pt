[tailwind css]: https://tailwindcss.com/docs
[tailwind css v2]: https://blog.tailwindcss.com/tailwindcss-v2
[discussões]: https://github.com/windicss/windicss/discussions
[Problemas da GitHub]: https://github.com/windicss/windicss/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc
[Discussões da GitHub]: https://github.com/windicss/windicss/discussions
[autoprefixer]: https://autoprefixer.github.io/
[referência de utilitários]: /utilities/
[utilitários]: /utilities/
[diretivas]: /features/directives

[comparação em vídeo]: https://twitter.com/antfu7/status/1361398324587163648
[opções]: /guide/configuration
[funcionalidades]: /features/

# Começar

A **Windi CSS** é uma abstração de CSS de próxima geração focada em utilitário.

Se já estiveres familiarizado com a [Tailwind CSS], pense da Windi CSS como uma alternativa **sobre demanda** para a Tailwind, que fornece tempos de carregamento mais rápido, **compatibilidade completa com a Tailwind v2.0**, e um grupo de funcionalidades fantásticas.

## Porquê a Windi CSS?

Uma citação da parte do autor deve ilustrar a sua motivação para criar a Windi CSS:

> Quando o meu projeto tornou-se muito grande e havia montes de componentes, o tempo alcançado pela compilação inicial era 3s, e atualizações instantâneas demoravam mais de 1s com a Tailwind CSS.
> \- [@voorjaar](https://github.com/voorjaar)

Ao examinar o teu HTML e CSS e gerar os utilitários sobre demanda, a Windi CSS é capaz de fornecer [tempos de carregamento mais rápidos][comparação em vídeo] e um atualização instantânea mais rápida em desenvolvimento, e não requer purificação em produção.

## Uso Básico

Todos [utilitários] de [Tailwind CSS] são suportados na Windi CSS sem qualquer configuração extra.

Tu podes usar as classes de utilitário nos teus componentes e folhas de estilos como de costume:

```html
<div class="py-8 px-8 max-w-sm mx-auto bg-white rounded-xl shadow-md space-y-2 sm:(py-4 flex items-center space-y-0 space-x-6)">
  <img class="block mx-auto h-24 rounded-full sm:(mx-0 flex-shrink-0)" src="/img/erin-lindford.jpg" alt="Woman's Face" />
  <div class="text-center space-y-2 sm:text-left">
    <div class="space-y-0.5">
      <p class="text-lg text-black font-semibold">Erin Lindford</p>
      <p class="text-gray-500 font-medium">Product Engineer</p>
    </div>
    <button class="px-4 py-1 text-sm text-purple-600 font-semibold rounded-full border border-purple-200 hover:(text-white bg-purple-600 border-transparent) focus:(outline-none ring-2 ring-purple-600 ring-offset-2)">
      Message
    </button>
  </div>
</div>
```

**Só os utilitários que usares gerarão o CSS correspondente.**

## Integrações

Nós fornecemos **integrações de primeira classe** para as tuas ferramentas favoritas com a melhor experiência de programação em cada um deles. Consulte os [guias de integração](/guide/installation) para começar!

## Funcionalidades

O Windi CSS oferece algumas excelentes funcionalidades em adição à tudo que está incluído na [Tailwind CSS v2][tailwind css v2]. Consulte o [próximo capítulo][funcionalidades] para mais detalhes.

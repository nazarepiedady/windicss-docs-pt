<Logo name="vscode" class="logo-float-xl"/>

# Sensor Inteligente da Windi CSS para o VS Code

<PackageInfo name="windicss-intellisense" :hideNpm="true" author="voorjaar" />

O sensor inteligente da Windi CSS melhora a experiência de programação da Windi ao fornecer aos utilizadores de Visual Studio Code funcionalidades avançadas tais como conclusão automática da escrita de código, destacamento da sintaxe, articulação de código, e construção.

## Instalação

**[Instalar através do Mercado do Visual Studio Code →](https://marketplace.visualstudio.com/items?itemName=voorjaar.windicss-intellisense)**

**[Instalar através da Abertura do Registo de VSX →](https://open-vsx.org/extension/voorjaar/windicss-intellisense)**

Esta extensão empacota um compilador de windicss, assim podes usá-lo sem a instalação da windicss, e também suporta o ficheiro de configuração `(tailwind|windi).config.(js|cjs|ts)`.

## Funcionalidades

### Conclusão Automática da Escrita de Código

Sugestões inteligentes para os utilitários e variantes.

<img src="https://raw.githubusercontent.com/windicss/windicss-intellisense/main/screenshots/completion.png" alt="Conclusão Automática da Escrita de Código"/>

### Pré-visualização ao Pairar

Veja o CSS completo para um nome de classe ao pairar o ponteiro do rato sobre ela.

<img src="https://raw.githubusercontent.com/windicss/windicss-intellisense/main/screenshots/hover.png" alt="Pré-visualização ao Pairar"/>

### Destacamento de Sintaxe

Destaque os utilitários, variantes e os importantes.

<img src="https://raw.githubusercontent.com/windicss/windicss-intellisense/main/screenshots/highlight.png" alt="Destacamento de Sintaxe"/>

### Pré-visualização de Cor

Pré-visualize a color e o espetro.

<img src="https://raw.githubusercontent.com/windicss/windicss-intellisense/main/screenshots/color.png" alt="Pré-visualização de Cor
"/>

### Articulação de Código

Enrole as classes demasiadamente longas para aumentar a legibilidade.

<img src="https://raw.githubusercontent.com/windicss/windicss-intellisense/main/screenshots/highlight.png" alt="Articulação de Código"/>

### Comandos de Compilação

Comandos embutidos, operação uma tecla.

<img src="https://raw.githubusercontent.com/windicss/windicss-intellisense/main/screenshots/commands.png" alt="Comandos de Compilação"/>

## Definições da Extensão

| Definições                           | Tipo    | Valor Predefinido  | Descrição                                                  |
| :--------------------------------- | :------ | :------- | :----------------------------------------------------------- |
| `windicss.enableColorDecorators`   | boolean | true     | Ativar os Decoradores de Cor.                                     |
| `windicss.enableHoverPreview`      | boolean | true     | Ativar o pairar sobre o nome da classe para pré-visualizar a CSS.               |
| `windicss.enableCodeCompletion`    | boolean | true     | Ativar/Desativar todas conclusões automáticas de escrita de código.                         |
| `windicss.enableUtilityCompletion` | boolean | true     | Ativar a Conclusão de Utilitário.                                   |
| `windicss.enableVariantCompletion` | boolean | true     | Ativar a Conclusão de Variante.                                   |
| `windicss.enableDynamicCompletion` | boolean | true     | Ativar Conclusão Utilitários Dinâmicos como p-${int}.           |
| `windicss.enableRemToPxPreview`    | boolean | true     | Ativar a pré-visualização da conversão de REM para PX.                                    |
| `windicss.enableCodeFolding`       | boolean | true     | Ativar a Articulação de Código de Nomes de Classe.                             |
| `windicss.foldByLength`            | boolean | false    | Código articulado por comprimento. A opção padrão é falsa, enrolará pela contagem utilitário. |
| `windicss.foldCount`               | number  | 3        | Usado pelo `foldByCount`.                                         |
| `windicss.foldLength`              | number  | 25       | Usado pelo `foldByLength`.                                         |
| `windicss.hiddenText`              | string  | ` ...`   | Segurador de Espaço usado quando articula-se o código.                          |
| `windicss.hiddenTextColor`         | string  | \#AED0A4 | Cor do Segurador de Espaço.                                          |
| `windicss.sortOnSave`              | boolean | false    | Executa o ordenamento de classe sobre o guardar do ficheiro.                             |

## Para Mais Informações

* [Windi CSS](https://github.com/windicss/windicss)
* [Documentação](https://windicss.org)
* [Discussões](https://github.com/windicss/windicss/discussions)
* [Questões](https://github.com/windicss/windicss-intellisense/issues)

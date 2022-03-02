---
layout: article-detail
title:  Пользовательские темы
category: "Плагины"
category-url: plugins
---

Создайте собственную тему Insomnia, создав [плагин](/insomnia/introduction-to-plugins/).

{:.alert .alert-primary}
**Note**: Чтобы начать работу с некоторыми живыми примерами, см. наш встроенный модуль [insomnia-plugin-themes](https://github.com/Kong/insomnia/tree/develop/plugins/insomnia-plugin-core-themes).

## Как создать собственную тему

1. Создайте новый проект для своей темы. Если вы планируете сделать эту тему доступной через NPM, начните название проекта с `insomnia-plugin-`. Следующий пример называется `insomnia-plugin-dark-colorblind-theme`.

2. Следуя инструкциям плагина, напишите свой [плагин package.json](https://docs.insomnia.rest/insomnia/introduction-to-plugins#plugin-packagejson). В файле ввода экспортируйте свои темы.

3. Начните с базового шаблона. Каждый раздел конфигурации имеет свойства `background`, `foreground` и `highlight` с изменяемыми свойствами цвета.

  Дополнительные сведения о том, какие имена свойств цвета соответствуют элементам пользовательского интерфейса, смотрите в комментариях к коду.

  {:.alert .alert-primary}
  **Note**: Приведенные ниже комментарии к коду не являются исчерпывающими, и стили могут применяться в других местах.

```ts
module.exports.themes = [{
  name:        'dark-colorblind', // theme name in kebab-case
  displayName: 'Dark Colorblind', // formatted theme name
  theme: {
    // Background, foreground, and highlight values nested directly in the theme 
    // object will serve as the default overwrites for all properties you add.
    background: {
      default:    '#21262D',  // primary background color
      success:    '#1F6FEB',  // POST request, 200 OK, parameter names
      notice:     '#E8F086',  // PATCH request
      warning:    '#A691AE',  // PUT request
      danger:     '#FF4242',  // DELETE request
      surprise:   '#FFC20A',  // accent (Dashboard link, GET request, 
                              // SEND button, branch button, add plugin button)
      info:       '#58A6FF'   // OPTIONS AND HEAD request
    },
    foreground: {
      default:     '#fff',    // primary font color
      success:     '#fff',    // secondary font color for success background
      notice:      '#000',    // secondary font color for notice background
      warning:     '#fff',    // secondary font color for warning background
      danger:      '#fff',    // secondary font color for danger background
      surprise:    '#000',    // secondary font color for surprise background
      info:        '#000'     // secondary font color for info background
    },
    highlight: {
      default: '#D3D3D3'      // sidebar highlight color
    },
    // The styles object targets sub-components of the Insomnia application.
    styles: {
      appHeader: {
        foreground: {
          surprise:   '#000'  // header branch button font color
        }
      },
      paneHeader: {
        foreground: {
          surprise:   '#000', // pane accent font color
          info:       '#000'  // pane response font color
        }
      },
      editor: {
        foreground: {
          default:    '#000', // primary editor font color
          surprise:   '#000', // editor accent font color
          info:       '#000'  // editor response font color
        }
      },
      dialog: {
        background: {
          default:    '#2E4052' // modal primary background color
        },
        foreground: {
          default:    '#fff'    // modal primary font color
        }
      }
    }
  }
}]
```

## Параметры стилей

Доступны следующие свойства `style`. Каждый из них может содержать свои собственные свойства `background`, `foreground` и `highlight`.

* appHeader
* dialog
* dialogFooter
* dialogHeader
* dropdown
* editor
* link
* overlay
* pane
* paneHeader
* sidebar
* sidebarHeader
* sidebarList
* tooltip
* transparentOverlay

## Пользовательские CSS

В дополнение к базовым изменениям и подкомпонентам вы можете добавить пользовательский CSS, используя свойство `rawCss`.

{:.alert .alert-primary}
**Note**: Как правило, лучше использовать предопределенные свойства для настройки темы, а не использовать пользовательский CSS. Это гарантирует, что ваша тема не сломается в будущем, когда мы внесем изменения в Insomnia.

```ts
module.exports.themes = [{
  name: 'my-dark-theme',
  displayName: 'My Dark Theme',
  theme: {
      rawCss: `
      .tooltip, .dropdown__menu {
        opacity: 0.95;
      }
    `,
  },
}];
```

## Схема плагина темы

Следующее определяет схему плагина темы. Если свойство не установлено, оно будет использовать Insomnia по умолчанию.

```ts
interface ThemeBlock {
  background?: {
    default?: string,
    success?: string,
    notice?: string,
    warning?: string,
    danger?: string,
    surprise?: string,
    info?: string,
  },
  foreground?: {
    default?: string,
    success?: string,
    notice?: string,
    warning?: string,
    danger?: string,
    surprise?: string,
    info?: string,
  },
  highlight?: {
    default: string,
    xxs?: string,
    xs?: string,
    sm?: string,
    md?: string,
    lg?: string,
    xl?: string,
  },
};

interface ThemeInner extends ThemeBlock {
  rawCss?: string,
  styles: ?{
    dialog?: ThemeBlock,
    dialogFooter?: ThemeBlock,
    dialogHeader?: ThemeBlock,
    dropdown?: ThemeBlock,
    editor?: ThemeBlock,
    link?: ThemeBlock,
    overlay?: ThemeBlock,
    pane?: ThemeBlock,
    paneHeader?: ThemeBlock,
    sidebar?: ThemeBlock,
    sidebarHeader?: ThemeBlock,
    sidebarList?: ThemeBlock,
    tooltip?: ThemeBlock,
    transparentOverlay?: ThemeBlock,
  },
};
```

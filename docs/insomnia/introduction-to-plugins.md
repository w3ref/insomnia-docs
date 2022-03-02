---
layout: article-detail
title:  Введение в плагины
category: "Плагины"
category-url: plugins
---

В этом разделе представлен обзор плагинов Insomnia, которые можно использовать для расширения функциональности Insomnia. Плагины обычно используются, когда требуется более сложное поведение, например настраиваемые механизмы аутентификации и сложные рабочие процессы.

Вы можете создать свой собственный плагин Insomnia и загрузить его через **Настройки Insomnia** в приложении или через NPM. Как правило, плагины делают следующее:

* Добавьте [пользовательский тег шаблона](/insomnia/template-tags) для отображения пользовательских значений.
* Определите [хук](/insomnia/hooks-and-actions), который может делать такие вещи, как перехват запросов и ответов, чтобы добавить собственное поведение.

Просмотрите наши текущие плагины NPM сообщества на [Insomnia Plugin Hub](https://insomnia.rest/plugins).

## Добавить плагин

Чтобы добавить плагин Insomnia, перейдите в **Настройки**, представленные значком шестеренки в правом верхнем углу вашего приложения. Затем перейдите на вкладку **Плагины**. Введите название плагина, который вы хотите добавить, затем нажмите **Установить плагин**.

Вам не нужно перезагружать приложение для применения плагина после его добавления. Плагины включены по умолчанию.

## Отключить плагин

Чтобы отключить плагин Insomnia, перейдите в **Настройки**, представленные значком шестеренки в правом верхнем углу вашего приложения. Затем перейдите на вкладку **Плагины**. Переключите **Включить?** для плагина, который вы хотите отключить.

## Удалить плагин

Чтобы навсегда удалить плагин Insomnia, перейдите в следующую папку на своем компьютере и вручную удалите папку плагина:

* MacOS:   `~/Library/Application Support/Insomnia/plugins/` (экранированная версия: `~/Library/Application\ Support/Insomnia/plugins/`)
* Windows: `%APPDATA%\Insomnia\plugins\`
* Linux:   `$XDG_CONFIG_HOME/Insomnia/plugins/` или `~/.config/Insomnia/plugins/`

Вы всегда можете [повторно добавить плагин](#add-a-plugin), если он еще доступен.

## Создать плагин

Плагин Insomnia — это модуль NodeJS, который размещается в определенном каталоге, распознаваемом Insomnia.

### Местоположение файла плагина

Чтобы Insomnia распознала ваш плагин как плагин Insomnia, ваши файлы должны находиться в следующих местах:

* MacOS:   `~/Library/Application Support/Insomnia/plugins/` (экранированная версия: `~/Library/Application\ Support/Insomnia/plugins/`)
* Windows: `%APPDATA%\Insomnia\plugins\`
* Linux:   `$XDG_CONFIG_HOME/Insomnia/plugins/` или `~/.config/Insomnia/plugins/`

{:.alert .alert-primary}
**Note**: Чтобы быстро создать плагин по правильному пути со стартовыми файлами через приложение Insomnia, перейдите в **Настройки** и щелкните вкладку **Плагины**. Нажмите **Создать новый плагин** и введите имя вашего плагина. Если вы не добавите заголовок **insomnia-plugin-**, он будет добавлен автоматически.

### Структура файла плагина

Для каталога плагинов Insomnia требуется как минимум два файла. В следующем примере заголовок плагина `base64` и содержит файлы `package.json` и `app.js`.

```bash
base64/             
 ├── package.json   # Метаданные модуля узла
 └── app.js         # Один или несколько файлов JavaScript
```

### Плагин package.json

Конфигурация `package.json` включает следующее содержимое:

* Общие
  * name (должно начинаться с `insomnia-plugin-`)
  * version
  * main (точка входа в файл)
* Метаданные о Insomnia
* Дополнительные метаданные плагина
* Внешние зависимости

Ниже приведен пример минимального `package.json`. `package.json` должен содержать атрибут `insomnia`, чтобы его можно было идентифицировать как плагин Insomnia.

```json-doc
{
  "name": "insomnia-plugin-base64", // NPM module name, must be prepended with insomnia-plugin-
  "version": "1.0.0",               // Plugin version
  "main": "app.js",                 // Entry point
  
  // Insomnia-specific metadata. Without this, Insomnia won't recognize the module as a plugin.
  "insomnia": {                    
    "name": "base64",                                                       // Internal Insomnia plugin name
    "displayName": "base64 Plugin",                                         // Plugin display name
    "description": "The base64 plugin encodes and decodes basic strings.",  // Plugin description

    // Optional plugin metadata

    // Plugin images for Plugin Hub and other interfaces
    "images": {
      // Plugin Icon
      // Suggested filetype: SVG (for scaling)
      // Suggested dimensions: 48x48
      "icon": "icon.svg", // relative path, relative to package root

      // Plugin Cover Image
      // Suggested filetype: SVG (for scaling)
      // Suggested dimensions: 952w x 398h
      "cover": "cover.svg", // relative path, relative to package root
    },

    // Force plugin hub and other entities to show specific author details
    // Useful for teams and organizations who work on the same plugin
    "publisher": {
      "name": "YOUR NAME HERE", // Plugin publisher name, displayed on plugin hub
      "icon": "https://...",    // Plugin publisher avatar or icon, absolute url
    }
  },
  
  // External dependencies are also supported
  "dependencies": [],
  "devDependencies": []
}
```

## Управление плагинами

Вкладка **Плагины** в меню **Настройки** включает следующие функции:

* Добавить существующий плагин
* Создайте новый плагин локально
* Переключите плагин, чтобы включить или отключить его
* Узнайте точное местоположение локального плагина на вашем компьютере.

{:.alert .alert-primary}
**Note**: Плагины можно загрузить и установить непосредственно с [Insomnia Plugin Hub](https://insomnia.rest/plugins).

## Публикация плагинов

Прежде чем опубликовать свой плагин, убедитесь, что вы соответствуете следующим критериям, чтобы ваш плагин был распознан Insomnia и был доступен на [Центре плагинов Insomnia](https://insomnia.rest/plugins).

Ваш плагин должен:

* Включать правильно структурированный файл `package.json`, содержащий атрибут `insomnia`. Смотрите [Плагин package.json](#plugin-packagejson) для получения дополнительной информации.
* Иметь имя пакета с префиксом `insomnia-plugin-`.
* Быть общедоступным.

После того, как вы убедились, что ваш подключаемый модуль соответствует критериям, описанным выше, опубликуйте общедоступный подключаемый модуль, следуя [инструкциям NPM по публикации общедоступных пакетов с незаданной областью](https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages). Опубликуйте _незадействованный_ пакет, чтобы убедиться, что он появится в Центре подключаемых модулей Insomnia.

Если ваш пакет не появится в концентраторе плагинов Insomnia через несколько дней, [свяжитесь с нами](https://insomnia.rest/support), указав название вашего плагина и ссылку на опубликованный пакет NPM.

### Публикация плагинов с ограниченной областью действия

Insomnia также может использовать частные (ограниченные) плагины. Обычно это позволяет корпоративным пользователям скрывать плагин от Insomnia.

Чтобы включить частные (ограниченные) плагины:

1. Перейдите в каталог `plugins` в [Данные приложения](/insomnia/application-data).
2. Запустите `npm install @scoped/plugin`.

## Отладка в приложении Insomnia

Приложение Insomnia позволяет выполнять отладку с помощью Chrome DevTools. Чтобы открыть DevTools, нажмите **Просмотр**, а затем **Переключить DevTools**.

Если вы хотите сосредоточиться именно на разрабатываемом плагине, вы можете найти его на вкладке **Источники** и/или отфильтровать **Консоль** по имени файла плагина.

## Теги шаблона

Смотрите [Теги шаблона](/insomnia/template-tags) для получения дополнительной информации.

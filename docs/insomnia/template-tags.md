---
layout: article-detail
title:  Теги шаблона
category: "Плагины"
category-url: plugins
---

Теги шаблона тесно связаны с [переменными среды](/insomnia/environment-variables/). Основное различие между тегами шаблона и переменными среды заключается в том, что теги шаблона действуют больше как операции, а не как переменные.

Теги могут выполнять такие действия, как преобразование строк, генерация случайных чисел, обработка UUID и создание меток времени.

## Используйте теги шаблона

Чтобы вставить тег шаблона, нажмите CTRL+Пробел везде, где можно использовать [Переменные среды](/insomnia/environment-variables/). Теги шаблона появятся под Переменными среды в списке автозаполнения и помечены символом `ƒ`.

## Пользовательские теги шаблона

Вы можете захотеть расширить функциональность Insomnia с помощью настраиваемого поведения, и вы можете сделать это, создав собственный тег шаблона в виде [плагина Insomnia](/insomnia/introduction-to-plugins/). После того как вы добавите свой собственный подключаемый модуль в приложение Insomnia, тег шаблона будет отображаться точно так же, как если бы это был собственный тег Insomnia.

## Теги ответа и запроса

Теги ответа и запроса позволяют ссылаться на значения между ответами и запросами и из них.

### Тег ответа

Используйте тег ответа для ссылки на значения из других ответов или [цепочку запросов](/insomnia/chaining-requests).

Это может быть полезно при включении идентификатора созданного ресурса в запрос `GET` сразу после его создания с помощью запроса `POST`. Это также полезно при ссылке на повторно используемый токен входа из ответа в переменной среды.

### Тег запроса

Тег запроса полезен для ссылки на значения из текущего запроса.

Например, используйте тег запроса для извлечения маркера CSRF из файла cookie, чтобы вы могли использовать это значение в качестве значения формы или заголовка.

## Схема тега шаблона

В этом примере показано использование `TemplateTag` для отображения пользовательских значений.

```ts
interface RenderContext {
  // API not finalized yet
};

interface TemplateTag {
  name: string,
  displayName: DisplayName,
  disablePreview?: () => boolean,
  description?: string,
  deprecated?: boolean,
  liveDisplayName?: (args) => ?string,
  validate?: (value: any) => ?string,
  priority?: number,
  args: Array<{
    displayName: string,
    description?: string,
    defaultValue: string | number | boolean,
    type: 'string' | 'number' | 'enum' | 'model' | 'boolean',
    
    // Only type === 'string'
    placeholder?: string,

    // Only type === 'model'
    modelType: string,

    // Only type === 'enum'
    options: Array<{
      displayName: string,
      value: string,
      description?: string,
      placeholder?: string,
    }>,
  }>,
  actions: Array<{
    name: string,
    icon?: string,
    run?: (context) => Promise<void>,
  }>,
};
```

### Пример: Тег шаблона для генерации случайного целого числа

Этот пример тега шаблона генерирует случайное целое число от 0 до 100.

```ts
/**
 * Example template tag that generates a random number 
 * between a user-provided MIN and MAX
 */
module.exports.templateTags = [{
    name: 'randomInteger',
    displayName: 'Random Integer',
    description: 'Generate a random integer.',
    args: [
        {
            displayName: 'Minimum',
            description: 'Minimum potential value',
            type: 'number',
            defaultValue: 0
        }, 
        {
            displayName: 'Maximum',
            description: 'Maximum potential value',
            type: 'number',
            defaultValue: 100
        }
    ],
    async run (context, min, max) {
        return Math.round(min + Math.random() * (max - min));
    }
}];
```

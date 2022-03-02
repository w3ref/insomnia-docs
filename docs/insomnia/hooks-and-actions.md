---
layout: article-detail
title:  Хуки и действия
category: "Плагины"
category-url: plugins
---

Хуки и действия позволяют добавлять дополнительные функции в ваши пользовательские плагины с помощью запросов, ответов и взаимодействий с пользовательским интерфейсом. Хуки полагаются на запрос или ответ. Действия зависят от взаимодействия с пользовательским интерфейсом.

## Хуки запроса и ответа

Плагины могут реализовывать функции ловушек, которые вызываются, когда происходят определенные события. В настоящее время плагин может экспортировать два разных типа хуков: `RequestHook` и `ResponseHook`.

```ts
interface RequestHook {
    app: AppContext;
    request: Request;
};

interface ResponseHook {
    app: AppContext;
    response: Response;
}

// Hooks are exported as an array of hook functions that get 
// called with the appropriate plugin API context.
module.exports.requestHooks = Array<(context: RequestHook) => void>;
module.exports.responseHooks = Array<(context: ResponseHook) => void>;
```

## Запросить действия

Действия можно добавить в нижнюю часть раскрывающегося контекстного меню запроса (щелкните правой кнопкой мыши запрос на боковой панели), определив подключаемый модуль действия запроса.

```ts
interface RequestAction {
    label: string;
    action: (context: Context, models: { 
        requestGroup: RequestGroup;
        request: Request;
    }): void | Promise<void>;
    icon?: string;
};

// Request actions are exported as an array of objects
module.exports.requestActions = Array<RequestAction>
```

### Пример: Плагин для получения сведений о запросе в модальном окне.

Этот плагин добавляет параметр **Просмотреть данные запроса** в раскрывающееся меню, которое появляется, когда вы щелкаете правой кнопкой мыши запрос на боковой панели.

```ts
module.exports.requestActions = [
  {
    label: 'See request data',
    action: async (context, data) => {
      const { request } = data;
      const html = `<code>${JSON.stringify(request)}</code>`;
      context.app.showGenericModalDialog('Results', { html });
    },
  },
];
```

### Пример: Отправить запрос

Этот плагин добавляет опцию **Отправить запрос** в раскрывающееся меню, которое появляется, когда вы щелкаете правой кнопкой мыши запрос на боковой панели.

```ts
module.exports.requestActions = [
  {
    label: "Send request",
    action: async (context, data) => {
      const { request } = data;
      const response = await context.network.sendRequest(request);
      const html = `<code>${request.name}: ${response.statusCode}</code>`;
      context.app.showGenericModalDialog("Results", { html });
    },
  },
];
```

## Действия с папками

Действия можно добавить в нижнюю часть раскрывающегося меню папки, определив подключаемый модуль действия папки (группы запросов).

```ts
interface RequestGroupAction {
    label: string;
    action: (context: Context, models: { 
        requestGroup: RequestGroup; 
        requests: Array<Request>;
    }): Promise<void>;
};

// Folder actions are exported as an array of objects
module.exports.requestGroupActions = Array<RequestGroupAction>
```

### Пример: Плагин для отправки всех запросов в папку

Этот плагин добавляет опцию **Отправить запросы** в раскрывающееся меню, которое появляется, когда вы нажимаете на папку с запросами. **Отправить запросы** отправляет все запросы в этой папке одновременно.

```ts
module.exports.requestGroupActions = [
  {
    label: 'Send Requests',
    action: async (context, data) => {
      const { requests } = data;

      let results = [];
      for (const request of requests) {
        const response = await context.network.sendRequest(request);
        results.push(`<li>${request.name}: ${response.statusCode}</li>`);
      }

      const html = `<ul>${results.join('\n')}</ul>`;

      context.app.showGenericModalDialog('Results', { html });
    },
  },
];
```

## Действия в рабочей области

Действия можно добавить в раскрывающийся список настроек «Коллекция» или «Документ», определив подключаемый модуль действий рабочей области. Они применимы к обоим типам рабочих областей, коллекциям запросов и проектным документам.

{:.alert .alert-primary}
**Note**: «Рабочее пространство» — это имя в нашей кодовой базе, которое мы используем для обозначения как документов, так и коллекций.

```ts
interface WorkspaceAction {
    label: string;
    action: (context: Context, models: { 
        workspace: Workspace;
        requestGroup: Array<RequestGroup>,;
        requests: Array<Request>;
    }): Promise<void>;
};

// Workspace actions are exported as an array of objects
module.exports.workspaceActions = Array<WorkspaceAction>;
```

### Пример: Плагин для экспорта текущей рабочей области

Этот плагин добавляет пользовательскую опцию в раскрывающееся меню «Документ или коллекция», которая экспортирует текущий документ или коллекцию.

```ts
const fs = require('fs');

module.exports.workspaceActions = [{
  label: 'My Plugin Action',
  icon: 'fa-star',
  action: async (context, models) => {
    const ex = await context.data.export.insomnia({
      includePrivate: false,
      format: 'json',
      workspace: models.workspace,
    });

    fs.WriteFileSync('/users/user/Desktop/export.json', ex);
  },
}];
```

## Документ Действия

Действия можно добавить в контекстное меню карточки Dashboard для документа. Это действие не работает для коллекций.

```ts
interface DocumentAction {
    label: string,
    action: (context: Context, spec: 
        contents: Record<string, any>;
        rawContents: string;
        format: string;
        formatVersion: string;
    ): void | Promise<void>;
    hideAfterClick?: boolean;
};

// Document actions are exported as an array of objects
module.exports.documentActions = Array<DocumentAction>;
```

## Генератор конфигураций

Генераторы конфигурации отображаются в раскрывающемся списке «Параметры документа» и могут использоваться для создания конфигурации из спецификации OpenAPI.

```ts
interface ConfigGenerator {
    label: string;
    generate: (info: SpecInfo) => Promise<{ document?: string; error?: string; }>;
};

// Config generators are exported as an array of objects
module.exports.configGenerators = Array<ConfigGenerator>;
```

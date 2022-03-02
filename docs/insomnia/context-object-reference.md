---
layout: article-detail
title:  Справочник по контекстным объектам
category: "Плагины"
category-url: plugins
---

Этот документ является ссылкой на объект Context. Контекстные методы предоставляют подключаемым модулям помощников для связи, взаимодействия и интеграции с Insomnia. Например, их можно использовать для отображения предупреждения или изменения заголовка запроса.

## context.request

Контекст запроса содержит помощники для взаимодействия с запросом Insomnia.

```ts
interface RequestContext {
    getId(): string;
    getName(): string;
    getUrl(): string;
    setUrl(url: string): void;
    getMethod(): string;
    setMethod(method: string): void;
    getHeaders(): Array<{ name: string, value: string }>;
    getHeader(name: string): string | null;
    hasHeader(name: string): boolean;
    removeHeader(name: string): void;
    setHeader(name: string, value: string): void;
    addHeader(name: string, value: string): void;
    getParameter(name: string): string | null;
    getParameters(): Array<{name: string, value: string}>;
    setParameter(name: string, value: string): void;
    hasParameter(name: string): boolean;
    addParameter(name: string, value: string): void;
    removeParameter(name: string): void;
    getBody(): RequestBody;
    setBody(body: RequestBody): void;
    getEnvironmentVariable(name: string): any;
    getEnvironment(): Object;
    setAuthenticationParameter(name: string, value: string): void;
    getAuthentication(): Object;
    setCookie(name: string, value: string): void;
    settingSendCookies(enabled: boolean): void;
    settingStoreCookies(enabled: boolean): void;
    settingEncodeUrl(enabled: boolean): void;
    settingDisableRenderRequestBody(enabled: boolean): void;
    settingFollowRedirects(enabled: boolean): void;
};

interface RequestBody {
  mimeType?: string;
  text?: string;
  fileName?: string;
  params?: RequestBodyParameter[];
}

interface RequestBodyParameter {
  name: string;
  value: string;
  description?: string;
  disabled?: boolean;
  multiline?: string;
  id?: string;
  fileName?: string;
  type?: string;
}
```

### Пример: установка заголовка Content-Type для каждого POST-запроса.

```ts
// Request hook to set header on every request
module.exports.requestHooks = [
  context => {
    if (context.request.getMethod().toUpperCase() === 'POST') {
      context.request.setHeader('Content-Type', 'application/json');
    }
  }
];
```

### Пример: Изменить тело запроса

```ts
// Replace "FOO" with "BAR" within a request body before sending
const regexp = new RegExp(/FOO/, 'g');

module.exports.requestHooks = [
  context => {
    const body = context.request.getBody();
    context.request.setBody({
      ...body,
      text: body.text.replace(regexp, 'BAR'),
    });
  }
];
```

### Пример: переопределить тело запроса

```ts
module.exports.requestHooks = [
  context => {
    context.request.setBody({
      mimeType: 'application/json',
      text: JSON.stringify({ foo: 'bar' }),
    });
  }
];
```

## context.response

Контекст ответа содержит помощники для взаимодействия с ответом Insomnia.

```ts
interface ResponseContext {
    getRequestId(): string;
    getStatusCode(): number;
    getStatusMessage(): string;
    getBytesRead(): number;
    getTime(): number;
    getBody(): Buffer | null;
    getBodyStream(): Readable;
    setBody(body: Buffer);
    getHeader(name: string): string | Array<string> | null;
    getHeaders(): Array<{ name: string, value: string }> | undefined;
    hasHeader(name: string): boolean,
}

```

### Пример: Сохранить ответ в файл

В этом примере показано, как вы можете написать ответ в файл.

```ts
const fs = require('fs');

// Response hook to save response to file
module.exports.responseHooks = [
  context => {
   context.response.getBodyStream().pipe(
      fs.createWriteStream('/Users/gschier/Desktop/response-body.txt')
    );
  }
];
```

### Пример: изменение тела ответа

Тело ответа работает с [буферами NodeJS](https://nodejs.org/api/buffer.html). Чтобы изменить тело ответа с помощью плагина, вам нужно перевести его в буфер и обратно.

Пример ниже связан с `responseHooks` и показывает, как работать с буферами NodeJS для:

* Преобразование буфера в строку
* Разобрать строку в JS-объект
* Измените ответ:
   * Генерация случайного числа
   * Подсказка пользователю для получения информации в модальном режиме
* Преобразование объекта JS в строку, а затем в буфер

```ts
const bufferToJsonObj = buf => JSON.parse(buf.toString('utf-8'));
const jsonObjToBuffer = obj => Buffer.from(JSON.stringify(obj), 'utf-8');

module.exports.responseHooks = [
  async ctx => {
    try{
      const resp = bufferToJsonObj(ctx.response.getBody());
      
      // Modify
      resp.__randomNumber = Math.random();

      // If you want something from a user, use a prompt:
      const promptResult = await ctx.app.prompt('Type something', { defaultValue: 'abcd' });
      resp.__customValue = promptResult;

      ctx.response.setBody(jsonObjToBuffer(resp));
    } catch {
      // no-op
    }
  }
]
```
_В этом примере к ответу JSON добавляются свойства `__randomNumber` и `__customValue`. Обновите функциональность по мере необходимости._

## context.store

Плагины могут хранить постоянные данные через контекст хранилища. Данные доступны только тому плагину, который их сохранил.

```ts
interface StoreContext {
    hasItem(key: string): Promise<boolean>;
    setItem(key: string, value: string): Promise<void>;
    getItem(key: string): Promise<string | null>;
    removeItem(key: string): Promise<void>;
    clear(): Promise<void>;
    all(): Promise<Array<{ key: string, value: string }>>;
}
```

## context.app

Контекст приложения содержит общий набор помощников, которые являются глобальными для приложения.

```ts
interface AppContext {
    getInfo(): { version: string, platform: string };
    alert(title: string, message?: string): Promise<void>;

    dialog(title: string, body: HTMLElement, options?: {
        onHide?:() => void;
        tall?: boolean;
        skinny?: boolean;
        wide?: boolean;
    }): void;

    prompt(title: string, options?: {
        label?: string;
        defaultValue?: string;
        submitName?: string;
        cancelable?: boolean;
    }): Promise<string>;

    getPath(name: string): string;
    
    showSaveDialog(options?: {
        defaultPath?: string;
    }): Promise<string | null>;

    clipboard: {
      readText(): string;
      writeText(text: string): void;
      clear(): void;
    };
}
```

## context.data

Контекст данных содержит помощники, связанные с импортом и экспортом рабочих пространств Insomnia.

```ts
interface ImportOptions {
    workspaceId?: string;
    workspaceScope?: 'design' | 'collection';
}

interface DataContext {
    import: {
        uri(uri: string, options?: ImportOptions): Promise<void>;
        raw(text: string, options?: ImportOptions): Promise<void>;
    },
    export: {
        insomnia(options?: { 
            includePrivate?: boolean,
            format?: 'json' | 'yaml',
        }): Promise<string>;
        har(options?: { includePrivate?: boolean }): Promise<string>;
    }
}
```

## context.network

Сетевой контекст содержит помощники, связанные с отправкой сетевых запросов.

```ts
interface NetworkContext {
    sendRequest(request: Request): Promise<Response>;
}
```

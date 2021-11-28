---
layout: article-detail
title:  GraphQL для OpenAPI
category: "Дизайн API"
category-url: api-design
---

Insomnia имеет автоматическое обнаружение API-интерфейсов GraphQL, задокументированных через OpenAPI. Чтобы включить автоматическое определение, ваш GraphQL API должен быть задокументирован со следующими значениями перед генерацией запросов на отладку:

* Путь должен быть `/graphql`
* Метод должен быть POST
* Тело запроса должно быть application/json и должно содержать запрос свойства со строкой типа.
* Тело ответа должно быть application/json.

Ниже вы можете увидеть пример описанной конечной точки API GraphQL, которая будет запускать автоматическое обнаружение с предварительно заполненным телом запроса, которое будет отображаться в запросах режима отладки:

```
paths:
  /graphql:
    post:
      summary: 'My GraphQL API Endpoint'
      description: 'My GraphQL API Endpoint'
      operationId: 'graphql'
      responses:
        '200':
          description: 'Successfull Query'
          content: 
            application/json:
              schema:
                type: object
      requestBody:
        content:
          application/json:
            schema:
              type: object
              example: 
                query: >
                  {
                    allFilms {
                      films {
                        title
                      }
                    }
                  }
              properties:
                query:
                  type: string
```
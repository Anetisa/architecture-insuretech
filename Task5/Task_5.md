# Задание 5. Проектирование GraphQL API

**Контракт Swagger:**
```yaml
swagger: '2.0'
info:
 description: API сервиса управления клиентскими данными
 version: 1.0.0
 title: Клиентский Сервис
host: api.client-service.com
basePath: /v1
schemes:
 - https
paths:
 /clients/{id}:
   get:
     tags:
       - Клиент
     summary: Получить информацию о клиенте по ID
     description: Возвращает информацию о клиенте.
     produces:
       - application/json
     parameters:
       - name: id
         in: path
         description: ID клиента
         required: true
         type: string
     responses:
       '200':
         description: Успешный ответ
         schema:
           $ref: '#/definitions/Client'
 /clients/{id}/documents:
   get:
     tags:
       - Документы
     summary: Список документов клиента
     description: Возвращает список документов клиента по ID.
     produces:
       - application/json
     parameters:
       - name: id
         in: path
         description: ID клиента для поиска его документов
         required: true
         type: string
     responses:
       '200':
         description: Успешный ответ
         schema:
           type: array
           items:
             $ref: '#/definitions/Document'
 /clients/{id}/relatives:
   get:
     tags:
       - Родственники
     summary: Информация о родственниках клиента
     description: Возвращает информацию о родственниках клиента по ID.
     produces:
       - application/json
     parameters:
       - name: id
         in: path
         description: ID клиента
         required: true
         type: string
     responses:
       '200':
         description: Успешный ответ
         schema:
           type: array
           items:
             $ref: '#/definitions/Relative'
definitions:
 Client:
   type: object
   properties:
     id:
       type: string
     name:
       type: string
     age:
       type: integer
 Document:
   type: object
   properties:
     id:
       type: string
     type:
       type: string
     number:
       type: string
     issueDate:
       type: string
     expiryDate:
       type: string
 Relative:
   type: object
   properties:
     id:
       type: string
     relationType:
       type: string
     name:
       type: string
     age:
       type: integer
 
```


**Анализ Swagger (REST API):**

Ресурсы в API:
- GET /clients/{id} → общая информация о клиенте
- GET /clients/{id}/documents → список документов клиента
- GET /clients/{id}/relatives → родственники клиента

Объекты данных:
- Client: id, name, age
- Document: id, type, number, issueDate, expiryDate
- Relative: id, relationType, name, age

**Как это будет работать:**

Пример запроса 1 — только имя клиента:
```graphql
query {
  client(id: "123") {
    name
  }
}
```

Пример запроса 2 — клиент + документы:
```
query {
  client(id: "123") {
    name
    documents {
      type
      number
    }
  }
}
```

Пример запроса 3 — всё сразу:
```
query {
  client(id: "123") {
    id
    name
    age
    documents {
      type
      number
    }
    relatives {
      name
      relationType
    }
  }
}
```


- GraphQL позволяет точно описывать потребности каждого сценария, не перегружая сеть и client-info.

- Уменьшается RPS, увеличивается производительность.

- Снижается связанность между сервисами.

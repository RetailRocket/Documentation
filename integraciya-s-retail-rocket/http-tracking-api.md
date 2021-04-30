---
description: >-
  В документе описываются принцыпы и способы передачи в систему Retail Rocket
  пользовательского поведения на интернет магазине. По средствам протокола HTTP.
---

# API трекинга пользовательского поведения

Пользовательского поведение необходимо для многих продуктов Retail Rocket: товарные рекомендации, автоматизированые комуникационные кампании, метрики эфективности комуникации и т.д.

## **Общие концепции**

Для передачи пользовательского поведения в систему Retail Rocket необходимо для значимых действий пользователя\(просмотр товара, добавление в корзину и т.д.\) вызывать соответствующий действию методы трекиинг API. Ниже описаны методы для каждого такого действия с их параметрами и с возможными кодами ответов и примером вызова.

### **Base URL**

Все методы трекинг API имеют единый префикс часть URL

[`https://apptracking.retailrocket.net/1.0/`](https://apptracking.retailrocket.net/1.0/)

## **Методы трекинга поведения пользователя**

Трекинг API придерживается [общих принципов интеграционных API](obshie-principy-integracii-s-retail-rocket.md).

{% hint style="info" %}
Более подробнее про параметры методов можно прочитать по их ссылкам:

* [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) \(`apiKey`\)
* [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) \(`sessionExternalId`\)
* [Временная метка вызова](obshie-principy-integracii-s-retail-rocket.md#vremya-polzovatelskogo-sobytiya) \(`timestamp`\)
* [Параметры товарных предложений](obshie-principy-integracii-s-retail-rocket.md#svedeniya-o-tovare) \(`stockId`, `productId`, `groupId`\)
{% endhint %}

### Просмотр карточки товара

Должен быть вызван при каждом просмотре карточки товара пользователем

#### URL

`https://apptracking.retailrocket.net`**`/1.0/view`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`ViewEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада к которому пренадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST 'https://apptracking.retailrocket.net/1.0/view?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5' \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Просмотр карточки группового товара

Должен быть вызван при каждом просмотре карточки товара на которой представлен групповой товар.

#### URL

`https://apptracking.retailrocket.net`**`/1.0/groupView`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`GroupViewEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `groupId` | Да | integer | [Идентификатор товарной группы](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `productIds` | Да | integer | [Список идентификаторов товаров](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада к которому пренадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/groupView?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"groupView\": 654321,
         \"productIds\": [123456, 234567, 345678],
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Добавление товара в корзину

Должен быть вызван при каждом добавление товара в корзину.

#### URL

`https://apptracking.retailrocket.net`**`/1.0/addToBasket`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`AddToBasketEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада к которому пренадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST 'https://apptracking.retailrocket.net/1.0/addToBasket?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5' \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Просмотр страницы категории товара

Должен быть вызван при просмотре страницы категори товаров

#### URL

`https://apptracking.retailrocket.net`**`/1.0/categoryView`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`CategoryViewEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `categoryView` | Да | integer | [Идентификатор категории товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST 'https://apptracking.retailrocket.net/1.0/categoryView?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5' \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"categoryView\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Заказ товара

​Должен быть вызван при для каждой товарной позиции в заказе.

#### URL

`https://apptracking.retailrocket.net`**`/1.0/order`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`OrderEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `productId` | Да | integer | [Идентификатор товара](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `stockId` | Нет | string | [Идентификатор склада к которому пренадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `price` | Нет | string | Цена с учетом скидок |
| `transaction` | Нет | string | Идентификатор попкупки |
| `timestamp` | Да | string | [Метка времени](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/order?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"quantity\": 2,
         \"price\": 234,
         \"transaction\": \"135243\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

### Поисковый запрос

Должен быть вызван при вводе поисковой фразы на поисковой странице/экране интернет магазина.

#### URL

`https://apptracking.retailrocket.net`**`/1.0/search`**

#### HTTP-метод

`POST`

#### Параметры строки запроса

| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается объект типа **`SearchEvent`** со следующими полями:

| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `sessionExternalId` | Да | string | [Идентификатор пользователя](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#upravlenie-sessiei) |
| `searchPhrase` | Да | integer | Поисковая фраза которую ввел пользователь |
| `stockId` | Нет | string | [Идентификатор склада к которому пренадлежит товар](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#svedeniya-o-tovare) |
| `timestamp` | Да | string | [Метка времени](https://docs.retailrocket.net/integraciya-s-retail-rocket/obshie-principy-integracii-s-retail-rocket#metka-vremeni-vyzova) |

#### Пример вызова

```bash
curl \
   -X POST 'https://apptracking.retailrocket.net/1.0/search?apiKey=608423a104249fa8e9952323&partnerId=608423a9b126ac6ab3f8f0a5' \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"categoryView\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```

## Пакетная загрузка пользовательского поведения

API предоставляет возможно пакетной загрузки пользовательского поведения. В теле вызова метода передается список пользовательских событий с временными метками.

#### URL

`https://apptracking.retailrocket.net`**`/1.0/visitorEvents`**

#### HTTP-метод

`POST`

#### Параметры строки запроса


| Имя параметра | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| `apiKey` | Да | string | [Ключ авторизации](obshie-principy-integracii-s-retail-rocket.md#avtorizaciya) |
| `partnerId` | Да | string | [Идентификатор интернет магазина](obshie-principy-integracii-s-retail-rocket.md#upravlenie-sessiei) |

#### HTTP-заголовки

`Content-type: application/json`

#### Тело запроса

В теле запроса передается список пользовательских событий любого из следующих типов

ViewEnvelope
| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| View | Да | ViewEvent | Тип подробно описан в разделе "Просмотр карточки товара"

GroupViewEnvelope
| Имя поля | Обязательное | Тип | Описание |
| :--- | :--- | :--- | :--- |
| GroupView | Да | GroupViewEvent | Тип подробно описан в разделе "Просмотр карточки группового товара"

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/visitorEvents" %}
{% api-method-summary %}

{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="apiKey" type="string" required=true %}
Ключ авторизации
{% endapi-method-parameter %}

{% api-method-parameter name="partnerId" type="string" required=true %}
Идентификатор интернет магазина
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="body" type="array" required=false %}
В качестве тела запроса используется массив объектов пользовательских событий
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Пользовательские события

#### View

| Имя поля | Описание |
| :--- | :--- |
| view | Объект типа view, налогичный телу запроса метода view |

#### AddToBasket

| Имя поля | Описание |
| :--- | :--- |
| addToBasket | Объект типа addToBasket, налогичный телу запроса метода addToBasket |

#### Order

### Пример вызова

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/visitorEvents?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
    [
        {
            \"view\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 123456,
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"groupView\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"groupId\": 654321,
                \"productIds\": [123456, 234567, 345678],
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"addToBasket\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 234567,
                \"stockId\": \"NewYork\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"order\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"productId\": 234567,
                \"stockId\": \"NewYork\",
                \"quantity\": 3,
                \"price\": 1321.43,
                \"transaction\": \"135243\",
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"categoryView\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"categoryId\": 123456,                 
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
        {
            \"search\": {
                \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
                \"searchPhrase\": \"подгузник для новорожденных\",            
                \"timestamp\": \"2018-09-15T15:53:00+00:00\"
            }
        },
    ]
"
```
{% endtab %}
{% endtabs %}

## Время ответа

Ожидаемое время ответа: 100 мс для всех ресурсов \(может отличаться из-за особенностей сетевой инфраструктуры\).

## Ограничения

По умолчанию действует ограничение в 50 запросов/секунду для площадки. При необходимости возможно изменить \(с помощью аккаунт менеджера\).


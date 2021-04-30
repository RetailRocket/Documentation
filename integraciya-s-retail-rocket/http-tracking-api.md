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

В теле запроса передается объект типа **view** со следующими полями:

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

В теле запроса передается объект типа **groupView** со следующими полями:

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

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/view" %}
{% api-method-summary %}
view
{% endapi-method-summary %}

{% api-method-description %}
Должен быть вызван при каждом просмотре карточки товара пользователем.
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
{% api-method-parameter name="sessionExternalId" type="string" required=true %}
Идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="productId" required=true type="integer" %}
Идентификатор товара.
{% endapi-method-parameter %}

{% api-method-parameter name="stockId" type="string" required=false %}
Идентификатор склада к которому пренадлежит товар.
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="timestamp" %}
Временная метка вызова
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
запрос принят
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
ошибка в запросе, необходимо проверить правильность построения запроса
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
ошибка аутентификации, проверьте корректность параметра apiKey
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
доступ запрещен
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
запрошенный ресурс не найден, необходимо проверить правильность URL
{% endapi-method-response-example-description %}

```text

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Пример вызова

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/view?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
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
{% endtab %}
{% endtabs %}

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/groupView" %}
{% api-method-summary %}
groupView
{% endapi-method-summary %}

{% api-method-description %}
Должен быть вызван при каждом просмотре карточки товара на которой представлен групповой товар.
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
{% api-method-parameter name="sessionExternalId" required=true type="string" %}
Идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="groupId" type="integer" required=true %}
Идентификатор товарной группы.
{% endapi-method-parameter %}

{% api-method-parameter name="productIds" required=true type="array" %}
Список идентификаторов товаров
{% endapi-method-parameter %}

{% api-method-parameter name="stockId" type="string" required=false %}
Идентификатор склада/региона с которого был просмотрен товар
{% endapi-method-parameter %}

{% api-method-parameter type="string" name="timestamp" %}
Временная метка пользовательского события
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
запрос принят
{% endapi-method-response-example-description %}

```javascript
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
ошибка в запросе, необходимо проверить правильность построения запроса
{% endapi-method-response-example-description %}

```javascript
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
ошибка аутентификации, проверьте корректность пары partnerId, ApiKey
{% endapi-method-response-example-description %}

```javascript
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
доступ запрещен
{% endapi-method-response-example-description %}

```javascript
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
запрошенный ресурс не найден, необходимо проверить правильность URL
{% endapi-method-response-example-description %}

```javascript
{}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Пример вызова

{% tabs %}
{% tab title="Bash" %}
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
{% endtab %}
{% endtabs %}

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/addToBasket" %}
{% api-method-summary %}
addToBasket
{% endapi-method-summary %}

{% api-method-description %}
Должен быть вызван при каждом добавление товара в корзину.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-query-parameters %}
{% api-method-parameter name="apiKey" type="string" required=true %}
Ключ авторизации, описан в разделе "Авторизация"
{% endapi-method-parameter %}

{% api-method-parameter name="partnerId" type="string" required=true %}
Идентификатор интернет магазина
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="sessionExternalId" type="string" required=true %}
Идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="productId" type="integer" required=true %}
Идентификатор товара
{% endapi-method-parameter %}

{% api-method-parameter name="stockId" type="string" required=false %}
Идентификатор склада/региона с которого был просмотрен товар
{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}
Временная метка пользовательского события
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
запрос принят
{% endapi-method-response-example-description %}

```text
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
ошибка в запросе, необходимо проверить правильность построения запроса
{% endapi-method-response-example-description %}

```text
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=401 %}
{% api-method-response-example-description %}
ошибка аутентификации, проверьте корректность пары partnerId, ApiKey
{% endapi-method-response-example-description %}

```text
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
доступ запрещен
{% endapi-method-response-example-description %}

```text
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
запрошенный ресурс не найден, необходимо проверить правильность URL
{% endapi-method-response-example-description %}

```text
{}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

Пример вызова

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/addToBasket?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
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
{% endtab %}
{% endtabs %}

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/order" %}
{% api-method-summary %}
order
{% endapi-method-summary %}

{% api-method-description %}
​Должен быть вызван при при покупки каждого товара. Если в заказе несколько товаров то метод должен быть вызван для каждого товара.
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
{% api-method-parameter name="sessionExternalId" type="string" required=true %}
Идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="productId" type="integer" required=true %}
Идентификатор товара
{% endapi-method-parameter %}

{% api-method-parameter name="stockId" type="string" required=false %}
Идентификатор склада/региона с которого был просмотрен товар
{% endapi-method-parameter %}

{% api-method-parameter name="quantity" type="integer" required=true %}
Кол-во купленных артиклов
{% endapi-method-parameter %}

{% api-method-parameter name="price" type="number" required=true %}
Цена с учетом скидок
{% endapi-method-parameter %}

{% api-method-parameter name="transaction" type="string" required=true %}
Идентификатор попкупки
{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}
Временная метка пользовательского события
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

Пример вызова

{% tabs %}
{% tab title="Bash" %}
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
{% endtab %}
{% endtabs %}

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/categoryView" %}
{% api-method-summary %}
categoryView
{% endapi-method-summary %}

{% api-method-description %}
Должен быть вызван при просмотре страницы категори товаров
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
{% api-method-parameter name="sessionExternalId" type="string" required=true %}
Идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="categoryId" type="integer" required=true %}
Идентификатор категории товара
{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}
Временная метка пользовательского события
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

Пример вызова

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/categoryView?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"categoryId\": 123456,
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```
{% endtab %}
{% endtabs %}

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/search" %}
{% api-method-summary %}
search
{% endapi-method-summary %}

{% api-method-description %}
Должен быть вызван при вводе поисковой фразы на поисковой странице/экране интернет магазина.
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
{% api-method-parameter name="sessionExternalId" type="string" required=true %}
Идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="searchPhrase" type="string" required=true %}
Поисковая фраза которую ввел пользователь
{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}
Временная метка пользовательского события
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

Пример вызова

{% tabs %}
{% tab title="Bash" %}
```bash
curl \
   -X POST https://apptracking.retailrocket.net/1.0/search?apiKey=608423a104249fa8e9952323'&'partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"searchPhrase\": \"подгузник для новорожденных\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```
{% endtab %}
{% endtabs %}

## Пакетная загрузка пользовательского поведения

API предоставляет возможно пакетной загрузки пользовательского поведения. В теле вызова метода передается список пользовательских событий с временными метками. 

### Схема данных в теле запроса

{% tabs %}
{% tab title="JSON Scheme" %}
```scheme
{
    "type" : "object",
    "oneOf" :
    [
        {
            "properties": {
                "view:": {
                    "type": "object"
                }
            }
        }
    ]
}
```
{% endtab %}
{% endtabs %}

{% api-method method="post" host="https://apptracking.retailrocket.net" path="/1.0/visitorEvents" %}
{% api-method-summary %}
visitorEvents
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


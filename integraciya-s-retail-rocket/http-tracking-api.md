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
Строковый идентификатор пользователя
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
   -X POST https://apptracking.retailrocket.net/1.0/view \
   -d apiKey=608423a104249fa8e9952323 \
   -d partnerId=608423a9b126ac6ab3f8f0a5 \
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

{% tab title="C\#" %}
```csharp
namespace App
{
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main()
        {
            var url = @"https://apptracking.retailrocket.net/1.0"
                        + "/view?"
                        + "&apiKey=608423a104249fa8e9952323"
                        + "&partnerId=608423a9b126ac6ab3f8f0a5";

            var jsonContent = @"
            { 
                ""sessionExternalId"": ""60842392e4881c65e6c5e423"",
                ""productId"": 123456,
                ""stockId"": ""NewYork"",
                ""timestamp"": ""2018-09-15T15:53:00+00:00""
            }";

            using var client = new HttpClient();

            await client.PostAsync(
                requestUri: url,
                content: new StringContent(
                    content: jsonContent,
                    Encoding.UTF8,
                    mediaType: "application/json"));
        }
    }
}
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
Строковый идентификатор пользователя
{% endapi-method-parameter %}

{% api-method-parameter name="groupId" type="integer" required=true %}
Идентификатор товарной группы.
{% endapi-method-parameter %}

{% api-method-parameter name="productId" required=true type="integer" %}
Идентификатор товара
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
   -X POST https://apptracking.retailrocket.net/1.0/groupView \
   -d apiKey=608423a104249fa8e9952323 \
   -d partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"groupView\": 654321,
         \"productId\": 123456,
         \"stockId\": \"NewYork\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```
{% endtab %}

{% tab title="C\#" %}
```csharp
namespace App
{
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main()
        {
            var url = @"https://apptracking.retailrocket.net/1.0" 
                    + "/groupView?"
                    + "&apiKey=608423a104249fa8e9952323"
                    + "&partnerId=608423a9b126ac6ab3f8f0a5";

            var jsonContent = @"
            { 
                ""sessionExternalId"": ""60842392e4881c65e6c5e423"",
                ""groupView"": 654321,
                ""productId"": 123456,
                ""stockId"": ""NewYork"",
                ""timestamp"": ""2018-09-15T15:53:00+00:00""
            }";

            using var client = new HttpClient();

            await client.PostAsync(
                requestUri: url,
                content: new StringContent(
                    content: jsonContent,
                    Encoding.UTF8,
                    mediaType: "application/json"));
        }
    }
}
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
Строковый идентификатор пользователя
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
   -X POST https://apptracking.retailrocket.net/1.0/addToBasket \
   -d apiKey=608423a104249fa8e9952323 \
   -d partnerId=608423a9b126ac6ab3f8f0a5 \
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

{% tab title="C\#" %}
```csharp
namespace App
{
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main()
        {
            var url = @"https://apptracking.retailrocket.net/1.0"
                        + "/addToBasket?"
                        + "&apiKey=608423a104249fa8e9952323"
                        + "&partnerId=608423a9b126ac6ab3f8f0a5";

            var jsonContent = @"
            { 
                ""sessionExternalId"": ""60842392e4881c65e6c5e423"",
                ""productId"": 123456,
                ""stockId"": ""NewYork"",
                ""timestamp"": ""2018-09-15T15:53:00+00:00""
            }";

            using var client = new HttpClient();

            await client.PostAsync(
                requestUri: url,
                content: new StringContent(
                    content: jsonContent,
                    Encoding.UTF8,
                    mediaType: "application/json"));
        }
    }
}
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
Строковый идентификатор пользователя
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
   -X POST https://apptracking.retailrocket.net/1.0/order \
   -d apiKey=608423a104249fa8e9952323 \
   -d partnerId=608423a9b126ac6ab3f8f0a5 \
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

{% tab title="C\#" %}
```csharp
namespace App
{
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main()
        {
            var url = @"https://apptracking.retailrocket.net/1.0"
                        + "/addToBasket?"
                        + "&apiKey=608423a104249fa8e9952323"
                        + "&partnerId=608423a9b126ac6ab3f8f0a5";

            var jsonContent = @"
            { 
                ""sessionExternalId"": ""60842392e4881c65e6c5e423"",
                ""productId"": 123456,
                ""stockId"": ""NewYork"",
                ""quantity"": 2,
                ""price"": 234,
                ""transaction"": ""135243"",
                ""timestamp"": ""2018-09-15T15:53:00+00:00""
            }";

            using var client = new HttpClient();

            await client.PostAsync(
                requestUri: url,
                content: new StringContent(
                    content: jsonContent,
                    Encoding.UTF8,
                    mediaType: "application/json"));
        }
    }
}
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

{% endapi-method-parameter %}

{% api-method-parameter name="partnerId" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="sessionExternalId" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="categoryId" type="integer" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}

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
   -X POST https://apptracking.retailrocket.net/1.0/categoryView \
   -d apiKey=608423a104249fa8e9952323 \
   -d partnerId=608423a9b126ac6ab3f8f0a5 \
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

{% tab title="C\#" %}
```csharp
namespace App
{
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main()
        {
            var url = @"https://apptracking.retailrocket.net/1.0"
                        + "/categoryView?"
                        + "&apiKey=608423a104249fa8e9952323"
                        + "&partnerId=608423a9b126ac6ab3f8f0a5";

            var jsonContent = @"
            { 
                ""sessionExternalId"": ""60842392e4881c65e6c5e423"",
                ""categoryId"": 123456,
                ""timestamp"": ""2018-09-15T15:53:00+00:00""
            }";

            using var client = new HttpClient();

            await client.PostAsync(
                requestUri: url,
                content: new StringContent(
                    content: jsonContent,
                    Encoding.UTF8,
                    mediaType: "application/json"));
        }
    }
}
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

{% endapi-method-parameter %}

{% api-method-parameter name="partnerId" type="string" required=true %}

{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="sessionExternalId" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="searchPhrase" type="string" required=true %}

{% endapi-method-parameter %}

{% api-method-parameter name="timestamp" type="string" required=false %}

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
   -X POST https://apptracking.retailrocket.net/1.0/search \
   -d apiKey=608423a104249fa8e9952323 \
   -d partnerId=608423a9b126ac6ab3f8f0a5 \
   -H "Content-type: application/json" \
   --data "
      {
         \"sessionExternalId\": \"60842392e4881c65e6c5e423\",
         \"searchPhrase\": \"pampers\",
         \"timestamp\": \"2018-09-15T15:53:00+00:00\"
      }
   "
```
{% endtab %}

{% tab title="C\#" %}
```csharp
namespace App
{
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;

    class Program
    {
        static async Task Main()
        {
            var url = @"https://apptracking.retailrocket.net/1.0"
                        + "/search?"
                        + "&apiKey=608423a104249fa8e9952323"
                        + "&partnerId=608423a9b126ac6ab3f8f0a5";

            var jsonContent = @"
            { 
                ""sessionExternalId"": ""60842392e4881c65e6c5e423"",
                ""searchPhrase"": ""pampers"",
                ""timestamp"": ""2018-09-15T15:53:00+00:00""
            }";

            using var client = new HttpClient();

            await client.PostAsync(
                requestUri: url,
                content: new StringContent(
                    content: jsonContent,
                    Encoding.UTF8,
                    mediaType: "application/json"));
        }
    }
}
```
{% endtab %}
{% endtabs %}

## Пакетная загрузка пользовательского поведения






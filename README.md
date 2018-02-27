> Описание для работы с тендерами

**Получение тендеров через API REST для сайта navodki.ru**

Теперь вы можете:
* получать все тендеры за прошедшие сутки; 
* следить за изменение статуса по дате обновления;
* анализировать и прогнозировать изменения цен и спроса на автомобили;
* размещать полученную информацию на вашем сайте.`*`

```
* Наличие ссылки на сайт NAVODKI.RU с гиперссылкой на страницу https://navodki.ru, 
не закрытой для индексации поисковыми системами, является единственным 
обязательным требованием для использования сервиса.
```


# REST API сайта NAVODKI.RU

API возвращает данные в формате JSON. Формат данных в большинстве случаев стандартный - коллекция объектов с полями *name* (название объекта) и *value* (его идентификатор).

Идентификатор любой сущности является целым числом.


### Управление списком

В системе установлен максимальный лимит на 10000. То есть дальше больше чем page=1000 указать невозможно


|  Параметр     | По умолчанию | Тип данных   | Описание   |
|:--------------|:--------------------------|:------------:|------------:|
|  limit        |  10           |  `int`      |   От 0 до 100   |
|  page         |  1            |  `int`      | номер страницы (максимальная страница 1000) |
|  sortby       |  updatedon    |  `string`   | сортировка по полю (доступны поля: name,published, updatedon, start_date, end_date, max_price, a_price, c_price) |
|  sortdir      |  DESC         |  `int`      | порядок сортировки |



### Формат данных в ответе

В случае успешного подсчета средней цены по указанным параметрам результат будет со статусом **200 OK**.

```
http://navodki.ru/api/tenders?tid=14448486&platform_id=1,3&categories=232,232
```

Пример успешного ответа:
```json
{
    "total":"17",
    "results":[
      {
          "tid": 14754476,
          "uri": "https://navodki.ru/tenders/14754476",
          "name": "Поставка канцелярских товаров",
          "subject": "Поставка канцелярских товаров",
          "status": 2,
          "status_name": "Работа комиссии",
          "platform_id": 3,
          "platform_name": "223 \u0437\u0430\u043a\u043e\u043d",
          "published": "12.02.2018",
          "updatedon": "19.02.2018",
          "start_date": 0,
          "end_date": "2018-02-19 14:00:00",
          "max_price": "135 000",
          "a_price": 0,
          "c_price": 0,
          "customers": [
              {
                  "name": "ОБЩЕСТВО С ОГРАНИЧЕННОЙ ОТВЕТСТВЕННОСТЬЮ ЭНЕРГОСЕРВИС КОМИ",
                  "INN": "1101068261"
              }
          ],
          "categories": [
              "152",
              "184"
          ],
          "regions": [
              {
                  "id": 12,
                  "path": "komi_resp",
                  "title": "Республика Коми"
              }
          ],
          "etp_id": 63,
          "etp_name": "РТС-тендер",
          "pw_id": 33329,
          "pw_name": "Запрос котировок в электронной форме",
          "currency_code": "RUB",
          "currency_digitalCode": "643",
          "currency_name": "Российский рубль",
          "attach": true
      }
    ]
}
```


### Список поддерживаемых параметров

На данный момент поддерживаются следующие параметры:


|  Поле         | Тип данных        | Фильтры         | Название |
|:--------------|:------------------|:----------------|:----------------:|
|  tid          |  `int`            | true            | [id тендера](#tid)                                             | 
|  uri          |  `string`         | true            | Ссылка                                                        | 
|  name         |   `string`        | false           | Наименование                                                  | 
|  subject      |  `string`         | false           | Наименование (для 223 закона)                              |  
|  status       |  `int`            | true            |[Статус](#user-content-status)                              |  
|  status_name  |  `string`         | false           |Наименование статуса                                   |  
|  platform_id  |  `int`            | true            |[Id закона](#user-content-platform_id)                 |  
|  platform_name|  `string`         | false           |Наименование закона                                  |  
|  published    |  `date`           | true            | [Дата публикации](#user-content-date)                     |  
|  updatedon    |  `date`           | true            | [Дата обновления](#user-content-date)                                 |  
|  start_date   |  `date`           | true            | [Дата начала подачи заявок](#user-content-date)                      |  
|  end_date     |  `date`           | true            | [Дата окончания подачи заявок](#user-content-date)                     |  
|  max_price    |  `float`          | true            |   [Начальная (максимальная) цена](#user-content-price)                 |  
|  a_price      |  `float`          | true            |   [Обеспечение заявки](#user-content-price)                               |  
|  c_price      |  `float`          | true            |   [Обеспечение контракта](#user-content-price)                            |  
|  customers    |  `array`          | true            |   [Заказчики](#user-content-customers)                                  |  
|  categories   |  `array`          | true            |   [Отрасли](#user-content-categories)                                  |  
|  regions      |  `array`          | true            |   [Регион](#user-content-regions)                                         |  
|  etp_id       |  `int`            | true            |   [Площадка](#user-content-etp_id)                                         |  
|  etp_name     |  `string`         | false           |   Наименование площадки                       |  
|  pw_id        |  `int`            | true            | [Способ определения поставщика](#user-content-pw_id)                      |  
|  pw_name      |  `string`         | false           |  Наименование способа определения поставщика                             | 
|  currency_name|  `int`            | false           | Наименование валюты                                    |  
|  currency_code|  `string`         | false           | Код валюты                                       |  
|  currency_digitalCode|  `int`     | false           | Цифровая валюта                           |  
|  attach       |  `boolean`        | true            |Метка о наличии документации                            |  







Расшифровка параметров:
- *total* - общее количество найденых результатов.
- *results* - массив данных

Если по каким-либо произошла ошибка, ответ будет иметь статус **400 Bad Request**, а тело ответа будет содержать следующее:

```javascript
{ "message": "Not Enough Data" }
```



## Методы для работы с типами транспорта и кузова


<a name="tid"></a>
### ID тендер

Выборка по id
```php
http://dev.navodki.ru/api/tenders?tid=14448486
```


<a name="status"></a>
### Статус

Выборка по статусу

```
http://dev.navodki.ru/api/tenders?status=2,3,4,6
```
  
Список значений: 

```javascript
[
    { 
      id: 2,
      name: "Работа комиссии",  
    },
    { 
      id: 3,
      name: "Закупка отменена",  
    },
    { 
      id: 4,
      name: "Закупка завершена",  
    },
    { 
      id: 6,
      name: "Идет прием заявок",  
    },
]
```

<a name="platform_id"></a>
### ID закона

Выборка тендеров по закону

```
http://dev.navodki.ru/api/tenders?platform_id=1,3
```
  
Список значений: 

```javascript
[
    { 
      id: 1,
      name: "44 закон",  
    },
    { 
      id: 3,
      name: "223 закон",  
    },
]
```


<a name="date"></a>
### Выборка дате для полей (published,updatedon,start_date,end_date)

Обе даты устанавливать обязательно

```
Формат ввода данных: "d-m-Y"
Минимальное значение: "01.01.2016"
Максимальноез значение: "текущая дата"

http://dev.navodki.ru/api/tenders?updatedon=07-02-2018,20-02-2018
```


<a name="price"></a>
### Выборка цене для полей (max_price,a_price,c_price)

Оба значения обязательны.

```
Разделитель: ","
Формат ввода данных: "int"
Минимальное значение: "0"
Максимальноез значение: "1000000000000000"

http://dev.navodki.ru/api/tenders?max_price=10000,10000000
```



<a name="customers"></a>
### Заказчики

У одной закупки может быть не ограниченое количество заказчиков.
Для выборки заказчиков используется **ИНН организации**

```
Разделитель: ","
Формат ввода данных: "number"
Допустимые значения: "10 или 12 цифр"

http://dev.navodki.ru/api/tenders?customers=7708697381
```


<a name="categories"></a>
### Отрасли


Выборка тендеров по отрасли закупки. Отрасль закупок прявязана к справочнику ОКПД2 с помощью кода каждому тендеру назначается отрасль

```
Разделитель: ","
Формат ввода данных: "int"

http://dev.navodki.ru/api/tenders?categories=244,380,245
```
  
Список значений: 

```
http://dev.navodki.ru/api/categories
```


<a name="regions"></a>
### Регион

Выборка данных по региону закупок заказчика.

```
Разделитель: ","
Формат ввода данных: "int"

http://dev.navodki.ru/api/tenders?regions=244,380,245
```
  
Список значений: 

```
http://dev.navodki.ru/api/regions
```


<a name="etp_id"></a>
### Площадка

Выборка данных по площадке размещения закупки.

```
Разделитель: ","
Формат ввода данных: "int"

http://dev.navodki.ru/api/tenders?etp=9,7,65,121,2,63,117
```
  
Список значений: 

```
http://dev.navodki.ru/api/etp
```

<a name="pw"></a>
### Способу определения поставщика

Выборка данных по способу определения поставщика

```
Разделитель: ","
Формат ввода данных: "int"

http://dev.navodki.ru/api/tenders?pw=212,3512
```
  
Список значений: 

```
http://dev.navodki.ru/api/pw
```
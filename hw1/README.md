# Домашнее задание 1

Знакомьтесь, Zwitter -- новая, амбициозная социальная сеть.
Чем именно она отличается от существующих -- это вопрос, на который еще предстоит дать ответ,
но одно известно точно: опытные основатели-стартапщики умудрились уже каким-то образом
привлечь пользователей в эту социальную сеть, и им теперь нужна ваша помощь
в том, чтобы разобраться, кто же эти самые привлеченные пользователи.

## Что надо делать?

Вам необходимо решить следующие задачи:

  * наладить ежедневный расчет ключевых метрик и показателей,
  * предоставить доступ к рассчитанным данным через HTTP API.

Ниже вы можете найти более подробную информацию по следующим вопросам:

  * [Требования](#Требования)
  * [Критерии сдачи задания](#Критерии-сдачи-задания)
  * [Процесс сдачи задания](#Процесс-сдачи-задания)
  * [Исходные данные](#Исходные-данные)
  * [Рассчитываемые метрики](#Рассчитываемые-метрики)
  * [Спецификация HTTP API](#Спецификация-http-api)
  * [Дополнительные комментарии](#Дополнительные-комментарии)

## Требования

Ваше решение вышеописанных задач должно удовлетворять следующим требованиям:

  * вычисленные данные должны быть готовы к 9 утра следующего дня (к примеру, результаты за 10-е число
    должны быть готовы к 09:00 11-го числа); готовность вычисленных данных определяется доступностью вычисленных метрик через HTTP API;
  * время ответа HTTP API не должно превышать 10 секунд;
  * запрещается запускать MapReduce-задачи как реакцию на HTTP-запрос;
  * ваш процесс расчета должен стабильно работать начиная с момента сдачи задания
    и до окончания курса, при этом ваше решение _не должно_ терять просчитанные данные за
    прошедшие дни.

## Критерии сдачи задания

За данное домашнее задание возможно заработать максимум 10 попугаев.
Попугаи выдаются за группы метрик, все метрики разделены на пять групп.
Чтобы сдать какую-либо группу метрик, нужно посчитать хотя бы одну метрику из группы.

  * за первую группу выдается *один* (1) попугай;
  * за вторую группу выдается *два* (2) попугая;
  * за третью группу выдается *два* (2) попугая;
  * за четвертую группу выдается *два* (2) попугая;
  * за пятую группу выдается *три* (3) попугая.

До 24.10 необходимо реализовать вычисление метрик и зарегистрировать ваше решение в системе.

Если в течение *семи* (7) подряд идущих дней в период *с 24.10 по 20.12* (обе даты включительно)
ваш HTTP API не выдает вычисленные значения метрик, то из вашего результата будет вычтен один попугай (за каждые 
неработающие 7 дней будет вычтено по одному попугаю).

Если в течение *семи* (7) подряд идущих дней в период *с 24.10 по 20.12* (обе даты включительно)
 ваш HTTP API выдает неправильный результат, то из вашего результата по данной группе метрик будет вычтен один
попугай (за каждые неработающие 7 дней будет вычтено по одному попугаю).

## Процесс сдачи задания

**Интерфейс будет доступен с 10-го октября!**

Для самопроверки и сдачи вашего решения вам необходимо будет использовать веб-интерфейс Zwitter,
доступный на порту `8888` машины `hadoop2-00.yandex.ru` (NB: для доступа к веб-интерфейсу
вам необходимо пробросить `8888`-й порт на локальную машину, аналогично веб-интерфейсу Hadoop).

Для самопроверки вы можете использовать страницу по адресу `http://hadoop2-00.yandex.ru:8888/ysda/hw1`, где
вы можете указать порт, на котором работает ваше HTTP API, и диапазон дат.
При нажатии на кнопку "Query" будет выполнен запрос к вашему решению, проверена корректность ответа
(структура ответа и типы возвращаемых значений), и произведена отрисовка ответа.

Используя данный интерфейс вы можете проверить, что ваше решение корректно интегрируется в систему.

## Исходные данные

На кластере в HDFS `/user/sandello/logs` веб-сервера социальной сети сохраняют логи. 

```
$ hadoop fs -ls /user/sandello/logs
Found 1 items
-rw-r--r--   3 sandello sandello    5750921 2016-10-07 18:03 /user/sandello/logs/access.log.2016-10-07
```

Пример записей в логе:

```
195.206.123.39 - - [24/Sep/2016:12:32:53 +0400] "GET /id18222 HTTP/1.1" 200 10703 "http://bing.com/" "Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.94 Safari/537.36"
176.62.191.48 - - [24/Sep/2016:12:32:53 +0400] "GET /id81715 HTTP/1.1" 200 32343 "-" "OFM search_robot .*google.*  (compatible; )"
193.9.247.243 - - [24/Sep/2016:12:32:53 +0400] "GET /id79304 HTTP/1.1" 200 29671 "http://twitter.com/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_8_0) AppleWebKit/537.8 (KHTML, like Gecko) Chrome/23.0.1255.0 Safari/537.8"
194.196.94.215 - - [24/Sep/2016:12:32:53 +0400] "GET / HTTP/1.1" 200 32598 "http://yandex.ru/yandsearch" "Mozilla/5.0 (Linux; U; Android 4.2.2; de-de; HTC_One/2.24.111.5 Build/JDQ39) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30"
176.120.29.168 - - [24/Sep/2016:12:32:56 +0400] "GET /id50192 HTTP/1.1" 200 20065 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/45.0.2454.99 Safari/537.36"
79.132.115.64 - - [24/Sep/2016:12:32:56 +0400] "GET /id49582 HTTP/1.1" 200 28244 "-" "Mozilla/5.0 (Windows NT 5.1; U; de; rv:1.9.1.6) Gecko/20091201 Firefox/3.5.6 Opera 11.00"
```

Каждый лог преставляет из себя файл формата [access.log](http://httpd.apache.org/docs/2.0/logs.html#accesslog).
Каждая строка лога представляет собой запись, состояющую из нескольких полей, разделенных пробелом.
Строковые поля записываются в кавычках; внутренние кавычки строк экранируются символом `\`.

Каждая запись содержит следующие поля (на примере первой строки из вышеприведенного фрагмента):

  * IP-адрес пользователя (`195.206.123.39`),
  * Далее идут два неиспользуемых в нашем случае поля (`-` и `-`),
  * Время запроса (`[24/Sep/2016:12:32:53 +0400]`),
  * Строка запроса (`"GET /id18222 HTTP/1.1"`),
  * HTTP-код ответа (`200`),
  * Размер ответа (`10703`),
  * Реферер (источник перехода; `"http://bing.com/"`),
  * Идентификационная строка браузера (User-Agent; `"Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.94 Safari/537.36"`).

Считаем, что пользователь идентифицируется IP-адресом. Пользователи с различными IP-адресами считаются разными.

Из каждой записи можно извлечь информацию о браузере пользователя. Вам предлагается придумать набор эвристик,
позволяющих по User-Agent опознать браузер пользователя. Для упрощения задачи ответ нужно выбрать среди множества
Chrome, Yandex Browser, Firefox, Safari, Internet Explorer, Other. В приведенном выше примере правильные ответы --
Chrome, Other, Chrome, Other, Chrome, Firefox.

Также по IP-адресу пользователя можно восстановить его местоположение. В директории HDFS
`/user/sandello/dicts` лежат две базы IP2LOCATION.

В файле `IP2LOCATION-LITE-DB1.CSV` закодирована информация о соответствии стран и IP-адресов;
в файле `IP2LOCATION-LITE-DB5.CSV` -- более точная гео-информация.

Пример записей:

```
# IP2LOCATION-LITE-DB1.CSV
"34643712","34644479","RU","Russian Federation"
"34644480","34644991","DE","Germany"
"34644992","34645503","US","United States"
"34645504","34646015","NL","Netherlands"
"34646016","34646271","RU","Russian Federation"
"34646272","34646527","NL","Netherlands"
```

```
# IP2LOCATION-LITE-DB5.CSV
"34643712","34644479","RU","Russian Federation","Moscow City","Moscow","55.752220","37.615560"
"34644480","34644735","DE","Germany","Bayern","Nuremberg","49.447780","11.068330"
"34644736","34644991","DE","Germany","Nordrhein-Westfalen","Remscheid","51.179830","7.192500"
"34644992","34645503","US","United States","California","Los Angeles","34.052230","-118.243680"
"34645504","34646015","NL","Netherlands","Noord-Holland","Amsterdam","52.374030","4.889690"
"34646016","34646271","RU","Russian Federation","Moscow City","Moscow","55.752220","37.615560"
```

Каждая строка состоит из нескольких полей:

  * нижняя граница диапазона IP-адресов (`34643712`),
  * верхняя граница диапазона IP-адресов (`34644479`),
  * код страны (`RU`),
  * полное название страны (`Russian Federation`),
  * регион (`Moscow City`),
  * город (`Moscow`),
  * широта (`55.752220`),
  * долгота (`37.615560`).

Перевод IP-адреса между численным представлением и строковым осуществляется через
кодирование/декодирование отдельных байт численного представления. Каждый IP-адрес
в численном представлении кодируется 32-битным числом, которое можно интерпретировать
как набор из 4 байт.

Пример (на основе первой записи из приведенного выше фрагмента базы):

```python
# Нижняя граница
dec = 34643712
byte_0 = (dec >> 24) & 0xff  # 2
byte_1 = (dec >> 16) & 0xff  # 16
byte_2 = (dec >>  8) & 0xff  # 159
byte_3 = (dec >>  0) & 0xff  # 0
txt = ".".join(map(str, [byte_0, byte_1, byte_2, byte_3]))  # "2.16.159.0"

# Верхняя граница
dec = 34644479
byte_0 = (dec >> 24) & 0xff  # 2
byte_1 = (dec >> 16) & 0xff  # 16
byte_2 = (dec >>  8) & 0xff  # 161
byte_3 = (dec >>  0) & 0xff  # 255
txt = ".".join(map(str, [byte_0, byte_1, byte_2, byte_3]))  # "2.16.161.255"

# Обратный перевод
txt = "2.16.160.128"
byte_0, byte_1, byte_2, byte_3 = map(int, txt.split("."))  # 2, 16, 160, 128
dec = byte_0 << 24 | byte_1 << 16 | byte_2 << 8 | byte_3 << 0  # 34644096
```

## Рассчитываемые метрики

В рамках данного задания вам по дневному логу необходимо посчитать ключевые метрики для веб-сайта социальной сети.

**Все** нижеприведенные метрики расчитываются только по успешным (HTTP-код 200) запросам к сайту.
Неуспешные запросы стоит игнорировать (обычно такие запросы порождены сканерами, ищущими известные уязвимости
в веб-сайтах).

Метрики делятся на пять групп.

Метрики первой группы:

  * `total_hits` -- Суммарное количество запросов к сайту.
  * `total_users` -- Суммарное количество уникальных пользователей, пользователь идентифицируется IP-адресом.
  * `top_10_pages` -- 10 самых посещаемых страниц сайта (при совпадении количества просмотров упорядочить страницы лексикографически).

Метрики второй группы:

Сессией считается последовательность запросов от одного пользователя с разницей по времени между последовательными запросами не более 30 минут. 

  * `average_session_time` -- Среднее время сессии в секундах (дробное число).
  * `average_session_length` -- Средняя длина сессии (количество посещенных страниц в течение одной сессии; дробное число).
  * `bounce_rate` -- Отношение количества сессий-скелетов (состоящих из посещения только одной, первой страницы) к общему количеству сессий.

Метрики третьей группы:

  * `users_by_country` -- Количество пользователей в разбивке по странам. Способ определения страны описан в секции "Исходные данные".
  
Метрики четвертой группы:

  * `new_users` -- Количество новых пользователей. Пользователь считается новым в день X, если он посетил социальную сеть Zwitter в день X, но не посещал ее в течение дней X-1, X-2, ..., X-13.
  * `lost_users` -- Количество потерянных пользоватеей. Пользователь считается потерянным в день X, если он посетил социальную сеть Zwitter в день X-13 и _не_ посещал социальную сеть Zwitter в дни X-12, X-11, ... и _не_ посещал в день X.

Метрики пятой группы:
  
  * `facebook_signup_conversion_3` -- Конверсия новых пользователей, пришедших из Facebook, в подписку за последние три дня. Назовем каналом привлечения пользователя источник перехода (реферер) первого хита пользователя на сайт. Момент первого хита пользователя также определяет первый день жизни пользователя на сайте. Назовем множество пользователей, привлеченных с Facebook в день X, X-1, X-2, трехдневной аудиторией сервиса на день X. Будем говорить, что пользователь совершил конверсию в день X, если в этот день он перешел на страницу `/signup` и не переходил на страницу `/signup` ни разу перед этим. Целевая метрика (конверсия пользователей из Facebook за последние три дня) на день X -- это отношение количества пользователей из трехдневной аудитории, совершивших конверсию в день X, к суммарному размеру трехдневной аудитории на день X.

## Спецификация HTTP API

Ваше решение должно уметь отвечать на запросы типа `GET` по URI `/api/hw1`; 
в параметрах запроса указывается начальная дата `start_date` и конечная дата `end_date`,
за которые необходимо вернуть данные. Даты указываются в формате `YYYY-MM-DD`.

В ответе вам необходимо вернуть JSON-документ, ключами которого являются даты
в запрашиваемом диапазоне, а значениями -- JSON-документы с рассчитываемыми метриками
(указывайте только те метрики, что вы реально рассчитываете).

Пример запроса:

```
GET /api/hw1?start_date=2016-10-01&end_date=2016-10-05
```

Пример ответа:
```json
{
  "2016-10-01": {
    "total_hits": 1000,
    "total_users": 100,
    "top_10_pages": [x
      "/", "/id1", "/id2", "/id3", "/id4", "/id5", "/id6", "/id7", "/id8", "/id9"
    ],
    "average_session_time": 300.0,
    "average_session_length": 5.0,
    "new_users": 10,
    "lost_users": 10,
    "users_by_country": {
      "Russian Federation": 100
    },
    "facebook_signup_conversion_3": 0.05
  },
  "2016-10-02": { ... },
  "2016-10-03": { ... },
  "2016-10-04": { ... },
  "2016-10-05": { ... }
}
```

Пример ответа только с метриками `total_hits` и `total_users`:
```json
{
  "2016-10-01": {
    "total_hits": 1000,
    "total_users": 100
  },
  "2016-10-02": { ... },
  "2016-10-03": { ... },
  "2016-10-04": { ... },
  "2016-10-05": { ... }
}
```

Для быстрого старта вы можете воспользоваться доступным [шаблоном HTTP API](./example.py).

## Дополнительные комментарии

  * Для периодического запуска процессов используйте `cron` ([[1]](https://ru.wikipedia.org/wiki/Cron), [[2]](http://help.ubuntu.ru/wiki/cron)).
  * Для того, чтобы запущенная вами программа не была остановлена при отключении ssh, используйте `screen` или `tmux` ([[1]](http://habrahabr.ru/post/126996/)).
  * Изучите (административный) веб-интерфейс Zwitter: он может быть полезен при отладке вашего сервиса.
  * Подумайте, где вы будете хранить результаты расчета? Как самый простой вариант -- сохраняйте результаты в файл с именем, соответствующим дате, за которую был произведен расчет. 
  * Для сохранности результатов на случай сбоев стоит использовать HDFS. 
  * Для быстрого доступа к данным, не стоит использовать HDFS, стоит использовать локальные жесткие диски.
  * Если вы хотите вычислить какую-то дополнительную метрику, которую мы не описали, напишите нам, вероятно, мы ее добавим в список рассчитываемых метрик. Может, вычисление этой метрики будет интересно не только вам.

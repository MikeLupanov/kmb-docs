# Основы работы с sqlmap.

## Документация и основные параметры

### Использование:

[Ссылка на GitHub](https://github.com/sqlmapproject/sqlmap/zipball/master) - https://github.com/sqlmapproject/sqlmap/zipball/master

```bash
Синтаксис: python sqlmap.py [options]
```

#### Опции:
```bash
-h, --help          Выводит краткую помощь по программе
-hh                 Выводит полную справку по программе
--version           Выводит версию программы
-v VERBOSE          Уровень детализации: 0-6(по-умолчанию 1)
```
#### Цели:
  Как минимум один из следующих вариантов должен присутствовать, чтобы определить цель
```bash
-d DIRECT           Прямое подключение к базе данных
-u URL, --url=URL   URL цели (например, "www.target.com/vuln.php?id=1")
-l LOGFILE          Вести логи от Burp или WebScarb proxy в файл
-m BULKFILE         Сканирование по списку целей, заданных в переданном файле
-r REQUESTFILE      Загрузить HTTP-запрос из файла
-g GOOGLEDORK       Использовать результат выдачи Google дорков как целевые url\'ы (site:, inurl:, intext:)
-c CONFIGFILE       Загрузить настройки из конфигурационного INI файла.
```


#### Запросы:
```bash
--data=DATA         Строка данных, которая будет передана POST запрсом
--param-del=PDEL    Обозначение, использующееся для разделения значений парматеров
--cookie=COOKIE     Заголовок http cookie
--cookie-del=CDEL   Обозначение, использующееся для разделения значиений cookie
--load-cookies=A..  Файл, содержащий cookies в формате Netscape/wget
--drop-set-cookie   Игнорировать заголовок Set-Cookie в ответе
--user-agent=AGENT  Заголовок HTTP User-Agent
--random-agent      Использовать случайный HTTP User-Agent заголовок
--host=HOST         HTTP Host заголовок
--referer=REFERER   HTTP Referer заголовок
--headers=HEADERS   Extra заголовки (т.е. "Accept-Language: fr\nETag: 123")
--auth-type=ATYPE   Тип HTTP аутентификации (Basic, Digest or NTLM)
--auth-cred=ACRED   Данные HTTP аутентификации (name:password)
--auth-private=A..  Файл приватного PEM ключа для HTTP аутентификации
--proxy=PROXY       HTTP прокси для подключения к целевому URL
--proxy-cred=PCRED  Данные для аутентификации через HTTP прокси (name:password)
--ignore-proxy      Игнорировать системные настройки прокси
--tor               Использовать TOR для соединения
--tor-port=TORPORT  Указать порт TOR proxy, отличающийся от дефолтного
--tor-type=TORTYPE  Указать тип TOR proxy (HTTP (умолч.), SOCKS4 или SOCKS5)
--check-tor         Проверить, используется ли TOR должным образом
--delay=DELAY       Задержка в секундах между каждым HTTP запросом
--timeout=TIMEOUT   Время ожидания в секундах до сброса соединения (30 дефолтно)
--retries=RETRIES   Количество повторов при такймауте (3 дефолтно)
--randomize=RPARAM  Случайное значение для заданных параметров
--safe-url=SAFURL   URL адрес, часто посещаемый в процессе тестирования
--safe-freq=SAFREQ  Тестовыые запросы между двумя обращениями к заданному safe URL
--skip-urlencode    Пропустить кодирование данных полезной нагрузки
--force-ssl         Принудительно использовать SSL/https
--hpp               Использовать загрязнение параметров запроса HPP
--eval=EVALCODE     Выполнить Python код перед запросом (т.е. "import hashlib;id2=hashlib.md5(id).hexdigest()")
```
[HPP about link](http://raz0r.name/articles/http-parameter-pollution)

#### Оптимизация:
Следующие опции могут быть использованы для улучшения производительности sqlmap
```bash
-o                  Задействовать все виды оптимизации
--predict-output    Предсказывать общие исходящие заголовки
--keep-alive        Использовать постоянное HTTP(S) соединение
--null-connection   Получить размер страницы без тела http ответа
--threads=THREADS   Максимальное количество одновременных http(s) запросов (дефолтно - 1)
```

#### Инъекции:
Эти параметры могут быть использованы, чтобы указать, какие параметры использовать для проверки полезной нагрузки, инъекций и ненадёжных скриптов.
```bash
-p TESTPARAMETER    Тестируемые параметры
--skip=SKIP         Пропустить тест для заданных параметров
--dbms=DBMS         Принудительно(?) использовать фоновые СУБД
--dbms-cred=DBMS..  Данные для аутентификации к СУБД (user:password)
--os=OS             Использовать серверных СУБД ОС для заданных значений
--invalid-bignum    Использовать большие числа для определения невалидных значений
--invalid-logical   Использовать логические операции для определения невалидных значений
--invalid-string    Использовать случайные строки для определения невалидных значений
--no-cast           Отключить полезную нагрузку
--no-escape         Отключить экранирование строк
--prefix=PREFIX     Полезная нагрузка в строке преффикса
--suffix=SUFFIX     Полезная нагрузка в строке суффикса
--tamper=TAMPER     Использовать скрипт для подделки данных инъекции
```

#### Обнаружение:
Эти параметры могут быть использованы для настройки уровней обнаружения
```bash
--level=LEVEL      Уровень тестирования (1-5, дефолтно 1)
--risk=RISK        Риск тестирования (0-3, дефолтно 1)
--string=STRING    Строка для соответствия, если запрос вернет TRUE
--not-string=NOT.. Строка для соответствия, если запрос вернет FALSE
--regexp=REGEXP    Регулярка для совпадения, когда запрос TRUE
--code=CODE        HTTP код, когда запрос TRUE
--text-only        Сравнение страниц на основе текстового содержимого
--titles           Сравнение страниц на основе их заголовков
```

#### Методы:
Эти параметры могут быть использованы для настройки методов тестирования конкретной инъекции SQL
```bash
--technique=TECH    Используемый метод SQL инъекции (дефолтно "BEUSTQ")
--time-sec=TIMESEC  Задержка ответа БД в секундах (дефолтно 5)
--union-cols=UCOLS  Диапазон колонок для теста с UNION запросом SQL инъекции
--union-char=UCHAR  Обознанчение для использования брутфорса количества колонок
--union-from=UFROM  Таблица для использования в части FROM запроса UNION
--dns-domain=DNS..  Доменное имя, используемое для атаки DNS эксфильтрации
--second-order=S..  URL итоговой страницы найденной для second-order запроса
```

#### Отпечатки:
```bash
-f, --fingerprint   Получение расширенных данных о версии СУБД по отпечатку
```

#### Перечисление:
Эти параметры могут быть использованы для перечисления серверных баз данных систем управления информации, структур и данных, содержащихся в таблицах. Более того, вы можете запустить свои собственные SQL запросы

```bash
-a, --all           Получить всё
-b, --banner        Получить текстовый банер СУБД (фициальное название, номер версии)
--current-user      Получить текущего пользователя СУБД
--current-db        Получить используемую базу данных
--hostname          Получить имя хоста сервера СУБД
--is-dba            Определить под Админом мы или нет
--users             Перечислить пользователей СУБД
--passwords         Перечислить парольные хэши пользователей СУБД
--privileges        Перечислить привелегии
--roles             Перечислить роли пользователей
--dbs               Перечислить базы данных в СУБД
--tables            Перечислить таблицы текущей БД
--columns           Перечислить колонки текцщей БД
--schema            Перечислить схемы СУБД
--count             Получить кол-во записей в таблицах
--dump              Дамп записей  текущей таблицы БД
--dump-all          Дамп всех таблиц из баз данных в СУБД
--search            Поиск колонок, таблиц и/или имен БД
--comments          Получить комментарии СУБД
-D DB               База данных в СУБД для перечисления
-T TBL              Таблица СУБД для перечисления
-C COL              Колонока таблицы СУБД для перечисления
-X EXCLUDECOL       Не перечислять последующие колонки
-U USER             Пользователь СУБД для перечесления
--exclude-sysdbs    Исключить системные базы данных СУБД при перечислении таблиц
--where=DUMPWHERE   Использовать WHERE, если таблица скрыта
--start=LIMITSTART  Извлекать первую запись результата запроса
--stop=LIMITSTOP    Извлекать последнюю запись результата запроса
--first=FIRSTCHAR   Извлекать первый символ слова в результате запроса
--last=LASTCHAR     Извлекать последний символ слова в результате запроса
--sql-query=QUERY   SQL запросы, которые должны быть выполнены
--sql-shell         Вызов интерактивного SQL shell\'а
--sql-file=SQLFILE  Выполнить SQL запросы из файла(ов)
```

#### Брутфорс:
```bash
Параметры для запуска Brute Force
--common-tables     Проверить наличие общих таблиц
--common-columns    Проверить наличие общих столбцов
```
#### Функции, определённые пользователем:
Эти параметры могут быть использованы для создания пользовательских функций
```bash
--udf-inject        Инжектировать SQL, определенные пользователем
--shared-lib=SHLIB  Локальный путь общей библиотеки
```

#### Доступ к файловой системе:
Эти параметры могут быть использованы для доступа к управлению серверной базой данных при доступе к ФС
```bash
--file-read=RFILE   Прочитать файл из ФС серверной СУБД
--file-write=WFILE  Записать файл в ФС
--file-dest=DFILE   Абсолютный путь для записи файла в серверной СУБД
```

#### Доступ к операционной системе:
Эти параметры могут быть использованы для доступа к управлению серверной базой данных при доступе к ОС сервера.
```bash
--os-cmd=OSCMD      Выполнить команду в командной оболочке ОС
--os-shell          Вызов интерактивной оболочки ОС
--os-pwn            Вызов собственного интерпритатора командной оболочки(out-of-band shell), meterpeter или VNC
--os-smbrelay       Быстрый вызов OBB, meterpeter или VNC
--os-bof            Эксплуатация переполнения буфера
--priv-esc          Повышение привелегий пользовательских процессов, работающих с БД
--msf-path=MSFPATH  Локальный путь, установки Metasploit Framework
--tmp-path=TMPPATH  Абсолютный путь к директории временных файлов
```

#### Доступ к реестру Windows:
Эти параметры могут быть использованы для доступа к реестру Windows серверной ОС
```bash
--reg-read          Прочитать значение ключа реестра
--reg-add           Записать значение ключ реестра
--reg-del           Удалить значение ключа реестра
--reg-key=REGKEY    Ключ реестра
--reg-value=REGVAL  Значение ключа реестра
--reg-data=REGDATA  Данные значения ключа реестра
--reg-type=REGTYPE  Тип значения ключа реестра
```

#### Общее:
Эти параметры могут быть использованы для установки некоторых общих параметров
```bash
-s SESSIONFILE      Згрузить сохраненную сессию из файла (.sqlite)
-t TRAFFICFILE      Записывать весь HTTPтраффик в файл
--batch             Не запрашивать ввода пользователя, действия по умолчанию
--charset=CHARSET   Задать кодировку для извлекаемых данных
--crawl=CRAWLDEPTH  Исследовать веб сайт начиная с заданного URL
--csv-del=CSVDEL    Разделять символы в выводе CSV (дефолтно ",")
--dump-format=DU..  Формат дампа данных (CSV (дефолтно), HTML или SQLITE)
--eta               Показывать расчетное время для каждого вывода
--flush-session     Игнорировать файлы сеансов текущей цели
--forms             Распарсить и тестировать формы на заданном URL
--fresh-queries     Игнорировать результаты запросов, хранящихся в файле сессии
--hex               Использовать функции хеширования СУБД для извлекаемых данных
--output-dir=ODIR   Кастомный пусть для исходящих данных
--parse-errors      Парсить и выводить ошибки
--pivot-column=P..  Имя основного (ключевого) столбца
--save              Сохранить настройки в конфигурационный INI файл
--scope=SCOPE       Регулярка для фильтра целей из предоставленных прокси в файле
--test-filter=TE..  Выбрать тесты на основании полезной нагрузки или заголовков (например, ROW)
--update            Обновить SQLmap
```

#### Дополнительно:
```bash
-z MNEMONICS        Использовать короткие мнемоники (типа "flu,bat,ban,tec=EU")
--alert=ALERT       Запуск консольных команд, когда будетнайдена SQL уязвимость
--answers=ANSWERS   Задать ответы на вопросы (например, "quit=N,follow=N")
--beep              Звуковой сигнал при нахождении уязвимости
--check-waf         Эвристическая проверка WAF/IPS/IDS защиты
--cleanup           Очистка СУБД при помощи SQLmap UDF и таблиц
--dependencies      Проверка отсутствующих зависимостей SQLmap
--disable-coloring  Отключить раскраску консоли
--gpage=GOOGLEPAGE  Указать номер страницы гугла, с которой начнется поиск по доркам
--identify-waf      Провести тестирование для WAF/IPS/IDS защиты
--mobile            Имитировать заголовок User-Agent смартфона из HTTP
--page-rank         Выводить рейтинг страниц для результатов Google дорков
--purge-output      Безопасное удаление всго содержимого из каталога output
--smart             Провести тестирование только при положительном эврестическом анализе
--wizard            Простой интерфейс для начинающих пользователей с запросами для всех действий
```

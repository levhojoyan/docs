---
title: "SQL-запросы в {{ mpg-full-name }}"
description: "{{ mpg-name }} позволяет визуализировать структуру данных на вашем {{ PG }}-кластере и отправлять SQL-запросы к базам из консоли управления {{ yandex-cloud }}. Для этого войдите в консоль управления, откройте страницу нужного кластера и перейдите на вкладку SQL."
---

# SQL-запросы в консоли управления

{{ mpg-name }} позволяет:

* визуализировать структуру данных и планы выполнения запросов на вашем {{ PG }}-кластере;
* отправлять SQL-запросы к базам из консоли управления {{ yandex-cloud }}.

{% include [web-sql-warning](../../_includes/mdb/mch/note-web-sql-console.md) %}

С помощью команд SQL нельзя выполнять действия, требующие прав суперпользователя.

Чтобы подключиться из консоли управления к кластеру {{ mpg-name }} и работать с данными в нем:

1. Перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-postgresql }}**.
1. Нажмите на имя нужного кластера.
1. [Включите опцию](../operations/update.md#change-additional-settings) **{{ ui-key.yacloud.mdb.forms.additional-field-websql }}**, если она еще не включена.
1. Выберите вкладку **{{ ui-key.yacloud.postgresql.cluster.switch_explore }}**.

{% include [web-sql-auth](../../_includes/mdb/web-sql-auth.md) %}

Справочник по поддерживаемым запросам можно найти в [документации {{ PG }}](https://www.postgresql.org/docs/current/sql.html).

## Визуализация структуры данных {#data-structure-visualization}

После авторизации вы можете видеть структуру выбранной базы данных и ее таблиц:

![structure](../../_assets/mdb/pg-web-sql-structure.png)

По нажатию на таблицу выводятся первые 1000 строк результата запроса `SELECT *` для этой таблицы, страницами по 20 строк (полноценную навигацию по всем данным базы консоль управления не поддерживает). В поле **{{ ui-key.yacloud.clickhouse.cluster.explore.label_offset }}** вы можете задать смещение, с которым следует показать таблицу результатов.

Наведите курсор на заголовок столбца, чтобы увидеть тип данных в столбце:

![table](../../_assets/mdb/pg-web-sql-table.png)


## SQL-запросы {#sql-queries}

Справа открыто окно ввода запроса. Начните вводить запрос, и редактор будет предлагать варианты ключевых слов:

![suggest](../../_assets/mdb/pg-web-sql-suggest.png)

Введите запрос и нажмите кнопку **{{ ui-key.yacloud.clickhouse.cluster.explore.button_execute }}**. Таблица результатов или сообщение об ошибке появится на панели результатов, которая находится под кнопками управления редактором.

![result](../../_assets/mdb/pg-web-sql-result.png)

## Анализ запросов {#sql-analyze}

Чтобы получить визуализацию плана выполнения SQL-запроса:

1. Введите запрос.
1. Нажмите на кнопку выпадающего меню рядом с кнопкой **{{ ui-key.yacloud.clickhouse.cluster.explore.button_execute }}**.
1. Выберите способ визуализации:

    * **{{ ui-key.yacloud.clickhouse.cluster.explore.button_explain-analyze }}** — запрос выполняется с помощью `EXPLAIN ANALYZE`. План запроса строится на основе данных, полученных во время выполнения. На вкладках появится точная информация о характеристиках запроса:
        * **{{ ui-key.yacloud.mdb.cluster.explain.value_cost }}** — стоимость частей запроса (в относительных единицах).
        * **{{ ui-key.yacloud.mdb.cluster.explain.value_time }}** — время выполнения всего запроса и его отдельных частей.
        * **{{ ui-key.yacloud.mdb.cluster.explain.value_buffers }}** — сведения об операциях I/O и потреблении RAM для каждой части запроса.
    * **{{ ui-key.yacloud.clickhouse.cluster.explore.button_explain }}** — запрос не выполняется, его план строится с помощью команды `EXPLAIN` на основе статистических данных, собранных {{ PG }}. На вкладке **{{ ui-key.yacloud.mdb.cluster.explain.value_cost }}** выводится примерная оценка стоимости всего запроса и его частей (в относительных единицах).

    В обоих случаях медленные и ресурсоемкие части запроса будут выделены цветом.

    Подробнее см. в документации {{ PG }}:

    * [Единицы стоимости частей запроса](https://www.postgresql.org/docs/current/runtime-config-query.html#RUNTIME-CONFIG-QUERY-CONSTANTS).
    * [Использование `EXPLAIN` и `EXPLAIN ANALYZE`](https://www.postgresql.org/docs/current/using-explain.html).

1. Чтобы просмотреть подробный план выполнения запроса в виде дерева, нажмите на кнопку ![full-screen](../../_assets/full-screen.svg). Для выхода из этого режима нажмите клавишу **Esc**.

    Каждая часть запроса представлена в виде блока с указанием абсолютного и относительного времени выполнения. Если часть запроса выполняется значительно дольше остальных или использует ресурсоемкие операции, блок будет отмечен ярлыками с указанием причины.

    Чтобы получить более подробную информацию о части запроса, нажмите на нужный блок — в нем появится панель с вкладками:

    * **Stats** — стоимость выполнения (в относительных единицах);
    * **I/O & Buffers** — сведения об операциях I/O и потреблении RAM;
    * **Misc** —общая информация о запросе, например, список задействованных полей;
    * **Info** — дополнительная информация, например, имена использованных таблиц и индексов.

## Ограничения запросов в консоли управления {#query-restrictions-in-the-management-console}

* Если вы закроете или перезагрузите страницу, текст запроса и его результаты будут потеряны. При этом каждый запрос, который вы запустили из консоли управления, будет выполнен независимо от состояния браузера.
* Консоль управления выведет только первые 1000 строк результата.
* Если выполнение запроса на кластере длится больше 10 минут, консоль управления сообщит об ошибке и не выведет результат, даже если запрос в конечном счете будет успешно обработан.
* Если в вашем кластере больше одного хоста {{ PG }}, то запросы из консоли управления отправляются на хост, который в момент запроса является мастером.
* Список таблиц берется из схемы _public_. Запросы к таблицам из других схем можно делать, явно указав схему, например: `SELECT * from information_schema.column_udt_usage`.

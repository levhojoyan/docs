# О сервисе {{ websql-full-name }}

{% include notitle [preview](../../_includes/note-preview-by-request.md) %}

{{ websql-full-name }} — это сервис {{ yandex-cloud }}, который позволит вам подключаться к кластерам управляемых баз данных, работать с базами данных, таблицами и схемами и выполнять SQL-запросы. Сервис работает в браузере и предлагает удобные подсказки для выполнения SQL-запросов.

## Пользовательский интерфейс

Для работы с {{ websql-full-name }} используйте:

* _Панель управления_ — крайне левая панель с иконками **Диспетчер соединений** ![image](../../_assets/websql/connections.svg), **Сохраненные запросы** ![image](../../_assets/websql/template.svg) и **История запросов** ![image](../../_assets/websql/history.svg).
* _Диспетчер соединений_ — панель просмотра существующих и добавления новых соединений баз данных.
* _Панель данных_ — крайне правая панель, на которой можно формировать SQL-запросы и просматривать результаты их выполнения, а также, просматривать настройки соединения.  

## Диспетчер соединений {#connection}

В {{ websql-full-name }} можно устанавливать, обновлять и контролировать статус соединения с базами данных. Соединения баз данных с одинаковыми URL и номером порта объединяются в _группы соединений_. Вы можете редактировать настройки соединения, переименовывать или удалять группы. После установления соединения с базой данных, вы можете просматривать таблицы, схемы и представления (`VIEW`).

В диспетчере соединений можно работать с [демо-соединениями](../operations/connect.md#demo) — предустановленными соединениями с тестовыми базами данных для знакомства с возможностями {{ websql-full-name }}.

## SQL-Запросы {#tables}

Для каждой базы данных можно выполнять различные SQL-запросы с использованием подсказок: начните вводить команду запроса и выбирайте подходящую из списка. Вы также можете использовать готовые _шаблоны_ или сохранять свои запросы в виде _пользовательских шаблонов_ запросов. 

С помощью _истории запросов_ можно посмотреть, какие запросы вы запускали ранее и повторить их.
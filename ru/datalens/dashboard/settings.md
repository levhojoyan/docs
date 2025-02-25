# Настройки дашборда



## Режимы отображения {#display-modes}

{{ datalens-short-name }} позволяет отображать дашборд в полноэкранном режиме. Этот режим скрывает часть интерфейсных элементов и увеличивает область отображения виджетов на экране.

При просмотре мобильной версии дашборда {{ datalens-short-name }} по умолчанию отображает чарты друг за другом по следующему правилу сортировки: слева направо, сверху вниз. Порядок отображения чартов и селекторов в мобильной версии или в рассылках можно изменить в настройках вкладок (подробнее в разделе [{#T}](../operations/dashboard/display-modes.md)).

## Настройки описания и сообщений {#message-settings}

Вы можете [добавить описание](../operations/dashboard/add-description.md) к дашборду. Чтобы посмотреть описание, нажмите значок ![image](../../_assets/datalens/info.svg) в правом верхнем углу экрана.

Также вы можете настроить дополнительные информационные сообщения:

* при [обращении в поддержку](../operations/dashboard/add-support-message.md). Тогда после нажатия на значок ![image](../../_assets/datalens/question.svg) в левом нижнем углу экрана и выбора **Создать обращение** пользователь увидит в окне **Информация** дополнительное сообщение.
* при [ошибке доступа к дашборду](../operations/dashboard/add-access-message.md). В этом случае, если у пользователя нет прав на просмотр дашборда, он увидит сохраненное сообщение.


## Автообновление {#auto-update}

Вы можете настроить [автоматическое обновление](../operations/dashboard/auto-update.md) данных на дашборде. Интервал обновления указывается в секундах, минимальное значение — 30 секунд. Настройка глобальная: после сохранения дашборда автообновление будет работать у всех пользователей, которые его откроют. Автообновление также работает в мобильной версии.

При использовании автообновления дашборда существуют следующие ограничения:

* обновляются данные только для открытой в браузере вкладки;
* обновляются данные только для текущей вкладки, которая считается активной, при этом:

  * если вкладка не выбрана как текущая, она не считается активной и данные не обновляются;
  * если вкладка выбрана как текущая, но сам браузер работает в фоновом режиме, вкладка считается активной и данные обновляются.


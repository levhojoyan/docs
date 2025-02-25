# Жизненные циклы объектов в бакете

{{ objstorage-name }} позволяет определить действия, которые автоматически производятся с отдельными объектами или группами объектов в бакете в заданные моменты времени.

Виды действий:

* Изменение [класса хранилища](storage-class.md) объектов или их неактивных [версий](versioning.md) на более «холодный».  
* Удаление объектов или их неактивных версий.
* Удаление незавершенных составных загрузок.

Фильтры для группировки объектов:

* Префикс ключа объекта.
* Минимальный или максимальный размер объекта — с помощью консоли управления, [S3 API](../s3/index.md) или [инструментов](../tools/index.md) с его поддержкой. 

Если вы используете S3 API, задайте конфигурацию жизненного цикла в [формате XML](../s3/api-ref/lifecycles/xml-config.md). Для различных инструментов с поддержкой S3 API могут понадобиться другие форматы конфигурации, например для AWS CLI применяется формат JSON.

Задать конфигурацию жизненных циклов объектов можно только для каждого отдельного бакета. Не получится задать конфигурацию жизненных циклов для группы бакетов, каталога или облака.

Раз в сутки к жизненным циклам применяются изменения, актуальные на момент 00:00 UTC. Операция выполняется в течение нескольких часов.

#### См. также {#see-also}

* [{#T}](../operations/buckets/lifecycles.md)

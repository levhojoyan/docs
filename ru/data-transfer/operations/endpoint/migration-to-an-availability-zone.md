# Миграция эндпоинтов и трансфера {{ data-transfer-name }} в другую зону доступности

[Трансферы](../../concepts/index.md#transfer) размещаются в одной [зоне доступности](../../../overview/concepts/geo-scope.md). Их можно перенести из одной зоны в другую. Для переноса выполняются различные действия в зависимости от того, какой сервис выступает в виде эндпоинта в трансфере: [пользовательская инсталляция БД](#on-premise) или [управляемая БД {{ yandex-cloud }}](#managed-service). Если вы используете [миграцию данных](../../tutorials/index.md#migration) между пользовательской инсталляцией и управляемой БД, сначала перенесите эндпоинт на основе пользовательской инсталляции, затем — на основе управляемой БД.

## Перенести эндпоинт на основе пользовательской инсталляции {#on-premise}

1. Если [тип вашего трансфера](../../concepts/transfer-lifecycle.md#transfer-types) — {{ dt-type-repl }} или {{ dt-type-copy-repl }}, [деактивируйте](../transfer.md#deactivate) трансфер и дождитесь его перехода в статус {{ dt-status-stopped }}.
1. [Создайте подсеть](../../../vpc/operations/subnet-create.md) в зоне доступности, в которую вы переносите эндпоинт.
1. Если пользовательская инсталляция установлена на виртуальной машине {{ yandex-cloud }}, выполните действия:

   
   1. [Остановите ВМ](../../../compute/operations/vm-control/vm-stop-and-start.md#stop).
   1. [Перенесите ВМ в другую зону доступности](../../../compute/operations/vm-control/vm-change-zone.md).
   1. [Запустите ВМ](../../../compute/operations/vm-control/vm-stop-and-start.md#start).


1. [Укажите новую подсеть](index.md#update) в настройках эндпоинта с пользовательской инсталляцией.
1. Если вы деактивировали трансфер ранее, [активируйте](../transfer.md#activate) его и дождитесь перехода трансфера в статус {{ dt-status-repl }}.

   {% note warning %}

   Если ваш трансфер включает в себя эндпоинт на основе управляемой БД, перед активацией трансфера [перенесите](#managed-service) этот эндпоинт в другую зону доступности.

   {% endnote %}

## Перенести эндпоинт на основе управляемой базы данных {#managed-service}

1. Перенесите хосты кластера в другую зону доступности. Подробнее см. в документации сервисов:

   * [{{ mch-full-name }}](../../../managed-clickhouse/operations/host-migration.md);
   * [{{ mes-full-name }}](../../../managed-elasticsearch/operations/host-migration.md);
   * [{{ mgp-full-name }}](../../../managed-greenplum/operations/cluster-backups.md#restore);
   * [{{ mmg-full-name }}](../../../managed-mongodb/operations/host-migration.md);
   * [{{ mmy-full-name }}](../../../managed-mysql/operations/host-migration.md);
   * [{{ mos-full-name }}](../../../managed-opensearch/operations/host-migration.md);
   * [{{ mpg-full-name }}](../../../managed-postgresql/operations/host-migration.md).

   Не размещайте все хосты кластера в зоне `{{ region-id }}-d`, иначе трансфер не будет работать корректно. Если вы переносите хосты в зону `{{ region-id }}-d`, расположите хотя бы один хост в зоне `{{ region-id }}-a` или `{{ region-id }}-b`. Трансфер автоматически выберет подходящую зону. Если в вашем кластере один хост, разместите его в зоне `{{ region-id }}-a` или `{{ region-id }}-b`.

1. Если [тип вашего трансфера](../../concepts/transfer-lifecycle.md#transfer-types) — {{ dt-type-repl }} или {{ dt-type-copy-repl }}, перезапустите трансфер, чтобы он получил сведения о новой топологии кластера. Трансферы типа {{ dt-type-copy }} перезапускать не нужно, так как во время их активации информация о новой топологии передается автоматически.

   {% include [reactivate-a-transfer](../../../_includes/data-transfer/reactivate-a-transfer.md) %}

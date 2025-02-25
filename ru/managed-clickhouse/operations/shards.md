# Управление шардами в кластере {{ CH }}

Вы можете включить шардирование для кластера, а также добавлять и настраивать отдельные шарды.

## Включить шардирование {#enable}

Кластеры {{ mch-name }} создаются с одним шардом. Чтобы начать непосредственно шардирование данных, [добавьте](#add-shard) еще один или несколько шардов и [создайте](../tutorials/sharding.md#example) распределенную таблицу.

## Добавить шард {#add-shard}

Количество шардов в кластерах {{ mch-name }} ограничено квотами на количество CPU и объем памяти, которые доступны кластерам БД в вашем облаке. Чтобы проверить используемые ресурсы, откройте страницу [Квоты]({{ link-console-quotas }}) и найдите блок **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Нажмите на имя нужного кластера и перейдите на вкладку **{{ ui-key.yacloud.clickhouse.cluster.switch_shards }}**.
  1. Нажмите кнопку **{{ ui-key.yacloud.mdb.cluster.shards.button_add }}**.
  1. Укажите параметры шарда:
     * Имя и вес.
     * Чтобы скопировать схему со случайной реплики одного из шардов на хосты нового шарда, выберите опцию **{{ ui-key.yacloud.mdb.forms.field_copy_schema }}**.
     * Нужное количество хостов.
  1. Нажмите кнопку **{{ ui-key.yacloud.mdb.forms.button_create-shard }}**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы добавить шард в кластер, выполните команду (в примере приведены не все доступные параметры):

  ```bash
  {{ yc-mdb-ch }} shards add <имя_нового_шарда> \
    --cluster-name=<имя_кластера> \
    --host zone-id=<зона_доступности>,`
      `subnet-name=<имя_подсети>
  ```

  Где:

  
  * `<имя_нового_шарда>` — должно быть уникальным в кластере.

    Может содержать латинские буквы, цифры, дефис и нижнее подчеркивание. Максимальная длина — 63 символа.
  * `--cluster-name` — имя кластера.

    Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).
  * `--host` — параметры хоста:
    * `zone-id` — [зона доступности](../../overview/concepts/geo-scope.md).
    * `subnet-name` — [имя подсети](../../vpc/concepts/network.md#subnet).


- {{ TF }}

  {% note info %}

  {{ TF }} не позволяет указывать вес шардов.

  {% endnote %}

  1. Откройте актуальный конфигурационный файл {{ TF }} с планом инфраструктуры.

     О том, как создать такой файл, см. в разделе [Создание кластера](cluster-create.md).
  1. Добавьте к описанию кластера {{ mch-name }} блок `host` типа `CLICKHOUSE` с заполненным полем `shard_name` или измените существующие хосты:

     ```hcl
     resource "yandex_mdb_clickhouse_cluster" "<имя_кластера>" {
       ...
       host {
         type       = "CLICKHOUSE"
         zone       = "<зона_доступности>"
         subnet_id  = yandex_vpc_subnet.<подсеть_в_зоне_доступности>.id
         shard_name = "<имя_шарда>"
       }
     }
     ```

  1. Проверьте корректность настроек.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Подтвердите изменение ресурсов.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  Подробнее см. в [документации провайдера {{ TF }}]({{ tf-provider-resources-link }}/mdb_clickhouse_cluster).

  {% include [Terraform timeouts](../../_includes/mdb/mch/terraform/timeouts.md) %}

- API

  Чтобы добавить шард в кластер, воспользуйтесь методом REST API [addShard](../api-ref/Cluster/addShard.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/AddShard](../api-ref/grpc/cluster_service.md#AddShard).

  Чтобы скопировать схему данных со случайной реплики одного из шардов на хосты нового шарда, передайте в запросе параметр `copySchema` со значением `true`.

{% endlist %}

{% note warning %}

Используйте опцию копирования схемы данных только в том случае, когда схема одинакова на всех шардах кластера.

{% endnote %}

## Получить список шардов в кластере {#list-shards}

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **{{ ui-key.yacloud.clickhouse.cluster.switch_shards }}**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы получить список шардов в кластере, выполните команду:

  ```bash
  {{ yc-mdb-ch }} shards list --cluster-name=<имя_кластера>
  ```

  Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- API

  Чтобы получить список шардов кластера, воспользуйтесь методом REST API [listShards](../api-ref/Cluster/listShards.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/ListShards](../api-ref/grpc/cluster_service.md#ListShards).

{% endlist %}

## Изменить шард {#shard-update}

Вы можете изменить вес шарда, а также [класс хоста](../concepts/instance-types.md) и размер хранилища.

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Нажмите на имя нужного кластера, затем выберите вкладку **{{ ui-key.yacloud.clickhouse.cluster.switch_shards }}**.
  1. Нажмите ![horizontal-ellipsis](../../_assets/horizontal-ellipsis.svg) и выберите пункт **{{ ui-key.yacloud.mdb.cluster.shards.button_action-edit }}**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы изменить шард в кластере:
  1. Посмотрите описание команды CLI для изменения шарда:

     ```bash
     {{ yc-mdb-ch }} shards update --help
     ```

  1. Запустите операцию, например, изменения веса для шарда:

     ```bash
     {{ yc-mdb-ch }} shards update <имя_шарда> \
       --cluster-name=<имя_кластера> \
       --weight=<вес_шарда>
     ```

     Где:
     * `<имя_шарда>` — можно запросить со [списком шардов в кластере](#list-shards).
     * `--cluster-name` — имя кластера.

       Имя кластера можно запросить со [списком кластеров в каталоге](cluster-list.md#list-clusters).
     * `--weight` — вес шарда. Минимальное значение — `0`.

     После успешного завершения операции CLI выведет информацию об измененном шарде.

- API

  Чтобы изменить шард, воспользуйтесь методом REST API [updateShard](../api-ref/Cluster/updateShard.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/UpdateShard](../api-ref/grpc/cluster_service.md#UpdateShard) и передайте в запросе:
  * Идентификатор кластера в параметре `clusterId`. Чтобы узнать идентификатор, [получите список кластеров в каталоге](cluster-list.md#list-clusters).
  * Имя шарда в параметре `shardName`.
  * Настройки шарда в параметре `configSpec`.
  * Список настроек, которые необходимо изменить, в параметре `updateMask`.

  {% include [Note API updateMask](../../_includes/note-api-updatemask.md) %}

{% endlist %}

## Удалить шард {#delete-shard}

Вы можете удалить шард из кластера {{ CH }}, если он не является:
* Единственным шардом.
* Единственным шардом в [группе шардов](shard-groups.md).

Удаление шарда приведет к удалению всех таблиц и данных, которые находятся на этом шарде.

{% list tabs %}

- Консоль управления

  1. В [консоли управления]({{ link-console-main }}) перейдите на страницу каталога и выберите сервис **{{ ui-key.yacloud.iam.folder.dashboard.label_managed-clickhouse }}**.
  1. Нажмите на имя нужного кластера и выберите вкладку **{{ ui-key.yacloud.clickhouse.cluster.switch_shards }}**.
  1. Нажмите на значок ![image](../../_assets/horizontal-ellipsis.svg) в строке нужного хоста и выберите пункт **{{ ui-key.yacloud.mdb.cluster.shards.button_action-remove }}**.

- CLI

  {% include [cli-install](../../_includes/cli-install.md) %}

  {% include [default-catalogue](../../_includes/default-catalogue.md) %}

  Чтобы удалить шард из кластера, выполните команду:

  ```bash
  {{ yc-mdb-ch }} shards delete <имя_шарда> \
    --cluster-name=<имя_кластера>
  ```

  Имя шарда можно запросить со [списком шардов в кластере](#list-shards), имя кластера — со [списком кластеров в каталоге](cluster-list.md#list-clusters).

- {{ TF }}

  1. Откройте актуальный конфигурационный файл {{ TF }} с планом инфраструктуры.

     О том, как создать такой файл, см. в разделе [Создание кластера](cluster-create.md).
  1. Удалите из описания кластера {{ mch-name }} блок `host` с описанием шарда.
  1. Проверьте корректность настроек.

     {% include [terraform-validate](../../_includes/mdb/terraform/validate.md) %}

  1. Введите слово `yes` и нажмите **Enter**.

     {% include [terraform-apply](../../_includes/mdb/terraform/apply.md) %}

  Подробнее см. в [документации провайдера {{ TF }}]({{ tf-provider-resources-link }}/mdb_clickhouse_cluster).

  {% include [Terraform timeouts](../../_includes/mdb/mch/terraform/timeouts.md) %}

- API

  Чтобы удалить шард, воспользуйтесь методом [deleteShard](../api-ref/Cluster/deleteShard.md) для ресурса [Cluster](../api-ref/Cluster/index.md) или вызовом gRPC API [ClusterService/DeleteShard](../api-ref/grpc/cluster_service.md#DeleteShard).

{% endlist %}
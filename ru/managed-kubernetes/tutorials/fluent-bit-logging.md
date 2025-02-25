---
title: "Настройка Fluent Bit для работы с {{ cloud-logging-full-name }}"
description: "Обработчик логов Fluent Bit позволяет транслировать логи кластера {{ managed-k8s-name }} в сервис {{ cloud-logging-name }}. Для передачи логов используется модуль Fluent Bit plugin for {{ cloud-logging-full-name }}."
---

# Настройка Fluent Bit для работы с {{ cloud-logging-full-name }}

{% note info %}

Включить отправку логов [мастера](../concepts/index.md#master) кластера {{ managed-k8s-name }} в [{{ cloud-logging-full-name }}](../../logging/) можно с помощью настройки `master logging` при [создании](../operations/kubernetes-cluster/kubernetes-cluster-create.md) или [изменении](../operations/kubernetes-cluster/kubernetes-cluster-update.md) кластера. Настройка доступна только в CLI, {{ TF }} и API. Подробнее см. в руководстве [Передача логов кластера {{ managed-k8s-full-name }} в {{ cloud-logging-name }}](../../logging/tutorials/k8s-fluent-bit-logging.md#master-logging).

{% endnote %}

Обработчик логов [Fluent Bit](https://fluentbit.io/) позволяет транслировать логи [подов](../concepts/index.md#pod) и [сервисов](../concepts/index.md#service) {{ managed-k8s-name }} в {{ cloud-logging-name }}. Для передачи логов используется модуль [Fluent Bit plugin for {{ cloud-logging-full-name }}](https://github.com/yandex-cloud/fluent-bit-plugin-yandex).

{% include notitle [Настройка Managed K8S Fluent Bit](../../_tutorials/k8s-fluent-bit-logging.md) %}

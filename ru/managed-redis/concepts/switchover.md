---
title: "Управление отказоустойчивостью в {{ mrd-full-name }}"
description: "Для обеспечения отказоустойчивости в кластере без шардирования используется {{ RD }} Sentinel. В кластере с шардированием используется кворум хостов-мастеров и обнаружение отказов на основе алгоритма Gossip." 
---

# Управление отказоустойчивостью

Для обеспечения отказоустойчивости {{ RD }}:

* В кластере без [шардирования](../../glossary/sharding.md) используется {{ RD }} Sentinel: сервисы Sentinel выполняют мониторинг состояния мастера и реплик, уведомляют о нарушениях в работе хостов, управляют выбором нового мастера и реконфигурацией реплик.

   Sentinel применяется только для кластеров с {{ RD }} версии 6.2.

* В шардированном кластере используется кворум хостов-мастеров и обнаружение отказов на основе [алгоритма Gossip](https://web.stanford.edu/~boyd/papers/gossip_infocom.html).

Сами по себе эти способы не обеспечивают полной отказоустойчивости:

* При нарушении сетевой связности возможна ситуация, когда 2 хоста будут доступны для записи в изолированной сети или одном шарде.
* Для выбора нового мастера необходимы как минимум 3 хоста в разных зонах доступности.
* Приоритет реплики при выборе мастера более важен, чем отставание от мастера, поэтому возможна потеря данных даже при использовании [команды WAIT](https://redis.io/commands/wait/).

Чтобы повысить отказоустойчивость, для {{ RD }} версии 7.0 в архитектуру {{ mrd-name }} интегрирован агент управления состоянием хоста `rdsync`, разработанный Яндексом. Состояние хоста хранится в распределенной системе управления конфигурацией. При потере соединения с хранилищем DCS (Distributed Configuration Store, например, {{ ZK }}, etcd, Consul) агент переводит хост в режим [protected mode]({{ rd.docs }}/manual/security/#protected-mode), а клиентские соединения разрываются. Если для хоста-реплики с наибольшим приоритетом требуется полная ресинхронизация данных, при выборе нового мастера агент останавливает репликацию и выбирает хост с наименьшим отставанием от мастера.

Благодаря агенту `rdsync` в кластере с {{ RD }} 7.0:

* Конфигурации из четного числа хостов (если кластер нешардированный) или одного-двух шардов (если кластер шардированный) являются отказоустойчивыми.

* Обработка [клиентского запроса]({{ rd.docs }}/reference/sentinel-clients/) имени хоста, доступного для записи, работает согласованно с агентом `rdsync` и выдает актуальную информацию клиентам, т. к. состояние всех хостов известно.

* Исключается потеря данных при использовании `WAIT` с числом доступных реплик `N/2`, где `N` – число хостов в кластере.

---
title: "Инструкция о создании платежного аккаунта для юридических лиц в {{ yandex-cloud }}"
description: "Из статьи вы узнаете, как создать платежный аккаунт юридическому лицу в {{ yandex-cloud }}. Отвечаем на частые вопросы: платежный аккаунт и платное потребление; стартовый грант; документы."
---

# Начало работы для юридических лиц

{% include notitle [start](../_includes/quickstart-start.md) %}

## Создание платежного аккаунта {#new-account}

Платежный аккаунт необходим, даже если вы планируете пользоваться только бесплатными сервисами. При создании первого платежного аккаунта, привязанного к пользовательскому аккаунту, вам будет начислен [стартовый грант](../usage-grant.md).

{% list tabs group=versions %}

   - Пробный период {#trial}

      ![quickstart](../../_assets/overview/legal-entity-trial-period.svg)

   - Платная версия {#paid}

      ![quickstart](../../_assets/overview/legal-entity-paid-version.svg)

{% endlist %}

{% include [main](../../_includes/billing/registration-main.md) %}

Предоставьте данные для создания платежного аккаунта:

1. Выберите способ оплаты: **{{ ui-key.yacloud.billing.account.overview.payment-type_label_card }}** или **{{ ui-key.yacloud.billing.account.overview.payment-type_label_invoice }}**. В любой момент после создания платежного аккаунта вы можете [изменить способ оплаты](../../billing/operations/change-payment-method.md).

{% list tabs group=payments %}

- Банковская карта {#card}

   1. Укажите юридическую информацию о вашей организации.

         {% include [contacts-note](../../_includes/billing/contacts-note.md) %}

   1. Привяжите корпоративную банковскую карту:

      {% include [pin-card-data](../../_includes/billing/pin-card-data.md) %}

      * Подтвердите, что карта является корпоративной и вы уполномочены ею распоряжаться.

      {% include [payment-card-types](../../_includes/billing/payment-card-types.md) %}

      {% include [yandex-account](../../_includes/billing/payment-card-validation.md) %}

   1. Укажите актуальные почту и телефон. Контактные данные нужны не только для связи с вами, но и для выставления счетов и финансовых документов.

   1. Если это ваш первый платежный аккаунт в {{ yandex-cloud }}, вам доступно подключение [пробного периода](../free-trial/concepts/quickstart.md).

      {% note info %}

      В некоторых случаях при создании платежного аккаунта с пробным периодом может потребоваться дополнительная верификация. Сообщение с подробной инструкцией появится на странице этого платежного аккаунта в консоли управления.

      {% endnote %}

      * Подключая пробный период, помните, что после его завершения ваши ресурсы будут приостановлены. Для возобновления работы потребуется перейти на [платную версию](../free-trial/concepts/upgrade-to-paid.md).
      * Если не подключать пробный период на данном этапе, ваш аккаунт будет создан с платным потреблением: после [использования стартового гранта](../usage-grant.md) вам не придется переходить на платную версию.

   1. Нажмите кнопку **{{ ui-key.yacloud.common.create }}**.

- Банковский перевод {#transfer}

   1. Укажите юридическую информацию о вашей организации и ваши контактные данные.

      {% include [contacts-note](../../_includes/billing/contacts-note.md) %}

   1. Нажмите кнопку **{{ ui-key.yacloud.common.create }}**.

   Вы получите письмо с описанием дальнейших действий на почту, указанную в пользовательском аккаунте. Активация платежного аккаунта может занять до трех рабочих дней.

   Если это ваш первый платежный аккаунт в {{ yandex-cloud }}, он будет создан в пробном периоде. Чтобы продолжить пользоваться ресурсами после завершения пробного периода, не забудьте перейти на [платную версию](../../billing/operations/activate-commercial.md). 

{% endlist %}


{% note info %}

{% include [offices-and-foreign-companies](../../_includes/billing/offices-and-foreign-companies.md) %}

{% endnote %}


{% include [start](../_includes/quickstart-qa-whats-next.md) %}
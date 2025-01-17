# Создание ресурса

Чтобы создать [ресурс](../../concepts/resource.md):

{% list tabs %}

- Консоль управления
  
  1. В [консоли управления]({{ link-console-main }}) выберите каталог, в котором нужно создать ресурс.

  1. Выберите сервис **{{ cdn-name }}**.

  1. На вкладке **CDN-ресурсы** нажмите кнопку **Создать ресурс**.

  1. В блоке **Контент** введите **Доменное имя источника** или выберите **Группу источников**.
  
     Вы можете создать новую группу источников:

     1. Нажмите кнопку **Создать новую**.
     1. Введите **Название группы**.
     1. Введите **Доменное имя источника** и выберите **Основной** или **Резервный**.
     1. Добавьте другие источники, если необходимо.
     1. Нажмите кнопку **Создать**. В поле **Группа источников** вы увидите название созданной группы.
     
     Подробнее см. в разделе [{#T}](../../concepts/origins.md).

  1. В блоке **Доменные имена для раздачи контента** введите **Доменное имя**. Вы можете добавить более одного **Доменного имени**. Первое имя считается основным.

     {% note warning %}

     После создания ресурса изменить основное имя будет невозможно.

     {% endnote %}
      
     В настройках вашего DNS-хостинга создайте для указанных доменных имен CNAME-записи со значением, которое отображается внизу блока **Доменные имена для раздачи контента**. Подробнее см. в разделе [{#T}](../../concepts/resource.md#hostnames).

  1. В блоке **Дополнительно**:

      1. Выберите **Протокол для источников**.
      1. Включите или выключите **Доступ конечных пользователей к контенту**.
      1. Если вы выбрали только протокол HTTP, то в поле **TLS-сертификат** выберите **Не использовать**. В других случаях выберите сертификат **Let's Encrypt®**. Подробнее см. в разделе [{#T}](../../concepts/clients-to-servers-tls.md).
      1. Выберите значение **Заголовка Host** или выберите **Свое значение** и введите **Значение заголовка**. Подробнее см. в разделе [{#T}](../../concepts/servers-to-origins-host.md).

  1. Нажмите кнопку **Создать**.


- Terraform

  Поставщик CDN должен быть активирован до использования ресурсов CDN. Сделать это можно в [консоли управления]({{ link-console-main }}) или с помощью команды [YC CLI](../../../cli/quickstart.md):

  ```
  yc cdn provider activate --folder-id <идентификатор каталога> --type gcore
  ```

  Если у вас ещё нет Terraform, [установите его и настройте провайдер {{ yandex-cloud }}](../../../solutions/infrastructure-management/terraform-quickstart.md#install-terraform).

  1. Опишите в конфигурационном файле параметры создаваемого CDN-ресурса:

      * `cname` — основное доменное имя для раздачи контента. Обязательный параметр.
      * `active` — флаг, указывающий на доступ к контенту для конечных пользователей. `True` — контент из CDN будет доступен клиентам. Необязательный параметр, значение по умолчанию: `true`.
      * `origin_protocol` — протокол для источников. Необязательный параметр, значение по умолчанию: `http`.
      * `secondary_hostnames` — дополнительные доменные имена. Необязательный параметр.
      * `origin_group_id` — идентификатор [группы источников](../../concepts/origins.md). Обязательный параметр. Используйте идентификатор из описания группы источников в ресурсе `yandex_cdn_origin_group`.

      Пример структуры конфигурационного файла:

      ```hcl
      terraform {
        required_providers {
          yandex = {
            source  = "yandex-cloud/yandex"
            version = "0.69.0"
          }
        }
      }

      provider "yandex" {
        token     = "<OAuth>"
        cloud_id  = "<идентификатор облака>"
        folder_id = "<идентификатор каталога>"
        zone      = "<зона доступности>"
      }

      resource "yandex_cdn_resource" "my_resource" {
          cname               = "cdn1.yandex-example.ru"
          active              = false
          origin_protocol     = "https"
          secondary_hostnames = ["cdn-example-1.yandex.ru", "cdn-example-2.yandex.ru"]
          origin_group_id     = yandex_cdn_origin_group.my_group.id
      }
      ```

      Более подробную информацию о параметрах `yandex_cdn_resource` в Terraform см. в [документации провайдера](https://registry.terraform.io/providers/yandex-cloud/yandex/latest/docs/resources/cdn_resource).

  1. В командной строке перейдите в папку, где расположен конфигурационный файл Terraform.

  1. Проверьте конфигурацию командой:
     ```
     terraform validate
     ```
     
     Если конфигурация является корректной, появится сообщение:
     
     ```
     Success! The configuration is valid.
     ```

  1. Выполните команду:
     ```
     terraform plan
     ```
  
     В терминале будет выведен список ресурсов с параметрами. На этом этапе изменения не будут внесены. Если в конфигурации есть ошибки, Terraform на них укажет.

  1. Примените изменения конфигурации:
     ```
     terraform apply
     ```
     
  1. Подтвердите изменения: введите в терминал слово `yes` и нажмите **Enter**.

     Проверить изменение CDN-ресурса можно в [консоли управления]({{ link-console-main }}) или с помощью команды [CLI](../../../cli/quickstart.md):

     ```
     yc cdn resource list
     ```

{% endlist %}

{% note info %}

Чтобы остановить работу созданного ресурса, [отключите доступ](disable-resource.md) конечных пользователей к контенту. Удаление ресурса через консоль управления пока невозможно — для этого нужно [обратиться в техническую поддержку](../../../support/overview.md).

{% endnote %}

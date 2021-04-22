# Создание подключения к Google Sheets


## Подключение к Google Sheets {#google-sheets-connection}

Чтобы создать подключение к Google Sheets:
1. Перейдите на [страницу подключений](https://datalens.yandex.ru/connections).
1. Нажмите кнопку **Создать подключение**.
1. Выберите коннектор **Google Sheets**.
1. Укажите параметры подключения:
    - **Название подключения**. Задайте имя подключения. Имя может быть произвольным.
    - **Ссылка**. Укажите ссылку на нужный лист таблицы из Google Sheets. Для этого в настройках доступа Google Sheets выберите `Разрешить доступ всем, у кого есть ссылка` (`Anyone with the link`) и скопируйте ссылку на нужный лист из адресной строки браузера.
     
1. Нажмите **Создать**. Подключение появится в списке.

{% note info %}

При загрузке данных из Google Sheets используется кэш с временем жизни до 5 минут, поэтому обновление данных в {{ datalens-short-name }} может происходить с задержкой.

{% endnote %}
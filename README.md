# Виртуальная стажировка
## MountainPass REST API
Разработка мобильного приложения для Android и IOS, которое упростило бы туристам задачу по отправке данных о горных перевалах.

Пользоваться мобильным приложением будут туристы. В горах они будут вносить данные о перевале в приложение и отправлять их в ФСТР (Федерации спортивного туризма России [pereval.online](https://pereval.online/ )), как только появится доступ в Интернет.

Модератор из федерации будет верифицировать и вносить в базу данных информацию, полученную от пользователей, а те в свою очередь смогут увидеть в мобильном приложении статус модерации и просматривать базу с объектами, внесёнными другими.
>для пользователя в мобильном приложении будут доступны следующие действия:

    Внесение информации о новом объекте (перевале) в карточку объекта.
    Редактирование в приложении неотправленных на сервер ФСТР данных об объектах. На перевале не всегда работает Интернет.
    Заполнение ФИО и контактных данных (телефон и электронная почта) с последующим их автозаполнением при внесении данных о новых объектах.
    Отправка данных на сервер ФСТР.
    Получение уведомления о статусе отправки (успешно/неуспешно).
    Согласие пользователя с политикой обработки персональных данных в случае нажатия на кнопку «Отправить» при отправке данных на сервер.

   > C помощью мобильного приложения пользователь будет передавать в ФСТР следующие данные о перевале:

    координаты перевала и его высота;
    имя пользователя;
    почта и телефон пользователя;
    название перевала;
    Фотографии перевала.

  После этого турист нажимает кнопку «Отправить» в мобильном приложении.
  Мобильное приложение вызовет метод
  ***submitData***:
  ***
  GET /submitData/ - получает и выводит информацию о всех записях (перевалах).
  ***
  POST /submitData/ - заявка на внесение информации об одном горном перевале
  
  Пример JSON с информацией о перевале

  > {
            "id": 5,
            "beauty_title": "пер.",
            "title": "Riffltor",
            "other_titles": "433.Альпы",
            "connect": "ледн. Karlingerkees - ледн. Pasterzenboden",
            "add_time": "2023-11-30T15:26:32.837387Z",
            "user": {
                "email": "user@example.com",
                "phone": "89998887766",
                "fam": "Конюхов",
                "name": "Федор",
                "otc": "Федорович"
            },
            "coords": {
                "latitude": 47.12173,
                "longitude": 12.68298,
                "height": 3100
            },
            "level": {
                "winter": "1b",
                "summer": "2b",
                "autumn": "1b",
                "spring": "1b"
            },
            "images": [
                {
                    "image": "https://pereval.online/imagecache/original/object/images/2019/11/27/9bf9a5-94493.jpg",
                    "title": "title"
                }
            ],
            "status": "NW"
        }


  > Результат метода: JSON

* status — код HTTP, целое число:
    * 500 — ошибка при выполнении операции;
    * 400 — Bad Request (при нехватке полей);
    * 200 — успех.
* message — строка:
    * Причина ошибки (если она была);
    * Отправлено успешно;
    * Если отправка успешна, дополнительно возвращается id вставленной записи.
* id — идентификатор, который был присвоен объекту при добавлении в базу данных.
  ***
  GET /submitData/{id} - получение данных о конкретном горном перевале с выводом всей информации
  ***
  PATCH /submitData/{id} - позволяет отредактировать существующую запись (замена), при условии, что она в статусе "new".
  Редактировать можно все поля, кроме тех, что содержат ФИО, адрес почты и номер телефона.
  ***
  GET /api/submitData/user_id__email=<str:email> - позволяет получить данные всех объектов, отправленных на сервер пользователем с почтой <***str:email***>.
  Фильтрация по адресу электронной почты реализуется с помощью пакета ***django-filter***.
  
  Список внешних зависимостей приведен в файле ***requirements.txt***
***
Проект размещен на хостинге ***pythonanywhere.com***:
https://haelgik.pythonanywhere.com/submitData/
 (в проекте используется база данных db.sqlite3)
 ***
 Документация ***Swagger***:
 https://haelgik.pythonanywhere.com/api/docs/
***
Документация ***Redoc***
https://haelgik.pythonanywhere.com/api/schema/redoc/

    

1. Перша частина ДЗ.
1.1. Створіть хмарну базу даних Atlas MongoDB "hw_08".
1.2. На базі ODM Mongoengine створено моделі для зберігання даних із файлів "authors.json" та "quotes.json" у колекціях "author" та "quote".
1.3. Під час зберігання цитат (quotes) поле автора в документі є не рядковим значенням, а Reference fields полем, де зберігається ObjectID з колекції author.
1.4. Написано скрипт create.py для завантаження json файлів у хмарну базу даних.
1.5. Реалізуйте скрипт для пошуку цитат за тегом, за ім'ям автора або набором тегів. Скрипт виконується в нескінченному циклі і за допомогою звичайного оператора input приймає команди у наступному форматі - команда: значення. Наприклад: 
name: Steve Martin — знайти та повернути список всіх цитат автора Steve Martin;
tag:life — знайти та повернути список цитат для тегу life;
tags:life,live — знайти та повернути список цитат, де є теги life або live
(примітка: без пробілів між тегами life, live);
exit — завершити виконання скрипту.
1.6. Виведення результатів пошуку лише у форматі utf-8.
Додатково написано скрипт "connect.py" для перевірки з'єднання з хмарною БД.

1.7. Додаткове завдання.
1.7.1. Реалізовано для команд "name:Steve Martin" та "tag:life" можливість скороченого запису значень для пошуку, як "name:st" та "tag:li" відповідно. Реалізовано за рахунок методу "__startswith". З іменем працює без зауважень, з тегом - не працює (причину не знайдено).
1.7.2. Реалізовано кешування для команд "name" та "tag" через redis_lru (функції оброблення команд обгорнуто в декоратор "@cache").

2. Друга частина ДЗ.
2.1. Модель контакту описано в "model_of_contact.py".
"producer.py" генерує певну кількість фейкових контактів (логічне поле для контакту по дефолту = True) та записує їх у базу даних. Потім поміщає у чергу RabbitMQ повідомлення, яке містить ObjectID створеного контакту, і так для всіх згенерованих контактів.
consumer.py отримує з черги RabbitMQ повідомлення, обробляє його та імітує функцією-заглушкою надсилання повідомлення по email. Після надсилання повідомлення логічне поле для контакту встановлюється в True. Скрипт працює постійно в очікуванні повідомлень з RabbitMQ.
2.2. Додаткова частина реалізована в "model_of_contact_additional.py", "consumer_email.py", "consumer_sms.py", "producer_additional.py".
У моделі додано додаткове поле — телефонний номер і поле, що відповідає за кращий спосіб надсилання повідомлень — SMS (по телефону) або email. "producer.py" відправляє у різні черги контакти для SMS та email. "consumer_sms.py" та "consumer_email.py" отримують свої контакти та обробляють їх.



# Проект 8-го спринта

### Описание
Репозиторий предназначен для сдачи проекта 8-го спринта.

### Структура репозитория
Вложенные файлы в репозиторий будут использоваться для проверки и предоставления обратной связи по проекту. Поэтому постарайтесь публиковать ваше решение согласно установленной структуре — так будет проще соотнести задания с решениями.

Внутри `src` расположены две папки:
- `/src/scripts`;
- `/src/pics`.

## Задача
### Описание задачи
Ваш агрегатор для доставки еды набирает популярность и вводит новую опцию — подписку. Она открывает для пользователей ряд возможностей, одна из которых — добавлять рестораны в избранное. Только тогда пользователю будут поступать уведомления о специальных акциях с ограниченным сроком действия. Систему, которая поможет реализовать это обновление, вам и нужно будет создать.
Благодаря обновлению рестораны смогут привлечь новую аудиторию, получить фидбэк на новые блюда и акции, продать профицит товара и увеличить продажи в непиковые часы. Акции длятся недолго, всего несколько часов, и часто бывают ситуативными, а значит, важно быстро доставить их кругу пользователей, у которых ресторан добавлен в избранное.  

Система работает так:

1. Ресторан отправляет через мобильное приложение акцию с ограниченным предложением. Например, такое: «Вот и новое блюдо — его нет в обычном меню. Дарим на него скидку 70% до 14:00! Нам важен каждый комментарий о новинке».
   
2. Сервис проверяет, у кого из пользователей ресторан находится в избранном списке.
   
3. Сервис формирует заготовки для push-уведомлений этим пользователям о временных акциях. Уведомления будут отправляться только пока действует акция.

 ### План проекта  
 Схематично работа сервиса будет выглядеть так: 

 ![image](src/pics/1.jpg)

Сервис будет:
 
1. читать данные из Kafka с помощью Spark Structured Streaming и Python в режиме реального времени.   
2. получать список подписчиков из базы данных Postgres.
3. джойнить данные из Kafka с данными из БД.
4. сохранять в памяти полученные данные, чтобы не собирать их заново после отправки в Postgres или Kafka.
5. отправлять выходное сообщение в Kafka с информацией об акции, пользователе со списком избранного и ресторане, а ещё вставлять записи в Postgres, чтобы впоследствии получить фидбэк от пользователя. Сервис push-уведомлений будет читать сообщения из Kafka и формировать готовые уведомления. Получать и анализировать фидбэк в вашу задачу не входит — этим займутся аналитики.

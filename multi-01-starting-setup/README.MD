Без отдельной сети:
1. Меняем урл для доступа к mongo
   ``mongodb://host.docker.internal:27017/course-goals``
2. Создаем монго контейнер
   ``docker run -d -p 27017:27017 --rm --name multi-mongo mongo``
3. Собираем бэк
   ``docker build -t multi-back ./multi-01-starting-setup/backend/``
4. Запускаем бэк
   ``docker run -d -p 80:80 --name multi-back -v /home/vasiliy/development/docker-lessons/multi-01-starting-setup/backend/logs/:/app/logs/ --rm multi-back``
5. Собираем фронт
   ``docker build -t multi-front ./multi-01-starting-setup/frontend/``
6. Запускаем фронт
   ``dc run -p 3000:3000 --name multi-front --rm -d multi-front``

С отдельной докер сетью:
1. Создаем сеть
``docker network create course-goals``
2. Создаем монго контейнер с именованным хранилищем
``docker run -d --network course-goals --rm --name multi-mongo -v multi-vol:/data/db -e MONGO_INITDB_ROOT_USERNAME=mongoadmin -e MONGO_INITDB_ROOT_PASSWORD=secret mongo``
3. Меняем урл для доступа к mongo
``mongodb://multi-mongo:27017/course-goals``
4. Создаем докерфайл
5. Собираем образ
``docker build -t multi-back ./multi-01-starting-setup/backend/``
6. Запускаем бэк
   * Чтобы работала аутентификация, нужно добавить [?authSource=admin](https://www.mongodb.com/docs/manual/reference/connection-string/)
   * Так же для того, чтобы не пересобирать образ при изменении кода, нужно прокинуть bind mount для папки backend
   * Чтобы не перезатерлась папка node_modules из контейнера несуществующей из папки backend, нужно добавить анонимный volume для нее
   * Добавляем nodemon для реалтайм перезагрузки контейнера при изменении кода
   * Добавляем .dockerignore и в нем указываем все, что не хотим копировать в контейнер
``docker run -d --network course-goals -p 80:80 --name multi-back -v /home/vasiliy/development/docker-lessons/multi-01-starting-setup/backend/:/app -v /app/node_modules --rm -e MONGO_USERNAME=mongoadmin -e MONGO_PASSWORD=secret multi-back``
7. Собираем фронт
``docker build -t multi-front ./multi-01-starting-setup/frontend/``
8. Запускаем фронт
   * Биндим src папку
   * В package.json уже есть демон, который рестартит контейнер
   * Так же пробрасываем в анонимный volume node_modules
   * Добавляем .dockerignore
``dc run -p 3000:3000 --name multi-front --rm -d -v /home/vasiliy/development/docker-lessons/multi-01-starting-setup/frontend/src:/app/src -v /app/node_modules multi-front``

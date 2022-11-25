1. Создаем сеть
``docker network create course-goals``
2. Создаем монго контейнер
``docker run -d --network course-goals --rm --name multi-mongo mongo``
3. Меняем урл для доступа к mongo
``mongodb://multi-mongo:27017/course-goals``
4. Создаем докерфайл
5. Собираем образ
``docker build -t multi ./multi-01-starting-setup/backend/``
6. Запускаем бэк
``docker run -d -p 3000:80 --network course-goals --name multi-back -v /home/vasiliy/development/docker-lessons/multi-01-starting-setup/backend/logs/:/app/logs/ multi``

Либо без отдельной сети:
1. Меняем урл для доступа к mongo
   ``mongodb://host.docker.internal:27017/course-goals``
2. Создаем монго контейнер
``docker run -d -p 27017:27017 --rm --name multi-mongo mongo``
3. Собираем бэк
``docker build -t multi-back ./multi-01-starting-setup/backend/``
4. Запускаем бэк
``docker run -d -p 80:80 --name multi-back -v /home/vasiliy/development/docker-lessons/multi-01-starting-setup/backend/logs/:/app/logs/ multi-back``
5. Собираем фронт
``docker build -t multi-front ./multi-01-starting-setup/frontend/``
6. Запускаем фронт
``dc run -p 3000:3000 --name multi-front --rm -d multi-front``

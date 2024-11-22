
# Лабораторная работа №4
## Задание
1. Для начала мы скачиваем Docker - это платформа контейнеризации с открытым исходным кодом, которая используется для оптимизации управления приложениями и разработки программного обеспечения.\
   
   Затем проверяем, что он установлен с помощью команды `docker --version`:

   <img width="565" alt="Снимок экрана 2024-11-22 в 17 57 53" src="https://github.com/user-attachments/assets/5bce318f-1719-4c4f-9035-81490dd1449a">
   
   После этого мы запускаем Docker:

3. Создаем отдельную папку для выполнения этой лабораторной и сразу переходим в нее с помощью команд:
   ```
   mkdir docker_project
   cd docker_project
   ```
4. Создаем файл Dockerfile внутри нашей папки docker_project:
   ```
   touch Dockerfile
   ```
5. Открываем наш созданный файл для редактирования:
   ```
   nano Dockerfile
   ```
6. В файле пишем следующие команды для того чтобы выполнить задание:
   ```
    FROM debian:latest

    RUN apt-get update && apt-get install -y libaa-bin iputils-ping

    CMD ["aafire"]
   ```

`FROM debian:latest`:

Эта команда указывает базовый образ для контейнера. В данном случае используется Debian с тегом latest, что означает последнюю доступную стабильную версию Debian.
Все остальные команды в Dockerfile будут выполняться на основе этого образа.

`RUN apt-get update && apt-get install -y libaa-bin iputils-ping`:

`RUN` — эта команда выполняет указанные операции в процессе сборки контейнера. Каждое выполнение команды RUN создает новый слой в образе.\
`apt-get update` — обновляет индекс пакетов в системе, чтобы убедиться, что доступные пакеты актуальны.\
`apt-get install -y libaa-bin iputils-ping` — устанавливает два пакета:\
`libaa-bin` — пакет, содержащий утилиту для работы с ASCII-графикой (например, aafire).\
`iputils-ping` — пакет, включающий утилиту ping для проверки доступности сетевых хостов.\
Флаг `-y` указывает, что система автоматически подтверждает установку, не запрашивая подтверждения у пользователя.

`CMD ["aafire"]`:

`CMD` задает команду, которая будет выполнена при запуске контейнера.

7. Затем собираем docker-образ с помощью команды `docker build -t aafire:latest .`:
   
   <img width="889" alt="Снимок экрана 2024-11-22 в 18 28 05" src="https://github.com/user-attachments/assets/d5166bdf-7404-4e2a-9555-ac8a750a319a">

8. Запускаем первый и второй контейнеры с помощью этих команд:
   ```
   docker run -dit --name container1 aafire
   docker run -dit --name container2 aafire
   ```
   Программа aafire будет выполнена внутри контейнера, что приведет к выводу анимации огня в терминале в фоне, так как флаг -d не будет блокировать терминал.
   
   <img width="1262" alt="Снимок экрана 2024-11-22 в 18 27 02" src="https://github.com/user-attachments/assets/3d816a06-89c4-49e6-86be-ff478c6be8ee">

9. Создаем пользовательскую сеть Docker
    ```
   docker network create myNetwork
   ```
11. Подключаем контейнеры к этой сети
    ```
     docker network connect myNetwork container1
     docker network connect myNetwork container2
     ```
12. Открываем терминал внутри первого контейнера
     ```
     docker exec -it container1 bash
     ```   
13. Затем проверяем связь с container2:\
    Узнаем IP-адрес второго контейнера с помощью команды `docker network inspect myNetwork`
    
    <img width="890" alt="Снимок экрана 2024-11-22 в 18 32 09" src="https://github.com/user-attachments/assets/a5e1b352-d1ac-411b-926b-c06ea176afcc">

    Затем выполняем эту команду:

    <img width="889" alt="Снимок экрана 2024-11-22 в 18 33 55" src="https://github.com/user-attachments/assets/fc4e9d1d-925d-449c-b3b5-88a70a5d495c">



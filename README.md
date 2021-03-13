<h1>Как развернуть и среду и затестить работу:</h1>

1) Шаг первый:
    - Установи на свою машину docker и docker-compose;
    - docker версия: Client: Docker Engine - Community || Version: 19.03.11;
    - docker-compose: version 1.24.1, build 4667896b;
    - Проект выполнен на Laravel Framework 7.30.1
    - Дополнительно установлены:
      a) "laravel-lang/lang": "8.0.0" - языковые файлы скопированы из vendor;
      b) "laravel/ui": "^v2.0.0" - заюзаны bootstrap scaffolding;
      с) как require-dev:
         - "barryvdh/laravel-debugbar": "v3.5.2"; (юзай APP_DEBUG переменную среды в true/false в ./backend/.env)
    
2) Шаг второй:
    - Клонируй данный репозиторий;
    - Пройди в папку docker
    - Открой консоль в данной папке и выполни ``docker-compose up --build`` (Разворачивание среды может продолжаться достаточно долго. Всё зависит от твоей машины)
    - Дождись когда выполниться установка пакетов composer, понять что это произошло можно по скрину ниже)
    ![image_one](https://github.com/nikolaykv/framework-team/blob/master/readme-images/image1.png)
    - Выхлоп консоли должен заканчиваться на: ``simple-test-composer exited width code 0`` - это означает что контейнер composer скачал все пакеты и завершил свою работу успешно (статус обязательно должен быть 0)
    - Останови работу docker (просто тут же в консоли жми ctrl+c) Оборви работу процесса в общем
    - После того как среда сбилдилась, а команда ``docker-compose up --build`` - как раз таки и выполняет билд среды (с отображением хода билда), выполни ``docker-compose up -d`` (флаг -d выполнит запуск среды в фоне. Смотри картинку ниже)
     ![image_two](https://github.com/nikolaykv/framework-team/blob/master/readme-images/image2.png)
    - Если видишь что изображения (это части нашей среды) выполнились со статусом done, то всё ништяк - приступай к следующему шагу
    - Если ты на линуксе, то выполни следующий команды:
        - ``sudo chmod 777 vendor -R``
        - ``sudo chmod 777 backend -R``
        - выполнять команды, естественно, нужно находясь уровнем выше указанных папок (это нужно для установки прав, так как на линухе с этим бывают проблемы. Сервер просто не может считать нужные ему файлы)
        
3) Шаг третий:
    - Установи пакеты npm
    - Скопируй .env.example и назови его .env в то же место где был .env.example (команда ``cp .env.example .env`` если на линухе ты)
    - открой скопированный .env (найди там настройки DB_CONNECTION (обычно это 9 строка))
    - Замени всё ниже девятой строчки на то что указанно в изображении ниже или ты можешь открыть файл /docker/.env и обнаружить там те же самые переменные среды, необходимые для того чтобы приложение приконектилось к базе
    
    ![image_three](https://github.com/nikolaykv/framework-team/blob/master/readme-images/image3.png)

4) Шаг четвертый:
    - Теперь необходимо сгенерировать ключ приложения Laravel
    - Выполни в папке /docker, где лежит файл docker-compose.yaml следующую команду ``docker exec -it simple-test-php-fpm /bin/bash`` - если у тебя баш, если не баш то погугли
    - тем самым ты попадешь в образ пыхи данной среды, видеть ты должен примерно следующее
     ![image_four](https://github.com/nikolaykv/framework-team/blob/master/readme-images/image4.png)
    - далее в среде контейнера выполни генерацию ключа стандартным способом Laravel (``php artisan key:generate``)
    - Миграции и прочие плюшки artisan запускать нужно там же
    
5) Шаг пятый:
    - Если всё сделано правильно, то пройди в браузере по адресу http://127.0.0.1/ - откроется авторизация ларки из коробки. Зарегистрируйся и проверяй веб морду
    - По адресу http://127.0.0.1:8080/ - доступен админер
   
6) В файле CHANGELOG.md можно ознакомиться с историей коммитов;

# Документация Insomnia

Добро пожаловать в репозиторий документации Insomnia с открытым исходным кодом. Найдите сайт документации Insomnia по адресу [docs.insomnia.rest](https://docs.insomnia.rest/) или ее перевод по адресу [insomnia.w3ref.ru](https://insomnia.w3ref.ru/).

Пожалуйста, обратитесь к нашим [Правилам участия](/CONTRIBUTING.md).

## Запуск локально

1. Клонируйте репозиторий.
2. Установите [Ruby](https://www.ruby-lang.org/en/) и [Bundler](https://bundler.io/).
3. Запустите `cd docs`.
4. Запустите `bundle install`.
5. Запустите `bundle exec jekyll serve`.
6. Смотреть на http://localhost:4000.

## Запуск с Docker

1. Клонируйте репозиторий.
2. Установите [Docker](https://docs.docker.com/get-docker/).
3. Запустите `make build`  or `docker build --tag insomnia-docs:latest .`.
4. Запустите `make run` or `docker run -it --rm -p 4000:4000 insomnia-docs:latest`.
5. Смотреть на http://localhost:4000.

---
layout: article-detail
title:  Установка Insomnia
category: "Начать"
category-url: get-started
---

Insomnia доступна в macOS, Windows и Linux. Если вы еще не загрузили Insomnia, посетите [страницу загрузки](https://insomnia.rest/download).

## Mac

{:.alert .alert-primary}
**Примечание**: Минимальная поддерживаемая версия macOS 10.12 Sierra.

Получите Insomnia на macOS, загрузив приложение или используя Brew.

[Скачать](https://insomnia.rest/download) и дважды щелкните образ диска. При появлении запроса перетащите Insomnia в папку **Applications**. Это гарантирует правильную установку будущих обновлений.

Пользователи macOS также могут установить Insomnia с помощью [Brew Cask](https://brew.sh/) через пакет Insomnia:

```bash
brew install --cask insomnia
```

## Windows

Получите Insomnia для Windows, загрузив или загрузив нашу портативную версию.

Приложение Windows представляет собой универсальный установщик `.exe`. Дважды щелкните файл установщика, чтобы установить Insomnia в существующую файловую систему. Этот вариант рекомендуется, так как он включает автоматические обновления приложений.

Существует также [портативная версия](https://github.com/Kong/insomnia/releases/tag/core%402021.6.0), которую можно запустить на месте без какой-либо установки.

### Удалить в Windows

Чтобы удалить Insomnia с компьютера Windows, просто зайдите в меню настроек Windows и выберите **Приложения**.

В разделе «Установка и удаление программ» щелкните приложение и выберите его удаление.

## Linux

Insomnia работает на распространенных дистрибутивах Linux.

### Ubuntu и Debian

Репозиторий пакетов Debian может быть добавлен и установлен с помощью `apt-get`.

```bash
# Добавить в источники
echo "deb [trusted=yes arch=amd64] https://download.konghq.com/insomnia-ubuntu/ default all" \
    | sudo tee -a /etc/apt/sources.list.d/insomnia.list

# Обновите исходники репозитория и установите Insomnia
sudo apt-get update
sudo apt-get install insomnia
```

Вы также можете скачать [последний пакет Debian](https://download.konghq.com/insomnia-ubuntu/).

### Snap

[Snap](https://snapcraft.io/) - это новый кроссплатформенный формат пакетов, поддерживающий удобные автоматические обновления. Вы можете просмотреть [Insomnia на Snapcraft](https://snapcraft.io/insomnia) или установить его напрямую с помощью следующей команды.

```bash
sudo snap install insomnia
```

Также существует [переносимый пакет AppImage](https://github.com/Kong/insomnia/releases/tag/core%402021.6.0), который можно запускать напрямую как исполняемый файл.

### Устранение неполадок при установке в Linux

Вот некоторые проблемы, которые раньше вызывали проблемы у пользователей Linux:

* Папка `/tmp` должна разрешать выполнение
* После установки snap, вам может потребоваться [`systemctl restart snapd.service`](https://bugs.launchpad.net/ubuntu/+source/snapd/+bug/1631514)
* Insomnia совместима только с 64-битными системами

## Предыдущие версии Insomnia

Чтобы вернуться к предыдущей версии Insomnia, загрузите версию с [GitHub Релизов](https://github.com/kong/insomnia/releases). Этот процесс предназначен только для отладки и аварийных ситуаций, так как приложение будет пытаться обновиться после запуска.

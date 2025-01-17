---
layout: article-detail
title: Регистрация и аутентификация
category: Безопасность
category-url: security
---

Поскольку пароль, который вы выбираете во время регистрации, используется в процессе шифрования (хотя и косвенно), очень важно, чтобы он никогда не отправлялся и не хранился на сервере в легко взломанной форме. Для достижения этой цели Insomnia использует протокол обмена зашифрованными ключами [Secure Remote Passwords (SRP)](http://srp.stanford.edu/).

Вы можете узнать больше о точной реализации SRP, которую используют платные планы Insomnia, в [RFC-2945](https://datatracker.ietf.org/doc/html/rfc2945).

Подробное описание SRP смотрите в [Mozilla's Node SRP](https://github.com/mozilla/node-srp).

### Как работает создание учетной записи

Это шаги, предпринятые на клиенте во время создания учетной записи.

1. Случайным образом сгенерируйте 256-битные ключи и соли `SYM_Account`, `SLT_Auth_1`, `SLT_Auth_2`, `SLT_Encryption`
2. Создайте пару ключей `PUB_Account`/`PRV_Account` для `RSA-OAEP` `SHA-256`
3. Создайте `SEC_PWD_Auth`, выполнив следующие шаги
    1. Объедините `SLT_Auth1` с адресом электронной почты, используя `HKDF` `SHA-256`, чтобы сформировать новую соль `SLT_TMP_1`
    2. Запустите 100'000 итераций `PBKDF2` `SHA-256` с `SLT_TMP_1`
4. Создайте `SEC_PWD_Enc`, выполнив следующие шаги
    1. Объедините SLT_Enc с адресом электронной почты, используя `HKDF` `SHA-256`, чтобы сформировать новую соль `SLT_TMP_2`
    2. Запустите 100'000 итераций `PBKDF2` `SHA-256` с `SLT_TMP_2`
5. Создайте `SRP_Verifier`, используя `SLT_Auth_2`, email address, `SEC_PWD_Auth`
6. Зашифруйте `SYM_Account`, используя `SEC_PWD`
7. Зашифруйте `PRV_Account`, используя `SYM_Account`
8. Отправить объект `M_Account` на сервер для создания

После создания учетной записи сервер отправит пользователю электронное письмо с подтверждением. Получив это письмо, пользователь может попытаться войти в систему.

### Как работает вход в учетную запись

Это шаги, предпринимаемые на клиенте во время входа в систему.

1. Получите `SEC_PWD_Auth`, используя те же шаги, что и при создании учетной записи
2. Используйте `SLT_Auth_2` для выполнения обмена `SRP`
3. Сохраните `SRP`-сгенерированный `K` локально для использования в качестве сеансового ключа

Теперь, когда мы знаем, как выполняется регистрация и аутентификация, мы можем поговорить о шифровании данных.

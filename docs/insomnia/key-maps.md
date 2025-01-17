---
layout: article-detail
title:  Карта клавиш
category: "Встроенные функции"
category-url: built-in-features
---

Insomnia поддерживает различные карты клавиш текстового редактора, выбрав `Preferences > General > Font > Text Editor Key Map`.

![Доступ к параметрам карты клавиш текстового редактора осуществляется через вкладку общих настроек.](/assets/images/key-maps.png)
_Чтобы выбрать раскладку клавиш текстового редактора, перейдите в раздел шрифтов на вкладке общих настроек._

В настоящее время доступны ключевые карты: Vim, Emacs и Sublime.

## Vim на Mac

По умолчанию при нажатии и удержании в macOS отображаются специальные символы.

Это не особенно полезно при использовании карты клавиш Vim, поскольку навигация ограничена одним движением за раз. Чтобы включить повторение клавиш, выполните в терминале следующее и перезапустите Insomnia:

`defaults write com.insomnia.app ApplePressAndHoldEnabled -bool false`

---
layout: article-detail
title:  Валидация SSL
category: Поддержка
category-url: support
---

Видели ли вы подобные ошибки сертификатов?

```bash
Error: unable to verify the first certificate
```

```bash
Error: Hostname/IP doesn't match certificate's altnames: "Host: example.com is not in the cert's altnames: DNS:*.surge.sh, DNS:surge.sh"
```

Если вы тестируете на локальном сервере разработки или знаете, что сертификат недействителен, вы можете отключить проверку в настройках, сняв флажок **Проверять SSL-сертификаты**.

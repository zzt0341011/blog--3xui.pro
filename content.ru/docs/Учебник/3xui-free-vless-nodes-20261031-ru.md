---
title: "Бесплатные узлы VLESS для 3x-ui (последнее обновление 31 октября 2026)"
description: "Анализ рисков бесплатных узлов VLESS, рекомендации по фильтрации и актуальный список узлов. Также включены рекомендуемые платные аэропорты с оплатой по трафику для долгосрочного использования."
keywords:
  - бесплатные узлы VLESS
  - бесплатные узлы 3x-ui
  - раздача узлов VLESS
  - риски бесплатного прокси
  - рекомендация аэропортов
  - аэропорт с оплатой по трафику
weight: 10
---

# Бесплатные узлы VLESS для 3x-ui (последнее обновление 31 октября 2026)

## 1. Можно ли использовать бесплатные узлы VLESS?

| Риск | Конкретное проявление | Степень серьёзности |
| --- | --- | --- |
| Утечка конфиденциальности | Оператор может записывать IP подключения, время, целевой домен и другие метаданные | Высокая |
| Мониторинг трафика | Оператор может наблюдать незашифрованные сетевые коммуникации | Высокая |
| Вредоносный DNS | Домены могут разрешаться на фишинговые или вредоносные сайты | Высокая |
| Атака «человек посередине» | Небезопасная конфигурация TLS может привести к перехвату или изменению трафика | Высокая |
| Внезапный отказ узла | Бесплатные узлы часто меняют IP, порт или пароль | Средняя |
| Безопасность аккаунта | Могут заманивать на поддельные сайты и просить ввести логин/пароль | Высокая |
| Нестабильные линии | Перегрузка, ограничение скорости и обрывы в часы пик | Средняя |

**Особое напоминание:**

То, что VLESS использует шифрование, **не означает**, что операторы бесплатных узлов не могут наблюдать за вашей сетевой активностью.

Например:

- Конкретное содержимое страниц HTTPS-сайтов обычно защищено TLS.
- Оператор узла всё равно может видеть время подключения, объём трафика, целевой IP и другую информацию.
- Если DNS не зашифрован должным образом, запросы доменов также могут быть раскрыты.
- Открытый HTTP-трафик может быть напрямую прочитан или изменён.

## 2. Как следует фильтровать бесплатные узлы VLESS?

Рекомендуется проверять как минимум следующие пункты:

| Параметр проверки | Рекомендация |
| --- | --- |
| security | Обязательно TLS или Reality |
| TLS-сертификат | Нормальная проверка, не пропускать проверку |
| allowInsecure | Должно быть false |
| Источник узла | Предпочитать проекты с историей обслуживания |
| DNS | Использовать доверенный зашифрованный DNS |
| Клиент | Использовать официальный Shadowrocket |
| Права | Не устанавливать неизвестные сертификаты или профили конфигурации, требуемые провайдером узла |

**Особые замечания:**

Не считайте узел безопасным только потому, что он подключается.  
Также не устанавливайте корневые сертификаты, неизвестные VPN-конфигурации или профили управления мобильными устройствами, которые требуют провайдеры бесплатных узлов.

Следующие узлы собраны из интернета. Используйте их с осторожностью:

```text
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.130.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443%3Fed%3D2560#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.17.148.22:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%F0%9F%87%BA%F0%9F%87%B8%20%E7%BE%8E%E5%9B%BD%20%7C%20USA
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.135.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTELEGRAM-TOOTFFARANGI#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.135.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%E7%BE%8E%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTelegram---TEHRANARGO#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@203.88.117.190:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@45.61.149.205:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443fp%3Dchrome#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%23TELEGRAM--MARAMBASHI--MARAMBASHI--MARAMBASHI--MARAMBASHI%3Fed#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@2.28.42.209:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443fp%3Dchrome#%E5%BE%B7%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@saas.sin.fan:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40vpn748359672.opengw.net%3A1458%3Fed%3D2560#%E7%BE%8E%E5%9B%BD23
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@45.61.149.205:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%F0%9F%87%BA%F0%9F%87%B8%20%E7%BE%8E%E5%9B%BD%20%7C%20USA
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@2.28.42.209:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%E5%BE%B7%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTelegram---bored_vpn#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.194.140.26:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffp%3Dchrome#%E8%8B%B1%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E5%B7%B4%E8%A5%BF%E5%9C%A3%E4%BF%9D%E7%BD%97%2BCloudFlare%E8%8A%82%E7%82%B9
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@saas.sin.fan:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.245.245.16:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffp%3Dchrome#%E6%B3%A2%E5%85%B062
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@144.31.181.177:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E5%BE%B7%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@104.17.148.22:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=http%2F1.1&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffp%3Dchrome#%E7%BE%8E%E5%9B%BD62
vless://e6df2095-a73c-4043-8693-8d5a03686870@188.114.98.0:443?encryption=none&security=tls&sni=lucasjow.us.ci&fp=chrome&alpn=http%2F1.1&type=ws&host=lucasjow.us.ci&path=%2F%3FTELEGRAM--i10VPN%3Fed#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@130.78.190.134:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E8%8D%B7%E5%85%B062
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@188.114.98.0:443?encryption=none&security=tls&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdipfp%3Dchrome#%E7%BE%8E%E5%9B%BD62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@103.39.78.118:8443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&type=ws&host=snip.edgeoneai.cc.cd&path=%2Ffdip%3Dsstp%3A%2F%2Fvpn%3Avpn%40public-vpn-200.opengw.net%3A443#%E9%A6%99%E6%B8%AF62
vless://495c7195-85b8-498a-bf20-2ea9ce9175b5@162.159.135.234:443?encryption=none&security=tls&sni=snip.edgeoneai.cc.cd&fp=chrome&alpn=h3%2Ch2&type=ws&host=snip.edgeoneai.cc.cd&path=%2F#%E7%BE%8E%E5%9B%BD%2BCloudFlare%E8%8A%82%E7%82%B9
```

## 3. Использование прокси-сервисов аэропортов

Следующие аэропорты работают по модели оплаты за трафик. На сайтах есть инструкции по использованию и установке программного обеспечения.  
После покупки трафика ограничение по времени отсутствует — трафик действует до полного исчерпания.  
Если сайт становится недоступен, значит он заблокирован — переходите на другой.

| Название | Цена | Трафик | Узлы |
| :--- | :--- | :--- | :--- |
| [魔戒](https://1.jnk.ink/L4q20S) | 1 юань | 1 ГБ | 30 |
| [网际快车](https://1.jnk.ink/ad2RVl) | 7 юаней | 20 ГБ | 54 |
| [牛逼](https://1.jnk.ink/LYet7x) | 14 юаней | 200 ГБ | 31 |
| [飞鸟](https://1.jnk.ink/i7OhaC) | 10 юаней | 200 ГБ | 25 |
| [happy猫](https://1.jnk.ink/5KiTxY) | 20 юаней | 200 ГБ | 27 |
| [农夫山泉](https://1.jnk.ink/i1fXTMYk) | 45 юаней | 200 ГБ | 40 |
| [宝贝云](https://1.jnk.ink/xxPwfy) | 55 юаней | 600 ГБ | 64 |
| [飞兔](https://1.jnk.ink/bbXkiN) | 30 юаней | 100 ГБ | 80 |
| [fscloud](https://1.jnk.ink/nKXcqQ) | 14 юаней/год | 100 ГБ/мес | 30 |
| [冲上云霄](https://1.jnk.ink/gXM8T) | 7 юаней/мес | 50 ГБ | 28 |

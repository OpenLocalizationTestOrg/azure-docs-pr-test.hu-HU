---
title: "Linux virtuális gépre az Azure-ban LEMP aaaDeploy |} Microsoft Docs"
description: "Az oktatóanyag - telepítés hello LEMP verem a Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: d8f9d84c5e9c0df4e9e985c10fe10f63a2f88214
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Egy Azure virtuális gépen LEMP webkiszolgáló telepítése
Ez a cikk bemutatja, hogyan hogyan toodeploy egy NGINX webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LEMP stack) az Ubuntu virtuális gép az Azure-ban. hello LEMP verem egy alternatív toohello népszerű [LÁMPA verem](tutorial-lamp-stack.md), amely az Azure-ban is telepíthet. toosee hello LEMP server műveletben, igény szerint telepítheti és egy WordPress-webhely konfigurálása. Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Hozzon létre egy Ubuntu virtuális gép (hello "L" hello LEMP verem)
> * A 80-as port megnyitása a webes adatforgalom számára
> * NGINX, a MySQL és a PHP telepítése
> * Ellenőrizze a telepítés és konfigurálás
> * WordPress telepítése hello LEMP kiszolgálón


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>NGINX, a MySQL és a PHP telepítése

Futtassa a következő parancs tooupdate Ubuntu csomag források hello és NGINX, a MySQL és a PHP telepítése. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Biztos, hogy felszólító tooinstall hello csomagok és más függőségek. Amikor a rendszer kéri, egy gyökérszintű jelszót beállítani a MySQL, majd [Enter] toocontinue. Kövesse a hátralévő utasításokat hello. Ez a folyamat a MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP telepíti. 

![MySQL legfelső szintű jelszófrissítési lap][1]

## <a name="verify-installation-and-configuration"></a>Ellenőrizze a telepítés és konfigurálás


### <a name="nginx"></a>NGINX

Hello verziójának NGINX egyeztetni hello a következő parancsot:
```bash
nginx -v
```

Az NGINX telepített, és a port 80 nyissa meg a tooyour VM, hello webkiszolgáló most elérhető hello az interneten. tooview hello NGINX kezdőlapján nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét. Hello tooSSH toohello VM használt nyilvános IP-cím használata:

![NGINX alapértelmezett oldal][3]


### <a name="mysql"></a>MySQL

MySQL hello verziója kérje meg a következő parancs hello (vegye figyelembe a hello beruházási `V` paraméter):

```bash
msql -V
```

Azt javasoljuk, hogy fut a következő parancsfájl toohelp biztonságos hello telepítésének MySQL hello:

```bash
mysql_secure_installation
```

Adja meg a MySQL gyökér szintű jelszavát, és a környezet hello biztonsági beállításainak konfigurálása.

Ha azt szeretné, hogy a MySQL-adatbázis toocreate, adja hozzá a felhasználókat, vagy módosítsa a konfigurációs beállításokat, bejelentkezési tooMySQL:

```bash
mysql -u root -p
```

Ha befejezte, lépjen ki hello mysql parancssorba írja be a `\q`.

### <a name="php"></a>PHP

Hello verzió a PHP egyeztetni hello a következő parancsot:

```bash
php -v
```

NGINX toouse hello PHP FastCGI folyamat-kezelő (PHP-FPM) konfigurálása. Futtassa a következő parancsok tooback hello eredeti NGINX kiszolgáló konfigurációs fájlt, és szerkessze a hello eredeti fájlt valamelyik szerkesztőben az Ön által választott hello:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

Hello szerkesztő, cserélje le a hello tartalmát `/etc/nginx/sites-available/default` hello következőre. Lásd: hello megjegyzések hello beállításainak ismertetése. Helyettesítse be a virtuális Gépet a hello nyilvános IP-címe *yourPublicIPAddress*, és a fennmaradó beállításokat hello hagyja. Mentse a hello fájlt.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Ellenőrizze a hello NGINX konfigurációját szintaktikai hibák:

```bash
sudo nginx -t
```

Ha hello szintaxisának helyességét, indítsa újra a NGINX a hello a következő parancsot:

```bash
sudo service nginx restart
```

Ha azt szeretné, hogy további tootest, hozzon létre egy gyors PHP adatai lap tooview a böngészőben. a következő parancs hello hello PHP adatok lapján hoz létre:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Most létrehozott hello PHP adatok lapján ellenőrizheti. Nyisson meg egy böngészőt, és nyissa meg túl`http://yourPublicIPAddress/info.php`. Helyettesítse be a virtuális gép hello nyilvános IP-címét. Az alábbihoz hasonló toothis kép.

![A PHP adatai lap][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure-ban LEMP kiszolgáló telepítette. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Ubuntu virtuális gép létrehozása
> * A 80-as port megnyitása a webes adatforgalom számára
> * NGINX, a MySQL és a PHP telepítése
> * Ellenőrizze a telepítés és konfigurálás
> * Hello LEMP veremben WordPress telepítése

Hogyan előzetes toohello következő útmutató toolearn toosecure webkiszolgálók SSL-tanúsítványokkal.

> [!div class="nextstepaction"]
> [Biztonságos webkiszolgáló SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png

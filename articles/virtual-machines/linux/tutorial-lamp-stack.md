---
title: "Linux virtuális gépre az Azure-ban LÁMPA aaaDeploy |} Microsoft Docs"
description: "Az oktatóanyag - telepítés hello LÁMPA verem a Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6c12603a-e391-4d3e-acce-442dd7ebb2fe
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 08/03/2017
ms.author: danlep
ms.openlocfilehash: a3d0ecb3277f15bd0a2fdc0d85b738a760e68865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-lamp-web-server-on-an-azure-vm"></a>Egy Azure virtuális gépen LÁMPA webkiszolgáló telepítése
Ez a cikk bemutatja, hogyan hogyan toodeploy Apache webalkalmazás-kiszolgáló, a MySQL és a PHP (hello LÁMPA stack) az Ubuntu virtuális gép az Azure-ban. Ha jobban szeret hello NGINX webkiszolgáló, lásd: hello [LEMP verem](tutorial-lemp-stack.md) oktatóanyag. toosee hello LÁMPA server műveletben, igény szerint telepítheti és egy WordPress-webhely konfigurálása. Ezen oktatóanyag segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Hozzon létre egy Ubuntu virtuális gép (hello "L" hello LÁMPA verem)
> * A 80-as port megnyitása a webes adatforgalom számára
> * Apache, a MySQL és a PHP telepítése
> * Ellenőrizze a telepítés és konfigurálás
> * WordPress telepítése hello LÁMPA kiszolgálón


Hello LÁMPA verem, beleértve az éles környezetbe, ajánlásokat bővebben lásd: hello [Ubuntu dokumentáció](https://help.ubuntu.com/community/ApacheMySQLPHP).

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-apache-mysql-and-php"></a>Apache, a MySQL és a PHP telepítése

Futtassa a következő parancs tooupdate Ubuntu csomag források hello és Apache, a MySQL és a PHP telepítése. Vegye figyelembe a hello beszúrási jel (^) hello parancs hello végén.


```bash
sudo apt update && sudo apt install lamp-server^
```



Biztos, hogy felszólító tooinstall hello csomagok és más függőségek. Amikor a rendszer kéri, egy gyökérszintű jelszót beállítani a MySQL, majd [Enter] toocontinue. Kövesse a hátralévő utasításokat hello. Ez a folyamat a MySQL hello minimálisan szükséges PHP szükséges bővítmények toouse PHP telepíti. 

![MySQL legfelső szintű jelszófrissítési lap][1]

## <a name="verify-installation-and-configuration"></a>Ellenőrizze a telepítés és konfigurálás


### <a name="apache"></a>Apache

Apache hello verziója egyeztetni hello a következő parancsot:
```bash
apache2 -v
```

Az Apache telepített, és a port 80 nyissa meg a tooyour VM, hello webkiszolgáló most elérhető hello az interneten. tooview hello Apache2 Ubuntu alapértelmezett oldal, nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét. Hello tooSSH toohello VM használt nyilvános IP-cím használata:

![Apache alapértelmezett oldal][3]


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

Ha azt szeretné, hogy további tootest, hozzon létre egy gyors PHP adatai lap tooview a böngészőben. a következő parancs hello hello PHP adatok lapján hoz létre:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```

Most létrehozott hello PHP adatok lapján ellenőrizheti. Nyisson meg egy böngészőt, és nyissa meg túl`http://yourPublicIPAddress/info.php`. Helyettesítse be a virtuális gép hello nyilvános IP-címét. Az alábbihoz hasonló toothis kép.

![A PHP adatai lap][2]

[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]


## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure-ban LÁMPA kiszolgáló telepítette. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Ubuntu virtuális gép létrehozása
> * A 80-as port megnyitása a webes adatforgalom számára
> * Apache, a MySQL és a PHP telepítése
> * Ellenőrizze a telepítés és konfigurálás
> * WordPress telepítése hello LÁMPA kiszolgálón

Hogyan előzetes toohello következő útmutató toolearn toosecure webkiszolgálók SSL-tanúsítványokkal.

> [!div class="nextstepaction"]
> [Biztonságos webkiszolgáló SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lamp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lamp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lamp-stack/apachesuccesspage.png
---
title: "az App Service web app tooRedis keresztül hello Memcache protokoll - Azure aaaConnect |} Microsoft Docs"
description: "Webes alkalmazás csatlakoztatása az Azure App service tooRedis gyorsítótár hello Memcache protokoll segítségével"
services: app-service\web
documentationcenter: php
author: SyntaxC4
manager: erikre
editor: riande
ms.assetid: 0fcdf9fa-2995-4839-ba4d-cfa389c4ba06
ms.service: app-service-web
ms.devlang: php
ms.topic: get-started-article
ms.tgt_pltfrm: windows
ms.workload: na
ms.date: 02/29/2016
ms.author: cfowler
ms.openlocfilehash: 48036d60fbbced59eb1e37584f507fffffff753d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# Webes alkalmazás csatlakoztatása az Azure App Service tooRedis gyorsítótár hello Memcache protokoll segítségével
Ebből a cikkből megtudhatja, hogyan tooconnect a WordPress webalkalmazás a [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) túl[Azure Redis Cache] [ 12] hello segítségével [Memcache] [ 13] protokoll. Ha egy létező webalkalmazása, amely egy Memcached-kiszolgálót használ a memórián belüli gyorsítótárazáshoz, áttelepítheti az tooAzure App Service és a használata hello belső gyorsítótárazási megoldását a Microsoft Azure-ban kevéssé vagy egyáltalán ne módosítsa tooyour alkalmazás kód. Ezenkívül használhatja a meglévő Memcache szakértői toocreate jól skálázható, elosztott alkalmazások az Azure App Service szolgáltatásban az Azure Redis Cache memórián belüli gyorsítótárazáshoz, olyan népszerű alkalmazás-keretrendszerek, például a .NET, PHP, Node.js, Java és Python használatával.  

App Service Web Apps lehetővé teszi, hogy az alkalmazás forgatókönyv hello webalkalmazások Memcache-segédkódjának, amely egy helyi Memcached-kiszolgálót, amely a hívások tooAzure Redis Cache-gyorsítótár Memcache-proxyként viselkedik. Ez lehetővé teszi a bármely alkalmazás, amely a Redis Cache hello Memcache protokoll toocache adatok segítségével kommunikál. A Memcache-segédkód hello protokoll szintjén működik, így is használható bármilyen alkalmazás vagy alkalmazás-keretrendszer mindaddig, amíg hello Memcache protokoll segítségével kommunikál.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Előfeltételek
hello webalkalmazások Memcache-segédkódjának bármilyen alkalmazással használható, feltéve hello Memcache protokoll segítségével kommunikál. Ha adott például hello referenciaalkalmazás egy méretezhető WordPress-webhely, amely az Azure piactér hello.

Kövesse az alábbi cikkekben leírt hello lépéseket:

* [Hello Azure Redis Cache szolgáltatás egy példányának kiosztása][0]
* [Egy méretezhető WordPress-webhely telepítése az Azure-ban][1]

Miután hello méretezhető WordPress-webhelyet, és a Redis Cache példányt fogja készen tooproceed az Azure App Service Web Apps hello Memcache-segédkód engedélyezését.

## Hello webalkalmazások Memcache-segédkódjának engedélyezése
Rendelés tooconfigure Memcache-segédkód a létre kell hoznia három alkalmazásbeállítást. Ezt megteheti többféle módszerrel, beleértve a hello segítségével [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), hello [klasszikus portál][3], hello [Azure PowerShell-parancsmagok] [ 5] vagy hello [Azure parancssori felület][5]. Ebben a bejegyzésben hello célokra fogom toouse hello [Azure Portal] [ 4] tooset hello Alkalmazásbeállítások. hello következő értékek lekérhetők **beállítások** a Redis Cache példány panelen.

![Azure Redis Cache beállításpanel](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### REDIS_HOST alkalmazásbeállítás hozzáadása
első Alkalmazásbeállítás kell hello toocreate hello **REDIS\_állomás** Alkalmazásbeállítás. Ez a beállítás megadja hello cél toowhich hello segédkód továbbítja hello információk gyorsítótárazása. hello lekérhetők hello REDIS_HOST alkalmazásbeállításhoz szükséges érték hello **tulajdonságok** a Redis Cache példány panelen.

![Azure Redis Cache gazdanév](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

Set hello kulcsa hello app beállítás túl**REDIS\_állomás** és hello app beállítás toohello hello értékének **állomásnév** hello Redis Cache példány.

![Webalkalmazás AppSetting REDIS_HOST](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### REDIS_KEY alkalmazásbeállítás hozzáadása
második Alkalmazásbeállítás kell hello toocreate hello **REDIS\_kulcs** Alkalmazásbeállítás. Ez a beállítás hello hitelesítési token szükséges toosecurely hozzáférés hello Redis Cache példányt biztosít. Hello hello REDIS_KEY alkalmazásbeállításhoz szükséges hello értéket le **hívóbetűk** hello Redis Cache példány panelen.

![Azure Redis Cache elsődleges kulcs](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

Set hello kulcsa hello app beállítás túl**REDIS\_kulcs** és hello app beállítás toohello hello értékének **elsődleges kulcs** hello Redis Cache példány.

![Azure-webhely alkalmazásbeállítás REDIS_KEY](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### A MEMCACHESHIM_REDIS_ENABLE alkalmazásbeállítás hozzáadása
hello utolsó Alkalmazásbeállítás használt tooenable hello Memcache-Segédkód a webalkalmazásokban használó hello REDIS_HOST és REDIS_KEY tooconnect toohello Azure Redis Cache és előre hello gyorsítótár hívások. Set hello kulcsa hello app beállítás túl**MEMCACHESHIM\_REDIS\_engedélyezése** és az érték túl hello**igaz**.

![Webalkalmazás alkalmazásbeállítás MEMCACHESHIM_REDIS_ENABLE](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

Miután a hozzáadása hello három (3) alkalmazásbeállítást, kattintson a **mentése**.

## Memcache bővítmény engedélyezése PHP-hoz
Ahhoz, hogy hello alkalmazás toospeak hello Memcache protokollt hogy a rendszer szükséges tooinstall hello Memcache bővítményt tooPHP – hello nyelvi keretrendszert a WordPress webhelyének.

### Hello php_memcache bővítmény letöltése
Keresse meg a túl[PECL][6]. Kattintson a kategória gyorsítótárazás hello, [memcache][7]. Hello letöltések oszlopban hivatkozásra hello dll-Fájljában.

![PHP PECL webhely](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Hello Non-Thread Safe (NTS) x86 hivatkozást a webalkalmazásokban engedélyezett PHP hello verziójának letöltéséhez. (Az alapértelmezett a PHP 5.4)

![PHP PECL-webhely Memcache-csomagja](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### Hello php_memcache bővítmény engedélyezése
Hello fájl letöltése után bontsa ki és töltse fel a hello **php\_memcache.dll** történő hello **d:\\otthoni\\hely\\wwwroot\\bin\\ext\\**  könyvtár. Miután hello php_memcache.dll már fel van töltve hello webalkalmazás, meg kell tooenable hello bővítmény toohello PHP futtatókörnyezetben. tooenable hello hello Azure portál, nyissa meg hello Memcache bővítménynek **Alkalmazásbeállítások** panel hello webalkalmazás, majd adja hozzá egy új alkalmazásbeállítást hello kulcsával **PHP\_bővítmények** és hello érték **bin\\ext\\php_memcache.dll**.

> [!NOTE]
> Ha hello webalkalmazás tooload több PHP-bővítményt, a PHP_EXTENSIONS értékének hello tooDLL fájlok relatív elérési utak vesszővel tagolt listáját kell lennie.
> 
> 

![Webalkalmazás alkalmazásbeállítás PHP_EXTENSIONS](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

Ha kész, kattintson a **Mentés**gombra.

## A Memcache WordPress beépülő modul telepítése
> [!NOTE]
> Emellett letöltheti a hello [Memcached Object Cache beépülő modult](https://wordpress.org/plugins/memcached/) WordPress.org webhelyről.
> 
> 

Az hello WordPress beépülő modulok oldalon kattintson **új hozzáadása**.

![WordPress beépülő modul oldal](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

Hello keresési mezőbe, írja be a **memcached** nyomja le az ENTER **Enter**.

![WordPress Új hozzáadása beépülő modul](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

Található **Memcached Object Cache** hello listában, majd kattintson **telepítés**.

![WordPress – A Memcache telepítése beépülő modul](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### Hello Memcache WordPress beépülő modul engedélyezése
> [!NOTE]
> Kövesse az ebben a blogban hello utasításait [hogyan tooenable egy a Web Apps hely bővítmény] [ 8] tooinstall Visual Studio Team Services.
> 
> 

A hello `wp-config.php` fájlt, adja hozzá a következő kód fent hello stop szerkesztési Megjegyzés hello fájl végéhez hello hello.

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

Ha beillesztette ezt a kódot, a monaco automatikusan menti hello dokumentum.

következő lépés hello tooenable hello objektum-gyorsítótár beépülő modul. Ehhez húzással **object-cache.php** a **wp-tartalom/plugins/memcached** mappa toohello **wp-content** mappa tooenable hello Memcache objektum Gyorsítótár-funkciókat.

![Keresse meg a hello memcache object-cache.php beépülő modul](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

Most, hogy hello **object-cache.php** fájl van-e hello **wp-content** mappa, hello Memcached Object Cache engedélyezve van.

![Hello memcache object-cache.php beépülő modul engedélyezése](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## Hello Memcached Object Cache beépülő modul működésének ellenőrzése
Hello lépéseket tooenable hello webalkalmazások Memcache-segédkódjának összes most már befejeződött. hello dolog maradt: tooverify, hogy hello adatok bekerülnek-e a Redis Cache példány.

### Hello nem SSL portok támogatása az Azure Redis Cache-gyorsítótár engedélyezése
> [!NOTE]
> A cikk írásának hello időpontban hello Redis CLI nem támogatja az SSL-kapcsolatot, így hello következő lépések szükségesek.
> 
> 

Hello Azure portál keresse meg a webalkalmazás létrehozott toohello Redis Cache példányt. Ha hello gyorsítótár panelje meg nyitva, kattintson a hello **beállítások** ikonra.

![Azure Redis Cache beállításgomb](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

Válassza ki **hozzáférési portok** hello listából.

![Azure Redis Cache hozzáférési port](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

Kattintson a **Nem** lehetőségre a **Hozzáférés engedélyezése csak SSL-en keresztül** beállításnál.

![Azure Redis Cache hozzáférési port csak SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

Látni fogja, hogy a nem SSL port hello most van-e állítva. Kattintson a **Save** (Mentés) gombra.

![Azure Redis Cache Redis hozzáférési portál nem SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### Csatlakozás tooAzure Redis Cache redis-cli ről
> [!NOTE]
> A lépés feltételezi, hogy a Redis helyben telepítve van a fejlesztési számítógépén. [A Redist ezen utasításokat követve telepítheti helyben][9].
> 
> 

Nyissa meg a megfelelő parancssori konzolt, a következő parancs kiválasztása és típus hello:

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

Cserélje le a hello  **&lt;állomásnév a redis gyorsítótár&gt;**  hello tényleges xxxxx.redis.cache.windows.net gazdanévre és hello  **&lt;elsődleges-kulcs-az-redis-cache&gt;**  hello gyorsítótár hello a hozzáférési kulcsot, majd nyomja le a **Enter**. Miután hello parancssori felület csatlakozott toohello Redis Cache példányt, adja ki a redis parancsot. Hello a képernyőfelvételen látható az alábbi I kiválasztott toolist hello kulcsok.

![Kapcsolódó tooAzure Redis Cache Redis CLI terminálban](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

hello hívás toolist hello kulcsok értéket kell visszaadnia. Ha nem, próbálja meg máshová toohello webalkalmazás, és próbálkozzon újra.

## Összegzés
Gratulálunk! hello WordPress alkalmazás most már rendelkezik egy központosított memórián belüli gyorsítótárral tooaid a teljesítmény növelésében. Ne feledje, hogy hello webalkalmazások Memcache-segédkódja bármilyen Memcache-ügyféllel függetlenül programozási nyelven vagy alkalmazás-keretrendszer is használható. hello webalkalmazások Memcache-segédkódjának vonatkozó visszajelzés vagy tooask kérdésekre tooprovide utáni túl[MSDN fórumain] [ 10] vagy [Stackoverflow][11].

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

[0]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: /powershell/azureps-cmdlets-docs
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org

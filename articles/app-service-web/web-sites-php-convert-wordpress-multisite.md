---
title: "WordPress átalakítása többhelyes környezet az Azure App Service-ben"
description: "Egy meglévő WordPress-webalkalmazás létrehozása az Azure katalógusában révén ismerje és átalakíthatja a WordPress többhelyes telepítés"
services: app-service\web
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: fe52dbf4-179c-42f1-adf9-d6a9af920c39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 4a15fb5e97d2ca57e5883c07651c372c54021c92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="convert-wordpress-to-multisite-in-azure-app-service"></a>WordPress átalakítása többhelyes környezet az Azure App Service-ben
## <a name="overview"></a>Áttekintés
*Által [Ben Lobaugh][ben-lobaugh], [Microsoft nyílt technológiák Inc.][ms-open-tech]*

Ebben az oktatóanyagban, megtudhatja, hogyan egy meglévő WordPress-webalkalmazás létrehozása az Azure katalógusában révén és átalakíthatja egy WordPress többhelyes telepítést. Emellett megtudhatja, hogyan rendelhető hozzá egyéni tartományt a telepítés belül alwebhelyek mindegyikének.

Feltételezzük, hogy rendelkezik-e a WordPress egy meglévő telepítését. Ha nem, kövesse az útmutatást [egy WordPress-webhely létrehozása az Azure-ban a gyűjteményből][website-from-gallery].

Egy meglévő WordPress konvertálása a többhelyes környezet egyetlen hely telepítésének általában meglehetősen egyszerű, és a kezdeti lépések származhat rögtön a [hozzon létre egy hálózati] [ wordpress-codex-create-a-network] lapot a [WordPress kódex](http://codex.wordpress.org).

Lássunk neki.

## <a name="allow-multisite"></a>Többhelyes konfiguráció engedélyezése
Először engedélyeznie kell a többhelyes telepítés keresztül a `wp-config.php` fájlt a **WP\_engedélyezése\_TÖBBHELYES** konstans. Két módszer a webes alkalmazás fájlok szerkesztésére: az egyik FTP és a második és a Git segítségével. Ha nincs tisztában a hogyan egyik módszert sem állíthatja, olvassa el az alábbi oktatóanyagok:

* [A MySQL és az FTP-PHP-webhely][website-w-mysql-and-ftp-ftp-setup]
* [PHP-webhely MySQL és Git][website-w-mysql-and-git-git-setup]

Nyissa meg a `wp-config.php` a szerkesztőben a fájlt, és adja hozzá a következő fenti a `/* That's all, stop editing! Happy blogging. */` sor.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Mindenképpen mentse a fájlt, és töltse fel a kiszolgálóhoz!

## <a name="network-setup"></a>Hálózati beállítás
Jelentkezzen be a *wp-rendszergazda* területén a webalkalmazást, és meg kell jelennie az új cikket a **eszközök** nevű menü **hálózati beállítás**. Kattintson a **hálózati beállítás** , és töltse ki a hálózat adatait.

![Hálózati beállítási képernyője][wordpress-network-setup]

Ez az oktatóanyag használja a *alkönyvtár* séma helyet, mert mindig működnek, és azt szeretné állítani minden aloldal egyéni tartományok az oktatóanyag későbbi részében. Azonban lehetővé kell tenni altartomány telepíteni, ha leképez egy tartományhoz, a telepítő a [Azure Portal](https://portal.azure.com) és helyettesítő DNS beállításának megfelelően.

További információ a altartomány vs alkönyvtár beállítások: a [többhelyes hálózati típusú] [ wordpress-codex-types-of-networks] a WordPress kódex foglalkozó.

## <a name="enable-the-network"></a>A hálózati engedélyezése
A hálózati konfigurálva van az adatbázisban, de egy további lépéssel engedélyezheti a hálózati funkcióit. Véglegesítse a `wp-config.php` beállításait és győződjön meg arról `web.config` megfelelően irányítja a helyekhez.

Kattintás után a **telepítése** gombra a *hálózati beállítás* lapon WordPress megpróbálja frissíteni a `wp-config.php` és `web.config` fájlokat. Azonban a frissítése sikerült-e a fájlokat mindig ellenőrizze. Ha nem, ezen a képernyőn megjelennek a szükséges frissítéseket. Módosítsa és mentse a fájlokat.

Elvégzése után ezeket a frissítéseket, szüksége lesz a jelentkezzen ki, majd jelentkezzen be a wp-rendszergazda irányítópult biztonsági.

Most kell egy további menü feliratú admin menüsávon **saját webhelyek**. Ebben a menüben lehetővé teszi, hogy az új hálózati keresztül vezérlését a **hálózati rendszergazda** irányítópult.

## <a name="adding-custom-domains"></a>Egyéni tartományok hozzáadása
A [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] beépülő modul segítségével egy kezelheti az egyéni tartományok hozzáadása a hálózat valamelyik helyéhez. Ahhoz, hogy a beépülő modul megfelelően működjenek kell tennie néhány további beállítást, a portál, valamint a tartományregisztrálónál.

## <a name="enable-domain-mapping-to-the-web-app"></a>A webalkalmazás tartomány hozzárendelésének engedélyezése
A **szabad** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) üzemmódja nem támogatja az egyéni tartományok hozzáadása webalkalmazások terv. Váltson át kell **megosztott** vagy **szabványos** mód. Ehhez tegye a következőket:

* Jelentkezzen be az Azure portálon, és keresse meg a webes alkalmazást. 
* Kattintson a **vertikális felskálázás** lapján **beállítások**.
* A **általános**, válassza *megosztott* vagy *STANDARD*
* Kattintson a **mentése**

Egy üzenet jelenhet kéri, hogy ellenőrizze a módosítást, és megerősíti, hogy a webalkalmazás most merülhet fel, a költség, attól függően, használatának és beállíthatja a további konfigurálást.

Feldolgozni az új beállítások néhány másodpercet vesz igénybe a tartomány megkezdték egy időben most van.

## <a name="verify-your-domain"></a>Ellenőrizze a tartományt
Mielőtt Azure Web Apps lehetővé teszi, hogy a hely rendelni egy tartományt, először ellenőrizze, hogy rendelkezik-e a tartomány hozzárendelését a engedélyezése. Ehhez hozzá kell adnia egy új CNAME rekordot a DNS-bejegyzés.

* Jelentkezzen be a tartomány DNS-kezelő
* Hozzon létre egy új CNAME *awverify*
* Pont *awverify* való *awverify. YOUR_DOMAIN.azurewebsites.NET*

Ez eltarthat egy ideig, a DNS módosítások teljes érvénybe lépjen, így ha az alábbi lépéseket nem működnek, nyissa meg kávészünetet, ellenőrizze, majd térjen vissza, és próbálja meg újból.

## <a name="add-the-domain-to-the-web-app"></a>A tartomány hozzáadása a webalkalmazáshoz
Térjen vissza a webalkalmazás az Azure portálon keresztül, kattintson a **beállítások**, és kattintson a **egyéni tartományok és SSL**.

Ha a *SSL-beállítások* vannak jelenik meg, megjelenik a mezőket, amelyen a webalkalmazás hozzárendelni kívánt összes tartományát bemeneteket. Ha egy tartomány nem szerepel, azt nem elérhetők leképezés belül WordPress, függetlenül attól, hogy a tartomány DNS beállítása.

![Egyéni tartományok párbeszédpanel kezelése][wordpress-manage-domains]

Írja be a tartomány, a szövegmezőbe, után Azure ellenőrzi, hogy a korábban létrehozott CNAME-rekordot. Ha a DNS propagálása nem teljes, egy piros jelző jeleníti meg. Ha sikeres volt, egy zöld pipa jelenik meg. 

Jegyezze fel az IP-cím szerepel a párbeszédpanel alján. Szüksége lesz a tartomány az A rekord beállítása.

## <a name="setup-the-domain-a-record"></a>A tartomány egy olyan rekordot beállítása
Ha a többi lépés sikeres volt, előfordulhat, hogy most rendel a tartományhoz az Azure-webalkalmazásban a DNS A rekord keresztül. 

Itt megjegyezni, hogy az Azure web apps fogadja el CNAME és az A rekordok, azonban fontos, *kell* egy A rekordot használja a megfelelő tartomány hozzárendelésének engedélyezése. Egy olyan CNAME REKORDOT nem továbbítható egy másik CNAME Ez mit Azure létre az Ön YOUR_DOMAIN.azurewebsites.net.

Az előző lépésben az IP-címet használja, a DNS-kezelő lépjen vissza, és az A rekord ponthoz, hogy az IP-beállítása.

## <a name="install-and-setup-the-plugin"></a>Telepítse és állítsa be a beépülő modul
WordPress többhelyes környezet jelenleg nem rendelkezik beépített módszert a egyéni tartományokat. Van azonban egy beépülő modul nevű [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] a funkciót, amely ad meg. Jelentkezzen be a webhely, a hálózati rendszergazda része, és telepítse a **WordPress MU tartomány leképezése** beépülő modul.

Miután telepítette, és a beépülő modul aktiválása, látogasson el **beállítások** > **tartomány leképezése** a beépülő modul konfigurálása. Az első szövegmezőjének *kiszolgáló IP-címe*, adjon meg az IP-cím, a tartomány az A rekord beállítására használatos. Adja meg *beállítások* meg desire (az alapértelmezett beállításokat gyakran rendben), majd kattintson **mentése**.

## <a name="map-the-domain"></a>A tartomány hozzárendelését
Látogasson el a **irányítópult** kívánja a tartomány hozzárendelését a helyhez. Kattintson a **eszközök** > **tartomány leképezése** és az új tartomány írja be a szövegmezőbe, majd kattintson **Hozzáadás**.

Alapértelmezés szerint az új tartomány automatikusan létrehozott hely tartományhoz felülíródik. Ha azt szeretné, hogy az új tartományhoz, a jelölőnégyzet küldött összes forgalom a *ebben a blogban elsődleges tartományt* mezőben mentés előtt. Tartományok korlátlan számú hozzá egy helyhez, de csak egy elsődleges lehet.

## <a name="do-it-again"></a>Hajtsa végre újra
Az Azure Web Apps lehetővé teszi a tartományok korlátlan számú hozzáadása a webalkalmazáshoz. Szüksége lesz a hajtható végre egy másik tartomány hozzáadása a **ellenőrizze a tartományt** és **beállítása a tartomány egy olyan rekordot** részekben az egyes tartományokhoz.    

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

[ben-lobaugh]: http://ben.lobaugh.net
[ms-open-tech]: http://msopentech.com
[website-from-gallery]: https://www.windowsazure.com/develop/php/tutorials/website-from-gallery/
[wordpress-codex-create-a-network]: http://codex.wordpress.org/Create_A_Network
[website-w-mysql-and-ftp-ftp-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-ftp/#header-0
[website-w-mysql-and-git-git-setup]: https://www.windowsazure.com/develop/php/tutorials/website-w-mysql-and-git/#header-1
[wordpress-network-setup]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-network-setup.png
[wordpress-codex-types-of-networks]: http://codex.wordpress.org/Before_You_Create_A_Network#Types_of_multisite_network
[wordpress-plugin-wordpress-mu-domain-mapping]: http://wordpress.org/extend/plugins/wordpress-mu-domain-mapping/

[wordpress-manage-domains]: ./media/web-sites-php-convert-wordpress-multisite/wordpress-manage-domains.png



---
title: aaaConvert WordPress tooMultisite az Azure App Service-ben
description: "Megtudhatja, hogyan tootake WordPress-webalkalmazás létrehozása az Azure-ban hello gyűjtemény révén, és konvertálja tooWordPress a többhelyes telepítés"
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
ms.openlocfilehash: 1153f0a8043de875f081704cd0a124776758878c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-wordpress-toomultisite-in-azure-app-service"></a>Alakítsa át a WordPress tooMultisite az Azure App Service-ben
## <a name="overview"></a>Áttekintés
*Által [Ben Lobaugh][ben-lobaugh], [Microsoft nyílt technológiák Inc.][ms-open-tech]*

Ebből az oktatóanyagból megtudhatja, hogyan tootake WordPress-webalkalmazás létrehozott Azure-ban és telepítése egy WordPress többhelyes be azt a konvertálás hello gyűjteménye. Emellett megtudhatja, hogyan tooassign egy egyéni tartomány tooeach a hello alwebhelyek belül a telepítést.

Feltételezzük, hogy rendelkezik-e a WordPress egy meglévő telepítését. Ha nem így tesz, adjon követve hello [egy WordPress-webhely létrehozása az Azure-ban hello gyűjteményből][website-from-gallery].

Egy meglévő WordPress konvertálása egyetlen hely telepítési tooMultisite általában meglehetősen egyszerű, és hello Itt a kezdeti lépések származhat rögtön hello [hozzon létre egy hálózati] [ wordpress-codex-create-a-network] oldalon, hello [WordPress kódex](http://codex.wordpress.org).

Lássunk neki.

## <a name="allow-multisite"></a>Többhelyes konfiguráció engedélyezése
A többhelyes telepítés tooenable keresztül hello először `wp-config.php` hello fájl **WP\_engedélyezése\_a többhelyes telepítés** konstans. A webes alkalmazás fájljai két módszer tooedit nincsenek: hello első az FTP és a Git segítségével második hello segítségével. Ha nem ismeri, hogyan toosetup mindkét eljárás, tekintse meg a következő oktatóanyagok toohello:

* [A MySQL és az FTP-PHP-webhely][website-w-mysql-and-ftp-ftp-setup]
* [PHP-webhely MySQL és Git][website-w-mysql-and-git-git-setup]

Nyissa meg hello `wp-config.php` hello szerkesztővel, a fájlt, és adja hozzá hello következő fent hello `/* That's all, stop editing! Happy blogging. */` sor.

    /* Multisite */

    define( 'WP_ALLOW_MULTISITE', true );

Lehet, hogy toosave hello fájlt, és töltse fel hátsó toohello server!

## <a name="network-setup"></a>Hálózati beállítás
Jelentkezzen be toohello *wp-rendszergazda* , és a webes alkalmazást területe kell megjelennie az hello egy új cikket **eszközök** nevű menü **hálózati beállítás**. Kattintson a **hálózati beállítás** , és töltse ki a hálózat hello részleteit.

![Hálózati beállítási képernyője][wordpress-network-setup]

Ez az oktatóanyag használja hello *alkönyvtár* séma helyet, mert mindig működnek, és azt szeretné állítani minden aloldal egyéni tartományok hello oktatóanyag későbbi részében. Azonban lehetséges toosetup altartomány telepíteni, ha leképez egy tartományba hello keresztül kell [Azure Portal](https://portal.azure.com) és helyettesítő DNS beállításának megfelelően.

További információ a altartomány vs alkönyvtár beállítások: hello [többhelyes hálózati típusú] [ wordpress-codex-types-of-networks] hello WordPress kódex foglalkozó.

## <a name="enable-hello-network"></a>Hello hálózati engedélyezése
hello hálózati most hello adatbázisban van konfigurálva, de egy további lépés tooenable hello hálózati funkciókat. Hello véglegesítése `wp-config.php` beállításait és győződjön meg arról `web.config` megfelelően irányítja a helyekhez.

Hello kattintás után **telepítése** hello gombjára *hálózati beállítás* lapon WordPress megpróbál tooupdate hello `wp-config.php` és `web.config` fájlokat. Azonban mindig ellenőrizze hello fájlok tooensure hello frissítése sikerült-e. Ha nem, ezen a képernyőn megjelennek hello szükséges frissítéseket. Módosítsa és mentse a hello fájlokat.

Elvégzése után ezeket a frissítéseket, szüksége lesz a kimenő toolog és a naplófájlok hello wp-rendszergazda irányítópult vissza.

Most kell egy további menü hello admin sávon feliratú **saját webhelyek**. Ebben a menüben toocontrol lehetővé teszi az új hálózati keresztül hello **hálózati rendszergazda** irányítópult.

## <a name="adding-custom-domains"></a>Egyéni tartományok hozzáadása
Hello [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] beépülő modul segítségével kezelheti tooadd egyéni tartományok tooany hely a hálózaton. Ahhoz, hogy hello beépülő modul toooperate megfelelően kell toodo néhány további beállítást, a portál hello, valamint a tartományregisztrálónál.

## <a name="enable-domain-mapping-toohello-web-app"></a>Tartomány leképezése toohello webalkalmazás engedélyezése
Hello **szabad** [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) terv üzemmódja nem támogatja az egyéni tartományok tooWeb alkalmazások hozzáadását. Szüksége lesz tooswitch túl**megosztott** vagy **szabványos** mód. toodo ezt:

* Jelentkezzen be Azure Portal toohello, és keresse meg a webes alkalmazást. 
* Kattintson a hello **vertikális felskálázás** lapján **beállítások**.
* A **általános**, válassza *megosztott* vagy *STANDARD*
* Kattintson a **mentése**

Előfordulhat, hogy egy üzenet jelenik meg tooverify hello módosítása és a webalkalmazás most fel Önnek a költség, attól függően, használati és egyéb konfigurációkészletet hello tudomásul.

Tooprocess hello új beállítások, néhány másodpercig tart most az egy időben toostart beállítása a tartományban van.

## <a name="verify-your-domain"></a>Ellenőrizze a tartományt
Azure Web Apps toomap egy tartományi toohello hely lehetővé teszi a, mielőtt először tooverify, hogy rendelkezik-e hello engedélyezési toomap hello tartomány. toodo Igen, hozzá kell adnia egy új CNAME rekord tooyour DNS-bejegyzést.

* Jelentkezzen be tooyour tartomány DNS-kezelő
* Hozzon létre egy új CNAME *awverify*
* Pont *awverify* túl*awverify. YOUR_DOMAIN.azurewebsites.NET*

Előfordulhat, hogy teljes körű érvénybe hello DNS módosítások toogo némi időt vesz igénybe, így hello lépések nem működnek, ha nyissa meg kávészünetet, ellenőrizze, majd térjen vissza, és próbálja meg újból.

## <a name="add-hello-domain-toohello-web-app"></a>Hello tartomány toohello webalkalmazás hozzáadása
Kattintson a webalkalmazás visszatérési tooyour hello Azure-portálon keresztül **beállítások**, és kattintson a **egyéni tartományok és SSL**.

Ha hello *SSL-beállítások* vannak jelenik meg, láthatja hello mezők hol kívánja tooassign tooyour webalkalmazás hello tartományok bemeneteket. Ha egy tartomány nem szerepel, azt nem elérhetők leképezés belül WordPress, függetlenül attól, milyen hello tartományi DNS beállítása.

![Egyéni tartományok párbeszédpanel kezelése][wordpress-manage-domains]

Írja be a tartomány hello szövegmezőbe, után Azure ellenőrzi, hogy hello korábban létrehozott CNAME-rekordot. Ha hello DNS propagálása nem teljes, egy piros jelző jeleníti meg. Ha sikeres volt, egy zöld pipa jelenik meg. 

Jegyezze fel az IP-cím szerepel a listában hello párbeszédpanel hello alján hello. A tartomány szüksége lesz a toosetup hello rekord.

## <a name="setup-hello-domain-a-record"></a>A telepítő hello tartomány A rekord
Hello más lépéseket volt sikeres, ha Ön most jogosult hello tartomány tooyour Azure web apphoz keresztül DNS A rekord. 

Fontos fontos toonote itt, a Azure-webalkalmazásokban azonban CNAME és az A rekordok elfogadja, *kell* egy A rekord tooenable megfelelő tartomány leképezése használja. Egy olyan CNAME REKORDOT nem továbbítható tooanother CNAME, vagyis mi Azure létre az Ön YOUR_DOMAIN.azurewebsites.net.

Az előző lépésben hello hello IP-címet használja, térjen vissza a tooyour DNS-kezelő és a telepítő hello egy rekord toopoint toothat IP-cím.

## <a name="install-and-setup-hello-plugin"></a>Telepítéséhez és beállításához hello beépülő modul
WordPress többhelyes környezet jelenleg nem rendelkezik olyan beépített módszerrel toomap egyéni tartományok. Van azonban egy beépülő modul nevű [WordPress MU tartomány leképezése] [ wordpress-plugin-wordpress-mu-domain-mapping] hello funkció, amely ad meg. Jelentkezzen be a webhely toohello hálózati rendszergazda része, és telepítse a hello **WordPress MU tartomány leképezése** beépülő modul.

Miután telepítése és aktiválása hello beépülő modul, látogasson el a **beállítások** > **tartomány leképezése** tooconfigure hello beépülő modul. Hello első szövegmezőben *kiszolgáló IP-címe*, bemeneti hello toosetup használt IP-cím hello hello tartomány egy olyan rekordot. Adja meg *beállítások* felügyelni (hello alapértelmezett gyakran rendben), majd **mentése**.

## <a name="map-hello-domain"></a>Hello tartományban
A Microsoft hello **irányítópult** hello hely toomap hello tartomány kívánja. Kattintson a **eszközök** > **tartomány leképezése** és típus hello új tartomány hello szövegmezőben, majd kattintson a **Hozzáadás**.

Alapértelmezés szerint a hello új tartományt egy átírt toohello automatikusan létrehozott hely tartomány lesz. Ha szeretné toohave új tartomány összes elküldött forgalom toohello, ellenőrizze a hello *ebben a blogban elsődleges tartományt* mezőben mentés előtt. Tartományok tooa hely korlátlan számú adhat hozzá, de csak egy elsődleges lehet.

## <a name="do-it-again"></a>Hajtsa végre újra
Az Azure Web Apps tooadd tartományok tooa webalkalmazás korlátlan számú engedélyezése. tooadd tooexecute hello szüksége lesz egy másik tartomány **ellenőrizze a tartományt** és **hello tartomány rekord telepítő** részekben az egyes tartományokhoz.    

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)

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



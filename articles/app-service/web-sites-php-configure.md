---
title: "PHP-konfigurálás az Azure App Service Web Apps |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az alapértelmezett a PHP-telepítés vagy egy egyéni PHP-telepítés hozzáadása az Azure App Service Web Apps."
services: app-service
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 95c4072b-8570-496b-9c48-ee21a223fb60
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 624dd416f37aacdb3d2f6e59afdc2efe646e610b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>Az Azure App Service Web Apps PHP-konfigurálás
## <a name="introduction"></a>Bevezetés
Ez az útmutató bemutatja, hogyan konfigurálhatja a beépített PHP futtatókörnyezetben a Web Apps [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), adjon meg egy egyéni PHP futtatókörnyezetben és bővítmények engedélyezése. App Service használni, Regisztráljon a [ingyenes próbaverzió]. Ahhoz, hogy a legtöbbet tudja kihozni Ez az útmutató, kell először létrehozhat egy PHP webalkalmazást az App Service-ben.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a>Útmutató: a beépített PHP verzióját módosítása
Alapértelmezés szerint a PHP 5.5 telepítve és használhatja az App Service webalkalmazás létrehozásakor. A legjobb módja a rendelkezésre álló kiadási változat, az alapértelmezett konfigurációval, és az engedélyezett bővítmények, központi telepítése egy parancsfájlt, amely meghívja a [phpinfo()] függvény.

PHP 5.6 és a PHP 7.0-s verziója is, elérhető, de alapértelmezés szerint nem engedélyezett. A PHP verzióját frissítéséhez hajtsa végre az ezen módszerek egyikét:

### <a name="azure-portal"></a>Azure Portal
1. Keresse meg a webalkalmazás a [Azure Portal](https://portal.azure.com) , majd kattintson a a **beállítások** gombra.
   
    ![A webalkalmazás-beállítások][settings-button]
2. Az a **beállítások** panelen válassza ki **Alkalmazásbeállítások** , és válassza az új PHP-verziót.
   
    ![Alkalmazásbeállítások][application-settings]
3. Kattintson a **mentése** gomb tetején a **webalkalmazás-beállításainak** panelen.
   
    ![Konfigurációs beállítások mentéséhez][save-button]

### <a name="azure-powershell-windows"></a>Az Azure PowerShell (Windows)
1. Nyissa meg az Azure PowerShell, és jelentkezzen be a fiók:
   
        PS C:\> Login-AzureRmAccount
2. Állítsa be a PHP verzióját a webalkalmazás számára.
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. A PHP verzióját most van beállítva. Ezek a beállítások ellenőrizheti:
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a>Azure parancssori felület (Linux, Mac, Windows)
Az Azure parancssori felület használatához rendelkeznie kell **Node.js** telepítve a számítógépre.

1. Nyissa meg terminált, és jelentkezzen be fiókjába.
   
        azure login
2. Állítsa be a PHP verzióját a webalkalmazás számára.
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. A PHP verzióját most van beállítva. Ezek a beállítások ellenőrizheti:
   
        azure site show {app-name}

> [!NOTE] 
> A [Azure CLI 2.0](https://github.com/Azure/azure-cli) egyenértékű a fenti parancsok a következők:
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-the-built-in-php-configurations"></a>Útmutató: a beépített PHP-beállítások módosítása
Bármely beépített PHP futási időben az alábbi lépéseket követve módosíthatja a konfigurációs beállításokat. (Php.ini irányelvek kapcsolatos információkért lásd: [php.ini irányelvek].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>A PHP módosítása\_INI\_felhasználó, a PHP\_INI\_PERDIR, a PHP\_INI\_beállításokkal
1. Adja hozzá a [. user.ini] fájlt a gyökérkönyvtárban.
2. Adja hozzá a konfigurációs beállításokat a `.user.ini` használja ugyanazt a szintaxist használja a fájl egy `php.ini` fájlt. Például, ha szeretné kapcsolni a `display_errors` meg, és állítsa be a beállítás `upload_max_filesize` 10M beállítást a `.user.ini` fájl tartalmazza egyrészt az ezt a szöveget:
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
3. A webes alkalmazás telepítése.
4. A webalkalmazás újraindítása. (Újraindítás szükség, mert a gyakorisága mely PHP beolvassa `.user.ini` fájlok szabályozza a `user_ini.cache_ttl` beállítás, amely egy rendszerszintű beállítás, és alapértelmezés szerint 300 másodpercig (5 percig). A webalkalmazás újraindítása kényszeríti a php-hez az új beállítások beolvasása a `.user.ini` fájl.)

Használata helyett egy `.user.ini` fájlt, használhatja a [ini_set()] parancsfájlok, amelyek nem rendszerszintű irányelvek konfigurációs beállítások megadása a függvényt.

### <a name="changing-phpinisystem-configuration-settings"></a>A PHP módosítása\_INI\_rendszer konfigurációs beállításait
1. Egy Alkalmazásbeállítás hozzáadása a webalkalmazáshoz kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`
2. Hozzon létre egy `settings.ini` fájlt a Kudu konzol használata (http://&lt;site-name&gt;. scm.azurewebsite.net) az a `d:\home\site\ini` könyvtár.
3. Adja hozzá a konfigurációs beállításokat a `settings.ini` fájlt használhatja ugyanazt a szintaxist használja a php.ini fájlban. Például, ha szeretné, mutasson a `curl.cainfo` beállítást egy `*.crt` fájlt, és állítsa be a "wincache.maxfilesize" 512 KB, a `settings.ini` fájl tartalmazza egyrészt az ezt a szöveget:
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. Indítsa újra a webalkalmazás betöltése a módosításokat.

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a>Útmutató: az alapértelmezett a PHP futtatókörnyezetben bővítmények engedélyezése
Az előző szakaszban leírtaknak megfelelően-e a legjobb módszer az alapértelmezett PHP verzióját, az alapértelmezett konfigurációval és az engedélyezett bővítmények telepítése egy parancsprogram, amely behívja [phpinfo()]. További kiterjesztések, kövesse az alábbi lépéseket:

### <a name="configure-via-ini-settings"></a>Keresztül ini-beállítások konfigurálása
1. Adja hozzá a `ext` könyvtárból a `d:\home\site` könyvtár.
2. PUT `.dll` bővítmény-fájlok a `ext` könyvtárat (például `php_xdebug.dll`). Győződjön meg arról, hogy a bővítmények alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis kompatibilisek legyenek-e.
3. Egy Alkalmazásbeállítás hozzáadása a webalkalmazáshoz kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`
4. Hozzon létre egy `ini` fájlt `d:\home\site\ini` nevű `extensions.ini`.
5. Adja hozzá a konfigurációs beállításokat a `extensions.ini` fájlt használhatja ugyanazt a szintaxist használja a php.ini fájlban. Ha szeretné engedélyezni a MongoDB és XDebug bővítmények, például a `extensions.ini` fájl tartalmazza egyrészt az ezt a szöveget:
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. Indítsa újra a webalkalmazás betöltése a módosításokat.

### <a name="configure-via-app-setting"></a>Alkalmazásbeállítás keresztül konfigurálása
1. Adja hozzá a `bin` könyvtár a gyökérkönyvtárba.
2. PUT `.dll` bővítmény-fájlok a `bin` könyvtárat (például `php_xdebug.dll`). Győződjön meg arról, hogy a bővítmények alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis kompatibilisek legyenek-e.
3. A webes alkalmazás telepítése.
4. Keresse meg a webalkalmazás az Azure portálon, majd kattintson a a **beállítások** gombra.
   
    ![A webalkalmazás-beállítások][settings-button]
5. Az a **beállítások** panelen válassza ki **Alkalmazásbeállítások** görgessen a **Alkalmazásbeállítások** szakasz.
6. Az a **Alkalmazásbeállítások** szakaszban, hozzon létre egy **PHP_EXTENSIONS** kulcs. A kulcs értéke lenne egy elérési út a webhely gyökeréhez viszonyítva: **bin\your-ext-fájl**.
   
    ![Alkalmazásbeállítások a bővítmény engedélyezése][php-extensions]
7. Kattintson a **mentése** gomb tetején a **webalkalmazás-beállításainak** panelen.
   
    ![Konfigurációs beállítások mentéséhez][save-button]

A Zend bővítmények használatával is támogatottak a **PHP_ZENDEXTENSIONS** kulcs. Ahhoz, hogy több kiterjesztést, vesszővel tagolt listáját tartalmazza `.dll` fájlok esetében az alkalmazás-beállítás értékét.

## <a name="how-to-use-a-custom-php-runtime"></a>Útmutató: egy egyéni PHP futtatókörnyezetben használja
Az alapértelmezett PHP futtatókörnyezetben helyett App Service Web Apps használhatja egy Ön által biztosított PHP-parancsfájlok végrehajtása PHP futtatókörnyezetben. Az Ön által biztosított futásidejű konfigurálhatja egy `php.ini` is nyújtó fájlt. A webalkalmazásokkal egy egyéni PHP futtatókörnyezetben használatához kövesse az alábbi lépéseket.

1. Szerezzen be egy nem szálbiztos, VC9 vagy VC11 PHP for Windows kompatibilis verziója. PHP for Windows újabb verzióiban itt található: [http://windows.php.net/download/]. Itt az archívumban található régebbi kiadásai: [http://windows.php.net/downloads/releases/archives/].
2. Módosítsa a `php.ini` fájl a futási időben. Vegye figyelembe, hogy a konfigurációs beállítások, amelyek a rendszer-szintű-csak irányelvek webes alkalmazások által használt rendszer figyelmen kívül hagyja. (Rendszer-szintű-csak irányelvek kapcsolatos információkért lásd: [php.ini irányelvek]).
3. Szükség esetén bővítmények vegye fel a PHP futtatókörnyezetben, és engedélyezze azokat a `php.ini` fájlt.
4. Adja hozzá a `bin` könyvtárból a gyökérkönyvtár, és a put azt a PHP futtatókörnyezetben tartalmazó könyvtárat (például `bin\php`).
5. A webes alkalmazás telepítése.
6. Keresse meg a webalkalmazás az Azure portálon, majd kattintson a a **beállítások** gombra.
   
    ![A webalkalmazás-beállítások][settings-button]
7. Az a **beállítások** panelen válassza ki **Alkalmazásbeállítások** görgessen a **kezelőleképezések** szakasz. Adja hozzá `*.php` a bővítmény mezőben, majd adja hozzá az elérési útját a `php-cgi.exe` végrehajtható. Ennél a PHP futtatókörnyezetben a `bin` directory alkalmazás gyökérkönyvtárában, az elérési út lesz `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
   
    ![Adja meg a kezelő meg leképezései][handler-mappings]
8. Kattintson a **mentése** gomb tetején a **webalkalmazás-beállításainak** panelen.
   
    ![Konfigurációs beállítások mentéséhez][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Hogyan: szerkesztő automatizálhatják az Azure-ban
Alapértelmezés szerint az App Service nem minden composer.json, ha nincs fiókja, a PHP-projektben. Ha [Git-telepítésének](app-service-deploy-local-git.md), engedélyezheti a feldolgozás során composer.json `git push` Composer bővítményt engedélyezésével.

> [!NOTE]
> Is [szerkesztő támogatja az App Service itt szavazzon](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
> 
> 

1. A PHP a webalkalmazás-alkalmazás paneljén a [Azure-portálon](https://portal.azure.com), kattintson a **eszközök** > **bővítmények**.
   
    ![Az Azure portál beállítások panel szerkesztő automatizálására az Azure-ban](./media/web-sites-php-configure/composer-extension-settings.png)
2. Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.
   
    ![Adja hozzá az Azure-ban szerkesztő automatizálására Composer bővítményt](./media/web-sites-php-configure/composer-extension-add.png)
3. Kattintson a **OK** jogi feltételek elfogadásának. Kattintson a **OK** újra a adja hozzá a kiterjesztést.
   
    A **bővítményeket telepített** panel most megjelenik a Composer bővítményt.  
    ![Fogadja el a jogi feltételeket, hogy szerkesztő automatizálhatják az Azure-ban](./media/web-sites-php-configure/composer-extension-view.png)
4. Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban. Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.
   
    ![Git-telepítés az szerkesztő Automation szolgáltatásban, az Azure-ban](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Következő lépések
További információkért lásd: a [PHP fejlesztői központ](/develop/php/).

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

[ingyenes próbaverzió]: https://www.windowsazure.com/pricing/free-trial/
[phpinfo()]: http://php.net/manual/en/function.phpinfo.php
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
[php.ini irányelvek]: http://www.php.net/manual/en/ini.list.php
[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php
[ini_set()]: http://www.php.net/manual/en/function.ini-set.php
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
[http://windows.php.net/download/]: http://windows.php.net/download/
[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png


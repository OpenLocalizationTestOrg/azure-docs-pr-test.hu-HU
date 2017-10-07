---
title: az Azure App Service Web Apps PHP aaaConfigure |} Microsoft Docs
description: "Ismerje meg, hogyan tooconfigure alapértelmezett PHP-telepítés hello, vagy egy egyéni PHP-telepítés hozzáadása az Azure App Service Web Apps."
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
ms.openlocfilehash: 2e461e4a269a4ce5614f5f05560f38bc53066251
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-php-in-azure-app-service-web-apps"></a>Az Azure App Service Web Apps PHP-konfigurálás
## <a name="introduction"></a>Bevezetés
Ez az útmutató bemutatja, hogyan tooconfigure hello Web Apps a beépített PHP futtatókörnyezetben [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), adjon meg egy egyéni PHP futtatókörnyezetben és bővítmények engedélyezése. App Service-ben toouse regisztráljon hello [ingyenes próbaverzió]. tooget hello leginkább az ebben az útmutatóban, először létre kell hoznia egy PHP webalkalmazást az App Service-ben.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-hello-built-in-php-version"></a>Útmutató: a PHP verzió beépített módosítása hello
Alapértelmezés szerint a PHP 5.5 telepítve és használhatja az App Service webalkalmazás létrehozásakor. hello legjobb módja toosee hello rendelkezésre álló kiadási változat, alapértelmezett konfigurációval és hello engedélyezett bővítmények között olyan parancsfájlt, amely meghívja a hello toodeploy [phpinfo()] függvény.

PHP 5.6 és a PHP 7.0-s verziója is, elérhető, de alapértelmezés szerint nem engedélyezett. tooupdate hello PHP verzióját, kövesse az alábbi módszerek egyikét:

### <a name="azure-portal"></a>Azure Portal
1. Keresse meg a webalkalmazás tooyour hello [Azure Portal](https://portal.azure.com) , majd kattintson a hello **beállítások** gombra.
   
    ![A webalkalmazás-beállítások][settings-button]
2. A hello **beállítások** panelen válassza ki **Alkalmazásbeállítások** , és válassza a hello új PHP verzióját.
   
    ![Alkalmazásbeállítások][application-settings]
3. Hello kattintson **mentése** hello hello tetején gomb **webalkalmazás-beállításainak** panelen.
   
    ![Konfigurációs beállítások mentéséhez][save-button]

### <a name="azure-powershell-windows"></a>Az Azure PowerShell (Windows)
1. Nyissa meg az Azure PowerShell és a bejelentkezési tooyour fiókot:
   
        PS C:\> Login-AzureRmAccount
2. Állítsa be a PHP verzióját hello hello webalkalmazás.
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. hello PHP verziója most van beállítva. Ezek a beállítások ellenőrizheti:
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a>Azure parancssori felület (Linux, Mac, Windows)
toouse hello Azure parancssori felület, akkor **Node.js** telepítve a számítógépre.

1. Nyissa meg a terminált, és a bejelentkezési tooyour fiók.
   
        azure login
2. Állítsa be a PHP verzióját hello hello webalkalmazás.
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. hello PHP verziója most van beállítva. Ezek a beállítások ellenőrizheti:
   
        azure site show {app-name}

> [!NOTE] 
> Hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) , amelyek a fenti egyenértékű toohello parancsok a következők:
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-hello-built-in-php-configurations"></a>Hogyan: hello beépített PHP-beállítások módosítása
A beépített PHP futási időben hello alábbi lépéseket követve módosíthatja hello konfigurációs beállításokat. (Php.ini irányelvek kapcsolatos információkért lásd: [php.ini irányelvek].)

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a>A PHP módosítása\_INI\_felhasználó, a PHP\_INI\_PERDIR, a PHP\_INI\_beállításokkal
1. Adja hozzá a [. user.ini] fájl tooyour gyökérkönyvtár.
2. Adja hozzá a konfigurációs beállítások toohello `.user.ini` fájl használata a használhatja ugyanazt a szintaxist hello egy `php.ini` fájl. Például, ha tooturn hello `display_errors` meg, és állítsa be a beállítás `upload_max_filesize` too10M, beállítása a `.user.ini` fájl tartalmazza egyrészt az ezt a szöveget:
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on toowrite errors tood:\home\LogFiles\php_errors.log
        ; log_errors=On
3. A webes alkalmazás telepítése.
4. Hello a webalkalmazás újraindítása. (Újraindítás szükség, mert mely PHP hello gyakorisága beolvassa `.user.ini` fájlok szabályozza hello `user_ini.cache_ttl` beállítás, amely egy rendszerszintű beállítás, és alapértelmezés szerint 300 másodpercig (5 percig). Újraindítás hello webalkalmazás kényszeríti a PHP tooread hello új beállításai hello `.user.ini` fájl.)

Egy alternatív toousing, egy `.user.ini` fájlt, használhatja a hello [ini_set()] parancsfájlok tooset konfigurációs beállítások, amelyek nem rendszerszintű irányelvek függvényt.

### <a name="changing-phpinisystem-configuration-settings"></a>A PHP módosítása\_INI\_rendszer konfigurációs beállításait
1. Adja hozzá az Alkalmazásbeállítás tooyour webalkalmazás hello kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`
2. Hozzon létre egy `settings.ini` fájlt a Kudu-konzollal (http://&lt;site-name&gt;. scm.azurewebsite.net) a hello `d:\home\site\ini` directory.
3. Adja hozzá a konfigurációs beállítások toohello `settings.ini` fájl használatával hello ugyanazt a szintaxist a php.ini fájlt használja. Például, ha toopoint hello `curl.cainfo` tooa beállítás `*.crt` fájlt, és too512K, beállítása wincache.maxfilesize a `settings.ini` fájl tartalmazza egyrészt az ezt a szöveget:
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. Indítsa újra a webes alkalmazás tooload hello módosításokat.

## <a name="how-to-enable-extensions-in-hello-default-php-runtime"></a>Hogyan: hello alapértelmezett a PHP futtatókörnyezetben bővítmények engedélyezése
Hello előző szakaszban, hello legjobb módja toosee hello alapértelmezett PHP verzióját, az alapértelmezett konfiguráció és a engedélyezve hello leírtaknak megfelelően-bővítmények között olyan parancsfájlt, amely meghívja a toodeploy [phpinfo()]. tooenable további kiterjesztések, kövesse az alábbi hello lépéseket:

### <a name="configure-via-ini-settings"></a>Keresztül ini-beállítások konfigurálása
1. Adja hozzá a `ext` directory toohello `d:\home\site` könyvtár.
2. PUT `.dll` szerepkörbővítmény-fájlokat a hello `ext` könyvtárat (például `php_xdebug.dll`). Győződjön meg arról, hogy hello bővítmények kompatibilisek alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis.
3. Adja hozzá az Alkalmazásbeállítás tooyour webalkalmazás hello kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`
4. Hozzon létre egy `ini` fájlt `d:\home\site\ini` nevű `extensions.ini`.
5. Adja hozzá a konfigurációs beállítások toohello `extensions.ini` fájl használatával hello ugyanazt a szintaxist a php.ini fájlt használja. Ha tooenable hello MongoDB és XDebug bővítmények, például a `extensions.ini` fájl tartalmazza egyrészt az ezt a szöveget:
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. Indítsa újra a webes alkalmazás tooload hello módosításokat.

### <a name="configure-via-app-setting"></a>Alkalmazásbeállítás keresztül konfigurálása
1. Adja hozzá a `bin` directory toohello gyökérkönyvtár.
2. PUT `.dll` szerepkörbővítmény-fájlokat a hello `bin` könyvtárat (például `php_xdebug.dll`). Győződjön meg arról, hogy hello bővítmények kompatibilisek alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis.
3. A webes alkalmazás telepítése.
4. Keresse meg a webalkalmazás tooyour hello Azure portálon, majd kattintson a hello **beállítások** gombra.
   
    ![A webalkalmazás-beállítások][settings-button]
5. A hello **beállítások** panelen válassza ki **Alkalmazásbeállítások** toohello görgessen **Alkalmazásbeállítások** szakasz.
6. A hello **Alkalmazásbeállítások** szakaszban, hozzon létre egy **PHP_EXTENSIONS** kulcs. a kulcs hello értéke lenne a legfelső szintű elérési útja relatív toowebsite: **bin\your-ext-fájl**.
   
    ![Alkalmazásbeállítások a bővítmény engedélyezése][php-extensions]
7. Hello kattintson **mentése** hello hello tetején gomb **webalkalmazás-beállításainak** panelen.
   
    ![Konfigurációs beállítások mentéséhez][save-button]

A Zend bővítmények használatával is támogatottak a **PHP_ZENDEXTENSIONS** kulcs. tooenable több kiterjesztést tartalmaz egy vesszővel elválasztott listája `.dll` hello app beállításérték fájljait.

## <a name="how-to-use-a-custom-php-runtime"></a>Útmutató: egy egyéni PHP futtatókörnyezetben használja
Hello alapértelmezett PHP futtatókörnyezetben helyett App Service Web Apps használhatja egy adni tooexecute PHP-parancsfájlok PHP futtatókörnyezetben. Ön által biztosított hello futásidejű konfigurálhatja egy `php.ini` is nyújtó fájlt. a webalkalmazásokkal, egy egyéni PHP futtatókörnyezetben toouse kövesse hello lépéseket.

1. Szerezzen be egy nem szálbiztos, VC9 vagy VC11 PHP for Windows kompatibilis verziója. PHP for Windows újabb verzióiban itt található: [http://windows.php.net/download/]. Régebbi kiadásai hello archív itt található: [http://windows.php.net/downloads/releases/archives/].
2. Módosítsa a hello `php.ini` fájl a futási időben. Vegye figyelembe, hogy a konfigurációs beállítások, amelyek a rendszer-szintű-csak irányelvek webes alkalmazások által használt rendszer figyelmen kívül hagyja. (Rendszer-szintű-csak irányelvek kapcsolatos információkért lásd: [php.ini irányelvek]).
3. Igény szerint adja hozzá a bővítmények tooyour PHP futtatókörnyezetben, és lehetővé teszi a hello `php.ini` fájlt.
4. Adja hozzá a `bin` directory tooyour legfelső szintű és put hello könyvtárat, amely tartalmazza azt a PHP futtatókörnyezetben (például `bin\php`).
5. A webes alkalmazás telepítése.
6. Keresse meg a webalkalmazás tooyour hello Azure portálon, majd kattintson a hello **beállítások** gombra.
   
    ![A webalkalmazás-beállítások][settings-button]
7. A hello **beállítások** panelen válassza ki **Alkalmazásbeállítások** toohello görgessen **kezelőleképezések** szakasz. Adja hozzá `*.php` toohello bővítmény mezőben, majd adja hozzá az elérési út toohello hello `php-cgi.exe` végrehajtható. Ha a PHP futtatókörnyezetben be hello `bin` hello elérési út lesz az alkalmazás gyökérkönyvtárában hello könyvtár, `D:\home\site\wwwroot\bin\php\php-cgi.exe`.
   
    ![Adja meg a kezelő meg leképezései][handler-mappings]
8. Hello kattintson **mentése** hello hello tetején gomb **webalkalmazás-beállításainak** panelen.
   
    ![Konfigurációs beállítások mentéséhez][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a>Hogyan: szerkesztő automatizálhatják az Azure-ban
Alapértelmezés szerint az App Service nem minden composer.json, ha nincs fiókja, a PHP-projektben. Ha [Git-telepítésének](app-service-deploy-local-git.md), engedélyezheti a feldolgozás során composer.json `git push` hello Composer bővítményt engedélyezésével.

> [!NOTE]
> Is [szerkesztő támogatja az App Service itt szavazzon](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!
> 
> 

1. A PHP a webalkalmazás-alkalmazás paneljén hello [Azure-portálon](https://portal.azure.com), kattintson a **eszközök** > **bővítmények**.
   
    ![Az Azure portál beállítások panel tooenable szerkesztő automation az Azure-ban](./media/web-sites-php-configure/composer-extension-settings.png)
2. Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.
   
    ![Composer bővítményt tooenable szerkesztő automation hozzáadása az Azure-ban](./media/web-sites-php-configure/composer-extension-add.png)
3. Kattintson a **OK** tooaccept jogi feltételeket. Kattintson a **OK** újra tooadd hello bővítmény.
   
    Hello **bővítményeket telepített** panel mostantól megjeleníti hello Composer bővítményt.  
    ![Fogadja el a jogi feltételeket tooenable szerkesztő automation az Azure-ban](./media/web-sites-php-configure/composer-extension-view.png)
4. Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban hello. Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.
   
    ![Git-telepítés az szerkesztő Automation szolgáltatásban, az Azure-ban](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
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


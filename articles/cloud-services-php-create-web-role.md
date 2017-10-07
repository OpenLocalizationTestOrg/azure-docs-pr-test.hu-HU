---
title: "aaaCreate Azure php webes és feldolgozói szerepkörök |} Microsoft Docs"
description: "Egy útmutató toocreating PHP webes és feldolgozói szerepkörök az Azure-felhőszolgáltatásban és konfigurálását hello PHP futtatókörnyezetben."
services: 
documentationcenter: php
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9f7ccda0-bd96-4f7b-a7af-fb279a9e975b
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 04a6e8c9c379cb0f854645941b6bc7d614bb91f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-php-web-and-worker-roles"></a>Hogyan toocreate PHP webes és feldolgozói szerepkörök
## <a name="overview"></a>Áttekintés
Ez az útmutató bemutatja, hogyan toocreate PHP webes vagy feldolgozói szerepkörök Windows fejlesztői környezetre, a PHP verzióját választhat hello "beépített" verziók, hello PHP konfigurációjának módosítása, bővítmények engedélyezésével és végezetül telepítése tooAzure. Azt is ismerteti, hogyan tooconfigure egy webes vagy feldolgozói szerepkör toouse egy Ön által biztosított PHP futtatókörnyezetben (az egyéni konfiguráció és bővítmények).

## <a name="what-are-php-web-and-worker-roles"></a>Mik azok a PHP webes és feldolgozói szerepkörök?
Azure biztosít három számítási modellt futó alkalmazások: Azure App Service, Azure virtuális gépek és Azure Cloud Services. Mindhárom modell támogatja a PHP. Felhőszolgáltatás, amely magában foglalja a webes és feldolgozói szerepköröket, itt *platformszolgáltatást (PaaS)*. A felhőszolgáltatásban a webes szerepkör lehetővé teszi egy dedikált Internet Information Services (IIS) webes server toohost előtér-webkiszolgáló. A feldolgozói szerepkör aszinkron, hosszan futó vagy folyamatos feladatokat futtat függetlenül a felhasználói interakcióktól vagy bemenettől futtathatja.

Ezekről a beállításokról további információkért lásd: [Azure által biztosított lehetőségek futtató számítási](cloud-services/cloud-services-choose-me.md).

## <a name="download-hello-azure-sdk-for-php"></a>Php-hez tartozó hello Azure SDK letöltése
Hello [Azure SDK for PHP] több összetevőből áll. Ez a cikk kettő fogja használni: Azure PowerShell és az Azure emulátorok hello. A két összetevő keresztül hello Microsoft Webplatform-telepítővel telepíthető. További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="create-a-cloud-services-project"></a>Egy Felhőszolgáltatás-projekt létrehozása
a PHP webes vagy feldolgozói szerepkör létrehozásának első lépése hello toocreate Azure szolgáltatás projektben. az Azure szolgáltatás projekt tárolójaként logikai webes és feldolgozói szerepkörök, és hello projektet tartalmaz [szolgáltatás definíciós (.csdef)] és [szolgáltatás konfigurációs (.cscfg)] fájlokat.

toocreate egy új Azure Service-projekt Azure PowerShell futtatása rendszergazdaként, és hajtsa végre a következő parancs hello:

    PS C:\>New-AzureServiceProject myProject

Ez a parancs létrehoz egy új könyvtárat (`myProject`) toowhich webes és feldolgozói szerepkörök is hozzáadhat.

## <a name="add-php-web-or-worker-roles"></a>A PHP webes vagy feldolgozói szerepkörök hozzáadása
a PHP webes szerepkör tooa projekt tooadd futtassa a következő parancsot a hello projekt gyökérkönyvtárában hello:

    PS C:\myProject> Add-AzurePHPWebRole roleName

A feldolgozói szerepkör esetében az alábbi parancsot használja:

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> Hello `roleName` paraméter nem kötelező. Ha ki van hagyva, hello szerepkörnév automatikusan generált. hello létrehozott első webes szerepkör lesz `WebRole1`, hello második lesz `WebRole2`, és így tovább. hello létrehozott első feldolgozói szerepkör lesz `WorkerRole1`, hello második lesz `WorkerRole2`, és így tovább.
>
>

## <a name="specify-hello-built-in-php-version"></a>Adja meg a hello beépített PHP verzióját
A PHP webes vagy feldolgozói szerepkör tooa projekt hozzáadásakor hello projekt konfigurációs fájlok módosítják, hogy a PHP esetén telepítve lesz az alkalmazás összes webes vagy feldolgozói példányt telepítették. Futtassa a következő parancs hello alapértelmezés szerint telepítendő PHP toosee hello verziója:

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

hello fenti hello parancs kimenetében alábbihoz hasonlóan fog megjelenni toowhat alább láthatók. Ebben a példában hello `IsDefault` jelző értéke túl`true` php 5.3.17, jelezve, hogy hello alapértelmezett PHP verzióját telepítve lesz.

```
Runtime Version     PackageUri                      IsDefault
------- -------     ----------                      ---------
Node 0.6.17         http://nodertncu.blob.core...   False
Node 0.6.20         http://nodertncu.blob.core...   True
Node 0.8.4          http://nodertncu.blob.core...   False
IISNode 0.1.21      http://nodertncu.blob.core...   True
Cache 1.8.0         http://nodertncu.blob.core...   True
PHP 5.3.17          http://nodertncu.blob.core...   True
PHP 5.4.0           http://nodertncu.blob.core...   False
```

Hello PHP futásidejű verzióját tooany hello PHP-verziók felsorolt állíthatja be. Például tooset hello PHP verzióját (hello nevű szerepkör `roleName`) too5.4.0, a következő parancs használata hello:

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> A rendelkezésre álló PHP-verziók a jövőbeli hello módosíthatja.
>
>

## <a name="customize-hello-built-in-php-runtime"></a>Hello beépített PHP futtatókörnyezetben testreszabása
Teljes felügyeletet gyakorolhat hello konfigurálása, hogy telepítve van, ha hello lépések felett, beleértve a módosítása hello PHP futtatókörnyezetben rendelkezik `php.ini` beállítások és bővítmények engedélyezésével.

toocustomize hello beépített PHP futtatókörnyezetben, kövesse az alábbi lépéseket:

1. Adja hozzá egy új mappát, `php`, toohello `bin` mappában található a webes szerepkör. A feldolgozói szerepkör esetében adja hozzá toohello szerepkör gyökérkönyvtár.
2. A hello `php` mappa, hozzon létre egy másik nevű `ext`. Helyezzünk `.dll` szerepkörbővítmény-fájlok (pl. `php_mongo.dll`), amelyet az tooenable ebben a mappában.
3. Adja hozzá a `php.ini` toohello fájl `php` mappát. Egyéni kiterjesztések engedélyezése, és állítsa be a PHP-irányelvek a fájlban található. Például, ha tooturn `display_errors` a, és engedélyezze a hello `php_mongo.dll` kiterjesztése, hello tartalmát a `php.ini` fájl a következőképpen nézne ki:

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> A beállításait, explicit módon beállítása nem hello `php.ini` fájlt a rendszer automatikusan megadja állítható tootheir alapértelmezett értékeket. Azonban vegye figyelembe, hogy adhat hozzá egy teljes `php.ini` fájlt.
>
>

## <a name="use-your-own-php-runtime"></a>A saját PHP futtatókörnyezetben használja
Válassza ki a beépített PHP futtatókörnyezetben, majd konfigurálja úgy a fent említett helyett bizonyos esetekben érdemes lehet tooprovide saját PHP futtatókörnyezetben. Használhat például egy webes vagy feldolgozói szerepkörben a fejlesztési környezetet használó azonos PHP futtatókörnyezetben hello. Így könnyebben tooensure hello alkalmazás nem változik meg a viselkedést a termelési környezetben.

### <a name="configure-a-web-role-toouse-your-own-php-runtime"></a>A webes szerepkör toouse saját PHP futtatókörnyezetben konfigurálása
a webes szerepkör toouse ad meg, a PHP futtatókörnyezetben tooconfigure kövesse az alábbi lépéseket:

1. Hozzon létre egy Azure szolgáltatás-projektet, és adja hozzá a PHP webes szerepkör ebben a témakörben korábban ismertetett módon.
2. Hozzon létre egy `php` hello mappájában `bin` mappa, amely a webes szerepkör gyökérkönyvtárában, és adja meg a PHP futásidejű (összes bináris fájljait, konfigurációs fájlokat, almappák stb.) toohello `php` mappa.
3. (VÁLASZTHATÓ) Ha a PHP futtatókörnyezetben használja hello [Microsoft SQL Server PHP Drivers][sqlsrv drivers], szüksége lesz tooconfigure a webes szerepkör tooinstall [SQL Server Native Client 2012] [ sql native client] Ha ki van építve. toodo, vegye fel a hello [sqlncli.msi x64 installer] toohello `bin` a webes szerepkör gyökérkönyvtár mappájában. hello indítási hello következő lépésben leírt eljárást csendes parancsprogramot hello telepítő amikor hello szerepkör lett kiépítve. Ha a PHP futtatókörnyezetben használja a hello Microsoft Drivers php-hez tartozó SQL Server, eltávolíthatja a következő sor a következő lépésben hello látható hello parancsprogramból hello:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Konfigurálja az indítási tevékenységhez meghatározása [Internet Information Services (IIS)] [ iis.net] vonatkozó kérések a PHP futásidejű toohandle toouse `.php` lapokat. toodo a, nyissa meg hello `setup_web.cmd` fájlt (a hello `bin` a webes szerepkör gyökérkönyvtár fájl) egy szövegszerkesztőben, és annak tartalmát a következő parancsfájl hello cseréje:

    ```cmd
    @ECHO ON
    cd "%~dp0"

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    SET PHP_FULL_PATH=%~dp0php\php-cgi.exe
    SET NEW_PATH=%PATH%;%RoleRoot%\base\x86

    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%',maxInstances='12',idleTimeout='60000',activityTimeout='3600',requestTimeout='60000',instanceMaxRequests='10000',protocol='NamedPipe',flushNamedPipe='False']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PATH',value='%NEW_PATH%']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /+"[fullPath='%PHP_FULL_PATH%'].environmentVariables.[name='PHP_FCGI_MAX_REQUESTS',value='10000']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/handlers /+"[name='PHP',path='*.php',verb='GET,HEAD,POST',modules='FastCgiModule',scriptProcessor='%PHP_FULL_PATH%',resourceType='Either',requireAccess='Script']" /commit:apphost
    %WINDIR%\system32\inetsrv\appcmd.exe set config -section:system.webServer/fastCgi /"[fullPath='%PHP_FULL_PATH%'].queueLength:50000"
    ```
5. Adja hozzá az alkalmazás fájlok tooyour webes szerepkör gyökérkönyvtár. Ez lesz hello webkiszolgáló gyökérkönyvtár.
6. Az alkalmazás közzétételére a hello [az alkalmazás közzétételére](#publish-your-application) az alábbi szakasz.

> [!NOTE]
> Hello `download.ps1` parancsfájl (a hello `bin` hello webes szerepkör gyökérkönyvtár mappa) a saját PHP futtatókörnyezetben a fent leírt hello lépések végrehajtását követően törölhető.
>
>

### <a name="configure-a-worker-role-toouse-your-own-php-runtime"></a>A feldolgozói szerepkör toouse saját PHP futtatókörnyezetben konfigurálása
a feldolgozói szerepkör toouse ad meg, a PHP futtatókörnyezetben tooconfigure kövesse az alábbi lépéseket:

1. Hozzon létre egy Azure szolgáltatás-projektet, és adja hozzá a PHP-feldolgozói szerepkör ebben a témakörben korábban ismertetett módon.
2. Hozzon létre egy `php` mappa hello feldolgozói szerepkör gyökérkönyvtárában, majd adja hozzá a PHP futásidejű (összes bináris fájljait, konfigurációs fájlokat, almappák stb.) toohello `php` mappa.
3. (VÁLASZTHATÓ) Ha a PHP futtatókörnyezetben használja [Microsoft SQL Server PHP Drivers][sqlsrv drivers], szüksége lesz tooconfigure a feldolgozói szerepkör tooinstall [SQL Server Native Client 2012] [ sql native client] Ha ki van építve. toodo, vegye fel a hello [sqlncli.msi x64 installer] toohello feldolgozói szerepkör gyökérkönyvtár. hello indítási hello következő lépésben leírt eljárást csendes parancsprogramot hello telepítő amikor hello szerepkör lett kiépítve. Ha a PHP futtatókörnyezetben használja a hello Microsoft Drivers php-hez tartozó SQL Server, eltávolíthatja a következő sor a következő lépésben hello látható hello parancsprogramból hello:

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. Adja meg, amely indítási feladat a `php.exe` végrehajtható toohello feldolgozói szerepkör PATH környezeti változóval hello szerepkör lett kiépítve. toodo a, nyissa meg hello `setup_worker.cmd` fájlt (hello feldolgozói szerepkör gyökérkönyvtár) egy szövegszerkesztőben, és cserélje ki a tartalmát a következő parancsfájl hello:

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service toohello web root directory...
    icacls ..\ /grant "Network Service":(OI)(CI)W
    if %ERRORLEVEL% neq 0 goto error
    echo OK

    if "%EMULATED%"=="true" exit /b 0

    msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES

    setx Path "%PATH%;%~dp0php" /M

    if %ERRORLEVEL% neq 0 goto error

    echo SUCCESS
    exit /b 0

    :error

    echo FAILED
    exit /b -1
    ```
5. Adja hozzá az alkalmazás fájlok tooyour feldolgozói szerepkör gyökérkönyvtár.
6. Az alkalmazás közzétételére a hello [az alkalmazás közzétételére](#publish-your-application) az alábbi szakasz.

## <a name="run-your-application-in-hello-compute-and-storage-emulators"></a>Futtassa az alkalmazást hello számítási és tárolási emulátorok
hello Azure emulátor egy helyi környezetet biztosítson, amelyben tesztelheti az Azure alkalmazás telepítése előtt toohello felhő. Néhány hello emulátorok és hello Azure környezetben közötti különbségek vannak. Ez még jobb, lásd: toounderstand [hello Azure storage Emulatorral a fejlesztéshez és teszteléshez](storage/common/storage-use-emulator.md).

Vegye figyelembe, hogy kell-e a PHP telepítése helyileg toouse hello compute emulator. hello compute emulator a helyi PHP-telepítés toorun fogja használni az alkalmazást.

toorun projektre a hello emulátorok hajtható végre a következő parancsot a projekt gyökérkönyvtárában hello:

    PS C:\MyProject> Start-AzureEmulator

Kimeneti hasonló toothis jelenik meg:

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

Megtekintheti a nyisson meg egy webböngészőben, és toohello helyi cím látható hello kimeneti böngészés hello emulátoron futó alkalmazás (`http://127.0.0.1:81` a fenti hello egy példa a kimenetre).

toostop hello emulátor, a parancs végrehajtásához:

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a>Az alkalmazás közzététele
toopublish az alkalmazás toofirst importálás van szüksége a közzétételi beállítások hello segítségével [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) parancsmag. Ezután az alkalmazást is közzétehet hello segítségével [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) parancsmag. További információ a bejelentkezés: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).

[Azure SDK for PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[szolgáltatás definíciós (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[szolgáltatás konfigurációs (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648

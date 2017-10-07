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
# <a name="how-toocreate-php-web-and-worker-roles"></a><span data-ttu-id="466cb-103">Hogyan toocreate PHP webes és feldolgozói szerepkörök</span><span class="sxs-lookup"><span data-stu-id="466cb-103">How toocreate PHP web and worker roles</span></span>
## <a name="overview"></a><span data-ttu-id="466cb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="466cb-104">Overview</span></span>
<span data-ttu-id="466cb-105">Ez az útmutató bemutatja, hogyan toocreate PHP webes vagy feldolgozói szerepkörök Windows fejlesztői környezetre, a PHP verzióját választhat hello "beépített" verziók, hello PHP konfigurációjának módosítása, bővítmények engedélyezésével és végezetül telepítése tooAzure.</span><span class="sxs-lookup"><span data-stu-id="466cb-105">This guide will show you how toocreate PHP web or worker roles in a Windows development environment, choose a specific version of PHP from hello "built-in" versions available, change hello PHP configuration, enable extensions, and finally, deploy tooAzure.</span></span> <span data-ttu-id="466cb-106">Azt is ismerteti, hogyan tooconfigure egy webes vagy feldolgozói szerepkör toouse egy Ön által biztosított PHP futtatókörnyezetben (az egyéni konfiguráció és bővítmények).</span><span class="sxs-lookup"><span data-stu-id="466cb-106">It also describes how tooconfigure a web or worker role toouse a PHP runtime (with custom configuration and extensions) that you provide.</span></span>

## <a name="what-are-php-web-and-worker-roles"></a><span data-ttu-id="466cb-107">Mik azok a PHP webes és feldolgozói szerepkörök?</span><span class="sxs-lookup"><span data-stu-id="466cb-107">What are PHP web and worker roles?</span></span>
<span data-ttu-id="466cb-108">Azure biztosít három számítási modellt futó alkalmazások: Azure App Service, Azure virtuális gépek és Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="466cb-108">Azure provides three compute models for running applications: Azure App Service, Azure Virtual Machines, and Azure Cloud Services.</span></span> <span data-ttu-id="466cb-109">Mindhárom modell támogatja a PHP.</span><span class="sxs-lookup"><span data-stu-id="466cb-109">All three models support PHP.</span></span> <span data-ttu-id="466cb-110">Felhőszolgáltatás, amely magában foglalja a webes és feldolgozói szerepköröket, itt *platformszolgáltatást (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="466cb-110">Cloud Services, which includes web and worker roles, provides *platform as a service (PaaS)*.</span></span> <span data-ttu-id="466cb-111">A felhőszolgáltatásban a webes szerepkör lehetővé teszi egy dedikált Internet Information Services (IIS) webes server toohost előtér-webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="466cb-111">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server toohost front-end web applications.</span></span> <span data-ttu-id="466cb-112">A feldolgozói szerepkör aszinkron, hosszan futó vagy folyamatos feladatokat futtat függetlenül a felhasználói interakcióktól vagy bemenettől futtathatja.</span><span class="sxs-lookup"><span data-stu-id="466cb-112">A worker role can run asynchronous, long-running or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="466cb-113">Ezekről a beállításokról további információkért lásd: [Azure által biztosított lehetőségek futtató számítási](cloud-services/cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="466cb-113">For more information about these options, see [Compute hosting options provided by Azure](cloud-services/cloud-services-choose-me.md).</span></span>

## <a name="download-hello-azure-sdk-for-php"></a><span data-ttu-id="466cb-114">Php-hez tartozó hello Azure SDK letöltése</span><span class="sxs-lookup"><span data-stu-id="466cb-114">Download hello Azure SDK for PHP</span></span>
<span data-ttu-id="466cb-115">Hello [Azure SDK for PHP] több összetevőből áll.</span><span class="sxs-lookup"><span data-stu-id="466cb-115">hello [Azure SDK for PHP] consists of several components.</span></span> <span data-ttu-id="466cb-116">Ez a cikk kettő fogja használni: Azure PowerShell és az Azure emulátorok hello.</span><span class="sxs-lookup"><span data-stu-id="466cb-116">This article will use two of them: Azure PowerShell and hello Azure emulators.</span></span> <span data-ttu-id="466cb-117">A két összetevő keresztül hello Microsoft Webplatform-telepítővel telepíthető.</span><span class="sxs-lookup"><span data-stu-id="466cb-117">These two components can be installed via hello Microsoft Web Platform Installer.</span></span> <span data-ttu-id="466cb-118">További információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="466cb-118">For more information, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="create-a-cloud-services-project"></a><span data-ttu-id="466cb-119">Egy Felhőszolgáltatás-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="466cb-119">Create a Cloud Services project</span></span>
<span data-ttu-id="466cb-120">a PHP webes vagy feldolgozói szerepkör létrehozásának első lépése hello toocreate Azure szolgáltatás projektben.</span><span class="sxs-lookup"><span data-stu-id="466cb-120">hello first step in creating a PHP web or worker role is toocreate an Azure Service project.</span></span> <span data-ttu-id="466cb-121">az Azure szolgáltatás projekt tárolójaként logikai webes és feldolgozói szerepkörök, és hello projektet tartalmaz [szolgáltatás definíciós (.csdef)] és [szolgáltatás konfigurációs (.cscfg)] fájlokat.</span><span class="sxs-lookup"><span data-stu-id="466cb-121">an Azure Service project serves as a logical container for web and worker roles, and it contains hello project's [service definition (.csdef)] and [service configuration (.cscfg)] files.</span></span>

<span data-ttu-id="466cb-122">toocreate egy új Azure Service-projekt Azure PowerShell futtatása rendszergazdaként, és hajtsa végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-122">toocreate a new Azure Service project, run Azure PowerShell as an administrator, and execute hello following command:</span></span>

    PS C:\>New-AzureServiceProject myProject

<span data-ttu-id="466cb-123">Ez a parancs létrehoz egy új könyvtárat (`myProject`) toowhich webes és feldolgozói szerepkörök is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="466cb-123">This command will create a new directory (`myProject`) toowhich you can add web and worker roles.</span></span>

## <a name="add-php-web-or-worker-roles"></a><span data-ttu-id="466cb-124">A PHP webes vagy feldolgozói szerepkörök hozzáadása</span><span class="sxs-lookup"><span data-stu-id="466cb-124">Add PHP web or worker roles</span></span>
<span data-ttu-id="466cb-125">a PHP webes szerepkör tooa projekt tooadd futtassa a következő parancsot a hello projekt gyökérkönyvtárában hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-125">tooadd a PHP web role tooa project, run hello following command from within hello project's root directory:</span></span>

    PS C:\myProject> Add-AzurePHPWebRole roleName

<span data-ttu-id="466cb-126">A feldolgozói szerepkör esetében az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="466cb-126">For a worker role, use this command:</span></span>

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> <span data-ttu-id="466cb-127">Hello `roleName` paraméter nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="466cb-127">hello `roleName` parameter is optional.</span></span> <span data-ttu-id="466cb-128">Ha ki van hagyva, hello szerepkörnév automatikusan generált.</span><span class="sxs-lookup"><span data-stu-id="466cb-128">If it is omitted, hello role name will be automatically generated.</span></span> <span data-ttu-id="466cb-129">hello létrehozott első webes szerepkör lesz `WebRole1`, hello második lesz `WebRole2`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="466cb-129">hello first web role created will be `WebRole1`, hello second will be `WebRole2`, and so on.</span></span> <span data-ttu-id="466cb-130">hello létrehozott első feldolgozói szerepkör lesz `WorkerRole1`, hello második lesz `WorkerRole2`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="466cb-130">hello first worker role created will be `WorkerRole1`, hello second will be `WorkerRole2`, and so on.</span></span>
>
>

## <a name="specify-hello-built-in-php-version"></a><span data-ttu-id="466cb-131">Adja meg a hello beépített PHP verzióját</span><span class="sxs-lookup"><span data-stu-id="466cb-131">Specify hello built-in PHP version</span></span>
<span data-ttu-id="466cb-132">A PHP webes vagy feldolgozói szerepkör tooa projekt hozzáadásakor hello projekt konfigurációs fájlok módosítják, hogy a PHP esetén telepítve lesz az alkalmazás összes webes vagy feldolgozói példányt telepítették.</span><span class="sxs-lookup"><span data-stu-id="466cb-132">When you add a PHP web or worker role tooa project, hello project's configuration files are modified so that PHP will be installed on each web or worker instance of your application when it is deployed.</span></span> <span data-ttu-id="466cb-133">Futtassa a következő parancs hello alapértelmezés szerint telepítendő PHP toosee hello verziója:</span><span class="sxs-lookup"><span data-stu-id="466cb-133">toosee hello version of PHP that will be installed by default, run hello following command:</span></span>

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

<span data-ttu-id="466cb-134">hello fenti hello parancs kimenetében alábbihoz hasonlóan fog megjelenni toowhat alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="466cb-134">hello output from hello command above will look similar toowhat is shown below.</span></span> <span data-ttu-id="466cb-135">Ebben a példában hello `IsDefault` jelző értéke túl`true` php 5.3.17, jelezve, hogy hello alapértelmezett PHP verzióját telepítve lesz.</span><span class="sxs-lookup"><span data-stu-id="466cb-135">In this example, hello `IsDefault` flag is set too`true` for PHP 5.3.17, indicating that it will be hello default PHP version installed.</span></span>

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

<span data-ttu-id="466cb-136">Hello PHP futásidejű verzióját tooany hello PHP-verziók felsorolt állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="466cb-136">You can set hello PHP runtime version tooany of hello PHP versions that are listed.</span></span> <span data-ttu-id="466cb-137">Például tooset hello PHP verzióját (hello nevű szerepkör `roleName`) too5.4.0, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-137">For example, tooset hello PHP version (for a role with hello name `roleName`) too5.4.0, use hello following command:</span></span>

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> <span data-ttu-id="466cb-138">A rendelkezésre álló PHP-verziók a jövőbeli hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="466cb-138">Available PHP versions may change in hello future.</span></span>
>
>

## <a name="customize-hello-built-in-php-runtime"></a><span data-ttu-id="466cb-139">Hello beépített PHP futtatókörnyezetben testreszabása</span><span class="sxs-lookup"><span data-stu-id="466cb-139">Customize hello built-in PHP runtime</span></span>
<span data-ttu-id="466cb-140">Teljes felügyeletet gyakorolhat hello konfigurálása, hogy telepítve van, ha hello lépések felett, beleértve a módosítása hello PHP futtatókörnyezetben rendelkezik `php.ini` beállítások és bővítmények engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="466cb-140">You have complete control over hello configuration of hello PHP runtime that is installed when you follow hello steps above, including modification of `php.ini` settings and enabling of extensions.</span></span>

<span data-ttu-id="466cb-141">toocustomize hello beépített PHP futtatókörnyezetben, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="466cb-141">toocustomize hello built-in PHP runtime, follow these steps:</span></span>

1. <span data-ttu-id="466cb-142">Adja hozzá egy új mappát, `php`, toohello `bin` mappában található a webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="466cb-142">Add a new folder, named `php`, toohello `bin` directory of your web role.</span></span> <span data-ttu-id="466cb-143">A feldolgozói szerepkör esetében adja hozzá toohello szerepkör gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="466cb-143">For a worker role, add it toohello role's root directory.</span></span>
2. <span data-ttu-id="466cb-144">A hello `php` mappa, hozzon létre egy másik nevű `ext`.</span><span class="sxs-lookup"><span data-stu-id="466cb-144">In hello `php` folder, create another folder called `ext`.</span></span> <span data-ttu-id="466cb-145">Helyezzünk `.dll` szerepkörbővítmény-fájlok (pl. `php_mongo.dll`), amelyet az tooenable ebben a mappában.</span><span class="sxs-lookup"><span data-stu-id="466cb-145">Put any `.dll` extension files (e.g., `php_mongo.dll`) that you want tooenable in this folder.</span></span>
3. <span data-ttu-id="466cb-146">Adja hozzá a `php.ini` toohello fájl `php` mappát.</span><span class="sxs-lookup"><span data-stu-id="466cb-146">Add a `php.ini` file toohello `php` folder.</span></span> <span data-ttu-id="466cb-147">Egyéni kiterjesztések engedélyezése, és állítsa be a PHP-irányelvek a fájlban található.</span><span class="sxs-lookup"><span data-stu-id="466cb-147">Enable any custom extensions and set any PHP directives in this file.</span></span> <span data-ttu-id="466cb-148">Például, ha tooturn `display_errors` a, és engedélyezze a hello `php_mongo.dll` kiterjesztése, hello tartalmát a `php.ini` fájl a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="466cb-148">For example, if you wanted tooturn `display_errors` on and enable hello `php_mongo.dll` extension, hello contents of your `php.ini` file would be as follows:</span></span>

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> <span data-ttu-id="466cb-149">A beállításait, explicit módon beállítása nem hello `php.ini` fájlt a rendszer automatikusan megadja állítható tootheir alapértelmezett értékeket.</span><span class="sxs-lookup"><span data-stu-id="466cb-149">Any settings that you don't explicitly set in hello `php.ini` file that you provide will automatically be set tootheir default values.</span></span> <span data-ttu-id="466cb-150">Azonban vegye figyelembe, hogy adhat hozzá egy teljes `php.ini` fájlt.</span><span class="sxs-lookup"><span data-stu-id="466cb-150">However, keep in mind that you can add a complete `php.ini` file.</span></span>
>
>

## <a name="use-your-own-php-runtime"></a><span data-ttu-id="466cb-151">A saját PHP futtatókörnyezetben használja</span><span class="sxs-lookup"><span data-stu-id="466cb-151">Use your own PHP runtime</span></span>
<span data-ttu-id="466cb-152">Válassza ki a beépített PHP futtatókörnyezetben, majd konfigurálja úgy a fent említett helyett bizonyos esetekben érdemes lehet tooprovide saját PHP futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="466cb-152">In some cases, instead of selecting a built-in PHP runtime and configuring it as described above, you may want tooprovide your own PHP runtime.</span></span> <span data-ttu-id="466cb-153">Használhat például egy webes vagy feldolgozói szerepkörben a fejlesztési környezetet használó azonos PHP futtatókörnyezetben hello.</span><span class="sxs-lookup"><span data-stu-id="466cb-153">For example, you can use hello same PHP runtime in a web or worker role that you use in your development environment.</span></span> <span data-ttu-id="466cb-154">Így könnyebben tooensure hello alkalmazás nem változik meg a viselkedést a termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="466cb-154">This makes it easier tooensure that hello application will not change behavior in your production environment.</span></span>

### <a name="configure-a-web-role-toouse-your-own-php-runtime"></a><span data-ttu-id="466cb-155">A webes szerepkör toouse saját PHP futtatókörnyezetben konfigurálása</span><span class="sxs-lookup"><span data-stu-id="466cb-155">Configure a web role toouse your own PHP runtime</span></span>
<span data-ttu-id="466cb-156">a webes szerepkör toouse ad meg, a PHP futtatókörnyezetben tooconfigure kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="466cb-156">tooconfigure a web role toouse a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="466cb-157">Hozzon létre egy Azure szolgáltatás-projektet, és adja hozzá a PHP webes szerepkör ebben a témakörben korábban ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="466cb-157">Create an Azure Service project and add a PHP web role as described previously in this topic.</span></span>
2. <span data-ttu-id="466cb-158">Hozzon létre egy `php` hello mappájában `bin` mappa, amely a webes szerepkör gyökérkönyvtárában, és adja meg a PHP futásidejű (összes bináris fájljait, konfigurációs fájlokat, almappák stb.) toohello `php` mappa.</span><span class="sxs-lookup"><span data-stu-id="466cb-158">Create a `php` folder in hello `bin` folder that is in your web role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) toohello `php` folder.</span></span>
3. <span data-ttu-id="466cb-159">(VÁLASZTHATÓ) Ha a PHP futtatókörnyezetben használja hello [Microsoft SQL Server PHP Drivers][sqlsrv drivers], szüksége lesz tooconfigure a webes szerepkör tooinstall [SQL Server Native Client 2012] [ sql native client] Ha ki van építve.</span><span class="sxs-lookup"><span data-stu-id="466cb-159">(OPTIONAL) If your PHP runtime uses hello [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need tooconfigure your web role tooinstall [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="466cb-160">toodo, vegye fel a hello [sqlncli.msi x64 installer] toohello `bin` a webes szerepkör gyökérkönyvtár mappájában.</span><span class="sxs-lookup"><span data-stu-id="466cb-160">toodo this, add hello [sqlncli.msi x64 installer] toohello `bin` folder in your web role's root directory.</span></span> <span data-ttu-id="466cb-161">hello indítási hello következő lépésben leírt eljárást csendes parancsprogramot hello telepítő amikor hello szerepkör lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="466cb-161">hello startup script described in hello next step will silently run hello installer when hello role is provisioned.</span></span> <span data-ttu-id="466cb-162">Ha a PHP futtatókörnyezetben használja a hello Microsoft Drivers php-hez tartozó SQL Server, eltávolíthatja a következő sor a következő lépésben hello látható hello parancsprogramból hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-162">If your PHP runtime does not use hello Microsoft Drivers for PHP for SQL Server, you can remove hello following line from hello script shown in hello next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="466cb-163">Konfigurálja az indítási tevékenységhez meghatározása [Internet Information Services (IIS)] [ iis.net] vonatkozó kérések a PHP futásidejű toohandle toouse `.php` lapokat.</span><span class="sxs-lookup"><span data-stu-id="466cb-163">Define a startup task that configures [Internet Information Services (IIS)][iis.net] toouse your PHP runtime toohandle requests for `.php` pages.</span></span> <span data-ttu-id="466cb-164">toodo a, nyissa meg hello `setup_web.cmd` fájlt (a hello `bin` a webes szerepkör gyökérkönyvtár fájl) egy szövegszerkesztőben, és annak tartalmát a következő parancsfájl hello cseréje:</span><span class="sxs-lookup"><span data-stu-id="466cb-164">toodo this, open hello `setup_web.cmd` file (in hello `bin` file of your web role's root directory) in a text editor and replace its contents with hello following script:</span></span>

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
5. <span data-ttu-id="466cb-165">Adja hozzá az alkalmazás fájlok tooyour webes szerepkör gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="466cb-165">Add your application files tooyour web role's root directory.</span></span> <span data-ttu-id="466cb-166">Ez lesz hello webkiszolgáló gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="466cb-166">This will be hello web server's root directory.</span></span>
6. <span data-ttu-id="466cb-167">Az alkalmazás közzétételére a hello [az alkalmazás közzétételére](#publish-your-application) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="466cb-167">Publish your application as described in hello [Publish your application](#publish-your-application) section below.</span></span>

> [!NOTE]
> <span data-ttu-id="466cb-168">Hello `download.ps1` parancsfájl (a hello `bin` hello webes szerepkör gyökérkönyvtár mappa) a saját PHP futtatókörnyezetben a fent leírt hello lépések végrehajtását követően törölhető.</span><span class="sxs-lookup"><span data-stu-id="466cb-168">hello `download.ps1` script (in hello `bin` folder of hello web role's root directory) can be deleted after you follow hello steps described above for using your own PHP runtime.</span></span>
>
>

### <a name="configure-a-worker-role-toouse-your-own-php-runtime"></a><span data-ttu-id="466cb-169">A feldolgozói szerepkör toouse saját PHP futtatókörnyezetben konfigurálása</span><span class="sxs-lookup"><span data-stu-id="466cb-169">Configure a worker role toouse your own PHP runtime</span></span>
<span data-ttu-id="466cb-170">a feldolgozói szerepkör toouse ad meg, a PHP futtatókörnyezetben tooconfigure kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="466cb-170">tooconfigure a worker role toouse a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="466cb-171">Hozzon létre egy Azure szolgáltatás-projektet, és adja hozzá a PHP-feldolgozói szerepkör ebben a témakörben korábban ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="466cb-171">Create an Azure Service project and add a PHP worker role as described previously in this topic.</span></span>
2. <span data-ttu-id="466cb-172">Hozzon létre egy `php` mappa hello feldolgozói szerepkör gyökérkönyvtárában, majd adja hozzá a PHP futásidejű (összes bináris fájljait, konfigurációs fájlokat, almappák stb.) toohello `php` mappa.</span><span class="sxs-lookup"><span data-stu-id="466cb-172">Create a `php` folder in hello worker role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) toohello `php` folder.</span></span>
3. <span data-ttu-id="466cb-173">(VÁLASZTHATÓ) Ha a PHP futtatókörnyezetben használja [Microsoft SQL Server PHP Drivers][sqlsrv drivers], szüksége lesz tooconfigure a feldolgozói szerepkör tooinstall [SQL Server Native Client 2012] [ sql native client] Ha ki van építve.</span><span class="sxs-lookup"><span data-stu-id="466cb-173">(OPTIONAL) If your PHP runtime uses [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need tooconfigure your worker role tooinstall [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="466cb-174">toodo, vegye fel a hello [sqlncli.msi x64 installer] toohello feldolgozói szerepkör gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="466cb-174">toodo this, add hello [sqlncli.msi x64 installer] toohello worker role's root directory.</span></span> <span data-ttu-id="466cb-175">hello indítási hello következő lépésben leírt eljárást csendes parancsprogramot hello telepítő amikor hello szerepkör lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="466cb-175">hello startup script described in hello next step will silently run hello installer when hello role is provisioned.</span></span> <span data-ttu-id="466cb-176">Ha a PHP futtatókörnyezetben használja a hello Microsoft Drivers php-hez tartozó SQL Server, eltávolíthatja a következő sor a következő lépésben hello látható hello parancsprogramból hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-176">If your PHP runtime does not use hello Microsoft Drivers for PHP for SQL Server, you can remove hello following line from hello script shown in hello next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="466cb-177">Adja meg, amely indítási feladat a `php.exe` végrehajtható toohello feldolgozói szerepkör PATH környezeti változóval hello szerepkör lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="466cb-177">Define a startup task that adds your `php.exe` executable toohello worker role's PATH environment variable when hello role is provisioned.</span></span> <span data-ttu-id="466cb-178">toodo a, nyissa meg hello `setup_worker.cmd` fájlt (hello feldolgozói szerepkör gyökérkönyvtár) egy szövegszerkesztőben, és cserélje ki a tartalmát a következő parancsfájl hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-178">toodo this, open hello `setup_worker.cmd` file (in hello worker role's root directory) in a text editor and replace its contents with hello following script:</span></span>

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
5. <span data-ttu-id="466cb-179">Adja hozzá az alkalmazás fájlok tooyour feldolgozói szerepkör gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="466cb-179">Add your application files tooyour worker role's root directory.</span></span>
6. <span data-ttu-id="466cb-180">Az alkalmazás közzétételére a hello [az alkalmazás közzétételére](#publish-your-application) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="466cb-180">Publish your application as described in hello [Publish your application](#publish-your-application) section below.</span></span>

## <a name="run-your-application-in-hello-compute-and-storage-emulators"></a><span data-ttu-id="466cb-181">Futtassa az alkalmazást hello számítási és tárolási emulátorok</span><span class="sxs-lookup"><span data-stu-id="466cb-181">Run your application in hello compute and storage emulators</span></span>
<span data-ttu-id="466cb-182">hello Azure emulátor egy helyi környezetet biztosítson, amelyben tesztelheti az Azure alkalmazás telepítése előtt toohello felhő.</span><span class="sxs-lookup"><span data-stu-id="466cb-182">hello Azure emulators provide a local environment in which you can test your Azure application before you deploy it toohello cloud.</span></span> <span data-ttu-id="466cb-183">Néhány hello emulátorok és hello Azure környezetben közötti különbségek vannak.</span><span class="sxs-lookup"><span data-stu-id="466cb-183">There are some differences between hello emulators and hello Azure environment.</span></span> <span data-ttu-id="466cb-184">Ez még jobb, lásd: toounderstand [hello Azure storage Emulatorral a fejlesztéshez és teszteléshez](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="466cb-184">toounderstand this better, see [Use hello Azure storage emulator for development and testing](storage/common/storage-use-emulator.md).</span></span>

<span data-ttu-id="466cb-185">Vegye figyelembe, hogy kell-e a PHP telepítése helyileg toouse hello compute emulator.</span><span class="sxs-lookup"><span data-stu-id="466cb-185">Note that you must have PHP installed locally toouse hello compute emulator.</span></span> <span data-ttu-id="466cb-186">hello compute emulator a helyi PHP-telepítés toorun fogja használni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="466cb-186">hello compute emulator will use your local PHP installation toorun your application.</span></span>

<span data-ttu-id="466cb-187">toorun projektre a hello emulátorok hajtható végre a következő parancsot a projekt gyökérkönyvtárában hello:</span><span class="sxs-lookup"><span data-stu-id="466cb-187">toorun your project in hello emulators, execute hello following command from your project's root directory:</span></span>

    PS C:\MyProject> Start-AzureEmulator

<span data-ttu-id="466cb-188">Kimeneti hasonló toothis jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="466cb-188">You will see output similar toothis:</span></span>

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

<span data-ttu-id="466cb-189">Megtekintheti a nyisson meg egy webböngészőben, és toohello helyi cím látható hello kimeneti böngészés hello emulátoron futó alkalmazás (`http://127.0.0.1:81` a fenti hello egy példa a kimenetre).</span><span class="sxs-lookup"><span data-stu-id="466cb-189">You can see your application running in hello emulator by opening a web browser and browsing toohello local address shown in hello output (`http://127.0.0.1:81` in hello example output above).</span></span>

<span data-ttu-id="466cb-190">toostop hello emulátor, a parancs végrehajtásához:</span><span class="sxs-lookup"><span data-stu-id="466cb-190">toostop hello emulators, execute this command:</span></span>

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a><span data-ttu-id="466cb-191">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="466cb-191">Publish your application</span></span>
<span data-ttu-id="466cb-192">toopublish az alkalmazás toofirst importálás van szüksége a közzétételi beállítások hello segítségével [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="466cb-192">toopublish your application, you need toofirst import your publish settings by using hello [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet.</span></span> <span data-ttu-id="466cb-193">Ezután az alkalmazást is közzétehet hello segítségével [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="466cb-193">Then you can publish your application by using hello [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet.</span></span> <span data-ttu-id="466cb-194">További információ a bejelentkezés: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="466cb-194">For information about signing in, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="next-steps"></a><span data-ttu-id="466cb-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="466cb-195">Next steps</span></span>
<span data-ttu-id="466cb-196">További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="466cb-196">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

[Azure SDK for PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[szolgáltatás definíciós (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[szolgáltatás konfigurációs (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648

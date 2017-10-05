---
title: "Php-hez tartozó Azure webes és feldolgozói szerepkörök létrehozása |} Microsoft Docs"
description: "Egy útmutató, a PHP webes és feldolgozói szerepkörök létrehozása az Azure-felhőszolgáltatás, és a PHP futtatókörnyezetben konfigurálása."
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
ms.openlocfilehash: 214fdcfe20f3fa4ebcbe41308404f8b7e7d15310
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-create-php-web-and-worker-roles"></a><span data-ttu-id="fa134-103">A PHP webes és feldolgozói szerepkörök létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa134-103">How to create PHP web and worker roles</span></span>
## <a name="overview"></a><span data-ttu-id="fa134-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="fa134-104">Overview</span></span>
<span data-ttu-id="fa134-105">Ez az útmutató bemutatja, hogyan PHP webes vagy feldolgozói szerepkörök létrehozása a Windows fejlesztői környezetben, a "beépített" verziók lehetőségek közül választhat a PHP verzióját, a PHP-konfigurációt, bővítmények engedélyezése, és végül telepítse az Azure.</span><span class="sxs-lookup"><span data-stu-id="fa134-105">This guide will show you how to create PHP web or worker roles in a Windows development environment, choose a specific version of PHP from the "built-in" versions available, change the PHP configuration, enable extensions, and finally, deploy to Azure.</span></span> <span data-ttu-id="fa134-106">Emellett bemutatja, hogyan lehet egy webes vagy feldolgozói szerepkör egy Ön által biztosított PHP futtatókörnyezetben (az egyéni konfiguráció és bővítmények) használatára konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="fa134-106">It also describes how to configure a web or worker role to use a PHP runtime (with custom configuration and extensions) that you provide.</span></span>

## <a name="what-are-php-web-and-worker-roles"></a><span data-ttu-id="fa134-107">Mik azok a PHP webes és feldolgozói szerepkörök?</span><span class="sxs-lookup"><span data-stu-id="fa134-107">What are PHP web and worker roles?</span></span>
<span data-ttu-id="fa134-108">Azure biztosít három számítási modellt futó alkalmazások: Azure App Service, Azure virtuális gépek és Azure Cloud Services.</span><span class="sxs-lookup"><span data-stu-id="fa134-108">Azure provides three compute models for running applications: Azure App Service, Azure Virtual Machines, and Azure Cloud Services.</span></span> <span data-ttu-id="fa134-109">Mindhárom modell támogatja a PHP.</span><span class="sxs-lookup"><span data-stu-id="fa134-109">All three models support PHP.</span></span> <span data-ttu-id="fa134-110">Felhőszolgáltatás, amely magában foglalja a webes és feldolgozói szerepköröket, itt *platformszolgáltatást (PaaS)*.</span><span class="sxs-lookup"><span data-stu-id="fa134-110">Cloud Services, which includes web and worker roles, provides *platform as a service (PaaS)*.</span></span> <span data-ttu-id="fa134-111">A felhőszolgáltatásban webes szerepkörrel rendelkező előtér-alkalmazások üzemeltetését egy dedikált Internet Information Services (IIS) webkiszolgálót biztosít a.</span><span class="sxs-lookup"><span data-stu-id="fa134-111">Within a cloud service, a web role provides a dedicated Internet Information Services (IIS) web server to host front-end web applications.</span></span> <span data-ttu-id="fa134-112">A feldolgozói szerepkör aszinkron, hosszan futó vagy folyamatos feladatokat futtat függetlenül a felhasználói interakcióktól vagy bemenettől futtathatja.</span><span class="sxs-lookup"><span data-stu-id="fa134-112">A worker role can run asynchronous, long-running or perpetual tasks independent of user interaction or input.</span></span>

<span data-ttu-id="fa134-113">Ezekről a beállításokról további információkért lásd: [Azure által biztosított lehetőségek futtató számítási](cloud-services/cloud-services-choose-me.md).</span><span class="sxs-lookup"><span data-stu-id="fa134-113">For more information about these options, see [Compute hosting options provided by Azure](cloud-services/cloud-services-choose-me.md).</span></span>

## <a name="download-the-azure-sdk-for-php"></a><span data-ttu-id="fa134-114">A PHP-hoz készült Azure SDK letöltése</span><span class="sxs-lookup"><span data-stu-id="fa134-114">Download the Azure SDK for PHP</span></span>
<span data-ttu-id="fa134-115">A [Azure SDK for PHP] több összetevőből áll.</span><span class="sxs-lookup"><span data-stu-id="fa134-115">The [Azure SDK for PHP] consists of several components.</span></span> <span data-ttu-id="fa134-116">Ez a cikk kettő fogja használni: Azure PowerShell és az Azure emulátorok.</span><span class="sxs-lookup"><span data-stu-id="fa134-116">This article will use two of them: Azure PowerShell and the Azure emulators.</span></span> <span data-ttu-id="fa134-117">A két összetevő keresztül a Microsoft Webplatform-telepítővel telepíthető.</span><span class="sxs-lookup"><span data-stu-id="fa134-117">These two components can be installed via the Microsoft Web Platform Installer.</span></span> <span data-ttu-id="fa134-118">További információt [az Azure PowerShell telepítésével és konfigurálásával](/powershell/azure/overview) foglalkozó témakörben talál.</span><span class="sxs-lookup"><span data-stu-id="fa134-118">For more information, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="create-a-cloud-services-project"></a><span data-ttu-id="fa134-119">Egy Felhőszolgáltatás-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fa134-119">Create a Cloud Services project</span></span>
<span data-ttu-id="fa134-120">Az első lépés a PHP webes vagy feldolgozói szerepkör létrehozása az Azure Service-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fa134-120">The first step in creating a PHP web or worker role is to create an Azure Service project.</span></span> <span data-ttu-id="fa134-121">az Azure szolgáltatás projekt tárolójaként logikai webes és feldolgozói szerepkörök, és a projekt tartalmaz [szolgáltatás definíciós (.csdef)] és [szolgáltatás konfigurációs (.cscfg)] fájlokat.</span><span class="sxs-lookup"><span data-stu-id="fa134-121">an Azure Service project serves as a logical container for web and worker roles, and it contains the project's [service definition (.csdef)] and [service configuration (.cscfg)] files.</span></span>

<span data-ttu-id="fa134-122">Hozzon létre egy új Azure szolgáltatás-projektet, Azure PowerShell futtatása rendszergazdaként, és hajtsa végre a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fa134-122">To create a new Azure Service project, run Azure PowerShell as an administrator, and execute the following command:</span></span>

    PS C:\>New-AzureServiceProject myProject

<span data-ttu-id="fa134-123">Ez a parancs létrehoz egy új könyvtárat (`myProject`), amelyhez webes és feldolgozói szerepkörök adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="fa134-123">This command will create a new directory (`myProject`) to which you can add web and worker roles.</span></span>

## <a name="add-php-web-or-worker-roles"></a><span data-ttu-id="fa134-124">A PHP webes vagy feldolgozói szerepkörök hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fa134-124">Add PHP web or worker roles</span></span>
<span data-ttu-id="fa134-125">A PHP webes szerepkör hozzáadása a projekthez, futtassa a következő parancsot a projekt gyökérkönyvtárában:</span><span class="sxs-lookup"><span data-stu-id="fa134-125">To add a PHP web role to a project, run the following command from within the project's root directory:</span></span>

    PS C:\myProject> Add-AzurePHPWebRole roleName

<span data-ttu-id="fa134-126">A feldolgozói szerepkör esetében az alábbi parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="fa134-126">For a worker role, use this command:</span></span>

    PS C:\myProject> Add-AzurePHPWorkerRole roleName

> [!NOTE]
> <span data-ttu-id="fa134-127">A `roleName` paraméter nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="fa134-127">The `roleName` parameter is optional.</span></span> <span data-ttu-id="fa134-128">Ha ki van hagyva, a szerepkör neve automatikusan generált.</span><span class="sxs-lookup"><span data-stu-id="fa134-128">If it is omitted, the role name will be automatically generated.</span></span> <span data-ttu-id="fa134-129">Az első létrehozott webes szerepkör lesz `WebRole1`, a második lesz `WebRole2`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="fa134-129">The first web role created will be `WebRole1`, the second will be `WebRole2`, and so on.</span></span> <span data-ttu-id="fa134-130">A létrehozott első feldolgozói szerepkör lesz `WorkerRole1`, a második lesz `WorkerRole2`, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="fa134-130">The first worker role created will be `WorkerRole1`, the second will be `WorkerRole2`, and so on.</span></span>
>
>

## <a name="specify-the-built-in-php-version"></a><span data-ttu-id="fa134-131">Adja meg a beépített PHP verzióját</span><span class="sxs-lookup"><span data-stu-id="fa134-131">Specify the built-in PHP version</span></span>
<span data-ttu-id="fa134-132">A PHP webes vagy feldolgozói szerepkör projekt hozzáadásakor a projekt konfigurációs fájlok módosítják, hogy a PHP esetén telepítve lesz az alkalmazás összes webes vagy feldolgozói példányt telepítették.</span><span class="sxs-lookup"><span data-stu-id="fa134-132">When you add a PHP web or worker role to a project, the project's configuration files are modified so that PHP will be installed on each web or worker instance of your application when it is deployed.</span></span> <span data-ttu-id="fa134-133">A verzió a PHP alapértelmezés szerint telepítendő megtekintéséhez futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fa134-133">To see the version of PHP that will be installed by default, run the following command:</span></span>

    PS C:\myProject> Get-AzureServiceProjectRoleRuntime

<span data-ttu-id="fa134-134">A fenti parancs kimenetét fogja hasonlítania alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="fa134-134">The output from the command above will look similar to what is shown below.</span></span> <span data-ttu-id="fa134-135">Ebben a példában a `IsDefault` jelző értéke `true` php 5.3.17, jelezve, hogy az alapértelmezett a PHP-verzió telepítve lesz.</span><span class="sxs-lookup"><span data-stu-id="fa134-135">In this example, the `IsDefault` flag is set to `true` for PHP 5.3.17, indicating that it will be the default PHP version installed.</span></span>

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

<span data-ttu-id="fa134-136">A PHP verzióinak felsorolt állíthat be a PHP futásidejű verzióját.</span><span class="sxs-lookup"><span data-stu-id="fa134-136">You can set the PHP runtime version to any of the PHP versions that are listed.</span></span> <span data-ttu-id="fa134-137">Ahhoz például, hogy állítsa be a PHP verzióját (nevű szerepkör `roleName`) 5.4.0, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fa134-137">For example, to set the PHP version (for a role with the name `roleName`) to 5.4.0, use the following command:</span></span>

    PS C:\myProject> Set-AzureServiceProjectRole roleName php 5.4.0

> [!NOTE]
> <span data-ttu-id="fa134-138">A rendelkezésre álló PHP-verziók a jövőben módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="fa134-138">Available PHP versions may change in the future.</span></span>
>
>

## <a name="customize-the-built-in-php-runtime"></a><span data-ttu-id="fa134-139">A beépített PHP futtatókörnyezetben testreszabása</span><span class="sxs-lookup"><span data-stu-id="fa134-139">Customize the built-in PHP runtime</span></span>
<span data-ttu-id="fa134-140">A PHP futtatókörnyezetben, ha követi a fenti lépéseket, beleértve a módosítása telepített konfigurációjának teljes hozzáférésük van `php.ini` beállítások és bővítmények engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="fa134-140">You have complete control over the configuration of the PHP runtime that is installed when you follow the steps above, including modification of `php.ini` settings and enabling of extensions.</span></span>

<span data-ttu-id="fa134-141">A beépített PHP futtatókörnyezetben testreszabásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fa134-141">To customize the built-in PHP runtime, follow these steps:</span></span>

1. <span data-ttu-id="fa134-142">Adja hozzá egy új mappát, `php`, hogy a `bin` mappában található a webes szerepkör.</span><span class="sxs-lookup"><span data-stu-id="fa134-142">Add a new folder, named `php`, to the `bin` directory of your web role.</span></span> <span data-ttu-id="fa134-143">A feldolgozói szerepkör esetében adja hozzá a szerepkört gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="fa134-143">For a worker role, add it to the role's root directory.</span></span>
2. <span data-ttu-id="fa134-144">Az a `php` mappa, hozzon létre egy másik nevű `ext`.</span><span class="sxs-lookup"><span data-stu-id="fa134-144">In the `php` folder, create another folder called `ext`.</span></span> <span data-ttu-id="fa134-145">Helyezzünk `.dll` szerepkörbővítmény-fájlok (pl. `php_mongo.dll`), amely engedélyezi a mappában.</span><span class="sxs-lookup"><span data-stu-id="fa134-145">Put any `.dll` extension files (e.g., `php_mongo.dll`) that you want to enable in this folder.</span></span>
3. <span data-ttu-id="fa134-146">Adja hozzá a `php.ini` fájlt a `php` mappát.</span><span class="sxs-lookup"><span data-stu-id="fa134-146">Add a `php.ini` file to the `php` folder.</span></span> <span data-ttu-id="fa134-147">Egyéni kiterjesztések engedélyezése, és állítsa be a PHP-irányelvek a fájlban található.</span><span class="sxs-lookup"><span data-stu-id="fa134-147">Enable any custom extensions and set any PHP directives in this file.</span></span> <span data-ttu-id="fa134-148">Például, ha szeretné kapcsolni `display_errors` a, és engedélyezze a `php_mongo.dll` kiterjesztése, a tartalmát a `php.ini` fájl a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="fa134-148">For example, if you wanted to turn `display_errors` on and enable the `php_mongo.dll` extension, the contents of your `php.ini` file would be as follows:</span></span>

        display_errors=On
        extension=php_mongo.dll

> [!NOTE]
> <span data-ttu-id="fa134-149">A beállításait, ne explicit módon beállítása a `php.ini` fájlt a rendszer automatikusan megadja az alapértelmezett értékre állítható be.</span><span class="sxs-lookup"><span data-stu-id="fa134-149">Any settings that you don't explicitly set in the `php.ini` file that you provide will automatically be set to their default values.</span></span> <span data-ttu-id="fa134-150">Azonban vegye figyelembe, hogy adhat hozzá egy teljes `php.ini` fájlt.</span><span class="sxs-lookup"><span data-stu-id="fa134-150">However, keep in mind that you can add a complete `php.ini` file.</span></span>
>
>

## <a name="use-your-own-php-runtime"></a><span data-ttu-id="fa134-151">A saját PHP futtatókörnyezetben használja</span><span class="sxs-lookup"><span data-stu-id="fa134-151">Use your own PHP runtime</span></span>
<span data-ttu-id="fa134-152">Válassza ki a beépített PHP futtatókörnyezetben, majd konfigurálja úgy a fent említett helyett bizonyos esetekben érdemes lehet adja meg a saját PHP futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fa134-152">In some cases, instead of selecting a built-in PHP runtime and configuring it as described above, you may want to provide your own PHP runtime.</span></span> <span data-ttu-id="fa134-153">Például a fejlesztői környezetben használt webes vagy feldolgozói szerepkörnek ugyanaz a PHP futtatókörnyezetben is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fa134-153">For example, you can use the same PHP runtime in a web or worker role that you use in your development environment.</span></span> <span data-ttu-id="fa134-154">Így könnyebben győződjön meg arról, hogy az alkalmazás nem módosítja az éles környezetben viselkedését.</span><span class="sxs-lookup"><span data-stu-id="fa134-154">This makes it easier to ensure that the application will not change behavior in your production environment.</span></span>

### <a name="configure-a-web-role-to-use-your-own-php-runtime"></a><span data-ttu-id="fa134-155">A saját PHP futtatókörnyezetben használja a webes szerepkör konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa134-155">Configure a web role to use your own PHP runtime</span></span>
<span data-ttu-id="fa134-156">A webes szerepkör egy Ön által biztosított PHP futtatókörnyezetben használandó konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fa134-156">To configure a web role to use a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="fa134-157">Hozzon létre egy Azure szolgáltatás-projektet, és adja hozzá a PHP webes szerepkör ebben a témakörben korábban ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="fa134-157">Create an Azure Service project and add a PHP web role as described previously in this topic.</span></span>
2. <span data-ttu-id="fa134-158">Hozzon létre egy `php` mappájában a `bin` mappa, amely a webes szerepkör gyökérkönyvtárában, és hozzáadhatja a PHP futtatókörnyezetben (összes bináris fájljait, konfigurációs fájlokat, almappák stb.) a `php` mappát.</span><span class="sxs-lookup"><span data-stu-id="fa134-158">Create a `php` folder in the `bin` folder that is in your web role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) to the `php` folder.</span></span>
3. <span data-ttu-id="fa134-159">(VÁLASZTHATÓ) Ha a PHP futtatókörnyezetben használja a [Microsoft SQL Server PHP Drivers][sqlsrv drivers], szüksége lesz a webes szerepkör telepítéséhez konfigurálása [SQL Server Native Client 2012] [ sql native client] Ha ki van építve.</span><span class="sxs-lookup"><span data-stu-id="fa134-159">(OPTIONAL) If your PHP runtime uses the [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need to configure your web role to install [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="fa134-160">Ehhez adja hozzá a [sqlncli.msi x64 installer] számára a `bin` a webes szerepkör gyökérkönyvtár mappájában.</span><span class="sxs-lookup"><span data-stu-id="fa134-160">To do this, add the [sqlncli.msi x64 installer] to the `bin` folder in your web role's root directory.</span></span> <span data-ttu-id="fa134-161">A következő lépésben ismertetett indítási parancsfájl csendes fog futtassa a telepítőt, amikor a szerepkör lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="fa134-161">The startup script described in the next step will silently run the installer when the role is provisioned.</span></span> <span data-ttu-id="fa134-162">Ha a PHP futtatókörnyezetben nem használja a Microsoft Drivers php-hez tartozó SQL Server, a parancsfájl a következő lépésben látható eltávolíthat a következő sort:</span><span class="sxs-lookup"><span data-stu-id="fa134-162">If your PHP runtime does not use the Microsoft Drivers for PHP for SQL Server, you can remove the following line from the script shown in the next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="fa134-163">Konfigurálja az indítási tevékenységhez meghatározása [Internet Information Services (IIS)] [ iis.net] a PHP futtatókörnyezetben használatára a tanúsítványigénylések `.php` lapokat.</span><span class="sxs-lookup"><span data-stu-id="fa134-163">Define a startup task that configures [Internet Information Services (IIS)][iis.net] to use your PHP runtime to handle requests for `.php` pages.</span></span> <span data-ttu-id="fa134-164">Ehhez nyissa meg a `setup_web.cmd` fájlt (a a `bin` a webes szerepkör gyökérkönyvtár fájl) egy szövegszerkesztőben, és cserélje ki annak tartalmát a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="fa134-164">To do this, open the `setup_web.cmd` file (in the `bin` file of your web role's root directory) in a text editor and replace its contents with the following script:</span></span>

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
5. <span data-ttu-id="fa134-165">Adja hozzá az alkalmazásfájlokat a webes szerepkör gyökérkönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="fa134-165">Add your application files to your web role's root directory.</span></span> <span data-ttu-id="fa134-166">Ez lesz a webkiszolgálón gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="fa134-166">This will be the web server's root directory.</span></span>
6. <span data-ttu-id="fa134-167">Az alkalmazás közzétételére, lásd: a [az alkalmazás közzétételére](#publish-your-application) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="fa134-167">Publish your application as described in the [Publish your application](#publish-your-application) section below.</span></span>

> [!NOTE]
> <span data-ttu-id="fa134-168">A `download.ps1` parancsfájl (a a `bin` mappában található a webes szerepkör gyökérkönyvtár) a saját PHP futtatókörnyezetben használja a fenti lépések végrehajtását követően törölhető.</span><span class="sxs-lookup"><span data-stu-id="fa134-168">The `download.ps1` script (in the `bin` folder of the web role's root directory) can be deleted after you follow the steps described above for using your own PHP runtime.</span></span>
>
>

### <a name="configure-a-worker-role-to-use-your-own-php-runtime"></a><span data-ttu-id="fa134-169">A saját PHP futtatókörnyezetben használandó feldolgozói szerepkörök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fa134-169">Configure a worker role to use your own PHP runtime</span></span>
<span data-ttu-id="fa134-170">A feldolgozói szerepkör egy Ön által biztosított PHP futtatókörnyezetben használandó konfigurálásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fa134-170">To configure a worker role to use a PHP runtime that you provide, follow these steps:</span></span>

1. <span data-ttu-id="fa134-171">Hozzon létre egy Azure szolgáltatás-projektet, és adja hozzá a PHP-feldolgozói szerepkör ebben a témakörben korábban ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="fa134-171">Create an Azure Service project and add a PHP worker role as described previously in this topic.</span></span>
2. <span data-ttu-id="fa134-172">Hozzon létre egy `php` mappa a feldolgozói szerepkör gyökérkönyvtárában, és adja hozzá a PHP futtatókörnyezetben (összes bináris fájljait, konfigurációs fájlokat, almappák stb.) a `php` mappát.</span><span class="sxs-lookup"><span data-stu-id="fa134-172">Create a `php` folder in the worker role's root directory, and then add your PHP runtime (all binaries, configuration files, subfolders, etc.) to the `php` folder.</span></span>
3. <span data-ttu-id="fa134-173">(VÁLASZTHATÓ) Ha a PHP futtatókörnyezetben használja [Microsoft SQL Server PHP Drivers][sqlsrv drivers], a feldolgozói szerepkör telepítéséhez konfigurálni kell [SQL Server Native Client 2012] [ sql native client] Ha ki van építve.</span><span class="sxs-lookup"><span data-stu-id="fa134-173">(OPTIONAL) If your PHP runtime uses [Microsoft Drivers for PHP for SQL Server][sqlsrv drivers], you will need to configure your worker role to install [SQL Server Native Client 2012][sql native client] when it is provisioned.</span></span> <span data-ttu-id="fa134-174">Ehhez adja hozzá a [sqlncli.msi x64 installer] a feldolgozói szerepkör gyökérkönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="fa134-174">To do this, add the [sqlncli.msi x64 installer] to the worker role's root directory.</span></span> <span data-ttu-id="fa134-175">A következő lépésben ismertetett indítási parancsfájl csendes fog futtassa a telepítőt, amikor a szerepkör lett kiépítve.</span><span class="sxs-lookup"><span data-stu-id="fa134-175">The startup script described in the next step will silently run the installer when the role is provisioned.</span></span> <span data-ttu-id="fa134-176">Ha a PHP futtatókörnyezetben nem használja a Microsoft Drivers php-hez tartozó SQL Server, a parancsfájl a következő lépésben látható eltávolíthat a következő sort:</span><span class="sxs-lookup"><span data-stu-id="fa134-176">If your PHP runtime does not use the Microsoft Drivers for PHP for SQL Server, you can remove the following line from the script shown in the next step:</span></span>

        msiexec /i sqlncli.msi /qn IACCEPTSQLNCLILICENSETERMS=YES
4. <span data-ttu-id="fa134-177">Adja meg, amely indítási feladat a `php.exe` végrehajtható, a szerepkör ki van építve a feldolgozói szerepkör PATH környezeti változóhoz.</span><span class="sxs-lookup"><span data-stu-id="fa134-177">Define a startup task that adds your `php.exe` executable to the worker role's PATH environment variable when the role is provisioned.</span></span> <span data-ttu-id="fa134-178">Ehhez nyissa meg a `setup_worker.cmd` fájlt (a feldolgozói szerepkör gyökérkönyvtár) egy szövegszerkesztőben, és cserélje ki annak tartalmát a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="fa134-178">To do this, open the `setup_worker.cmd` file (in the worker role's root directory) in a text editor and replace its contents with the following script:</span></span>

    ```cmd
    @echo on

    cd "%~dp0"

    echo Granting permissions for Network Service to the web root directory...
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
5. <span data-ttu-id="fa134-179">Adja hozzá az alkalmazásfájlokat a feldolgozói szerepkör gyökérkönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="fa134-179">Add your application files to your worker role's root directory.</span></span>
6. <span data-ttu-id="fa134-180">Az alkalmazás közzétételére, lásd: a [az alkalmazás közzétételére](#publish-your-application) az alábbi szakasz.</span><span class="sxs-lookup"><span data-stu-id="fa134-180">Publish your application as described in the [Publish your application](#publish-your-application) section below.</span></span>

## <a name="run-your-application-in-the-compute-and-storage-emulators"></a><span data-ttu-id="fa134-181">Futtassa az alkalmazást a számítási és tárolási emulátorok</span><span class="sxs-lookup"><span data-stu-id="fa134-181">Run your application in the compute and storage emulators</span></span>
<span data-ttu-id="fa134-182">Az Azure emulátorok adjon meg egy helyi környezet, amelyben tesztelheti az Azure-alkalmazásában a felhőbe való telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="fa134-182">The Azure emulators provide a local environment in which you can test your Azure application before you deploy it to the cloud.</span></span> <span data-ttu-id="fa134-183">Néhány az emulátorok és az Azure környezetbe közötti különbségek vannak.</span><span class="sxs-lookup"><span data-stu-id="fa134-183">There are some differences between the emulators and the Azure environment.</span></span> <span data-ttu-id="fa134-184">Ez jobb megértése, lásd: [fejlesztéshez és teszteléshez használja az Azure storage emulator](storage/common/storage-use-emulator.md).</span><span class="sxs-lookup"><span data-stu-id="fa134-184">To understand this better, see [Use the Azure storage emulator for development and testing](storage/common/storage-use-emulator.md).</span></span>

<span data-ttu-id="fa134-185">Vegye figyelembe, hogy a PHP telepítése helyi a compute emulator használatával kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="fa134-185">Note that you must have PHP installed locally to use the compute emulator.</span></span> <span data-ttu-id="fa134-186">A compute emulator a helyi PHP-telepítés használatával futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fa134-186">The compute emulator will use your local PHP installation to run your application.</span></span>

<span data-ttu-id="fa134-187">A projekt futtatásához az emulátorok, a projekt gyökérkönyvtárából végre az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="fa134-187">To run your project in the emulators, execute the following command from your project's root directory:</span></span>

    PS C:\MyProject> Start-AzureEmulator

<span data-ttu-id="fa134-188">Ez hasonló kimenetet fog látni:</span><span class="sxs-lookup"><span data-stu-id="fa134-188">You will see output similar to this:</span></span>

    Creating local package...
    Starting Emulator...
    Role is running at http://127.0.0.1:81
    Started

<span data-ttu-id="fa134-189">Láthatja, hogy az alkalmazás futtatása az emulátorban nyisson meg egy webböngészőben, és a helyi cím, a kimenetben megjelenő tallózással (`http://127.0.0.1:81` a fenti példa kimenetben).</span><span class="sxs-lookup"><span data-stu-id="fa134-189">You can see your application running in the emulator by opening a web browser and browsing to the local address shown in the output (`http://127.0.0.1:81` in the example output above).</span></span>

<span data-ttu-id="fa134-190">Az emulátorok leállításához hajtsa végre a parancsot:</span><span class="sxs-lookup"><span data-stu-id="fa134-190">To stop the emulators, execute this command:</span></span>

    PS C:\MyProject> Stop-AzureEmulator

## <a name="publish-your-application"></a><span data-ttu-id="fa134-191">Az alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="fa134-191">Publish your application</span></span>
<span data-ttu-id="fa134-192">Az alkalmazás közzétételére, előbb importálnia kell a közzétételi beállítások használatával a [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fa134-192">To publish your application, you need to first import your publish settings by using the [Import-AzurePublishSettingsFile](https://msdn.microsoft.com/library/azure/dn790370.aspx) cmdlet.</span></span> <span data-ttu-id="fa134-193">Az alkalmazás használatával közzétehet, majd a [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="fa134-193">Then you can publish your application by using the [Publish-AzureServiceProject](https://msdn.microsoft.com/library/azure/dn495166.aspx) cmdlet.</span></span> <span data-ttu-id="fa134-194">További információ a bejelentkezés: [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fa134-194">For information about signing in, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa134-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fa134-195">Next steps</span></span>
<span data-ttu-id="fa134-196">További információkért lásd: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="fa134-196">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

[Azure SDK for PHP]: /develop/php/common-tasks/download-php-sdk/
[install ps and emulators]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[szolgáltatás definíciós (.csdef)]: http://msdn.microsoft.com/library/windowsazure/ee758711.aspx
[szolgáltatás konfigurációs (.cscfg)]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
[iis.net]: http://www.iis.net/
[sql native client]: http://msdn.microsoft.com/sqlserver/aa937733.aspx
[sqlsrv drivers]: http://php.net/sqlsrv
[sqlncli.msi x64 installer]: http://go.microsoft.com/fwlink/?LinkID=239648

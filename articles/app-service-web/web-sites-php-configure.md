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
# <a name="configure-php-in-azure-app-service-web-apps"></a><span data-ttu-id="36bc8-103">Az Azure App Service Web Apps PHP-konfigurálás</span><span class="sxs-lookup"><span data-stu-id="36bc8-103">Configure PHP in Azure App Service Web Apps</span></span>
## <a name="introduction"></a><span data-ttu-id="36bc8-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="36bc8-104">Introduction</span></span>
<span data-ttu-id="36bc8-105">Ez az útmutató bemutatja, hogyan tooconfigure hello Web Apps a beépített PHP futtatókörnyezetben [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), adjon meg egy egyéni PHP futtatókörnyezetben és bővítmények engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="36bc8-105">This guide will show you how tooconfigure hello built-in PHP runtime for Web Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), provide a custom PHP runtime, and enable extensions.</span></span> <span data-ttu-id="36bc8-106">App Service-ben toouse regisztráljon hello [ingyenes próbaverzió].</span><span class="sxs-lookup"><span data-stu-id="36bc8-106">toouse App Service, sign up for hello [free trial].</span></span> <span data-ttu-id="36bc8-107">tooget hello leginkább az ebben az útmutatóban, először létre kell hoznia egy PHP webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="36bc8-107">tooget hello most from this guide, you should first create a PHP web app in App Service.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-hello-built-in-php-version"></a><span data-ttu-id="36bc8-108">Útmutató: a PHP verzió beépített módosítása hello</span><span class="sxs-lookup"><span data-stu-id="36bc8-108">How to: Change hello built-in PHP version</span></span>
<span data-ttu-id="36bc8-109">Alapértelmezés szerint a PHP 5.5 telepítve és használhatja az App Service webalkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="36bc8-109">By default, PHP 5.5 is installed and immediately available for use when you create an App Service web app.</span></span> <span data-ttu-id="36bc8-110">hello legjobb módja toosee hello rendelkezésre álló kiadási változat, alapértelmezett konfigurációval és hello engedélyezett bővítmények között olyan parancsfájlt, amely meghívja a hello toodeploy [phpinfo()] függvény.</span><span class="sxs-lookup"><span data-stu-id="36bc8-110">hello best way toosee hello available release revision, its default configuration, and hello enabled extensions is toodeploy a script that calls hello [phpinfo()] function.</span></span>

<span data-ttu-id="36bc8-111">PHP 5.6 és a PHP 7.0-s verziója is, elérhető, de alapértelmezés szerint nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="36bc8-111">PHP 5.6 and PHP 7.0 versions are also available, but not enabled by default.</span></span> <span data-ttu-id="36bc8-112">tooupdate hello PHP verzióját, kövesse az alábbi módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="36bc8-112">tooupdate hello PHP version, follow one of these methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="36bc8-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="36bc8-113">Azure Portal</span></span>
1. <span data-ttu-id="36bc8-114">Keresse meg a webalkalmazás tooyour hello [Azure Portal](https://portal.azure.com) , majd kattintson a hello **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="36bc8-114">Browse tooyour web app in hello [Azure Portal](https://portal.azure.com) and click on hello **Settings** button.</span></span>
   
    ![A webalkalmazás-beállítások][settings-button]
2. <span data-ttu-id="36bc8-116">A hello **beállítások** panelen válassza ki **Alkalmazásbeállítások** , és válassza a hello új PHP verzióját.</span><span class="sxs-lookup"><span data-stu-id="36bc8-116">From hello **Settings** blade select **Application Settings** and choose hello new PHP version.</span></span>
   
    ![Alkalmazásbeállítások][application-settings]
3. <span data-ttu-id="36bc8-118">Hello kattintson **mentése** hello hello tetején gomb **webalkalmazás-beállításainak** panelen.</span><span class="sxs-lookup"><span data-stu-id="36bc8-118">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Konfigurációs beállítások mentéséhez][save-button]

### <a name="azure-powershell-windows"></a><span data-ttu-id="36bc8-120">Az Azure PowerShell (Windows)</span><span class="sxs-lookup"><span data-stu-id="36bc8-120">Azure PowerShell (Windows)</span></span>
1. <span data-ttu-id="36bc8-121">Nyissa meg az Azure PowerShell és a bejelentkezési tooyour fiókot:</span><span class="sxs-lookup"><span data-stu-id="36bc8-121">Open Azure PowerShell, and login tooyour account:</span></span>
   
        PS C:\> Login-AzureRmAccount
2. <span data-ttu-id="36bc8-122">Állítsa be a PHP verzióját hello hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="36bc8-122">Set hello PHP version for hello web app.</span></span>
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. <span data-ttu-id="36bc8-123">hello PHP verziója most van beállítva.</span><span class="sxs-lookup"><span data-stu-id="36bc8-123">hello PHP version is now set.</span></span> <span data-ttu-id="36bc8-124">Ezek a beállítások ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="36bc8-124">You can confirm these settings:</span></span>
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a><span data-ttu-id="36bc8-125">Azure parancssori felület (Linux, Mac, Windows)</span><span class="sxs-lookup"><span data-stu-id="36bc8-125">Azure Command-Line Interface (Linux, Mac, Windows)</span></span>
<span data-ttu-id="36bc8-126">toouse hello Azure parancssori felület, akkor **Node.js** telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="36bc8-126">toouse hello Azure Command-Line Interface, you must have **Node.js** installed on your computer.</span></span>

1. <span data-ttu-id="36bc8-127">Nyissa meg a terminált, és a bejelentkezési tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="36bc8-127">Open Terminal, and login tooyour account.</span></span>
   
        azure login
2. <span data-ttu-id="36bc8-128">Állítsa be a PHP verzióját hello hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="36bc8-128">Set hello PHP version for hello web app.</span></span>
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. <span data-ttu-id="36bc8-129">hello PHP verziója most van beállítva.</span><span class="sxs-lookup"><span data-stu-id="36bc8-129">hello PHP version is now set.</span></span> <span data-ttu-id="36bc8-130">Ezek a beállítások ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="36bc8-130">You can confirm these settings:</span></span>
   
        azure site show {app-name}

> [!NOTE] 
> <span data-ttu-id="36bc8-131">Hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) , amelyek a fenti egyenértékű toohello parancsok a következők:</span><span class="sxs-lookup"><span data-stu-id="36bc8-131">hello [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands that are equivalent toohello above are:</span></span>
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-hello-built-in-php-configurations"></a><span data-ttu-id="36bc8-132">Hogyan: hello beépített PHP-beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="36bc8-132">How to: Change hello built-in PHP configurations</span></span>
<span data-ttu-id="36bc8-133">A beépített PHP futási időben hello alábbi lépéseket követve módosíthatja hello konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="36bc8-133">For any built-in PHP runtime, you can change any of hello configuration options by following hello steps below.</span></span> <span data-ttu-id="36bc8-134">(Php.ini irányelvek kapcsolatos információkért lásd: [php.ini irányelvek].)</span><span class="sxs-lookup"><span data-stu-id="36bc8-134">(For information about php.ini directives, see [List of php.ini directives].)</span></span>

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a><span data-ttu-id="36bc8-135">A PHP módosítása\_INI\_felhasználó, a PHP\_INI\_PERDIR, a PHP\_INI\_beállításokkal</span><span class="sxs-lookup"><span data-stu-id="36bc8-135">Changing PHP\_INI\_USER, PHP\_INI\_PERDIR, PHP\_INI\_ALL configuration settings</span></span>
1. <span data-ttu-id="36bc8-136">Adja hozzá a [. user.ini] fájl tooyour gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="36bc8-136">Add a [.user.ini] file tooyour root directory.</span></span>
2. <span data-ttu-id="36bc8-137">Adja hozzá a konfigurációs beállítások toohello `.user.ini` fájl használata a használhatja ugyanazt a szintaxist hello egy `php.ini` fájl.</span><span class="sxs-lookup"><span data-stu-id="36bc8-137">Add configuration settings toohello `.user.ini` file using hello same syntax you would use in a `php.ini` file.</span></span> <span data-ttu-id="36bc8-138">Például, ha tooturn hello `display_errors` meg, és állítsa be a beállítás `upload_max_filesize` too10M, beállítása a `.user.ini` fájl tartalmazza egyrészt az ezt a szöveget:</span><span class="sxs-lookup"><span data-stu-id="36bc8-138">For example, if you wanted tooturn hello `display_errors` setting on and set `upload_max_filesize` setting too10M, your `.user.ini` file would contain this text:</span></span>
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on toowrite errors tood:\home\LogFiles\php_errors.log
        ; log_errors=On
3. <span data-ttu-id="36bc8-139">A webes alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="36bc8-139">Deploy your web app.</span></span>
4. <span data-ttu-id="36bc8-140">Hello a webalkalmazás újraindítása.</span><span class="sxs-lookup"><span data-stu-id="36bc8-140">Restart hello web app.</span></span> <span data-ttu-id="36bc8-141">(Újraindítás szükség, mert mely PHP hello gyakorisága beolvassa `.user.ini` fájlok szabályozza hello `user_ini.cache_ttl` beállítás, amely egy rendszerszintű beállítás, és alapértelmezés szerint 300 másodpercig (5 percig).</span><span class="sxs-lookup"><span data-stu-id="36bc8-141">(Restarting is necessary because hello frequency with which PHP reads `.user.ini` files is governed by hello `user_ini.cache_ttl` setting, which is a system level setting and is 300 seconds (5 minutes) by default.</span></span> <span data-ttu-id="36bc8-142">Újraindítás hello webalkalmazás kényszeríti a PHP tooread hello új beállításai hello `.user.ini` fájl.)</span><span class="sxs-lookup"><span data-stu-id="36bc8-142">Restarting hello web app forces PHP tooread hello new settings in hello `.user.ini` file.)</span></span>

<span data-ttu-id="36bc8-143">Egy alternatív toousing, egy `.user.ini` fájlt, használhatja a hello [ini_set()] parancsfájlok tooset konfigurációs beállítások, amelyek nem rendszerszintű irányelvek függvényt.</span><span class="sxs-lookup"><span data-stu-id="36bc8-143">As an alternative toousing a `.user.ini` file, you can use hello [ini_set()] function in scripts tooset configuration options that are not system-level directives.</span></span>

### <a name="changing-phpinisystem-configuration-settings"></a><span data-ttu-id="36bc8-144">A PHP módosítása\_INI\_rendszer konfigurációs beállításait</span><span class="sxs-lookup"><span data-stu-id="36bc8-144">Changing PHP\_INI\_SYSTEM configuration settings</span></span>
1. <span data-ttu-id="36bc8-145">Adja hozzá az Alkalmazásbeállítás tooyour webalkalmazás hello kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="36bc8-145">Add an App Setting tooyour Web App with hello key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
2. <span data-ttu-id="36bc8-146">Hozzon létre egy `settings.ini` fájlt a Kudu-konzollal (http://&lt;site-name&gt;. scm.azurewebsite.net) a hello `d:\home\site\ini` directory.</span><span class="sxs-lookup"><span data-stu-id="36bc8-146">Create an `settings.ini` file using Kudu Console (http://&lt;site-name&gt;.scm.azurewebsite.net) in hello `d:\home\site\ini` directory.</span></span>
3. <span data-ttu-id="36bc8-147">Adja hozzá a konfigurációs beállítások toohello `settings.ini` fájl használatával hello ugyanazt a szintaxist a php.ini fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="36bc8-147">Add configuration settings toohello `settings.ini` file using hello same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="36bc8-148">Például, ha toopoint hello `curl.cainfo` tooa beállítás `*.crt` fájlt, és too512K, beállítása wincache.maxfilesize a `settings.ini` fájl tartalmazza egyrészt az ezt a szöveget:</span><span class="sxs-lookup"><span data-stu-id="36bc8-148">For example, if you wanted toopoint hello `curl.cainfo` setting tooa `*.crt` file and set 'wincache.maxfilesize' setting too512K, your `settings.ini` file would contain this text:</span></span>
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. <span data-ttu-id="36bc8-149">Indítsa újra a webes alkalmazás tooload hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="36bc8-149">Restart your Web App tooload hello changes.</span></span>

## <a name="how-to-enable-extensions-in-hello-default-php-runtime"></a><span data-ttu-id="36bc8-150">Hogyan: hello alapértelmezett a PHP futtatókörnyezetben bővítmények engedélyezése</span><span class="sxs-lookup"><span data-stu-id="36bc8-150">How to: Enable extensions in hello default PHP runtime</span></span>
<span data-ttu-id="36bc8-151">Hello előző szakaszban, hello legjobb módja toosee hello alapértelmezett PHP verzióját, az alapértelmezett konfiguráció és a engedélyezve hello leírtaknak megfelelően-bővítmények között olyan parancsfájlt, amely meghívja a toodeploy [phpinfo()].</span><span class="sxs-lookup"><span data-stu-id="36bc8-151">As noted in hello previous section, hello best way toosee hello default PHP version, its default configuration, and hello enabled extensions is toodeploy a script that calls [phpinfo()].</span></span> <span data-ttu-id="36bc8-152">tooenable további kiterjesztések, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="36bc8-152">tooenable additional extensions, follow hello steps below:</span></span>

### <a name="configure-via-ini-settings"></a><span data-ttu-id="36bc8-153">Keresztül ini-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36bc8-153">Configure via ini settings</span></span>
1. <span data-ttu-id="36bc8-154">Adja hozzá a `ext` directory toohello `d:\home\site` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="36bc8-154">Add a `ext` directory toohello `d:\home\site` directory.</span></span>
2. <span data-ttu-id="36bc8-155">PUT `.dll` szerepkörbővítmény-fájlokat a hello `ext` könyvtárat (például `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="36bc8-155">Put `.dll` extension files in hello `ext` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="36bc8-156">Győződjön meg arról, hogy hello bővítmények kompatibilisek alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="36bc8-156">Make sure that hello extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="36bc8-157">Adja hozzá az Alkalmazásbeállítás tooyour webalkalmazás hello kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="36bc8-157">Add an App Setting tooyour Web App with hello key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
4. <span data-ttu-id="36bc8-158">Hozzon létre egy `ini` fájlt `d:\home\site\ini` nevű `extensions.ini`.</span><span class="sxs-lookup"><span data-stu-id="36bc8-158">Create an `ini` file in `d:\home\site\ini` called `extensions.ini`.</span></span>
5. <span data-ttu-id="36bc8-159">Adja hozzá a konfigurációs beállítások toohello `extensions.ini` fájl használatával hello ugyanazt a szintaxist a php.ini fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="36bc8-159">Add configuration settings toohello `extensions.ini` file using hello same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="36bc8-160">Ha tooenable hello MongoDB és XDebug bővítmények, például a `extensions.ini` fájl tartalmazza egyrészt az ezt a szöveget:</span><span class="sxs-lookup"><span data-stu-id="36bc8-160">For example, if you wanted tooenable hello MongoDB and XDebug extensions, your `extensions.ini` file would contain this text:</span></span>
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. <span data-ttu-id="36bc8-161">Indítsa újra a webes alkalmazás tooload hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="36bc8-161">Restart your Web App tooload hello changes.</span></span>

### <a name="configure-via-app-setting"></a><span data-ttu-id="36bc8-162">Alkalmazásbeállítás keresztül konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36bc8-162">Configure via App Setting</span></span>
1. <span data-ttu-id="36bc8-163">Adja hozzá a `bin` directory toohello gyökérkönyvtár.</span><span class="sxs-lookup"><span data-stu-id="36bc8-163">Add a `bin` directory toohello root directory.</span></span>
2. <span data-ttu-id="36bc8-164">PUT `.dll` szerepkörbővítmény-fájlokat a hello `bin` könyvtárat (például `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="36bc8-164">Put `.dll` extension files in hello `bin` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="36bc8-165">Győződjön meg arról, hogy hello bővítmények kompatibilisek alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="36bc8-165">Make sure that hello extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="36bc8-166">A webes alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="36bc8-166">Deploy your web app.</span></span>
4. <span data-ttu-id="36bc8-167">Keresse meg a webalkalmazás tooyour hello Azure portálon, majd kattintson a hello **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="36bc8-167">Browse tooyour web app in hello Azure Portal and click on hello **Settings** button.</span></span>
   
    ![A webalkalmazás-beállítások][settings-button]
5. <span data-ttu-id="36bc8-169">A hello **beállítások** panelen válassza ki **Alkalmazásbeállítások** toohello görgessen **Alkalmazásbeállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="36bc8-169">From hello **Settings** blade select **Application Settings** and scroll toohello **App settings** section.</span></span>
6. <span data-ttu-id="36bc8-170">A hello **Alkalmazásbeállítások** szakaszban, hozzon létre egy **PHP_EXTENSIONS** kulcs.</span><span class="sxs-lookup"><span data-stu-id="36bc8-170">In hello **App settings** section, create a **PHP_EXTENSIONS** key.</span></span> <span data-ttu-id="36bc8-171">a kulcs hello értéke lenne a legfelső szintű elérési útja relatív toowebsite: **bin\your-ext-fájl**.</span><span class="sxs-lookup"><span data-stu-id="36bc8-171">hello value for this key would be a path relative toowebsite root: **bin\your-ext-file**.</span></span>
   
    ![Alkalmazásbeállítások a bővítmény engedélyezése][php-extensions]
7. <span data-ttu-id="36bc8-173">Hello kattintson **mentése** hello hello tetején gomb **webalkalmazás-beállításainak** panelen.</span><span class="sxs-lookup"><span data-stu-id="36bc8-173">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Konfigurációs beállítások mentéséhez][save-button]

<span data-ttu-id="36bc8-175">A Zend bővítmények használatával is támogatottak a **PHP_ZENDEXTENSIONS** kulcs.</span><span class="sxs-lookup"><span data-stu-id="36bc8-175">Zend extensions are also supported by using a **PHP_ZENDEXTENSIONS** key.</span></span> <span data-ttu-id="36bc8-176">tooenable több kiterjesztést tartalmaz egy vesszővel elválasztott listája `.dll` hello app beállításérték fájljait.</span><span class="sxs-lookup"><span data-stu-id="36bc8-176">tooenable multiple extensions, include a comma-separated list of `.dll` files for hello app setting value.</span></span>

## <a name="how-to-use-a-custom-php-runtime"></a><span data-ttu-id="36bc8-177">Útmutató: egy egyéni PHP futtatókörnyezetben használja</span><span class="sxs-lookup"><span data-stu-id="36bc8-177">How to: Use a custom PHP runtime</span></span>
<span data-ttu-id="36bc8-178">Hello alapértelmezett PHP futtatókörnyezetben helyett App Service Web Apps használhatja egy adni tooexecute PHP-parancsfájlok PHP futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="36bc8-178">Instead of hello default PHP runtime, App Service Web Apps can use a PHP runtime that you provide tooexecute PHP scripts.</span></span> <span data-ttu-id="36bc8-179">Ön által biztosított hello futásidejű konfigurálhatja egy `php.ini` is nyújtó fájlt.</span><span class="sxs-lookup"><span data-stu-id="36bc8-179">hello runtime that you provide can be configured by a `php.ini` file that you also provide.</span></span> <span data-ttu-id="36bc8-180">a webalkalmazásokkal, egy egyéni PHP futtatókörnyezetben toouse kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="36bc8-180">toouse a custom PHP runtime with Web Apps, follow hello steps below.</span></span>

1. <span data-ttu-id="36bc8-181">Szerezzen be egy nem szálbiztos, VC9 vagy VC11 PHP for Windows kompatibilis verziója.</span><span class="sxs-lookup"><span data-stu-id="36bc8-181">Obtain a non-thread-safe, VC9 or VC11 compatible version of PHP for Windows.</span></span> <span data-ttu-id="36bc8-182">PHP for Windows újabb verzióiban itt található: [http://windows.php.net/download/].</span><span class="sxs-lookup"><span data-stu-id="36bc8-182">Recent releases of PHP for Windows can be found here: [http://windows.php.net/download/].</span></span> <span data-ttu-id="36bc8-183">Régebbi kiadásai hello archív itt található: [http://windows.php.net/downloads/releases/archives/].</span><span class="sxs-lookup"><span data-stu-id="36bc8-183">Older releases can be found in hello archive here: [http://windows.php.net/downloads/releases/archives/].</span></span>
2. <span data-ttu-id="36bc8-184">Módosítsa a hello `php.ini` fájl a futási időben.</span><span class="sxs-lookup"><span data-stu-id="36bc8-184">Modify hello `php.ini` file for your runtime.</span></span> <span data-ttu-id="36bc8-185">Vegye figyelembe, hogy a konfigurációs beállítások, amelyek a rendszer-szintű-csak irányelvek webes alkalmazások által használt rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="36bc8-185">Note that any configuration settings that are system-level-only directives will be ignored by Web Apps.</span></span> <span data-ttu-id="36bc8-186">(Rendszer-szintű-csak irányelvek kapcsolatos információkért lásd: [php.ini irányelvek]).</span><span class="sxs-lookup"><span data-stu-id="36bc8-186">(For information about system-level-only directives, see [List of php.ini directives]).</span></span>
3. <span data-ttu-id="36bc8-187">Igény szerint adja hozzá a bővítmények tooyour PHP futtatókörnyezetben, és lehetővé teszi a hello `php.ini` fájlt.</span><span class="sxs-lookup"><span data-stu-id="36bc8-187">Optionally, add extensions tooyour PHP runtime and enable them in hello `php.ini` file.</span></span>
4. <span data-ttu-id="36bc8-188">Adja hozzá a `bin` directory tooyour legfelső szintű és put hello könyvtárat, amely tartalmazza azt a PHP futtatókörnyezetben (például `bin\php`).</span><span class="sxs-lookup"><span data-stu-id="36bc8-188">Add a `bin` directory tooyour root directory, and put hello directory that contains your PHP runtime in it (for example, `bin\php`).</span></span>
5. <span data-ttu-id="36bc8-189">A webes alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="36bc8-189">Deploy your web app.</span></span>
6. <span data-ttu-id="36bc8-190">Keresse meg a webalkalmazás tooyour hello Azure portálon, majd kattintson a hello **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="36bc8-190">Browse tooyour web app in hello Azure Portal and click on hello **Settings** button.</span></span>
   
    ![A webalkalmazás-beállítások][settings-button]
7. <span data-ttu-id="36bc8-192">A hello **beállítások** panelen válassza ki **Alkalmazásbeállítások** toohello görgessen **kezelőleképezések** szakasz.</span><span class="sxs-lookup"><span data-stu-id="36bc8-192">From hello **Settings** blade select **Application Settings** and scroll toohello **Handler mappings** section.</span></span> <span data-ttu-id="36bc8-193">Adja hozzá `*.php` toohello bővítmény mezőben, majd adja hozzá az elérési út toohello hello `php-cgi.exe` végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="36bc8-193">Add `*.php` toohello Extension field and add hello path toohello `php-cgi.exe` executable.</span></span> <span data-ttu-id="36bc8-194">Ha a PHP futtatókörnyezetben be hello `bin` hello elérési út lesz az alkalmazás gyökérkönyvtárában hello könyvtár, `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span><span class="sxs-lookup"><span data-stu-id="36bc8-194">If you put your PHP runtime in hello `bin` directory in hello root of you application, hello path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span></span>
   
    ![Adja meg a kezelő meg leképezései][handler-mappings]
8. <span data-ttu-id="36bc8-196">Hello kattintson **mentése** hello hello tetején gomb **webalkalmazás-beállításainak** panelen.</span><span class="sxs-lookup"><span data-stu-id="36bc8-196">Click hello **Save** button at hello top of hello **Web app settings** blade.</span></span>
   
    ![Konfigurációs beállítások mentéséhez][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a><span data-ttu-id="36bc8-198">Hogyan: szerkesztő automatizálhatják az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="36bc8-198">How to: Enable Composer automation in Azure</span></span>
<span data-ttu-id="36bc8-199">Alapértelmezés szerint az App Service nem minden composer.json, ha nincs fiókja, a PHP-projektben.</span><span class="sxs-lookup"><span data-stu-id="36bc8-199">By default, App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="36bc8-200">Ha [Git-telepítésének](app-service-deploy-local-git.md), engedélyezheti a feldolgozás során composer.json `git push` hello Composer bővítményt engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="36bc8-200">If you use [Git deployment](app-service-deploy-local-git.md), you can enable composer.json processing during `git push` by enabling hello Composer extension.</span></span>

> [!NOTE]
> <span data-ttu-id="36bc8-201">Is [szerkesztő támogatja az App Service itt szavazzon](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span><span class="sxs-lookup"><span data-stu-id="36bc8-201">You can [vote for first-class Composer support in App Service here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span></span>
> 
> 

1. <span data-ttu-id="36bc8-202">A PHP a webalkalmazás-alkalmazás paneljén hello [Azure-portálon](https://portal.azure.com), kattintson a **eszközök** > **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="36bc8-202">In your PHP web app's blade in hello [Azure portal](https://portal.azure.com), click **Tools** > **Extensions**.</span></span>
   
    ![Az Azure portál beállítások panel tooenable szerkesztő automation az Azure-ban](./media/web-sites-php-configure/composer-extension-settings.png)
2. <span data-ttu-id="36bc8-204">Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="36bc8-204">Click **Add**, then click **Composer**.</span></span>
   
    ![Composer bővítményt tooenable szerkesztő automation hozzáadása az Azure-ban](./media/web-sites-php-configure/composer-extension-add.png)
3. <span data-ttu-id="36bc8-206">Kattintson a **OK** tooaccept jogi feltételeket.</span><span class="sxs-lookup"><span data-stu-id="36bc8-206">Click **OK** tooaccept legal terms.</span></span> <span data-ttu-id="36bc8-207">Kattintson a **OK** újra tooadd hello bővítmény.</span><span class="sxs-lookup"><span data-stu-id="36bc8-207">Click **OK** again tooadd hello extension.</span></span>
   
    <span data-ttu-id="36bc8-208">Hello **bővítményeket telepített** panel mostantól megjeleníti hello Composer bővítményt.</span><span class="sxs-lookup"><span data-stu-id="36bc8-208">hello **Installed extensions** blade will now show hello Composer extension.</span></span>  
    <span data-ttu-id="36bc8-209">![Fogadja el a jogi feltételeket tooenable szerkesztő automation az Azure-ban](./media/web-sites-php-configure/composer-extension-view.png)</span><span class="sxs-lookup"><span data-stu-id="36bc8-209">![Accept legal terms tooenable Composer automation in Azure](./media/web-sites-php-configure/composer-extension-view.png)</span></span>
4. <span data-ttu-id="36bc8-210">Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="36bc8-210">Now, perform `git add`, `git commit`, and `git push` like in hello previous section.</span></span> <span data-ttu-id="36bc8-211">Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.</span><span class="sxs-lookup"><span data-stu-id="36bc8-211">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Git-telepítés az szerkesztő Automation szolgáltatásban, az Azure-ban](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a><span data-ttu-id="36bc8-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36bc8-213">Next steps</span></span>
<span data-ttu-id="36bc8-214">További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="36bc8-214">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

> [!NOTE]
> <span data-ttu-id="36bc8-215">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="36bc8-215">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="36bc8-216">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="36bc8-216">No credit cards required; no commitments.</span></span>
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


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
# <a name="configure-php-in-azure-app-service-web-apps"></a><span data-ttu-id="cfe69-103">Az Azure App Service Web Apps PHP-konfigurálás</span><span class="sxs-lookup"><span data-stu-id="cfe69-103">Configure PHP in Azure App Service Web Apps</span></span>
## <a name="introduction"></a><span data-ttu-id="cfe69-104">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cfe69-104">Introduction</span></span>
<span data-ttu-id="cfe69-105">Ez az útmutató bemutatja, hogyan konfigurálhatja a beépített PHP futtatókörnyezetben a Web Apps [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), adjon meg egy egyéni PHP futtatókörnyezetben és bővítmények engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cfe69-105">This guide will show you how to configure the built-in PHP runtime for Web Apps in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), provide a custom PHP runtime, and enable extensions.</span></span> <span data-ttu-id="cfe69-106">App Service használni, Regisztráljon a [ingyenes próbaverzió].</span><span class="sxs-lookup"><span data-stu-id="cfe69-106">To use App Service, sign up for the [free trial].</span></span> <span data-ttu-id="cfe69-107">Ahhoz, hogy a legtöbbet tudja kihozni Ez az útmutató, kell először létrehozhat egy PHP webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="cfe69-107">To get the most from this guide, you should first create a PHP web app in App Service.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="how-to-change-the-built-in-php-version"></a><span data-ttu-id="cfe69-108">Útmutató: a beépített PHP verzióját módosítása</span><span class="sxs-lookup"><span data-stu-id="cfe69-108">How to: Change the built-in PHP version</span></span>
<span data-ttu-id="cfe69-109">Alapértelmezés szerint a PHP 5.5 telepítve és használhatja az App Service webalkalmazás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cfe69-109">By default, PHP 5.5 is installed and immediately available for use when you create an App Service web app.</span></span> <span data-ttu-id="cfe69-110">A legjobb módja a rendelkezésre álló kiadási változat, az alapértelmezett konfigurációval, és az engedélyezett bővítmények, központi telepítése egy parancsfájlt, amely meghívja a [phpinfo()] függvény.</span><span class="sxs-lookup"><span data-stu-id="cfe69-110">The best way to see the available release revision, its default configuration, and the enabled extensions is to deploy a script that calls the [phpinfo()] function.</span></span>

<span data-ttu-id="cfe69-111">PHP 5.6 és a PHP 7.0-s verziója is, elérhető, de alapértelmezés szerint nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="cfe69-111">PHP 5.6 and PHP 7.0 versions are also available, but not enabled by default.</span></span> <span data-ttu-id="cfe69-112">A PHP verzióját frissítéséhez hajtsa végre az ezen módszerek egyikét:</span><span class="sxs-lookup"><span data-stu-id="cfe69-112">To update the PHP version, follow one of these methods:</span></span>

### <a name="azure-portal"></a><span data-ttu-id="cfe69-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cfe69-113">Azure Portal</span></span>
1. <span data-ttu-id="cfe69-114">Keresse meg a webalkalmazás a [Azure Portal](https://portal.azure.com) , majd kattintson a a **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="cfe69-114">Browse to your web app in the [Azure Portal](https://portal.azure.com) and click on the **Settings** button.</span></span>
   
    ![A webalkalmazás-beállítások][settings-button]
2. <span data-ttu-id="cfe69-116">Az a **beállítások** panelen válassza ki **Alkalmazásbeállítások** , és válassza az új PHP-verziót.</span><span class="sxs-lookup"><span data-stu-id="cfe69-116">From the **Settings** blade select **Application Settings** and choose the new PHP version.</span></span>
   
    ![Alkalmazásbeállítások][application-settings]
3. <span data-ttu-id="cfe69-118">Kattintson a **mentése** gomb tetején a **webalkalmazás-beállításainak** panelen.</span><span class="sxs-lookup"><span data-stu-id="cfe69-118">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Konfigurációs beállítások mentéséhez][save-button]

### <a name="azure-powershell-windows"></a><span data-ttu-id="cfe69-120">Az Azure PowerShell (Windows)</span><span class="sxs-lookup"><span data-stu-id="cfe69-120">Azure PowerShell (Windows)</span></span>
1. <span data-ttu-id="cfe69-121">Nyissa meg az Azure PowerShell, és jelentkezzen be a fiók:</span><span class="sxs-lookup"><span data-stu-id="cfe69-121">Open Azure PowerShell, and login to your account:</span></span>
   
        PS C:\> Login-AzureRmAccount
2. <span data-ttu-id="cfe69-122">Állítsa be a PHP verzióját a webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="cfe69-122">Set the PHP version for the web app.</span></span>
   
        PS C:\> Set-AzureWebsite -PhpVersion {5.5 | 5.6 | 7.0} -Name {app-name}
3. <span data-ttu-id="cfe69-123">A PHP verzióját most van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cfe69-123">The PHP version is now set.</span></span> <span data-ttu-id="cfe69-124">Ezek a beállítások ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="cfe69-124">You can confirm these settings:</span></span>
   
        PS C:\> Get-AzureWebsite -Name {app-name} | findstr PhpVersion

### <a name="azure-command-line-interface-linux-mac-windows"></a><span data-ttu-id="cfe69-125">Azure parancssori felület (Linux, Mac, Windows)</span><span class="sxs-lookup"><span data-stu-id="cfe69-125">Azure Command-Line Interface (Linux, Mac, Windows)</span></span>
<span data-ttu-id="cfe69-126">Az Azure parancssori felület használatához rendelkeznie kell **Node.js** telepítve a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="cfe69-126">To use the Azure Command-Line Interface, you must have **Node.js** installed on your computer.</span></span>

1. <span data-ttu-id="cfe69-127">Nyissa meg terminált, és jelentkezzen be fiókjába.</span><span class="sxs-lookup"><span data-stu-id="cfe69-127">Open Terminal, and login to your account.</span></span>
   
        azure login
2. <span data-ttu-id="cfe69-128">Állítsa be a PHP verzióját a webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="cfe69-128">Set the PHP version for the web app.</span></span>
   
        azure site set --php-version {5.5 | 5.6 | 7.0} {app-name}

3. <span data-ttu-id="cfe69-129">A PHP verzióját most van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cfe69-129">The PHP version is now set.</span></span> <span data-ttu-id="cfe69-130">Ezek a beállítások ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="cfe69-130">You can confirm these settings:</span></span>
   
        azure site show {app-name}

> [!NOTE] 
> <span data-ttu-id="cfe69-131">A [Azure CLI 2.0](https://github.com/Azure/azure-cli) egyenértékű a fenti parancsok a következők:</span><span class="sxs-lookup"><span data-stu-id="cfe69-131">The [Azure CLI 2.0](https://github.com/Azure/azure-cli) commands that are equivalent to the above are:</span></span>
>
>

    az login
    az appservice web config update --php-version {5.5 | 5.6 | 7.0} -g {resource-group-name} -n {app-name}
    az appservice web config show -g {resource-group-name} -n {app-name}

## <a name="how-to-change-the-built-in-php-configurations"></a><span data-ttu-id="cfe69-132">Útmutató: a beépített PHP-beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="cfe69-132">How to: Change the built-in PHP configurations</span></span>
<span data-ttu-id="cfe69-133">Bármely beépített PHP futási időben az alábbi lépéseket követve módosíthatja a konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="cfe69-133">For any built-in PHP runtime, you can change any of the configuration options by following the steps below.</span></span> <span data-ttu-id="cfe69-134">(Php.ini irányelvek kapcsolatos információkért lásd: [php.ini irányelvek].)</span><span class="sxs-lookup"><span data-stu-id="cfe69-134">(For information about php.ini directives, see [List of php.ini directives].)</span></span>

### <a name="changing-phpiniuser-phpiniperdir-phpiniall-configuration-settings"></a><span data-ttu-id="cfe69-135">A PHP módosítása\_INI\_felhasználó, a PHP\_INI\_PERDIR, a PHP\_INI\_beállításokkal</span><span class="sxs-lookup"><span data-stu-id="cfe69-135">Changing PHP\_INI\_USER, PHP\_INI\_PERDIR, PHP\_INI\_ALL configuration settings</span></span>
1. <span data-ttu-id="cfe69-136">Adja hozzá a [. user.ini] fájlt a gyökérkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="cfe69-136">Add a [.user.ini] file to your root directory.</span></span>
2. <span data-ttu-id="cfe69-137">Adja hozzá a konfigurációs beállításokat a `.user.ini` használja ugyanazt a szintaxist használja a fájl egy `php.ini` fájlt.</span><span class="sxs-lookup"><span data-stu-id="cfe69-137">Add configuration settings to the `.user.ini` file using the same syntax you would use in a `php.ini` file.</span></span> <span data-ttu-id="cfe69-138">Például, ha szeretné kapcsolni a `display_errors` meg, és állítsa be a beállítás `upload_max_filesize` 10M beállítást a `.user.ini` fájl tartalmazza egyrészt az ezt a szöveget:</span><span class="sxs-lookup"><span data-stu-id="cfe69-138">For example, if you wanted to turn the `display_errors` setting on and set `upload_max_filesize` setting to 10M, your `.user.ini` file would contain this text:</span></span>
   
        ; Example Settings
        display_errors=On
        upload_max_filesize=10M
   
        ; OPTIONAL: Turn this on to write errors to d:\home\LogFiles\php_errors.log
        ; log_errors=On
3. <span data-ttu-id="cfe69-139">A webes alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="cfe69-139">Deploy your web app.</span></span>
4. <span data-ttu-id="cfe69-140">A webalkalmazás újraindítása.</span><span class="sxs-lookup"><span data-stu-id="cfe69-140">Restart the web app.</span></span> <span data-ttu-id="cfe69-141">(Újraindítás szükség, mert a gyakorisága mely PHP beolvassa `.user.ini` fájlok szabályozza a `user_ini.cache_ttl` beállítás, amely egy rendszerszintű beállítás, és alapértelmezés szerint 300 másodpercig (5 percig).</span><span class="sxs-lookup"><span data-stu-id="cfe69-141">(Restarting is necessary because the frequency with which PHP reads `.user.ini` files is governed by the `user_ini.cache_ttl` setting, which is a system level setting and is 300 seconds (5 minutes) by default.</span></span> <span data-ttu-id="cfe69-142">A webalkalmazás újraindítása kényszeríti a php-hez az új beállítások beolvasása a `.user.ini` fájl.)</span><span class="sxs-lookup"><span data-stu-id="cfe69-142">Restarting the web app forces PHP to read the new settings in the `.user.ini` file.)</span></span>

<span data-ttu-id="cfe69-143">Használata helyett egy `.user.ini` fájlt, használhatja a [ini_set()] parancsfájlok, amelyek nem rendszerszintű irányelvek konfigurációs beállítások megadása a függvényt.</span><span class="sxs-lookup"><span data-stu-id="cfe69-143">As an alternative to using a `.user.ini` file, you can use the [ini_set()] function in scripts to set configuration options that are not system-level directives.</span></span>

### <a name="changing-phpinisystem-configuration-settings"></a><span data-ttu-id="cfe69-144">A PHP módosítása\_INI\_rendszer konfigurációs beállításait</span><span class="sxs-lookup"><span data-stu-id="cfe69-144">Changing PHP\_INI\_SYSTEM configuration settings</span></span>
1. <span data-ttu-id="cfe69-145">Egy Alkalmazásbeállítás hozzáadása a webalkalmazáshoz kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="cfe69-145">Add an App Setting to your Web App with the key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
2. <span data-ttu-id="cfe69-146">Hozzon létre egy `settings.ini` fájlt a Kudu konzol használata (http://&lt;site-name&gt;. scm.azurewebsite.net) az a `d:\home\site\ini` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cfe69-146">Create an `settings.ini` file using Kudu Console (http://&lt;site-name&gt;.scm.azurewebsite.net) in the `d:\home\site\ini` directory.</span></span>
3. <span data-ttu-id="cfe69-147">Adja hozzá a konfigurációs beállításokat a `settings.ini` fájlt használhatja ugyanazt a szintaxist használja a php.ini fájlban.</span><span class="sxs-lookup"><span data-stu-id="cfe69-147">Add configuration settings to the `settings.ini` file using the same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="cfe69-148">Például, ha szeretné, mutasson a `curl.cainfo` beállítást egy `*.crt` fájlt, és állítsa be a "wincache.maxfilesize" 512 KB, a `settings.ini` fájl tartalmazza egyrészt az ezt a szöveget:</span><span class="sxs-lookup"><span data-stu-id="cfe69-148">For example, if you wanted to point the `curl.cainfo` setting to a `*.crt` file and set 'wincache.maxfilesize' setting to 512K, your `settings.ini` file would contain this text:</span></span>
   
        ; Example Settings
        curl.cainfo="%ProgramFiles(x86)%\Git\bin\curl-ca-bundle.crt"
        wincache.maxfilesize=512
4. <span data-ttu-id="cfe69-149">Indítsa újra a webalkalmazás betöltése a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cfe69-149">Restart your Web App to load the changes.</span></span>

## <a name="how-to-enable-extensions-in-the-default-php-runtime"></a><span data-ttu-id="cfe69-150">Útmutató: az alapértelmezett a PHP futtatókörnyezetben bővítmények engedélyezése</span><span class="sxs-lookup"><span data-stu-id="cfe69-150">How to: Enable extensions in the default PHP runtime</span></span>
<span data-ttu-id="cfe69-151">Az előző szakaszban leírtaknak megfelelően-e a legjobb módszer az alapértelmezett PHP verzióját, az alapértelmezett konfigurációval és az engedélyezett bővítmények telepítése egy parancsprogram, amely behívja [phpinfo()].</span><span class="sxs-lookup"><span data-stu-id="cfe69-151">As noted in the previous section, the best way to see the default PHP version, its default configuration, and the enabled extensions is to deploy a script that calls [phpinfo()].</span></span> <span data-ttu-id="cfe69-152">További kiterjesztések, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cfe69-152">To enable additional extensions, follow the steps below:</span></span>

### <a name="configure-via-ini-settings"></a><span data-ttu-id="cfe69-153">Keresztül ini-beállítások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cfe69-153">Configure via ini settings</span></span>
1. <span data-ttu-id="cfe69-154">Adja hozzá a `ext` könyvtárból a `d:\home\site` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="cfe69-154">Add a `ext` directory to the `d:\home\site` directory.</span></span>
2. <span data-ttu-id="cfe69-155">PUT `.dll` bővítmény-fájlok a `ext` könyvtárat (például `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="cfe69-155">Put `.dll` extension files in the `ext` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="cfe69-156">Győződjön meg arról, hogy a bővítmények alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis kompatibilisek legyenek-e.</span><span class="sxs-lookup"><span data-stu-id="cfe69-156">Make sure that the extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="cfe69-157">Egy Alkalmazásbeállítás hozzáadása a webalkalmazáshoz kulccsal `PHP_INI_SCAN_DIR` és érték`d:\home\site\ini`</span><span class="sxs-lookup"><span data-stu-id="cfe69-157">Add an App Setting to your Web App with the key `PHP_INI_SCAN_DIR` and value `d:\home\site\ini`</span></span>
4. <span data-ttu-id="cfe69-158">Hozzon létre egy `ini` fájlt `d:\home\site\ini` nevű `extensions.ini`.</span><span class="sxs-lookup"><span data-stu-id="cfe69-158">Create an `ini` file in `d:\home\site\ini` called `extensions.ini`.</span></span>
5. <span data-ttu-id="cfe69-159">Adja hozzá a konfigurációs beállításokat a `extensions.ini` fájlt használhatja ugyanazt a szintaxist használja a php.ini fájlban.</span><span class="sxs-lookup"><span data-stu-id="cfe69-159">Add configuration settings to the `extensions.ini` file using the same syntax you would use in a php.ini file.</span></span> <span data-ttu-id="cfe69-160">Ha szeretné engedélyezni a MongoDB és XDebug bővítmények, például a `extensions.ini` fájl tartalmazza egyrészt az ezt a szöveget:</span><span class="sxs-lookup"><span data-stu-id="cfe69-160">For example, if you wanted to enable the MongoDB and XDebug extensions, your `extensions.ini` file would contain this text:</span></span>
   
        ; Enable Extensions
        extension=d:\home\site\ext\php_mongo.dll
        zend_extension=d:\home\site\ext\php_xdebug.dll
6. <span data-ttu-id="cfe69-161">Indítsa újra a webalkalmazás betöltése a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cfe69-161">Restart your Web App to load the changes.</span></span>

### <a name="configure-via-app-setting"></a><span data-ttu-id="cfe69-162">Alkalmazásbeállítás keresztül konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cfe69-162">Configure via App Setting</span></span>
1. <span data-ttu-id="cfe69-163">Adja hozzá a `bin` könyvtár a gyökérkönyvtárba.</span><span class="sxs-lookup"><span data-stu-id="cfe69-163">Add a `bin` directory to the root directory.</span></span>
2. <span data-ttu-id="cfe69-164">PUT `.dll` bővítmény-fájlok a `bin` könyvtárat (például `php_xdebug.dll`).</span><span class="sxs-lookup"><span data-stu-id="cfe69-164">Put `.dll` extension files in the `bin` directory (for example, `php_xdebug.dll`).</span></span> <span data-ttu-id="cfe69-165">Győződjön meg arról, hogy a bővítmények alapértelmezett a PHP verzióját és azok VC9 és nem szálbiztos verziójának (nts) kompatibilis kompatibilisek legyenek-e.</span><span class="sxs-lookup"><span data-stu-id="cfe69-165">Make sure that the extensions are compatible with default version of PHP and are VC9 and non-thread-safe (nts) compatible.</span></span>
3. <span data-ttu-id="cfe69-166">A webes alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="cfe69-166">Deploy your web app.</span></span>
4. <span data-ttu-id="cfe69-167">Keresse meg a webalkalmazás az Azure portálon, majd kattintson a a **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="cfe69-167">Browse to your web app in the Azure Portal and click on the **Settings** button.</span></span>
   
    ![A webalkalmazás-beállítások][settings-button]
5. <span data-ttu-id="cfe69-169">Az a **beállítások** panelen válassza ki **Alkalmazásbeállítások** görgessen a **Alkalmazásbeállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cfe69-169">From the **Settings** blade select **Application Settings** and scroll to the **App settings** section.</span></span>
6. <span data-ttu-id="cfe69-170">Az a **Alkalmazásbeállítások** szakaszban, hozzon létre egy **PHP_EXTENSIONS** kulcs.</span><span class="sxs-lookup"><span data-stu-id="cfe69-170">In the **App settings** section, create a **PHP_EXTENSIONS** key.</span></span> <span data-ttu-id="cfe69-171">A kulcs értéke lenne egy elérési út a webhely gyökeréhez viszonyítva: **bin\your-ext-fájl**.</span><span class="sxs-lookup"><span data-stu-id="cfe69-171">The value for this key would be a path relative to website root: **bin\your-ext-file**.</span></span>
   
    ![Alkalmazásbeállítások a bővítmény engedélyezése][php-extensions]
7. <span data-ttu-id="cfe69-173">Kattintson a **mentése** gomb tetején a **webalkalmazás-beállításainak** panelen.</span><span class="sxs-lookup"><span data-stu-id="cfe69-173">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Konfigurációs beállítások mentéséhez][save-button]

<span data-ttu-id="cfe69-175">A Zend bővítmények használatával is támogatottak a **PHP_ZENDEXTENSIONS** kulcs.</span><span class="sxs-lookup"><span data-stu-id="cfe69-175">Zend extensions are also supported by using a **PHP_ZENDEXTENSIONS** key.</span></span> <span data-ttu-id="cfe69-176">Ahhoz, hogy több kiterjesztést, vesszővel tagolt listáját tartalmazza `.dll` fájlok esetében az alkalmazás-beállítás értékét.</span><span class="sxs-lookup"><span data-stu-id="cfe69-176">To enable multiple extensions, include a comma-separated list of `.dll` files for the app setting value.</span></span>

## <a name="how-to-use-a-custom-php-runtime"></a><span data-ttu-id="cfe69-177">Útmutató: egy egyéni PHP futtatókörnyezetben használja</span><span class="sxs-lookup"><span data-stu-id="cfe69-177">How to: Use a custom PHP runtime</span></span>
<span data-ttu-id="cfe69-178">Az alapértelmezett PHP futtatókörnyezetben helyett App Service Web Apps használhatja egy Ön által biztosított PHP-parancsfájlok végrehajtása PHP futtatókörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cfe69-178">Instead of the default PHP runtime, App Service Web Apps can use a PHP runtime that you provide to execute PHP scripts.</span></span> <span data-ttu-id="cfe69-179">Az Ön által biztosított futásidejű konfigurálhatja egy `php.ini` is nyújtó fájlt.</span><span class="sxs-lookup"><span data-stu-id="cfe69-179">The runtime that you provide can be configured by a `php.ini` file that you also provide.</span></span> <span data-ttu-id="cfe69-180">A webalkalmazásokkal egy egyéni PHP futtatókörnyezetben használatához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cfe69-180">To use a custom PHP runtime with Web Apps, follow the steps below.</span></span>

1. <span data-ttu-id="cfe69-181">Szerezzen be egy nem szálbiztos, VC9 vagy VC11 PHP for Windows kompatibilis verziója.</span><span class="sxs-lookup"><span data-stu-id="cfe69-181">Obtain a non-thread-safe, VC9 or VC11 compatible version of PHP for Windows.</span></span> <span data-ttu-id="cfe69-182">PHP for Windows újabb verzióiban itt található: [http://windows.php.net/download/].</span><span class="sxs-lookup"><span data-stu-id="cfe69-182">Recent releases of PHP for Windows can be found here: [http://windows.php.net/download/].</span></span> <span data-ttu-id="cfe69-183">Itt az archívumban található régebbi kiadásai: [http://windows.php.net/downloads/releases/archives/].</span><span class="sxs-lookup"><span data-stu-id="cfe69-183">Older releases can be found in the archive here: [http://windows.php.net/downloads/releases/archives/].</span></span>
2. <span data-ttu-id="cfe69-184">Módosítsa a `php.ini` fájl a futási időben.</span><span class="sxs-lookup"><span data-stu-id="cfe69-184">Modify the `php.ini` file for your runtime.</span></span> <span data-ttu-id="cfe69-185">Vegye figyelembe, hogy a konfigurációs beállítások, amelyek a rendszer-szintű-csak irányelvek webes alkalmazások által használt rendszer figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="cfe69-185">Note that any configuration settings that are system-level-only directives will be ignored by Web Apps.</span></span> <span data-ttu-id="cfe69-186">(Rendszer-szintű-csak irányelvek kapcsolatos információkért lásd: [php.ini irányelvek]).</span><span class="sxs-lookup"><span data-stu-id="cfe69-186">(For information about system-level-only directives, see [List of php.ini directives]).</span></span>
3. <span data-ttu-id="cfe69-187">Szükség esetén bővítmények vegye fel a PHP futtatókörnyezetben, és engedélyezze azokat a `php.ini` fájlt.</span><span class="sxs-lookup"><span data-stu-id="cfe69-187">Optionally, add extensions to your PHP runtime and enable them in the `php.ini` file.</span></span>
4. <span data-ttu-id="cfe69-188">Adja hozzá a `bin` könyvtárból a gyökérkönyvtár, és a put azt a PHP futtatókörnyezetben tartalmazó könyvtárat (például `bin\php`).</span><span class="sxs-lookup"><span data-stu-id="cfe69-188">Add a `bin` directory to your root directory, and put the directory that contains your PHP runtime in it (for example, `bin\php`).</span></span>
5. <span data-ttu-id="cfe69-189">A webes alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="cfe69-189">Deploy your web app.</span></span>
6. <span data-ttu-id="cfe69-190">Keresse meg a webalkalmazás az Azure portálon, majd kattintson a a **beállítások** gombra.</span><span class="sxs-lookup"><span data-stu-id="cfe69-190">Browse to your web app in the Azure Portal and click on the **Settings** button.</span></span>
   
    ![A webalkalmazás-beállítások][settings-button]
7. <span data-ttu-id="cfe69-192">Az a **beállítások** panelen válassza ki **Alkalmazásbeállítások** görgessen a **kezelőleképezések** szakasz.</span><span class="sxs-lookup"><span data-stu-id="cfe69-192">From the **Settings** blade select **Application Settings** and scroll to the **Handler mappings** section.</span></span> <span data-ttu-id="cfe69-193">Adja hozzá `*.php` a bővítmény mezőben, majd adja hozzá az elérési útját a `php-cgi.exe` végrehajtható.</span><span class="sxs-lookup"><span data-stu-id="cfe69-193">Add `*.php` to the Extension field and add the path to the `php-cgi.exe` executable.</span></span> <span data-ttu-id="cfe69-194">Ennél a PHP futtatókörnyezetben a `bin` directory alkalmazás gyökérkönyvtárában, az elérési út lesz `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span><span class="sxs-lookup"><span data-stu-id="cfe69-194">If you put your PHP runtime in the `bin` directory in the root of you application, the path will be `D:\home\site\wwwroot\bin\php\php-cgi.exe`.</span></span>
   
    ![Adja meg a kezelő meg leképezései][handler-mappings]
8. <span data-ttu-id="cfe69-196">Kattintson a **mentése** gomb tetején a **webalkalmazás-beállításainak** panelen.</span><span class="sxs-lookup"><span data-stu-id="cfe69-196">Click the **Save** button at the top of the **Web app settings** blade.</span></span>
   
    ![Konfigurációs beállítások mentéséhez][save-button]

<a name="composer" />

## <a name="how-to-enable-composer-automation-in-azure"></a><span data-ttu-id="cfe69-198">Hogyan: szerkesztő automatizálhatják az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="cfe69-198">How to: Enable Composer automation in Azure</span></span>
<span data-ttu-id="cfe69-199">Alapértelmezés szerint az App Service nem minden composer.json, ha nincs fiókja, a PHP-projektben.</span><span class="sxs-lookup"><span data-stu-id="cfe69-199">By default, App Service doesn't do anything with composer.json, if you have one in your PHP project.</span></span> <span data-ttu-id="cfe69-200">Ha [Git-telepítésének](app-service-deploy-local-git.md), engedélyezheti a feldolgozás során composer.json `git push` Composer bővítményt engedélyezésével.</span><span class="sxs-lookup"><span data-stu-id="cfe69-200">If you use [Git deployment](app-service-deploy-local-git.md), you can enable composer.json processing during `git push` by enabling the Composer extension.</span></span>

> [!NOTE]
> <span data-ttu-id="cfe69-201">Is [szerkesztő támogatja az App Service itt szavazzon](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span><span class="sxs-lookup"><span data-stu-id="cfe69-201">You can [vote for first-class Composer support in App Service here](https://feedback.azure.com/forums/169385-web-apps-formerly-websites/suggestions/6477437-first-class-support-for-composer-and-pip)!</span></span>
> 
> 

1. <span data-ttu-id="cfe69-202">A PHP a webalkalmazás-alkalmazás paneljén a [Azure-portálon](https://portal.azure.com), kattintson a **eszközök** > **bővítmények**.</span><span class="sxs-lookup"><span data-stu-id="cfe69-202">In your PHP web app's blade in the [Azure portal](https://portal.azure.com), click **Tools** > **Extensions**.</span></span>
   
    ![Az Azure portál beállítások panel szerkesztő automatizálására az Azure-ban](./media/web-sites-php-configure/composer-extension-settings.png)
2. <span data-ttu-id="cfe69-204">Kattintson a **Hozzáadás**, majd kattintson a **szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="cfe69-204">Click **Add**, then click **Composer**.</span></span>
   
    ![Adja hozzá az Azure-ban szerkesztő automatizálására Composer bővítményt](./media/web-sites-php-configure/composer-extension-add.png)
3. <span data-ttu-id="cfe69-206">Kattintson a **OK** jogi feltételek elfogadásának.</span><span class="sxs-lookup"><span data-stu-id="cfe69-206">Click **OK** to accept legal terms.</span></span> <span data-ttu-id="cfe69-207">Kattintson a **OK** újra a adja hozzá a kiterjesztést.</span><span class="sxs-lookup"><span data-stu-id="cfe69-207">Click **OK** again to add the extension.</span></span>
   
    <span data-ttu-id="cfe69-208">A **bővítményeket telepített** panel most megjelenik a Composer bővítményt.</span><span class="sxs-lookup"><span data-stu-id="cfe69-208">The **Installed extensions** blade will now show the Composer extension.</span></span>  
    <span data-ttu-id="cfe69-209">![Fogadja el a jogi feltételeket, hogy szerkesztő automatizálhatják az Azure-ban](./media/web-sites-php-configure/composer-extension-view.png)</span><span class="sxs-lookup"><span data-stu-id="cfe69-209">![Accept legal terms to enable Composer automation in Azure](./media/web-sites-php-configure/composer-extension-view.png)</span></span>
4. <span data-ttu-id="cfe69-210">Most, hajtsa végre `git add`, `git commit`, és `git push` , például az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="cfe69-210">Now, perform `git add`, `git commit`, and `git push` like in the previous section.</span></span> <span data-ttu-id="cfe69-211">Most láthatja, hogy szerkesztő telepíti a composer.json definiált függőségek.</span><span class="sxs-lookup"><span data-stu-id="cfe69-211">You'll now see that Composer is installing dependencies defined in composer.json.</span></span>
   
    ![Git-telepítés az szerkesztő Automation szolgáltatásban, az Azure-ban](./media/web-sites-php-configure/composer-extension-success.png)

## <a name="next-steps"></a><span data-ttu-id="cfe69-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cfe69-213">Next steps</span></span>
<span data-ttu-id="cfe69-214">További információkért lásd: a [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="cfe69-214">For more information, see the [PHP Developer Center](/develop/php/).</span></span>

> [!NOTE]
> <span data-ttu-id="cfe69-215">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="cfe69-215">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="cfe69-216">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="cfe69-216">No credit cards required; no commitments.</span></span>
> 
> 

<span data-ttu-id="cfe69-217">[ingyenes próbaverzió]: https://www.windowsazure.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="cfe69-217">[free trial]: https://www.windowsazure.com/pricing/free-trial/</span></span>
<span data-ttu-id="cfe69-218">[phpinfo()]: http://php.net/manual/en/function.phpinfo.php</span><span class="sxs-lookup"><span data-stu-id="cfe69-218">[phpinfo()]: http://php.net/manual/en/function.phpinfo.php</span></span>
[select-php-version]: ./media/web-sites-php-configure/select-php-version.png
<span data-ttu-id="cfe69-219">[php.ini irányelvek]: http://www.php.net/manual/en/ini.list.php</span><span class="sxs-lookup"><span data-stu-id="cfe69-219">[List of php.ini directives]: http://www.php.net/manual/en/ini.list.php</span></span>
<span data-ttu-id="cfe69-220">[. user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php</span><span class="sxs-lookup"><span data-stu-id="cfe69-220">[.user.ini]: http://www.php.net/manual/en/configuration.file.per-user.php</span></span>
<span data-ttu-id="cfe69-221">[ini_set()]: http://www.php.net/manual/en/function.ini-set.php</span><span class="sxs-lookup"><span data-stu-id="cfe69-221">[ini_set()]: http://www.php.net/manual/en/function.ini-set.php</span></span>
[application-settings]: ./media/web-sites-php-configure/application-settings.png
[settings-button]: ./media/web-sites-php-configure/settings-button.png
[save-button]: ./media/web-sites-php-configure/save-button.png
[php-extensions]: ./media/web-sites-php-configure/php-extensions.png
[handler-mappings]: ./media/web-sites-php-configure/handler-mappings.png
<span data-ttu-id="cfe69-222">[http://windows.php.net/download/]: http://windows.php.net/download/</span><span class="sxs-lookup"><span data-stu-id="cfe69-222">[http://windows.php.net/download/]: http://windows.php.net/download/</span></span>
<span data-ttu-id="cfe69-223">[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/</span><span class="sxs-lookup"><span data-stu-id="cfe69-223">[http://windows.php.net/downloads/releases/archives/]: http://windows.php.net/downloads/releases/archives/</span></span>
[SETPHPVERCLI]: ./media/web-sites-php-configure/ChangePHPVersion-XPlatCLI.png
[GETPHPVERCLI]: ./media/web-sites-php-configure/ShowPHPVersion-XplatCLI.png
[SETPHPVERPS]: ./media/web-sites-php-configure/ChangePHPVersion-PS.png
[GETPHPVERPS]: ./media/web-sites-php-configure/ShowPHPVersion-PS.png


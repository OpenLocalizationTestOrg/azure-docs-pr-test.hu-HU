---
title: "Nyílt forráskódú technológiák gyakori kérdések az Azure web apps |} Microsoft Docs"
description: "Adott válaszok nyílt forráskódú technológiákkal kapcsolatos gyakori kérdések az Azure App Service Web Apps szolgáltatása."
services: app-service\web
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: d37b53242c0b231d83425a59ecbe50216216a95b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a><span data-ttu-id="7324e-103">Gyakori kérdések az Azure Web Apps nyílt forráskódú technológiák</span><span class="sxs-lookup"><span data-stu-id="7324e-103">Open-source technologies FAQs for Web Apps in Azure</span></span>

<span data-ttu-id="7324e-104">Ez a cikk gyakran ismételt kérdések (GYIK) választ rendelkezik tudnivalók a nyílt forráskódú technológiák a [az Azure App Service Web Apps szolgáltatásának](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="7324e-104">This article has answers to frequently asked questions (FAQs) about issues with open-source technologies for the [Web Apps feature of Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a><span data-ttu-id="7324e-105">A ClearDB adatbázist nem működik.</span><span class="sxs-lookup"><span data-stu-id="7324e-105">My ClearDB database is down.</span></span> <span data-ttu-id="7324e-106">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="7324e-106">How do I resolve this?</span></span>

<span data-ttu-id="7324e-107">Adatbázissal kapcsolatos problémák esetén kérje [ClearDB támogatási](https://www.cleardb.com/developers/help/support).</span><span class="sxs-lookup"><span data-stu-id="7324e-107">For database-related issues, contact [ClearDB support](https://www.cleardb.com/developers/help/support).</span></span> 

<span data-ttu-id="7324e-108">A ClearDB kapcsolatos gyakori kérdésekre adott válaszok, lásd: [ClearDB – gyakori kérdések](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="7324e-108">For answers to common questions about ClearDB, see [ClearDB FAQs](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-isnt-my-cleardb-database-listed-in-the-portal"></a><span data-ttu-id="7324e-109">A ClearDB adatbázist miért nem szerepel a portálon?</span><span class="sxs-lookup"><span data-stu-id="7324e-109">Why isn't my ClearDB database listed in the portal?</span></span>

<span data-ttu-id="7324e-110">Ha a ClearDB-adatbázis létrehozása a [Azure-portálon](http://portal.azure.com/), az adatbázis nem jelenik meg a [a klasszikus Azure portálon](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="7324e-110">If you create a ClearDB database in the [Azure portal](http://portal.azure.com/), the database doesn't appear in the [Azure classic portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="7324e-111">Ez elkerülhető, hogy manuálisan társíthatja az adatbázis a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7324e-111">To work around this, you can manually link your database to the web app.</span></span>

<span data-ttu-id="7324e-112">Hasonló módon ha a ClearDB-adatbázis létrehozása a [a klasszikus Azure portálon](http://manage.windowsazure.com/), nem jelenik meg az adatbázis a [Azure-portálon](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="7324e-112">Similarly, if you create a ClearDB database in the [Azure classic portal](http://manage.windowsazure.com/),  you won't see your database in the [Azure portal](http://portal.azure.com/).</span></span> <span data-ttu-id="7324e-113">Ebben az esetben nincs megkerülő megoldás érhető el.</span><span class="sxs-lookup"><span data-stu-id="7324e-113">In this case, no workaround is available.</span></span> 

<span data-ttu-id="7324e-114">További információkért lásd: [ClearDB MySQL-adatbázisok – gyakori kérdések az Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="7324e-114">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a><span data-ttu-id="7324e-115">A ClearDB adatbázist miért nem volt át az előfizetést az áttelepítés során?</span><span class="sxs-lookup"><span data-stu-id="7324e-115">Why wasn't my ClearDB database migrated during my subscription migration?</span></span>

<span data-ttu-id="7324e-116">Erőforrás-áttelepítés végrehajtása előfizetések között, bizonyos korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="7324e-116">When you perform resource migration across subscriptions, some limitations apply.</span></span> <span data-ttu-id="7324e-117">A ClearDB MySQL-adatbázis egy külső szolgáltatás, és nem telepíti át az Azure-előfizetés az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="7324e-117">A ClearDB MySQL database is a third-party service and is not migrated during an Azure subscription migration.</span></span>

<span data-ttu-id="7324e-118">Ha nem Ön a MySQL-adatbázis áttelepítése az Azure-erőforrások áttelepítése előtt, előfordulhat, hogy a ClearDB MySQL-adatbázis nem elérhető.</span><span class="sxs-lookup"><span data-stu-id="7324e-118">If you don't manage the migration of your MySQL database before you migrate your Azure resources, your ClearDB MySQL database might be unavailable.</span></span> <span data-ttu-id="7324e-119">Ennek elkerülése érdekében először, manuálisan telepítse át a ClearDB adatbázist, majd utána áttelepíteni a webalkalmazás az Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="7324e-119">To avoid this, first, manually migrate your ClearDB database, and then migrate the Azure subscription for your web app.</span></span>

<span data-ttu-id="7324e-120">További információkért lásd: [ClearDB MySQL-adatbázisok – gyakori kérdések az Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="7324e-120">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="how-do-i-turn-on-php-logging-to-troubleshoot-php-issues"></a><span data-ttu-id="7324e-121">Hogyan PHP PHP kapcsolatos problémák elhárítása naplózás bekapcsolása?</span><span class="sxs-lookup"><span data-stu-id="7324e-121">How do I turn on PHP logging to troubleshoot PHP issues?</span></span>

<span data-ttu-id="7324e-122">PHP-naplózás bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="7324e-122">To turn on PHP logging:</span></span>

1. <span data-ttu-id="7324e-123">Jelentkezzen be a [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="7324e-123">Sign in to your [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
2. <span data-ttu-id="7324e-124">A felső menüben válassza ki **Debug konzol** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="7324e-124">In the top menu, select **Debug Console** > **CMD**.</span></span>
3. <span data-ttu-id="7324e-125">Válassza ki a **hely** mappát.</span><span class="sxs-lookup"><span data-stu-id="7324e-125">Select the **Site** folder.</span></span>
4. <span data-ttu-id="7324e-126">Válassza ki a **wwwroot** mappát.</span><span class="sxs-lookup"><span data-stu-id="7324e-126">Select the **wwwroot** folder.</span></span>
5. <span data-ttu-id="7324e-127">Válassza ki a  **+**  ikonra, és válassza **új fájl**.</span><span class="sxs-lookup"><span data-stu-id="7324e-127">Select the **+** icon, and then select **New File**.</span></span>
6. <span data-ttu-id="7324e-128">A fájl neve **. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="7324e-128">Set the file name to **.user.ini**.</span></span>
7. <span data-ttu-id="7324e-129">Jelölje be a ceruza ikonra a **. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="7324e-129">Select the pencil icon next to **.user.ini**.</span></span>
8. <span data-ttu-id="7324e-130">A fájlban adja hozzá ezt a kódot:`log_errors=on`</span><span class="sxs-lookup"><span data-stu-id="7324e-130">In the file, add this code: `log_errors=on`</span></span>
9. <span data-ttu-id="7324e-131">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="7324e-131">Select **Save**.</span></span>
10. <span data-ttu-id="7324e-132">Jelölje be a ceruza ikonra a **wp-config.php**.</span><span class="sxs-lookup"><span data-stu-id="7324e-132">Select the pencil icon next to **wp-config.php**.</span></span>
11. <span data-ttu-id="7324e-133">Módosítsa a szöveg a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="7324e-133">Change the text to the following code:</span></span>
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging to /wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings to screendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors to screenini_set('display_errors', 0);
   ```
12. <span data-ttu-id="7324e-134">Az Azure portálon, a webes alkalmazás menü indítsa újra a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7324e-134">In the Azure portal, in the web app menu, restart your web app.</span></span>

<span data-ttu-id="7324e-135">További információkért lásd: [engedélyezése WordPress hibanaplókat](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="7324e-135">For more information, see [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a><span data-ttu-id="7324e-136">Hogyan Python-alkalmazások hibáinak jelentkezni az App Service szolgáltatásban üzemeltetett alkalmazásokat?</span><span class="sxs-lookup"><span data-stu-id="7324e-136">How do I log Python application errors in apps that are hosted in App Service?</span></span>

<span data-ttu-id="7324e-137">Rögzítheti a Python-alkalmazások hibáinak:</span><span class="sxs-lookup"><span data-stu-id="7324e-137">To capture Python application errors:</span></span>

1. <span data-ttu-id="7324e-138">Válassza ki az Azure portálon, a web app alkalmazásban **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="7324e-138">In the Azure portal, in your web app, select **Settings**.</span></span>
2. <span data-ttu-id="7324e-139">Az a **beállítások** lapon jelölje be **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7324e-139">On the **Settings** tab, select **Application settings**.</span></span>
3. <span data-ttu-id="7324e-140">A **Alkalmazásbeállítások**, adja meg a következő kulcs/érték pár:</span><span class="sxs-lookup"><span data-stu-id="7324e-140">Under **App settings**, enter the following key/value pair:</span></span>
    * <span data-ttu-id="7324e-141">Kulcs: WSGI_LOG</span><span class="sxs-lookup"><span data-stu-id="7324e-141">Key : WSGI_LOG</span></span>
    * <span data-ttu-id="7324e-142">Érték: D:\home\site\wwwroot\logs.txt (adja meg a kívánt fájlnevet)</span><span class="sxs-lookup"><span data-stu-id="7324e-142">Value : D:\home\site\wwwroot\logs.txt (enter your choice of file name)</span></span>

<span data-ttu-id="7324e-143">Meg kell jelennie a logs.txt fájlban a wwwroot hibáit.</span><span class="sxs-lookup"><span data-stu-id="7324e-143">You should now see errors in the logs.txt file in the wwwroot folder.</span></span>

## <a name="how-do-i-change-the-version-of-the-nodejs-application-that-is-hosted-in-app-service"></a><span data-ttu-id="7324e-144">A Node.js-alkalmazás az App Service-ben futtatott verziójának módosítása?</span><span class="sxs-lookup"><span data-stu-id="7324e-144">How do I change the version of the Node.js application that is hosted in App Service?</span></span>

<span data-ttu-id="7324e-145">Ha módosítani szeretné a Node.js-alkalmazás verziója, az alábbi lehetőségek egyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="7324e-145">To change the version of the Node.js application, you can use one of the following options:</span></span>

*   <span data-ttu-id="7324e-146">Az Azure-portálon használni **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7324e-146">In the Azure portal, use **App settings**.</span></span>
    1. <span data-ttu-id="7324e-147">Az Azure-portálon keresse meg a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7324e-147">In the Azure portal, go to your web app.</span></span>
    2. <span data-ttu-id="7324e-148">Az a **beállítások** panelen válassza **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7324e-148">On the **Settings** blade, select **Application settings**.</span></span>
    3. <span data-ttu-id="7324e-149">A **Alkalmazásbeállítások**, WEBSITE_NODE_DEFAULT_VERSION is megadható a kulcsot, valamint a Node.js értékeként kívánt verzióját.</span><span class="sxs-lookup"><span data-stu-id="7324e-149">In **App settings**, you can include WEBSITE_NODE_DEFAULT_VERSION as the key, and the version of Node.js you want as the value.</span></span>
    4. <span data-ttu-id="7324e-150">Lépjen a [Kudu konzol](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="7324e-150">Go to your [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
    5. <span data-ttu-id="7324e-151">A Node.js-verzió ellenőrzéséhez írja be a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7324e-151">To check the Node.js version, enter the following command:</span></span>  
   ```
   node -v
   ```
*   <span data-ttu-id="7324e-152">Módosítsa a iisnode.yml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7324e-152">Modify the iisnode.yml file.</span></span> <span data-ttu-id="7324e-153">A Node.js-verzió az iisnode.yml fájlt módosítása csak állítja be a futtatási környezetet, hogy az iisnode használja.</span><span class="sxs-lookup"><span data-stu-id="7324e-153">Changing the Node.js version in the iisnode.yml file only sets the runtime environment that iisnode uses.</span></span> <span data-ttu-id="7324e-154">A Kudu cmd, míg mások továbbra is használhatják a Node.js-verzió az beállított **Alkalmazásbeállítások** az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7324e-154">Your Kudu cmd and others still use the Node.js version that is set in **App settings** in the Azure portal.</span></span>

    <span data-ttu-id="7324e-155">Állítsa be manuálisan a iisnode.yml fájlt, hozzon létre egy iisnode.yml fájlt az alkalmazás gyökérmappájában lévő mappának.</span><span class="sxs-lookup"><span data-stu-id="7324e-155">To set the iisnode.yml manually, create an iisnode.yml file in your app root folder.</span></span> <span data-ttu-id="7324e-156">A fájlban a következő sort a következők:</span><span class="sxs-lookup"><span data-stu-id="7324e-156">In the file, include the following line:</span></span>
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   <span data-ttu-id="7324e-157">Be iisnode.yml fájlt package.json forrás vezérlő üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="7324e-157">Set the iisnode.yml file by using package.json during source control deployment.</span></span>
    <span data-ttu-id="7324e-158">Az Azure forrás vezérlő telepítési folyamat a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="7324e-158">The Azure source control deployment process involves the following steps:</span></span>
    1. <span data-ttu-id="7324e-159">Tartalom áthelyezése az Azure-webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7324e-159">Moves content to the Azure web app.</span></span>
    2. <span data-ttu-id="7324e-160">Egy alapértelmezett telepítési parancsfájl hoz létre, ha nincs a webes alkalmazás gyökérmappájában (Deploy.cmd fájl, .deployment fájlok) ilyen.</span><span class="sxs-lookup"><span data-stu-id="7324e-160">Creates a default deployment script, if there isn’t one (deploy.cmd, .deployment files) in the web app root folder.</span></span>
    3. <span data-ttu-id="7324e-161">A telepítési parancsfájlt, amelyben létrehoz egy iisnode.yml fájlt Ha, említse meg a package.json fájlban a Node.js-verzió fut > motor`"engines": {"node": "5.9.1","npm": "3.7.3"}`</span><span class="sxs-lookup"><span data-stu-id="7324e-161">Runs a deployment script in which it creates an iisnode.yml file if you mention the Node.js version in the package.json file > engine `"engines": {"node": "5.9.1","npm": "3.7.3"}`</span></span>
    4. <span data-ttu-id="7324e-162">A iisnode.yml fájlt a következő kódsort rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="7324e-162">The iisnode.yml file has the following line of code:</span></span>
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-the-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a><span data-ttu-id="7324e-163">A WordPress alkalmazás az App Service-ben üzemeltetett "Error egy adatbázis-kapcsolatot létesít." hibaüzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7324e-163">I see the message "Error establishing a database connection" in my WordPress app that's hosted in App Service.</span></span> <span data-ttu-id="7324e-164">Hogyan hibaelhárítása Ez?</span><span class="sxs-lookup"><span data-stu-id="7324e-164">How do I troubleshoot this?</span></span>

<span data-ttu-id="7324e-165">Ha ez a hiba jelenik meg az Azure WordPress-alkalmazás, php_errors.log és debug.log, hogy teljes a lépések részletes leírást talál [engedélyezése WordPress hibanaplókat](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="7324e-165">If you see this error in your Azure WordPress app, to enable php_errors.log and debug.log, complete the steps detailed in [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

<span data-ttu-id="7324e-166">A naplókat, ha engedélyezve vannak Reprodukálja a hibát, és ellenőrizze a naplókat, ha elfogy kapcsolatok megjelenítéséhez:</span><span class="sxs-lookup"><span data-stu-id="7324e-166">When the logs are enabled, reproduce the error, and then check the logs to see if you are running out of connections:</span></span>
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded the ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

<span data-ttu-id="7324e-167">Ha ez a hiba a debug.log vagy php_errors.log fájlok jelenik meg, az alkalmazás túllépte a kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="7324e-167">If you see this error in your debug.log or php_errors.log files, your app is exceeding the number of connections.</span></span> <span data-ttu-id="7324e-168">Ha a ClearDB, ellenőrizze a rendelkezésre álló kapcsolatok száma a [service-csomag](https://www.cleardb.com/pricing.view).</span><span class="sxs-lookup"><span data-stu-id="7324e-168">If you’re hosting on ClearDB, verify the number of connections that are available in your [service plan](https://www.cleardb.com/pricing.view).</span></span>

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a><span data-ttu-id="7324e-169">Hogyan debug a Node.js-alkalmazás az App Service-ben üzemeltetett?</span><span class="sxs-lookup"><span data-stu-id="7324e-169">How do I debug a Node.js app that's hosted in App Service?</span></span>

1.  <span data-ttu-id="7324e-170">Lépjen a [Kudu konzol](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span><span class="sxs-lookup"><span data-stu-id="7324e-170">Go to your [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span></span>
2.  <span data-ttu-id="7324e-171">Nyissa meg az alkalmazás naplók mappába (D:\home\LogFiles\Application).</span><span class="sxs-lookup"><span data-stu-id="7324e-171">Go to your application logs folder (D:\home\LogFiles\Application).</span></span>
3.  <span data-ttu-id="7324e-172">A logging_errors.txt fájlban ellenőrizze a tartalom.</span><span class="sxs-lookup"><span data-stu-id="7324e-172">In the logging_errors.txt file, check for content.</span></span>

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a><span data-ttu-id="7324e-173">Natív Python-modulok telepítése egy App Service webalkalmazás vagy API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7324e-173">How do I install native Python modules in an App Service web app or API app?</span></span>

<span data-ttu-id="7324e-174">Néhány csomag Előfordulhat, hogy telepítse az Azure-ban a pip használatával.</span><span class="sxs-lookup"><span data-stu-id="7324e-174">Some packages might not install by using pip in Azure.</span></span> <span data-ttu-id="7324e-175">A csomag nem feltétlenül érhető el a Python-Csomagindexben, vagy lehet, hogy egy fordító szükséges (a fordító nem érhető el a számítógépen, hogy fut a webes alkalmazás az App Service szolgáltatásban).</span><span class="sxs-lookup"><span data-stu-id="7324e-175">The package might not be available on the Python Package Index, or a compiler might be required (a compiler is not available on the computer that is running the web app in App Service).</span></span> <span data-ttu-id="7324e-176">Az App Service web apps és API-alkalmazások natív modulok telepítésével kapcsolatos információkért lásd: [telepítése Python-modulok az App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span><span class="sxs-lookup"><span data-stu-id="7324e-176">For information about installing native modules in App Service web apps and API apps, see [Install Python modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span></span>

## <a name="how-do-i-deploy-a-django-app-to-app-service-by-using-git-and-the-new-version-of-python"></a><span data-ttu-id="7324e-177">Hogyan telepíthetem a Django-alkalmazást az App Service a Git szoftver, a Python új verziójának használatával?</span><span class="sxs-lookup"><span data-stu-id="7324e-177">How do I deploy a Django app to App Service by using Git and the new version of Python?</span></span>

<span data-ttu-id="7324e-178">A Django telepítésével kapcsolatos információkért lásd: [központi telepítése egy Django-alkalmazást az App Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span><span class="sxs-lookup"><span data-stu-id="7324e-178">For information about installing Django, see [Deploying a Django app to App Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span></span>

## <a name="where-are-the-tomcat-log-files-located"></a><span data-ttu-id="7324e-179">A Tomcat naplófájlok helyét?</span><span class="sxs-lookup"><span data-stu-id="7324e-179">Where are the Tomcat log files located?</span></span>

<span data-ttu-id="7324e-180">Az Azure piactér és egyéni telepítésekhez:</span><span class="sxs-lookup"><span data-stu-id="7324e-180">For Azure Marketplace and custom deployments:</span></span>

* <span data-ttu-id="7324e-181">Mappájának helye: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span><span class="sxs-lookup"><span data-stu-id="7324e-181">Folder location: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span></span>
* <span data-ttu-id="7324e-182">Fontos fájlok:</span><span class="sxs-lookup"><span data-stu-id="7324e-182">Files of interest:</span></span>
    * <span data-ttu-id="7324e-183">catalina. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-183">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-184">gazdagép-kezelő. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-184">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-185">localhost. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-185">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-186">a Manager objektum. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-186">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-187">site_access_log. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-187">site_access_log.*yyyy-mm-dd*.log</span></span>


<span data-ttu-id="7324e-188">A portál **Alkalmazásbeállítások** központi telepítések:</span><span class="sxs-lookup"><span data-stu-id="7324e-188">For portal **App settings** deployments:</span></span>

* <span data-ttu-id="7324e-189">Mappájának helye: D:\home\LogFiles</span><span class="sxs-lookup"><span data-stu-id="7324e-189">Folder location: D:\home\LogFiles</span></span>
* <span data-ttu-id="7324e-190">Fontos fájlok:</span><span class="sxs-lookup"><span data-stu-id="7324e-190">Files of interest:</span></span>
    * <span data-ttu-id="7324e-191">catalina. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-191">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-192">gazdagép-kezelő. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-192">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-193">localhost. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-193">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-194">a Manager objektum. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-194">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="7324e-195">site_access_log. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="7324e-195">site_access_log.*yyyy-mm-dd*.log</span></span>

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a><span data-ttu-id="7324e-196">Hogyan hibáinak elhárítása JDBC kapcsolat hibái?</span><span class="sxs-lookup"><span data-stu-id="7324e-196">How do I troubleshoot JDBC driver connection errors?</span></span>

<span data-ttu-id="7324e-197">A következő üzenetet láthatja a Tomcat naplókban:</span><span class="sxs-lookup"><span data-stu-id="7324e-197">You might see the following message in your Tomcat logs:</span></span>

```
The web application[ROOT] registered the JDBC driver [com.mysql.jdbc.Driver] but failed to unregister it when the web application was stopped. To prevent a memory leak,the JDBC Driver has been forcibly unregistered
```

<span data-ttu-id="7324e-198">A hiba elhárításához:</span><span class="sxs-lookup"><span data-stu-id="7324e-198">To resolve the error:</span></span>

1. <span data-ttu-id="7324e-199">Távolítsa el a sqljdbc*.jar fájlt az alkalmazás/lib mappából.</span><span class="sxs-lookup"><span data-stu-id="7324e-199">Remove the sqljdbc*.jar file from your app/lib folder.</span></span>
2. <span data-ttu-id="7324e-200">Ha az egyéni Tomcat- vagy Azure piactér Tomcat webkiszolgáló használ, másolja a .jar fájlt a Tomcat lib mappába.</span><span class="sxs-lookup"><span data-stu-id="7324e-200">If you are using the custom Tomcat or Azure Marketplace Tomcat web server, copy this .jar file to the Tomcat lib folder.</span></span>
3. <span data-ttu-id="7324e-201">Ha engedélyezi az Azure portálról Java (válasszon **Java 1.8** > **Tomcat kiszolgálót**), másolja a sqljdbc.* jar-fájlra a mappában, az alkalmazás párhuzamos.</span><span class="sxs-lookup"><span data-stu-id="7324e-201">If you are enabling Java from the Azure portal (select **Java 1.8** > **Tomcat server**), copy the sqljdbc.* jar file in the folder that's parallel to your app.</span></span> <span data-ttu-id="7324e-202">Ezt követően adja hozzá a következő classpath a web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="7324e-202">Then, add the following classpath setting to the web.config file:</span></span>

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path to the sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-to-copy-live-log-files"></a><span data-ttu-id="7324e-203">Miért látom hibák, amikor élő naplófájlok másolása?</span><span class="sxs-lookup"><span data-stu-id="7324e-203">Why do I see errors when I attempt to copy live log files?</span></span>

<span data-ttu-id="7324e-204">Ha megpróbálja élő naplófájlok Java-alkalmazások (például Tomcat) másolja, láthatja az FTP-hiba:</span><span class="sxs-lookup"><span data-stu-id="7324e-204">If you try to copy live log files for a Java app (for example, Tomcat), you might see this FTP error:</span></span>

```
Error transferring file [filename] Copying files from remote side failed.
    
The process cannot access the file because it is being used by another process.
```

<span data-ttu-id="7324e-205">A hibaüzenet a következő változhat, attól függően, hogy az FTP-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="7324e-205">The error message might vary, depending on the FTP client.</span></span>

<span data-ttu-id="7324e-206">Minden Java-alkalmazások rendelkeznek a zárolási problémát.</span><span class="sxs-lookup"><span data-stu-id="7324e-206">All Java apps have this locking issue.</span></span> <span data-ttu-id="7324e-207">Csak a Kudu támogatja, a fájl letöltése, az alkalmazás futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="7324e-207">Only Kudu supports downloading this file while the app is running.</span></span>

<span data-ttu-id="7324e-208">Az alkalmazás leállítása lehetővé teszi az FTP-hozzáférést a fájlokhoz.</span><span class="sxs-lookup"><span data-stu-id="7324e-208">Stopping the app allows FTP access to these files.</span></span>

<span data-ttu-id="7324e-209">Egy másik megoldás, a webjobs-feladat ütemezés szerint fut, és másolja át ezeket a fájlokat egy másik címtárba írható.</span><span class="sxs-lookup"><span data-stu-id="7324e-209">Another workaround is to write a WebJob that runs on a schedule and copies these files to a different directory.</span></span> <span data-ttu-id="7324e-210">Egy minta-projekt tekintse meg a [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projekt.</span><span class="sxs-lookup"><span data-stu-id="7324e-210">For a sample project, see the [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.</span></span>

## <a name="where-do-i-find-the-log-files-for-jetty"></a><span data-ttu-id="7324e-211">Hol található a naplófájlok a Jetty?</span><span class="sxs-lookup"><span data-stu-id="7324e-211">Where do I find the log files for Jetty?</span></span>

<span data-ttu-id="7324e-212">Piactér-vagy egyéni telepítések esetén a naplófájl a D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs mappában van.</span><span class="sxs-lookup"><span data-stu-id="7324e-212">For Marketplace and custom deployments, the log file is in the D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs folder.</span></span> <span data-ttu-id="7324e-213">Vegye figyelembe, hogy a mappa helyét a Jetty használ verziójától függ.</span><span class="sxs-lookup"><span data-stu-id="7324e-213">Note that the folder location depends on the version of Jetty you are using.</span></span> <span data-ttu-id="7324e-214">Például az elérési út az itt megadott a Jetty 9.1.2.</span><span class="sxs-lookup"><span data-stu-id="7324e-214">For example, the path provided here is for Jetty 9.1.2.</span></span> <span data-ttu-id="7324e-215">Keresse meg jetty_*YYYY_MM_DD*. stderrout.log.</span><span class="sxs-lookup"><span data-stu-id="7324e-215">Look for jetty_*YYYY_MM_DD*.stderrout.log.</span></span>

<span data-ttu-id="7324e-216">A portál Alkalmazásbeállítás telepítések esetében a naplófájl D:\home\LogFiles van.</span><span class="sxs-lookup"><span data-stu-id="7324e-216">For portal App Setting deployments, the log file is in D:\home\LogFiles.</span></span> <span data-ttu-id="7324e-217">Keresse meg jetty_*YYYY_MM_DD*. stderrout.log</span><span class="sxs-lookup"><span data-stu-id="7324e-217">Look for jetty_*YYYY_MM_DD*.stderrout.log</span></span>

## <a name="can-i-send-email-from-my-azure-web-app"></a><span data-ttu-id="7324e-218">Küldhetek e-mailt a saját Azure-webalkalmazás?</span><span class="sxs-lookup"><span data-stu-id="7324e-218">Can I send email from my Azure web app?</span></span>

<span data-ttu-id="7324e-219">App Service beépített e-mail-szolgáltatás nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7324e-219">App Service doesn't have a built-in email feature.</span></span> <span data-ttu-id="7324e-220">Az e-mailek küldése az alkalmazásból, néhány jó alternatíva ez című [Stack Overflow vitafórum](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span><span class="sxs-lookup"><span data-stu-id="7324e-220">For some good alternatives for sending email from your app, see this [Stack Overflow discussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span></span>

## <a name="why-does-my-wordpress-site-redirect-to-another-url"></a><span data-ttu-id="7324e-221">Miért nem saját WordPress-webhely átirányítja egy másik URL-CÍMRE?</span><span class="sxs-lookup"><span data-stu-id="7324e-221">Why does my WordPress site redirect to another URL?</span></span>

<span data-ttu-id="7324e-222">Ha nemrég áttelepítette az Azure-ba, WordPress előfordulhat, hogy átirányítja a régi tartomány URL-CÍMRE.</span><span class="sxs-lookup"><span data-stu-id="7324e-222">If you have recently migrated to Azure, WordPress might redirect to the old domain URL.</span></span> <span data-ttu-id="7324e-223">Ezt a MySQL-adatbázis beállítása okozza.</span><span class="sxs-lookup"><span data-stu-id="7324e-223">This is caused by a setting in the MySQL database.</span></span>

<span data-ttu-id="7324e-224">WordPress ismerős + az Azure hely bővítménye által az átirányítási URL-címet közvetlenül az adatbázis frissítéséhez használhatja.</span><span class="sxs-lookup"><span data-stu-id="7324e-224">WordPress Buddy+ is an Azure Site Extension that you can use to update the redirection URL directly in the database.</span></span> <span data-ttu-id="7324e-225">WordPress ismerős + használatával kapcsolatos további információkért lásd: [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="7324e-225">For more information about using WordPress Buddy+, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

<span data-ttu-id="7324e-226">Azt is megteheti, ha szeretné-e manuálisan frissítse a átirányítási URL-címet az SQL-lekérdezések vagy PHPMyAdmin, lásd: [WordPress: rossz URL-cím átirányítása](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span><span class="sxs-lookup"><span data-stu-id="7324e-226">Alternatively, if you prefer to manually update the redirection URL by using SQL queries or PHPMyAdmin, see [WordPress: Redirecting to wrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span></span>

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a><span data-ttu-id="7324e-227">A WordPress bejelentkezési jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="7324e-227">How do I change my WordPress sign-in password?</span></span>

<span data-ttu-id="7324e-228">Ha elfelejtette a jelszavát WordPress-bejelentkezés, segítségével WordPress ismerős + a frissítést.</span><span class="sxs-lookup"><span data-stu-id="7324e-228">If you have forgotten your WordPress sign-in password, you can use WordPress Buddy+ to update it.</span></span> <span data-ttu-id="7324e-229">A jelszó alaphelyzetbe állításához, a WordPress ismerős + Azure hely-kiterjesztés telepítése, és fejezze be a leírt lépéseket [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="7324e-229">To reset your password, install the WordPress Buddy+ Azure Site Extension, and then complete the steps described in [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="i-cant-sign-in-to-wordpress-how-do-i-resolve-this"></a><span data-ttu-id="7324e-230">Nem lehet bejelentkezni WordPress.</span><span class="sxs-lookup"><span data-stu-id="7324e-230">I can't sign in to WordPress.</span></span> <span data-ttu-id="7324e-231">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="7324e-231">How do I resolve this?</span></span>

<span data-ttu-id="7324e-232">Ha nemrég a beépülő modul telepítése után WordPress tévedéssel, lehetséges, hogy egy hibás beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="7324e-232">If you find yourself locked out of WordPress after recently installing a plugin, you might have a faulty plugin.</span></span> <span data-ttu-id="7324e-233">WordPress ismerős +, amelyek segítségével Azure hely bővítmény letiltása a WordPress beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="7324e-233">WordPress Buddy+ is an Azure Site Extension that can help you disable plugins in WordPress.</span></span> <span data-ttu-id="7324e-234">További információkért lásd: [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="7324e-234">For more information, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="how-do-i-migrate-my-wordpress-database"></a><span data-ttu-id="7324e-235">Hogyan telepítse át a WordPress adatbázist?</span><span class="sxs-lookup"><span data-stu-id="7324e-235">How do I migrate my WordPress database?</span></span>

<span data-ttu-id="7324e-236">Amely csatlakozik a WordPress webhely MySQL-adatbázis áttelepítéséhez több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="7324e-236">You have multiple options for migrating the MySQL database that's connected to your WordPress website:</span></span>

* <span data-ttu-id="7324e-237">A fejlesztők: Használja a [parancssort vagy PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span><span class="sxs-lookup"><span data-stu-id="7324e-237">Developers: Use the [command prompt or PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span></span>
* <span data-ttu-id="7324e-238">Nem-Fejlesztők: [WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span><span class="sxs-lookup"><span data-stu-id="7324e-238">Non-developers: Use [WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span></span>

## <a name="how-do-i-help-make-wordpress-more-secure"></a><span data-ttu-id="7324e-239">Hogyan segít, WordPress biztonságosabbá?</span><span class="sxs-lookup"><span data-stu-id="7324e-239">How do I help make WordPress more secure?</span></span>

<span data-ttu-id="7324e-240">Ajánlott biztonsági eljárások a WordPress, lásd: [ajánlott eljárások az Azure-ban WordPress biztonsági](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span><span class="sxs-lookup"><span data-stu-id="7324e-240">To learn about security best practices for WordPress, see [Best practices for WordPress security in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span></span>

## <a name="i-am-trying-to-use-phpmyadmin-and-i-see-the-message-access-denied-how-do-i-resolve-this"></a><span data-ttu-id="7324e-241">Kísérlet PHPMyAdmin használja, és "Hozzáférés megtagadva." a következő üzenet jelenik meg</span><span class="sxs-lookup"><span data-stu-id="7324e-241">I am trying to use PHPMyAdmin, and I see the message “Access denied.”</span></span> <span data-ttu-id="7324e-242">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="7324e-242">How do I resolve this?</span></span>

<span data-ttu-id="7324e-243">A probléma tapasztalhat, ha a MySQL alkalmazásbeli funkció még nem fut. Ez App Service-példányban.</span><span class="sxs-lookup"><span data-stu-id="7324e-243">You might experience this issue if the MySQL in-app feature isn't running yet in this App Service instance.</span></span> <span data-ttu-id="7324e-244">A probléma megoldása érdekében próbálkozzon a webhely eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7324e-244">To resolve the issue, try to access your website.</span></span> <span data-ttu-id="7324e-245">A szükséges eljárásokat, beleértve a MySQL alkalmazáson belüli folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="7324e-245">This starts the required processes, including the MySQL in-app process.</span></span> <span data-ttu-id="7324e-246">Annak ellenőrzése, hogy MySQL alkalmazásbeli fut-e, a Process Explorert, hogy mysqld.exe jelleggel szerepel-e.</span><span class="sxs-lookup"><span data-stu-id="7324e-246">To verify that MySQL in-app is running, in Process Explorer, ensure that mysqld.exe is listed in the processes.</span></span>

<span data-ttu-id="7324e-247">Miután meggyőződött arról, hogy MySQL alkalmazásbeli fut-e, próbálja meg PHPMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="7324e-247">After you ensure that MySQL in-app is running, try to use PHPMyAdmin.</span></span>

## <a name="i-get-an-http-403-error-when-i-try-to-import-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a><span data-ttu-id="7324e-248">A HTTP 403-as hiba jelenik meg: való importálására vagy exportálására alkalmazásbeli MySQL adatbázis PHPMyadmin használatával.</span><span class="sxs-lookup"><span data-stu-id="7324e-248">I get an HTTP 403 error when I try to import or export my MySQL in-app database by using PHPMyadmin.</span></span> <span data-ttu-id="7324e-249">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="7324e-249">How do I resolve this?</span></span>

<span data-ttu-id="7324e-250">Ha a Látványelem egy régebbi verzióját használja, akkor léptek fel egy ismert hiba.</span><span class="sxs-lookup"><span data-stu-id="7324e-250">If you are using an older version of Chrome, you might be experiencing a known bug.</span></span> <span data-ttu-id="7324e-251">A probléma megoldása érdekében váltson a Chrome egy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="7324e-251">To resolve the issue, upgrade to a newer version of Chrome.</span></span> <span data-ttu-id="7324e-252">Is próbálkozzon másik böngészővel, például az Internet Explorer vagy Edge, ha a probléma nem történik meg.</span><span class="sxs-lookup"><span data-stu-id="7324e-252">Also try using a different browser, like Internet Explorer or Edge, where the issue does not occur.</span></span>

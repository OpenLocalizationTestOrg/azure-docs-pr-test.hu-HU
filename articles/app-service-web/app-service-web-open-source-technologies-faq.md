---
title: "aaaOpen-forrás technológiák gyakori kérdések az Azure web apps |} Microsoft Docs"
description: "Az Azure App Service Web Apps szolgáltatása hello nyílt forráskódú technológiákkal kapcsolatos kérdések válaszok toofrequently beolvasása."
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
ms.openlocfilehash: 35cff4f322859d25972747cf55aa7c4316381a51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="open-source-technologies-faqs-for-web-apps-in-azure"></a><span data-ttu-id="2330f-103">Gyakori kérdések az Azure Web Apps nyílt forráskódú technológiák</span><span class="sxs-lookup"><span data-stu-id="2330f-103">Open-source technologies FAQs for Web Apps in Azure</span></span>

<span data-ttu-id="2330f-104">Ez a cikk kérdések (GYIK) válaszok toofrequently rendelkezik hello nyílt forráskódú technológiák összefüggő problémákkal kapcsolatos [az Azure App Service Web Apps szolgáltatásának](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="2330f-104">This article has answers toofrequently asked questions (FAQs) about issues with open-source technologies for hello [Web Apps feature of Azure App Service](https://azure.microsoft.com/services/app-service/web/).</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-cleardb-database-is-down-how-do-i-resolve-this"></a><span data-ttu-id="2330f-105">A ClearDB adatbázist nem működik.</span><span class="sxs-lookup"><span data-stu-id="2330f-105">My ClearDB database is down.</span></span> <span data-ttu-id="2330f-106">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="2330f-106">How do I resolve this?</span></span>

<span data-ttu-id="2330f-107">Adatbázissal kapcsolatos problémák esetén kérje [ClearDB támogatási](https://www.cleardb.com/developers/help/support).</span><span class="sxs-lookup"><span data-stu-id="2330f-107">For database-related issues, contact [ClearDB support](https://www.cleardb.com/developers/help/support).</span></span> 

<span data-ttu-id="2330f-108">Válaszok toocommon kérdésekre ClearDB, lásd: [ClearDB – gyakori kérdések](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="2330f-108">For answers toocommon questions about ClearDB, see [ClearDB FAQs](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-isnt-my-cleardb-database-listed-in-hello-portal"></a><span data-ttu-id="2330f-109">A ClearDB adatbázist miért nem szerepel a hello portálon?</span><span class="sxs-lookup"><span data-stu-id="2330f-109">Why isn't my ClearDB database listed in hello portal?</span></span>

<span data-ttu-id="2330f-110">Ha egy ClearDB adatbázist hoz létre a hello [Azure-portálon](http://portal.azure.com/), hello adatbázis nem jelenik meg a hello [a klasszikus Azure portálon](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="2330f-110">If you create a ClearDB database in hello [Azure portal](http://portal.azure.com/), hello database doesn't appear in hello [Azure classic portal](http://manage.windowsazure.com/).</span></span> <span data-ttu-id="2330f-111">Ez elkerülhető toowork, manuálisan társíthatja az adatbázis toohello webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2330f-111">toowork around this, you can manually link your database toohello web app.</span></span>

<span data-ttu-id="2330f-112">Hasonló módon ha hello egy ClearDB adatbázist hoz létre [a klasszikus Azure portálon](http://manage.windowsazure.com/), nem jelenik meg az adatbázis a hello [Azure-portálon](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2330f-112">Similarly, if you create a ClearDB database in hello [Azure classic portal](http://manage.windowsazure.com/),  you won't see your database in hello [Azure portal](http://portal.azure.com/).</span></span> <span data-ttu-id="2330f-113">Ebben az esetben nincs megkerülő megoldás érhető el.</span><span class="sxs-lookup"><span data-stu-id="2330f-113">In this case, no workaround is available.</span></span> 

<span data-ttu-id="2330f-114">További információkért lásd: [ClearDB MySQL-adatbázisok – gyakori kérdések az Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="2330f-114">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="why-wasnt-my-cleardb-database-migrated-during-my-subscription-migration"></a><span data-ttu-id="2330f-115">A ClearDB adatbázist miért nem volt át az előfizetést az áttelepítés során?</span><span class="sxs-lookup"><span data-stu-id="2330f-115">Why wasn't my ClearDB database migrated during my subscription migration?</span></span>

<span data-ttu-id="2330f-116">Erőforrás-áttelepítés végrehajtása előfizetések között, bizonyos korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="2330f-116">When you perform resource migration across subscriptions, some limitations apply.</span></span> <span data-ttu-id="2330f-117">A ClearDB MySQL-adatbázis egy külső szolgáltatás, és nem telepíti át az Azure-előfizetés az áttelepítés során.</span><span class="sxs-lookup"><span data-stu-id="2330f-117">A ClearDB MySQL database is a third-party service and is not migrated during an Azure subscription migration.</span></span>

<span data-ttu-id="2330f-118">Ha nem Ön hello a MySQL-adatbázis áttelepítése az Azure-erőforrások áttelepítése előtt, előfordulhat, hogy a ClearDB MySQL-adatbázis nem elérhető.</span><span class="sxs-lookup"><span data-stu-id="2330f-118">If you don't manage hello migration of your MySQL database before you migrate your Azure resources, your ClearDB MySQL database might be unavailable.</span></span> <span data-ttu-id="2330f-119">tooavoid ez, először, manuálisan telepítse át a ClearDB adatbázist, majd utána áttelepíteni az hello Azure-előfizetés a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2330f-119">tooavoid this, first, manually migrate your ClearDB database, and then migrate hello Azure subscription for your web app.</span></span>

<span data-ttu-id="2330f-120">További információkért lásd: [ClearDB MySQL-adatbázisok – gyakori kérdések az Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span><span class="sxs-lookup"><span data-stu-id="2330f-120">For more information, see [FAQs for ClearDB MySQL databases with Azure App Service](https://docs.microsoft.com/azure/store-cleardb-faq/).</span></span>

## <a name="how-do-i-turn-on-php-logging-tootroubleshoot-php-issues"></a><span data-ttu-id="2330f-121">Hogyan PHP tootroubleshoot PHP problémák naplózás bekapcsolása?</span><span class="sxs-lookup"><span data-stu-id="2330f-121">How do I turn on PHP logging tootroubleshoot PHP issues?</span></span>

<span data-ttu-id="2330f-122">a PHP-naplózást tooturn:</span><span class="sxs-lookup"><span data-stu-id="2330f-122">tooturn on PHP logging:</span></span>

1. <span data-ttu-id="2330f-123">Jelentkezzen be tooyour [Kudu webhely](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="2330f-123">Sign in tooyour [Kudu website](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
2. <span data-ttu-id="2330f-124">Hello felső menüben válassza ki a **Debug konzol** > **CMD**.</span><span class="sxs-lookup"><span data-stu-id="2330f-124">In hello top menu, select **Debug Console** > **CMD**.</span></span>
3. <span data-ttu-id="2330f-125">Jelölje be hello **hely** mappát.</span><span class="sxs-lookup"><span data-stu-id="2330f-125">Select hello **Site** folder.</span></span>
4. <span data-ttu-id="2330f-126">Jelölje be hello **wwwroot** mappát.</span><span class="sxs-lookup"><span data-stu-id="2330f-126">Select hello **wwwroot** folder.</span></span>
5. <span data-ttu-id="2330f-127">Jelölje be hello  **+**  ikonra, és válassza **új fájl**.</span><span class="sxs-lookup"><span data-stu-id="2330f-127">Select hello **+** icon, and then select **New File**.</span></span>
6. <span data-ttu-id="2330f-128">Állítsa be a hello fájlnév túl**. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="2330f-128">Set hello file name too**.user.ini**.</span></span>
7. <span data-ttu-id="2330f-129">Válasszon hello ceruza ikont mellett túl**. user.ini**.</span><span class="sxs-lookup"><span data-stu-id="2330f-129">Select hello pencil icon next too**.user.ini**.</span></span>
8. <span data-ttu-id="2330f-130">Hello fájlban adja hozzá ezt a kódot:`log_errors=on`</span><span class="sxs-lookup"><span data-stu-id="2330f-130">In hello file, add this code: `log_errors=on`</span></span>
9. <span data-ttu-id="2330f-131">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="2330f-131">Select **Save**.</span></span>
10. <span data-ttu-id="2330f-132">Válasszon hello ceruza ikont mellett túl**wp-config.php**.</span><span class="sxs-lookup"><span data-stu-id="2330f-132">Select hello pencil icon next too**wp-config.php**.</span></span>
11. <span data-ttu-id="2330f-133">A következő kód hello szöveg toohello módosítása:</span><span class="sxs-lookup"><span data-stu-id="2330f-133">Change hello text toohello following code:</span></span>
   ```
   //Enable WP_DEBUG modedefine('WP_DEBUG', true);//Enable debug logging too/wp-content/debug.logdefine('WP_DEBUG_LOG', true);
   //Supress errors and warnings tooscreendefine('WP_DEBUG_DISPLAY', false);//Supress PHP errors tooscreenini_set('display_errors', 0);
   ```
12. <span data-ttu-id="2330f-134">Indítsa újra a webes alkalmazás hello hello webes alkalmazás menü, az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2330f-134">In hello Azure portal, in hello web app menu, restart your web app.</span></span>

<span data-ttu-id="2330f-135">További információkért lásd: [engedélyezése WordPress hibanaplókat](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="2330f-135">For more information, see [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

## <a name="how-do-i-log-python-application-errors-in-apps-that-are-hosted-in-app-service"></a><span data-ttu-id="2330f-136">Hogyan Python-alkalmazások hibáinak jelentkezni az App Service szolgáltatásban üzemeltetett alkalmazásokat?</span><span class="sxs-lookup"><span data-stu-id="2330f-136">How do I log Python application errors in apps that are hosted in App Service?</span></span>

<span data-ttu-id="2330f-137">Python-alkalmazások hibáinak toocapture:</span><span class="sxs-lookup"><span data-stu-id="2330f-137">toocapture Python application errors:</span></span>

1. <span data-ttu-id="2330f-138">Válassza ki a webalkalmazás az Azure-portálon hello **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2330f-138">In hello Azure portal, in your web app, select **Settings**.</span></span>
2. <span data-ttu-id="2330f-139">A hello **beállítások** lapon jelölje be **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="2330f-139">On hello **Settings** tab, select **Application settings**.</span></span>
3. <span data-ttu-id="2330f-140">A **Alkalmazásbeállítások**, adja meg a következő kulcs/érték pár hello:</span><span class="sxs-lookup"><span data-stu-id="2330f-140">Under **App settings**, enter hello following key/value pair:</span></span>
    * <span data-ttu-id="2330f-141">Kulcs: WSGI_LOG</span><span class="sxs-lookup"><span data-stu-id="2330f-141">Key : WSGI_LOG</span></span>
    * <span data-ttu-id="2330f-142">Érték: D:\home\site\wwwroot\logs.txt (adja meg a kívánt fájlnevet)</span><span class="sxs-lookup"><span data-stu-id="2330f-142">Value : D:\home\site\wwwroot\logs.txt (enter your choice of file name)</span></span>

<span data-ttu-id="2330f-143">Meg kell jelennie hibák hello logs.txt fájlban hello wwwroot mappában.</span><span class="sxs-lookup"><span data-stu-id="2330f-143">You should now see errors in hello logs.txt file in hello wwwroot folder.</span></span>

## <a name="how-do-i-change-hello-version-of-hello-nodejs-application-that-is-hosted-in-app-service"></a><span data-ttu-id="2330f-144">Hogyan változtathatom meg a Node.js-alkalmazás az App Service-ben üzemeltetett hello hello verzióját?</span><span class="sxs-lookup"><span data-stu-id="2330f-144">How do I change hello version of hello Node.js application that is hosted in App Service?</span></span>

<span data-ttu-id="2330f-145">hello Node.js-alkalmazás toochange hello verzióját használja hello alábbi beállítások egyikét:</span><span class="sxs-lookup"><span data-stu-id="2330f-145">toochange hello version of hello Node.js application, you can use one of hello following options:</span></span>

*   <span data-ttu-id="2330f-146">Hello Azure-portálon, használjon **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="2330f-146">In hello Azure portal, use **App settings**.</span></span>
    1. <span data-ttu-id="2330f-147">A hello Azure-portálon válassza a tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2330f-147">In hello Azure portal, go tooyour web app.</span></span>
    2. <span data-ttu-id="2330f-148">A hello **beállítások** panelen válassza **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="2330f-148">On hello **Settings** blade, select **Application settings**.</span></span>
    3. <span data-ttu-id="2330f-149">A **Alkalmazásbeállítások**, WEBSITE_NODE_DEFAULT_VERSION hello kulcsként is felvehet, és azt hello Node.js verziója hello értékként.</span><span class="sxs-lookup"><span data-stu-id="2330f-149">In **App settings**, you can include WEBSITE_NODE_DEFAULT_VERSION as hello key, and hello version of Node.js you want as hello value.</span></span>
    4. <span data-ttu-id="2330f-150">Nyissa meg tooyour [Kudu konzol](https://*yourwebsitename*.scm.azurewebsites.net).</span><span class="sxs-lookup"><span data-stu-id="2330f-150">Go tooyour [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net).</span></span>
    5. <span data-ttu-id="2330f-151">toocheck hello Node.js verziót, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2330f-151">toocheck hello Node.js version, enter hello following command:</span></span>  
   ```
   node -v
   ```
*   <span data-ttu-id="2330f-152">Módosítsa a hello iisnode.yml fájlt.</span><span class="sxs-lookup"><span data-stu-id="2330f-152">Modify hello iisnode.yml file.</span></span> <span data-ttu-id="2330f-153">Változó hello Node.js verziót a hello iisnode.yml fájlt csak beállítja hello futtatókörnyezetben, hogy az iisnode használja.</span><span class="sxs-lookup"><span data-stu-id="2330f-153">Changing hello Node.js version in hello iisnode.yml file only sets hello runtime environment that iisnode uses.</span></span> <span data-ttu-id="2330f-154">A Kudu cmd, míg mások továbbra is használhatják az beállított hello Node.js-verzió **Alkalmazásbeállítások** a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2330f-154">Your Kudu cmd and others still use hello Node.js version that is set in **App settings** in hello Azure portal.</span></span>

    <span data-ttu-id="2330f-155">tooset hello iisnode.yml fájlt, hozzon létre manuálisan egy iisnode.yml fájlt az alkalmazás gyökérmappájába.</span><span class="sxs-lookup"><span data-stu-id="2330f-155">tooset hello iisnode.yml manually, create an iisnode.yml file in your app root folder.</span></span> <span data-ttu-id="2330f-156">Hello fájlban a következő sor hello a következők:</span><span class="sxs-lookup"><span data-stu-id="2330f-156">In hello file, include hello following line:</span></span>
   ```
   nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
   ```
   
*   <span data-ttu-id="2330f-157">Be hello iisnode.yml fájlt package.json forrás vezérlő üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="2330f-157">Set hello iisnode.yml file by using package.json during source control deployment.</span></span>
    <span data-ttu-id="2330f-158">hello Azure forrás vezérlő telepítési folyamata magában foglalja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2330f-158">hello Azure source control deployment process involves hello following steps:</span></span>
    1. <span data-ttu-id="2330f-159">Azure-webalkalmazás tartalmi toohello helyezi.</span><span class="sxs-lookup"><span data-stu-id="2330f-159">Moves content toohello Azure web app.</span></span>
    2. <span data-ttu-id="2330f-160">Egy alapértelmezett telepítési parancsfájl hoz létre, ha nincs hello webes alkalmazás gyökérmappájában (Deploy.cmd fájl, .deployment fájlok) ilyen.</span><span class="sxs-lookup"><span data-stu-id="2330f-160">Creates a default deployment script, if there isn’t one (deploy.cmd, .deployment files) in hello web app root folder.</span></span>
    3. <span data-ttu-id="2330f-161">Futtatja a telepítési parancsfájlt, amelyben létrehoz egy iisnode.yml fájlt Ha, említse meg a Node.js-verzió hello hello package.json fájl > motor`"engines": {"node": "5.9.1","npm": "3.7.3"}`</span><span class="sxs-lookup"><span data-stu-id="2330f-161">Runs a deployment script in which it creates an iisnode.yml file if you mention hello Node.js version in hello package.json file > engine `"engines": {"node": "5.9.1","npm": "3.7.3"}`</span></span>
    4. <span data-ttu-id="2330f-162">hello iisnode.yml fájlt a következő kódsort hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2330f-162">hello iisnode.yml file has hello following line of code:</span></span>
        ```
        nodeProcessCommandLine: "D:\Program Files (x86)\nodejs\5.9.1\node.exe"
        ```

## <a name="i-see-hello-message-error-establishing-a-database-connection-in-my-wordpress-app-thats-hosted-in-app-service-how-do-i-troubleshoot-this"></a><span data-ttu-id="2330f-163">A WordPress alkalmazás az App Service-ben üzemeltetett üdvözlőüzenetére "Adatbázis-kapcsolatot létrehozó Error" látható.</span><span class="sxs-lookup"><span data-stu-id="2330f-163">I see hello message "Error establishing a database connection" in my WordPress app that's hosted in App Service.</span></span> <span data-ttu-id="2330f-164">Hogyan hibaelhárítása Ez?</span><span class="sxs-lookup"><span data-stu-id="2330f-164">How do I troubleshoot this?</span></span>

<span data-ttu-id="2330f-165">Ha ez a hiba a Azure WordPress alkalmazás, tooenable php_errors.log és debug.log című teljes hello lépéseket részletesen [engedélyezése WordPress hibanaplókat](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span><span class="sxs-lookup"><span data-stu-id="2330f-165">If you see this error in your Azure WordPress app, tooenable php_errors.log and debug.log, complete hello steps detailed in [Enable WordPress error logs](https://blogs.msdn.microsoft.com/azureossds/2015/10/09/logging-php-errors-in-wordpress-2/).</span></span>

<span data-ttu-id="2330f-166">Hello naplókat, ha engedélyezve vannak Reprodukálja hello hibát, és ellenőrizze az hello naplók toosee, ha nincs kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="2330f-166">When hello logs are enabled, reproduce hello error, and then check hello logs toosee if you are running out of connections:</span></span>
```
[09-Oct-2015 00:03:13 UTC] PHP Warning: mysqli_real_connect(): (HY000/1226): User ‘abcdefghijk79' has exceeded hello ‘max_user_connections’ resource (current value: 4) in D:\home\site\wwwroot\wp-includes\wp-db.php on line 1454
```

<span data-ttu-id="2330f-167">Ha ez a hiba a debug.log vagy php_errors.log fájlok jelenik meg, az alkalmazás meghaladja hello kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="2330f-167">If you see this error in your debug.log or php_errors.log files, your app is exceeding hello number of connections.</span></span> <span data-ttu-id="2330f-168">A ClearDB tárolja, ha ellenőrizze a rendelkezésre álló kapcsolatok hello száma a [service-csomag](https://www.cleardb.com/pricing.view).</span><span class="sxs-lookup"><span data-stu-id="2330f-168">If you’re hosting on ClearDB, verify hello number of connections that are available in your [service plan](https://www.cleardb.com/pricing.view).</span></span>

## <a name="how-do-i-debug-a-nodejs-app-thats-hosted-in-app-service"></a><span data-ttu-id="2330f-169">Hogyan debug a Node.js-alkalmazás az App Service-ben üzemeltetett?</span><span class="sxs-lookup"><span data-stu-id="2330f-169">How do I debug a Node.js app that's hosted in App Service?</span></span>

1.  <span data-ttu-id="2330f-170">Nyissa meg tooyour [Kudu konzol](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span><span class="sxs-lookup"><span data-stu-id="2330f-170">Go tooyour [Kudu console](https://*yourwebsitename*.scm.azurewebsites.net/DebugConsole).</span></span>
2.  <span data-ttu-id="2330f-171">Nyissa meg tooyour application logs mappában (D:\home\LogFiles\Application).</span><span class="sxs-lookup"><span data-stu-id="2330f-171">Go tooyour application logs folder (D:\home\LogFiles\Application).</span></span>
3.  <span data-ttu-id="2330f-172">Hello logging_errors.txt fájlban ellenőrizze a tartalom.</span><span class="sxs-lookup"><span data-stu-id="2330f-172">In hello logging_errors.txt file, check for content.</span></span>

## <a name="how-do-i-install-native-python-modules-in-an-app-service-web-app-or-api-app"></a><span data-ttu-id="2330f-173">Natív Python-modulok telepítése egy App Service webalkalmazás vagy API-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="2330f-173">How do I install native Python modules in an App Service web app or API app?</span></span>

<span data-ttu-id="2330f-174">Néhány csomag Előfordulhat, hogy telepítse az Azure-ban a pip használatával.</span><span class="sxs-lookup"><span data-stu-id="2330f-174">Some packages might not install by using pip in Azure.</span></span> <span data-ttu-id="2330f-175">hello csomag valószínűleg csak akkor érhető el a Python-Csomagindexet hello, vagy lehet, hogy egy fordító szükséges (a fordító nem áll rendelkezésre hello futtató hello webalkalmazást az App Service szolgáltatásban).</span><span class="sxs-lookup"><span data-stu-id="2330f-175">hello package might not be available on hello Python Package Index, or a compiler might be required (a compiler is not available on hello computer that is running hello web app in App Service).</span></span> <span data-ttu-id="2330f-176">Az App Service web apps és API-alkalmazások natív modulok telepítésével kapcsolatos információkért lásd: [telepítése Python-modulok az App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span><span class="sxs-lookup"><span data-stu-id="2330f-176">For information about installing native modules in App Service web apps and API apps, see [Install Python modules in App Service](https://blogs.msdn.microsoft.com/azureossds/2015/06/29/install-native-python-modules-on-azure-web-apps-api-apps/).</span></span>

## <a name="how-do-i-deploy-a-django-app-tooapp-service-by-using-git-and-hello-new-version-of-python"></a><span data-ttu-id="2330f-177">Hogyan telepíthetem a Django app tooApp Service Python a Git és hello verziójának használatával?</span><span class="sxs-lookup"><span data-stu-id="2330f-177">How do I deploy a Django app tooApp Service by using Git and hello new version of Python?</span></span>

<span data-ttu-id="2330f-178">A Django telepítésével kapcsolatos információkért lásd: [egy Django app tooApp szolgáltatás telepítése](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span><span class="sxs-lookup"><span data-stu-id="2330f-178">For information about installing Django, see [Deploying a Django app tooApp Service](https://blogs.msdn.microsoft.com/azureossds/2016/08/25/deploying-django-app-to-azure-app-services-using-git-and-new-version-of-python/).</span></span>

## <a name="where-are-hello-tomcat-log-files-located"></a><span data-ttu-id="2330f-179">Hol vannak hello Tomcat naplófájlok található?</span><span class="sxs-lookup"><span data-stu-id="2330f-179">Where are hello Tomcat log files located?</span></span>

<span data-ttu-id="2330f-180">Az Azure piactér és egyéni telepítésekhez:</span><span class="sxs-lookup"><span data-stu-id="2330f-180">For Azure Marketplace and custom deployments:</span></span>

* <span data-ttu-id="2330f-181">Mappájának helye: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span><span class="sxs-lookup"><span data-stu-id="2330f-181">Folder location: D:\home\site\wwwroot\bin\apache-tomcat-8.0.33\logs</span></span>
* <span data-ttu-id="2330f-182">Fontos fájlok:</span><span class="sxs-lookup"><span data-stu-id="2330f-182">Files of interest:</span></span>
    * <span data-ttu-id="2330f-183">catalina. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-183">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-184">gazdagép-kezelő. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-184">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-185">localhost. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-185">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-186">a Manager objektum. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-186">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-187">site_access_log. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-187">site_access_log.*yyyy-mm-dd*.log</span></span>


<span data-ttu-id="2330f-188">A portál **Alkalmazásbeállítások** központi telepítések:</span><span class="sxs-lookup"><span data-stu-id="2330f-188">For portal **App settings** deployments:</span></span>

* <span data-ttu-id="2330f-189">Mappájának helye: D:\home\LogFiles</span><span class="sxs-lookup"><span data-stu-id="2330f-189">Folder location: D:\home\LogFiles</span></span>
* <span data-ttu-id="2330f-190">Fontos fájlok:</span><span class="sxs-lookup"><span data-stu-id="2330f-190">Files of interest:</span></span>
    * <span data-ttu-id="2330f-191">catalina. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-191">catalina.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-192">gazdagép-kezelő. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-192">host-manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-193">localhost. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-193">localhost.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-194">a Manager objektum. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-194">manager.*yyyy-mm-dd*.log</span></span>
    * <span data-ttu-id="2330f-195">site_access_log. *éééé-hh-nn*.log</span><span class="sxs-lookup"><span data-stu-id="2330f-195">site_access_log.*yyyy-mm-dd*.log</span></span>

## <a name="how-do-i-troubleshoot-jdbc-driver-connection-errors"></a><span data-ttu-id="2330f-196">Hogyan hibáinak elhárítása JDBC kapcsolat hibái?</span><span class="sxs-lookup"><span data-stu-id="2330f-196">How do I troubleshoot JDBC driver connection errors?</span></span>

<span data-ttu-id="2330f-197">A következő üzenet a Tomcat naplókban hello tapasztalhatja:</span><span class="sxs-lookup"><span data-stu-id="2330f-197">You might see hello following message in your Tomcat logs:</span></span>

```
hello web application[ROOT] registered hello JDBC driver [com.mysql.jdbc.Driver] but failed toounregister it when hello web application was stopped. tooprevent a memory leak,hello JDBC Driver has been forcibly unregistered
```

<span data-ttu-id="2330f-198">tooresolve hello hiba:</span><span class="sxs-lookup"><span data-stu-id="2330f-198">tooresolve hello error:</span></span>

1. <span data-ttu-id="2330f-199">Távolítsa el az alkalmazás/lib mappában hello sqljdbc*.jar fájlt.</span><span class="sxs-lookup"><span data-stu-id="2330f-199">Remove hello sqljdbc*.jar file from your app/lib folder.</span></span>
2. <span data-ttu-id="2330f-200">Ha hello egyéni Tomcat vagy Azure piactér Tomcat webkiszolgálókat használ, a .jar fájl toohello Tomcat lib mappa másolása.</span><span class="sxs-lookup"><span data-stu-id="2330f-200">If you are using hello custom Tomcat or Azure Marketplace Tomcat web server, copy this .jar file toohello Tomcat lib folder.</span></span>
3. <span data-ttu-id="2330f-201">Ha engedélyezi a Java hello Azure portal (válasszon **Java 1.8** > **Tomcat kiszolgálót**), másolása hello sqljdbc.* jar-fájlra, amely párhuzamos tooyour app hello mappában.</span><span class="sxs-lookup"><span data-stu-id="2330f-201">If you are enabling Java from hello Azure portal (select **Java 1.8** > **Tomcat server**), copy hello sqljdbc.* jar file in hello folder that's parallel tooyour app.</span></span> <span data-ttu-id="2330f-202">Adja hozzá a következő classpath beállítás toohello web.config fájl hello:</span><span class="sxs-lookup"><span data-stu-id="2330f-202">Then, add hello following classpath setting toohello web.config file:</span></span>

    ```
    <httpPlatform>
    <environmentVariables>
    <environmentVariablename ="JAVA_OPTS" value=" -Djava.net.preferIPv4Stack=true
    -Xms128M -classpath %CLASSPATH%;[Path toohello sqljdbc*.jarfile]" />
    </environmentVariables>
    </httpPlatform>
    ```

## <a name="why-do-i-see-errors-when-i-attempt-toocopy-live-log-files"></a><span data-ttu-id="2330f-203">Miért látom hibák, amikor élő naplófájlok toocopy?</span><span class="sxs-lookup"><span data-stu-id="2330f-203">Why do I see errors when I attempt toocopy live log files?</span></span>

<span data-ttu-id="2330f-204">Ha élő naplófájlok toocopy a Java-alkalmazások (például Tomcat), láthatja az FTP-hiba:</span><span class="sxs-lookup"><span data-stu-id="2330f-204">If you try toocopy live log files for a Java app (for example, Tomcat), you might see this FTP error:</span></span>

```
Error transferring file [filename] Copying files from remote side failed.
    
hello process cannot access hello file because it is being used by another process.
```

<span data-ttu-id="2330f-205">hello hibaüzenet hello FTP-ügyfél függően változhat.</span><span class="sxs-lookup"><span data-stu-id="2330f-205">hello error message might vary, depending on hello FTP client.</span></span>

<span data-ttu-id="2330f-206">Minden Java-alkalmazások rendelkeznek a zárolási problémát.</span><span class="sxs-lookup"><span data-stu-id="2330f-206">All Java apps have this locking issue.</span></span> <span data-ttu-id="2330f-207">Csak a Kudu támogatja, a fájl letöltése hello alkalmazás futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="2330f-207">Only Kudu supports downloading this file while hello app is running.</span></span>

<span data-ttu-id="2330f-208">Leállítása hello app engedélyezi az FTP-hozzáférést toothese fájlok.</span><span class="sxs-lookup"><span data-stu-id="2330f-208">Stopping hello app allows FTP access toothese files.</span></span>

<span data-ttu-id="2330f-209">Egy másik megoldás, toowrite webjobs-feladat ütemezés szerint fut, és másolja át a fájlok tooa másik címtárhoz.</span><span class="sxs-lookup"><span data-stu-id="2330f-209">Another workaround is toowrite a WebJob that runs on a schedule and copies these files tooa different directory.</span></span> <span data-ttu-id="2330f-210">Egy minta-projekt lásd: hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) projekt.</span><span class="sxs-lookup"><span data-stu-id="2330f-210">For a sample project, see hello [CopyLogsJob](https://github.com/kamilsykora/CopyLogsJob) project.</span></span>

## <a name="where-do-i-find-hello-log-files-for-jetty"></a><span data-ttu-id="2330f-211">Hol található a Jetty hello naplófájlok?</span><span class="sxs-lookup"><span data-stu-id="2330f-211">Where do I find hello log files for Jetty?</span></span>

<span data-ttu-id="2330f-212">A piactéren, és egyéni telepítésekhez hello naplófájl hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs mappában van.</span><span class="sxs-lookup"><span data-stu-id="2330f-212">For Marketplace and custom deployments, hello log file is in hello D:\home\site\wwwroot\bin\jetty-distribution-9.1.2.v20140210\logs folder.</span></span> <span data-ttu-id="2330f-213">Vegye figyelembe, hogy hello mappájának helye hello használt Jetty módjától függ.</span><span class="sxs-lookup"><span data-stu-id="2330f-213">Note that hello folder location depends on hello version of Jetty you are using.</span></span> <span data-ttu-id="2330f-214">Például hello elérési utat az itt megadott a Jetty 9.1.2.</span><span class="sxs-lookup"><span data-stu-id="2330f-214">For example, hello path provided here is for Jetty 9.1.2.</span></span> <span data-ttu-id="2330f-215">Keresse meg jetty_*YYYY_MM_DD*. stderrout.log.</span><span class="sxs-lookup"><span data-stu-id="2330f-215">Look for jetty_*YYYY_MM_DD*.stderrout.log.</span></span>

<span data-ttu-id="2330f-216">A portál Alkalmazásbeállítás telepítések esetében hello naplófájl D:\home\LogFiles van.</span><span class="sxs-lookup"><span data-stu-id="2330f-216">For portal App Setting deployments, hello log file is in D:\home\LogFiles.</span></span> <span data-ttu-id="2330f-217">Keresse meg jetty_*YYYY_MM_DD*. stderrout.log</span><span class="sxs-lookup"><span data-stu-id="2330f-217">Look for jetty_*YYYY_MM_DD*.stderrout.log</span></span>

## <a name="can-i-send-email-from-my-azure-web-app"></a><span data-ttu-id="2330f-218">Küldhetek e-mailt a saját Azure-webalkalmazás?</span><span class="sxs-lookup"><span data-stu-id="2330f-218">Can I send email from my Azure web app?</span></span>

<span data-ttu-id="2330f-219">App Service beépített e-mail-szolgáltatás nem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="2330f-219">App Service doesn't have a built-in email feature.</span></span> <span data-ttu-id="2330f-220">Az e-mailek küldése az alkalmazásból, néhány jó alternatíva ez című [Stack Overflow vitafórum](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span><span class="sxs-lookup"><span data-stu-id="2330f-220">For some good alternatives for sending email from your app, see this [Stack Overflow discussion](http://stackoverflow.com/questions/17666161/sending-email-from-azure).</span></span>

## <a name="why-does-my-wordpress-site-redirect-tooanother-url"></a><span data-ttu-id="2330f-221">Miért saját WordPress-webhely tooanother URL-Címének átirányítása?</span><span class="sxs-lookup"><span data-stu-id="2330f-221">Why does my WordPress site redirect tooanother URL?</span></span>

<span data-ttu-id="2330f-222">Ha nemrégiben telepítette át tooAzure, a WordPress előfordulhat, hogy toohello régi tartomány URL-Címének átirányítása.</span><span class="sxs-lookup"><span data-stu-id="2330f-222">If you have recently migrated tooAzure, WordPress might redirect toohello old domain URL.</span></span> <span data-ttu-id="2330f-223">Ennek oka egy beállítás hello MySQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="2330f-223">This is caused by a setting in hello MySQL database.</span></span>

<span data-ttu-id="2330f-224">WordPress ismerős + használható tooupdate hello átirányítási URL-címet közvetlenül a hello adatbázis bővítmény Azure hely.</span><span class="sxs-lookup"><span data-stu-id="2330f-224">WordPress Buddy+ is an Azure Site Extension that you can use tooupdate hello redirection URL directly in hello database.</span></span> <span data-ttu-id="2330f-225">WordPress ismerős + használatával kapcsolatos további információkért lásd: [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="2330f-225">For more information about using WordPress Buddy+, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

<span data-ttu-id="2330f-226">Másik lehetőségként toomanually frissítési hello átirányítási URL-Címének használatával, az SQL-lekérdezések vagy PHPMyAdmin tetszés szerint lásd: [WordPress: toowrong URL-cím átirányítása](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span><span class="sxs-lookup"><span data-stu-id="2330f-226">Alternatively, if you prefer toomanually update hello redirection URL by using SQL queries or PHPMyAdmin, see [WordPress: Redirecting toowrong URL](https://blogs.msdn.microsoft.com/azureossds/2016/07/12/wordpress-redirecting-to-wrong-url/).</span></span>

## <a name="how-do-i-change-my-wordpress-sign-in-password"></a><span data-ttu-id="2330f-227">A WordPress bejelentkezési jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="2330f-227">How do I change my WordPress sign-in password?</span></span>

<span data-ttu-id="2330f-228">Ha elfelejtette a jelszavát WordPress-bejelentkezés, WordPress ismerős + tooupdate használhatja azt.</span><span class="sxs-lookup"><span data-stu-id="2330f-228">If you have forgotten your WordPress sign-in password, you can use WordPress Buddy+ tooupdate it.</span></span> <span data-ttu-id="2330f-229">a jelszó, a telepítés hello WordPress ismerős + Azure hely bővítmény és a majd teljes hello lépéseket tooreset [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="2330f-229">tooreset your password, install hello WordPress Buddy+ Azure Site Extension, and then complete hello steps described in [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="i-cant-sign-in-toowordpress-how-do-i-resolve-this"></a><span data-ttu-id="2330f-230">Nem tud bejelentkezni tooWordPress.</span><span class="sxs-lookup"><span data-stu-id="2330f-230">I can't sign in tooWordPress.</span></span> <span data-ttu-id="2330f-231">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="2330f-231">How do I resolve this?</span></span>

<span data-ttu-id="2330f-232">Ha nemrég a beépülő modul telepítése után WordPress tévedéssel, lehetséges, hogy egy hibás beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="2330f-232">If you find yourself locked out of WordPress after recently installing a plugin, you might have a faulty plugin.</span></span> <span data-ttu-id="2330f-233">WordPress ismerős +, amelyek segítségével Azure hely bővítmény letiltása a WordPress beépülő modulok.</span><span class="sxs-lookup"><span data-stu-id="2330f-233">WordPress Buddy+ is an Azure Site Extension that can help you disable plugins in WordPress.</span></span> <span data-ttu-id="2330f-234">További információkért lásd: [WordPress eszközök és a MySQL-áttelepítés a WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span><span class="sxs-lookup"><span data-stu-id="2330f-234">For more information, see [WordPress tools and MySQL migration with WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/).</span></span>

## <a name="how-do-i-migrate-my-wordpress-database"></a><span data-ttu-id="2330f-235">Hogyan telepítse át a WordPress adatbázist?</span><span class="sxs-lookup"><span data-stu-id="2330f-235">How do I migrate my WordPress database?</span></span>

<span data-ttu-id="2330f-236">Egy csatlakoztatott tooyour WordPress-webhely áttelepítése hello MySQL adatbázis több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="2330f-236">You have multiple options for migrating hello MySQL database that's connected tooyour WordPress website:</span></span>

* <span data-ttu-id="2330f-237">A fejlesztők: Használata hello [parancssort vagy PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span><span class="sxs-lookup"><span data-stu-id="2330f-237">Developers: Use hello [command prompt or PHPMyAdmin](https://blogs.msdn.microsoft.com/azureossds/2016/03/02/migrating-data-between-mysql-databases-using-kudu-console-azure-app-service/)</span></span>
* <span data-ttu-id="2330f-238">Nem-Fejlesztők: [WordPress ismerős +](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span><span class="sxs-lookup"><span data-stu-id="2330f-238">Non-developers: Use [WordPress Buddy+](https://blogs.msdn.microsoft.com/azureossds/2016/12/21/wordpress-tools-and-mysql-migration-with-wordpress-buddy/)</span></span>

## <a name="how-do-i-help-make-wordpress-more-secure"></a><span data-ttu-id="2330f-239">Hogyan segít, WordPress biztonságosabbá?</span><span class="sxs-lookup"><span data-stu-id="2330f-239">How do I help make WordPress more secure?</span></span>

<span data-ttu-id="2330f-240">Tekintse meg a WordPress, ajánlott biztonsági eljárásokkal kapcsolatos toolearn [ajánlott eljárások az Azure-ban WordPress biztonsági](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span><span class="sxs-lookup"><span data-stu-id="2330f-240">toolearn about security best practices for WordPress, see [Best practices for WordPress security in Azure](https://blogs.msdn.microsoft.com/azureossds/2016/12/26/best-practices-for-wordpress-security-on-azure/).</span></span>

## <a name="i-am-trying-toouse-phpmyadmin-and-i-see-hello-message-access-denied-how-do-i-resolve-this"></a><span data-ttu-id="2330f-241">Próbálok toouse PHPMyAdmin, és látható üdvözlőüzenetére "Hozzáférés megtagadva."</span><span class="sxs-lookup"><span data-stu-id="2330f-241">I am trying toouse PHPMyAdmin, and I see hello message “Access denied.”</span></span> <span data-ttu-id="2330f-242">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="2330f-242">How do I resolve this?</span></span>

<span data-ttu-id="2330f-243">A probléma tapasztalhat, ha hello MySQL alkalmazásbeli funkció még nem fut. Ez App Service-példányban.</span><span class="sxs-lookup"><span data-stu-id="2330f-243">You might experience this issue if hello MySQL in-app feature isn't running yet in this App Service instance.</span></span> <span data-ttu-id="2330f-244">tooresolve hello problémát, próbálja meg tooaccess a webhelyet.</span><span class="sxs-lookup"><span data-stu-id="2330f-244">tooresolve hello issue, try tooaccess your website.</span></span> <span data-ttu-id="2330f-245">Hello szükséges eljárások, beleértve a hello MySQL alkalmazáson belüli folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="2330f-245">This starts hello required processes, including hello MySQL in-app process.</span></span> <span data-ttu-id="2330f-246">hello folyamatok jelenik meg, hogy az alkalmazás fut, a Process Explorer MySQL győződjön meg arról, hogy mysqld.exe tooverify.</span><span class="sxs-lookup"><span data-stu-id="2330f-246">tooverify that MySQL in-app is running, in Process Explorer, ensure that mysqld.exe is listed in hello processes.</span></span>

<span data-ttu-id="2330f-247">Miután meggyőződött arról, hogy fut-e a MySQL alkalmazásbeli, próbálja toouse PHPMyAdmin.</span><span class="sxs-lookup"><span data-stu-id="2330f-247">After you ensure that MySQL in-app is running, try toouse PHPMyAdmin.</span></span>

## <a name="i-get-an-http-403-error-when-i-try-tooimport-or-export-my-mysql-in-app-database-by-using-phpmyadmin-how-do-i-resolve-this"></a><span data-ttu-id="2330f-248">A HTTP 403-as hiba jelenik meg: tooimport próbálja vagy alkalmazásbeli MySQL adatbázis exportálása PHPMyadmin használatával.</span><span class="sxs-lookup"><span data-stu-id="2330f-248">I get an HTTP 403 error when I try tooimport or export my MySQL in-app database by using PHPMyadmin.</span></span> <span data-ttu-id="2330f-249">Hogyan lehet elhárítani ezt?</span><span class="sxs-lookup"><span data-stu-id="2330f-249">How do I resolve this?</span></span>

<span data-ttu-id="2330f-250">Ha a Látványelem egy régebbi verzióját használja, akkor léptek fel egy ismert hiba.</span><span class="sxs-lookup"><span data-stu-id="2330f-250">If you are using an older version of Chrome, you might be experiencing a known bug.</span></span> <span data-ttu-id="2330f-251">tooresolve hello problémát, a frissítési tooa Chrome újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="2330f-251">tooresolve hello issue, upgrade tooa newer version of Chrome.</span></span> <span data-ttu-id="2330f-252">Is próbálkozzon másik böngészővel, például az Internet Explorer vagy Edge, ahol hello nem fordul elő.</span><span class="sxs-lookup"><span data-stu-id="2330f-252">Also try using a different browser, like Internet Explorer or Edge, where hello issue does not occur.</span></span>

---
title: "Webalkalmazások konfigurálása az Azure App Service-ben"
description: "A webes alkalmazás beállítása az Azure App Service szolgáltatások"
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: cacbcf879555907f81d824dc1069b05579dca010
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a><span data-ttu-id="7bdf8-103">Webalkalmazások konfigurálása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="7bdf8-103">Configure web apps in Azure App Service</span></span>
<span data-ttu-id="7bdf8-104">Ez a témakör ismerteti, hogyan konfigurálhatja a web app használatával a [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-104">This topic explains how to configure a web app using the [Azure Portal].</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a><span data-ttu-id="7bdf8-105">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="7bdf8-105">Application settings</span></span>
1. <span data-ttu-id="7bdf8-106">Az a [Azure Portal], nyissa meg a webalkalmazás a panelt.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-106">In the [Azure Portal], open the blade for the web app.</span></span>
2. <span data-ttu-id="7bdf8-107">Kattintson a **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-107">Click **All Settings**.</span></span>
3. <span data-ttu-id="7bdf8-108">Kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-108">Click **Application Settings**.</span></span>

![Alkalmazásbeállítások][configure01]

<span data-ttu-id="7bdf8-110">A **Alkalmazásbeállítások** panel több kategóriák szerint csoportosítva beállításokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-110">The **Application settings** blade has settings grouped under several categories.</span></span>

### <a name="general-settings"></a><span data-ttu-id="7bdf8-111">Általános beállítások</span><span class="sxs-lookup"><span data-stu-id="7bdf8-111">General settings</span></span>
<span data-ttu-id="7bdf8-112">**Keretrendszer-verziók**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-112">**Framework versions**.</span></span> <span data-ttu-id="7bdf8-113">Állítsa be ezeket a beállításokat, ha az alkalmazás használja, minden ezen keretrendszerek:</span><span class="sxs-lookup"><span data-stu-id="7bdf8-113">Set these options if your app uses any these frameworks:</span></span> 

* <span data-ttu-id="7bdf8-114">**.NET-keretrendszer**: állítsa be a .NET-keretrendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-114">**.NET Framework**: Set the .NET framework version.</span></span> 
* <span data-ttu-id="7bdf8-115">**A PHP**: állítsa be a PHP verzióját vagy **OFF** PHP letiltása.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-115">**PHP**: Set the PHP version, or **OFF** to disable PHP.</span></span> 
* <span data-ttu-id="7bdf8-116">**Java**: válassza ki a Java verzióját vagy **OFF** Java letiltása.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-116">**Java**: Select the Java version or **OFF** to disable Java.</span></span> <span data-ttu-id="7bdf8-117">Használja a **webes tároló** lehetőséget választhat a Tomcat- és Jetty-verziót.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-117">Use the **Web Container** option to choose between Tomcat and Jetty versions.</span></span>
* <span data-ttu-id="7bdf8-118">**Python**: Jelölje ki a Python-verziót, vagy **OFF** Python letiltása.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-118">**Python**: Select the Python version, or **OFF** to disable Python.</span></span>

<span data-ttu-id="7bdf8-119">Technikai okokból Java az alkalmazás engedélyezése letiltja a .NET, PHP és Python beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-119">For technical reasons, enabling Java for your app disables the .NET, PHP, and Python options.</span></span>

<span data-ttu-id="7bdf8-120"><a name="platform"></a>
**Platform**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-120"><a name="platform"></a>
**Platform**.</span></span> <span data-ttu-id="7bdf8-121">Választja ki, hogy a webalkalmazás fut, 32 bites vagy 64 bites környezetben.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-121">Selects whether your web app runs in a 32-bit or 64-bit environment.</span></span> <span data-ttu-id="7bdf8-122">A 64 bites környezet Basic vagy Standard módot igényel.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-122">The 64-bit environment requires Basic or Standard mode.</span></span> <span data-ttu-id="7bdf8-123">Ingyenes, és megosztott mód mindig 32-bit-es környezetben fusson.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-123">Free and Shared modes always run in a 32-bit environment.</span></span>

<span data-ttu-id="7bdf8-124">**Webes szoftvercsatornák**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-124">**Web Sockets**.</span></span> <span data-ttu-id="7bdf8-125">Állítsa be **ON** ahhoz, hogy a WebSocket protokoll; például, ha a webes alkalmazás használ [ASP.NET SignalR] vagy [socket.io].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-125">Set **ON** to enable the WebSocket protocol; for example, if your web app uses [ASP.NET SignalR] or [socket.io].</span></span>

<span data-ttu-id="7bdf8-126"><a name="alwayson"></a>
**Always On**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-126"><a name="alwayson"></a>
**Always On**.</span></span> <span data-ttu-id="7bdf8-127">Alapértelmezés szerint a webalkalmazások a memóriából, ha bizonyos ideig inaktív.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-127">By default, web apps are unloaded if they are idle for some period of time.</span></span> <span data-ttu-id="7bdf8-128">Ez lehetővé teszi, hogy a rendszer, ezért az erőforrások megőrzése.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-128">This lets the system conserve resources.</span></span> <span data-ttu-id="7bdf8-129">Basic vagy Standard módban, ahol engedélyezheti **mindig a** az alkalmazás mindig betöltve a folyamatosan.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-129">In Basic or Standard mode, you can enable **Always On** to keep the app loaded all the time.</span></span> <span data-ttu-id="7bdf8-130">Ha az alkalmazás fut a folyamatos webjobs-feladatok webjobs-feladatok futtatása indított CRON-kifejezés használatával, vagy engedélyezze **mindig a**, vagy a webes feladatok nem futtatható megbízhatóan.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-130">If your app runs continuous WebJobs or runs WebJobs triggered using a CRON expression, you should enable **Always On**, or the web jobs may not run reliably.</span></span>

<span data-ttu-id="7bdf8-131">**Felügyelt folyamatkezelési verzió**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-131">**Managed Pipeline Version**.</span></span> <span data-ttu-id="7bdf8-132">Beállítja az IIS [folyamatkezelési mód].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-132">Sets the IIS [pipeline mode].</span></span> <span data-ttu-id="7bdf8-133">Ne módosítsa integrált (alapértelmezett) kivéve, ha egy örökölt alkalmazás, amely az IIS egy régebbi verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-133">Leave this set to Integrated (the default) unless you have a legacy app that requires an older version of IIS.</span></span>

<span data-ttu-id="7bdf8-134">**Az automatikus felcserélés**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-134">**Auto Swap**.</span></span> <span data-ttu-id="7bdf8-135">Ha engedélyezte az automatikus felcserélés egy üzembe helyezési tárhelyet, App Service lesz automatikusan felcserélni a webalkalmazás éles környezetben ha egy frissítés leküldése a tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-135">If you enable Auto Swap for a deployment slot, App Service will automatically swap the web app into production when you push an update to that slot.</span></span> <span data-ttu-id="7bdf8-136">További információkért lásd: [központi telepítése az átmeneti üzembe helyezési ponti az Azure App Service web Apps](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="7bdf8-136">For more information, see [Deploy to staging slots for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span>

### <a name="debugging"></a><span data-ttu-id="7bdf8-137">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="7bdf8-137">Debugging</span></span>
<span data-ttu-id="7bdf8-138">**Távoli hibakeresés**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-138">**Remote Debugging**.</span></span> <span data-ttu-id="7bdf8-139">Távoli hibakeresésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-139">Enables remote debugging.</span></span> <span data-ttu-id="7bdf8-140">Ha engedélyezve van, használhatja a távoli hibakereső a Visual Studio közvetlenül kapcsolódni a webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-140">When enabled, you can use the remote debugger in Visual Studio to connect directly to your web app.</span></span> <span data-ttu-id="7bdf8-141">Távoli hibakeresés továbbra is engedélyezett marad 48 órán keresztül.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-141">Remote debugging will remain enabled for 48 hours.</span></span> 

### <a name="app-settings"></a><span data-ttu-id="7bdf8-142">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="7bdf8-142">App settings</span></span>
<span data-ttu-id="7bdf8-143">Ebben a szakaszban található név/érték párok, akkor web app betölti a kezdőlapon.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-143">This section contains name/value pairs that you web app will load on start up.</span></span> 

* <span data-ttu-id="7bdf8-144">.NET-alkalmazások esetén ezek a beállítások vannak be a nézetmodellbe, a .NET-konfiguráció `AppSettings` futásidőben, felülírva a meglévő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-144">For .NET apps, these settings are injected into your .NET configuration `AppSettings` at runtime, overriding existing settings.</span></span> 
* <span data-ttu-id="7bdf8-145">A PHP, Python, Java és csomópont alkalmazások férhetnek hozzá ezeket a beállításokat a környezeti változók futásidőben.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-145">PHP, Python, Java and Node applications can access these settings as environment variables at runtime.</span></span> <span data-ttu-id="7bdf8-146">Minden egyes alkalmazás-beállítás két környezeti változók jönnek létre; egy alkalmazás beállítási bejegyzésre, és egy másik APPSETTING_ előtaggal rendelkező által a megadott néven.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-146">For each app setting, two environment variables are created; one with the name specified by the app setting entry, and another with a prefix of APPSETTING_.</span></span> <span data-ttu-id="7bdf8-147">Is tartalmaznak ugyanarra az értékre.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-147">Both contain the same value.</span></span>

### <a name="connection-strings"></a><span data-ttu-id="7bdf8-148">Kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="7bdf8-148">Connection strings</span></span>
<span data-ttu-id="7bdf8-149">Kapcsolati karakterláncok kapcsolt erőforrásokban.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-149">Connection strings for linked resources.</span></span> 

<span data-ttu-id="7bdf8-150">.NET-alkalmazások esetén a kapcsolati karakterláncok vannak be a nézetmodellbe, a .NET-konfiguráció `connectionStrings` beállításokat futásidőben, felülírva a meglévő bejegyzéseket, ahol a kulcs értéke a hivatkozott adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-150">For .NET apps, these connection strings are injected into your .NET configuration `connectionStrings` settings at runtime, overriding existing entries where the key equals the linked database name.</span></span> 

<span data-ttu-id="7bdf8-151">A PHP, Python, Java és csomópont alkalmazások ezek a beállítások használhatók környezeti változók futásidőben, a kapcsolat típusa előtagként.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-151">For PHP, Python, Java and Node applications, these settings will be available as environment variables at runtime, prefixed with the connection type.</span></span> <span data-ttu-id="7bdf8-152">A környezeti változó előtagok a következők:</span><span class="sxs-lookup"><span data-stu-id="7bdf8-152">The environment variable prefixes are as follows:</span></span> 

* <span data-ttu-id="7bdf8-153">SQL Server:`SQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="7bdf8-153">SQL Server: `SQLCONNSTR_`</span></span>
* <span data-ttu-id="7bdf8-154">MySQL:`MYSQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="7bdf8-154">MySQL: `MYSQLCONNSTR_`</span></span>
* <span data-ttu-id="7bdf8-155">SQL-adatbázis:`SQLAZURECONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="7bdf8-155">SQL Database: `SQLAZURECONNSTR_`</span></span>
* <span data-ttu-id="7bdf8-156">Egyéni:`CUSTOMCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="7bdf8-156">Custom: `CUSTOMCONNSTR_`</span></span>

<span data-ttu-id="7bdf8-157">Például, ha a MySql-kapcsolati karakterlánc lett nevű `connectionstring1`, akkor elérhetőek a környezeti változó `MYSQLCONNSTR_connectionString1`.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-157">For example, if a MySql connection string were named `connectionstring1`, it would be accessed through the environment variable `MYSQLCONNSTR_connectionString1`.</span></span>

### <a name="default-documents"></a><span data-ttu-id="7bdf8-158">Alapértelmezett dokumentum</span><span class="sxs-lookup"><span data-stu-id="7bdf8-158">Default documents</span></span>
<span data-ttu-id="7bdf8-159">Az alapértelmezett dokumentum egy a weblap, akkor jelenik meg, a webhely a gyökér URL-címen.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-159">The default document is the web page that is displayed at the root URL for a website.</span></span>  <span data-ttu-id="7bdf8-160">A listán szereplő első egyező fájlok szolgál.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-160">The first matching file in the list is used.</span></span> 

<span data-ttu-id="7bdf8-161">Webalkalmazások használhatja a modulok, hogy útvonal URL-címe alapján. ahelyett, hogy nincs alapértelmezett dokumentum ilyen szolgáltató statikus tartalmat, ebben az esetben nincs.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-161">Web apps might use modules that route based on URL, rather than serving static content, in which case there is no default document as such.</span></span>    

### <a name="handler-mappings"></a><span data-ttu-id="7bdf8-162">Kezelőleképezések</span><span class="sxs-lookup"><span data-stu-id="7bdf8-162">Handler mappings</span></span>
<span data-ttu-id="7bdf8-163">Ez a terület segítségével egyéni parancsfájl processzorok, a tanúsítványigénylések meghatározott fájlnév-kiterjesztések hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-163">Use this area to add custom script processors to handle requests for specific file extensions.</span></span> 

* <span data-ttu-id="7bdf8-164">**Bővítmény**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-164">**Extension**.</span></span> <span data-ttu-id="7bdf8-165">A fájl kiterjesztése például *.php vagy handler.fcgi kezelni.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-165">The file extension to be handled, such as *.php or handler.fcgi.</span></span> 
* <span data-ttu-id="7bdf8-166">**Parancsfájl-feldolgozó elérési útja**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-166">**Script Processor Path**.</span></span> <span data-ttu-id="7bdf8-167">A parancsprogram-feldolgozó abszolút elérési utat.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-167">The absolute path of the script processor.</span></span> <span data-ttu-id="7bdf8-168">A parancsprogram-feldolgozó által a fájlokat, amelyek megfelelnek a fájlnévkiterjesztés kérelmek dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-168">Requests to files that match the file extension will be processed by the script processor.</span></span> <span data-ttu-id="7bdf8-169">Az elérési utat használja `D:\home\site\wwwroot` utal, hogy az alkalmazás gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-169">Use the path `D:\home\site\wwwroot` to refer to your app's root directory.</span></span>
* <span data-ttu-id="7bdf8-170">**További argumentumok**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-170">**Additional Arguments**.</span></span> <span data-ttu-id="7bdf8-171">Parancssori argumentumokat használni a parancsprogram-feldolgozó</span><span class="sxs-lookup"><span data-stu-id="7bdf8-171">Optional command-line arguments for the script processor</span></span> 

### <a name="virtual-applications-and-directories"></a><span data-ttu-id="7bdf8-172">Virtuális alkalmazások és könyvtárak</span><span class="sxs-lookup"><span data-stu-id="7bdf8-172">Virtual applications and directories</span></span>
<span data-ttu-id="7bdf8-173">Konfigurálja a virtuális alkalmazások és könyvtárak, adja meg az egyes virtuális könyvtárakat és az annak megfelelő fizikai elérési út a webhely gyökeréhez viszonyítva.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-173">To configure virtual applications and directories, specify each virtual directory and its corresponding physical path relative to the website root.</span></span> <span data-ttu-id="7bdf8-174">Ha, akkor jelölje be a **alkalmazás** jelölőnégyzetet, hogy egy alkalmazás egy virtuális könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-174">Optionally, you can select the **Application** checkbox to mark a virtual directory as an application.</span></span>

## <a name="enabling-diagnostic-logs"></a><span data-ttu-id="7bdf8-175">Diagnosztikai naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="7bdf8-175">Enabling diagnostic logs</span></span>
<span data-ttu-id="7bdf8-176">Diagnosztikai naplók engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="7bdf8-176">To enable diagnostic logs:</span></span>

1. <span data-ttu-id="7bdf8-177">A webalkalmazás panelen kattintson **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-177">In the blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="7bdf8-178">Kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-178">Click **Diagnostic logs**.</span></span> 

<span data-ttu-id="7bdf8-179">Diagnosztikai naplók írása egy webalkalmazás, amely támogatja a naplózási beállítások:</span><span class="sxs-lookup"><span data-stu-id="7bdf8-179">Options for writing diagnostic logs from a web application that supports logging:</span></span> 

* <span data-ttu-id="7bdf8-180">**Alkalmazásnaplózás**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-180">**Application Logging**.</span></span> <span data-ttu-id="7bdf8-181">A fájlrendszer alkalmazásnaplók ír.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-181">Writes application logs to the file system.</span></span> <span data-ttu-id="7bdf8-182">A naplózás 12 óra ideig tart.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-182">Logging lasts for a period of 12 hours.</span></span> 

<span data-ttu-id="7bdf8-183">**Szint**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-183">**Level**.</span></span> <span data-ttu-id="7bdf8-184">Ha alkalmazás-naplózás engedélyezve van, ezzel a beállítással megadhatja, amely lesz adatmennyiség rögzített (hiba, figyelmeztetés, információ, vagy a részletes).</span><span class="sxs-lookup"><span data-stu-id="7bdf8-184">When application logging is enabled, this option specifies the amount of information that will be recorded (Error, Warning, Information, or Verbose).</span></span>

<span data-ttu-id="7bdf8-185">**Webkiszolgáló naplózásának**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-185">**Web server logging**.</span></span> <span data-ttu-id="7bdf8-186">A W3C bővített naplófájlformátum naplók lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-186">Logs are saved in the W3C extended log file format.</span></span> 

<span data-ttu-id="7bdf8-187">**A részletes hibaüzeneteket**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-187">**Detailed error messages**.</span></span> <span data-ttu-id="7bdf8-188">Menti a hiba részleteit a .htm fájl üzenetek.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-188">Saves detailed error messages .htm files.</span></span> <span data-ttu-id="7bdf8-189">A fájlok a /LogFiles/DetailedErrors kerülnek mentésre.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-189">The files are saved under /LogFiles/DetailedErrors.</span></span> 

<span data-ttu-id="7bdf8-190">**Sikertelen kérelmek követésének**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-190">**Failed request tracing**.</span></span> <span data-ttu-id="7bdf8-191">Naplók sikertelen kérelmek XML-fájlok számára.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-191">Logs failed requests to XML files.</span></span> <span data-ttu-id="7bdf8-192">A fájlok fájl/naplófájlok/W3SVC*xxx*, ahol a xxx egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-192">The files are saved under /LogFiles/W3SVC*xxx*, where xxx is a unique identifier.</span></span> <span data-ttu-id="7bdf8-193">Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-193">This folder contains an XSL file and one or more XML files.</span></span> <span data-ttu-id="7bdf8-194">Ügyeljen arra, hogy az XSL-fájl letöltése, mert formázására és az XML-fájlok tartalmát szűrés funkciókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-194">Make sure to download the XSL file, because it provides functionality for formatting and filtering the contents of the XML files.</span></span>

<span data-ttu-id="7bdf8-195">A naplófájlban, létre kell hoznia FTP hitelesítő adatokat, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="7bdf8-195">To view the log files, you must create FTP credentials, as follows:</span></span>

1. <span data-ttu-id="7bdf8-196">A webalkalmazás panelen kattintson **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-196">In the blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="7bdf8-197">Kattintson a **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-197">Click **Deployment credentials**.</span></span>
3. <span data-ttu-id="7bdf8-198">Adjon meg egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-198">Enter a user name and password.</span></span>
4. <span data-ttu-id="7bdf8-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-199">Click **Save**.</span></span>

![Telepítési hitelesítő adatok beállítása][configure03]

<span data-ttu-id="7bdf8-201">A teljes FTP-felhasználó neve "app\username" hol *app* a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-201">The full FTP user name is “app\username” where *app* is the name of your web app.</span></span> <span data-ttu-id="7bdf8-202">A felhasználónév, szerepel a webalkalmazás panelen, a **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-202">The username is listed in the web app blade, under **Essentials**.</span></span>  

![FTP telepítési hitelesítő adatok][configure02]

## <a name="other-configuration-tasks"></a><span data-ttu-id="7bdf8-204">Egyéb konfigurációs feladatok</span><span class="sxs-lookup"><span data-stu-id="7bdf8-204">Other configuration tasks</span></span>
### <a name="ssl"></a><span data-ttu-id="7bdf8-205">SSL</span><span class="sxs-lookup"><span data-stu-id="7bdf8-205">SSL</span></span>
<span data-ttu-id="7bdf8-206">Basic vagy Standard módban SSL-tanúsítványokat az egyéni tartományt is feltölthet.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-206">In Basic or Standard mode, you can upload SSL certificates for a custom domain.</span></span> <span data-ttu-id="7bdf8-207">További információ: [HTTPS engedélyezése az webalkalmazáshoz].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-207">For more information, see [Enable HTTPS for a web app].</span></span> 

<span data-ttu-id="7bdf8-208">A feltöltött tanúsítványok megtekintéséhez kattintson **összes beállítás** > **egyéni tartományok és SSL**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-208">To view your uploaded certificates, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="domain-names"></a><span data-ttu-id="7bdf8-209">Tartománynevek</span><span class="sxs-lookup"><span data-stu-id="7bdf8-209">Domain names</span></span>
<span data-ttu-id="7bdf8-210">A webalkalmazás egyéni tartományneveket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-210">Add custom domain names for your web app.</span></span> <span data-ttu-id="7bdf8-211">További információ: [egy webalkalmazást az Azure App Service szolgáltatásban az egyéni tartománynév konfigurálása].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-211">For more information, see [Configure a custom domain name for a web app in Azure App Service].</span></span>

<span data-ttu-id="7bdf8-212">A tartománynevek megtekintéséhez kattintson **összes beállítás** > **egyéni tartományok és SSL**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-212">To view your domain names, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="deployments"></a><span data-ttu-id="7bdf8-213">Központi telepítés</span><span class="sxs-lookup"><span data-stu-id="7bdf8-213">Deployments</span></span>
* <span data-ttu-id="7bdf8-214">Folyamatos üzembe helyezést beállítani.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-214">Set up continuous deployment.</span></span> <span data-ttu-id="7bdf8-215">Lásd: [Git használatával is telepíthet webalkalmazásokat az Azure App Service](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="7bdf8-215">See [Using Git to deploy Web Apps in Azure App Service](web-sites-deploy.md).</span></span>
* <span data-ttu-id="7bdf8-216">Üzembe helyezési pontok.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-216">Deployment slots.</span></span> <span data-ttu-id="7bdf8-217">Lásd: [előkészítési környezetek telepítése az Azure App Service Web Apps].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-217">See [Deploy to Staging Environments for Web Apps in Azure App Service].</span></span>

<span data-ttu-id="7bdf8-218">Az üzembe helyezési megtekintéséhez kattintson **összes beállítás** > **üzembe helyezési**.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-218">To view your deployment slots, click **All Settings** > **Deployment slots**.</span></span>

### <a name="monitoring"></a><span data-ttu-id="7bdf8-219">Figyelés</span><span class="sxs-lookup"><span data-stu-id="7bdf8-219">Monitoring</span></span>
<span data-ttu-id="7bdf8-220">Basic vagy Standard módban tesztelheti a HTTP vagy HTTPS-végpontokkal, legfeljebb három földrajzilag elosztott helyekről rendelkezésre állását.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-220">In Basic or Standard mode, you can  test the availability of HTTP or HTTPS endpoints, from up to three geo-distributed locations.</span></span> <span data-ttu-id="7bdf8-221">A figyelési teszt sikertelen, ha a HTTP válaszkódot (4xx vagy 5xx) hiba vagy a választ több mint 30 másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-221">A monitoring test fails if the HTTP response code is an error (4xx or 5xx) or the response takes more than 30 seconds.</span></span> <span data-ttu-id="7bdf8-222">A végpont tekinthető érhető el, ha a figyelési tesztek sikeres legyen, az összes meghatározott helyeiről.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-222">An endpoint is considered available if the monitoring tests succeed from all the specified locations.</span></span> 

<span data-ttu-id="7bdf8-223">További információkért lásd: [Útmutató: webes végpont állapotának figyelése].</span><span class="sxs-lookup"><span data-stu-id="7bdf8-223">For more information, see [How to: Monitor web endpoint status].</span></span>

> [!NOTE]
> <span data-ttu-id="7bdf8-224">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása] oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-224">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="7bdf8-225">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="7bdf8-225">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="7bdf8-226">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7bdf8-226">Next steps</span></span>
* <span data-ttu-id="7bdf8-227">[Egyéni tartománynév konfigurálása az Azure App Service-ben]</span><span class="sxs-lookup"><span data-stu-id="7bdf8-227">[Configure a custom domain name in Azure App Service]</span></span>
* <span data-ttu-id="7bdf8-228">[HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben]</span><span class="sxs-lookup"><span data-stu-id="7bdf8-228">[Enable HTTPS for an app in Azure App Service]</span></span>
* <span data-ttu-id="7bdf8-229">[A webalkalmazás skálázása az Azure App Service-ben]</span><span class="sxs-lookup"><span data-stu-id="7bdf8-229">[Scale a web app in Azure App Service]</span></span>
* <span data-ttu-id="7bdf8-230">[Az Azure App Service Web Apps figyelési alapjai]</span><span class="sxs-lookup"><span data-stu-id="7bdf8-230">[Monitoring basics for Web Apps in Azure App Service]</span></span>

<!-- URL List -->

<span data-ttu-id="7bdf8-231">[ASP.NET SignalR]: http://www.asp.net/signalr</span><span class="sxs-lookup"><span data-stu-id="7bdf8-231">[ASP.NET SignalR]: http://www.asp.net/signalr</span></span>
<span data-ttu-id="7bdf8-232">[Azure Portal]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="7bdf8-232">[Azure Portal]: https://portal.azure.com/</span></span>
<span data-ttu-id="7bdf8-233">[Egyéni tartománynév konfigurálása az Azure App Service-ben]: ./app-service-web-tutorial-custom-domain.md</span><span class="sxs-lookup"><span data-stu-id="7bdf8-233">[Configure a custom domain name in Azure App Service]: ./app-service-web-tutorial-custom-domain.md</span></span>
<span data-ttu-id="7bdf8-234">[előkészítési környezetek telepítése az Azure App Service Web Apps]: ./web-sites-staged-publishing.md</span><span class="sxs-lookup"><span data-stu-id="7bdf8-234">[Deploy to Staging Environments for Web Apps in Azure App Service]: ./web-sites-staged-publishing.md</span></span>
<span data-ttu-id="7bdf8-235">[HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben]: ./app-service-web-tutorial-custom-ssl.md</span><span class="sxs-lookup"><span data-stu-id="7bdf8-235">[Enable HTTPS for an app in Azure App Service]: ./app-service-web-tutorial-custom-ssl.md</span></span>
<span data-ttu-id="7bdf8-236">[Útmutató: webes végpont állapotának figyelése]: http://go.microsoft.com/fwLink/?LinkID=279906</span><span class="sxs-lookup"><span data-stu-id="7bdf8-236">[How to: Monitor web endpoint status]: http://go.microsoft.com/fwLink/?LinkID=279906</span></span>
<span data-ttu-id="7bdf8-237">[Az Azure App Service Web Apps figyelési alapjai]: ./web-sites-monitor.md</span><span class="sxs-lookup"><span data-stu-id="7bdf8-237">[Monitoring basics for Web Apps in Azure App Service]: ./web-sites-monitor.md</span></span>
<span data-ttu-id="7bdf8-238">[folyamatkezelési mód]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application</span><span class="sxs-lookup"><span data-stu-id="7bdf8-238">[pipeline mode]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application</span></span>
<span data-ttu-id="7bdf8-239">[A webalkalmazás skálázása az Azure App Service-ben]: ./web-sites-scale.md</span><span class="sxs-lookup"><span data-stu-id="7bdf8-239">[Scale a web app in Azure App Service]: ./web-sites-scale.md</span></span>
<span data-ttu-id="7bdf8-240">[socket.io]: ./web-sites-nodejs-chat-app-socketio.md</span><span class="sxs-lookup"><span data-stu-id="7bdf8-240">[socket.io]: ./web-sites-nodejs-chat-app-socketio.md</span></span>
<span data-ttu-id="7bdf8-241">[Az Azure App Service kipróbálása]: https://azure.microsoft.com/try/app-service/</span><span class="sxs-lookup"><span data-stu-id="7bdf8-241">[Try App Service]: https://azure.microsoft.com/try/app-service/</span></span>

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

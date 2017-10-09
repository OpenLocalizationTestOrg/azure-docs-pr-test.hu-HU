---
title: "aaaConfigure webalkalmazásokkal az Azure App Service-ben"
description: "Hogyan tooconfigure egy webalkalmazást az Azure App Service szolgáltatások"
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
ms.openlocfilehash: 8697ab6f21cfeb470e11f0d82c68692d43142fc5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-web-apps-in-azure-app-service"></a><span data-ttu-id="bed87-103">Webalkalmazások konfigurálása az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="bed87-103">Configure web apps in Azure App Service</span></span>
<span data-ttu-id="bed87-104">Ez a témakör ismerteti, hogyan web app használatával tooconfigure hello [Azure Portal].</span><span class="sxs-lookup"><span data-stu-id="bed87-104">This topic explains how tooconfigure a web app using hello [Azure Portal].</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="application-settings"></a><span data-ttu-id="bed87-105">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="bed87-105">Application settings</span></span>
1. <span data-ttu-id="bed87-106">A hello [Azure Portal], nyissa meg hello webalkalmazás hello panelen.</span><span class="sxs-lookup"><span data-stu-id="bed87-106">In hello [Azure Portal], open hello blade for hello web app.</span></span>
2. <span data-ttu-id="bed87-107">Kattintson a **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="bed87-107">Click **All Settings**.</span></span>
3. <span data-ttu-id="bed87-108">Kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="bed87-108">Click **Application Settings**.</span></span>

![Alkalmazásbeállítások][configure01]

<span data-ttu-id="bed87-110">Hello **Alkalmazásbeállítások** panel több kategóriák szerint csoportosítva beállításokkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bed87-110">hello **Application settings** blade has settings grouped under several categories.</span></span>

### <a name="general-settings"></a><span data-ttu-id="bed87-111">Általános beállítások</span><span class="sxs-lookup"><span data-stu-id="bed87-111">General settings</span></span>
<span data-ttu-id="bed87-112">**Keretrendszer-verziók**.</span><span class="sxs-lookup"><span data-stu-id="bed87-112">**Framework versions**.</span></span> <span data-ttu-id="bed87-113">Állítsa be ezeket a beállításokat, ha az alkalmazás használja, minden ezen keretrendszerek:</span><span class="sxs-lookup"><span data-stu-id="bed87-113">Set these options if your app uses any these frameworks:</span></span> 

* <span data-ttu-id="bed87-114">**.NET-keretrendszer**: Set hello .NET-keretrendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="bed87-114">**.NET Framework**: Set hello .NET framework version.</span></span> 
* <span data-ttu-id="bed87-115">**A PHP**: hello PHP verzióját, vagy **OFF** toodisable PHP.</span><span class="sxs-lookup"><span data-stu-id="bed87-115">**PHP**: Set hello PHP version, or **OFF** toodisable PHP.</span></span> 
* <span data-ttu-id="bed87-116">**Java**: Select hello Java-verziót vagy **OFF** toodisable Java.</span><span class="sxs-lookup"><span data-stu-id="bed87-116">**Java**: Select hello Java version or **OFF** toodisable Java.</span></span> <span data-ttu-id="bed87-117">Használjon hello **webes tároló** beállítás toochoose Tomcat- és Jetty verziói között.</span><span class="sxs-lookup"><span data-stu-id="bed87-117">Use hello **Web Container** option toochoose between Tomcat and Jetty versions.</span></span>
* <span data-ttu-id="bed87-118">**Python**: Select hello Python verzióját, vagy **OFF** toodisable Python.</span><span class="sxs-lookup"><span data-stu-id="bed87-118">**Python**: Select hello Python version, or **OFF** toodisable Python.</span></span>

<span data-ttu-id="bed87-119">Technikai okokból hello .NET, PHP és Python beállítások Java engedélyezése az alkalmazás letiltása.</span><span class="sxs-lookup"><span data-stu-id="bed87-119">For technical reasons, enabling Java for your app disables hello .NET, PHP, and Python options.</span></span>

<span data-ttu-id="bed87-120"><a name="platform"></a>
**Platform**.</span><span class="sxs-lookup"><span data-stu-id="bed87-120"><a name="platform"></a>
**Platform**.</span></span> <span data-ttu-id="bed87-121">Választja ki, hogy a webalkalmazás fut, 32 bites vagy 64 bites környezetben.</span><span class="sxs-lookup"><span data-stu-id="bed87-121">Selects whether your web app runs in a 32-bit or 64-bit environment.</span></span> <span data-ttu-id="bed87-122">hello 64 bites környezet Basic vagy Standard módot igényel.</span><span class="sxs-lookup"><span data-stu-id="bed87-122">hello 64-bit environment requires Basic or Standard mode.</span></span> <span data-ttu-id="bed87-123">Ingyenes, és megosztott mód mindig 32-bit-es környezetben fusson.</span><span class="sxs-lookup"><span data-stu-id="bed87-123">Free and Shared modes always run in a 32-bit environment.</span></span>

<span data-ttu-id="bed87-124">**Webes szoftvercsatornák**.</span><span class="sxs-lookup"><span data-stu-id="bed87-124">**Web Sockets**.</span></span> <span data-ttu-id="bed87-125">Állítsa be **ON** tooenable hello WebSocket protokoll; például, ha a webes alkalmazás használ [ASP.NET SignalR] vagy [socket.io].</span><span class="sxs-lookup"><span data-stu-id="bed87-125">Set **ON** tooenable hello WebSocket protocol; for example, if your web app uses [ASP.NET SignalR] or [socket.io].</span></span>

<span data-ttu-id="bed87-126"><a name="alwayson"></a>
**Always On**.</span><span class="sxs-lookup"><span data-stu-id="bed87-126"><a name="alwayson"></a>
**Always On**.</span></span> <span data-ttu-id="bed87-127">Alapértelmezés szerint a webalkalmazások a memóriából, ha bizonyos ideig inaktív.</span><span class="sxs-lookup"><span data-stu-id="bed87-127">By default, web apps are unloaded if they are idle for some period of time.</span></span> <span data-ttu-id="bed87-128">Ez lehetővé teszi, hogy az erőforrások megőrzése hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="bed87-128">This lets hello system conserve resources.</span></span> <span data-ttu-id="bed87-129">Basic vagy Standard módban, ahol engedélyezheti **mindig a** tookeep hello app betöltött összes hello idő.</span><span class="sxs-lookup"><span data-stu-id="bed87-129">In Basic or Standard mode, you can enable **Always On** tookeep hello app loaded all hello time.</span></span> <span data-ttu-id="bed87-130">Ha az alkalmazás fut a folyamatos webjobs-feladatok webjobs-feladatok futtatása indított CRON-kifejezés használatával, vagy engedélyezze **mindig a**, vagy hello webes feladatok nem futtatható megbízhatóan.</span><span class="sxs-lookup"><span data-stu-id="bed87-130">If your app runs continuous WebJobs or runs WebJobs triggered using a CRON expression, you should enable **Always On**, or hello web jobs may not run reliably.</span></span>

<span data-ttu-id="bed87-131">**Felügyelt folyamatkezelési verzió**.</span><span class="sxs-lookup"><span data-stu-id="bed87-131">**Managed Pipeline Version**.</span></span> <span data-ttu-id="bed87-132">Készletek hello IIS [folyamatkezelési mód].</span><span class="sxs-lookup"><span data-stu-id="bed87-132">Sets hello IIS [pipeline mode].</span></span> <span data-ttu-id="bed87-133">Hagyja meg a beállított tooIntegrated (hello alapértelmezett), kivéve, ha egy örökölt alkalmazás, amely az IIS egy régebbi verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="bed87-133">Leave this set tooIntegrated (hello default) unless you have a legacy app that requires an older version of IIS.</span></span>

<span data-ttu-id="bed87-134">**Az automatikus felcserélés**.</span><span class="sxs-lookup"><span data-stu-id="bed87-134">**Auto Swap**.</span></span> <span data-ttu-id="bed87-135">Ha engedélyezte az automatikus felcserélés egy üzembe helyezési tárhelyet, az App Service lesz automatikusan felcserélése hello webalkalmazás éles környezetben amikor egy frissítés toothat helyre.</span><span class="sxs-lookup"><span data-stu-id="bed87-135">If you enable Auto Swap for a deployment slot, App Service will automatically swap hello web app into production when you push an update toothat slot.</span></span> <span data-ttu-id="bed87-136">További információkért lásd: [központi telepítése az Azure App Service web Apps toostaging tárhelyek](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="bed87-136">For more information, see [Deploy toostaging slots for web apps in Azure App Service](web-sites-staged-publishing.md).</span></span>

### <a name="debugging"></a><span data-ttu-id="bed87-137">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="bed87-137">Debugging</span></span>
<span data-ttu-id="bed87-138">**Távoli hibakeresés**.</span><span class="sxs-lookup"><span data-stu-id="bed87-138">**Remote Debugging**.</span></span> <span data-ttu-id="bed87-139">Távoli hibakeresésének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bed87-139">Enables remote debugging.</span></span> <span data-ttu-id="bed87-140">Ha engedélyezve van, használhatja hello távoli hibakereső a Visual Studio tooconnect közvetlen tooyour webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bed87-140">When enabled, you can use hello remote debugger in Visual Studio tooconnect directly tooyour web app.</span></span> <span data-ttu-id="bed87-141">Távoli hibakeresés továbbra is engedélyezett marad 48 órán keresztül.</span><span class="sxs-lookup"><span data-stu-id="bed87-141">Remote debugging will remain enabled for 48 hours.</span></span> 

### <a name="app-settings"></a><span data-ttu-id="bed87-142">Alkalmazásbeállítások</span><span class="sxs-lookup"><span data-stu-id="bed87-142">App settings</span></span>
<span data-ttu-id="bed87-143">Ebben a szakaszban található név/érték párok, akkor web app betölti a kezdőlapon.</span><span class="sxs-lookup"><span data-stu-id="bed87-143">This section contains name/value pairs that you web app will load on start up.</span></span> 

* <span data-ttu-id="bed87-144">.NET-alkalmazások esetén ezek a beállítások vannak be a nézetmodellbe, a .NET-konfiguráció `AppSettings` futásidőben, felülírva a meglévő beállításokat.</span><span class="sxs-lookup"><span data-stu-id="bed87-144">For .NET apps, these settings are injected into your .NET configuration `AppSettings` at runtime, overriding existing settings.</span></span> 
* <span data-ttu-id="bed87-145">A PHP, Python, Java és csomópont alkalmazások férhetnek hozzá ezeket a beállításokat a környezeti változók futásidőben.</span><span class="sxs-lookup"><span data-stu-id="bed87-145">PHP, Python, Java and Node applications can access these settings as environment variables at runtime.</span></span> <span data-ttu-id="bed87-146">Minden egyes alkalmazás-beállítás két környezeti változók jönnek létre; egy megadott hello app beállítás bejegyzést, és egy másik APPSETTING_ előtaggal rendelkező hello nevet.</span><span class="sxs-lookup"><span data-stu-id="bed87-146">For each app setting, two environment variables are created; one with hello name specified by hello app setting entry, and another with a prefix of APPSETTING_.</span></span> <span data-ttu-id="bed87-147">Mindkettő rendelkezik hello ugyanazt az értéket.</span><span class="sxs-lookup"><span data-stu-id="bed87-147">Both contain hello same value.</span></span>

### <a name="connection-strings"></a><span data-ttu-id="bed87-148">Kapcsolati karakterláncok</span><span class="sxs-lookup"><span data-stu-id="bed87-148">Connection strings</span></span>
<span data-ttu-id="bed87-149">Kapcsolati karakterláncok kapcsolt erőforrásokban.</span><span class="sxs-lookup"><span data-stu-id="bed87-149">Connection strings for linked resources.</span></span> 

<span data-ttu-id="bed87-150">.NET-alkalmazások esetén a kapcsolati karakterláncok vannak be a nézetmodellbe, a .NET-konfiguráció `connectionStrings` beállításokat futásidőben, felülírva a meglévő bejegyzéseket, ahol hello kulcs értéke hello csatolt adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="bed87-150">For .NET apps, these connection strings are injected into your .NET configuration `connectionStrings` settings at runtime, overriding existing entries where hello key equals hello linked database name.</span></span> 

<span data-ttu-id="bed87-151">A PHP, Python, Java és csomópont alkalmazások ezek a beállítások használhatók környezeti változók előtagként hello kapcsolattípus futásidőben.</span><span class="sxs-lookup"><span data-stu-id="bed87-151">For PHP, Python, Java and Node applications, these settings will be available as environment variables at runtime, prefixed with hello connection type.</span></span> <span data-ttu-id="bed87-152">hello környezeti változó előtagok a következők:</span><span class="sxs-lookup"><span data-stu-id="bed87-152">hello environment variable prefixes are as follows:</span></span> 

* <span data-ttu-id="bed87-153">SQL Server:`SQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="bed87-153">SQL Server: `SQLCONNSTR_`</span></span>
* <span data-ttu-id="bed87-154">MySQL:`MYSQLCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="bed87-154">MySQL: `MYSQLCONNSTR_`</span></span>
* <span data-ttu-id="bed87-155">SQL-adatbázis:`SQLAZURECONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="bed87-155">SQL Database: `SQLAZURECONNSTR_`</span></span>
* <span data-ttu-id="bed87-156">Egyéni:`CUSTOMCONNSTR_`</span><span class="sxs-lookup"><span data-stu-id="bed87-156">Custom: `CUSTOMCONNSTR_`</span></span>

<span data-ttu-id="bed87-157">Például, ha a MySql-kapcsolati karakterlánc lett nevű `connectionstring1`, akkor elérhetőek hello környezeti változó `MYSQLCONNSTR_connectionString1`.</span><span class="sxs-lookup"><span data-stu-id="bed87-157">For example, if a MySql connection string were named `connectionstring1`, it would be accessed through hello environment variable `MYSQLCONNSTR_connectionString1`.</span></span>

### <a name="default-documents"></a><span data-ttu-id="bed87-158">Alapértelmezett dokumentum</span><span class="sxs-lookup"><span data-stu-id="bed87-158">Default documents</span></span>
<span data-ttu-id="bed87-159">hello alapértelmezett dokumentum egy hello weblap, akkor jelenik meg, a webhely hello gyökér URL-címen.</span><span class="sxs-lookup"><span data-stu-id="bed87-159">hello default document is hello web page that is displayed at hello root URL for a website.</span></span>  <span data-ttu-id="bed87-160">hello első egyező hello listában fájllal.</span><span class="sxs-lookup"><span data-stu-id="bed87-160">hello first matching file in hello list is used.</span></span> 

<span data-ttu-id="bed87-161">Webalkalmazások használhatja a modulok, hogy útvonal URL-címe alapján. ahelyett, hogy nincs alapértelmezett dokumentum ilyen szolgáltató statikus tartalmat, ebben az esetben nincs.</span><span class="sxs-lookup"><span data-stu-id="bed87-161">Web apps might use modules that route based on URL, rather than serving static content, in which case there is no default document as such.</span></span>    

### <a name="handler-mappings"></a><span data-ttu-id="bed87-162">Kezelőleképezések</span><span class="sxs-lookup"><span data-stu-id="bed87-162">Handler mappings</span></span>
<span data-ttu-id="bed87-163">A terület tooadd egyéni parancsfájl processzorok toohandle kérelmek adott fájlkiterjesztésekre használja.</span><span class="sxs-lookup"><span data-stu-id="bed87-163">Use this area tooadd custom script processors toohandle requests for specific file extensions.</span></span> 

* <span data-ttu-id="bed87-164">**Bővítmény**.</span><span class="sxs-lookup"><span data-stu-id="bed87-164">**Extension**.</span></span> <span data-ttu-id="bed87-165">hello fájl kiterjesztése toobe kezelni, például *.php vagy handler.fcgi.</span><span class="sxs-lookup"><span data-stu-id="bed87-165">hello file extension toobe handled, such as *.php or handler.fcgi.</span></span> 
* <span data-ttu-id="bed87-166">**Parancsfájl-feldolgozó elérési útja**.</span><span class="sxs-lookup"><span data-stu-id="bed87-166">**Script Processor Path**.</span></span> <span data-ttu-id="bed87-167">hello abszolút elérési útja hello parancsfájl-feldolgozókhoz.</span><span class="sxs-lookup"><span data-stu-id="bed87-167">hello absolute path of hello script processor.</span></span> <span data-ttu-id="bed87-168">Hello parancsprogram-feldolgozó által a kérelmek toofiles hello fájlkiterjesztés megfelelő dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="bed87-168">Requests toofiles that match hello file extension will be processed by hello script processor.</span></span> <span data-ttu-id="bed87-169">Elérési út használata hello `D:\home\site\wwwroot` toorefer tooyour alkalmazás gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="bed87-169">Use hello path `D:\home\site\wwwroot` toorefer tooyour app's root directory.</span></span>
* <span data-ttu-id="bed87-170">**További argumentumok**.</span><span class="sxs-lookup"><span data-stu-id="bed87-170">**Additional Arguments**.</span></span> <span data-ttu-id="bed87-171">Parancssori argumentumokat használni hello parancsprogram-feldolgozó</span><span class="sxs-lookup"><span data-stu-id="bed87-171">Optional command-line arguments for hello script processor</span></span> 

### <a name="virtual-applications-and-directories"></a><span data-ttu-id="bed87-172">Virtuális alkalmazások és könyvtárak</span><span class="sxs-lookup"><span data-stu-id="bed87-172">Virtual applications and directories</span></span>
<span data-ttu-id="bed87-173">tooconfigure virtuális alkalmazások és könyvtárak, adja meg az egyes virtuális könyvtárakat és az annak megfelelő fizikai elérési út relatív toohello webhely gyökeréhez.</span><span class="sxs-lookup"><span data-stu-id="bed87-173">tooconfigure virtual applications and directories, specify each virtual directory and its corresponding physical path relative toohello website root.</span></span> <span data-ttu-id="bed87-174">Másik lehetőségként választhatja hello **alkalmazás** jelölőnégyzet toomark alkalmazásként egy virtuális könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="bed87-174">Optionally, you can select hello **Application** checkbox toomark a virtual directory as an application.</span></span>

## <a name="enabling-diagnostic-logs"></a><span data-ttu-id="bed87-175">Diagnosztikai naplók engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bed87-175">Enabling diagnostic logs</span></span>
<span data-ttu-id="bed87-176">diagnosztikai naplók tooenable:</span><span class="sxs-lookup"><span data-stu-id="bed87-176">tooenable diagnostic logs:</span></span>

1. <span data-ttu-id="bed87-177">A webalkalmazás hello paneljén kattintson **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="bed87-177">In hello blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="bed87-178">Kattintson a **diagnosztikai naplók**.</span><span class="sxs-lookup"><span data-stu-id="bed87-178">Click **Diagnostic logs**.</span></span> 

<span data-ttu-id="bed87-179">Diagnosztikai naplók írása egy webalkalmazás, amely támogatja a naplózási beállítások:</span><span class="sxs-lookup"><span data-stu-id="bed87-179">Options for writing diagnostic logs from a web application that supports logging:</span></span> 

* <span data-ttu-id="bed87-180">**Alkalmazásnaplózás**.</span><span class="sxs-lookup"><span data-stu-id="bed87-180">**Application Logging**.</span></span> <span data-ttu-id="bed87-181">Írja az alkalmazásnaplók toohello fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="bed87-181">Writes application logs toohello file system.</span></span> <span data-ttu-id="bed87-182">A naplózás 12 óra ideig tart.</span><span class="sxs-lookup"><span data-stu-id="bed87-182">Logging lasts for a period of 12 hours.</span></span> 

<span data-ttu-id="bed87-183">**Szint**.</span><span class="sxs-lookup"><span data-stu-id="bed87-183">**Level**.</span></span> <span data-ttu-id="bed87-184">Ha az alkalmazásadatok naplózása engedélyezve van, ez a beállítás ennyi hello rögzített (hiba, figyelmeztetés, információ, vagy a részletes) adatokat.</span><span class="sxs-lookup"><span data-stu-id="bed87-184">When application logging is enabled, this option specifies hello amount of information that will be recorded (Error, Warning, Information, or Verbose).</span></span>

<span data-ttu-id="bed87-185">**Webkiszolgáló naplózásának**.</span><span class="sxs-lookup"><span data-stu-id="bed87-185">**Web server logging**.</span></span> <span data-ttu-id="bed87-186">Naplók hello W3C bővített naplófájlformátum lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="bed87-186">Logs are saved in hello W3C extended log file format.</span></span> 

<span data-ttu-id="bed87-187">**A részletes hibaüzeneteket**.</span><span class="sxs-lookup"><span data-stu-id="bed87-187">**Detailed error messages**.</span></span> <span data-ttu-id="bed87-188">Menti a hiba részleteit a .htm fájl üzenetek.</span><span class="sxs-lookup"><span data-stu-id="bed87-188">Saves detailed error messages .htm files.</span></span> <span data-ttu-id="bed87-189">hello mentésre /LogFiles/DetailedErrors alatt.</span><span class="sxs-lookup"><span data-stu-id="bed87-189">hello files are saved under /LogFiles/DetailedErrors.</span></span> 

<span data-ttu-id="bed87-190">**Sikertelen kérelmek követésének**.</span><span class="sxs-lookup"><span data-stu-id="bed87-190">**Failed request tracing**.</span></span> <span data-ttu-id="bed87-191">Naplók kérelmek tooXML fájlokat nem sikerült.</span><span class="sxs-lookup"><span data-stu-id="bed87-191">Logs failed requests tooXML files.</span></span> <span data-ttu-id="bed87-192">hello menti a/naplófájlok/W3SVC*xxx*, ahol a xxx egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="bed87-192">hello files are saved under /LogFiles/W3SVC*xxx*, where xxx is a unique identifier.</span></span> <span data-ttu-id="bed87-193">Ez a mappa tartalmaz egy XSL-fájlt és egy vagy több XML-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bed87-193">This folder contains an XSL file and one or more XML files.</span></span> <span data-ttu-id="bed87-194">Győződjön meg arról, hogy toodownload hello XSL-fájl, mert funkciókat biztosít a formázás és szűrés hello XML-fájlok hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="bed87-194">Make sure toodownload hello XSL file, because it provides functionality for formatting and filtering hello contents of hello XML files.</span></span>

<span data-ttu-id="bed87-195">kell tooview hello naplófájlok, FTP hitelesítő adatokat, az alábbiak szerint hozzon létre:</span><span class="sxs-lookup"><span data-stu-id="bed87-195">tooview hello log files, you must create FTP credentials, as follows:</span></span>

1. <span data-ttu-id="bed87-196">A webalkalmazás hello paneljén kattintson **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="bed87-196">In hello blade for your web app, click **All settings**.</span></span>
2. <span data-ttu-id="bed87-197">Kattintson a **üzembe helyezési hitelesítő adatok**.</span><span class="sxs-lookup"><span data-stu-id="bed87-197">Click **Deployment credentials**.</span></span>
3. <span data-ttu-id="bed87-198">Adjon meg egy felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="bed87-198">Enter a user name and password.</span></span>
4. <span data-ttu-id="bed87-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bed87-199">Click **Save**.</span></span>

![Telepítési hitelesítő adatok beállítása][configure03]

<span data-ttu-id="bed87-201">Ha a rendszer "app\username" Hello teljes FTP-felhasználó nevét *app* hello a webes alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="bed87-201">hello full FTP user name is “app\username” where *app* is hello name of your web app.</span></span> <span data-ttu-id="bed87-202">hello felhasználónév szerepel hello webalkalmazás panelen a **Essentials**.</span><span class="sxs-lookup"><span data-stu-id="bed87-202">hello username is listed in hello web app blade, under **Essentials**.</span></span>  

![FTP telepítési hitelesítő adatok][configure02]

## <a name="other-configuration-tasks"></a><span data-ttu-id="bed87-204">Egyéb konfigurációs feladatok</span><span class="sxs-lookup"><span data-stu-id="bed87-204">Other configuration tasks</span></span>
### <a name="ssl"></a><span data-ttu-id="bed87-205">SSL</span><span class="sxs-lookup"><span data-stu-id="bed87-205">SSL</span></span>
<span data-ttu-id="bed87-206">Basic vagy Standard módban SSL-tanúsítványokat az egyéni tartományt is feltölthet.</span><span class="sxs-lookup"><span data-stu-id="bed87-206">In Basic or Standard mode, you can upload SSL certificates for a custom domain.</span></span> <span data-ttu-id="bed87-207">További információ: [HTTPS engedélyezése az webalkalmazáshoz].</span><span class="sxs-lookup"><span data-stu-id="bed87-207">For more information, see [Enable HTTPS for a web app].</span></span> 

<span data-ttu-id="bed87-208">tooview a feltöltött tanúsítványok kattintson **összes beállítás** > **egyéni tartományok és SSL**.</span><span class="sxs-lookup"><span data-stu-id="bed87-208">tooview your uploaded certificates, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="domain-names"></a><span data-ttu-id="bed87-209">Tartománynevek</span><span class="sxs-lookup"><span data-stu-id="bed87-209">Domain names</span></span>
<span data-ttu-id="bed87-210">A webalkalmazás egyéni tartományneveket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="bed87-210">Add custom domain names for your web app.</span></span> <span data-ttu-id="bed87-211">További információ: [egy webalkalmazást az Azure App Service szolgáltatásban az egyéni tartománynév konfigurálása].</span><span class="sxs-lookup"><span data-stu-id="bed87-211">For more information, see [Configure a custom domain name for a web app in Azure App Service].</span></span>

<span data-ttu-id="bed87-212">tooview a tartománynevek kattintson **összes beállítás** > **egyéni tartományok és SSL**.</span><span class="sxs-lookup"><span data-stu-id="bed87-212">tooview your domain names, click **All Settings** > **Custom domains and SSL**.</span></span>

### <a name="deployments"></a><span data-ttu-id="bed87-213">Központi telepítés</span><span class="sxs-lookup"><span data-stu-id="bed87-213">Deployments</span></span>
* <span data-ttu-id="bed87-214">Folyamatos üzembe helyezést beállítani.</span><span class="sxs-lookup"><span data-stu-id="bed87-214">Set up continuous deployment.</span></span> <span data-ttu-id="bed87-215">Lásd: [az Azure App Service Web Apps használatával Git toodeploy](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="bed87-215">See [Using Git toodeploy Web Apps in Azure App Service](web-sites-deploy.md).</span></span>
* <span data-ttu-id="bed87-216">Üzembe helyezési pontok.</span><span class="sxs-lookup"><span data-stu-id="bed87-216">Deployment slots.</span></span> <span data-ttu-id="bed87-217">Lásd: [tooStaging környezetek telepítése az Azure App Service Web Apps].</span><span class="sxs-lookup"><span data-stu-id="bed87-217">See [Deploy tooStaging Environments for Web Apps in Azure App Service].</span></span>

<span data-ttu-id="bed87-218">Kattintson a tooview az üzembe helyezési **összes beállítás** > **üzembe helyezési**.</span><span class="sxs-lookup"><span data-stu-id="bed87-218">tooview your deployment slots, click **All Settings** > **Deployment slots**.</span></span>

### <a name="monitoring"></a><span data-ttu-id="bed87-219">Figyelés</span><span class="sxs-lookup"><span data-stu-id="bed87-219">Monitoring</span></span>
<span data-ttu-id="bed87-220">Basic vagy Standard módban hello rendelkezésre állási HTTP vagy HTTPS-végpontok, tesztelheti a toothree földrajzilag elosztott helyeken fel.</span><span class="sxs-lookup"><span data-stu-id="bed87-220">In Basic or Standard mode, you can  test hello availability of HTTP or HTTPS endpoints, from up toothree geo-distributed locations.</span></span> <span data-ttu-id="bed87-221">A figyelési teszt sikertelen, ha HTTP-válaszkód hello (4xx vagy 5xx) hiba vagy hello válasz több mint 30 másodpercet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="bed87-221">A monitoring test fails if hello HTTP response code is an error (4xx or 5xx) or hello response takes more than 30 seconds.</span></span> <span data-ttu-id="bed87-222">A végpont tekinthető érhető el, ha az összes sikeres hello figyelési tesztek hello megadott helyeken.</span><span class="sxs-lookup"><span data-stu-id="bed87-222">An endpoint is considered available if hello monitoring tests succeed from all hello specified locations.</span></span> 

<span data-ttu-id="bed87-223">További információkért lásd: [Útmutató: webes végpont állapotának figyelése].</span><span class="sxs-lookup"><span data-stu-id="bed87-223">For more information, see [How to: Monitor web endpoint status].</span></span>

> [!NOTE]
> <span data-ttu-id="bed87-224">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása], ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="bed87-224">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service], where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="bed87-225">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="bed87-225">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="bed87-226">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bed87-226">Next steps</span></span>
* <span data-ttu-id="bed87-227">[Egyéni tartománynév konfigurálása az Azure App Service-ben]</span><span class="sxs-lookup"><span data-stu-id="bed87-227">[Configure a custom domain name in Azure App Service]</span></span>
* <span data-ttu-id="bed87-228">[HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben]</span><span class="sxs-lookup"><span data-stu-id="bed87-228">[Enable HTTPS for an app in Azure App Service]</span></span>
* <span data-ttu-id="bed87-229">[A webalkalmazás skálázása az Azure App Service-ben]</span><span class="sxs-lookup"><span data-stu-id="bed87-229">[Scale a web app in Azure App Service]</span></span>
* <span data-ttu-id="bed87-230">[Az Azure App Service Web Apps figyelési alapjai]</span><span class="sxs-lookup"><span data-stu-id="bed87-230">[Monitoring basics for Web Apps in Azure App Service]</span></span>

<!-- URL List -->

[ASP.NET SignalR]: http://www.asp.net/signalr
[Azure Portal]: https://portal.azure.com/
[Egyéni tartománynév konfigurálása az Azure App Service-ben]: ./app-service-web-tutorial-custom-domain.md
[tooStaging környezetek telepítése az Azure App Service Web Apps]: ./web-sites-staged-publishing.md
[HTTPS engedélyezése az alkalmazásoknak az Azure App Service-ben]: ./app-service-web-tutorial-custom-ssl.md
[Útmutató: webes végpont állapotának figyelése]: http://go.microsoft.com/fwLink/?LinkID=279906
[Az Azure App Service Web Apps figyelési alapjai]: ./web-sites-monitor.md
[folyamatkezelési mód]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[A webalkalmazás skálázása az Azure App Service-ben]: ./web-sites-scale.md
[socket.io]: ./web-sites-nodejs-chat-app-socketio.md
[App Service kipróbálása]: https://azure.microsoft.com/try/app-service/

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png

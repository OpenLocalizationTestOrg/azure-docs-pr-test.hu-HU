---
title: "aaaManage egy webalkalmazást az Azure App Service-ben"
description: "Hivatkozások tooresources egy webalkalmazást az Azure App Service kezeléséhez."
services: app-service\web
documentationcenter: 
author: erikre
manager: erikre
editor: 
ms.assetid: d5e2887a-84f9-4747-a573-867635cb8b39
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: rachelap
ms.openlocfilehash: daf69245e66068b0e97e3ae1c3fb5fce45605b91
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a><span data-ttu-id="2ceb3-103">Egy webalkalmazást az Azure App Service kezelése</span><span class="sxs-lookup"><span data-stu-id="2ceb3-103">Manage a web app in Azure App Service</span></span>
<span data-ttu-id="2ceb3-104">Ez a témakör a webes alkalmazás kezelésére szolgáló hivatkozások tooresources [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-104">This topic contains links tooresources for managing a web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="2ceb3-105">Felügyeleti tartalmazza az összes a webes alkalmazás zökkenőmentes mindig hello feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-105">Management includes all of hello tasks that keep your web app running smoothly.</span></span> 

<span data-ttu-id="2ceb3-106">A webalkalmazás hello élettartama különböző felügyeleti műveleteket kell végrehajtania, a kezdeti telepítés toonormal műveletet, a karbantartási és a frissítések mozgatásakor.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-106">Over hello lifetime of a web app, you will perform different management tasks, as you move from initial deployment toonormal operation, maintenance, and updates.</span></span>

<span data-ttu-id="2ceb3-107">Számos web app feladat hello Azure-portálon végezhető.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-107">Many web app management tasks can be performed in hello Azure Portal.</span></span>

## <a name="before-you-deploy-your-web-app-tooproduction"></a><span data-ttu-id="2ceb3-108">A webes alkalmazás tooproduction központi telepítése előtt</span><span class="sxs-lookup"><span data-stu-id="2ceb3-108">Before you deploy your web app tooproduction</span></span>
### <a name="choose-a-tier"></a><span data-ttu-id="2ceb3-109">Egy szint kiválasztása</span><span class="sxs-lookup"><span data-stu-id="2ceb3-109">Choose a tier</span></span>
<span data-ttu-id="2ceb3-110">Az Azure App Service öt rétegek akkor érhető el: ingyenes, a megosztott, Basic, Standard és Premium.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-110">Azure App Service is offered in five tiers: Free, Shared, Basic, Standard, and Premium.</span></span> <span data-ttu-id="2ceb3-111">Hello szolgáltatásait, és az egyes rétegekhez díjszabási információkért lásd: [díjszabása](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-111">For information about hello features and pricing for each tier, see [Pricing details](https://azure.microsoft.com/pricing/details/app-service/).</span></span> 

* <span data-ttu-id="2ceb3-112">[App Service-csomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) hello alatt több webalkalmazások csoportosítását teszik azonos tartozó.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-112">[App Service plans](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) let you group multiple web apps under hello same tier.</span></span>
* <span data-ttu-id="2ceb3-113">Mindig is [rétegek kapcsoló](web-sites-scale.md) a webalkalmazás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-113">You can always [switch tiers](web-sites-scale.md) after you create your web app.</span></span>

### <a name="configuration"></a><span data-ttu-id="2ceb3-114">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="2ceb3-114">Configuration</span></span>
<span data-ttu-id="2ceb3-115">Használjon hello [Azure Portal](https://portal.azure.com/) tooset különböző konfigurációs beállításait.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-115">Use hello [Azure Portal](https://portal.azure.com/) tooset various configuration options.</span></span> <span data-ttu-id="2ceb3-116">További információkért lásd: [webes alkalmazások konfigurálása az Azure App Service](web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-116">For details, see [Configure web apps in Azure App Service](web-sites-configure.md).</span></span> <span data-ttu-id="2ceb3-117">Íme egy gyors Ellenőrzőlista:</span><span class="sxs-lookup"><span data-stu-id="2ceb3-117">Here is a quick checklist:</span></span>

* <span data-ttu-id="2ceb3-118">Válassza ki **futásidejű verziók** a .NET, PHP, Java vagy Python, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-118">Select **runtime versions** for .NET, PHP, Java, or Python, if needed.</span></span>
* <span data-ttu-id="2ceb3-119">Engedélyezése **websocket elemek** Ha a webalkalmazás hello WebSocket protokollt használja.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-119">Enable **WebSockets** if your web app uses hello WebSocket protocol.</span></span> <span data-ttu-id="2ceb3-120">(Ez magában foglalja a használó alkalmazások [ASP.NET SignalR](http://www.asp.net/signalr) vagy [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span><span class="sxs-lookup"><span data-stu-id="2ceb3-120">(This includes apps that use [ASP.NET SignalR](http://www.asp.net/signalr) or [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span></span>
* <span data-ttu-id="2ceb3-121">A folyamatos webjobs fut?</span><span class="sxs-lookup"><span data-stu-id="2ceb3-121">Are you running continuous web jobs?</span></span> <span data-ttu-id="2ceb3-122">Ha igen, engedélyezze a **mindig a**.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-122">If so, enable **Always On**.</span></span>
* <span data-ttu-id="2ceb3-123">Set hello **alapértelmezett dokumentum**, például a index.html.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-123">Set hello **default document**, such as index.html.</span></span>

<span data-ttu-id="2ceb3-124">Továbbá toothese alapszintű konfigurációs beállításokkal érdemes lehet tooconfigure hello következő:</span><span class="sxs-lookup"><span data-stu-id="2ceb3-124">In addition toothese basic configuration settings, you may want tooconfigure hello following:</span></span>

* <span data-ttu-id="2ceb3-125">**Secure Socket Layer (SSL)** titkosítás.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-125">**Secure Socket Layer (SSL)** encryption.</span></span> <span data-ttu-id="2ceb3-126">SSL toouse egy egyéni tartománynév, akkor be kell szereznie egy SSL tanúsítvány és a webes alkalmazás toouse konfigurálni azt.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-126">toouse SSL with a custom domain name, you must get an SSL certificate and configure your web app toouse it.</span></span> <span data-ttu-id="2ceb3-127">Lásd: [HTTPS engedélyezése az Azure App Service-ben webalkalmazáshoz](app-service-web-tutorial-custom-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-127">See [Enable HTTPS for a web app in Azure App Service](app-service-web-tutorial-custom-ssl.md).</span></span>
* <span data-ttu-id="2ceb3-128">**Egyéni tartomány nevét.**</span><span class="sxs-lookup"><span data-stu-id="2ceb3-128">**Custom domain name.**</span></span> <span data-ttu-id="2ceb3-129">A webalkalmazás automatikusan egy altartomány azurewebsites.net alatt van.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-129">Your web app automatically has a subdomain under azurewebsites.net.</span></span> <span data-ttu-id="2ceb3-130">Egy egyéni tartománynevet, például contoso.com lehet társítani. Lásd: [egyéni tartománynév beállítása az Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-130">You can associate a custom domain name, such as contoso.com. See [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span>

<span data-ttu-id="2ceb3-131">Nyelvspecifikus konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="2ceb3-131">Language-specific configuration:</span></span>

* <span data-ttu-id="2ceb3-132">**A PHP**: [PHP konfigurálása az Azure App Service Web Apps](web-sites-php-configure.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-132">**PHP**: [Configure PHP in Azure App Service Web Apps](web-sites-php-configure.md).</span></span>
* <span data-ttu-id="2ceb3-133">**Python**: [Python konfigurálása az Azure App Service Web Apps](web-sites-python-configure.md)</span><span class="sxs-lookup"><span data-stu-id="2ceb3-133">**Python**: [Configuring Python with Azure App Service Web Apps](web-sites-python-configure.md)</span></span>

## <a name="while-your-web-app-is-running"></a><span data-ttu-id="2ceb3-134">A webes alkalmazás futtatása közben</span><span class="sxs-lookup"><span data-stu-id="2ceb3-134">While your web app is running</span></span>
<span data-ttu-id="2ceb3-135">A webalkalmazás működik, amíg azt szeretné, akkor érhető el, és hogy toomeet felhasználói forgalom méretezés toomake.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-135">While your web app is running, you want toomake sure it is available, and that it scales toomeet user traffic.</span></span> <span data-ttu-id="2ceb3-136">Tootroubleshoot hibákat is szükség lehet.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-136">You may also need tootroubleshoot errors.</span></span>

### <a name="monitoring"></a><span data-ttu-id="2ceb3-137">Figyelés</span><span class="sxs-lookup"><span data-stu-id="2ceb3-137">Monitoring</span></span>
* <span data-ttu-id="2ceb3-138">Hello Azure portál, keresztül is [metrikák hozzáadása](web-sites-monitor.md) például CPU-használat és ügyfél-kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-138">Through hello Azure Portal, you can [add performance metrics](web-sites-monitor.md) such as CPU usage and number of client requests.</span></span>
* <span data-ttu-id="2ceb3-139">[A webalkalmazás skálázása](web-sites-scale.md) a válasz tootraffic.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-139">[Scale your web app](web-sites-scale.md) in response tootraffic.</span></span> <span data-ttu-id="2ceb3-140">Attól függően, hogy a réteg méretezheti hello virtuális gépek száma és/vagy a Virtuálisgép-példányok hello hello méretét.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-140">Depending on your tier, you can scale hello number of VMs and/or hello size of hello VM instances.</span></span> <span data-ttu-id="2ceb3-141">Hello Standard és prémium csomag szükséges is beállíthat automatikus skálázást, így a webes alkalmazás automatikusan méretezi a meghatározott ütemezés szerint vagy válasz tooload.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-141">In hello Standard and Premium tiers, you can also set up autoscaling, so your web app scales automatically, either on a fixed schedule, or in response tooload.</span></span>  

### <a name="backups"></a><span data-ttu-id="2ceb3-142">Biztonsági másolatok</span><span class="sxs-lookup"><span data-stu-id="2ceb3-142">Backups</span></span>
* <span data-ttu-id="2ceb3-143">Állítsa be [automatikus biztonsági mentésekhez](web-sites-backup.md) webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-143">Set [automatic backups](web-sites-backup.md) of your web app.</span></span> <span data-ttu-id="2ceb3-144">További információ a biztonsági másolatok [Ez a videó](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-144">Learn more about backups in [this video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span></span>
* <span data-ttu-id="2ceb3-145">További tudnivalók a hello-beállítások [adatbázis helyreállítás](../sql-database/sql-database-business-continuity.md) az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-145">Learn about hello options for [database recovery](../sql-database/sql-database-business-continuity.md) in Azure SQL Database.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="2ceb3-146">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="2ceb3-146">Troubleshooting</span></span>
* <span data-ttu-id="2ceb3-147">Ha valamilyen hiba, akkor [hibaelhárítása a Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), a diagnosztikai naplók segítségével, és élő hello felhőbeli hibakeresést.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-147">If something goes wrong, you can [troubleshoot in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), using diagnostic logs and live debugging in hello cloud.</span></span> 
* <span data-ttu-id="2ceb3-148">Visual Studio kívül vannak különböző módokon toocollect diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-148">Outside of Visual Studio, there are various ways toocollect diagnostic logs.</span></span> <span data-ttu-id="2ceb3-149">Lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-149">See [Enable diagnostics logging for web apps in Azure App Service](web-sites-enable-diagnostic-log.md).</span></span>
* <span data-ttu-id="2ceb3-150">Node.js-alkalmazások, lásd: [hogyan toodebug egy Node.js webalkalmazás az Azure App Service](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-150">For Node.js applications, see [How toodebug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

### <a name="restoring-data"></a><span data-ttu-id="2ceb3-151">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="2ceb3-151">Restoring Data</span></span>
* <span data-ttu-id="2ceb3-152">[Visszaállítás](web-sites-restore.md) egy korábban mentett webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-152">[Restore](web-sites-restore.md) a web app that was previously backed up.</span></span>

## <a name="when-you-update-your-web-app"></a><span data-ttu-id="2ceb3-153">A webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="2ceb3-153">When you update your web app</span></span>
<span data-ttu-id="2ceb3-154">Ha nem engedélyezte az automatikus biztonsági mentésekhez, létrehozhat egy [manuális biztonsági mentés](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-154">If you have not enabled automatic backups, you can create a [manual backup](web-sites-backup.md).</span></span>

<span data-ttu-id="2ceb3-155">Érdemes lehet [előkészített központi telepítési](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="2ceb3-155">Consider using [staged deployment](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="2ceb3-156">Ez a beállítás lehetővé teszi a frissítések tooa egymás mellett futó telepítési átmeneti közzététele a éles telepítés.</span><span class="sxs-lookup"><span data-stu-id="2ceb3-156">This option lets you publish updates tooa staging deployment that runs side-by-side with your production deployment.</span></span> 


<!-- Anchors. -->

[Before you deploy your site tooproduction]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website



---
title: "Egy webalkalmazást az Azure App Service kezelése"
description: "Az Azure App Service webalkalmazás kezelésére szolgáló erőforrások hivatkozásait."
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
ms.openlocfilehash: 9e19618a1b24bbdf3163ddfc3423c5c932dcd7af
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="manage-a-web-app-in-azure-app-service"></a><span data-ttu-id="5c72e-103">Egy webalkalmazást az Azure App Service kezelése</span><span class="sxs-lookup"><span data-stu-id="5c72e-103">Manage a web app in Azure App Service</span></span>
<span data-ttu-id="5c72e-104">Ez a témakör a webes alkalmazás kezelésével kapcsolatos forrásokra mutató hivatkozásokat tartalmaz [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="5c72e-104">This topic contains links to resources for managing a web app in [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span> <span data-ttu-id="5c72e-105">Felügyeleti tartalmaz minden olyan feladat, amely a webes alkalmazás zökkenőmentes tárolni.</span><span class="sxs-lookup"><span data-stu-id="5c72e-105">Management includes all of the tasks that keep your web app running smoothly.</span></span> 

<span data-ttu-id="5c72e-106">A webes alkalmazás életciklusa alatt különböző felügyeleti műveleteket kell végrehajtania, normál működés, a karbantartási és a frissítések kezdeti telepítéséből mozgatása.</span><span class="sxs-lookup"><span data-stu-id="5c72e-106">Over the lifetime of a web app, you will perform different management tasks, as you move from initial deployment to normal operation, maintenance, and updates.</span></span>

<span data-ttu-id="5c72e-107">Számos web app felügyeleti feladatot az Azure portálon hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="5c72e-107">Many web app management tasks can be performed in the Azure Portal.</span></span>

## <a name="before-you-deploy-your-web-app-to-production"></a><span data-ttu-id="5c72e-108">A webalkalmazás üzemi központi telepítése előtt</span><span class="sxs-lookup"><span data-stu-id="5c72e-108">Before you deploy your web app to production</span></span>
### <a name="choose-a-tier"></a><span data-ttu-id="5c72e-109">Egy szint kiválasztása</span><span class="sxs-lookup"><span data-stu-id="5c72e-109">Choose a tier</span></span>
<span data-ttu-id="5c72e-110">Az Azure App Service öt rétegek akkor érhető el: ingyenes, a megosztott, Basic, Standard és Premium.</span><span class="sxs-lookup"><span data-stu-id="5c72e-110">Azure App Service is offered in five tiers: Free, Shared, Basic, Standard, and Premium.</span></span> <span data-ttu-id="5c72e-111">A szolgáltatások és az egyes rétegekhez árképzési kapcsolatos információkért lásd: [díjszabása](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="5c72e-111">For information about the features and pricing for each tier, see [Pricing details](https://azure.microsoft.com/pricing/details/app-service/).</span></span> 

* <span data-ttu-id="5c72e-112">[App Service-csomagok](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) lehetővé teszik, hogy több webalkalmazások alatt azonos tartozó csoport.</span><span class="sxs-lookup"><span data-stu-id="5c72e-112">[App Service plans](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) let you group multiple web apps under the same tier.</span></span>
* <span data-ttu-id="5c72e-113">Mindig is [rétegek kapcsoló](web-sites-scale.md) a webalkalmazás létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="5c72e-113">You can always [switch tiers](web-sites-scale.md) after you create your web app.</span></span>

### <a name="configuration"></a><span data-ttu-id="5c72e-114">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="5c72e-114">Configuration</span></span>
<span data-ttu-id="5c72e-115">Használja a [Azure Portal](https://portal.azure.com/) különböző konfigurációs beállítások megadása.</span><span class="sxs-lookup"><span data-stu-id="5c72e-115">Use the [Azure Portal](https://portal.azure.com/) to set various configuration options.</span></span> <span data-ttu-id="5c72e-116">További információkért lásd: [webes alkalmazások konfigurálása az Azure App Service](web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-116">For details, see [Configure web apps in Azure App Service](web-sites-configure.md).</span></span> <span data-ttu-id="5c72e-117">Íme egy gyors Ellenőrzőlista:</span><span class="sxs-lookup"><span data-stu-id="5c72e-117">Here is a quick checklist:</span></span>

* <span data-ttu-id="5c72e-118">Válassza ki **futásidejű verziók** a .NET, PHP, Java vagy Python, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="5c72e-118">Select **runtime versions** for .NET, PHP, Java, or Python, if needed.</span></span>
* <span data-ttu-id="5c72e-119">Engedélyezése **websocket elemek** Ha a webes alkalmazást használ a WebSocket protokoll.</span><span class="sxs-lookup"><span data-stu-id="5c72e-119">Enable **WebSockets** if your web app uses the WebSocket protocol.</span></span> <span data-ttu-id="5c72e-120">(Ez magában foglalja a használó alkalmazások [ASP.NET SignalR](http://www.asp.net/signalr) vagy [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span><span class="sxs-lookup"><span data-stu-id="5c72e-120">(This includes apps that use [ASP.NET SignalR](http://www.asp.net/signalr) or [socket.io](web-sites-nodejs-chat-app-socketio.md).)</span></span>
* <span data-ttu-id="5c72e-121">A folyamatos webjobs fut?</span><span class="sxs-lookup"><span data-stu-id="5c72e-121">Are you running continuous web jobs?</span></span> <span data-ttu-id="5c72e-122">Ha igen, engedélyezze a **mindig a**.</span><span class="sxs-lookup"><span data-stu-id="5c72e-122">If so, enable **Always On**.</span></span>
* <span data-ttu-id="5c72e-123">Állítsa be a **alapértelmezett dokumentum**, például a index.html.</span><span class="sxs-lookup"><span data-stu-id="5c72e-123">Set the **default document**, such as index.html.</span></span>

<span data-ttu-id="5c72e-124">A alapszintű konfigurációs beállítások mellett célszerű konfigurálja a következőket:</span><span class="sxs-lookup"><span data-stu-id="5c72e-124">In addition to these basic configuration settings, you may want to configure the following:</span></span>

* <span data-ttu-id="5c72e-125">**Secure Socket Layer (SSL)** titkosítás.</span><span class="sxs-lookup"><span data-stu-id="5c72e-125">**Secure Socket Layer (SSL)** encryption.</span></span> <span data-ttu-id="5c72e-126">SSL használata egy egyéni tartománynevet, SSL-tanúsítvány beszerzése és a webalkalmazás a használatára konfigurálnia kell.</span><span class="sxs-lookup"><span data-stu-id="5c72e-126">To use SSL with a custom domain name, you must get an SSL certificate and configure your web app to use it.</span></span> <span data-ttu-id="5c72e-127">Lásd: [HTTPS engedélyezése az Azure App Service-ben webalkalmazáshoz](app-service-web-tutorial-custom-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-127">See [Enable HTTPS for a web app in Azure App Service](app-service-web-tutorial-custom-ssl.md).</span></span>
* <span data-ttu-id="5c72e-128">**Egyéni tartomány nevét.**</span><span class="sxs-lookup"><span data-stu-id="5c72e-128">**Custom domain name.**</span></span> <span data-ttu-id="5c72e-129">A webalkalmazás automatikusan egy altartomány azurewebsites.net alatt van.</span><span class="sxs-lookup"><span data-stu-id="5c72e-129">Your web app automatically has a subdomain under azurewebsites.net.</span></span> <span data-ttu-id="5c72e-130">Egy egyéni tartománynevet, például contoso.com lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="5c72e-130">You can associate a custom domain name, such as contoso.com.</span></span> <span data-ttu-id="5c72e-131">Lásd: [egyéni tartománynév beállítása az Azure App Service](app-service-web-tutorial-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-131">See [Configure a custom domain name in Azure App Service](app-service-web-tutorial-custom-domain.md).</span></span>

<span data-ttu-id="5c72e-132">Nyelvspecifikus konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="5c72e-132">Language-specific configuration:</span></span>

* <span data-ttu-id="5c72e-133">**A PHP**: [PHP konfigurálása az Azure App Service Web Apps](web-sites-php-configure.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-133">**PHP**: [Configure PHP in Azure App Service Web Apps](web-sites-php-configure.md).</span></span>
* <span data-ttu-id="5c72e-134">**Python**: [Python konfigurálása az Azure App Service Web Apps](web-sites-python-configure.md)</span><span class="sxs-lookup"><span data-stu-id="5c72e-134">**Python**: [Configuring Python with Azure App Service Web Apps](web-sites-python-configure.md)</span></span>

## <a name="while-your-web-app-is-running"></a><span data-ttu-id="5c72e-135">A webes alkalmazás futtatása közben</span><span class="sxs-lookup"><span data-stu-id="5c72e-135">While your web app is running</span></span>
<span data-ttu-id="5c72e-136">Miközben a webalkalmazás fut, győződjön meg arról, akkor érhető el, és hogy felhasználói forgalom megfelelő méretezés szeretné.</span><span class="sxs-lookup"><span data-stu-id="5c72e-136">While your web app is running, you want to make sure it is available, and that it scales to meet user traffic.</span></span> <span data-ttu-id="5c72e-137">Is szükség lehet hibák elhárítása.</span><span class="sxs-lookup"><span data-stu-id="5c72e-137">You may also need to troubleshoot errors.</span></span>

### <a name="monitoring"></a><span data-ttu-id="5c72e-138">Figyelés</span><span class="sxs-lookup"><span data-stu-id="5c72e-138">Monitoring</span></span>
* <span data-ttu-id="5c72e-139">Az Azure portálon keresztül is [metrikák hozzáadása](web-sites-monitor.md) például CPU-használat és ügyfél-kérelmek számát jelenti.</span><span class="sxs-lookup"><span data-stu-id="5c72e-139">Through the Azure Portal, you can [add performance metrics](web-sites-monitor.md) such as CPU usage and number of client requests.</span></span>
* <span data-ttu-id="5c72e-140">[A webalkalmazás skálázása](web-sites-scale.md) forgalom adott válaszként.</span><span class="sxs-lookup"><span data-stu-id="5c72e-140">[Scale your web app](web-sites-scale.md) in response to traffic.</span></span> <span data-ttu-id="5c72e-141">Attól függően, hogy a réteg a virtuális gépek számát és/vagy a Virtuálisgép-példány mérete méretezheti.</span><span class="sxs-lookup"><span data-stu-id="5c72e-141">Depending on your tier, you can scale the number of VMs and/or the size of the VM instances.</span></span> <span data-ttu-id="5c72e-142">A Standard és Premium rétegek is beállíthat automatikus skálázást, a webes alkalmazás automatikusan, méretezi a meghatározott ütemezés szerint vagy betölteni válaszként.</span><span class="sxs-lookup"><span data-stu-id="5c72e-142">In the Standard and Premium tiers, you can also set up autoscaling, so your web app scales automatically, either on a fixed schedule, or in response to load.</span></span>  

### <a name="backups"></a><span data-ttu-id="5c72e-143">Biztonsági másolatok</span><span class="sxs-lookup"><span data-stu-id="5c72e-143">Backups</span></span>
* <span data-ttu-id="5c72e-144">Állítsa be [automatikus biztonsági mentésekhez](web-sites-backup.md) webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="5c72e-144">Set [automatic backups](web-sites-backup.md) of your web app.</span></span> <span data-ttu-id="5c72e-145">További információ a biztonsági másolatok [Ez a videó](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span><span class="sxs-lookup"><span data-stu-id="5c72e-145">Learn more about backups in [this video](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).</span></span>
* <span data-ttu-id="5c72e-146">További tudnivalók a beállítások [adatbázis helyreállítás](../sql-database/sql-database-business-continuity.md) az Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="5c72e-146">Learn about the options for [database recovery](../sql-database/sql-database-business-continuity.md) in Azure SQL Database.</span></span>

### <a name="troubleshooting"></a><span data-ttu-id="5c72e-147">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="5c72e-147">Troubleshooting</span></span>
* <span data-ttu-id="5c72e-148">Ha valamilyen hiba, akkor [hibaelhárítása a Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), a diagnosztikai naplók segítségével, és működés közbeni hibakeresés a felhőben.</span><span class="sxs-lookup"><span data-stu-id="5c72e-148">If something goes wrong, you can [troubleshoot in Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), using diagnostic logs and live debugging in the cloud.</span></span> 
* <span data-ttu-id="5c72e-149">Visual Studio kívül különböző módja van diagnosztikai naplók gyűjtésére.</span><span class="sxs-lookup"><span data-stu-id="5c72e-149">Outside of Visual Studio, there are various ways to collect diagnostic logs.</span></span> <span data-ttu-id="5c72e-150">Lásd: [az Azure App Service web Apps diagnosztikai naplózás engedélyezése](web-sites-enable-diagnostic-log.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-150">See [Enable diagnostics logging for web apps in Azure App Service](web-sites-enable-diagnostic-log.md).</span></span>
* <span data-ttu-id="5c72e-151">Node.js-alkalmazások, lásd: [az Azure App Service a Node.js webalkalmazás hibakeresése](web-sites-nodejs-debug.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-151">For Node.js applications, see [How to debug a Node.js web app in Azure App Service](web-sites-nodejs-debug.md).</span></span>

### <a name="restoring-data"></a><span data-ttu-id="5c72e-152">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="5c72e-152">Restoring Data</span></span>
* <span data-ttu-id="5c72e-153">[Visszaállítás](web-sites-restore.md) egy korábban mentett webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5c72e-153">[Restore](web-sites-restore.md) a web app that was previously backed up.</span></span>

## <a name="when-you-update-your-web-app"></a><span data-ttu-id="5c72e-154">A webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="5c72e-154">When you update your web app</span></span>
<span data-ttu-id="5c72e-155">Ha nem engedélyezte az automatikus biztonsági mentésekhez, létrehozhat egy [manuális biztonsági mentés](web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-155">If you have not enabled automatic backups, you can create a [manual backup](web-sites-backup.md).</span></span>

<span data-ttu-id="5c72e-156">Érdemes lehet [előkészített központi telepítési](web-sites-staged-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="5c72e-156">Consider using [staged deployment](web-sites-staged-publishing.md).</span></span> <span data-ttu-id="5c72e-157">Ez a beállítás lehetővé teszi egy átmeneti telepítéshez egymás mellett futó közzétenni a frissítéseket a éles telepítés.</span><span class="sxs-lookup"><span data-stu-id="5c72e-157">This option lets you publish updates to a staging deployment that runs side-by-side with your production deployment.</span></span> 


<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website



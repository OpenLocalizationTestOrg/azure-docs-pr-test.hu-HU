---
title: "Vertikális felskálázás egy alkalmazást az Azure-ban |} Microsoft Docs"
description: "Megtudhatja, hogyan vertikális felskálázás az Azure App Service kapacitás és szolgáltatások hozzáadása egy alkalmazáshoz."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2016
ms.author: cephalin
ms.openlocfilehash: 75ddbacbd4dd14597b786d26f0730477f6c85811
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scale-up-an-app-in-azure"></a><span data-ttu-id="8988e-103">Vertikális felskálázás egy alkalmazást az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="8988e-103">Scale up an app in Azure</span></span>
<span data-ttu-id="8988e-104">Ez a cikk bemutatja, hogyan alkalmazás skálázása az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="8988e-104">This article shows you how to scale your app in Azure App Service.</span></span> <span data-ttu-id="8988e-105">Két munkafolyamatok méretezési, bővített fel és kibővítési, és ez a cikk azt ismerteti, a munkafolyamat felskálázott.</span><span class="sxs-lookup"><span data-stu-id="8988e-105">There are two workflows for scaling, scale up and scale out, and this article explains the scale up workflow.</span></span>

* <span data-ttu-id="8988e-106">[Vertikális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): több Processzor, memória, lemezterület és extra funkciók, például dedikált virtuális gépek (VM), egyedi tartományok és tanúsítványok, előkészítési tárhelyek, automatikus skálázás és több beolvasása.</span><span class="sxs-lookup"><span data-stu-id="8988e-106">[Scale up](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Get more CPU, memory, disk space, and extra features like dedicated virtual machines (VMs), custom domains and certificates, staging slots, autoscaling, and more.</span></span> <span data-ttu-id="8988e-107">Vertikális felskálázás az alkalmazáshoz tartozó App Service-csomag tarifacsomagját módosításával.</span><span class="sxs-lookup"><span data-stu-id="8988e-107">You scale up by changing the pricing tier of the App Service plan that your app belongs to.</span></span>
* <span data-ttu-id="8988e-108">[Horizontális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): növelje az alkalmazást futtató Virtuálisgép-példányok számát.</span><span class="sxs-lookup"><span data-stu-id="8988e-108">[Scale out](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Increase the number of VM instances that run your app.</span></span>
  <span data-ttu-id="8988e-109">Ki lehet terjeszteni akár 20 példányokhoz, attól függően, hogy a tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="8988e-109">You can scale out to as many as 20 instances, depending on your pricing tier.</span></span> <span data-ttu-id="8988e-110">[App Service Environment-környezetek](../app-service/app-service-app-service-environments-readme.md) a **prémium** réteg tovább növeli a kibővíthető számát, hogy legfeljebb 50 példányt.</span><span class="sxs-lookup"><span data-stu-id="8988e-110">[App Service Environments](../app-service/app-service-app-service-environments-readme.md) in **Premium** tier will further increase your scale-out count to 50 instances.</span></span> <span data-ttu-id="8988e-111">Kiterjesztésére vonatkozó további információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="8988e-111">For more information about scaling out, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md).</span></span> <span data-ttu-id="8988e-112">Nem található az automatikus skálázást, amely a példányok száma alapján automatikusan előre meghatározott szabályok és ütemezések méretezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="8988e-112">There you will find out how to use autoscaling, which is to scale instance count automatically based on predefined rules and schedules.</span></span>

<span data-ttu-id="8988e-113">A skálázási beállításokat másodpercre csak alkalmazásához, és hatással lévő összes alkalmazásra a [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8988e-113">The scale settings take only seconds to apply and affect all apps in your [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
<span data-ttu-id="8988e-114">Nem kell módosítania kell a kódot, vagy telepítse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8988e-114">They do not require you to change your code or redeploy your application.</span></span>

<span data-ttu-id="8988e-115">A díjszabás és az App Service-csomagokról szolgáltatásokkal kapcsolatos további információkért lásd: [App Service díjszabás](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="8988e-115">For information about the pricing and features of individual App Service plans, see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>  

> [!NOTE]
> <span data-ttu-id="8988e-116">Az App Service-csomagot váltottunk az **szabad** réteg, el kell távolítania a [költségkeretek](https://azure.microsoft.com/pricing/spending-limits/) biztosítani az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="8988e-116">Before you switch an App Service plan from the **Free** tier, you must first remove the [spending limits](https://azure.microsoft.com/pricing/spending-limits/) in place for your Azure subscription.</span></span> <span data-ttu-id="8988e-117">Beállításainak megtekintése vagy módosítása a Microsoft Azure App Service előfizetési, lásd: [Microsoft Azure-előfizetések][azuresubscriptions].</span><span class="sxs-lookup"><span data-stu-id="8988e-117">To view or change options for your Microsoft Azure App Service subscription, see [Microsoft Azure Subscriptions][azuresubscriptions].</span></span>
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a><span data-ttu-id="8988e-118">Vertikális felskálázás tarifacsomag</span><span class="sxs-lookup"><span data-stu-id="8988e-118">Scale up your pricing tier</span></span>
1. <span data-ttu-id="8988e-119">A böngészőben nyissa meg a [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="8988e-119">In your browser, open the [Azure portal][portal].</span></span>
2. <span data-ttu-id="8988e-120">Az alkalmazás paneljén kattintson **összes beállítás**, és kattintson a **vertikális Felskálázás**.</span><span class="sxs-lookup"><span data-stu-id="8988e-120">In your app's blade, click **All settings**, and then click **Scale Up**.</span></span>
   
    ![Navigáljon az Azure alkalmazás növelheti.][ChooseWHP]
3. <span data-ttu-id="8988e-122">Válassza ki a réteg, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8988e-122">Choose your tier, and then click **Select**.</span></span>
   
    <span data-ttu-id="8988e-123">A **értesítések** lapon villogjon, egy zöld **sikeres** a művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="8988e-123">The **Notifications** tab will flash a green **SUCCESS** after the operation is complete.</span></span>

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a><span data-ttu-id="8988e-124">Kapcsolódó erőforrások méretezése</span><span class="sxs-lookup"><span data-stu-id="8988e-124">Scale related resources</span></span>
<span data-ttu-id="8988e-125">Ha az alkalmazás egyéb szolgáltatások, például az Azure SQL Database vagy az Azure Storage függ is méretezheti fel ezeket az erőforrásokat a igények alapján.</span><span class="sxs-lookup"><span data-stu-id="8988e-125">If your app depends on other services, such as Azure SQL Database or Azure Storage, you can also scale up those resources based on your needs.</span></span> <span data-ttu-id="8988e-126">Ezeket az erőforrásokat az App Service-csomag nincs korlátozva, és külön-külön lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="8988e-126">These resources are not scaled with the App Service plan and must be scaled separately.</span></span>

1. <span data-ttu-id="8988e-127">A **Essentials**, kattintson a **erőforráscsoport** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="8988e-127">In **Essentials**, click the **Resource group** link.</span></span>
   
    ![Vertikális felskálázás az Azure alkalmazás kapcsolódó erőforrások](./media/web-sites-scale/RGEssentialsLink.png)
2. <span data-ttu-id="8988e-129">Az a **összegzés** része a **erőforráscsoport** paneljén kattintson egy skálázni kívánt erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8988e-129">In the **Summary** part of the **Resource group** blade, click a resource that you want to scale.</span></span> <span data-ttu-id="8988e-130">Az alábbi képernyőfelvételen látható egy SQL-adatbázis és egy Azure Storage típusú erőforrást.</span><span class="sxs-lookup"><span data-stu-id="8988e-130">The following screenshot shows a SQL Database resource and an Azure Storage resource.</span></span>
   
    ![Keresse meg a vertikális felskálázás az Azure alkalmazás erőforráscsoport panel](./media/web-sites-scale/ResourceGroup.png)
3. <span data-ttu-id="8988e-132">Egy SQL-adatbázis erőforrás kattintson **beállítások** > **tarifacsomag** méretezni a tarifacsomag.</span><span class="sxs-lookup"><span data-stu-id="8988e-132">For a SQL Database resource, click **Settings** > **Pricing tier** to scale the pricing tier.</span></span>
   
    ![Vertikális felskálázás az Azure alkalmazás az SQL-adatbázis háttér](./media/web-sites-scale/ScaleDatabase.png)
   
    <span data-ttu-id="8988e-134">Is bekapcsolhatja a [georeplikáció](../sql-database/sql-database-geo-replication-overview.md) az SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="8988e-134">You can also turn on [geo-replication](../sql-database/sql-database-geo-replication-overview.md) for your SQL Database instance.</span></span>
   
    <span data-ttu-id="8988e-135">Egy Azure Storage erőforrás kattintson **beállítások** > **konfigurációs** méretezése a tárolási beállítások konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="8988e-135">For an Azure Storage resource, click **Settings** > **Configuration** to scale up your storage options.</span></span>
   
    ![Vertikális felskálázás az Azure-alkalmazás által használt Azure Storage-fiókban](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a><span data-ttu-id="8988e-137">Fejlesztői szolgáltatásainak megismerése</span><span class="sxs-lookup"><span data-stu-id="8988e-137">Learn about developer features</span></span>
<span data-ttu-id="8988e-138">Attól függően, hogy ez a tarifacsomag a következő fejlesztőknek készített funkciók érhetők el:</span><span class="sxs-lookup"><span data-stu-id="8988e-138">Depending on the pricing tier, the following developer-oriented features are available:</span></span>

### <a name="bitness"></a><span data-ttu-id="8988e-139">Bitszámának</span><span class="sxs-lookup"><span data-stu-id="8988e-139">Bitness</span></span>
* <span data-ttu-id="8988e-140">A **alapvető**, **szabványos**, és **prémium** rétegek támogatja a 64 bites és 32 bites alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="8988e-140">The **Basic**, **Standard**, and **Premium** tiers support 64-bit and 32-bit applications.</span></span>
* <span data-ttu-id="8988e-141">A **szabad** és **megosztott** terv rétegek csak a 32 bites alkalmazások támogatásához.</span><span class="sxs-lookup"><span data-stu-id="8988e-141">The **Free** and **Shared** plan tiers support 32-bit applications only.</span></span>

### <a name="debugger-support"></a><span data-ttu-id="8988e-142">Hibakereső támogatás</span><span class="sxs-lookup"><span data-stu-id="8988e-142">Debugger support</span></span>
* <span data-ttu-id="8988e-143">Hibakereső támogatás érhető el a **szabad**, **megosztott**, és **alapvető** módot, az App Service-csomag egy kapcsolattípust.</span><span class="sxs-lookup"><span data-stu-id="8988e-143">Debugger support is available for the **Free**, **Shared**, and **Basic** modes at one connection per App Service plan.</span></span>
* <span data-ttu-id="8988e-144">Hibakereső támogatás érhető el a **szabványos** és **prémium** módot, az App Service-csomag öt egyidejű kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="8988e-144">Debugger support is available for the **Standard** and **Premium** modes at five concurrent connections per App Service plan.</span></span>

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a><span data-ttu-id="8988e-145">Egyéb funkciókkal kapcsolatos tudnivalók</span><span class="sxs-lookup"><span data-stu-id="8988e-145">Learn about other features</span></span>
* <span data-ttu-id="8988e-146">További információk az App Service a fennmaradó funkciók csomagokat, beleértve a díjszabás és lehetőségeket az összes felhasználóra (beleértve a fejlesztők), lásd: [App Service díjszabás](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="8988e-146">For detailed information about all of the remaining features in the App Service plans, including pricing and features of interest to all users (including developers), see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>

> [!NOTE]
> <span data-ttu-id="8988e-147">Ismerkedés az Azure App Service szolgáltatásban, az az Azure-fiók regisztrálása előtt szeretné, ha Ugrás [App Service kipróbálása](https://azure.microsoft.com/try/app-service/) ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="8988e-147">If you want to get started with Azure App Service before you sign up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/) where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="8988e-148">Nincs bankkártyára szükséges találhatók és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="8988e-148">No credit cards are required and there are no commitments.</span></span>
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a><span data-ttu-id="8988e-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8988e-149">Next steps</span></span>
* <span data-ttu-id="8988e-150">Ismerkedés az Azure-ral, lásd: [Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8988e-150">To get started with Azure, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8988e-151">Árképzési, támogatást és szolgáltatásiszint-szerződés kapcsolatos információkért látogasson el a következő hivatkozások.</span><span class="sxs-lookup"><span data-stu-id="8988e-151">For information about pricing, support, and SLA, visit the following links.</span></span>
  
    [<span data-ttu-id="8988e-152">Adatátvitel díjszabása</span><span class="sxs-lookup"><span data-stu-id="8988e-152">Data Transfers Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [<span data-ttu-id="8988e-153">A Microsoft Azure-támogatás ügyfeleknek</span><span class="sxs-lookup"><span data-stu-id="8988e-153">Microsoft Azure Support Plans</span></span>](https://azure.microsoft.com/support/plans/)
  
    [<span data-ttu-id="8988e-154">Szolgáltatói szerződések</span><span class="sxs-lookup"><span data-stu-id="8988e-154">Service Level Agreements</span></span>](https://azure.microsoft.com/support/legal/sla/)
  
    [<span data-ttu-id="8988e-155">SQL adatbázis díjszabása</span><span class="sxs-lookup"><span data-stu-id="8988e-155">SQL Database Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/sql-database/)
  
    <span data-ttu-id="8988e-156">[Virtuális gépek és Felhőszolgáltatások mérete a Microsoft Azure][vmsizes]</span><span class="sxs-lookup"><span data-stu-id="8988e-156">[Virtual Machine and Cloud Service Sizes for Microsoft Azure][vmsizes]</span></span>
  
    [<span data-ttu-id="8988e-157">Az App Service díjszabás</span><span class="sxs-lookup"><span data-stu-id="8988e-157">App Service Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/app-service/)
  
    [<span data-ttu-id="8988e-158">Az App Service díjszabás részletei – SSL-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="8988e-158">App Service Pricing Details - SSL Connections</span></span>](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* <span data-ttu-id="8988e-159">Azure App Service információ gyakorlati tanácsok felépítése méretezhető és rugalmas architektúra, beleértve: [gyakorlati tanácsok: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span><span class="sxs-lookup"><span data-stu-id="8988e-159">For information about Azure App Service best practices, including building a scalable and resilient architecture, see [Best Practices: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span></span>
* <span data-ttu-id="8988e-160">App Service apps méretezésével kapcsolatos videók lásd a következőket:</span><span class="sxs-lookup"><span data-stu-id="8988e-160">For videos about scaling App Service apps, see the following resources:</span></span>
  
  * [<span data-ttu-id="8988e-161">Mikor érdemes méretezni az Azure - lengyel Schackow webhelyek</span><span class="sxs-lookup"><span data-stu-id="8988e-161">When to Scale Azure Websites - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [<span data-ttu-id="8988e-162">Az automatikus skálázás Azure-webhelyek, CPU vagy ütemezett - rendelkező lengyel Schackow</span><span class="sxs-lookup"><span data-stu-id="8988e-162">Auto Scaling Azure Websites, CPU or Scheduled - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [<span data-ttu-id="8988e-163">Az Azure webhelyek méretezéssel -, lengyel Schackow</span><span class="sxs-lookup"><span data-stu-id="8988e-163">How Azure Websites Scale - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

<!-- LINKS -->
[vmsizes]:/pricing/details/app-service/
[SQLaccountsbilling]:http://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:http://go.microsoft.com/fwlink/?LinkID=235288
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ChooseBasicInstances]: ./media/web-sites-scale/scale2InstancesBasic.png
[SaveButton]: ./media/web-sites-scale/05SaveButton.png
[BasicComplete]: ./media/web-sites-scale/06BasicComplete.png
[ScaleStandard]: ./media/web-sites-scale/scale3InstancesStandard.png
[Autoscale]: ./media/web-sites-scale/scale4AutoScale.png
[SetTargetMetrics]: ./media/web-sites-scale/scale5AutoScaleTargetMetrics.png
[SetFirstRule]: ./media/web-sites-scale/scale6AutoScaleFirstRule.png
[SetSecondRule]: ./media/web-sites-scale/scale7AutoScaleSecondRule.png
[SetThirdRule]: ./media/web-sites-scale/scale8AutoScaleThirdRule.png
[SetRulesFinal]: ./media/web-sites-scale/scale9AutoScaleFinal.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png

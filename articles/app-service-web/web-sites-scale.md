---
title: "egy alkalmazás az Azure-ban aaaScale |} Microsoft Docs"
description: "Megtudhatja, hogyan tooscale be egy alkalmazást az Azure App Service tooadd kapacitás és a szolgáltatásokat."
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
ms.openlocfilehash: 97f46f77aeef95aec90d38e8396a023820e3caeb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-up-an-app-in-azure"></a><span data-ttu-id="a31a7-103">Vertikális felskálázás egy alkalmazást az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a31a7-103">Scale up an app in Azure</span></span>
<span data-ttu-id="a31a7-104">Ez a cikk bemutatja, hogyan tooscale az alkalmazást az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="a31a7-104">This article shows you how tooscale your app in Azure App Service.</span></span> <span data-ttu-id="a31a7-105">Két munkafolyamatok méretezési, bővített fel és kibővítési, és ez a cikk azt ismerteti, hello felskálázott munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="a31a7-105">There are two workflows for scaling, scale up and scale out, and this article explains hello scale up workflow.</span></span>

* <span data-ttu-id="a31a7-106">[Vertikális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): több Processzor, memória, lemezterület és extra funkciók, például dedikált virtuális gépek (VM), egyedi tartományok és tanúsítványok, előkészítési tárhelyek, automatikus skálázás és több beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a31a7-106">[Scale up](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Get more CPU, memory, disk space, and extra features like dedicated virtual machines (VMs), custom domains and certificates, staging slots, autoscaling, and more.</span></span> <span data-ttu-id="a31a7-107">Vertikális felskálázás az alkalmazáshoz tartozó App Service-csomag tarifacsomag hello módosításával.</span><span class="sxs-lookup"><span data-stu-id="a31a7-107">You scale up by changing hello pricing tier of the App Service plan that your app belongs to.</span></span>
* <span data-ttu-id="a31a7-108">[Horizontális felskálázás](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): növelje az alkalmazást futtató Virtuálisgép-példányok hello számát.</span><span class="sxs-lookup"><span data-stu-id="a31a7-108">[Scale out](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Increase hello number of VM instances that run your app.</span></span>
  <span data-ttu-id="a31a7-109">Ki lehet terjeszteni tooas több mint 20 példányok, attól függően, hogy a tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="a31a7-109">You can scale out tooas many as 20 instances, depending on your pricing tier.</span></span> <span data-ttu-id="a31a7-110">[App Service Environment-környezetek](../app-service/app-service-app-service-environments-readme.md) a **prémium** réteg tovább növeli a kibővíthető száma too50 példányok.</span><span class="sxs-lookup"><span data-stu-id="a31a7-110">[App Service Environments](../app-service/app-service-app-service-environments-readme.md) in **Premium** tier will further increase your scale-out count too50 instances.</span></span> <span data-ttu-id="a31a7-111">Kiterjesztésére vonatkozó további információkért lásd: [méretezése példányszám manuális vagy automatikus](../monitoring-and-diagnostics/insights-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="a31a7-111">For more information about scaling out, see [Scale instance count manually or automatically](../monitoring-and-diagnostics/insights-how-to-scale.md).</span></span> <span data-ttu-id="a31a7-112">Nem található ki, hogyan toouse automatikus skálázást, amely tooscale példányszám automatikusan alapján előre meghatározott szabályok és ütemezések.</span><span class="sxs-lookup"><span data-stu-id="a31a7-112">There you will find out how toouse autoscaling, which is tooscale instance count automatically based on predefined rules and schedules.</span></span>

<span data-ttu-id="a31a7-113">hello skálázási beállításokat hajtsa végre a megfelelő csak másodperc tooapply, és hatással lévő összes alkalmazásra a [App Service-csomag](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a31a7-113">hello scale settings take only seconds tooapply and affect all apps in your [App Service plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
<span data-ttu-id="a31a7-114">Nem szükséges, toochange a kódot, és telepítse újra az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a31a7-114">They do not require you toochange your code or redeploy your application.</span></span>

<span data-ttu-id="a31a7-115">További információ a hello tarifa- és szolgáltatásokat az App Service-csomagokról: [App Service díjszabás](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="a31a7-115">For information about hello pricing and features of individual App Service plans, see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>  

> [!NOTE]
> <span data-ttu-id="a31a7-116">App Service-csomagot a hello váltottunk **szabad** réteg, először el kell távolítania hello [költségkeretek](https://azure.microsoft.com/pricing/spending-limits/) biztosítani az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a31a7-116">Before you switch an App Service plan from hello **Free** tier, you must first remove hello [spending limits](https://azure.microsoft.com/pricing/spending-limits/) in place for your Azure subscription.</span></span> <span data-ttu-id="a31a7-117">Tekintse meg a tooview, vagy módosítsa a Microsoft Azure App Service előfizetési beállítások [Microsoft Azure-előfizetések][azuresubscriptions].</span><span class="sxs-lookup"><span data-stu-id="a31a7-117">tooview or change options for your Microsoft Azure App Service subscription, see [Microsoft Azure Subscriptions][azuresubscriptions].</span></span>
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a><span data-ttu-id="a31a7-118">Vertikális felskálázás tarifacsomag</span><span class="sxs-lookup"><span data-stu-id="a31a7-118">Scale up your pricing tier</span></span>
1. <span data-ttu-id="a31a7-119">A böngészőben nyissa meg a hello [Azure-portálon][portal].</span><span class="sxs-lookup"><span data-stu-id="a31a7-119">In your browser, open hello [Azure portal][portal].</span></span>
2. <span data-ttu-id="a31a7-120">Az alkalmazás paneljén kattintson **összes beállítás**, és kattintson a **vertikális Felskálázás**.</span><span class="sxs-lookup"><span data-stu-id="a31a7-120">In your app's blade, click **All settings**, and then click **Scale Up**.</span></span>
   
    ![Lépjen be az Azure alkalmazást tooscale.][ChooseWHP]
3. <span data-ttu-id="a31a7-122">Válassza ki a réteg, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="a31a7-122">Choose your tier, and then click **Select**.</span></span>
   
    <span data-ttu-id="a31a7-123">Hello **értesítések** lapon villogjon, egy zöld **sikeres** hello művelet befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="a31a7-123">hello **Notifications** tab will flash a green **SUCCESS** after hello operation is complete.</span></span>

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a><span data-ttu-id="a31a7-124">Kapcsolódó erőforrások méretezése</span><span class="sxs-lookup"><span data-stu-id="a31a7-124">Scale related resources</span></span>
<span data-ttu-id="a31a7-125">Ha az alkalmazás egyéb szolgáltatások, például az Azure SQL Database vagy az Azure Storage függ is méretezheti fel ezeket az erőforrásokat a igények alapján.</span><span class="sxs-lookup"><span data-stu-id="a31a7-125">If your app depends on other services, such as Azure SQL Database or Azure Storage, you can also scale up those resources based on your needs.</span></span> <span data-ttu-id="a31a7-126">Ezeket az erőforrásokat az App Service-csomag hello nem méretezi, és külön-külön lehet méretezni.</span><span class="sxs-lookup"><span data-stu-id="a31a7-126">These resources are not scaled with hello App Service plan and must be scaled separately.</span></span>

1. <span data-ttu-id="a31a7-127">A **Essentials**, kattintson a hello **erőforráscsoport** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a31a7-127">In **Essentials**, click hello **Resource group** link.</span></span>
   
    ![Vertikális felskálázás az Azure alkalmazás kapcsolódó erőforrások](./media/web-sites-scale/RGEssentialsLink.png)
2. <span data-ttu-id="a31a7-129">A hello **összegzés** hello része **erőforráscsoport** paneljén kattintson egy erőforrás, amelyet az tooscale.</span><span class="sxs-lookup"><span data-stu-id="a31a7-129">In hello **Summary** part of hello **Resource group** blade, click a resource that you want tooscale.</span></span> <span data-ttu-id="a31a7-130">a következő képernyőkép hello jeleníti meg, egy SQL-adatbázis és egy Azure Storage típusú erőforrást.</span><span class="sxs-lookup"><span data-stu-id="a31a7-130">hello following screenshot shows a SQL Database resource and an Azure Storage resource.</span></span>
   
    ![Keresse meg a tooresource csoport panel tooscale be az Azure alkalmazást](./media/web-sites-scale/ResourceGroup.png)
3. <span data-ttu-id="a31a7-132">Egy SQL-adatbázis erőforrás kattintson **beállítások** > **tarifacsomag** tooscale hello tarifacsomagra vált.</span><span class="sxs-lookup"><span data-stu-id="a31a7-132">For a SQL Database resource, click **Settings** > **Pricing tier** tooscale hello pricing tier.</span></span>
   
    ![Vertikális felskálázás az Azure alkalmazás hello SQL-adatbázis háttér](./media/web-sites-scale/ScaleDatabase.png)
   
    <span data-ttu-id="a31a7-134">Is bekapcsolhatja a [georeplikáció](../sql-database/sql-database-geo-replication-overview.md) az SQL Database-példányt.</span><span class="sxs-lookup"><span data-stu-id="a31a7-134">You can also turn on [geo-replication](../sql-database/sql-database-geo-replication-overview.md) for your SQL Database instance.</span></span>
   
    <span data-ttu-id="a31a7-135">Egy Azure Storage erőforrás kattintson **beállítások** > **konfigurációs** tooscale a tárolási beállítások konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="a31a7-135">For an Azure Storage resource, click **Settings** > **Configuration** tooscale up your storage options.</span></span>
   
    ![Vertikális felskálázás az Azure-alkalmazás által használt hello Azure Storage-fiókban](./media/web-sites-scale/ScaleStorage.png)

<a name="devfeatures"></a>

## <a name="learn-about-developer-features"></a><span data-ttu-id="a31a7-137">Fejlesztői szolgáltatásainak megismerése</span><span class="sxs-lookup"><span data-stu-id="a31a7-137">Learn about developer features</span></span>
<span data-ttu-id="a31a7-138">Attól függően, hogy a hello IP-címek hello következő fejlesztőknek készített funkciók érhetők el:</span><span class="sxs-lookup"><span data-stu-id="a31a7-138">Depending on hello pricing tier, hello following developer-oriented features are available:</span></span>

### <a name="bitness"></a><span data-ttu-id="a31a7-139">Bitszámának</span><span class="sxs-lookup"><span data-stu-id="a31a7-139">Bitness</span></span>
* <span data-ttu-id="a31a7-140">Hello **alapvető**, **szabványos**, és **prémium** rétegek támogatja a 64 bites és 32 bites alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="a31a7-140">hello **Basic**, **Standard**, and **Premium** tiers support 64-bit and 32-bit applications.</span></span>
* <span data-ttu-id="a31a7-141">Hello **szabad** és **megosztott** terv rétegek csak a 32 bites alkalmazások támogatásához.</span><span class="sxs-lookup"><span data-stu-id="a31a7-141">hello **Free** and **Shared** plan tiers support 32-bit applications only.</span></span>

### <a name="debugger-support"></a><span data-ttu-id="a31a7-142">Hibakereső támogatás</span><span class="sxs-lookup"><span data-stu-id="a31a7-142">Debugger support</span></span>
* <span data-ttu-id="a31a7-143">Hibakereső támogatás érhető el a hello **szabad**, **megosztott**, és **alapvető** módot, az App Service-csomag egy kapcsolattípust.</span><span class="sxs-lookup"><span data-stu-id="a31a7-143">Debugger support is available for hello **Free**, **Shared**, and **Basic** modes at one connection per App Service plan.</span></span>
* <span data-ttu-id="a31a7-144">Hibakereső támogatás érhető el a hello **szabványos** és **prémium** módot, az App Service-csomag öt egyidejű kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="a31a7-144">Debugger support is available for hello **Standard** and **Premium** modes at five concurrent connections per App Service plan.</span></span>

<a name="OtherFeatures"></a>

## <a name="learn-about-other-features"></a><span data-ttu-id="a31a7-145">Egyéb funkciókkal kapcsolatos tudnivalók</span><span class="sxs-lookup"><span data-stu-id="a31a7-145">Learn about other features</span></span>
* <span data-ttu-id="a31a7-146">Részletes információ összes fennmaradó hello App Service szolgáltatások hello csomagokat, beleértve a díjszabás és érdeklődési tooall a felhasználók (beleértve a fejlesztők) szolgáltatások: [App Service díjszabás](https://azure.microsoft.com/pricing/details/web-sites/).</span><span class="sxs-lookup"><span data-stu-id="a31a7-146">For detailed information about all of hello remaining features in hello App Service plans, including pricing and features of interest tooall users (including developers), see [App Service Pricing Details](https://azure.microsoft.com/pricing/details/web-sites/).</span></span>

> [!NOTE]
> <span data-ttu-id="a31a7-147">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/) ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="a31a7-147">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/) where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a31a7-148">Nincs bankkártyára szükséges találhatók és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="a31a7-148">No credit cards are required and there are no commitments.</span></span>
> 
> 

<a name="Next Steps"></a>

## <a name="next-steps"></a><span data-ttu-id="a31a7-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a31a7-149">Next steps</span></span>
* <span data-ttu-id="a31a7-150">Azure-ban használatába tooget lásd [Microsoft Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a31a7-150">tooget started with Azure, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a31a7-151">Információk a díjszabásról támogatást és szolgáltatásiszint-szerződés, látogasson el a következő hivatkozások hello.</span><span class="sxs-lookup"><span data-stu-id="a31a7-151">For information about pricing, support, and SLA, visit hello following links.</span></span>
  
    [<span data-ttu-id="a31a7-152">Adatátvitel díjszabása</span><span class="sxs-lookup"><span data-stu-id="a31a7-152">Data Transfers Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/data-transfers/)
  
    [<span data-ttu-id="a31a7-153">A Microsoft Azure-támogatás ügyfeleknek</span><span class="sxs-lookup"><span data-stu-id="a31a7-153">Microsoft Azure Support Plans</span></span>](https://azure.microsoft.com/support/plans/)
  
    [<span data-ttu-id="a31a7-154">Szolgáltatói szerződések</span><span class="sxs-lookup"><span data-stu-id="a31a7-154">Service Level Agreements</span></span>](https://azure.microsoft.com/support/legal/sla/)
  
    [<span data-ttu-id="a31a7-155">SQL adatbázis díjszabása</span><span class="sxs-lookup"><span data-stu-id="a31a7-155">SQL Database Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/sql-database/)
  
    <span data-ttu-id="a31a7-156">[Virtuális gépek és Felhőszolgáltatások mérete a Microsoft Azure][vmsizes]</span><span class="sxs-lookup"><span data-stu-id="a31a7-156">[Virtual Machine and Cloud Service Sizes for Microsoft Azure][vmsizes]</span></span>
  
    [<span data-ttu-id="a31a7-157">Az App Service díjszabás</span><span class="sxs-lookup"><span data-stu-id="a31a7-157">App Service Pricing Details</span></span>](https://azure.microsoft.com/pricing/details/app-service/)
  
    [<span data-ttu-id="a31a7-158">Az App Service díjszabás részletei – SSL-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="a31a7-158">App Service Pricing Details - SSL Connections</span></span>](https://azure.microsoft.com/pricing/details/web-sites/#ssl-connections)
* <span data-ttu-id="a31a7-159">Azure App Service információ gyakorlati tanácsok felépítése méretezhető és rugalmas architektúra, beleértve: [gyakorlati tanácsok: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span><span class="sxs-lookup"><span data-stu-id="a31a7-159">For information about Azure App Service best practices, including building a scalable and resilient architecture, see [Best Practices: Azure App Service Web Apps](http://blogs.msdn.com/b/windowsazure/archive/2014/02/10/best-practices-windows-azure-websites-waws.aspx).</span></span>
* <span data-ttu-id="a31a7-160">App Service apps méretezésével kapcsolatos videók tekintse meg a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="a31a7-160">For videos about scaling App Service apps, see hello following resources:</span></span>
  
  * [<span data-ttu-id="a31a7-161">Ha Azure webhelyek - lengyel Schackow tooScale</span><span class="sxs-lookup"><span data-stu-id="a31a7-161">When tooScale Azure Websites - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/azure-web-sites-free-vs-standard-scaling/)
  * [<span data-ttu-id="a31a7-162">Az automatikus skálázás Azure-webhelyek, CPU vagy ütemezett - rendelkező lengyel Schackow</span><span class="sxs-lookup"><span data-stu-id="a31a7-162">Auto Scaling Azure Websites, CPU or Scheduled - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/auto-scaling-azure-web-sites/)
  * [<span data-ttu-id="a31a7-163">Az Azure webhelyek méretezéssel -, lengyel Schackow</span><span class="sxs-lookup"><span data-stu-id="a31a7-163">How Azure Websites Scale - with Stefan Schackow</span></span>](https://azure.microsoft.com/resources/videos/how-azure-web-sites-scale/)

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

---
title: "az Azure RemoteApp kívüli áttelepítése aaaOptions |} Microsoft Docs"
description: "További tudnivalók az Azure RemoteApp kívüli áttelepítése hello-beállítások."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: c4e0e5bc-5c13-4487-b1b6-ebf2a5edc1f0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 75324597881520d0c75939983b728ae9bbd7f436
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="options-for-migrating-out-of-azure-remoteapp"></a><span data-ttu-id="a93b2-103">Az Azure RemoteApp kívüli áttelepítése beállítások</span><span class="sxs-lookup"><span data-stu-id="a93b2-103">Options for migrating out of Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a93b2-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="a93b2-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="a93b2-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="a93b2-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>


<span data-ttu-id="a93b2-106">Ha az Azure RemoteApp segítségével hello miatt megszüntette [használatból való kivonást közlemény](https://go.microsoft.com/fwlink/?linkid=821148) vagy befejezte a próbaverzió, mert ki az Azure RemoteApp tooanother alkalmazásszolgáltatási toomigrate van szükség.</span><span class="sxs-lookup"><span data-stu-id="a93b2-106">If you have stopped using Azure RemoteApp because of hello [retirement announcement](https://go.microsoft.com/fwlink/?linkid=821148) or because you've finished your evaluation, you need toomigrate off of Azure RemoteApp tooanother app service.</span></span> <span data-ttu-id="a93b2-107">Áttelepítés két különböző megközelítés: egy önállóan felügyelt (gyakran nevezik infrastruktúra szolgáltatásként [IaaS]) üzembe helyezése vagy egy teljes körűen felügyelt (más néven Platformszolgáltatást) vagy [PaaS/SaaS] szolgáltatott szoftver kínál.</span><span class="sxs-lookup"><span data-stu-id="a93b2-107">There are two different approaches for migrating: a self-managed (often called Infrastructure as a Service [IaaS]) deployment or a fully managed (often called Platform as a Service or Software as a Service [PaaS/SaaS]) offering.</span></span> 

<span data-ttu-id="a93b2-108">Önkiszolgáló IaaS saját munka központi telepítések, felügyelt, üzemeltetett, és a saját tulajdonában, közvetlenül telepített virtuális gépek (VM) vagy a fizikai rendszerek.</span><span class="sxs-lookup"><span data-stu-id="a93b2-108">Self-service IaaS is a do-it-yourself deployment that is managed, operated, and owned by you, directly deployed on virtual machines (VMs) or physical systems.</span></span> <span data-ttu-id="a93b2-109">Más hello: végén, egy teljes körűen felügyelt PaaS/SaaS kínál az Azure RemoteApp hasonlít - partner biztosít szolgáltatás szintű rétege egy távelérési megoldás, amely kezeli a működési és karbantartás közben, hello ügyfélként néhány lemezképet és az alkalmazás felügyeleti.</span><span class="sxs-lookup"><span data-stu-id="a93b2-109">At hello other end, a fully managed PaaS/SaaS offering is more like Azure RemoteApp - a partner provides a service layer on top of a remoting solution that handles operational and servicing, while you, as hello customer, do some image and app management.</span></span>

<span data-ttu-id="a93b2-110">[Áttelepítési lehetőségek hello Azure RemoteApp előadások megtekintése](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), vagy olvassa el a további tudnivalókat (többek között különböző beállításokat tartalmazó hello példát).</span><span class="sxs-lookup"><span data-stu-id="a93b2-110">[View hello Azure RemoteApp webinars on migration options](https://social.msdn.microsoft.com/Forums/azure/40557aaa-3e9f-403c-b221-ad3eac10dc56/migration-option-webinar-recordings?forum=AzureRemoteApp), or read on for more information (including examples of hello different hosting options).</span></span>

## <a name="self-managed-iaas-solutions"></a><span data-ttu-id="a93b2-111">Önálló felügyelt (IaaS) megoldások</span><span class="sxs-lookup"><span data-stu-id="a93b2-111">Self-managed (IaaS) solutions</span></span>
### <a name="rds-on-iaas"></a><span data-ttu-id="a93b2-112">**Infrastruktúra-szolgáltatási a távoli asztali szolgáltatások**</span><span class="sxs-lookup"><span data-stu-id="a93b2-112">**RDS on IaaS**</span></span>
<span data-ttu-id="a93b2-113">Telepítése natív munkamenet-alapú távoli asztali szolgáltatások (a Windows Server) használó telepítés RemoteApp vagy asztali számítógépek helyszíni vagy üzemeltetett környezetben (hasonló Azure virtuális gépeken).</span><span class="sxs-lookup"><span data-stu-id="a93b2-113">You can deploy a native session-based Remote Desktop Services (in Windows Server) deployment using either RemoteApp or desktops on-premises or in a hosted environment (like on Azure VMs).</span></span> <span data-ttu-id="a93b2-114">Távoli asztali szolgáltatások IaaS üzemelő példányok esetében az ügyfelek már ismeri a legjobb és, amelyek a meglévő technikai segítséget az RDS központi telepítésével.</span><span class="sxs-lookup"><span data-stu-id="a93b2-114">RDS on IaaS deployments are best for customers already familiar with and that have existing technical expertise with RDS deployments.</span></span> 

> [!NOTE]
> <span data-ttu-id="a93b2-115">Szükség van mennyiségi licencelési szoftverek megbízhatósági (SA) a távoli asztali ügyfél-hozzáférési licencek toouse ennek e telepítési beállításnak.</span><span class="sxs-lookup"><span data-stu-id="a93b2-115">You need Volume Licensing with Software Assurance (SA) for RDS client access licenses toouse this deployment option.</span></span>

<span data-ttu-id="a93b2-116">Az Azure virtuális gépeken futó távoli asztali szolgáltatások telepítése az egyszerűbb, mint legalább egyszer használatos üzembe helyezési és sablonok javítását (olvasni egy [áttekintése](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) , majd [nyissa meg azokat](https://aka.ms/rdautomation)).</span><span class="sxs-lookup"><span data-stu-id="a93b2-116">Deploying RDS on Azure VMs is easier than ever when you use deployment and patching templates (read an [overview](https://blogs.technet.microsoft.com/enterprisemobility/2015/07/13/azure-resource-manager-template-for-rds-deployment/) and then [go get them](https://aka.ms/rdautomation)).</span></span> <span data-ttu-id="a93b2-117">Kaphat hello Azure klasszikus üzembe helyezési modell erőforrások (nem az Azure erőforrás-modellje erőforrások) Azure RemoteApp belül az azonos rugalmas méretezési képességeket hello segítségével [automatikus skálázás parancsfájl](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), bár vannak további testreszabásokat és beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a93b2-117">You can get hello same elastic scaling capabilities with Azure classic deployment model resources (not Azure Resource Model resources) within Azure RemoteApp by using hello [auto scaling script](https://gallery.technet.microsoft.com/scriptcenter/Automatic-Scaling-of-9b4f5e76), although there are more customizations and configurations.</span></span> <span data-ttu-id="a93b2-118">Az Azure virtuális gépeken futó távoli asztali szolgáltatások telepítésekor támogatási keresztül valósul meg [Azure támogatja](https://azure.microsoft.com/support/plans/), azonos támogatási szakemberek számára, amelyet támogat, és az Azure RemoteApp hello.</span><span class="sxs-lookup"><span data-stu-id="a93b2-118">When you deploy RDS on Azure VMs, support is provided through [Azure Support](https://azure.microsoft.com/support/plans/), hello same support professionals that supported you with Azure RemoteApp.</span></span> <span data-ttu-id="a93b2-119">Akkor is beolvasása költségeket, a meglévő használati elvégezzék becslése [Azure támogatási](https://azure.microsoft.com/support/plans/), vagy saját kezűleg számítások hajthat végre használatával egy hamarosan toobe kiadott költség Számológép.</span><span class="sxs-lookup"><span data-stu-id="a93b2-119">You can get cost estimates based on your existing usage by contacting [Azure Support](https://azure.microsoft.com/support/plans/), or you can perform calculations yourself using a soon toobe released Cost Calculator.</span></span>  <span data-ttu-id="a93b2-120">Emellett N sorozatú virtuális gépe (jelenleg magán előnézetben) hozzáadhat vGPU - túl hall több vGPU hozzáadásáról és arról, hogyan[hasznosítására távoli asztali szolgáltatások fejlesztések a Windows Server 2016](https://myignite.microsoft.com/videos/2794) az ignite-on munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="a93b2-120">Also, with N-series VMs (currently in private preview) you can add vGPU - hear more about adding vGPU and about how too[harness RDS improvements in Windows Server 2016](https://myignite.microsoft.com/videos/2794) in our Ignite session.</span></span>   

<span data-ttu-id="a93b2-121">Részletes üzembe helyezési útmutatók a tudunk [Windows Server 2012 R2](http://aka.ms/rdsonazure) és [Windows Server 2016](http://aka.ms/rdsonazure2016) tooassist a telepítés.</span><span class="sxs-lookup"><span data-stu-id="a93b2-121">We have step by step deployment guides for [Windows Server 2012 R2](http://aka.ms/rdsonazure) and [Windows Server 2016](http://aka.ms/rdsonazure2016) tooassist with your deployment.</span></span> <span data-ttu-id="a93b2-122">Tekintse meg a hello [távoli asztal blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) a legújabb híreket hello.</span><span class="sxs-lookup"><span data-stu-id="a93b2-122">Check out hello [Remote Desktop blog](https://blogs.technet.microsoft.com/enterprisemobility/?product=windows-server-remote-desktop-services) for hello latest news.</span></span>

### <a name="citrix-on-iaas"></a><span data-ttu-id="a93b2-123">**Az infrastruktúra-szolgáltatási Citrix**</span><span class="sxs-lookup"><span data-stu-id="a93b2-123">**Citrix on IaaS**</span></span>
<span data-ttu-id="a93b2-124">Egy natív Citrix telepítési XenApp munkamenet-alapú vagy XenDesktop lehet telepíteni a helyszíni vagy üzemeltetett környezetben (például Azure virtuális gépeken).</span><span class="sxs-lookup"><span data-stu-id="a93b2-124">A native Citrix deployment of session-based XenApp or XenDesktop can be deployed on-premises or within a hosted environment (such as on Azure VMs).</span></span> 

<span data-ttu-id="a93b2-125">Tekintse meg a hello részletes telepítési útmutató, [Citrix XA 7.6 Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), további információt.</span><span class="sxs-lookup"><span data-stu-id="a93b2-125">Check out hello step-by-step deployment guide, [Citrix XA 7.6 on Azure](http://www.citrixandmicrosoft.com/Documents/Citrix-Azure Deployment Guide-v.1.0.docx), for more information.</span></span> <span data-ttu-id="a93b2-126">Tudjon meg többet az [Azure Citrix](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), beleértve a ár Számológép.</span><span class="sxs-lookup"><span data-stu-id="a93b2-126">Read more about [Citrix on Azure](http://www.citrixandmicrosoft.com/Solutions/AzureCloud.aspx), including a price calculator.</span></span> <span data-ttu-id="a93b2-127">Is megkeresheti a [Citrix forduljon](http://citrix.com/English/contact/index.asp) toodiscuss a beállítások.</span><span class="sxs-lookup"><span data-stu-id="a93b2-127">You can also find a [Citrix contact](http://citrix.com/English/contact/index.asp) toodiscuss your options with.</span></span>

## <a name="fully-managed-paassaas-offerings"></a><span data-ttu-id="a93b2-128">Teljes körűen felügyelt (PaaS/SaaS) ajánlatok</span><span class="sxs-lookup"><span data-stu-id="a93b2-128">Fully managed (PaaS/SaaS) offerings</span></span>

### <a name="citrix-xenapp-essentials-released-april-2017"></a><span data-ttu-id="a93b2-129">Citrix XenApp Essentials (kiadott 2017. április)</span><span class="sxs-lookup"><span data-stu-id="a93b2-129">Citrix XenApp Essentials (released April 2017)</span></span>
<span data-ttu-id="a93b2-130">Most már elérhető hello [Azure piactér](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials hello új alkalmazás virtualizálási kiszolgáló, hello power kombinálásával és rugalmasságát hello Citrix felhőplatform hello egyszerű, előírásoknak megfelelő, és egyszerű felhasználásához látnia, Microsoft Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="a93b2-130">Available now on hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Citrix.XenAppEssentials), Citrix XenApp Essentials is hello new application virtualization service, combining hello power and flexibility of hello Citrix Cloud platform with hello simple, prescriptive, and easy-to-consume vision of Microsoft Azure RemoteApp.</span></span> 

<span data-ttu-id="a93b2-131">Meglévő Azure RemoteApp-ügyfeleknek is [regisztrálhat egy ingyenes próbaverzióra](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span><span class="sxs-lookup"><span data-stu-id="a93b2-131">Existing Azure RemoteApp customers can [register for a free trial](https://www.citrix.com/products/citrix-cloud/form/xenapp-essentials-msft-trial/).</span></span>  <span data-ttu-id="a93b2-132">Megjegyzés: Csak a Citrix felhasználói szolgáltatást ingyenesen elérhető szabad, az Azure számítási és tárolási költségek alkalmazása</span><span class="sxs-lookup"><span data-stu-id="a93b2-132">Note: Only Citrix user service charge is free, Azure compute and storage costs apply</span></span>

<span data-ttu-id="a93b2-133">tudj meg többet:</span><span class="sxs-lookup"><span data-stu-id="a93b2-133">Learn More:</span></span>
- [<span data-ttu-id="a93b2-134">Azure RemoteApp tooCitrix XenApp Essentials áttelepítése</span><span class="sxs-lookup"><span data-stu-id="a93b2-134">Migrate from Azure RemoteApp tooCitrix XenApp Essentials</span></span>](remoteapp-migrate-citrix.md)
- [<span data-ttu-id="a93b2-135">Citrix és a Microsoft</span><span class="sxs-lookup"><span data-stu-id="a93b2-135">Citrix and Microsoft</span></span>](https://www.citrix.com/global-partners/microsoft/remote-app.html)
- <span data-ttu-id="a93b2-136">[Citrix XenApp Essentials bemutató](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span><span class="sxs-lookup"><span data-stu-id="a93b2-136">[Citrix XenApp Essentials presentation](https://www.youtube.com/watch?v=91Z7CCfQ-9k).</span></span>  

### <a name="citrix-cloud-xenapp-service-and-xendesktop-service"></a><span data-ttu-id="a93b2-137">Citrix felhő XenApp és XenDesktop szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="a93b2-137">Citrix Cloud XenApp Service and XenDesktop Service</span></span> 

<span data-ttu-id="a93b2-138">[Citrix felhő XenApp és XenDesktop szolgáltatás](https://www.citrix.com/products/citrix-cloud/services.html) hello legjobb megoldás az alkalmazások és asztali számítógépek, valamint speciális kezelési és is figyelési képességek hello kézbesítését.</span><span class="sxs-lookup"><span data-stu-id="a93b2-138">[Citrix Cloud XenApp Service and XenDesktop Service](https://www.citrix.com/products/citrix-cloud/services.html) is hello best solution for hello delivery of both apps and desktops, plus advanced management and monitoring capabilities.</span></span> 

#### <a name="conexlink-platform-name-mycloudit"></a><span data-ttu-id="a93b2-139">Conexlink (Platform nevének: MyCloudIT)</span><span class="sxs-lookup"><span data-stu-id="a93b2-139">Conexlink (Platform name: MyCloudIT)</span></span>
<span data-ttu-id="a93b2-140">[MyCloudIT](https://mycloudit.com) használható automatizálási platform, az informatikai cégek toosimplify, optimalizálása, és a méret hello áttelepítési és a távoli asztali számítógépek, távoli alkalmazások és az infrastruktúra kézbesítését hello Microsoft Azure felhőbe.</span><span class="sxs-lookup"><span data-stu-id="a93b2-140">[MyCloudIT](https://mycloudit.com) is an automation platform for IT companies toosimplify, optimize, and scale hello migration and delivery of remote desktops, remote applications, and infrastructure in hello Microsoft Azure Cloud.</span></span> 

<span data-ttu-id="a93b2-141">hello MyCloudIT platform telepítési idő csökkenti a költségeket, 30 %-kal Azure 95 %-kal, és az ügyfél teljes informatikai infrastruktúra helyezi a hangsúlyt hello felhő néhány billentyűparancsra be.</span><span class="sxs-lookup"><span data-stu-id="a93b2-141">hello MyCloudIT platform reduces deployment time by 95%, Azure cost by 30%, and moves their client's entire IT infrastructure into hello cloud in a matter of a few key strokes.</span></span> <span data-ttu-id="a93b2-142">Partnerek kezelheti az ügyfelek egy globális irányítópultról, szolgáltatás végfelhasználók hello világ tenni, mielőtt a soha nem, nő a bevételek többletterhelést vagy kiterjedt Azure képzési hozzáadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="a93b2-142">Partners can now manage customers from one global dashboard, service end users around hello world like never before, and grow revenues without adding additional overhead or extensive Azure training.</span></span>  

> <span data-ttu-id="a93b2-143">Elsődleges hely: Dallas, TX, USA</span><span class="sxs-lookup"><span data-stu-id="a93b2-143">Primary location: Dallas, TX, USA</span></span>
> 
> <span data-ttu-id="a93b2-144">Művelet régió: világszerte</span><span class="sxs-lookup"><span data-stu-id="a93b2-144">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="a93b2-145">Partneri állapota: [arany](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span><span class="sxs-lookup"><span data-stu-id="a93b2-145">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/conexlink_4298787366/843036_1?k=Conexlink)</span></span>
> 
> <span data-ttu-id="a93b2-146">Microsoft Felhőszolgáltató: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-146">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="a93b2-147">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-147">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="a93b2-148">Az Azure RemoteApp áttelepítési megoldások: Igen, [további](https://mycloudit.com/remote-app-microsoft/)</span><span class="sxs-lookup"><span data-stu-id="a93b2-148">Azure RemoteApp migration solutions: Yes, [learn more](https://mycloudit.com/remote-app-microsoft/)</span></span>
> 
> <span data-ttu-id="a93b2-149">Brian Garoutte, üzleti fejlesztését VP</span><span class="sxs-lookup"><span data-stu-id="a93b2-149">Brian Garoutte, VP of Business Development</span></span>
> 
> <span data-ttu-id="a93b2-150">Telefon: 972-218-0741</span><span class="sxs-lookup"><span data-stu-id="a93b2-150">Phone: 972-218-0741</span></span>
>   
> <span data-ttu-id="a93b2-151">E-mail:[brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span><span class="sxs-lookup"><span data-stu-id="a93b2-151">Email: [brian.garoutte@conexlink.com](mailto:brian.garoutte@conexlink.com)</span></span>

### <a name="frame"></a><span data-ttu-id="a93b2-152">Keret</span><span class="sxs-lookup"><span data-stu-id="a93b2-152">Frame</span></span>

<span data-ttu-id="a93b2-153">A vállalati és kormányzati, felügyelt szolgáltatók és vezető szoftvergyártók informatikai szervezetek keret toocreate kiválasztása, és a biztonságos-, szoftver-meghatározott munkaterületek hello felhőben kezelése.</span><span class="sxs-lookup"><span data-stu-id="a93b2-153">IT organizations in enterprise and government, managed service providers, and leading software vendors choose Frame toocreate and manage their secure, software-defined workspaces in hello cloud.</span></span> <span data-ttu-id="a93b2-154">Kis toolarge vállalatoknak, keret segítségével nagyon egyszerűen toolet felhasználók férnek hozzá a Windows-alkalmazások bármelyik böngészőben bármilyen eszközről.</span><span class="sxs-lookup"><span data-stu-id="a93b2-154">From small toolarge organizations, Frame makes it incredibly easy toolet users access Windows applications on any browser from any device.</span></span> <span data-ttu-id="a93b2-155">hello keret platform tartalmaz mindent, ami egy rendszergazda igények toodeploy alkalmazások például hello Azure-infrastruktúra és a távoli asztali szolgáltatások licencek hello felhőből (a saját Azure-fiók és a licencek megadása nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="a93b2-155">hello Frame platform includes everything an admin needs toodeploy applications from hello cloud including hello Azure infrastructure and RDS licenses (bringing your own Azure account and licenses is optional).</span></span> 

<span data-ttu-id="a93b2-156">További információ [Azure keret](https://www.fra.me/ara).</span><span class="sxs-lookup"><span data-stu-id="a93b2-156">Learn more about [Frame on Azure](https://www.fra.me/ara).</span></span> 

> <span data-ttu-id="a93b2-157">Elsődleges hely: San Mateo, CA, USA</span><span class="sxs-lookup"><span data-stu-id="a93b2-157">Primary location: San Mateo, CA, USA</span></span>
>
> <span data-ttu-id="a93b2-158">Művelet régió: világszerte</span><span class="sxs-lookup"><span data-stu-id="a93b2-158">Operation region: Worldwide</span></span>
>
> <span data-ttu-id="a93b2-159">Microsoft-partnernek: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-159">Microsoft Partner: Yes</span></span>
> 
> <span data-ttu-id="a93b2-160">Telefon: 1-480-269-4668</span><span class="sxs-lookup"><span data-stu-id="a93b2-160">Phone: 1-480-269-4668</span></span>

### <a name="awingu"></a><span data-ttu-id="a93b2-161">Awingu</span><span class="sxs-lookup"><span data-stu-id="a93b2-161">Awingu</span></span>
<span data-ttu-id="a93b2-162">Awingu örökölt alkalmazások, SaaS és dokumentumok fut egy html5 böngészőből egyszerű online munkaterület megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="a93b2-162">Awingu provides a simple online workspace solution running legacy apps, SaaS and documents from an html5 browser.</span></span> <span data-ttu-id="a93b2-163">Ilyen elérhetővé alkalmazások biztonságosan az eszköz bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="a93b2-163">As such, making any applications securely available on any type of device.</span></span> <span data-ttu-id="a93b2-164">A szolgáltatott szoftver szolgáltatásokat a beállítások széles köre op Single-Sign-On érhető el.</span><span class="sxs-lookup"><span data-stu-id="a93b2-164">For SaaS services, a wide range op Single-Sign-On options is available.</span></span> <span data-ttu-id="a93b2-165">Is sokoldalú (felhő) fájlok rendszerek mélyen integrálható a munkaterületre.</span><span class="sxs-lookup"><span data-stu-id="a93b2-165">Also diverse (cloud) files systems can be deeply integrated into your workspace.</span></span> <span data-ttu-id="a93b2-166">Következő toofull mobilitási Awingu tartozó gazdag online munkaterület rendszerében optimális biztonsági részletes vezérlőkkel (pl. letöltése/feltöltése), teljes használati naplózást, a többtényezős hitelesítés (pl. Azure MFA), a munkamenet rögzítése és a további.</span><span class="sxs-lookup"><span data-stu-id="a93b2-166">Next toofull mobility, Awingu's rich online workspace will give optimal security with granular controls (e.g. downloading/uploading), full usage auditing, Multi-Factor Authentication (e.g. Azure MFA), session recording and more.</span></span> <span data-ttu-id="a93b2-167">Az a-kész, Awingu lehetővé teszi, hogy a dokumentum és az alkalmazás optimális és biztonságos együttműködési megosztásának munkamenet.</span><span class="sxs-lookup"><span data-stu-id="a93b2-167">Out-of-the-box, Awingu enables document and application session sharing for optimized and secure collaboration.</span></span>
<span data-ttu-id="a93b2-168">Awingu a megoldás, több-bérlős, több AD, és nyissa meg az API-t.</span><span class="sxs-lookup"><span data-stu-id="a93b2-168">Awingu's solution is multi-tenant, multi-AD and open API.</span></span> <span data-ttu-id="a93b2-169">Használják a kis és nagy vállalatok egyaránt Felhőbeli szolgáltatók és [ISV-k](http://www.isv2saas.com).</span><span class="sxs-lookup"><span data-stu-id="a93b2-169">It is used by small and large businesses, Cloud Service Providers and [ISVs](http://www.isv2saas.com).</span></span> <span data-ttu-id="a93b2-170">Ezen ügyfelek különösen értékeljük hello egyszerű használat, alacsony és a könnyű telepítése teljes tulajdonlási költségek.</span><span class="sxs-lookup"><span data-stu-id="a93b2-170">These customers especially appreciate hello easy-of-use, ease-to-install and low TCO.</span></span>

<span data-ttu-id="a93b2-171">Awingu mindent tudó van [hello Azure piactéren elérhető](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) 2 beépített egyidejű felhasználókkal.</span><span class="sxs-lookup"><span data-stu-id="a93b2-171">Awingu All-in-One is [available in hello Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/awingu.awingu-arm) with 2 built-in concurrent users.</span></span> <span data-ttu-id="a93b2-172">További licencek keresztül érhetők el egy [számos forgalmazóknak és viszonteladók számára](http://www.awingu.com/reseller).</span><span class="sxs-lookup"><span data-stu-id="a93b2-172">Additional licenses are available through a [wide range of distributors and resellers](http://www.awingu.com/reseller).</span></span>

<span data-ttu-id="a93b2-173">További információ [alternatív tooAzure RemoteApp, a Awingu](http://alternative-for-azure-remoteapp.awingu.com/).</span><span class="sxs-lookup"><span data-stu-id="a93b2-173">Learn more about [Awingu on as alternative tooAzure RemoteApp](http://alternative-for-azure-remoteapp.awingu.com/).</span></span>


> <span data-ttu-id="a93b2-174">Elsődleges hely: Belgium</span><span class="sxs-lookup"><span data-stu-id="a93b2-174">Primary location: Belgium</span></span>
> 
> <span data-ttu-id="a93b2-175">Régiókban működő: EMEA, Észak-Amerikából és Brazília</span><span class="sxs-lookup"><span data-stu-id="a93b2-175">Operating regions: EMEA, North America and Brazil</span></span>
> 
> <span data-ttu-id="a93b2-176">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-176">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span> 
> 
> <span data-ttu-id="a93b2-177">**Globális**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-177">**Global**:</span></span>
> 
> <span data-ttu-id="a93b2-178">Arnaud Marlière, a róluk szóló</span><span class="sxs-lookup"><span data-stu-id="a93b2-178">Arnaud Marlière, CMO</span></span>
> 
> <span data-ttu-id="a93b2-179">E-mail:[arnaud@awingu.com](mailto:arnaud@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="a93b2-179">Email: [arnaud@awingu.com](mailto:arnaud@awingu.com)</span></span>
> 
> <span data-ttu-id="a93b2-180">Telefon: +1 646 583 3025</span><span class="sxs-lookup"><span data-stu-id="a93b2-180">Phone: +1 646 583 3025</span></span>
> 
> <span data-ttu-id="a93b2-181">**Belgium HQ**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-181">**Belgium HQ**:</span></span>
> 
> <span data-ttu-id="a93b2-182">Ottergemsesteenweg-Zuid 808 B44</span><span class="sxs-lookup"><span data-stu-id="a93b2-182">Ottergemsesteenweg-Zuid 808 B44</span></span>
> 
> <span data-ttu-id="a93b2-183">9000 Gent</span><span class="sxs-lookup"><span data-stu-id="a93b2-183">9000 Gent</span></span>
> 
> <span data-ttu-id="a93b2-184">E-mail:[info@awingu.com](mailto:info@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="a93b2-184">Email: [info@awingu.com](mailto:info@awingu.com)</span></span> 
> 
> <span data-ttu-id="a93b2-185">Telefon: +32 9 296 40 11</span><span class="sxs-lookup"><span data-stu-id="a93b2-185">Phone: +32 9 296 40 11</span></span>
> 
> <span data-ttu-id="a93b2-186">**USA**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-186">**USA**:</span></span>
> 
> <span data-ttu-id="a93b2-187">7. emelet, 1177 Ave hello Americas, a</span><span class="sxs-lookup"><span data-stu-id="a93b2-187">7th floor, 1177 Ave of hello Americas,</span></span>
> 
> <span data-ttu-id="a93b2-188">New York közt, NY 10036</span><span class="sxs-lookup"><span data-stu-id="a93b2-188">New York, NY 10036</span></span>
> 
> <span data-ttu-id="a93b2-189">E-mail:[info.us@awingu.com](mailto:info.us@awingu.com)</span><span class="sxs-lookup"><span data-stu-id="a93b2-189">Email: [info.us@awingu.com](mailto:info.us@awingu.com)</span></span>

### <a name="microsoft-hosted-service-provider"></a><span data-ttu-id="a93b2-190">Microsoft-szolgáltatás szolgáltatót</span><span class="sxs-lookup"><span data-stu-id="a93b2-190">Microsoft Hosted Service Provider</span></span>
<span data-ttu-id="a93b2-191">Üzemeltetési partnerek az esetek többségében egy teljes körűen felügyelt Windows asztali üzemeltetett alkalmazásszolgáltatás, amelyek magukban foglalhatják kezelése hello Azure-erőforrások, operációs rendszerek, alkalmazások és segélyszolgálat hello partner használatával a Microsoft a licencszerződések és más szoftverszolgáltatóval együtt egy Szolgáltatóilicenc tooallow továbbértékesítése előfizető hozzáférési licenc (SAL) alatt.</span><span class="sxs-lookup"><span data-stu-id="a93b2-191">Hosting partners typically offer a fully managed hosted Windows desktop and application service, which may include managing hello Azure resources, operating systems, applications, and helpdesk using hello partner's licensing agreements with Microsoft and other software providers along with being a Service Provider License Agreement tooallow reselling of Subscriber Access License (SAL).</span></span> <span data-ttu-id="a93b2-192">hello alábbi információkat jelenít meg adatokat és néhány hello infrastruktúrájáról szakosodott boltoknak az Azure RemoteApp áttelepítési rendelkező ügyfelek az elérhetőségeit.</span><span class="sxs-lookup"><span data-stu-id="a93b2-192">hello following information provides details and contact information for some of hello hosters that specialize in assisting customers with their Azure RemoteApp migration.</span></span> <span data-ttu-id="a93b2-193">Tekintse meg [hello aktuális tárolt szolgáltatók listáját](http://aka.ms/rdsonazurecertified) hello elérési út és a frissítésfelmérő IaaS a távoli asztali szolgáltatások, amelyek végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="a93b2-193">Check out [hello current list of Hosted Service Providers](http://aka.ms/rdsonazurecertified) that have completed hello RDS on IaaS learning path and assessment.</span></span>  

### <a name="citrix-service-provider-program"></a><span data-ttu-id="a93b2-194">Citrix Service Provider programot</span><span class="sxs-lookup"><span data-stu-id="a93b2-194">Citrix Service Provider Program</span></span>
<span data-ttu-id="a93b2-195">Citrix Service Provider programot hello megkönnyíti a szolgáltatás szolgáltatók toodeliver hello egyszerűség virtuális a felhőalapú informatika tooSMBs, az egyszerű, használatalapú fizetési modell szeretnék hello szolgáltatásokat kínáló őket.</span><span class="sxs-lookup"><span data-stu-id="a93b2-195">hello Citrix Service Provider Program makes it easy for service providers toodeliver hello simplicity of virtual cloud computing tooSMBs, offering them hello services they want in an easy, pay-as-you-go model.</span></span> <span data-ttu-id="a93b2-196">Citrix szolgáltatók nő a Microsoft SPLA vállalatok számára, és bontsa ki a távoli asztali szolgáltatások platform beruházások bármely, helyfüggetlen hozzáférés, hello legszélesebb alkalmazások támogatása, a gazdag felhasználói élmény, a nagyobb biztonság és a jobb méretezhetőséget.</span><span class="sxs-lookup"><span data-stu-id="a93b2-196">Citrix Service Providers grow their Microsoft SPLA businesses and expand their RDS platform investments with any device, anywhere access, hello broadest application support, a rich experience, added security and increased scalability.</span></span> <span data-ttu-id="a93b2-197">Citrix szolgáltatók pedig további előfizetők vonzerőt, ügyfelek elégedettségének növelése, és a működési költségek csökkentése.</span><span class="sxs-lookup"><span data-stu-id="a93b2-197">In turn, Citrix Service Providers attract more subscribers, increase customer satisfaction and reduce their operational costs.</span></span> <span data-ttu-id="a93b2-198">[További](http://www.citrix.com/products/service-providers.html) vagy [partner keresése](https://www.citrix.com/buy/partnerlocator.html).</span><span class="sxs-lookup"><span data-stu-id="a93b2-198">[Learn more](http://www.citrix.com/products/service-providers.html) or [find a partner](https://www.citrix.com/buy/partnerlocator.html).</span></span>

#### <a name="acuutech"></a><span data-ttu-id="a93b2-199">Acuutech</span><span class="sxs-lookup"><span data-stu-id="a93b2-199">Acuutech</span></span>
<span data-ttu-id="a93b2-200">[Acuutech](http://www.acuutech.com) biztosítására szakosodott vállalattal teljes asztali és ISV alkalmazások kézbesítéséhez üzemeltetett asztali megoldásokat biztosító Microsoft technológia tooa globális ügyfél alap épülő Azure és a saját adatközpontját a feladatait.</span><span class="sxs-lookup"><span data-stu-id="a93b2-200">[Acuutech](http://www.acuutech.com) specializes in providing hosted desktop solutions, delivering full desktop and ISV applications experiences built on Microsoft technology tooa global client base from Azure and their own datacenters.</span></span>

> <span data-ttu-id="a93b2-201">Elsődleges hely: London, UK; Jelen; Houston, TX</span><span class="sxs-lookup"><span data-stu-id="a93b2-201">Primary location: London, UK; Singapore; Houston, TX</span></span>
> 
> <span data-ttu-id="a93b2-202">Művelet régió: világszerte</span><span class="sxs-lookup"><span data-stu-id="a93b2-202">Operation region: Worldwide</span></span>
> 
> <span data-ttu-id="a93b2-203">Partneri állapota: arany</span><span class="sxs-lookup"><span data-stu-id="a93b2-203">Partner status: Gold</span></span>
> 
> <span data-ttu-id="a93b2-204">Microsoft Felhőszolgáltató: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-204">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="a93b2-205">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-205">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="a93b2-206">Az Azure RemoteApp áttelepítési megoldások: Igen, [további](http://www.acuutech.com/ara-migration/)</span><span class="sxs-lookup"><span data-stu-id="a93b2-206">Azure RemoteApp migration solutions: Yes, [learn more](http://www.acuutech.com/ara-migration/)</span></span>
> 
> <span data-ttu-id="a93b2-207">**Egyesült Királyság**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-207">**United Kingdom**:</span></span>
>   
> <span data-ttu-id="a93b2-208">5/6 York House Langston közúti</span><span class="sxs-lookup"><span data-stu-id="a93b2-208">5/6 York House, Langston Road,</span></span>
>   
> <span data-ttu-id="a93b2-209">Loughton, Essex IG10 3TQ</span><span class="sxs-lookup"><span data-stu-id="a93b2-209">Loughton, Essex IG10 3TQ</span></span>
>   
> <span data-ttu-id="a93b2-210">Telefon: +44 (0) 20 8502 2155</span><span class="sxs-lookup"><span data-stu-id="a93b2-210">Phone: +44 (0) 20 8502 2155</span></span>
> 
> <span data-ttu-id="a93b2-211">**Szingapúri**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-211">**Singapore**:</span></span>
>   
> <span data-ttu-id="a93b2-212">100 Cecil utca, #09-02,</span><span class="sxs-lookup"><span data-stu-id="a93b2-212">100 Cecil Street, #09-02,</span></span> 
>   
> <span data-ttu-id="a93b2-213">Földgömb, szingapúri 069532 hello</span><span class="sxs-lookup"><span data-stu-id="a93b2-213">hello Globe, Singapore 069532</span></span>
> 
> <span data-ttu-id="a93b2-214">Telefon: + 65 6709 4933</span><span class="sxs-lookup"><span data-stu-id="a93b2-214">Phone: +65 6709 4933</span></span>
>   
> <span data-ttu-id="a93b2-215">**Észak-Amerika**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-215">**North America**:</span></span>
>   
> <span data-ttu-id="a93b2-216">3601 S. Sandman St.</span><span class="sxs-lookup"><span data-stu-id="a93b2-216">3601 S. Sandman St.</span></span>
>   
> <span data-ttu-id="a93b2-217">Suite 200, Houston, TX 77098</span><span class="sxs-lookup"><span data-stu-id="a93b2-217">Suite 200, Houston, TX 77098</span></span>
>   
> <span data-ttu-id="a93b2-218">Telefon: +1 713 691 0800</span><span class="sxs-lookup"><span data-stu-id="a93b2-218">Phone: +1 713 691 0800</span></span>

#### <a name="aspex"></a><span data-ttu-id="a93b2-219">ASPEX</span><span class="sxs-lookup"><span data-stu-id="a93b2-219">ASPEX</span></span>
<span data-ttu-id="a93b2-220">[ASPEX](http://www.aspex.be/en) biztosítására szakosodott vállalattal tér át toohello felhő- és ISV ISV-k "toooptimize az aktuális felhő beállítások keresése.</span><span class="sxs-lookup"><span data-stu-id="a93b2-220">[ASPEX](http://www.aspex.be/en) specializes in ISVs transitioning toohello Cloud and ISV‘ looking toooptimize their current cloud setups.</span></span> <span data-ttu-id="a93b2-221">ASPEX azokat a felügyelt szolgáltatásokat, a devops és a tanácsadási szolgáltatásokat kínál.</span><span class="sxs-lookup"><span data-stu-id="a93b2-221">ASPEX offers a wide range of managed services, devops, and consulting services.</span></span>  

> <span data-ttu-id="a93b2-222">Elsődleges hely: Antwerpen, Belgium</span><span class="sxs-lookup"><span data-stu-id="a93b2-222">Primary location: Antwerp, Belgium</span></span>
> 
> <span data-ttu-id="a93b2-223">Művelet régió: Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="a93b2-223">Operation region: Western Europe</span></span>
> 
> <span data-ttu-id="a93b2-224">Partneri állapota: [ezüst](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span><span class="sxs-lookup"><span data-stu-id="a93b2-224">Partner status: [Silver](https://partnercenter.microsoft.com/pcv/solution-providers/aspex_9397f5dd-ebdd-405b-b926-19a5bda61f7a/cfe00bac-ea36-4591-a60b-ec001c4c3dff)</span></span>
> 
> <span data-ttu-id="a93b2-225">Microsoft Felhőszolgáltató: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-225">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="a93b2-226">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-226">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="a93b2-227">Az Azure RemoteApp áttelepítési megoldások: Igen, [további](https://www.aspex.be/en/azure-remote-apps)</span><span class="sxs-lookup"><span data-stu-id="a93b2-227">Azure RemoteApp migration solutions: Yes, [learn more](https://www.aspex.be/en/azure-remote-apps)</span></span>
> 
> <span data-ttu-id="a93b2-228">Telefon: +3232202198</span><span class="sxs-lookup"><span data-stu-id="a93b2-228">Phone: +3232202198</span></span>
> 
> <span data-ttu-id="a93b2-229">E-mail:[info@aspex.be](mailto:info@aspex.be)</span><span class="sxs-lookup"><span data-stu-id="a93b2-229">Mail: [info@aspex.be](mailto:info@aspex.be)</span></span>
> 
> <span data-ttu-id="a93b2-230">Webes: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span><span class="sxs-lookup"><span data-stu-id="a93b2-230">Web: [http://cloud.aspex.be/contact-ara-0](http://cloud.aspex.be/contact-ara-0)</span></span>

#### <a name="caasecom"></a><span data-ttu-id="a93b2-231">Caase.com</span><span class="sxs-lookup"><span data-stu-id="a93b2-231">Caase.com</span></span>
<span data-ttu-id="a93b2-232">[Caase.com](http://www.caase.com/) segítségével a vállalatok, helyi, nem állami szervek és kifizetni a Microsoft Cloud hello munka intelligensebb úgy útjuk egészségügyi intézmények.</span><span class="sxs-lookup"><span data-stu-id="a93b2-232">[Caase.com](http://www.caase.com/) helps businesses, local governments, non-governmental bodies and healthcare institutions with their journey towards a smarter way of work in hello Microsoft Cloud.</span></span> <span data-ttu-id="a93b2-233">Bármely helyen bármely és alacsony informatikai költségekkel alatt álló hatékony és biztonságos.</span><span class="sxs-lookup"><span data-stu-id="a93b2-233">Being productive and secure at any place, with any device and at low IT cost.</span></span> <span data-ttu-id="a93b2-234">Caase.com Microsoft Office365, az Azure, a nagyvállalati mobilitás és a biztonsági és a Windows egy igaz specialistája.</span><span class="sxs-lookup"><span data-stu-id="a93b2-234">Caase.com is a true specialist for Microsoft Office365, Azure, Enterprise Mobility and Security and Windows.</span></span> <span data-ttu-id="a93b2-235">A tanácsadási, áttelepítési szolgáltatások, a bevezetési programok, az képzési kezelési és támogatási Caase.com hoz létre egy optimalizált és biztonságos platform együttműködésre is ügyfelek alkalmazottak, partnerek és szállítók számára.</span><span class="sxs-lookup"><span data-stu-id="a93b2-235">With our consultancy, migration services, adoption programs, training, management and support Caase.com creates an optimized and secure platform for collaboration for both customers’ employees, partners and suppliers.</span></span>
<span data-ttu-id="a93b2-236">Caase.com hello mastermind hello Azure távoli munkaterület (mobil munkahelyi) és hello digitális munkahelyi (közösségi Intranet).</span><span class="sxs-lookup"><span data-stu-id="a93b2-236">Caase.com is hello mastermind of hello Azure Remote Workspace (mobile workplace) and hello Digital Workplace (Social Intranet).</span></span> <span data-ttu-id="a93b2-237">A két megoldás – valósítható meg bevezetésével – hello foundation, amely biztosítja, hogy ezek a megoldások hello felhasználók hello Kellemesen, sikeres és hatékony élmény legyenek az útvonal toohello Microsoft Cloud.</span><span class="sxs-lookup"><span data-stu-id="a93b2-237">Both solutions – accomplished with adoption – are hello foundation which ensures that hello users of these solutions have hello most pleasant, successful and effective experience in their route toohello Microsoft Cloud.</span></span>
<span data-ttu-id="a93b2-238">Holland fordítási ánd keresztül itt támogató film: http://caase.com/over-ons/</span><span class="sxs-lookup"><span data-stu-id="a93b2-238">Dutch translation ánd a supporting movie over here: http://caase.com/over-ons/</span></span>

> <span data-ttu-id="a93b2-239">Művelet régió: holland alapú, globális reach</span><span class="sxs-lookup"><span data-stu-id="a93b2-239">Operation region: Dutch based, global reach</span></span>
> 
> <span data-ttu-id="a93b2-240">Partneri állapota: [arany](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span><span class="sxs-lookup"><span data-stu-id="a93b2-240">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/caasecom_4295593260/51159_3)</span></span>
> 
> <span data-ttu-id="a93b2-241">Microsoft Felhőszolgáltató: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-241">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="a93b2-242">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-242">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="a93b2-243">Az Azure RemoteApp áttelepítési megoldások: Igen, [további](http://caase.com/diensten/microsoft-azure/).</span><span class="sxs-lookup"><span data-stu-id="a93b2-243">Azure RemoteApp migration solutions: Yes, [learn more](http://caase.com/diensten/microsoft-azure/).</span></span>
> 
> 
> <span data-ttu-id="a93b2-244">Hollandia:</span><span class="sxs-lookup"><span data-stu-id="a93b2-244">Netherlands:</span></span>
> 
> <span data-ttu-id="a93b2-245">Rigtersbleek-Zandvoort 10 (De Spinnerij)</span><span class="sxs-lookup"><span data-stu-id="a93b2-245">Rigtersbleek-Zandvoort 10 (De Spinnerij)</span></span>
> 
> <span data-ttu-id="a93b2-246">7521 BE, Enschede</span><span class="sxs-lookup"><span data-stu-id="a93b2-246">7521 BE, Enschede</span></span>
> 
> <span data-ttu-id="a93b2-247">Telefon: +31 (0) 88 4320 000</span><span class="sxs-lookup"><span data-stu-id="a93b2-247">Phone: +31 (0) 88 4320 000</span></span>


#### <a name="nerdio"></a><span data-ttu-id="a93b2-248">Nerdio</span><span class="sxs-lookup"><span data-stu-id="a93b2-248">Nerdio</span></span>
<span data-ttu-id="a93b2-249">[Az Azure-Nerdio](http://getnerdio.com/nfa/) egy informatikai automatizálási platform ridiculously egyszerű kiépítés, felügyeleti és a teljes informatikai környezetek optimalizálási letölti a Microsoft cloud hello.</span><span class="sxs-lookup"><span data-stu-id="a93b2-249">[Nerdio for Azure](http://getnerdio.com/nfa/) is an IT automation platform that delivers ridiculously simple provisioning, management and optimization of complete IT environments in hello Microsoft cloud.</span></span> <span data-ttu-id="a93b2-250">Önálló virtuális asztalok, a távoli alkalmazások és a kiszolgálók a néhány óra múlva.</span><span class="sxs-lookup"><span data-stu-id="a93b2-250">Stand up virtual desktops, remote apps and servers in a couple of hours.</span></span> <span data-ttu-id="a93b2-251">Három kattintással hello környezet felügyeletéhez vagy kisebb Nerdio felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="a93b2-251">Administer hello environment in three clicks or less with Nerdio Admin Portal.</span></span> <span data-ttu-id="a93b2-252">Használjon intelligens automatikus skálázást, és mentse too60 40 % Azure IaaS-erőforrásokra.</span><span class="sxs-lookup"><span data-stu-id="a93b2-252">Use intelligent auto-scaling and save 40 too60% in Azure IaaS resources.</span></span>

> <span data-ttu-id="a93b2-253">Elsődleges hely: Chicago, Illinois művelet régió: világszerte Partner állapota: [arany](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Felhőszolgáltató: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-253">Primary location: Chicago, IL Operation region: Worldwide Partner status: [Gold](https://partnercenter.microsoft.com/en-us/pcv/solution-providers/adar-inc_341c9afa-f12c-46f5-8f7b-3f9ef59a66a5/3a7ae479-3ac2-42f6-84e2-d456dc7424e1) Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="a93b2-254">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-254">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="a93b2-255">Az Azure RemoteApp áttelepítési megoldások: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-255">Azure RemoteApp migration solutions: Yes</span></span>
> 
> 
> <span data-ttu-id="a93b2-256">8001 Lincoln Ave</span><span class="sxs-lookup"><span data-stu-id="a93b2-256">8001 Lincoln Ave</span></span>
> 
> <span data-ttu-id="a93b2-257">Suite 212</span><span class="sxs-lookup"><span data-stu-id="a93b2-257">Suite 212</span></span>
> 
> <span data-ttu-id="a93b2-258">Skokie, IL 60077</span><span class="sxs-lookup"><span data-stu-id="a93b2-258">Skokie, IL 60077</span></span>
> 
> <span data-ttu-id="a93b2-259">USA</span><span class="sxs-lookup"><span data-stu-id="a93b2-259">USA</span></span>
> 
> <span data-ttu-id="a93b2-260">(844) 4NERDIO kiegészítő 6</span><span class="sxs-lookup"><span data-stu-id="a93b2-260">(844) 4NERDIO ext. 6</span></span>
> 
> [sayhello@getnerdio.com](mailto:sayhello@getnerdio.com)

#### <a name="saasplaza"></a><span data-ttu-id="a93b2-261">**SaaSplaza**</span><span class="sxs-lookup"><span data-stu-id="a93b2-261">**SaaSplaza**</span></span>
<span data-ttu-id="a93b2-262">[SaaSplaza](http://www.saasplaza.com/) kínál a teljes Microsoft Dynamics (NAV, AX, a csoportházirend, SA, CRM) kaphat privát és nyilvános Azure-felhőbe.</span><span class="sxs-lookup"><span data-stu-id="a93b2-262">[SaaSplaza](http://www.saasplaza.com/) offers complete Microsoft Dynamics portfolio (NAV, AX, GP, SL, CRM) private and public cloud (Azure).</span></span>

> <span data-ttu-id="a93b2-263">Elsődleges hely: holland</span><span class="sxs-lookup"><span data-stu-id="a93b2-263">Primary location: Netherlands</span></span>
> 
> <span data-ttu-id="a93b2-264">Művelet régió: világszerte</span><span class="sxs-lookup"><span data-stu-id="a93b2-264">Operation Region: Worldwide</span></span>
> 
> <span data-ttu-id="a93b2-265">Partneri állapota: [arany](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span><span class="sxs-lookup"><span data-stu-id="a93b2-265">Partner status: [Gold](https://partnercenter.microsoft.com/pcv/solution-providers/saasplaza_4295495801/791011_2?k=saasplaza)</span></span>
> 
> <span data-ttu-id="a93b2-266">Microsoft Felhőszolgáltató: Igen</span><span class="sxs-lookup"><span data-stu-id="a93b2-266">Microsoft Cloud Service Provider: Yes</span></span>
> 
> <span data-ttu-id="a93b2-267">Munkamenet-alapú RemoteApp- és asztali megoldásokat nyújtsanak: Igen, mindkettő</span><span class="sxs-lookup"><span data-stu-id="a93b2-267">Offer session-based RemoteApp and Desktop solutions: Yes, both</span></span>
> 
> <span data-ttu-id="a93b2-268">**EMEA**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-268">**EMEA**:</span></span>
> 
> <span data-ttu-id="a93b2-269">Prins Mauritslaan 29-35</span><span class="sxs-lookup"><span data-stu-id="a93b2-269">Prins Mauritslaan 29-35</span></span>
> 
> <span data-ttu-id="a93b2-270">71 LP Badhoevedorp</span><span class="sxs-lookup"><span data-stu-id="a93b2-270">71 LP Badhoevedorp</span></span>
> 
> <span data-ttu-id="a93b2-271">hello Hollandia</span><span class="sxs-lookup"><span data-stu-id="a93b2-271">hello Netherlands</span></span>
> 
> <span data-ttu-id="a93b2-272">Telefon: +31 20 547 8060</span><span class="sxs-lookup"><span data-stu-id="a93b2-272">Phone: +31 20 547 8060</span></span> 
> 
>  <span data-ttu-id="a93b2-273">**Dél-Amerika**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-273">**Americas**:</span></span>
> 
> <span data-ttu-id="a93b2-274">171 Szászország közúti, Suite 105</span><span class="sxs-lookup"><span data-stu-id="a93b2-274">171 Saxony Road, Suite 105</span></span>
> 
> <span data-ttu-id="a93b2-275">Encinitas, CA 92024</span><span class="sxs-lookup"><span data-stu-id="a93b2-275">Encinitas, CA 92024</span></span>
> 
> <span data-ttu-id="a93b2-276">San Diego</span><span class="sxs-lookup"><span data-stu-id="a93b2-276">San Diego</span></span>
> 
> <span data-ttu-id="a93b2-277">Egyesült Államok</span><span class="sxs-lookup"><span data-stu-id="a93b2-277">United States</span></span>
> 
> <span data-ttu-id="a93b2-278">Telefon: +1 858 385 8900</span><span class="sxs-lookup"><span data-stu-id="a93b2-278">Phone: +1 858 385 8900</span></span> 
> 
> <span data-ttu-id="a93b2-279">**APAC**:</span><span class="sxs-lookup"><span data-stu-id="a93b2-279">**APAC**:</span></span>
> 
> <span data-ttu-id="a93b2-280">105 Cecil utca.</span><span class="sxs-lookup"><span data-stu-id="a93b2-280">105 Cecil Street</span></span>
>    
> <span data-ttu-id="a93b2-281">\#11-08, hello nyolcszög</span><span class="sxs-lookup"><span data-stu-id="a93b2-281">\#11-08, hello Octagon</span></span>
> 
> <span data-ttu-id="a93b2-282">Szingapúri 069534</span><span class="sxs-lookup"><span data-stu-id="a93b2-282">Singapore 069534</span></span>
> 
> <span data-ttu-id="a93b2-283">Szingapúr</span><span class="sxs-lookup"><span data-stu-id="a93b2-283">Singapore</span></span>
>   
> <span data-ttu-id="a93b2-284">Telefonkönyv - szingapúri: + 65 6222 6591</span><span class="sxs-lookup"><span data-stu-id="a93b2-284">Phone - Singapore: +65 6222 6591</span></span>
> 
> <span data-ttu-id="a93b2-285">Telefonkönyv - Ausztrália: +61 2 8310 5568</span><span class="sxs-lookup"><span data-stu-id="a93b2-285">Phone - Australia: +61 2 8310 5568</span></span> 
>    
> <span data-ttu-id="a93b2-286">Telefonkönyv - Új-Zéland: + 64 4 488 0321</span><span class="sxs-lookup"><span data-stu-id="a93b2-286">Phone - New Zealand: +64 4 488 0321</span></span>
> 
## <a name="need-more-help"></a><span data-ttu-id="a93b2-287">További segítségre van szüksége?</span><span class="sxs-lookup"><span data-stu-id="a93b2-287">Need more help?</span></span>
<span data-ttu-id="a93b2-288">Továbbra is súgó kiválasztása vagy további kérdései?</span><span class="sxs-lookup"><span data-stu-id="a93b2-288">Still need help choosing or have further questions?</span></span> <span data-ttu-id="a93b2-289">A következő módszerek tooget súgó hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="a93b2-289">Use one of hello following methods tooget help.</span></span> 

1. <span data-ttu-id="a93b2-290">E-mailben nekünk az [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="a93b2-290">Email us at [arainfo@microsoft.com](mailto:arainfo@microsoft.com).</span></span>
2. <span data-ttu-id="a93b2-291">Ügyfél [az Azure támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="a93b2-291">Contact [Azure support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span> <span data-ttu-id="a93b2-292">Első lépésként megnyitása egy [az Azure támogatási esetet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="a93b2-292">Start by opening an [Azure support case](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
3. <span data-ttu-id="a93b2-293">Hívjon fel minket.</span><span class="sxs-lookup"><span data-stu-id="a93b2-293">Call us.</span></span> <span data-ttu-id="a93b2-294">[Helyi értékesítési számos](https://azure.microsoft.com/overview/sales-number/).</span><span class="sxs-lookup"><span data-stu-id="a93b2-294">[Find a local sales number](https://azure.microsoft.com/overview/sales-number/).</span></span>


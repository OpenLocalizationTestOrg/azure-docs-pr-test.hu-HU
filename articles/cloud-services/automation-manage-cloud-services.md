---
title: "Azure Cloud Services Azure Automation segítségével aaaManage |} Microsoft Docs"
description: "További információk a hello Azure Automation szolgáltatás hogyan lehet használt toomanage Azure felhőszolgáltatások méretekben."
services: cloud-services, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 8e920fb94955466bfec71cc332444f5f0ee497a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a><span data-ttu-id="6d29d-103">Azure Automation használata Azure Cloud Services kezelése</span><span class="sxs-lookup"><span data-stu-id="6d29d-103">Managing Azure Cloud Services using Azure Automation</span></span>
<span data-ttu-id="6d29d-104">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet az Azure felhőszolgáltatások használt toosimplify kezelését.</span><span class="sxs-lookup"><span data-stu-id="6d29d-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure cloud services.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="6d29d-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="6d29d-105">What is Azure Automation?</span></span>
<span data-ttu-id="6d29d-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="6d29d-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="6d29d-107">Azure Automation használ, hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatokat lehet automatizált tooincrease megbízhatóságát, hatékonyságát és idő toovalue a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="6d29d-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="6d29d-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi a igényeinek toomeet a szervezet növekedésének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="6d29d-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="6d29d-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="6d29d-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="6d29d-110">Működési munkaterhelés csökkentése és az szabadítson fel informatikai / DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket hozzáadja az Azure Automation automatikusan futtatásához.</span><span class="sxs-lookup"><span data-stu-id="6d29d-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a><span data-ttu-id="6d29d-111">Hogyan segíthet az Azure Automation Azure cloud services kezelése?</span><span class="sxs-lookup"><span data-stu-id="6d29d-111">How can Azure Automation help manage Azure cloud services?</span></span>
<span data-ttu-id="6d29d-112">Azure felhőszolgáltatások által biztosított hello hello PowerShell-parancsmagok használatával kezelhető az Azure Automationben [Azure PowerShell eszközök](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="6d29d-112">Azure cloud services can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="6d29d-113">Azure Automation a felhőalapú szolgáltatás PowerShell parancsmagok használatával elérhető hello kezdő verzióról rendelkezik, így végezhetők el a cloud service felügyeleti feladatok hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6d29d-113">Azure Automation has these cloud service PowerShell cmdlets available out of hello box, so that you can perform all of your cloud service management tasks within hello service.</span></span> <span data-ttu-id="6d29d-114">Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="6d29d-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="6d29d-115">Néhány példa az Azure felhőalapú szolgáltatások közé tartoznak az Azure Automation toomanage használ:</span><span class="sxs-lookup"><span data-stu-id="6d29d-115">Some example uses of Azure Automation toomanage Azure Cloud Services include:</span></span>

* [<span data-ttu-id="6d29d-116">Egy Felhőszolgáltatás, amikor az Azure Blob storage frissítése a szolgáltatáskonfigurációs séma vagy cspkg folyamatos telepítése</span><span class="sxs-lookup"><span data-stu-id="6d29d-116">Continous deployment of a Cloud Service whenever cscfg or cspkg is updated in Azure Blob storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [<span data-ttu-id="6d29d-117">A felhőalapú szolgáltatás példányok párhuzamosan, egyszerre több frissítési tartományt újraindítása</span><span class="sxs-lookup"><span data-stu-id="6d29d-117">Rebooting Cloud Service instances in parallel, one upgrade domain at a time</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a><span data-ttu-id="6d29d-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d29d-118">Next Steps</span></span>
<span data-ttu-id="6d29d-119">Most, hogy megismerte az Azure Automation, és hogyan lehet Azure felhőszolgáltatások használt toomanage hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="6d29d-119">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure cloud services, follow these links toolearn more about Azure Automation.</span></span>

* [<span data-ttu-id="6d29d-120">Azure Automation – áttekintés</span><span class="sxs-lookup"><span data-stu-id="6d29d-120">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="6d29d-121">Az első runbookom</span><span class="sxs-lookup"><span data-stu-id="6d29d-121">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="6d29d-122">Azure Automation tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="6d29d-122">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)

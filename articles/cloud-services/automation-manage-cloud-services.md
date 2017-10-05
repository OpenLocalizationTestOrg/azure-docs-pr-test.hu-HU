---
title: "Azure Automation használatával Azure Cloud Services kezelése |} Microsoft Docs"
description: "További tudnivalók hogyan az Azure Automation szolgáltatás léptékű Azure felhőszolgáltatások kezelésére használható."
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
ms.openlocfilehash: 6b5acac1b8647c324988c316cd5602b3dba98a1d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a><span data-ttu-id="32a15-103">Azure Automation használata Azure Cloud Services kezelése</span><span class="sxs-lookup"><span data-stu-id="32a15-103">Managing Azure Cloud Services using Azure Automation</span></span>
<span data-ttu-id="32a15-104">Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható az Azure felhőalapú szolgáltatások felügyelete leegyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="32a15-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of your Azure cloud services.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="32a15-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="32a15-105">What is Azure Automation?</span></span>
<span data-ttu-id="32a15-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="32a15-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="32a15-107">Azure Automation használ, hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="32a15-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="32a15-108">Azure Automation szolgáltatásbeli egy magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely az igényeinek, ha a szervezet növekedésének megfelelően méretezi biztosít.</span><span class="sxs-lookup"><span data-stu-id="32a15-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="32a15-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="32a15-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="32a15-110">Működési munkaterhelés csökkentése és az szabadítson fel informatikai / azáltal, hogy a felhő felügyeleti feladatok automatikusan Azure Automation által futtatandó value DevOps személyzet üzleti hozzáadó munkahelyi összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="32a15-110">Lower operational overhead and free up IT / DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a><span data-ttu-id="32a15-111">Hogyan segíthet az Azure Automation Azure cloud services kezelése?</span><span class="sxs-lookup"><span data-stu-id="32a15-111">How can Azure Automation help manage Azure cloud services?</span></span>
<span data-ttu-id="32a15-112">Azure felhőszolgáltatások által biztosított PowerShell-parancsmagok használatával kezelhető az Azure Automationben a [Azure PowerShell eszközök](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="32a15-112">Azure cloud services can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="32a15-113">Azure Automation a felhőalapú szolgáltatás PowerShell parancsmagok használatával elérhető alapesetben rendelkezik, így a felhőalapú szolgáltatás felügyeleti feladatokat a szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="32a15-113">Azure Automation has these cloud service PowerShell cmdlets available out of the box, so that you can perform all of your cloud service management tasks within the service.</span></span> <span data-ttu-id="32a15-114">Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások, Azure-szolgáltatások és a 3. fél rendszerek között összetett feladatok automatizálása a parancsmagjaival is párosítható.</span><span class="sxs-lookup"><span data-stu-id="32a15-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="32a15-115">Néhány példa használati Azure Automation Azure Cloud Services kezelése, többek között:</span><span class="sxs-lookup"><span data-stu-id="32a15-115">Some example uses of Azure Automation to manage Azure Cloud Services include:</span></span>

* [<span data-ttu-id="32a15-116">Egy Felhőszolgáltatás, amikor az Azure Blob storage frissítése a szolgáltatáskonfigurációs séma vagy cspkg folyamatos telepítése</span><span class="sxs-lookup"><span data-stu-id="32a15-116">Continous deployment of a Cloud Service whenever cscfg or cspkg is updated in Azure Blob storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [<span data-ttu-id="32a15-117">A felhőalapú szolgáltatás példányok párhuzamosan, egyszerre több frissítési tartományt újraindítása</span><span class="sxs-lookup"><span data-stu-id="32a15-117">Rebooting Cloud Service instances in parallel, one upgrade domain at a time</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a><span data-ttu-id="32a15-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32a15-118">Next Steps</span></span>
<span data-ttu-id="32a15-119">Most, hogy megismerte az Azure Automation, és hogyan használat Azure cloud services kezelése alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="32a15-119">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure cloud services, follow these links to learn more about Azure Automation.</span></span>

* [<span data-ttu-id="32a15-120">Azure Automation – áttekintés</span><span class="sxs-lookup"><span data-stu-id="32a15-120">Azure Automation Overview</span></span>](../automation/automation-intro.md)
* [<span data-ttu-id="32a15-121">Az első runbookom</span><span class="sxs-lookup"><span data-stu-id="32a15-121">My first runbook</span></span>](../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="32a15-122">Azure Automation tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="32a15-122">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)

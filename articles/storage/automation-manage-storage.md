---
title: "Azure Storage használata az Azure Automation kezelése"
description: "Hogyan az Azure Automation szolgáltatás segítségével kezelheti az Azure Storage léptékű megismerése."
services: storage, automation
documentationcenter: 
author: jodoglevy
manager: eamono
editor: 
ms.assetid: bac41c39-1d95-46aa-a481-ec17bbb21b40
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: eamono
ms.openlocfilehash: fbf1cd9e73615f8d991f348cb705aa9df8b55b4e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-storage-using-azure-automation"></a><span data-ttu-id="5a670-103">Azure Storage használata az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="5a670-103">Managing Azure Storage using Azure Automation</span></span>
<span data-ttu-id="5a670-104">Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható egyszerűbbé teheti a Azure Storage-blobot, táblát és üzenetsort kezelését.</span><span class="sxs-lookup"><span data-stu-id="5a670-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of your Azure Storage blobs, tables, and queues.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="5a670-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="5a670-105">What is Azure Automation?</span></span>
<span data-ttu-id="5a670-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="5a670-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="5a670-107">Azure Automation használ, hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatok automatizálható a megbízhatóság és a hatékonyság növelésére, és idejének csökkentése érdekében a szervezet értékre.</span><span class="sxs-lookup"><span data-stu-id="5a670-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated to increase reliability and efficiency, and reduce time to value for your organization.</span></span>

<span data-ttu-id="5a670-108">Azure Automation szolgáltatásbeli egy magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely az igényeinek, ha a szervezet növekedésének megfelelően méretezi biztosít.</span><span class="sxs-lookup"><span data-stu-id="5a670-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales to meet your needs as your organization grows.</span></span> <span data-ttu-id="5a670-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="5a670-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="5a670-110">Működési munkaterhelés csökkentése és az szabadítson fel informatikai / azáltal, hogy a felhő felügyeleti feladatok automatikusan Azure Automation által futtatandó value DevOps személyzet üzleti hozzáadó munkahelyi összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="5a670-110">Lower operational overhead and free up IT / DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-storage"></a><span data-ttu-id="5a670-111">Hogyan segíthet az Azure Automation kezelése az Azure Storage?</span><span class="sxs-lookup"><span data-stu-id="5a670-111">How can Azure Automation help manage Azure Storage?</span></span>
<span data-ttu-id="5a670-112">Az Azure Storage által biztosított PowerShell-parancsmagok használatával kezelhető az Azure Automationben [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="5a670-112">Azure Storage can be managed in Azure Automation by using the PowerShell cmdlets that are available in [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="5a670-113">Azure Automation a elérhető Storage PowerShell parancsmagok alapesetben rendelkezik, így a blob, table és várólista felügyeleti feladatok a szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="5a670-113">Azure Automation has these Storage PowerShell cmdlets available out of the box, so that you can perform all of your blob, table, and queue management tasks within the service.</span></span> <span data-ttu-id="5a670-114">Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások, Azure-szolgáltatások és a 3. fél rendszerek között összetett feladatok automatizálása a parancsmagjaival is párosítható.</span><span class="sxs-lookup"><span data-stu-id="5a670-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="5a670-115">A [Azure Automation-runbook gyűjtemény](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) termék csoportját, és a Közösség runbookok Ismerkedés az Azure Storage, más Azure-szolgáltatások és a 3. fél rendszerek felügyeletének automatizálására különböző tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5a670-115">The [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks to get started automating management of Azure Storage, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="5a670-116">Gyűjteményelem forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="5a670-116">Gallery runbooks include:</span></span>

* [<span data-ttu-id="5a670-117">Távolítsa el az Azure Storage Blobs, amelyek az egyes napok régi használatával Automation szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5a670-117">Remove Azure Storage Blobs that are Certain Days Old Using Automation Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [<span data-ttu-id="5a670-118">Az Azure Storage Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="5a670-118">Download a Blob from Azure Storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [<span data-ttu-id="5a670-119">Egy Azure virtuális vagy virtuális gépeinek egy felhőalapú szolgáltatás, biztonsági mentés minden lemez</span><span class="sxs-lookup"><span data-stu-id="5a670-119">Backup all disks for a single Azure VM or for all VMs in a Cloud Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a><span data-ttu-id="5a670-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a670-120">Next Steps</span></span>
<span data-ttu-id="5a670-121">Most, hogy megismerte az Azure Automation, és hogyan használható az Azure Storage blobot, táblát és üzenetsort kezelése alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="5a670-121">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Storage blobs, tables, and queues, follow these links to learn more about Azure Automation.</span></span>

<span data-ttu-id="5a670-122">Tekintse meg az Azure Automation [létrehozása vagy importálása az Azure Automationben runbook](../automation/automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="5a670-122">See the Azure Automation tutorial [Creating or importing a runbook in Azure Automation](../automation/automation-creating-importing-runbook.md).</span></span>


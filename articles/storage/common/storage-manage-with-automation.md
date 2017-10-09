---
title: "Azure Storage használata az Azure Automation aaaManage"
description: "További információk a hello Azure Automation szolgáltatás hogyan lehet használt toomanage Azure Storage méretekben."
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
ms.openlocfilehash: 5edfba1a9ce1f9c81b5bd0889de19099f3d86fc6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-storage-using-azure-automation"></a><span data-ttu-id="32d26-103">Azure Storage használata az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="32d26-103">Managing Azure Storage using Azure Automation</span></span>
<span data-ttu-id="32d26-104">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet használt toosimplify kezelése az Azure Storage blobot, táblát és üzenetsort.</span><span class="sxs-lookup"><span data-stu-id="32d26-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of your Azure Storage blobs, tables, and queues.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="32d26-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="32d26-105">What is Azure Automation?</span></span>
<span data-ttu-id="32d26-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="32d26-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="32d26-107">Azure Automation használ, hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatok automatizált tooincrease megbízhatóság és a hatékonyságot, a és a szervezet idő toovalue csökkentése.</span><span class="sxs-lookup"><span data-stu-id="32d26-107">Using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability and efficiency, and reduce time toovalue for your organization.</span></span>

<span data-ttu-id="32d26-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi a igényeinek toomeet a szervezet növekedésének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="32d26-108">Azure Automation provides a highly-reliable and highly-available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="32d26-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="32d26-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="32d26-110">Működési munkaterhelés csökkentése és az szabadítson fel informatikai / DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket hozzáadja az Azure Automation automatikusan futtatásához.</span><span class="sxs-lookup"><span data-stu-id="32d26-110">Lower operational overhead and free up IT / DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-storage"></a><span data-ttu-id="32d26-111">Hogyan segíthet az Azure Automation kezelése az Azure Storage?</span><span class="sxs-lookup"><span data-stu-id="32d26-111">How can Azure Automation help manage Azure Storage?</span></span>
<span data-ttu-id="32d26-112">Az Azure Storage által biztosított hello PowerShell-parancsmagok használatával kezelhető az Azure Automationben [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="32d26-112">Azure Storage can be managed in Azure Automation by using hello PowerShell cmdlets that are available in [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="32d26-113">Azure Automation a elérhető Storage PowerShell parancsmagok hello kezdő verzióról rendelkezik, így a blob, table és várólista felügyeleti feladatok hello szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="32d26-113">Azure Automation has these Storage PowerShell cmdlets available out of hello box, so that you can perform all of your blob, table, and queue management tasks within hello service.</span></span> <span data-ttu-id="32d26-114">Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="32d26-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="32d26-115">Hello [Azure Automation-runbook gyűjtemény](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) termék csoportját, és a Közösség runbookok tooget lépések az Azure Storage, más Azure-szolgáltatások és a 3. fél rendszerek felügyeletének automatizálására különböző tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="32d26-115">hello [Azure Automation runbook gallery](https://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) contains a variety of product team and community runbooks tooget started automating management of Azure Storage, other Azure services, and 3rd party systems.</span></span> <span data-ttu-id="32d26-116">Gyűjteményelem forgatókönyvek a következők:</span><span class="sxs-lookup"><span data-stu-id="32d26-116">Gallery runbooks include:</span></span>

* [<span data-ttu-id="32d26-117">Távolítsa el az Azure Storage Blobs, amelyek az egyes napok régi használatával Automation szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="32d26-117">Remove Azure Storage Blobs that are Certain Days Old Using Automation Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
* [<span data-ttu-id="32d26-118">Az Azure Storage Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="32d26-118">Download a Blob from Azure Storage</span></span>](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
* [<span data-ttu-id="32d26-119">Egy Azure virtuális vagy virtuális gépeinek egy felhőalapú szolgáltatás, biztonsági mentés minden lemez</span><span class="sxs-lookup"><span data-stu-id="32d26-119">Backup all disks for a single Azure VM or for all VMs in a Cloud Service</span></span>](https://gallery.technet.microsoft.com/scriptcenter/Backup-all-disks-for-a-ede940d5)

## <a name="next-steps"></a><span data-ttu-id="32d26-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="32d26-120">Next Steps</span></span>
<span data-ttu-id="32d26-121">Most, hogy megismerte az Azure Automation, és hogyan Azure Storage-blobok használt toomanage, táblát és üzenetsort hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="32d26-121">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Storage blobs, tables, and queues, follow these links toolearn more about Azure Automation.</span></span>

<span data-ttu-id="32d26-122">Lásd: hello Azure Automation-oktatóanyag [létrehozása vagy importálása az Azure Automationben runbook](../../automation/automation-creating-importing-runbook.md).</span><span class="sxs-lookup"><span data-stu-id="32d26-122">See hello Azure Automation tutorial [Creating or importing a runbook in Azure Automation](../../automation/automation-creating-importing-runbook.md).</span></span>


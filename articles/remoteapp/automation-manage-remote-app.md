---
title: "Azure RemoteApp használata az Azure Automation kezelése |} Microsoft Docs"
description: "További tudnivalók hogyan az Azure Automation szolgáltatás Azure RemoteApp kezelésére használható."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 589f01ef-f9c1-4e7b-a040-1b46862d3544
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: magoedte;csand
ms.openlocfilehash: 59ac11f153c0bd74a1106400dbbf5790968b657c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-remoteapp-using-azure-automation"></a><span data-ttu-id="f6fac-103">Azure RemoteApp használata az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="f6fac-103">Managing Azure RemoteApp using Azure Automation</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f6fac-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="f6fac-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="f6fac-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="f6fac-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="f6fac-106">Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható az Azure RemoteApp felügyeleti leegyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="f6fac-106">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure RemoteApp.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="f6fac-107">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="f6fac-107">What is Azure Automation?</span></span>
<span data-ttu-id="f6fac-108">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="f6fac-108">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="f6fac-109">Azure Automation használ, manuális, gyakran ismétlődő, hosszan futó és hibalehetőséget feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="f6fac-109">Using Azure Automation, manual, frequently-repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="f6fac-110">Azure Automation szolgáltatásbeli biztosít egy magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="f6fac-110">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="f6fac-111">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="f6fac-111">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="f6fac-112">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai és DevOps alkalmazottak munka, amely üzleti értéket hozzáadja a felhőbeli felügyeleti feladatok automatikusan Azure Automation által futtatandó mozgatásával összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="f6fac-112">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-remoteapp"></a><span data-ttu-id="f6fac-113">Hogyan segíthet az Azure Automation kezelése az Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="f6fac-113">How can Azure Automation help manage Azure RemoteApp?</span></span>
<span data-ttu-id="f6fac-114">RemoteApp által biztosított PowerShell-parancsmagok használatával kezelhető az Azure Automationben a [Azure PowerShell eszközök](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="f6fac-114">RemoteApp can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="f6fac-115">Azure Automation a érhető el a RemoteApp PowerShell-parancsmagok alapesetben rendelkezik, így a RemoteApp felügyeleti feladatokat a szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="f6fac-115">Azure Automation has these RemoteApp PowerShell cmdlets available out of the box, so that you can perform all of your RemoteApp management tasks within the service.</span></span> <span data-ttu-id="f6fac-116">Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások, Azure-szolgáltatások és a 3. fél rendszerek között összetett feladatok automatizálása a parancsmagjaival is párosítható.</span><span class="sxs-lookup"><span data-stu-id="f6fac-116">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f6fac-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6fac-117">Next steps</span></span>
<span data-ttu-id="f6fac-118">Most, hogy megismerte az Azure Automation, és hogyan használható az Azure RemoteApp kezeléséhez alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="f6fac-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure RemoteApp, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="f6fac-119">Tekintse meg az Azure Automation szolgáltatásbeli [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="f6fac-119">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>


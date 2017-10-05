---
title: "Azure Automation DSC – áttekintés |} Microsoft Docs"
description: "Egy áttekintés az Azure Automation szükséges konfiguráló (DSC), a feltételek és ismert problémák"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "PowerShell dsc, a kívánt állapot konfigurációs, a powershell dsc azure"
ms.assetid: fd40cb68-c1a6-48c3-bba2-710b607d1555
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 468321fa6863d78bc0d179fbe5c2ed6195040d50
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-dsc-overview"></a><span data-ttu-id="69cfa-104">Azure Automation DSC – áttekintés</span><span class="sxs-lookup"><span data-stu-id="69cfa-104">Azure Automation DSC Overview</span></span>

<span data-ttu-id="69cfa-105">Az Azure Automation DSC az Azure-szolgáltatások, amely lehetővé teszi írási, kezelése és PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) fordítási [konfigurációk](https://msdn.microsoft.com/powershell/dsc/configurations), importálása [DSC erőforrások](https://msdn.microsoft.com/powershell/dsc/resources), és rendelje hozzá a konfigurációk a célcsomópontokat, mind a felhőben.</span><span class="sxs-lookup"><span data-stu-id="69cfa-105">Azure Automation DSC is an Azure service that allows you to write, manage, and compile PowerShell Desired State Configuration (DSC) [configurations](https://msdn.microsoft.com/powershell/dsc/configurations), import [DSC Resources](https://msdn.microsoft.com/powershell/dsc/resources), and assign configurations to target nodes, all in the cloud.</span></span>

## <a name="why-use-azure-automation-dsc"></a><span data-ttu-id="69cfa-106">Miért érdemes használni az Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="69cfa-106">Why use Azure Automation DSC</span></span>

<span data-ttu-id="69cfa-107">Az Azure Automation DSC számos előnye van, mint az Azure-on kívüli DSC.</span><span class="sxs-lookup"><span data-stu-id="69cfa-107">Azure Automation DSC provides several advantages over using DSC outside of Azure.</span></span>

### <a name="built-in-pull-server"></a><span data-ttu-id="69cfa-108">Beépített lekérési kiszolgálójával</span><span class="sxs-lookup"><span data-stu-id="69cfa-108">Built-in pull server</span></span>

<span data-ttu-id="69cfa-109">Azure Automation szolgáltatásbeli biztosít egy [DSC lekérési kiszolgálójával](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) , hogy a célcsomópontokat automatikusan megjelenik a konfigurációk, felelnek meg a kívánt állapot, és megfelelőségi jelentéseket küldhetnek vissza.</span><span class="sxs-lookup"><span data-stu-id="69cfa-109">Azure Automation provides a [DSC pull server](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) so that target nodes automatically receive configurations, conform to the desired state, and report back on their compliance.</span></span>
<span data-ttu-id="69cfa-110">A beépített lekérési kiszolgálójával, az Azure Automationben szükségtelenné teszi beállítása és karbantartása a saját lekérési kiszolgálójával.</span><span class="sxs-lookup"><span data-stu-id="69cfa-110">The built-in pull server in Azure Automation eliminates the need to set up and maintain your own pull server.</span></span>
<span data-ttu-id="69cfa-111">Azure Automation szolgáltatásbeli virtuális vagy fizikai Windows vagy Linux rendszerű gépek, a felhőben, vagy a helyszíni célba.</span><span class="sxs-lookup"><span data-stu-id="69cfa-111">Azure Automation can target virtual or physical Windows or Linux machines, in the cloud or on-premises.</span></span>

### <a name="management-of-all-your-dsc-artifacts"></a><span data-ttu-id="69cfa-112">A DSC-összetevők felügyelete</span><span class="sxs-lookup"><span data-stu-id="69cfa-112">Management of all your DSC artifacts</span></span>

<span data-ttu-id="69cfa-113">Az Azure Automation DSC biztosítható az ugyanabban a felügyeleti réteg [PowerShell célállapot-konfiguráció](https://msdn.microsoft.com/powershell/dsc/overview) , az Azure Automation kínál a PowerShell-parancsprogramok.</span><span class="sxs-lookup"><span data-stu-id="69cfa-113">Azure Automation DSC brings the same management layer to [PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) as Azure Automation offers for PowerShell scripting.</span></span>

<span data-ttu-id="69cfa-114">Azure-portálról, vagy a PowerShell kezelheti az összes a DSC konfigurációk, erőforrások és célcsomópontokat.</span><span class="sxs-lookup"><span data-stu-id="69cfa-114">From the Azure portal, or from PowerShell, you can manage all your DSC configurations, resources, and target nodes.</span></span>

![Az Azure Automation panel képernyőfelvétele](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a><span data-ttu-id="69cfa-116">Jelentési adatok importálása a Naplóelemzési</span><span class="sxs-lookup"><span data-stu-id="69cfa-116">Import reporting data into Log Analytics</span></span>

<span data-ttu-id="69cfa-117">Az Azure Automation DSC Szolgáltatásban felügyelt csomópontok részletes jelentéskészítési állapot adatokat küldeni a a beépített lekérési kiszolgálójával.</span><span class="sxs-lookup"><span data-stu-id="69cfa-117">Nodes that are managed with Azure Automation DSC send detailed reporting status data to the built-in pull server.</span></span>
<span data-ttu-id="69cfa-118">Azure Automation DSC ezeket az adatokat küldeni a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="69cfa-118">You can configure Azure Automation DSC to send this data to your Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>
<span data-ttu-id="69cfa-119">A DSC állapot adatokat küldeni a Naplóelemzési munkaterület, lásd: [előre Azure Automation DSC jelentésadatait OMS szolgáltatáshoz](automation-dsc-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="69cfa-119">To learn how to send DSC status data to your Log Analytics workspace, see [Forward Azure Automation DSC reporting data to OMS Log Analytics](automation-dsc-diagnostics.md).</span></span>

## <a name="introduction-video"></a><span data-ttu-id="69cfa-120">Bevezető videó</span><span class="sxs-lookup"><span data-stu-id="69cfa-120">Introduction video</span></span>

<span data-ttu-id="69cfa-121">Inkább néz videót olvasás helyett?</span><span class="sxs-lookup"><span data-stu-id="69cfa-121">Prefer watching to reading?</span></span> <span data-ttu-id="69cfa-122">Rá egy pillantást az alábbi videó az 2015. május, amikor bejelentette először Azure Automation DSC Szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="69cfa-122">Have a look at the following video from May 2015, when Azure Automation DSC was first announced.</span></span>

>[!NOTE]
><span data-ttu-id="69cfa-123">A fogalmakat, és ez a videó tárgyalt életciklusa helyesek, amíg a Azure Automation DSC sokkal fejlődött, mivel ez a videó lett felvéve.</span><span class="sxs-lookup"><span data-stu-id="69cfa-123">While the concepts and life cycle discussed in this video are correct, Azure Automation DSC has progressed a lot since this video was recorded.</span></span>
><span data-ttu-id="69cfa-124">Az általánosan elérhető van jóval szélesebb körű felhasználói Felületet az Azure portálon, és számos további képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="69cfa-124">It is now generally available, has a much more extensive UI in the Azure portal, and supports many additional capabilities.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a><span data-ttu-id="69cfa-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69cfa-125">Next steps</span></span>

* <span data-ttu-id="69cfa-126">Megtudhatja, hogyan előkészítésére csomópontokat a felügyelethez az Azure Automation DSC Szolgáltatásban, lásd: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md)</span><span class="sxs-lookup"><span data-stu-id="69cfa-126">To learn how to onboard nodes to be managed with Azure Automation DSC, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md)</span></span>
* <span data-ttu-id="69cfa-127">Azure Automation DSC használatának megkezdéséhez tekintse meg [Ismerkedés az Azure Automation DSC](automation-dsc-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="69cfa-127">To get started using Azure Automation DSC, see [Getting started with Azure Automation DSC](automation-dsc-getting-started.md)</span></span>
* <span data-ttu-id="69cfa-128">A DSC-konfigurációk fordítása, így hozzárendelheti azokat a célcsomópontokat, lásd: [fordítása Azure Automation DSC-konfigurációja](automation-dsc-compile.md)</span><span class="sxs-lookup"><span data-stu-id="69cfa-128">To learn about compiling DSC configurations so that you can assign them to target nodes, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md)</span></span>
* <span data-ttu-id="69cfa-129">PowerShell parancsmag-referencia az Azure Automation DSC Szolgáltatásban, lásd: [Azure Automation DSC-parancsmagok](/powershell/module/azurerm.automation/#automation)</span><span class="sxs-lookup"><span data-stu-id="69cfa-129">For PowerShell cmdlet reference for Azure Automation DSC, see [Azure Automation DSC cmdlets](/powershell/module/azurerm.automation/#automation)</span></span>
* <span data-ttu-id="69cfa-130">Díjszabási információkért lásd: [Azure Automation DSC díjszabása](https://azure.microsoft.com/pricing/details/automation/)</span><span class="sxs-lookup"><span data-stu-id="69cfa-130">For pricing information, see [Azure Automation DSC pricing](https://azure.microsoft.com/pricing/details/automation/)</span></span>
* <span data-ttu-id="69cfa-131">Egy Azure Automation DSC használata a folyamatos üzembe helyezési folyamat példát, olvassa el [IaaS virtuális gépek használata Azure Automation DSC és Chocolatey folyamatos üzembe helyezés](automation-dsc-cd-chocolatey.md)</span><span class="sxs-lookup"><span data-stu-id="69cfa-131">To see an example of using Azure Automation DSC in a continuous deployment pipeline, see  [Continuous Deployment to IaaS VMs Using Azure Automation DSC and Chocolatey](automation-dsc-cd-chocolatey.md)</span></span>
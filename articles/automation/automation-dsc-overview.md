---
title: "aaaAzure Automation DSC – áttekintés |} Microsoft Docs"
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
ms.openlocfilehash: 5b8e5104c7b5bed848c015ac26a8b7d1f5b24de9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-dsc-overview"></a><span data-ttu-id="b9be3-104">Azure Automation DSC – áttekintés</span><span class="sxs-lookup"><span data-stu-id="b9be3-104">Azure Automation DSC Overview</span></span>

<span data-ttu-id="b9be3-105">Az Azure Automation DSC az Azure-szolgáltatások, amely lehetővé teszi toowrite, kezelése és PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) fordítási [konfigurációk](https://msdn.microsoft.com/powershell/dsc/configurations), importálja [DSC erőforrások](https://msdn.microsoft.com/powershell/dsc/resources), és rendelje hozzá konfigurációk tootarget csomópontok, minden hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="b9be3-105">Azure Automation DSC is an Azure service that allows you toowrite, manage, and compile PowerShell Desired State Configuration (DSC) [configurations](https://msdn.microsoft.com/powershell/dsc/configurations), import [DSC Resources](https://msdn.microsoft.com/powershell/dsc/resources), and assign configurations tootarget nodes, all in hello cloud.</span></span>

## <a name="why-use-azure-automation-dsc"></a><span data-ttu-id="b9be3-106">Miért érdemes használni az Azure Automation DSC</span><span class="sxs-lookup"><span data-stu-id="b9be3-106">Why use Azure Automation DSC</span></span>

<span data-ttu-id="b9be3-107">Az Azure Automation DSC számos előnye van, mint az Azure-on kívüli DSC.</span><span class="sxs-lookup"><span data-stu-id="b9be3-107">Azure Automation DSC provides several advantages over using DSC outside of Azure.</span></span>

### <a name="built-in-pull-server"></a><span data-ttu-id="b9be3-108">Beépített lekérési kiszolgálójával</span><span class="sxs-lookup"><span data-stu-id="b9be3-108">Built-in pull server</span></span>

<span data-ttu-id="b9be3-109">Azure Automation szolgáltatásbeli biztosít egy [DSC lekérési kiszolgálójával](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) , hogy a célcsomópontokat automatikusan megjelenik a konfigurációk, felel meg a szükséges toohello állapotát, és megfelelőségi jelentéseket küldhetnek vissza.</span><span class="sxs-lookup"><span data-stu-id="b9be3-109">Azure Automation provides a [DSC pull server](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) so that target nodes automatically receive configurations, conform toohello desired state, and report back on their compliance.</span></span>
<span data-ttu-id="b9be3-110">hello beépített lekérési kiszolgálójával, az Azure Automationben hello kell tooset kiküszöböli be, és karbantartása a saját lekérési kiszolgálójával.</span><span class="sxs-lookup"><span data-stu-id="b9be3-110">hello built-in pull server in Azure Automation eliminates hello need tooset up and maintain your own pull server.</span></span>
<span data-ttu-id="b9be3-111">Azure Automation szolgáltatásbeli virtuális vagy fizikai Windows vagy Linux rendszerű gépek, hello felhőalapú vagy helyszíni célba.</span><span class="sxs-lookup"><span data-stu-id="b9be3-111">Azure Automation can target virtual or physical Windows or Linux machines, in hello cloud or on-premises.</span></span>

### <a name="management-of-all-your-dsc-artifacts"></a><span data-ttu-id="b9be3-112">A DSC-összetevők felügyelete</span><span class="sxs-lookup"><span data-stu-id="b9be3-112">Management of all your DSC artifacts</span></span>

<span data-ttu-id="b9be3-113">Az Azure Automation DSC számos lehetőséget kínál azonos felügyeleti réteg túl hello[PowerShell célállapot-konfiguráció](https://msdn.microsoft.com/powershell/dsc/overview) , az Azure Automation kínál a PowerShell-parancsprogramok.</span><span class="sxs-lookup"><span data-stu-id="b9be3-113">Azure Automation DSC brings hello same management layer too[PowerShell Desired State Configuration](https://msdn.microsoft.com/powershell/dsc/overview) as Azure Automation offers for PowerShell scripting.</span></span>

<span data-ttu-id="b9be3-114">Hello Azure-portálon, vagy a PowerShell kezelheti az összes a DSC konfigurációk, erőforrások és célcsomópontokat.</span><span class="sxs-lookup"><span data-stu-id="b9be3-114">From hello Azure portal, or from PowerShell, you can manage all your DSC configurations, resources, and target nodes.</span></span>

![Képernyőfelvétel a hello Azure Automation panel](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a><span data-ttu-id="b9be3-116">Jelentési adatok importálása a Naplóelemzési</span><span class="sxs-lookup"><span data-stu-id="b9be3-116">Import reporting data into Log Analytics</span></span>

<span data-ttu-id="b9be3-117">Az Azure Automation DSC Szolgáltatásban felügyelt csomópontok részletes jelentéskészítési állapot adatok toohello beépített lekérési kiszolgálójával küldése.</span><span class="sxs-lookup"><span data-stu-id="b9be3-117">Nodes that are managed with Azure Automation DSC send detailed reporting status data toohello built-in pull server.</span></span>
<span data-ttu-id="b9be3-118">Azure Automation DSC toosend konfigurálhatja az adatok tooyour a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület.</span><span class="sxs-lookup"><span data-stu-id="b9be3-118">You can configure Azure Automation DSC toosend this data tooyour Microsoft Operations Management Suite (OMS) Log Analytics workspace.</span></span>
<span data-ttu-id="b9be3-119">Hogyan toosend DSC állapot adatok tooyour Naplóelemzési munkaterület: toolearn [előre Azure Automation DSC jelentési adatok tooOMS Naplóelemzési](automation-dsc-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="b9be3-119">toolearn how toosend DSC status data tooyour Log Analytics workspace, see [Forward Azure Automation DSC reporting data tooOMS Log Analytics](automation-dsc-diagnostics.md).</span></span>

## <a name="introduction-video"></a><span data-ttu-id="b9be3-120">Bevezető videó</span><span class="sxs-lookup"><span data-stu-id="b9be3-120">Introduction video</span></span>

<span data-ttu-id="b9be3-121">Tanul tooreading?</span><span class="sxs-lookup"><span data-stu-id="b9be3-121">Prefer watching tooreading?</span></span> <span data-ttu-id="b9be3-122">Tekintse meg a következő videó 2015. május, amikor bejelentette először Azure Automation DSC hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b9be3-122">Have a look at hello following video from May 2015, when Azure Automation DSC was first announced.</span></span>

>[!NOTE]
><span data-ttu-id="b9be3-123">Hello fogalmak és a videó tárgyalt életciklusa helyességét, amíg a Azure Automation DSC sokkal fejlődött, mivel ez a videó lett felvéve.</span><span class="sxs-lookup"><span data-stu-id="b9be3-123">While hello concepts and life cycle discussed in this video are correct, Azure Automation DSC has progressed a lot since this video was recorded.</span></span>
><span data-ttu-id="b9be3-124">Az általánosan elérhető van jóval szélesebb körű felhasználói Felületet a hello Azure-portálon, és számos további képességeket biztosít.</span><span class="sxs-lookup"><span data-stu-id="b9be3-124">It is now generally available, has a much more extensive UI in hello Azure portal, and supports many additional capabilities.</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a><span data-ttu-id="b9be3-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9be3-125">Next steps</span></span>

* <span data-ttu-id="b9be3-126">toolearn hogyan tooonboard csomópontok toobe felügyelete az Azure Automation DSC Szolgáltatásban, lásd: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md)</span><span class="sxs-lookup"><span data-stu-id="b9be3-126">toolearn how tooonboard nodes toobe managed with Azure Automation DSC, see [Onboarding machines for management by Azure Automation DSC](automation-dsc-onboarding.md)</span></span>
* <span data-ttu-id="b9be3-127">Azure Automation DSC használatának tooget lásd [Ismerkedés az Azure Automation DSC](automation-dsc-getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="b9be3-127">tooget started using Azure Automation DSC, see [Getting started with Azure Automation DSC](automation-dsc-getting-started.md)</span></span>
* <span data-ttu-id="b9be3-128">információ fordítása a DSC-konfigurációk, így hozzárendelheti azokat tootarget csomópontok toolearn lásd [fordítása Azure Automation DSC-konfigurációja](automation-dsc-compile.md)</span><span class="sxs-lookup"><span data-stu-id="b9be3-128">toolearn about compiling DSC configurations so that you can assign them tootarget nodes, see [Compiling configurations in Azure Automation DSC](automation-dsc-compile.md)</span></span>
* <span data-ttu-id="b9be3-129">PowerShell parancsmag-referencia az Azure Automation DSC Szolgáltatásban, lásd: [Azure Automation DSC-parancsmagok](/powershell/module/azurerm.automation/#automation)</span><span class="sxs-lookup"><span data-stu-id="b9be3-129">For PowerShell cmdlet reference for Azure Automation DSC, see [Azure Automation DSC cmdlets](/powershell/module/azurerm.automation/#automation)</span></span>
* <span data-ttu-id="b9be3-130">Díjszabási információkért lásd: [Azure Automation DSC díjszabása](https://azure.microsoft.com/pricing/details/automation/)</span><span class="sxs-lookup"><span data-stu-id="b9be3-130">For pricing information, see [Azure Automation DSC pricing](https://azure.microsoft.com/pricing/details/automation/)</span></span>
* <span data-ttu-id="b9be3-131">Azure Automation DSC használata a folyamatos üzembe helyezés sorban, például toosee lásd: [folyamatos üzembe helyezés tooIaaS virtuális gépek használata Azure Automation DSC és Chocolatey](automation-dsc-cd-chocolatey.md)</span><span class="sxs-lookup"><span data-stu-id="b9be3-131">toosee an example of using Azure Automation DSC in a continuous deployment pipeline, see  [Continuous Deployment tooIaaS VMs Using Azure Automation DSC and Chocolatey](automation-dsc-cd-chocolatey.md)</span></span>

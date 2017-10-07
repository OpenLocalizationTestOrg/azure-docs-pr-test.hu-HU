---
title: "Azure RemoteApp használata az Azure Automation aaaManage |} Microsoft Docs"
description: "További információk a hello Azure Automation szolgáltatás hogyan lehet használt toomanage Azure RemoteApp."
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
ms.openlocfilehash: a4cb23e292c797256e971acb3b363b025f140f16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-remoteapp-using-azure-automation"></a><span data-ttu-id="b50d3-103">Azure RemoteApp használata az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="b50d3-103">Managing Azure RemoteApp using Azure Automation</span></span>
> [!IMPORTANT]
> <span data-ttu-id="b50d3-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="b50d3-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="b50d3-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b50d3-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="b50d3-106">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet használt toosimplify kezelése az Azure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b50d3-106">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure RemoteApp.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="b50d3-107">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="b50d3-107">What is Azure Automation?</span></span>
<span data-ttu-id="b50d3-108">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="b50d3-108">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="b50d3-109">Azure Automation használata, manuális, gyakran ismétlődő, hosszan futó és hibalehetőséget feladatok lehet automatizált tooincrease megbízhatóságát, hatékonyságát és a szervezet idő toovalue.</span><span class="sxs-lookup"><span data-stu-id="b50d3-109">Using Azure Automation, manual, frequently-repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="b50d3-110">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely toomeet méretezi igényeinek.</span><span class="sxs-lookup"><span data-stu-id="b50d3-110">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="b50d3-111">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="b50d3-111">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="b50d3-112">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai, és automatikusan Azure Automation futtathatja DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket ad meg.</span><span class="sxs-lookup"><span data-stu-id="b50d3-112">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-remoteapp"></a><span data-ttu-id="b50d3-113">Hogyan segíthet az Azure Automation kezelése az Azure RemoteApp?</span><span class="sxs-lookup"><span data-stu-id="b50d3-113">How can Azure Automation help manage Azure RemoteApp?</span></span>
<span data-ttu-id="b50d3-114">RemoteApp által biztosított hello hello PowerShell-parancsmagok használatával kezelhető az Azure Automationben [Azure PowerShell eszközök](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="b50d3-114">RemoteApp can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell tools](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="b50d3-115">Azure Automation a érhető el a RemoteApp PowerShell-parancsmagok hello kezdő verzióról rendelkezik, így hello szolgáltatáson belül a RemoteApp felügyeleti feladatokat végezheti el.</span><span class="sxs-lookup"><span data-stu-id="b50d3-115">Azure Automation has these RemoteApp PowerShell cmdlets available out of hello box, so that you can perform all of your RemoteApp management tasks within hello service.</span></span> <span data-ttu-id="b50d3-116">Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="b50d3-116">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b50d3-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b50d3-117">Next steps</span></span>
<span data-ttu-id="b50d3-118">Most, hogy megismerte az Azure Automation, és hogyan lehet használt toomanage Azure RemoteApp hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="b50d3-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure RemoteApp, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="b50d3-119">Lásd: hello Azure Automation [első lépéseket ismertető oktatóanyagban](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="b50d3-119">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>


---
title: "Azure Automation virtuális gépek aaaManage |} Microsoft Docs"
description: "További információk a hello Azure Automation szolgáltatás hogyan lehet használt toomanage léptékű Azure virtuális gépeken."
services: virtual-machines-windows, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: ce49f83a-f409-42ee-af74-a8ea2caa9ae8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2016
ms.author: timlt
ms.openlocfilehash: bfe7b3a51b6e82bd7cd5b0a83df7226476ed4f36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-virtual-machines-using-azure-automation"></a><span data-ttu-id="12943-103">Az Azure Virtual Machines kezelése az Azure Automation használatával</span><span class="sxs-lookup"><span data-stu-id="12943-103">Managing Azure Virtual Machines using Azure Automation</span></span>
<span data-ttu-id="12943-104">Ez az útmutató bemutatja a toohello Azure Automation szolgáltatás, és hogyan használható az Azure virtuális gépek kezelésére toosimplify.</span><span class="sxs-lookup"><span data-stu-id="12943-104">This guide introduces you toohello Azure Automation service and how it can be used toosimplify managing your Azure virtual machines.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="12943-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="12943-105">What is Azure Automation?</span></span>
<span data-ttu-id="12943-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="12943-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="12943-107">Azure Automation használatával hosszan futó, manuális, hibákhoz vezethet, és gyakran ismétlődő feladatokat lehet automatizált tooincrease megbízhatóságát, hatékonyságát és az-időérték a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="12943-107">By using Azure Automation, long-running, manual, error-prone, and frequently repeated tasks can be automated tooincrease reliability, efficiency, and time-to-value for your organization.</span></span>

<span data-ttu-id="12943-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású és nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi a igényeinek toomeet a szervezet növekedésének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="12943-108">Azure Automation provides a highly reliable and highly available workflow execution engine that scales toomeet your needs as your organization grows.</span></span> <span data-ttu-id="12943-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a külső rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="12943-109">In Azure Automation, processes can be kicked off manually, by third-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="12943-110">Működési munkaterhelés csökkentése és az szabadítson fel informatikai és DevOps személyzeti toofocus munka, amely a felhőbeli felügyeleti feladatok futtatásával automatikusan az Azure Automation szolgáltatásban, üzleti értéket ad meg.</span><span class="sxs-lookup"><span data-stu-id="12943-110">You can lower operational overhead and free up IT and DevOps staff toofocus on work that adds business value by running your cloud management tasks automatically with Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-virtual-machines"></a><span data-ttu-id="12943-111">Hogyan segíthet az Azure Automation kezelése az Azure virtuális gépeken?</span><span class="sxs-lookup"><span data-stu-id="12943-111">How can Azure Automation help manage Azure virtual machines?</span></span>
<span data-ttu-id="12943-112">Virtuális gépek használatával kezelhető az Azure Automationben [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span><span class="sxs-lookup"><span data-stu-id="12943-112">Virtual machines can be managed in Azure Automation by using [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx).</span></span> <span data-ttu-id="12943-113">Az Azure Automation hello Azure PowerShell-parancsmagok tartalmaz, minden hello szolgáltatást a virtuális gép felügyeleti feladatok végrehajtása érdekében.</span><span class="sxs-lookup"><span data-stu-id="12943-113">Azure Automation includes hello Azure PowerShell cmdlets, so you can perform all of your virtual machine management tasks within hello service.</span></span> <span data-ttu-id="12943-114">Hello parancsmagot az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello parancsmagjai is párosítható az Azure-szolgáltatások és a külső rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="12943-114">You can also pair hello cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and third-party systems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="12943-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="12943-115">Next steps</span></span>
<span data-ttu-id="12943-116">Most, hogy megismerte hello alapjai Azure Automation, és hogyan lehet használt toomanage Azure virtuális gépeken, tudjon meg többet:</span><span class="sxs-lookup"><span data-stu-id="12943-116">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure virtual machines, learn more:</span></span>

* [<span data-ttu-id="12943-117">Azure Automation – áttekintés</span><span class="sxs-lookup"><span data-stu-id="12943-117">Azure Automation Overview</span></span>](../../automation/automation-intro.md)
* [<span data-ttu-id="12943-118">Az első runbookom</span><span class="sxs-lookup"><span data-stu-id="12943-118">My first runbook</span></span>](../../automation/automation-first-runbook-graphical.md)
* [<span data-ttu-id="12943-119">Azure Automation tanulási térkép</span><span class="sxs-lookup"><span data-stu-id="12943-119">Azure Automation learning map</span></span>](https://azure.microsoft.com/documentation/learning-paths/automation/)


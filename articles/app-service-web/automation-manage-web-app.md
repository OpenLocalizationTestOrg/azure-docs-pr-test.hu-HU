---
title: "Azure Web App használatával Azure Automation aaaManage |} Microsoft Docs"
description: "Azure Automation szolgáltatás hello hogyan lehet használt toomanage Azure Web Apps megismerése."
services: app-service\web, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: c960a44f-f941-401d-afba-a4bc0f0394eb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte
ms.openlocfilehash: 6d80351a2927f26753cfbaead6e1c0c3c94e86e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-web-app-using-azure-automation"></a><span data-ttu-id="afe28-103">Azure Web App használatával Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="afe28-103">Managing Azure Web App using Azure Automation</span></span>
<span data-ttu-id="afe28-104">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet Azure Web Apps használt toosimplify kezelését.</span><span class="sxs-lookup"><span data-stu-id="afe28-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure Web App.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="afe28-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="afe28-105">What is Azure Automation?</span></span>
<span data-ttu-id="afe28-106">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="afe28-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="afe28-107">Azure Automation használ, manuális, ismétlődő, hosszan futó és hibalehetőséget feladatok lehet automatizált tooincrease megbízhatóságát, hatékonyságát és idő toovalue a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="afe28-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="afe28-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely toomeet méretezi igényeinek.</span><span class="sxs-lookup"><span data-stu-id="afe28-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="afe28-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="afe28-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="afe28-110">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai, és automatikusan Azure Automation futtathatja DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket ad meg.</span><span class="sxs-lookup"><span data-stu-id="afe28-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a><span data-ttu-id="afe28-111">Hogyan segíthet az Azure Automation kezelése az Azure-webalkalmazás?</span><span class="sxs-lookup"><span data-stu-id="afe28-111">How can Azure Automation help manage Azure Web App?</span></span>
<span data-ttu-id="afe28-112">Webes alkalmazás által biztosított hello hello PowerShell-parancsmagok használatával kezelhető az Azure Automationben [Azure PowerShell-modulok](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="afe28-112">Web App can be managed in Azure Automation by using hello PowerShell cmdlets that are available in hello [Azure PowerShell modules](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="afe28-113">Is [a webes alkalmazás PowerShell-parancsmagjainak telepítése az Azure Automationben](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), hogy a webalkalmazás felügyeleti feladatokat hello szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="afe28-113">You can [install these Web App PowerShell cmdlets in Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), so that you can perform all of your Web App management tasks within hello service.</span></span> <span data-ttu-id="afe28-114">Ezeket a parancsmagokat az Azure Automationben hello parancsmagjaival más Azure-szolgáltatások tooautomate összetett feladatokat is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="afe28-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="afe28-115">Íme néhány példa a alkalmazásszolgáltatások automatizálással kezelése:</span><span class="sxs-lookup"><span data-stu-id="afe28-115">Here are some examples of managing App Services with Automation:</span></span>

* [<span data-ttu-id="afe28-116">Webalkalmazások kezelésére szolgáló parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="afe28-116">Scripts for managing Web Apps</span></span>](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a><span data-ttu-id="afe28-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="afe28-117">Next steps</span></span>
<span data-ttu-id="afe28-118">Most, hogy megismerte az Azure Automation, és hogyan lehet használt toomanage Azure Web Apps hello alapjait, kövesse az alábbi hivatkozások toolearn Azure automatizálásával kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="afe28-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure Web App, follow these links toolearn more about Azure Automation.</span></span>

* <span data-ttu-id="afe28-119">Lásd: hello Azure Automation [első lépéseket ismertető oktatóanyagban](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="afe28-119">See hello Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>


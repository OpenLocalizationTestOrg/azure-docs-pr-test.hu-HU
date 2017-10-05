---
title: "Azure Web App használatával Azure Automation kezelése |} Microsoft Docs"
description: "Hogyan az Azure Automation szolgáltatás segítségével kezelheti az Azure Web Apps megismerése."
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
ms.openlocfilehash: a96332346747114620ee6ebd2a2d0c4417d4a213
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="managing-azure-web-app-using-azure-automation"></a><span data-ttu-id="df253-103">Azure Web App használatával Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="df253-103">Managing Azure Web App using Azure Automation</span></span>
<span data-ttu-id="df253-104">Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható az egyszerűbb kezelés Azure webalkalmazás számára.</span><span class="sxs-lookup"><span data-stu-id="df253-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure Web App.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="df253-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="df253-105">What is Azure Automation?</span></span>
<span data-ttu-id="df253-106">[Azure Automation szolgáltatásbeli](../automation/automation-intro.md) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="df253-106">[Azure Automation](../automation/automation-intro.md) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="df253-107">Használja az Azure Automation, manuális, ismétlődik, hosszan futó és hibalehetőséget feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="df253-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="df253-108">Azure Automation szolgáltatásbeli biztosít egy magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="df253-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="df253-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="df253-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="df253-110">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai és DevOps alkalmazottak munka, amely üzleti értéket hozzáadja a felhőbeli felügyeleti feladatok automatikusan Azure Automation által futtatandó mozgatásával összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="df253-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a><span data-ttu-id="df253-111">Hogyan segíthet az Azure Automation kezelése az Azure-webalkalmazás?</span><span class="sxs-lookup"><span data-stu-id="df253-111">How can Azure Automation help manage Azure Web App?</span></span>
<span data-ttu-id="df253-112">Webes alkalmazás által biztosított PowerShell-parancsmagok használatával kezelhető az Azure Automationben a [Azure PowerShell-modulok](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="df253-112">Web App can be managed in Azure Automation by using the PowerShell cmdlets that are available in the [Azure PowerShell modules](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="df253-113">Is [a webes alkalmazás PowerShell-parancsmagjainak telepítése az Azure Automationben](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), hogy a webalkalmazás felügyeleti feladatokat a szolgáltatáson belül végezheti el.</span><span class="sxs-lookup"><span data-stu-id="df253-113">You can [install these Web App PowerShell cmdlets in Azure Automation](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), so that you can perform all of your Web App management tasks within the service.</span></span> <span data-ttu-id="df253-114">Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások különböző Azure-szolgáltatások és a 3. fél rendszerek összetett feladatok automatizálása a parancsmagjaival is párosítható.</span><span class="sxs-lookup"><span data-stu-id="df253-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="df253-115">Íme néhány példa a alkalmazásszolgáltatások automatizálással kezelése:</span><span class="sxs-lookup"><span data-stu-id="df253-115">Here are some examples of managing App Services with Automation:</span></span>

* [<span data-ttu-id="df253-116">Webalkalmazások kezelésére szolgáló parancsfájlok</span><span class="sxs-lookup"><span data-stu-id="df253-116">Scripts for managing Web Apps</span></span>](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a><span data-ttu-id="df253-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="df253-117">Next steps</span></span>
<span data-ttu-id="df253-118">Most, hogy megismerte az Azure Automation, és hogyan használható Azure Web Apps kezeléséhez alapjait, az alábbi hivatkozásokból tudhat meg többet az Azure Automation.</span><span class="sxs-lookup"><span data-stu-id="df253-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure Web App, follow these links to learn more about Azure Automation.</span></span>

* <span data-ttu-id="df253-119">Tekintse meg az Azure Automation szolgáltatásbeli [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md)</span><span class="sxs-lookup"><span data-stu-id="df253-119">See the Azure Automation [Getting Started Tutorial](../automation/automation-first-runbook-graphical.md)</span></span>


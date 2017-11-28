---
title: "Azure API Management használata az Azure Automation kezelése"
description: "További tudnivalók hogyan az Azure Automation szolgáltatás Azure API Management kezelésére használható."
services: api-management, automation
documentationcenter: 
author: csand-msft
manager: eamono
editor: 
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 73a1135482b88ea7c228bc8b228d47c57b2e70a4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="managing-azure-api-management-using-azure-automation"></a><span data-ttu-id="49e1d-103">Azure API Management használata az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="49e1d-103">Managing Azure API Management using Azure Automation</span></span>
<span data-ttu-id="49e1d-104">Az útmutatóból megismerheti az Azure Automation szolgáltatás, és hogyan használható az Azure API Management felügyeleti leegyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="49e1d-104">This guide will introduce you to the Azure Automation service, and how it can be used to simplify management of Azure API Management.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="49e1d-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="49e1d-105">What is Azure Automation?</span></span>
<span data-ttu-id="49e1d-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="49e1d-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="49e1d-107">Használja az Azure Automation, manuális, ismétlődik, hosszan futó és hibalehetőséget feladatok automatizálhatók megbízhatóságát, hatékonyságát és a szervezet hamarabb növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="49e1d-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated to increase reliability, efficiency, and time to value for your organization.</span></span>

<span data-ttu-id="49e1d-108">Azure Automation szolgáltatásbeli biztosít egy magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely méretezi az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="49e1d-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales to meet your needs.</span></span> <span data-ttu-id="49e1d-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="49e1d-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="49e1d-110">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai és DevOps alkalmazottak munka, amely üzleti értéket hozzáadja a felhőbeli felügyeleti feladatok automatikusan Azure Automation által futtatandó mozgatásával összpontosíthat.</span><span class="sxs-lookup"><span data-stu-id="49e1d-110">Reduce operational overhead and free up IT and DevOps staff to focus on work that adds business value by moving your cloud management tasks to be run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a><span data-ttu-id="49e1d-111">Hogyan segíthet az Azure Automation kezelése az Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="49e1d-111">How can Azure Automation help manage Azure API Management?</span></span>
<span data-ttu-id="49e1d-112">Az API Management használatával kezelhető az Azure Automationben a [Azure API Management API Windows PowerShell-parancsmagok](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span><span class="sxs-lookup"><span data-stu-id="49e1d-112">API Management can be managed in Azure Automation by using the [Windows PowerShell cmdlets for Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span></span> <span data-ttu-id="49e1d-113">Azure Automation belül szkripteket PowerShell munkafolyamat a parancsmagokkal API-felügyeleti feladatok elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="49e1d-113">Within Azure Automation you can write PowerShell workflow scripts to perform many of your API Management tasks using the cmdlets.</span></span> <span data-ttu-id="49e1d-114">Ezek a parancsmagok az Azure Automationben más Azure-szolgáltatások, Azure-szolgáltatások és a 3. fél rendszerek között összetett feladatok automatizálása a parancsmagjaival is párosítható.</span><span class="sxs-lookup"><span data-stu-id="49e1d-114">You can also pair these cmdlets in Azure Automation with the cmdlets for other Azure services, to automate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="49e1d-115">Íme néhány példa a automatizálást API Management használata:</span><span class="sxs-lookup"><span data-stu-id="49e1d-115">Here are some examples of using API Management with Automation:</span></span>

* [<span data-ttu-id="49e1d-116">Az Azure API Management – Using PowerShell biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="49e1d-116">Azure API Management – Using PowerShell for backup and restore</span></span>](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a><span data-ttu-id="49e1d-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="49e1d-117">Next Steps</span></span>
<span data-ttu-id="49e1d-118">Most, hogy megismerte az Azure Automation, és hogyan használható az Azure API Management kezelése alapjait, az alábbi hivatkozásokból további.</span><span class="sxs-lookup"><span data-stu-id="49e1d-118">Now that you've learned the basics of Azure Automation and how it can be used to manage Azure API Management, follow these links to learn more.</span></span>

* <span data-ttu-id="49e1d-119">Tekintse meg az Azure Automation [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="49e1d-119">See the Azure Automation [getting started tutorial](../automation/automation-first-runbook-graphical.md).</span></span>


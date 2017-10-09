---
title: "Azure API Management használata az Azure Automation aaaManage"
description: "További információk a hello Azure Automation szolgáltatás hogyan lehet használt toomanage Azure API Management."
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
ms.openlocfilehash: 05b8e3cad786fa701feb88f7b6d9629f16686d15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-azure-api-management-using-azure-automation"></a><span data-ttu-id="ad08e-103">Azure API Management használata az Azure Automation kezelése</span><span class="sxs-lookup"><span data-stu-id="ad08e-103">Managing Azure API Management using Azure Automation</span></span>
<span data-ttu-id="ad08e-104">Az útmutatóból megismerheti toohello Azure Automation szolgáltatás, és hogyan lehet Azure API Management használt toosimplify kezelését.</span><span class="sxs-lookup"><span data-stu-id="ad08e-104">This guide will introduce you toohello Azure Automation service, and how it can be used toosimplify management of Azure API Management.</span></span>

## <a name="what-is-azure-automation"></a><span data-ttu-id="ad08e-105">Mi az Azure Automation?</span><span class="sxs-lookup"><span data-stu-id="ad08e-105">What is Azure Automation?</span></span>
<span data-ttu-id="ad08e-106">[Azure Automation szolgáltatásbeli](https://azure.microsoft.com/services/automation/) egy Azure szolgáltatás felhő kezelésüket folyamatok automatizálása révén.</span><span class="sxs-lookup"><span data-stu-id="ad08e-106">[Azure Automation](https://azure.microsoft.com/services/automation/) is an Azure service for simplifying cloud management through process automation.</span></span> <span data-ttu-id="ad08e-107">Azure Automation használ, manuális, ismétlődő, hosszan futó és hibalehetőséget feladatok lehet automatizált tooincrease megbízhatóságát, hatékonyságát és idő toovalue a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="ad08e-107">Using Azure Automation, manual, repeated, long-running, and error-prone tasks can be automated tooincrease reliability, efficiency, and time toovalue for your organization.</span></span>

<span data-ttu-id="ad08e-108">Azure Automation szolgáltatásbeli biztosít a magas rendelkezésre állású, nagymértékben megbízható munkafolyamat-végrehajtási motorjának, amely toomeet méretezi igényeinek.</span><span class="sxs-lookup"><span data-stu-id="ad08e-108">Azure Automation provides a highly-reliable, highly-available workflow execution engine that scales toomeet your needs.</span></span> <span data-ttu-id="ad08e-109">Az Azure Automationben folyamatok is lehet kezdődött el manuálisan, a 3. fél rendszerek vagy rendszeres időközönként, hogy a feladatok fordulhat elő, pontosan, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="ad08e-109">In Azure Automation, processes can be kicked off manually, by 3rd-party systems, or at scheduled intervals so that tasks happen exactly when needed.</span></span>

<span data-ttu-id="ad08e-110">Csökkentheti a működési munkaterhelés és szabadítson fel informatikai, és automatikusan Azure Automation futtathatja DevOps személyzet toofocus munka, amely azáltal, hogy a felhő felügyeleti feladatok toobe üzleti értéket ad meg.</span><span class="sxs-lookup"><span data-stu-id="ad08e-110">Reduce operational overhead and free up IT and DevOps staff toofocus on work that adds business value by moving your cloud management tasks toobe run automatically by Azure Automation.</span></span>

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a><span data-ttu-id="ad08e-111">Hogyan segíthet az Azure Automation kezelése az Azure API Management?</span><span class="sxs-lookup"><span data-stu-id="ad08e-111">How can Azure Automation help manage Azure API Management?</span></span>
<span data-ttu-id="ad08e-112">Az API Management hello segítségével kezelhető az Azure Automationben [Azure API Management API Windows PowerShell-parancsmagok](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span><span class="sxs-lookup"><span data-stu-id="ad08e-112">API Management can be managed in Azure Automation by using hello [Windows PowerShell cmdlets for Azure API Management API](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/).</span></span> <span data-ttu-id="ad08e-113">Azure Automation belül írhat PowerShell munkafolyamat parancsfájlok tooperform az API-felügyeleti feladatok hello-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="ad08e-113">Within Azure Automation you can write PowerShell workflow scripts tooperform many of your API Management tasks using hello cmdlets.</span></span> <span data-ttu-id="ad08e-114">Ezeket a parancsmagokat az Azure Automationben más Azure-szolgáltatások, az összetett feladatok tooautomate hello-parancsmagjaival is párosítható az Azure-szolgáltatások és a 3. fél rendszerek között.</span><span class="sxs-lookup"><span data-stu-id="ad08e-114">You can also pair these cmdlets in Azure Automation with hello cmdlets for other Azure services, tooautomate complex tasks across Azure services and 3rd party systems.</span></span>

<span data-ttu-id="ad08e-115">Íme néhány példa a automatizálást API Management használata:</span><span class="sxs-lookup"><span data-stu-id="ad08e-115">Here are some examples of using API Management with Automation:</span></span>

* [<span data-ttu-id="ad08e-116">Az Azure API Management – Using PowerShell biztonsági mentése és visszaállítása</span><span class="sxs-lookup"><span data-stu-id="ad08e-116">Azure API Management – Using PowerShell for backup and restore</span></span>](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## <a name="next-steps"></a><span data-ttu-id="ad08e-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad08e-117">Next Steps</span></span>
<span data-ttu-id="ad08e-118">Most, hogy megismerte az Azure Automation, és hogyan lehet használt toomanage Azure API Management hello alapjait, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="ad08e-118">Now that you've learned hello basics of Azure Automation and how it can be used toomanage Azure API Management, follow these links toolearn more.</span></span>

* <span data-ttu-id="ad08e-119">Lásd: hello Azure Automation [használatába bevezető oktatóanyagot](../automation/automation-first-runbook-graphical.md).</span><span class="sxs-lookup"><span data-stu-id="ad08e-119">See hello Azure Automation [getting started tutorial](../automation/automation-first-runbook-graphical.md).</span></span>


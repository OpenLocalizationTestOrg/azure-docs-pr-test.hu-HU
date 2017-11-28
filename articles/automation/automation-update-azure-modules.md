---
title: "Az Azure Automationben Azure modulok frissítése |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan most már frissítheti az Azure Automationben alapértelmezés szerint biztosított közös Azure PowerShell-modulok."
services: automation
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: tysonn
ms.assetid: 
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/13/2017
ms.author: magoedte
ms.openlocfilehash: ed8c97b642d406a05817ec6c67f31a1b4bce93b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-update-azure-powershell-modules-in-azure-automation"></a><span data-ttu-id="776d0-103">Az Azure Automationben Azure PowerShell-modulok frissítése</span><span class="sxs-lookup"><span data-stu-id="776d0-103">How to update Azure PowerShell modules in Azure Automation</span></span>

<span data-ttu-id="776d0-104">A leggyakoribb Azure PowerShell-modulok minden Automation-fiókban alapértelmezés szerint biztosítottak.</span><span class="sxs-lookup"><span data-stu-id="776d0-104">The most common Azure PowerShell modules are provided by default in each Automation account.</span></span>  <span data-ttu-id="776d0-105">Az Azure-csapat rendszeresen, frissíti az Azure modulok, így lehetővé teszi a modulok a fiók frissítését, ha új verziói érhetők el a portálról az általunk az Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="776d0-105">The Azure team updates the Azure modules regularly, so in the Automation account we provide a way for you to update the modules in the account when new versions are available from the portal.</span></span>  

<span data-ttu-id="776d0-106">Modulok rendszeresen frissülnek a csoport által, mert negatívan befolyásolhatja a változás, például egy paraméter átnevezése vagy teljesen elavulttá parancsmag típusától függően a runbookok a belefoglalt parancsmagok módosítások alakulhat ki.</span><span class="sxs-lookup"><span data-stu-id="776d0-106">Because modules are updated regularly by the product group, changes can occur with the  included cmdlets, which may negatively impact your runbooks depending on the type of change, such as renaming a parameter or deprecating a cmdlet entirely.</span></span> <span data-ttu-id="776d0-107">A runbookok és a folyamatok automatizálásához azok érintő elkerüléséhez erősen ajánlott tesztelése, és a folytatás előtt ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="776d0-107">To avoid impacting your runbooks and the processes they automate, it is strongly recommended that you test and validate before proceeding.</span></span>  <span data-ttu-id="776d0-108">Ha erre a célra készült dedikált Automation-fiók nem rendelkezik, fontolja meg, hogy a teszteléssel számos különböző alkalmazási helyzetek és kombinációinak a runbookok fejlesztése során továbbá frissítése iteratív változásokra hozzon létre egyet a PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="776d0-108">If you do not have a dedicated Automation account intended for this purpose, consider creating one so that you can test many different scenarios and permutations during the development of your runbooks, in addition to iterative changes such as updating the PowerShell modules.</span></span>  <span data-ttu-id="776d0-109">Után az eredményeket a rendszer érvényesíti, és telepítette a szükséges módosításokat, folytassa a runbookokat, amely szükséges a módosítási áttelepítésének koordinációs, és hajtsa végre a frissítést, éles környezetben alább leírtak.</span><span class="sxs-lookup"><span data-stu-id="776d0-109">After the results are validated and you have applied any changes required, proceed with coordinating the migration of any runbooks that required modification and perform the update as described below in production.</span></span>     

## <a name="updating-azure-modules"></a><span data-ttu-id="776d0-110">Az Azure modulok frissítése</span><span class="sxs-lookup"><span data-stu-id="776d0-110">Updating Azure Modules</span></span>

1. <span data-ttu-id="776d0-111">A modulokban panelen található az Automation-fiók nincs lehetőség érhető el nevű **frissítés Azure modulok**.</span><span class="sxs-lookup"><span data-stu-id="776d0-111">In the Modules blade of your Automation account there is an option called **Update Azure Modules**.</span></span>  <span data-ttu-id="776d0-112">Mindig engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="776d0-112">It is always enabled.</span></span><br><br> <span data-ttu-id="776d0-113">![Modulok panel Azure modulok beállítás frissítése](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span><span class="sxs-lookup"><span data-stu-id="776d0-113">![Update Azure Modules option in Modules blade](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span></span>

2. <span data-ttu-id="776d0-114">Kattintson a **frissítés Azure modulok** és az Ön számára jelenik meg, amely arra kéri, ha folytatni kívánja megerősítési értesítés.</span><span class="sxs-lookup"><span data-stu-id="776d0-114">Click **Update Azure Modules** and you will be presented with a confirmation notification that asks you if you want to continue.</span></span><br><br> <span data-ttu-id="776d0-115">![Azure-modulok értesítési frissítése](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span><span class="sxs-lookup"><span data-stu-id="776d0-115">![Update Azure Modules notification](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span></span>

3. <span data-ttu-id="776d0-116">Kattintson a **Igen** , és a modul frissítési folyamat megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="776d0-116">Click **Yes** and the module update process will begin.</span></span>  <span data-ttu-id="776d0-117">A frissítési folyamat veszi körülbelül 15 – 20 perc frissítése a következő modult:</span><span class="sxs-lookup"><span data-stu-id="776d0-117">The update process takes about 15-20 minutes to update the following modules:</span></span>

  * <span data-ttu-id="776d0-118">Azure</span><span class="sxs-lookup"><span data-stu-id="776d0-118">Azure</span></span>
  * <span data-ttu-id="776d0-119">Azure.Storage</span><span class="sxs-lookup"><span data-stu-id="776d0-119">Azure.Storage</span></span>
  * <span data-ttu-id="776d0-120">AzureRm.Automation</span><span class="sxs-lookup"><span data-stu-id="776d0-120">AzureRm.Automation</span></span>
  * <span data-ttu-id="776d0-121">AzureRm.Compute</span><span class="sxs-lookup"><span data-stu-id="776d0-121">AzureRm.Compute</span></span>
  * <span data-ttu-id="776d0-122">AzureRm.Profile</span><span class="sxs-lookup"><span data-stu-id="776d0-122">AzureRm.Profile</span></span>
  * <span data-ttu-id="776d0-123">AzureRm.Resources</span><span class="sxs-lookup"><span data-stu-id="776d0-123">AzureRm.Resources</span></span>
  * <span data-ttu-id="776d0-124">AzureRm.Sql</span><span class="sxs-lookup"><span data-stu-id="776d0-124">AzureRm.Sql</span></span>
  * <span data-ttu-id="776d0-125">AzureRm.Storage</span><span class="sxs-lookup"><span data-stu-id="776d0-125">AzureRm.Storage</span></span>

    <span data-ttu-id="776d0-126">Ha a modulok már naprakészek legyenek, majd a folyamat befejezi néhány másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="776d0-126">If the modules are already up to date, then the process will complete in a few seconds.</span></span>  <span data-ttu-id="776d0-127">A frissítési folyamat befejezéséről értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="776d0-127">When the update process completes you will be notified.</span></span><br><br> ![Azure-modulok frissítési állapotának frissítése](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> <span data-ttu-id="776d0-129">Azure Automation szolgáltatásbeli használni fog a legújabb modulok az Automation-fiók egy új ütemezett feladat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="776d0-129">Azure Automation will use the latest modules in your Automation account when a new scheduled job is run.</span></span>    

<span data-ttu-id="776d0-130">Ha használatával ezek Azure PowerShell-modulok parancsmagjait a runbookok Azure-erőforrások kezeléséhez, majd célszerű minden hónapban a frissítési folyamat végrehajtásához vagy így ahhoz, hogy biztosítsa, hogy rendelkezik-e a legújabb modulok.</span><span class="sxs-lookup"><span data-stu-id="776d0-130">If you use cmdlets from these Azure PowerShell modules in your runbooks to manage Azure resources, then you will want to perform this update process every month or so to assure that you have the latest modules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="776d0-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="776d0-131">Next steps</span></span>

* <span data-ttu-id="776d0-132">Integrációs modulok és az egyéni modulok további Automation integrálása más rendszerek, a szolgáltatások vagy a megoldások létrehozásával kapcsolatos további információkért lásd: [integrációs modulok](automation-integration-modules.md).</span><span class="sxs-lookup"><span data-stu-id="776d0-132">To learn more about Integration Modules and how to create custom modules to further integrate Automation with other systems, services, or solutions, see [Integration Modules](automation-integration-modules.md).</span></span>

* <span data-ttu-id="776d0-133">Érdemes lehet forrás vezérlő integrációs [GitHub vállalati](automation-scenario-source-control-integration-with-github-ent.md) vagy [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) központi kezelésére, és szabályozhatja a automatizálási runbook és konfigurációs portfóliót kiadásaiban.</span><span class="sxs-lookup"><span data-stu-id="776d0-133">Consider source control integration using [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) or [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) to centrally manage and control releases of your Automation runbook and configuration portfolio.</span></span>  
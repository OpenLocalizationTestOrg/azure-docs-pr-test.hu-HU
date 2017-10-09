---
title: aaaUpdate Azure az Azure Automationben modulok |} Microsoft Docs
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
ms.openlocfilehash: afa404a643349f044e55be2280e435b575049dec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooupdate-azure-powershell-modules-in-azure-automation"></a><span data-ttu-id="d01e9-103">Hogyan tooupdate Azure PowerShell-modulok az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="d01e9-103">How tooupdate Azure PowerShell modules in Azure Automation</span></span>

<span data-ttu-id="d01e9-104">az egyes Automation-fiók alapértelmezés szerint biztosított hello leggyakoribb Azure PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="d01e9-104">hello most common Azure PowerShell modules are provided by default in each Automation account.</span></span>  <span data-ttu-id="d01e9-105">hello Azure csapata frissítések rendszeresen, hello Azure modulok, így az Automation-fiók hello nyújtunk oly módon, tooupdate hello modulok hello fiók Ha új verzió hello portálról érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d01e9-105">hello Azure team updates hello Azure modules regularly, so in hello Automation account we provide a way for you tooupdate hello modules in hello account when new versions are available from hello portal.</span></span>  

<span data-ttu-id="d01e9-106">Modulok a hello termékcsoport rendszeresen frissíteni, mert negatívan befolyásolhatja a runbookokat hello típusú változás, például egy paraméter átnevezése vagy teljesen elavulttá parancsmag függően szereplő hello parancsmagok módosítások alakulhat ki.</span><span class="sxs-lookup"><span data-stu-id="d01e9-106">Because modules are updated regularly by hello product group, changes can occur with hello  included cmdlets, which may negatively impact your runbooks depending on hello type of change, such as renaming a parameter or deprecating a cmdlet entirely.</span></span> <span data-ttu-id="d01e9-107">a runbookok és hello érintő tooavoid folyamatok automatizálása, teszteléséhez, és ellenőrizze a folytatás előtt erősen ajánlott.</span><span class="sxs-lookup"><span data-stu-id="d01e9-107">tooavoid impacting your runbooks and hello processes they automate, it is strongly recommended that you test and validate before proceeding.</span></span>  <span data-ttu-id="d01e9-108">Ha erre a célra készült dedikált Automation-fiók nem rendelkezik, fontolja meg, hozzon létre egyet, hogy számos különböző alkalmazási helyzetek és kombinációinak tesztelheti a runbookokat, továbbá tooiterative módosítások például hello frissítése hello fejlesztése során PowerShell-modulok.</span><span class="sxs-lookup"><span data-stu-id="d01e9-108">If you do not have a dedicated Automation account intended for this purpose, consider creating one so that you can test many different scenarios and permutations during hello development of your runbooks, in addition tooiterative changes such as updating hello PowerShell modules.</span></span>  <span data-ttu-id="d01e9-109">Miután hello eredmények ellenőrzését, és telepítette a szükséges módosításokat, összehangolása szükséges módosítását runbookokat hello áttelepítésének folytatásához és hello frissítését éles alább leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="d01e9-109">After hello results are validated and you have applied any changes required, proceed with coordinating hello migration of any runbooks that required modification and perform hello update as described below in production.</span></span>     

## <a name="updating-azure-modules"></a><span data-ttu-id="d01e9-110">Az Azure modulok frissítése</span><span class="sxs-lookup"><span data-stu-id="d01e9-110">Updating Azure Modules</span></span>

1. <span data-ttu-id="d01e9-111">Hello modulban panelen található az Automation-fiók nincs lehetőség nevű **frissítés Azure modulok**.</span><span class="sxs-lookup"><span data-stu-id="d01e9-111">In hello Modules blade of your Automation account there is an option called **Update Azure Modules**.</span></span>  <span data-ttu-id="d01e9-112">Mindig engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d01e9-112">It is always enabled.</span></span><br><br> <span data-ttu-id="d01e9-113">![Modulok panel Azure modulok beállítás frissítése](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span><span class="sxs-lookup"><span data-stu-id="d01e9-113">![Update Azure Modules option in Modules blade](media/automation-update-azure-modules/automation-update-azure-modules-option.png)</span></span>

2. <span data-ttu-id="d01e9-114">Kattintson a **frissítés Azure modulok** és az Ön számára jelenik meg, amely arra kéri, ha azt szeretné, toocontinue megerősítési értesítés.</span><span class="sxs-lookup"><span data-stu-id="d01e9-114">Click **Update Azure Modules** and you will be presented with a confirmation notification that asks you if you want toocontinue.</span></span><br><br> <span data-ttu-id="d01e9-115">![Azure-modulok értesítési frissítése](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span><span class="sxs-lookup"><span data-stu-id="d01e9-115">![Update Azure Modules notification](media/automation-update-azure-modules/automation-update-azure-modules-popup.png)</span></span>

3. <span data-ttu-id="d01e9-116">Kattintson a **Igen** és hello modul frissítési folyamat megkezdődik.</span><span class="sxs-lookup"><span data-stu-id="d01e9-116">Click **Yes** and hello module update process will begin.</span></span>  <span data-ttu-id="d01e9-117">hello frissítési folyamat veszi 15-20 perc tooupdate hello modulok a következő kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="d01e9-117">hello update process takes about 15-20 minutes tooupdate hello following modules:</span></span>

  * <span data-ttu-id="d01e9-118">Azure</span><span class="sxs-lookup"><span data-stu-id="d01e9-118">Azure</span></span>
  * <span data-ttu-id="d01e9-119">Azure.Storage</span><span class="sxs-lookup"><span data-stu-id="d01e9-119">Azure.Storage</span></span>
  * <span data-ttu-id="d01e9-120">AzureRm.Automation</span><span class="sxs-lookup"><span data-stu-id="d01e9-120">AzureRm.Automation</span></span>
  * <span data-ttu-id="d01e9-121">AzureRm.Compute</span><span class="sxs-lookup"><span data-stu-id="d01e9-121">AzureRm.Compute</span></span>
  * <span data-ttu-id="d01e9-122">AzureRm.Profile</span><span class="sxs-lookup"><span data-stu-id="d01e9-122">AzureRm.Profile</span></span>
  * <span data-ttu-id="d01e9-123">AzureRm.Resources</span><span class="sxs-lookup"><span data-stu-id="d01e9-123">AzureRm.Resources</span></span>
  * <span data-ttu-id="d01e9-124">AzureRm.Sql</span><span class="sxs-lookup"><span data-stu-id="d01e9-124">AzureRm.Sql</span></span>
  * <span data-ttu-id="d01e9-125">AzureRm.Storage</span><span class="sxs-lookup"><span data-stu-id="d01e9-125">AzureRm.Storage</span></span>

    <span data-ttu-id="d01e9-126">Ha hello modulok már toodate be, majd hello folyamat befejezi néhány másodpercen belül.</span><span class="sxs-lookup"><span data-stu-id="d01e9-126">If hello modules are already up toodate, then hello process will complete in a few seconds.</span></span>  <span data-ttu-id="d01e9-127">Hello frissítési folyamat befejezéséről értesítést fog kapni.</span><span class="sxs-lookup"><span data-stu-id="d01e9-127">When hello update process completes you will be notified.</span></span><br><br> ![Azure-modulok frissítési állapotának frissítése](media/automation-update-azure-modules/automation-update-azure-modules-updatestatus.png)

> [!NOTE]
> <span data-ttu-id="d01e9-129">Azure Automation szolgáltatásbeli használni fog hello legújabb modulok az Automation-fiók egy új ütemezett feladat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="d01e9-129">Azure Automation will use hello latest modules in your Automation account when a new scheduled job is run.</span></span>    

<span data-ttu-id="d01e9-130">Parancsmagok használatakor ezeket a runbookok toomanage Azure az Azure PowerShell-modulok az erőforrásokat, akkor érdemes tooperform a frissítési folyamat havonta vagy úgy, hogy rendelkezik tooassure hello legújabb modulok.</span><span class="sxs-lookup"><span data-stu-id="d01e9-130">If you use cmdlets from these Azure PowerShell modules in your runbooks toomanage Azure resources, then you will want tooperform this update process every month or so tooassure that you have hello latest modules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d01e9-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d01e9-131">Next steps</span></span>

* <span data-ttu-id="d01e9-132">További információ az integrációs modulok, és hogyan toocreate az egyéni modulok toofurther integrálhatja más rendszerek, a szolgáltatások vagy a megoldások, Automation toolearn lásd [integrációs modulok](automation-integration-modules.md).</span><span class="sxs-lookup"><span data-stu-id="d01e9-132">toolearn more about Integration Modules and how toocreate custom modules toofurther integrate Automation with other systems, services, or solutions, see [Integration Modules](automation-integration-modules.md).</span></span>

* <span data-ttu-id="d01e9-133">Érdemes lehet forrás vezérlő integrációs [GitHub vállalati](automation-scenario-source-control-integration-with-github-ent.md) vagy [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally kezelésére, és szabályozhatja a automatizálási runbook és konfigurációs portfóliót kiadásaiban.</span><span class="sxs-lookup"><span data-stu-id="d01e9-133">Consider source control integration using [GitHub Enterprise](automation-scenario-source-control-integration-with-github-ent.md) or [Visual Studio Team Services](automation-scenario-source-control-integration-with-vsts.md) toocentrally manage and control releases of your Automation runbook and configuration portfolio.</span></span>  

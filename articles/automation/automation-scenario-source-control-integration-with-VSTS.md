---
title: "Azure Automation integrálása Visual Stuido Team Services verziókezelő |} Microsoft Docs"
description: "A forgatókönyv bemutatja, hogyan egy Azure Automation-fiók és a Visual Stuido Team Services a verziókövetési rendszerrel való integráció beállításával."
services: automation
documentationcenter: 
author: eamono
manager: 
editor: 
keywords: "az Azure powershell, a VSTS, verziókezelő, automatizálás"
ms.assetid: a43b395a-e740-41a3-ae62-40eac9d0ec00
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2017
ms.openlocfilehash: 01f9c01c9e04e02dbb548b68cf99684ba6ddd57e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-automation-scenario---automation-source-control-integration-with-visual-studio-team-services"></a><span data-ttu-id="f16c1-104">Azure Automation forgatókönyv - automatizálási verziókövetés integrálása a Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="f16c1-104">Azure Automation scenario - Automation source control integration with Visual Studio Team Services</span></span>

<span data-ttu-id="f16c1-105">Ebben a forgatókönyvben egy Azure Automation-forgatókönyveket vagy a DSC-konfigurációk verziókövetési felügyeletéhez használt Visual Studio Team Services projekt rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f16c1-105">In this scenario, you have a Visual Studio Team Services project that you are using to manage Azure Automation runbooks or DSC configurations under source control.</span></span>
<span data-ttu-id="f16c1-106">Ez a cikk ismerteti, hogyan VSTS integrálása az Azure Automation-környezet, így folyamatos integrációt történik, az egyes be.</span><span class="sxs-lookup"><span data-stu-id="f16c1-106">This article describes how to integrate VSTS with your Azure Automation environment so that continuous integration happens for each check-in.</span></span>

## <a name="getting-the-scenario"></a><span data-ttu-id="f16c1-107">A forgatókönyv beszerzése</span><span class="sxs-lookup"><span data-stu-id="f16c1-107">Getting the scenario</span></span>

<span data-ttu-id="f16c1-108">Ebben a forgatókönyvben két PowerShell-forgatókönyvek, amelyek közvetlenül importálhatók áll a [forgatókönyvek](automation-runbook-gallery.md) az Azure-portálon vagy letölthető a [PowerShell-galériában](https://www.powershellgallery.com).</span><span class="sxs-lookup"><span data-stu-id="f16c1-108">This scenario consists of two PowerShell runbooks that you can import directly from the [Runbook Gallery](automation-runbook-gallery.md) in the Azure portal or download from the [PowerShell Gallery](https://www.powershellgallery.com).</span></span>

### <a name="runbooks"></a><span data-ttu-id="f16c1-109">Runbookok</span><span class="sxs-lookup"><span data-stu-id="f16c1-109">Runbooks</span></span>

<span data-ttu-id="f16c1-110">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="f16c1-110">Runbook</span></span> | <span data-ttu-id="f16c1-111">Leírás</span><span class="sxs-lookup"><span data-stu-id="f16c1-111">Description</span></span>| 
--------|------------|
<span data-ttu-id="f16c1-112">Szinkronizálási-VSTS</span><span class="sxs-lookup"><span data-stu-id="f16c1-112">Sync-VSTS</span></span> | <span data-ttu-id="f16c1-113">Importálhat forgatókönyvek vagy konfigurációk VSTS verziókezelő végzett egy be.</span><span class="sxs-lookup"><span data-stu-id="f16c1-113">Import runbooks or configurations from VSTS source control when a check-in is done.</span></span> <span data-ttu-id="f16c1-114">Ha manuálisan futtassa azt importálja, és minden runbookok vagy az Automation-fiók konfigurációk közzététele.</span><span class="sxs-lookup"><span data-stu-id="f16c1-114">If run manually, it will import and publish all runbooks or configurations into the Automation account.</span></span>| 
<span data-ttu-id="f16c1-115">Szinkronizálási-VSTSGit</span><span class="sxs-lookup"><span data-stu-id="f16c1-115">Sync-VSTSGit</span></span> | <span data-ttu-id="f16c1-116">Importálhat forgatókönyvek vagy konfigurációk VSTS a Git verziókezelő végzett egy be.</span><span class="sxs-lookup"><span data-stu-id="f16c1-116">Import runbooks or configurations from VSTS under Git source control when a check-in is done.</span></span> <span data-ttu-id="f16c1-117">Ha manuálisan futtassa azt importálja, és minden runbookok vagy az Automation-fiók konfigurációk közzététele.</span><span class="sxs-lookup"><span data-stu-id="f16c1-117">If run manually, it will import and publish all runbooks or configurations into the Automation account.</span></span>|

### <a name="variables"></a><span data-ttu-id="f16c1-118">Változók</span><span class="sxs-lookup"><span data-stu-id="f16c1-118">Variables</span></span>

<span data-ttu-id="f16c1-119">Változó</span><span class="sxs-lookup"><span data-stu-id="f16c1-119">Variable</span></span> | <span data-ttu-id="f16c1-120">Leírás</span><span class="sxs-lookup"><span data-stu-id="f16c1-120">Description</span></span>|
-----------|------------|
<span data-ttu-id="f16c1-121">VSToken</span><span class="sxs-lookup"><span data-stu-id="f16c1-121">VSToken</span></span> | <span data-ttu-id="f16c1-122">Biztonságos változóeszköz hoz létre, amely tartalmazza a VSTS személyes hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="f16c1-122">Secure variable asset you will create that contains the VSTS personal access token.</span></span> <span data-ttu-id="f16c1-123">Megismerheti a VSTS személyes hozzáférési jogkivonat létrehozásához a [VSTS hitelesítés lap](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview).</span><span class="sxs-lookup"><span data-stu-id="f16c1-123">You can learn how to create a VSTS personal access token on the [VSTS authentication page](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview).</span></span> 
## <a name="installing-and-configuring-this-scenario"></a><span data-ttu-id="f16c1-124">A forgatókönyv telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f16c1-124">Installing and configuring this scenario</span></span>

<span data-ttu-id="f16c1-125">Hozzon létre egy [személyes hozzáférési jogkivonat](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) a VSTS, amelyek segítségével a runbookok vagy a beállításokat a rendszerbe az automation-fiók szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="f16c1-125">Create a [personal access token](https://www.visualstudio.com/en-us/docs/integrate/get-started/auth/overview) in VSTS that you will use to sync the runbooks or configurations into your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPersonalToken.png) 

<span data-ttu-id="f16c1-126">Hozzon létre egy [biztonságos változó](automation-variables.md) a személyes hozzáférési jogkivonat tárolásához, hogy a runbook VSTS hitelesítést, és a runbookok vagy konfiguráció az Automation-fiók szinkronizálása az automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="f16c1-126">Create a [secure variable](automation-variables.md) in your automation account to hold the personal access token so that the runbook can authenticate to VSTS and sync the runbooks or configurations into the Automation account.</span></span> <span data-ttu-id="f16c1-127">A VSToken nevére.</span><span class="sxs-lookup"><span data-stu-id="f16c1-127">You can name this VSToken.</span></span> 

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSTokenVariable.png)

<span data-ttu-id="f16c1-128">A forgatókönyv, amely szinkronizálva lesz a runbookok vagy az automation-fiók konfigurációk importálása.</span><span class="sxs-lookup"><span data-stu-id="f16c1-128">Import the runbook that will sync your runbooks or configurations into the automation account.</span></span> <span data-ttu-id="f16c1-129">Használhatja a [VSTS minta runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) vagy az [a Git minta runbook VSTS] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) a PowerShellGallery.com attól függően, hogy a VSTS-forrás használatára vezérlő vagy a Git VSTS, és telepítse az automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="f16c1-129">You can use the [VSTS sample runbook](https://www.powershellgallery.com/packages/Sync-VSTS/1.0/DisplayScript) or the [VSTS with Git sample runbook] (https://www.powershellgallery.com/packages/Sync-VSTSGit/1.0/DisplayScript) from the PowerShellGallery.com depending on if you use VSTS source control or VSTS with Git and deploy to your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPowerShellGallery.png)

<span data-ttu-id="f16c1-130">Most [közzététele](automation-creating-importing-runbook.md#publishing-a-runbook) ezt a runbookot, hogy létrehozhasson egy webhook.</span><span class="sxs-lookup"><span data-stu-id="f16c1-130">You can now [publish](automation-creating-importing-runbook.md#publishing-a-runbook) this runbook so you can create a webhook.</span></span> 
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSPublishRunbook.png)

<span data-ttu-id="f16c1-131">Hozzon létre egy [webhook](automation-webhooks.md) a Sync-VSTS runbookhoz és adja meg a lent látható módon a paramétereket.</span><span class="sxs-lookup"><span data-stu-id="f16c1-131">Create a [webhook](automation-webhooks.md) for this Sync-VSTS runbook and fill in the parameters as shown below.</span></span> <span data-ttu-id="f16c1-132">Győződjön meg arról, szüksége lesz, a szolgáltatás hook a VSTS másolja át a webhook URL-címét.</span><span class="sxs-lookup"><span data-stu-id="f16c1-132">Make sure you copy the webhook url as you will need it for a service hook in VSTS.</span></span> <span data-ttu-id="f16c1-133">A VSAccessTokenVariableName esetén (VSToken) ahhoz, hogy a személyes hozzáférési jogkivonat korábban létrehozott biztonságos változó.</span><span class="sxs-lookup"><span data-stu-id="f16c1-133">The VSAccessTokenVariableName is the name (VSToken) of the secure variable that you created earlier to hold the personal access token.</span></span> 

<span data-ttu-id="f16c1-134">VSTS (szinkronizálási-VSTS.ps1) integrálása lépnek a következő paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="f16c1-134">Integrating with VSTS (Sync-VSTS.ps1) will take the following parameters.</span></span>
### <a name="sync-vsts-parameters"></a><span data-ttu-id="f16c1-135">Szinkronizálási-VSTS-paraméterek</span><span class="sxs-lookup"><span data-stu-id="f16c1-135">Sync-VSTS Parameters</span></span>

<span data-ttu-id="f16c1-136">Paraméter</span><span class="sxs-lookup"><span data-stu-id="f16c1-136">Parameter</span></span> | <span data-ttu-id="f16c1-137">Leírás</span><span class="sxs-lookup"><span data-stu-id="f16c1-137">Description</span></span>| 
--------|------------|
<span data-ttu-id="f16c1-138">WebhookData</span><span class="sxs-lookup"><span data-stu-id="f16c1-138">WebhookData</span></span> | <span data-ttu-id="f16c1-139">A küldött a VSTS szolgáltatás hook beadása információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f16c1-139">This will contain the checkin information sent from the VSTS service hook.</span></span> <span data-ttu-id="f16c1-140">Ez a paraméter kell hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="f16c1-140">You should leave this parameter blank.</span></span>| 
<span data-ttu-id="f16c1-141">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f16c1-141">ResourceGroup</span></span> | <span data-ttu-id="f16c1-142">Azt a nevet, hogy az automation-fiók megtalálható az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="f16c1-142">This is the name of the resource group that the automation account is in.</span></span>|
<span data-ttu-id="f16c1-143">AutomationAccountName</span><span class="sxs-lookup"><span data-stu-id="f16c1-143">AutomationAccountName</span></span> | <span data-ttu-id="f16c1-144">Szinkronizálja a VSTS automation-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-144">The name of the automation account that will sync with VSTS.</span></span>|
<span data-ttu-id="f16c1-145">VSFolder</span><span class="sxs-lookup"><span data-stu-id="f16c1-145">VSFolder</span></span> | <span data-ttu-id="f16c1-146">Amennyiben a runbookokat és konfigurációk léteznek VSTS mappájában neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-146">The name of the folder in VSTS where the runbooks and configurations exist.</span></span>|
<span data-ttu-id="f16c1-147">VSAccount</span><span class="sxs-lookup"><span data-stu-id="f16c1-147">VSAccount</span></span> | <span data-ttu-id="f16c1-148">A Visual Studio Team Services-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="f16c1-148">The name of the Visual Studio Team Services account.</span></span>| 
<span data-ttu-id="f16c1-149">VSAccessTokenVariableName</span><span class="sxs-lookup"><span data-stu-id="f16c1-149">VSAccessTokenVariableName</span></span> | <span data-ttu-id="f16c1-150">A biztonságos változó (VSToken), amely tárolja a VSTS személyes hozzáférési jogkivonat neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-150">The name of the secure variable (VSToken) that holds the VSTS personal access token.</span></span>| 


![](media/automation-scenario-source-control-integration-with-VSTS/VSTSWebhook.png)

<span data-ttu-id="f16c1-151">Ha VSTS git (szinkronizálási-VSTSGit.ps1) használ a következő paraméterek fog tartani.</span><span class="sxs-lookup"><span data-stu-id="f16c1-151">If you are using VSTS with GIT (Sync-VSTSGit.ps1) it will take the following parameters.</span></span>

<span data-ttu-id="f16c1-152">Paraméter</span><span class="sxs-lookup"><span data-stu-id="f16c1-152">Parameter</span></span> | <span data-ttu-id="f16c1-153">Leírás</span><span class="sxs-lookup"><span data-stu-id="f16c1-153">Description</span></span>|
--------|------------|
<span data-ttu-id="f16c1-154">WebhookData</span><span class="sxs-lookup"><span data-stu-id="f16c1-154">WebhookData</span></span> | <span data-ttu-id="f16c1-155">A küldött a VSTS szolgáltatás hook beadása információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f16c1-155">This will contain the checkin information sent from the VSTS service hook.</span></span> <span data-ttu-id="f16c1-156">Ez a paraméter kell hagyja üresen.</span><span class="sxs-lookup"><span data-stu-id="f16c1-156">You should leave this parameter blank.</span></span>| <span data-ttu-id="f16c1-157">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="f16c1-157">ResourceGroup</span></span> | <span data-ttu-id="f16c1-158">Ez az erőforráscsoport, amely az automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="f16c1-158">This the name of the resource group that the automation account is in.</span></span>|
<span data-ttu-id="f16c1-159">AutomationAccountName</span><span class="sxs-lookup"><span data-stu-id="f16c1-159">AutomationAccountName</span></span> | <span data-ttu-id="f16c1-160">Szinkronizálja a VSTS automation-fiók neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-160">The name of the automation account that will sync with VSTS.</span></span>|
<span data-ttu-id="f16c1-161">VSAccount</span><span class="sxs-lookup"><span data-stu-id="f16c1-161">VSAccount</span></span> | <span data-ttu-id="f16c1-162">A Visual Studio Team Services-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="f16c1-162">The name of the Visual Studio Team Services account.</span></span>|
<span data-ttu-id="f16c1-163">VSProject</span><span class="sxs-lookup"><span data-stu-id="f16c1-163">VSProject</span></span> | <span data-ttu-id="f16c1-164">Amennyiben a runbookokat és konfigurációk léteznek VSTS a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="f16c1-164">The name of the project in VSTS where the runbooks and configurations exist.</span></span>|
<span data-ttu-id="f16c1-165">GitRepo</span><span class="sxs-lookup"><span data-stu-id="f16c1-165">GitRepo</span></span> | <span data-ttu-id="f16c1-166">A Git-tárház neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-166">The name of the Git repository.</span></span>|
<span data-ttu-id="f16c1-167">GitBranch</span><span class="sxs-lookup"><span data-stu-id="f16c1-167">GitBranch</span></span> | <span data-ttu-id="f16c1-168">A fiókirodai VSTS Git-tárházban neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-168">The name of the branch in VSTS Git repository.</span></span>|
<span data-ttu-id="f16c1-169">Mappa</span><span class="sxs-lookup"><span data-stu-id="f16c1-169">Folder</span></span> | <span data-ttu-id="f16c1-170">A VSTS Git ágában mappa neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-170">The name of the folder in VSTS Git branch.</span></span>|
<span data-ttu-id="f16c1-171">VSAccessTokenVariableName</span><span class="sxs-lookup"><span data-stu-id="f16c1-171">VSAccessTokenVariableName</span></span> | <span data-ttu-id="f16c1-172">A biztonságos változó (VSToken), amely tárolja a VSTS személyes hozzáférési jogkivonat neve.</span><span class="sxs-lookup"><span data-stu-id="f16c1-172">The name of the secure variable (VSToken) that holds the VSTS personal access token.</span></span>|

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSGitWebhook.png)

<span data-ttu-id="f16c1-173">Hozzon létre egy service hook VSTS jelölőnégyzet-modulok a mappába, amely elindítja a kód be a webhook.</span><span class="sxs-lookup"><span data-stu-id="f16c1-173">Create a service hook in VSTS for check-ins to the folder that triggers this webhook on code check-in.</span></span> <span data-ttu-id="f16c1-174">A szolgáltatás integrálása egy új előfizetés létrehozásakor válassza a webes csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="f16c1-174">Select Web Hooks as the service to integrate with when you create a new subscription.</span></span> <span data-ttu-id="f16c1-175">További service hurkok kapcsolatos a [VSTS szolgáltatás hurkok dokumentáció](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).</span><span class="sxs-lookup"><span data-stu-id="f16c1-175">You can learn more about service hooks on [VSTS Service Hooks documentation](https://www.visualstudio.com/en-us/docs/marketplace/integrate/service-hooks/get-started).</span></span>
![](media/automation-scenario-source-control-integration-with-VSTS/VSTSServiceHook.png)

<span data-ttu-id="f16c1-176">Meg kell tudni hajtsa végre az összes ellenőrzés-modulok a runbookok és a beállításokat a VSTS rendszerbe, és ezek automatikusan be vannak szinkronizálási d azokat az automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="f16c1-176">You should now be able to do all check-ins of your runbooks and configurations into VSTS and have these automatically sync’d into your automation account.</span></span>

![](media/automation-scenario-source-control-integration-with-VSTS/VSTSSyncRunbookOutput.png)

<span data-ttu-id="f16c1-177">Ha futtatja a runbook manuálisan nélkül VSTS által indított alatt, üresen webhookdata paraméter, és a teljes szinkronizálás fog tenni a megadott VSTS mappából.</span><span class="sxs-lookup"><span data-stu-id="f16c1-177">If you run this runbook manually without being triggered by VSTS, you can leave the webhookdata parameter empty and it will do a full sync from the VSTS folder specified.</span></span>

<span data-ttu-id="f16c1-178">Ha kívánja, a forgatókönyv eltávolításához távolítsa el a szolgáltatás hook VSTS, a runbookot és a VSToken változó törlése.</span><span class="sxs-lookup"><span data-stu-id="f16c1-178">If you wish to uninstall the scenario, remove the service hook from VSTS, delete the runbook, and the VSToken variable.</span></span>

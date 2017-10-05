---
title: "Runbookok beállításainak |} Microsoft Docs"
description: "Azure Automation és módosítsa őket az Azure felügyeleti portálon és a Windows PowerShell használatával az egyes runbookok konfigurációs beállításokat ismerteti."
services: automation
documentationcenter: 
author: mgoedtel
manager: stevenka
editor: tysonn
ms.assetid: a726f20c-a952-48b8-88ee-36d76aa3ac61
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/11/2016
ms.author: bwren
ms.openlocfilehash: 20ecbc270e61d234e026e6ba2634c7aad63b3355
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="runbook-settings"></a><span data-ttu-id="530ea-103">Runbook beállításai</span><span class="sxs-lookup"><span data-stu-id="530ea-103">Runbook settings</span></span>
<span data-ttu-id="530ea-104">Azure Automation minden egyes runbookja több beállítással, amelyek segítenek azonosítani és módosítani annak naplózási viselkedését rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="530ea-104">Each runbook in Azure Automation has multiple settings that help it to be identified and to change its logging behavior.</span></span> <span data-ttu-id="530ea-105">Egyes beállítások alábbiakban módosításukhoz szükséges eljárások leírását.</span><span class="sxs-lookup"><span data-stu-id="530ea-105">Each of these settings is described below followed by procedures on how to modify them.</span></span>

## <a name="settings"></a><span data-ttu-id="530ea-106">Beállítások</span><span class="sxs-lookup"><span data-stu-id="530ea-106">Settings</span></span>
### <a name="name-and-description"></a><span data-ttu-id="530ea-107">Név és leírás</span><span class="sxs-lookup"><span data-stu-id="530ea-107">Name and description</span></span>
<span data-ttu-id="530ea-108">A runbook a neve nem módosítható, miután lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="530ea-108">You cannot change the name of a runbook after it has been created.</span></span> <span data-ttu-id="530ea-109">A leírás megadása nem kötelező, és legfeljebb 512 karakterből állhat.</span><span class="sxs-lookup"><span data-stu-id="530ea-109">The Description is optional and can be up to 512 characters.</span></span>

### <a name="tags"></a><span data-ttu-id="530ea-110">Címkék</span><span class="sxs-lookup"><span data-stu-id="530ea-110">Tags</span></span>
<span data-ttu-id="530ea-111">A címkék lehetővé teszik önálló szavak és kifejezések runbook azonosításához hozzárendelni.</span><span class="sxs-lookup"><span data-stu-id="530ea-111">Tags allow you to assign distinct words and phrases to help identify a runbook.</span></span> <span data-ttu-id="530ea-112">Ha például a runbook elküldeni a [PowerShell-galériában](https://www.powershellgallery.com/), megadhatja az adott címkék szerepelnie kell a runbook kategóriák azonosításához.</span><span class="sxs-lookup"><span data-stu-id="530ea-112">For example, when you submit a runbook to the [PowerShell Gallery](https://www.powershellgallery.com/), you specify particular tags to identify which categories the runbook should be listed in.</span></span> <span data-ttu-id="530ea-113">Megadhat egy runbook több címkék vesszővel elválasztva őket.</span><span class="sxs-lookup"><span data-stu-id="530ea-113">You can specify multiple tags for a runbook by separating them with commas.</span></span>

### <a name="logging"></a><span data-ttu-id="530ea-114">Naplózás</span><span class="sxs-lookup"><span data-stu-id="530ea-114">Logging</span></span>
<span data-ttu-id="530ea-115">Alapértelmezés szerint részletes és az állapot rekord nem szerepel a feladatelőzményekben.</span><span class="sxs-lookup"><span data-stu-id="530ea-115">By default, Verbose and Progress records are not written to job history.</span></span> <span data-ttu-id="530ea-116">Módosíthatja az egyes runbookok beállításait, és ezeket a rekordokat naplózzák.</span><span class="sxs-lookup"><span data-stu-id="530ea-116">You can change the settings for a particular runbook to log these records.</span></span> <span data-ttu-id="530ea-117">Rekordokkal kapcsolatos további információkért lásd: [Runbook-kimenet és üzenetek](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="530ea-117">For more information on these records, see [Runbook Output and Messages](automation-runbook-output-and-messages.md).</span></span>

## <a name="changing-runbook-settings"></a><span data-ttu-id="530ea-118">Runbookok beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="530ea-118">Changing runbook settings</span></span>

### <a name="changing-runbook-settings-with-the-azure-portal"></a><span data-ttu-id="530ea-119">Az Azure-portálon runbookok beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="530ea-119">Changing runbook settings with the Azure portal</span></span>
<span data-ttu-id="530ea-120">Az Azure-portálon az egyes runbookok beállításait módosíthatja a **beállítások** panel a runbook.</span><span class="sxs-lookup"><span data-stu-id="530ea-120">You can change settings for a runbook in the Azure portal from the **Settings** blade for the runbook.</span></span>

1. <span data-ttu-id="530ea-121">Válassza ki az Azure-portálon **Automation** és majd kattintson az automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="530ea-121">In the Azure portal, select **Automation** and then then click the name of an automation account.</span></span>
2. <span data-ttu-id="530ea-122">Válassza ki a **Runbookok** fülre.</span><span class="sxs-lookup"><span data-stu-id="530ea-122">Select the **Runbooks** tab.</span></span>
3. <span data-ttu-id="530ea-123">Kattintson a nevére, a runbook, és a program kéri, hogy a beállítások panelről a runbook.</span><span class="sxs-lookup"><span data-stu-id="530ea-123">Click the name of a runbook and you are directed to the settings blade for the runbook.</span></span> <span data-ttu-id="530ea-124">Itt adja meg vagy módosíthatja a címkék, a forgatókönyv leírása, naplózási és nyomkövetési beállítások konfigurálása és problémák megoldásához támogatási eszközök eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="530ea-124">From here you can specify or modify tags, the runbook description, configure logging and tracing settings, and access support tools to help you solve problems.</span></span>     

### <a name="changing-runbook-settings-with-windows-powershell"></a><span data-ttu-id="530ea-125">A Windows PowerShell segítségével a runbookok beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="530ea-125">Changing runbook settings with Windows PowerShell</span></span>
<span data-ttu-id="530ea-126">Használhatja a [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) parancsmag használatával módosíthatja az egyes runbookok beállításait.</span><span class="sxs-lookup"><span data-stu-id="530ea-126">You can use the [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet to change the settings for a runbook.</span></span> <span data-ttu-id="530ea-127">Ha azt szeretné, több címkék adhatók meg, vagy megadhatja egy tömb vagy egy karakterláncot a címkék paraméter vesszővel tagolt értékeket tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="530ea-127">If you want to specify multiple tags, you can either provide an array or a single string with comma delimited values to the Tags parameter.</span></span> <span data-ttu-id="530ea-128">Az aktuális címkék kaphat a [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span><span class="sxs-lookup"><span data-stu-id="530ea-128">You can get the current tags with the [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span></span>

<span data-ttu-id="530ea-129">Az alábbi Példaparancsok szemléltetik egy runbook tulajdonságainak beállításához.</span><span class="sxs-lookup"><span data-stu-id="530ea-129">The following sample commands show how to set the properties for a runbook.</span></span> <span data-ttu-id="530ea-130">Ez a minta három címkék hozzáadása a meglévő címkét, és határozza meg, hogy részletes rekordok legyenek naplózva.</span><span class="sxs-lookup"><span data-stu-id="530ea-130">This sample adds three tags to the existing tags and specifies that verbose records should be logged.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a><span data-ttu-id="530ea-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="530ea-131">Next steps</span></span>
* <span data-ttu-id="530ea-132">Megtudhatja, hogyan hozhat létre és runbook-kimenet és a hiba üzeneteket beolvasni, lásd: [Runbook-kimenet és üzenetek](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="530ea-132">To learn how to create and retrieve output and error messages from runbooks, see [Runbook Output and Messages](automation-runbook-output-and-messages.md)</span></span> 
* <span data-ttu-id="530ea-133">Annak megértése, hogyan hozzáadása egy runbookot, amely már fejlesztette ki a közösségi vagy más forrásból, és a saját runbook létrehozásáról [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="530ea-133">To understand how to add a runbook that was already developed by the community or other source, or to create your own runbook see [Creating or Importing a Runbook](automation-creating-importing-runbook.md)</span></span> 


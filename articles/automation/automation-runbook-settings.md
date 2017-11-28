---
title: "aaaRunbook beállítások |} Microsoft Docs"
description: "Bemutatja az Azure Automationben runbook hello konfigurációs beállításait, és hogyan szolgáltatást is használja azokat az Azure felügyeleti portálon és a Windows PowerShell hello toochange."
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
ms.openlocfilehash: 6f0ef09c148355a351464424c22c33df9300f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="runbook-settings"></a><span data-ttu-id="8225d-103">Runbook beállításai</span><span class="sxs-lookup"><span data-stu-id="8225d-103">Runbook settings</span></span>
<span data-ttu-id="8225d-104">Az Azure Automationben minden runbook rendelkezik, amelyek segítenek azonosítani toobe és toochange több beállításokat annak naplózási viselkedését.</span><span class="sxs-lookup"><span data-stu-id="8225d-104">Each runbook in Azure Automation has multiple settings that help it toobe identified and toochange its logging behavior.</span></span> <span data-ttu-id="8225d-105">Ezek a beállítások leírását alább módosításukhoz szükséges eljárások hogyan toomodify őket.</span><span class="sxs-lookup"><span data-stu-id="8225d-105">Each of these settings is described below followed by procedures on how toomodify them.</span></span>

## <a name="settings"></a><span data-ttu-id="8225d-106">Beállítások</span><span class="sxs-lookup"><span data-stu-id="8225d-106">Settings</span></span>
### <a name="name-and-description"></a><span data-ttu-id="8225d-107">Név és leírás</span><span class="sxs-lookup"><span data-stu-id="8225d-107">Name and description</span></span>
<span data-ttu-id="8225d-108">Egy runbook hello neve nem módosítható, miután lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="8225d-108">You cannot change hello name of a runbook after it has been created.</span></span> <span data-ttu-id="8225d-109">hello leírás megadása nem kötelező, és be too512 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="8225d-109">hello Description is optional and can be up too512 characters.</span></span>

### <a name="tags"></a><span data-ttu-id="8225d-110">Címkék</span><span class="sxs-lookup"><span data-stu-id="8225d-110">Tags</span></span>
<span data-ttu-id="8225d-111">A címkék lehetővé tooassign önálló szavak és kifejezések toohelp azonosítani egy runbookot.</span><span class="sxs-lookup"><span data-stu-id="8225d-111">Tags allow you tooassign distinct words and phrases toohelp identify a runbook.</span></span> <span data-ttu-id="8225d-112">Ha például a runbook-toohello nyújt [PowerShell-galériában](https://www.powershellgallery.com/), megadhatja, hogy adott címkék tooidentify kategóriák hello runbook fel kell sorolni.</span><span class="sxs-lookup"><span data-stu-id="8225d-112">For example, when you submit a runbook toohello [PowerShell Gallery](https://www.powershellgallery.com/), you specify particular tags tooidentify which categories hello runbook should be listed in.</span></span> <span data-ttu-id="8225d-113">Megadhat egy runbook több címkék vesszővel elválasztva őket.</span><span class="sxs-lookup"><span data-stu-id="8225d-113">You can specify multiple tags for a runbook by separating them with commas.</span></span>

### <a name="logging"></a><span data-ttu-id="8225d-114">Naplózás</span><span class="sxs-lookup"><span data-stu-id="8225d-114">Logging</span></span>
<span data-ttu-id="8225d-115">Alapértelmezés szerint részletes és az állapot rekord nem szerepel toojob előzményeit.</span><span class="sxs-lookup"><span data-stu-id="8225d-115">By default, Verbose and Progress records are not written toojob history.</span></span> <span data-ttu-id="8225d-116">Egy adott runbookhoz toolog hello beállításait módosíthatja ezeket a rekordokat.</span><span class="sxs-lookup"><span data-stu-id="8225d-116">You can change hello settings for a particular runbook toolog these records.</span></span> <span data-ttu-id="8225d-117">Rekordokkal kapcsolatos további információkért lásd: [Runbook-kimenet és üzenetek](automation-runbook-output-and-messages.md).</span><span class="sxs-lookup"><span data-stu-id="8225d-117">For more information on these records, see [Runbook Output and Messages](automation-runbook-output-and-messages.md).</span></span>

## <a name="changing-runbook-settings"></a><span data-ttu-id="8225d-118">Runbookok beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="8225d-118">Changing runbook settings</span></span>

### <a name="changing-runbook-settings-with-hello-azure-portal"></a><span data-ttu-id="8225d-119">Azure-portálon hello runbookok beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="8225d-119">Changing runbook settings with hello Azure portal</span></span>
<span data-ttu-id="8225d-120">Hello Azure-portálon az egyes runbookok beállításait hello módosítható **beállítások** hello runbook panelen.</span><span class="sxs-lookup"><span data-stu-id="8225d-120">You can change settings for a runbook in hello Azure portal from hello **Settings** blade for hello runbook.</span></span>

1. <span data-ttu-id="8225d-121">Hello Azure-portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.</span><span class="sxs-lookup"><span data-stu-id="8225d-121">In hello Azure portal, select **Automation** and then then click hello name of an automation account.</span></span>
2. <span data-ttu-id="8225d-122">Jelölje be hello **Runbookok** fülre.</span><span class="sxs-lookup"><span data-stu-id="8225d-122">Select hello **Runbooks** tab.</span></span>
3. <span data-ttu-id="8225d-123">Kattintson a runbook hello nevére és irányított toohello beállítások panel hello runbook.</span><span class="sxs-lookup"><span data-stu-id="8225d-123">Click hello name of a runbook and you are directed toohello settings blade for hello runbook.</span></span> <span data-ttu-id="8225d-124">Itt megadhatja vagy címkék módosítása, hello forgatókönyv leírása, naplózási és nyomkövetési beállítások konfigurálása, és támogatási eszközök toohelp előforduló problémák megoldásához eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="8225d-124">From here you can specify or modify tags, hello runbook description, configure logging and tracing settings, and access support tools toohelp you solve problems.</span></span>     

### <a name="changing-runbook-settings-with-windows-powershell"></a><span data-ttu-id="8225d-125">A Windows PowerShell segítségével a runbookok beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="8225d-125">Changing runbook settings with Windows PowerShell</span></span>
<span data-ttu-id="8225d-126">Használhatja a hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) parancsmag toochange hello egyes runbookok beállításait.</span><span class="sxs-lookup"><span data-stu-id="8225d-126">You can use hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) cmdlet toochange hello settings for a runbook.</span></span> <span data-ttu-id="8225d-127">Ha azt szeretné, toospecify több címke, vagy megadhatja egy tömb vagy egy karakterláncot vesszővel tagolt értékek toohello címkék paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="8225d-127">If you want toospecify multiple tags, you can either provide an array or a single string with comma delimited values toohello Tags parameter.</span></span> <span data-ttu-id="8225d-128">Hello aktuális címkék hello kaphat [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span><span class="sxs-lookup"><span data-stu-id="8225d-128">You can get hello current tags with hello [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).</span></span>

<span data-ttu-id="8225d-129">hello a következő mintaparancsok bemutatják, hogyan tooset hello egy runbook tulajdonságainak.</span><span class="sxs-lookup"><span data-stu-id="8225d-129">hello following sample commands show how tooset hello properties for a runbook.</span></span> <span data-ttu-id="8225d-130">Ez a minta ad három címkék toohello meglévő címkét, és határozza meg, hogy részletes rekordok legyenek naplózva.</span><span class="sxs-lookup"><span data-stu-id="8225d-130">This sample adds three tags toohello existing tags and specifies that verbose records should be logged.</span></span>

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a><span data-ttu-id="8225d-131">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8225d-131">Next steps</span></span>
* <span data-ttu-id="8225d-132">Hogyan toocreate és lekérése kimenet és a hiba a runbookok, kapcsolatban lásd: toolearn [Runbook-kimenet és üzenetek](automation-runbook-output-and-messages.md)</span><span class="sxs-lookup"><span data-stu-id="8225d-132">toolearn how toocreate and retrieve output and error messages from runbooks, see [Runbook Output and Messages](automation-runbook-output-and-messages.md)</span></span> 
* <span data-ttu-id="8225d-133">Hogyan tooadd már fejlesztette ki hello közösségi forrás, illetve más toocreate saját runbook runbook: toounderstand [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md)</span><span class="sxs-lookup"><span data-stu-id="8225d-133">toounderstand how tooadd a runbook that was already developed by hello community or other source, or toocreate your own runbook see [Creating or Importing a Runbook](automation-creating-importing-runbook.md)</span></span> 


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
# <a name="runbook-settings"></a>Runbook beállításai
Az Azure Automationben minden runbook rendelkezik, amelyek segítenek azonosítani toobe és toochange több beállításokat annak naplózási viselkedését. Ezek a beállítások leírását alább módosításukhoz szükséges eljárások hogyan toomodify őket.

## <a name="settings"></a>Beállítások
### <a name="name-and-description"></a>Név és leírás
Egy runbook hello neve nem módosítható, miután lett létrehozva. hello leírás megadása nem kötelező, és be too512 karakter lehet.

### <a name="tags"></a>Címkék
A címkék lehetővé tooassign önálló szavak és kifejezések toohelp azonosítani egy runbookot. Ha például a runbook-toohello nyújt [PowerShell-galériában](https://www.powershellgallery.com/), megadhatja, hogy adott címkék tooidentify kategóriák hello runbook fel kell sorolni. Megadhat egy runbook több címkék vesszővel elválasztva őket.

### <a name="logging"></a>Naplózás
Alapértelmezés szerint részletes és az állapot rekord nem szerepel toojob előzményeit. Egy adott runbookhoz toolog hello beállításait módosíthatja ezeket a rekordokat. Rekordokkal kapcsolatos további információkért lásd: [Runbook-kimenet és üzenetek](automation-runbook-output-and-messages.md).

## <a name="changing-runbook-settings"></a>Runbookok beállításainak módosítása

### <a name="changing-runbook-settings-with-hello-azure-portal"></a>Azure-portálon hello runbookok beállításainak módosítása
Hello Azure-portálon az egyes runbookok beállításait hello módosítható **beállítások** hello runbook panelen.

1. Hello Azure-portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.
2. Jelölje be hello **Runbookok** fülre.
3. Kattintson a runbook hello nevére és irányított toohello beállítások panel hello runbook. Itt megadhatja vagy címkék módosítása, hello forgatókönyv leírása, naplózási és nyomkövetési beállítások konfigurálása, és támogatási eszközök toohelp előforduló problémák megoldásához eléréséhez.     

### <a name="changing-runbook-settings-with-windows-powershell"></a>A Windows PowerShell segítségével a runbookok beállításainak módosítása
Használhatja a hello [Set-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603786.aspx) parancsmag toochange hello egyes runbookok beállításait. Ha azt szeretné, toospecify több címke, vagy megadhatja egy tömb vagy egy karakterláncot vesszővel tagolt értékek toohello címkék paraméterrel. Hello aktuális címkék hello kaphat [Get-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603728.aspx).

hello a következő mintaparancsok bemutatják, hogyan tooset hello egy runbook tulajdonságainak. Ez a minta ad három címkék toohello meglévő címkét, és határozza meg, hogy részletes rekordok legyenek naplózva.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureRmAutomationRunbook -ResourceGroupName "ResourceGroup01" `
    –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## <a name="next-steps"></a>Következő lépések
* Hogyan toocreate és lekérése kimenet és a hiba a runbookok, kapcsolatban lásd: toolearn [Runbook-kimenet és üzenetek](automation-runbook-output-and-messages.md) 
* Hogyan tooadd már fejlesztette ki hello közösségi forrás, illetve más toocreate saját runbook runbook: toounderstand [létrehozása vagy egy Runbook importálása](automation-creating-importing-runbook.md) 


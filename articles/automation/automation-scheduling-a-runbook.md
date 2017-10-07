---
title: "Azure Automation forgatókönyv aaaScheduling |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate egy Azure Automation ütemezési, hogy automatikusan vagy indítható el egy runbook egy adott időpontban egy ismétlődő ütemezés szerint."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
ms.assetid: 710979ff-99d8-41e4-ac6d-6bf26b8ea654
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/09/2016
ms.author: bwren
ROBOTS: NOINDEX, NOFOLLOW
ms.openlocfilehash: c215b7ff6aa200466f3be566facba3c0cffcc924
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Runbook ütemezése az Azure Automationben
egy runbook tooschedule az Azure Automation toostart egy megadott időpontban, csatolás tooone vagy további ütemezéseket. Ütemezés szerint lehet konfigurált tooeither, egyszer futnak le, vagy a feladatról óránként vagy naponta ütemezés runbookokat hello a klasszikus Azure portálon lévő és a runbookokat hello Azure-portálon a, továbbá ütemezheti őket hello hét heti, havi, meghatározott napjain vagy napjain hello hónapban, vagy egy adott hello hónap napja.  Egy runbook csatolt toomultiple ütemezéseket, és ütemezés rendelkezhet több hozzá kapcsolt forgatókönyvből tooit.

## <a name="creating-a-schedule"></a>Ütemezés létrehozása
Hello Azure-portálon a runbookok új ütemtervet hozhat létre a klasszikus portálon hello, vagy a Windows PowerShell használatával. Akkor is hello lehetőséget egy új ütemezést, ha egy runbook tooa ütemezéshez hello klasszikus Azure vagy az Azure-portálon.

> [!NOTE]
> Ütemezés hozzárendelése egy runbookhoz, ha Automation hello modulok aktuális verziói hello tárol a fiókját, és csatolja őket toothat ütemezés.  Ez azt jelenti, hogy ha egy modul 1.0-s verziójával rendelkezett a fiókjában, amikor a feladatütemezés létrehozása, majd frissíti a hello modul tooversion 2.0, hello ütemezés továbbra is toouse 1.0.  A sorrend toouse hello frissített verziója egy új ütemezést kell létrehoznia. 
> 
> 

### <a name="toocreate-a-new-schedule-in-hello-azure-classic-portal"></a>egy új ütemezést, a klasszikus Azure portálon hello toocreate
1. A klasszikus Azure portálon hello válassza ki az Automation, és válassza majd hello automation-fiók nevét.
2. Jelölje be hello **eszközök** fülre.
3. Hello ablak hello alul kattintson **beállítás hozzáadása**.
4. Kattintson a **ütemezés hozzáadása**.
5. Adjon meg egy **neve** és opcionálisan egy **leírás** hello új schedule.your az ütemezés futtatásához **egyszer**, **óránkénti**, **Napi**, **heti**, vagy **havi**.
6. Adjon meg egy **kezdete** és egyéb beállítások hello típusú kiválasztott ütemezéstípustól függően.

### <a name="toocreate-a-new-schedule-in-hello-azure-portal"></a>toocreate egy új ütemezést a hello Azure-portálon
1. Kattintson az automation-fiók, az Azure-portálon hello hello **eszközök** csempe tooopen hello **eszközök** panelen.
2. Kattintson a hello **ütemezések** csempe tooopen hello **ütemezések** panelen.
3. Kattintson a **ütemezés hozzáadása** hello panel hello tetején.
4. A hello **új ütemezés** panelen adjon meg egy **neve** és opcionálisan egy **leírás** hello új ütemezés.
5. Válassza ki az e hello ütemezése egyszer, vagy feladatról ütemezés kiválasztásával **egyszer** vagy **ismétlődési**.  Választásakor **egyszer** adjon meg egy **kezdési időpont** majd **létrehozása**.  Választásakor **ismétlődési**, adjon meg egy **kezdési időpont** és milyen gyakran hello runbook toorepeat - érdemes, az hello gyakoriságát **óra**, **nap**, **hét**, illetve ha **hónap**.  Ha **hét** vagy **hónap** hello legördülő listából hello **ismétlődési beállítást** fog megjelenni hello panelen és kiválasztáskor, hello **ismétlődési a beállítás** panel számára jelenik meg, és hello hét napja, választhat, ha a kiválasztott **hét**.  Ha a kiválasztott **hónap**, szerint is választhat **hét napjai** vagy hello hónap adott napjaira hello naptár, és végül szeretné, hogy toorun azt a hello hónap utolsó napján hello, vagy nem, és kattintson a **OK** .   

### <a name="toocreate-a-new-schedule-with-windows-powershell"></a>a Windows PowerShell használatával új ütemezés toocreate
Használhatja a hello [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) parancsmag toocreate egy új ütemezést, az Azure Automationben klasszikus runbookok vagy [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) parancsmaggal runbookokat hello Azure a portál. Meg kell adnia hello kezdési idejét hello ütemezése és hello gyakorisága fusson.

a következő Példaparancsok hello megjelenítése hogyan toocreate egy új ütemezést, amely fut minden nap du. 3:30 2015. január 20 kezdődően az Azure szolgáltatásfelügyelet parancsmagjával.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

hello következő minta parancsok bemutatja hogyan toocreate hello ütemezésének 15 és az Azure Resource Manager parancsmagjával havonta 30.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-tooa-runbook"></a>Egy ütemezés tooa runbook csatolása
Egy runbook csatolt toomultiple ütemezéseket, és ütemezés rendelkezhet több hozzá kapcsolt forgatókönyvből tooit. Ha a runbook paraméterekkel rendelkezik, majd is értékeket ad meg a számukra. Adjon meg értéket minden kötelező paraméterhez, és előfordulhat, hogy adjon meg értékeket a választható paramétereket.  Ezeket az értékeket minden alkalommal, amikor az ütemezés szerint elindult hello runbook fogja használni.  Csatolhat hello ugyanazon runbook tooanother ütemezést, és adja meg a különböző paraméterértékeket.

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-classic-portal"></a>a klasszikus Azure portálon hello ütemezés tooa runbookokhoz toolink
1. Hello a klasszikus Azure portálon, válassza ki **Automation** és majd kattintson az automation-fiók hello nevére.
2. Jelölje be hello **Runbookok** fülre.
3. Kattintson a hello runbook tooschedule hello nevét.
4. Kattintson a hello **ütemezés** fülre.
5. Ha hello runbook nem jelenleg csatolt tooa ütemezést, akkor az aktiválási hello beállítás túl**tooa új ütemezés hivatkozás** vagy **tooan meglévő ütemezés hivatkozás**.  Ha hello a runbook jelenleg csatolt tooa ütemezést, kattintson a **hivatkozás** : hello alsó részén hello ablak tooaccess ezeket a beállításokat.
6. Ha hello runbook paraméterekkel rendelkezik, felkéri ezek értékeinek megadására.  

### <a name="toolink-a-schedule-tooa-runbook-with-hello-azure-portal"></a>toolink ütemezés tooa runbookkal hello Azure-portálon
1. Kattintson az automation-fiók, az Azure-portálon hello hello **Runbookok** csempe tooopen hello **Runbookok** panelen.
2. Kattintson a hello runbook tooschedule hello nevét.
3. Ha hello runbook nem jelenleg csatolt tooa ütemezést, majd hoz adott hello beállítás toocreate kell egy új ütemezést vagy meglévő ütemezés tooan hivatkozásra.  
4. Ha hello runbook paraméterekkel rendelkezik, kiválaszthatja a hello beállítást **(alapértelmezett: Azure) futtatási beállítások módosítása** és hello **paraméterek** panel oszlik be hello információ ennek megfelelően.  

### <a name="toolink-a-schedule-tooa-runbook-with-windows-powershell"></a>a Windows PowerShell-lel ütemezés tooa runbook toolink
Használhatja a hello [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) toolink ütemezés tooa klasszikus runbook vagy [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) parancsmaggal runbookokat hello Azure-portálon a.  Hello paraméterek paraméterrel megadhatja hello gyermekrunbook paramétereinek értékeit. Lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) a paraméterértékek meghatározásáról a további információt.

hello következő minta parancsok megjelenítése hogyan toolink egy Azure szolgáltatásfelügyeleti parancsmaggal paraméterekkel ütemezés szerint.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

hello következő minta parancsok megjelenítése hogyan toolink az Azure Resource Manager parancsmaggal paraméterekkel ütemezés tooa runbook.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Ütemezés letiltása
Ha letilt egy ütemezést, minden hozzá kapcsolt forgatókönyvből tooit nem fog működni, hogy ütemezés szerint. Manuálisan ütemezésének letiltása, vagy az ütemezések gyakorisággal lejárati idő beállítása a létrehozott. Hello lejárati idő elérésekor hello ütemezés letiltásra kerül.

### <a name="toodisable-a-schedule-from-hello-azure-classic-portal"></a>egy ütemezés, a klasszikus Azure portálon hello toodisable
Letilthatja a klasszikus Azure portálon hello ütemezés részleteit megjelenítő oldalon hello ütemezés hello ütemezés szerint.

1. A klasszikus Azure portálon hello válassza ki az Automation, és kattintson majd hello automation-fiók nevét.
2. Hello eszközök lapon válassza ki.
3. Kattintson egy ütemezés tooopen hello nevét annak információs lapját.
4. Változás **engedélyezve** túl**nem**.

### <a name="toodisable-a-schedule-from-hello-azure-portal"></a>toodisable egy ütemezéshez hello Azure-portálon
1. Kattintson az automation-fiók, az Azure-portálon hello hello **eszközök** csempe tooopen hello **eszközök** panelen.
2. Kattintson a hello **ütemezések** csempe tooopen hello **ütemezések** panelen.
3. Kattintson egy ütemezés tooopen hello részleteit megjelenítő panelen hello nevére.
4. Változás **engedélyezve** túl**nem**.

### <a name="toodisable-a-schedule-with-windows-powershell"></a>a Windows PowerShell-lel ütemezés toodisable
Használhatja a hello [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) parancsmag toochange hello klasszikus runbook meglévő ütemezés tulajdonságainak vagy [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) parancsmaggal runbookokat hello Azure a portál. toodisable hello ütemezése, adja meg **hamis** a hello **IsEnabled** paraméter.

a következő Példaparancsok hello bemutatják, hogyan egy ütemezés használatával toodisable hello Azure szolgáltatásfelügyelet parancsmag.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

hello következő minta parancsok megjelenítése hogyan toodisable annak ütemezését, hogy egy runbook az Azure Resource Manager parancsmagjával.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Következő lépések
* toolearn több ütemezések használatával kapcsolatban lásd: [Azure Automation szolgáltatásbeli ütemezési eszközök](http://msdn.microsoft.com/library/azure/dn940016.aspx)
* Lásd az Azure Automation runbookjai használatába tooget [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) 


---
title: "Az Azure Automationben runbook ütemezése |} Microsoft Docs"
description: "Ismerteti, hogyan ütemezés létrehozása az Azure Automationben, így képes automatikusan elindít egy runbookot egy adott időpontban vagy egy ismétlődő ütemezés szerint."
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
ms.openlocfilehash: 52f1d55f141bb1b3948e3b7039cfc131a5e407b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="scheduling-a-runbook-in-azure-automation"></a>Runbook ütemezése az Azure Automationben
A megadott időben elindítani Azure Automation forgatókönyv ütemezése, csatolható egy vagy több ütemezés. Ütemezés beállítható úgy, hogy a runbookok a klasszikus Azure portálon, és az Azure-portálon a runbookok egyszeri és egy ismétlődés óránkénti futtatási vagy napi ütemezés, továbbá ütemezheti őket heti, havi, meghatározott napokat a hét vagy a mont napjain h, vagy a hónap adott napja.  Egy runbook több ütemezéssel is lehet társítani, és egy ütemezés szerint lehet kapcsolni több runbook.

## <a name="creating-a-schedule"></a>Ütemezés létrehozása
A runbookok új ütemtervet hozhat létre az Azure portálon, a klasszikus portálon, vagy a Windows PowerShell használatával. Új ütemezés létrehozására, ha egy runbook egy ütemezést az Azure klasszikus vagy az Azure portál használatával is rendelkezik.

> [!NOTE]
> Ütemezés hozzárendelése egy runbookhoz, ha Automation tárolja a fiókját a modulok aktuális verziója, és csatolja őket, hogy az ütemezést.  Ez azt jelenti, hogy ha egy modul 1.0-s verzió fiókjában ütemezés létrehozása, és a 2.0-s verzióját frissíti a modul, az ütemezés továbbra is 1.0 használja.  A frissített verziója használatához létre kell hoznia egy új ütemezést. 
> 
> 

### <a name="to-create-a-new-schedule-in-the-azure-classic-portal"></a>Új ütemezés létrehozása a klasszikus Azure portálon
1. A klasszikus Azure portálon válassza ki az Automation, és válassza majd automation-fiók nevét.
2. Válassza ki a **eszközök** fülre.
3. Az ablak alján kattintson **beállítás hozzáadása**.
4. Kattintson a **ütemezés hozzáadása**.
5. Adjon meg egy **neve** és opcionálisan egy **leírás** az új schedule.your az ütemezés futtatásához **egyszer**, **óránkénti**, **napi**, **heti**, vagy **havi**.
6. Adjon meg egy **kezdete** és egyéb beállítások kiválasztott ütemezéstípustól függően.

### <a name="to-create-a-new-schedule-in-the-azure-portal"></a>Új ütemezés létrehozása az Azure-portálon
1. Az Azure portálon, az automation-fiók, kattintson a **eszközök** csempére kattintva nyissa meg a **eszközök** panelen.
2. Kattintson a **ütemezések** csempére kattintva nyissa meg a **ütemezések** panelen.
3. Kattintson a **ütemezés hozzáadása** a panel tetején.
4. Az a **új ütemezés** panelen adjon meg egy **neve** és opcionálisan egy **leírás** az új ütemezés.
5. Válassza ki, hogy az ütemezés futtatásához egy alkalommal vagy feladatról ütemezés kiválasztásával **egyszer** vagy **ismétlődési**.  Választásakor **egyszer** adjon meg egy **kezdési időpont** majd **létrehozása**.  Ha **ismétlődési**, adja meg egy **kezdési időpont** és a gyakoriság, milyen gyakran szeretné a runbook ismételje meg a-az **óra**, **nap**, **hét**, vagy **hónap**.  Ha **hét** vagy **hónap** a legördülő listából a **ismétlődési beállítást** fog megjelenni a panelen és kiválasztáskor, a **ismétlődési beállítást** panel számára jelenik meg, és kiválaszthatja a hét napja, ha a kiválasztott **hét**.  Ha a kiválasztott **hónap**, szerint is választhat **hét napjai** vagy a naptáron a hónap adott napjaira és végezetül szeretné futtatni a hónap utolsó napján, vagy nem, és kattintson a **OK**.   

### <a name="to-create-a-new-schedule-with-windows-powershell"></a>Új ütemezés létrehozása a Windows PowerShell használatával
Használhatja a [New-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) parancsmag új ütemezés létrehozása az Azure Automationben klasszikus runbookok vagy [New-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603577.aspx) parancsmaggal a runbookok az Azure portálon. Meg kell adnia az ütemezés és a gyakoriság fusson kezdési idejét.

Az alábbi Példaparancsok szemléltetik hozzon létre egy új ütemezést, amely a minden nap 3:30 PM 2015. január 20 kezdődően az Azure szolgáltatásfelügyelet parancsmagjával.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName –StartTime "1/20/2016 15:30:00" –DayInterval 1

Az alábbi Példaparancsok bemutatja, hogyan hozzon létre egy ütemezést, a 15. és az Azure Resource Manager parancsmagjával havonta 30.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    New-AzureRMAutomationSchedule –AutomationAccountName $automationAccountName –Name `
    $scheduleName -StartTime "7/01/2016 15:30:00" -MonthInterval 1 `
    -DaysOfMonth Fifteenth,Thirtieth -ResourceGroupName "ResourceGroup01"


## <a name="linking-a-schedule-to-a-runbook"></a>Ütemezés összekapcsolása runbookkal
Egy runbook több ütemezéssel is lehet társítani, és egy ütemezés szerint lehet kapcsolni több runbook. Ha a runbook paraméterekkel rendelkezik, majd is értékeket ad meg a számukra. Adjon meg értéket minden kötelező paraméterhez, és előfordulhat, hogy adjon meg értékeket a választható paramétereket.  Ezeket az értékeket fogja használni minden alkalommal, amikor a runbook az ütemezés szerint elindult.  Ugyanaz a runbook egy másik ütemezés csatolja, és adjon meg másik paraméterértékeket.

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-classic-portal"></a>Az ütemezés összekapcsolása runbookkal a klasszikus Azure portálon
1. A klasszikus Azure portálon, válassza ki a **Automation** és majd kattintson az automation-fiók nevét.
2. Válassza ki a **Runbookok** fülre.
3. Kattintson az ütemezni kívánt runbook nevére.
4. Kattintson a **ütemezés** fülre.
5. Ha a runbook jelenleg nem kapcsolódik egy ütemezéshez, akkor nyílik lehetőség **hivatkozásra egy új ütemezést** vagy **meglévő ütemezés mutató hivatkozás**.  Ha a runbook jelenleg hozzá van kapcsolva egy ütemezést, kattintson a **hivatkozás** ezek a beállítások eléréséhez az ablak alján.
6. Ha a runbook paraméterekkel rendelkezik, a rendszer kéri, ezek értékeinek megadására.  

### <a name="to-link-a-schedule-to-a-runbook-with-the-azure-portal"></a>Az ütemezés összekapcsolása runbookkal a az Azure-portálon
1. Az Azure portálon, az automation-fiók, kattintson a **Runbookok** csempére kattintva nyissa meg a **Runbookok** panelen.
2. Kattintson az ütemezni kívánt runbook nevére.
3. Ha a runbook jelenleg nem kapcsolódik egy ütemezést, majd nyílik létrehozhat egy új ütemezést, vagy meglévő ütemezés mutató hivatkozást.  
4. Ha a runbook paraméterekkel rendelkezik, válassza a beállítás **(alapértelmezett: Azure) futtatási beállítások módosítása** és a **paraméterek** panel oszlik, ahol megadhatja a ennek megfelelően.  

### <a name="to-link-a-schedule-to-a-runbook-with-windows-powershell"></a>Ütemezés összekapcsolása runbookkal a Windows PowerShell
Használhatja a [Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) összekapcsolhat egy ütemezést egy klasszikus runbookhoz vagy [Register-AzureRmAutomationScheduledRunbook](https://msdn.microsoft.com/library/mt603575.aspx) parancsmaggal a runbookok az Azure portálon.  A paraméterek paraméterrel megadhatja a gyermekrunbook paramétereinek értékeit. Lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) a paraméterértékek meghatározásáról a további információt.

A következő mintaparancsok bemutatják, miként kapcsolhat össze egy ütemezést egy Azure szolgáltatásfelügyeleti parancsmaggal paraméterekkel.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params

A következő mintaparancsok bemutatják, hogyan kapcsolhat össze egy ütemezést egy runbookhoz, az Azure Resource Manager parancsmaggal paraméterekkel.

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureRmAutomationScheduledRunbook –AutomationAccountName $automationAccountName `
    –Name $runbookName –ScheduleName $scheduleName –Parameters $params `
    -ResourceGroupName "ResourceGroup01"

## <a name="disabling-a-schedule"></a>Ütemezés letiltása
Ha letilt egy ütemezést, minden hozzá kapcsolt forgatókönyvre nem fog működni, hogy ütemezés szerint. Manuálisan ütemezésének letiltása, vagy az ütemezések gyakorisággal lejárati idő beállítása a létrehozott. A lejárati idő elérésekor a ütemezés letiltásra kerül.

### <a name="to-disable-a-schedule-from-the-azure-classic-portal"></a>A klasszikus Azure portálon ütemezésének letiltása
Az ütemezés részleteit megjelenítő oldalon az ütemezés a klasszikus Azure portálon ütemezés letilthatja.

1. A klasszikus Azure portálon válassza ki az Automation, és kattintson majd automation-fiók nevét.
2. Válassza az eszközök lapot.
3. Kattintson a nevére, nyissa meg annak információs lapját a kívánt ütemezést.
4. Változás **engedélyezett** való **nem**.

### <a name="to-disable-a-schedule-from-the-azure-portal"></a>Azure-portálról ütemezésének letiltása
1. Az Azure portálon, az automation-fiók, kattintson a **eszközök** csempére kattintva nyissa meg a **eszközök** panelen.
2. Kattintson a **ütemezések** csempére kattintva nyissa meg a **ütemezések** panelen.
3. Kattintson a Részletek panel megnyitásához ütemezés nevét.
4. Változás **engedélyezett** való **nem**.

### <a name="to-disable-a-schedule-with-windows-powershell"></a>A Windows PowerShell-lel ütemezésének letiltása
Használhatja a [Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) parancsmag klasszikus runbook meglévő ütemezés tulajdonságainak módosításához vagy [Set-AzureRmAutomationSchedule](https://msdn.microsoft.com/library/mt603566.aspx) parancsmaggal a runbookok az Azure portálon. Az ütemezés letiltásához adja meg a **hamis** a a **IsEnabled** paraméter.

Az alábbi Példaparancsok szemléltetik az Azure szolgáltatásfelügyelet parancsmaggal ütemezésének letiltása.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false

Az alábbi Példaparancsok szemléltetik egy runbook az Azure Resource Manager parancsmagjával ütemezésének letiltása.

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-MonthlyDaysOfMonthSchedule"
    Set-AzureRmAutomationSchedule –AutomationAccountName $automationAccountName `
    –Name $scheduleName –IsEnabled $false -ResourceGroupName "ResourceGroup01"


## <a name="next-steps"></a>Következő lépések
* Ütemezések használatával kapcsolatos további tudnivalókért lásd: [Azure Automation szolgáltatásbeli ütemezési eszközök](http://msdn.microsoft.com/library/azure/dn940016.aspx)
* Ismerkedés az Azure Automation runbookjai, lásd: [Runbook elindítása az Azure Automationben](automation-starting-a-runbook.md) 


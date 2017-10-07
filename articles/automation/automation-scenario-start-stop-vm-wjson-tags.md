---
title: "JSON-formátumú címkék tooschedule Azure VM állapotának aaaUse |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan toouse JSON címkék tooautomate hello ütemezéséről a virtuális gép indítási és leállítási karakterláncokat."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: f6bbf1dea1c193e5d1010f12f3b1ed63562f9daf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-toocreate-a-schedule-for-azure-vm-startup-and-shutdown"></a>Azure Automation-forgatókönyv: egy ütemezést az Azure virtuális gép indítási és leállítási toocreate használatával a JSON-formátumú címkéket
Ügyfelek gyakran szeretné tooschedule hello indítási és virtuális gépek toohelp leállása előfizetés költségek csökkentése vagy üzleti és műszaki követelményeken támogatja.

hello következő forgatókönyv lehetővé teszi fel az automatikus indítási és leállítási a virtuális gépek tooset nevű ütemezés egy erőforráscsoport szintjén vagy Azure virtuális gépek szintjén címke használatával. Ezt az ütemezést is lehet konfigurálni az vasárnap tooSaturday egy indítási idő-és leállítási ideje.

Néhány, a-kész lehetőség van. Ezek a következők:

* [Virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) lehetővé teszik a bejövő vagy kimenő tooscale automatikus skálázási beállításokat.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) szolgáltatást, amely rendelkezik beépített funkcióval hello az indítási és leállítási műveletek ütemezés.

Azonban ezek a beállítások csak támogatja az adott forgatókönyveket, és nem lehet alkalmazott tooinfrastructure,--szolgáltatás (IaaS) virtuális gépeket.

Hello ütemezés címke alkalmazott tooa erőforráscsoportban, esetén is tartalmaz, amelyeket alkalmazott tooall virtuális gépeket. Ha egy ütemezés is közvetlenül alkalmazott tooa VM, hello utolsó ütemezés előnyt élvez sorrend hello:

1. Ütemezés alkalmazott tooa erőforráscsoport
2. Adja meg az alkalmazott tooa erőforráscsoport és a virtuális gép hello erőforráscsoportban található
3. Ütemezés alkalmazása tooa virtuális gép

Ebben a forgatókönyvben alapvetően a megadott formátumban JSON karakterláncnak vesz igénybe, és hozzáadja azt a hello értékeként egy címke nevű ütemezés. Ezután egy runbook felsorolja az összes erőforráscsoport-sablonok és virtuális gépek és hello ütemezések azonosítja az egyes virtuális gépek a korábban felsorolt hello forgatókönyvek alapján. Ezután végighalad a virtuális gépek, amelyeken a csatolt hello, és értékeli ki, mit kell tenni. Például meghatározza, hogy igénylő virtuális gépek toobe leállt, állítsa le vagy figyelmen kívül hagyja.

Ezek a runbookok hitelesítést hello [Azure-beli futtató fiók](automation-sec-configure-azure-runas-account.md).

## <a name="download-hello-runbooks-for-hello-scenario"></a>Töltse le a hello runbookokat hello a forgatókönyvhöz
Ez a forgatókönyv áll tölthető le: hello négy PowerShell munkafolyamat-forgatókönyvekről [TechNet Gallery](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) vagy hello [GitHub](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) tárház ebben a projektben.

| Forgatókönyv | Leírás |
| --- | --- |
| Teszt-ResourceSchedule |Minden virtuális gép ütemezés ellenőrzi, és leállítása vagy indítása hello ütemezés attól függően hajtja végre. |
| Adja hozzá ResourceSchedule |Hello ütemezés címke tooa VM vagy erőforrás csoport hozzáadása. |
| Frissítés-ResourceSchedule |Cserélje le a egy új hello meglévő ütemezés címke módosítja. |
| Remove-ResourceSchedule |Hello ütemezés címke eltávolítása a virtuális gép vagy az erőforrás-csoportot. |

## <a name="install-and-configure-this-scenario"></a>A forgatókönyv telepítése és konfigurálása
### <a name="install-and-publish-hello-runbooks"></a>Telepítse és runbookokat hello közzététele
Runbookokat hello a letöltés után importálhatja azokat a hello eljárással [létrehozása vagy importálása az Azure Automationben runbook](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Minden runbook közzététele, miután sikeresen importálta az Automation-fiók be.

### <a name="add-a-schedule-toohello-test-resourceschedule-runbook"></a>Egy ütemezés toohello teszt-ResourceSchedule runbook hozzáadása
Kövesse a lépéseket tooenable hello ütemezés hello teszt-ResourceSchedule runbook. Ez az hello runbookot, amely ellenőrzi a virtuális gépek el kell indítani, állítsa le, vagy a bal oldalon, mert a.

1. Az Azure-portálon hello, nyissa meg az Automation-fiók, és kattintson a hello **Runbookok** csempére.
2. A hello **teszt-ResourceSchedule** panelen hello kattintson **ütemezések** csempére.
3. A hello **ütemezések** panelen kattintson a **ütemezés hozzáadása**.
4. A hello **ütemezések** panelen válassza **ütemezés tooyour runbook hivatkozás**. Válassza ki **hozzon létre egy új ütemezést**.
5. A hello **új ütemezés** panelen hello nevében típusú ezt az ütemezést, például: *HourlyExecution*.
6. Hello ütemezés **Start**, hello kezdési idő tooan óra növekmény beállítása.
7. Válassza ki **ismétlődési**, és ezután a **megismétlődik minden időköz**, jelölje be **1 óra**.
8. Ellenőrizze, hogy **beállíthatja a lejárati idejét** értéke túl**nem**, és kattintson a **létrehozása** toosave az új ütemezést.
9. A hello **ütemterv forgatókönyv** beállítások panelen válassza **paraméterek és futtatási beállítások**. A Test-ResourceSchedule hello **paraméterek** panelen adja meg az előfizetés hello nevét a hello **SubscriptionName** mező.  Ez az hello egyetlen paraméter, amely runbook hello szükséges.  Amikor végzett, kattintson a **OK**.

runbook-ütemezés hello alábbihoz hasonló hello kész:

![Runbook. teszt-ResourceSchedule konfigurált.](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-hello-json-string"></a>JSON-karakterlánc formátuma hello
Ez a megoldás alapvetően vesz egy JSON megadott formátumú karakterlánc, és hozzáadja azt egy címke hello értékként nevű ütemezés. Egy runbook összes erőforráscsoport-sablonok és virtuális gépek sorolja fel, és azonosítja az egyes virtuális gépek hello ütemezések.

hello runbook fut a hurokban, amelyek csatolt hello virtuális gépeket, és ellenőrzi, milyen műveleteket kell végezni. hello az alábbiakban látható egy példa hogyan hello megoldások formátumban kell lennie:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Ez a struktúra kapcsolatos részletes információkat a következő:

1. Ez a JSON-struktúra hello formátuma nem optimalizált toowork körül hello 256 karakteres korlátozása egy egyetlen címke az Azure-ban.
2. *TzId* jelöli hello hello virtuális gép időzónáját. Ezt az Azonosítót kérhetők le hello TimeZoneInfo .NET osztály használatával egy PowerShell-munkamenet--**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![A PowerShell GetSystemTimeZones](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Hétköznapokon érték nulla toosix jelölik. hello nulla érték vasárnap.
   * hello kezdési ideje képviselt a hello **S** attribútum, értéke pedig egy 24 órás formátumban.
   * hello vége vagy -leállítás idő képviselt a hello **E** attribútum, értéke pedig egy 24 órás formátumban.

     Ha hello **S** és **E** attribútumok minden értéket veheti fel nulla (0), hello virtuális gép marad a jelenlegi állapotában a kiértékelési hello időpontjában.
3. Ha azt szeretné, hogy egy adott napjára hello hét tooskip értékelési, nem szakasz hozzáadása hello hét adott napjaira vonatkozóan. Hello a következő példában csak hétfő kiértékeli, és hello más hello hét napjai figyelmen kívül hagyja:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Címke erőforráscsoport-sablonok és virtuális gépeken
tooshut le a virtuális gépek, kell tootag hello virtuális gépek vagy a hello erőforráscsoportok, amelyben fontosságúak található. A ütemezés címkével nem rendelkező virtuális gépek nem értékeli ki. Nem, ezért azok elindítva, vagy állítsa le.

Nincsenek két módon tootag erőforráscsoportok vagy virtuális gépek ezen megoldás. Azt közvetlenül a hello portálon teheti meg. Vagy hello Add-ResourceSchedule, a frissítés-ResourceSchedule és a Remove-ResourceSchedule runbookok is használhatja.

### <a name="tag-through-hello-portal"></a>Címke hello portálon keresztül
Ezen lépések tootag egy virtuális gép vagy erőforráscsoport hello portálon kövesse:

1. Egybesimítására hello JSON karakterláncát, és győződjön meg arról, hogy nincs-e szóközt.  A JSON karakterláncnak kell kinéznie:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Jelölje be hello **címke** ikonja egy virtuális gép vagy az erőforrás csoport tooapply ezt az ütemezést.

   ![Virtuálisgép-kód beállítását](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Címkék vannak definiálva a következő egy kulcs/érték pár. Típus **ütemezés** a hello **kulcs** mezőben, és JSON-karakterláncban hello majd beillesztheti hello **érték** mező. Kattintson a **Save** (Mentés) gombra. Az új címke ekkor meg kell jelennie az erőforrás címkék hello listáját.

   ![Virtuális gép ütemezés címke](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>PowerShell-címke
Minden importált runbookokat hello parancsfájl, amely leírja, hogyan a tooexecute közvetlenül a PowerShell runbookokat hello hello elején súgó-információkat tartalmaznak. Add-ScheduleResource és frissítés-ScheduleResource runbookokat hello powershellből hívása. Ehhez a virtuális gép vagy az erőforrás-csoporton hello portálon kívül az toocreate vagy frissítés hello ütemezés címkét, amelyek lehetővé teszik a szükséges paraméterek átadása.

toocreate, adja hozzá, és a PowerShell segítségével címkék törlése, először túl[állítsa be a PowerShell környezetet az Azure-](/powershell/azure/overview). Hello telepítő befejezése után az alábbi lépésekkel hello lépne.

### <a name="create-a-schedule-tag-with-powershell"></a>Egy ütemezés címke létrehozása a PowerShell használatával
1. Nyisson meg egy PowerShell-munkamenetet. Kövesse a következő példa tooauthenticate a Futtatás mint fiók és toospecify előfizetés hello:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Adjon meg egy ütemezést kivonatoló táblát. Íme egy példa hogyan kell létrehozni:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Hello runbook által igényelt hello paraméterek megadása. A következő példa hello azt céloz meg egy virtuális Gépet:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Ha egy erőforráscsoport folyamatban címkézést, távolítsa el a hello *VMName* hello $params kivonatoló paramétert tábla az alábbiak szerint:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Futtassa a következő paraméterek toocreate hello ütemezés címke hello hello Add-ResourceSchedule runbook:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. egy erőforrás csoport vagy a virtuális gép címke tooupdate hello hajtható végre **frissítés-ResourceSchedule** runbook a következő paraméterek hello:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>A PowerShell-lel ütemezés címke eltávolítása
1. Nyisson meg egy PowerShell-munkamenetet, és futtassa a következő tooauthenticate a Futtatás mint fiók és tooselect hello, és adjon meg egy előfizetési:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Hello runbook által igényelt hello paraméterek megadása. A következő példa hello azt céloz meg egy virtuális Gépet:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Ha szeretne eltávolítani egy címkét egy erőforráscsoportból, távolítsa el a hello *VMName* hello $params kivonatoló paramétert tábla az alábbiak szerint:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Hello Remove-ResourceSchedule runbook tooremove hello ütemezés címke hajtható végre:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. erőforrás csoport vagy a virtuális gép címkét, tooupdate hello Remove-ResourceSchedule runbook a következő paraméterek hello hajtható végre:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> Azt javasoljuk, hogy proaktív nyomon, hogy ezek a runbookok (és hello virtuális gép állapota) a virtuális gépek éppen tooverify állítsa le, és ennek megfelelően elindult.
>

hello Azure-portálon válassza hello tooview hello részletek hello teszt-ResourceSchedule runbook feladat **feladatok** csempe hello runbook. hello feladat összegzése hello bemeneti paraméterek megjelenítése és hello kimeneti adatfolyam, továbbá hello feladat toogeneral információ és a kivételek Ha sor került ilyenre.

Hello **feladat összegzése** hello kimeneti, figyelmeztető és adatfolyamok üzeneteit tartalmazza. Jelölje be hello **kimeneti** tooview csempe részletes eredményeinek hello a runbook végrehajtása.

![Teszt-ResourceSchedule kimeneti](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Következő lépések
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md).
* További információ az runbooktípusokkal, és azok előnyeit és -korlátozások toolearn lásd [Azure Automation-runbook típusok](automation-runbook-types.md).
* PowerShell parancsfájl támogatási funkciókkal kapcsolatos további információkért lásd: [natív PowerShell-parancsfájl támogatás az Azure Automationben](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* További információ a runbook-naplózás és a kimeneti, toolearn lásd [Runbook-kimenet és üzenetek az Azure Automationben](automation-runbook-output-and-messages.md).
* További információk az Azure-beli futtató fiókot, és hogyan tooauthenticate a runbookok használatával, lásd: toolearn [hitelesítéséhez az Azure-beli futtató fiók runbookok](automation-sec-configure-azure-runas-account.md).

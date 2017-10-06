---
title: "aaaMy első PowerShell-munkafolyamati forgatókönyv az Azure Automationben |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan hello létrehozása, tesztelése és a PowerShell munkafolyamat használatával egyszerű szöveges runbook közzététele."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: "powershell-munkafolyamat, powershell-munkafolyamat példák, munkafolyamat powershell"
ms.assetid: 0002d7f7-e2b5-46e3-b5eb-4596b84fd526
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/26/2017
ms.author: magoedte;bwren
ms.openlocfilehash: b5a34d363ef4865878f3f68119833367b5280f83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-powershell-workflow-runbook"></a>Az első PowerShell-alapú munkafolyamat-forgatókönyvem

> [!div class="op_single_selector"]
> * [Grafikus](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-munkafolyamat](automation-first-runbook-textual.md)
> 
> 

Ez az oktatóanyag bemutatja, hogyan hello létrehozása egy [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks) az Azure Automationben. Először egy egyszerű runbookot, tesztelése és közzététele során elmagyarázza, hogyan tootrack hello hello runbook-feladat állapotának. A Microsoft hello runbook módosítsa tooactually kezelése az Azure-erőforrások, ebben az esetben az Azure virtuális gép elindítása. Végül biztosítjuk hello robusztusabb runbook runbook paraméterek hozzáadásával.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban szüksége lesz a következő hello:

* Egy Azure-előfizetés. Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).
* [Automation-fiók](automation-sec-configure-azure-runas-account.md) toohold hello runbook és tooAzure erőforrások hitelesítéséhez.  Ezt a fiókot kell toostart engedéllyel rendelkezik, és állítsa le a virtuális gép hello.
* Egy Azure virtuális gép. Ezt a gépet leállítjuk és elindítjuk, tehát ne olyan virtuális gépet használjon, amely élesben működik.

## <a name="step-1---create-new-runbook"></a>1. lépés – Új forgatókönyv létrehozása
Hozzon létre egy egyszerű runbookot, amely hello szöveg először foglalkozunk *Hello World*.

1. Hello Azure-portálon nyissa meg az Automation-fiók.  
   hello Automation-fiók lap lehetővé teszi az erőforrások hello áttekintő képet ebben a fiókban. Valószínűleg már rendelkezik adategységekkel. Ezek többsége hello modulok automatikusan új Automation-fiók található. Be kell hello hitelesítőadat-eszköz hello említett [Előfeltételek](#prerequisites).
2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.<br><br> ![Runbookok szabályozása](media/automation-first-runbook-textual/runbooks-control-tiles.png)
3. Hozzon létre egy új runbookot hello kattintva **hozzáadása egy runbook** gombra, majd **hozzon létre egy új runbookot**.
4. Hello runbook hello nevezze *MyFirstRunbook-munkafolyamat*.
5. Ebben az esetben az oktatóanyagban módosítjuk toocreate egy [PowerShell-munkafolyamati forgatókönyv](automation-runbook-types.md#powershell-workflow-runbooks) ezért **Powershell munkafolyamat** a **runbooktípusba**.<br><br> ![Új runbook](media/automation-first-runbook-textual/new-runbook-properties.png)
6. Kattintson a **létrehozása** toocreate hello runbookot és a nyitott hello szöveges szerkesztő.

## <a name="step-2---add-code-toohello-runbook"></a>2. lépés - kód toohello runbook hozzáadása
Közvetlenül a hello runbookot, akkor vagy típuskód vagy parancsmagok, a runbookokat és az eszközök közül választhat hello könyvtár vezérlő, és azokat hozzáadott bármely kapcsolódó paraméterekkel toohello runbook. A forgatókönyv azt követően írja be közvetlenül a hello runbook.

1. A runbook jelenleg üres csak szükség hello *munkafolyamat* kulcsszót, a runbook, és a rendszer encase hello teljes munkafolyamat hello zárójelek közé foglalt hello nevét.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   }
   ```
2. Írja be a következőt: *Write-Output "Hello World."* hello kapcsos zárójelek.

   ```
   Workflow MyFirstRunbook-Workflow
   {
   Write-Output "Hello World"
   }
   ```
3. Hello runbook gombra kattintva mentse **mentése**.<br><br> ![Runbook mentése](media/automation-first-runbook-textual/automation-runbook-edit-controls-save.png)

## <a name="step-3---test-hello-runbook"></a>3. lépés – hello runbook tesztelése
Ahhoz, hogy hello runbook toomake tehessenek közzé az tootest szeretnénk elérhető éles környezetben, azt toomake meg arról, hogy megfelelően működik-e. Egy forgatókönyv tesztelésekor a **Piszkozat** verziót futtatja, és interaktív módon megtekinti a kimenetét.

1. Kattintson a **teszt ablaktábla** tooopen hello teszt ablaktáblán.<br><br> ![Teszt panel](media/automation-first-runbook-textual/automation-runbook-edit-controls-test.png)
2. Kattintson a **Start** toostart hello tesztelése. Csak az engedélyezett hello lehetőség legyen.
3. Létrejön egy [forgatókönyv-feladat](automation-runbook-execution.md), és megjelenik annak állapota.  
   hello feladat állapota elindul *várakozik* azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toocome érhető el a vár. Ezután ez az objektum túl*indítása* dolgozó jogcímek hello feladatot, amikor, majd *futtató* amikor hello runbook ténylegesen elindul.  
4. Hello runbook feladat befejeződik, a kimenet jelenik meg. A mi esetünkben ez a következő: *Hello World*.<br><br> ![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
5. Zárja be a hello teszt ablaktábla tooreturn toohello vászonra.

## <a name="step-4---publish-and-start-hello-runbook"></a>4. lépés: közzététele és hello runbook elindítása
hello runbookot, hogy az imént létrehozott még mindig vázlat módban van. Igazolnia kell, hogy toopublish azt azt futtatása előtt az éles környezetben. Egy runbook közzétételekor hello meglévő közzétett verzió felülírja a hello vázlatként megjelölt verziót. Ebben az esetben nem tudunk egy közzétett verziója még mivel az imént létrehozott hello runbook.

1. Kattintson a **közzététel** toopublish hello runbookot, majd **Igen** megjelenésekor.<br><br> ![Közzététel](media/automation-first-runbook-textual/automation-runbook-edit-controls-publish.png)
2. Ha bal oldali tooview hello runbook hello **Runbookok** panelen, megjelenik egy **szerzői állapot** a **közzétett**.
3. Görgessen hátsó toohello jobb tooview hello ablaktábla **MyFirstRunbook-munkafolyamat**.  
   hello hello tetején beállítások lehetővé teszik a számunkra toostart hello runbookot, az ütemezés toostart valamikor a jövőbeli hello, vagy hozzon létre egy [webhook](automation-webhooks.md) indíthatók el, egy HTTP-hívás keresztül.
4. Toostart hello runbook szeretnénk, kattintson a **Start** , majd **Igen** megjelenésekor.<br><br> ![Runbook indítása](media/automation-first-runbook-textual/automation-runbook-controls-start.png)
5. A feladat panelen, amely az imént létrehozott hello runbook-feladat van megnyitva. Azt is zárja be az ezen az ablaktáblán, de ebben az esetben azt kell nyitva hagyja, hogy figyelemmel követheti a hello feladat előrehaladását.
6. hello feladat állapota látható **feladat összegzése** és egyezések hello imént látott amikor tesztelt hello runbook állapotok.<br><br> ![Feladat összegzése](media/automation-first-runbook-textual/job-pane-status-blade-jobsummary.png)
7. Egyszer a runbook állapota látható hello *befejezve*, kattintson a **kimeneti**. hello Tesztkimenet ablaktáblán már meg van nyitva, és láthatja a *Hello World*.<br><br> ![Feladat összegzése](media/automation-first-runbook-textual/job-pane-status-blade-outputtile.png)  
8. Bezárás hello Tesztkimenet ablaktáblán.
9. Kattintson a **összes napló** tooopen hello adatfolyamok ablaktábla hello runbook-feladat. Csak azt kell látnia *Hello World* hello kimeneti adatfolyam, de ez lehet megjelenítése más adatfolyamok, például a Verbose és a hiba a runbook-feladat Ha hello runbook ír toothem.<br><br> ![Feladat összegzése](media/automation-first-runbook-textual/job-pane-status-blade-alllogstile.png)
10. Zárja be és hello adatfolyamok hello feladat panelen tooreturn toohello MyFirstRunbook ablaktábla.
11. Kattintson a **feladatok** tooopen hello feladatok ablaktábla a runbook. Ez a runbook által létrehozott hello feladatok listája. Csak egy feladat feltüntetve mivel csak miatt hello feladat egyszer kell látható.<br><br> ![Feladatok](media/automation-first-runbook-textual/runbook-control-job-tile.png)
12. Kattintson a feladat tooopen hello azonos, amely azt megtekinteni, amikor a Microsoft hello runbook indítása feladat panelen. Ez lehetővé teszi toogo vissza az idő és a nézet számára egy adott runbookhoz létrehozott összes feladat hello részleteit.

## <a name="step-5---add-authentication-toomanage-azure-resources"></a>5. lépés - hitelesítési toomanage hozzá Azure-erőforrások
Most már teszteltük és közzétettük a forgatókönyvet, de még nem csinál semmi hasznosat. Azt szeretnénk, ha az Azure-erőforrások kezeléséhez toohave. Bár, ha jelenleg nincs hitelesítés tooin hello hello hitelesítő adatok, amelyek használatával említett képes toodo működésbe [Előfeltételek](#prerequisites). Nézzük meg, a hello **Add-AzureRMAccount** parancsmag.

1. Nyissa meg hello szöveges szerkesztő kattintva **szerkesztése** hello MyFirstRunbook-munkafolyamat panelen.<br><br> ![Runbook szerkesztése](media/automation-first-runbook-textual/automation-runbook-controls-edit.png)
2. Nem szükséges hello **Write-Output** többé. sor, úgy lépjen tovább, és törölje azt.
3. Hello kurzor elhelyezése egy üres sor hello kapcsos zárójelek között.
4. Írja be vagy másolja be a következő kódot az Automation futtató fiókhoz hello hitelesítés kezelésére hello:

   ```
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
   -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   ```
5. Kattintson a **teszt ablaktábla** , hogy hello runbook tesztelése.
6. Kattintson a **Start** toostart hello tesztelése. Miután befejeződött, hasonló toohello kimenete a következő, a fiók alapszintű adatainak megjelenítése kell kapnia. Ez megerősíti, hogy hello hitelesítő adat érvénytelen.<br><br> ![Hitelesítés](media/automation-first-runbook-textual/runbook-auth-output.png)

## <a name="step-6---add-code-toostart-a-virtual-machine"></a>6. lépés - kód toostart a virtuális gépek hozzáadása
Most, hogy a runbook tooour Azure-előfizetés hitelesíti magát, hogy kezelhetik az erőforrásokat. A parancs toostart egy virtuális gép jelenleg felvenni. Az Azure-előfizetése kiválaszthatja a virtuális gépi, és most azt is, amelyek neve a hello runbook hardcoding.

1. Után *Add-AzureRmAccount*, típus *Start-AzureRmVM-név (VMName) - ResourceGroupName "NameofResourceGroup"* biztosító hello és hello virtuális gép toostart erőforráscsoport nevét.  

   ```
   workflow MyFirstRunbook-Workflow
   {
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name 'VMName' -ResourceGroupName 'ResourceGroupName'
   }
   ```
2. Hello runbook mentéséhez, és kattintson a **teszt ablaktábla** , hogy azt tesztelése.
3. Kattintson a **Start** toostart hello tesztelése. Miután befejeződött, ellenőrizze, hogy a virtuális gép hello lett elindítva.

## <a name="step-7---add-an-input-parameter-toohello-runbook"></a>7. lépés – egy bemeneti paraméter toohello runbook hozzáadása
A runbook jelenleg kezdődik hello virtuális gépet, hogy a Microsoft hello runbook szoftveresen kötött, de lenne hasznos, ha igazolnia kell megadni hello virtuális gép hello runbook indításakor. A Microsoft most hozzáadjuk bemeneti paraméterek toohello runbook tooprovide funkció.

1. Paraméterek hozzáadása *VMName* és *ResourceGroupName* toohello runbook és ezek a változók használata hello **Start-AzureRmVM** parancsmag az alábbi hello példában látható módon.

   ```
   workflow MyFirstRunbook-Workflow
   {
    Param(
     [string]$VMName,
     [string]$ResourceGroupName
    )  
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
   Start-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
   }
   ```
2. Hello runbook mentéséhez, és hello teszt ablaktábla megnyitása. Vegye figyelembe, hogy most biztosítható értékek hello hello tesztben használt két bemeneti változók.
3. Bezárás hello teszt ablaktáblán.
4. Kattintson a **közzététel** toopublish hello új hello forgatókönyv verzióját.
5. Állítsa le, amelyet az előző lépésben hello hello virtuális gépet.
6. Kattintson a **Start** toostart hello runbook. A típushoz hello **VMName** és **ResourceGroupName** hello virtuális géphez, hogy toostart fog.<br><br> ![Runbook indítása](media/automation-first-runbook-textual/automation-pass-params.png)<br>  
7. Hello runbook befejezését követően ellenőrizze, hogy a virtuális gép hello elindítása.  

## <a name="next-steps"></a>Következő lépések
* Grafikus forgatókönyvek használatába tooget lásd [saját első grafikus forgatókönyv](automation-first-runbook-graphical.md)
* a PowerShell-forgatókönyvek, használatába tooget lásd [saját első PowerShell-forgatókönyv](automation-first-runbook-textual-powershell.md)
* További információ az runbooktípusokkal, azok előnyeit, és a korlátozásokról toolearn lásd [Azure Automation-runbook típusok](automation-runbook-types.md)
* További információk a PowerShell-parancsprogramok támogatásáról: [PowerShell-parancsprogramok natív támogatása az Azure Automationben](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)

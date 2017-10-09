---
title: "Azure Automation grafikus forgatókönyvnek első aaaMy |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan hello létrehozása, tesztelése és egy egyszerű grafikus runbook közzététele."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: runbook, runbook-sablon, runbook automation, azure runbook
ms.assetid: dcb88f19-ed2b-4372-9724-6625cd287c8a
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/17/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 964cf8bae75ca597959bfc39b2b07c22bbc9acb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="my-first-graphical-runbook"></a>Az első grafikus forgatókönyvem

> [!div class="op_single_selector"]
> * [Grafikus](automation-first-runbook-graphical.md)
> * [PowerShell](automation-first-runbook-textual-powershell.md)
> * [PowerShell-munkafolyamat](automation-first-runbook-textual.md)
> 
> 

Ez az oktatóanyag bemutatja, hogyan hello létrehozása egy [grafikus forgatókönyvnek](automation-runbook-types.md#graphical-runbooks) az Azure Automationben.  Először egy egyszerű runbookot, amely teszteli, és teszi közzé, amíg azt ismertetik, hogyan tootrack hello hello runbook-feladat állapotának.  A Microsoft hello runbook módosítsa tooactually kezelése az Azure-erőforrások, ebben az esetben az Azure virtuális gép elindítása.  Majd a Microsoft hello oktatóanyag elvégzéséhez azáltal, hogy sokkal hatékonyabban hello runbook runbook-paraméterek és a feltételes hivatkozások hozzáadásával.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban szüksége lesz a következő hello.

* Egy Azure-előfizetés.  Ha még nem rendelkezik fiókkal, [aktiválhatja MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), illetve <a href="/pricing/free-account/" target="_blank">[regisztrálhat egy ingyenes fiókot](https://azure.microsoft.com/free/).
* [Azure Automation-fiók](automation-sec-configure-azure-runas-account.md) toohold hello runbook és tooAzure erőforrások hitelesítéséhez.  Ezt a fiókot kell toostart engedéllyel rendelkezik, és állítsa le a virtuális gép hello.
* Egy Azure virtuális gép.  Ezt a gépet leállítjuk és elindítjuk, tehát ne legyen éles használatban.

## <a name="step-1---create-runbook"></a>1. lépés – Runbook létrehozása
Először hozzon létre egy egyszerű runbookot, amely hello szöveg *Hello World*.

1. Hello Azure-portálon nyissa meg az Automation-fiók.  
   hello Automation-fiók lap lehetővé teszi az erőforrások hello áttekintő képet ebben a fiókban.  Valószínűleg rendelkezik már adategységekkel.  Ezek többsége hello modulok automatikusan új Automation-fiók található.  Be kell hello hitelesítőadat-eszköz hello említett [Előfeltételek](#prerequisites).
2. Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.<br> ![Runbookok szabályozása](media/automation-first-runbook-graphical/runbooks-resources-tile.png)
3. Hozzon létre egy új runbookot hello kattintva **hozzáadása egy runbook** gombra, majd **hozzon létre egy új runbookot**.
4. Hello runbook hello nevezze *MyFirstRunbook grafikus*.
5. Ebben az esetben az oktatóanyagban módosítjuk toocreate egy [grafikus forgatókönyvnek](automation-graphical-authoring-intro.md) ezért **grafikus** a **runbooktípusba**.<br> ![Új runbook](media/automation-first-runbook-graphical/create-new-runbook.png)<br>
6. Kattintson a **létrehozása** toocreate hello runbookot és a nyitott hello grafikus szerkesztő.

## <a name="step-2---add-activities-toohello-runbook"></a>2. lépés - tevékenységek toohello runbook hozzáadása
hello könyvtár vezérlő hello bal oldalán hello szerkesztő tooselect tevékenységek tooadd tooyour runbook lehetővé teszi.  Fogjuk tooadd egy **Write-Output** parancsmag toooutput szöveg hello runbookból.

1. Hello könyvtár vezérlő, kattintson a hello keresési mezőbe, és írja be **Write-Output**.  hello keresési eredmények alatt jelenik meg. <br> ![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/search-powershell-cmdlet-writeoutput.png)
2. Görgessen lefelé toohello hello lista aljára.  Kattintson jobb gombbal is **Write-Output** válassza **toocanvas hozzáadása** vagy hello ellipszis következő toohello parancsmag kattintson, és válassza **toocanvas hozzáadása**.
3. Kattintson a hello **Write-Output** vásznon a hello tevékenység.  Ekkor megnyílik hello konfigurációs vezérlő panel, amely lehetővé teszi tooconfigure hello tevékenység.
4. Hello **címke** toohello neve hello parancsmag alapértelmezés szerint, de jelenleg ez módosítható toosomething könnyebben értelmezhető. Módosítsa úgy túl*írási Hello World toooutput*.
5. Kattintson a **paraméterek** tooprovide hello parancsmag-paraméterek értékét.  
   Néhány parancsmaggal paraméterkészletek rendelkezik, és melyik meg egy toouse kell tooselect. Ebben az esetben **Write-Output** be van állítva csak egy paraméter, így nem kell egy tooselect. <br> ![A Write-Output tulajdonságai](media/automation-first-runbook-graphical/write-output-properties-b.png)
6. Jelölje be hello **InputObject** paraméter.  Ez az hello paraméter ahol adtuk meg hello szöveg toosend toohello kimeneti adatfolyam.
7. A hello **adatforrás** legördülő menüből válassza **PowerShell-kifejezést**.  Hello **adatforrás** legördülő biztosít a különböző forrásokból, hogy toopopulate a paraméter értékét használja.  
   Használhatja ilyen források (pl. egy másik tevékenység, egy Automation-adategység vagy egy PowerShell-kifejezés) kimenetét.  Ebben az esetben, ellenőrizni szeretnénk toooutput hello szöveg *Hello World*. Megadhatunk egy karakterláncot egy PowerShell-kifejezéssel is.
8. A hello **kifejezés** mezőbe írja be *"Hello World"* majd **OK** kétszer tooreturn toohello vászonra.<br> ![PowerShell-kifejezés](media/automation-first-runbook-graphical/expression-hello-world.png)
9. Hello runbook gombra kattintva mentse **mentése**.<br> ![Runbook mentése](media/automation-first-runbook-graphical/runbook-toolbar-save-revised20165.png)

## <a name="step-3---test-hello-runbook"></a>3. lépés – hello runbook tesztelése
Ahhoz, hogy hello runbook toomake tehessenek közzé az tootest szeretnénk elérhető éles környezetben, azt toomake meg arról, hogy megfelelően működik-e.  Egy forgatókönyv tesztelésekor a **Piszkozat** verziót futtatja, és interaktív módon megtekinti a kimenetét.

1. Kattintson a **teszt ablaktábla** tooopen hello teszt panelen.<br> ![Teszt panel](media/automation-first-runbook-graphical/runbook-toolbar-test-revised20165.png)
2. Kattintson a **Start** toostart hello tesztelése.  Csak az engedélyezett hello lehetőség legyen.
3. A [runbook-feladat](automation-runbook-execution.md) jön létre, és annak állapotát hello ablaktábláján megjelenik.  
   hello feladatállapot indítja el *várakozik* azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.  Majd áthelyezi azt túl*indítása* dolgozó jogcímek hello feladatot, amikor, majd *futtató* amikor hello runbook ténylegesen elindul.  
4. Hello runbook feladat befejeződik, a kimenet jelenik meg. A mi esetünkben ez a következő: *Hello World*.<br> ![Hello World](media/automation-first-runbook-graphical/runbook-test-results.png)
5. Zárja be a hello teszt panel tooreturn toohello vászonra.

## <a name="step-4---publish-and-start-hello-runbook"></a>4. lépés: közzététele és hello runbook elindítása
hello runbook létrehozott még mindig vázlat módban van. Igazolnia kell, hogy toopublish azt azt futtatása előtt az éles környezetben.  Egy runbook közzétételekor hello meglévő közzétett verzió felülírja a hello vázlatként megjelölt verziót.  Ebben az esetben nem tudunk egy közzétett verziója még mivel az imént létrehozott hello runbook.

1. Kattintson a **közzététel** toopublish hello runbookot, majd **Igen** megjelenésekor.<br> ![Közzététel](media/automation-first-runbook-graphical/runbook-toolbar-publish-revised20166.png)
2. Ha bal oldali tooview hello runbook hello **Runbookok** panelen láthatja az **szerzői állapot** a **közzétett**.
3. Görgessen hátsó toohello jobb tooview hello paneljén **MyFirstRunbook**.  
   hello hello tetején beállítások lehetővé teszik a számunkra toostart hello runbookot, az ütemezés toostart valamikor a jövőbeli hello, vagy hozzon létre egy [webhook](automation-webhooks.md) indíthatók el, egy HTTP-hívás keresztül.
4. Toostart hello runbook szeretnénk, kattintson a **Start** , majd **Igen** megjelenésekor.<br> ![Runbook indítása](media/automation-first-runbook-graphical/runbook-controls-start-revised20165.png)
5. Egy feladat panelen létrehozott hello runbook-feladat van megnyitva.  Azt is zárja be a panelt, de ebben az esetben azt nyitva hagyja, hogy figyelemmel követheti a hello feladat előrehaladását.
6. hello feladat állapota látható **feladat összegzése** és egyezések hello imént látott amikor tesztelt hello runbook állapotok.<br> ![Feladat összegzése](media/automation-first-runbook-graphical/runbook-job-summary.png)
7. Egyszer a runbook állapota látható hello *befejezve*, kattintson a **kimeneti**. Hello **kimeneti** panel már meg van nyitva, és láthatja a *Hello World* hello ablaktáblán.<br> ![Feladat összegzése](media/automation-first-runbook-graphical/runbook-job-output.png)  
8. Bezárás hello kimenete panelre.
9. Kattintson a **összes napló** tooopen hello adatfolyamok panel hello runbook-feladat.  Csak azt kell látnia *Hello World* hello kimeneti adatfolyam, de ez lehet megjelenítése más adatfolyamok, például a Verbose és a hiba a runbook-feladat Ha hello runbook ír toothem.<br> ![Feladat összegzése](media/automation-first-runbook-graphical/runbook-job-AllLogs.png)
10. Hello összes napló panel és hello feladat panelen tooreturn toohello MyFirstRunbook panel bezárása.
11. Kattintson a **feladatok** tooopen hello feladatok panelen ehhez a runbookhoz.  A parancs megjeleníti a runbook által létrehozott összes hello feladatok. Csak egy feladat feltüntetve mivel csak miatt hello feladat egyszer kell látható.<br> ![Feladatok](media/automation-first-runbook-graphical/runbook-control-jobs.png)
12. Ez a feladat kattintva tooopen hello azonos feladat panelen, amely azt a Microsoft hello runbook indításakor tekinthetők.  Ez lehetővé teszi toogo vissza az idő és a nézet számára egy adott runbookhoz létrehozott összes feladat hello részleteit.

## <a name="step-5---create-variable-assets"></a>5. lépés: Változó adategységek létrehozása
Most már teszteltük és közzétettük a forgatókönyvet, de még nem csinál semmi hasznosat. Azt szeretnénk, ha az Azure-erőforrások kezeléséhez toohave.  Hello runbook tooauthenticate konfigurálja azt, mielőtt azt hozzon létre egy változó toohold hello előfizetési Azonosítót, és hivatkozni rá után beállítjuk hello tevékenység tooauthenticate az alábbi 6. lépésben.  Többek között egy referencia toohello előfizetési kontextust lehetővé teszi a tooeasily munkahelyi több előfizetések között.  A folytatás előtt másolja az előfizetés-Azonosítóval hello előfizetések lehetőséget ki hello navigációs ablaktáblán.  

1. Hello Automation-fiók paneljén kattintson hello **eszközök** csempe és hello **eszközök** panel már meg van nyitva.
2. Hello Eszközök panelen, kattintson a hello **változók** csempére.
3. Hello változók paneljén kattintson **változó hozzáadása**.<br>![Automation változó](media/automation-first-runbook-graphical/create-new-subscriptionid-variable.png)
4. Hello új változó panelen a hello **neve** adja meg a **AzureSubscriptionId** és hello **érték** mezőben adja meg az előfizetés-azonosító.  Tartsa *karakterlánc* a hello **típus** és hello alapértelmezett értéke **titkosítási**.  
5. Kattintson a **létrehozása** toocreate hello változó.  

## <a name="step-6---add-authentication-toomanage-azure-resources"></a>6. lépés - hitelesítési toomanage hozzá Azure-erőforrások
Most, hogy egy változó toohold az előfizetés-azonosító, azt is konfigurálhatja a runbook tooauthenticate hello futtató hitelesítő adatokat, amelyek az említett tooin hello [Előfeltételek](#prerequisites).  Nézzük meg, hello Azure-beli futtató kapcsolat hozzáadásával **eszköz** és **Add-AzureRMAccount** parancsmag toohello vászonra.  

1. Nyissa meg hello grafikus szerkesztő kattintva **szerkesztése** hello MyFirstRunbook panelen.<br> ![Runbook szerkesztése](media/automation-first-runbook-graphical/runbook-controls-edit-revised20165.png)
2. Nem szükséges hello **írási Hello World toooutput** többé, amelyek a jobb gombbal, és válassza ki **törlése**.
3. Bontsa ki könyvtár vezérlő hello, **kapcsolatok** , és adja hozzá **AzureRunAsConnection** toohello vászonra kiválasztásával **toocanvas hozzáadása**.
4. Válassza ki a vásznon a hello, **AzureRunAsConnection** hello konfigurációs vezérlő ablak, írja be a **beolvasása Futtatás mint kapcsolat** a hello **címke** szövegmező.  Hello kapcsolat
5. Hello könyvtár vezérlő, írja be **Add-AzureRmAccount** hello keresési szövegmezőben.
6. Hozzáadás **Add-AzureRmAccount** toohello vászonra.<br> ![Add-AzureRMAccount](media/automation-first-runbook-graphical/search-powershell-cmdlet-addazurermaccount.png)
7. Vigye **beolvasása Futtatás mint kapcsolat** hello alsó hello alakzat egy kör látható, amíg. Hello kör kattintással és húzással vigye túl hello nyíl**Add-AzureRmAccount**.  hello nyíl létrehozott egy *hivatkozás*.  hello runbook kezdődik-e **beolvasása Futtatás mint kapcsolat** , majd futtassa **Add-AzureRmAccount**.<br> ![Hivatkozás létrehozása a tevékenységek között](media/automation-first-runbook-graphical/runbook-link-auth-activities.png)
8. Válassza ki a vásznon a hello, **Add-AzureRmAccount** és konfigurációs hello szabályozza ablaktábla típus **bejelentkezési tooAzure** a hello **címke** szövegmező.
9. Kattintson a **paraméterek** és hello tevékenység paraméter konfigurációs panel jelenik meg.
10. **Adja hozzá-AzureRmAccount** paraméterkészletek, rendelkezik, ezért ellenőriznünk kell egy tooselect előtt is nyújtunk a paraméterértékek.  Kattintson a **paraméter** , és válassza a hello **ServicePrincipalCertificatewithSubscriptionId** paraméterhalmaz.
11. Miután hello paraméterkészletet alakítanak, hello paraméterek hello tevékenység paraméter konfigurációs panelen jelennek meg.  Kattintson az **APPLICATIONID** elemre.<br> ![Azure RM-fiók paramétereinek hozzáadása](media/automation-first-runbook-graphical/add-azurermaccount-params.png)
12. Hello paraméterérték panelen válassza ki a **tevékenység kimeneti** a hello **adatforrás** válassza ki **beolvasása Futtatás mint kapcsolat** hello listájából, hello **mező elérési út** szövegmező típus **ApplicationId**, és kattintson a **OK**.  Hello mező elérési hello hello tulajdonság neve megadása azt esetén, mert hello tevékenység kimenete több tulajdonságait azonosítójú objektum.
13. Kattintson a **CERTIFICATETHUMBPRINT**, hello paraméterérték panelen válassza ki a **tevékenység kimeneti** a hello **adatforrás**.  Válassza ki **beolvasása Futtatás mint kapcsolat** hello listájából, hello **mező elérési útja** szövegmező típus **CertificateThumbprint**, és kattintson a **OK**.
14. Kattintson a **szolgáltatásnév**, hello paraméterérték panelen válassza ki a **ConstantValue** a hello **adatforrás**, hello választógomb **True**, és kattintson a **OK**.
15. Kattintson a **TENANTID**, hello paraméterérték panelen válassza ki a **tevékenység kimeneti** a hello **adatforrás**.  Válassza ki **beolvasása Futtatás mint kapcsolat** hello listájából, hello **mező elérési útja** szövegmező típus **TenantId**, és kattintson a **OK** kétszer.  
16. Hello könyvtár vezérlő, írja be **Set-AzureRmContext** hello keresési szövegmezőben.
17. Adja hozzá **Set-AzureRmContext** toohello vászonra.
18. Válassza ki a vásznon a hello, **Set-AzureRmContext** és konfigurációs hello szabályozza ablaktábla típus **előfizetés-azonosító megadása** a hello **címke** szövegmező.
19. Kattintson a **paraméterek** és hello tevékenység paraméter konfigurációs panel jelenik meg.
20. **Set-AzureRmContext** paraméterkészletek, rendelkezik, ezért ellenőriznünk kell egy tooselect előtt is nyújtunk a paraméterértékek.  Kattintson a **paraméter** , és válassza a hello **SubscriptionId** paraméterhalmaz.  
21. Miután hello paraméterkészletet alakítanak, hello paraméterek hello tevékenység paraméter konfigurációs panelen jelennek meg.  Kattintson a **SubscriptionID** elemre.
22. Hello paraméterérték panelen válassza ki a **Változóeszköz** a hello **adatforrás** válassza ki **AzureSubscriptionId** hello listán, és kattintson a **OK**  kétszer.   
23. Vigye **bejelentkezési tooAzure** mindaddig, amíg egy kör látható hello alakzat hello aljára. Hello kör kattintással és húzással vigye túl hello nyíl**előfizetés-azonosító megadása**.

A runbook hello ezen a ponton a következő hasonlóan kell kinéznie: <br>![Forgatókönyv-hitelesítés konfigurálása](media/automation-first-runbook-graphical/runbook-auth-config.png)

## <a name="step-7---add-activity-toostart-a-virtual-machine"></a>7. lépés – tevékenység toostart a virtuális gépek hozzáadása
Itt azt hozzá egy **Start-AzureRmVM** tevékenység toostart egy virtuális gépet.  Választhatja ki a bármely virtuális gép az Azure-előfizetése, és akkor megoldás, amely name hello parancsmag be most.

1. Hello könyvtár vezérlő, írja be **Start-AzureRm** hello keresési szövegmezőben.
2. Adja hozzá **Start-AzureRmVM** toohello vászonra kattintson és húzza rá alatt **előfizetés-azonosító megadása**.
3. Vigye **előfizetés-azonosító megadása** mindaddig, amíg egy kör látható hello alakzat hello aljára.  Hello kör kattintással és húzással vigye túl hello nyíl**Start-AzureRmVM**.
4. Jelölje ki a **Start-AzureRmVM** elemet.  Kattintson a **paraméterek** , majd **paraméter** tooview hello beállítása **Start-AzureRmVM**.  Jelölje be hello **ResourceGroupNameParameterSetName** paraméterhalmaz. Figyelje meg, hogy a **ResourceGroupName** és a **Name** mellett felkiáltójel van.  Ez azt jelzi, hogy ezek kötelező paraméterek.  Azt is észreveheti, hogy mindkét helyen szöveges értéket kell megadni.
5. Válassza ki a **Name** paramétert.  Válassza ki **PowerShell-kifejezést** a hello **adatforrás** és hello virtuális gép, amely először a runbook idézőjeleket tartalmazó körülvett hello nevet írja be.  Kattintson az **OK** gombra.<br>![Start-AzureRmVM nevének paraméteres értéke](media/automation-first-runbook-graphical/runbook-startvm-nameparameter.png)
6. Válassza a **ResourceGroupName** elemet. Használjon **PowerShell-kifejezést** a hello **adatforrás** és a dupla idézőjelek között körül hello erőforráscsoport hello nevét írja be.  Kattintson az **OK** gombra.<br> ![Start-AzureRmVM paraméterek](media/automation-first-runbook-graphical/startazurermvm-params.png)
7. Kattintson a vizsgálat panelre, így hello runbook tesztelése.
8. Kattintson a **Start** toostart hello tesztelése.  Miután befejeződött, ellenőrizze, hogy a virtuális gép hello lett elindítva.

A runbook hello ezen a ponton a következő hasonlóan kell kinéznie: <br>![Forgatókönyv-hitelesítés konfigurálása](media/automation-first-runbook-graphical/runbook-startvm.png)

## <a name="step-8---add-additional-input-parameters-toohello-runbook"></a>8. lépés - a további bemeneti paraméterek toohello runbook hozzáadása
A runbook jelenleg kezdődik hello virtuális gép a Microsoft hello megadott hello erőforráscsoportban **Start-AzureRmVM** parancsmag.  A runbook több hasznos, ha igazolnia kell megadni mindkét hello runbook elindításakor lehet.  Jelenleg most felvenni bemeneti paraméterek toohello runbook tooprovide funkció.

1. Nyissa meg hello grafikus szerkesztő kattintva **szerkesztése** a hello **MyFirstRunbook** ablaktáblán.
2. Kattintson a **bemeneti és kimeneti** , majd **bemenet hozzáadása** tooopen hello Runbook bemeneti paraméter ablaktábla.<br> ![Runbook-bemenet és -kimenet](media/automation-first-runbook-graphical/runbook-toolbar-InputandOutput-revised20165.png)
3. Adja meg *VMName* a hello **neve**.  Tartsa *karakterlánc* a hello **típus**, de módosítása **kötelező** túl*Igen*.  Kattintson az **OK** gombra.
4. Hozzon létre egy második kötelező bemeneti paraméter *ResourceGroupName* majd **OK** tooclose hello **bemeneti és kimeneti** ablaktáblán.<br> ![Runbook bemeneti paraméterei](media/automation-first-runbook-graphical/start-azurermvm-params-outputs.png)
5. Jelölje be hello **Start-AzureRmVM** tevékenység, és kattintson **paraméterek**.
6. Változás hello **adatforrás** a **neve** túl**Runbook bemeneti** , és válassza **VMName**.<br>
7. Változás hello **adatforrás** a **ResourceGroupName** túl**Runbook bemeneti** , és válassza **ResourceGroupName**.<br> ![Start-AzureVM paraméterei](media/automation-first-runbook-graphical/start-azurermvm-params-runbookinput.png)
8. Hello runbook mentéséhez, és hello teszt ablaktábla megnyitása.  Vegye figyelembe, hogy most biztosítható értékek hello hello tesztben használt két bemeneti változók.
9. Bezárás hello teszt ablaktáblán.
10. Kattintson a **közzététel** toopublish hello új hello forgatókönyv verzióját.
11. Állítsa le, amelyet az előző lépésben hello hello virtuális gépet.
12. Kattintson a **Start** toostart hello runbook.  A típushoz hello **VMName** és **ResourceGroupName** hello virtuális géphez, hogy toostart fog.<br> ![Runbook indítása](media/automation-first-runbook-graphical/runbook-start-inputparams.png)
13. Hello runbook befejezését követően ellenőrizze, hogy a virtuális gép hello elindítása.

## <a name="step-9---create-a-conditional-link"></a>9. lépés – Feltételes hivatkozás létrehozása
A Microsoft most módosítja az hello runbook így csak kísérel meg toostart hello virtuális gép még nem fut. Ha.  Adja hozzá ehhez a **Get-AzureRmVM** parancsmag toohello runbookot, amely lekérdezi a hello példány szintű hello virtuális gép állapotát. Ezután hozzáadhat egy PowerShell-munkafolyamati kódmodul nevű **állapotának beolvasása** egy PowerShell szövegrészletet a code toodetermine hello virtuális gép állapot esetén fut vagy leállt.  Egy feltételes hivatkozás hello **állapotának beolvasása** modul csak akkor fut, **Start-AzureRmVM** hello aktuális fut. leállítása.  Végül azt egy üzenet tooinform, ha hello virtuális gép sikeresen elindítva, vagy nem használatával lett hello PowerShell Write-Output parancsmag kimenetét.

1. Nyissa meg **MyFirstRunbook** hello grafikus szerkesztőben.
2. Hello kapcsolat eltávolítása között **előfizetés-azonosító megadása** és **Start-AzureRmVM** azt, és nyomja le az hello *törlése* kulcs.
3. Hello könyvtár vezérlő, írja be **Get-AzureRm** hello keresési szövegmezőben.
4. Adja hozzá **Get-AzureRmVM** toohello vászonra.
5. Válassza ki **Get-AzureRmVM** , majd **paraméter** tooview hello beállítása **Get-AzureRmVM**.  Jelölje be hello **GetVirtualMachineInResourceGroupNameParamSet** paraméterhalmaz.  Figyelje meg, hogy a **ResourceGroupName** és a **Name** mellett felkiáltójel van.  Ez azt jelzi, hogy ezek kötelező paraméterek.  Azt is észreveheti, hogy mindkét helyen szöveges értéket kell megadni.
6. A **Name** paraméterhez tartozó **Adatforrás** elemét módosítsa a **Forgatókönyv bemenet** beállításra, majd válassza a **VMName** lehetőséget.  Kattintson az **OK** gombra.
7. A **ResourceGroupName** paraméterhez tartozó **Adatforrás** elemét módosítsa a **Forgatókönyv bemenet** beállításra, majd válassza a **ResourceGroupName** lehetőséget.  Kattintson az **OK** gombra.
8. A **Status** paraméterhez tartozó **Adatforrás** elemét módosítsa a **Konstans érték** beállításra, majd kattintson az **Igaz** gombra.  Kattintson az **OK** gombra.  
9. Hozzon létre kapcsolatot **előfizetés-azonosító megadása** túl**Get-AzureRmVM**.
10. A hello könyvtár vezérlő, bontsa ki a **Runbook-vezérlés** , és adja hozzá **kód** toohello vászonra.  
11. Hozzon létre kapcsolatot **Get-AzureRmVM** túl**kód**.  
12. Kattintson a **kód** és hello konfigurációs ablaktábláján címke módosítása túl**állapotának beolvasása**.
13. Válassza ki **kód** paraméter, és hello **kód szerkesztése** panel jelenik meg.  
14. Hello kód szerkesztése illessze be a következő kódrészletét hello:
    
     ```
     $StatusesJson = $ActivityOutput['Get-AzureRmVM'].StatusesText
     $Statuses = ConvertFrom-Json $StatusesJson
     $StatusOut =""
     foreach ($Status in $Statuses){
     if($Status.Code -eq "Powerstate/running"){$StatusOut = "running"}
     elseif ($Status.Code -eq "Powerstate/deallocated") {$StatusOut = "stopped"}
     }
     $StatusOut
     ```
15. Hozzon létre kapcsolatot **állapotának beolvasása** túl**Start-AzureRmVM**.<br> ![Runbook kódmodullal](media/automation-first-runbook-graphical/runbook-startvm-get-status.png)  
16. Jelöljön ki hello hivatkozás és hello konfigurációs ablaktáblán módosítsa **feltétel alkalmazására** túl**Igen**.   Megjegyzés: hello hivatkozás bekapcsolása tooa szaggatott vonal jelzi, hogy hello céltevékenységben csak akkor fut, ha hello feltétel tootrue oldja fel.  
17. A hello **kifejezésnek a feltétel**, típus *$ActivityOutput ["állapotának beolvasása"] - eq "Leállítva"*.  **Start-AzureRmVM** most csak fut. Ha hello virtuális gép leáll.
18. Bontsa ki könyvtár vezérlő hello, **parancsmagok** , majd **Microsoft.PowerShell.Utility**.
19. Adja hozzá **Write-Output** toohello vászonra kétszer.<br> ![Runbook Write-Output parancsmaggal](media/automation-first-runbook-graphical/runbook-startazurermvm-complete.png)
20. A hello első **Write-Output** szabályozása, kattintson a **paraméterek** , és módosítsa a hello **címke** érték túl*VM indítása értesítés*.
21. A **InputObject**, módosítsa **adatforrás** túl**PowerShell-kifejezést** és hello kifejezés típusának *"$VMName sikeresen elindult."* .
22. A hello második **Write-Output** szabályozása, kattintson a **paraméterek** , és módosítsa a hello **címke** érték túl*értesítés VM indítása nem sikerült*
23. A **InputObject**, módosítsa **adatforrás** túl**PowerShell-kifejezést** és hello kifejezés típusának *"$VMName nem tudott elindulni."* .
24. Hozzon létre kapcsolatot **Start-AzureRmVM** túl**VM indítása értesítés** és **értesítés VM indítása nem sikerült**.
25. Jelölje ki a hello kapcsolatot túl**VM indítása értesítés** , és módosítsa **feltétel alkalmazására** túl**igaz**.
26. A hello **kifejezésnek a feltétel**, típus *$ActivityOutput ["Start-AzureRmVM"]. IsSuccessStatusCode - eq $true*.  A Write-Output ellenőrzéséhez most csak fut, ha hello virtuális gép sikeresen elindult.
27. Jelölje ki a hello kapcsolatot túl**értesítés VM indítása nem sikerült** , és módosítsa **feltétel alkalmazására** túl**igaz**.
28. A hello **kifejezésnek a feltétel**, típus *$ActivityOutput ["Start-AzureRmVM"]. IsSuccessStatusCode - ne $true*.  A Write-Output ellenőrzés most csak fut. Ha hello virtuális gép nem sikeresen elindult.
29. Hello runbook mentéséhez, és hello teszt ablaktábla megnyitása.
30. Indítsa el a hello runbook hello virtuális gép leállt, és addig kell elindítani.

## <a name="next-steps"></a>Következő lépések
* toolearn grafikus szerzői kapcsolatos további információkért lásd: [grafikus készítése az Azure Automationben](automation-graphical-authoring-intro.md)
* a PowerShell-forgatókönyvek, használatába tooget lásd [saját első PowerShell-forgatókönyv](automation-first-runbook-textual-powershell.md)
* a PowerShell munkafolyamat-forgatókönyvekről, használatába tooget lásd: [az első PowerShell-munkafolyamati forgatókönyv](automation-first-runbook-textual.md)


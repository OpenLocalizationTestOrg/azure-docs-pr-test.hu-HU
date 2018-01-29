---
title: "Azure Automation szolgáltatásbeli hibrid forgatókönyv-feldolgozók |} Microsoft Docs"
description: "Ez a cikk bemutatja, telepítéséről és használatáról a hibrid forgatókönyv-feldolgozó, amely egy Azure Automation, amely lehetővé teszi runbookok futtatását a helyi adatközpontban, illetve a felhőbeli szolgáltató gépeken funkciója."
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/21/2017
ms.author: magoedte;bwren
ms.openlocfilehash: 0fcf7485d64d2e947be1730d168ed0a645a50714
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/03/2018
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>A saját adatközpont vagy a felhőbe a hibrid forgatókönyv-feldolgozó erőforrások automatizálásának
Az Azure Automation Runbookjai nem férnek hozzá erőforrásokhoz, a többi felhőből vagy a helyszíni környezetben, mivel azok Azure felhőben lefutott.  A hibrid forgatókönyv-feldolgozó szolgáltatás az Azure Automation lehetővé teszi a runbookok futtatásához, közvetlenül a szerepkört futtató számítógépen és a helyi erőforrások kezelése környezetben erőforrásokon. Runbookok által tárolt és az Azure Automationben kezelt és ezután a felhasználóikhoz kerülnek egy vagy több kijelölt számítógépekre.  

Ez a funkció az alábbi ábrán látható:<br>  

![Hibrid forgatókönyv-feldolgozó – áttekintés](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

A hibrid forgatókönyv-feldolgozó szerepkört és a telepítési szempontok a műszaki áttekintés: [Automation architektúrájának áttekintése](automation-offering-get-started.md#automation-architecture-overview).    

## <a name="hybrid-runbook-worker-groups"></a>Hibrid forgatókönyv-feldolgozó csoportok
Minden egyes hibrid forgatókönyv-feldolgozó az ügynök telepítése során a hibrid forgatókönyv-feldolgozó csoport tagja.  Egy csoport ügynököt lehetnek, de több ügynök is telepíthető egy magas rendelkezésre állási csoportot.

A hibrid forgatókönyv-feldolgozók a runbook indításakor, a csoport az fut, adja meg.  A csoportnak a tagjai határozza meg, melyik worker kiszolgálja a kérést.  Egy adott munkavégző nem adható meg.

## <a name="relationship-to-service-management-automation"></a>A Szolgáltatáskezelési automatizálás kapcsolata
[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) lehetővé teszi az Azure Automation a helyi adatközpontban által támogatott ugyanazon runbookok futtatását. SMA általában a Windows Azure Pack, van telepítve, a Windows Azure Pack tartalmaz egy grafikus felülettel SMA kezelésre. Azure Automation eltérően SMA telepíteni kell egy helyi, amely tartalmazza a webkiszolgálók az API-t, magában foglalja a runbookokat és az SMA konfigurációs adatbázis és a forgatókönyv-feldolgozók runbook-feladatok végrehajtásához. Azure Automation szolgáltatásbeli biztosítja ezeket a szolgáltatásokat, a felhőben, és egyetlen követelménye, hogy a helyi környezetben a hibrid forgatókönyv-feldolgozók karbantartása.

Ha egy meglévő felhasználó SMA, áthelyezheti a runbookok Azure Automation szolgáltatásban, hogy a hibrid forgatókönyv-feldolgozó módosítás nélkül használható, feltéve, hogy azok a saját hitelesítéshez, erőforrások ismertetett [runbookok futtatnak egy hibrid forgatókönyv Munkavégző](automation-hrw-run-runbooks.md).  Az SMA Runbookjai a worker-kiszolgálón, amely előfordulhat, hogy hitelesítésre, hogy a runbookok számára a szolgáltatás fiók kontextusában fut.

Az alábbi feltételek segítségével megállapításához, hogy hibrid forgatókönyv-feldolgozó vagy a Service Management Automation Azure Automation igényeinek jobban megfelelő.

* SMA egy Windows Azure Pack csatlakoznak, ha egy grafikus felügyeleti felületet szükséges összetevők helyi telepítése szükséges. Helyi erőforrások több mint Azure Automation, amelyet csak egy helyi forgatókönyv-feldolgozók telepített ügynök újabb karbantartási költségekkel van szükség. Az Operations Management Suite további csökkentése a karbantartási költségeket az ügynökök felügyeletét.
* Azure Automation a runbookok tárolja a felhőben, és továbbítja őket a helyszíni hibrid forgatókönyv-feldolgozók. Ha a biztonsági házirend nem engedélyezi ezt a viselkedést, akkor SMA kell használnia.
* SMA megtalálható a System Center; és így a System Center 2012 R2 licenc szükséges. Azure Automation szolgáltatásbeli egy rétegzett előfizetés modellen alapul.
* Azure Automation szolgáltatásbeli fejlett szolgáltatások, mint a grafikus forgatókönyvek, amelyek nem érhetők el az SMA.

## <a name="installing-the-windows-hybrid-runbook-worker"></a>A Windows hibrid forgatókönyv-feldolgozó telepítése 

Telepítése és konfigurálása a Windows hibrid forgatókönyv-feldolgozók, kétféleképpen érhető el.  Az ajánlott módszer használ egy Automation-runbook teljesen automatizálja a Windows-számítógép konfigurálásához szükséges.  A másik módszer a következő kézi telepítését és konfigurálását a szerepkör lépésenkénti útmutatót.  

> [!NOTE]
> A kiszolgálók, a hibrid forgatókönyv-feldolgozói szerepkör a kívánt állapot konfigurációs szolgáltatása (DSC) támogató konfigurációjának kezelésére, szüksége DSC-csomópontként hozzáadni.  További információ a bevezetési azokat a-kezeléshez a DSC, lásd: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md).           
><br>
>Ha engedélyezi a [frissítés felügyeleti megoldás](../operations-management-suite/oms-solution-update-management.md), az OMS-munkaterület kapcsolódó minden Windows-számítógép automatikusan a hibrid forgatókönyv-feldolgozót a megoldásban forgatókönyvek támogatására van konfigurálva.  Azonban nincs regisztrálva a hibrid feldolgozó csoportokkal már definiálva van az Automation-fiókban.  A számítógép Automation-forgatókönyv támogatásához, mindaddig, amíg az megoldás és a hibrid forgatókönyv-feldolgozó csoport tagsága is ugyanazt a fiókot használ az Automation-fiók hibrid forgatókönyv-feldolgozó csoporthoz lehet hozzáadni.  Ez a funkció a hibrid runbook-feldolgozó 7.2.12024.0-s verziójától érhető el.  

Tekintse át a következő információkat vonatkozó a [hardver- és szoftverkövetelmények](automation-offering-get-started.md#hybrid-runbook-worker) és [adatait a hálózatot](automation-offering-get-started.md#network-planning) mielőtt elkezdené a hibrid forgatókönyv-feldolgozók telepítését.  Miután sikeresen telepítette a forgatókönyv-feldolgozók, tekintse át a [runbookot futtatni a hibrid forgatókönyv-feldolgozó](automation-hrw-run-runbooks.md) megtudhatja, hogyan konfigurálhatja a runbook automatizálása a helyszíni adatközpontját, illetve más felhőalapú környezetben.  
 
### <a name="automated-deployment"></a>Automatikus telepítés

A következő lépésekkel automatizálhatja a telepítése és konfigurálása a Windows hibrid feldolgozói szerepkör.  

1. Töltse le a *New-OnPremiseHybridWorker.ps1* parancsfájl a [PowerShell-galériában](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) közvetlenül a hibrid forgatókönyv-feldolgozó szerepkört futtató számítógépen vagy egy másik számítógép a környezetben, és másolja a dolgozó.  

    A *New-OnPremiseHybridWorker.ps1* parancsfájl paraméterei a következők végrehajtása során:

  * *AutomationAccountName* (kötelező) – az Automation-fiók nevét.  
  * *ResourceGroupName* (kötelező) – az Automation-fiók társított az erőforráscsoport nevét.  
  * *HybridGroupName* (kötelező) – a runbookok támogató ebben a forgatókönyvben a célként megadott hibrid forgatókönyv-feldolgozó csoport nevét. 
  *  *A SubscriptionID* (kötelező) – az Azure-előfizetési azonosítót, amely az Automation-fiók.
  *  *WorkspaceName* (nem kötelező) – az OMS-munkaterület nevét.  Ha nem rendelkezik az OMS-munkaterület, a parancsfájl létrehozza és konfigurálja az egyik.  

     > [!NOTE]
     > A támogatott OMS-integrációját csak Automation régiók jelenleg - **Ausztrália délkeleti**, **USA keleti régiója 2**, **Délkelet-Ázsia**, és **Nyugat-Európa** .  Ha az Automation-fiók nem egy adott helyre, a parancsfájl létrehoz egy OMS-munkaterület, de figyelmeztetést kap, hogy azt nem összeköt őket.
     > 
2. A számítógépen indítása **Windows PowerShell** a a **Start** képernyő rendszergazdai módban.  
3. Az PowerShell parancssori rendszerhéj lépjen abba a mappába, amely tartalmazza a parancsfájl letöltése, és végrehajtja a paraméterek értékeinek módosítása *- AutomationAccountName*, *- ResourceGroupName*, *- HybridGroupName*, *- SubscriptionId*, és *- WorkspaceName*.

     > [!NOTE] 
     > A parancsfájl végrehajtása után a Azure hitelesítést kéri.  Ön **kell** olyan fiókkal jelentkezzen be, amely az előfizetés-Rendszergazdák szerepkör tagja, és az előfizetés társadminisztrátoraként.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Vállalja, hogy a telepítés megkezdésére **NuGet** és a hitelesítést az Azure hitelesítő adatait kéri.<br><br> ![A New-OnPremiseHybridWorker parancsprogram végrehajtása](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. A parancsfájl befejezése után a hibrid dolgozó csoportok panelen jelennek meg az új csoport és a tagok száma, vagy ha egy meglévő csoportot, a tagok száma növeli.  A csoportot a listából kiválaszthatja a a **hibrid dolgozó csoportok** panelhez, és válassza a **hibrid feldolgozók** csempére.  Az a **hibrid feldolgozók** panelen megjelenik a felsorolt csoport minden tagja.  

### <a name="manual-deployment"></a>Manuális telepítése 
Hajtsa végre az első két lépés egyszer Automation környezetét, majd ismételje meg a fennmaradó munkavégző számítógépenként.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Az Operations Management Suite-munkaterület létrehozása
Ha még nem rendelkezik az Operations Management Suite-munkaterülethez, majd hozzon létre egyet a következő útmutatást [kezelése a munkaterület](../log-analytics/log-analytics-manage-access.md). Ha már rendelkezik egy meglévő munkaterülethez használhatja.

#### <a name="2-add-automation-solution-to-operations-management-suite-workspace"></a>2. Automation-megoldás hozzáadása az Operations Management Suite-munkaterülettel
Megoldások funkciókat adnak hozzá az Operations Management Suite.  Az Automation-megoldás bővíti olyan funkciókkal, az Azure Automation, beleértve a hibrid forgatókönyv-feldolgozó támogatását.  A megoldás a munkaterület hozzáadásakor automatikusan leküldi munkavégző összetevők az ügynök számítógépre, amelyen telepíteni fogja a következő lépéssel.

Kövesse az utasításokat, [hozzáadása a megoldások végzi megoldás](../log-analytics/log-analytics-add-solutions.md) hozzáadása a **Automation** megoldást jelent az Operations Management Suite-munkaterülettel.

#### <a name="3-install-the-microsoft-monitoring-agent"></a>3. A Microsoft Monitoring Agent telepítése
A Microsoft Monitoring Agent a számítógépek Operations Management Suite kapcsolatot.  Telepítse az ügynököt a helyi számítógépen, és csatlakoztassa a munkaterület, automatikusan letölti a hibrid forgatókönyv-feldolgozó szükséges összetevőket.

Kövesse az utasításokat, [Log Analyticshez való csatlakozás Windows számítógépek](../log-analytics/log-analytics-windows-agent.md) az ügynök telepítéséhez a helyi számítógépen.  Megismételheti a folyamatot a több számítógép számára több Worker hozzá a környezethez.

Ha az ügynök sikeresen csatlakozott-e az Operations Management Suite, akkor jelenik meg a **csatlakoztatott források** az Operations Management Suite lapján **beállítások** ablaktáblán.  Ellenőrizheti, hogy az ügynök megfelelően sikeresen letöltötte az Automation-megoldást, ha egy nevű mappát **AzureAutomationFiles** a C:\Program Files\Microsoft figyelési Agent\Agent.  A hibrid forgatókönyv-feldolgozó verziójával megerősítéséhez lépjen C:\Program Files\Microsoft figyelési Agent\Agent\AzureAutomation\, majd jegyezze fel a \\ *verzió* almappájában.   

#### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. Telepítheti a forgatókönyvek környezetét, és csatlakozzon az Azure Automation
Amikor hozzáad egy ügynök Operations Management Suite, az Automation-megoldás leküldi a **HybridRegistration** PowerShell-modult, amely tartalmazza a **Add-HybridRunbookWorker** parancsmag.  Ez a parancsmag segítségével telepítheti a forgatókönyvek környezetét a számítógépen, és regisztrálhatja azt az Azure Automation.

Nyisson meg egy PowerShell-munkamenetet rendszergazdai módban, és a következő parancsok futtatásával importálja a modult.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Ezután futtassa a **Add-HybridRunbookWorker** parancsmag a következő szintaxis használatával:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

Ez a parancsmag a szükséges adatokat kaphat a **kulcsok kezelése** panel az Azure portálon.  Nyissa meg ezt a panelt kiválasztásával a **kulcsok** parancsát a **beállítások** az Automation-fiók panelén.

![Hibrid forgatókönyv-feldolgozó – áttekintés](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **Csoportnév** a hibrid forgatókönyv-feldolgozó csoport neve. Ha ez a csoport már létezik az automation-fiókban, az aktuális számítógépet hozzáadni.  Ha még nem létezik, majd kerül.
* **Végpont** van a **URL-cím** mező mellett a **kulcsok kezelése** panelen.
* **Token** van a **elsődleges elérési kulcsot** a a **kulcsok kezelése** panelen.  

Használja a **-Verbose** kapcsoló **Add-HybridRunbookWorker** telepítésével kapcsolatos részletes adatokat fogadhat.

#### <a name="5-install-powershell-modules"></a>5. PowerShell-modulok telepítése
A Runbookok a tevékenységek és az Azure Automation-környezetbe telepített modulban definiált parancsmagokat használhatja.  Ezek a modulok nem automatikusan telepít a helyszíni számítógépek, ezért telepítenie kell őket manuálisan.  A kivétel: az Azure modult, amely az összes Azure-szolgáltatások és a tevékenységek parancsmagok való hozzáférés biztosításához az Azure Automation alapértelmezés szerint telepítve van.

Mivel az elsődleges célja, hogy a hibrid forgatókönyv-feldolgozó szolgáltatás helyi erőforrásokat kezeljen, valószínűleg telepíteni szeretné a modulokat, amelyek támogatják ezeket az erőforrásokat.  Olvassa el a [modulok telepítése](http://msdn.microsoft.com/library/dd878350.aspx) Windows PowerShell-modul telepítéséről további információt.  Telepített modulok PSModulePath környezeti változó által hivatkozott, hogy a hibrid feldolgozó automatikusan importált helyen kell lennie.  További információkért lásd: [a PSModulePath telepítési elérési út](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx). 

## <a name="removing-hybrid-runbook-worker"></a>Hibrid forgatókönyv-feldolgozó eltávolítása 
Egy vagy több hibrid forgatókönyv-feldolgozók eltávolítása egy csoportból, vagy távolítsa el a csoport a követelményeitől függően.  A helyi számítógépről távolítsa el a hibrid forgatókönyv-feldolgozók, hajtsa végre az alábbi lépéseket.

1. Az Azure-portálon lépjen az Automation-fiók.  
2. Az a **beállítások** panelen válassza **kulcsok** meg és jegyezze fel a mező értékei **URL-cím** és **elsődleges elérési kulcsot**.  Ezt az információt a következő lépésre van szükség.
3. Nyisson meg egy PowerShell-munkamenetet rendszergazdai módban, és futtassa a következő parancsot: `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Használja a **-Verbose** részletes napló, az eltávolítási folyamat váltani.

> [!NOTE]
> Ez nem távolítja el a Microsoft Monitoring Agent a számítógépről, csak a funkciókat és a hibrid forgatókönyv-feldolgozói szerepkör konfigurációja.  

## <a name="remove-hybrid-worker-groups"></a>Hibridfeldolgozó-csoport törlése
Távolítsa el a csoportot, először távolítsa el a hibrid forgatókönyv-feldolgozó minden számítógépen, a korábban bemutatott eljárás segítségével csoport tagja, és ezután hajtsa végre az alábbi lépések végrehajtásával távolítsa el a csoportot.  

1. Nyissa meg az Automation-fiók az Azure portálon.
2. Válassza ki a **hibrid dolgozó csoportok** csempe és a a **hibrid dolgozó csoportok** panelen válassza ki a törölni kívánt csoportot.  A megadott csoport kijelölése után a **hibrid feldolgozócsoport** tulajdonságok panelen jelenik meg.<br> ![Hibrid forgatókönyv-feldolgozó csoport panel](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. A kijelölt csoporthoz tartozó tulajdonságok paneljén kattintson **törlése**.  Válasszon egy üzenet jelenik meg, ez a művelet megerősítését kéri **Igen** Ha biztos benne, hogy a műveletet.<br> ![Csoport törlése megerősítő párbeszédpanele](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> A folyamat eltarthat néhány másodpercig, az előrehaladását nyomon követheti az **Értesítések** menüpont alatt.  

## <a name="troubleshooting"></a>Hibaelhárítás 
A hibrid forgatókönyv-feldolgozó attól függ, hogy a Microsoft figyelési ügynök kommunikáljon az Automation-fiók regisztrálása a munkavégző, runbook-feladatok fogadása és jelentse állapotát. A dolgozó regisztrálása meghiúsul, ha az alábbiakban néhány a hiba lehetséges okai:  

1. A hibrid feldolgozó a proxy vagy az tűzfal mögött van.  
    Ellenőrizze, hogy a számítógép *.azure-automation.net kimenő hozzáféréssel rendelkezzen a 443-as porton.  

2. A hibrid feldolgozó futtató számítógép nem felel meg a minimális hardver [követelmények](automation-offering-get-started.md#hybrid-runbook-worker).  
    A hibrid forgatókönyv-feldolgozó futtató számítógépeken meg kell felelnie a minimális hardverkövetelményeknek, ez a szolgáltatás futtatásához kijelölése előtt. Ellenkező esetben az attól függően, hogy az erőforrás-használat más háttérfolyamatot és végrehajtása során runbookok által okozott versengés, a számítógép túlterhelt fog válik, és runbook-feladat késleltetés vagy időtúllépések okozhat.
   Győződjön meg arról, a hibrid forgatókönyv-feldolgozó szolgáltatás futtatására kijelölt számítógép megfelel a minimális hardverkövetelményeknek.  Ha igen, figyelheti a Processzor- és memóriafelhasználását a hibrid forgatókönyv-feldolgozó folyamat teljesítményét és a Windows között a korrelációs meghatározásához.  Ha memória vagy a CPU-terhelés, jelezheti, hogy a szükséges frissítése vagy további processzorok hozzáadásával, vagy növelje a memória erőforrás szűk cím, és hárítsa el a hibát. Azt is megteheti válassza ki a különböző számítási erőforrása, amely támogathatja a minimális követelményeknek és a skála, ha terheléshez növelését szükség.
    
3. A Microsoft Monitoring Agent szolgáltatás nem fut.  
    Ha a Microsoft figyelési ügynök Windows szolgáltatás nem fut, ez megakadályozza, hogy a hibrid forgatókönyv-feldolgozó Azure Automation kommunikál.  Ellenőrizze az ügynök fut-e a következő parancs beírásával PowerShell: `get-service healthservice`.  Ha a szolgáltatás leáll, adja meg a következő parancsot a PowerShell elindítani a szolgáltatást: `start-service healthservice`.  

4. Az a **alkalmazások és szolgáltatások Logs\Operations kezelője** Eseménynapló, látni esemény 4502 és EventMessage tartalmazó **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**következő leírást: *a szolgáltatás által bemutatott tanúsítványt <wsid>. oms.opinsights.azure.com nem a Microsoft-szolgáltatásokhoz használt hitelesítésszolgáltató állította ki. Lépjen kapcsolatba a hálózati rendszergazdával, ha a proxy, amely elfogja a TLS/SSL-kommunikáció futnak. A kb3126513 jelű további információkat talál a csatlakozási problémák.*
    Ezt okozhatja a proxy vagy a hálózati tűzfal blokkolja a Microsoft Azure-kommunikációt.  Ellenőrizze, hogy a számítógép *.azure-automation.net kimenő hozzáféréssel rendelkezzen a 443-as porton.

Naplók minden hibridfeldolgozó C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, helyileg tárolja.  Ellenőrizheti, hogy vannak-e bármilyen figyelmeztetés vagy írni hibaesemények a **alkalmazások és szolgáltatások Logs\Microsoft-SMA\Operations** és **alkalmazások és szolgáltatások Logs\Operations kezelője** esemény naplózása, amelyek azt jelzi, a kapcsolattal vagy más probléma érdekében, hogy a szerepkört az Azure Automation vagy probléma befolyásolja a normál műveletek során.  

## <a name="next-steps"></a>További lépések
Felülvizsgálati [runbookot futtatni a hibrid forgatókönyv-feldolgozó](automation-hrw-run-runbooks.md) megtudhatja, hogyan konfigurálhatja a runbook automatizálása a helyszíni adatközpontját, illetve más felhőalapú környezetben.

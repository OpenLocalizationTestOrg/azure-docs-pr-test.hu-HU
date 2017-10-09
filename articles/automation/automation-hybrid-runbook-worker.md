---
title: "Automatizálási hibrid forgatókönyv-feldolgozók aaaAzure |} Microsoft Docs"
description: "Ez a cikk bemutatja, telepítéséről és használatáról a hibrid forgatókönyv-feldolgozó, amely egy Azure Automation, amely lehetővé teszi a runbookok toorun gépeken a helyi adatközpontban, illetve a felhőbeli szolgáltató funkciója."
services: automation
documentationcenter: 
author: mgoedtel
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
ms.openlocfilehash: ccee35e8324149a79ff692a867e5ce7801299bcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automate-resources-in-your-data-center-or-cloud-with-hybrid-runbook-worker"></a>A saját adatközpont vagy a felhőbe a hibrid forgatókönyv-feldolgozó erőforrások automatizálásának
Az Azure Automation Runbookjai nem lehet hozzáférni erőforrásokat a többi felhőből vagy a helyszíni környezetben, mert hello Azure felhőben futtatja.  hello hibrid forgatókönyv-feldolgozó az Azure Automation funkcióval toorun runbookok közvetlenül hello szerepkört üzemeltető hello számítógépen és a hello környezet toomanage erőforrásokon helyi erőforrásokhoz. Runbookok vannak tárolva és felügyelete az Azure Automation és kézbesítését tooone vagy több kijelölt számítógépek.  

Ez a funkció mutatja be a következő kép hello:<br>  

![Hibrid forgatókönyv-feldolgozó – áttekintés](media/automation-offering-get-started/automation-infradiagram-networkcomms.png)

A műszaki áttekintés hello hibrid forgatókönyv-feldolgozó szerepkört és a központi telepítési szempontok: [Automation architektúrájának áttekintése](automation-offering-get-started.md#automation-architecture-overview).    

## <a name="hybrid-runbook-worker-groups"></a>Hibrid forgatókönyv-feldolgozó csoportok
Minden egyes hibrid forgatókönyv-feldolgozó hello ügynök telepítésekor megadott hibrid forgatókönyv-feldolgozó csoport tagja.  Egy csoport ügynököt lehetnek, de több ügynök is telepíthető egy magas rendelkezésre állási csoportot.

A hibrid forgatókönyv-feldolgozók a runbook indításakor, meg kell adnia azt futtató hello csoport.  hello csoport tagjai hello határozza meg, melyik worker szolgáltatások hello kérelem.  Egy adott munkavégző nem adható meg.

## <a name="relationship-tooservice-management-automation"></a>Kapcsolat tooService Szolgáltatáskezelés-automatizálás
[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) lehetővé teszi a toorun hello ugyanazon runbookok, amelyek támogatottak az Azure Automation a helyi adatközpontban. SMA általában a Windows Azure Pack, van telepítve, a Windows Azure Pack tartalmaz egy grafikus felülettel SMA kezelésre. Azure Automation eltérően SMA telepíteni kell egy helyi, amely tartalmazza a webes kiszolgálók toohost hello API, egy adatbázis toocontain runbookok és SMA konfigurációs és a forgatókönyv-feldolgozók tooexecute runbook-feladatok. Azure Automation szolgáltatásbeli hello felhőben ezeket a szolgáltatásokat biztosít, és csak a helyi környezetben a hibrid forgatókönyv-feldolgozók toomaintain hello használatához.

Ha egy meglévő felhasználó SMA, áthelyezheti a runbookok tooAzure Automation toobe használt hibrid forgatókönyv-feldolgozó változtatás nélkül, feltéve, hogy a saját hitelesítési tooresources működnek [runbookok futtatnak egy hibrid forgatókönyv Munkavégző](automation-hrw-run-runbooks.md).  Az SMA Runbookjai hello szolgáltatás fiókjának hello worker-kiszolgálón, amely előfordulhat, hogy hitelesítésre, hogy a runbookokat hello hello környezetében futnak.

Azure Automation a hibrid forgatókönyv-feldolgozó vagy a Service Management Automation igényeinek jobban megfelelő-e a következő feltételek toodetermine hello is használhatja.

* SMA összetevők, amelyek csatlakoztatott tooWindows Azure Pack, ha egy grafikus felügyeleti felületet szükség a helyi telepítése szükséges. Helyi erőforrások több mint Azure Automation, amelyet csak egy helyi forgatókönyv-feldolgozók telepített ügynök újabb karbantartási költségekkel van szükség. az Operations Management Suite további csökkentése a karbantartási költségek hello ügynökök felügyeletét.
* Azure Automation szolgáltatásbeli tárolja a runbookokat hello felhőben, és továbbítja azokat tooon helyszíni hibrid forgatókönyv-feldolgozók. Ha a biztonsági házirend nem engedélyezi ezt a viselkedést, akkor SMA kell használnia.
* SMA megtalálható a System Center; és így a System Center 2012 R2 licenc szükséges. Azure Automation szolgáltatásbeli egy rétegzett előfizetés modellen alapul.
* Azure Automation szolgáltatásbeli fejlett szolgáltatások, mint a grafikus forgatókönyvek, amelyek nem érhetők el az SMA.

## <a name="installing-hello-windows-hybrid-runbook-worker"></a>Hibrid forgatókönyv-feldolgozót a Windows hello telepítése 

tooinstall és konfigurálása a Windows hibrid forgatókönyv-feldolgozók, kétféleképpen érhető el.  hello ajánlott módszert használ az Automation-runbook toocompletely szükséges hello folyamat automatizálásához tooconfigure a Windows rendszerű számítógépeken.  hello második módszer van a következő a lépésről lépésre toomanually telepítése, és konfiguráljon hello szerepkört.  

> [!NOTE]
> a kiszolgálók hello hibrid forgatókönyv-feldolgozói szerepkör a kívánt állapot konfigurációs szolgáltatása (DSC) támogató toomanage hello konfigurációját, kell tooadd DSC-csomópontként őket.  További információ a bevezetési azokat a-kezeléshez a DSC, lásd: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md).           
><br>
>Ha engedélyezi a hello [frissítés felügyeleti megoldás](../operations-management-suite/oms-solution-update-management.md), minden Windows számítógép csatlakoztatva OMS-munkaterület automatikusan konfigurálja a megoldásban a hibrid forgatókönyv-feldolgozó toosupport runbookok tooyour.  Azonban nincs regisztrálva a hibrid feldolgozó csoportokkal már definiálva van az Automation-fiókban.  hello számítógép lehet hozzáadni tooa hibrid forgatókönyv-feldolgozó csoport az Automation-fiók toosupport Automation-runbook mindaddig, amíg hello hello megoldás és a hibrid forgatókönyv-feldolgozó csoport tagsága is fiókot használja.  Ez a funkció a hibrid forgatókönyv-feldolgozó hello 7.2.12024.0 tooversion bővült.  

Felülvizsgálati hello hello vonatkozó információkat a következő [hardver- és szoftverkövetelmények](automation-offering-get-started.md#hybrid-runbook-worker) és [adatait a hálózatot](automation-offering-get-started.md#network-planning) mielőtt elkezdené a hibrid forgatókönyv-feldolgozók telepítését.  Miután sikeresen telepítette a forgatókönyv-feldolgozók, tekintse át a [runbookot futtatni a hibrid forgatókönyv-feldolgozó](automation-hrw-run-runbooks.md) toolearn hogyan tooconfigure a runbookok tooautomate dolgozza fel a helyszíni adatközpontját vagy egyéb felhőalapú környezetben.  
 
### <a name="automated-deployment"></a>Automatikus telepítés

Hajtsa végre a következő hello tooautomate hello telepítésének és konfigurálásának hello Windows hibrid feldolgozói szerepkör lépések.  

1. Töltse le a hello *New-OnPremiseHybridWorker.ps1* hello parancsfájlt [PowerShell-galériában](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker/1.0/DisplayScript) közvetlenül a hello futtató hello hibrid forgatókönyv-feldolgozói szerepkör vagy egy másik számítógépre a környezet és toohello munkavégző másolásához.  

    Hello *New-OnPremiseHybridWorker.ps1* parancsfájlhoz szükséges paraméterek végrehajtása közben a következő hello:

  * *AutomationAccountName* (kötelező) – az Automation-fiók hello nevét.  
  * *ResourceGroupName* (kötelező) – az Automation-fiók társított hello erőforráscsoport hello nevét.  
  * *HybridGroupName* (kötelező) – Ez a forgatókönyv támogatása hello runbookok célként megadott hibrid forgatókönyv-feldolgozó csoport hello nevét. 
  *  *A SubscriptionID* (kötelező) – hello Azure előfizetés-azonosító, amely az Automation-fiók.
  *  *WorkspaceName* (nem kötelező) – hello OMS munkaterület nevét.  Ha nem rendelkezik az OMS-munkaterület, hello parancsfájl hoz létre, és konfigurálja az egyik.  

     > [!NOTE]
     > Jelenleg hello támogatott OMS-integrációját csak Automation régiók a következők - **Ausztrália délkeleti**, **USA keleti régiója 2**, **Délkelet-Ázsia**, és **nyugati régiója Európa**.  Ha az Automation-fiók nem egy adott helyre, hello parancsfájl létrehozza az OMS-munkaterület, de figyelmeztetést kap, hogy azt nem összeköt őket.
     > 
2. A számítógépen indítsa el a **Windows PowerShell** a hello **Start** képernyő rendszergazdai módban.  
3. A hello PowerShell parancssori felület, keresse meg a toohello mappát, amelybe a letöltött és módosítása a paraméterek értékeit hello végre hello parancsfájl *- AutomationAccountName*, *- ResourceGroupName* , *- HybridGroupName*, *- SubscriptionId*, és *- WorkspaceName*.

     > [!NOTE] 
     > Az Azure-ral felszólító tooauthenticate hello parancsfájl végrehajtása után áll.  Ön **kell** olyan fiókkal jelentkezzen be, amely hello előfizetés-Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként.  
     >  
    
        .\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> `
        -ResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
        -SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfOMSWorkspace>

4. Biztosan rákérdezéses tooagree tooinstall **NuGet** és rákérdezéses tooauthenticate a Azure hitelesítő adataival.<br><br> ![A New-OnPremiseHybridWorker parancsprogram végrehajtása](media/automation-hybrid-runbook-worker/new-onpremisehybridworker-scriptoutput.png)

5. Hello parancsfájl befejezése után hello hibrid dolgozó csoportok panelen megjelenik a hello új csoport és a tagok száma, vagy ha egy meglévő csoportot, hello található tagok számát növeli.  Kiválaszthat hello csoportot hello hello listából **hibrid dolgozó csoportok** panel megnyitásához, és jelölje be hello **hibrid feldolgozók** csempére.  A hello **hibrid feldolgozók** panelen, megjelenik minden felsorolt hello csoport tagja.  

### <a name="manual-deployment"></a>Manuális telepítése 
Egyszer hajtsa végre az hello két lépést az Automation-környezet, majd ismételje meg a hátralévő lépéseket munkavégző számítógépenként hello.

#### <a name="1-create-operations-management-suite-workspace"></a>1. Az Operations Management Suite-munkaterület létrehozása
Ha még nem rendelkezik az Operations Management Suite-munkaterülethez, majd hozzon létre egyet a következő útmutatást [kezelése a munkaterület](../log-analytics/log-analytics-manage-access.md). Ha már rendelkezik egy meglévő munkaterülethez használhatja.

#### <a name="2-add-automation-solution-toooperations-management-suite-workspace"></a>2. Adja hozzá az Automation-megoldás tooOperations Management Suite-munkaterülettel
Megoldás hozzáadása funkció tooOperations felügyeleti csomag.  Automation-megoldás hello bővíti olyan funkciókkal, az Azure Automation, beleértve a hibrid forgatókönyv-feldolgozó támogatását.  Amikor hello megoldás tooyour munkaterületen, az automatikusan leküldi munkavégző összetevők toohello ügynök számítógép, amely a következő lépésben hello telepíteni fogja.

Hajtsa végre a hello található utasítások segítségével: [tooadd hello megoldások végzi megoldás](../log-analytics/log-analytics-add-solutions.md) tooadd hello **Automation** megoldás tooyour Operations Management Suite-munkaterülettel.

#### <a name="3-install-hello-microsoft-monitoring-agent"></a>3. Hello Microsoft Monitoring Agent telepítése
a Microsoft Monitoring Agent hello számítógépek tooOperations Management Suite csatlakozik.  Amikor hello ügynök telepítése a helyi számítógépen, és csatlakozni tud tooyour munkaterület automatikusan letölti hibrid forgatókönyv-feldolgozó szükséges hello összetevők.

Hajtsa végre a hello található utasítások segítségével: [csatlakozás Windows számítógépek tooLog Analytics](../log-analytics/log-analytics-windows-agents.md) tooinstall hello ügynök hello a helyi számítógépen.  Az eljárás megismétlésével a számítógépek több tooadd több munkavállalók tooyour környezetben.

Hello ügynök sikeresen csatlakozott tooOperations felügyeleti csomagban, amikor az jelenik meg hello **csatlakoztatott források** hello Operations Management Suite lapján **beállítások** ablaktáblán.  Ellenőrizheti, hogy hello ügynök megfelelően töltött hello Folyamatautomatizálási megoldása során van egy nevű mappát **AzureAutomationFiles** a C:\Program Files\Microsoft figyelési Agent\Agent.  a hibrid forgatókönyv-feldolgozó hello tooconfirm hello verzióban tooC:\Program Files\Microsoft Agent\Agent\AzureAutomation\ figyelése és megjegyzés hello navigálhat \\ *verzió* almappájában.   

#### <a name="4-install-hello-runbook-environment-and-connect-tooazure-automation"></a>4. Telepítheti hello forgatókönyvek környezetét, és csatlakozzon az Automation tooAzure
Ha egy ügynök tooOperations felügyeleti csomag hozzáadásához hello Automation-megoldás leküldi hello **HybridRegistration** PowerShell-modult, amely tartalmazza a hello **Add-HybridRunbookWorker** parancsmag.  Ez a parancsmag tooinstall hello forgatókönyvek környezetét hello számítógépen használja, és regisztrálhatja azt az Azure Automation.

Nyisson meg egy PowerShell-munkamenetet rendszergazdai módban, és futtassa a következő parancsok tooimport hello modul hello.

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

Futtassa a hello **Add-HybridRunbookWorker** parancsmag szintaxisa a következő hello használata:

    Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>

Ez a parancsmag a hello szükséges hello adatokat kaphat **kulcsok kezelése** panel az Azure-portálon hello.  Nyissa meg ezt a panelt hello kiválasztásával **kulcsok** hello kapcsolót **beállítások** az Automation-fiók panelén.

![Hibrid forgatókönyv-feldolgozó – áttekintés](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* **Csoportnév** hello hello hibrid forgatókönyv-feldolgozó csoport neve. Ha ez a csoport már létezik a hello automation-fiókot, majd hello aktuális a számítógép nincs felvéve tooit.  Ha még nem létezik, majd kerül.
* **Végpont** hello van **URL-cím** hello mezőjét **kulcsok kezelése** panelen.
* **Token** hello van **elsődleges elérési kulcsot** a hello **kulcsok kezelése** panelen.  

Használjon hello **-Verbose** kapcsoló **Add-HybridRunbookWorker** tooreceive részletes hello telepítési információit.

#### <a name="5-install-powershell-modules"></a>5. PowerShell-modulok telepítése
Runbookokat hello tevékenységek és az Azure Automation-környezetbe telepített hello modulban definiált parancsmagok bármelyikét használhatja.  Ezek a modulok nincsenek automatikusan telepített tooon helyszíni számítógépek, ezért telepítenie kell őket manuálisan.  hello kivétel: hello Azure modul, amely hozzáférési toocmdlets biztosító Azure-szolgáltatások és a tevékenységek az Azure Automation alapértelmezés szerint telepítve van.

Hello elsődleges hello hibrid forgatókönyv-feldolgozó szolgáltatás célja toomanage helyi erőforrások, mert valószínűleg szüksége tooinstall hello modulokat, amelyek támogatják ezeket az erőforrásokat.  Olvassa el a túl[modulok telepítése](http://msdn.microsoft.com/library/dd878350.aspx) Windows PowerShell-modul telepítéséről további információt.  Telepített modulok PSModulePath környezeti változó által hivatkozott, így automatikusan hozzák hello hibridfeldolgozó helyen kell lennie.  További információkért lásd: [módosítása hello PSModulePath telepítési útvonal](https://msdn.microsoft.com/library/dd878326%28v=vs.85%29.aspx). 

## <a name="removing-hybrid-runbook-worker"></a>Hibrid forgatókönyv-feldolgozó eltávolítása 
Egy vagy több hibrid forgatókönyv-feldolgozók eltávolítása egy csoportból, vagy a követelményeitől függően hello csoportnak, távolítsa el.  tooremove a hibrid forgatókönyv-feldolgozót a helyi számítógépről, hajtsa végre a lépéseket követve hello.

1. Hello Azure-portálon lépjen a tooyour Automation-fiók.  
2. A hello **beállítások** panelen válassza **kulcsok** meg és jegyezze fel a hello értékek mező **URL-cím** és **elsődleges elérési kulcsot**.  Ez az információ hello következő lépésre van szükség.
3. Nyisson meg egy PowerShell-munkamenetet rendszergazdai módban, és futtassa a következő hello parancs - `Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey>`.  Használjon hello **-Verbose** hello eltávolítása során a részletes napló váltani.

> [!NOTE]
> Ez nem távolítja el a Microsoft Monitoring Agent hello hello számítógépről, csak a hello funkcionalitásra vagy a hello hibrid forgatókönyv-feldolgozói szerepkör konfigurációja.  

## <a name="remove-hybrid-worker-groups"></a>Hibridfeldolgozó-csoport törlése
tooremove egy csoportot, először tooremove hello hibrid forgatókönyv-feldolgozó minden számítógépről, amely a korábban bemutatott hello eljárással hello csoport tagja, és a következő lépéseket tooremove hello csoport hello végezze.  

1. Nyissa meg a hello Automation-fiók hello Azure-portálon.
2. Jelölje be hello **hibrid dolgozó csoportok** csempe és hello **hibrid dolgozó csoportok** panelen, jelölje be hello csoport toodelete kívánja.  Miután kijelölte hello adott csoportból, hello **hibrid feldolgozócsoport** tulajdonságok panelen jelenik meg.<br> ![Hibrid forgatókönyv-feldolgozó csoport panel](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)   
3. A kijelölt csoport hello hello tulajdonságok paneljén kattintson **törlése**.  Egy üzenet jelenik meg azzal a kérdéssel, tooconfirm, ez a művelet, jelölje be **Igen** Ha biztos benne, hogy azt szeretné, hogy tooproceed.<br> ![Csoport törlése megerősítő párbeszédpanele](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)<br> A folyamat eltarthat néhány másodpercig toocomplete, és nyomon követheti a folyamat állapotát **értesítések** hello menüből.  

## <a name="troubleshooting"></a>Hibaelhárítás 
hello az automatizálási fiók tooregister hello munkavégző a Microsoft Monitoring Agent toocommunicate függ hello hibrid forgatókönyv-feldolgozó, runbook-feladatok és az állapot jelentése. A hello worker regisztrálása meghiúsul, ha az alábbiakban néhány hello hiba lehetséges okai:  

1. hello hibridfeldolgozó a proxy vagy az tűzfal mögött van.  
    Győződjön meg arról, hello számítógép 443-as port kimenő hozzáférést too*.azure-automation.net rendelkezik.  

2. hello számítógép hello hibridfeldolgozó futó rendelkezik kisebb, mint hello minimális hardver [követelmények](automation-offering-get-started.md#hybrid-runbook-worker).  
    Meg kell felelniük a hibrid forgatókönyv-feldolgozó hello futtató számítógépek hardverre vonatkozó minimális elvárásokat hello toohost kijelölő, mielőtt ezt a szolgáltatást. Ellenkező esetben az hello erőforrás-használat más háttérfolyamatot és versengés okozta a runbookok végrehajtása közben, attól függően, hogy hello számítógép túlterhelt fog válik, és runbook feladat működés lelassulhat, vagy időtúllépés miatt.
   Erősítse meg a kijelölt toorun hello hibrid forgatókönyv-feldolgozó szolgáltatás hello számítógép megfelel-e hello hardverre vonatkozó minimális elvárásokat.  Ha igen, figyelheti CPU és memória kihasználtsága toodetermine bármely korrelációs hibrid forgatókönyv-feldolgozó folyamat hello teljesítményét és a Windows között.  Ha memória vagy a CPU-terhelés, ez lehetséges, hogy hello kell tooupgrade jelzik, vagy további processzorok hozzáadásával, vagy növelje memória tooaddress erőforrás szűk hello és hello hiba megoldásához. Azt is megteheti válassza ki a különböző számítási erőforrása, amely támogatja a hello minimális követelményeknek, és ha terheléshez jelezheti növelését szükséges.
    
3. hello Microsoft Monitoring Agent szolgáltatás nem fut.  
    Hello Microsoft Monitoring Agent Windows szolgáltatás nem fut, ha ez megakadályozza, hogy hibrid forgatókönyv-feldolgozó hello Azure Automation kommunikál.  Ellenőrizze a hello ügynök fut-e a következő parancsot a PowerShell hello megadásával: `get-service healthservice`.  Ha hello szolgáltatás leáll, írja be a következő parancsot a PowerShell toostart hello szolgáltatásban hello: `start-service healthservice`.  

4. A hello **alkalmazások és szolgáltatások Logs\Operations kezelője** Eseménynapló, látni esemény 4502 és EventMessage tartalmazó **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**a leírása a következő hello: *hello szolgáltatás hello tanúsítvány <wsid>. oms.opinsights.azure.com nem a Microsoft-szolgáltatásokhoz használt hitelesítésszolgáltató állította ki. Lépjen kapcsolatba a hálózati rendszergazda toosee, ha a proxy, amely elfogja a TLS/SSL-kommunikáció futnak. hello kb3126513 jelű további információkat talál a csatlakozási problémák.*
    Ezt a proxy vagy a hálózati tűzfal blockking kommunikációs tooMicrosoft Azure okozhatja.  Győződjön meg arról, hello számítógép 443-as port kimenő hozzáférést too*.azure-automation.net rendelkezik.

Naplók minden hibridfeldolgozó C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes, helyileg tárolja.  Ellenőrizheti, hogy vannak-e minden figyelmeztetés vagy hiba eseményeket toohello **alkalmazások és szolgáltatások Logs\Microsoft-SMA\Operations** és **alkalmazások és szolgáltatások Logs\Operations kezelője** esemény naplózása, amelyek azt jelzi, a kapcsolattal vagy más probléma hello szerepkör tooAzure Automation vagy probléma bevezetésének befolyásolja a normál műveletek során.  

## <a name="next-steps"></a>Következő lépések
Felülvizsgálati [runbookot futtatni a hibrid forgatókönyv-feldolgozó](automation-hrw-run-runbooks.md) toolearn hogyan tooconfigure a runbookok tooautomate dolgozza fel a helyszíni adatközpontját vagy egyéb felhőalapú környezetben.

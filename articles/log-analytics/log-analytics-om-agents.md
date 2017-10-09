---
title: aaaConnect Operations Manager tooLog Analytics |} Microsoft Docs
description: "toomaintain a System Center Operations Manager és használja a meglévő befektetések kiterjesztett Naplóelemzési képességeket, az Operations Manager integrálható az OMS-munkaterület."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 245ef71e-15a2-4be8-81a1-60101ee2f6e6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: magoedte
ms.openlocfilehash: b2841c7aa209fec7357dc4c8b1ff4325fdaa37ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-operations-manager-toolog-analytics"></a>Csatlakozás az Operations Manager tooLog elemzés
toomaintain a System Center Operations Manager és használja a meglévő befektetések kiterjesztett Naplóelemzési képességeket, az Operations Manager integrálható az OMS-munkaterület.  Ez lehetővé teszi, miközben továbbra toouse Operations Manager az OMS hello lehetőségek is használja a:

* Az informatikai szolgáltatások, az Operations Manager hello állapotának ellenőrzéséhez
* A támogatási incidensek és a probléma felügyeleti ITSM megoldásaival való integráció fenntartása
* Telepített ügynökök tooon helyszíni és a nyilvános felhő infrastruktúra-szolgáltatási virtuális gépek és az Operations Manager figyelt hello életciklusát kezelése

Érték tooyour szolgáltatás műveletek stratégia integrálása a System Center Operations Manager hozzáadja a hello sebesség és a összegyűjtésének, tárolásának és elemzéséhez az Operations Manager OMS hatékonyságát.  OMS segít korrelálja és munkahelyi hello hibáinak problémák azonosítása és felszínre hozza a meglévő probléma felügyeleti folyamat támogatásához az ismétlődések felé.   hello hello keresési motor tooexamine teljesítmény-, esemény- és riasztás adatainak, gazdag irányítópultok rugalmasságát, és a jelentéskészítési képességek tooexpose ezeket az adatokat a ésszerű módszerrel mutatja be OMS számos lehetőséget kínál, az Operations Manager complimenting hello erőssége.

jelentési toohello Operations Manager felügyeleti csoport hello ügynökök adatokat gyűjteni a kiszolgálókról, hello Naplóelemzési adatforrások és az OMS-előfizetésben engedélyezett megoldások alapján.  Attól függően, hogy engedélyezte a hello megoldás ezek a megoldások adatait vagy elküldi közvetlenül egy Operations Manager felügyeleti kiszolgálóról toohello OMS webes szolgáltatás, vagy miatt hello hello ügynök által felügyelt rendszeren gyűjtött adatok mennyiségét küldött közvetlenül hello ügynök tooOMS webes szolgáltatás. hello felügyeleti kiszolgáló továbbítja hello OMS adatok közvetlen toohello OMS webszolgáltatás; a program soha nem készült toohello OperationsManager vagy OperationsManagerDW adatbázis.  Ha egy felügyeleti kiszolgáló hello OMS webszolgáltatással kapcsolata megszakad, azt gyorsítótárazza a hello adatokat helyileg, amíg a kommunikációs helyreáll, az OMS Szolgáltatáshoz.  Ha hello felügyeleti kiszolgáló megfelelő tooplanned karbantartás vagy nem tervezett leállás offline állapotban, a hello felügyeleti csoport egy másik felügyeleti kiszolgálóra folytatja OMS kapcsolatot.  

hello következő ábra szemlélteti hello kapcsolat hello felügyeleti kiszolgálók és az ügynökök a System Center Operations Manager felügyeleti csoport és az OMS-ben, beleértve a hello irányát és portok között.   

![OMS-műveletek-manager-integráció-diagram](./media/log-analytics-om-agents/oms-operations-manager-connection.png)

Az IT-biztonsági házirendeknek nem engedélyezi a hálózati tooconnect toohello Internet a számítógépek, ha a felügyeleti kiszolgálók kell konfigurált tooconnect toohello OMS átjáró tooreceive konfigurációs adatokat, és hello megoldás függ összegyűjtött adatok küldése engedélyezve van.  További információkat és lépéseket, hogyan tooconfigure az Operations Manager felügyeleti csoport toocommunicate egy OMS átjáró toohello OMS szolgáltatással: a [számítógépek tooOMS hello OMS átjáró használatával csatlakozzon](log-analytics-oms-gateway.md).  

## <a name="system-requirements"></a>Rendszerkövetelmények
Megkezdése előtt tekintse át a következő előfeltételek megfelel részletek tooverify hello.

* Csak akkor támogatja az Operations Manager 2016-Operations Manager 2012 SP1 UR6 OMS és nagyobb, és az Operations Manager 2012 R2 UR2 és nagyobb.  A proxytámogatás az Operations Manager 2012 SP1 UR 7-es és az Operations Manager 2012 R2 UR 3-as verziójában jelent meg.
* Az összes Operations Manager-ügynökök meg kell felelnie a minimális támogatási követelményeknek. Győződjön meg arról, hogy ügynökök hello minimális Update, ellenkező esetben Windows ügynök forgalom sikertelenek lehetnek és sok hiba előfordulhat, hogy kitöltse a hello Operations Manager eseménynaplójában.
* Az OMS-előfizetéssel.  További információkért tekintse át [Ismerkedés a Naplóelemzési](log-analytics-get-started.md).

### <a name="network"></a>Network (Hálózat)
lista hello proxy és tűzfal kért konfigurációs információkat hello Operations Manager-ügynök, a felügyeleti kiszolgálók és az operatív konzol toocommunicate az OMS alábbi hello információt.  Minden egyes összetevő érkező forgalom értéke kimenő, a hálózati toohello OMS szolgáltatásból.     

|Erőforrás | Portszám| HTTP-ellenőrzés kihagyása|  
|---------|------|-----------------------|  
|**Ügynök**|||  
|\*.ods.opinsights.azure.com| 443 |Igen|  
|\*.oms.opinsights.azure.com| 443|Igen|  
|\*.blob.core.windows.net| 443|Igen|  
|\*.azure-automation.net| 443|Igen|  
|**Felügyeleti kiszolgáló**|||  
|\*.service.opinsights.azure.com| 443||  
|\*.blob.core.windows.net| 443| Igen|  
|\*.ods.opinsights.azure.com| 443| Igen|  
|*.azure-automation.net | 443| Igen|  
|**Az Operations Manager konzol tooOMS**|||  
|service.systemcenteradvisor.com| 443||  
|\*.service.opinsights.azure.com| 443||  
|\*.live.com| 80-as és 443-as||  
|\*.microsoft.com| 80-as és 443-as||  
|\*.microsoftonline.com| 80-as és 443-as||  
|\*.mms.microsoft.com| 80-as és 443-as||  
|login.windows.net| 80-as és 443-as||  


## <a name="connecting-operations-manager-toooms"></a>Csatlakozás az Operations Manager tooOMS
Hajtsa végre a következő lépéseket tooconfigure sorozatát hello az Operations Manager felügyeleti csoport tooconnect tooone az OMS-munkaterület.

1. Hello Operations Manager konzolon válassza hello **felügyeleti** munkaterületen.
2. Hello Operations Management Suite csomópontot, és kattintson a **kapcsolat**.
3. Kattintson a hello **tooOperations felügyeleti csomag regisztrálása** hivatkozásra.
4. A hello **Operations Management Suite előkészítési varázslója: hitelesítés** lapon adja meg az üdvözlő e-mail címet vagy telefonszámot, és az OMS-előfizetéshez társított hello rendszergazdai fiók jelszavát, majd kattintson a  **Jelentkezzen be a**.
5. Ön hitelesítése után vannak sikeresen hello a **Operations Management Suite előkészítési varázslója: Munkaterület kiválasztása** lap, amelyek tooselect az OMS-munkaterület kéri.  Ha egynél több munkaterületen, válassza ki hello tooregister szeretné, hogy a hello Operations Manager felügyeleti csoport hello legördülő listából, és kattintson a **következő**.
   
   > [!NOTE]
   > Az Operations Manager csak egyszerre egy OMS-munkaterület támogat. hello kapcsolat és a regisztrált tooOMS hello előző munkaterület volt hello számítógépek törlődnek az OMS Szolgáltatáshoz.
   > 
   > 
6. A hello **Operations Management Suite előkészítési varázslója: Összegzés** lapon hagyja jóvá a beállításokat, ha azok a helyes-e, kattintson **létrehozása**.
7. A hello **Operations Management Suite előkészítési varázslója: Befejezés** kattintson **Bezárás**.

### <a name="add-agent-managed-computers"></a>Ügynök által felügyelt számítógépek hozzáadása
Integrációjának konfigurálása után az OMS-munkaterület, ez csak OMS kapcsolatot létesít, nincs adatgyűjtés ügynök hello tooyour felügyeleti csoportban. Ez nem fordulhat elő, amíg mely adott ügynök által felügyelt számítógépek adatokat gyűjt a Log Analyticshez konfigurálása után. Kiválaszthatja hello számítógép-objektumok önállóan, vagy kiválaszthat egy csoportot, amely tartalmazza a Windows számítógép-objektumok. Nem választhat ki egy csoportot, amely egy másik osztály, például logikai lemezek vagy az SQL-adatbázis példányait tartalmazza.

1. Operations Manager-konzol megnyitása hello és select hello **felügyeleti** munkaterületen.
2. Hello Operations Management Suite csomópontot, és kattintson a **kapcsolat**.
3. Kattintson a hello **egy számítógépcsoport hozzáadásához** hello műveletek alatt hello jobb oldali ablaktábla hello a fejléc.
4. A hello **számítógép keresése** párbeszédpanelen is kereshet számítógépek vagy csoportok az Operations Manager általi megfigyelés alatt. Jelölje ki azt a számítógépek vagy csoportok tooonboard tooOMS **Hozzáadás**, és kattintson a **OK**.

Megtekintheti a számítógépek és csoportok toocollect adatokat az Operations Management Suite csomópontjából hello felügyelt számítógépek konfigurálva hello **felügyeleti** munkaterület hello operatív konzol.  Itt adja hozzá, vagy távolítsa el a számítógép- és szükség szerint.

### <a name="configure-oms-proxy-settings-in-hello-operations-console"></a>Az operatív konzolon hello OMS proxybeállításainak konfigurálása
Hajtsa végre a következő lépéseket, ha egy belső proxykiszolgáló hello felügyeleti csoport és az OMS-webszolgáltatás közötti hello.  Ezek a beállítások központilag felügyelt hello felügyeleti csoportból, tooagent által felügyelt elosztottrendszer, OMS hello hatókör toocollect adatok szerepelnek.  Ez akkor hasznos, ha egyes megoldások Mellőzés-e hello felügyeleti kiszolgáló és a küldési adatokat közvetlenül tooOMS webszolgáltatás számára.

1. Operations Manager-konzol megnyitása hello és select hello **felügyeleti** munkaterületen.
2. Bontsa ki az Operations Management Suite, majd a **kapcsolatok**.
3. Hello OMS kapcsolat nézetet, kattintson **Proxy kiszolgáló konfigurálása**.
4. A **Operations Management Suite varázslója: proxykiszolgáló** lapon jelölje be **használja a proxy server tooaccess hello Operations Management Suite**, majd írja be a hello hello portszámot, például a http:// URL-cím corpproxy:80, és kattintson **Befejezés**.

Ha a proxykiszolgálóhoz hitelesítés szükséges, hajtsa végre a hello alábbi lépéseit tooconfigure hitelesítő adatokat és beállításokat, amelyekre szükségük toopropagate toomanaged számítógépek jelentéseket tooOMS hello felügyeleti csoportban.

1. Operations Manager-konzol megnyitása hello és select hello **felügyeleti** munkaterületen.
2. A **RunAs Configuration** (RunAs-konfiguráció) területen válassza a **Profiles** (Profilok) lehetőséget.
3. Nyissa meg hello **System Center Advisor Futtatás mint profil Proxy** profil.
4. A futtató profil varázsló hello kattintson a Hozzáadás toouse egy futtató fiókot. Létrehozhat egy [Futtatás mint fiók](https://technet.microsoft.com/library/hh321655.aspx) vagy egy meglévő fiókot használjon. Ezt a fiókot kell toohave megfelelő engedélyek toopass hello proxykiszolgálón keresztül.
5. tooset hello fiók toomanage, válassza a **egy kijelölt osztály, csoport vagy objektum**, kattintson a **válasszon...** majd **csoporthoz...** tooopen hello **csoport keresési** mezőbe.
6. Keresse meg és válassza ki **Microsoft System Center Advisor figyelési kiszolgáló csoport**.  Kattintson a **OK** hello csoport tooclose hello kijelölése után **csoport keresési** mezőbe.
7. Kattintson a **OK** tooclose hello **hozzáadása egy futtató fiókot** mezőbe.
8. Kattintson a **mentése** toocomplete hello varázsló, és mentse a módosításokat.

Miután hello kapcsolat jön létre, és mely ügynökök gyűjt és adatok tooOMS jelentést konfigurál, hello alábbi konfiguráció alkalmazása hello felügyeleti csoportban, nem feltétlenül sorrendben:

* Futtató fiók hello **Microsoft.SystemCenter.Advisor.RunAsAccount.Certificate** jön létre.  Az hello futtató profilhoz társítva **Microsoft System Center Advisor Futtatás mint profil Blob** célzott két osztály - és **gyűjtési kiszolgáló** és **Operations Manager felügyeleti csoport** .
* Két összekötők jönnek létre.  hello először nevű **Microsoft.SystemCenter.Advisor.DataConnector** , és automatikusan konfigurálja a előfizetést, amely továbbítja a hello felügyeleti csoport tooOMS napló minden osztály példányai alapján generált összes riasztás Elemzés. hello második összekötő **az Advisor Connector**, amely felelős az OMS webes szolgáltatással folytatott kommunikáció és az adatok megosztása.
* Az ügynökök és a kiválasztott toocollect adatok hello felügyeleti csoportban található csoportok hozzáadott toohello **Microsoft System Center Advisor figyelési kiszolgáló csoport**.

## <a name="management-pack-updates"></a>Felügyeleti csomagok frissítéséhez
Konfiguráció befejezése után hello Operations Manager felügyeleti csoport hello OMS szolgáltatás kapcsolatot létesít.  hello felügyeleti kiszolgáló hello webszolgáltatás szinkronizálja, és a frissített konfigurációs információk fogadása hello megoldások engedélyezte, amelyekbe beépül az Operations Manager felügyeleti csomagok hello formában.   Az Operations Manager ellenőrzi a felügyeleti csomagok és az automatikus frissítések letöltése, és importálja azokat, ha azok elérhetők.  Két szabály van különösen ami szabályozhatja, hogy ez a viselkedés:

* **Microsoft.SystemCenter.Advisor.MPUpdate** -hello alap OMS felügyeleti csomagok frissíti. Alapértelmezés szerint 12 óránként fut.
* **Microsoft.SystemCenter.Advisor.Core.GetIntelligencePacksRule** -frissíti a megoldás felügyeleti csomagok engedélyezve van a munkaterületen. Alapértelmezés szerint öt (5) percenként fut.

Bírálja felül az alábbi két szabály tooeither automatikus letöltés tiltása letiltásával őket, vagy módosítsa hello gyakorisága milyen gyakran hello felügyeleti kiszolgáló szinkronizálása OMS toodetermine Ha egy új felügyeleti csomag elérhető, és le kell tölteni.  Hello lépésekkel [hogyan tooOverride egy szabály vagy figyelő](https://technet.microsoft.com/library/hh212869.aspx) toomodify hello **gyakoriság** paraméter másodperc toochange a szinkronizálási ütemezés hello, vagy módosítsa a hello **engedélyezve**  paraméter toodisable hello szabályok.  Cél hello tooall objektumok Operations Manager felügyeleti csoport osztály felülbírálja.

Ha azt szeretné, hogy a meglévő vezérlő folyamatának a üzemi felügyeleti csoportban található felügyeleti csomag kiadásokban vezérlése a következő toocontinue, hello szabályok letiltása, és lehetővé teszi az adott időben, amikor a frissítések engedélyezettek. Ha rendelkezik olyan fejlesztési vagy QA felügyeleti csoporthoz a környezetben, és nem rendelkezik Internet kapcsolat toohello, konfigurálható, hogy a felügyeleti csoport egy OMS-munkaterület toosupport ebben a forgatókönyvben.  Ez lehetővé teszi a tooreview, és hello iteratív kiadásaiban hello OMS felügyeleti csomagok értékeli őket az üzemi felügyeleti csoportjába feloldása előtt.

## <a name="switch-an-operations-manager-group-tooa-new-oms-workspace"></a>Az Operations Manager csoport tooa kapcsoló új OMS-munkaterület
1. Jelentkezzen be tooyour OMS-előfizetés és a munkaterület létrehozása [a Microsoft Operations Management Suite](http://oms.microsoft.com/).
2. Nyissa meg hello Operations Manager konzolt egy olyan fiókkal, amely tagja az Operations Manager-Rendszergazdák szerepkör hello és select hello **felügyeleti** munkaterületen.
3. Bontsa ki az Operations Management Suite, és válassza **kapcsolatok**.
4. Jelölje be hello **művelet Management Suite újrakonfigurálása** hello hello ablaktábla közel-oldalán hivatkozásra kattintva.
5. Hajtsa végre a hello **Operations Management Suite előkészítési varázslója** , és adja meg az üdvözlő e-mail címét vagy telefonszámát számát és az új OMS-munkaterület társított hello rendszergazdai fiók jelszavát.
   
   > [!NOTE]
   > Hello **Operations Management Suite előkészítési varázslója: Munkaterület kiválasztása** lapon megadja hello meglévő munkaterület, amely használatban van.
   > 
   > 

## <a name="validate-operations-manager-integration-with-oms"></a>Az Operations Manager integrációja, OMS ellenőrzése
Azt is ellenőrizze, hogy az OMS tooOperations Manager integrációs sikeres néhány különböző módja van.

### <a name="tooconfirm-integration-from-hello-oms-portal"></a>tooconfirm integrációját hello OMS-portálon
1. Az hello OMS-portálon, kattintson a hello **beállítások** csempéje
2. Válassza ki **csatlakoztatott adatforrások**.
3. A System Center Operations Manager szakasz hello hello táblázatban megtekintheti az hello felügyeleti csoport ügynököket és állapot hello számú felsorolt adatok utolsó fogadásakor hello neve.
   
   ![OMS-beállítások-connectedsources](./media/log-analytics-om-agents/oms-settings-connectedsources.png)
4. Megjegyzés: hello **munkaterület azonosítója** hello bal oldali hello-beállítások lap értéket.  Az Operations Manager felügyeleti csoportjának az alábbi szemben érvényesíti.  

### <a name="tooconfirm-integration-from-hello-operations-console"></a>az operatív konzolról hello tooconfirm integráció
1. Operations Manager-konzol megnyitása hello és select hello **felügyeleti** munkaterületen.
2. Válassza ki **felügyeleti csomagok** és hello **keres:** szöveg írja be a következőt **Advisor** vagy **Eszközintelligencia**.
3. Attól függően, hogy engedélyezte a hello megoldások tekintse meg a találatok között hello felsorolt megfelelő felügyeleti csomagot.  Például, ha engedélyezte a hello riasztási felügyeleti megoldás, hello felügyeleti csomag Microsoft System Center Advisor Riasztáskezelési hello listában van.
4. A hello **figyelés** megtekintéséhez nyissa meg a toohello **Operations Management Suite\Health állapot** nézet.  Jelöljön ki egy felügyeleti csoportban hello **felügyeleti kiszolgáló állapota** ablaktáblán, és a hello **részletes nézet** panelen ellenőrizze tulajdonság értéke hello **hitelesítési szolgáltatás URI** megfelel a hello OMS-munkaterület azonosítója.
   
   ![OMS-opsmgr-mg-authsvcuri-Property-MS](./media/log-analytics-om-agents/oms-opsmgr-mg-authsvcuri-property-ms.png)

## <a name="remove-integration-with-oms"></a>Távolítsa el az OMS integrációja
Integráció az Operations Manager felügyeleti csoport és az OMS-munkaterület között már nincs szükség, ha nincsenek több szükséges lépések tooproperly eltávolítása hello kapcsolatot és hello felügyeleti csoport konfigurációját. hello következő eljárás rendelkezik, az OMS-munkaterület frissíteni a felügyeleti csoport hello hivatkozás törlésével, törölje az hello OMS-összekötőt, és törölje a OMS támogató felügyeleti csomagok.   

Hello megoldások engedélyezte, amelyekbe beépül az Operations Manager felügyeleti csomagok, és hello felügyeleti csomagok szükséges toosupport integrációs hello OMS szolgáltatáshoz nem lehet könnyen törölni hello felügyeleti csoportból.  Ennek az az oka hello OMS felügyeleti csomagok némelyike más kapcsolódó felügyeleti csomagoktól tartalmazhat függőségeket.  függőség rendelkező más felügyeleti csomagoktól függ toodelete felügyeleti csomagok letöltése hello parancsfájl [függőségekkel rendelkező felügyeleti csomag eltávolítása](https://gallery.technet.microsoft.com/scriptcenter/Script-to-remove-a-84f6873e) TechNet Script Center.  

1. Nyissa meg az Operations Manager parancs-rendszerhéj hello egy olyan fiókkal, amely hello Operations Manager-Rendszergazdák szerepkör tagja.
   
    > [!WARNING]
    > Ellenőrizze, nem rendelkezik a folytatás előtt hello név hello word Advisor vagy IntelligencePack bármilyen egyéni felügyeleti csomagok, ellenkező esetben a lépéseket követve hello törölje őket hello felügyeleti csoportból.
    > 

2. A rendszerhéj hello parancssort írja be az`Get-SCOMManagementPack -name "*Advisor*" | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
3. Következő típusa`Get-SCOMManagementPack -name “*IntelligencePack*” | Remove-SCOMManagementPack -ErrorAction SilentlyContinue`
4. tooremove a felügyeleti csomagok fennmaradó, amely rendelkezik egy függőséget más System Center Advisor felügyeleti csomagok, hello parancsfájllal *RecursiveRemove.ps1* a TechNet Script Center hello korábban letöltött.  
 
    > [!NOTE]
    > Ne törölje hello Microsoft System Center Advisor vagy a Microsoft System Center Advisor belső felügyeleti csomag.  
    >  

5. Nyissa meg a hello Operations Manager operatív konzol egy olyan fiókkal, amely hello Operations Manager-Rendszergazdák szerepkör tagja.
6. A **felügyeleti**, jelölje be hello **felügyeleti csomagok** csomópont és a hello **keres:** mezőbe írja be **Advisor** , és ellenőrizze a hello következő felügyeleti csomagokat továbbra is az Ön felügyeleti csoportjába importált:
   
   * A Microsoft System Center Advisor
   * A Microsoft System Center Advisor belső
7. Az hello OMS-portálon, kattintson a hello **beállítások** csempére.
8. Válassza ki **csatlakoztatott adatforrások**.
9. A System Center Operations Manager szakasz hello hello táblázatban kell megjelennie hello neve hello felügyeleti csoport kívánt tooremove hello munkaterületről.  Hello oszlopban **utolsó adatok**, kattintson a **eltávolítása**.  
   
    > [!NOTE]
    > Hello **eltávolítása** hivatkozás nem lesz elérhető 14 nap elteltével nem észlelhető hello csatlakoztatott felügyeleti csoportból tevékenység esetén.  
    > 

10. Egy ablakban jelenik meg, hogy szeretné-e a hello eltávolító tooproceed tooconfirm kéri.  Kattintson a **Igen** tooproceed. 

toodelete hello két összekötők - Microsoft.SystemCenter.Advisor.DataConnector és az Advisor Connector, mentse a hello PowerShell parancsfájl tooyour számítógép alatt, és hajtsa végre a következő példák hello használata:

```
    .\OM2012_DeleteConnector.ps1 “Advisor Connector” <ManagementServerName>
    .\OM2012_DeleteConnectors.ps1 “Microsoft.SytemCenter.Advisor.DataConnector” <ManagementServerName>
```

> [!NOTE]
> hello számítógépen futtatja a parancsfájlt, ha nem a felügyeleti kiszolgáló hello Operations Manager parancs-rendszerhéj attól függően, hogy a felügyeleti csoport hello verziójának telepítve kell lennie.
> 
> 

```
    `param(
    [String] $connectorName,
    [String] $msName="localhost"
    )
    $mg = new-object Microsoft.EnterpriseManagement.ManagementGroup $msName
    $admin = $mg.GetConnectorFrameworkAdministration()
    ##########################################################################################
    # Configures a connector with hello specified name.
    ##########################################################################################
    function New-Connector([String] $name)
    {
         $connectorForTest = $null;
         foreach($connector in $admin.GetMonitoringConnectors())
    {
    if($connectorName.Name -eq ${name})
    {
         $connectorForTest = Get-SCOMConnector -id $connector.id
    }
    }
    if ($connectorForTest -eq $null)
    {
         $testConnector = New-Object Microsoft.EnterpriseManagement.ConnectorFramework.ConnectorInfo
         $testConnector.Name = $name
         $testConnector.Description = "${name} Description"
         $testConnector.DiscoveryDataIsManaged = $false
         $connectorForTest = $admin.Setup($testConnector)
         $connectorForTest.Initialize();
    }
    return $connectorForTest
    }
    ##########################################################################################
    # Removes a connector with hello specified name.
    ##########################################################################################
    function Remove-Connector([String] $name)
    {
        $testConnector = $null
        foreach($connector in $admin.GetMonitoringConnectors())
       {
        if($connector.Name -eq ${name})
       {
         $testConnector = Get-SCOMConnector -id $connector.id
       }
      }
     if ($testConnector -ne $null)
     {
        if($testConnector.Initialized)
     {
     foreach($alert in $testConnector.GetMonitoringAlerts())
     {
       $alert.ConnectorId = $null;
       $alert.Update("Delete Connector");
     }
     $testConnector.Uninitialize()
     }
     $connectorIdForTest = $admin.Cleanup($testConnector)
     }
    }
    ##########################################################################################
    # Delete a connector's Subscription
    ##########################################################################################
    function Delete-Subscription([String] $name)
    {
      foreach($testconnector in $admin.GetMonitoringConnectors())
      {
      if($testconnector.Name -eq $name)
      {
        $connector = Get-SCOMConnector -id $testconnector.id
      }
    }
    $subs = $admin.GetConnectorSubscriptions()
    foreach($sub in $subs)
    {
      if($sub.MonitoringConnectorId -eq $connector.id)
      {
        $admin.DeleteConnectorSubscription($admin.GetConnectorSubscription($sub.Id))
      }
     }
    }
    #New-Connector $connectorName
    write-host "Delete-Subscription"
    Delete-Subscription $connectorName
    write-host "Remove-Connector"
    Remove-Connector $connectorName
```

A jövőbeli hello újracsatlakozni a felügyeleti csoport tooan OMS-munkaterület, ha szüksége toore-importálási hello `Microsoft.SystemCenter.Advisor.Resources.\<Language>\.mpb` felügyeleticsomag-fájlt a legújabb kumulatív hello alkalmazott tooyour felügyeleti csoportban.  Ez a fájl található hello `%ProgramFiles%\Microsoft System Center 2012` vagy hello `System Center 2012 R2\Operations Manager\Server\Management Packs for Update Rollups` mappát.

## <a name="next-steps"></a>Következő lépések
tooadd és összefog adatok, lásd: [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).



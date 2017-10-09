---
title: "ügyfélállapot-megoldást az OMS aaaAgent |} Microsoft Docs"
description: "Ez a cikk tervezett toohelp hogyan toouse a megoldás toomonitor hello állapotának tisztában a jelentéskészítési közvetlenül a tooOMS vagy a System Center Operations Manager ügynököt."
services: operations-management-suite
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: operations-management-suite
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/17/2017
ms.author: magoedte
ms.openlocfilehash: 071b14b4ab7af6680ae458eaa331246755c5bb56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#  <a name="agent-health-solution-in-oms"></a>Ügynökállapot megoldás
hello ügyfélállapot megoldás OMS segít megérteni, az összes hello ügynök közvetlenül toohello OMS-munkaterület vagy a System Center Operations Manager felügyeleti csoport csatlakoztatva tooOMS, amelyek nem válaszol, és a küldő működési adatokat.  Akkor is is nyomon követheti, hány ügynök van telepítve, ahonnan földrajzilag elosztott-e, és hajtsa végre a más lekérdezések toomaintain tájékoztatási hello eloszlás Azure, más felhőkörnyezetekben vagy a helyszínen telepített ügynökök.    

## <a name="prerequisites"></a>Előfeltételek
Ez a megoldás telepítése előtt győződjön meg róla, jelenleg támogatott van [Windows-ügynökök](../log-analytics/log-analytics-windows-agents.md) toohello OMS-munkaterület jelentéskészítés vagy reporting tooan [Operations Manager felügyeleti csoport](../log-analytics/log-analytics-om-agents.md) részét képező a OMS-munkaterület.    

## <a name="solution-components"></a>Megoldás-összetevők
Ez a megoldás a következő erőforrások tooyour munkaterület és a közvetlenül csatlakoztatott ügynökök vagy az Operations Manager csatlakoztatott felügyeleti csoporthoz hozzáadott hello áll.

### <a name="management-packs"></a>Felügyeleti csomagok
Ha a System Center Operations Manager felügyeleti csoport csatlakoztatott tooan OMS-munkaterület, az Operations Manager felügyeleti csomagok a következő hello vannak telepítve.  Ezeket a felügyeleti csomagokat a megoldás hozzáadását követően a rendszer a közvetlenül kapcsolódó Windows rendszerű számítógépekre is telepíti. Nincs mit tooconfigure vagy a felügyeleti csomagok kezelése.

* Microsoft System Center Advisor HealthAssessment Direct Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentDirect)
* Microsoft System Center Advisor HealthAssessment Server Channel Intelligence Pack (Microsoft.IntelligencePacks.HealthAssessmentViaServer).  

A megoldás felügyeleti csomagok frissítésének további információkért lásd: [csatlakozás az Operations Manager tooLog Analytics](../log-analytics/log-analytics-om-agents.md).

## <a name="configuration"></a>Konfiguráció
Adja hozzá a hello ügyfélállapot megoldás tooyour ismertetett eljárással hello OMS-munkaterület [megoldások hozzáadása](../log-analytics/log-analytics-add-solutions.md). Nincs szükség további konfigurációra.


## <a name="data-collection"></a>Adatgyűjtés
### <a name="supported-agents"></a>Támogatott ügynökök
a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti.

| Összekapcsolt forrás | Támogatott | Leírás |
| --- | --- | --- |
| Windows-ügynökök | Igen | A szívverés eseményeket a rendszer a közvetlen Windows-ügynököktől gyűjti össze.|
| System Center Operations Manage felügyeleti csoport | Igen | Szívverés események jelentési toohello felügyeleti csoport 60 másodpercenként ügynökök gyűjtött, és továbbítja a tooLog elemzés. Közvetlen kapcsolat az Operations Manager-ügynökök tooLog Analytics nincs szükség. Szívverés eseményadatok adattárból hello felügyeleti csoport toohello Naplóelemzési továbbítja.|

## <a name="using-hello-solution"></a>Hello megoldással
Hello megoldás tooyour OMS-munkaterület felvételekor hello **ügyfélállapot** csempe megkapja tooyour OMS irányítópult. Ez a csempe megjeleníti hello ügynökök száma és a nem válaszoló ügynökök száma hello hello utolsó 24 órában.<br><br> ![Ügynökállapot megoldás csempe az irányítópulton](./media/oms-solution-agenthealth/agenthealth-solution-tile-homepage.png)

Kattintson a hello **ügyfélállapot** csempe tooopen hello **ügyfélállapot** irányítópult.  hello irányítópult szerepel a következő táblázat hello hello oszlopa. Egyes oszlopok hello felső tíz események száma, amelyek megfelelnek az, hogy az oszlop feltételek hello megadott időtartomány sorolja fel. Futtathatja a napló keresési kiválasztásával hello teljes lista biztosító **láthatja az összes** hello jobb alsó oszlopok, vagy hello oszlop fejlécére kattint.

| Oszlop | Leírás |
|--------|-------------|
| Ügynökök darabszáma egységidő alatt | Az ügynökök számának trendje egy hét napos időszakra vetítve, a Linux- és Windows-ügynököket is beleértve.|
| Nem válaszoló ügynökök száma | Ügynökök, amelyek még nem küld szívverést hello az elmúlt 24 órában listáját.|
| Eloszlás operációsrendszer-típusok szerint | A környezetben található Windows- és Linux-ügynökök számának eloszlása.|
| Eloszlás ügynökverzió szerint | A partíció hello különböző ügynök verziók telepítve a környezetben, és minden egyes számát.|
| Eloszlás ügynökkategória szerint | Ügynököket, amelyeket fel a szívverés események küldi hello különböző kategóriáihoz partíciójának: közvetlen ügynökök, OpsMgr ügynökök vagy hello OpsMgr felügyeleti kiszolgálón.|
| Eloszlás felügyeleti csoport szerint | A partíció hello különböző SCOM felügyeleti csoportok a környezetben.|
| Az ügynökök földrajzi helye | A partíció hello más országok ügynökök és a teljes számát hello száma az egyes országok telepített ügynökök esetében.|
| Telepített átjárók száma | kiszolgálók száma hello hello OMS telepített átjárót, és ezek a kiszolgálók listáját.|

![Ügynökállapot megoldás irányítópultja – példa](./media/oms-solution-agenthealth/agenthealth-solution-dashboard.png)  

## <a name="log-analytics-records"></a>Log Analytics-rekordok
hello megoldás egyféle típusú rekord hello OMS-tárház hoz létre.  

### <a name="heartbeat-records"></a>Szívverés rekordok
Egy **Szívverés** típusú rekord készül.  Ezeket a rekordokat a következő táblázat hello hello jellemzőkkel rendelkezik.  

| Tulajdonság | Leírás |
| --- | --- |
| Típus | *Szívverés*|
| Kategória | Az érték lehet *Direct Agent* (Közvetlen ügynök), *SCOM Agent* (SCOM-ügynök) vagy *SCOM Management Server* (SCOM felügyeleti kiszolgáló).|
| Computer | A számítógép neve.|
| OSType | Windows vagy Linux operációs rendszer.|
| OSMajorVersion | Az operációs rendszer főverziója.|
| OSMinorVersion | Az operációs rendszer alverziója.|
| Verzió | Az OMS-ügynök vagy Operations Manager-ügynök verziója.|
| SCAgentChannel | Az érték *Direct* (Közvetlen) és/vagy *SCManagementServer*.|
| IsGatewayInstalled | Ha az OMS-átjáró telepítve van, az érték *true* (igaz), más esetben *false* (hamis).|
| ComputerIP | Hello számítógép IP-címét.|
| RemoteIPCountry | A földrajzi hely, ahol a számítógép üzemel.|
| ManagementGroupName | Az Operations Manager felügyeleti csoportjának neve.|
| SourceComputerId | A számítógép egyedi azonosítója.|
| RemoteIPLongitude | A számítógép földrajzi helyének hosszúsági koordinátája.|
| RemoteIPLatitude | A számítógép fölrajzi helyének szélességi koordinátája.|

Minden ügynök jelentéskészítő tooan Operations Manager felügyeleti kiszolgáló két szívveréseket küld, és SCAgentChannel tulajdonság értékét egyaránt kiterjed **közvetlen** és **SCManagementServer** attól függően, hogy mi Napló Analytics adatforrások és az OMS-előfizetésben engedélyezett megoldások. Emlékezzen vissza, ha a megoldások esetén küldött közvetlenül az Operations Manager felügyeleti kiszolgáló toohello OMS webes szolgáltatás, vagy közvetlenül a hello ügynök tooOMS webszolgáltatás küldött hello ügynökön gyűjtött adatok mennyiségét hello miatt. A szívverés eseményeket, amelyek hello érték **SCManagementServer**, hello ComputerIP érték hello hello felügyeleti kiszolgáló IP-címét is, mivel hello adatok ténylegesen töltheti fel azt.  Ahol SCAgentChannel túl beállítása szívverések**közvetlen**, hello ügynök hello nyilvános IP-címe.  

## <a name="sample-log-searches"></a>Naplókeresési minták
hello következő táblázat a megoldás által gyűjtött rekordok minta napló keres.

| Lekérdezés | Leírás |
| --- | --- |
| Type=Heartbeat &#124; Distinct Computer |Az ügynökök száma összesen |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-24HOURS |Az elmúlt 24 órában hello nem válaszoló ügynökök száma |
| Type=Heartbeat &#124; measure max(TimeGenerated) as LastCall by Computer &#124; where LastCall < NOW-15MINUTES |Az elmúlt 15 perc hello nem válaszoló ügynökök száma |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer IN {Type=Heartbeat TimeGenerated>NOW-24HOURS &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Számítógépek online (a hello elmúlt 24 óra) |
| Type=Heartbeat TimeGenerated>NOW-24HOURS Computer NOT IN {Type=Heartbeat TimeGenerated>NOW-30MINUTES &#124; distinct Computer} &#124; measure max(TimeGenerated) as LastCall by Computer |Teljes ügynökök offline állapotba az elmúlt 30 perc (a hello elmúlt 24 óra) |
| Type=Heartbeat &#124; measure countdistinct(Computer) by OSType |Az ügynökök számának trendje időbeli alakulásának lekérése operációsrendszer-típusonként|
| Type=Heartbeat&#124;measure countdistinct(Computer) by OSType |Eloszlás operációsrendszer-típusok szerint |
| Type=Heartbeat&#124;measure countdistinct(Computer) by Version |Eloszlás ügynökverzió szerint |
| Type=Heartbeat&#124;measure count() by Category |Eloszlás ügynökkategória szerint |
| Type=Heartbeat&#124;measure countdistinct(Computer) by ManagementGroupName | Eloszlás felügyeleti csoport szerint |
| Type=Heartbeat&#124;measure countdistinct(Computer) by RemoteIPCountry |Az ügynökök földrajzi helye |
| Type=Heartbeat IsGatewayInstalled=true&#124;Distinct Computer |A telepített OMS-átjárók száma |


>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](../log-analytics/log-analytics-log-search-upgrade.md), majd a fenti lekérdezések hello megváltozna toohello következő.
>
>| Lekérdezés | Leírás |
|:---|:---|
| Heartbeat &#124; distinct Computer |Az ügynökök száma összesen |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(24h) |Az elmúlt 24 órában hello nem válaszoló ügynökök száma |
| Heartbeat &#124; summarize LastCall = max(TimeGenerated) by Computer &#124; where LastCall < ago(15m) |Az elmúlt 15 perc hello nem válaszoló ügynökök száma |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer in ((Heartbeat &#124; where TimeGenerated > ago(24h) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Számítógépek online (a hello elmúlt 24 óra) |
| Heartbeat &#124; where TimeGenerated > ago(24h) and Computer !in ((Heartbeat &#124; where TimeGenerated > ago(30m) &#124; distinct Computer)) &#124; summarize LastCall = max(TimeGenerated) by Computer |Teljes ügynökök offline állapotba az elmúlt 30 perc (a hello elmúlt 24 óra) |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Az ügynökök számának trendje időbeli alakulásának lekérése operációsrendszer-típusonként|
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by OSType |Eloszlás operációsrendszer-típusok szerint |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by Version |Eloszlás ügynökverzió szerint |
| Heartbeat &#124; summarize AggregatedValue = count() by Category |Eloszlás ügynökkategória szerint |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by ManagementGroupName | Eloszlás felügyeleti csoport szerint |
| Heartbeat &#124; summarize AggregatedValue = dcount(Computer) by RemoteIPCountry |Az ügynökök földrajzi helye |
| Heartbeat &#124; where iff(isnotnull(toint(IsGatewayInstalled)), IsGatewayInstalled == true, IsGatewayInstalled == "true") == true &#124; distinct Computer |A telepített OMS-átjárók száma |

## <a name="next-steps"></a>Következő lépések

* A Log Analytics-riasztások létrehozásával kapcsolatos információkért lásd: [Riasztások a Log Analyticsben](../log-analytics/log-analytics-alerts.md).

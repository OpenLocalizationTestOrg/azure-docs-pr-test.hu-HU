---
title: "az Operations Management Suite (OMS) megoldás aaaAlert |} Microsoft Docs"
description: "hello riasztási felügyeleti megoldás a Naplóelemzési segítségével elemezheti az összes hello riasztások a környezetben.  Továbbá tooconsolidating riasztásokban OMS vezérlőben azt importál riasztások csatlakoztatott System Center Operations Manager felügyeleti csoportokból származó Naplóelemzési."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: fe5d534e-0418-4e2f-9073-8025e13271a8
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2017
ms.author: bwren
ms.openlocfilehash: aff9bd8d88839c5227bb9ec3a1b5209a3cd7cdf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="alert-management-solution-in-operations-management-suite-oms"></a>Riasztási felügyeleti megoldás az Operations Management Suite (OMS)

![Felügyeleti figyelmeztető ikon](media/log-analytics-solution-alert-management/icon.png)

hello riasztási felügyeleti megoldás segítségével elemezheti hello riasztások a Log Analyticshez tárházban összes.  Ezek a riasztások különböző forrásokból, beleértve az olyan forrásból származó jutott [Naplóelemzési által létrehozott](log-analytics-alerts.md) vagy [Nagios vagy Zabbix importált](log-analytics-linux-agents.md).  hello megoldás is importálja a riasztások bármelyik [csatlakoztatott felügyeleti csoportok System Center Operations Manager](log-analytics-om-agents.md).

## <a name="prerequisites"></a>Előfeltételek
hello megoldás együttműködve biztosítja a hello Log Analytics-tárházban típusú rekordok **riasztási**, ezért el kell végeznie, függetlenül a beállítás kötelező toocollect ezeket a rekordokat.

- A Naplóelemzési riasztások [riasztási szabályok létrehozásához](log-analytics-alerts.md) toocreate riasztási rekordok közvetlenül a hello tárházban.
- Nagios és Zabbix riasztások [ezek a kiszolgálók konfigurálása](log-analytics-linux-agents.md) toosend tooLog Analytics riasztást küld.
- A System Center Operations Manager-riasztások [csatlakozás az Operations Manager felügyeleti csoport tooyour Naplóelemzési munkaterület](log-analytics-om-agents.md).  Minden létrehozott System Center Operations Manager riasztást a rendszer importálta a Naplóelemzési.  

## <a name="configuration"></a>Konfiguráció
Adja hozzá a hello Riasztáskezelési megoldás tooyour ismertetett eljárással hello OMS-munkaterület [megoldások hozzáadása](log-analytics-add-solutions.md).  Nincs szükség további konfigurációra.

## <a name="management-packs"></a>Felügyeleti csomagok
Ha a System Center Operations Manager felügyeleti csoport csatlakoztatott tooyour OMS-munkaterület, majd a következő felügyeleti csomagok hello vannak telepítve a System Center Operations Manager ebben a megoldásban hozzáadásakor.  Nincs, konfigurációs vagy karbantartás hello felügyeleti csomag szükséges.  

* A Microsoft System Center Advisor Riasztáskezelési (Microsoft.IntelligencePacks.AlertManagement)

A megoldás felügyeleti csomagok frissítésének további információkért lásd: [csatlakozás az Operations Manager tooLog Analytics](log-analytics-om-agents.md).

## <a name="data-collection"></a>Adatgyűjtés
### <a name="agents"></a>Ügynökök
a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti.

| Összekapcsolt forrás | Támogatás | Leírás |
|:--- |:--- |:--- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Nem |A közvetlen Windows-ügynökök nem hoznak létre riasztásokat.  Napló Analytics riasztások az események hozhatók létre, és a Windows ügynökök gyűjtött teljesítményadatokat. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem |Közvetlen Linux-ügynökök nem hoznak létre riasztásokat.  Napló Analytics riasztások hozhatók létre az események és teljesítményadatok összegyűjtésére a Linux-ügynököt.  Ezek a kiszolgálók hello Linux-ügynök igénylő begyűjti a Nagios és Zabbix riasztásokat. |
| [System Center Operations Manager felügyeleti csoport](log-analytics-om-agents.md) |Igen |Riasztásokat, amelyek akkor jönnek létre, az Operations Manager-ügynökök toohello felügyeleticsoport-i, és továbbítja a tooLog elemzés.<br><br>Közvetlen kapcsolat az Operations Manager-ügynökök tooLog Analytics nincs szükség. Riasztási adatokat adattárból hello felügyeleti csoport toohello Naplóelemzési továbbítja. |


### <a name="collection-frequency"></a>A gyűjtés gyakorisága
- Riasztási rekordok olyan elérhető toohello megoldás, amint hello tárház tárolódnak.
- Riasztási adatokat küldi hello Operations Manager felügyeleti csoport tooLog Analytics három percenként.  

## <a name="using-hello-solution"></a>Hello megoldással
Hello Riasztáskezelési megoldás tooyour OMS-munkaterület felvételekor hello **Riasztáskezelési** csempe tooyour OMS irányítópult kerül.  Ez a csempe egy száma és a grafikus ábrázolása a jelenleg aktív riasztás belül hello utolsó 24 órában hello számát jeleníti meg.  Az időtartomány nem módosítható.

![Kezelési riasztási csempe](media/log-analytics-solution-alert-management/tile.png)

Kattintson a hello **Riasztáskezelési** csempe tooopen hello **Riasztáskezelési** irányítópult.  hello irányítópult szerepel a következő táblázat hello hello oszlopa.  Egyes oszlopok hello felső 10 riasztások száma egyeztetésével, hogy az oszlop feltételek hello megadva hatókör és a kívánt időtartományt sorolja fel.  Futtathatja a napló keresési kattintva hello teljes lista biztosító **láthatja az összes** hello oszlop vagy hello oszlop fejlécére kattintva hello alján.

| Oszlop | Leírás |
|:--- |:--- |
| A kritikus riasztások |Minden riasztás kiegészített egy kritikus riasztás neve szerint csoportosítva.  Kattintson egy riasztás neve toorun a napló keresés riasztás összes rekordot ad vissza. |
| Figyelmeztető riasztások |Minden riasztás kiegészített egy figyelmeztető riasztás neve szerint csoportosítva.  Kattintson egy riasztás neve toorun a napló keresés riasztás összes rekordot ad vissza. |
| SCOM aktív riasztások |Az összes riasztás összegyűjtése az Operations Manager bármely állapotú eltérő *lezárva* létrehozott hello riasztás forrás szerint csoportosítva. |
| Az összes aktív riasztás |Minden riasztás kiegészített bármely riasztás neve szerint csoportosítva. Csak tartalmazza az Operations Manager riasztásait bármely állapotú eltérő *lezárva*. |

Görgessen toohello jobb, ha a hello irányítópult tooperform meg több közös lekérdezések felsorolja a [naplófájl-keresési](log-analytics-log-searches.md) riasztási adatok.

![Riasztási kezelési irányítópult](media/log-analytics-solution-alert-management/dashboard.png)


## <a name="log-analytics-records"></a>Log Analytics-rekordok
hello Riasztáskezelési megoldás elemzi a típusú rekordot **riasztási**.  Nagios vagy Zabbix gyűjtött vagy Naplóelemzési által létrehozott riasztásokat nem közvetlenül hello megoldás által gyűjtött.

hello megoldás riasztások importálása a System Center Operations Manager, és mindegyik típussal rendelkező létrehoz egy megfelelő rekordot **riasztási** és az egy SourceSystem **OpsManager**.  Ezeket a rekordokat a következő táblázat hello hello jellemzőkkel rendelkezik:  

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*Riasztás* |
| SourceSystem |*OpsManager* |
| AlertContext |Hello adatelem hello XML-formátumot hozott létre riasztási toobe okozó részleteit. |
| AlertDescription |Hello riasztás részletes leírása. |
| AlertId |Hello riasztás GUID Azonosítóját. |
| AlertName |Hello riasztás nevét. |
| AlertPriority |Hello riasztás prioritását. |
| AlertSeverity |Hello riasztás súlyossági szintje. |
| AlertState |Feloldási állapot legújabb hello riasztás. |
| LastModifiedBy |Hello hello riasztást utoljára módosító felhasználó neve. |
| ManagementGroupName |Hello riasztást ahol hello felügyeleti csoport neve. |
| RepeatCount |Száma hello azonos riasztás generálása óta feloldott objektum azonos figyelt hello a alkalommal. |
| ResolvedBy |Hello riasztás feloldva hello felhasználó neve. Üres, ha hello riasztás még nincs megoldott. |
| SourceDisplayName |Figyelési objektum hello riasztást kiváltó hello tartozó megjelenített név. |
| SourceFullName |Figyelési objektum hello riasztást kiváltó hello teljes neve. |
| TicketId |Hello riasztás, ha a System Center Operations Manager környezet hello integrálva van egy folyamatot, a riasztások a jegyektől hozzárendelése Jegyazonosító megadására.  Üres nem jegy azonosítója hozzá van rendelve. |
| TimeGenerated |Dátum és idő, amely a riasztás hello lett létrehozva. |
| TimeLastModified |Utolsó módosításának dátuma és időpontja hello riasztás. |
| TimeRaised |Dátum és idő hello riasztást hozott létre. |
| TimeResolved |Dátum és idő, amely a riasztás hello lett feloldva. Üres, ha hello riasztás még nincs megoldott. |

## <a name="sample-log-searches"></a>Naplókeresési minták
hello következő táblázat a megoldás által gyűjtött riasztási rekordok minta napló keres: 

| Lekérdezés | Leírás |
|:--- |:--- |
| Típusú riasztás SourceSystem = OpsManager AlertSeverity = TimeRaised hiba = > most már 24 ÓRÁNKÉNT |Elmúlt 24 óra során hello kiadott kritikus riasztások |
| Típusú riasztás AlertSeverity = figyelmeztetés TimeRaised = > most már 24 ÓRÁNKÉNT |Elmúlt 24 óra során hello kiadott figyelmeztető riasztások |
| Típusú riasztás SourceSystem = OpsManager AlertState =! lezárt TimeRaised = > most már 24 ÓRÁS &#124; mérték count() által SourceDisplayName darabszámként |Elmúlt 24 óra során hello kiadott aktív riasztásokkal rendelkező források |
| Típusú riasztás SourceSystem = OpsManager AlertSeverity = TimeRaised hiba = > most már 24 ÓRÁS AlertState! = lezárva |Hello során létrejött elmúlt 24 órában, amelyek még mindig aktív kritikus riasztások |
| Típusú riasztás SourceSystem = OpsManager TimeRaised = > most már 24 ÓRÁS AlertState = bezárása |Riasztások ismétléseik hello elmúlt 24 órában, amely már be van zárva |
| Típusú riasztás SourceSystem = OpsManager TimeRaised = > most - 1 nap &#124; mérték count() által AlertSeverity darabszámként |Során hello súlyosságuk szerint csoportosítva elmúlt 1 napban kiadott riasztások |
| Típusú riasztás SourceSystem = OpsManager TimeRaised = > most - 1 nap &#124; RepeatCount desc rendezése |Során hello száma szerint rendezett elmúlt 1 napban kiadott riasztások |


>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd lekérdezések megelőző hello megváltozna toohello következő:
>
>| Lekérdezés | Leírás |
|:---|:---|
| Riasztás &#124; Ha SourceSystem "OpsManager" és a AlertSeverity == "error" és a TimeRaised == > ago(24h) |Elmúlt 24 óra során hello kiadott kritikus riasztások |
| Riasztás &#124; Ha AlertSeverity "figyelmeztetés" és a TimeRaised == > ago(24h) |Elmúlt 24 óra során hello kiadott figyelmeztető riasztások |
| Riasztás &#124; Ha SourceSystem == "OpsManager" és a AlertState! = "Lezárva" és a TimeRaised > ago(24h) &#124; összesíteni a Count = count() SourceDisplayName által |Elmúlt 24 óra során hello kiadott aktív riasztásokkal rendelkező források |
| Riasztás &#124; Ha SourceSystem "OpsManager" és a AlertSeverity == "error" és a TimeRaised == > ago(24h) és AlertState! = "Lezárva" |Hello során létrejött elmúlt 24 órában, amelyek még mindig aktív kritikus riasztások |
| Riasztás &#124; Ha SourceSystem "OpsManager" és a TimeRaised == > ago(24h) és AlertState == "Lezárva" |Riasztások ismétléseik hello elmúlt 24 órában, amely már be van zárva |
| Riasztás &#124; Ha SourceSystem "OpsManager" és a TimeRaised == > ago(1d) &#124; összesíteni a Count = count() AlertSeverity által |Során hello súlyosságuk szerint csoportosítva elmúlt 1 napban kiadott riasztások |
| Riasztás &#124; Ha SourceSystem "OpsManager" és a TimeRaised == > ago(1d) &#124; Rendezze a RepeatCount desc |Során hello száma szerint rendezett elmúlt 1 napban kiadott riasztások |


## <a name="next-steps"></a>Következő lépések
* A Log Analytics-riasztások létrehozásával kapcsolatos információkért lásd: [Riasztások a Log Analyticsben](log-analytics-alerts.md).

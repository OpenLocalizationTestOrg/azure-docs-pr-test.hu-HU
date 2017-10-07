---
title: "a Naplóelemzési aaaComputer csoportok keresések jelentkezzen |} Microsoft Docs"
description: "A Naplóelemzési számítógépcsoportok lehetővé teszik a tooscope napló keresések tooa adott számítógépek csoportja.  Ez a cikk ismerteti a hello különböző módszereket használhat toocreate számítógépcsoportokat és toouse a napló azokat a keresési."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: bwren
ms.openlocfilehash: 7dafea9829e541f5582a1d855fafb82aa4d94430
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>A Naplóelemzési számítógépcsoportok jelentkezzen keresések

>[!NOTE]
> Ez a cikk leírja hello számítógépcsoportok hello aktuális napló Anayltics lekérdezési nyelv használatával.    Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor számítógépcsoportok eltérően működik.  Megjegyzések hello új query Language szerepelnek ebben a cikkben hello különböző szintaxisát és viselkedését.  


A Naplóelemzési számítógépcsoportok lehetővé teszik tooscope [keresések jelentkezzen](log-analytics-log-searches.md) tooa adott számítógépek csoportja.  Minden egyes csoport fel van töltve, vagy az Ön által meghatározott lekérdezés segítségével számítógépek vagy csoportok különböző forrásokból származó importálásával.  Amikor hello a csoport szerepel egy naplófájl-keresési, hello eredményei korlátozott toorecords megfelelő hello hello számítógépére.

## <a name="creating-a-computer-group"></a>Olyan számítógép-csoport létrehozása
Bármely hello módszerek használatával a következő táblázat hello Naplóelemzési is létrehozhat egy számítógép (csoport).  Az egyes módszerek részletek hello lentebbi szerepelnek. 

| Módszer | Leírás |
|:--- |:--- |
| Naplókeresés |Hozzon létre egy naplófájl-keresés, amely számítógépek listáját adja vissza, és menteni a hello eredményeket a számítógép (csoport). |
| Log Search API |Használjon hello napló keresése API tooprogrammatically hello naplófájl-keresési eredményei alapján számítógépcsoport létrehozásához. |
| Active Directory |Automatikus vizsgálatokat végez a hello csoportba tartozik az Active Directory-tartomány tagja, és hozzon létre egy csoportot a Naplóelemzési minden egyes biztonsági csoport ügynök számítógépek. |
| A WSUS |Automatikusan keresése a WSUS-kiszolgálók és ügyfelek csoportok megcélzása, és hozzon létre egy csoportot a Naplóelemzési minden. |

### <a name="log-search"></a>Naplókeresés
Naplófájl-keresési létre számítógépcsoportok tartalmaz minden Ön által meghatározott keresési lekérdezés által visszaadott hello számítógép.  Ez a lekérdezés futtatása minden alkalommal, amikor hello számítógépcsoport szolgál, hogy a módosítások hello csoport létrehozása óta is megjelenik.

Használja a következő eljárás toocreate számítógépcsoport napló keresési hello.

1. [Hozzon létre egy naplófájl-keresési](log-analytics-log-searches.md) , amely számítógépek listáját adja vissza.  hello keresési kell használatával adja vissza egy meghatározott készletét számítógépek hasonlót **Distinct Computer** vagy **számítógépenként count() mérésére** hello lekérdezésben.  
2. Kattintson a hello **mentése** üdvözlő képernyőt hello tetején gombra.
3. Válassza ki **Igen** túl**lekérdezés mentése számítógépcsoportként**.
4. Írja be a **neve** és egy **kategória** hello csoport.  Ha a keresés hello azonos névvel és kategória már létezik, majd rákérdezéses toooverwrite el vannak azt.  Az azonos név különböző kategóriákba hello több keresések lehet. 

Az alábbiakban menthető számítógépcsoportként keresési példák.

    Computer="Computer1" OR Computer="Computer2" | distinct Computer 
    Computer=*srv* | measure count() by Computer

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md) majd hello következő változtatások készült toohello eljárás toocreate egy új számítógépcsoportot.
>  
> - tartalmaznia kell egy számítógép (csoport) lekérdezés toocreate hello `distinct Computer`.  Az alábbiakban látható egy példa egy lekérdezés toocreate egy számítógép (csoport).<br>`Heartbeat | where Computer contains "srv" `
> - Amikor létrehoz egy új számítógépcsoport, alias hozzáadása toohello nevét kell megadnia.  Hello alias hello számítógép (csoport) egy lekérdezésben használatakor, az alább ismertetett használja.  

### <a name="log-search-api"></a>Naplófájl-keresési API
Naplófájl-keresési API-JÁNAK vannak hello létre számítógépcsoportok, a naplófájl-keresési létrehozott keresések hello azonos.

További információ a hello napló keresése API használatával számítógép csoport létrehozásakor: [Naplóelemzési naplóban számítógépcsoportokat keresési REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Naplóelemzési tooimport Active Directory-csoporttagságok konfigurálásakor elemzi a csoporttagság hello bármely tartományhoz csatlakoztatott számítógép hello OMS-ügynököt.  Olyan számítógép-csoport minden egyes biztonsági csoport az Active Directoryban létrejön Naplóelemzési, és minden számítógép nincs felvéve toohello számítógépcsoportok megfelelő toohello biztonsági csoportok tagjai.  A csoporttagság folyamatosan frissítjük 4 óránként.  

Állítsa be a Naplóelemzési tooimport Active Directory biztonsági csoportokat a hello **számítógépcsoportok** Naplóelemzési menüjében **beállítások**.  Válassza ki **Automation** , majd **importálása az Active Directory-csoporttagságok számítógépekről**.  Nincs szükség további konfigurációra.

![A számítógépcsoportok Active Directoryból](media/log-analytics-computer-groups/configure-activedirectory.png)

Csoportok vitték, hello menü listák hello csoporttagsággal rendelkező számítógépek száma észleli és hello importált-csoportok száma.  Kattinthat, akár a hivatkozások tooreturn hello **ComputerGroup** ezeket az adatokat rögzíti.

### <a name="windows-server-update-service"></a>A Windows Server Update Service
Konfigurálásakor Naplóelemzési tooimport WSUS-csoporttagságok, azokkal a számítógépekkel hello OMS-ügynököt a csoport tagsága célzó hello megvizsgálja.  Használata ügyféloldali célcsoport-kezelési, csatlakoztatott tooOMS, és a csoportok megcélzása WSUS részét számítógépek csoport tagságát importálva van tooLog elemzés. Kiszolgálóoldali használata server ahhoz, hogy hello csoport tagsági információ toobe célcsoport-kezelési, OMS-ügynököt kell telepíteni a WSUS hello hello importált tooOMS.  A csoporttagság folyamatosan frissítjük 4 óránként. 

Állítsa be a Naplóelemzési tooimport Active Directory biztonsági csoportokat a hello **számítógépcsoportok** Naplóelemzési menüjében **beállítások**.  Válassza ki **Active Directory** , majd **importálása az Active Directory-csoporttagságok számítógépekről**.  Nincs szükség további konfigurációra.

![A számítógépcsoportok Active Directoryból](media/log-analytics-computer-groups/configure-wsus.png)

Csoportok vitték, hello menü listák hello csoporttagsággal rendelkező számítógépek száma észleli és hello importált-csoportok száma.  Kattinthat, akár a hivatkozások tooreturn hello **ComputerGroup** ezeket az adatokat rögzíti.

## <a name="managing-computer-groups"></a>Számítógépcsoportok kezelése
Számítógépcsoportokat keresési napló létrehozott megtekintheti vagy a hello napló keresése API hello **számítógépcsoportok** Naplóelemzési menüjében **beállítások**.  Kattintson a hello **x** a hello **eltávolítása** oszlop toodelete hello számítógépcsoport.  Kattintson a hello **tagok megtekintéséhez** , amely a tagot ad vissza egy csoport toorun hello csoport napló keresés ikonra. 

![Mentett számítógépcsoportokat](media/log-analytics-computer-groups/configure-saved.png)

toomodify hello csoport, hozzon létre egy új hello csoport azonos **kategória** és **neve** toooverwrite hello eredeti csoport.

## <a name="using-a-computer-group-in-a-log-search"></a>Számítógépcsoport használata egy napló keresése
A következő szintaxist toorefer tooa számítógépcsoport a naplófájl-keresési hello használhatja.  Adja meg hello **kategória** nem kötelező szükséges, ha azonos nevet különböző kategóriákba hello a számítógépcsoportok. 

    $ComputerGroups[Category: Name]

A keresés futtatásakor hello keresési bármely számítógép csoportok tagjai hello először megoldott.  Ha hello csoport naplófájl-keresési alapul, majd a keresés fut. hello csoport tagjai tooreturn hello hello legfelső szintű napló keresés végrehajtása előtt.

Számítógépcsoportok általában használt hello **IN** záradék a következő hello napló keresése a következő példa hello hasonlóan:

    Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]

>[!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), akkor a számítógép (csoport) egy lekérdezésben való kezelésével a alias függvényében, mint például a következő hello használja:
> 
>  `UpdateSummary | where Computer IN (MyComputerGroup)`

## <a name="computer-group-records"></a>Számítógép rekordcsoportjának
Minden létrehozott Active Directory vagy a WSUS számítógépcsoport-tagság hello OMS tárháza rekord jön létre.  Ezeket a rekordokat típusa lehet **ComputerGroup** és a következő táblázat hello hello jellemzőkkel rendelkezik.  Rekordok nem jönnek létre a napló keresések alapján számítógépcsoportokhoz.

| Tulajdonság | Leírás |
|:--- |:--- |
| Típus |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Computer |Hello tartozó számítógép nevét. |
| Csoport |Hello csoport neve. |
| GroupFullName |Teljes elérési útja toohello csoport hello forrás- és a forrás neve. |
| GroupSource |Forrás-csoport a gyűjtése történt. <br><br>Active Directoryban<br>A WSUS<br>WSUSClientTargeting |
| GroupSourceName |Az összegyűjtött hello forrás, amely hello csoport nevére.  Active Directory szolgáltatásban ez pedig hello tartomány nevét. |
| ManagementGroupName |SCOM-ügynököt hello felügyeleti csoport neve.  Más ügynökök, ez pedig AOI -\<munkaterület azonosítója\> |
| TimeGenerated |Dátum és idő hello számítógép csoport létrehozásakor vagy frissítésekor. |

## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) tooanalyze hello adatokat gyűjteni az adatforrások és megoldásokat.  


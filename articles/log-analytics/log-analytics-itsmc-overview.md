---
title: "Service Management-összekötő az OMS aaaIT |} Microsoft Docs"
description: "Hello IT Service Management-összekötő toocentrally figyelheti és hello OMS ITSM munkaelemek kezeléséhez, és gyorsan hárítson el minden problémát."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 33ed5d432591b836eb41ba982c66c96f22879444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Központi kezelését az informatikai szolgáltatás Management Connector (előzetes verzió) segítségével ITSM munkaelemek

![Informatikai Management-összekötő szimbólum](./media/log-analytics-itsmc/itsmc-symbol.png)

Hello IT Service Management Connector (ITSMC) az OMS szolgáltatáshoz toocentrally monitor használata, és a munkaelemek kezelésére a ITSM termékek vagy szolgáltatások között.

hello IT Service Management-összekötő integrálható a meglévő IT Service Management (ITSM) termékek és szolgáltatások a OMS szolgáltatáshoz.  hello megoldás ITSM termékek vagy szolgáltatások kétirányú integrációt, akkor ha biztosít hello OMS felhasználók egy beállítás toocreate incidensek, a riasztásokat vagy a események ITSM megoldásban. hello összekötő is különféle adatok, például incidensek és változáskérések ITSM megoldásból az OMS szolgáltatáshoz.

Az informatikai szolgáltatás Management-összekötő segítségével:

  - Központilag figyelheti és ITSM termékek vagy szolgáltatások a szervezetben használt munkaelemek kezelésére.
  - Hozzon létre ITSM munkaelemeket (például a riasztást, esemény, incidens) ITSM az OMS-riasztások és a keresési napló.
  - Olvassa el az incidensek és változáskérések az ITSM megoldásból, és vonatkozó naplóadatok Naplóelemzési munkaterület függ.
  - Váratlan és szokatlan eseményeket található, és a megoldásukkal együtt, még mielőtt a végfelhasználók hello hívja, és jelentést őket toohello segélyszolgálat.
  - Munkahelyi elemek adatok importálása a Naplóelemzési és fő teljesítménymutatók (KPI) jelentések létrehozása.  Ezekre a jelentésekre, segítségével azonosíthatja, értékeléséhez és számos fontos elemek, például a kártevő szoftverek assessment működjön.
  - Részletesebben is elemezheti válogatott irányítópultok incidensek megtekintéséhez a változáskérésekhez és az érintett rendszerek.
  - Végezzen hibaelhárítást a gyorsabb egyéb megoldások hello Naplóelemzési munkaterület az adatok.


## <a name="prerequisites"></a>Előfeltételek

tooimport hello ITSM munkaelemek az OMS szolgáltatáshoz, hello megoldás hello IT Service Management-összekötő az hello OMS és hello IT Service Manager termék vagy szolgáltatás, ahonnan importálni hello munkaelemek közötti kapcsolat szükséges.


## <a name="configuration"></a>Konfiguráció

Hozzáadás hello IT Service Management-összekötő megoldás tooyour OMS munkaterület, ismertetett hello eljárással [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).

Informatikai szolgáltatás Management-összekötő csempe hello megoldások katalógusban látható módon:

![összekötő csempe](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Sikeres hozzáadása után megjelenik IT Service Management-összekötő a hello **OMS** > **beállítások** > **csatlakoztatott források.**

![Csatlakoztatott ITSMC](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Alapértelmezés szerint a hello IT Service Management-összekötő frissítése 24 óránként egyszer a hello kapcsolati adatait. toorefresh a kapcsolat azonnal az esetleges módosításokat, vagy a sablon adatfrissítések, amelyek miatt, kattintson a hello frissítési gomb látható következő tooyour kapcsolat.

 ![ITSMC frissítése](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Felügyeleti csomagok
Ez a megoldás nem igényel minden olyan felügyeleti csomagot.

## <a name="connected-sources"></a>Összekapcsolt források

hello következő ITSM termékek és szolgáltatások által támogatott IT Service Management-összekötő hello:

- [A System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [A ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-hello-solution"></a>Hello megoldással

Hello OMS IT Service Management-összekötő csatlakozott a ITSM szolgáltatással, miután a hello Naplóelemzési szolgáltatások hello adatgyűjtés csatlakoztatott hello ITSM termékek/szolgáltatás elindul.

> [!NOTE]
> - Log Analytics nevű eseményként is megjelenik megoldás IT Service Management-összekötő által importált adatok **ServiceDesk_CL**.
- Esemény nevű mezőt tartalmaz **ServiceDeskWorkItemType_s**. amely incidens, mint az értékét vagy változáskérés is magával attól függően, hogy hello munkaelem hello szereplő adatok **ServiceDesk_CL** esemény.

## <a name="input-data"></a>A bemeneti adatok
A munkaelemek hello ITSM termékek vagy szolgáltatások importálására.

hello alábbi információkat jeleníti meg hello IT Service Management-összekötő által összegyűjtött adatok:

> [!NOTE]
> Attól függően, hogy hello munkaelem típusa Log Analyticshez importálni **ServiceDesk_CL** hello a következő mezőket tartalmazza:

**Munkaelem:** **incidensek**  
ServiceDeskWorkItemType_s = "Esemény"

**Mezők**

- ServiceDeskConnectionName
- Szolgáltatás ügyfélszolgálati azonosítója
- Állapot
- Sürgős
- Gyakorolt hatás
- Prioritás
- Eszkalációs
- Hozta létre
- Megoldó
- Lezárt
- Forrás
- Rendelt
- Kategória
- Cím
- Leírás
- Létrehozás dátuma
- Lezárás dátuma
- Megoldás dátuma
- Utolsó módosítás dátuma
- Computer


**Munkaelem:** **Változáskérések**

ServiceDeskWorkItemType_s = "módosítási kérés"

**Mezők**
- ServiceDeskConnectionName
- Szolgáltatás ügyfélszolgálati azonosítója
- Hozta létre
- Lezárt
- Forrás
- Rendelt
- Cím
- Típus
- Kategória
- Állapot
- Eszkalációs
- Ütközés állapota
- Sürgős
- Prioritás
- Kockázati
- Gyakorolt hatás
- Rendelt
- Létrehozás dátuma
- Lezárás dátuma
- Utolsó módosítás dátuma
- Kért dátuma
- Tervezett kezdő dátuma
- Tervezett befejezési dátum
- Munkahelyi kezdő dátuma
- Munka befejezési dátum
- Leírás
- Computer

## <a name="output-data-for-a-servicenow-incident"></a>A ServiceNow, amelyhez a kimeneti adatok

| OMS mező | ITSM mező |
|:--- |:--- |
| ServiceDeskId_s| Szám |
| IncidentState_s | Állapot |
| Urgency_s |Sürgős |
| Impact_s |Gyakorolt hatás|
| Priority_s | Prioritás |
| CreatedBy_s | Által megnyitott |
| ResolvedBy_s | Megoldó|
| ClosedBy_s  | Lezárt |
| Source_s| Kapcsolat típusa |
| AssignedTo_s | Hozzárendelt túl |
| Category_s | Kategória |
| Title_s|  Rövid leírás |
| Description_s|  Megjegyzések |
| CreatedDate_t|  Megnyitása |
| ClosedDate_t| Lezárt|
| ResolvedDate_t|Feloldva|
| Computer  | konfigurációs elem |

## <a name="output-data-for-a-servicenow-change-request"></a>A ServiceNow kimeneti adatok változáskérés

| OMS mező | ITSM mező |
|:--- |:--- |
| ServiceDeskId_s| Szám |
| CreatedBy_s | Által kért |
| ClosedBy_s | Lezárt |
| AssignedTo_s | Hozzárendelt túl |
| Title_s|  Rövid leírás |
| Type_s|  Típus |
| Category_s|  Catgory |
| CRState_s|  Állapot|
| Urgency_s|  Sürgős |
| Priority_s| Prioritás|
| Risk_s| Kockázati|
| Impact_s| Gyakorolt hatás|
| RequestedDate_t  | A kért dátum szerint |
| ClosedDate_t | Lezárás dátuma |
| PlannedStartDate_t  |     Tervezett kezdő dátuma |
| PlannedEndDate_t  |   Tervezett befejezési dátum |
| WorkStartDate_t  | Tényleges kezdési dátuma |
| WorkEndDate_t | Tényleges befejezési dátum|
| Description_s | Leírás |
| Computer  | Konfigurációs elem |

**A minta Naplóelemzési képernyő ITSM adatokat:**

![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>Informatikai szolgáltatás Management-összekötő – integrálva más OMS-megoldásokkal

Informatikai szolgáltatás Management-összekötő jelenleg hello Szolgáltatástérkép megoldással való integráció.

Szolgáltatástérkép automatikus felderítésére szolgál az alkalmazás-összetevők a Windows hello- és Linux rendszerek és a maps hello szolgáltatások közötti kommunikáció. Ez lehetővé teszi, hogy a kiszolgálók gondolunk, hogy –, hogy a kritikus szolgáltatások összekapcsolt rendszerként tooview. Szolgáltatástérkép jeleníti meg a kiszolgálók, a folyamatok közötti kapcsolatokat, és portok között bármely TCP-csatlakoztatott architektúra a konfiguráció nem szükséges másik ügynököt telepíteni. További információ: [Szolgáltatástérkép](../operations-management-suite/operations-management-suite-service-map.md).

Ez az integráció létrehozott hello ITSM megoldásokban, ahogy az alábbi példa hello hello szolgáltatás ügyfélszolgálati elemek tekintheti meg:

![Az integrált megoldást ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Az OMS-értesítések ITSM munkaelemek létrehozása

Hello OMS riasztások létrehozhat társított munkaelemekhez kapcsolódó hello ITSM források.  toodo a, a következő eljárás használata hello:

1. A **naplófájl-keresési** ablakban futtassa egy keresési lekérdezés tooview naplóadatokat. Lekérdezés eredményei munkaelemek hello forrása.
2. A **naplófájl-keresési**, kattintson a **riasztási** tooopen hello **riasztási szabály hozzáadása** lap.

    ![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. A hello **riasztási szabály hozzáadása** ablakban adja meg a szükséges hello adatait **neve**, **súlyossági**, **keresési lekérdezés**, és  **Riasztási feltételek** (ablakban/Időmetrika mérési).
4. Válassza ki **Igen** a **ITSM műveletek**.
5. Válassza ki a ITSM kapcsolatot a hello **válassza ki a kapcsolat** listája.
6. Adja meg szükség szerint hello adatait.
7. az egyes, a riasztások területen válassza hello naplóbejegyzések külön munkaelem toocreate **minden naplóbejegyzés egyes munkaelemek létrehozása** jelölőnégyzetet.

    Vagy

    a jelölőnégyzet nem kijelölt toocreate csak egy munkaelem naplóbejegyzések ebben a riasztásban vonta tetszőleges számú hagyja.

7. Kattintson a **Save** (Mentés) gombra.

hello OMS riasztás jön létre a **riasztások**. hello megfelelő ITSM kapcsolat munkahelyi elemek jönnek létre, ha hello megadott riasztás feltétel teljesül.

## <a name="create-itsm-work-items-from-oms-logs"></a>Az OMS-naplók ITSM munkaelemek létrehozása

A csatlakoztatott hello ITSM forrásokban OMS napló keresési segítségével létrehozható munkaelemek. toodo a, a következő eljárás használata hello:

1. A **naplófájl-keresési**, hello szükséges adatokat, válassza ki a hello részletes, és kattintson **létrehozás munkaelem**.

    Hello **ITSM munkaelem létrehozása** ablak jelenik meg:

    ![Napló elemzési képernyő](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Adja hozzá a következő adatok hello:

  - **Munkaelem-cím**: hello munkaelem címét.
  - **Munkaelem-leírás**: hello új munkaelem leírását.
  - **Érintett számítógép**: Ha a napló adatokat talált hello számítógép nevét.
  - **Válassza ki a kapcsolat**: ITSM kapcsolat használni kívánt toocreate ezt a munkaelemet.
  - **Munkaelem**: típusú munkaelemet.

3. az incidensek egy meglévő munkaelemsablonból toouse kattintson **Igen** alatt **Generate munkaelem hello sablon alapján** lehetőséget, majd kattintson a **létrehozása**.

    Vagy

    Kattintson a **nem** Ha azt szeretné, tooprovide a szabott értékekhez.

4. Adja meg a megfelelő értékeket hello hello **ügyfél típusú**, **hatás**, **sürgősség**, **kategória**, és **Sub kategória**  szövegmezőbe, és kattintson **létrehozása**.

hello munkaelem hello ITSM, amelyen megtekintheti a OMS létrejön.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Az OMS ITSM kapcsolatok hibáinak elhárítása
1.  Ha a csatlakoztatott adatforrás felhasználói Felületről való kapcsolódás sikertelen, és elérhetővé hello **hiba történt a kapcsolat mentése** üzenet, a következő hello:
 - A ServiceNow, Cherwell és Provance kapcsolatok esetén győződjön meg arról, helytelenül beírt hello felhasználónév/jelszó és az ügyfél azonosítója/titkos ügyfélkulcsot az egyes hello kapcsolatok. Ha hello hiba továbbra is fennáll, ellenőrizheti a hello megfelelő ITSM termék toomake hello kapcsolatban, hogy megfelelő jogosultságokkal.
 - Esetén Service Manager alkalmazást, győződjön meg arról, hogy hello webalkalmazás telepítése sikeres volt, és a hibrid kapcsolat jön létre. tooverify hello kapcsolat sikeresen létrejött hello helyszíni Service Manager géppel, látogasson el a hello webes alkalmazás URL-címet adja meg a megfelelő hello hello dokumentációjának részletes [a hibrid kapcsolat](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Ha a ServiceNow adatait van nem történt-e szinkronizálva az OMS, győződjön meg arról, hello ServiceNow példánynak nem alvó állapotban van. Ez egy kis ideig fordulhat elő, a hello ServiceNow fejlesztői esetben, ha inaktív. Más, a jelentés hello probléma.
3.  Ha a riasztások elindulása esetén első az OMS Szolgáltatáshoz, azonban munkaelemek első létrehozása ITSM termék vagy a konfigurációs elemek nem kapják meg a létrehozott/kapcsolódó toowork elemek vagy bármely általános információkat, hello a következő:
 -  Az OMS-portálon IT Service Management-összekötő megoldás lehet használt tooget, kapcsolatok munkahelyi elemek/számítógépek stb. Hello hibaüzenet hello állapot paneljén kattintson, keresse meg a túl**naplófájl-keresési** és hello kapcsolat hello hibát hello részletei használatával a hello hibaüzenet megtekintéséhez.
 - közvetlenül megtekintheti hello hibák/kapcsolatos információkat a hello **naplófájl-keresési** használatával lapon *típus = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>A Service Manager webes alkalmazás telepítésének hibaelhárítása
1.  Esetén a webes alkalmazás központi telepítési problémák léptek fel, ellenőrizze, hogy megfelelő engedélyekkel hello előfizetésben említett toocreate/telepítés erőforrásokat.
2.  Ha **az objektumhivatkozás nincs beállítva az objektum tooinstance** hello futtatása során hibaüzenet jelenik meg [parancsfájl](log-analytics-itsmc-service-manager-script.md) győződjön meg arról, hogy az érvényes értékek **felhasználói konfiguráció**szakasz.
3.  Ha nem toocreate service bus relay-névtér, győződjön meg arról, adott hello igényel hello előfizetés erőforrás-szolgáltató regisztrálva van. Ha nincs regisztrálva, manuálisan hozza létre a hello Azure-portálon. Akkor is létrehozható közben [hello hibrid kapcsolat létrehozása](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) a hello Azure-portálon.


## <a name="contact-us"></a>Kapcsolat

A lekérdezést vagy hello IT Service Management-összekötő visszajelzést, lépjen velünk kapcsolatba [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Következő lépések
[Adja hozzá a ITSM termékek vagy szolgáltatások tooIT Management-összekötő](log-analytics-itsmc-connections.md).

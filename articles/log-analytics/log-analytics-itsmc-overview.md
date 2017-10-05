---
title: "Informatikai Management-összekötő az OMS szolgáltatással |} Microsoft Docs"
description: "Az informatikai szolgáltatás Management-összekötő használatával központilag figyelheti és az OMS ITSM munkaelemek kezeléséhez, és gyorsan hárítson el minden problémát."
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
ms.openlocfilehash: 54974ef06efdae69ddbfa12b1ba9278b48b113d3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Központi kezelését az informatikai szolgáltatás Management Connector (előzetes verzió) segítségével ITSM munkaelemek

![Informatikai Management-összekötő szimbólum](./media/log-analytics-itsmc/itsmc-symbol.png)

Segítségével az informatikai szolgáltatás Management Connector (ITSMC) az OMS szolgáltatáshoz központilag figyelheti és munkaelemek kezelésére a ITSM termékek vagy szolgáltatások között.

Az informatikai szolgáltatás Management-összekötő integrálható a meglévő IT Service Management (ITSM) termékek és szolgáltatások a OMS szolgáltatáshoz.  A megoldás rendelkezik kétirányú integrációt ITSM termékek vagy szolgáltatások, ahol azt az OMS-felhasználók lehetőséget biztosít az incidensek, a riasztásokat vagy a események létrehozása ITSM megoldásban. Az összekötő is különféle adatok, például incidensek és változáskérések ITSM megoldásból az OMS szolgáltatáshoz.

Az informatikai szolgáltatás Management-összekötő segítségével:

  - Központilag figyelheti és ITSM termékek vagy szolgáltatások a szervezetben használt munkaelemek kezelésére.
  - Hozzon létre ITSM munkaelemeket (például a riasztást, esemény, incidens) ITSM az OMS-riasztások és a keresési napló.
  - Olvassa el az incidensek és változáskérések az ITSM megoldásból, és vonatkozó naplóadatok Naplóelemzési munkaterület függ.
  - Váratlan és szokatlan eseményeket található, és a megoldásukkal együtt, még mielőtt a végfelhasználók hívja, és jelentse őket a támogató szolgálathoz.
  - Munkahelyi elemek adatok importálása a Naplóelemzési és fő teljesítménymutatók (KPI) jelentések létrehozása.  Ezekre a jelentésekre, segítségével azonosíthatja, értékeléséhez és számos fontos elemek, például a kártevő szoftverek assessment működjön.
  - Részletesebben is elemezheti válogatott irányítópultok incidensek megtekintéséhez a változáskérésekhez és az érintett rendszerek.
  - Végezzen hibaelhárítást a gyorsabb egyéb megoldások a Naplóelemzési munkaterület az adatok.


## <a name="prerequisites"></a>Előfeltételek

A ITSM munkaelemek importálása az OMS szolgáltatáshoz, a megoldás az informatikai szolgáltatás Management-összekötő az OMS a és a informatikai SM termék vagy szolgáltatás, amelyből importál a munkaelemek közötti kapcsolat szükséges.


## <a name="configuration"></a>Konfiguráció

Az informatikai szolgáltatás Management-összekötő megoldás felvétele a OMS munkahelyi tárhelyre, ismertetett eljárással [hozzáadni a Naplóelemzési megoldások a megoldások gyűjteményből](log-analytics-add-solutions.md).

Informatikai szolgáltatás Management-összekötő csempére a megoldások katalógusban látható módon:

![összekötő csempe](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Sikeres hozzáadása után az IT Service Management-összekötő alatt megjelenik **OMS** > **beállítások** > **csatlakoztatott források.**

![Csatlakoztatott ITSMC](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Alapértelmezés szerint az IT Service Management-összekötő 24 óránként egyszer a a kapcsolati adatok frissítése. Azonnal az esetleges módosításokat, vagy a Szolgáltatássablon-frissítések, amelyek miatt, a frissítés gombra, a kapcsolat mellett megjelenik a kapcsolati adatok frissítéséhez.

 ![ITSMC frissítése](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Felügyeleti csomagok
Ez a megoldás nem igényel minden olyan felügyeleti csomagot.

## <a name="connected-sources"></a>Összekapcsolt források

A következő ITSM termékek vagy szolgáltatások az informatikai szolgáltatás Management-összekötő által támogatott:

- [A System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [A ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-the-solution"></a>A megoldás használata

Az OMS informatikai Management-összekötő csatlakozott a ITSM szolgáltatással, miután a Naplóelemzési szolgáltatások kezdődik, az adatgyűjtés a csatlakoztatott ITSM termékek/szolgáltatás.

> [!NOTE]
> - Log Analytics nevű eseményként is megjelenik megoldás IT Service Management-összekötő által importált adatok **ServiceDesk_CL**.
- Esemény nevű mezőt tartalmaz **ServiceDeskWorkItemType_s**. amely a számot, incidens, vagy változáskérés, attól függően, hogy a munka elem szereplő adatok a **ServiceDesk_CL** esemény.

## <a name="input-data"></a>A bemeneti adatok
A munkaelemek ITSM termékek vagy szolgáltatások importálására.

A következő információkat az informatikai szolgáltatás Management-összekötő által összegyűjtött adatok láthatók:

> [!NOTE]
> Attól függően, hogy a munkaelem-típusban importálni a Log Analyticshez **ServiceDesk_CL** a következő mezőket tartalmazza:

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
| AssignedTo_s | Rendelt  |
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
| AssignedTo_s | Rendelt  |
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

Informatikai szolgáltatás Management-összekötő jelenleg a Service Map megoldással való integráció.

Szolgáltatástérkép automatikusan észleli az alkalmazás-összetevők, a Windows és Linux rendszerek, és leképezi a szolgáltatások közötti kommunikáció. Lehetővé teszi a kiszolgálók, úgy gondolja, hogy azok –, hogy a kritikus szolgáltatások összekapcsolt rendszerként. Szolgáltatástérkép jeleníti meg a kiszolgálók, a folyamatok közötti kapcsolatokat, és portok között bármely TCP-csatlakoztatott architektúra a konfiguráció nem szükséges másik ügynököt telepíteni. További információ: [Szolgáltatástérkép](../operations-management-suite/operations-management-suite-service-map.md).

Ez az integráció tekintheti meg a szolgáltatás ügyfélszolgálati létrehozott elemek szerepelnek a ITSM megoldások a következő példában látható módon:

![Az integrált megoldást ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Az OMS-értesítések ITSM munkaelemek létrehozása

Az OMS riasztásokhoz kapcsolódó munkaelemek a csatlakoztatott ITSM forrásokban is létrehozhat.  Ehhez használja az alábbi eljárást:

1. A **naplófájl-keresési** ablakban futtassa a napló keresési lekérdezés adatainak megtekintéséhez. Lekérdezés eredményei munkaelemek forrását.
2. A **naplófájl-keresési**, kattintson a **riasztási** megnyitásához a **riasztási szabály hozzáadása** lap.

    ![Napló elemzési képernyő](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. Az a **riasztási szabály hozzáadása** ablakban adja meg a szükséges adatokat a **neve**, **súlyossági**, **keresési lekérdezés**, és **riasztás feltételek** (ablakban/Időmetrika mérési).
4. Válassza ki **Igen** a **ITSM műveletek**.
5. Válassza ki a ITSM kapcsolatot a **válassza ki a kapcsolat** listája.
6. Adja meg szükség szerint adatait.
7. Minden naplóbejegyzés a riasztás egy külön munkaelemet létrehozásához válassza a **minden naplóbejegyzés egyes munkaelemek létrehozása** jelölőnégyzetet.

    Vagy

    hagyja üresen ezt a jelölőnégyzetet, jelölje ki az ebben a riasztásban vonta naplóbejegyzések tetszőleges számú csak egy munkaelem létrehozásához.

7. Kattintson a **Save** (Mentés) gombra.

Az OMS-riasztás alapján jön létre **riasztások**. A megfelelő ITSM kapcsolat munkaelemek jönnek létre, ha a megadott riasztás feltétel teljesül.

## <a name="create-itsm-work-items-from-oms-logs"></a>Az OMS-naplók ITSM munkaelemek létrehozása

A csatlakoztatott ITSM forrásokban munkaelemek OMS napló keresési használatával hozhat létre. Ehhez használja az alábbi eljárást:

1. A **naplófájl-keresési**, keresse meg a szükséges adatokat, válassza ki a részleteket, és kattintson **létrehozás munkaelem**.

    A **ITSM munkaelem létrehozása** ablak jelenik meg:

    ![Napló elemzési képernyő](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Adja hozzá a következő adatokat:

  - **Munkaelem-cím**: a munkaelem címét.
  - **Munkaelem-leírás**: az új munkaelemhez leírását.
  - **Érintett számítógép**: Ha a napló adatokat talált a számítógép nevét.
  - **Válassza ki a kapcsolat**: ITSM kapcsolat, amelyben szeretné létrehozni ezt a munkaelemet.
  - **Munkaelem**: típusú munkaelemet.

3. Incidens egy meglévő munkaelemsablonból használatához kattintson **Igen** alatt **Generate elemet a sablon alapján** lehetőséget, majd kattintson a **létrehozása**.

    Vagy

    Kattintson a **nem** Ha lehetővé szeretné tenni a szabott értékekhez.

4. Adja meg a megfelelő értékeket a **ügyfél típusú**, **hatás**, **sürgősség**, **kategória**, és **alkategória** szövegmezőbe, és kattintson **létrehozása**.

A munkaelem létrejön a ITSM, amelyen megtekintheti az OMS Szolgáltatáshoz.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Az OMS ITSM kapcsolatok hibáinak elhárítása
1.  Ha a csatlakoztatott adatforrás felhasználói Felületről való kapcsolódás sikertelen, és kapott a **hiba történt a kapcsolat mentése** üzenet, tegye a következőket:
 - A ServiceNow, Cherwell és Provance kapcsolatok esetén győződjön meg arról megfelelően a felhasználónév/jelszó és az ügyfél azonosítója/ügyfélkulcs az egyes kapcsolatok. Ha a probléma továbbra is fennáll, ellenőrizze, ha a megfelelő engedélyekkel rendelkezik a megfelelő ITSM termékben való csatlakozáshoz.
 - Esetén Service Manager alkalmazást, győződjön meg arról, hogy a webalkalmazás telepítése sikeres volt, és a hibrid kapcsolat jön létre. Ellenőrizze, hogy sikeresen létrejött a kapcsolat a helyszíni Service Manager számítógéppel, látogasson el a webes alkalmazás URL-CÍMÉT, hogy dokumentációjában ismertetett módon a [a hibrid kapcsolat](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Ha ServiceNow adatait van nem történt-e szinkronizálva az OMS, győződjön meg arról, hogy a példány nem alszik ServiceNow. Ez egy kis ideig fordulhat elő, a ServiceNow fejlesztői esetekben üresjáratban. Más jelentse a hibát.
3.  Ha riasztások elindulása esetén első az OMS Szolgáltatáshoz, de a munkaelemek nem ITSM első létrehozott termék vagy a konfigurációs elemek nem kapják meg a létrehozott/kapcsolódó munkaelemek vagy bármely általános információkat, tegye a következőket:
 -  Informatikai Management-összekötő megoldás az OMS-portálon is használható a beolvasandó, kapcsolatok munkahelyi elemek/számítógépek stb. A hibaüzenet a állapot paneljén kattintson, keresse meg **naplófájl-keresési** és a kapcsolat, amely rendelkezik a hiba részleteit a hibaüzenet megtekintéséhez.
 - közvetlenül megtekintheti a hibák/kapcsolatos információk a **naplófájl-keresési** használatával lapon *típus = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>A Service Manager webes alkalmazás telepítésének hibaelhárítása
1.  Esetén a webes alkalmazás központi telepítési problémák léptek fel győződjön meg arról, hogy megfelelő engedélyekkel rendelkezik az előfizetés említett erőforrások létrehozása vagy telepítése.
2.  Ha **hivatkozás nincs beállítva egy objektumpéldányra objektum** futtatása során hibaüzenet jelenik meg a [parancsfájl](log-analytics-itsmc-service-manager-script.md) győződjön meg arról, hogy az érvényes értékek **felhasználói konfiguráció** a szakasz.
3.  Ha Ön nem tudja létrehozni a service bus relay-névtér, győződjön meg arról, hogy a szükséges erőforrás-szolgáltató regisztrálva van az előfizetés. Ha nem regisztrált, manuálisan hozza létre az Azure portálról. Akkor is létrehozható közben [a hibrid kapcsolat létrehozása](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) Azure-portálról.


## <a name="contact-us"></a>Kapcsolat

A lekérdezést vagy az informatikai szolgáltatás Management-összekötő visszajelzést, lépjen velünk kapcsolatba [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Következő lépések
[ITSM termékek és szolgáltatások hozzáadása IT Service Management-összekötő](log-analytics-itsmc-connections.md).

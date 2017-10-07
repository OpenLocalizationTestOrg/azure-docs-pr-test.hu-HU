---
title: "aaaCreating riasztások az OMS szolgáltatáshoz |} Microsoft Docs"
description: "Log Analytics riasztások határozza meg az OMS-adattárban lévő fontos adatokat és is proaktív értesítést küldenek, problémák vagy meg kíván hívni műveletek tooattempt toocorrect őket.  Ez a cikk ismerteti, hogyan toocreate riasztási szabály és a részletek hello különféle műveletek tarthat."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 6cfd2a46-b6a2-4f79-a67b-08ce488f9a91
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: bwren
ms.openlocfilehash: 3d035b2426dda9645b19e6c993dc26a2d95a2a78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-alert-rules-in-log-analytics"></a>A Naplóelemzési riasztási szabályok használata
A riasztási szabályok, amelyek automatikusan futnak a napló keresések rendszeres időközönként riasztások jönnek létre.  Akkor hozzon létre egy riasztási rekordot, ha hello eredmények bizonyos feltételeknek.  hello szabály követően automatikusan futtathatja egy vagy több műveletek tooproactively hello riasztás értesíti vagy meg kíván hívni egy másik folyamat.   

Ez a cikk hello folyamatok toocreate ismerteti, és szerkesztheti a riasztási szabályok használatával hello OMS-portálon.  Hello különböző beállításait, valamint hogyan tooimplement szükséges logikát, lásd: [Naplóelemzési ismertetése riasztások](log-analytics-alerts.md).

>[!NOTE]
> Jelenleg létrehozásának vagy hello Azure-portál használatával riasztási szabály módosítása. 

## <a name="create-an-alert-rule"></a>Riasztási szabály létrehozása

hello OMS-portálon riasztási szabály, akkor először hozzon létre egy napló toocreate hello azt jelzi, hogy hello riasztást kell meghívnia keres.  Hello **riasztás** gomb lesz elérhető, létrehozhat és hello riasztási szabály konfigurálása.

>[!NOTE]
> Legfeljebb 250 riasztási szabályok jelenleg az OMS-munkaterület hozható létre. 

1. Hello OMS áttekintése lapon kattintson **naplófájl-keresési**.
2. Hozzon létre egy új naplófájl keresési lekérdezést, vagy válasszon ki egy mentett napló keresést. 
3. Kattintson a **riasztási** hello lap tooopen hello hello tetején **riasztási szabály hozzáadása** képernyő.
4. Konfigurálja a témakörben található információk alapján hello riasztási szabály [riasztási szabályok részletei](#details-of-alert-rules) alatt.
6. Kattintson a **mentése** toocomplete hello riasztási szabályt.  Futtatása azonnal elindul.


## <a name="edit-an-alert-rule"></a>Riasztási szabály szerkesztése
Minden riasztási szabályok listája hello olvashatók be **riasztások** Naplóelemzési menüjében **beállítások**.  

![Riasztások kezelése](./media/log-analytics-alerts/configure.png)

1. A hello OMS konzolon válassza hello **beállítások** csempére.
2. Válassza ki **riasztások**.

Ez a nézet több műveletet is elvégezheti.

* A szabály letiltása kiválasztásával **ki** következő tooit.
* Riasztási szabály szerkesztése tovább tooit a hello ceruza ikonra kattintva.
* Távolítsa el a riasztási szabály hello kattintva **X** következő tooit ikonra. 

## <a name="details-of-alert-rules"></a>Riasztási szabályok részletei
Amikor hoz létre, vagy az OMS-portálon hello riasztási szabály szerkesztése, akivel együttműködik hello **riasztás szabály hozzáadása** vagy **riasztást szabály szerkesztése** lap.  a következő táblák hello hello mezők ezen a képernyőn írja le.

![Riasztási szabály](media/log-analytics-alerts/add-alert-rule.png)

### <a name="alert-information"></a>Riasztási információkat
Ezek a alapbeállításainak megadása hello riasztási szabály és hello riasztásokat hoz létre.

| Tulajdonság | Leírás |
|:--- |:---|
| Név | Egyedi név tooidentify hello riasztási szabályt. Ez a név bármely hello szabály által létrehozott riasztásokat tartalmazza.  |
| Leírás | Hello riasztási szabály opcionális leírása. |
| Súlyosság |Az a szabály által létrehozott riasztások súlyossága. |

### <a name="search-query-and-time-window"></a>Keresési lekérdezés- és ablak
hello keresési lekérdezés- és ablak, amely vissza hello rögzíti, amelyek kiértékelt toodetermine, ha minden riasztást kell létrehozni.

| Tulajdonság | Leírás |
|:--- |:---|
| Keresési lekérdezés | Ez a futtatni kívánt hello lekérdezés.  Ez a lekérdezés által visszaadott hello rekordok használt toodetermine lesz, hogy egy riasztás jön létre.<br><br>Válassza ki **használata aktuális keresési lekérdezés** toouse aktuális lekérdezés hello, vagy válasszon egy meglévő mentett keresés hello listából.  hello lekérdezés szintaxisát hello szövegmezőben, ahol módosíthatja azt szükség valósul meg. |
| Időablak |Megadja a hello időtartomány hello lekérdezéshez.  hello lekérdezés csak azt jelzi, hogy hello ebben a tartományban jöttek létre aktuális időt adja vissza.  Ez bármilyen 5 perc és 24 óra közötti érték lehet.  Nagyobb vagy egyenlő toohello riasztási gyakoriságot kell lennie.  <br><br> Például ha a hello időt ablak van beállítva, too60 perc és hello lekérdezés futtatása: 1:15 előtti csak 12:15 előtti és 1:15 előtti között létrejövő rekordok fog adja vissza. |

Ha a riasztási szabály hello hello időkerete ad meg, hello hello keresési feltételeknek időtartományt a meglévő rekordok száma jelenik meg.  Ez segít meghatározni hello gyakorisága, amely felkínálja hello kívánt eredmények száma.

### <a name="schedule"></a>Ütemezés
Meghatározza, hogy milyen gyakran hello keresési lekérdezés futtatása.

| Tulajdonság | Leírás |
|:--- |:---|
| A riasztások gyakorisága | Itt adhatja meg, milyen gyakran hello lekérdezést kell futtatni. Bármely érték 5 perc és 24 óra közötti lehet. Hello időkerete-nál kisebb egyenlő tooor kell lennie.  Ha hello értéke nagyobb, mint hello időtartományt, azzal alatt elmulasztott rekordok kockáztatja ki.<br><br>Vegye figyelembe például egy olyan időkeretet, 30 perc és 60 perc gyakorisága.  Ha hello lekérdezés futtatása, 1:00, 12:30 és 1:00 PM rekordok adja vissza.  hello hello lekérdezés futna legközelebb 2:00 amikor meghaladná a 1:30 és 2:00 között rögzíti.  1:00 és 1:30 között létrejövő rekordok volna soha nem értékelhető ki. |


### <a name="generate-alert-based-on"></a>Riasztás alapján
Meghatározza a hello hello keresési lekérdezés toodetermine, ha a riasztás hello eredményei alapján kiértékelendő feltételeket kell létrehozni.  Ezen adatok kiválasztott riasztási szabály hello típusától függően eltérőek lesznek.  Kaphat részletek hello a különböző riasztási szabályok típusai [Naplóelemzési ismertetése riasztások](log-analytics-alerts.md).

| Tulajdonság | Leírás |
|:--- |:---|
| Végfelhasználói értesítések letiltása | A riasztási szabály hello tiltási bekapcsolásakor hello szabályhoz tartozó műveleteket egy meghatározott ideig új riasztás létrehozása után le vannak tiltva. hello szabály továbbra is fut, és létrehozza a riasztási rekordok hello feltétel teljesülése esetén. Ez a tooallow toocorrect hello probléma idő duplikált műveletek futtatása nélkül. |

#### <a name="number-of-results-alert-rules"></a>Eredmények riasztási szabályok száma

| Tulajdonság | Leírás |
|:--- |:---|
| Találatok száma |Riasztás jön létre, ha hello lekérdezés által visszaadott rekordok számát hello **nagyobb, mint** vagy **kisebb, mint** hello értéket megadnia.  |

#### <a name="metric-measurement-alert-rules"></a>Metrika mérési riasztási szabályok

| Tulajdonság | Leírás |
|:--- |:---|
| Összesített értékét | A küszöbérték, hogy minden összesített hello eredmények meghaladható toobe megsértése számít. |
| Eseményindító riasztás alapján | a létrehozott egy riasztási toobe megszegése hello száma.  Megadhat **megszegése teljes** megszegése hello eredmények között bármilyen kombinációját beállítása vagy **egymást követő megszegése** , amely megszegése hello toorequire egymást követő minták a kell megjelennie. |

### <a name="actions"></a>Műveletek
A riasztási szabályok mindig létrehoz egy [rekord riasztási](#alert-records) hello küszöbérték elérése esetén.  Is meghatározhat egy vagy több válaszok toobe futtatnia, például egy e-mailek küldéséhez, vagy a runbook indítása.



#### <a name="email-actions"></a>E-mailek műveletek
E-mailek műveletek hello adatokkal hello riasztási tooone vagy további címzettek e-mail küldése.

| Tulajdonság | Leírás |
|:--- |:---|
| E-mailes értesítés |Adja meg **Igen** Ha azt szeretné, hogy egy e-mailek toobe küldött, amikor hello figyelmeztetés jelenik meg. |
| Tárgy |Tulajdonos hello e-mailben.  Üdvözlő e-mail törzsét hello nem módosítható. |
| Címzettek |Címzett e-mail címét.  Ha ad meg egynél több címet, majd külön hello címeket pontosvesszővel (;). |

#### <a name="webhook-actions"></a>Webhookműveletek
Webhookműveletek lehetővé teszik a tooinvoke keresztül egy HTTP POST kérelemben egy külső folyamatban.

| Tulajdonság | Leírás |
|:--- |:---|
| Webhook |Adja meg **Igen** Ha azt szeretné, a webhook toocall hello riasztás aktiválása. |
| Webhook URL-CÍMÉT |hello hello webhook URL-címe |
| Egyéni JSON-adattartalmat tartalmazza |Válassza ezt a beállítást, ha azt szeretné, hogy egy egyéni adattartalom tooreplace hello alapértelmezett adattartalom. |
| Adjon meg egyéni JSON-adattartalmat |hello hello webhook egyéni hasznos.  Lásd az előző szakaszát. |

#### <a name="runbook-actions"></a>Runbook-műveletek
Runbook műveletek az Azure Automationben runbook indítása. 

>[!NOTE]
> A munkaterületen a művelet toobe engedélyezve van a telepített hello Automation-megoldást kell rendelkeznie. 


| Tulajdonság | Leírás |
|:--- |:---|
| Forgatókönyv | Adja meg **Igen** Ha azt szeretné, egy Azure Automation-runbook toostart hello riasztás aktiválása.  |
| Automation-fiók | Megadja a hello forgatókönyvek a kijelölt Automation-fiók.  Ez az hello műveleti fiók, amely toohello munkaterület van kapcsolva. |
| Válassza ki a runbookot | Válassza ki a megjeleníteni kívánt toostart riasztás létrejöttekor hello runbookot. |
| Futtassa a | Válassza ki **Azure** toorun hello runbook hello felhőben.  Válassza ki **hibridfeldolgozó** toorun hello runbook az ügynökön [hibrid forgatókönyv-feldolgozó](../automation/automation-hybrid-runbook-worker.md ) telepítve.  |




## <a name="next-steps"></a>Következő lépések
* Hello telepítése [Riasztáskezelési megoldás](log-analytics-solution-alert-management.md) Naplóelemzési riasztások együtt létrehozott tooanalyze riasztások gyűjtése a System Center Operations Manager (SCOM).
* Tudjon meg többet az [keresések jelentkezzen](log-analytics-log-searches.md) , amely riasztást generál.
* A forgatókönyv a [konfigurálása egy webook](log-analytics-alerts-webhooks.md) a riasztási szabályt.  
* Megtudhatja, hogyan toowrite [az Azure Automation runbookjai](https://azure.microsoft.com/documentation/services/automation) riasztások által azonosított tooremediate problémák.


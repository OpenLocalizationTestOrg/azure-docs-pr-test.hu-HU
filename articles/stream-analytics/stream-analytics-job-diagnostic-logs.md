---
title: "diagnosztikai naplók az Azure Stream Analytics aaaTroubleshoot |} Microsoft Docs"
description: "Ismerje meg, hogyan tooanalyze diagnosztikai naplók Stream Analytics feladatok a Microsoft Azure-ban."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeffstok
ms.openlocfilehash: 600fce73636dd137f8f3a137f1d77b59b4a88801
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-stream-analytics-by-using-diagnostics-logs"></a>Azure Stream Analytics hibaelhárítás diagnosztikai naplók segítségével

Egy Azure Stream Analytics-feladat alkalmanként váratlanul feldolgozása leáll. Fontos toobe képes tootroubleshoot a kind esemény is. hello esemény oka lehet egy nem várt lekérdezés eredménye, kapcsolat toodevices, vagy egy nem várt szolgáltatáskiesés. hello diagnosztikai naplók a Stream Analytics segítségével hello OK problémákat azonosítása, amikor fordulhat elő, és a helyreállítási idő csökkentése.

## <a name="log-types"></a>Napló típusa

A Stream Analytics kétféle típusú naplókat kínál: 
* [Tevékenységi naplóit](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) (always on). Tevékenységi naplóit osszon ki feladatokat végre műveletek betekintést.
* [Diagnosztikai naplók](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) (konfigurálható). Diagnosztikai naplók betekintést gazdagabb mindent, ami egy feladattal történik. Diagnosztikai naplók kezdő hello feladat létrehozásakor, és a záró hello feladat törlésekor. Ezek közé tartozik az események, hello feladat frissítésekor, és futtatása.

> [!NOTE]
> Használhatja a szolgáltatásokat, mint az Azure tárolás, az Azure Event Hubs és az Azure Naplóelemzés tooanalyze nem megfelelő adatokat. A fizetési modell olyan szolgáltatási hello van szó.
>

## <a name="turn-on-diagnostics-logs"></a>Kapcsolja be a diagnosztikai naplók

Diagnosztikai naplók **ki** alapértelmezés szerint. a diagnosztikai naplók, tooturn hajtsa végre ezeket a lépéseket:

1.  Jelentkezzen be Azure-portálon toohello, és válassza a toohello folyamatos átviteli feladat panelen. A **figyelés**, jelölje be **diagnosztikai naplók**.

    ![Panel navigációs toodiagnostics naplók](./media/stream-analytics-job-diagnostic-logs/image1.png)  

2.  Válassza ki **a diagnosztika bekapcsolásához**.

    ![Kapcsolja be a diagnosztikai naplók](./media/stream-analytics-job-diagnostic-logs/image2.png)

3.  A hello **diagnosztikai beállítások** lap, a **állapot**, jelölje be **a**.

    ![A diagnosztikai naplók állapotának módosítása](./media/stream-analytics-job-diagnostic-logs/image3.png)

4.  Hello archiválási cél (tárfiók, az event hubs, Log Analytics), amelyet beállítása. Ezután válassza ki, hogy szeretné-e (a végrehajtási, a szerzői műveletek) toocollect naplók hello kategóriák. 

5.  Hello új diagnosztikai konfiguráció mentéséhez.

hello diagnosztikai konfigurációja körülbelül 10 percig tootake érvénybe lép. Ezt követően hello naplózza a konfigurált hello archiválási cél szereplő start (megtekintheti a hello **diagnosztikai naplók** lap):

![Panel navigációs toodiagnostics naplók - archiválási célok](./media/stream-analytics-job-diagnostic-logs/image4.png)

Diagnosztika konfigurálásával kapcsolatos további információkért lásd: [gyűjtése és diagnosztikai adatokat az Azure-erőforrások felhasználását](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostics-log-categories"></a>A diagnosztikai naplófájlok kategóriák

Jelenleg nem rögzíteni diagnosztikai naplók két csoportja:

* **Szerzői**. Rögzíti a naplóesemények, amelyek a szerzői műveletek kapcsolódó toojob: feladat létrehozása, hozzáadása és törlése bemenetekhez és kimenetekhez, hozzáadása és hello lekérdezés, indítása és leállítása hello feladat frissítése.
* **Végrehajtási**. Feladat végrehajtása során előforduló eseményeket rögzíti:
    * Kapcsolódási hibák
    * Adatok feldolgozása hibák, beleértve:
        * Az eseményeket, amelyek nem felelnek meg a toohello Lekérdezésdefiníció (nem egyező típusú és értékek, hiányzó mezőket, és így tovább)
        * Kifejezés kiértékelése hibák
    * Egyéb kapcsolódó események és hibák

## <a name="diagnostics-logs-schema"></a>Diagnosztikai naplók séma

Összes napló JSON formátumban vannak tárolva. Mindegyik bejegyzés rendelkezik a következő általános karakterláncmezőket hello:

Név | Leírás
------- | -------
time | Időbélyeg (UTC) a hello napló.
resourceId | Hello erőforrás, amely hello művelet azonosítója történt, nagybetűvel. Ez magában foglalja a hello előfizetés-azonosító, hello erőforráscsoport és hello feladat neve. Például   **/SUBSCRIPTIONS/6503D296-DAC1-4449-9B03-609A1F4A1C87/RESOURCEGROUPS/MY-RESOURCE-GROUP/PROVIDERS/MICROSOFT. STREAMANALYTICS/STREAMINGJOBS/MYSTREAMINGJOB**.
category | Kategória, vagy jelentkezzen **végrehajtási** vagy **szerzői műveletek**.
operationName | Bejelentkezve hello művelet neve. Például **események küldése: SQL kimeneti írási hiba toomysqloutput**.
status | Hello művelet állapotát. Például **sikertelen** vagy **sikeres**.
szint | Naplózási szint. Például **hiba**, **figyelmeztetés**, vagy **tájékoztató**.
properties | Napló bejegyzés-specifikus részletei, szerializálható kézjegyként JSON karakterláncnak. További információkért tekintse meg a következő szakaszok hello.

### <a name="execution-log-properties-schema"></a>Végrehajtási tulajdonságok séma

A feladatvégrehajtási naplók rendelkezik eseményekről, amelyek a Stream Analytics-feladat végrehajtása során történt. hello séma tulajdonságok attól függően változik, az esemény hello típusa. Jelenleg a következő végrehajtási naplók típusait hello van:

### <a name="data-errors"></a>Adatok hibák

Bármely hello feladat adatainak feldolgozása során előforduló hiba van a kategória a naplók. Ezek a naplók leggyakrabban, beolvasott adatok szerializálása, során jönnek létre, és írási műveletek. Ezek a naplók nem tartalmaznak kapcsolódási hibák. Kapcsolódási hibák általános események tekintendők.

Név | Leírás
------- | -------
Forrás | Hello feladat neve bemeneti vagy kimeneti, ahol hello hiba történt.
Üzenet | Az üzenet társított hello hiba.
Típus | Hiba típusa. Például **DataConversionError**, **CsvParserError**, vagy **ServiceBusPropertyColumnMissingError**.
Adatok | Tartalmazza a hasznos adatok tooaccurately keresse meg a hello hello hiba forrását. Tárgy tootruncation méretétől függően.

Attól függően, hogy hello **operationName** értéknek, adatok hibák olyan hello séma a következő:
* **Események szerializálni**. Szerializálható esemény olvasási műveletek során bekövetkező események. Jelentkeznek, amikor hello hello bemeneti adatok nem elégíti ki hello lekérdezés séma az alábbi okok valamelyike miatt:
    * *Típuseltérés (de) esemény során szerializálni*: hello mező hello hibát okozó azonosítja.
    * *Nem olvasható be az esemény, érvénytelen szerializálási*: hello hely hello bemeneti adatok hol hello hiba történt a információt tartalmaz. A blob bemeneti, eltolás és egy minta hello adatok blob neve tartalmazza.
* **Események küldése**. Az írási műveletek során bekövetkező események küldése Adatfolyam-esemény hello hibát okozó hello azonosításának.

### <a name="generic-events"></a>Általános események

Általános események fedik le minden más.

Név | Leírás
-------- | --------
Hiba | (választható) Hiba adatok. Általában ami kivételek adatai, amennyiben az rendelkezésre áll.
Üzenet| A fenti üzenet jelenik meg.
Típus | Üzenet típusa. A Maps toointernal kategorizálási hibák. Például **JobValidationError** vagy **BlobOutputAdapterInitializationFailure**.
Korrelációs azonosító | [GUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) , amely egyedileg azonosítja hello feladat végrehajtása. Minden végrehajtási naplóbejegyzése hello idő hello feladat indítása, amíg hello feladat leáll rendelkezik hello azonos **korrelációs azonosító** érték.

## <a name="next-steps"></a>Következő lépések

* [Bevezetés tooStream elemzés](stream-analytics-introduction.md)
* [A Stream Analytics használatába](stream-analytics-real-time-fraud-detection.md)
* [Stream Analytics-feladatok méretezése](stream-analytics-scale-jobs.md)
* [Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [A Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)

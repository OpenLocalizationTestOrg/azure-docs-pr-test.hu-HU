---
title: "Az Azure Stream Analytics-eszközök használata a Visual Studio |} Microsoft Docs"
description: "Első lépések útmutató az Azure Stream Analytics Tools for Visual Studio"
keywords: A Visual studio
documentationcenter: 
services: stream-analytics
author: 
manager: 
editor: 
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 
ms.author: 
ms.openlocfilehash: 618c1055795a75e0ed71dacddba3e076f81f4946
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Az Azure Stream Analytics-eszközök használata a Visual Studio
## <a name="introduction"></a>Bevezetés
Ebben az oktatóanyagban elsajátíthatja létrehozására, szerzői, helyi tesztelése, kezelésére és a Stream Analytics-feladatok debug Azure Stream Analytics Tools for Visual Studio használatával. 

Ez az oktatóanyag befejezése után fogja tudni:
* Ismerkedjen meg Stream Analytics Tools for Visual Studio.
* Konfigurálhatja és telepítheti a Stream Analytics-feladat.
* A tesztfeladat helyileg helyi mintaadatokkal.
* A probléma megoldásához használja a figyelés.
* Projektek exportálni a meglévő feladatokat.

## <a name="prerequisites"></a>Előfeltételek
Az oktatóanyag teljesítéséhez a következő előfeltételekre lesz szüksége:
* A lépéseket, amelyek előtt "Stream Analytics-feladat létrehozása" fejeződött be a [hozhat létre egy IoT-megoldás a Stream Analytics-oktatóprogram](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Használja a Visual Studio 2015, Visual Studio 2013 update 4 vagy Visual Studio 2012. Enterprise (Ultimate/prémium), Professional és Community Edition kiadásai támogatottak. Express kiadás nem támogatott. A Visual Studio 2017 nem támogatott. 
* Használja az Azure SDK for .NET 2.7.1-es verzió vagy újabb. Telepítse a [Webplatform-telepítővel](http://www.microsoft.com/web/downloads/platform.aspx).
* Telepítse a [Stream Analytics eszközök Visual Studio](http://aka.ms/asatoolsvs).

## <a name="create-a-stream-analytics-project"></a>A Stream Analytics-projekt létrehozása
1. A Visual Studióban, kattintson a **fájl** menüre, majd válassza **új projekt**. 

2. A bal oldali sablonok listáján válassza ki a **Stream Analytics** majd **Azure Stream Analytics alkalmazás**.

3. Adja meg a projekt **neve**, **hely**, és **megoldásnév** más projektek a.

    ![Új projekt ablakról](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    A **téren** projekt jön létre a **Megoldáskezelőben**.

    ![A Solution Explorer generált téren projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-the-correct-subscription"></a>Válassza ki a megfelelő előfizetés
1. A Visual Studióban, kattintson a **nézet** menüre, és nyissa meg **Server Explorer**.

2. Jelentkezzen be az Azure-fiókjával. 

## <a name="define-the-input-sources"></a>A bemeneti forrás megadása
1.  A **Megoldáskezelőben**, bontsa ki a **bemenetek** csomópont, és nevezze át **Input.json** való **EntryStream.json**. Kattintson duplán a **EntryStream.json**.
2.  A **bemeneti Alias** most **EntryStream**. A bemeneti alias a lekérdezés parancsfájl használatban van. 
3.  A **forrástípus**, jelölje be **adatfolyam**.
4.  A **forrás**, jelölje be **Eseményközpont**.
5.  A **Service Bus Namespace**, jelölje be a **TollData** lehetőséget.
6.  A **Eseményközpont neveként**, jelölje be **bejegyzés**.
7.  A **Eseményközpont házirend neve**, jelölje be **RootManageSharedAccessKey** (az alapértelmezett érték).
8.  A **esemény szerializálási formátum**, jelölje be **Json**. 
9.  A **kódolás**, jelölje be **UTF8**. A beállítások az alábbi képernyőfelvételen hasonlóan kell kinéznie:

    ![Bemeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. A varázsló befejezéséhez kattintson a **mentése**. Most már egy másik bemeneti forrás a kilépési adatfolyam létrehozására is hozzáadhat. Kattintson a jobb gombbal a **bemenetek** csomópontját, és válassza **új elem**.

    ![Új elem](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. A ablakban válassza ki **Azure Stream Analytics bemeneti**, és módosítsa a **neve** való **ExitStream.json**. Kattintson az **Add** (Hozzáadás) parancsra.

    ![Új elem ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. Kattintson duplán a **ExitStream.json** a projektet, és kövesse a lépéseket, ha Ön a bejegyzés adatfolyam esetében tette. Ügyeljen arra, hogy adja meg **kilépéshez** a a **Eseményközpont neveként** az alábbi képernyőfelvételen látható módon:

    ![ExitStream ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    Most már definiált két bemeneti adatfolyamok:

    ![Be- és kilépés bemeneti adatfolyamok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    Ezután adja hozzá a car regisztrációs adatokat tartalmazó blob elérési referenciaadat-bemenetek.

13. Kattintson a jobb gombbal a **bemenetek** csomópontja a projektet, és a ugyanaz az adatfolyam bemenetek hasonló módon lépéseit kövesse. A **bemeneti Alias**, adja meg **regisztrációs**, majd a **forrástípus**, jelölje be **referenciaadatok**.

    ![Regisztrációs ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. A **Tárfiók**, jelölje be a **tolldata** lehetőséget. A **tároló**, jelölje be **tolldata**, majd a **elérési út mintája**, adja meg **registration.json**. A fájlnév nagybetűk különbözőnek számítanak, és kisbetűnek kell lennie.
15. A varázsló befejezéséhez kattintson a **mentése**.

Most már a bemenetek vannak definiálva.

## <a name="define-the-output"></a>Adja meg a kimeneti
1.  A **Megoldáskezelőben**, bontsa ki a **bemenetek** csomópontot, és kattintson duplán **Output.json**.

2.  A **kimeneti Alias**, adja meg **kimeneti**. 
3.  A **gyűjtése**, jelölje be **SQL-adatbázis**.
4.  A **adatbázis**, jelölje be **TollDataDB**.
5.  A **felhasználónév**, adja meg **tolladmin**. 
6.  A **jelszó**, adja meg **123toll!**.
7.  A **tábla**, adja meg **TollDataRefJoin**.
8.  Kattintson a **Save** (Mentés) gombra.

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a>A Stream Analytics-lekérdezés létrehozása
Ez az oktatóanyag megkísérli kérdésekre több üzleti adatok toll kapcsolódó. Létrehozza a megfelelő lerövidíti a Stream Analytics használható Stream Analytics-lekérdezéseket is.
Az első Stream Analytics-feladat indítása előtt most megismerkedhet egy egyszerű forgatókönyv és a lekérdezés szintaxisa.

### <a name="introduction-to-the-stream-analytics-query-language"></a>A Stream Analytics lekérdezési nyelv bemutatása
Tegyük fel, hogy szeretné-e, adjon meg egy téren kiállítási gépjárműveket száma. Mivel ez a példa állandó folyama miatt események, meg kell adnia egy adott időn belül. Módosítsa a kérdés, hogy "hogyan különböző adja meg egy téren kiállítási három percenként?" Ezzel a módszerrel adatok száma gyakran nevezik az átfedésmentes száma.

A Stream Analytics-lekérdezés, amely a kérdésre választ ad meg:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

A Stream Analytics használ lekérdezésnyelvet, amely például az SQL, és adja meg a lekérdezés idővel kapcsolatos szempontok néhány bővítmények hozzáadása.

További információkért lásd: [Időkezelést](https://msdn.microsoft.com/library/azure/mt582045.aspx) és [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN a lekérdezésben használt szerkezeteket tartalmaz.

Most, hogy az első Stream Analytics lekérdezési írt, tesztelheti, hogy legyen. A mintaadatfájlok a TollApp mappában található a következő elérési utat használja:

.. \TollApp\TollApp\Data

Ez a mappa a következő fájlokat tartalmazza:
*   Entry.JSON
*   Exit.JSON
*   Registration.JSON

## <a name="count-the-number-of-vehicles-entering-a-toll-booth"></a>Az egy téren kiállítási sűrítéses számát
A projektben kattintson duplán a **Script.asaql** a parancsfájl megnyitása a **Lekérdezésszerkesztő**. Másolja és illessze be a parancsfájl az előző szakaszban a programba. A lekérdezés-szerkesztő támogatja az IntelliSense zintaxisszínek és a hiba jelölő.

![Lekérdezés-szerkesztő](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>A Stream Analytics lekérdezések helyileg tesztelése

1. Szintaktikai hiba van a lekérdezés fordítása, kattintson jobb gombbal a projektre, és válassza ki **Build**. 

2. A mintaadatok lekérdezése ellenőrzéséhez használhatja a helyi mintaadatok. Kattintson a jobb gombbal a bemeneti, és válassza ki **adja hozzá a helyi bemeneti**.

    ![Helyi bemenet hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. Az előugró ablakban jelölje ki a mintaadatok a helyi elérési utat. Kattintson a **Save** (Mentés) gombra.

    ![Helyi bemeneti ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    Nevű fájl **local_EntryStream.json** automatikusan hozzáadódik a bemeneti adatok mappába.

    ![Fájlok hozzáadása a bemeneti adatok mappába](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. Az a **Lekérdezésszerkesztő**, kattintson a **futtassa helyileg**. Vagy is nyomja le az F5 billentyűt.

    ![Helyileg történő futtatása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Helyi futtatáskor kimeneti](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    Nyomja le bármelyik billentyűt a eredményének megtekintéséhez a **ASA helyi futtatása eredmény** ablak a Visual Studióban. 

    ![Helyi Futtatás eredmény ASA ablak](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. Kattintson a **eredmény mappa megnyitása a** ellenőrizze, hogy a kimeneti fájlok egyaránt CSV és a JSON formátumban.

    ![Nyissa meg a kimeneti eredmény mappa](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-the-input-data"></a>A minta bemeneti adatok
Minta bemeneti adatok bemeneti forrásból származó helyi fájlba is. 
1. Kattintson a jobb gombbal a bemeneti konfigurációs fájlban, és válassza ki **mintaadatok**. 

   ![Adatmintavétel](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    Egyelőre csak az event hubs vagy az IoT-központ segítségével mintavételi. Más bemeneti forrás nem támogatottak.

2. Az előugró ablakban adja meg a mintaadatokat mentéséhez használt helyi elérési útja. Kattintson a **minta**.

    ![A minta adatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    Megjelenik a folyamatjelző a **kimeneti** ablak. 

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-to-azure"></a>Egy Azure Stream Analytics-lekérdezés elküldése
1. Az a **Lekérdezésszerkesztő**, kattintson a **küldje el az Azure-bA** a parancsfájl-szerkesztőben.

    ![Küldje el az Azure-bA](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Válassza ki **hozzon létre egy új Azure Stream Analytics-feladat**. Adja meg a **feladatnév**, és válassza ki a megfelelő **előfizetés**. Kattintson a **nyújt**.

    ![Küldje el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a>Feladat indítása
Most, hogy a feladat létrehozását követően a feladatok nézetben automatikusan megnyílik. 
1. A feladat elindításához kattintson a **zöld nyílra** gombra.

    ![Feladat indítása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Válassza ki az alapértelmezett beállítást, és kattintson a **Start**.
 
    ![Indítsa el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    A feladat **állapot** vált **futtató**, és **bemeneti események** és **a kimeneti eseményekben** jelennek meg.

    ![A feladat összegzése futási állapota](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-the-results-in-visual-studio"></a>Az eredményeket a Visual Studióban
1. A Visual Studióban nyissa meg a **Server Explorer** , és kattintson a jobb gombbal a **TollDataRefJoin** tábla.
2. Válassza ki **tábla adatok megjelenítése** a feladat eredményének megtekintéséhez.
   
    ![A Server Explorer tábla adatok megjelenítése kiválasztása](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-the-job-metrics"></a>A feladat metrikák megtekintése
Néhány egyszerű feladatot statisztikai található **feladat metrikák**. 

![Feladat metrikák ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-the-job-in-server-explorer"></a>A feladat a Server Explorer felsorolása
A **Server Explorer**, kattintson a **Stream Analytics-feladatok** majd **frissítése**. A feladat jelenik meg az **Stream Analytics-feladatok**.

![A Server Explorer felsorolt Stream Analytics-feladatok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-the-job-view"></a>Nyissa meg a feladat megtekintése
A feladat nézet megnyitásához, bontsa ki a feladatot csomópontját, és kattintson duplán a **feladatok** csomópont.

![Feladatok nézet csomópont](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-to-a-project"></a>A projekt egy meglévő feladat exportálása
A projekt exportálhatja egy meglévő feladat két módja van.

A **Server Explorer**, kattintson a jobb gombbal a projekt csomópontjára a **Stream Analytics-feladatok** csomópont, és válassza **új Stream Analytics projekt exportálása**.

![Új Stream Analytics projekt exportálása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

A projekt létrehozása történik **Megoldáskezelőben**.

![A Solution Explorer létrehozott projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
Ezen kívül a feladatok nézetben, és kattintson a **készítése a projekt**.

![-Projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a>Ismert problémák és korlátozások
 
- A rendszer nem támogatja a Power BI-kimenet és a Azure dátum Lake Store kimenet.
- Nincs nincs hozzáadása vagy cseréje, felhasználó által definiált függvények JavaScript szerkesztő támogatása.

## <a name="next-steps"></a>Következő lépések
* [Az Azure Stream Analytics bemutatása](stream-analytics-introduction.md)
* [Ismerkedés az Azure Stream Analytics segítségével](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

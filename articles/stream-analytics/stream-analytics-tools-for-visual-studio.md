---
title: Azure Stream Analytics Tools for Visual Studio aaaUse |} Microsoft Docs
description: "Első lépések útmutató hello Azure Stream Analytics Tools for Visual Studio"
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
ms.openlocfilehash: bda8e548040509a6f29f1b713268bc38f73228fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-stream-analytics-tools-for-visual-studio"></a>Az Azure Stream Analytics-eszközök használata a Visual Studio
## <a name="introduction"></a>Bevezetés
Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Stream Analytics eszközök Visual Studio toocreate, a szerzői, helyi tesztelése, kezelése és hibakeresése a Stream Analytics-feladatok. 

Ez az oktatóanyag befejezése után fogja tudni:
* Ismerkedjen meg Stream Analytics Tools for Visual Studio.
* Konfigurálhatja és telepítheti a Stream Analytics-feladat.
* A tesztfeladat helyileg helyi mintaadatokkal.
* Figyelési tootroubleshoot problémák használja.
* Meglévő feladatok tooprojects exportálja.

## <a name="prerequisites"></a>Előfeltételek
toocomplete ebben az oktatóanyagban a következő előfeltételek hello szüksége:
* A befejezés előtt "Stream Analytics-feladat létrehozása" hello lépéseket hello [hozhat létre egy IoT-megoldás a Stream Analytics-oktatóprogram](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-build-an-iot-solution-using-stream-analytics). 
* Használja a Visual Studio 2015, Visual Studio 2013 update 4 vagy Visual Studio 2012. Enterprise (Ultimate/prémium), Professional és Community Edition kiadásai támogatottak. Express kiadás nem támogatott. A Visual Studio 2017 nem támogatott. 
* Használjon hello Azure SDK for .NET 2.7.1-es verzió vagy újabb. Telepítheti azt a hello [webplatform-telepítő](http://www.microsoft.com/web/downloads/platform.aspx).
* Telepítse a hello [Stream Analytics Tools for Visual Studio](http://aka.ms/asatoolsvs).

## <a name="create-a-stream-analytics-project"></a>A Stream Analytics-projekt létrehozása
1. A Visual Studióban, kattintson a hello **fájl** menüre, majd válassza **új projekt**. 

2. Hello sablonok hello bal oldali listában jelölje ki **Stream Analytics** majd **Azure Stream Analytics alkalmazás**.

3. Adja meg a projekt hello **neve**, **hely**, és **megoldásnév** más projektek a.

    ![Új projekt ablakról](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-01.png)

    A **téren** projekt jön létre a **Megoldáskezelőben**.

    ![A Solution Explorer generált téren projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-create-project-02.png)

## <a name="choose-hello-correct-subscription"></a>Válassza ki a megfelelő előfizetés hello
1. A Visual Studióban, kattintson a hello **nézet** menüre, és nyissa meg **Server Explorer**.

2. Jelentkezzen be az Azure-fiókjával. 

## <a name="define-hello-input-sources"></a>Hello bemeneti forrás megadása
1.  A **Megoldáskezelőben**, bontsa ki a hello **bemenetek** csomópont, és nevezze át **Input.json** túl**EntryStream.json**. Kattintson duplán a **EntryStream.json**.
2.  Hello **bemeneti Alias** most **EntryStream**. hello bemeneti alias hello lekérdezés parancsfájl használatban van. 
3.  A **forrástípus**, jelölje be **adatfolyam**.
4.  A **forrás**, jelölje be **Eseményközpont**.
5.  A **Service Bus Namespace**, jelölje be hello **TollData** lehetőséget.
6.  A **Eseményközpont neveként**, jelölje be **bejegyzés**.
7.  A **Eseményközpont házirend neve**, jelölje be **RootManageSharedAccessKey** (hello az alapértelmezett érték).
8.  A **esemény szerializálási formátum**, jelölje be **Json**. 
9.  A **kódolás**, jelölje be **UTF8**. A beállítások a következő képernyőkép hello hasonlóan kell kinéznie:

    ![Bemeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-01.png)
 
10. toofinish hello varázsló, kattintson a **mentése**. Most már egy másik bemeneti forrás toocreate hello kilépési adatfolyam is hozzáadhat. Kattintson a jobb gombbal hello **bemenetek** csomópontját, és válassza **új elem**.

    ![Új elem](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-02.png)
 
11. Hello ablakban válassza ki **Azure Stream Analytics bemeneti**, és módosítsa a hello **neve** túl**ExitStream.json**. Kattintson az **Add** (Hozzáadás) parancsra.

    ![Új elem ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-03.png)
 
12. Kattintson duplán a **ExitStream.json** hello projektet, és kövesse hello azonos lépések hello bejegyzés adatfolyamhoz hasonló módon. Lehet, hogy tooenter **kilépéshez** a hello **Eseményközpont neveként** látható módon a következő képernyőkép hello:

    ![ExitStream ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-04.png)

    Most már definiált két bemeneti adatfolyamok:

    ![Be- és kilépés bemeneti adatfolyamok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-05.png)
 
    Ezután adja hozzá a referenciaadat-bemenetek hello blob fájl, amely tartalmazza a car regisztrációs adatokat.

13. Kattintson a jobb gombbal hello **bemenetek** csomópont a hello projektet, majd hajtsa végre hello azonos lépések hello adatfolyam bemenetek hasonló módon. A **bemeneti Alias**, adja meg **regisztrációs**, majd a **forrástípus**, jelölje be **referenciaadatok**.

    ![Regisztrációs ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-input-06.png)

14. A **Tárfiók**, jelölje be hello **tolldata** lehetőséget. A **tároló**, jelölje be **tolldata**, majd a **elérési út mintája**, adja meg **registration.json**. A fájlnév nagybetűk különbözőnek számítanak, és kisbetűnek kell lennie.
15. toofinish hello varázsló, kattintson a **mentése**.

Most már az összes hello bemenet vannak definiálva.

## <a name="define-hello-output"></a>Hello kimeneti meghatározása
1.  A **Megoldáskezelőben**, bontsa ki a hello **bemenetek** csomópontot, és kattintson duplán **Output.json**.

2.  A **kimeneti Alias**, adja meg **kimeneti**. 
3.  A **gyűjtése**, jelölje be **SQL-adatbázis**.
4.  A **adatbázis**, jelölje be **TollDataDB**.
5.  A **felhasználónév**, adja meg **tolladmin**. 
6.  A **jelszó**, adja meg **123toll!**.
7.  A **tábla**, adja meg **TollDataRefJoin**.
8.  Kattintson a **Save** (Mentés) gombra.

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-define-output-01.png)
 
## <a name="create-a-stream-analytics-query"></a>A Stream Analytics-lekérdezés létrehozása
Ez az oktatóanyag megpróbál tooanswer több üzleti kérdéseket, amelyek kapcsolódó tootoll adatokat. Létrehozza a Stream Analytics lekérdezések használható a Stream Analytics tooprovide azokra adott válaszokat is.
Az első Stream Analytics-feladat indítása előtt a következőkben megtudhatja egy egyszerű forgatókönyv és hello lekérdezés szintaxisa.

### <a name="introduction-toohello-stream-analytics-query-language"></a>Bevezetés toohello Stream Analytics lekérdezési nyelv
Tegyük fel, hogy kell-e, adjon meg egy téren kiállítási járművekről gyűjtött toocount hello száma. Mivel ez a példa állandó folyama miatt események, hogy toodefine egy időszakot. "Hány járművekről gyűjtött adja meg egy téren kiállítási három percenként?" hello kérdés toobe módosítása Ez általában az adatok módon toocount említett tooas hello átfedésmentes száma.

Tekintse meg a kérdéshez válaszoló hello Stream Analytics-lekérdezés:

        SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count 
        FROM EntryStream TIMESTAMP BY EntryTime 
        GROUP BY TUMBLINGWINDOW(minute, 3), TollId 

A Stream Analytics használ egy lekérdező nyelv, amely például az SQL és néhány bővítmények toospecify idővel kapcsolatos szempontok hello lekérdezés hozzáadja.

További információkért lásd: [Időkezelést](https://msdn.microsoft.com/library/azure/mt582045.aspx) és [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN hello lekérdezésben használt szerkezeteket.

Most, hogy az első Stream Analytics lekérdezési írt, a rendszer idő tootest azt. Hello mintaadatfájlok a TollApp mappában található a következő elérési út hello használata:

.. \TollApp\TollApp\Data

Ez a mappa tartalmaz a következő fájlok hello:
*   Entry.JSON
*   Exit.JSON
*   Registration.JSON

## <a name="count-hello-number-of-vehicles-entering-a-toll-booth"></a>Megadott számú hello sűrítéses egy téren kiállítási
Hello projektben kattintson duplán a **Script.asaql** tooopen hello parancsfájl hello **Lekérdezésszerkesztő**. Másolja, és beillesztheti az előző szakaszban hello hello parancsfájl hello szerkesztő. hello Lekérdezésszerkesztő támogatja az IntelliSense zintaxisszínek és hello hiba jelölő.

![Lekérdezés-szerkesztő](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-query-01.png)
 
### <a name="test-stream-analytics-queries-locally"></a>A Stream Analytics lekérdezések helyileg tesztelése

1. Kattintson a jobb gombbal a projekt hello toocompile hello lekérdezés toosee szintaktikai hiba esetén, és válassza ki **Build**. 

2. toovalidate Ez a lekérdezés elleni mintaadatok, használhatja a helyi mintaadatok. Kattintson a jobb gombbal a hello bemeneti, és válassza ki **adja hozzá a helyi bemeneti**.

    ![Helyi bemenet hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-01.png)
 
3. Hello előugró ablakban jelölje ki a helyi elérési út hello mintaadatok. Kattintson a **Save** (Mentés) gombra.

    ![Helyi bemeneti ablak hozzáadása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-02.png)
 
    Nevű fájl **local_EntryStream.json** automatikusan fel lesz véve tooyour bemenetek mappát.

    ![A hozzáadott tooinputs fájlmappa](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-add-local-input-03.png)
 
4. A hello **Lekérdezésszerkesztő**, kattintson a **futtassa helyileg**. Vagy nyomja le az F5 billentyűt hello.

    ![Helyileg történő futtatása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-01.png)

    ![Helyi futtatáskor kimeneti](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-local-run-02.png)

    Nyomja le a kulcs tooview hello kimenetet hello **ASA helyi futtatása eredmény** ablak a Visual Studióban. 

    ![Helyi Futtatás eredmény ASA ablak](./media/stream-analytics-tools-for-vs/local-testing-output.png)

5. Kattintson a **eredmény mappa megnyitása a** toocheck hello kimeneti fájlok egyaránt CSV és a JSON formátumban.

    ![Nyissa meg a kimeneti eredmény mappa](./media/stream-analytics-tools-for-vs/local-testing-files.png)
 

### <a name="sample-hello-input-data"></a>A minta hello bemeneti adatok
A minta bemeneti adatok bemeneti forrás tooa helyi fájlból is. 
1. Kattintson a jobb gombbal a hello bemeneti konfigurációs fájlban, és válassza ki **mintaadatok**. 

   ![Adatmintavétel](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-01.png)

    Egyelőre csak az event hubs vagy az IoT-központ segítségével mintavételi. Más bemeneti forrás nem támogatottak.

2. A hello előugró ablak írja be a hello használt helyi elérési utat toosave hello mintaadatok. Kattintson a **minta**.

    ![A minta adatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-02.png)
 
    Hello a folyamat állapotát a hello látható **kimeneti** ablak. 

    ![Kimeneti ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-sample-data-03.png)
 
### <a name="submit-a-stream-analytics-query-tooazure"></a>A Stream Analytics lekérdezési tooAzure elküldése
1. A hello **Lekérdezésszerkesztő**, kattintson a **tooAzure nyújt** hello parancsfájl-szerkesztőben.

    ![Küldje el a tooAzure](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-01.png)
 
2. Válassza ki **hozzon létre egy új Azure Stream Analytics-feladat**. Adja meg a hello **feladat neve**, és jelölje be hello megfelelő **előfizetés**. Kattintson a **nyújt**.

    ![Küldje el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-submit-job-02.png)

 
### <a name="start-a-job"></a>Feladat indítása
Most, hogy a feladat létrehozását követően hello feladat nézet automatikusan megnyílik. 
1. toostart hello feladat, kattintson a hello **zöld nyílra** gombra.

    ![Feladat indítása](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-01.png)
 
2. Válassza ki a hello alapértelmezett beállítást, és kattintson a **Start**.
 
    ![Indítsa el a feladatok ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-02.png)

    hello feladat **állapot** túl változik**futtató**, és **bemeneti események** és **a kimeneti eseményekben** jelennek meg.

    ![A feladat összegzése futási állapota](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-start-job-03.png)

## <a name="check-hello-results-in-visual-studio"></a>A Visual Studio hello eredmények ellenőrzése
1. A Visual Studióban nyissa meg a **Server Explorer** , és kattintson a jobb gombbal hello **TollDataRefJoin** tábla.
2. Válassza ki **tábla adatok megjelenítése** toosee hello kimenetét a feladatot.
   
    ![A Server Explorer tábla adatok megjelenítése kiválasztása](media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-check-results.jpg)


### <a name="view-hello-job-metrics"></a>Nézet hello feladat metrikák
Néhány egyszerű feladatot statisztikai található **feladat metrikák**. 

![Feladat metrikák ablak](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-metrics-01.png)

 
## <a name="list-hello-job-in-server-explorer"></a>A Server Explorer lista hello feladat
A **Server Explorer**, kattintson a **Stream Analytics-feladatok** majd **frissítése**. hello feladat jelenik meg az **Stream Analytics-feladatok**.

![A Server Explorer felsorolt Stream Analytics-feladatok](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-list-jobs-01.png)


## <a name="open-hello-job-view"></a>Nyissa meg hello feladat megtekintése
tooopen hello feladat nézet, bontsa ki a feladatot csomópontját, és kattintson duplán a hello **feladatok** csomópont.

![Feladatok nézet csomópont](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-job-view-01.png)


## <a name="export-an-existing-job-tooa-project"></a>Meglévő feladat tooa projekt exportálása
Exportálhatja a meglévő feladat tooa projekt két módja van.

A **Server Explorer**, kattintson a jobb gombbal hello feladat csomópontja hello **Stream Analytics-feladatok** csomópont, és válassza **tooNew Stream Analytics projekt exportálása**.

![Exportálás tooNew Stream Analytics-projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-01.png)

hello projekt jön létre a **Megoldáskezelőben**.

![A Solution Explorer létrehozott projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-02.png)
 
Is használhat hello feladat megtekintése, és kattintson a **készítése a projekt**.

![-Projekt](./media/stream-analytics-tools-for-vs/stream-analytics-tools-for-vs-export-job-03.png)

## <a name="known-issues-and-limitations"></a>Ismert problémák és korlátozások
 
- A rendszer nem támogatja a Power BI-kimenet és a Azure dátum Lake Store kimenet.
- Nincs nincs hozzáadása vagy cseréje, felhasználó által definiált függvények JavaScript szerkesztő támogatása.

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Ismerkedés az Azure Stream Analytics segítségével](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

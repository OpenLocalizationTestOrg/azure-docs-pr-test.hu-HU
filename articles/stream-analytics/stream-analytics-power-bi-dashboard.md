---
title: "az Azure Stream Analytics aaaPower BI-irányítópulton |} Microsoft Docs"
description: "Használja a valós idejű streamelési Power BI irányítópult toogather üzleti intelligencia, és a Stream Analytics-feladat nagy mennyiségű adatok elemzését."
keywords: "elemzések irányítópultján, valós idejű irányítópulton"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: jeffstok
ms.openlocfilehash: cb9127576230e9d327b437b674f31cc23869bfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>A Stream Analytics és a Power BI: az adatfolyamként történő adatok valós idejű elemzési irányítópult
Az Azure Stream Analytics lehetővé teszi a tootake segítséget az üzleti intelligencia eszközök, ami hello [Microsoft Power BI](https://powerbi.com/). Ebből a cikkből megismerheti, hogyan üzleti intelligencia eszközök használatával létrehozni a Power BI kimenetként az Azure Stream Analytics-feladatok. Azt is megtudhatja, hogyan toocreate és a valós idejű irányítópulton.

Ez a cikk továbbra is fennáll, a Stream Analytics hello [valós idejű csalások felderítéséhez](stream-analytics-real-time-fraud-detection.md) oktatóanyag. Azt, hogy az oktatóanyagban létrehozott hello munkafolyamat épül, és hozzáadja a Power bi-ban, hogy egy Streaming Analytics-feladat által észlelt rosszindulatú telefonhívásokat jelenítheti meg a kimeneti. 

Figyelheti az [videó](https://www.youtube.com/watch?v=SGUpT-a99MA) , amely bemutatja, hogy ebben a forgatókönyvben.


## <a name="prerequisites"></a>Előfeltételek

Megkezdése előtt győződjön meg arról, hogy a következő hello:

* Egy Azure-fiók.
* A Power BI egy fiókot. A munkahelyi vagy iskolai fiók is használhatja.
* Befejezett verziója hello [valós idejű csalások felderítéséhez](stream-analytics-real-time-fraud-detection.md) oktatóanyag. hello oktatóanyag fiktív telefonhívások metaadatokat hoz létre alkalmazást tartalmaz. Hello az oktatóanyagban létrehoz egy eseményközpontot, és adatfolyam-telefonhívás adatok toohello eseményközpont hello küldése. Írhat egy lekérdezést, amely észleli a csalárd hívások (azonos number hello: hello hívásait azonos idő más-más helyen). 


## <a name="add-power-bi-output"></a>A Power BI-kimenet hozzáadása
Hello valós idejű csalások észlelése oktatóanyagban hello kimeneti küldött tooAzure Blob Storage tárolóban. Ebben a szakaszban adja hozzá egy kimeneti által küldött adatokat tooPower BI.

1. Hello Azure-portálon nyissa meg a korábban létrehozott hello Streaming Analytics-feladathoz. Ha hello javasolt név, hello feladat nevű `sa_frauddetection_job_demo`.

2. Jelölje be hello **kimenetek** hello középső hello feladat irányítópult mezőbe, majd válassza ki **+ Hozzáadás**.

3. A **kimeneti Alias**, adja meg `CallStream-PowerBI`. Használhat egy másik nevet. Ha így tesz, jegyezze fel, mert később szüksége hello nevét. 

4. A **gyűjtése**, jelölje be **Power BI**.

   ![Hozzon létre egy kimeneti Power bi-ban](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. Kattintson a **engedélyezik**.

    Egy ablak, ahol megadhatja a Azure hitelesítő adatait a munkahelyi vagy iskolai fiókkal. 

    ![Hozzáférés tooPower BI hitelesítő adatok megadása](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. Adja meg a hitelesítő adatokat. Ne feledje, és megadta a hitelesítő adatait, akkor most is jogosultságot ad engedélyt toohello Streaming Analytics-feladat tooaccess a Power BI területen.

7. Amikor rendszer irányítja toohello **új kimeneti** panelen adja meg a következő információ hello:

    * **Munkaterület csoport**: Munkaterület kiválasztása toocreate hello dataset, ahová a Power BI-bérlőben.
    * **A DataSet neve**: Adjon meg `sa-dataset`. Használhat egy másik nevet. Ha így tesz, jegyezze fel a azt későbbi használatra.
    * **Tábla neve**: Adjon meg `fraudulent-calls`. Jelenleg a Power BI kimenetét a Stream Analytics-feladatok lehet csak egy tábla a DataSet adatkészlet.

    ![PBI munkaterület](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > Ha a Power BI a DataSet adatkészlet és rendelkező táblát hello hello megegyező nevekkel, adja meg, ha a hello Stream Analytics-feladat, a rendszer felülírja az meglévők hello.
    > Azt javasoljuk, hogy explicit módon hozzon létre a DataSet adatkészlet és a tábla a Power BI-fiókjába. Akkor jönnek létre automatikusan a Stream Analytics-feladat indítása és hello feladat szivattyúzó kimeneti elindul a Power BI-bA. A feladat lekérdezési eredmények nem ad vissza, ha hello adatkészlet és a tábla nem jönnek létre.
    >

8. Kattintson a **Create** (Létrehozás) gombra.

a következő beállítások hello hello dataset jön létre:

* **defaultRetentionPolicy: BasicFIFO**: adata FIFO, amely legfeljebb 200 000 sorok.
* **defaultMode: pushStreaming**: hello dataset támogatja az adatfolyam-továbbítási csempék és jelentés-alapú látványelemek hagyományos (más néven leküldéses).

Az egyéb jelzőkkel jelenleg nem hozható létre adathalmazokat.

A Power BI-adatkészletek kapcsolatos további információkért lásd: hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) hivatkozás.


## <a name="write-hello-query"></a>Hello lekérdezés írása

1. Bezárás hello **kimenetek** panel megnyitásához, és visszatérési toohello feladat panelen.

2. Kattintson a hello **lekérdezés** mezőbe. 

3. Adja meg a következő lekérdezés hello. Ez Ez hasonló toohello önillesztés lekérdezés hello csalás észlelés oktatóanyag létrehozott. hello különbség az, hogy ez a lekérdezés elküldi az eredmények toohello új kimeneti létrehozott (`CallStream-PowerBI`). 

    >[!NOTE]
    >Ha nem Ön volt name hello bemeneti `CallStream` hello csalás észlelés oktatóanyagban helyettesítse be a nevet `CallStream` a hello **FROM** és **csatlakozás** hello lekérdezés záradékok.

        /* Our criteria for fraud:
        Calls made from hello same caller tootwo phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where hello caller is hello same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where hello switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. Kattintson a **Save** (Mentés) gombra.


## <a name="test-hello-query"></a>Hello lekérdezés tesztelése
Ez a szakasz az opcionális de javasolt. 

1. Ha hello TelcoStreaming alkalmazás jelenleg nem fut, indítsa el az alábbiak szerint:

    * Nyisson meg egy parancsablakot.
    * Nyissa meg a toohello mappa, amelyben hello telcogenerator.exe és módosított telcodatagen.exe.config fájlokat is.
    * Futtassa a következő parancs hello:

            telcodatagen.exe 1000 .2 2

2. A hello **lekérdezés** panelen kattintson hello pontokból következő toohello `CallStream` adja meg, és válassza ki **az adatokat a bemeneti**.

3. Adja meg, hogy adatokat, és kattintson a három perc alatt érkezett **OK**. Várjon, amíg értesítést kap, hogy hello adatok mintavételezett.

4. Kattintson a **teszt** , és győződjön meg arról, hogy eredményeket kap.


## <a name="run-hello-job"></a>Hello feladat futtatása

1. Győződjön meg arról, hogy hello TelcoStreaming alkalmazás fut-e.

2. Bezárás hello **lekérdezés** panelen.

3. Hello feladat panelen kattintson **Start**.

    ![Hello Stream Analytics-feladat indítása](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

A Streaming Analytics-feladat indítása hello bejövő adatfolyam a csalárd hívások keres. hello feladatot is hello adatkészlet és a tábla a Power bi-ban létrehoz, és elindítja a hello csalárd hívások toothem kapcsolatos adatok küldése.


## <a name="create-hello-dashboard-in-power-bi"></a>A Power BI hello irányítópult létrehozása

1. Nyissa meg túl[powerbi.com webhelyen](https://powerbi.com) , és jelentkezzen be munkahelyi vagy iskolai fiókjával. Hello Stream Analytics-feladat lekérdezés kiírathatja a eredményeket, jelenik meg, hogy a dataset már létrejött:

    ![Folyamatos átviteli adatkészletet a Power bi-ban](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. A munkaterületen kattintson  **+ &nbsp;létrehozása**.

    ![a Power BI munkaterületen hello létrehozása gomb](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. Hozzon létre egy új irányítópult, és adjon neki nevet `Fraudulent Calls`.

    ![Hozzon létre egy irányítópultot, és adjon neki egy nevet a Power BI munkaterület](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. Hello felső hello ablak, kattintson a **Hozzáadás csempe**, jelölje be **egyéni STREAMING adatok**, és kattintson a **következő**.

    ![Egyéni adatfolyam-adatkészlet](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. A **a DATSETS**, válassza az adatkészlet majd **következő**.

    ![A folyamatos átviteli adatkészletet](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. A **képi megjelenítés típus**, jelölje be **kártya**, majd a hello **mezők** listáról válassza ki **fraudulentcalls**.

    ![Az új csempe a képi megjelenítés részletei](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. Kattintson a **Tovább** gombra.

8. Töltse ki a csempe adatait, például a title és a felirat.

    ![Cím és az új csempe alcíme](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. Kattintson az **Alkalmaz** gombra.

    Most már rendelkezik egy csalás számláló!

    ![Csalás számláló](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. Hajtsa végre hello lépések újra tooadd egy csempe (4. lépés kezdve). Megadott idő hello a következő:

    * Amikor elérhetővé túl**képi megjelenítés típus**, jelölje be **vonaldiagram**. 
    * Tengely hozzáadása, és válassza ki **windowend**. 
    * Adjon hozzá egy értéket, és válassza ki **fraudulentcalls**.
    * A **idő ablak toodisplay**, jelölje be az utolsó 10 percet hello.

    ![A vonaldiagram csempe létrehozása](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. Kattintson a **következő**, cím és alcíme hozzáadása, és kattintson a **alkalmaz**.

    hello Power BI-irányítópultot most kétféle nézetét biztosítja az adatok csalárd hívások kapcsolatos, a streamelési adatok hello észlelhető.

    ![A Power BI irányítópultjáról és azokról a csalárd hívások két csempe befejeződött](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a>További információ a Power BI szolgáltatásról

Ez az oktatóanyag bemutatja, hogyan toocreate csak néhány típusú képi megjelenítések, ha a DataSet adatkészlethez. A Power BI segítségével a szervezet más felhasználói üzleti intelligencia eszközök létrehozása. További ötletek tekintse meg a következő erőforrások hello:

* A Power BI irányítópulttal például, tekintse meg a hello [első lépések a Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videó.
* Streaming Analytics konfigurálásával kapcsolatos további információkat a feladat kimenete tooPower BI és a Power BI csoportokat használ, tekintse át a hello [Power BI](stream-analytics-define-outputs.md#power-bi) hello szakasza [Stream Analytics kimenete](stream-analytics-define-outputs.md) cikk. 
* Általában a Power BI használatával kapcsolatos információkért lásd: [Power BI-irányítópultjain](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).


## <a name="learn-about-limitations-and-best-practices"></a>További tudnivalók a korlátozások és ajánlott eljárások
Jelenleg a Power BI hívható nagyjából egyszer száma másodpercenként. Adatfolyam-továbbítási látványelemek 15 KB-os csomagokat támogatja. Felett marad, hogy a folyamatos átviteli látványelemek sikertelen (de leküldéses továbbra is toowork). Ezek a korlátozások miatt a Power BI adatmodelljeinek legtöbb természetes toocases, ahol az Azure Stream Analytics does jelentős mennyiségű adat terhelés csökkentését. Egy Átfedésmentes ablak vagy Hopping ablak tooensure adatleküldés legfeljebb egy leküldéses másodpercenként, és a lekérdezés fájljai belül hello átviteli követelmények használatát javasoljuk.

Használhatja a következő egyenlet toocompute hello érték toogive hello az ablak másodpercben:

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Példa:

* Rendelkezik egy másodperces időközönként adatküldés 1000 eszközök.
* A Power BI Pro SKU, amely támogatja a óránként 1 000 000 sorok hello használ.
* Azt szeretné, hogy toopublish hello adatmennyiség átlagos száma az eszköz tooPower BI.

Ennek eredményeképpen hello egyenlet:

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Ebben a konfigurációban hello eredeti lekérdezés toohello következő módosíthatja:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a>Újítsa meg az engedélyt
Ha hello jelszó változott, mivel a feladat meg lett létrehozva, vagy utolsó hitelesített, tooreauthenticate a Power BI-fiók szükséges. Ha az Azure többtényezős hitelesítés az Azure Active Directory (Azure AD) bérlő van beállítva, toorenew Power BI engedélyezési kéthetente is kell. Ha nem újítja meg, például a feladat kimenetére hiánya probléma volt megjelenik vagy egy `Authenticate user error` hello műveletet rögzít.

Hasonlóképpen elindul egy feladat, miután hello-token érvényessége lejárt, ha hiba történik, és hello feladat sikertelen lesz. a probléma tooresolve futtató hello feladat leállítása, és nyissa meg a Power BI kimeneti tooyour. tooavoid adatvesztés, jelölje be hello **újra a portálon** hivatkozásra, és indítsa újra a feladatot hello **feladat utolsó befejezési időpontja**.

Hello engedélyezési frissítése a Power BI, után egy zöld riasztás jelenik meg hello engedélyezési terület tooreflect, hogy hello problémát sikerült megoldani.

## <a name="get-help"></a>Segítségkérés
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Az Azure Stream Analytics lekérdezési nyelvi referencia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Az Azure Stream Analytics felügyeleti REST API-referencia](https://msdn.microsoft.com/library/azure/dn835031.aspx)

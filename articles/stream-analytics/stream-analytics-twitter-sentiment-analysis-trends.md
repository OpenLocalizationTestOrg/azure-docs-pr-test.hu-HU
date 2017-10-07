---
title: "aaaReal-Twitter véleményeket elemzés az Azure Stream Analytics |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Stream Analytics Twitter-véleményeket valós idejű elemzés céljából. Részletes útmutatás az esemény generációs toodata az élő irányítópulton."
keywords: "valós időben twitterről trendelemzés, véleményeket elemzés, közösségi elemzés, trend elemzés példa"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 42068691-074b-4c3b-a527-acafa484fda2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: jeffstok
ms.openlocfilehash: 157790caa7ea6f5570dd9c9d3bd9694d437eb4c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-twitter-sentiment-analysis-in-azure-stream-analytics"></a>Azure Stream Analytics elemzés, valós idejű Twitter véleményeket

Megtudhatja, hogyan toobuild véleményeket elemzés megoldást a közösségi média elemzés, valós idejű hozásával Twitter az Azure Event Hubs események. Ezután az Azure Stream Analytics lekérdezési tooanalyze hello adatok és a tárolókban későbbi használatra eredmények hello vagy írhat irányítópult használata és [Power BI](https://powerbi.com/) valós idejű elemzések tooprovide.

Közösségi elemzőeszközök segítségével a szervezetek trendekkel kapcsolatos témakörök ismertetése. Trendekkel témakörök tulajdonosok és a közösségi nagyszámú bejegyzéseket rendelkező szokások szolgálnak. Véleményeket elemzés, amely más néven *véleményével adatbányászati*, használja a közösségi média analytics eszközök toodetermine szokások felé egy termék, érdemes és így tovább. 

Twitter-trend valós idejű elemzési kiváló példája egy elemzőeszköz, mert hello hashtaggel történő előfizetés modell lehetővé teszi a toolisten toospecific kulcsszavak (hashtageket) és fejlesztéséhez hírcsatorna hello véleményeket elemzése.

## <a name="scenario-social-media-sentiment-analysis-in-real-time"></a>Forgatókönyv: Közösségi véleményeket elemzés, valós idejű

A vállalat, amely rendelkezik egy hírek media webhelyen olyan iránt érdeklődik előnyös használhatóságát a versenytársak való kiemeli a webhely tartalmát, amely azonnal megfelelő tooits olvasók által. hello vállalati közösségi elemzés valós idejű véleményeket Twitter-adatok elemzése végrehajtásával vonatkozó tooreaders témakörök használja.

tooidentify trendekkel kapcsolatos témakörök valós időben Twitterről, a vállalat igényeinek valós idejű elemzési hello tweetet kötet és a céggel kapcsolatos véleményeket kapcsolatos főbb témakörök hello. Más szóval hello kell olyan céggel kapcsolatos véleményeket analysis elemzés, amely a közösségi média hírcsatorna alapul.

## <a name="prerequisites"></a>Előfeltételek
Ebben az oktatóanyagban egy ügyfélalkalmazást, amely tooTwitter kapcsolódik, és megkeresi a Twitter-üzeneteket, amelyek bizonyos hashtageket (amely állíthatja be) használja. Ahhoz, toorun alkalmazás hello és elemezheti Azure Streaming Analytics segítségével Twitter-üzeneteket, rendelkeznie kell hello következő hello:

* Azure-előfizetés
* Twitter-fiók 
* Egy Twitter-alkalmazás, és hello [OAuth jogkivonat](https://dev.twitter.com/oauth/overview/application-owner-access-tokens) az adott alkalmazáshoz. Magas szintű című cikkben talál útmutatást nyújtunk toocreate később egy Twitter-alkalmazást.
* hello TwitterWPFClient alkalmazás, amelyben a hello Twitter hírcsatorna. tooget az alkalmazás, a letöltési hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) a Githubról fájlt, és ezután bontsa ki a hello csomagot egy mappába a számítógépen. Ha azt szeretné, hogy toosee hello forráskódot, és hibakereső hello alkalmazást futtat, kaphat forráskódjának hello [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

## <a name="create-an-event-hub-for-streaming-analytics-input"></a>Létrehoz egy eseményközpontot Streaming Analytics bevitelhez

hello mintaalkalmazás eseményeket hoz létre, és tooan Azure event hubs leküldéses értesítések. Az Azure event hubs esemény adatfeldolgozást a Stream Analytics hello előnyben részesített módja. További információkért lásd: hello [Azure Event Hubs dokumentáció](../event-hubs/event-hubs-what-is-event-hubs.md).


### <a name="create-an-event-hub-namespace-and-event-hub"></a>Event hub névtér és eseményközpont létrehozása
Ebben az eljárásban először létre kell hoznia egy event hub névtér, és ezután hozzáadhat egy event hub toothat névtér. Event hub névterek toologically csoport kapcsolódó esemény bus példányok. 

1. Jelentkezzen be Azure-portálon toohello, és kattintson **új** > **az eszközök internetes hálózatát** > **Eseményközpont**. 

2. A hello **névtér létrehozása** panelen adja meg például a névtér nevét `<yourname>-socialtwitter-eh-ns`. Bármilyen név hello névtér esetében is használhatja, de hello nevének kell lennie egy URL-cím érvényes, és Azure között egyedinek kell lennie. 
    
3. Válasszon egy előfizetést, és hozzon létre vagy válasszon egy erőforráscsoportot, majd kattintson **létrehozása**. 

    ![Az event hub névtér létrehozása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-namespace.png)
 
4. Hello névtér telepítése befejeződött, található hello event hub névtér elemre az Azure-erőforrások. 

5. Kattintson az új névtér hello, és hello névtér paneljén kattintson  **+ &nbsp;Eseményközpont**. 

    ![hello Eseményközpont hozzáadása gombra egy új eseményközpont létrehozása ](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub-button.png)    
 
6. Név hello új eseményközpont `socialtwitter-eh`. Használhat egy másik nevet. Ha így tesz, jegyezze fel, mert később szüksége hello nevét. Hello event hub tooset más beállításokat nem szükséges.

    ![Egy új eseményközpont létrehozása panel](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-eventhub.png)
 
7. Kattintson a **Create** (Létrehozás) gombra.


### <a name="grant-access-toohello-event-hub"></a>Adjon hozzáférést toohello event hubs

A folyamat küldhet adatokat tooan eseményközpont, hello eseményközpont olyan házirendet, amely lehetővé teszi, hogy a megfelelő hozzáférési kell rendelkeznie. hello hozzáférési házirendet hoz létre egy kapcsolati karakterláncot, amely tartalmazza az engedélyezési adatok.

1.  Hello esemény névtér paneljén kattintson **Event Hubs** , és kattintson az új eseményközpont hello nevére.

2.  Hello event hub paneljén kattintson **megosztott elérési házirendek** majd  **+ &nbsp;Hozzáadás**.

    >[!NOTE]
    >Ellenőrizze, hogy hello eseményközpont, nem hello event hub névtér dolgozik.

3.  Nevű házirend hozzáadása `socialtwitter-access` és **jogcím**, jelölje be **kezelése**.

    ![Az új esemény központi hozzáférési házirend létrehozása panel](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-shared-access-policy-manage.png)
 
4.  Kattintson a **Create** (Létrehozás) gombra.

5.  Hello házirend központi telepítése után kattintson rá a megosztott elérési házirendek hello listája.

6.  Keresés hello jelölőnégyzetet **KAPCSOLATI karakterlánc elsődleges kulcs** hello Másolás gombra következő toohello kapcsolati karakterláncot, majd. 
    
    ![Hello házirend hello elsődleges kapcsolódási karakterlánc kulcs másolása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-shared-access-policy-copy-connection-string.png)
 
7.  Hello kapcsolati karakterlánc illessze be a szövegszerkesztőben. Szükség van a kapcsolati karakterlánc hello a következő szakaszban néhány kisebb módosításokat tooit elvégzése után.

    hello kapcsolati karakterlánc így néz ki:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=;EntityPath=socialtwitter-eh

    Figyelje meg, hogy hello kapcsolati karakterláncot tartalmaz-e több kulcs-érték párok, pontosvesszővel elválasztva: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, és `EntityPath`.  

    > [!NOTE]
    > Biztonsági okokból hello kapcsolati karakterlánc hello példában részei el lettek távolítva.

8.  Hello szövegszerkesztőben, távolítsa el a hello `EntityPath` pár hello kapcsolati karakterlánc (ne feledje azt megelőző tooremove hello pontosvessző). Amikor elkészült, akkor a kapcsolati karakterlánc hello néz ki:

        Endpoint=sb://YOURNAME-socialtwitter-eh-ns.servicebus.windows.net/;SharedAccessKeyName=socialtwitter-access;SharedAccessKey=Gw2NFZw6r...FxKbXaC2op6a0ZsPkI=


## <a name="configure-and-start-hello-twitter-client-application"></a>Konfigurálni és elindítani a hello Twitter-ügyfélalkalmazás
hello ügyfélalkalmazás tweetet események lekérése Twitter-ről. A rendezés toodo tette, engedély toocall hello Streamelési API-k Twitter kell. tooconfigure, hogy engedélyt, létrehoz egy alkalmazást a Twitter, amely egyedi hitelesítő adataikat (például az OAuth jogkivonat) hoz létre. Konfigurálhatja hello ügyfél alkalmazás toouse ezeket a hitelesítő adatokat, amikor az API-hívásokban teszi. 

### <a name="create-a-twitter-application"></a>Twitter-alkalmazás létrehozása
Ha még nem rendelkezik egy Twitter-alkalmazás, amely ehhez az oktatóanyaghoz használható, akkor hozzon létre egyet. Már rendelkeznie kell egy Twitter-fiók.

> [!NOTE]
> az alkalmazás létrehozása és hello kulcsokat, a titkos kulcsok és a token első Twitter hello pontos folyamat változhatnak. Ha ezek az utasítások nem egyeznek meg, tekintse meg hello Twitter hely, tekintse meg a toohello Twitter fejlesztői dokumentációját.

1. Nyissa meg toohello [Twitter alkalmazás kezelése lap](https://apps.twitter.com/). 

2. Hozzon létre egy új alkalmazást. 

    * Hello webhely URL-címe adja meg egy érvényes URL-címet. Nincs toobe élő webhelyet. (Nem lehet csak megadni `localhost`.)
    * Hello visszahívási mezőt hagyja üresen. Ebben az oktatóanyagban használt hello ügyfélalkalmazás visszahívások nem igényel.

    ![Az alkalmazás létrehozása a Twitteren](./media/stream-analytics-twitter-sentiment-analysis-trends/create-twitter-application.png)

3. Igény szerint megváltoztathatja csak tooread hello alkalmazás engedélyeket.

4. Hello alkalmazás létrehozásakor Ugrás toohello **kulcsok és a hozzáférési jogkivonatok** lap.

5. Kattintson a hello gomb toogenerate olyan hozzáférési jogkivonatot, és a hozzáférési jogkivonat titkos kulcs.

Tartani lesz szüksége, ezt az információt, mivel a következő eljárásban hello szüksége lesz rá.

>[!NOTE]
>hello kulcsok és titkos hello Twitter-alkalmazás biztosítanak hozzáférést tooyour Twitter-fiók. Ez az információ tekinti érzékeny, hello ugyanaz, mint a Twitter-jelszó. Például nincs beágyazás ezt az információt az alkalmazás tooothers rendelt. 


### <a name="configure-hello-client-application"></a>Hello ügyfélalkalmazás konfigurálása
Létrehoztunk Önnek egy ügyfélalkalmazást, amely összeköti a tooTwitter használatával végzett [Twitter a Streamelési API-k](https://dev.twitter.com/streaming/overview) toocollect tweetet események kapcsolatos témakörök egy adott készletét. hello alkalmazás használ hello [Sentiment140](http://help.sentiment140.com/) nyílt forráskódú eszköz, amely rendeli hozzá a következő véleményeket érték tooeach tweetet hello:

* 0 = negatív.
* 2 = neutral
* 4 = pozitív

Hello tweetet események véleményeket érték van rendelve, miután azok elküldte azokat korábban létrehozott eseményközpont toohello.

Mielőtt hello alkalmazás fut, egyes adatok, például a hello Twitter kulcsok és a hello event hub kapcsolati karakterlánc van szükség. Megadhatja, hogy hello konfigurációs adatait az alábbi módszerek:

* Hello alkalmazás futtatásához, és aztán hello alkalmazás felhasználói felületén tooenter hello kulcsok, titkok és kapcsolati karakterláncot. Ha így tesz, hello konfigurációs adatok az aktuális munkamenetre, de nem menti a rendszer.
* Hello alkalmazás .config fájl és a set hello értékeket szerkesztése. Ez a módszer továbbra is fennáll, hello konfigurációs adatokat, de azt is jelenti, hogy a bizalmas adatokat a számítógép egyszerű szövegként vannak tárolva.

hello következő eljárás dokumentumok mindkét megközelítés. 

1. Győződjön meg arról, hogy a letöltött és unzipped hello [TwitterWPFClient.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TwitterClient/TwitterWPFClient.zip) alkalmazás, a hello előfeltételek.

2. tooset hello értékek futási időben (és csak az aktuális munkamenet hello), futtassa a hello `TwitterWPFClient.exe` alkalmazás. Amikor a hello alkalmazás kéri, adja meg a következő értékek hello:

    * hello Twitter kulcsa (API-kulcsot).
    * hello Twitter felhasználói titok (API titok).
    * hello Twitter hozzáférési jogkivonat.
    * hello Twitter Access Token titkos kulcsot.
    * hello kapcsolati karakterlánc információ korábban mentett. Győződjön meg arról, hogy eltávolította a hello hello kapcsolati karakterláncot használó `EntityPath` a kulcs-érték pár.
    * hello Twitter kulcsszavak, amelyet a toodetermine céggel kapcsolatos véleményeket.

   ![TwitterWpfClient alkalmazás fut, melyen rejtett beállítások](./media/stream-analytics-twitter-sentiment-analysis-trends/wpfclientlines.png)

3. tooset hello értékek folyamatosan fennáll, használja a szerkesztő tooopen hello TwitterWpfClient.exe.config szövegfájlba. Ezt a hello `<appSettings>` elem, ehhez:

    * Állítsa be `oauth_consumer_key` toohello Twitter kulcsa (API-kulcsot). 
    * Állítsa be `oauth_consumer_secret` toohello Twitter felhasználói titok (API titok).
    * Állítsa be `oauth_token` toohello Twitter hozzáférési jogkivonat.
    * Állítsa be `oauth_token_secret` toohello Twitter Access Token titkos kulcsot.

    Később a hello `<appSettings>` elem, ezeket a módosításokat:

    * Állítsa be `EventHubName` toohello eseményközpont neveként (Ez azt jelenti, hogy toohello értéke hello entitás elérési út).
    * Állítsa be `EventHubNameConnectionString` toohello kapcsolati karakterláncot. Győződjön meg arról, hogy eltávolította a hello hello kapcsolati karakterláncot használó `EntityPath` a kulcs-érték pár.

    Hello `<appSettings>` szakasz a következő példa hello tűnik. (Az érthetőség és a biztonsági, azt becsomagolt a sorokat és eltávolítani néhány karaktert.)

    ![TwitterWpfClient alkalmazás konfigurációs fájlt egy szövegszerkesztőben, hello Twitter kulcsok és titkos kulcsok és hello események központ kapcsolati karakterlánc adatait megjelenítő](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-tiwtter-app-config.png)
 
4. Ha már hello alkalmazás indította el, TwitterWpfClient.exe most kell futtatni. 

5. Kattintson a hello zöld start gomb toocollect közösségi céggel kapcsolatos véleményeket. A hello Tweetet eseményeket **CreatedAt**, **témakör**, és **SentimentScore** értékek tooyour eseményközpont küldi el.

    ![TwitterWpfClient alkalmazás fut, a Twitter-üzenetek listáját ábrázoló](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-app-listing.png)

    >[!NOTE]
    >Ha hibába ütközik, és a Twitter-üzeneteket hello hello ablakának alsó részén megjelennek az adatfolyam nem jelenik meg, gondosan ellenőrizze a hello kulcsok és titkos kulcsok. Jelölniük hello kapcsolati karakterlánc (Győződjön meg arról, hogy nem tartalmazza a hello `EntityPath` kulcs-érték.)


## <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása

Most, hogy a rendszer valós időben Twitterről a adatfolyam-tweetet események, beállíthat egy Stream Analytics-feladat tooanalyze ezeket az eseményeket valós időben.

1. Hello Azure-portálon, kattintson **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.

2. Név hello feladat `socialtwitter-sa-job` , és adja meg egy előfizetési, erőforráscsoportot és helyet.

    Egy jó ötlet tooplace hello feladat és hello eseményközpont a hello ugyanabban a régióban a legjobb teljesítmény és, hogy nem kell fizetnie tootransfer adatok régiók között.

    ![Egy új Stream Analytics-feladat létrehozása](./media/stream-analytics-twitter-sentiment-analysis-trends/newjob.png)

3. Kattintson a **Create** (Létrehozás) gombra.

    hello feladat jön létre, és hello portál megjeleníti a feladat részleteit.


## <a name="specify-hello-job-input"></a>Adjon meg hello feladat bemeneti

1. A Stream Analytics-feladat, a alatt **feladat topológia** hello középső hello feladat panelen, kattintson a **bemenetek**. 

2. A hello **bemenetek** panelen kattintson  **+ &nbsp;Hozzáadás** hello panel ezekkel az értékekkel, majd töltse ki:

    * **A bemeneti alias**: hello név használata `TwitterStream`. Ha más nevet használjon, jegyezze fel a, mert később szüksége.
    * **Adatforrás típusa**: válasszon **adatfolyam**.
    * **Forrás**: válasszon **eseményközpont**.
    * **Beállítás importálása**: válasszon **eseményközpont használható a jelenlegi előfizetés**. 
    * **Service bus-névtér**: Jelölje ki a korábban létrehozott hello esemény hub névteret (`<yourname>-socialtwitter-eh-ns`).
    * **Az Event hubs**: korábban létrehozott eseményközpont válassza hello (`socialtwitter-eh`).
    * **Eseményközpont házirend neve**: válassza ki a korábban létrehozott hello hozzáférési házirend (`socialtwitter-access`).

    ![Új bemeneti Streaming Analytics-feladat létrehozása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-twitter-new-input.png)

3. Kattintson a **Create** (Létrehozás) gombra.


## <a name="specify-hello-job-query"></a>Hello feladat-lekérdezés megadása

A Stream Analytics egy egyszerű, deklaratív lekérdezési modellel, átalakítások leíró támogatja. toolearn hello nyelvi kapcsolatos további információkért lásd: hello [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx).  Ez az oktatóanyag segítséget nyújt a szerzői és több lekérdezések tesztelése Twitter-adatok.

megjegyzések között témakörök toocompare hello száma, használhatja a [Átfedésmentes ablak](https://msdn.microsoft.com/library/azure/dn835055.aspx) megjegyzések által témakör öt másodpercenként tooget hello száma.

1. Bezárás hello **bemenetek** panelt, ha még nem tette meg.

2. Hello feladat panelen, kattintson a hello **lekérdezés** mezőbe. Azure hello bemenetek sorolja fel, és amelyet hello feladat van konfigurálva, és lehetővé teszi, hogy hozzon létre egy lekérdezést, amely lehetővé teszi hello bemeneti adatfolyam átalakítására, toohello kimeneti keresztül küldött.

3. Győződjön meg arról, hogy hello TwitterWpfClient alkalmazás fut-e. 

3. A hello **lekérdezés** panelen kattintson hello pontokból következő toohello `TwitterStream` adja meg, és válassza ki **az adatokat a bemeneti**.

    ![Menü Beállítások toouse mintaadatainak hello Streaming Analytics-feladat bejegyzést, mintaadatokkal"bemeneti" kijelölve](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-sample-data-from-input.png)

    Ekkor megnyílik egy panel, amely lehetővé teszi, hogy mennyi minta adatok tooget, mennyi ideig tooread hello bemeneti adatfolyam definiált adja meg.

4. Állítsa be **perc** too3 majd **OK**. 
    
    ![Mintavételi hello bemeneti adatfolyam, a "3 perc" kiválasztott beállítások.](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-input-create-sample-data.png)

    Azure 3 percet értékelésével hello bemeneti adatfolyamból minták, és értesítést küld, amikor készen áll a mintaadatok hello. (Ez időt vesz igénybe egy rövid ideig.) 

    hello mintaadatok ideiglenesen tárolja, és áll rendelkezésre, míg a hello lekérdezés ablak megnyitásához. Ha bezárja a hello lekérdezési ablakba, hello minta adatait a rendszer törli, és el kell toocreate mintaadatok új készletét. 

5. Hello lekérdezés hello kód szerkesztő toohello alábbi módosítása:

    ```
    SELECT System.Timestamp as Time, Topic, COUNT(*)
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY TUMBLINGWINDOW(s, 5), Topic
    ```

    Ha nem a `TwitterStream` hello alias hello bevitellel, mint helyettesítse be az aliast, a `TwitterStream` hello lekérdezésben.  

    Ez a lekérdezés használ hello **TIMESTAMP BY** kulcsszó toospecify hello hasznos toobe hello historikus számítási használt Timestamp típusú mező. Ha ez a mező nincs megadva, a hello leképezési művelet hello eseményközpont érkező minden esemény hello idő alapján történik. További információ: hello "érkezésének ideje vs alkalmazási idő" szakasza [Stream Analytics lekérdezési hivatkozás](https://msdn.microsoft.com/library/azure/dn834998.aspx).

    Ez a lekérdezés is segítségével éri el az egyes ablak hello end időbélyeg hello **System.Timestamp** tulajdonság.

5. Kattintson a **teszt**. hello lekérdezés, amely akkor mintát hello adatok alapján futtatja.
    
6. Kattintson a **Save** (Mentés) gombra. Ezáltal hello lekérdezés hello Streaming Analytics-feladat részeként. (Hello mintaadatok nem menti azt.)


## <a name="experiment-using-different-fields-from-hello-stream"></a>Különböző mezőkkel hello adatfolyamból kísérlet 

hello következő táblázatban hello JSON streamadatok részét képező hello mezőket. A Lekérdezésszerkesztő hello szabad tooexperiment látja.

|JSON-tulajdonság | Meghatározás|
|--- | ---|
|createdAt | hello ideje, hogy hello tweetet lett létrehozva|
|Témakör | a megadott kulcsszó hello témakör, amely megfelel a hello|
|SentimentScore | hello véleményeket pontszám a Sentiment140|
|Szerző | hello Twitter leíró hello tweetet küldött|
|Szöveg | hello tweetet hello teljes törzse|


## <a name="create-an-output-sink"></a>Kimeneti fogadóként létrehozása

Most már definiált az eseménystream egy event hub bemeneti tooingest események és a lekérdezés tooperform átalakítás hello adatfolyam keresztül. hello utolsó lépése toodefine kimeneti fogadóként hello feladat.  

Ebben az oktatóanyagban összesítve hello tweetet események hello feladat lekérdezés tooAzure Blob-tároló az írása.  Az eredmények tooAzure SQL adatbázis Azure Table storage, az Event Hubs is beküldéssel vagy Power BI, attól függően, hogy az alkalmazás igényel.

## <a name="specify-hello-job-output"></a>Adja meg a feladat kimenetére hello

1. A hello **feladat topológia** területen kattintson a hello **kimeneti** mezőbe. 

2. A hello **kimenetek** panelen kattintson a  **+ &nbsp;Hozzáadás** hello panel ezekkel az értékekkel, majd töltse ki:

    * **A kimeneti alias**: hello név használata `TwitterStream-Output`. 
    * **Gyűjtése**: válasszon **Blob-tároló**.
    * **Importálási lehetőségeit**: válasszon **használja a jelenlegi előfizetés blob-tároló**.
    * **A tárfiók**. Válassza ki **hozzon létre egy új tárfiókot.**
    * **A tárfiók** (második mezőben). Adja meg `YOURNAMEsa`, ahol `YOURNAME` a neve, vagy egy másik egyedi karakterlánc. hello neve csak kisbetűket és számokat használhatja, és az Azure között egyedinek kell lennie. 
    * **Tároló**. Adja meg `socialtwitter`.
    hello tárfiók neve vagy a tároló neve használt együtt tooprovide egy URI-JÁNAK hello blob-tároló, ez például a következők: 

    `http://YOURNAMEsa.blob.core.windows.net/socialtwitter/...`
    
    ![Panel "Új kimeneti" Stream Analytics-feladat](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-create-output-blob-storage.png)
    
4. Kattintson a **Create** (Létrehozás) gombra. 

    Azure hello tárfiókot hoz létre, és automatikusan létrehoz egy kulcsot. 

5. Bezárás hello **kimenetek** panelen. 


## <a name="start-hello-job"></a>Hello feladat indítása

Egy feladat bemeneti, a lekérdezés és a kimeneti meg van adva. Készen áll a toostart hello Stream Analytics-feladat.

1. Győződjön meg arról, hogy hello TwitterWpfClient alkalmazás fut-e. 

2. Hello feladat panelen kattintson **Start**.

    ![Hello Stream Analytics-feladat indítása](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-output.png)

3. A hello **indítási feladat** panelen a **feladatkiemenetét kezdési időpont**, jelölje be **most** , majd **Start**. 

    !["Indítsa el a feladat" panel hello Stream Analytics-feladat](./media/stream-analytics-twitter-sentiment-analysis-trends/stream-analytics-sa-job-start-job-blade.png)

    Azure értesíti, amikor hello feladat elindult, és hello feladat panelen hello állapot jelenik meg **futtató**.

    ![Futó feladat](./media/stream-analytics-twitter-sentiment-analysis-trends/jobrunning.png)

## <a name="view-output-for-sentiment-analysis"></a>Nézet kimeneti véleményeket elemzés

A feladat elindult, és hello valós időben Twitterről adatfolyam feldolgozása után megtekintheti a hello kimeneti véleményeket elemzés céljából.

Egy eszköz, például használhatja [Azure Tártallózó](https://http://storageexplorer.com/) vagy [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction) tooview a feladat kimeneti valós időben. Itt használható [Power BI](https://powerbi.com/) , például az alkalmazás tooinclude személyre szabott irányítópultot tooextend hello egyet a következő képernyőkép hello látható:

![Power BI](./media/stream-analytics-twitter-sentiment-analysis-trends/power-bi.png)


## <a name="create-another-query-tooidentify-trending-topics"></a>Hozzon létre egy másik lekérdezést tooidentify trendekkel kapcsolatos témakörök

Azon alapul, használhatja a toounderstand Twitter sentiment egy másik lekérdezés egy [késleltetett ablak](https://msdn.microsoft.com/library/azure/dn835051.aspx). tooidentify trendekkel kapcsolatos témakörök, célszerű figyelni témakörök, amelyek a megadott időn belül megjegyzések vonatkozó küszöbérték cross.

Ez az oktatóanyag céljából hello akkor ellenőrizze témakörök, amelyek nem szerepelnek több mint 20 alkalommal hello utolsó 5 másodperc.

1. Hello feladat panelen kattintson **leállítása** toostop hello feladat. 

2. A hello **feladat topológia** területen kattintson a hello **lekérdezés** mezőbe. 

3. Hello lekérdezés toohello következő módosítása:

    ```    
    SELECT System.Timestamp as Time, Topic, COUNT(*) as Mentions
    FROM TwitterStream TIMESTAMP BY CreatedAt
    GROUP BY SLIDINGWINDOW(s, 5), topic
    HAVING COUNT(*) > 20
    ```

4. Kattintson a **Save** (Mentés) gombra.

5. Győződjön meg arról, hogy hello TwitterWpfClient alkalmazás fut-e. 

6. Kattintson a **Start** toorestart hello feladat hello új lekérdezést.


## <a name="get-support"></a>Támogatás kérése
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

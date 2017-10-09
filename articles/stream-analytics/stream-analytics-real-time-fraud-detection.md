# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Az Azure Stream Analytics első lépéseiben: valós idejű csalások felderítése

Ez az oktatóanyag egy végpontok közötti ábra bemutatja, hogyan nyújt az Azure Stream Analytics toouse. Az alábbiak végrehajtásának módját ismerheti meg: 

* Kapcsolja a streaming eseményeket az Azure Event Hubs egy példányát. Ebben az oktatóanyagban egy alkalmazást, amely azt adja meg, amely a mobiltelefon metaadat-bejegyzéseket adatfolyam szimulálja fogja használni.

* SQL-szerű Stream Analytics lekérdezések tootransform adatok, szoftverfelügyeleti, vagy csak a következő minták írása. Hogyan toouse egy lekérdezés tooexamine bejövő streamből hello, és keresse meg, előfordulhat, hogy rosszindulatúak hívások jelenik meg.

* Hello eredmények tooan kimeneti fogadóját (tárolás), amely további betekintést nyerjen elemezheti küldése. Ebben az esetben akkor küld hello gyanús hívás adatok tooAzure Blob-tároló.

Ebben az oktatóanyagban használjuk hello példa valós idejű csalások felderítéséhez, a telefonhívás-adatok alapján. De hello eljárás azt mutatja be olyan is alkalmas más típusú csalások felderítéséhez, például a hitelkártya csalás vagy azonosító adatokkal való visszaéléseket. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Forgatókönyv: Távközlési és SIM csalások felderítéséhez valós időben

A távközlési vállalat rendelkezik nagy mennyiségű bejövő hívások adatait. hello vállalat szeretne a csalárd toodetect meghívja valós időben, hogy a felhasználók értesítése, vagy állítsa le az adott szolgáltatást. Egy adott típusú SIM csalás magában foglalja a hello több hívás ugyanazzal az identitással körül hello azonos időben azonban földrajzilag különböző helyeken. toodetect csalás, az ilyen típusú hello a vállalat igényeinek tooexamine bejövő phone rögzíti, és meghatározott mintákat keressen – hello körül felé indított hívások ebben az esetben a különböző országokban egy időben. A telefon azt jelzi, hogy ebbe a kategóriába tartozik írt toostorage további elemzés céljából.

## <a name="prerequisites"></a>Előfeltételek

Ebben az oktatóanyagban a telefonhívás-adatok szimulálása lesz egy ügyfél-alkalmazással, amely mintát telefonhívás metaadatokat hoz létre. Keresse meg például a csalárd hívások néhány hello azt jelzi, hogy az alkalmazás hello eredményez. 

Megkezdése előtt győződjön meg arról, hogy a következő hello:

* Egy Azure-fiók.
* hello hívás-esemény készítő alkalmazást. Ez a letöltés hello kaphat [TelcoGenerator.zip fájl](http://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) a hello Microsoft Download Center. Bontsa ki a csomagot egy mappába a számítógépen. Ha azt szeretné, toosee hello forráskódját és a hibakereső futtatási hello app, kaphat a hello alkalmazás forráskódjának [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator). 

    >[!NOTE]
    >Előfordulhat, hogy a Windows hello letöltött .zip fájl blokkolja. Ha Ön nem bontsa ki azt, kattintson a jobb gombbal a hello fájlt, és válassza ki **tulajdonságok**. Ha megjelenik a hello "ezt a fájlt egy másik számítógépről érkezett, és előfordulhat, hogy lehet letiltott toohelp a számítógép védelme" üzenet, jelölje be hello **Unblock** lehetőséget, majd kattintson a **alkalmaz**.

Ha azt szeretné, hogy hello Streaming Analytics-feladat eredményének tooexamine hello, is kell egy eszközt az Azure Blob Storage-tároló hello tartalom megtekintése. Ha a Visual Studio használata esetén használhatja [Azure Tools for Visual Studio](https://docs.microsoft.com/en-us/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) vagy [Visual Studio Cloud Explorer](https://docs.microsoft.com/en-us/azure/vs-azure-tools-resources-managing-with-cloud-explorer). Másik lehetőségként telepítheti önálló eszközök, például a [Azure Tártallózó](http://storageexplorer.com/) vagy [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

## <a name="create-an-azure-event-hubs-tooingest-events"></a>Hozzon létre az Azure event hubs tooingest események

tooanalyze adatfolyam, akkor *betöltési* Azure be azt. A szokásos módon tooingest adata toouse [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), amely lehetővé teszi több millió eseményt másodpercenként és majd feldolgozni, és hello esemény információk tárolására. Ebben az oktatóanyagban létrehoz egy eseményközpontot, és már rendelkezik a hello hívás-készítő alkalmazás küldési hívás adatok toothat esemény eseményközpont. Az event hubs kapcsolatban bővebben lásd: hello [Azure Service Bus-dokumentáció](https://docs.microsoft.com/en-us/azure/service-bus/).

>[!NOTE]
>Ez az eljárás részletes verziójához, lásd: [az Event Hubs-névtér létrehozása, és az event hub használatával hello Azure-portálon](../event-hubs/event-hubs-create.md). 

### <a name="create-a-namespace-and-event-hub"></a>A névtér és az event hub létrehozása
Ebben az eljárásban először létre kell hoznia egy event hub névtér, és ezután hozzáadhat egy event hub toothat namepsace. Event hub névterek toologically csoport kapcsolódó esemény bus példányok. 

1. Jelentkezzen be hello Azure-portálon, majd kattintson a **új** > **az eszközök internetes hálózatát** > **Eseményközpont**. 

2. A hello **névtér létrehozása** panelen adja meg például a névtér nevét `<yourname>-eh-ns-demo`. Bármilyen név hello névtér esetében is használhatja, de hello nevének kell lennie egy URL-cím érvényes, és Azure között egyedinek kell lennie. 
    
3. Válasszon egy előfizetést, és hozzon létre vagy válasszon egy erőforráscsoportot, majd kattintson **létrehozása**. 

    ![Az event hub névtér létrehozása](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png)
 
4. Hello névtér telepítése befejeződött, található hello event hub névtér elemre az Azure-erőforrások. 

5. Kattintson az új névtér hello, és hello névtér paneljén kattintson  **+ &nbsp;Eseményközpont**. 

    ![hello Eseményközpont hozzáadása gombra egy új eseményközpont létrehozása ](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
6. Név hello új eseményközpont `sa-eh-frauddetection-demo`. Használhat egy másik nevet. Ha így tesz, jegyezze fel, mert később szüksége hello nevét. Nincs szükség a tooset az egyéb beállításokat hello event hub most.

    ![Egy új eseményközpont létrehozása panel](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png)
    
 
7. Kattintson a **Create** (Létrehozás) gombra.
### <a name="grant-access-toohello-event-hub-and-get-a-connection-string"></a>Engedélyezze a hozzáférést toohello eseményközpont, majd egy kapcsolati karakterlánc beolvasása

A folyamat küldhet adatokat tooan eseményközpont, hello eseményközpont olyan házirendet, amely lehetővé teszi, hogy a megfelelő hozzáférési kell rendelkeznie. hello hozzáférési házirendet hoz létre egy kapcsolati karakterláncot, amely tartalmazza az engedélyezési adatok.

1.  Hello esemény névtér paneljén kattintson **Event Hubs** , és kattintson az új eseményközpont hello nevére.

2.  Hello event hub paneljén kattintson **megosztott elérési házirendek** majd  **+ &nbsp;Hozzáadás**.

    >[!NOTE]
    >Ellenőrizze, hogy hello eseményközpont, nem hello event hub névtér dolgozik.

3.  Nevű házirend hozzáadása `sa-policy-manage-demo` és **jogcím**, jelölje be **kezelése**.

    ![Az új esemény központi hozzáférési házirend létrehozása panel](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png)
 
4.  Kattintson a **Create** (Létrehozás) gombra.

5.  Hello házirend központi telepítése után kattintson rá a megosztott elérési házirendek hello listája.

6.  Keresés hello jelölőnégyzetet **KAPCSOLATI karakterlánc elsődleges kulcs** hello Másolás gombra következő toohello kapcsolati karakterláncot, majd. 
    
    ![Hello házirend hello elsődleges kapcsolódási karakterlánc kulcs másolása](./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png)
 
7.  Hello kapcsolati karakterlánc illessze be a szövegszerkesztőben. Szükség van a kapcsolati karakterlánc hello a következő szakaszban néhány kisebb módosításokat tooit elvégzése után.

    hello kapcsolati karakterlánc így néz ki:

        Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=sa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=sa-eh-frauddetection-demo

    Figyelje meg, hogy hello kapcsolati karakterláncot tartalmaz-e több kulcs-érték párok, pontosvesszővel elválasztva: `Endpoint`, `SharedAccessKeyName`, `SharedAccessKey`, és `EntityPath`.  

## <a name="configure-and-start-hello-event-generator-application"></a>Konfigurálni és elindítani a hello esemény készítő alkalmazás

A kezdéshez hello TelcoGenerator app, konfigurálja úgy, hogy elküldi hívás rekordok toohello eseményközpont újonnan létrehozott.

### <a name="configure-hello-telcogeneratorapp"></a>Hello TelcoGeneratorapp konfigurálása

1.  Hello szerkesztő másolásakor hello kapcsolati karakterláncot, jegyezze fel a hello `EntityPath` értékét, és távolítsa el a hello `EntityPath` pár (ne feledje azt megelőző tooremove hello pontosvessző). 

2.  Amennyiben Ön unzipped hello TelcoGenerator.zip fájl hello mappában nyissa meg a hello telcodatagen.exe.config fájlt valamelyik szerkesztőben. (Nincs több .config kiterjesztésű fájl, ezért ügyeljen arra, hogy nyissa meg a hello jobbról egy.)

3.  A hello `<appSettings>` elem, ehhez:

    * Állítsa be hello hello `EventHubName` kulcs toohello eseményközpont neveként (Ez azt jelenti, hogy toohello értéke hello entitás elérési út).
    * Állítsa be hello hello `Microsoft.ServiceBus.ConnectionString` kulcs toohello kapcsolati karakterláncot. 

    Hello `<appSettings>` szakasz a következő példa hello fog megjelenni. (Az átláthatóság érdekében a Microsoft hello sorok becsomagolt és néhány karaktert a hello engedélyezési jogkivonat.)

    ![TelcoGenerator alkalmazás konfigurációs fájl megjelenítő hello esemény központ nevét és a kapcsolati karakterlánc](./media/stream-analytics-real-time-fraud-detection/stream-analytics-telcogenerator-config-file-app-settings.png)
 
4.  Hello fájl mentéséhez. 

### <a name="start-hello-app"></a>Indítsa el hello alkalmazást
1.  Nyisson meg egy parancsablakot, és módosítsa a toohello mappát, ahol hello TelcoGenerator app tömörítetlen.
2.  Adja meg a következő parancs hello:

        telcodatagen.exe 1000 .2 2

    hello paraméterek a következők: 

    * Óránként EK számát. 
    * SIM kártyával csalás valószínűség: Milyen gyakran, minden hívások százalékában hello alkalmazást kell szimulálása csalárd hívásakor. .2 hello érték azt jelenti, hogy körülbelül 20 %-a hello hívás rekordok csalárd megjelenése.
    * Hossza (óra). alkalmazás hello órák száma hello kell futtatnia. Is leállíthatja hello app bármikor hello parancssorból Ctrl + C billentyűt lenyomásával.

    Néhány másodperc múlva hello alkalmazás indítása, telefonhívás rekordok megjelenítése az üdvözlő képernyőt, toohello eseményközpont küldi azokat.

A valós idejű csalások észlelése alkalmazásban használt hello kulcsmezők némelyike hello következő:

|**Rekord**|**Meghatározása**|
|----------|--------------|
|`CallrecTime`|hello hívás időbélyegzőjét hello kezdési időpontja. |
|`SwitchNum`|hello telefon kapcsoló tooconnect hello hívás. Ebben a példában hello kapcsolók karakterláncok, amelyek megfelelnek a hello származási (Egyesült Államok, Kína, UK, Németország vagy Ausztrália) vonatkoznak. |
|`CallingNum`|hello hívó hello telefonszámát. |
|`CallingIMSI`|hello International Mobile Subscriber Identity (IMSI). Ez az egyedi azonosító hello hello hívó. |
|`CalledNum`|hello hello hívás címzett telefonszáma. |
|`CalledIMSI`|Nemzetközi mobil előfizető Identity (IMSI). Ez az egyedi azonosítója hello hello hívás címzettje. |


## <a name="create-a-stream-analytics-job-toomanage-streaming-data"></a>A Stream Analytics-feladat adatfolyam toomanage létrehozása

Most, hogy egy az események streamjét a hívást, beállíthat egy Stream Analytics-feladat. hello feladat hello eseményközpont beállított adatok olvasását. 

### <a name="create-hello-job"></a>Hello feladat létrehozása 

1. Hello Azure-portálon, kattintson **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**.

2. Név hello feladat `sa_frauddetection_job_demo`, adjon meg egy előfizetési, erőforráscsoportot és helyet.

    Egy jó ötlet tooplace hello feladat és hello eseményközpont a hello ugyanabban a régióban a legjobb teljesítmény és, hogy nem kell fizetnie tootransfer adatok régiók között.

    ![Új Stream Analytics-feladat létrehozása](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png)

3. Kattintson a **Create** (Létrehozás) gombra.

    hello feladat jön létre, és hello portál megjeleníti a feladat részleteit. Semmi sem fut-e még, azonban – akkor tooconfigure hello feldolgozás előtt indíthatók el.

### <a name="configure-job-input"></a>Feladat bemeneti konfigurálása

1. Hello irányítópult vagy hello **összes erőforrás** panelen, keresése és kijelölése hello `sa_frauddetection_job_demo` Stream Analytics-feladat. 
2. A hello **feladat topológia** szakasza hello Stream Analytics-feladat paneljén kattintson hello **bemeneti** mezőbe.

    ![A topológia hello Streaming Analytics-feladat panelen beviteli mezőbe](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. Kattintson a  **+ &nbsp;Hozzáadás** hello panel ezekkel az értékekkel, majd töltse ki:

    * **A bemeneti alias**: hello név használata `CallStream`. Ha más nevet használjon, jegyezze fel a, mert később szüksége.
    * **Adatforrás típusa**: válasszon **adatfolyam**. (**Referenciaadatok** toostatic keresési adatokat, amelyeket az oktatóanyag nem használ hivatkozik.)
    * **Forrás**: válasszon **eseményközpont**.
    * **Beállítás importálása**: válasszon **eseményközpont használható a jelenlegi előfizetés**. 
    * **Service bus-névtér**: Jelölje ki a korábban létrehozott hello esemény hub névteret (`<yourname>-eh-ns-demo`).
    * **Az Event hubs**: korábban létrehozott eseményközpont válassza hello (`sa-eh-frauddetection-demo`).
    * **Eseményközpont házirend neve**: válassza ki a korábban létrehozott hello hozzáférési házirend (`sa-policy-manage-demo`).

    ![Új bemeneti Streaming Analytics-feladat létrehozása](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png)

4. Kattintson a **Create** (Létrehozás) gombra.

## <a name="create-queries-tootransform-real-time-data"></a>Lekérdezések létrehozása tootransform valós idejű adatok

Ezen a ponton rendelkezik egy Stream Analytics-feladat bejövő adatfolyam tooread beállítása. hello következő lépésre toocreate hello adatok valós idejű átalakítást. Ehhez létrehozhat egy lekérdezést. A Stream Analytics egy egyszerű, deklaratív lekérdezési modellel, amely leírja a valós idejű feldolgozással átalakítások támogatja. hello lekérdezések egy SQL-szerű nyelv, amely rendelkezik a egyes kiterjesztések adott toostream analytics használja. 

Egy nagyon egyszerű lekérdezést egyszerűen előfordulhat, hogy az összes bejövő hello-adatok olvasása. Azonban Ön gyakran lekérdezéseket hozhat létre keresse meg a meghatározott adatok vagy kapcsolatok hello adataiban. Ebben a szakaszban hello oktatóanyag létrehoz és teszteléséhez több lekérdezések toolearn néhány módszer egy bemeneti adatfolyam elemzéshez alakíthatja át. 

az itt létrehozott hello lekérdezések hello átalakított adatok toohello képernyő csak jeleníti meg. Egy későbbi szakasz ismerteti konfigurálhatja a kimeneti fogadóként és a lekérdezés hello átalakított adatok toothat fogadó írja.

toolearn hello nyelvi kapcsolatos további információkért lásd: hello [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/dn834998.aspx).

### <a name="get-sample-data-for-testing-queries"></a>Tesztelési lekérdezések minta-adatok beolvasása

hello TelcoGenerator alkalmazása elküldi hívás rekordok toohello eseményközpont, és a Stream Analytics-feladat az eseményközpont hello konfigurált tooread. A lekérdezés tootest hello feladat toomake meg arról, hogy helyesen van-e olvasási is használhatja. túl a hello Azure-konzolon a lekérdezés tesztelése, mintaadatokat van szüksége. Ennél a bemutatónál mintaadatok lesz kinyerése hello adatfolyam hello eseményközpont az adatforrásból.

1. Ellenőrizze, hogy hello TelcoGenerator alkalmazást futtató és termelő hívást rögzíti.
2. Hello portálon vissza toohello Streaming Analytics-feladat panelen. (Ha korábban bezárta a hello panelen, keresse meg a `sa_frauddetection_job_demo` a hello **összes erőforrás** panel.)
3. Kattintson a hello **lekérdezés** mezőbe. Azure hello bemenetek sorolja fel, és amelyet hello feladat van konfigurálva, és lehetővé teszi, hogy hozzon létre egy lekérdezést, amely lehetővé teszi hello bemeneti adatfolyam átalakítására, toohello kimeneti keresztül küldött.
4. A hello **lekérdezés** panelen kattintson hello pontokból következő toohello `CallStream` adja meg, és válassza ki **az adatokat a bemeneti**.

    ![Menü Beállítások toouse mintaadatainak hello Streaming Analytics-feladat bejegyzést, mintaadatokkal"bemeneti" kijelölve](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)

    Ekkor megnyílik egy panel, amely lehetővé teszi, hogy mennyi minta adatok tooget, mennyi ideig tooread hello bemeneti adatfolyam definiált adja meg.

5. Állítsa be **perc** too3 majd **OK**. 
    
    ![Mintavételi hello bemeneti adatfolyam, a "3 perc" kiválasztott beállítások.](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure 3 percet értékelésével hello bemeneti adatfolyamból minták, és értesítést küld, amikor készen áll a mintaadatok hello. (Ez időt vesz igénybe egy rövid ideig.) 

hello mintaadatok ideiglenesen tárolja, és áll rendelkezésre, míg a hello lekérdezés ablak megnyitásához. Ha hello lekérdezési ablak bezárásához hello minta adatait a rendszer törli, és összekapcsolta toocreate mintaadatok új készletét. 

Alternatív megoldásként egy .JSON kiterjesztésű fájlt, amely rendelkezik a mintaadatok azt kaphat [a Githubról](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json), majd töltse fel az adott .JSON kiterjesztésű fájl toouse minta adatként történjen a hello `CallStream` bemeneti. 

### <a name="test-using-a-pass-through-query"></a>Csatlakoztatott lekérdezéssel tesztelése

Szeretne tooarchive minden eseményt, ha használható a továbbított lekérdezést tooread hello hasznos hello esemény található összes hello mezőhöz.

1. Adja meg a lekérdezés hello lekérdezési ablakban:

        SELECT 
            *
        FROM 
            CallStream

    >[!NOTE]
    >Az SQL kulcsszavak nem kis-és nagybetűket, és szóköz nincs jelentős eltérés.

    Ebben a lekérdezésben `CallStream` van alias hello, hello bemeneti létrehozásakor megadott. Ha egy másik aliast, használja az ezen a néven.

2. Kattintson a **teszt**.

    hello Stream Analytics-feladat hello mintaadatok hello lekérdezés fut, és hello kimenet megjelenítése a hello ablak hello alján. Ez jelzi, hogy a hello eseményközpont és hello Streaming Analytics-feladat helyesen vannak konfigurálva. (Amint azt később hoz létre kimeneti fogadóként adott hello lekérdezés adatokat írhat.)

    ![Stream Analytics-feladat kimeneti, előállított 73 rekordok megjelenítése](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    hello pontos látható rekordok számát a rekordok rögzítette a 3 perces minta függ.
 
### <a name="reduce-hello-number-of-fields-using-a-column-projection"></a>Csökkentse a mezőket egy oszlop leképezése hello számát

Sok esetben az elemzési hello bemeneti adatfolyam összes hello oszlopa nem szükséges. A lekérdezés tooproject kevesebb mint mezők hello csatlakoztatott lekérdezés által visszaadott is használhatja.

1. Hello lekérdezés hello kód szerkesztő toohello alábbi módosítása:

        SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
        FROM 
            CallStream

2. Kattintson a **teszt** újra. 

    ![Stream Analytics-feladat kimeneti kivetítéshez, 25 rekordot generált megjelenítő](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Count bejövő hívások régió: az összesítő Átfedésmentes ablak

Tegyük fel, hogy a bejövő hívások régiónként toocount hello száma. A streamelési adatok, ha azt szeretné, hogy a számbavételi, például tooperform aggregátumfüggvények kell toosegment hello adatfolyam historikus egységekbe (mivel hello adatfolyam maga gyakorlatilag végtelen). Ehhez használja a Streaming Analytics [ablak függvény](stream-analytics-window-functions.md). Majd dolgozhat hello adatok adott ablakra egységként.

Ez a transzformáció keresi, amely nem fedi át a historikus windows sorozatát – minden ablak adatokat csoportosíthatja, és összesítés diszkrét készlete fog rendelkezni. Ez a Windows-típus hivatkozott tooas egy *Átfedésmentes ablak* . Hello Átfedésmentes ablak, belül kaphat a hello szerint csoportosítva beérkező hívások számát `SwitchNum`, amely hello hívás származási helyének hello ország jelenti. 

1. Hello lekérdezés hello kód szerkesztő toohello alábbi módosítása:

        SELECT 
            System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
        FROM
            CallStream TIMESTAMP BY CallRecTime 
        GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum

    Ez a lekérdezés használ hello `Timestamp By` hello kulcsszót `FROM` záradék toospecify melyik hello időbélyeg mezője bemeneti adatfolyam toouse toodefine hello Átfedésmentes ablak. Ebben az esetben hello ablak elosztja hello adatok történő hello `CallRecTime` rekordokban levő mező. (Ha nincs megadva, hello leképezési művelet használja, amely minden esemény érkezik hello eseményközpont hello idő. "Érkezési idő Visual Studio alkalmazás idő" című [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx). 

    hello leképezése tartalmaz `System.Timestamp`, minden Windows hello végén időbélyeg adja vissza, amely. 

    hello használhatja, amelyet az Átfedésmentes ablak toouse toospecify, [TUMBLINGWINDOW](https://msdn.microsoft.com/library/dn835055.aspx) hello függvényt `GROUP BY `záradékban. Hello függvény egy időegységet (a mikroszekundum tooa naponta) és egy ablak mérete (darabszám) kell megadni. Ebben a példában hello Átfedésmentes ablak áll 5 másodperces időközönként, ország száma a hívások minden 5 másodperc alatt érkezett elérhetővé válik.

2. Kattintson a **teszt** újra. Hello eredmények között, figyelje meg, hogy a hello időbélyegeket **WindowEnd** 5 másodperces lépésekben vannak.

    ![Stream Analytics-feladat kimeneti összesítési, előállított 13 rekordok megjelenítése](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Önillesztés használatával SIM csalások észlelése

Például azt is fontolóra veheti csalárd használati toobe hívások oly módon hello azonos felhasználói azonban egy másik 5 másodpercen belül különböző helyeken. Például hello ugyanahhoz a felhasználóhoz szabályosan hívás sikertelen hello USA és Ausztrália: hello ugyanannyi időt vesz igénybe. 

Ezekben az esetekben a toocheck, használhat streaming adatok toojoin hello adatfolyam tooitself hello alapján hello önillesztés `CallRecTime` érték. Majd meg a hívás rögzíti, ahol hello `CallingIMSI` érték (szám származó hello) van hello azonos, de hello `SwitchNum` (származási) értéke nem hello azonos.

Használatakor a csatlakozzon a streamelési adatok hello illesztési biztosítania kell a megfelelő sorok milyen távolságban hello néhány korlátot időben választhatók el egymástól. (Ahogy azt korábban említettük, hello adatfolyam a gyakorlatilag végtelen.) hello idő értéktartományán hello kapcsolat meghatározott hello `ON` hello illesztési, hello záradékában `DATEDIFF` függvény. Ebben az esetben a hello illesztési hívás adatok 5 másodperces intervallumban alapul.

1. Hello lekérdezés hello kód szerkesztő toohello alábbi módosítása: 

        SELECT  System.Timestamp as Time, 
            CS1.CallingIMSI, 
            CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, 
            CS1.SwitchNum as Switch1, 
            CS2.SwitchNum as Switch2 
        FROM CallStream CS1 TIMESTAMP BY CallRecTime 
            JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
            ON CS1.CallingIMSI = CS2.CallingIMSI 
            AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
        WHERE CS1.SwitchNum != CS2.SwitchNum

    Ez a lekérdezés olyan, mint bármely SQL illesztési kivételével hello `DATEDIFF` hello illesztési függvényt. Egy verziója `DATEDIFF` , amely adott tooStreaming elemzés, és azt meg kell jelenniük hello `ON...BETWEEN` záradékban. egy időegységet (ebben a példában másodperc) és hello aliasok hello két forrás hello illesztéshez hello paraméterei. (Ez nem azonos a normál SQL hello `DATEDIFF` függvény.) 

    Hello `WHERE` záradék tartalmaz, amely hello csalárd hívás hello feltétel: vannak hello származó kapcsolók nem hello azonos. 

2. Kattintson a **teszt** újra. 

    ![Stream Analytics-feladat kimeneti generált önillesztés, látható 6 rekordok](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Kattintson a **Save** (Mentés) gombra. Ezáltal hello önillesztés lekérdezés hello Streaming Analytics-feladat részeként. (Hello mintaadatok nem menti azt.)

    ![A Stream Analytics-feladat mentése](./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png)

## <a name="create-an-output-sink-toostore-transformed-data"></a>Hozzon létre egy kimeneti fogadó toostore átalakított adatok

Egy esemény-adatfolyam, egy event hub bemeneti tooingest események és a lekérdezés tooperform átalakítás meghatározta hello adatfolyam keresztül. hello utolsó lépése toodefine hello feladat kimeneti fogadóként – Ez azt jelenti, hogy a hely toowrite hello átalakítva adatfolyamot. 

Kimeneti mosdók is használhatja sok erőforrást – SQL Server-adatbázis, a table storage, Data Lake-tároló, a Power BI és még egy másik eseményközpont. Ebben az oktatóanyagban hello adatfolyam tooAzure Blob-tároló, amely jellemző döntés eseményadatok újabb elemzés, mivel így alkalmazkodik a strukturálatlan adatok gyűjtéséhez fog írni.

Ha egy meglévő blob storage-fiókot, használhatja, amely. Ebben az oktatóanyagban mutat be, hogyan toocreate egy új tárolási fiók csak a jelen oktatóanyag.

### <a name="create-an-azure-blob-storage-account"></a>Azure Blob Storage-fiók létrehozása

1. Hello Azure-portálon, a visszatérési toohello Streaming Analytics-feladat panelen. (Ha korábban bezárta a hello panelen, keresse meg a `sa_frauddetection_job_demo` a hello **összes erőforrás** panel.)
2. A hello **feladat topológia** területen kattintson a hello **kimeneti** mezőbe. 
3. A hello **kimenetek** panelen kattintson a  **+ &nbsp;Hozzáadás** hello panel ezekkel az értékekkel, majd töltse ki:

    * **A kimeneti alias**: hello név használata `CallStream-FraudulentCalls`. 
    * **Gyűjtése**: válasszon **Blob-tároló**.
    * **Importálási lehetőségeit**: válasszon **használja a jelenlegi előfizetés blob-tároló**.
    * **A tárfiók**. Válassza ki **hozzon létre új tárfiókot.**
    * **A tárfiók** (második mezőben). Adja meg `YOURNAMEsademo`, ahol `YOURNAME` a neve, vagy egy másik egyedi karakterlánc. hello neve csak kisbetűket és számokat használhatja, és az Azure között egyedinek kell lennie. 
    * **Tároló**. Adja meg `sa-fraudulentcalls-demo`.
    hello tárfiók neve vagy a tároló neve használt együtt tooprovide egy URI-JÁNAK hello blob-tároló, ez például a következők: 

    `http://yournamesademo.blob.core.windows.net/sa-fraudulentcalls-demo/...`
    
    ![Panel "Új kimeneti" Stream Analytics-feladat](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png)
    
4. Kattintson a **Create** (Létrehozás) gombra. 

    Azure hello tárfiókot hoz létre, és automatikusan létrehoz egy kulcsot. 

5. Bezárás hello **kimenetek** panelen. 

## <a name="start-hello-streaming-analytics-job"></a>Hello Streaming Analytics-feladat indítása

hello feladat konfigurálva van. A megadott bemeneti (hello eseményközpont), átalakítás (lekérdezés toolook hello csalárd hívások) és egy kimeneti (blob-tároló). Most elindíthatja hello feladat. 

1. Ellenőrizze, fut-e hello TelcoGenerator alkalmazást.

2. Hello feladat panelen kattintson **Start**.

    ![Hello Stream Analytics-feladat indítása](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-output.png)

3. A hello **indítási feladat** panelen, a feladat kimenete kezdési időpontja, jelölje be a **most**. 

4. Kattintson a **Start**. 

    !["Indítsa el a feladat" panel hello Stream Analytics-feladat](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start-job-blade.png)

    Azure értesíti, amikor hello feladat elindult, és hello feladat panelen hello állapot jelenik meg **futtató**.

    ![Stream Analytics-feladat állapotát, a "Fut" megjelenítése](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-running-status.png)
    

## <a name="examine-hello-transformed-data"></a>Vizsgálja meg a hello átalakított adatok

Most már rendelkezik egy teljes Streaming Analytics-feladathoz. hello feladat telefonhívás metaadatok adatfolyam vizsgálata, valós idejű csalárd telefonhívásokat keres, és ezek csalárd hívások toostorage kapcsolatos adatok írásakor. 

toocomplete ebben az oktatóanyagban érdemes toolook hello adatait hello Streaming Analytics-feladat által. hello adatok írása tooAzure Blog tárolási az adattömbök (fájlok). Bármely Azure Blob Storage olvasó eszközt is használhatja. Amint hello Előfeltételek szakaszában, használhatja a Visual Studio Azure-bővítményeket, vagy használhatja egy eszköz, például [Azure Tártallózó](http://storageexplorer.com/) vagy [Azure Explorer](http://www.cerebrata.com/products/azure-explorer/introduction). 

A blob Storage tárolóban fájl tartalmának hello vizsgálja meg, ha valami hasonló hello:

![Az Azure blob storage Streaming Analytics kimenet](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

További cikkek hello csalás észlelés forgatókönyv folytatásához, és ebben az oktatóanyagban létrehozott hello erőforrásokat használó van. Ha azt szeretné, hogy toocontinue, lásd: hello javaslatok alapján **további lépések** később.

Azonban ha elkészült, és nem kell létrehozott hello erőforrásokat, törölheti őket, hogy a szükségtelen Azure költségek. Ebben az esetben javasoljuk, hogy Ön hello a következő:

1. Hello Streaming Analytics-feladat leállítása. A hello **feladatok** panelen kattintson a **leállítása** hello tetején.
2. Állítsa le a hello Telco készítő alkalmazást. Eredeti állapotába hello app hello parancsablakot nyomja le a Ctrl + C.
3. Ha új blob storage-fiók ebben az oktatóanyagban az imént létrehozott, törölje azt. 
4. Törölje a hello Streaming Analytics-feladathoz.
5. Hello eseményközpont törlése.
6. Hello event hub névtér törlése.

## <a name="get-support"></a>Támogatás kérése

Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Következő lépések

Ez az oktatóanyag folytatása hello a következő cikkeket:

* [A Stream Analytics és a Power BI: az adatfolyamként történő adatok valós idejű elemzési irányítópult](stream-analytics-power-bi-dashboard.md). Ez a cikk bemutatja, hogyan toosend hello hello Stream Analytics TelCo kimenete feladat tooPower BI valós idejű képi megjelenítés és elemzésére.
* [Hogyan toostore adatait az Azure Redis Cache használata az Azure Functions az Azure Stream Analytics](stream-analytics-functions-redis.md). Ez a cikk bemutatja, hogyan toouse Azure Functions toowrite csalárd meghívja tooan Azure Redis cache-keresztül a Service Bus-üzenetsorba.

További információ a Stream Analytics általában próbálja ki az alábbi cikkek:

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

---
title: "az IoT-megoldás a Stream Analytics aaaBuild |} Microsoft Docs"
description: "Első lépések útmutató hello Stream Analytics IoT-megoldás az őrbódét forgatókönyv"
keywords: "IOT-megoldás, ablakban funkciók"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: a473ea0a-3eaa-4e5b-aaa1-fec7e9069f20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: e37fc5b56c4ffc4a2d7b820afe0c17631e577ea0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-an-iot-solution-by-using-stream-analytics"></a>Az IoT-megoldás létrehozása a Stream Analytics segítségével
## <a name="introduction"></a>Bevezetés
Ebből az oktatóanyagból megtudhatja, hogyan toouse Azure Stream Analytics tooget valós idejű elemzése az adatokat. A fejlesztők könnyedén kombinálhatja adatstreamek, például kattintás-adatfolyamok, a naplókat és az eszköz által létrehozott események előzményrekordjaira vagy referencia adatok tooderive üzleti elemzések készítése. Teljes körűen felügyelt, valós idejű stream számítási szolgáltatás, amely a Microsoft Azure-ban Azure Stream Analytics biztosít beépített hibatűrési, alacsony késéssel és méretezhetőség tooget másolatot, és fut-e percben.

Ez az oktatóanyag befejezése után fogja tudni:

* Ismerkedjen meg hello Azure Stream Analytics-portálról.
* Konfigurálhatja és telepítheti a folyamatos átviteli feladatnak.
* Fogalmazza meg valós problémákat és azok megoldási hello Stream Analytics lekérdezési nyelv használatával.
* Hogyan megoldások streaming az ügyfeleknek a Stream Analytics az vetett bizalmat.
* Figyelés és naplózás élmény tootroubleshoot problémák hello használata.

## <a name="prerequisites"></a>Előfeltételek
Akkor lesz kell hello Előfeltételek toocomplete követően ez az oktatóanyag:

* hello legújabb verziójának [Azure PowerShell](/powershell/azure/overview)
* A Visual Studio 2017, 2015, vagy szabad hello [Visual Studio Community](https://www.visualstudio.com/products/visual-studio-community-vs.aspx)
* Egy [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/)
* Hello számítógépen rendszergazdai jogosultságokkal
* Töltse le a [TollApp.zip](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) a hello Microsoft Download Center
* Választható lehetőség: A hello TollApp esemény generátor a forráskód [GitHub](https://aka.ms/azure-stream-analytics-toll-source)

## <a name="scenario-introduction-hello-toll"></a>A forgatókönyv bevezető: "Hello, téren!"
Téren állomás egy közös jelenség. Tapasztal őket több gyorsforgalmi, hidak és alagutakon keresztül hello world. Minden téren állomás van több téren fülkéit foglalja magában. A manuális fülkéit foglalja magában toopay hello téren tooan kísérő leállítása. Automatikus fülkéit foglalja magában: felett minden kiállítási érzékelő egy RFID kártya, amely elhelyezett toohello szélvédőkeret, a vehicle, ha hello téren kiállítási vizsgálatokat végez. Egy esemény adatfolyamként, amelyben érdekes művelet végrehajtható is könnyen toovisualize hello áthaladását járművekről gyűjtött ezek téren az állomások keresztül.

![Kép autók téren fülkéit foglalja magában:](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image1.jpg)

## <a name="incoming-data"></a>Bejövő adatforgalom
Ez az oktatóanyag két adatstreamek működik. Érzékelők telepített hello be, és kilépési hello téren állomások hello első adatfolyam eredményez. második hello adatfolyama-statikus keresési adatkészletre mutató vehicle regisztrációs adatokat.

### <a name="entry-data-stream"></a>Bejegyzés adatfolyam
hello bejegyzés adatfolyam autók információkat tartalmaz, téren állomások kapunál.

| TollID | EntryTime | LicensePlate | Állapot | Ellenőrizze | Modell | VehicleType | VehicleWeight | Téren | Címke |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 |2014-09-10 12:01:00.000 |7001 JNB |NY |Honda |CRV |1 |0 |7 | |
| 1 |2014-09-10 12:02:00.000 |1001 YXZ |NY |Toyota |Camry |1 |0 |4 |123456789 |
| 3 |2014-09-10 12:02:00.000 |1004 ABC |KI |Ford |Taurus |1 |0 |5 |456789123 |
| 2 |2014-09-10 12:03:00.000 |XYZ 1003 |KI |Toyota |Corolla |1 |0 |4 | |
| 1 |2014-09-10 12:03:00.000 |1007 BNJ |NY |Honda |CRV |1 |0 |5 |789123456 |
| 2 |2014-09-10 12:05:00.000 |1007 CDE |NJ |Toyota |4 x 4 |1 |0 |6 |321987654 |

Hello oszlopok rövid leírása itt található:

| Oszlop | Leírás |
| --- | --- |
| TollID |hello téren kiállítási azonosítója, amely egyedileg azonosít egy téren kiállítási |
| EntryTime |hello dátum és idő hello vehicle toohello téren kiállítási UTC-bejegyzés |
| LicensePlate |hello vehicle hello licenc lemez számát |
| Állapot |Az Amerikai Egyesült Államokban állapotának |
| Ellenőrizze |hello autó hello gyártója |
| Modell |hello autó hello modell száma |
| VehicleType |Utas járművekről gyűjtött vagy a kereskedelmi járművekről gyűjtött 2 vagy 1 |
| WeightType |Vehicle súly tonna; az utasok járművekről gyűjtött 0 |
| Téren |USD hello téren értéke |
| Címke |hello e-címke, amely automatizálja a fizetési; hello autó üres, ha hello fizetési manuálisan végezhető el |

### <a name="exit-data-stream"></a>Kilépés adatfolyam
hello kilépési adatfolyam autók hello téren állomás Kilépés adatait tartalmazza.

| **TollId** | **ExitTime** | **LicensePlate** |
| --- | --- | --- |
| 1 |2014-09-10T12:03:00.0000000Z |7001 JNB |
| 1 |2014-09-10T12:03:00.0000000Z |1001 YXZ |
| 3 |2014-09-10T12:04:00.0000000Z |1004 ABC |
| 2 |2014-09-10T12:07:00.0000000Z |XYZ 1003 |
| 1 |2014-09-10T12:08:00.0000000Z |1007 BNJ |
| 2 |2014-09-10T12:07:00.0000000Z |1007 CDE |

Hello oszlopok rövid leírása itt található:

| Oszlop | Leírás |
| --- | --- |
| TollID |hello téren kiállítási azonosítója, amely egyedileg azonosít egy téren kiállítási |
| ExitTime |hello dátum és idő hello vehicle elhagyásakor téren kiállítási UTC szerint |
| LicensePlate |hello vehicle hello licenc lemez számát |

### <a name="commercial-vehicle-registration-data"></a>Kereskedelmi vehicle regisztrációs adatok
hello oktatóprogram egy kereskedelmi vehicle adatbázist statikus pillanatképet.

| LicensePlate | RegistrationId | Lejárt |
| --- | --- | --- |
| SVT 6023 |285429838 |1 |
| XLZ 3463 |362715656 |0 |
| 1005 BIZTONSÁGI |876133137 |1 |
| RIV 8632 |992711956 |0 |
| SNY 7188 |592133890 |0 |
| ELH 9896 |678427724 |1 |

Hello oszlopok rövid leírása itt található:

| Oszlop | Leírás |
| --- | --- |
| LicensePlate |hello vehicle hello licenc lemez számát |
| RegistrationId |hello vehicle regisztrációs azonosítója |
| Lejárt |regisztrációs állapotát hello vehicle hello: 0, ha a vehicle regisztrációs aktív, ha lejárt a regisztrációs 1 |

## <a name="set-up-hello-environment-for-azure-stream-analytics"></a>Az Azure Stream Analytics hello környezet beállítása
toocomplete ebben az oktatóanyagban egy Microsoft Azure-előfizetés szükséges. A Microsoft biztosít az ingyenes Microsoft Azure-szolgáltatásokhoz.

Ha még nem rendelkezik Azure-fiókot, akkor [ingyenes próbaverzióját kérelem](http://azure.microsoft.com/pricing/free-trial/).

> [!NOTE]
> toosign az ingyenes próbaverzió szolgáltatáshoz, a mobileszközök, amelyek fogadhat szöveges üzeneteinek és érvényes hitelkártyát kell.
> 
> 

Győződjön meg arról, hogy toofollow hello hello "Törlése az Azure-fiók" szakasz ebben a cikkben hello végén lévő lépéseket, így biztosíthatja, hogy az Azure-kreditjeinek hello lehető legjobb felhasználását.

## <a name="provision-azure-resources-required-for-hello-tutorial"></a>Hello az oktatóanyaghoz szükség Azure-erőforrások kiépítése
Ez az oktatóanyag igényel a két event hubs tooreceive *bejegyzés* és *kilépéshez* adatfolyamokat. Az Azure SQL Database hello eredményeit hello Stream Analytics-feladatok kimenete. Az Azure Storage a vehicle regisztrációjával kapcsolatos hivatkozási adatokat tárolja.

Használhatja hello Setup.ps1 parancsfájl hello TollApp mappában a Githubon toocreate minden szükséges erőforrásokat. Hello érdekében idő azt javasoljuk, hogy kell futtatnia. Ha azt szeretné, hogy hogyan tooconfigure hello Azure-portálon erőforrásainak tekintse meg a vonatkozó további toolearn toohello "Oktatóanyag erőforrások konfigurálása az Azure-portálon a" függelék.

Töltse le és mentse a hello támogató [TollApp](https://github.com/Azure/azure-stream-analytics/blob/master/Samples/TollApp/TollApp.zip) mappa és a fájlokat.

Nyissa meg a **Microsoft Azure PowerShell** ablak *rendszergazdaként*. Ha még nem rendelkezik Azure PowerShell, kövesse a hello utasításait [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview) tooinstall azt.

Mivel a Windows automatikusan blokkolja .ps1 .dll vagy .exe fájlok, meg kell tooset hello végrehajtási házirend hello parancsfájl futtatása előtt. Ellenőrizze, fut-e hello Azure PowerShell ablak *rendszergazdaként*. Futtatás **Set-ExecutionPolicy unrestricted**. Amikor a rendszer kéri, írja be a **Y**.

![Képernyőkép a "Set-ExecutionPolicy unrestricted" Azure PowerShell ablakban fut](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image2.png)

Futtatás **Get-ExecutionPolicy** toomake meg arról, hogy működőképes-e hello parancsot.

![Képernyőkép a "Get-ExecutionPolicy" Azure PowerShell ablakban fut](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image3.png)

Nyissa meg megegyező hello parancsfájlok és készítő alkalmazás toohello könyvtárat.

![Képernyőkép a "cd .\TollApp\TollApp" hello Azure PowerShell ablakban fut](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image4.png)

Típus **.\\ Setup.ps1** tooset Azure-fiókja, hozzon létre és konfigurálja az összes szükséges erőforrásokat, és indítsa el a toogenerate események. hello parancsfájl véletlenszerűen szerzi be a régió toocreate az erőforrások. tooexplicitly adjon meg egy régiót, hello átadhatók **-hely** paraméter a következő példa hello hasonlóan:

**. \\Setup.ps1-helyre "USA középső RÉGIÓJA"**

![Az Azure-bejelentkezés hello oldalát bemutató képernyőkép](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image5.png)

hello parancsfájl megnyitja hello **bejelentkezés** Microsoft Azure oldalán. Adja meg a fiók hitelesítő adatait.

> [!NOTE]
> Ha a fiókjának van hozzáférési toomultiple előfizetések, fogja ismételt tooenter hello előfizetés nevét, amelyet az toouse hello az oktatóanyaghoz.
> 
> 

hello parancsfájlt is igénybe vehet néhány percet toorun. Már befejezte a hello kimenete a következő képernyőkép hello hasonlóan kell kinéznie.

![Képernyőfelvétel a hello parancsfájl hello Azure PowerShell-ablakban](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image6.PNG)

Ezenkívül megjelenik egy másik ablak, amely a következő képernyőkép hasonló toohello. Ez az alkalmazás küld események tooAzure Event Hubs, vagyis szükséges toorun hello oktatóanyag. Tehát nem hello állítsa vagy az ablak bezárása hello az oktatóanyag befejezése előtt.

![Képernyőkép a "Küldő event hub adatok"](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image7.png)

Meg kell tudni toosee az erőforrások az Azure portálon most. Nyissa meg túl<https://portal.azure.com>, és jelentkezzen be a fiók hitelesítő adataival. Vegye figyelembe, hogy jelenleg bizonyos funkciók használja hello klasszikus portálon. Ezeket a lépéseket egyértelműen jelzik.

### <a name="azure-event-hubs"></a>Azure Event Hubs
Hello Azure-portálon, kattintson **további szolgáltatások** hello felügyeleti bal oldali ablaktábla hello alján. Típus **az Event hubs** a hello mezőbe, és kattintson a **az Event hubs**. Megnyílik egy új böngészőt ablak toodisplay hello **SERVICE BUS** hello területén **klasszikus portál**. Itt látható hello hello Setup.ps1 parancsfájl által létrehozott Eseményközpont.

![Service Bus](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image8.png)

Kattintson egy kezdetű hello *tolldata*. Kattintson a hello **EVENT HUBS** fülre. Látni fogja a két az event hubs nevű *bejegyzés* és *kilépéshez* létre ebben a névtérben.

![Event Hubs lapon hello klasszikus portál](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image9.png)

### <a name="azure-storage-container"></a>Az Azure Storage-tároló áttekintése
1. Lépjen vissza a böngészőben nyissa meg tooAzure portál toohello fülre. Kattintson a **tárolási** a hello bal oldalán található hello Azure portál toosee hello Azure tároló hello az oktatóanyagban használt.
   
    ![Tárolási menüpont](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image11.png)
2. Kattintson egy kezdődő hello *tolldata*. Kattintson a hello **TÁROLÓK** lapon toosee hello létre tárolót.
   
    ![Tárolók lapján hello Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image10.png)
3. Kattintson a hello **tolldata** tároló toosee hello vehicle regisztrációs adatokat tartalmazó JSON-fájl feltöltése.
   
    ![Képernyőfelvétel a hello registration.json fájl hello tárolóban](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image12.png)

### <a name="azure-sql-database"></a>Azure SQL Database
1. Lépjen vissza Azure-portálon toohello hello böngészőben megnyitott hello első lapján. Kattintson a **SQL-ADATBÁZISOK** hello bal oldalán található hello Azure portál toosee hello SQL-adatbázis, amely hello oktatóprogram használható, és kattintson a **tolldatadb**.
   
    ![Képernyőfelvétel a hello SQL-adatbázis létrehozása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15.png)
2. Másolás hello kiszolgáló neve nélkül hello portszámot (*kiszolgálónév*. database.windows.net, például).
    ![Képernyőfelvétel a hello SQL-adatbázis létrehozása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image15a.png)

## <a name="connect-toohello-database-from-visual-studio"></a>Toohello adatbázis csatlakoztatja a Visual Studio
Használja a Visual Studio tooaccess lekérdezési eredmények hello kimeneti adatbázisban.

Csatlakozás toohello SQL-adatbázis (hello cél) a Visual Studio eszközből:

1. Nyissa meg a Visual Studio, és kattintson a **eszközök** > **tooDatabase csatlakozás**.
2. Kattintson az **Microsoft SQL Server** adatforrásként.
   
    ![Változás adatforrás párbeszédpanel](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image16.png)
3. A hello **kiszolgálónév** mezőbe illessze be az előző szakaszban hello fájlból másolt hello Azure-portálon hello nevét (Ez azt jelenti, hogy *kiszolgálónév*. database.windows.net).
4. Kattintson a **SQL Server-hitelesítés használható**.
5. Adja meg **tolladmin** a hello **felhasználónév** mező és **123toll!** a hello **jelszó** mező.
6. Kattintson a **válasszon vagy adjon meg egy adatbázisnevet**, és válassza ki **TollDataDB** hello adatbázisként.
   
    ![Kapcsolat párbeszédpanel hozzáadása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image17.jpg)
7. Kattintson az **OK** gombra.
8. Nyissa meg a Server Explorer.
   
    ![Server Explorer (Kiszolgálókezelő)](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image18.png)
9. Lásd: hello TollDataDB adatbázis négy tábláit.
   
    ![Hello TollDataDB adatbázis](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image19.jpg)

## <a name="event-generator-tollapp-sample-project"></a>Esemény generátor: TollApp mintaprojektet
PowerShell parancsfájl hello automatikusan elindul, toosend események hello TollApp minta alkalmazás használatával. Tooperform nincs szükség további lépésekre.

Azonban ha érdekli megvalósítás részletei, megtalálhatja hello TollApp alkalmazás forráskódját hello github [minták/TollApp](https://aka.ms/azure-stream-analytics-toll-source).

![Képernyőkép a jelenik meg a Visual Studio mintakód](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image20.png)

## <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása
1. A hello Azure-portálon kattintson egy új Stream Analytics-feladat hello zöld plusz jel hello lap toocreate hello bal felső sarkában. Válassza ki **Eszközintelligencia + analitika** majd **Stream Analytics-feladat**.
   
    ![Új gomb](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image21.png)
2. Adja meg a feladat nevét, hello előfizetések, javítsa ki, és hozzon létre egy új erőforráscsoportot a hello hello Event hub tárolási és ugyanabban a régióban (alapértelmezett érték déli középső Régiójában hello parancsfájl).
3. Kattintson a **PIN-kód toodashboard** , majd **létrehozása** hello lap hello alján.
   
    ![A Stream Analytics-feladat beállítás létrehozása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image22.png)

## <a name="define-input-sources"></a>Adja meg a bemeneti források
1. hello feladat létrehoz és hello feladat lap megnyitásához. Vagy analytics-feladat létrehozása a portál irányítópultján hello hello is kattinthat.

2. Kattintson a hello **BEMENETEK** toodefine hello forrásadatok fülre.
   
    ![hello bemeneti adatok lap](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image24.png)
3. Kattintson a **vegye fel a bemeneti**.
   
    ![hello Hozzáadás bemeneti lehetősége](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image25.png)
4. Adja meg **EntryStream** , **bemeneti ÁLJEL**.
5. Forrástípus **adatfolyam**
6. Forrás **eseményközpont**.
7. **Service bus-névtér** TollData hello a legördülő listán hello kell lennie.
8. **Eseményközpont neveként** túl meg kell**bejegyzés**.
9. **Eseményközpont házirend neve*van **RootManageSharedAccessKey** (hello az alapértelmezett érték).
10. Válassza ki **JSON** a **esemény SZERIALIZÁLÁSI formátum** és **UTF8** a **KÓDOLÁS**.
   
    A beállítások hasonlóan fog kinézni:
   
    ![Eseményközpont-beállítások](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image28.png)

10. Kattintson a **létrehozása** hello toofinish hello varázsló hello alján.
    
    Most, hogy a létrehozott hello bejegyzés adatfolyam, követi hello azonos lépéseket toocreate hello kilépési adatfolyam. Lehet, hogy tooenter értékek hello a következő képernyőkép a.
    
    ![Hello kilépési adatfolyam beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image31.png)
    
    Két bemeneti adatfolyamok definiált:
    
    ![A bemeneti adatfolyam definiált hello Azure-portálon](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image32.png)
    
    A következő adhat referenciaadat-bemenetek hello blob fájl, amely tartalmazza a car regisztrációs adatokat.
11. Kattintson a **hozzáadása**, és kövesse a hello azonos hello adatfolyam-bemenet esetén, de válassza **REFERENCIAADATOK** helyett **adatfolyam** és hello **bemeneti Alias**  van **regisztrációs**.

12. tárfiók kezdetű **tolldata**. hello Tárolónév kell **tolldata**, és hello **elérési út mintája** kell **registration.json**. Ezt a fájlnevet és nagybetűk különbözőnek kell lennie **kis**.
    
    ![Blog tárolási beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image34.png)
13. Kattintson a **létrehozása** toofinish hello varázsló.

Most már az összes bemenet vannak definiálva.

## <a name="define-output"></a>Adja meg a kimeneti
1. Hello Stream Analytics feladat áttekintés ablaktábláján válassza **KIMENETEK**.
   
    ![hello kimenet lapon és a "Kimenetnek hozzáadása" lehetőséget](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image37.png)
2. Kattintson az **Add** (Hozzáadás) parancsra.
3. Set hello **kimeneti alias** too'output ", majd **gyűjtése** túl**SQL-adatbázis**.
3. Jelölje ki "Connect tooDatabase a Visual Studio" című szakaszban hello hello használt hello kiszolgálónevet. hello adatbázisnév **TollDataDB**.
4. Adja meg **tolladmin** a hello **felhasználónév** mezőben **123toll!** a hello **jelszó** mező, és **TollDataRefJoin** a hello **tábla** mező.
   
    ![SQL-adatbázis beállításai](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image38.png)
5. Kattintson a **Create** (Létrehozás) gombra.

## <a name="azure-stream-analytics-query"></a>Az Azure Stream analytics-lekérdezés
Hello **lekérdezés** lap tartalmaz egy SQL-lekérdezést, hogy átalakítások hello bejövő adatok.

![A lekérdezés hozzáadott toohello lekérdezés lap](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image39.png)

Ez az oktatóanyag több üzleti kérdéseket kapcsolódó tootoll adatok és szerkezetek Stream Analytics-lekérdezéseket is használható az Azure Stream Analytics tooprovide tooanswer kísérel meg egy megfelelő választ.

Az első Stream Analytics-feladat indítása előtt most megismerkedhet néhány forgatókönyvek és hello lekérdezés szintaxisa.

## <a name="introduction-tooazure-stream-analytics-query-language"></a>Bevezetés tooAzure Stream Analytics lekérdezési nyelv
- - -
Tegyük fel, hogy kell-e, adjon meg egy téren kiállítási járművekről gyűjtött toocount hello száma. Mivel ez egy állandó folyama miatt események, hogy toodefine egy "időszakának idő." "Hány járművekről gyűjtött adja meg egy téren kiállítási három percenként?" hello kérdés toobe módosítsa. Ez a leggyakrabban hivatkozott tooas hello átfedésmentes száma.

Nézzük hello Azure Stream Analytics-lekérdezés, amely a kérdésre választ:

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*) AS Count
    FROM EntryStream TIMESTAMP BY EntryTime
    GROUP BY TUMBLINGWINDOW(minute, 3), TollId

Ahogy látja, az Azure Stream Analytics használ lekérdezésnyelvet, amely például az SQL, és néhány bővítmények toospecify idővel kapcsolatos szempontok hello lekérdezés hozzáadja.

További részletekért olvassa el [Időkezelést](https://msdn.microsoft.com/library/azure/mt582045.aspx) és [Ablakozó](https://msdn.microsoft.com/library/azure/dn835019.aspx) MSDN hello lekérdezésben használt szerkezeteket.

## <a name="testing-azure-stream-analytics-queries"></a>Tesztelés Azure Stream Analytics-lekérdezések
Most, hogy az első Azure Stream Analytics lekérdezési írt, akkor idő tootest mintaadatfájlok használatával azt az elérési út a következő hello TollApp mappájában található:

**.. \\TollApp\\TollApp\\adatok**

Ez a mappa tartalmaz a következő fájlok hello:

* Entry.JSON
* Exit.JSON
* Registration.JSON

## <a name="question-1-number-of-vehicles-entering-a-toll-booth"></a>1. kérdés: Sűrítéses egy téren kiállítási száma
1. Nyissa meg a hello Azure-portálon, és lépjen tooyour létrehozott Azure Stream Analytics-feladat. Kattintson a hello **lekérdezés** lapra, és illessze be az előző szakasz hello lekérdezést.

2. toovalidate szimbólum, majd válassza a mintaadatok, hello adatok feltöltése a hello EntryStream bemeneti kattintva lekérdezése hello... **tölthet fel fájlból adatot**.

    ![Képernyőfelvétel a hello Entry.json fájl](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image41.png)
3. Hello ablaktáblán válassza hello fájl (Entry.json) a helyi számítógép kattintson megjelenő **OK**. Hello **teszt** ikon fog most megvilágítására és kattintható lehet.
   
    ![Képernyőfelvétel a hello Entry.json fájl](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image42.png)
3. Ellenőrizze, hogy hello hello lekérdezés eredménye a várt módon:
   
    ![Hello teszt eredményei](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image43.png)

## <a name="question-2-report-total-time-for-each-car-toopass-through-hello-toll-booth"></a>2. kérdés: Jelentés fordított időt minden car toopass hello téren kiállítási keresztül
hello átlagos idő, amely egy autó toopass hello téren keresztül a szükséges tooassess hello hatékonyságát hello folyamat és a felhasználói élmény hello segítségével.

toofind hello teljes idő, toojoin hello EntryTime adatfolyam hello ExitTime adatfolyam van szüksége. Hello adatfolyamok TollId és LicencePlate oszlopokon csatlakozni fog. Hello **csatlakozás** operátor szükséges toospecify historikus eltérést hello elfogadható idő hello különbségének csatlakoztatott események ismerteti. Használandó **DATEDIFF** működéséhez toospecify, hogy az események egymástól legfeljebb 15 percen kell lennie. Hello is alkalmazási **DATEDIFF** függvény tooexit és időpontokban toocompute hello tényleges idő egy autó tölt hello bejegyzés toll állomás. Vegye figyelembe a hello használata hello különbségét **DATEDIFF** használat esetén a egy **kiválasztása** utasítás helyett egy **csatlakozás** feltétel.

    SELECT EntryStream.TollId, EntryStream.EntryTime, ExitStream.ExitTime, EntryStream.LicensePlate, DATEDIFF (minute , EntryStream.EntryTime, ExitStream.ExitTime) AS DurationInMinutes
    FROM EntryStream TIMESTAMP BY EntryTime
    JOIN ExitStream TIMESTAMP BY ExitTime
    ON (EntryStream.TollId= ExitStream.TollId AND EntryStream.LicensePlate = ExitStream.LicensePlate)
    AND DATEDIFF (minute, EntryStream, ExitStream ) BETWEEN 0 AND 15

1. tootest ebben a lekérdezésben frissítés hello lekérdezése hello **lekérdezés** hello feladat. Adja hozzá a hello tesztfájl **ExitStream** ugyanúgy, mint az **EntryStream** fent lett megadva.
   
2. Kattintson a **teszt**.

3. Válassza ki a hello jelölőnégyzetet tootest hello lekérdezés és a nézet hello kimenete:
   
    ![Kimeneti hello vizsgálat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image45.png)

## <a name="question-3-report-all-commercial-vehicles-with-expired-registration"></a>3. kérdés: Jelentés minden, kereskedelmi járművekről gyűjtött lejárt regisztrálása
Az Azure Stream Analytics statikus pillanatkép-készítési adatok toojoin használható historikus adatfolyamokat. toodemonstrate Ez a funkció használatát hello a következő minta kérdést.

Ha egy kereskedelmi vehicle regisztrálva van a hello téren vállalati, azt is továbbíthatja hello téren kiállítási nélkül lett a vizsgálathoz. Kereskedelmi Vehicle regisztrációs keresési tábla tooidentify összes kereskedelmi járművekről gyűjtött regisztráció lejárt fogja használni.

```
SELECT EntryStream.EntryTime, EntryStream.LicensePlate, EntryStream.TollId, Registration.RegistrationId
FROM EntryStream TIMESTAMP BY EntryTime
JOIN Registration
ON EntryStream.LicensePlate = Registration.LicensePlate
WHERE Registration.Expired = '1'
```

tootest hivatkozási adatok lekérdezéséhez, szükség van egy bemeneti forrás toodefine hello referenciaadatok, amely már elvégezte a.

tootest Ez a lekérdezés, Beillesztés hello lekérdezést a hello **lekérdezés** lapra, majd **teszt**, és adja meg a hello két beviteli móddal, hello regisztrációs az adatokat, majd kattintson **teszt**.  
   
![Kimeneti hello vizsgálat](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image46.png)

## <a name="start-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat indítása
Most is idő toofinish hello konfigurációs és kezdő hello feladat. A kérdés 3, amely, hogy megfelel hello hello sémája kimeneti hello lekérdezés mentése **TollDataRefJoin** kimeneti táblához.

Nyissa meg toohello feladat **IRÁNYÍTÓPULT**, és kattintson a **START**.

![Képernyőfelvétel a hello Start gomb hello feladat irányítópult](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image48.png)

A hello párbeszédpanel, módosítsa a hello **START kimeneti** idő túl**egyéni idő**. Hello óra tooone óra hello aktuális ideje előtt módosíthatja. Ez a módosítás biztosítja, hogy a hello eseményközpont összes események feldolgozása hello oktatóanyag elején hello toogenerate hello események indítása óta. Most kattintson hello **Start** gomb toostart hello feladat.

![Egyéni idő kiválasztása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image49.png)

Kezdő hello feladat néhány percet is igénybe vehet. A Stream Analytics hello legfelső szintű lapon látható hello állapota.

![Képernyőfelvétel a hello hello feladat állapota](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image50.png)

## <a name="check-results-in-visual-studio"></a>A Visual Studio ellenőrzésének az eredménye
1. Nyissa meg a Visual Studio Server Explorer, és kattintson a jobb gombbal a hello **TollDataRefJoin** tábla.
2. Kattintson a **tábla adatok megjelenítése** toosee hello kimenetét a feladatot.
   
    !["Tábla adatok megjelenítése" a Server Explorer kiválasztása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image51.jpg)

## <a name="scale-out-azure-stream-analytics-jobs"></a>Horizontális felskálázás az Azure Stream Analytics feladatok
Az Azure Stream Analytics célja tooelastically méretezni, hogy a nagy mennyiségű adat is képes kezelni. hello Azure Stream Analytics lekérdezési használhatja egy **PARTITION BY** záradék tootell hello rendszer, hogy ez a lépés ki lesz skálázva. **PartitionId** a rendszer hello különleges oszlopot ad hozzá toomatch hello Partícióazonosító hello bemeneti (eseményközpont).

    SELECT TollId, System.Timestamp AS WindowEnd, COUNT(*)AS Count
    FROM EntryStream TIMESTAMP BY EntryTime PARTITION BY PartitionId
    GROUP BY TUMBLINGWINDOW(minute,3), TollId, PartitionId

1. Hello aktuális feladat leállításával, frissítés hello lekérdezést hello **lekérdezés** fülre, majd nyissa meg hello **beállítások** áttételi hello feladat irányítópulton. Kattintson a **méretezési**.
   
    **ADATFOLYAM-egységek** határozza meg, hogy a feladat hello számítási teljesítmény mennyisége hello fogadására.
2. Változás hello legördülő lista 1-6.
   
    ![Képernyőkép a 6 adatfolyam-továbbítási egység kiválasztása](media/stream-analytics-build-an-iot-solution-using-stream-analytics/image52.png)
3. Nyissa meg toohello **KIMENETEK** lapon, és módosítsa a hello hello SQL táblázat neve túl**TollDataTumblingCountPartitioned**.

Ha először hello feladat most, Azure Stream Analytics munkahelyi szét több számítási erőforrással is javítható az átviteli teljesítmény. Vegye figyelembe, hogy hello TollApp alkalmazás is küld események által TollId particionálva.

## <a name="monitor"></a>Figyelés
Hello **FIGYELŐ** terület tartalmazza a feladat futtatásával hello statisztikája. Első alkalommal konfiguráció szükséges hello toouse hello storage-fiók ugyanabban a régióban (például a dokumentum többi hello neve nem ingyenes).   

![A figyelő képernyőképe](media/stream-analytics-build-an-iot-solution-using-stream-analytics/monitoring.png)

Van-e hozzáférési **tevékenységi naplóit** hello feladat irányítópultról **beállítások** terület is.


## <a name="conclusion"></a>Összegzés
Ez az oktatóanyag bevezetett toohello Azure Stream Analytics szolgáltatás. Azt mutatja, hogyan tooconfigure bemenetekhez és kimenetekhez a hello Stream Analytics-feladat. Hello oktatóanyag viszonylag hello téren forgatókönyvet használja, a problémák merülhetnek fel az Azure Stream Analytics egyszerű SQL-szerű lekérdezéseket mozgásérzékelési, és hogyan azok megoldhatók adatok hello területen található általános típusú. hello oktatóanyag leírt SQL bővítmény szerkezetek az ideiglenes adatai. Bemutatta, hogyan toojoin adatfolyamot, valamint hogyan hello adatfolyam tooenrich statikus referenciaadatokkal, és milyen tooscale ki a lekérdezés tooachieve nagyobb átviteli sebességgel.

Ez az oktatóanyag egy jó bevezetést tartalmaz, bár nincs kész bármilyen módon. Hello SAQL nyelv használatával további lekérdezési mintáinak található [gyakori Stream Analytics használati minták példák lekérdezése](stream-analytics-stream-analytics-query-patterns.md).
Tekintse meg a toohello [online dokumentáció](https://azure.microsoft.com/documentation/services/stream-analytics/) további információk az Azure Stream Analytics toolearn.

## <a name="clean-up-your-azure-account"></a>Az Azure-fiók törlése
1. Állítsa le az Azure-portálon hello hello Stream Analytics-feladat.
   
    hello Setup.ps1 parancsfájl létrehoz két eseményközpontokhoz és SQL-adatbázis. a következő utasításokat súgó hello az oktatóanyag végén hello távolítja el az erőforrások hello.
2. A PowerShell ablakban írja be a **.\\ Cleanup.ps1** toostart hello a parancsfájlt, amely törli a hello az oktatóanyagban használt erőforrások.
   
   > [!NOTE]
   > Erőforrások hello neve azonosítja. Győződjön meg arról, hogy az egyes elemek eltávolításának megerősítése előtt gondosan ellenőrizze.
   > 
   > 



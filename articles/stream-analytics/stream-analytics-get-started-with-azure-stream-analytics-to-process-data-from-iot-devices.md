---
title: "aaaIoT valós idejű adatfolyamokat és Azure Stream Analytics |} Microsoft Docs"
description: "IoT-érzékelőcímkék és -adatfolyamok streamelemzéssel és valós idejű adatfeldolgozással"
keywords: "iot-megoldás, bevezetés az iot használatába"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 3e829055-75ed-469f-91f5-f0dc95046bdb
ms.service: stream-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 422e6b719d0289880aa7f17fdc585e2b768c63d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-stream-analytics-tooprocess-data-from-iot-devices"></a>Ismerkedés az Azure Stream Analytics tooprocess adatokat az IoT-eszközök
Ebből az oktatóanyagból megtudhatja, hogyan toocreate adatfolyam-logika toogather adatainak feldolgozása az eszközök internetes hálózatát (IoT) eszközökről. Egy valódi, az eszközök internetes hálózatát (IoT) használata eset toodemonstrate használjuk hogyan toobuild a megoldást gyorsan és gazdaságosan.

## <a name="prerequisites"></a>Előfeltételek
* [Azure-előfizetés](https://azure.microsoft.com/pricing/free-trial/)
* A mintalékérdezés és a mintaadatfájlok letölthetők a [GitHubból](https://aka.ms/azure-stream-analytics-get-started-iot)

## <a name="scenario"></a>Forgatókönyv
Contoso, amely a vállalat hello ipari automatizálás területén, rendelkezik teljesen automatizált a gyártási folyamatait. a gyár hello gépek, amelyek képesek valós idejű adatstreameket kibocsátó érzékelők rendelkezik. Ebben a forgatókönyvben a termelési szint egyik igazgatója valós idejű elemzése toohave hello érzékelő adatokat toolook szeretne beállítani a minták, és tegye őket. Hello Stream Analytics Query Language (SAQL) keresztül hello érzékelő adatokat toofind érdekes szabályszerűségeket hello bejövő adatfolyam adatok használjuk.

Itt az adatokat egy Texas Instrument Sensor Tag eszköz állítja elő.

![Texas Instruments Sensor Tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

hello hasznos hello adatok JSON formátumban van, és a következőhöz hasonló hello:

    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

A valós forgatókönyvekben több száz ilyen érzékelő állíthat elő eseményeket streamként. Ideális esetben egy átjáróeszköz futna kód toopush ezeket az eseményeket túl[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) vagy [Azure IoT-központok](https://azure.microsoft.com/services/iot-hub/). A Stream Analytics-feladat volna ezeket az eseményeket az Event Hubs betöltési és hello adatfolyamok valós idejű elemzési lekérdezések futtatása. Ezután küldenie hello eredmények tooone a hello [kimenetek támogatott](stream-analytics-define-outputs.md).

A használat megkönnyítése érdekében ez a Kezdeti lépések útmutató valódi SensorTag eszközökről származó mintaadatfájlokat biztosít. Hello mintaadatok kapcsolatos lekérdezések futtatása, és tekintse meg a eredményeket. A következő útmutatókból megtudhatja, hogyan tooconnect a feladat-tooinputs és kimenetekhez, és telepítheti azokat toohello Azure-szolgáltatás.

## <a name="create-a-stream-analytics-job"></a>Stream Analytics-feladat létrehozása
1. A hello [Azure-portálon](http://portal.azure.com), kattintson a hello plusz jel, és írja be **STREAM ANALYTICS** a hello szöveg ablak toohello jobbra. Válassza ki **Stream Analytics-feladat** hello eredménylistában.
   
    ![Új Stream Analytics-feladat létrehozása](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)
2. Adja meg a feladat egyedi nevét, és ellenőrizze a hello előfizetés van hello a feladathoz tartozó helyes névre. Ezután hozzon létre egy új erőforráscsoportot, vagy válasszon ki egy meglévőt az előfizetésén.
3. Ezután válassza ki a feladat helyét. Feldolgozási sebesség és az adatátvitel hello kiválasztásával költség csökkentése hello erőforráscsoport és a kívánt tárfiókot és ugyanazon a helyen ajánlott.
   
    ![Új Stream Analytics-feladat létrehozásának részletei](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.png)
   
   > [!NOTE]
   > Régiónként csak egyszer hozza létre ezt a tárfiókot. Ez a tároló az adott régióban létrehozott összes Stream Analytics-feladat között meg lesz osztva.
   > 
   > 
4. Ellenőrizze a hello mezőben tooplace a feladatot az irányítópulton, és kattintson a **létrehozása**.
   
    ![feladat létrehozása folyamatban](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03a.png)
5. Egy "telepítése megkezdődött..." kell megjelennie a böngészőablak jobb hello felső szerepelnek. Amint azt változik befejeződött tooa ablak alább látható módon.
   
    ![feladat létrehozása folyamatban](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03b.png)

### <a name="create-an-azure-stream-analytics-query"></a>Azure Stream Analytics-lekérdezés létrehozása
A feladat után létrehozott idő tooopen azt és -buildek lekérdezést. A feladat könnyen hozzá hello csempére kattintva érheti el.

![Feladat csempe](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

A hello **feladat topológia** ablaktáblán kattintson a hello **lekérdezés** toogo toohello Lekérdezésszerkesztő mezőben. Hello **lekérdezés** szerkesztő segítségével tooenter egy T-SQL lekérdezések hello átalakítása hello beérkező eseményadatok keresztül.

![Lekérdezésmező](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

### <a name="query-archive-your-raw-data"></a>Lekérdezés: Nyers adatok archiválása
hello lekérdezések legegyszerűbb formája az, amely az összes bemeneti adatok tooits kimeneti kijelölt archiválja továbbított lekérdezést. Töltse le a hello mintaadatfájlokat a [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) tooa helyre a számítógépen. 

1. Illessze be a hello lekérdezés hello PassThrough.txt fájlból. 
   
    ![Teszt bemeneti stream](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)
2. Tooyour bemeneti következő hello három pont gombra, és válassza ki **tölthet fel fájlból adatot** mezőbe.
   
    ![Teszt bemeneti stream](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06a.png)
3. Ablaktábla megnyitja az hello jobb emiatt, azt válassza hello HelloWorldASA-InputStream.json fájlt a letöltött helyről, és kattintson a **OK** hello ablaktábla hello alján.
   
    ![Teszt bemeneti stream](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06b.png)
4. Kattintson a hello **tesztelése** a terület hello ablak bal hello felső áttételi és feldolgozni a lekérdezés tesztelése hello minta-adatkészleteken ellen. Eredmények megnyílik egy alatt a lekérdezést, hello feldolgozása befejeződött.
   
    ![Teszteredmények](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

### <a name="query-filter-hello-data-based-on-a-condition"></a>Lekérdezés: Hello adatok szűrése feltétel alapján
Próbáljuk meg toofilter hello eredményeket egy feltétel alapján. Csak a "sensorA." érkező események tooshow eredmények tapasztalatairól hello lekérdezés hello Filtering.txt fájlban van.

![Adatstream szűrése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Vegye figyelembe, hogy a kis-és nagybetűket lekérdezés hello összehasonlítja a karakterlánc-érték. Kattintson a hello **teszt** áttételi újra tooexecute hello lekérdezés. hello lekérdezés 1860 eseményből 389 sort kell visszaadnia.

![Második kimeneti eredmény a lekérdezéstesztelésből](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

### <a name="query-alert-tootrigger-a-business-workflow"></a>Lekérdezés: Riasztási tootrigger üzleti munkafolyamat
Tegyük részletesebbé a lekérdezést. Érzékelő bármilyen típusú azt szeretné, 30 másodperces ablakban toomonitor átlaghőmérséklet és megjelenítse az eredményeket, csak ha hello átlaghőmérséklet meghaladja a 100 fokot. Azt írni fogja a hello következő lekérdezni, és kattintson a **teszt** toosee hello eredmények. hello lekérdezés hello ThresholdAlerting.txt fájlban van.

![30 másodperces szűrőlekérdezés](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Meg kell jelennie csak 245 sorok és érzékelők nevét tartalmazó eredmények hello átlaghőmérséklet nagyobb, mint 100 esetén. Ez a lekérdezés hello az események streamjét szerint csoportosítja **dspl**, hello érzékelő neve, vagyis több mint egy **Átfedésmentes ablak** 30 másodperc. A historikus lekérdezések tartalmaznia kell hogyan szeretnénk idő tooprogress. Hello segítségével **TIMESTAMP BY** záradék, hello meg **OUTPUTTIME** minden historikus számítás oszlop tooassociate idejében. Részletes információkért olvassa el hello MSDN cikkek [Időkezelést](https://msdn.microsoft.com/library/azure/mt582045.aspx) és [Ablakozó függvény](https://msdn.microsoft.com/library/azure/dn835019.aspx).

### <a name="query-detect-absence-of-events"></a>Lekérdezés: Események hiányának észlelése
Hogyan azt írhat egy lekérdezést toofind bemeneti események hiánya? Keressük hello utolsó ideje, hogy érzékelő adatokat küldött, és majd nem küldött tovább perc hello eseményeket. hello lekérdezés hello AbsenseOfEvent.txt fájlban van.

![Események hiányának észlelése](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

Jelen példában használjuk a **LEFT OUTER** toohello csatlakozás azonos adatfolyam (önillesztés). **BELSŐ** illesztés esetén csak akkor kapunk eredményt, ha van egyezés.  Az egy **LEFT OUTER** illesztési, ha hello balra hello illesztés oldalán lévő eseményhez nincs egyezés, facettype típusa NULL összes hello oszlopait hello jobb oldali sor ad vissza. Ez a módszer nagyon hasznos toofind események hiányát. Az [ILLESZTÉS](https://msdn.microsoft.com/library/azure/dn835026.aspx) részleteit az MSDN-dokumentációnkban találja.

## <a name="conclusion"></a>Összegzés
hello Ez az oktatóanyag célja toodemonstrate hogyan toowrite különböző Stream Analytics lekérdezési nyelv lekérdezéseket és ellenőrizze, hogy annak az eredménye hello böngészőben. Ezek azonban csak az első lépések. A Stream Analytics rengeteg lehetőséget rejt még magában. A Stream Analytics számos bemenetekhez és kimenetekhez, és képes még akkor is, funkcióinak használata az Azure Machine Learning toomake azt egy robusztus eszköz adatfolyamokat. További információk a Stream Analytics tooexplore elindíthatja használatával a [tanulási térkép](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). További információ toowrite lekérdezések, olvassa el a cikk hello [gyakori lekérdezési minták](stream-analytics-stream-analytics-query-patterns.md).


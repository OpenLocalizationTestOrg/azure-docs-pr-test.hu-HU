---
title: "aaaStream elemzés valós idejű feldolgozással az Azure Functions |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egy Azure-függvény csatlakozott a Service Bus-üzenetsorba, toopopulate egy egy Stream Analytics-feladat eredményének hello Azure Redis Cache."
keywords: "adatfolyam esetében a redis cache service bus-üzenetsorba"
services: stream-analytics
author: ryancrawcour
manager: jhubbard
documentationcenter: 
ms.assetid: d428bb33-4244-4001-b93d-c77bed816527
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/28/2017
ms.author: ryancraw
ms.openlocfilehash: 5ef4fe76c2cadf896a80eeaf421f010c315918af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toostore-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Hogyan Azure Stream Analytics egy Azure Redis Cache használata az Azure Functions a toostore adatait
Az Azure Stream Analytics lehetővé teszi gyors fejlesztésére és központi telepítése az alacsony költségű megoldások toogain valós idejű elemzése eszközök, érzékelőket, infrastruktúra, és alkalmazások vagy bármilyen streamet. Ez lehetővé teszi a különböző használati esetek például valós idejű felügyeleti és figyelési parancs és vezérlő, csalások felderítéséhez, csatlakoztatott autók vagy sok más. Ilyen helyzetekben érdemes lehet egy Azure Redis Cache-gyorsítótár például egy elosztott adattárba Azure Stream Analytics outputted toostore adatok.

Tegyük fel, hogy egy távközlési vállalati részét képezik. Ahol több érkező hívást hello ugyanazzal az identitással toodetect SIM csalás próbál, a hello azonos időben, de különböző földrajzi helyeken. Az összes hello lehetséges csalárd telefonhívásokat tárolása az Azure Redis cache-rendszer biztosítja. Az ebben a blogban nyújtunk segítséget a hogyan könnyen befejezheti a feladatot. 

## <a name="prerequisites"></a>Előfeltételek
Teljes hello [valós idejű csalások felderítéséhez] [ fraud-detection] segédlet az ASA

## <a name="architecture-overview"></a>Architektúra áttekintése
![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/architecture-overview.png)

Ahogy az ábra megelőző hello, Stream Analytics lehetővé teszi a bemeneti adatok toobe streaming lekérdezett és tooan kimeneti küldött. Hello kimeneti alapján, az Azure Functions majd elindítható valamilyen esemény. 

A blogban található Microsoft hello Azure Functions részét, ez az adatcsatorna összpontosítson, vagy pontosabban hello a csalárd adatokat tároló hello gyorsítótárba esemény váltanak.
Hello befejezése után [valós idejű csalások felderítéséhez] [ fraud-detection] oktatóanyagban rendelkezik (az eseményközpontok) bemeneti, a lekérdezés és kimenetnek (blob-tároló) már konfigurálva és fusson. Az ebben a blogban módosítjuk hello kimeneti toouse a Service Bus-üzenetsorba helyette. Ezt követően csatlakoztassa azt egy Azure-függvény toothis várólista. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Hozzon létre, és csatlakozzon a Service Bus-üzenetsorba kimenete
toocreate a Service Bus-üzenetsorba, hajtsa végre az 1. és 2 hello .NET szakasz [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted].
Most tegyük csatlakozás hello várólista toohello Stream Analytics-feladat a hello létrehozott korábbi csalások észlelése segédlet.

1. A hello Azure-portálon, válassza a toohello **kimenetek** panel a feladatot, és válassza ki a **hozzáadása** hello oldal hello tetején.
   
    ![Kimenet hozzáadása](./media/stream-analytics-functions-redis/adding-outputs.png)
2. Válasszon **Service Bus-üzenetsorba** , hello **gyűjtése** és az üdvözlő képernyőt hello útmutatás. A Service Bus-üzenetsorba hello meg arról, hogy toochoose hello névterekkel létrehozott [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted]. Ha elkészült, kattintson a hello "jobb oldali" gombra.
3. Adja meg a következő értékek hello:
   
   * **Esemény szerializáló formátum**: JSON-ban
   * **Kódolás**: UTF8
   * **FORMÁTUM**: sorral elválasztott
4. Kattintson a hello **létrehozása** tooadd gombra kattint, a forrás- és, hogy a Stream Analytics képes csatlakozni toohello tárfiók tooverify.
5. A hello **lekérdezés** lapra, cserélje ki hello aktuális lekérdezés hello következőre. Cserélje le * [YOUR SERVICE BUS NAME] * a 3. lépésben létrehozott hello kimeneti nevű. 
   
    ```    
   
        SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
   
        INTO [YOUR SERVICE BUS NAME]
   
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   
        WHERE CS1.SwitchNum != CS2.SwitchNum
   
    ```

## <a name="create-an-azure-redis-cache"></a>Azure Redis Cache létrehozása
Hozzon létre egy Azure Redis cache-hello .NET szakasz [hogyan tooUse Azure Redis Cache] [ use-rediscache] amíg hello szakasz nevű ***hello gyorsítótár-ügyfelek konfigurálása***.
Művelet befejeződése után egy új Redis Cache rendelkezik. A **összes beállítás**, jelölje be **hívóbetűk** és jegyezze fel a hello ***elsődleges kapcsolódási karakterlánc***.

![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Egy Azure-függvény létrehozása
Hajtsa végre a [az első Azure-függvény létrehozása] [ functions-getstarted] az Azure Functions használatába oktatóanyag tooget. Ha már rendelkezik egy Azure függvény volna, például toouse, akkor hagyja ki azokat, amelyek túl[tooRedis gyorsítótár írása](#Writing-to-Redis-Cache)

1. Hello portálon alkalmazásszolgáltatások hello bal oldali navigációs sávon válassza az Azure-függvény app name tooget toohello függvény app webhelyet.
    ![Képernyőkép a szolgáltatások függvény alkalmazáslistájában](./media/stream-analytics-functions-redis/app-services-function-list.png)
2. Kattintson a **új függvény > ServiceBusQueueTrigger – C#**. Hello a következő mezőket, kövesse az alábbi utasításokat:
   
   * **Várólista neve**: hello azonos nevet a hello sor létrehozásakor megadott hello névként [Ismerkedés a Service Bus-üzenetsorok] [ servicebus-getstarted] (nem hello neve hello a service bus). Győződjön meg arról, amely a Stream Analytics kimeneti csatlakoztatott toohello hello várólista használja.
   * **A Service Bus kapcsolati**: válasszon **a kapcsolati karakterlánc hozzáadásának**. toofind hello kapcsolati karakterláncot, nyissa meg toohello klasszikus portálra, válassza ki **Service Bus**, hozott létre, a service bus hello és **KAPCSOLATADATOK** üdvözlő képernyőt hello alján. Győződjön meg arról, hogy a fő képernyőn hello ezen a lapon. Másolja és illessze be hello kapcsolati karakterláncot. Érzi, hogy szabad tooenter bármely kapcsolat neve.
     
       ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * **AccessRights**: válasszon **kezelése**
3. Kattintson a **Create** (Létrehozás) gombra

## <a name="writing-tooredis-cache"></a>Írás tooRedis gyorsítótár
Létrehoztunk egy Azure-függvény, amely egy Service Bus-üzenetsorba olvassa be most. Összes toodo marad, a függvény toowrite ezen adatok toohello Redis Cache használja. 

1. Válassza ki az újonnan létrehozott **ServiceBusQueueTrigger**, és kattintson a **Alkalmazásbeállítások működik** a hello jobb felső sarokban. Válassza ki **Ugrás tooApp szolgáltatás beállításaira > Beállítások > Alkalmazásbeállítások**
2. Kapcsolati karakterláncok szakasz hello, hozzon létre egy nevet a hello **neve** szakasz. Illessze be a hello talált hello elsődleges kapcsolati karakterláncot **Redis gyorsítótárat létrehozni** történő hello lépés **érték** szakasz. Válassza ki **egyéni** ahol felirat látható **SQL-adatbázis**.
3. Kattintson a **mentése** hello tetején.
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/function-connection-string.png)
4. Most lépjen vissza toohello App Service-beállításokat, és válassza ki **eszközök > App Service-szerkesztő (előzetes verzió) > a > Lépjen**.
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/app-service-editor.png)
5. Egy tetszőleges szerkesztővel, hozzon létre egy JSON fájlt **project.json** a következő hello és tooyour helyi lemezre menti.
   
        {
            "frameworks": {
                "net46": {
                    "dependencies": {
                        "StackExchange.Redis":"1.1.603",
                        "Newtonsoft.Json": "9.0.1"
                    }
                }
            }
        }
6. A feltöltés a függvény (nem WWWROOT) hello legfelső szintű könyvtárba. Nevű fájlba kell megjelennie **project.lock.json** jelennek meg automatikusan, erősítse meg, hogy Nuget hello csomagok "StackExchange.Redis" és "Newtonsoft.Json" importálva.
7. A hello **run.csx** fájlt, cserélje le a következő kód hello hello előre generált kódot. Hello lazyConnection függvényben, cserélje le az "Kapcs NAME" 2. lépésében létrehozott hello nevű **adattárolásra való hello Redis gyorsítótár**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers tooa property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract hello time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using hello cache object...
        // Simple put of integral data types into hello cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from hello cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect toohello Service Bus
    private static Lazy<ConnectionMultiplexer> lazyConnection = 
        new Lazy<ConnectionMultiplexer>(() =>
            {
                var cnn = ConfigurationManager.ConnectionStrings["CONN NAME"].ConnectionString
                return ConnectionMultiplexer.Connect();
            });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

````

## <a name="start-hello-stream-analytics-job"></a>Hello Stream Analytics-feladat indítása
1. Hello telcodatagen.exe alkalmazás indításához. hello használata a következőképpen történik:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````
2. A Stream Analytics-feladat panelen hello hello portálon kattintson az **Start** hello oldal hello tetején.
   
    ![Képernyőkép a indítási feladat](./media/stream-analytics-functions-redis/starting-job.png)
3. A hello **indítási feladat** panelen megjelenő, válassza ki **most** majd hello **Start** üdvözlő képernyőt hello alján gombra. hello feladat állapota tooStarting és egyes módosítások tooRunning időpont után.
   
    ![Képernyőkép a start feladat idő kiválasztása](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Futtassa a megoldás és eredmények ellenőrzése
Ha visszalép tooyour **ServiceBusQueueTrigger** lapon meg kell jelennie jelentkezzen utasításokat. Ezek a naplók megjelenítése hello Service Bus-üzenetsorba, tegye közzé hello adatbázis kapott azonosítóértékeket valami, és be szeretné olvasni hello időparaméterrel hello kulcsként ki!

amely az adatok a Redis gyorsítótár tooverify tooyour Redis gyorsítótár oldal hello új portálon lépjen (ahogy az előző hello [hozzon létre egy Azure Redis Cache](#Create-an-Azure-Redis-Cache) lépés), és válassza a konzolt.

Ezután írhat Redis parancsok tooconfirm, hogy adatokat valójában hello gyorsítótárában.

![Képernyőkép a Redis-konzol](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Következő lépések
Hello új szempontot Azure Functions Izgatottan várja el a Stream analytics együtt teheti meg, és Reméljük, ez új lehetőségek oldja meg. Ha olyan visszajelzést, amit meg akar mellett a, érzi, hogy szabad toouse hello [Azure UserVoice webhelyén](https://feedback.azure.com/forums/270577-stream-analytics).

Ha új Microsoft Azure, a Microsoft meghívhatjuk tootry által végzett regisztrációt azt egy [ingyenes Azure próba-fiókot](https://azure.microsoft.com/pricing/free-trial/). Ha új tooStream Analytics áll, akkor azt meghívhatjuk túl[az első Stream Analytics-feladat létrehozása](stream-analytics-create-a-job.md).

Ha bármelyik segítséget vagy kérdése van, írjon [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) vagy [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fórumok. 

Azt is láthatja, hogy a következő erőforrások hello:

* [Az Azure Functions fejlesztői segédanyagai](../azure-functions/functions-reference.md)
* [Az Azure Functions C# fejlesztői leírás](../azure-functions/functions-reference-csharp.md)
* [Az Azure Functions F # fejlesztői leírás](../azure-functions/functions-reference-fsharp.md)
* [Az Azure Functions NodeJS fejlesztői leírás](../azure-functions/functions-reference.md)
* [Az Azure Functions eseményindítók és kötések](../azure-functions/functions-triggers-bindings.md)
* [Hogyan toomonitor Azure Redis Cache-gyorsítótár](../redis-cache/cache-how-to-monitor.md)

minden hello legújabb híreket és szolgáltatásokat, naprakész toostay kövesse [ @AzureStreaming ](https://twitter.com/AzureStreaming) a Twitteren.

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md

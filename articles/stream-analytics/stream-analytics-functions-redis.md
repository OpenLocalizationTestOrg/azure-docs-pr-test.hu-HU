---
title: "Adatfolyam-elemzés, valós idejű feldolgozása az Azure Functions |} Microsoft Docs"
description: "Megtudhatja, hogyan használható az Azure-függvény csatlakozott a Service Bus-üzenetsorba, egy Stream Analytics-feladat eredményének Azure Redis gyorsítótár adatokkal való feltöltése."
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
ms.openlocfilehash: ad14cc858ff513573e2718a26a9ab5c524e1adc6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-store-data-from-azure-stream-analytics-in-an-azure-redis-cache-using-azure-functions"></a>Azure Stream Analytics adatok tárolása az az Azure Redis Cache Azure Functions használatával
Az Azure Stream Analytics lehetővé teszi gyors fejlesztésére, és alacsony költségű megoldások ahhoz, hogy az eszközök, érzékelőket, infrastruktúra, és alkalmazások vagy bármilyen streamet valós idejű elemzése telepítését. Ez lehetővé teszi a különböző használati esetek például valós idejű felügyeleti és figyelési parancs és vezérlő, csalások felderítéséhez, csatlakoztatott autók vagy sok más. Ilyen helyzetekben érdemes lehet például egy Azure Redis cache-egy elosztott adattárba Azure Stream Analytics outputted adatok tárolására.

Tegyük fel, hogy egy távközlési vállalati részét képezik. Ha több, ugyanazzal az identitással, ugyanazon érkező hívások idő, de különböző földrajzilag SIM csalások felderítésére kívánt helyét. Az összes a potenciális csalárd telefonhívásokat tárolása az Azure Redis cache-rendszer biztosítja. Az ebben a blogban nyújtunk segítséget a hogyan könnyen befejezheti a feladatot. 

## <a name="prerequisites"></a>Előfeltételek
Fejezze be a [valós idejű csalások felderítéséhez] [ fraud-detection] segédlet az ASA

## <a name="architecture-overview"></a>Architektúra áttekintése
![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/architecture-overview.png)

A fenti ábrán látható, a Stream Analytics lehetővé teszi a bemeneti adatok lekérdezése és küldött egy kimeneti adatfolyam. A kimeneti alapján, az Azure Functions majd elindítható valamilyen esemény. 

Az ebben a blogban azt összpontosítanak az adatcsatornát az Azure Functions része, vagy pontosabban az időt. az esemény félrevezető adatokat tároló a gyorsítótárba.
Miután befejezte a [valós idejű csalások felderítéséhez] [ fraud-detection] oktatóanyagban rendelkezik (az eseményközpontok) bemeneti, a lekérdezés és kimenetnek (blob-tároló) már konfigurálva és fusson. Az ebben a blogban módosítjuk használja helyette a Service Bus-üzenetsorba kimenete. Ezt követően nem csatlakozni az Azure-függvény ebből a várólistából. 

## <a name="create-and-connect-a-service-bus-queue-output"></a>Hozzon létre, és csatlakozzon a Service Bus-üzenetsorba kimenete
Hozzon létre egy Service Bus-üzenetsorba, kövesse a lépéseket, 1. és 2. a .NET részt [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted].
Most tegyük a várólista csatlakozni a Stream Analytics-feladat, amely a korábbi csalások észlelése segédlet jött létre.

1. Az Azure-portálon lépjen a **kimenetek** a feladat, és válassza ki a panel **Hozzáadás** az oldal tetején.
   
    ![Kimenet hozzáadása](./media/stream-analytics-functions-redis/adding-outputs.png)
2. Válasszon **Service Bus-üzenetsorba** , a **gyűjtése** és kövesse a képernyőn megjelenő utasításokat. Ügyeljen arra, hogy válassza ki a Service Bus-üzenetsorba, a létrehozott névtér [Ismerkedés a Service Bus-üzenetsorok][servicebus-getstarted]. Ha elkészült, kattintson a "jobb oldali" gombra.
3. Adja meg a következő értékeket:
   
   * **Esemény szerializáló formátum**: JSON-ban
   * **Kódolás**: UTF8
   * **FORMÁTUM**: sorral elválasztott
4. Kattintson a **létrehozása** gombra, adja hozzá a forrás, és győződjön meg arról, hogy a Stream Analytics sikeresen csatlakozott-e a tárfiók.
5. Az a **lekérdezés** lapon, az aktuális lekérdezés cserélje le a következő. Cserélje le * [YOUR SERVICE BUS NAME] * a 3. lépésben létrehozott kimeneti névvel. 
   
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
Hozzon létre egy Azure Redis Cache-gyorsítótár a .NET részt [használata Azure Redis Cache hogyan] [ use-rediscache] mindaddig, amíg a szakasz nevű ***a gyorsítótár-ügyfelek konfigurálása***.
Művelet befejeződése után egy új Redis Cache rendelkezik. A **összes beállítás**, jelölje be **hívóbetűk** és jegyezze fel a ***elsődleges kapcsolódási karakterlánc***.

![Képernyőkép-architektúra](./media/stream-analytics-functions-redis/redis-cache-keys.png)

## <a name="create-an-azure-function"></a>Egy Azure-függvény létrehozása
Hajtsa végre a [az első Azure-függvény létrehozása] [ functions-getstarted] oktatóanyag az Azure Functions első lépéseiben. Ha már rendelkezik egy Azure függvény használni szeretné, majd ugorjon előre [Redis Cache írása](#Writing-to-Redis-Cache)

1. A portál a alkalmazásszolgáltatások válassza a bal oldali navigációs sávon, majd az Azure-függvény alkalmazásnév, a függvény app webhely eléréséhez.
    ![Képernyőkép a szolgáltatások függvény alkalmazáslistájában](./media/stream-analytics-functions-redis/app-services-function-list.png)
2. Kattintson a **új függvény > ServiceBusQueueTrigger – C#**. Az alábbi mezők kövesse az alábbi utasításokat:
   
   * **Várólista neve**: neve megegyezik a sor létrehozásakor megadott név [Ismerkedés a Service Bus-üzenetsorok] [ servicebus-getstarted] (nem a service bus neve). Ellenőrizze, hogy csatlakozik-e a Stream Analytics kimeneti várólista használja.
   * **A Service Bus kapcsolati**: válasszon **a kapcsolati karakterlánc hozzáadásának**. Keresse meg a kapcsolati karakterláncot, lépjen a klasszikus portálra, válassza ki **Service Bus**, a service bus hozott létre, és **KAPCSOLATADATOK** a képernyő alján. Győződjön meg arról, hogy a fő képernyőn ezen a lapon. Másolja és illessze be a kapcsolati karakterláncot. Nyugodtan adjon meg a kapcsolat neve.
     
       ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/servicebus-connection.png)
   * **AccessRights**: válasszon **kezelése**
3. Kattintson a **Create** (Létrehozás) gombra

## <a name="writing-to-redis-cache"></a>Redis gyorsítótár írása
Létrehoztunk egy Azure-függvény, amely egy Service Bus-üzenetsorba olvassa be most. Ehhez marad csak a funkcióval a Redis Cache ezeket az adatokat írni. 

1. Válassza ki az újonnan létrehozott **ServiceBusQueueTrigger**, és kattintson a **Alkalmazásbeállítások működéséhez** a jobb felső sarokban. Válassza ki **az App Service-beállítások > Beállítások > Alkalmazásbeállítások**
2. A kapcsolati karakterláncok szakaszban, hozzon létre egy nevet a a **neve** szakasz. Illessze be a található a elsődleges kapcsolati karakterláncot a **Redis gyorsítótárat létrehozni** lépjen be a **érték** szakasz. Válassza ki **egyéni** ahol felirat látható **SQL-adatbázis**.
3. Kattintson a **mentése** tetején.
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/function-connection-string.png)
4. Most lépjen vissza a alkalmazás Service beállításai, és válassza ki **eszközök > App Service-szerkesztő (előzetes verzió) > a > Lépjen**.
   
    ![Képernyőkép a service bus-kapcsolat](./media/stream-analytics-functions-redis/app-service-editor.png)
5. Egy tetszőleges szerkesztővel, hozzon létre egy JSON fájlt **project.json** a következőre, és a helyi lemezre menteni.
   
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
6. Ez a fájl feltöltése a legfelső szintű könyvtárba, a függvény (nem WWWROOT). Nevű fájlba kell megjelennie **project.lock.json** jelennek meg automatikusan, erősítse meg, hogy a Nuget-csomagok "StackExchange.Redis" és "Newtonsoft.Json" importálta-e.
7. Az a **run.csx** fájlt, cserélje le az előre generált kódot az alábbira. A lazyConnection függvényben cserélje le az "Kapcs NAME" 2. lépésében létrehozott nevű **adatok tárolásához a Redis gyorsítótárba**.

````

    using System;
    using System.Threading.Tasks;
    using StackExchange.Redis;
    using Newtonsoft.Json;
    using System.Configuration;

    public static void Run(string myQueueItem, TraceWriter log)
    {
        log.Info($"Function processed message: {myQueueItem}");

        // Connection refers to a property that returns a ConnectionMultiplexer
        IDatabase db = Connection.GetDatabase();
        log.Info($"Created database {db}");

        // Parse JSON and extract the time
        var message = JsonConvert.DeserializeObject<dynamic>(myQueueItem);
        string time = message.time;
        string callingnum1 = message.callingnum1;

        // Perform cache operations using the cache object...
        // Simple put of integral data types into the cache
        string key = time + " - " + callingnum1;
        db.StringSet(key, myQueueItem);
        log.Info($"Object put in database. Key is {key} and value is {myQueueItem}");

        // Simple get of data types from the cache
        string value = db.StringGet(key);
        log.Info($"Database got: {value}"); 
    }

    // Connect to the Service Bus
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

## <a name="start-the-stream-analytics-job"></a>A Stream Analytics-feladat indítása
1. A telcodatagen.exe alkalmazás indításához. A használati a következőképpen történik:````telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]````
2. A Stream Analytics-feladat paneljéről a portálon, kattintson a **Start** az oldal tetején.
   
    ![Képernyőkép a indítási feladat](./media/stream-analytics-functions-redis/starting-job.png)
3. Az a **indítási feladat** panelen megjelenő, válassza ki **most** , majd a **Start** gomb a képernyő alján. A feladat állapotmódosítások indítása és futtatásának ideje módosításai után.
   
    ![Képernyőkép a start feladat idő kiválasztása](./media/stream-analytics-functions-redis/start-job-time.png)

## <a name="run-solution-and-check-results"></a>Futtassa a megoldás és eredmények ellenőrzése
Visszatér a **ServiceBusQueueTrigger** lapon meg kell jelennie jelentkezzen utasításokat. Ezek a naplók megjelenítése a Service Bus-üzenetsorba, tegye közzé az adatbázis kapott azonosítóértékeket valami, és be szeretné olvasni, az idő használata a kulcs!

Győződjön meg arról, hogy az adatok a Redis gyorsítótárt, lépjen a Redis gyorsítótár lap az új portálon (ahogy az előző [hozzon létre egy Azure Redis Cache](#Create-an-Azure-Redis-Cache) lépés), és válassza a konzolt.

Ezután írhat Redis-parancsok futtatásával győződjön meg arról, hogy adatokat valójában a gyorsítótárban.

![Képernyőkép a Redis-konzol](./media/stream-analytics-functions-redis/redis-console.png)

## <a name="next-steps"></a>Következő lépések
A rendszer az Azure Functions új szempontot várakozással, és együtt teheti meg a Stream analytics Reméljük, ez új lehetőségek oldja meg. Ha olyan visszajelzést, amit meg akar mellett a, szabadon használhatja a [Azure UserVoice webhelyén](https://feedback.azure.com/forums/270577-stream-analytics).

Ha új Microsoft Azure, azt hívhat meg, hogy próbálja ki által regisztrációt egy [ingyenes Azure próba-fiókot](https://azure.microsoft.com/pricing/free-trial/). Ha még nem ismeri a Stream Analytics, akkor kérjük [az első Stream Analytics-feladat létrehozása](stream-analytics-create-a-job.md).

Ha bármelyik segítséget vagy kérdése van, írjon [MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) vagy [Stackoverflow](http://stackoverflow.com/questions/tagged/azure-stream-analytics) fórumok. 

Megtekintheti a következőket is:

* [Az Azure Functions fejlesztői segédanyagai](../azure-functions/functions-reference.md)
* [Az Azure Functions C# fejlesztői leírás](../azure-functions/functions-reference-csharp.md)
* [Az Azure Functions F # fejlesztői leírás](../azure-functions/functions-reference-fsharp.md)
* [Az Azure Functions NodeJS fejlesztői leírás](../azure-functions/functions-reference.md)
* [Az Azure Functions eseményindítók és kötések](../azure-functions/functions-triggers-bindings.md)
* [Azure Redis Cache figyelése](../redis-cache/cache-how-to-monitor.md)

Ezek naprakész állapotát az összes a legújabb híreket és szolgáltatásokat, hajtsa végre a [ @AzureStreaming ](https://twitter.com/AzureStreaming) a Twitteren.

[fraud-detection]: stream-analytics-real-time-fraud-detection.md
[servicebus-getstarted]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md
[use-rediscache]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md
[functions-getstarted]: ../azure-functions/functions-create-first-azure-function.md

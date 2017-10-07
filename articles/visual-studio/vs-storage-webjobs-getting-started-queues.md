---
title: "a queue storage és a Visual Studio (webjobs-feladat projektek) kapcsolódó szolgáltatások használatába aaaGetting |} Microsoft Docs"
description: "Hogyan tooget el az Azure Queue storage segítségével a webjobs-feladat projektben szolgáltatások csatlakozás a Visual Studio használatával tooa storage-fiók összekötése után."
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5c3ef267-2a67-44e9-ab4a-1edd7015034f
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 47a446aa5c6bbf25526339823db4952ac1a8802f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-queue-storage-and-visual-studio-connected-services-webjob-projects"></a>Ismerkedés az Azure Queue storage és a Visual Studio csatlakoztatva (webjobs-feladat projektek) szolgáltatások
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan használatának az Azure Queue storage létrehozott vagy egy Azure storage-fiók használatával hivatkozott után a Visual Studio Azure webjobs-feladat projektben hello Visual Studio **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel megnyitásához. Ha a tárolási fiók tooa webjobs-feladat projekt hozzáadása hello Visual Studio használatával **kapcsolódó szolgáltatások hozzáadása** párbeszédpanelen hello megfelelő Azure Storage NuGet-csomagok vannak telepítve, és hello megfelelő .NET hivatkozások hozzáadott toohello projekt és hello tárfiók kapcsolati karakterláncok frissítése hello App.config fájlban.  

Ez a cikk ismerteti a C# mintakódok hogyan toouse hello Azure WebJobs SDK-verzió megjelenítése 1.x az Azure Queue storage szolgáltatás hello.

Az Azure Queue storage egy olyan szolgáltatás, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLT használ, hitelesített hívásokon keresztül hello world üzenetek nagy számban tárolásához. Egyetlen üzenetsor mentése too64 KB méretű is lehet, és tartalmazhat több millió üzenetet toohello maximális kapacitásán belül a storage-fiók mentése. Lásd: [Ismerkedés az Azure Queue Storage használatának .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) további információt. ASP.NET kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).

## <a name="how-tootrigger-a-function-when-a-queue-message-is-received"></a>Hogyan tootrigger a függvény egy várólista-üzenet fogadásakor.
toowrite egy függvényt, amely a WebJobs SDK hello meghívja a várólista-üzenet fogadásakor, használja a hello **QueueTrigger** attribútum. hello attribútum konstruktora a hello várólista toopoll hello nevét megadó karakterlánc-paramétert fogad. toosee hogyan tooset hello várólistacímke dinamikusan, tekintse meg [hogyan tooset konfigurációs beállítások](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Karakterlánc-üzenetek
A következő példa hello, hello várólista tartalmaz egy karakterlánc-üzenet, így **QueueTrigger** alkalmazott tooa karakterlánc paraméter nevű **logMessage** tartalmazó hello hello várólista üzenet tartalma. hello függvény [írja a naplót üzenet toohello irányítópult](#how-to-write-logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Emellett **karakterlánc**, hello lehet, egy bájttömböt egy **CloudQueueMessage** objektumot, vagy egy Ön által meghatározott POCO.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
A következő példa hello, várólista üdvözlőüzenetére tartalmazza a JSON a **BlobInformation** objektum, amely tartalmazza a **Blobnév** tulajdonság. hello SDK automatikusan deserializes hello objektum.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni. Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Aszinkron funkciók
a következő aszinkron függvény hello [írja a naplót toohello irányítópult](#how-to-write-logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Aszinkron funkciók is igénybe vehet egy [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), ahogy az alábbi példa, amely egy blob másolja hello. (Hello előzetesben **queueTrigger** helyőrző, lásd: hello [Blobok](#how-to-read-and-write-blobs-and-tables-while-processing-a-queue-message) szakaszában.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

## <a name="types-hello-queuetrigger-attribute-works-with"></a>Típusok hello QueueTrigger attribútum együttműködik
Használhat **QueueTrigger** a következő típusok hello:

* **karakterlánc**
* A JSON-ként szerializált POCO típus
* **byte]**
* **CloudQueueMessage**

## <a name="polling-algorithm"></a>Lekérdezési algoritmus
hello SDK valósítja meg a tétlen-várólista tárolási tranzakciós költségeket a lekérdezési véletlenszerű exponenciális vissza az indító algoritmus tooreduce hello hatását.  Ha üzenet található, hello SDK két másodpercet vár, és ellenőrzi, egy másik üzenet; Ha üzenet található azt, mielőtt újra próbálkozna körülbelül négy másodpercet vár. Későbbi sikertelen bejelentkezési kísérletek tooget egy üzenetsor-üzenetet, miután a hello várakozási idő tooincrease folytatódik, amíg eléri a maximális várakozási idő hello, mely alapértelmezett tooone perc. [hello maximális várakozási idő az konfigurálható](#how-to-set-configuration-options).

## <a name="multiple-instances"></a>Több példánya
Ha több példányt a webalkalmazás fut, a egy folyamatos webjobs-feladatok minden számítógépen fut, és minden gép fog Várjon, amíg az eseményindítók és toorun funkciók kísérlet. Ennek eredményeképpen előfordulhat, egyes esetekben feldolgozása toosome funkciók hello ugyanazokat az adatokat kétszer, ezért funkciók idempotent (úgy, hogy az ugyanazon bemeneti adatok nem eredményez hello ismételten hívása ismétlődő eredmények írása) kell lennie.  

## <a name="parallel-execution"></a>Párhuzamos végrehajtás
Ha több funkciók különböző várólisták figyel, hello SDK fogja hívni azokat párhuzamosan üzenetek fogadása egy időben.

hello azonos érvényét veszti, ha több üzenetek fogadása az egy adott sorba. Alapértelmezés szerint hello SDK egyszerre az 16 várólista üzenetkötegek lekérdezi és feldolgozza azokat párhuzamosan hello függvény végrehajtása. [hello köteg mérete konfigurálható](#how-to-set-configuration-options). Ha feldolgozott hello szám lefelé toohalf hello kötegelt méretű lekérdezi, hello SDK lekérdezi egy másik köteg, és megkezdi az üzenetek feldolgozását. Ezért hello maximális száma párhuzamos éppen feldolgozott üzeneteinek másodpercenkénti függvény, egy és egy félig alkalommal hello kötegmérete. Ez a korlátozás vonatkozik külön-külön tooeach függvény, amely rendelkezik egy **QueueTrigger** attribútum. Ha nem szeretné, egy olyan sort a fogadott üzenetet párhuzamos futtatáshoz, állítsa be a hello köteg mérete too1.

## <a name="get-queue-or-queue-message-metadata"></a>Várólista vagy várólista üzenet metaadatok beolvasása
Kaphat a következő üzenet tulajdonságai toohello metódus-aláírás paraméterek hozzáadásával hello:

* **DateTimeOffset** expirationTime
* **DateTimeOffset** insertionTime
* **DateTimeOffset** nextVisibleTime
* **karakterlánc** queueTrigger (tartalmazza a szöveges üzenet)
* **karakterlánc** azonosítója
* **karakterlánc** popReceipt
* **int** dequeueCount

Ha azt szeretné, közvetlenül az Azure storage API hello toowork, azt is megteheti egy **CloudStorageAccount** paraméter.

hello alábbi példa írja az összes a metaadatok tooan INFO alkalmazásnaplóban. Hello példában logMessage és a queueTrigger tartalma hello hello várólista üzenet.

        public static void WriteLog([QueueTrigger("logqueue")] string logMessage,
            DateTimeOffset expirationTime,
            DateTimeOffset insertionTime,
            DateTimeOffset nextVisibleTime,
            string id,
            string popReceipt,
            int dequeueCount,
            string queueTrigger,
            CloudStorageAccount cloudStorageAccount,
            TextWriter logger)
        {
            logger.WriteLine(
                "logMessage={0}\n" +
            "expirationTime={1}\ninsertionTime={2}\n" +
                "nextVisibleTime={3}\n" +
                "id={4}\npopReceipt={5}\ndequeueCount={6}\n" +
                "queue endpoint={7} queueTrigger={8}",
                logMessage, expirationTime,
                insertionTime,
                nextVisibleTime, id,
                popReceipt, dequeueCount,
                cloudStorageAccount.QueueEndpoint,
                queueTrigger);
        }

Íme egy mintanaplót hello mintakód szerint:

        logMessage=Hello world!
        expirationTime=10/14/2014 10:31:04 PM +00:00
        insertionTime=10/7/2014 10:31:04 PM +00:00
        nextVisibleTime=10/7/2014 10:41:23 PM +00:00
        id=262e49cd-26d3-4303-ae88-33baf8796d91
        popReceipt=AgAAAAMAAAAAAAAAfc9H0n/izwE=
        dequeueCount=1
        queue endpoint=https://contosoads.queue.core.windows.net/
        queueTrigger=Hello world!

## <a name="graceful-shutdown"></a>Biztonságos leállításának
Egy függvény a folyamatos webjobs-feladat fut fogad el egy **CancellationToken** paramétert, amely lehetővé teszi, hogy ha hello hello operációs rendszer toonotify hello függvény a webjobs-feladat megszakadt toobe tárgya. Az értesítési toomake meg arról, hogy hello függvény nem bontható váratlanul körbe, hogy adatok inkonzisztens állapotban is használhatja.

a következő példa azt mutatja meg hogyan hello toocheck függvények közelgő webjobs-feladat-záráshoz.

    public static void GracefulShutdownDemo(
                [QueueTrigger("inputqueue")] string inputText,
                TextWriter logger,
                CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(1000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
    }

**Megjegyzés:** hello irányítópult előfordulhat, hogy helyesen jelenjen meg hello állapota és kimenete, amely le lett állítva a funkciókat.

További információkért lásd: [webjobs-feladatok szabályos leállítást](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a name="how-toocreate-a-queue-message-while-processing-a-queue-message"></a>Hogyan toocreate várólista üzenet-várólista üzenet feldolgozása közben
egy függvényt, amely létrehoz egy új sor üzenetet, használjon hello toowrite **várólista** attribútum. Például **QueueTrigger**, adja meg a várólistacímke hello karakterláncból, vagy beállíthatja [hello várólista nevének beállítása dinamikusan](#how-to-set-configuration-options).

### <a name="string-queue-messages"></a>Karakterlánc-üzenetek
a következő példakód nem aszinkron hello hello várólista "outputqueue" nevű új várólista üzenet ugyanaz tartalom másként hello üzenetsorban található üzenetet kapott hello várólista "inputqueue" nevű hello hoz létre. (Aszinkron funkciók használata **IAsyncCollector<T>**  később az itt látható módon.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
toocreate egy kimeneti paraméter toohello írja egy üzenetsor-üzenetet, amely tartalmazza a karakterláncot, pass hello POCO helyett egy POCO **várólista** attribútumok konstruktorában.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

hello SDK automatikusan hello objektum tooJSON rendezi sorba. Egy üzenetsor mindig létrejön, még akkor is, ha hello objektum értéke null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Hozzon létre több üzenetet vagy aszinkron függvény
toocreate több üzenetet, hogy hello paramétertípus hello kimeneti várólista **ICollector<T>**  vagy **IAsyncCollector<T>**, ahogy az alábbi példa hello.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Minden várólista-üzenet létrehozásakor a rendszer azonnal hello **Hozzáadás** metódust.

### <a name="types-that-hello-queue-attribute-works-with"></a>Típusok hello várólista attribútum, amely együttműködik
Használhatja a hello **várólista** hello paramétereinek típusa a következő attribútumot:

* **kimenő karakterlánc** (hoz létre a üzenetsor-üzenetet, ha a paraméter értéke nem null értékű hello függvény végén)
* **kimenő byte []** (működik, mint az **karakterlánc**)
* **kimenő CloudQueueMessage** (működik, mint az **karakterlánc**)
* **kimenő POCO** (szerializálható típust, létrehoz egy üzenetet null objektummal rendelkező Ha hello paraméterének értéke null, ha hello függvény karakterlánccal végződik-e)
* **ICollector**
* **IAsyncCollector**
* **CloudQueue** (üzenetek manuális létrehozásához használatával hello Azure Storage API közvetlenül)

### <a name="use-webjobs-sdk-attributes-in-hello-body-of-a-function"></a>Egy függvény törzséhez hello a WebJobs SDK attribútumok használata
Ha toodo kell néhány működik a függvény a WebJobs SDK attribútum használatához **várólista**, **Blob**, vagy **tábla**, használhatja a hello **IBinder** felületet.

a következő példa hello időt vesz igénybe egy bemeneti várólista-üzenet, és hoz létre egy új üzenet hello azonos kimeneti várólistában lévő tartalom. hello kimeneti várólista neve hello függvénytörzs hello kóddal van beállítva.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

Hello **IBinder** felület is használható hello **tábla** és **Blob** attribútumok.

## <a name="how-tooread-and-write-blobs-and-tables-while-processing-a-queue-message"></a>Hogyan tooread és írási blobok és táblák egy várólista üzenet feldolgozása közben
Hello **Blob** és **tábla** attribútumok lehetővé teszik a tooread, és írja blobok és táblákat. Ebben a szakaszban hello minták tooblobs alkalmazni. Hogyan tootrigger dolgozza fel, ha a blobok létrehozott vagy frissített megjelenítése mintakódok, lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), és mintakódok olvasási és írási táblák, lásd: [hogyan toouse Azure-tábla tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Karakterlánc üzenetek blob műveletek időt.
Egy karakterláncot tartalmazó várólista üzenet **queueTrigger** hello használható helyőrző **Blob** attribútum **blobPath** hello tartalmát tartalmazó paraméter üdvözlőüzenetére.

hello alábbi példában **adatfolyam** blobok tooread és írási objektumokat. várólista üdvözlőüzenetére hello textblobs tárolóban lévő blob hello neve. Hello blob másolatát "-új" hozzáfűzött toohello neve jön létre az hello azonos tároló.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Hello **Blob** attribútum konstruktora vesz egy **blobPath** paraméter meghatározza, hogy hello tároló és a blob neve. A helyőrző kapcsolatos további információkért lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md).

Ha hello attribútum decorates egy **adatfolyam** objektumot egy másik konstruktor paraméterrel hello **FileAccess** olvasási, írási vagy olvasási/írási módban.

hello alábbi példában egy **CloudBlockBlob** toodelete blob objektum. várólista üdvözlőüzenetére hello blob hello neve.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
Egy POCO hello üzenetsor JSON-ként tárolja, használhatja, amely hello hello objektumának neve helyőrzők **várólista** attribútum **blobPath** paraméter. Metaadatok tulajdonság Várólistanevek helyőrzőként is használhatja. Lásd: [várólista vagy várólista üzenet metaadatok](#get-queue-or-queue-message-metadata).

hello alábbi példa másolja át a blob tooa új blob eltérő kiterjesztéssel. hello várólista az üzenet egy **BlobInformation** , amely tartalmazza az objektum **Blobnév** és **BlobNameWithoutExtension** tulajdonságait. hello tulajdonságnevek helyőrzőként hello blob elérési út a hello **Blob** attribútumok.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni. Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Ha toodo néhány működik a függvény egy blob tooan objektum kötés előtt, használható hello attribútum hello függvénytörzs hello, ahogy az [egy függvény törzséhez hello használata a WebJobs SDK attribútumokat](#use-webjobs-sdk-attributes-in-the-body-of-a-function).

### <a name="types-you-can-use-hello-blob-attribute-with"></a>Adattípusok hello Blob-attribútum
Hello **Blob** attribútum is használható a következő típusok hello:

* **Az adatfolyam** (olvasási vagy írási, hello FileAccess konstruktor paraméterrel megadott)
* **TextReader**
* **TextWriter**
* **karakterlánc** (olvasni.)
* **kimenő karakterlánc** (írási; hoz létre egy blobot, csak ha hello karakterlánc-paraméter null értékű hello függvény)
* POCO (olvasás)
* kimenő POCO (írás; mindig létrehoz egy blob, mint null objektumot hoz létre, ha POCO paraméter értéke null, ha hello függvény)
* **CloudBlobStream** (írás)
* **ICloudBlob** (olvasása vagy írása)
* **CloudBlockBlob** (olvasása vagy írása)
* **CloudPageBlob** (olvasása vagy írása)

## <a name="how-toohandle-poison-messages"></a>Hogyan toohandle elhalt üzenetek
Neve üzeneteket, amelyek tartalmát a függvény toofail hatására *elhalt üzenetek*. Hello függvény meghibásodása esetén üdvözlőüzenetére várólista nem törlődik, és végül van felvételre újra, ezért hello ciklus toobe ismétlődik. hello SDK automatikusan megszakíthatja hello ciklus korlátozott számú ismétlés után, vagy manuálisan is elvégezhető.

### <a name="automatic-poison-message-handling"></a>Automatikus elhalt üzenetek kezelésének
hello SDK telefonhívásokhoz too5 alkalommal tooprocess egy üzenetsor be egy olyan függvényt. Hello ötödik próbálja meghibásodásakor hello üzenet áthelyezett tooa poison várólista. Láthatja, hogyan tooconfigure hello az újrapróbálkozások maximális száma [hogyan tooset konfigurációs beállítások](#how-to-set-configuration-options).

hello poison várólista nevű *{originalqueuename}*-elhalt. Írhat egy függvény tooprocess üzenetek hello poison várólistából naplózásukhoz vagy, hogy szükség van-e a kézi beavatkozást értesítést küld.

A következő példa hello hello **CopyBlob** függvény sikertelenek lesznek, amikor egy üzenetsor-üzenetet, amely nem létezik blob hello nevét tartalmazza. Ebben az esetben üdvözlőüzenetére áthelyeznek hello copyblobqueue várólista toohello copyblobqueue-poison várólista. Hello **ProcessPoisonMessage** majd naplók hello az elhalt üzenet.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

        public static void ProcessPoisonMessage(
            [QueueTrigger("copyblobqueue-poison")] string blobName, TextWriter logger)
        {
            logger.WriteLine("Failed toocopy blob, name=" + blobName);
        }

hello alábbi ábrán az alábbi funkciók a konzol kimeneti az elhalt üzenet feldolgozásakor.

![Elhalt üzenetek kezelésének a konzol kimeneti](./media/vs-storage-webjobs-getting-started-queues/poison.png)

### <a name="manual-poison-message-handling"></a>Manuális elhalt üzenetek kezelésének
Hello szám, ahányszor egy üzenet felvételre feldolgozásra kaphat hozzáadásával egy **int** nevű paraméter **dequeueCount** tooyour függvény. Ezután a jelölőnégyzet hello created funkciókódot száma, és hajtsa végre a saját elhalt üzenetek kezelésének amikor hello száma meghaladja a küszöbértéket, ahogy az alábbi példa hello.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed toocopy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a name="how-tooset-configuration-options"></a>Hogyan tooset konfigurációs beállítások
Használhatja a hello **JobHostConfiguration** típus tooset hello alábbi konfigurációs beállítások:

* Hello SDK kapcsolati karakterláncok beállítása a kódban.
* Konfigurálása **QueueTrigger** beállításokat, például a maximális created száma.
* Várólistanevek konfigurációs beolvasása sikertelen.

### <a name="set-sdk-connection-strings-in-code"></a>A kód SDK kapcsolati karakterláncok beállítása
Hello SDK kapcsolati karakterláncok beállítása a kód lehetővé teszi, hogy Ön toouse saját kapcsolódási karakterlánc neve, a konfigurációs fájlok vagy a környezeti változók, ahogy az alábbi példa hello.

        static void Main(string[] args)
        {
            var _storageConn = ConfigurationManager
                .ConnectionStrings["MyStorageConnection"].ConnectionString;

            var _dashboardConn = ConfigurationManager
                .ConnectionStrings["MyDashboardConnection"].ConnectionString;

            var _serviceBusConn = ConfigurationManager
                .ConnectionStrings["MyServiceBusConnection"].ConnectionString;

            JobHostConfiguration config = new JobHostConfiguration();
            config.StorageConnectionString = _storageConn;
            config.DashboardConnectionString = _dashboardConn;
            config.ServiceBusConnectionString = _serviceBusConn;
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="configure-queuetrigger--settings"></a>QueueTrigger beállításainak konfigurálása
Hello következő toohello várólista üzenetfeldolgozást vonatkozó beállításokat konfigurálhatja:

* hello egyidejűleg végre párhuzamosan toobe átveszik várólista üzenetek maximális száma (alapértelmezett érték 16).
* Az újrapróbálkozások maximális számát hello tooa poison várólista várólista üzenet elküldése előtt (alapértelmezett érték 5).
* hello maximális várakozási idő előtt lekérdezési újra, ha üres-e a várólista (alapértelmezett érték 1 perc).

a következő példa azt mutatja meg hogyan hello tooconfigure ezeket a beállításokat:

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.Queues.BatchSize = 8;
            config.Queues.MaxDequeueCount = 4;
            config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

### <a name="set-values-for-webjobs-sdk-constructor-parameters-in-code"></a>Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
A várólista nevét, a blob neve vagy a tároló néha szeretné toospecify, vagy egy táblázatot a merevlemez-kód helyett a kód adjon neki nevet. Például érdemes toospecify hello várólista nevét **QueueTrigger** a konfigurációs fájl vagy a környezeti változóban.

A történő ehhez a **NameResolver** toohello objektum **JobHostConfiguration** típusa. Ön százalékjel (%) jelentkezik be a WebJobs SDK attribútum konstruktorparaméterek körülvett különleges helyőrzőket tartalmaznak, és a **NameResolver** kód hello tényleges értékek toobe használja ezeket a helyőrzők helyett határozza meg.

Tegyük fel, hogy azt szeretné, hogy toouse várólista neve például hello tesztkörnyezetben logqueuetest és egy elnevezett logqueueprod éles környezetben. A kódolt várólista neve helyett toospecify hello név egyik bejegyzésének hello kívánt **appSettings** gyűjteményt, amely rendelkezik hello tényleges várólista neve. Ha hello **appSettings** kulcs logqueue, a függvény a következő példa hello nézhet.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

A **NameResolver** osztály majd sikerült hello várólista nevét a **appSettings** a hello a következő példában látható módon:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Hello át **NameResolver** toohello osztály **JobHost** objektumot, ahogy az alábbi példa hello.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Megjegyzés:** várólista, a táblának és a blob nevének feloldása minden alkalommal, amikor egy függvény hívása esetén, de a blob-tároló nevének feloldása csak akkor, ha hello alkalmazás indítása. A blob-tároló neve nem módosítható, hello feladat futása közben.

## <a name="how-tootrigger-a-function-manually"></a>Hogyan tootrigger függvény manuálisan
egy függvény tootrigger manuálisan, használja a hello **hívás** vagy **CallAsync** hello metódusa **JobHost** objektum és hello **NoAutomaticTrigger** hello függvény, ahogy az alábbi példa hello attribútum.

        public class Program
        {
            static void Main(string[] args)
            {
                JobHost host = new JobHost();
                host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
            }

            [NoAutomaticTrigger]
            public static void CreateQueueMessage(
                TextWriter logger,
                string value,
                [Queue("outputqueue")] out string message)
            {
                message = value;
                logger.WriteLine("Creating queue message: ", message);
            }
        }

## <a name="how-toowrite-logs"></a>Hogyan naplózza az toowrite
hello irányítópulton látható naplók két helyen: hello webjobs-feladat hello lapját, és egy adott webjobs-feladat meghíváshoz hello lap.

![Naplózza a webjobs-feladat lap](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

![Naplók függvény meghívása lap](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

Egy függvény vagy hello hívó konzol módszerek kimenete **Main()** metódus a hello irányítópult-oldalon hello webjobs-feladat, és nem egy adott metódushívás hello oldalán megjelenik. Hello TextWriter objektum, hogy a paraméter a a metódus aláírásában kimenete hello irányítópult-oldalon metódushívási jelenik meg.

Konzol kimeneti nem lehet csatolt tooa adott metódushívás mert hello konzol egyszálas, miközben sok munkakörök futtathatnak: hello ugyanannyi időt vesz igénybe. Ezért hello SDK biztosít minden függvény meghívása a saját egyedi napló író objektummal.

toowrite [alkalmazás nyomkövetési naplóit](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), használjon **Console.Out** (INFO jelölésű naplókat hoz létre) és **Console.Error** (a hiba jelöléssel naplókat hoz létre). A másik lehetőség az toouse [nyomkövetési vagy TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), amely biztosítja, hogy részletes, figyelmeztetés, és kritikus szintek hozzáadása tooInfo és a hiba. Alkalmazás nyomkövetési naplóit hello webes alkalmazások naplófájljainak, Azure-táblákban, jelenik meg, vagy az Azure-blobok attól függően, hogy hogyan konfigurálja az Azure-webalkalmazásban. Mivel minden a konzol kimeneti értéke igaz, hello legutóbbi 100 alkalmazásnaplók a hello irányítópult-oldalon hello webjobs-feladat, a függvény hívása nem hello lapján is megjelennek.

Konzol eredmény jelenik meg, csak akkor, ha a hello program fut az Azure webjobs-feladat, nem hello program helyileg fut. Ha irányítópult hello vagy néhány más környezetben.

Hello irányítópult kapcsolati karakterlánc toonull beállításával letiltani a naplózást. További információkért lásd: [hogyan tooset konfigurációs beállítások](#how-to-set-configuration-options).

hello következő példa bemutatja, számos módon toowrite naplók:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

A WebJobs SDK irányítópult hello, hello hello kimenete **TextWriter** jön létre, amikor egy adott toohello lapján továbblépne meghívása funkciót, és válassza ki az objektum **váltása kimeneti**:

![Hívás-hivatkozás](./media/vs-storage-webjobs-getting-started-queues/dashboardinvocations.png)

![Naplók függvény meghívása lap](./media/vs-storage-webjobs-getting-started-queues/dashboardlogs.png)

A WebJobs SDK irányítópult hello, hello legutóbbi 100 sorok a konzol kimeneti megjelenítése mentése lépjen a webjobs-feladat hello (nem a hello függvény meghívása) toohello lapra, és válassza ki **váltása kimeneti**.

![Váltás kimeneti](./media/vs-storage-webjobs-getting-started-queues/dashboardapplogs.png)

Egy folyamatos webjobs-feladat, az alkalmazás naplóiban jelennek meg az/data/feladatok/folyamatos/*{webjobname}*/job_log.txt hello web app fájlrendszerben.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Az Azure blob hello alkalmazásban naplók néznek ki: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, hiba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Egy Azure-tábla hello a **Console.Out** és **Console.Error** naplók néznek ki:

![A tábla adatai napló](./media/vs-storage-webjobs-getting-started-queues/tableinfo.png)

![Hibanapló tábla](./media/vs-storage-webjobs-getting-started-queues/tableerror.png)

## <a name="next-steps"></a>Következő lépések
Ez a cikk nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei az Azure-üzenetsorok használata. További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure WebJobs-dokumentáció erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).


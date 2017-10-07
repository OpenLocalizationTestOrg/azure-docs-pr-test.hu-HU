---
title: "a WebJobs SDK hello Azure üzenetsor-kezelési tárolási aaaHow toouse"
description: "Ismerje meg, hogyan toouse Azure várólista hello WebJobs SDK szolgáltatással. Hozzon létre vagy töröljön a várólisták; Helyezze, Belepillantás, get, és törölje az üzenetsor-üzeneteket, és több."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: dbfac5d9-f4a0-4e3e-9ecc-af3d7bf80463
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: 49f844436b0453489800b2762a5c7dc30b9db805
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-queue-storage-with-hello-webjobs-sdk"></a>Hogyan toouse Azure várólista hello WebJobs SDK szolgáltatással
## <a name="overview"></a>Áttekintés
Ez az útmutató C# mintakódok hogyan toouse hello Azure WebJobs SDK-verzió megjelenítése 1.x az Azure queue storage szolgáltatás hello.

hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md) vagy túl[több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

A legtöbb hello kódtöredékek csak megjelenítése funkciók nem hello hello létrehozó kód mellől `JobHost` objektum ebben a példában látható módon:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

hello útmutató hello a következő témaköröket tartalmazza:

* [Hogyan tootrigger a függvény egy várólista-üzenet fogadásakor.](#trigger)
  * Karakterlánc-üzenetek
  * POCO üzenetek
  * Aszinkron funkciók
  * Típusok hello QueueTrigger attribútum együttműködik
  * Lekérdezési algoritmus
  * Több példánya
  * Párhuzamos végrehajtás
  * Várólista vagy várólista üzenet metaadatok beolvasása
  * Biztonságos leállításának
* [Hogyan toocreate várólista üzenet-várólista üzenet feldolgozása közben](#createqueue)
  * Karakterlánc-üzenetek
  * POCO üzenetek
  * Hozzon létre több üzenetet vagy aszinkron függvény
  * Típusok hello várólista attribútum együttműködik
  * Egy függvény törzséhez hello a WebJobs SDK attribútumok használata
* [Hogyan tooread és írási blobok egy várólista üzenet feldolgozása közben](#blobs)
  * Karakterlánc-üzenetek
  * POCO üzenetek
  * Típusok hello Blob attribútum együttműködik
* [Hogyan toohandle elhalt üzenetek](#poison)
  * Automatikus elhalt üzenetek kezelésének
  * Manuális elhalt üzenetek kezelésének
* [Hogyan tooset konfigurációs beállítások](#config)
  * A kód SDK kapcsolati karakterláncok beállítása
  * QueueTrigger beállításainak konfigurálása
  * Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* [Hogyan tootrigger függvény manuálisan](#manual)
* [Hogyan naplózza az toowrite](#logs)
* [Hogyan toohandle hibák és időtúllépések konfigurálása](#errors)
* [Következő lépések](#nextsteps)

## <a id="trigger"></a>Hogyan tootrigger a függvény egy várólista-üzenet fogadásakor.
toowrite egy függvényt, amely a WebJobs SDK hello meghívja a várólista-üzenet fogadásakor, használja a hello `QueueTrigger` attribútum. hello attribútum konstruktora a hello várólista toopoll hello nevét megadó karakterlánc-paramétert fogad. Emellett [hello várólista nevének beállítása dinamikusan](#config).

### <a name="string-queue-messages"></a>Karakterlánc-üzenetek
A következő példa hello, hello várólista tartalmaz egy karakterlánc-üzenet, így `QueueTrigger` alkalmazott tooa karakterlánc paraméter nevű `logMessage` tartalmazó hello hello várólista üzenet tartalma. hello függvény [írja a naplót üzenet toohello irányítópult](#logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Emellett `string`, hello lehet, egy bájttömböt egy `CloudQueueMessage` objektumot, vagy egy Ön által meghatározott POCO.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
A következő példa hello, várólista üdvözlőüzenetére tartalmazza a JSON a `BlobInformation` objektum, amely tartalmazza a `BlobName` tulajdonság. hello SDK automatikusan deserializes hello objektum.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers tooblob: " + blobInfo.BlobName);
        }

hello SDK használ hello [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) tooserialize és üzenetek deszerializálni. Ha létrehoz egy programot, amely nem használja a WebJobs SDK hello üzenetsor-üzeneteket, írhat kód például a következő példa toocreate egy POCO üzenetsor hello adott hello SDK tudja értelmezni.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Aszinkron funkciók
a következő aszinkron függvény hello [írja a naplót toohello irányítópult](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Aszinkron funkciók is igénybe vehet egy [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), ahogy az alábbi példa, amely egy blob másolja hello. (Hello előzetesben `queueTrigger` helyőrző, lásd: hello [Blobok](#blobs) szakaszában.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Típusok hello QueueTrigger attribútum együttműködik
Használhat `QueueTrigger` a következő típusok hello:

* `string`
* A JSON-ként szerializált POCO típus
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Lekérdezési algoritmus
hello SDK valósítja meg a tétlen-várólista tárolási tranzakciós költségeket a lekérdezési véletlenszerű exponenciális vissza az indító algoritmus tooreduce hello hatását.  Ha üzenet található, hello SDK két másodpercet vár, és ellenőrzi, egy másik üzenet; Ha üzenet található azt, mielőtt újra próbálkozna körülbelül négy másodpercet vár. Későbbi sikertelen bejelentkezési kísérletek tooget egy üzenetsor-üzenetet, miután a hello várakozási idő tooincrease folytatódik, amíg eléri a maximális várakozási idő hello, mely alapértelmezett tooone perc. [hello maximális várakozási idő az konfigurálható](#config).

### <a id="instances"></a>Több példánya
Ha több példányt a webalkalmazás fut, egy folyamatos webjobs-feladat fut az egyes gépek, és minden gép fog Várjon, amíg az eseményindítók és toorun funkciók kísérlet. hello WebJobs SDK várólista eseményindító automatikusan megakadályozza, hogy egy függvény feldolgozási sor üzenetet többször; funkciók nincs toobe toobe idempotent írása. Azonban ha azt szeretné, hogy tooensure, hogy egy függvény csak egy példányban fut, akkor is, ha hello állomás webes alkalmazás több példánya van, használhatja a hello `Singleton` attribútum.

### <a id="parallel"></a>Párhuzamos végrehajtás
Ha több funkciók különböző várólisták figyel, hello SDK fogja hívni azokat párhuzamosan üzenetek fogadása egy időben.

hello azonos érvényét veszti, ha több üzenetek fogadása az egy adott sorba. Alapértelmezés szerint hello SDK egyszerre az 16 várólista üzenetkötegek lekérdezi és feldolgozza azokat párhuzamosan hello függvény végrehajtása. [hello köteg mérete konfigurálható](#config). Ha feldolgozott hello szám lefelé toohalf hello kötegelt méretű lekérdezi, hello SDK lekérdezi egy másik köteg, és megkezdi az üzenetek feldolgozását. Ezért hello maximális száma párhuzamos éppen feldolgozott üzeneteinek másodpercenkénti függvény, egy és egy félig alkalommal hello kötegmérete. Ez a korlátozás vonatkozik külön-külön tooeach függvény, amely rendelkezik egy `QueueTrigger` attribútum.

Ha nem szeretné, egy olyan sort a fogadott üzenetet párhuzamos futtatáshoz, hello köteg mérete too1 állíthatja be. Lásd még: **várólista feldolgozása teljesebb körű vezérlése** a [Azure WebJobs SDK 1.1.0-ás RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>Várólista vagy várólista üzenet metaadatok beolvasása
Kaphat a következő üzenet tulajdonságai toohello metódus-aláírás paraméterek hozzáadásával hello:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (tartalmazza a szöveges üzenet)
* `string`azonosítója
* `string`popReceipt
* `int`dequeueCount

Ha azt szeretné, közvetlenül az Azure storage API hello toowork, azt is megteheti egy `CloudStorageAccount` paraméter.

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

### <a id="graceful"></a>Biztonságos leállításának
Egy függvény a folyamatos webjobs-feladat fut fogad el egy `CancellationToken` paramétert, amely lehetővé teszi, hogy ha hello hello operációs rendszer toonotify hello függvény a webjobs-feladat megszakadt toobe tárgya. Az értesítési toomake meg arról, hogy hello függvény nem bontható váratlanul körbe, hogy adatok inkonzisztens állapotban is használhatja.

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

## <a id="createqueue"></a>Hogyan toocreate várólista üzenet-várólista üzenet feldolgozása közben
egy függvényt, amely létrehoz egy új sor üzenetet, használjon hello toowrite `Queue` attribútum. Például `QueueTrigger`, adja meg a várólistacímke hello karakterláncból, vagy beállíthatja [hello várólista nevének beállítása dinamikusan](#config).

### <a name="string-queue-messages"></a>Karakterlánc-üzenetek
a következő példakód nem aszinkron hello hello várólista "outputqueue" nevű új várólista üzenet ugyanaz tartalom másként hello üzenetsorban található üzenetet kapott hello várólista "inputqueue" nevű hello hoz létre. (Aszinkron funkciók használata `IAsyncCollector<T>` később az itt látható módon.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
toocreate egy kimeneti paraméter toohello írja egy üzenetsor-üzenetet, amely tartalmazza a karakterláncot, pass hello POCO helyett egy POCO `Queue` attribútumok konstruktorában.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

hello SDK automatikusan hello objektum tooJSON rendezi sorba. Egy üzenetsor mindig létrejön, még akkor is, ha hello objektum értéke null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Hozzon létre több üzenetet vagy aszinkron függvény
toocreate több üzenetet, hogy hello paramétertípus hello kimeneti várólista `ICollector<T>` vagy `IAsyncCollector<T>`, ahogy az alábbi példa hello.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Minden várólista-üzenet létrehozásakor a rendszer azonnal hello `Add` metódust.

### <a name="types-that-hello-queue-attribute-works-with"></a>Típusok hello várólista attribútum, amely együttműködik
Használhatja a hello `Queue` hello paramétereinek típusa a következő attribútumot:

* `out string`(hoz létre a üzenetsor-üzenetet, ha a paraméter értéke nem null értékű hello függvény végén)
* `out byte[]`(működik, mint az `string`)
* `out CloudQueueMessage`(működik, mint az `string`)
* `out POCO`(egy szerializálható típust, létrehoz egy üzenetet null objektummal rendelkező Ha hello paraméterének értéke null, ha hello függvény karakterlánccal végződik-e)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(az üzenet segítségével manuálisan közvetlenül hello Azure Storage API létrehozása)

### <a id="ibinder"></a>Egy függvény törzséhez hello a WebJobs SDK attribútumok használata
Ha toodo kell néhány működik a függvény a WebJobs SDK attribútum használatához `Queue`, `Blob`, vagy `Table`, használhatja a hello `IBinder` felületet.

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

Hello `IBinder` felület is használható hello `Table` és `Blob` attribútumok.

## <a id="blobs"></a>Hogyan tooread és írási blobok és táblák egy várólista üzenet feldolgozása közben
Hello `Blob` és `Table` attribútumok lehetővé teszik a tooread, és írja blobok és táblákat. Ebben a szakaszban hello minták tooblobs alkalmazni. Hogyan tootrigger dolgozza fel, ha a blobok létrehozott vagy frissített megjelenítése mintakódok, lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), és mintakódok olvasási és írási táblák, lásd: [hogyan toouse Azure-tábla tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Karakterlánc üzenetek blob műveletek időt.
Egy karakterláncot tartalmazó várólista üzenet `queueTrigger` hello használható helyőrző `Blob` attribútum `blobPath` üdvözlőüzenetére hello tartalmát tartalmazó paraméter.

hello alábbi példában `Stream` blobok tooread és írási objektumokat. várólista üdvözlőüzenetére hello textblobs tárolóban lévő blob hello neve. Hello blob másolatát "-új" hozzáfűzött toohello neve jön létre az hello azonos tároló.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Hello `Blob` attribútum konstruktora vesz egy `blobPath` paraméter meghatározza, hogy hello tároló és a blob neve. A helyőrző kapcsolatos további információkért lásd: [hogyan toouse Azure blob-tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),

Ha hello attribútum decorates egy `Stream` objektumot egy másik konstruktor paraméterrel hello `FileAccess` olvasási, írási vagy olvasási/írási módban.

hello alábbi példában egy `CloudBlockBlob` toodelete blob objektum. várólista üdvözlőüzenetére hello blob hello neve.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
Egy POCO hello üzenetsor JSON-ként tárolja, használhatja, amely hello hello objektumának neve helyőrzők `Queue` attribútum `blobPath` paraméter. Is [metaadatok tulajdonságnevek várólistára](#queuemetadata) helyőrzőként.

hello alábbi példa másolja át a blob tooa új blob eltérő kiterjesztéssel. hello várólista az üzenet egy `BlobInformation` , amely tartalmazza az objektum `BlobName` és `BlobNameWithoutExtension` tulajdonságok. hello tulajdonságnevek helyőrzőként hello blob elérési út a hello `Blob` attribútumok.

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

Ha néhány működik toodo a függvényben előtt kötelező egy blob tooan objektum, használhatja a hello attribútum hello függvénytörzs hello, [ahogy korábban hello várólista attribútum](#ibinder).

### <a id="blobattributetypes"></a>Adattípusok hello Blob-attribútum
Hello `Blob` attribútum is használható a következő típusok hello:

* `Stream`(olvasására vagy írására, hello FileAccess konstruktor paraméterrel megadott)
* `TextReader`
* `TextWriter`
* `string`(olvasás)
* `out string`(írási; hoz létre egy blobot, csak ha hello karakterlánc-paraméter null értékű hello függvény)
* POCO (olvasás)
* kimenő POCO (írás; mindig létrehoz egy blob, mint null objektumot hoz létre, ha POCO paraméter értéke null, ha hello függvény)
* `CloudBlobStream`(írja)
* `ICloudBlob`(olvasás vagy írás)
* `CloudBlockBlob`(olvasás vagy írás)
* `CloudPageBlob`(olvasás vagy írás)

## <a id="poison"></a>Hogyan toohandle elhalt üzenetek
Neve üzeneteket, amelyek tartalmát a függvény toofail hatására *elhalt üzenetek*. Hello függvény meghibásodása esetén üdvözlőüzenetére várólista nem törlődik, és végül van felvételre újra, ezért hello ciklus toobe ismétlődik. hello SDK automatikusan megszakíthatja hello ciklus korlátozott számú ismétlés után, vagy manuálisan is elvégezhető.

### <a name="automatic-poison-message-handling"></a>Automatikus elhalt üzenetek kezelésének
hello SDK telefonhívásokhoz too5 alkalommal tooprocess egy üzenetsor be egy olyan függvényt. Hello ötödik próbálja meghibásodásakor hello üzenet áthelyezett tooa poison várólista. [Az újrapróbálkozások maximális számát hello konfigurálható](#config).

hello poison várólista nevű *{originalqueuename}*-elhalt. Írhat egy függvény tooprocess üzenetek hello poison várólistából naplózásukhoz vagy, hogy szükség van-e a kézi beavatkozást értesítést küld.

A következő példa hello hello `CopyBlob` függvény sikertelenek lesznek, amikor egy üzenetsor-üzenetet, amely nem létezik blob hello nevét tartalmazza. Ebben az esetben üdvözlőüzenetére áthelyeznek hello copyblobqueue várólista toohello copyblobqueue-poison várólista. Hello `ProcessPoisonMessage` majd naplók hello az elhalt üzenet.

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

![Elhalt üzenetek kezelésének a konzol kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Manuális elhalt üzenetek kezelésének
Hello szám, ahányszor egy üzenet felvételre feldolgozásra kaphat hozzáadásával egy `int` nevű paraméter `dequeueCount` tooyour függvény. Ezután a jelölőnégyzet hello created funkciókódot száma, és hajtsa végre a saját elhalt üzenetek kezelésének amikor hello száma meghaladja a küszöbértéket, ahogy az alábbi példa hello.

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

## <a id="config"></a>Hogyan tooset konfigurációs beállítások
Használhatja a hello `JobHostConfiguration` típus tooset hello alábbi konfigurációs beállítások:

* Hello SDK kapcsolati karakterláncok beállítása a kódban.
* Konfigurálása `QueueTrigger` beállításokat, például a maximális created száma.
* Várólistanevek konfigurációs beolvasása sikertelen.

### <a id="setconnstr"></a>A kód SDK kapcsolati karakterláncok beállítása
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

### <a id="configqueue"></a>QueueTrigger beállításainak konfigurálása
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

### <a id="setnamesincode"></a>Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
A várólista nevét, a blob neve vagy a tároló néha szeretné toospecify, vagy egy táblázatot a merevlemez-kód helyett a kód adjon neki nevet. Például érdemes toospecify hello várólista nevét `QueueTrigger` a konfigurációs fájl vagy a környezeti változóban.

A történő ehhez a `NameResolver` toohello objektum `JobHostConfiguration` típusa. Ön százalékjel (%) jelentkezik be a WebJobs SDK attribútum konstruktorparaméterek körülvett különleges helyőrzőket tartalmaznak, és a `NameResolver` kód hello tényleges értékek toobe használja ezeket a helyőrzők helyett határozza meg.

Tegyük fel, hogy azt szeretné, hogy toouse várólista neve például hello tesztkörnyezetben logqueuetest és egy elnevezett logqueueprod éles környezetben. A kódolt várólista neve helyett toospecify hello név egyik bejegyzésének hello kívánt `appSettings` gyűjteményt, amely rendelkezik hello tényleges várólista neve. Ha hello `appSettings` kulcs logqueue, a függvény a következő példa hello nézhet.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

A `NameResolver` osztály majd sikerült hello várólista nevét a `appSettings` a hello a következő példában látható módon:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Hello át `NameResolver` toohello osztály `JobHost` objektumot, ahogy az alábbi példa hello.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Megjegyzés:** várólista, a táblának és a blob nevének feloldása minden alkalommal, amikor egy függvény hívása esetén, de a blob-tároló nevének feloldása csak akkor, ha hello alkalmazás indítása. A blob-tároló neve nem módosítható, hello feladat futása közben.

## <a id="manual"></a>Hogyan tootrigger függvény manuálisan
egy függvény tootrigger manuálisan, használja a hello `Call` vagy `CallAsync` hello metódusa `JobHost` objektum és hello `NoAutomaticTrigger` hello függvény, ahogy az alábbi példa hello attribútuma.

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

## <a id="logs"></a>Hogyan naplózza az toowrite
hello irányítópulton látható naplók két helyen: hello webjobs-feladat hello lapját, és egy adott webjobs-feladat meghíváshoz hello lap.

![Naplózza a webjobs-feladat lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Egy függvény vagy hello hívó konzol módszerek kimenete `Main()` metódus a hello irányítópult-oldalon hello webjobs-feladat, és nem egy adott metódushívás hello oldalán megjelenik. Hello TextWriter objektum, hogy a paraméter a a metódus aláírásában kimenete hello irányítópult-oldalon metódushívási jelenik meg.

Konzol kimeneti nem lehet csatolt tooa adott metódushívás mert hello konzol egyszálas, miközben sok munkakörök futtathatnak: hello ugyanannyi időt vesz igénybe. Ezért hello SDK biztosít minden függvény meghívása a saját egyedi napló író objektummal.

toowrite [alkalmazás nyomkövetési naplóit](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), használjon `Console.Out` (INFO jelölésű naplókat hoz létre) és `Console.Error` (a hiba jelöléssel naplókat hoz létre). A másik lehetőség az toouse [nyomkövetési vagy TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), amely biztosítja, hogy részletes, figyelmeztetés, és kritikus szintek hozzáadása tooInfo és a hiba. Alkalmazás nyomkövetési naplóit hello webes alkalmazások naplófájljainak, Azure-táblákban, jelenik meg, vagy az Azure-blobok attól függően, hogy hogyan konfigurálja az Azure-webalkalmazásban. Mivel minden a konzol kimeneti értéke igaz, hello legutóbbi 100 alkalmazásnaplók a hello irányítópult-oldalon hello webjobs-feladat, a függvény hívása nem hello lapján is megjelennek.

Konzol eredmény jelenik meg, csak akkor, ha a hello program fut az Azure webjobs-feladat, nem hello program helyileg fut. Ha irányítópult hello vagy néhány más környezetben.

Tiltsa le a magas teljesítmény forgatókönyvek irányítópult naplózást. Alapértelmezés szerint hello SDK írja a naplókat toostorage, és ez a tevékenység ronthatja a teljesítményt, hány üzenet feldolgozásakor. naplózás, toodisable hello irányítópult kapcsolati karakterlánc toonull állítsa be, ahogy az alábbi példa hello.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

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

A WebJobs SDK irányítópult hello, hello hello kimenete `TextWriter` jön létre, amikor egy adott toohello lapján továbblépne meghívása funkciót, és kattintson az objektum **váltása kimeneti**:

![Függvény meghívása hivatkozásra](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

A WebJobs SDK irányítópult hello, hello legutóbbi 100 sorok konzol kimeneti megjelenítése mentése során, lépjen a webjobs-feladat hello (nem a hello függvény meghívása) toohello lapra, majd kattintson az **váltása kimeneti**.

![Kattintson a Váltás kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

Egy folyamatos webjobs-feladat, az alkalmazás naplóiban jelennek meg az/data/feladatok/folyamatos/*{webjobname}*/job_log.txt hello web app fájlrendszerben.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Az Azure blob hello alkalmazásban naplók néznek ki: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13, hiba, contosoadsnew, 491e54, 635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Egy Azure-tábla hello a `Console.Out` és `Console.Error` naplók néznek ki:

![A tábla adatai napló](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Hibanapló tábla](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Ha azt szeretné, a saját naplózó tooplug, lásd: [ebben a példában](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Hogyan toohandle hibák és időtúllépések konfigurálása
hello WebJobs SDK is magában foglalja a [időtúllépés](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) , melyekkel egy függvény toobe megszűnik, ha toocause attribútum nem fejezi be a megadott időn belül. Ha azt szeretné tooraise riasztást, ha túl sok hiba fordulhat elő, a megadott időn belül, hello is használható `ErrorTrigger` attribútum. Íme egy [ErrorTrigger példa](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    too= "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors toohello Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Dinamikusan is letiltja, és a funkciók toocontrol engedélyezi, hogy akkor is elindítható, a konfiguráció kapcsoló, amelyet egy Alkalmazásbeállítás vagy a környezeti változó neve. Mintakód, lásd: hello `Disable` attribútumnak [hello WebJobs SDK-minták tárház](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott a kódot, hogy hogyan minták toohandle gyakori forgatókönyvei az Azure-üzenetsorok használata. További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

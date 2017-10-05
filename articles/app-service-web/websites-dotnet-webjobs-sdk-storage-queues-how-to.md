---
title: "Az Azure Queue Storage használata a WebJobs SDK-val"
description: "Ismerje meg az Azure a queue storage használata a WebJobs SDK-val. Hozzon létre vagy töröljön a várólisták; Helyezze, Belepillantás, get, és törölje az üzenetsor-üzeneteket, és több."
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
ms.openlocfilehash: 63b987a2c9471f2929b8d2dd605323910d2ad43b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-queue-storage-with-the-webjobs-sdk"></a>Az Azure Queue Storage használata a WebJobs SDK-val
## <a name="overview"></a>Áttekintés
Ez az útmutató C# mintakódok az Azure WebJobs SDK-verzió használatát mutatják be az Azure üzenetsor-kezelési tárolási szolgáltatással 1.x.

Az útmutató azt feltételezi, hogy tudja [hogyan webjobs-feladat-projekt létrehozása a Visual Studio kapcsolati karakterláncok a tárfiókhoz adott pontra](websites-dotnet-webjobs-sdk-get-started.md) vagy [több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

A legtöbb kódrészletek csak megjelenítése funkciók, nem a kódot, amely létrehozza a `JobHost` objektum ebben a példában látható módon:

        static void Main(string[] args)
        {
            JobHost host = new JobHost();
            host.RunAndBlock();
        }

Az útmutató a következő témakörökből áll:

* [Egy függvény események várólista üzenet fogadásakor.](#trigger)
  * Karakterlánc-üzenetek
  * POCO üzenetek
  * Aszinkron funkciók
  * Együttműködve biztosítja a QueueTrigger attribútum típusa
  * Lekérdezési algoritmus
  * Több példánya
  * Párhuzamos végrehajtás
  * Várólista vagy várólista üzenet metaadatok beolvasása
  * Biztonságos leállításának
* [Egy várólista üzenet feldolgozása közben egy üzenetsor létrehozása](#createqueue)
  * Karakterlánc-üzenetek
  * POCO üzenetek
  * Hozzon létre több üzenetet vagy aszinkron függvény
  * A várólista attribútum együttműködve típusok
  * Egy függvény törzséhez a WebJobs SDK attribútumok használata
* [Olvasási és írási blobok várólista üzenet feldolgozásakor a program hogyan](#blobs)
  * Karakterlánc-üzenetek
  * POCO üzenetek
  * A Blob attribútum együttműködve típusok
* [Az elhalt üzenetek kezelésének módját](#poison)
  * Automatikus elhalt üzenetek kezelésének
  * Manuális elhalt üzenetek kezelésének
* [Hogyan konfigurációs beállítások megadása](#config)
  * A kód SDK kapcsolati karakterláncok beállítása
  * QueueTrigger beállításainak konfigurálása
  * Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* [Hogyan kell manuálisan kezdeményezi egy függvény](#manual)
* [Naplók írásával](#logs)
* [Hibák kezelésének és időtúllépések konfigurálása](#errors)
* [Következő lépések](#nextsteps)

## <a id="trigger"></a>Egy függvény események várólista üzenet fogadásakor.
Egy függvényt, amely a WebJobs SDK meghívja a várólista-üzenet fogadásakor használja a `QueueTrigger` attribútum. Az attribútum konstruktora karakterlánc paramétert, és kérdezze le a várólista nevét adja meg. Emellett [dinamikusan beállítani a várólista nevét](#config).

### <a name="string-queue-messages"></a>Karakterlánc-üzenetek
A következő példában a várólista tartalmaz egy karakterlánc-üzenet, így `QueueTrigger` egy karakterlánc-paramétert nevű alkalmazott `logMessage` tartalmazó az üzenetsorban lévő üzenetet tartalmát. A függvény [egy naplófájlüzenetre ír az irányítópult](#logs).

        public static void ProcessQueueMessage([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            logger.WriteLine(logMessage);
        }

Emellett `string`, a paraméter lehet egy bájttömböt egy `CloudQueueMessage` objektumot, vagy egy Ön által meghatározott POCO.

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
A következő példában az üzenetsorban lévő üzenetet tartalmazza a JSON a `BlobInformation` objektum, amely tartalmazza a `BlobName` tulajdonság. Az SDK automatikusan deserializes az objektum.

        public static void WriteLogPOCO([QueueTrigger("logqueue")] BlobInformation blobInfo, TextWriter logger)
        {
            logger.WriteLine("Queue message refers to blob: " + blobInfo.BlobName);
        }

Az SDK-t használ a [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) szerializálása és deszerializálása üzenetek. Üzenetsor-üzeneteket hoz létre egy programot, amely a WebJobs SDK nem használja, ha írhat kódot létrehozni egy POCO üzenetsor-üzenetet, amely az SDK tudja értelmezni az alábbi példához hasonló.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "log.txt" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

### <a name="async-functions"></a>Aszinkron funkciók
A következő aszinkron függvény [napló ír az irányítópult](#logs).

        public async static Task ProcessQueueMessageAsync([QueueTrigger("logqueue")] string logMessage, TextWriter logger)
        {
            await logger.WriteLineAsync(logMessage);
        }

Aszinkron funkciók is igénybe vehet egy [cancellation token](http://www.asp.net/mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4#CancelToken), ahogy az az alábbi példa, amely egy blob másolja. (További tájékoztatás a `queueTrigger` helyőrző, tekintse meg a [Blobok](#blobs) szakasz.)

        public async static Task ProcessQueueMessageAsyncCancellationToken(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput,
            CancellationToken token)
        {
            await blobInput.CopyToAsync(blobOutput, 4096, token);
        }

### <a id="qtattributetypes"></a>Együttműködve biztosítja a QueueTrigger attribútum típusa
Használhat `QueueTrigger` a következő típusú:

* `string`
* A JSON-ként szerializált POCO típus
* `byte[]`
* `CloudQueueMessage`

### <a id="polling"></a>Lekérdezési algoritmus
Az SDK-t egy véletlenszerű exponenciális vissza az indító algoritmus üresjárati-várólista tárolási tranzakciós költségeket a lekérdezési hatásainak csökkentése érdekében valósít meg.  Ha üzenet található, az SDK két másodpercet vár, és ellenőrzi, egy másik üzenet; Ha üzenet található azt, mielőtt újra próbálkozna körülbelül négy másodpercet vár. A várakozási idő után további sikertelenül megpróbálják a várólista üzenet jelenik meg, folyamatosan nő, nem éri a maximális várakozási idő, alapértelmezett értéke, egy perc alatt. [A maximális várakozási idő az konfigurálható](#config).

### <a id="instances"></a>Több példánya
Ha több példányt a webalkalmazás fut, egy folyamatos webjobs-feladat fut az egyes gépek, és minden gép fog Várjon, amíg az eseményindítók és funkciók futtatására tett kísérlet. A WebJobs SDK várólista eseményindító automatikusan megakadályozza, hogy a függvény egy várólista üzenet feldolgozása többször; funkciók nem kell az idempotent kell írni. Azonban ha biztos szeretne lenni abban, hogy csak egy példányát a függvény fut, akkor is, ha a gazdagép webes alkalmazás több példánya van, használja a `Singleton` attribútum.

### <a id="parallel"></a>Párhuzamos végrehajtás
Ha több funkciók különböző várólisták figyeli, az SDK-t fogja hívni azokat párhuzamosan üzenetek fogadása egy időben.

Ugyanez vonatkozik több üzenetet egyetlen várólista fogadásakor. Alapértelmezés szerint az SDK egyszerre az 16 várólista üzenetkötegek lekérdezi és feldolgozza azokat párhuzamosan függvény végrehajtása. [A Köteg mérete nem konfigurálható](#config). A feldolgozott számát a Köteg mérete felének lekérdezi, ha az SDK-t egy másik köteg lekérdezi, és megkezdi az üzenetek feldolgozását. Ezért a maximális száma párhuzamos éppen feldolgozott üzeneteinek másodpercenkénti függvény, egy és egy félig kötegelt méretének. Ezt a határt külön vonatkozik minden funkció, amely rendelkezik egy `QueueTrigger` attribútum.

Ha nem szeretné, egy olyan sort a fogadott üzenetet párhuzamos futtatáshoz, beállíthatja a köteg méretének 1. Lásd még: **várólista feldolgozása teljesebb körű vezérlése** a [Azure WebJobs SDK 1.1.0-ás RTM](https://azure.microsoft.com/blog/azure-webjobs-sdk-1-1-0-rtm/).

### <a id="queuemetadata"></a>Várólista vagy várólista üzenet metaadatok beolvasása
A következő üzenet tulajdonságai kaphat paramétereket ad a metódus-aláírás:

* `DateTimeOffset`expirationTime
* `DateTimeOffset`insertionTime
* `DateTimeOffset`nextVisibleTime
* `string`queueTrigger (tartalmazza a szöveges üzenet)
* `string`azonosítója
* `string`popReceipt
* `int`dequeueCount

Ha közvetlenül az Azure storage API dolgozni szeretne, azt is megteheti egy `CloudStorageAccount` paraméter.

A következő példa egy információ alkalmazás naplófájlba írja a metaadatok mindegyikét. A példában logMessage és queueTrigger is tartalmazhat az üzenetsorban lévő üzenetet tartalmát.

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

Íme egy minta naplót kell írni a minta kóddal:

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
Egy függvény a folyamatos webjobs-feladat fut fogad el egy `CancellationToken` paramétert, amely lehetővé teszi, hogy az operációs rendszer, a függvény értesíti, amikor a webjobs-feladat megszakítása. Az értesítés segítségével győződjön meg arról, hogy a függvény nem bontható váratlanul oly módon, hogy az adatok inkonzisztens állapotban hagyja.

A következő példa bemutatja, hogyan közelgő webjobs-feladat futása függvények kereséséhez.

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

**Megjegyzés:** az irányítópult előfordulhat, hogy helyesen jelenjen meg az állapot és a kimeneti funkciót le lett állítva.

További információkért lásd: [webjobs-feladatok szabályos leállítást](http://blog.amitapple.com/post/2014/05/webjobs-graceful-shutdown/#.VCt1GXl0wpR).   

## <a id="createqueue"></a>Egy várólista üzenet feldolgozása közben egy üzenetsor létrehozása
Egy függvényt, amely létrehoz egy új sor írása, használja a `Queue` attribútum. Például `QueueTrigger`, adja meg a várólista neve karakterláncként vagy beállíthatja a [dinamikusan beállítani a várólista nevét](#config).

### <a name="string-queue-messages"></a>Karakterlánc-üzenetek
A következő nem aszinkron kódminta létrehoz egy új várólista-üzenetet a várólistában, ugyanahhoz a tartalomhoz, mint az üzenetsorban lévő üzenetet kapott a várólista "inputqueue" nevű, "outputqueue" nevű. (Aszinkron funkciók használata `IAsyncCollector<T>` később az itt látható módon.)

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] out string outputQueueMessage )
        {
            outputQueueMessage = queueMessage;
        }

### <a name="poco-plain-old-clr-objecthttpenwikipediaorgwikiplainoldclrobject-queue-messages"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
Hozzon létre egy üzenetsor-üzenetet, amely egy karakterlánc helyett egy POCO tartalmaz, adja át a POCO típus az output paraméterként a `Queue` attribútumok konstruktorában.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] BlobInformation blobInfoInput,
            [Queue("outputqueue")] out BlobInformation blobInfoOutput )
        {
            blobInfoOutput = blobInfoInput;
        }

Az SDK automatikusan a JSON-objektum rendezi sorba. Egy üzenetsor mindig létrejön, még akkor is, ha az objektum értéke null.

### <a name="create-multiple-messages-or-in-async-functions"></a>Hozzon létre több üzenetet vagy aszinkron függvény
Több üzenetet létrehozni, győződjön meg a paraméter típusa a kimeneti várólista `ICollector<T>` vagy `IAsyncCollector<T>`, a következő példában látható módon.

        public static void CreateQueueMessages(
            [QueueTrigger("inputqueue")] string queueMessage,
            [Queue("outputqueue")] ICollector<string> outputQueueMessage,
            TextWriter logger)
        {
            logger.WriteLine("Creating 2 messages in outputqueue");
            outputQueueMessage.Add(queueMessage + "1");
            outputQueueMessage.Add(queueMessage + "2");
        }

Minden várólista üzenet jön létre azonnal amikor a `Add` metódust.

### <a name="types-that-the-queue-attribute-works-with"></a>Amely a várólista attribútum kompatibilis típusok
Használhatja a `Queue` attribútum a következő paraméter típusa:

* `out string`(hoz létre üzenetsor-üzenetet, ha a paraméter értéke nem null értékű akkor, ha a függvény karakterlánccal végződik-e)
* `out byte[]`(működik, mint az `string`)
* `out CloudQueueMessage`(működik, mint az `string`)
* `out POCO`(egy szerializálható típust, létrehoz egy üzenetet null objektummal rendelkező, ha a paraméter értéke null, ha a függvény karakterlánccal végződik-e)
* `ICollector`
* `IAsyncCollector`
* `CloudQueue`(a manuális közvetlenül az Azure Storage API használatával üzenet létrehozása)

### <a id="ibinder"></a>Egy függvény törzséhez a WebJobs SDK attribútumok használata
Ha néhány a munkáját a függvény használata, mint a WebJobs SDK-attribútum előtt kell `Queue`, `Blob`, vagy `Table`, használhatja a `IBinder` felületet.

Az alábbi példa időt vesz igénybe egy bemeneti várólista-üzenet, és létrehoz egy új üzenetet a kimeneti várólistában lévő ugyanahhoz a tartalomhoz. A kimeneti várólista neve van beállítva, a függvény törzséhez tartozó kóddal.

        public static void CreateQueueMessage(
            [QueueTrigger("inputqueue")] string queueMessage,
            IBinder binder)
        {
            string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
            QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
            CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
            outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
        }

A `IBinder` felület is használható a `Table` és `Blob` attribútumok.

## <a id="blobs"></a>Hogyan olvashatja és írhatja a blobok és táblák egy várólista üzenet feldolgozása közben
A `Blob` és `Table` attribútumok lehetővé teszik a olvasását és írását, blobok és táblákat. Ebben a szakaszban a mintákat a blobok vonatkozik. Mutatják be, akkor a blobok létrehozott vagy frissített folyamatok vált mintakódok, lásd: [Azure blob storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md), és mintakódok olvasási és írási táblák, lásd: [Azure table storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-tables-how-to.md).

### <a name="string-queue-messages-triggering-blob-operations"></a>Karakterlánc üzenetek blob műveletek időt.
Egy karakterláncot tartalmazó várólista üzenet `queueTrigger` használható helyőrző a `Blob` attribútum `blobPath` paraméter, amely tartalmazza az üzenet tartalmát.

Az alábbi példában `Stream` objektumok olvasását és írását blobokat. Az üzenetsorban lévő üzenetet a textblobs tárolóban lévő blob neve. Blob másolatát "-új" hozzáfűzi a nevet ugyanabban a tárolóban jön létre.

        public static void ProcessQueueMessage(
            [QueueTrigger("blobcopyqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}",FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new",FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

A `Blob` attribútum konstruktora vesz egy `blobPath` paraméter meghatározza, hogy a tároló és a blob neve. A helyőrző kapcsolatos további információkért lásd: [Azure blob storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md),

Ha az attribútum decorates egy `Stream` objektumot egy másik konstruktor paraméter határozza meg a `FileAccess` olvasási, írási vagy olvasási/írási módban.

Az alábbi példában egy `CloudBlockBlob` blob törlendő objektum. Az üzenetsorban lévő üzenetet a blob neve.

        public static void DeleteBlob(
            [QueueTrigger("deleteblobqueue")] string blobName,
            [Blob("textblobs/{queueTrigger}")] CloudBlockBlob blobToDelete)
        {
            blobToDelete.Delete();
        }

### <a id="pocoblobs"></a>POCO [(egyszerű régi CLR objektum](http://en.wikipedia.org/wiki/Plain_Old_CLR_Object)) várólista-üzenetek
Egy POCO az üzenetsorban lévő üzenetet JSON-ként tárolja, a helyőrzők, amelyek neve az objektum tulajdonságainak használhatják a `Queue` attribútum `blobPath` paraméter. Is [metaadatok tulajdonságnevek várólistára](#queuemetadata) helyőrzőként.

A következő példa egy új blobot egy másik kiterjesztést a blob másolja. A várólista az üzenet egy `BlobInformation` , amely tartalmazza az objektum `BlobName` és `BlobNameWithoutExtension` tulajdonságok. A tulajdonságnevek helyőrzőként a blob elérési útját a `Blob` attribútumok.

        public static void CopyBlobPOCO(
            [QueueTrigger("copyblobqueue")] BlobInformation blobInfo,
            [Blob("textblobs/{BlobName}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{BlobNameWithoutExtension}.txt", FileAccess.Write)] Stream blobOutput)
        {
            blobInput.CopyTo(blobOutput, 4096);
        }

Az SDK-t használ a [Newtonsoft.Json NuGet-csomag](http://www.nuget.org/packages/Newtonsoft.Json) szerializálása és deszerializálása üzenetek. Üzenetsor-üzeneteket hoz létre egy programot, amely a WebJobs SDK nem használja, ha írhat kódot létrehozni egy POCO üzenetsor-üzenetet, amely az SDK tudja értelmezni az alábbi példához hasonló.

        BlobInformation blobInfo = new BlobInformation() { BlobName = "boot.log", BlobNameWithoutExtension = "boot" };
        var queueMessage = new CloudQueueMessage(JsonConvert.SerializeObject(blobInfo));
        logQueue.AddMessage(queueMessage);

Ha néhány célra a függvény előtt blob kötés olyan objektumra kell, a függvény törzséhez tartozó, a attribútumot használhatja [ahogy korábban a várólista attribútum](#ibinder).

### <a id="blobattributetypes"></a>A Blob attribútumot is használhatja típusok
A `Blob` attribútum a következő típusú használható:

* `Stream`(olvasására vagy írására, a FileAccess konstruktor paraméterrel megadott)
* `TextReader`
* `TextWriter`
* `string`(olvasás)
* `out string`(írási; hoz létre egy blobot, csak akkor, ha a karakterlánc-paraméter null értékű akkor, ha a függvény)
* POCO (olvasás)
* kimenő POCO (írás; mindig létrehoz egy blobot, mint null objektumot hoz létre, ha POCO paraméter értéke null, ha a függvény)
* `CloudBlobStream`(írja)
* `ICloudBlob`(olvasás vagy írás)
* `CloudBlockBlob`(olvasás vagy írás)
* `CloudPageBlob`(olvasás vagy írás)

## <a id="poison"></a>Az elhalt üzenetek kezelésének módját
Neve üzeneteket, amelyek tartalmát a sikertelen függvény hatására *elhalt üzenetek*. Ha a függvény nem sikerül, az üzenetsorban lévő üzenetet nem törlődik, és végül van felvenni, ebben az esetben meg kell ismételni a ciklus, amely. Az SDK automatikusan megszakíthatja a ciklus korlátozott számú ismétlés után, vagy manuálisan is elvégezhető.

### <a name="automatic-poison-message-handling"></a>Automatikus elhalt üzenetek kezelésének
Az SDK telefonhívásokhoz függvény legfeljebb 5-ször feldolgozni egy üzenetsor-üzenetet. Ha ötödik ismételje meg a sikertelen, az üzenet poison sor került. [Az újrapróbálkozások maximális száma: konfigurálható](#config).

Az elhalt várólista nevű *{originalqueuename}*-elhalt. Írhat egy folyamat üzenetek működnek, mint az elhalt üzenetsorból naplózásukhoz, vagy értesítést küld, hogy manuális beavatkozást van szükség.

Az alábbi példában a `CopyBlob` függvény sikertelenek lesznek, amikor egy üzenetsor-üzenetet, amely nem létezik blob nevét tartalmazza. Ebben az esetben az üzenet átkerül a copyblobqueue várólistából a copyblobqueue-poison várólista. A `ProcessPoisonMessage` naplózza az elhalt üzenet.

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
            logger.WriteLine("Failed to copy blob, name=" + blobName);
        }

A következő ábrán az alábbi funkciók a konzol kimeneti az elhalt üzenet feldolgozásakor.

![Elhalt üzenetek kezelésének a konzol kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/poison.png)

### <a name="manual-poison-message-handling"></a>Manuális elhalt üzenetek kezelésének
Kaphat a szám, ahányszor egy üzenet felvételre feldolgozásra hozzáadása egy `int` nevű paraméter `dequeueCount` a függvénynek. Ezután ellenőrizze a funkciókódot dequeue száma, és hajtsa végre a saját elhalt üzenetek kezelésének száma meghaladja a küszöbértéket, amikor a következő példában látható módon.

        public static void CopyBlob(
            [QueueTrigger("copyblobqueue")] string blobName, int dequeueCount,
            [Blob("textblobs/{queueTrigger}", FileAccess.Read)] Stream blobInput,
            [Blob("textblobs/{queueTrigger}-new", FileAccess.Write)] Stream blobOutput,
            TextWriter logger)
        {
            if (dequeueCount > 3)
            {
                logger.WriteLine("Failed to copy blob, name=" + blobName);
            }
            else
            {
            blobInput.CopyTo(blobOutput, 4096);
            }
        }

## <a id="config"></a>Hogyan konfigurációs beállítások megadása
Használhatja a `JobHostConfiguration` típus a következő konfigurációs beállítások megadásához:

* Az SDK-kapcsolati karakterláncok beállítása a kódban.
* Konfigurálása `QueueTrigger` beállításokat, például a maximális created száma.
* Várólistanevek konfigurációs beolvasása sikertelen.

### <a id="setconnstr"></a>A kód SDK kapcsolati karakterláncok beállítása
Az SDK-kapcsolati karakterláncok beállítása a kód lehetővé teszi, hogy a saját kapcsolódási karakterlánc neve, a konfigurációs fájlok vagy a környezeti változók használatát a következő példában látható módon.

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
A várólista-üzenet feldolgozása a következő beállításokat konfigurálhatja:

* Az üzenetsor-üzeneteket, amelyek átveszik egyidejűleg hajthatnak végre párhuzamosan maximális száma (alapértelmezett érték 16).
* A maximális számú újrapróbálkozást poison várólistába várólista üzenet elküldése előtt (alapértelmezett érték 5).
* A maximális várakozási idő előtt lekérdezési újra, ha üres-e a várólista (alapértelmezett érték 1 perc).

A következő példa bemutatja, hogyan konfigurálhatja ezeket a beállításokat:

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
Néha szeretne hozzáadni, adja meg a várólista nevét, a blob neve vagy a tároló, vagy egy táblázatot a merevlemez-kód helyett a kód adjon neki nevet. Például előfordulhat, hogy a várólista nevét adja meg szeretné `QueueTrigger` a konfigurációs fájl vagy a környezeti változóban.

A történő ehhez a `NameResolver` az objektum a `JobHostConfiguration` típusa. Ön százalékjel (%) jelentkezik be a WebJobs SDK attribútum konstruktorparaméterek körülvett különleges helyőrzőket tartalmaznak, és a `NameResolver` kód határozza meg azokat a helyőrzők helyett használandó tényleges értékek.

Tegyük fel, hogy a tesztkörnyezetben logqueuetest és éles környezetben egy elnevezett logqueueprod nevű várólista használni kívánt. A kódolt várólista neve helyett adja meg a nevét, a bejegyzés szeretné a `appSettings` gyűjteményt, amely rendelkezik a tényleges várólista neve. Ha a `appSettings` kulcs logqueue, a függvény a következő példa nézhet.

        public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
        {
            Console.WriteLine(logMessage);
        }

A `NameResolver` osztály majd lehetett beolvasni a várólista nevét a `appSettings` a következő példában látható módon:

        public class QueueNameResolver : INameResolver
        {
            public string Resolve(string name)
            {
                return ConfigurationManager.AppSettings[name].ToString();
            }
        }

Adja meg a `NameResolver` az osztályt a `JobHost` objektum az az alábbi példában látható módon.

        static void Main(string[] args)
        {
            JobHostConfiguration config = new JobHostConfiguration();
            config.NameResolver = new QueueNameResolver();
            JobHost host = new JobHost(config);
            host.RunAndBlock();
        }

**Megjegyzés:** várólista, a táblának és a blob nevének feloldása minden alkalommal, amikor egy függvény hívása esetén, de csak akkor, ha az alkalmazás indítása a rendszer feloldja a blob tároló neveit. A blob-tároló neve nem módosítható, a feladat futása közben.

## <a id="manual"></a>Hogyan kell manuálisan kezdeményezi egy függvény
Egy függvény manuálisan kezdeményezi, használja a `Call` vagy `CallAsync` metódust a `JobHost` objektum és a `NoAutomaticTrigger` attribútuma a függvény a következő példában látható módon.

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

## <a id="logs"></a>Naplók írásával
Az irányítópult az naplók két helyen jeleníti meg: a lap a webjobs-feladat, és egy adott webjobs-feladat meghívása tartozó lapon.

![Naplózza a webjobs-feladat lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

Konzol módszerekkel, amely meghívja a függvényben vagy a a kimenetét a `Main()` módszer akkor jelenik meg, az irányítópult-oldalon a webjobs-feladat, és nem egy adott metódus meghívása tartozó lapon. A TextWriter objektumból, hogy a paraméter a a metódus aláírásában kimeneti metódushívási irányítópult-oldalon jelenik meg.

Konzol kimeneti egy adott metódushívás nem csatolható, mivel a konzol egyszálas, miközben sok munkakörök esetleg fut egyszerre. Ezért az SDK-t biztosít minden függvény meghívása a saját egyedi napló író objektummal.

Írni [alkalmazás nyomkövetési naplóit](web-sites-dotnet-troubleshoot-visual-studio.md#logsoverview), használjon `Console.Out` (hoz létre a naplók adatai jelölésű) és `Console.Error` (hoz létre naplókat hiba jelöléssel). A másik lehetőség az használandó [nyomkövetési vagy TraceSource](http://blogs.msdn.com/b/mcsuksoldev/archive/2014/09/04/adding-trace-to-azure-web-sites-and-web-jobs.aspx), amely biztosítja, hogy részletes, figyelmeztetés, és a kritikus szintek mellett adatai és a hiba. Alkalmazás nyomkövetési naplók jelennek meg a webes alkalmazások naplófájljainak, Azure-táblákban, vagy Azure-blobok attól függően, hogy hogyan konfigurálja az Azure-webalkalmazásban. Mivel minden a konzol kimeneti értéke igaz, a legutóbbi 100 alkalmazásnaplók is megjelennek az irányítópult-oldalon a webjobs-feladat, nem egy függvény meghívása tartozó lapon.

Konzol eredmény jelenik meg, csak akkor, ha a program fut az Azure webjobs-feladat, nem, ha a program helyben fut az irányítópulton vagy más környezetben.

Tiltsa le a magas teljesítmény forgatókönyvek irányítópult naplózást. Alapértelmezés szerint az SDK-t menti el a naplókat tárhelyre, és ez a tevékenység ronthatja a teljesítményt, hány üzenet feldolgozásakor. Tiltsa le a naplózást, állítsa be az irányítópult a karakterlánc null értékű a következő példában látható módon.

        JobHostConfiguration config = new JobHostConfiguration();       
        config.DashboardConnectionString = "";        
        JobHost host = new JobHost(config);
        host.RunAndBlock();

A következő példa bemutatja a naplók írni többféle módon:

        public static void WriteLog(
            [QueueTrigger("logqueue")] string logMessage,
            TextWriter logger)
        {
            Console.WriteLine("Console.Write - " + logMessage);
            Console.Out.WriteLine("Console.Out - " + logMessage);
            Console.Error.WriteLine("Console.Error - " + logMessage);
            logger.WriteLine("TextWriter - " + logMessage);
        }

A WebJobs SDK irányítópulton kimenetét a `TextWriter` objektum mutat be, amikor egy adott oldalt lépjen be meghívása működik, és kattintson a **váltása kimeneti**:

![Függvény meghívása hivatkozásra](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardinvocations.png)

![Naplók függvény meghívása lap](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardlogs.png)

A WebJobs SDK-irányítópulton a legutóbbi 100 sor konzol kimeneti megjelenítése fel a webjobs-feladat (nem a függvény hívása) lépjen a lapra, és kattintson **váltása kimeneti**.

![Kattintson a Váltás kimeneti](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/dashboardapplogs.png)

Egy folyamatos webjobs-feladat, az alkalmazás naplóiban jelennek meg az/data/feladatok/folyamatos/*{webjobname}*/job_log.txt a webes alkalmazás fájlrendszerben.

        [09/26/2014 21:01:13 > 491e54: INFO] Console.Write - Hello world!
        [09/26/2014 21:01:13 > 491e54: ERR ] Console.Error - Hello world!
        [09/26/2014 21:01:13 > 491e54: INFO] Console.Out - Hello world!

Egy Azure blob-ehhez hasonló alkalmazások naplók megjelenését: 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738373502,0,17404,17,Console.Write - Hello world!, 2014-09-26T21:01:13,Error,contosoadsnew,491e54,635473620738373502,0,17404,19,Console.Error - Hello world!, 2014-09-26T21:01:13,Information,contosoadsnew,491e54,635473620738529920,0,17404,17,Console.Out - Hello world!,

Az Azure tábla és a `Console.Out` és `Console.Error` naplók néznek ki:

![A tábla adatai napló](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableinfo.png)

![Hibanapló tábla](./media/websites-dotnet-webjobs-sdk-storage-queues-how-to/tableerror.png)

Ha azt szeretné, hogy csatlakoztassák a saját naplózó, lásd: [ebben a példában](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Program.cs).

## <a id="errors"></a>Hibák kezelésének és időtúllépések konfigurálása
A WebJobs SDK is magában foglalja a [időtúllépés](http://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs) , amely segítségével okozhat a működnek, ha nem fejezi be a megadott időn belül. Ha szeretne egy riasztás, ha túl sok hiba fordulhat elő, a megadott időn belül, és a `ErrorTrigger` attribútum. Íme egy [ErrorTrigger példa](https://github.com/Azure/azure-webjobs-sdk-extensions/wiki/Error-Monitoring).

```
public static void ErrorMonitor(
[ErrorTrigger("00:01:00", 1)] TraceFilter filter, TextWriter log,
[SendGrid(
    To = "admin@emailaddress.com",
    Subject = "Error!")]
 SendGridMessage message)
{
    // log last 5 detailed errors to the Dashboard
   log.WriteLine(filter.GetDetailedMessage(5));
   message.Text = filter.GetDetailedMessage(1);
}
```

Dinamikusan is letilthatja, és engedélyezi a vezérlőhöz funkcióit, hogy akkor is elindítható, a konfiguráció kapcsoló, amelyet egy Alkalmazásbeállítás vagy a környezeti változó neve. Mintakód, tekintse meg a `Disable` attribútumnak [a WebJobs SDK-minták tárház](https://github.com/Azure/azure-webjobs-sdk-samples/blob/master/BasicSamples/MiscOperations/Functions.cs).

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezeli az Azure-üzenetsorok használata gyakori forgatókönyvei. Azure webjobs-feladatok és a WebJobs SDK használatával kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).

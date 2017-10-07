---
title: a WebJobs SDK hello Azure blob storage aaaHow toouse
description: "Ismerje meg, hogyan toouse Azure blob-tároló hello WebJobs SDK a. Indul el egy folyamatot, ha egy új blob a tárolóban lévő jelenik meg, és \"poison blobok\" kezelni."
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: erikre
editor: 
ms.assetid: bf32f919-f7bc-4aaa-916e-461c02f2e26c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/01/2016
ms.author: glenga
ms.openlocfilehash: b34ea8cffee7c0475641886150dee521130a3132
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-blob-storage-with-hello-webjobs-sdk"></a>Hogyan toouse Azure blob-tároló hello WebJobs SDK a
## <a name="overview"></a>Áttekintés
Ez az útmutató C# kódot, hogy hogyan minták tootrigger egy folyamatot, ha az Azure blob létrehozásakor vagy frissítésekor. hello kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.

Hogyan toocreate blobok megjelenítése mintakódok, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

hello az útmutató feltételezi, hogy tudja [hogyan toocreate egy webjobs-feladat projektet, a Visual Studio kapcsolati karakterláncok adott pont tooyour tárfiók](websites-dotnet-webjobs-sdk-get-started.md) vagy túl[több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Hogyan tootrigger egy függvény blob létrehozásakor vagy frissítésekor
Ez a szakasz bemutatja, hogyan toouse hello `BlobTrigger` attribútum. 

> [!NOTE]
> hello WebJobs SDK vizsgálatok napló fájlok toowatch új vagy módosított blobot. Ez a folyamat nincs valós idejű; egy olyan függvényt előfordulhat, hogy nem beolvasása indulnak el, amíg több percig vagy már hello blob létrehozása után. Emellett [tárolási naplófájlokat hoz létre a "legjobb erőfeszítéseket"](https://msdn.microsoft.com/library/azure/hh343262.aspx) időközönként; nincs garancia, hogy az összes esemény rögzíteni kell. Bizonyos körülmények között előfordulhat, hogy lehet kihagyni a naplókat. Ha hello gyorsabb és megbízhatóbb korlátozásai blob eseményindítók nem elfogadható az alkalmazáshoz, hello ajánlott módszer toocreate egy üzenetsor hello blob létrehozásakor, és használja a hello [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum helyett Hello `BlobTrigger` hello blob hello függvény attribútum.
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a>A blob kiterjesztésű egyetlen helyőrzője
hello alábbi kódminta átmásolja a terjesztendő megjelenő szöveg blobok hello *bemeneti* tároló toohello *kimeneti* tároló:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

hello attribútum konstruktora a hello tároló nevének és hello blob nevét megadó karakterlánc-paramétert fogad. Ebben a példában, ha egy blob neve *Blob1.txt* jön létre az hello *bemeneti* tároló, hello funkció hoz létre egy blobot nevű *Blob1.txt* a hello *kimeneti*  tároló. 

Megadhatja egy mintát hello blob név helyőrzője, ahogy az alábbi példakód hello:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Ez a kód csak blobok kezdve "eredeti-" nevű másolja. Például *eredeti-Blob1.txt* a hello *bemeneti* tároló túl másolódik*másolási-Blob1.txt* a hello *kimeneti* tároló.

Ha egy minta toospecify blob neveivel, amelyeknek kapcsos zárójelek hello neve, dupla hello kapcsos zárójelek. Például, ha azt szeretné, hogy toofind blobot, amely hello *képek* ilyen nevű tárolóban:

        {20140101}-soundfile.mp3

Ezt a mintát használja:

        images/{{20140101}}-{name}

Hello példában hello *neve* helyőrző értéke lenne *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Külön blob-névnek és kiterjesztésnek helyőrzők
hello következő minta kódmódosításokat hello fájlkiterjesztés, a blobok, amelyek szerepelnek a hello átmásolja *bemeneti* tároló toohello *kimeneti* tároló. hello kód naplózza hello hello kiterjesztését *bemeneti* blob-, és beállítja a hello hello kiterjesztését *kimeneti* blob túl*.txt*.

        public static void CopyBlobToTxtFile([BlobTrigger("input/{name}.{ext}")] TextReader input,
            [Blob("output/{name}.txt")] out string output,
            string name,
            string ext,
            TextWriter logger)
        {
            logger.WriteLine("Blob name:" + name);
            logger.WriteLine("Blob extension:" + ext);
            output = input.ReadToEnd();
        }

## <a id="types"></a>Hogy köthető tooblobs típusok
Használhatja a hello `BlobTrigger` hello típusa a következő attribútumot:

* `string`
* `TextReader`
* `Stream`
* `ICloudBlob`
* `CloudBlockBlob`
* `CloudPageBlob`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `IEnumerable<CloudBlockBlob>`
* `IEnumerable<CloudPageBlob>`
* Egyéb típusú általi [ICloudBlobStreamBinder](#icbsb) 

Ha azt szeretné, közvetlenül az Azure storage-fiók hello toowork, azt is megteheti egy `CloudStorageAccount` paraméter toohello metódus-aláírás.

Tekintse meg a hello [blob-kötés kód hello azure-webjobs-sdk-tárházban github.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Kötési toostring beolvasásakor szöveges blob tartalom
Ha a szöveges BLOB várható, `BlobTrigger` lehet alkalmazott tooa `string` paraméter. hello alábbi kódminta köti a szöveges blob tooa `string` nevű paraméter `logMessage`. hello függvény adott hello blob toohello WebJobs SDK irányítópult paraméter toowrite hello tartalmát használja. 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Első szerializált blob tartalma ICloudBlobStreamBinder használatával
hello alábbi kódminta használ, amely megvalósítja az osztály `ICloudBlobStreamBinder` tooenable hello `BlobTrigger` attribútum egy blob toohello toobind `WebImage` típusa.

        public static void WaterMark(
            [BlobTrigger("images3/{name}")] WebImage input,
            [Blob("images3-watermarked/{name}")] out WebImage output)
        {
            output = input.AddTextWatermark("WebJobs SDK", 
                horizontalAlign: "Center", verticalAlign: "Middle",
                fontSize: 48, opacity: 50);
        }
        public static void Resize(
            [BlobTrigger("images3-watermarked/{name}")] WebImage input,
            [Blob("images3-resized/{name}")] out WebImage output)
        {
            var width = 180;
            var height = Convert.ToInt32(input.Height * 180 / input.Width);
            output = input.Resize(width, height);
        }

Hello `WebImage` kötés kód megtalálható egy `WebImageBinder` abból származó osztály `ICloudBlobStreamBinder`.

        public class WebImageBinder : ICloudBlobStreamBinder<WebImage>
        {
            public Task<WebImage> ReadFromStreamAsync(Stream input, 
                System.Threading.CancellationToken cancellationToken)
            {
                return Task.FromResult<WebImage>(new WebImage(input));
            }
            public Task WriteToStreamAsync(WebImage value, Stream output,
                System.Threading.CancellationToken cancellationToken)
            {
                var bytes = value.GetBytes();
                return output.WriteAsync(bytes, 0, bytes.Length, cancellationToken);
            }
        }

## <a name="getting-hello-blob-path-for-hello-triggering-blob"></a>Hello blob elérési út hello időt. a blob beolvasása
tooget hello tároló neve és a blob neve, amely kiváltása hello függvény hello BLOB tartalmaznak egy `blobTrigger` karakterlánc-paramétert a hello függvény aláírásához.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Hogyan toohandle poison blobok
Ha egy `BlobTrigger` függvény sikertelen, hello SDK meghívja az ebben az esetben, ha hello hibát egy átmeneti hiba okozta. Hello hiba okozta hello blob hello tartalmát, hello függvény sikertelen, minden alkalommal, amikor megkísérli tooprocess hello blob. Alapértelmezés szerint hello SDK hívásokat too5 alkalommal be a következő függvényt egy adott blob. Hello ötödik próbálja meghibásodásakor hello SDK ad hozzá egy tooa üzenetsor nevű *webjobs-blobtrigger-poison*.

Az újrapróbálkozások maximális számát hello lehet konfigurálni. hello azonos [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob kezelésére és a várólista elhalt üzenetek kezelésének beállítást kell használni. 

az elhalt blobok várólista üdvözlőüzenetére egy JSON-objektum, amely tartalmazza a következő tulajdonságai hello:

* FunctionId (hello formátumban *{webjobs-feladat neve}*. Működik. *{Függvény neve}*, például: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" vagy "PageBlob")
* ContainerName
* Blobnév
* ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")

Hello alábbi mintakód, hello `CopyBlob` függvénynek kód kiváltó toofail minden alkalommal, amikor azt nevezzük. Miután hello SDK meghívja a hello az újrapróbálkozások maximális számát, hello poison blob várólista jön létre, egy üzenet, és üzenetet dolgoz fel hello `LogPoisonBlob` függvény. 

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("textblobs/output-{name}")] out string output)
        {
            throw new Exception("Exception for testing poison blob handling");
            output = input.ReadToEnd();
        }

        public static void LogPoisonBlob(
        [QueueTrigger("webjobs-blobtrigger-poison")] PoisonBlobMessage message,
            TextWriter logger)
        {
            logger.WriteLine("FunctionId: {0}", message.FunctionId);
            logger.WriteLine("BlobType: {0}", message.BlobType);
            logger.WriteLine("ContainerName: {0}", message.ContainerName);
            logger.WriteLine("BlobName: {0}", message.BlobName);
            logger.WriteLine("ETag: {0}", message.ETag);
        }

hello SDK automatikusan deserializes üdvözlőüzenetére JSON. Íme hello `PoisonBlobMessage` osztály: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>A BLOB lekérdezési algoritmus
hello WebJobs SDK megvizsgálja az összes tároló által megadott `BlobTrigger` alkalmazás indításkor attribútumok. Nagy tárfiókokban Ez a vizsgálat hosszabb ideig is tarthat, így előfordulhat, hogy egy kis ideig, mielőtt új blobok találhatók és `BlobTrigger` funkciók lesznek végrehajtva.

naplózza a SDK rendszeres időközönként olvassa be az hello blob-tároló hello toodetect új vagy módosított blobokkal alkalmazás indítása után. hello blob naplók pufferelve van-e, és csak fizikailag írása 10 percenként vagy Igen, vagyis jelentős késleltetés után blob létrehozásakor vagy frissítésekor előtt hello megfelelő `BlobTrigger` függvény végrehajtása. 

A blobok hello segítségével létrehozott kivétel `Blob` attribútum. Amikor hello WebJobs SDK létrehoz egy új blob, átadja hello új blob azonnal tooany megfelelő `BlobTrigger` funkciók. Ezért ha fenntartása blob bemenetekhez és kimenetekhez, hello SDK segítségével dolgozza fel őket hatékony. Ha azt szeretné, hogy fut a blob feldolgozási funkciók a létrehozott vagy más módon frissítve blobok kis késés, azt javasoljuk, de `QueueTrigger` helyett `BlobTrigger`.

### <a id="receipts"></a>A BLOB visszaigazolások
hello WebJobs SDK beállításnál ellenőrizze, hogy nincs `BlobTrigger` függvény menüelemnek egynél többször a hello azonos új vagy frissített blob. Ennek érdekében karbantartása *blob-visszaigazolások* a rendelés toodetermine, ha egy adott blobverzió feldolgozása megtörtént.

BLOB visszaigazolások nevű tárolóban vannak tárolva *azure-webjobs-állomások* hello AzureWebJobsStorage kapcsolati karakterlánc által meghatározott hello az Azure storage-fiókban. Egy blob fogadását a következő információk hello rendelkezik:

* hello hello BLOB hívott függvény ("*{webjobs-feladat neve}*. Működik. *{Függvény neve}*", például:"WebJob1.Functions.CopyBlob")
* hello tároló neve
* hello blob típushoz ("BlockBlob" vagy "PageBlob")
* hello blob neve
* hello ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")

Ha azt szeretné, hogy tooforce újrafeldolgozása blob, manuálisan törölheti, hogy a blob hello blob visszaigazolás hello *azure-webjobs-állomások* tároló.

## <a id="queues"></a>Hello várólisták cikkben említett kapcsolódó témakörök
Hogyan toohandle blob feldolgozási elindul egy üzenetsor-üzenetet, vagy a WebJobs SDK forgatókönyvek nem adott tooblob feldolgozása, lásd: [hogyan toouse Azure várólista tároló hello WebJobs SDK a](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Kapcsolódó témakörök hivatkozásra, ha a cikkben ismertetett hello alábbiakat foglalja magába:

* Aszinkron funkciók
* Több példánya
* Biztonságos leállításának
* Egy függvény törzséhez hello a WebJobs SDK attribútumok használata
* Hello SDK kapcsolati karakterláncok beállítása a kódban.
* Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* Konfigurálása `MaxDequeueCount` poison blob kezelésére.
* Manuálisan kezdeményezi egy függvény
* Naplók írása

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott mintakódok, hogy hogyan toohandle gyakori helyzetek Azure végzett munka blobok megjelenítése. További információ a hogyan toouse Azure webjobs-feladatok és a WebJobs SDK hello: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).


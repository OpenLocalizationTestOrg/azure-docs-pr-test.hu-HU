---
title: "How to use Azure blob storage with the WebJobs SDK (Az Azure Blob Storage használata a WebJobs SDK-val)"
description: "További tudnivalók az Azure blob storage használata a WebJobs SDK-val. Indul el egy folyamatot, ha egy új blob a tárolóban lévő jelenik meg, és \"poison blobok\" kezelni."
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
ms.openlocfilehash: e0a792ccdf8097d5cde254d6d4690a64838378ea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-blob-storage-with-the-webjobs-sdk"></a>How to use Azure blob storage with the WebJobs SDK (Az Azure Blob Storage használata a WebJobs SDK-val)
## <a name="overview"></a>Áttekintés
Ez az útmutató C# mintakódok bemutatják, hogyan indul el egy folyamatot, ha az Azure blob létrehozásakor vagy frissítésekor. A kód minták használata [WebJobs SDK](websites-dotnet-webjobs-sdk.md) verzió 1.x.

Blobok létrehozását mutatják be mintakódok, lásd: [Azure a queue storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Az útmutató azt feltételezi, hogy tudja [hogyan webjobs-feladat-projekt létrehozása a Visual Studio kapcsolati karakterláncok a tárfiókhoz adott pontra](websites-dotnet-webjobs-sdk-get-started.md) vagy [több tárfiókot](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/MultipleStorageAccountsEndToEndTests.cs).

## <a id="trigger"></a>Hogyan indul el egy function, ha egy blob létrehozásakor vagy frissítésekor
Ez a szakasz bemutatja, hogyan használja a `BlobTrigger` attribútum. 

> [!NOTE]
> A WebJobs SDK megvizsgálja az új vagy módosított blobok figyelendő naplófájljait. Ez a folyamat nincs valós idejű; egy függvény előfordulhat, hogy nem get indulnak el, néhány percig, vagy már a blob létrehozása után. Emellett [tárolási naplófájlokat hoz létre a "legjobb erőfeszítéseket"](https://msdn.microsoft.com/library/azure/hh343262.aspx) időközönként; nincs garancia, hogy az összes esemény rögzíteni kell. Bizonyos körülmények között előfordulhat, hogy lehet kihagyni a naplókat. Ha gyorsabb és megbízhatóbb vonatkozó korlátozások blob eseményindítók nem elfogadható az alkalmazáshoz, az ajánlott módszer a blob létrehozásakor, és használja egy üzenetsor létrehozásához-e a [QueueTrigger](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#trigger) attribútum helyett a `BlobTrigger` a függvény, amely feldolgozza a blob attribútum.
> 
> 

### <a name="single-placeholder-for-blob-name-with-extension"></a>A blob kiterjesztésű egyetlen helyőrzője
A következő példakód másolja át a megjelenő szöveg blobok a *bemeneti* tárolót, hogy a *kimeneti* tároló:

        public static void CopyBlob([BlobTrigger("input/{name}")] TextReader input,
            [Blob("output/{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Az attribútum konstruktora a, amelyben a tároló neve és a blob nevének helyőrzője karakterlánc-paramétert fogad. Ebben a példában, ha egy blob neve *Blob1.txt* jön létre a *bemeneti* tároló, a függvény létrehoz egy blob nevű *Blob1.txt* a a *kimeneti* tároló. 

Megadhatja egy mintát a blob nevének helyőrzője, a következő kód mintában látható módon:

        public static void CopyBlob([BlobTrigger("input/original-{name}")] TextReader input,
            [Blob("output/copy-{name}")] out string output)
        {
            output = input.ReadToEnd();
        }

Ez a kód csak blobok kezdve "eredeti-" nevű másolja. Például *eredeti-Blob1.txt* a a *bemeneti* tároló másolódik *másolási-Blob1.txt* a a *kimeneti* tároló.

Ha meg kell adnia egy mintát a blob neve kapcsos zárójelek a neve, dupla a kapcsos zárójelek. Például, ha a keresett blobot, amely a *képek* ilyen nevű tárolóban:

        {20140101}-soundfile.mp3

Ezt a mintát használja:

        images/{{20140101}}-{name}

A példában a *neve* helyőrző értéke lenne *soundfile.mp3*. 

### <a name="separate-blob-name-and-extension-placeholders"></a>Külön blob-névnek és kiterjesztésnek helyőrzők
A következő példakód módosítja a fájlnévkiterjesztés, a megjelenő blobot másol a *bemeneti* tárolót, hogy a *kimeneti* tároló. A kód kiterjesztését naplózza a *bemeneti* blob-, és beállítja a kiterjesztését a *kimeneti* a blob *.txt*.

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

## <a id="types"></a>Blobok köthető típusai
Használhatja a `BlobTrigger` attribútum a következő esetében:

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

Ha közvetlenül az Azure storage-fiók dolgozni szeretne, azt is megteheti egy `CloudStorageAccount` a metódus-aláírás paramétert.

Tekintse meg a [blob-kötés kód az azure-webjobs-sdk-tárházban github.com](https://github.com/Azure/azure-webjobs-sdk/blob/master/test/Microsoft.Azure.WebJobs.Host.EndToEndTests/BlobBindingEndToEndTests.cs).

## <a id="string"></a>Szöveges blob tartalma karakterláncra kötés beolvasása
Ha a szöveges BLOB várható, `BlobTrigger` alkalmazható egy `string` paraméter. A következő példakód köti a szöveges blob egy `string` nevű paraméter `logMessage`. A funkció, hogy a paraméter a blob tartalmát írni a WebJobs SDK irányítópult. 

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name, 
            TextWriter logger)
        {
             logger.WriteLine("Blob name: {0}", name);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }

## <a id="icbsb"></a>Első szerializált blob tartalma ICloudBlobStreamBinder használatával
A következő példakód használ, amely megvalósítja az osztály `ICloudBlobStreamBinder` engedélyezése a `BlobTrigger` attribútum egy blob kötni a `WebImage` típusa.

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

A `WebImage` kötés kód megtalálható egy `WebImageBinder` abból származó osztály `ICloudBlobStreamBinder`.

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

## <a name="getting-the-blob-path-for-the-triggering-blob"></a>A blob elérési út az eseményindító blob beolvasása
A tároló neve és a blob neve, amely a függvény kiváltása BLOB, közé tartozik egy `blobTrigger` karakterlánc-paramétert függvény aláírásában.

        public static void WriteLog([BlobTrigger("input/{name}")] string logMessage,
            string name,
            string blobTrigger,
            TextWriter logger)
        {
             logger.WriteLine("Full blob path: {0}", blobTrigger);
             logger.WriteLine("Content:");
             logger.WriteLine(logMessage);
        }


## <a id="poison"></a>Az elhalt blobok kezelése
Ha egy `BlobTrigger` függvény sikertelen, az SDK meghívja az ebben az esetben, ha a hibát egy átmeneti hiba okozta. Ha a hiba oka a blob tartalmát, a függvény minden alkalommal, amikor megpróbálja feldolgozni a blob sikertelen lesz. Alapértelmezés szerint az SDK meghívja a függvény legfeljebb 5-ször egy adott BLOB. Az ötödik próbálja meghiúsul, ha az SDK-t ad hozzá egy üzenet nevű várólista *webjobs-blobtrigger-poison*.

Az újrapróbálkozások maximális számát lehet konfigurálni. Azonos [MaxDequeueCount](websites-dotnet-webjobs-sdk-storage-queues-how-to.md#configqueue) poison blob kezelésére és a várólista elhalt üzenetek kezelésének beállítást kell használni. 

Az elhalt blobok várólista üzenet egy JSON-objektum, amely tartalmazza a következő tulajdonságokkal:

* FunctionId (formátumú *{webjobs-feladat neve}*. Működik. *{Függvény neve}*, például: WebJob1.Functions.CopyBlob)
* BlobType ("BlockBlob" vagy "PageBlob")
* ContainerName
* Blobnév
* ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")

Az alábbi példakódban az `CopyBlob` függvénynek kódot, amely azt eredményezi, hogy minden alkalommal, amikor a hívott sikertelen. Miután az SDK meghívja az újrapróbálkozások maximális számát, az elhalt blob várólista jön létre, egy üzenet, és üzenetet dolgoz fel a `LogPoisonBlob` függvény. 

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

Az SDK automatikusan deserializes a JSON-üzenet. Itt a `PoisonBlobMessage` osztály: 

        public class PoisonBlobMessage
        {
            public string FunctionId { get; set; }
            public string BlobType { get; set; }
            public string ContainerName { get; set; }
            public string BlobName { get; set; }
            public string ETag { get; set; }
        }

### <a id="polling"></a>A BLOB lekérdezési algoritmus
A WebJobs SDK megvizsgálja az összes tároló által megadott `BlobTrigger` alkalmazás indításkor attribútumok. Nagy tárfiókokban Ez a vizsgálat hosszabb ideig is tarthat, így előfordulhat, hogy egy kis ideig, mielőtt új blobok találhatók és `BlobTrigger` funkciók lesznek végrehajtva.

Új vagy módosított blobok észleléséhez az alkalmazás indítása után, az SDK rendszeres időközönként olvassa be a blob storage-naplók. A blob naplók pufferelve van-e, és csak fizikailag írása 10 percenként vagy Igen, ezért előfordulhat, hogy jelentős késleltetés után blob létrehozásakor vagy frissítésekor a megfelelő előtt `BlobTrigger` függvény végrehajtása. 

A blobok, amelyek használatával hoz létre kivétel történik a `Blob` attribútum. Ha a WebJobs SDK hoz létre egy új blob, adja át az új blob azonnal bármely megfelelő `BlobTrigger` funkciók. Ezért ha fenntartása blob bemenetekhez és kimenetekhez, az SDK-t tud feldolgozni azokat hatékony. Ha azt szeretné, hogy fut a blob feldolgozási funkciók a létrehozott vagy más módon frissítve blobok kis késés, azt javasoljuk, de `QueueTrigger` helyett `BlobTrigger`.

### <a id="receipts"></a>A BLOB visszaigazolások
A WebJobs SDK beállításnál ellenőrizze, hogy nincs `BlobTrigger` függvény egynél többször a azonos új vagy frissített BLOB meghívása megtörténik. Ennek érdekében karbantartása *blob-visszaigazolások* annak meghatározására, ha egy adott blobverzió feldolgozása megtörtént.

A BLOB visszaigazolások nevű tárolóban vannak tárolva *azure-webjobs-állomások* AzureWebJobsStorage kapcsolati karakterlánc által meghatározott az Azure storage-fiókban. Egy blob fogadását rendelkezik a következő információkat:

* A BLOB hívott függvény ("*{webjobs-feladat neve}*. Működik. *{Függvény neve}*", például:"WebJob1.Functions.CopyBlob")
* A tároló neve
* A blob típusú ("BlockBlob" vagy "PageBlob")
* A blob neve
* Az ETag (például egy blob verziójának azonosítója: "0x8D1DC6E70A277EF")

Ha egy blobot újrafeldolgozása kényszeríteni kívánja, a blob fogadását, hogy a BLOB manuálisan törölheti a *azure-webjobs-állomások* tároló.

## <a id="queues"></a>A várólisták a cikkben említett kapcsolódó témakörök
Hogyan kezelje a blob feldolgozási sor üzenetet által indított információt, vagy a WebJobs SDK forgatókönyvek nem kizárólag a blobra feldolgozása, lásd [Azure a queue storage használata a WebJobs SDK](websites-dotnet-webjobs-sdk-storage-queues-how-to.md). 

Kapcsolódó témakörök hivatkozásra, ha a cikkben ismertetett közé tartoznak a következők:

* Aszinkron funkciók
* Több példánya
* Biztonságos leállításának
* Egy függvény törzséhez a WebJobs SDK attribútumok használata
* Az SDK-kapcsolati karakterláncok beállítása a kódban.
* Értékek beállítása a WebJobs SDK konstruktorparaméterek kódot
* Konfigurálása `MaxDequeueCount` poison blob kezelésére.
* Manuálisan kezdeményezi egy függvény
* Naplók írása

## <a id="nextsteps"></a> Következő lépések
Ez az útmutató nyújtott mintakódok, amelyek bemutatják, hogyan kezeli az Azure-blobokkal dolgozik gyakori forgatókönyvei. Azure webjobs-feladatok és a WebJobs SDK használatával kapcsolatos további információkért lásd: [Azure webjobs-feladatok ajánlott erőforrások](http://go.microsoft.com/fwlink/?linkid=390226).


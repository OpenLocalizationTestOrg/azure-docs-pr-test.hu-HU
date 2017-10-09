---
title: "a blob storage és a Visual Studio (felhőszolgáltatások) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának Azure Blob storage egy Visual Studio felhőszolgáltatás-projektet a Visual Studio használatával tooa tárfiók kapcsolódás szolgáltatások csatlakoztatása után"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 62fb7fcff0a90008859ebe23755f13ef0555e380
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Ismerkedés az Azure Blob Storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooget után indítják el az Azure Blob Storage szolgáltatással létrehozott vagy egy Azure Storage-fiók hivatkozott hello Visual Studio használatával **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel a Visual Studio felhő szolgáltatási projektet. Mutat be, hogyan tooaccess blobtárolók, és hogyan tooperform gyakori feladatokat, például feltöltése, listázása és blobok letöltése és létrehozása. hello minták C nyelven íródtak\# és hello [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

Az Azure Blob Storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatot, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Egy blob bármilyen méretű lehet. Blobok lehetnek többek között a lemezképek, a hang- és fájlok, a nyers adatok és a fájlok.

Ugyanúgy, mint a él fájlok, mappák, a tárolási BLOB élő tárolókban lévő. Miután létrehozta a tárolási, hello tárolási hozzon létre egy vagy több tárolóban. Például "Lapkivágások" nevű tárolási, az úgynevezett "képek" toostore képek hello storage hozhat létre, és más néven "hang" toostore hangfájlok. Hello tárolók létrehozása után feltöltheti egyes blob fájlok toothem.

* Programozott módon kezelésére a blobok további információkért lásd: [az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md).
* Azure Storage kapcsolatos általános információkért lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).
* Azure Cloud Services kapcsolatos általános információkért lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/).
* ASP.NET-alkalmazások programozásával kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).

## <a name="access-blob-containers-in-code"></a>Hozzáférés a blob tárolók kód
tooprogrammatically blobok a felhőszolgáltatás-projektekre nézve, ha azok már nem található a következő elemek tooadd hello kell.

1. Adja hozzá a következő kód névtér nyilatkozatok toohello felső részén a C# fájlban, amelyben tooprogrammatically access Azure Storage kívánja hello.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. Első egy **CloudBlobClient** tooreference a tárfiókban lévő meglévő tároló objektum.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. Első egy **CloudBlobContainer** tooreference egy adott blob tároló objektum.
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> Minden hello előző eljárásban a következő szakaszok hello hello kód elé hello kód használható.
> 
> 

## <a name="create-a-container-in-code"></a>A kód egy tároló létrehozása
> [!NOTE]
> Egyes hajtsa végre a hívások tooAzure tárolási ki az ASP.NET API-k aszinkron jellegűek. Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt. a következő példa hello hello kód azt feltételezi, hogy aszinkron programozási módszerek használatát.
> 
> 

egy tároló a tárfiókban lévő toocreate, toodo szüksége van adjon hozzá egy túl**CreateIfNotExistsAsync** hasonlóan hello a következő kódot:

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


toomake hello fájlok hello tároló elérhető tooeveryone belül, beállíthat hello tároló toobe nyilvános hello a következő kód használatával.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


Hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat, de módosítja, vagy törölje azokat, csak ha hello megfelelő hozzáférési kulcsot.

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
Az Azure Storage támogatja a blokkblobokat és a lapblobokat. Hello legtöbb esetben a blokkblob típusú toouse ajánlott hello.

a fájl tooa blokkblob tooupload beolvasni a tároló hivatkozását, és használja úgy tooget le a blokkblob hivatkozását. Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **UploadFromStream** metódust. Ez a művelet hello blob hoz létre, ha korábban már nem létezik, vagy felülírja, ha már létezik. a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.

    // Retrieve a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását. Hello tároló segítségével **ListBlobs** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül. tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **IListBlobItem**, tooa kell alakítania azt **CloudBlockBlob**, **CloudPageBlob**, vagy  **CloudBlobDirectory** objektum. Ha hello típusa ismeretlen, használhatja a típus-ellenőrzés toodetermine mely toocast azt. hello következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello az egyes elemek URI **fényképek** tároló:

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

Ahogy az előző példakód hello, hello blob szolgáltatás rendelkezik hello fogalma könyvtárak belül tárolókat is beleértve. Ez az, hogy a blobokat több mappa hasonló struktúrában rendezhetők. Vegyük példaként a következő nevű tároló blokkblobokat hello **fényképek**:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

A hívás esetén **ListBlobs** hello tárolóban (hello korábbi példa szerint), a visszaadott hello gyűjtemény tartalmaz **CloudBlobDirectory** és **CloudBlockBlob** objektumok képviselő hello illetve hello felső szinten található blobokat. Hello ennek kimenete a következő:

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


Másik lehetőségként beállíthatja a hello **Listblobs** paraméterét hello **ListBlobs** módszert **igaz**. Minden blob ad vissza az eredmény egy **CloudBlockBlob**directory függetlenül. Itt hívás, amely hello túl**ListBlobs**:

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

és az alábbiakban hello eredmények:

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

További információkért lásd: [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).

## <a name="download-blobs"></a>Blobok letöltése
toodownload blobokat, először kérjen le egy blobhivatkozást, és majd meghívják a hello **DownloadToStream** metódust. hello alábbi példában hello **DownloadToStream** metódus tootransfer hello blob tartalma tooa stream objektumot, hogy Ön majd megőrizni a tooa helyi fájlt.

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents tooa file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

Is használhatja a hello **DownloadToStream** metódus toodownload hello tartalmát szöveges karakterláncként blob.

    // Get a reference tooa blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>Blobok törlése
toodelete blob, először kérjen le egy blobhivatkozást, és ezután hívja meg a **törlése** metódust.

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>Blobok listázása több oldalon aszinkron módon
Ha nagyszámú BLOB listázásakor, vagy azt szeretné, hogy egy listázási művelet eredmények toocontrol hello száma, blobok megjelenített eredményoldalra is listázhatja. Ez a példa bemutatja, hogyan tooreturn eredményezi lapok aszinkron módon történik, így végrehajtása ne tiltsa le a tooreturn eredmények számos várakozás során.

Ez a példa bemutatja egy egyszerű blob listázása, de hierarchikus listát is végrehajthatók hello beállítása **Listblobs** hello paramétere **ListBlobsSegmentedAsync** metódus túl **hamis**.

Hello minta metódushívások egy aszinkron metódus, mert azt kell lennie végrehajtásával kerüli meg a hello **aszinkron** kulcsszót, és azt kell visszaadnia egy **feladat** objektum. hello await kulcsszó hello megadott **ListBlobsSegmentedAsync** metódus hello minta metódus végrehajtásának felfüggesztése, hello listázási feladat befejezéséig.

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        // List blobs toohello console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        // Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
        // When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
        do
        {
            // This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
            // or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get hello continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


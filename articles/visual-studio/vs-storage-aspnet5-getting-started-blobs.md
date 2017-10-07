---
title: "a blob storage és a Visual Studio (az ASP.NET Core) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget el, a Visual Studio ASP.NET Core projektben Azure Blob storage használatával, a Visual Studio használatával storage-fiók létrehozása után kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8eedf331896b21658c7b30a68a4391d8d60cd729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a>Ismerkedés az Azure Blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET mag)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooget el az Azure Blob Storage tárolót a Visual Studióban létrehozott vagy egy Azure storage-fiók ASP.NET Core projektben hivatkozott hello Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen után.

Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatot, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához. Egy blob bármilyen méretű lehet. Blobok lehetnek többek között a lemezképek, a hang- és fájlok, a nyers adatok és a fájlok. Ez a cikk ismerteti, hogyan tooget használatába a blob storage hello Visual Studio használatával Azure-tárfiók létrehozása után **kapcsolódó szolgáltatások hozzáadása** az ASP.NET Core projekt párbeszédpanel.

Ugyanúgy, mint a él fájlok, mappák, a tárolási BLOB élő tárolókban lévő. Miután létrehozta a tárolási, hello tárolási hozzon létre egy vagy több tárolóban. Például "Lapkivágások" nevű tárolási, az úgynevezett "képek" toostore képek hello storage hozhat létre, és más néven "hang" toostore hangfájlok. Hello tárolók létrehozása után feltöltheti egyes blob fájlok toothem. Lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md) programozott módon kezelésére a blobok további tájékoztatást.

## <a name="access-blob-containers-in-code"></a>Hozzáférés a blob tárolók kód
tooprogrammatically blobok elérése az ASP.NET Core projektben, kell tooadd hello a következő elemek, ha azok már nem létezik.

1. Adja hozzá a következő kód névtér nyilatkozatok toohello felső részén a C# fájlban használni kívánt tooprogrammatically hozzáférés az Azure storage hello.
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. A következő kód tooget hello használja, a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    **Megjegyzés:** a következő részekben hello az összes fenti kódot hello kód előtt hello használja.
3. Használja a **CloudBlobClient** tooget objektum egy **CloudBlobContainer** hivatkozás tooan meglévő tárolóhoz a tárfiók.
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a>A kód egy tároló létrehozása
Is használhatja a hello **CloudBlobClient** toocreate a tárfiókban lévő tárolója. Toodo szüksége egy hívás tooadd túl**CreateIfNotExistsAsync** hasonlóan hello a következő kódot:

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference tooa container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


**Megjegyzés:** hello API-hívások tooAzure tárolási hajtsa végre az ASP.NET Core aszinkron jellegűek. Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt. az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.

toomake hello fájlok hello tároló elérhető tooeveryone belül, beállíthat hello tároló toobe nyilvános hello a következő kód használatával.

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
tooupload fájl blob egy tárolóba beolvasni a tároló hivatkozását, és egy blobhivatkozást tooget használják. Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **UploadFromStreamAsync** metódust. Ez a művelet hello blob hoz létre, ha már nem létezik, vagy felülírja, ha már létezik. a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.

    // Get a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with hello contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását. Majd hívása hello tároló **ListBlobsSegmentedAsync** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül. tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **IListBlobItem**, tooa kell alakítania azt **CloudBlockBlob**, **CloudPageBlob**, vagy  **CloudBlobDirectory** objektum. Ha nem tudja hello blob írja be, használhatja a típus-ellenőrzés toodetermine mely toocast azt. hello a következő kód bemutatja, hogyan tooretrieve és a kimeneti hello a tárolóban lévő egyes elemek URI Azonosítóját.

    BlobContinuationToken token = null;
    do
    {
        BlobResultSegment resultSegment = await container.ListBlobsSegmentedAsync(token);
        token = resultSegment.ContinuationToken;

        foreach (IListBlobItem item in resultSegment.Results)
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
    } while (token != null);

Nincsenek más módokon toolist hello tartalmát egy blob-tároló. Lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) további információt.

## <a name="download-a-blob"></a>Blob letöltése
toodownload blob, először a hivatkozás toohello blob kapnak, és hívhatja hello **DownloadToStreamAsync** metódust. hello alábbi példában hello **DownloadToStreamAsync** metódus tootransfer hello blob tartalma tooa stream objektumra, majd mentheti egy helyi fájl.

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save hello blob contents tooa file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

Más módon toosave blobokat fájlokba. Lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md#download-blobs) további információt.

## <a name="delete-a-blob"></a>Blob törlése
toodelete blob, először a hivatkozás toohello blob kapnak, és hívhatja hello **DeleteAsync** metódust.

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


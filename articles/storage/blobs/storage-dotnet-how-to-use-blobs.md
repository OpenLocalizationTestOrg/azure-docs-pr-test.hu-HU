---
title: "az Azure Blob storage (object storage) .NET használatának lépései aaaGet |} Microsoft Docs"
description: "Strukturálatlan adatok tárolása az Azure Blob storage (object storage) hello felhő."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: d18a8fc8-97cb-4d37-a408-a6f8107ea8b3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 03/27/2017
ms.author: marsma
ms.openlocfilehash: 3df0cf14b69d85cdc2f62cc3c8b901be102fa026
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-using-net"></a>Get started with Azure Blob Storage using .NET (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel)

[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

### <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Ez az oktatóanyag bemutatja, hogyan toowrite .NET helykódja olyan gyakori forgatókönyveket tartalmaz Azure Blob storage használatával. A tárgyalt forgatókönyvekben szerepel a blobok feltöltése, listázása, letöltése és törlése.

**Előfeltételek:**

* [Microsoft Visual Studio](https://www.visualstudio.com/)
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager a .NET-hez](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* Egy [Azure-tárfiók](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>További példák
További példák a Blob Storage használatára: [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel). Töltse le a mintaalkalmazást hello és futtatáshoz, vagy keresse meg a hello kódja a Githubon.

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Hozzáadás irányelvekkel
Adja hozzá a következő hello **használatával** irányelvek toohello felső részén hello `Program.cs` fájlt:

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-hello-connection-string"></a>Hello kapcsolati karakterlánc elemzése
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-blob-service-client"></a>Hello Blob szolgáltatásügyfél létrehozása
Hello **CloudBlobClient** osztály lehetővé teszi a Blob Storage tárolóban tárolt tooretrieve tárolók és blobok. Egyirányú toocreate hello szolgáltatás ügyfele a következő:

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
Most már készen áll a toowrite kódot, amely adatokat olvas és ír tooBlob adattárolás áll.

## <a name="create-a-container"></a>Tároló létrehozása
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

A példa bemutatja, hogyan toocreate egy tárolót, ha még nem létezik:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create hello container if it doesn't already exist.
container.CreateIfNotExists();
```

Alapértelmezés szerint hello új tároló privát, ami azt jelenti, hogy meg kell adnia a tárolási kulcs toodownload blobok elérése az ebben a tárolóban. Ha azt szeretné, hogy toomake hello fájlok hello tároló elérhető tooeveryone belül, beállíthatja hello tároló toobe nyilvános hello kód a következő használatával:

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

Az hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat. Azonban módosítsa vagy törölje azokat, csak ha hello megfelelő tárelérési kulccsal vagy egy közös hozzáférésű jogosultságkódot.

## <a name="upload-a-blob-into-a-container"></a>Blobok feltöltése a tárolóba
Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat.  A legtöbb esetben blokkblob típusú toouse ajánlott hello.

a fájl tooa blokkblob tooupload beolvasni a tároló hivatkozását, és használja úgy tooget le a blokkblob hivatkozását. Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **UploadFromStream** metódust. Ez a művelet hello blob hoz létre, ha korábban már nem létezik, vagy felülírja, ha már létezik.

a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

// Create or overwrite hello "myblob" blob with contents from a local file.
using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
{
    blockBlob.UploadFromStream(fileStream);
}
```

## <a name="list-hello-blobs-in-a-container"></a>Lista hello a tárolóban lévő blobok
toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását. Hello tároló segítségével **ListBlobs** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül. tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **IListBlobItem**, tooa kell alakítania azt **CloudBlockBlob**, **CloudPageBlob**, vagy  **CloudBlobDirectory** objektum. Ha hello típusa ismeretlen, használhatja a típus-ellenőrzés toodetermine mely toocast azt. hello következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello az egyes elemek URI _fényképek_ tároló:

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("photos");

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
```

Ha belefoglalja az elérési úttal kapcsolatos információkat a blobnevekbe, olyan virtuális könyvtárstruktúrát hozhat létre, amelyet egy hagyományos fájlrendszerhez hasonlóan rendszerezhet és bejárhat. hello könyvtárstruktúrát virtuális csak – hello csak forrásanyag is elérhető a Blob Storage tárolók és blobok. Azonban hello a storage ügyféloldali kódtára kínál a **CloudBlobDirectory** toorefer tooa virtuális könyvtár objektum és az ezzel a módszerrel blobokkal hello folyamat leegyszerűsítése érdekében.

Vegyük példaként a következő nevű tároló blokkblobokat hello *fényképek*:

```
photo1.jpg
2010/architecture/description.txt
2010/architecture/photo3.jpg
2010/architecture/photo4.jpg
2011/architecture/photo5.jpg
2011/architecture/photo6.jpg
2011/architecture/description.txt
2011/photo7.jpg
```

A hívás esetén **ListBlobs** a hello *fényképek* tároló (ahogy kódrészletet megelőző hello), hierarchikus listát ad vissza. Is tartalmaz **CloudBlobDirectory** és **CloudBlockBlob** objektumokat, üdvözlő könyvtárak és hello tárolóban lévő blobok képviselve. Ennek kimenete hello néz ki:

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

Másik lehetőségként beállíthatja a hello **Listblobs** hello paramétere **ListBlobs** módszert **igaz**. Ebben az esetben hello tároló összes blobjának adja vissza a rendszer egy **CloudBlockBlob** objektum. hívás túl hello**ListBlobs** tooreturn egy strukturálatlan lista néz ki:

```csharp
// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

és hello eredmények néznek ki:

```
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>Blobok letöltése
toodownload blobokat, először kérjen le egy blobhivatkozást, és majd meghívják a hello **DownloadToStream** metódust. hello alábbi példában hello **DownloadToStream** metódus tootransfer hello blob tartalma tooa stream objektumot, hogy Ön majd megőrizni a tooa helyi fájlt.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "photo1.jpg".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

// Save blob contents tooa file.
using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
{
    blockBlob.DownloadToStream(fileStream);
}
```

Is használhatja a hello **DownloadToStream** metódus toodownload hello tartalmát szöveges karakterláncként blob.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt"
CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

string text;
using (var memoryStream = new MemoryStream())
{
    blockBlob2.DownloadToStream(memoryStream);
    text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
}
```

## <a name="delete-blobs"></a>Blobok törlése
toodelete blob, először kérjen le egy blobhivatkozást, és ezután hívja meg a **törlése** metódust.

```csharp
// Retrieve storage account from connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello blob client.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve reference tooa previously created container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Retrieve reference tooa blob named "myblob.txt".
CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

// Delete hello blob.
blockBlob.Delete();
```

## <a name="list-blobs-in-pages-asynchronously"></a>Blobok listázása több oldalon aszinkron módon
Ha nagyszámú BLOB listázásakor, vagy azt szeretné, hogy egy listázási művelet eredmények toocontrol hello száma, blobok megjelenített eredményoldalra is listázhatja. Ez a példa bemutatja, hogyan tooreturn eredményezi lapok aszinkron módon történik, így végrehajtása ne tiltsa le a tooreturn eredmények számos várakozás során.

Ez a példa bemutatja egy egyszerű blob listázása, de hierarchikus listát is végrehajthatók hello beállítása _Listblobs_ hello paramétere **ListBlobsSegmentedAsync** metódus too_false_.

Hello minta metódushívások egy aszinkron metódus, mert azt kell lennie végrehajtásával kerüli meg a hello _aszinkron_ kulcsszót, és azt kell visszaadnia egy **feladat** objektum. hello await kulcsszó hello megadott **ListBlobsSegmentedAsync** metódus hello minta metódus végrehajtásának felfüggesztése, hello listázási feladat befejezéséig.

```csharp
async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
{
    //List blobs toohello console window, with paging.
    Console.WriteLine("List blobs in pages:");

    int i = 0;
    BlobContinuationToken continuationToken = null;
    BlobResultSegment resultSegment = null;

    //Call ListBlobsSegmentedAsync and enumerate hello result segment returned, while hello continuation token is non-null.
    //When hello continuation token is null, hello last page has been returned and execution can exit hello loop.
    do
    {
        //This overload allows control of hello page size. You can return all remaining results by passing null for hello maxResults parameter,
        //or by calling a different overload.
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
```

## <a name="writing-tooan-append-blob"></a>Írás tooan hozzáfűző blob
A hozzáfűző blob hozzáfűzési feladatokra, például naplózásra van optimalizálva. Blokkblob, például a hozzáfűző blob blokkok magában foglalja, de ha hozzáad egy új blokkot tooan hozzáfűző blob, mindig hozzáfűzött toohello hello blob végéhez. Hozzáfűző blob meglévő blokkja nem frissíthető és nem törölhető. a hozzáfűző BLOB azonosítók hello blokk nem érhetők el, mert azok egy blokkblobhoz tartoznak.

A hozzáfűző blob minden blokkja különböző méretű mentése tooa legfeljebb 4 MB lehet, és a hozzáfűző blob legfeljebb 50 000 blokkot tartalmazhat. hello maximális hozzáfűző blob mérete ezért valamivel több mint 195 GB (4 MB X 50 000 blokk).

hello az alábbi példa létrehoz egy új hozzáfűző blob, és hozzáfűzi egyes adatok tooit, egy egyszerű naplózási műveletet szimulálva.

```csharp
//Parse hello connection string for hello storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

//Create service client for credentialed access toohello Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference tooa container.
CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

//Create hello container if it does not already exist.
container.CreateIfNotExists();

//Get a reference tooan append blob.
CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

//Create hello append blob. Note that if hello blob already exists, hello CreateOrReplace() method will overwrite it.
//You can check whether hello blob exists tooavoid overwriting it by using CloudAppendBlob.Exists().
appendBlob.CreateOrReplace();

int numBlocks = 10;

//Generate an array of random bytes.
Random rnd = new Random();
byte[] bytes = new byte[numBlocks];
rnd.NextBytes(bytes);

//Simulate a logging operation by writing text data and byte data toohello end of hello append blob.
for (int i = 0; i < numBlocks; i++)
{
    appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
        DateTime.UtcNow, bytes[i], Environment.NewLine));
}

//Read hello append blob toohello console window.
Console.WriteLine(appendBlob.DownloadText());
```

Lásd: [ismertetése Blokkblobokat, Lapblobokat és hozzáfűző Blobok](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) hello különbségei hello három típusú blobok további információt.

## <a name="managing-security-for-blobs"></a>A blobok biztonságának kezelése
Alapértelmezés szerint Azure Storage biztosítja az adatok védelmét, ha hozzáférést toohello fiók tulajdonosa, aki rendelkezik hello tárelérési kulcsok korlátozza. Amikor a tárfiókban lévő Blobadatok tooshare van szüksége, fontos toodo nem így van, a fiók hozzáférési kulcsait hello biztonság csökkenése nélkül. Továbbá, hogy biztonságos hello hálózaton keresztül, és az Azure Storage blob adatok tooensure titkosíthatók.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-tooblob-data"></a>Hozzáférés tooblob adatok ellenőrzése
Alapértelmezés szerint hello blob storage-fiók adatai elérhető csak toostorage fiók tulajdonosának. Alapértelmezés szerint a hello hívóbetűre intézett hitelesítési kérésekhez Blob-tároló szükséges. Azonban érdemes toomake egyes blob-adatok elérhető tooother felhasználók. Erre két lehetősége van:

* **Névtelen hozzáférés:** Egy tárolót vagy a benne található blobokat nyilvánosan elérhetővé teheti névtelen hozzáférés céljából. Lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](storage-manage-access-to-resources.md) további információt.
* **Közös hozzáférésű jogosultságkód:** a közös hozzáférésű jogosultságkód (SAS), amely delegált hozzáférést tooa erőforrás a tárfiókban lévő megadott időtartamra és jogosultságokkal rendelkező ügyfelek biztosíthat. További információk: [Using Shared Access Signatures (SAS) (Közös hozzáférésű jogosultságkód (SAS) használata)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

### <a name="encrypting-blob-data"></a>Blobadatok titkosítása
Azure Storage támogatja a titkosítást hello ügyfél, mind a hello kiszolgálón:

* **Ügyféloldali titkosítás:** hello a Storage ügyféloldali kódtára a .NET támogatja a titkosított adatok ügyfélalkalmazásokon belüli előtt tooAzure tárolási feltöltését, és az adatok visszafejtése toohello ügyfél letöltése során. hello kódtár emellett támogatja az Azure Key Vault integration tárfiókkulcs-kezelés számára. További információ: [Client-Side Encryption with .NET for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Ügyféloldali titkosítás a .NET for Microsoft Azure Storage szolgáltatással). Lásd még: [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md) (Oktatóanyag: Blobok titkosítása és visszafejtése a Microsoft Azure Storage-ban az Azure Key Vault használatával).
* **Kiszolgálóoldali titkosítás**: Az Azure Storage már a kiszolgálóoldali titkosítást is támogatja. Lásd: [Azure Storage Service Encryption for Data at Rest (Preview)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Az Azure Storage szolgáltatás inaktívadat-titkosítása (Előzetes verzió)).

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Blob storage alapjait hello, kövesse az alábbi hivatkozások további toolearn.

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure Storage Explorer
* [A Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.

### <a name="blob-storage-samples"></a>Blob Storage-minták
* [Getting Started with Azure Blob Storage in .NET (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel)](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Blob Storage-hivatkozás
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [REST API – referencia](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a>Fogalmi útmutatók
* [Adatátvitel az AzCopy parancssori segédprogram hello](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Get started with File storage for .NET (Bevezetés a File Storage for .NET használatába)](../files/storage-dotnet-how-to-use-files.md)
* [Hogyan toouse Azure blob-tároló hello WebJobs SDK a](../../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

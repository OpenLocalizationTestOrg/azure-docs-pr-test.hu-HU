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
# <a name="get-started-with-azure-blob-storage-using-net"></a><span data-ttu-id="7f1cb-103">Get started with Azure Blob Storage using .NET (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel)</span><span class="sxs-lookup"><span data-stu-id="7f1cb-103">Get started with Azure Blob storage using .NET</span></span>

[!INCLUDE [storage-selector-blob-include](../../../includes/storage-selector-blob-include.md)]

[!INCLUDE [storage-check-out-samples-dotnet](../../../includes/storage-check-out-samples-dotnet.md)]

<span data-ttu-id="7f1cb-104">Az Azure Blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-104">Azure Blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="7f1cb-105">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-105">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="7f1cb-106">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-106">Blob storage is also referred tooas object storage.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="7f1cb-107">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="7f1cb-107">About this tutorial</span></span>
<span data-ttu-id="7f1cb-108">Ez az oktatóanyag bemutatja, hogyan toowrite .NET helykódja olyan gyakori forgatókönyveket tartalmaz Azure Blob storage használatával.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-108">This tutorial shows how toowrite .NET code for some common scenarios using Azure Blob storage.</span></span> <span data-ttu-id="7f1cb-109">A tárgyalt forgatókönyvekben szerepel a blobok feltöltése, listázása, letöltése és törlése.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-109">Scenarios covered include uploading, listing, downloading, and deleting blobs.</span></span>

<span data-ttu-id="7f1cb-110">**Előfeltételek:**</span><span class="sxs-lookup"><span data-stu-id="7f1cb-110">**Prerequisites:**</span></span>

* [<span data-ttu-id="7f1cb-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7f1cb-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/)
* [<span data-ttu-id="7f1cb-112">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="7f1cb-112">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="7f1cb-113">Azure Configuration Manager a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="7f1cb-113">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* <span data-ttu-id="7f1cb-114">Egy [Azure-tárfiók](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="7f1cb-114">An [Azure storage account](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-a-storage-account)</span></span>

[!INCLUDE [storage-dotnet-client-library-version-include](../../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="7f1cb-115">További példák</span><span class="sxs-lookup"><span data-stu-id="7f1cb-115">More samples</span></span>
<span data-ttu-id="7f1cb-116">További példák a Blob Storage használatára: [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel).</span><span class="sxs-lookup"><span data-stu-id="7f1cb-116">For additional examples using Blob storage, see [Getting Started with Azure Blob Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/).</span></span> <span data-ttu-id="7f1cb-117">Töltse le a mintaalkalmazást hello és futtatáshoz, vagy keresse meg a hello kódja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-117">You can download hello sample application and run it, or browse hello code on GitHub.</span></span>

[!INCLUDE [storage-blob-concepts-include](../../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="7f1cb-118">Hozzáadás irányelvekkel</span><span class="sxs-lookup"><span data-stu-id="7f1cb-118">Add using directives</span></span>
<span data-ttu-id="7f1cb-119">Adja hozzá a következő hello **használatával** irányelvek toohello felső részén hello `Program.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-119">Add hello following **using** directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.WindowsAzure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="7f1cb-120">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="7f1cb-120">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-blob-service-client"></a><span data-ttu-id="7f1cb-121">Hello Blob szolgáltatásügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="7f1cb-121">Create hello Blob service client</span></span>
<span data-ttu-id="7f1cb-122">Hello **CloudBlobClient** osztály lehetővé teszi a Blob Storage tárolóban tárolt tooretrieve tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-122">hello **CloudBlobClient** class enables you tooretrieve containers and blobs stored in Blob storage.</span></span> <span data-ttu-id="7f1cb-123">Egyirányú toocreate hello szolgáltatás ügyfele a következő:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-123">Here's one way toocreate hello service client:</span></span>

```csharp
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
```
<span data-ttu-id="7f1cb-124">Most már készen áll a toowrite kódot, amely adatokat olvas és ír tooBlob adattárolás áll.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-124">Now you are ready toowrite code that reads data from and writes data tooBlob storage.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="7f1cb-125">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="7f1cb-125">Create a container</span></span>
[!INCLUDE [storage-container-naming-rules-include](../../../includes/storage-container-naming-rules-include.md)]

<span data-ttu-id="7f1cb-126">A példa bemutatja, hogyan toocreate egy tárolót, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-126">This example shows how toocreate a container if it does not already exist:</span></span>

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

<span data-ttu-id="7f1cb-127">Alapértelmezés szerint hello új tároló privát, ami azt jelenti, hogy meg kell adnia a tárolási kulcs toodownload blobok elérése az ebben a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-127">By default, hello new container is private, meaning that you must specify your storage access key toodownload blobs from this container.</span></span> <span data-ttu-id="7f1cb-128">Ha azt szeretné, hogy toomake hello fájlok hello tároló elérhető tooeveryone belül, beállíthatja hello tároló toobe nyilvános hello kód a következő használatával:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-128">If you want toomake hello files within hello container available tooeveryone, you can set hello container toobe public using hello following code:</span></span>

```csharp
container.SetPermissions(
    new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });
```

<span data-ttu-id="7f1cb-129">Az hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-129">Anyone on hello Internet can see blobs in a public container.</span></span> <span data-ttu-id="7f1cb-130">Azonban módosítsa vagy törölje azokat, csak ha hello megfelelő tárelérési kulccsal vagy egy közös hozzáférésű jogosultságkódot.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-130">However, you can modify or delete them only if you have hello appropriate account access key or a shared access signature.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="7f1cb-131">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="7f1cb-131">Upload a blob into a container</span></span>
<span data-ttu-id="7f1cb-132">Az Azure Blob Storage támogatja a blokkblobokat és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-132">Azure Blob Storage supports block blobs and page blobs.</span></span>  <span data-ttu-id="7f1cb-133">A legtöbb esetben blokkblob típusú toouse ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-133">In most cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="7f1cb-134">a fájl tooa blokkblob tooupload beolvasni a tároló hivatkozását, és használja úgy tooget le a blokkblob hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-134">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="7f1cb-135">Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **UploadFromStream** metódust.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-135">Once you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStream** method.</span></span> <span data-ttu-id="7f1cb-136">Ez a művelet hello blob hoz létre, ha korábban már nem létezik, vagy felülírja, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-136">This operation creates hello blob if it didn't previously exist, or overwrites it if it does exist.</span></span>

<span data-ttu-id="7f1cb-137">a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-137">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

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

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="7f1cb-138">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="7f1cb-138">List hello blobs in a container</span></span>
<span data-ttu-id="7f1cb-139">toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-139">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="7f1cb-140">Hello tároló segítségével **ListBlobs** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-140">You can then use hello container's **ListBlobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="7f1cb-141">tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **IListBlobItem**, tooa kell alakítania azt **CloudBlockBlob**, **CloudPageBlob**, vagy  **CloudBlobDirectory** objektum.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-141">tooaccess hello  rich set of properties and methods for a returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="7f1cb-142">Ha hello típusa ismeretlen, használhatja a típus-ellenőrzés toodetermine mely toocast azt.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-142">If hello type is unknown, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="7f1cb-143">hello következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello az egyes elemek URI _fényképek_ tároló:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-143">hello following code demonstrates how tooretrieve and output hello URI of each item in hello _photos_ container:</span></span>

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

<span data-ttu-id="7f1cb-144">Ha belefoglalja az elérési úttal kapcsolatos információkat a blobnevekbe, olyan virtuális könyvtárstruktúrát hozhat létre, amelyet egy hagyományos fájlrendszerhez hasonlóan rendszerezhet és bejárhat.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-144">By including path information in blob names, you can create a virtual directory structure you can organize and traverse as you would a traditional file system.</span></span> <span data-ttu-id="7f1cb-145">hello könyvtárstruktúrát virtuális csak – hello csak forrásanyag is elérhető a Blob Storage tárolók és blobok.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-145">hello directory structure is virtual only--hello only resources available in Blob storage are containers and blobs.</span></span> <span data-ttu-id="7f1cb-146">Azonban hello a storage ügyféloldali kódtára kínál a **CloudBlobDirectory** toorefer tooa virtuális könyvtár objektum és az ezzel a módszerrel blobokkal hello folyamat leegyszerűsítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-146">However, hello storage client library offers a **CloudBlobDirectory** object toorefer tooa virtual directory and simplify hello process of working with blobs that are organized in this way.</span></span>

<span data-ttu-id="7f1cb-147">Vegyük példaként a következő nevű tároló blokkblobokat hello *fényképek*:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-147">For example, consider hello following set of block blobs in a container named *photos*:</span></span>

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

<span data-ttu-id="7f1cb-148">A hívás esetén **ListBlobs** a hello *fényképek* tároló (ahogy kódrészletet megelőző hello), hierarchikus listát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-148">When you call **ListBlobs** on hello *photos* container (as in hello preceding code snippet), a hierarchical listing is returned.</span></span> <span data-ttu-id="7f1cb-149">Is tartalmaz **CloudBlobDirectory** és **CloudBlockBlob** objektumokat, üdvözlő könyvtárak és hello tárolóban lévő blobok képviselve.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-149">It contains both **CloudBlobDirectory** and **CloudBlockBlob** objects, representing hello directories and blobs in hello container, respectively.</span></span> <span data-ttu-id="7f1cb-150">Ennek kimenete hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-150">hello resulting output looks like:</span></span>

```
Directory: https://<accountname>.blob.core.windows.net/photos/2010/
Directory: https://<accountname>.blob.core.windows.net/photos/2011/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

<span data-ttu-id="7f1cb-151">Másik lehetőségként beállíthatja a hello **Listblobs** hello paramétere **ListBlobs** módszert **igaz**.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-151">Optionally, you can set hello **UseFlatBlobListing** parameter of hello **ListBlobs** method to **true**.</span></span> <span data-ttu-id="7f1cb-152">Ebben az esetben hello tároló összes blobjának adja vissza a rendszer egy **CloudBlockBlob** objektum.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-152">In this case, every blob in hello container is returned as a **CloudBlockBlob** object.</span></span> <span data-ttu-id="7f1cb-153">hívás túl hello**ListBlobs** tooreturn egy strukturálatlan lista néz ki:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-153">hello call too**ListBlobs** tooreturn a flat listing looks like this:</span></span>

```csharp
// Loop over items within hello container and output hello length and URI.
foreach (IListBlobItem item in container.ListBlobs(null, true))
{
    ...
}
```

<span data-ttu-id="7f1cb-154">és hello eredmények néznek ki:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-154">and hello results look like this:</span></span>

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

## <a name="download-blobs"></a><span data-ttu-id="7f1cb-155">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="7f1cb-155">Download blobs</span></span>
<span data-ttu-id="7f1cb-156">toodownload blobokat, először kérjen le egy blobhivatkozást, és majd meghívják a hello **DownloadToStream** metódust.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-156">toodownload blobs, first retrieve a blob reference and then call hello **DownloadToStream** method.</span></span> <span data-ttu-id="7f1cb-157">hello alábbi példában hello **DownloadToStream** metódus tootransfer hello blob tartalma tooa stream objektumot, hogy Ön majd megőrizni a tooa helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-157">hello following example uses hello **DownloadToStream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>

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

<span data-ttu-id="7f1cb-158">Is használhatja a hello **DownloadToStream** metódus toodownload hello tartalmát szöveges karakterláncként blob.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-158">You can also use hello **DownloadToStream** method toodownload hello contents of a blob as a text string.</span></span>

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

## <a name="delete-blobs"></a><span data-ttu-id="7f1cb-159">Blobok törlése</span><span class="sxs-lookup"><span data-stu-id="7f1cb-159">Delete blobs</span></span>
<span data-ttu-id="7f1cb-160">toodelete blob, először kérjen le egy blobhivatkozást, és ezután hívja meg a **törlése** metódust.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-160">toodelete a blob, first get a blob reference and then call the **Delete** method on it.</span></span>

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

## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="7f1cb-161">Blobok listázása több oldalon aszinkron módon</span><span class="sxs-lookup"><span data-stu-id="7f1cb-161">List blobs in pages asynchronously</span></span>
<span data-ttu-id="7f1cb-162">Ha nagyszámú BLOB listázásakor, vagy azt szeretné, hogy egy listázási művelet eredmények toocontrol hello száma, blobok megjelenített eredményoldalra is listázhatja.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-162">If you are listing a large number of blobs, or you want toocontrol hello number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="7f1cb-163">Ez a példa bemutatja, hogyan tooreturn eredményezi lapok aszinkron módon történik, így végrehajtása ne tiltsa le a tooreturn eredmények számos várakozás során.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-163">This example shows how tooreturn results in pages asynchronously, so that execution is not blocked while waiting tooreturn a large set of results.</span></span>

<span data-ttu-id="7f1cb-164">Ez a példa bemutatja egy egyszerű blob listázása, de hierarchikus listát is végrehajthatók hello beállítása _Listblobs_ hello paramétere **ListBlobsSegmentedAsync** metódus too_false_.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-164">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting hello _useFlatBlobListing_ parameter of hello **ListBlobsSegmentedAsync** method too_false_.</span></span>

<span data-ttu-id="7f1cb-165">Hello minta metódushívások egy aszinkron metódus, mert azt kell lennie végrehajtásával kerüli meg a hello _aszinkron_ kulcsszót, és azt kell visszaadnia egy **feladat** objektum.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-165">Because hello sample method calls an asynchronous method, it must be prefaced with hello _async_ keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="7f1cb-166">hello await kulcsszó hello megadott **ListBlobsSegmentedAsync** metódus hello minta metódus végrehajtásának felfüggesztése, hello listázási feladat befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-166">hello await keyword specified for hello **ListBlobsSegmentedAsync** method suspends execution of hello sample method until hello listing task completes.</span></span>

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

## <a name="writing-tooan-append-blob"></a><span data-ttu-id="7f1cb-167">Írás tooan hozzáfűző blob</span><span class="sxs-lookup"><span data-stu-id="7f1cb-167">Writing tooan append blob</span></span>
<span data-ttu-id="7f1cb-168">A hozzáfűző blob hozzáfűzési feladatokra, például naplózásra van optimalizálva.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-168">An append blob is optimized for append operations, such as logging.</span></span> <span data-ttu-id="7f1cb-169">Blokkblob, például a hozzáfűző blob blokkok magában foglalja, de ha hozzáad egy új blokkot tooan hozzáfűző blob, mindig hozzáfűzött toohello hello blob végéhez.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-169">Like a block blob, an append blob is comprised of blocks, but when you add a new block tooan append blob, it is always appended toohello end of hello blob.</span></span> <span data-ttu-id="7f1cb-170">Hozzáfűző blob meglévő blokkja nem frissíthető és nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-170">You cannot update or delete an existing block in an append blob.</span></span> <span data-ttu-id="7f1cb-171">a hozzáfűző BLOB azonosítók hello blokk nem érhetők el, mert azok egy blokkblobhoz tartoznak.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-171">hello block IDs for an append blob are not exposed as they are for a block blob.</span></span>

<span data-ttu-id="7f1cb-172">A hozzáfűző blob minden blokkja különböző méretű mentése tooa legfeljebb 4 MB lehet, és a hozzáfűző blob legfeljebb 50 000 blokkot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-172">Each block in an append blob can be a different size, up tooa maximum of 4 MB, and an append blob can include a maximum of 50,000 blocks.</span></span> <span data-ttu-id="7f1cb-173">hello maximális hozzáfűző blob mérete ezért valamivel több mint 195 GB (4 MB X 50 000 blokk).</span><span class="sxs-lookup"><span data-stu-id="7f1cb-173">hello maximum size of an append blob is therefore slightly more than 195 GB (4 MB X 50,000 blocks).</span></span>

<span data-ttu-id="7f1cb-174">hello az alábbi példa létrehoz egy új hozzáfűző blob, és hozzáfűzi egyes adatok tooit, egy egyszerű naplózási műveletet szimulálva.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-174">hello example below creates a new append blob and appends some data tooit, simulating a simple logging operation.</span></span>

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

<span data-ttu-id="7f1cb-175">Lásd: [ismertetése Blokkblobokat, Lapblobokat és hozzáfűző Blobok](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) hello különbségei hello három típusú blobok további információt.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-175">See [Understanding Block Blobs, Page Blobs, and Append Blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) for more information about hello differences between hello three types of blobs.</span></span>

## <a name="managing-security-for-blobs"></a><span data-ttu-id="7f1cb-176">A blobok biztonságának kezelése</span><span class="sxs-lookup"><span data-stu-id="7f1cb-176">Managing security for blobs</span></span>
<span data-ttu-id="7f1cb-177">Alapértelmezés szerint Azure Storage biztosítja az adatok védelmét, ha hozzáférést toohello fiók tulajdonosa, aki rendelkezik hello tárelérési kulcsok korlátozza.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-177">By default, Azure Storage keeps your data secure by limiting access toohello account owner, who is in possession of hello account access keys.</span></span> <span data-ttu-id="7f1cb-178">Amikor a tárfiókban lévő Blobadatok tooshare van szüksége, fontos toodo nem így van, a fiók hozzáférési kulcsait hello biztonság csökkenése nélkül.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-178">When you need tooshare blob data in your storage account, it is important toodo so without compromising hello security of your account access keys.</span></span> <span data-ttu-id="7f1cb-179">Továbbá, hogy biztonságos hello hálózaton keresztül, és az Azure Storage blob adatok tooensure titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-179">Additionally, you can encrypt blob data tooensure that it is secure going over hello wire and in Azure Storage.</span></span>

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-tooblob-data"></a><span data-ttu-id="7f1cb-180">Hozzáférés tooblob adatok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="7f1cb-180">Controlling access tooblob data</span></span>
<span data-ttu-id="7f1cb-181">Alapértelmezés szerint hello blob storage-fiók adatai elérhető csak toostorage fiók tulajdonosának.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-181">By default, hello blob data in your storage account is accessible only toostorage account owner.</span></span> <span data-ttu-id="7f1cb-182">Alapértelmezés szerint a hello hívóbetűre intézett hitelesítési kérésekhez Blob-tároló szükséges.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-182">Authenticating requests against Blob storage requires hello account access key by default.</span></span> <span data-ttu-id="7f1cb-183">Azonban érdemes toomake egyes blob-adatok elérhető tooother felhasználók.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-183">However, you may wish toomake certain blob data available tooother users.</span></span> <span data-ttu-id="7f1cb-184">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-184">You have two options:</span></span>

* <span data-ttu-id="7f1cb-185">**Névtelen hozzáférés:** Egy tárolót vagy a benne található blobokat nyilvánosan elérhetővé teheti névtelen hozzáférés céljából.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-185">**Anonymous access:** You can make a container or its blobs publicly available for anonymous access.</span></span> <span data-ttu-id="7f1cb-186">Lásd: [kezelheti a névtelen olvasási hozzáférés toocontainers és blobok](storage-manage-access-to-resources.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-186">See [Manage anonymous read access toocontainers and blobs](storage-manage-access-to-resources.md) for more information.</span></span>
* <span data-ttu-id="7f1cb-187">**Közös hozzáférésű jogosultságkód:** a közös hozzáférésű jogosultságkód (SAS), amely delegált hozzáférést tooa erőforrás a tárfiókban lévő megadott időtartamra és jogosultságokkal rendelkező ügyfelek biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-187">**Shared access signatures:** You can provide clients with a shared access signature (SAS), which provides delegated access tooa resource in your storage account, with permissions that you specify and over an interval that you specify.</span></span> <span data-ttu-id="7f1cb-188">További információk: [Using Shared Access Signatures (SAS) (Közös hozzáférésű jogosultságkód (SAS) használata)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="7f1cb-188">See [Using Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span>

### <a name="encrypting-blob-data"></a><span data-ttu-id="7f1cb-189">Blobadatok titkosítása</span><span class="sxs-lookup"><span data-stu-id="7f1cb-189">Encrypting blob data</span></span>
<span data-ttu-id="7f1cb-190">Azure Storage támogatja a titkosítást hello ügyfél, mind a hello kiszolgálón:</span><span class="sxs-lookup"><span data-stu-id="7f1cb-190">Azure Storage supports encrypting blob data both at hello client and on hello server:</span></span>

* <span data-ttu-id="7f1cb-191">**Ügyféloldali titkosítás:** hello a Storage ügyféloldali kódtára a .NET támogatja a titkosított adatok ügyfélalkalmazásokon belüli előtt tooAzure tárolási feltöltését, és az adatok visszafejtése toohello ügyfél letöltése során.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-191">**Client-side encryption:** hello Storage Client Library for .NET supports encrypting data within client applications before uploading tooAzure Storage, and decrypting data while downloading toohello client.</span></span> <span data-ttu-id="7f1cb-192">hello kódtár emellett támogatja az Azure Key Vault integration tárfiókkulcs-kezelés számára.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-192">hello library also supports integration with Azure Key Vault for storage account key management.</span></span> <span data-ttu-id="7f1cb-193">További információ: [Client-Side Encryption with .NET for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Ügyféloldali titkosítás a .NET for Microsoft Azure Storage szolgáltatással).</span><span class="sxs-lookup"><span data-stu-id="7f1cb-193">See [Client-Side Encryption with .NET for Microsoft Azure Storage](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) for more information.</span></span> <span data-ttu-id="7f1cb-194">Lásd még: [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md) (Oktatóanyag: Blobok titkosítása és visszafejtése a Microsoft Azure Storage-ban az Azure Key Vault használatával).</span><span class="sxs-lookup"><span data-stu-id="7f1cb-194">Also see [Tutorial: Encrypt and decrypt blobs in Microsoft Azure Storage using Azure Key Vault](storage-encrypt-decrypt-blobs-key-vault.md).</span></span>
* <span data-ttu-id="7f1cb-195">**Kiszolgálóoldali titkosítás**: Az Azure Storage már a kiszolgálóoldali titkosítást is támogatja.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-195">**Server-side encryption**: Azure Storage now supports server-side encryption.</span></span> <span data-ttu-id="7f1cb-196">Lásd: [Azure Storage Service Encryption for Data at Rest (Preview)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) (Az Azure Storage szolgáltatás inaktívadat-titkosítása (Előzetes verzió)).</span><span class="sxs-lookup"><span data-stu-id="7f1cb-196">See [Azure Storage Service Encryption for Data at Rest (Preview)](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f1cb-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f1cb-197">Next steps</span></span>
<span data-ttu-id="7f1cb-198">Most, hogy megismerte a Blob storage alapjait hello, kövesse az alábbi hivatkozások további toolearn.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-198">Now that you've learned hello basics of Blob storage, follow these links toolearn more.</span></span>

### <a name="microsoft-azure-storage-explorer"></a><span data-ttu-id="7f1cb-199">Microsoft Azure Storage Explorer</span><span class="sxs-lookup"><span data-stu-id="7f1cb-199">Microsoft Azure Storage Explorer</span></span>
* <span data-ttu-id="7f1cb-200">[A Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7f1cb-200">[Microsoft Azure Storage Explorer (MASE)](../../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

### <a name="blob-storage-samples"></a><span data-ttu-id="7f1cb-201">Blob Storage-minták</span><span class="sxs-lookup"><span data-stu-id="7f1cb-201">Blob storage samples</span></span>
* [<span data-ttu-id="7f1cb-202">Getting Started with Azure Blob Storage in .NET (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel)</span><span class="sxs-lookup"><span data-stu-id="7f1cb-202">Getting Started with Azure Blob Storage in .NET</span></span>](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a><span data-ttu-id="7f1cb-203">Blob Storage-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="7f1cb-203">Blob storage reference</span></span>
* [<span data-ttu-id="7f1cb-204">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="7f1cb-204">Storage Client Library for .NET reference</span></span>](https://msdn.microsoft.com/library/azure/mt347887.aspx)
* [<span data-ttu-id="7f1cb-205">REST API – referencia</span><span class="sxs-lookup"><span data-stu-id="7f1cb-205">REST API reference</span></span>](/rest/api/storageservices/azure-storage-services-rest-api-reference)

### <a name="conceptual-guides"></a><span data-ttu-id="7f1cb-206">Fogalmi útmutatók</span><span class="sxs-lookup"><span data-stu-id="7f1cb-206">Conceptual guides</span></span>
* [<span data-ttu-id="7f1cb-207">Adatátvitel az AzCopy parancssori segédprogram hello</span><span class="sxs-lookup"><span data-stu-id="7f1cb-207">Transfer data with hello AzCopy command-line utility</span></span>](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [<span data-ttu-id="7f1cb-208">Get started with File storage for .NET (Bevezetés a File Storage for .NET használatába)</span><span class="sxs-lookup"><span data-stu-id="7f1cb-208">Get started with File storage for .NET</span></span>](../files/storage-dotnet-how-to-use-files.md)
* [<span data-ttu-id="7f1cb-209">Hogyan toouse Azure blob-tároló hello WebJobs SDK a</span><span class="sxs-lookup"><span data-stu-id="7f1cb-209">How toouse Azure blob storage with hello WebJobs SDK</span></span>](../../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

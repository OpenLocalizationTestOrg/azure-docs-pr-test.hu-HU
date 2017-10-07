---
title: "a blob storage és a Visual Studio (az ASP.NET Core) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget el, a Visual Studio ASP.NET Core projektben Azure Blob storage használatával, a Visual Studio használatával storage-fiók létrehozása után kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 094b596a-c92c-40c4-a0f5-86407ae79672
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: a4c31c08c38d99eb9c66fe2af72ca42fa3804688
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet-core"></a><span data-ttu-id="d78c5-103">Ismerkedés az Azure Blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET mag)</span><span class="sxs-lookup"><span data-stu-id="d78c5-103">Get started with Azure Blob storage and Visual Studio connected services (ASP.NET Core)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="d78c5-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d78c5-104">Overview</span></span>
<span data-ttu-id="d78c5-105">Ez a cikk ismerteti, hogyan tooget el az Azure Blob Storage tárolót a Visual Studióban létrehozott vagy egy Azure storage-fiók ASP.NET Core projektben hivatkozott hello Visual Studio kapcsolódó szolgáltatások hozzáadása párbeszédpanelen után.</span><span class="sxs-lookup"><span data-stu-id="d78c5-105">This article describes how tooget started using Azure Blob storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio Add Connected Services dialog.</span></span>

<span data-ttu-id="d78c5-106">Az Azure Blob storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatot, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="d78c5-106">Azure Blob storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="d78c5-107">Egy blob bármilyen méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="d78c5-107">A single blob can be any size.</span></span> <span data-ttu-id="d78c5-108">Blobok lehetnek többek között a lemezképek, a hang- és fájlok, a nyers adatok és a fájlok.</span><span class="sxs-lookup"><span data-stu-id="d78c5-108">Blobs can be things like images, audio and video files, raw data, and document files.</span></span> <span data-ttu-id="d78c5-109">Ez a cikk ismerteti, hogyan tooget használatába a blob storage hello Visual Studio használatával Azure-tárfiók létrehozása után **kapcsolódó szolgáltatások hozzáadása** az ASP.NET Core projekt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d78c5-109">This article describes how tooget started with blob storage after you create an Azure storage account by using hello Visual Studio **Add Connected Services** dialog in an ASP.NET Core project.</span></span>

<span data-ttu-id="d78c5-110">Ugyanúgy, mint a él fájlok, mappák, a tárolási BLOB élő tárolókban lévő.</span><span class="sxs-lookup"><span data-stu-id="d78c5-110">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="d78c5-111">Miután létrehozta a tárolási, hello tárolási hozzon létre egy vagy több tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d78c5-111">After you have created a storage, you create one or more containers in hello storage.</span></span> <span data-ttu-id="d78c5-112">Például "Lapkivágások" nevű tárolási, az úgynevezett "képek" toostore képek hello storage hozhat létre, és más néven "hang" toostore hangfájlok.</span><span class="sxs-lookup"><span data-stu-id="d78c5-112">For example, in a storage called "Scrapbook," you can create containers in hello storage called "images" toostore pictures and another called "audio" toostore audio files.</span></span> <span data-ttu-id="d78c5-113">Hello tárolók létrehozása után feltöltheti egyes blob fájlok toothem.</span><span class="sxs-lookup"><span data-stu-id="d78c5-113">After you create hello containers, you can upload individual blob files toothem.</span></span> <span data-ttu-id="d78c5-114">Lásd: [az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md) programozott módon kezelésére a blobok további tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="d78c5-114">See [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) for more information on programmatically manipulating blobs.</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="d78c5-115">Hozzáférés a blob tárolók kód</span><span class="sxs-lookup"><span data-stu-id="d78c5-115">Access blob containers in code</span></span>
<span data-ttu-id="d78c5-116">tooprogrammatically blobok elérése az ASP.NET Core projektben, kell tooadd hello a következő elemek, ha azok már nem létezik.</span><span class="sxs-lookup"><span data-stu-id="d78c5-116">tooprogrammatically access blobs in ASP.NET Core projects, you need tooadd hello following items, if they're not already present.</span></span>

1. <span data-ttu-id="d78c5-117">Adja hozzá a következő kód névtér nyilatkozatok toohello felső részén a C# fájlban használni kívánt tooprogrammatically hozzáférés az Azure storage hello.</span><span class="sxs-lookup"><span data-stu-id="d78c5-117">Add hello following code namespace declarations toohello top of any C# file in which you want tooprogrammatically access Azure storage.</span></span>
   
        using Microsoft.Extensions.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Extensions.Logging.LogLevel;
2. <span data-ttu-id="d78c5-118">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="d78c5-118">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="d78c5-119">A következő kód tooget hello használja, a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="d78c5-119">Use hello following code tooget your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = new CloudStorageAccount(
            new Microsoft.WindowsAzure.Storage.Auth.StorageCredentials(
            "<storage-account-name>",
            "<access-key>"), true);
   
    <span data-ttu-id="d78c5-120">**Megjegyzés:** a következő részekben hello az összes fenti kódot hello kód előtt hello használja.</span><span class="sxs-lookup"><span data-stu-id="d78c5-120">**NOTE:** Use all of hello above code in front of hello code in hello following sections.</span></span>
3. <span data-ttu-id="d78c5-121">Használja a **CloudBlobClient** tooget objektum egy **CloudBlobContainer** hivatkozás tooan meglévő tárolóhoz a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d78c5-121">Use a **CloudBlobClient** object tooget a **CloudBlobContainer** reference tooan existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

## <a name="create-a-container-in-code"></a><span data-ttu-id="d78c5-122">A kód egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d78c5-122">Create a container in code</span></span>
<span data-ttu-id="d78c5-123">Is használhatja a hello **CloudBlobClient** toocreate a tárfiókban lévő tárolója.</span><span class="sxs-lookup"><span data-stu-id="d78c5-123">You can also use hello **CloudBlobClient** toocreate a container in your storage account.</span></span> <span data-ttu-id="d78c5-124">Toodo szüksége egy hívás tooadd túl**CreateIfNotExistsAsync** hasonlóan hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="d78c5-124">All you need toodo is tooadd a call too**CreateIfNotExistsAsync** as in hello following code:</span></span>

    // Create a blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get a reference tooa container named "my-new-container."
    CloudBlobContainer container = blobClient.GetContainerReference("my-new-container");

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="d78c5-125">**Megjegyzés:** hello API-hívások tooAzure tárolási hajtsa végre az ASP.NET Core aszinkron jellegűek.</span><span class="sxs-lookup"><span data-stu-id="d78c5-125">**NOTE:** hello APIs that perform calls tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="d78c5-126">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="d78c5-126">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="d78c5-127">az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="d78c5-127">hello code below assumes async programming methods are being used.</span></span>

<span data-ttu-id="d78c5-128">toomake hello fájlok hello tároló elérhető tooeveryone belül, beállíthat hello tároló toobe nyilvános hello a következő kód használatával.</span><span class="sxs-lookup"><span data-stu-id="d78c5-128">toomake hello files within hello container available tooeveryone, you can set hello container toobe public by using hello following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="d78c5-129">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="d78c5-129">Upload a blob into a container</span></span>
<span data-ttu-id="d78c5-130">tooupload fájl blob egy tárolóba beolvasni a tároló hivatkozását, és egy blobhivatkozást tooget használják.</span><span class="sxs-lookup"><span data-stu-id="d78c5-130">tooupload a blob file into a container, get a container reference and use it tooget a blob reference.</span></span> <span data-ttu-id="d78c5-131">Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **UploadFromStreamAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="d78c5-131">After you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStreamAsync** method.</span></span> <span data-ttu-id="d78c5-132">Ez a művelet hello blob hoz létre, ha már nem létezik, vagy felülírja, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="d78c5-132">This operation creates hello blob if it's not already there, or overwrites it if it does exist.</span></span> <span data-ttu-id="d78c5-133">a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="d78c5-133">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

    // Get a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with hello contents of a local file
    // named "myfile".
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        await blockBlob.UploadFromStreamAsync(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="d78c5-134">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="d78c5-134">List hello blobs in a container</span></span>
<span data-ttu-id="d78c5-135">toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="d78c5-135">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="d78c5-136">Majd hívása hello tároló **ListBlobsSegmentedAsync** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül.</span><span class="sxs-lookup"><span data-stu-id="d78c5-136">You can then call hello container's **ListBlobsSegmentedAsync** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="d78c5-137">tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **IListBlobItem**, tooa kell alakítania azt **CloudBlockBlob**, **CloudPageBlob**, vagy  **CloudBlobDirectory** objektum.</span><span class="sxs-lookup"><span data-stu-id="d78c5-137">tooaccess hello rich set of properties and methods for a returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="d78c5-138">Ha nem tudja hello blob írja be, használhatja a típus-ellenőrzés toodetermine mely toocast azt.</span><span class="sxs-lookup"><span data-stu-id="d78c5-138">If you don't know hello blob type, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="d78c5-139">hello a következő kód bemutatja, hogyan tooretrieve és a kimeneti hello a tárolóban lévő egyes elemek URI Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d78c5-139">hello following code demonstrates how tooretrieve and output hello URI of each item in a container.</span></span>

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

<span data-ttu-id="d78c5-140">Nincsenek más módokon toolist hello tartalmát egy blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="d78c5-140">There are others ways toolist hello contents of a blob container.</span></span> <span data-ttu-id="d78c5-141">Lásd: [az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) további információt.</span><span class="sxs-lookup"><span data-stu-id="d78c5-141">See [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md#list-the-blobs-in-a-container) for more information.</span></span>

## <a name="download-a-blob"></a><span data-ttu-id="d78c5-142">Blob letöltése</span><span class="sxs-lookup"><span data-stu-id="d78c5-142">Download a blob</span></span>
<span data-ttu-id="d78c5-143">toodownload blob, először a hivatkozás toohello blob kapnak, és hívhatja hello **DownloadToStreamAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="d78c5-143">toodownload a blob, first get a reference toohello blob, and then call hello **DownloadToStreamAsync** method.</span></span> <span data-ttu-id="d78c5-144">hello alábbi példában hello **DownloadToStreamAsync** metódus tootransfer hello blob tartalma tooa stream objektumra, majd mentheti egy helyi fájl.</span><span class="sxs-lookup"><span data-stu-id="d78c5-144">hello following example uses hello **DownloadToStreamAsync** method tootransfer hello blob contents tooa stream object that you can then save as a local file.</span></span>

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save hello blob contents tooa file named "myfile".
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        await blockBlob.DownloadToStreamAsync(fileStream);
    }

<span data-ttu-id="d78c5-145">Más módon toosave blobokat fájlokba.</span><span class="sxs-lookup"><span data-stu-id="d78c5-145">There are other ways toosave blobs as files.</span></span> <span data-ttu-id="d78c5-146">Lásd: [az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md#download-blobs) további információt.</span><span class="sxs-lookup"><span data-stu-id="d78c5-146">See [Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md#download-blobs) for more information.</span></span>

## <a name="delete-a-blob"></a><span data-ttu-id="d78c5-147">Blob törlése</span><span class="sxs-lookup"><span data-stu-id="d78c5-147">Delete a blob</span></span>
<span data-ttu-id="d78c5-148">toodelete blob, először a hivatkozás toohello blob kapnak, és hívhatja hello **DeleteAsync** metódust.</span><span class="sxs-lookup"><span data-stu-id="d78c5-148">toodelete a blob, first get a reference toohello blob, and then call hello **DeleteAsync** method on it.</span></span>

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    await blockBlob.DeleteAsync();

## <a name="next-steps"></a><span data-ttu-id="d78c5-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d78c5-149">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


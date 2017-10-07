---
title: "a blob storage és a Visual Studio (felhőszolgáltatások) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának Azure Blob storage egy Visual Studio felhőszolgáltatás-projektet a Visual Studio használatával tooa tárfiók kapcsolódás szolgáltatások csatlakoztatása után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 1144a958-f75a-4466-bb21-320b7ae8f304
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 158197a9d49bc4f26841d689405529192c52f529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="20320-103">Ismerkedés az Azure Blob Storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)</span><span class="sxs-lookup"><span data-stu-id="20320-103">Get started with Azure Blob Storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="20320-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="20320-104">Overview</span></span>
<span data-ttu-id="20320-105">Ez a cikk ismerteti, hogyan tooget után indítják el az Azure Blob Storage szolgáltatással létrehozott vagy egy Azure Storage-fiók hivatkozott hello Visual Studio használatával **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel a Visual Studio felhő szolgáltatási projektet.</span><span class="sxs-lookup"><span data-stu-id="20320-105">This article describes how tooget started with Azure Blob Storage after you created or referenced an Azure Storage account by using hello Visual Studio **Add Connected Services** dialog in a Visual Studio cloud services project.</span></span> <span data-ttu-id="20320-106">Mutat be, hogyan tooaccess blobtárolók, és hogyan tooperform gyakori feladatokat, például feltöltése, listázása és blobok letöltése és létrehozása.</span><span class="sxs-lookup"><span data-stu-id="20320-106">We'll show you how tooaccess and create blob containers, and how tooperform common tasks like uploading, listing, and downloading blobs.</span></span> <span data-ttu-id="20320-107">hello minták C nyelven íródtak\# és hello [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="20320-107">hello samples are written in C\# and use hello [Microsoft Azure Storage Client Library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="20320-108">Az Azure Blob Storage egy olyan szolgáltatás nagy mennyiségű strukturálatlan adatot, amely képes bárhonnan elérhetők a HTTP vagy HTTPS PROTOKOLLON keresztül hello world tárolásához.</span><span class="sxs-lookup"><span data-stu-id="20320-108">Azure Blob Storage is a service for storing large amounts of unstructured data that can be accessed from anywhere in hello world via HTTP or HTTPS.</span></span> <span data-ttu-id="20320-109">Egy blob bármilyen méretű lehet.</span><span class="sxs-lookup"><span data-stu-id="20320-109">A single blob can be any size.</span></span> <span data-ttu-id="20320-110">Blobok lehetnek többek között a lemezképek, a hang- és fájlok, a nyers adatok és a fájlok.</span><span class="sxs-lookup"><span data-stu-id="20320-110">Blobs can be things like images, audio and video files, raw data, and document files.</span></span>

<span data-ttu-id="20320-111">Ugyanúgy, mint a él fájlok, mappák, a tárolási BLOB élő tárolókban lévő.</span><span class="sxs-lookup"><span data-stu-id="20320-111">Just as files live in folders, storage blobs live in containers.</span></span> <span data-ttu-id="20320-112">Miután létrehozta a tárolási, hello tárolási hozzon létre egy vagy több tárolóban.</span><span class="sxs-lookup"><span data-stu-id="20320-112">After you have created a storage, you create one or more containers in hello storage.</span></span> <span data-ttu-id="20320-113">Például "Lapkivágások" nevű tárolási, az úgynevezett "képek" toostore képek hello storage hozhat létre, és más néven "hang" toostore hangfájlok.</span><span class="sxs-lookup"><span data-stu-id="20320-113">For example, in a storage called "Scrapbook," you can create containers in hello storage called "images" toostore pictures and another called "audio" toostore audio files.</span></span> <span data-ttu-id="20320-114">Hello tárolók létrehozása után feltöltheti egyes blob fájlok toothem.</span><span class="sxs-lookup"><span data-stu-id="20320-114">After you create hello containers, you can upload individual blob files toothem.</span></span>

* <span data-ttu-id="20320-115">Programozott módon kezelésére a blobok további információkért lásd: [az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="20320-115">For more information on programmatically manipulating blobs, see [Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span>
* <span data-ttu-id="20320-116">Azure Storage kapcsolatos általános információkért lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/).</span><span class="sxs-lookup"><span data-stu-id="20320-116">For general information about Azure Storage, see [Storage documentation](https://azure.microsoft.com/documentation/services/storage/).</span></span>
* <span data-ttu-id="20320-117">Azure Cloud Services kapcsolatos általános információkért lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/).</span><span class="sxs-lookup"><span data-stu-id="20320-117">For general information about Azure Cloud Services, see [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/).</span></span>
* <span data-ttu-id="20320-118">ASP.NET-alkalmazások programozásával kapcsolatos további információkért lásd: [ASP.NET](http://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="20320-118">For more information about programming ASP.NET applications, see [ASP.NET](http://www.asp.net).</span></span>

## <a name="access-blob-containers-in-code"></a><span data-ttu-id="20320-119">Hozzáférés a blob tárolók kód</span><span class="sxs-lookup"><span data-stu-id="20320-119">Access blob containers in code</span></span>
<span data-ttu-id="20320-120">tooprogrammatically blobok a felhőszolgáltatás-projektekre nézve, ha azok már nem található a következő elemek tooadd hello kell.</span><span class="sxs-lookup"><span data-stu-id="20320-120">tooprogrammatically access blobs in cloud service projects, you need tooadd hello following items, if they're not already present.</span></span>

1. <span data-ttu-id="20320-121">Adja hozzá a következő kód névtér nyilatkozatok toohello felső részén a C# fájlban, amelyben tooprogrammatically access Azure Storage kívánja hello.</span><span class="sxs-lookup"><span data-stu-id="20320-121">Add hello following code namespace declarations toohello top of any C# file in which you wish tooprogrammatically access Azure Storage.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="20320-122">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="20320-122">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="20320-123">A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="20320-123">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("<storage account name>_AzureStorageConnectionString"));
3. <span data-ttu-id="20320-124">Első egy **CloudBlobClient** tooreference a tárfiókban lévő meglévő tároló objektum.</span><span class="sxs-lookup"><span data-stu-id="20320-124">Get a **CloudBlobClient** object tooreference an existing container in your storage account.</span></span>
   
        // Create a blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
4. <span data-ttu-id="20320-125">Első egy **CloudBlobContainer** tooreference egy adott blob tároló objektum.</span><span class="sxs-lookup"><span data-stu-id="20320-125">Get a **CloudBlobContainer** object tooreference a specific blob container.</span></span>
   
        // Get a reference tooa container named "mycontainer."
        CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

> [!NOTE]
> <span data-ttu-id="20320-126">Minden hello előző eljárásban a következő szakaszok hello hello kód elé hello kód használható.</span><span class="sxs-lookup"><span data-stu-id="20320-126">Use all of hello code shown in hello previous procedure in front of hello code shown in hello following sections.</span></span>
> 
> 

## <a name="create-a-container-in-code"></a><span data-ttu-id="20320-127">A kód egy tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="20320-127">Create a container in code</span></span>
> [!NOTE]
> <span data-ttu-id="20320-128">Egyes hajtsa végre a hívások tooAzure tárolási ki az ASP.NET API-k aszinkron jellegűek.</span><span class="sxs-lookup"><span data-stu-id="20320-128">Some APIs that perform calls out tooAzure Storage in ASP.NET are asynchronous.</span></span> <span data-ttu-id="20320-129">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="20320-129">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="20320-130">a következő példa hello hello kód azt feltételezi, hogy aszinkron programozási módszerek használatát.</span><span class="sxs-lookup"><span data-stu-id="20320-130">hello code in hello following example assumes that you are using async programming methods.</span></span>
> 
> 

<span data-ttu-id="20320-131">egy tároló a tárfiókban lévő toocreate, toodo szüksége van adjon hozzá egy túl**CreateIfNotExistsAsync** hasonlóan hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="20320-131">toocreate a container in your storage account, all you need toodo is add a call too**CreateIfNotExistsAsync** as in hello following code:</span></span>

    // If "mycontainer" doesn't exist, create it.
    await container.CreateIfNotExistsAsync();


<span data-ttu-id="20320-132">toomake hello fájlok hello tároló elérhető tooeveryone belül, beállíthat hello tároló toobe nyilvános hello a következő kód használatával.</span><span class="sxs-lookup"><span data-stu-id="20320-132">toomake hello files within hello container available tooeveryone, you can set hello container toobe public by using hello following code.</span></span>

    await container.SetPermissionsAsync(new BlobContainerPermissions
    {
        PublicAccess = BlobContainerPublicAccessType.Blob
    });


<span data-ttu-id="20320-133">Hello Internet bárki láthatja a nyilvános tárolókban lévő blobokat, de módosítja, vagy törölje azokat, csak ha hello megfelelő hozzáférési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="20320-133">Anyone on hello Internet can see blobs in a public container, but you can modify or delete them only if you have hello appropriate access key.</span></span>

## <a name="upload-a-blob-into-a-container"></a><span data-ttu-id="20320-134">Blobok feltöltése a tárolóba</span><span class="sxs-lookup"><span data-stu-id="20320-134">Upload a blob into a container</span></span>
<span data-ttu-id="20320-135">Az Azure Storage támogatja a blokkblobokat és a lapblobokat.</span><span class="sxs-lookup"><span data-stu-id="20320-135">Azure Storage supports block blobs and page blobs.</span></span> <span data-ttu-id="20320-136">Hello legtöbb esetben a blokkblob típusú toouse ajánlott hello.</span><span class="sxs-lookup"><span data-stu-id="20320-136">In hello majority of cases, block blob is hello recommended type toouse.</span></span>

<span data-ttu-id="20320-137">a fájl tooa blokkblob tooupload beolvasni a tároló hivatkozását, és használja úgy tooget le a blokkblob hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="20320-137">tooupload a file tooa block blob, get a container reference and use it tooget a block blob reference.</span></span> <span data-ttu-id="20320-138">Miután egy blobhivatkozást, az adatok tooit bármilyen streamet feltölthet által hívó hello **UploadFromStream** metódust.</span><span class="sxs-lookup"><span data-stu-id="20320-138">Once you have a blob reference, you can upload any stream of data tooit by calling hello **UploadFromStream** method.</span></span> <span data-ttu-id="20320-139">Ez a művelet hello blob hoz létre, ha korábban már nem létezik, vagy felülírja, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="20320-139">This operation creates hello blob if it didn't previously exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="20320-140">a következő példa azt mutatja meg hogyan hello tooupload blob egy tárolóba, és feltételezi, hogy hello tároló már létre lett hozva.</span><span class="sxs-lookup"><span data-stu-id="20320-140">hello following example shows how tooupload a blob into a container and assumes that hello container was already created.</span></span>

    // Retrieve a reference tooa blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite hello "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-hello-blobs-in-a-container"></a><span data-ttu-id="20320-141">Lista hello a tárolóban lévő blobok</span><span class="sxs-lookup"><span data-stu-id="20320-141">List hello blobs in a container</span></span>
<span data-ttu-id="20320-142">toolist hello BLOB a tárolóban lévő először kapnak a tároló hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="20320-142">toolist hello blobs in a container, first get a container reference.</span></span> <span data-ttu-id="20320-143">Hello tároló segítségével **ListBlobs** metódus tooretrieve hello blobokat és/vagy könyvtárakat belül.</span><span class="sxs-lookup"><span data-stu-id="20320-143">You can then use hello container's **ListBlobs** method tooretrieve hello blobs and/or directories within it.</span></span> <span data-ttu-id="20320-144">tooaccess hello tulajdonságai és metódusai visszaadásakor széles skáláját **IListBlobItem**, tooa kell alakítania azt **CloudBlockBlob**, **CloudPageBlob**, vagy  **CloudBlobDirectory** objektum.</span><span class="sxs-lookup"><span data-stu-id="20320-144">tooaccess hello rich set of properties and methods for a  returned **IListBlobItem**, you must cast it tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="20320-145">Ha hello típusa ismeretlen, használhatja a típus-ellenőrzés toodetermine mely toocast azt.</span><span class="sxs-lookup"><span data-stu-id="20320-145">If hello type is unknown, you can use a type check toodetermine which toocast it to.</span></span> <span data-ttu-id="20320-146">hello következő kód bemutatja, hogyan tooretrieve és a kimeneti hello hello az egyes elemek URI **fényképek** tároló:</span><span class="sxs-lookup"><span data-stu-id="20320-146">hello following code demonstrates how tooretrieve and output hello URI of each item in hello **photos** container:</span></span>

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

<span data-ttu-id="20320-147">Ahogy az előző példakód hello, hello blob szolgáltatás rendelkezik hello fogalma könyvtárak belül tárolókat is beleértve.</span><span class="sxs-lookup"><span data-stu-id="20320-147">As shown in hello previous code sample, hello blob service has hello concept of directories within containers, as well.</span></span> <span data-ttu-id="20320-148">Ez az, hogy a blobokat több mappa hasonló struktúrában rendezhetők.</span><span class="sxs-lookup"><span data-stu-id="20320-148">This is so that you can organize your blobs in a more folder-like structure.</span></span> <span data-ttu-id="20320-149">Vegyük példaként a következő nevű tároló blokkblobokat hello **fényképek**:</span><span class="sxs-lookup"><span data-stu-id="20320-149">For example, consider hello following set of block blobs in a container named **photos**:</span></span>

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

<span data-ttu-id="20320-150">A hívás esetén **ListBlobs** hello tárolóban (hello korábbi példa szerint), a visszaadott hello gyűjtemény tartalmaz **CloudBlobDirectory** és **CloudBlockBlob** objektumok képviselő hello illetve hello felső szinten található blobokat.</span><span class="sxs-lookup"><span data-stu-id="20320-150">When you call **ListBlobs** on hello container (as in hello previous sample), hello collection returned contains **CloudBlobDirectory** and **CloudBlockBlob** objects representing hello directories and blobs contained at hello top level.</span></span> <span data-ttu-id="20320-151">Hello ennek kimenete a következő:</span><span class="sxs-lookup"><span data-stu-id="20320-151">Here is hello resulting output:</span></span>

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


<span data-ttu-id="20320-152">Másik lehetőségként beállíthatja a hello **Listblobs** paraméterét hello **ListBlobs** módszert **igaz**.</span><span class="sxs-lookup"><span data-stu-id="20320-152">Optionally, you can set hello **UseFlatBlobListing** parameter of of hello **ListBlobs** method to **true**.</span></span> <span data-ttu-id="20320-153">Minden blob ad vissza az eredmény egy **CloudBlockBlob**directory függetlenül.</span><span class="sxs-lookup"><span data-stu-id="20320-153">This results in every blob being returned as a **CloudBlockBlob**, regardless of directory.</span></span> <span data-ttu-id="20320-154">Itt hívás, amely hello túl**ListBlobs**:</span><span class="sxs-lookup"><span data-stu-id="20320-154">Here is hello call too**ListBlobs**:</span></span>

    // Loop over items within hello container and output hello length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

<span data-ttu-id="20320-155">és az alábbiakban hello eredmények:</span><span class="sxs-lookup"><span data-stu-id="20320-155">and here are hello results:</span></span>

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg

<span data-ttu-id="20320-156">További információkért lásd: [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span><span class="sxs-lookup"><span data-stu-id="20320-156">For more information, see [CloudBlobContainer.ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx).</span></span>

## <a name="download-blobs"></a><span data-ttu-id="20320-157">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="20320-157">Download blobs</span></span>
<span data-ttu-id="20320-158">toodownload blobokat, először kérjen le egy blobhivatkozást, és majd meghívják a hello **DownloadToStream** metódust.</span><span class="sxs-lookup"><span data-stu-id="20320-158">toodownload blobs, first retrieve a blob reference and then call hello **DownloadToStream** method.</span></span> <span data-ttu-id="20320-159">hello alábbi példában hello **DownloadToStream** metódus tootransfer hello blob tartalma tooa stream objektumot, hogy Ön majd megőrizni a tooa helyi fájlt.</span><span class="sxs-lookup"><span data-stu-id="20320-159">hello following example uses hello **DownloadToStream** method tootransfer hello blob contents tooa stream object that you can then persist tooa local file.</span></span>

    // Get a reference tooa blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents tooa file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

<span data-ttu-id="20320-160">Is használhatja a hello **DownloadToStream** metódus toodownload hello tartalmát szöveges karakterláncként blob.</span><span class="sxs-lookup"><span data-stu-id="20320-160">You can also use hello **DownloadToStream** method toodownload hello contents of a blob as a text string.</span></span>

    // Get a reference tooa blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a><span data-ttu-id="20320-161">Blobok törlése</span><span class="sxs-lookup"><span data-stu-id="20320-161">Delete blobs</span></span>
<span data-ttu-id="20320-162">toodelete blob, először kérjen le egy blobhivatkozást, és ezután hívja meg a **törlése** metódust.</span><span class="sxs-lookup"><span data-stu-id="20320-162">toodelete a blob, first get a blob reference and then call the **Delete** method.</span></span>

    // Get a reference tooa blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete hello blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a><span data-ttu-id="20320-163">Blobok listázása több oldalon aszinkron módon</span><span class="sxs-lookup"><span data-stu-id="20320-163">List blobs in pages asynchronously</span></span>
<span data-ttu-id="20320-164">Ha nagyszámú BLOB listázásakor, vagy azt szeretné, hogy egy listázási művelet eredmények toocontrol hello száma, blobok megjelenített eredményoldalra is listázhatja.</span><span class="sxs-lookup"><span data-stu-id="20320-164">If you are listing a large number of blobs, or you want toocontrol hello number of results you return in one listing operation, you can list blobs in pages of results.</span></span> <span data-ttu-id="20320-165">Ez a példa bemutatja, hogyan tooreturn eredményezi lapok aszinkron módon történik, így végrehajtása ne tiltsa le a tooreturn eredmények számos várakozás során.</span><span class="sxs-lookup"><span data-stu-id="20320-165">This example shows how tooreturn results in pages asynchronously, so that execution is not blocked while waiting tooreturn a large set of results.</span></span>

<span data-ttu-id="20320-166">Ez a példa bemutatja egy egyszerű blob listázása, de hierarchikus listát is végrehajthatók hello beállítása **Listblobs** hello paramétere **ListBlobsSegmentedAsync** metódus túl **hamis**.</span><span class="sxs-lookup"><span data-stu-id="20320-166">This example shows a flat blob listing, but you can also perform a hierarchical listing, by setting hello **useFlatBlobListing** parameter of hello **ListBlobsSegmentedAsync** method too**false**.</span></span>

<span data-ttu-id="20320-167">Hello minta metódushívások egy aszinkron metódus, mert azt kell lennie végrehajtásával kerüli meg a hello **aszinkron** kulcsszót, és azt kell visszaadnia egy **feladat** objektum.</span><span class="sxs-lookup"><span data-stu-id="20320-167">Because hello sample method calls an asynchronous method, it must be prefaced with hello **async** keyword, and it must return a **Task** object.</span></span> <span data-ttu-id="20320-168">hello await kulcsszó hello megadott **ListBlobsSegmentedAsync** metódus hello minta metódus végrehajtásának felfüggesztése, hello listázási feladat befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="20320-168">hello await keyword specified for hello **ListBlobsSegmentedAsync** method suspends execution of hello sample method until hello listing task completes.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="20320-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20320-169">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-blobs-next-steps](../../includes/vs-storage-dotnet-blobs-next-steps.md)]


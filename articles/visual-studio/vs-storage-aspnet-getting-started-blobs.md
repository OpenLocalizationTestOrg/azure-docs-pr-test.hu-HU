---
title: "az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) aaaGet elsajátítása |} Microsoft Docs"
description: "Hogyan tooget el az Azure blob storage használatával egy ASP.NET-projekt, a Visual Studio használó Visual Studio kapcsolódó szolgáltatások tooa tárfiókot kapcsolódás után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraig
ms.openlocfilehash: 7b3e160da5bb95967ca4650b124afb8e867c03d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="96639-103">Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="96639-103">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a><span data-ttu-id="96639-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="96639-104">Overview</span></span>

<span data-ttu-id="96639-105">Az Azure blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja.</span><span class="sxs-lookup"><span data-stu-id="96639-105">Azure blob storage is a service that stores unstructured data in hello cloud as objects/blobs.</span></span> <span data-ttu-id="96639-106">A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt.</span><span class="sxs-lookup"><span data-stu-id="96639-106">Blob storage can store any type of text or binary data, such as a document, media file, or application installer.</span></span> <span data-ttu-id="96639-107">A BLOB storage is az említett tooas objektum tároló.</span><span class="sxs-lookup"><span data-stu-id="96639-107">Blob storage is also referred tooas object storage.</span></span>

<span data-ttu-id="96639-108">Ez az oktatóanyag bemutatja, hogyan toowrite ASP.NET helykódja olyan gyakori forgatókönyveket tartalmaz, az Azure blob storage használatával.</span><span class="sxs-lookup"><span data-stu-id="96639-108">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure blob storage.</span></span> <span data-ttu-id="96639-109">Forgatókönyvek például a blob tároló, létrehozása és feltöltése, listázása, letöltése és blobok törlése.</span><span class="sxs-lookup"><span data-stu-id="96639-109">Scenarios include creating a blob container, and uploading, listing, downloading, and deleting blobs.</span></span>

##<a name="prerequisites"></a><span data-ttu-id="96639-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="96639-110">Prerequisites</span></span>

* [<span data-ttu-id="96639-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="96639-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="96639-112">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="96639-112">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="96639-113">Hozzon létre az MVC-vezérlő</span><span class="sxs-lookup"><span data-stu-id="96639-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="96639-114">A hello **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és hello helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.</span><span class="sxs-lookup"><span data-stu-id="96639-114">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![A vezérlő tooan ASP.NET MVC alkalmazás hozzáadása](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. <span data-ttu-id="96639-116">A hello **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="96639-116">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. <span data-ttu-id="96639-118">A hello **vezérlő hozzáadása** párbeszédpanelen neve hello vezérlő *BlobsController*, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="96639-118">On hello **Add Controller** dialog, name hello controller *BlobsController*, and select **Add**.</span></span>

    ![Hello MVC-vezérlő neve](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. <span data-ttu-id="96639-120">Adja hozzá a következő hello *használatával* irányelvek toohello `BlobsController.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="96639-120">Add hello following *using* directives toohello `BlobsController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a><span data-ttu-id="96639-121">A blob-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="96639-121">Create a blob container</span></span>

<span data-ttu-id="96639-122">Egy blob-tároló blobokat és mappák beágyazott hierarchiája.</span><span class="sxs-lookup"><span data-stu-id="96639-122">A blob container is a nested hierarchy of blobs and folders.</span></span> <span data-ttu-id="96639-123">hello következő lépések bemutatják, hogyan toocreate egy blob-tároló:</span><span class="sxs-lookup"><span data-stu-id="96639-123">hello following steps illustrate how toocreate a blob container:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="96639-124">hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="96639-124">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="96639-125">Nyissa meg hello `BlobsController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="96639-125">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="96639-126">Adja hozzá a hívott metódus **CreateBlobContainer** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="96639-126">Add a method called **CreateBlobContainer** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="96639-127">Hello belül **CreateBlobContainer** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-127">Within hello **CreateBlobContainer** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="96639-128">Kód tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait következő hello Azure szolgáltatáskonfiguráció hello használata.</span><span class="sxs-lookup"><span data-stu-id="96639-128">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration.</span></span> <span data-ttu-id="96639-129">(Változás  *&lt;-tárfióknév >* toohello hello éri el az Azure storage-fiók nevét.)</span><span class="sxs-lookup"><span data-stu-id="96639-129">(Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="96639-130">Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-130">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="96639-131">Első egy **CloudBlobContainer** egy hivatkozás toohello kívánt blobtároló neve képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="96639-131">Get a **CloudBlobContainer** object that represents a reference toohello desired blob container name.</span></span> <span data-ttu-id="96639-132">Hello **CloudBlobClient.GetContainerReference** metódus nem tesz egy kérelmet a blob storage.</span><span class="sxs-lookup"><span data-stu-id="96639-132">hello **CloudBlobClient.GetContainerReference** method does not make a request against blob storage.</span></span> <span data-ttu-id="96639-133">hello hivatkozás hello blob-tároló létezik-e adja vissza.</span><span class="sxs-lookup"><span data-stu-id="96639-133">hello reference is returned whether or not hello blob container exists.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="96639-134">Hello hívás **CloudBlobContainer.CreateIfNotExists** metódus toocreate hello tárolót, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="96639-134">Call hello **CloudBlobContainer.CreateIfNotExists** method toocreate hello container if it does not yet exist.</span></span> <span data-ttu-id="96639-135">Hello **CloudBlobContainer.CreateIfNotExists** metódus beolvasása **igaz** Ha hello tároló nem létezik, és sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="96639-135">hello **CloudBlobContainer.CreateIfNotExists** method returns **true** if hello container does not exist, and is successfully created.</span></span> <span data-ttu-id="96639-136">Ellenkező esetben **hamis** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="96639-136">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. <span data-ttu-id="96639-137">Frissítés hello **ViewBag** hello nevű hello blob tároló.</span><span class="sxs-lookup"><span data-stu-id="96639-137">Update hello **ViewBag** with hello name of hello blob container.</span></span>

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. <span data-ttu-id="96639-138">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **Blobok**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="96639-138">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="96639-139">A hello **nézet hozzáadása** párbeszédpanelen adja meg **CreateBlobContainer** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="96639-139">On hello **Add View** dialog, enter **CreateBlobContainer** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="96639-140">Nyissa meg `CreateBlobContainer.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="96639-140">Open `CreateBlobContainer.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="96639-141">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="96639-141">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="96639-142">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="96639-142">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. <span data-ttu-id="96639-143">Hello alkalmazás futtatásához, és válassza ki **Blob-tároló létrehozása** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="96639-143">Run hello application, and select **Create Blob Container** toosee results similar toohello following screen shot:</span></span>
  
    ![A blob-tároló létrehozása](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    <span data-ttu-id="96639-145">Ahogy korábban említettük hello **CloudBlobContainer.CreateIfNotExists** metódus beolvasása **igaz** csak hello tároló nem létezik és jön létre.</span><span class="sxs-lookup"><span data-stu-id="96639-145">As mentioned previously, hello **CloudBlobContainer.CreateIfNotExists** method returns **true** only when hello container doesn't exist and is created.</span></span> <span data-ttu-id="96639-146">Ezért, ha futtatja a hello app hello tároló létezik, hello metódus visszaadja **hamis**.</span><span class="sxs-lookup"><span data-stu-id="96639-146">Therefore, if you run hello app when hello container exists, hello method returns **false**.</span></span> <span data-ttu-id="96639-147">toorun hello alkalmazás többször, törölnie kell hello tároló hello app ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="96639-147">toorun hello app multiple times, you must delete hello container before running hello app again.</span></span> <span data-ttu-id="96639-148">Törlése hello tároló megteheti a hello **CloudBlobContainer.Delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-148">Deleting hello container can be done via hello **CloudBlobContainer.Delete** method.</span></span> <span data-ttu-id="96639-149">Hello tároló hello használatával törölheti is [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="96639-149">You can also delete hello container using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="upload-a-blob-into-a-blob-container"></a><span data-ttu-id="96639-150">Egy blob feltöltése a blob-tárolóba</span><span class="sxs-lookup"><span data-stu-id="96639-150">Upload a blob into a blob container</span></span>

<span data-ttu-id="96639-151">Miután megismerte [létrejött a blob-tároló](#create-a-blob-container), feltöltheti a fájlokat, hogy tárolóba.</span><span class="sxs-lookup"><span data-stu-id="96639-151">Once you've [created a blob container](#create-a-blob-container), you can upload files into that container.</span></span> <span data-ttu-id="96639-152">Ez a szakasz végigvezeti a helyi fájl tooa blobtárolóban feltöltése.</span><span class="sxs-lookup"><span data-stu-id="96639-152">This section walks you through uploading a local file tooa blob container.</span></span> <span data-ttu-id="96639-153">hello lépések azt feltételezik, hogy létrehozta a következő nevű blobtárolóban *-blob-tároló*.</span><span class="sxs-lookup"><span data-stu-id="96639-153">hello steps assume you've created a blob container named *test-blob-container*.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="96639-154">hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="96639-154">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="96639-155">Nyissa meg hello `BlobsController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="96639-155">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="96639-156">Adja hozzá a hívott metódus **UploadBlob** , amely visszaadja az **EmptyResult**.</span><span class="sxs-lookup"><span data-stu-id="96639-156">Add a method called **UploadBlob** that returns an **EmptyResult**.</span></span>

    ```csharp
    public EmptyResult UploadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="96639-157">Hello belül **UploadBlob** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-157">Within hello **UploadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="96639-158">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="96639-158">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="96639-159">Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-159">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="96639-160">Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="96639-160">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="96639-161">Ahogy korábban, az Azure storage támogatja különböző blob típusok.</span><span class="sxs-lookup"><span data-stu-id="96639-161">As explained earlier, Azure storage supports different blob types.</span></span> <span data-ttu-id="96639-162">tooretrieve hivatkozás tooa oldalakra vonatkozó blob, hívás hello **CloudBlobContainer.GetPageBlobReference** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-162">tooretrieve a reference tooa page blob, call hello **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="96639-163">egy hivatkozási tooa blokkblob tooretrieve hívás hello **CloudBlobContainer.GetBlockBlobReference** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-163">tooretrieve a reference tooa block blob, call hello **CloudBlobContainer.GetBlockBlobReference** method.</span></span> <span data-ttu-id="96639-164">Általában a blokkblob típusú toouse ajánlott hello jelenti.</span><span class="sxs-lookup"><span data-stu-id="96639-164">Usually, block blob is hello recommended type toouse.</span></span> <span data-ttu-id="96639-165">(< Blob-név > módosítása * toogive hello blob egyszer feltölteni kívánt toohello nevet.)</span><span class="sxs-lookup"><span data-stu-id="96639-165">(Change <blob-name>* toohello name you want toogive hello blob once uploaded.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="96639-166">Miután egy blobhivatkozást, bármely adatok adatfolyam tooit feltöltheti hello blob hivatkozási objektumot meghívásával **UploadFromStream** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-166">Once you have a blob reference, you can upload any data stream tooit by calling hello blob reference object's **UploadFromStream** method.</span></span> <span data-ttu-id="96639-167">Hello **UploadFromStream** módszer hello blob hoz létre, ha nem létezik, vagy felülírja, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="96639-167">hello **UploadFromStream** method creates hello blob if it doesn't exist, or overwrites it if it does exist.</span></span> <span data-ttu-id="96639-168">(Változás  *&lt;fájlfeltöltés >* tooa teljesen minősített elérési útja toohello fájlt tooupload.)</span><span class="sxs-lookup"><span data-stu-id="96639-168">(Change *&lt;file-to-upload>* tooa fully qualified path toohello file you want tooupload.)</span></span>

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. <span data-ttu-id="96639-169">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **Blobok**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="96639-169">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="96639-170">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="96639-170">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="96639-171">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="96639-171">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="96639-172">Hello alkalmazás futtatásához, és válassza ki **feltöltése a blob**.</span><span class="sxs-lookup"><span data-stu-id="96639-172">Run hello application, and select **Upload blob**.</span></span>  
  
<span data-ttu-id="96639-173">a szakasz - hello [egy blob-tároló hello blobok listázása](#list-the-blobs-in-a-blob-container) -bemutatja, hogyan toolist hello a blob-tárolóban lévő blobok.</span><span class="sxs-lookup"><span data-stu-id="96639-173">hello section - [List hello blobs in a blob container](#list-the-blobs-in-a-blob-container) - illustrates how toolist hello blobs in a blob container.</span></span>  

## <a name="list-hello-blobs-in-a-blob-container"></a><span data-ttu-id="96639-174">Lista hello blobot, amely egy blob-tároló</span><span class="sxs-lookup"><span data-stu-id="96639-174">List hello blobs in a blob container</span></span>

<span data-ttu-id="96639-175">Ez a szakasz bemutatja, hogyan toolist hello a blob-tárolóban lévő blobok.</span><span class="sxs-lookup"><span data-stu-id="96639-175">This section illustrates how toolist hello blobs in a blob container.</span></span> <span data-ttu-id="96639-176">hello mintakód hivatkozik hello *-blob-tároló* hello szakaszban létrehozott [blob tárolókat hozhat létre](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="96639-176">hello sample code references hello *test-blob-container* created in hello section, [Create a blob container](#create-a-blob-container).</span></span>

> [!NOTE]
> 
> <span data-ttu-id="96639-177">hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="96639-177">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="96639-178">Nyissa meg hello `BlobsController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="96639-178">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="96639-179">Adja hozzá a hívott metódus **ListBlobs** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="96639-179">Add a method called **ListBlobs** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult ListBlobs()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. <span data-ttu-id="96639-180">Hello belül **ListBlobs** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-180">Within hello **ListBlobs** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="96639-181">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="96639-181">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="96639-182">Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-182">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="96639-183">Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="96639-183">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="96639-184">toolist hello blobot, amely egy blob tároló használatára hello **CloudBlobContainer.ListBlobs** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-184">toolist hello blobs in a blob container, use hello **CloudBlobContainer.ListBlobs** method.</span></span> <span data-ttu-id="96639-185">Hello **CloudBlobContainer.ListBlobs** metódus értéket ad vissza egy **IListBlobItem** objektumot, hogy tooa adatoszlopon **CloudBlockBlob**, **CloudPageBlob**, vagy **CloudBlobDirectory** objektum.</span><span class="sxs-lookup"><span data-stu-id="96639-185">hello **CloudBlobContainer.ListBlobs** method returns an **IListBlobItem** object that you cast tooa **CloudBlockBlob**, **CloudPageBlob**, or **CloudBlobDirectory** object.</span></span> <span data-ttu-id="96639-186">hello következő kódrészletet enumerálása valamennyi hello BLOB egy blob-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="96639-186">hello following code snippet enumerates all hello blobs in a blob container.</span></span> <span data-ttu-id="96639-187">Minden egyes blob típuskonverzió toohello megfelelő objektum típusa és a neve alapján (vagy URI hello esetében egy **CloudBlobDirectory**) tooa listán szerepel.</span><span class="sxs-lookup"><span data-stu-id="96639-187">Each blob is cast toohello appropriate object based on its type, and its name (or URI in hello case of a **CloudBlobDirectory**) is added tooa list.</span></span>

    ```csharp
    List<string> blobs = new List<string>();

    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob blob = (CloudPageBlob)item;
            blobs.Add(blob.Name);
        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory dir = (CloudBlobDirectory)item;
            blobs.Add(dir.Uri.ToString());
        }
    }

    return View(blobs);
    ```

    <span data-ttu-id="96639-188">Blob tárolók hozzáadása tooblobs, a könyvtárak tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="96639-188">In addition tooblobs, blob containers can contain directories.</span></span> <span data-ttu-id="96639-189">Most tegyük fel, a következő nevű blobtárolóban *-blob-tároló* a következő hierarchia hello:</span><span class="sxs-lookup"><span data-stu-id="96639-189">Let's suppose you have a blob container called *test-blob-container* with hello following hierarchy:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

    <span data-ttu-id="96639-190">Előző példakódban hello használ, hello **blobok** karakterlánc lista tartalmazza-e értékeket hasonló toohello következő:</span><span class="sxs-lookup"><span data-stu-id="96639-190">Using hello preceding code example, hello **blobs** string list contains values similar toohello following:</span></span>

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    <span data-ttu-id="96639-191">Ahogy látja, hello lista tartalmazza-e csak hello legfelső szintű entitások; nem hello beágyazott azokat (*bar.png* és *baz.png*).</span><span class="sxs-lookup"><span data-stu-id="96639-191">As you can see, hello list includes only hello top-level entities; not hello nested ones (*bar.png* and *baz.png*).</span></span> <span data-ttu-id="96639-192">toolist összes hello belüli egy blob-tároló, meg kell hívnia hello **CloudBlobContainer.ListBlobs** metódus és pass **igaz** a hello **Listblobs** a paraméter.</span><span class="sxs-lookup"><span data-stu-id="96639-192">toolist all hello entities within a blob container, you must call hello **CloudBlobContainer.ListBlobs** method and pass **true** for hello **useFlatBlobListing** parameter.</span></span>    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    <span data-ttu-id="96639-193">A beállítás hello **Listblobs** paraméter túl**igaz** adja vissza egy strukturálatlan lista összes entitások hello blob a tárolóban, és a következő eredmények hello eredményez:</span><span class="sxs-lookup"><span data-stu-id="96639-193">Setting hello **useFlatBlobListing** parameter too**true** returns a flat listing of all entities in hello blob container, and yields hello following results:</span></span>

        foo.png
        dir1/bar.png
        dir2/baz.png

1. <span data-ttu-id="96639-194">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **Blobok**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="96639-194">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Blobs**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="96639-195">A hello **nézet hozzáadása** párbeszédpanelen adja meg **ListBlobs** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="96639-195">On hello **Add View** dialog, enter **ListBlobs** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="96639-196">Nyissa meg `ListBlobs.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="96639-196">Open `ListBlobs.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```html
    @model List<string>
    @{
        ViewBag.Title = "List blobs";
    }
    
    <h2>List blobs</h2>
    
    <ul>
        @foreach (var item in Model)
        {
        <li>@item</li>
        }
    </ul>
    ```

1. <span data-ttu-id="96639-197">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="96639-197">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="96639-198">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="96639-198">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. <span data-ttu-id="96639-199">Hello alkalmazás futtatásához, és válassza ki **blobok listázása** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="96639-199">Run hello application, and select **List blobs** toosee results similar toohello following screen shot:</span></span>
  
    ![A BLOB listázása](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a><span data-ttu-id="96639-201">Blobok letöltése</span><span class="sxs-lookup"><span data-stu-id="96639-201">Download blobs</span></span>

<span data-ttu-id="96639-202">Ez a szakasz bemutatja, hogyan toodownload blob, és továbbra is fennáll, toolocal tároló- és olvasási hello tartalmát egy karakterlánccá egyesít.</span><span class="sxs-lookup"><span data-stu-id="96639-202">This section illustrates how toodownload a blob and either persist it toolocal storage or read hello contents into a string.</span></span> <span data-ttu-id="96639-203">hello mintakód hivatkozik hello *-blob-tároló* hello szakaszban létrehozott [blob tárolókat hozhat létre](#create-a-blob-container).</span><span class="sxs-lookup"><span data-stu-id="96639-203">hello sample code references hello *test-blob-container* created in hello section, [Create a blob container](#create-a-blob-container).</span></span>

1. <span data-ttu-id="96639-204">Nyissa meg hello `BlobsController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="96639-204">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="96639-205">Adja hozzá a hívott metódus **DownloadBlob** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="96639-205">Add a method called **DownloadBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. <span data-ttu-id="96639-206">Hello belül **DownloadBlob** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-206">Within hello **DownloadBlob** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="96639-207">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="96639-207">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="96639-208">Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-208">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="96639-209">Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="96639-209">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="96639-210">Egy blob referenciaobjektum beolvasása meghívásával **CloudBlobContainer.GetBlockBlobReference** vagy **CloudBlobContainer.GetPageBlobReference** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-210">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="96639-211">(Változás  *&lt;blob-neve >* toohello neve hello blob tölti le.)</span><span class="sxs-lookup"><span data-stu-id="96639-211">(Change *&lt;blob-name>* toohello name of hello blob you are downloading.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. <span data-ttu-id="96639-212">egy blob toodownload hello használata **CloudBlockBlob.DownloadToStream** vagy **CloudPageBlob.DownloadToStream** metódus hello blob típusától függően.</span><span class="sxs-lookup"><span data-stu-id="96639-212">toodownload a blob, use hello **CloudBlockBlob.DownloadToStream** or **CloudPageBlob.DownloadToStream** method, depending on hello blob type.</span></span> <span data-ttu-id="96639-213">hello következő kódrészletet használja hello **CloudBlockBlob.DownloadToStream** metódus tootransfer a blob tartalmát tooa adatfolyam objektum, amely majd megőrzött tooa helyi fájl: (változás  *&lt;helyi fájlnév >* toohello teljesen minősített fájl nevét képviselő letöltött hello blob, ahová.)</span><span class="sxs-lookup"><span data-stu-id="96639-213">hello following code snippet uses hello **CloudBlockBlob.DownloadToStream** method tootransfer a blob's contents tooa stream object that is then persisted tooa local file: (Change *&lt;local-file-name>* toohello fully qualified file name representing where you want hello blob downloaded.)</span></span> 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. <span data-ttu-id="96639-214">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="96639-214">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="96639-215">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="96639-215">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="96639-216">Hello alkalmazás futtatásához, és válassza ki **letöltési blob** toodownload hello blob.</span><span class="sxs-lookup"><span data-stu-id="96639-216">Run hello application, and select **Download blob** toodownload hello blob.</span></span> <span data-ttu-id="96639-217">hello megadott hello blob **CloudBlobContainer.GetBlockBlobReference** metódus hívása letölti toohello helyre a hello **File.OpenWrite** metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="96639-217">hello blob specified in hello **CloudBlobContainer.GetBlockBlobReference** method call downloads toohello location you specify in hello **File.OpenWrite** method call.</span></span> 

## <a name="delete-blobs"></a><span data-ttu-id="96639-218">Blobok törlése</span><span class="sxs-lookup"><span data-stu-id="96639-218">Delete blobs</span></span>

<span data-ttu-id="96639-219">hello következő lépések bemutatják, hogyan toodelete blob:</span><span class="sxs-lookup"><span data-stu-id="96639-219">hello following steps illustrate how toodelete a blob:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="96639-220">hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="96639-220">hello code in this section assumes that you have completed hello steps in hello section, [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="96639-221">Nyissa meg hello `BlobsController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="96639-221">Open hello `BlobsController.cs` file.</span></span>

1. <span data-ttu-id="96639-222">Adja hozzá a hívott metódus **DeleteBlob** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="96639-222">Add a method called **DeleteBlob** that returns an **ActionResult**.</span></span>

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```

1. <span data-ttu-id="96639-223">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-223">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="96639-224">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="96639-224">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. <span data-ttu-id="96639-225">Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.</span><span class="sxs-lookup"><span data-stu-id="96639-225">Get a **CloudBlobClient** object represents a blob service client.</span></span>
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. <span data-ttu-id="96639-226">Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="96639-226">Get a **CloudBlobContainer** object that represents a reference toohello blob container name.</span></span> 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. <span data-ttu-id="96639-227">Egy blob referenciaobjektum beolvasása meghívásával **CloudBlobContainer.GetBlockBlobReference** vagy **CloudBlobContainer.GetPageBlobReference** metódust.</span><span class="sxs-lookup"><span data-stu-id="96639-227">Get a blob reference object by calling **CloudBlobContainer.GetBlockBlobReference** or **CloudBlobContainer.GetPageBlobReference** method.</span></span> <span data-ttu-id="96639-228">(Változás  *&lt;blob-neve >* toohello neve hello blob törli.)</span><span class="sxs-lookup"><span data-stu-id="96639-228">(Change *&lt;blob-name>* toohello name of hello blob you are deleting.)</span></span>

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. toodelete a blob, use hello **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. <span data-ttu-id="96639-229">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="96639-229">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="96639-230">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="96639-230">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. <span data-ttu-id="96639-231">Hello alkalmazás futtatásához, és válassza ki **Delete blob** toodelete hello blob hello megadott **CloudBlobContainer.GetBlockBlobReference** metódus hívása.</span><span class="sxs-lookup"><span data-stu-id="96639-231">Run hello application, and select **Delete blob** toodelete hello blob specified in hello **CloudBlobContainer.GetBlockBlobReference** method call.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="96639-232">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="96639-232">Next steps</span></span>
<span data-ttu-id="96639-233">Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.</span><span class="sxs-lookup"><span data-stu-id="96639-233">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="96639-234">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="96639-234">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-tables.md)
  * [<span data-ttu-id="96639-235">Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="96639-235">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](vs-storage-aspnet-getting-started-queues.md)

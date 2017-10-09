---
title: "az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) aaaGet elsajátítása |} Microsoft Docs"
description: "Hogyan tooget el az Azure blob storage használatával egy ASP.NET-projekt, a Visual Studio használó Visual Studio kapcsolódó szolgáltatások tooa tárfiókot kapcsolódás után"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: b3497055-bef8-4c95-8567-181556b50d95
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: tarcher
ms.openlocfilehash: eb38889f239a63852d6928e8be10c3d3f1746e9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-blob-storage-and-visual-studio-connected-services-aspnet"></a>Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)
[!INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>Áttekintés

Az Azure blob storage egy olyan szolgáltatás, amely hello felhő strukturálatlan adatokat objektumként/blobként tárolja. A Blob Storage képes tárolni bármilyen szöveget vagy bináris adatot, például dokumentumot, médiafájlt vagy egy alkalmazástelepítőt. A BLOB storage is az említett tooas objektum tároló.

Ez az oktatóanyag bemutatja, hogyan toowrite ASP.NET helykódja olyan gyakori forgatókönyveket tartalmaz, az Azure blob storage használatával. Forgatókönyvek például a blob tároló, létrehozása és feltöltése, listázása, letöltése és blobok törlése.

##<a name="prerequisites"></a>Előfeltételek

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-fiók](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Hozzon létre az MVC-vezérlő 

1. A hello **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és hello helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.

    ![A vezérlő tooan ASP.NET MVC alkalmazás hozzáadása](./media/vs-storage-aspnet-getting-started-blobs/add-controller-menu.png)

1. A hello **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-blobs/add-controller.png)

1. A hello **vezérlő hozzáadása** párbeszédpanelen neve hello vezérlő *BlobsController*, és válassza ki **Hozzáadás**.

    ![Hello MVC-vezérlő neve](./media/vs-storage-aspnet-getting-started-blobs/add-controller-name.png)

1. Adja hozzá a következő hello *használatával* irányelvek toohello `BlobsController.cs` fájlt:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

## <a name="create-a-blob-container"></a>A blob-tároló létrehozása

Egy blob-tároló blobokat és mappák beágyazott hierarchiája. hello következő lépések bemutatják, hogyan toocreate egy blob-tároló:

> [!NOTE]
> 
> hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `BlobsController.cs` fájlt.

1. Adja hozzá a hívott metódus **CreateBlobContainer** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult CreateBlobContainer()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **CreateBlobContainer** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Kód tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait következő hello Azure szolgáltatáskonfiguráció hello használata. (Változás  *&lt;-tárfióknév >* toohello hello éri el az Azure storage-fiók nevét.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Első egy **CloudBlobContainer** egy hivatkozás toohello kívánt blobtároló neve képviselő objektum. Hello **CloudBlobClient.GetContainerReference** metódus nem tesz egy kérelmet a blob storage. hello hivatkozás hello blob-tároló létezik-e adja vissza. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Hello hívás **CloudBlobContainer.CreateIfNotExists** metódus toocreate hello tárolót, ha még nem létezik. Hello **CloudBlobContainer.CreateIfNotExists** metódus beolvasása **igaz** Ha hello tároló nem létezik, és sikeresen létrejött. Ellenkező esetben **hamis** adja vissza.    

    ```csharp
    ViewBag.Success = container.CreateIfNotExists();
    ```

1. Frissítés hello **ViewBag** hello nevű hello blob tároló.

    ```csharp
    ViewBag.BlobContainerName = container.Name;
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **Blobok**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **CreateBlobContainer** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `CreateBlobContainer.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "Create Blob Container";
    }
    
    <h2>Create Blob Container results</h2>

    Creation of @ViewBag.BlobContainerName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create blob container", "CreateBlobContainer", "Blobs")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **Blob-tároló létrehozása** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![A blob-tároló létrehozása](./media/vs-storage-aspnet-getting-started-blobs/create-blob-container-results.png)

    Ahogy korábban említettük hello **CloudBlobContainer.CreateIfNotExists** metódus beolvasása **igaz** csak hello tároló nem létezik és jön létre. Ezért, ha futtatja a hello app hello tároló létezik, hello metódus visszaadja **hamis**. toorun hello alkalmazás többször, törölnie kell hello tároló hello app ismételt futtatása előtt. Törlése hello tároló megteheti a hello **CloudBlobContainer.Delete** metódust. Hello tároló hello használatával törölheti is [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="upload-a-blob-into-a-blob-container"></a>Egy blob feltöltése a blob-tárolóba

Miután megismerte [létrejött a blob-tároló](#create-a-blob-container), feltöltheti a fájlokat, hogy tárolóba. Ez a szakasz végigvezeti a helyi fájl tooa blobtárolóban feltöltése. hello lépések azt feltételezik, hogy létrehozta a következő nevű blobtárolóban *-blob-tároló*. 

> [!NOTE]
> 
> hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `BlobsController.cs` fájlt.

1. Adja hozzá a hívott metódus **UploadBlob** , amely visszaadja az **EmptyResult**.

    ```csharp
    public EmptyResult UploadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Hello belül **UploadBlob** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Ahogy korábban, az Azure storage támogatja különböző blob típusok. tooretrieve hivatkozás tooa oldalakra vonatkozó blob, hívás hello **CloudBlobContainer.GetPageBlobReference** metódust. egy hivatkozási tooa blokkblob tooretrieve hívás hello **CloudBlobContainer.GetBlockBlobReference** metódust. Általában a blokkblob típusú toouse ajánlott hello jelenti. (< Blob-név > módosítása * toogive hello blob egyszer feltölteni kívánt toohello nevet.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. Miután egy blobhivatkozást, bármely adatok adatfolyam tooit feltöltheti hello blob hivatkozási objektumot meghívásával **UploadFromStream** metódust. Hello **UploadFromStream** módszer hello blob hoz létre, ha nem létezik, vagy felülírja, ha már létezik. (Változás  *&lt;fájlfeltöltés >* tooa teljesen minősített elérési útja toohello fájlt tooupload.)

    ```csharp
    using (var fileStream = System.IO.File.OpenRead(<file-to-upload>))
    {
        blob.UploadFromStream(fileStream);
    }
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **Blobok**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Upload blob", "UploadBlob", "Blobs")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **feltöltése a blob**.  
  
a szakasz - hello [egy blob-tároló hello blobok listázása](#list-the-blobs-in-a-blob-container) -bemutatja, hogyan toolist hello a blob-tárolóban lévő blobok.  

## <a name="list-hello-blobs-in-a-blob-container"></a>Lista hello blobot, amely egy blob-tároló

Ez a szakasz bemutatja, hogyan toolist hello a blob-tárolóban lévő blobok. hello mintakód hivatkozik hello *-blob-tároló* hello szakaszban létrehozott [blob tárolókat hozhat létre](#create-a-blob-container).

> [!NOTE]
> 
> hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `BlobsController.cs` fájlt.

1. Adja hozzá a hívott metódus **ListBlobs** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult ListBlobs()
    {
        // hello code in this section goes here.

        return View();
    }
    ```
 
1. Hello belül **ListBlobs** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. toolist hello blobot, amely egy blob tároló használatára hello **CloudBlobContainer.ListBlobs** metódust. Hello **CloudBlobContainer.ListBlobs** metódus értéket ad vissza egy **IListBlobItem** objektumot, hogy tooa adatoszlopon **CloudBlockBlob**, **CloudPageBlob**, vagy **CloudBlobDirectory** objektum. hello következő kódrészletet enumerálása valamennyi hello BLOB egy blob-tárolóban. Minden egyes blob típuskonverzió toohello megfelelő objektum típusa és a neve alapján (vagy URI hello esetében egy **CloudBlobDirectory**) tooa listán szerepel.

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

    Blob tárolók hozzáadása tooblobs, a könyvtárak tartalmazhat. Most tegyük fel, a következő nevű blobtárolóban *-blob-tároló* a következő hierarchia hello:

        foo.png
        dir1/bar.png
        dir2/baz.png

    Előző példakódban hello használ, hello **blobok** karakterlánc lista tartalmazza-e értékeket hasonló toohello következő:

        foo.png
        <storage-account-url>/test-blob-container/dir1
        <storage-account-url>/test-blob-container/dir2

    Ahogy látja, hello lista tartalmazza-e csak hello legfelső szintű entitások; nem hello beágyazott azokat (*bar.png* és *baz.png*). toolist összes hello belüli egy blob-tároló, meg kell hívnia hello **CloudBlobContainer.ListBlobs** metódus és pass **igaz** a hello **Listblobs** a paraméter.    

    ```csharp
    ...
    foreach (IListBlobItem item in container.ListBlobs(useFlatBlobListing:true))
    ...
    ```

    A beállítás hello **Listblobs** paraméter túl**igaz** adja vissza egy strukturálatlan lista összes entitások hello blob a tárolóban, és a következő eredmények hello eredményez:

        foo.png
        dir1/bar.png
        dir2/baz.png

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **Blobok**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **ListBlobs** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `ListBlobs.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

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

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("List blobs", "ListBlobs", "Blobs")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **blobok listázása** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![A BLOB listázása](./media/vs-storage-aspnet-getting-started-blobs/listblobs.png)

## <a name="download-blobs"></a>Blobok letöltése

Ez a szakasz bemutatja, hogyan toodownload blob, és továbbra is fennáll, toolocal tároló- és olvasási hello tartalmát egy karakterlánccá egyesít. hello mintakód hivatkozik hello *-blob-tároló* hello szakaszban létrehozott [blob tárolókat hozhat létre](#create-a-blob-container).

1. Nyissa meg hello `BlobsController.cs` fájlt.

1. Adja hozzá a hívott metódus **DownloadBlob** , amely visszaadja az **ActionResult**.

    ```csharp
    public EmptyResult DownloadBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```
 
1. Hello belül **DownloadBlob** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Egy blob referenciaobjektum beolvasása meghívásával **CloudBlobContainer.GetBlockBlobReference** vagy **CloudBlobContainer.GetPageBlobReference** metódust. (Változás  *&lt;blob-neve >* toohello neve hello blob tölti le.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
    ```

1. egy blob toodownload hello használata **CloudBlockBlob.DownloadToStream** vagy **CloudPageBlob.DownloadToStream** metódus hello blob típusától függően. hello következő kódrészletet használja hello **CloudBlockBlob.DownloadToStream** metódus tootransfer a blob tartalmát tooa adatfolyam objektum, amely majd megőrzött tooa helyi fájl: (változás  *&lt;helyi fájlnév >* toohello teljesen minősített fájl nevét képviselő letöltött hello blob, ahová.) 

    ```csharp
    using (var fileStream = System.IO.File.OpenWrite(<local-file-name>))
    {
        blob.DownloadToStream(fileStream);
    }
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Download blob", "DownloadBlob", "Blobs")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **letöltési blob** toodownload hello blob. hello megadott hello blob **CloudBlobContainer.GetBlockBlobReference** metódus hívása letölti toohello helyre a hello **File.OpenWrite** metódus hívása. 

## <a name="delete-blobs"></a>Blobok törlése

hello következő lépések bemutatják, hogyan toodelete blob:

> [!NOTE]
> 
> hello ebben a szakaszban kód azt feltételezi, hogy végrehajtotta hello hello területen [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `BlobsController.cs` fájlt.

1. Adja hozzá a hívott metódus **DeleteBlob** , amely visszaadja az **ActionResult**.

    ```csharp
    public EmptyResult DeleteBlob()
    {
        // hello code in this section goes here.

        return new EmptyResult();
    }
    ```

1. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```
   
1. Első egy **CloudBlobClient** objektum egy blob szolgáltatásügyfél jelöli.
   
    ```csharp
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
    ```

1. Első egy **CloudBlobContainer** egy hivatkozás toohello blobtároló neve képviselő objektum. 
   
    ```csharp
    CloudBlobContainer container = blobClient.GetContainerReference("test-blob-container");
    ```

1. Egy blob referenciaobjektum beolvasása meghívásával **CloudBlobContainer.GetBlockBlobReference** vagy **CloudBlobContainer.GetPageBlobReference** metódust. (Változás  *&lt;blob-neve >* toohello neve hello blob törli.)

    ```csharp
    CloudBlockBlob blob = container.GetBlockBlobReference(<blob-name>);
        ```

1. toodelete a blob, use hello **Delete** method.

    ```csharp
    blob.Delete();
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete blob", "DeleteBlob", "Blobs")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **Delete blob** toodelete hello blob hello megadott **CloudBlobContainer.GetBlockBlobReference** metódus hívása. 

## <a name="next-steps"></a>Következő lépések
Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.

  * [Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)](./vs-storage-aspnet-getting-started-tables.md)
  * [Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)](./vs-storage-aspnet-getting-started-queues.md)

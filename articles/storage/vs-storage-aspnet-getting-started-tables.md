---
title: "az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) lépései aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának az Azure table storage egy ASP.NET-projekt, a Visual Studio használó Visual Studio kapcsolódó szolgáltatások tooa tárfiókot kapcsolódás után"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: tarcher
ms.openlocfilehash: e7ed17098c8742954972dc9e1b50eca77221e327
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="9647f-103">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="9647f-103">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="9647f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9647f-104">Overview</span></span>

<span data-ttu-id="9647f-105">Az Azure Table storage lehetővé teszi nagy mennyiségű toostore strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="9647f-105">Azure Table storage enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="9647f-106">hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="9647f-106">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="9647f-107">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="9647f-107">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="9647f-108">Ez az oktatóanyag bemutatja, hogyan toowrite ASP.NET olyan gyakori forgatókönyveket tartalmaz, az Azure table storage entitások használata helykódja.</span><span class="sxs-lookup"><span data-stu-id="9647f-108">This tutorial shows how toowrite ASP.NET code for some common scenarios using Azure table storage entities.</span></span> <span data-ttu-id="9647f-109">Ilyen például, egy tábla létrehozása és hozzáadása, lekérdezése és tábla entitások törlése.</span><span class="sxs-lookup"><span data-stu-id="9647f-109">These scenarios include creating a table, and adding, querying, and deleting table entities.</span></span> 

##<a name="prerequisites"></a><span data-ttu-id="9647f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9647f-110">Prerequisites</span></span>

* [<span data-ttu-id="9647f-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9647f-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="9647f-112">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="9647f-112">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="9647f-113">Hozzon létre az MVC-vezérlő</span><span class="sxs-lookup"><span data-stu-id="9647f-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="9647f-114">A hello **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és hello helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.</span><span class="sxs-lookup"><span data-stu-id="9647f-114">In hello **Solution Explorer**, right-click **Controllers**, and, from hello context menu, select **Add->Controller**.</span></span>

    ![A vezérlő tooan ASP.NET MVC alkalmazás hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. <span data-ttu-id="9647f-116">A hello **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-116">On hello **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. <span data-ttu-id="9647f-118">A hello **vezérlő hozzáadása** párbeszédpanelen neve hello vezérlő *TablesController*, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-118">On hello **Add Controller** dialog, name hello controller *TablesController*, and select **Add**.</span></span>

    ![Hello MVC-vezérlő neve](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. <span data-ttu-id="9647f-120">Adja hozzá a következő hello *használatával* irányelvek toohello `TablesController.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="9647f-120">Add hello following *using* directives toohello `TablesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a><span data-ttu-id="9647f-121">Hozzon létre egy modellosztállyal</span><span class="sxs-lookup"><span data-stu-id="9647f-121">Create a model class</span></span>

<span data-ttu-id="9647f-122">Számos hello példák ezen cikk használja egy **TableEntity**-származtatott osztályt **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="9647f-122">Many of hello examples in this article use a **TableEntity**-derived class called **CustomerEntity**.</span></span> <span data-ttu-id="9647f-123">a lépéseket követve hello végigvezeti Önt az osztályhoz tartozik, mint egy modellosztállyal deklaráló:</span><span class="sxs-lookup"><span data-stu-id="9647f-123">hello following steps guide you through declaring this class as a model class:</span></span>

1. <span data-ttu-id="9647f-124">A hello **Megoldáskezelőben**, kattintson a jobb gombbal **modellek**, és hello helyi menüből válassza ki a **Hozzáadás -> osztály**.</span><span class="sxs-lookup"><span data-stu-id="9647f-124">In hello **Solution Explorer**, right-click **Models**, and, from hello context menu, select **Add->Class**.</span></span>

1. <span data-ttu-id="9647f-125">A hello **új elem hozzáadása** párbeszédpanelen name hello osztály **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="9647f-125">On hello **Add New Item** dialog, name hello class, **CustomerEntity**.</span></span>

1. <span data-ttu-id="9647f-126">Nyissa meg hello `CustomerEntity.cs` fájlt, és adja hozzá a következő hello **használatával** irányelv:</span><span class="sxs-lookup"><span data-stu-id="9647f-126">Open hello `CustomerEntity.cs` file, and add hello following **using** directive:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. <span data-ttu-id="9647f-127">Módosítsa a hello osztály, hogy a befejezésekor hello osztály hasonlóan a következő kód hello van deklarálva.</span><span class="sxs-lookup"><span data-stu-id="9647f-127">Modify hello class so that, when finished, hello class is declared as in hello following code.</span></span> <span data-ttu-id="9647f-128">hello osztály deklarál egy entitásosztályt nevű **CustomerEntity** , hogy használ hello az ügyfél első hello sor kulcsként és vezetékneve hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="9647f-128">hello class declares an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and last name as hello partition key.</span></span>

    ```csharp
    public class CustomerEntity : TableEntity
    {
        public CustomerEntity(string lastName, string firstName)
        {
            this.PartitionKey = lastName;
            this.RowKey = firstName;
        }

        public CustomerEntity() { }

        public string Email { get; set; }
    }
    ```

## <a name="create-a-table"></a><span data-ttu-id="9647f-129">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="9647f-129">Create a table</span></span>

<span data-ttu-id="9647f-130">hello következő lépések bemutatják, hogyan toocreate egy táblázatot:</span><span class="sxs-lookup"><span data-stu-id="9647f-130">hello following steps illustrate how toocreate a table:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="9647f-131">Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="9647f-131">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="9647f-132">Nyissa meg hello `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9647f-132">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="9647f-133">Adja hozzá a hívott metódus **CreateTable** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="9647f-133">Add a method called **CreateTable** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateTable()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="9647f-134">Hello belül **CreateTable** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="9647f-134">Within hello **CreateTable** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9647f-135">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="9647f-135">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="9647f-136">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="9647f-136">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="9647f-137">Első egy **CloudTable** egy hivatkozás toohello kívánt tábla nevét képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-137">Get a **CloudTable** object that represents a reference toohello desired table name.</span></span> <span data-ttu-id="9647f-138">Hello **CloudTableClient.GetTableReference** metódus nem tesz, a table storage egy kérelmet.</span><span class="sxs-lookup"><span data-stu-id="9647f-138">hello **CloudTableClient.GetTableReference** method does not make a request against table storage.</span></span> <span data-ttu-id="9647f-139">hello hivatkozás hello tábla létezik-e adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9647f-139">hello reference is returned whether or not hello table exists.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="9647f-140">Hello hívás **CloudTable.CreateIfNotExists** metódus toocreate hello tábla, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9647f-140">Call hello **CloudTable.CreateIfNotExists** method toocreate hello table if it does not yet exist.</span></span> <span data-ttu-id="9647f-141">Hello **CloudTable.CreateIfNotExists** metódus beolvasása **igaz** Ha hello tábla nem létezik, és sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="9647f-141">hello **CloudTable.CreateIfNotExists** method returns **true** if hello table does not exist, and is successfully created.</span></span> <span data-ttu-id="9647f-142">Ellenkező esetben **hamis** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9647f-142">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. <span data-ttu-id="9647f-143">Frissítés hello **ViewBag** hello nevű hello tábla.</span><span class="sxs-lookup"><span data-stu-id="9647f-143">Update hello **ViewBag** with hello name of hello table.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. <span data-ttu-id="9647f-144">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="9647f-144">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="9647f-145">A hello **nézet hozzáadása** párbeszédpanelen adja meg **CreateTable** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-145">On hello **Add View** dialog, enter **CreateTable** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="9647f-146">Nyissa meg `CreateTable.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="9647f-146">Open `CreateTable.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="9647f-147">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9647f-147">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="9647f-148">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="9647f-148">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. <span data-ttu-id="9647f-149">Hello alkalmazás futtatásához, és válassza ki **tábla létrehozása** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9647f-149">Run hello application, and select **Create table** toosee results similar toohello following screen shot:</span></span>
  
    ![tábla létrehozása](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    <span data-ttu-id="9647f-151">Ahogy korábban említettük hello **CloudTable.CreateIfNotExists** metódus beolvasása **igaz** csak hello tábla nem létezik és jön létre.</span><span class="sxs-lookup"><span data-stu-id="9647f-151">As mentioned previously, hello **CloudTable.CreateIfNotExists** method returns **true** only when hello table doesn't exist and is created.</span></span> <span data-ttu-id="9647f-152">Ezért, ha futtatja a hello app hello tábla létezik, hello metódus visszaadja **hamis**.</span><span class="sxs-lookup"><span data-stu-id="9647f-152">Therefore, if you run hello app when hello table exists, hello method returns **false**.</span></span> <span data-ttu-id="9647f-153">toorun hello alkalmazás többször, törölnie kell hello tábla hello app ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="9647f-153">toorun hello app multiple times, you must delete hello table before running hello app again.</span></span> <span data-ttu-id="9647f-154">Törlése hello tábla megteheti a hello **CloudTable.Delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="9647f-154">Deleting hello table can be done via hello **CloudTable.Delete** method.</span></span> <span data-ttu-id="9647f-155">Hello tábla hello segítségével törölheti is [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="9647f-155">You can also delete hello table using hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="9647f-156">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9647f-156">Add an entity tooa table</span></span>

<span data-ttu-id="9647f-157">*Entitások* tooC leképezése\# származtatott egyéni osztály használatával objektumok **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="9647f-157">*Entities* map tooC\# objects by using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="9647f-158">egy entitás tooa tábla tooadd hozzon létre egy osztályt, amely meghatározza az entitás hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="9647f-158">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="9647f-159">Ebben a szakaszban láthatja, hogyan hello toodefine egy entitásosztályt, amely használja, az ügyfél első hello sor kulcsként és vezetékneve hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="9647f-159">In this section, you'll see how toodefine an entity class that uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="9647f-160">Együtt egy entitás partíció- és sorkulcsa egyértelműen hello entitás hello táblában.</span><span class="sxs-lookup"><span data-stu-id="9647f-160">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="9647f-161">Az azonos partíciókulcsú entitások gyorsabban lekérdezhetők, mint a különböző partíciókulcsúak, de az eltérő partíciókulcsok használata a párhuzamos műveletek nagyobb fokú méretezhetőségét teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="9647f-161">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="9647f-162">Bármely tulajdonság hello table szolgáltatásban tárolni hello tulajdonság egy egyaránt és értékek beolvasása tévő támogatott típus nyilvános tulajdonságának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="9647f-162">For any property that should be stored in hello table service, hello property must be a public property of a supported type that exposes both setting and retrieving values.</span></span>
<span data-ttu-id="9647f-163">hello entitásosztályt *kell* deklarálható nyilvános, paraméter nélküli konstruktora.</span><span class="sxs-lookup"><span data-stu-id="9647f-163">hello entity class *must* declare a public parameter-less constructor.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="9647f-164">Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="9647f-164">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="9647f-165">Nyissa meg hello `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9647f-165">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="9647f-166">Adja hozzá a következő direktíva, amely hello hello lévő kódot úgy hello `TablesController.cs` fájl férhetnek hozzá hello **CustomerEntity** osztály:</span><span class="sxs-lookup"><span data-stu-id="9647f-166">Add hello following directive so that hello code in hello `TablesController.cs` file can access hello **CustomerEntity** class:</span></span>

    ```csharp
    using StorageAspnet.Models;
    ```

1. <span data-ttu-id="9647f-167">Adja hozzá a hívott metódus **AddEntity** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="9647f-167">Add a method called **AddEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="9647f-168">Hello belül **AddEntity** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="9647f-168">Within hello **AddEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9647f-169">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="9647f-169">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="9647f-170">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="9647f-170">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="9647f-171">Első egy **CloudTable** tooadd hello új entitás fog hivatkozás toohello tábla toowhich képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-171">Get a **CloudTable** object that represents a reference toohello table toowhich you are going tooadd hello new entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="9647f-172">Példányt létrehozni és inicializálni a hello **CustomerEntity** osztály.</span><span class="sxs-lookup"><span data-stu-id="9647f-172">Instantiate and initialize hello **CustomerEntity** class.</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. <span data-ttu-id="9647f-173">Hozzon létre egy **TableOperation** objektum, amely beszúrja az ügyfélentitást hello.</span><span class="sxs-lookup"><span data-stu-id="9647f-173">Create a **TableOperation** object that inserts hello customer entity.</span></span>

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. <span data-ttu-id="9647f-174">Hello beszúrási művelet végrehajtása a hívó hello által **CloudTable.Execute** metódust.</span><span class="sxs-lookup"><span data-stu-id="9647f-174">Execute hello insert operation by calling hello **CloudTable.Execute** method.</span></span> <span data-ttu-id="9647f-175">Hello művelet eredménye hello hello vizsgálatával ellenőrizheti **TableResult.HttpStatusCode** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="9647f-175">You can verify hello result of hello operation by inspecting hello **TableResult.HttpStatusCode** property.</span></span> <span data-ttu-id="9647f-176">2xx állapotkódot azt jelzi, hogy hello ügyfél által kért hello művelet feldolgozása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="9647f-176">A status code of 2xx indicates hello action requested by hello client was processed successfully.</span></span> <span data-ttu-id="9647f-177">Az új entitások sikeres Beszúrások például egy HTTP-állapotkód: 204, amely hello művelet feldolgozása sikeresen megtörtént, és hello kiszolgáló nem adott vissza a tartalom a eredményez.</span><span class="sxs-lookup"><span data-stu-id="9647f-177">For example, successful insertions of new entities results in an HTTP status code of 204, meaning that hello operation was successfully processed and hello server did not return any content.</span></span>

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. <span data-ttu-id="9647f-178">Frissítés hello **ViewBag** hello táblanév és hello beszúrási művelet hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="9647f-178">Update hello **ViewBag** with hello table name, and hello results of hello insert operation.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. <span data-ttu-id="9647f-179">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="9647f-179">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="9647f-180">A hello **nézet hozzáadása** párbeszédpanelen adja meg **AddEntity** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-180">On hello **Add View** dialog, enter **AddEntity** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="9647f-181">Nyissa meg `AddEntity.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="9647f-181">Open `AddEntity.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. <span data-ttu-id="9647f-182">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9647f-182">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="9647f-183">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="9647f-183">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. <span data-ttu-id="9647f-184">Hello alkalmazás futtatásához, és válassza ki **entitás** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9647f-184">Run hello application, and select **Add entity** toosee results similar toohello following screen shot:</span></span>
  
    ![Entitás hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    <span data-ttu-id="9647f-186">Hello entitás hozzáadásának hello területen hello lépéseket követve ellenőrizheti [beolvasása egyetlen entitás](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="9647f-186">You can verify that hello entity was added by following hello steps in hello section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="9647f-187">Is használhatja a hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview összes hello entitások a táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="9647f-187">You can also use hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview all hello entities for your tables.</span></span>

## <a name="add-a-batch-of-entities-tooa-table"></a><span data-ttu-id="9647f-188">Egy entitás tooa tábla kötegelt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9647f-188">Add a batch of entities tooa table</span></span>

<span data-ttu-id="9647f-189">Továbbá toobeing képes a túl[egy entitás tooa tábla egyik felvenni egy időben](#add-an-entity-to-a-table), azt is megteheti entitások kötegben.</span><span class="sxs-lookup"><span data-stu-id="9647f-189">In addition toobeing able too[add an entity tooa table one at a time](#add-an-entity-to-a-table), you can also add entities in batch.</span></span> <span data-ttu-id="9647f-190">Entitás hozzáadása a kötegelt csökkenti az üzenetváltások a kódot és az Azure table szolgáltatás hello közötti hello számát.</span><span class="sxs-lookup"><span data-stu-id="9647f-190">Adding entities in batch reduces hello number of round-trips between your code and hello Azure table service.</span></span> <span data-ttu-id="9647f-191">hello lépések bemutatják, hogyan tooadd több entitások tooa tábla egy egyetlen insert művelet:</span><span class="sxs-lookup"><span data-stu-id="9647f-191">hello following steps illustrate how tooadd multiple entities tooa table with a single insert operation:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="9647f-192">Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="9647f-192">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="9647f-193">Nyissa meg hello `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9647f-193">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="9647f-194">Adja hozzá a hívott metódus **AddEntities** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="9647f-194">Add a method called **AddEntities** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntities()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="9647f-195">Hello belül **AddEntities** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="9647f-195">Within hello **AddEntities** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9647f-196">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="9647f-196">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="9647f-197">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="9647f-197">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="9647f-198">Első egy **CloudTable** folyamatos tooadd hello új entitások áll hivatkozás toohello tábla toowhich képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-198">Get a **CloudTable** object that represents a reference toohello table toowhich you are going tooadd hello new entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="9647f-199">Néhány hello alapján ügyfélobjektumot példányosítható **CustomerEntity** osztály hello a szakaszban bemutatott minta [hozzáadása egy entitás tooa tábla](#add-an-entity-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="9647f-199">Instantiate some customer objects based on hello **CustomerEntity** model class presented in hello section, [Add an entity tooa table](#add-an-entity-to-a-table).</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. <span data-ttu-id="9647f-200">Első egy **TableBatchOperation** objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-200">Get a **TableBatchOperation** object.</span></span>

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. <span data-ttu-id="9647f-201">Entitások toohello kötegelt beszúrási művelet objektum hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9647f-201">Add entities toohello batch insert operation object.</span></span>

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. <span data-ttu-id="9647f-202">Hello kötegelt beszúrási művelet végrehajtása a hívó hello által **CloudTable.ExecuteBatch** metódust.</span><span class="sxs-lookup"><span data-stu-id="9647f-202">Execute hello batch insert operation by calling hello **CloudTable.ExecuteBatch** method.</span></span>   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. <span data-ttu-id="9647f-203">Hello **CloudTable.ExecuteBatch** metódus listáját adja vissza **TableResult** objektumok ahol minden **TableResult** objektum vizsgálni toodetermine hello sikeres vagy sikertelen lehet. az egyes műveleteket.</span><span class="sxs-lookup"><span data-stu-id="9647f-203">hello **CloudTable.ExecuteBatch** method returns a list of **TableResult** objects where each **TableResult** object can be examined toodetermine hello success or failure of each individual operation.</span></span> <span data-ttu-id="9647f-204">Ehhez a példához hello tooa listanézet továbbítja, és lehetővé teszik a hello nézetben minden egyes művelet hello eredmények megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9647f-204">For this example, pass hello list tooa view and let hello view display hello results of each operation.</span></span> 
 
    ```csharp
    return View(results);
    ```

1. <span data-ttu-id="9647f-205">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="9647f-205">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="9647f-206">A hello **nézet hozzáadása** párbeszédpanelen adja meg **AddEntities** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-206">On hello **Add View** dialog, enter **AddEntities** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="9647f-207">Nyissa meg `AddEntities.cshtml`, és módosítsa azt, hogy azt a következőhöz hasonló hello.</span><span class="sxs-lookup"><span data-stu-id="9647f-207">Open `AddEntities.cshtml`, and modify it so that it looks like hello following.</span></span>

    ```csharp
    @model IEnumerable<Microsoft.WindowsAzure.Storage.Table.TableResult>
    @{
        ViewBag.Title = "AddEntities";
    }
    
    <h2>Add-entities results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        @foreach (var result in Model)
        {
        <tr>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((result.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@result.HttpStatusCode</td>
        </tr>
        }
    </table>
    ```

1. <span data-ttu-id="9647f-208">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9647f-208">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="9647f-209">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="9647f-209">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. <span data-ttu-id="9647f-210">Hello alkalmazás futtatásához, és válassza ki **új entitásokat** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9647f-210">Run hello application, and select **Add entities** toosee results similar toohello following screen shot:</span></span>
  
    ![Entitások hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    <span data-ttu-id="9647f-212">Hello entitás hozzáadásának hello területen hello lépéseket követve ellenőrizheti [beolvasása egyetlen entitás](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="9647f-212">You can verify that hello entity was added by following hello steps in hello section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="9647f-213">Is használhatja a hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview összes hello entitások a táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="9647f-213">You can also use hello [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview all hello entities for your tables.</span></span>

## <a name="get-a-single-entity"></a><span data-ttu-id="9647f-214">Egyetlen entitás beolvasása</span><span class="sxs-lookup"><span data-stu-id="9647f-214">Get a single entity</span></span>

<span data-ttu-id="9647f-215">Ez a szakasz bemutatja, hogyan tooget egy tábla használatával egyetlen entitás hello entitás sorkulcsa és partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="9647f-215">This section illustrates how tooget a single entity from a table using hello entity's row key and partition key.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="9647f-216">Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment), és adatokat használ [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="9647f-216">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="9647f-217">Nyissa meg hello `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9647f-217">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="9647f-218">Adja hozzá a hívott metódus **GetSingle** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="9647f-218">Add a method called **GetSingle** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetSingle()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="9647f-219">Hello belül **GetSingle** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="9647f-219">Within hello **GetSingle** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9647f-220">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="9647f-220">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="9647f-221">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="9647f-221">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="9647f-222">Első egy **CloudTable** , amelyből hello entity keres referencia toohello tábla képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-222">Get a **CloudTable** object that represents a reference toohello table from which you are retrieving hello entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="9647f-223">Hozzon létre egy lekérése művelet objektumot, amely származó entitásobjektumra **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="9647f-223">Create a retrieve operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="9647f-224">hello első paramétere hello *partitionKey*, és a második paraméter hello hello *rowKey*.</span><span class="sxs-lookup"><span data-stu-id="9647f-224">hello first parameter is hello *partitionKey*, and hello second parameter is hello *rowKey*.</span></span> <span data-ttu-id="9647f-225">Hello segítségével **CustomerEntity** hello szakaszban bemutatott osztály- és [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table), kód részlet lekérdezések hello tábla a következő hello egy **CustomerEntity** entitás egy *partitionKey* "Smith" értékének és egy *rowKey* "Ben" értékét:</span><span class="sxs-lookup"><span data-stu-id="9647f-225">Using hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table), hello following code snippet queries hello table for a **CustomerEntity** entity with a *partitionKey* value of "Smith" and a *rowKey* value of "Ben":</span></span>

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. <span data-ttu-id="9647f-226">Hello lekérése művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="9647f-226">Execute hello retrieve operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. <span data-ttu-id="9647f-227">Hello eredmény toohello nézetben megjelenített átadni.</span><span class="sxs-lookup"><span data-stu-id="9647f-227">Pass hello result toohello view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="9647f-228">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="9647f-228">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="9647f-229">A hello **nézet hozzáadása** párbeszédpanelen adja meg **GetSingle** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-229">On hello **Add View** dialog, enter **GetSingle** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="9647f-230">Nyissa meg `GetSingle.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="9647f-230">Open `GetSingle.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "GetSingle";
    }
    
    <h2>Get Single results</h2>
    
    <table border="1">
        <tr>
            <th>HTTP result</th>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        <tr>
            <td>@Model.HttpStatusCode</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).Email)</td>
        </tr>
    </table>
    ```

1. <span data-ttu-id="9647f-231">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9647f-231">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="9647f-232">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="9647f-232">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. <span data-ttu-id="9647f-233">Hello alkalmazás futtatásához, és válassza ki **beolvasása egyetlen** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9647f-233">Run hello application, and select **Get Single** toosee results similar toohello following screen shot:</span></span>
  
    ![Egyetlen beolvasása](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a><span data-ttu-id="9647f-235">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="9647f-235">Get all entities in a partition</span></span>

<span data-ttu-id="9647f-236">Hello szakaszban említett [hozzáadása egy entitás tooa tábla](#add-an-entity-to-a-table), egy partíciót, és a sorkulcs hello kombinációja egyedi módon azonosítja az entitás egy tábla.</span><span class="sxs-lookup"><span data-stu-id="9647f-236">As mentioned in hello section, [Add an entity tooa table](#add-an-entity-to-a-table), hello combination of a partition and a row key uniquely identify an entity in a table.</span></span> <span data-ttu-id="9647f-237">Az azonos partíciókulcsú entitások gyorsabban entitások lehet lekérdezni a különböző partíciókulcsúak.</span><span class="sxs-lookup"><span data-stu-id="9647f-237">Entities with the same partition key can be queried faster than entities with different partition keys.</span></span> <span data-ttu-id="9647f-238">Ez a szakasz bemutatja, hogyan tooquery megadott partícióról entitásokhoz hello tábla.</span><span class="sxs-lookup"><span data-stu-id="9647f-238">This section illustrates how tooquery a table for all hello entities from a specified partition.</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="9647f-239">Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment), és adatokat használ [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="9647f-239">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="9647f-240">Nyissa meg hello `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9647f-240">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="9647f-241">Adja hozzá a hívott metódus **GetPartition** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="9647f-241">Add a method called **GetPartition** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetPartition()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="9647f-242">Hello belül **GetPartition** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="9647f-242">Within hello **GetPartition** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9647f-243">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="9647f-243">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="9647f-244">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="9647f-244">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="9647f-245">Első egy **CloudTable** egy toohello referenciatábla, amelyből keres hello entitásokat képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-245">Get a **CloudTable** object that represents a reference toohello table from which you are retrieving hello entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="9647f-246">Hozható létre egy **TableQuery** hello hello lekérdezést objektum **ahol** záradékban.</span><span class="sxs-lookup"><span data-stu-id="9647f-246">Instantiate a **TableQuery** object specifying hello query in hello **Where** clause.</span></span> <span data-ttu-id="9647f-247">Hello segítségével **CustomerEntity** hello szakaszban bemutatott osztály- és [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table), hello következő részlet lekérdezések hello tábla összes entitásra vonatkozó kódot, ha hello  **PartitionKey** (az ügyfél utolsó neve) értéke "Smith":</span><span class="sxs-lookup"><span data-stu-id="9647f-247">Using hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table), hello following code snippet queries hello table for a all entities where hello **PartitionKey** (customer's last name) has a value of "Smith":</span></span>

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. <span data-ttu-id="9647f-248">A hurkon belül hívás hello **CloudTable.ExecuteQuerySegmented** hello lekérdezés objektum példánya létre, hogy átadja hello előző lépésben metódust.</span><span class="sxs-lookup"><span data-stu-id="9647f-248">Within a loop, call hello **CloudTable.ExecuteQuerySegmented** method passing hello query object you instantiated in hello previous step.</span></span>  <span data-ttu-id="9647f-249">Hello **CloudTable.ExecuteQuerySegmented** metódus értéket ad vissza egy **TableContinuationToken** által - amikor **null** -azt jelzi, hogy nincsenek-e további entitások tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="9647f-249">hello **CloudTable.ExecuteQuerySegmented** method returns a **TableContinuationToken** object that - when **null** - indicates that there are no more entities tooretrieve.</span></span> <span data-ttu-id="9647f-250">Hello hurkon belül használ az egy másik hurok tooiterate hello entitást adott vissza.</span><span class="sxs-lookup"><span data-stu-id="9647f-250">Within hello loop, use another loop tooiterate over hello returned entities.</span></span> <span data-ttu-id="9647f-251">Az alábbi kódpéldát hello minden visszaadott entitás tooa listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9647f-251">In hello following code example, each returned entity is added tooa list.</span></span> <span data-ttu-id="9647f-252">Egyszer hello hurok véget ér, hello lista átadása tooa nézetének megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="9647f-252">Once hello loop ends, hello list is passed tooa view for display:</span></span> 

    ```csharp
    List<CustomerEntity> customers = new List<CustomerEntity>();
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = table.ExecuteQuerySegmented(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity customer in resultSegment.Results)
        {
            customers.Add(customer);
        }
    } while (token != null);

    return View(customers);
    ```

1. <span data-ttu-id="9647f-253">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="9647f-253">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="9647f-254">A hello **nézet hozzáadása** párbeszédpanelen adja meg **GetPartition** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-254">On hello **Add View** dialog, enter **GetPartition** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="9647f-255">Nyissa meg `GetPartition.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="9647f-255">Open `GetPartition.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @model IEnumerable<StorageAspnet.Models.CustomerEntity>
    @{
        ViewBag.Title = "GetPartition";
    }
    
    <h2>Get Partition results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>Email</th>
        </tr>
        @foreach (var customer in Model)
        {
        <tr>
            <td>@(customer.RowKey)</td>
            <td>@(customer.PartitionKey)</td>
            <td>@(customer.Email)</td>
        </tr>
        }
    </table>
    ```

1. <span data-ttu-id="9647f-256">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9647f-256">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="9647f-257">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="9647f-257">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. <span data-ttu-id="9647f-258">Hello alkalmazás futtatásához, és válassza ki **első partíció** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9647f-258">Run hello application, and select **Get Partition** toosee results similar toohello following screen shot:</span></span>
  
    ![Partíció beolvasása](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a><span data-ttu-id="9647f-260">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="9647f-260">Delete an entity</span></span>

<span data-ttu-id="9647f-261">Ez a szakasz bemutatja, hogyan toodelete egy entitás egy táblázatban.</span><span class="sxs-lookup"><span data-stu-id="9647f-261">This section illustrates how toodelete an entity from a table.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="9647f-262">Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment), és adatokat használ [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="9647f-262">This section assumes you have completed hello steps in [Set up hello development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="9647f-263">Nyissa meg hello `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9647f-263">Open hello `TablesController.cs` file.</span></span>

1. <span data-ttu-id="9647f-264">Adja hozzá a hívott metódus **DeleteEntity** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="9647f-264">Add a method called **DeleteEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="9647f-265">Hello belül **DeleteEntity** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="9647f-265">Within hello **DeleteEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="9647f-266">Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)</span><span class="sxs-lookup"><span data-stu-id="9647f-266">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration: (Change *&lt;storage-account-name>* toohello name of hello Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="9647f-267">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="9647f-267">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="9647f-268">Első egy **CloudTable** egy toohello referenciatábla hello entitás törli, amelyből képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="9647f-268">Get a **CloudTable** object that represents a reference toohello table from which you are deleting hello entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="9647f-269">Hozzon létre egy törlési művelet objektumot, amely fogad, származtatott felületnek entitásobjektumot **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="9647f-269">Create a delete operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="9647f-270">Ebben az esetben használjuk hello **CustomerEntity** hello szakaszban bemutatott osztály- és [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="9647f-270">In this case, we use hello **CustomerEntity** class and data presented in hello section [Add a batch of entities tooa table](#add-a-batch-of-entities-to-a-table).</span></span> <span data-ttu-id="9647f-271">entitás hello **ETag** tooa érvényes értéket kell állítani.</span><span class="sxs-lookup"><span data-stu-id="9647f-271">hello entity's **ETag** must be set tooa valid value.</span></span>  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. <span data-ttu-id="9647f-272">Hello delete műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="9647f-272">Execute hello delete operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. <span data-ttu-id="9647f-273">Hello eredmény toohello nézetben megjelenített átadni.</span><span class="sxs-lookup"><span data-stu-id="9647f-273">Pass hello result toohello view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="9647f-274">A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="9647f-274">In hello **Solution Explorer**, expand hello **Views** folder, right-click **Tables**, and from hello context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="9647f-275">A hello **nézet hozzáadása** párbeszédpanelen adja meg **DeleteEntity** hello nézet nevét, és válassza ki a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9647f-275">On hello **Add View** dialog, enter **DeleteEntity** for hello view name, and select **Add**.</span></span>

1. <span data-ttu-id="9647f-276">Nyissa meg `DeleteEntity.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:</span><span class="sxs-lookup"><span data-stu-id="9647f-276">Open `DeleteEntity.cshtml`, and modify it so that it looks like hello following code snippet:</span></span>

    ```csharp
    @model Microsoft.WindowsAzure.Storage.Table.TableResult
    @{
        ViewBag.Title = "DeleteEntity";
    }
    
    <h2>Delete Entity results</h2>
    
    <table border="1">
        <tr>
            <th>First name</th>
            <th>Last name</th>
            <th>HTTP result</th>
        </tr>
        <tr>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).RowKey)</td>
            <td>@((Model.Result as StorageAspnet.Models.CustomerEntity).PartitionKey)</td>
            <td>@Model.HttpStatusCode</td>
        </tr>
    </table>

    ```

1. <span data-ttu-id="9647f-277">A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="9647f-277">In hello **Solution Explorer**, expand hello **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="9647f-278">Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="9647f-278">After hello last **Html.ActionLink**, add hello following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. <span data-ttu-id="9647f-279">Hello alkalmazás futtatásához, és válassza ki **entitás törlése** toosee eredménye a következő képernyőfelvétel hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="9647f-279">Run hello application, and select **Delete entity** toosee results similar toohello following screen shot:</span></span>
  
    ![Egyetlen beolvasása](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a><span data-ttu-id="9647f-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9647f-281">Next steps</span></span>
<span data-ttu-id="9647f-282">Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.</span><span class="sxs-lookup"><span data-stu-id="9647f-282">View more feature guides toolearn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="9647f-283">Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="9647f-283">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="9647f-284">Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="9647f-284">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-queues.md)

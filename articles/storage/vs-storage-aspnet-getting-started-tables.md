---
title: "Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) |} Microsoft Docs"
description: "Ismerkedés az Azure table storage használata egy ASP.NET-projekt, a Visual Studio egy tárfiókot, a Visual Studio kapcsolódó szolgáltatások használatával történő kapcsolódás után"
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
ms.openlocfilehash: d9cb32483d3f582bbeb0ccc6a204a8b6d9ea5c96
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a><span data-ttu-id="69064-103">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="69064-103">Get started with Azure table storage and Visual Studio Connected Services (ASP.NET)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="69064-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="69064-104">Overview</span></span>

<span data-ttu-id="69064-105">Az Azure Table storage lehetővé teszi nagy mennyiségű strukturált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="69064-105">Azure Table storage enables you to store large amounts of structured data.</span></span> <span data-ttu-id="69064-106">A szolgáltatás egy NoSQL-adattár, amely elfogadja az érkező hitelesített hívásokat belül és kívül az Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="69064-106">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="69064-107">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="69064-107">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="69064-108">Ez az oktatóanyag bemutatja, hogyan írhat kódot ASP.NET olyan gyakori forgatókönyveket tartalmaz, az Azure table storage entitások használata.</span><span class="sxs-lookup"><span data-stu-id="69064-108">This tutorial shows how to write ASP.NET code for some common scenarios using Azure table storage entities.</span></span> <span data-ttu-id="69064-109">Ilyen például, egy tábla létrehozása és hozzáadása, lekérdezése és tábla entitások törlése.</span><span class="sxs-lookup"><span data-stu-id="69064-109">These scenarios include creating a table, and adding, querying, and deleting table entities.</span></span> 

##<a name="prerequisites"></a><span data-ttu-id="69064-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69064-110">Prerequisites</span></span>

* [<span data-ttu-id="69064-111">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="69064-111">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="69064-112">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="69064-112">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a><span data-ttu-id="69064-113">Hozzon létre az MVC-vezérlő</span><span class="sxs-lookup"><span data-stu-id="69064-113">Create an MVC controller</span></span> 

1. <span data-ttu-id="69064-114">A a **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és a helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.</span><span class="sxs-lookup"><span data-stu-id="69064-114">In the **Solution Explorer**, right-click **Controllers**, and, from the context menu, select **Add->Controller**.</span></span>

    ![Vezérlő hozzáadása az ASP.NET MVC alkalmazások számára](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. <span data-ttu-id="69064-116">A a **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-116">On the **Add Scaffold** dialog, select **MVC 5 Controller - Empty**, and select **Add**.</span></span>

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. <span data-ttu-id="69064-118">Az a **vezérlő hozzáadása** párbeszédpanelen, a tartományvezérlő nevét *TablesController*, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-118">On the **Add Controller** dialog, name the controller *TablesController*, and select **Add**.</span></span>

    ![Neve az MVC-vezérlő](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. <span data-ttu-id="69064-120">Adja hozzá a következő *használatával* irányelvek a `TablesController.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="69064-120">Add the following *using* directives to the `TablesController.cs` file:</span></span>

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a><span data-ttu-id="69064-121">Hozzon létre egy modellosztállyal</span><span class="sxs-lookup"><span data-stu-id="69064-121">Create a model class</span></span>

<span data-ttu-id="69064-122">Számos ezen példája a következő cikket: használja a **TableEntity**-származtatott osztályt **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="69064-122">Many of the examples in this article use a **TableEntity**-derived class called **CustomerEntity**.</span></span> <span data-ttu-id="69064-123">A következő lépések végigvezetik egy modellt a forrásosztállyal deklaráló:</span><span class="sxs-lookup"><span data-stu-id="69064-123">The following steps guide you through declaring this class as a model class:</span></span>

1. <span data-ttu-id="69064-124">Az a **Megoldáskezelőben**, kattintson a jobb gombbal **modellek**, és a helyi menüből válassza ki a **Hozzáadás -> osztály**.</span><span class="sxs-lookup"><span data-stu-id="69064-124">In the **Solution Explorer**, right-click **Models**, and, from the context menu, select **Add->Class**.</span></span>

1. <span data-ttu-id="69064-125">Az a **új elem hozzáadása** párbeszédpanelen a név az osztály **CustomerEntity**.</span><span class="sxs-lookup"><span data-stu-id="69064-125">On the **Add New Item** dialog, name the class, **CustomerEntity**.</span></span>

1. <span data-ttu-id="69064-126">Nyissa meg a `CustomerEntity.cs` fájlt, és adja hozzá a következő **használatával** irányelv:</span><span class="sxs-lookup"><span data-stu-id="69064-126">Open the `CustomerEntity.cs` file, and add the following **using** directive:</span></span>

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. <span data-ttu-id="69064-127">Módosítsa az osztályt, hogy a befejeződése után az osztály hasonlóan az alábbi kódra van deklarálva.</span><span class="sxs-lookup"><span data-stu-id="69064-127">Modify the class so that, when finished, the class is declared as in the following code.</span></span> <span data-ttu-id="69064-128">Az osztály deklarál egy entitásosztályt nevű **CustomerEntity** , amely használ az ügyfél keresztnevét sorkulcsnak és a vezetéknevét partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="69064-128">The class declares an entity class called **CustomerEntity** that uses the customer's first name as the row key and last name as the partition key.</span></span>

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

## <a name="create-a-table"></a><span data-ttu-id="69064-129">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="69064-129">Create a table</span></span>

<span data-ttu-id="69064-130">A következő lépések bemutatják, hogyan hozzon létre egy táblát:</span><span class="sxs-lookup"><span data-stu-id="69064-130">The following steps illustrate how to create a table:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="69064-131">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="69064-131">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span> 

1. <span data-ttu-id="69064-132">Nyissa meg az `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="69064-132">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="69064-133">Adja hozzá a hívott metódus **CreateTable** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="69064-133">Add a method called **CreateTable** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult CreateTable()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="69064-134">Belül a **CreateTable** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="69064-134">Within the **CreateTable** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="69064-135">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="69064-135">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="69064-136">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="69064-136">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="69064-137">Első egy **CloudTable** hivatkozni kell a kívánt táblanév képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-137">Get a **CloudTable** object that represents a reference to the desired table name.</span></span> <span data-ttu-id="69064-138">A **CloudTableClient.GetTableReference** metódus nem tesz, a table storage egy kérelmet.</span><span class="sxs-lookup"><span data-stu-id="69064-138">The **CloudTableClient.GetTableReference** method does not make a request against table storage.</span></span> <span data-ttu-id="69064-139">A hivatkozás a tábla létezik-e adja vissza.</span><span class="sxs-lookup"><span data-stu-id="69064-139">The reference is returned whether or not the table exists.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="69064-140">Hívja a **CloudTable.CreateIfNotExists** metódus tábla létrehozása, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="69064-140">Call the **CloudTable.CreateIfNotExists** method to create the table if it does not yet exist.</span></span> <span data-ttu-id="69064-141">A **CloudTable.CreateIfNotExists** metódus beolvasása **igaz** , ha a tábla nem létezik, és sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="69064-141">The **CloudTable.CreateIfNotExists** method returns **true** if the table does not exist, and is successfully created.</span></span> <span data-ttu-id="69064-142">Ellenkező esetben **hamis** adja vissza.</span><span class="sxs-lookup"><span data-stu-id="69064-142">Otherwise, **false** is returned.</span></span>    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. <span data-ttu-id="69064-143">Frissítés a **ViewBag** a tábla nevével.</span><span class="sxs-lookup"><span data-stu-id="69064-143">Update the **ViewBag** with the name of the table.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. <span data-ttu-id="69064-144">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **táblák**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="69064-144">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="69064-145">Az a **nézet hozzáadása** párbeszédpanelen adja meg **CreateTable** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-145">On the **Add View** dialog, enter **CreateTable** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="69064-146">Nyissa meg `CreateTable.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="69064-146">Open `CreateTable.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. <span data-ttu-id="69064-147">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="69064-147">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="69064-148">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="69064-148">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. <span data-ttu-id="69064-149">Futtassa az alkalmazást, és válassza ki **tábla létrehozása** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="69064-149">Run the application, and select **Create table** to see results similar to the following screen shot:</span></span>
  
    ![tábla létrehozása](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    <span data-ttu-id="69064-151">Ahogy korábban említettük a **CloudTable.CreateIfNotExists** metódus beolvasása **igaz** csak a tábla nem létezik és jön létre.</span><span class="sxs-lookup"><span data-stu-id="69064-151">As mentioned previously, the **CloudTable.CreateIfNotExists** method returns **true** only when the table doesn't exist and is created.</span></span> <span data-ttu-id="69064-152">Ezért, ha futtatja a az alkalmazás a tábla létezik, a metódus visszaadja **hamis**.</span><span class="sxs-lookup"><span data-stu-id="69064-152">Therefore, if you run the app when the table exists, the method returns **false**.</span></span> <span data-ttu-id="69064-153">Az alkalmazás többször is lefuthat, törölnie kell az a táblázat az alkalmazás ismételt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="69064-153">To run the app multiple times, you must delete the table before running the app again.</span></span> <span data-ttu-id="69064-154">A tábla törlésével megteheti a **CloudTable.Delete** metódust.</span><span class="sxs-lookup"><span data-stu-id="69064-154">Deleting the table can be done via the **CloudTable.Delete** method.</span></span> <span data-ttu-id="69064-155">A tábla használatával is törölheti a [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy a [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span><span class="sxs-lookup"><span data-stu-id="69064-155">You can also delete the table using the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) or the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md).</span></span>  

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="69064-156">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="69064-156">Add an entity to a table</span></span>

<span data-ttu-id="69064-157">*Entitások* C leképezés\# származtatott egyéni osztály használatával objektumok **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="69064-157">*Entities* map to C\# objects by using a custom class derived from **TableEntity**.</span></span> <span data-ttu-id="69064-158">Ha hozzá szeretne adni egy entitást egy táblához, hozzon létre egy osztályt, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="69064-158">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="69064-159">Ebben a szakaszban láthatja, hogyan adhat meg egy entitásosztályt, amely az ügyfél keresztnevét használja sorkulcsnak és a vezetéknevét partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="69064-159">In this section, you'll see how to define an entity class that uses the customer's first name as the row key and last name as the partition key.</span></span> <span data-ttu-id="69064-160">Egy entitás partíció- és sorkulcsa együttesen azonosítja az entitást a táblán belül.</span><span class="sxs-lookup"><span data-stu-id="69064-160">Together, an entity's partition and row key uniquely identify the entity in the table.</span></span> <span data-ttu-id="69064-161">Az azonos partíciókulcsú entitások gyorsabban lekérdezhetők, mint a különböző partíciókulcsúak, de az eltérő partíciókulcsok használata a párhuzamos műveletek nagyobb fokú méretezhetőségét teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="69064-161">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="69064-162">A tulajdonság a table szolgáltatásban tárolni a tulajdonság egy egyaránt és értékek beolvasása tévő támogatott típus nyilvános tulajdonságának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="69064-162">For any property that should be stored in the table service, the property must be a public property of a supported type that exposes both setting and retrieving values.</span></span>
<span data-ttu-id="69064-163">Az entitásosztály *kell* deklarálható nyilvános, paraméter nélküli konstruktora.</span><span class="sxs-lookup"><span data-stu-id="69064-163">The entity class *must* declare a public parameter-less constructor.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="69064-164">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="69064-164">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="69064-165">Nyissa meg az `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="69064-165">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="69064-166">Adja hozzá a következő direktíva, hogy a kód a `TablesController.cs` fájl férhetnek hozzá a **CustomerEntity** osztály:</span><span class="sxs-lookup"><span data-stu-id="69064-166">Add the following directive so that the code in the `TablesController.cs` file can access the **CustomerEntity** class:</span></span>

    ```csharp
    using StorageAspnet.Models;
    ```

1. <span data-ttu-id="69064-167">Adja hozzá a hívott metódus **AddEntity** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="69064-167">Add a method called **AddEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="69064-168">Belül a **AddEntity** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="69064-168">Within the **AddEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="69064-169">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="69064-169">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="69064-170">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="69064-170">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="69064-171">Első egy **CloudTable** a tábla, amelyhez az új entitás hozzáadása szeretne hivatkozni képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-171">Get a **CloudTable** object that represents a reference to the table to which you are going to add the new entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="69064-172">Példányt létrehozni és inicializálni a **CustomerEntity** osztály.</span><span class="sxs-lookup"><span data-stu-id="69064-172">Instantiate and initialize the **CustomerEntity** class.</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. <span data-ttu-id="69064-173">Hozzon létre egy **TableOperation** objektum, amely beszúrja az ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="69064-173">Create a **TableOperation** object that inserts the customer entity.</span></span>

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. <span data-ttu-id="69064-174">Az insert művelet végrehajtása meghívásával a **CloudTable.Execute** metódust.</span><span class="sxs-lookup"><span data-stu-id="69064-174">Execute the insert operation by calling the **CloudTable.Execute** method.</span></span> <span data-ttu-id="69064-175">A művelet eredményét vizsgálatával ellenőrizheti a **TableResult.HttpStatusCode** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="69064-175">You can verify the result of the operation by inspecting the **TableResult.HttpStatusCode** property.</span></span> <span data-ttu-id="69064-176">2xx állapotkódot azt jelzi, hogy az ügyfél által kért művelet feldolgozása sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="69064-176">A status code of 2xx indicates the action requested by the client was processed successfully.</span></span> <span data-ttu-id="69064-177">Például egy HTTP-állapotkód: 204, ami azt jelenti, hogy a művelet feldolgozása sikeresen megtörtént, és a kiszolgáló új entitások eredmények sikeres Beszúrások nem adott vissza tartalmakhoz.</span><span class="sxs-lookup"><span data-stu-id="69064-177">For example, successful insertions of new entities results in an HTTP status code of 204, meaning that the operation was successfully processed and the server did not return any content.</span></span>

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. <span data-ttu-id="69064-178">Frissítés a **ViewBag** a táblázat nevét, és az insert művelet eredménye.</span><span class="sxs-lookup"><span data-stu-id="69064-178">Update the **ViewBag** with the table name, and the results of the insert operation.</span></span>

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. <span data-ttu-id="69064-179">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **táblák**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="69064-179">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="69064-180">Az a **nézet hozzáadása** párbeszédpanelen adja meg **AddEntity** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-180">On the **Add View** dialog, enter **AddEntity** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="69064-181">Nyissa meg `AddEntity.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="69064-181">Open `AddEntity.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. <span data-ttu-id="69064-182">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="69064-182">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="69064-183">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="69064-183">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. <span data-ttu-id="69064-184">Futtassa az alkalmazást, és válassza ki **entitás** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="69064-184">Run the application, and select **Add entity** to see results similar to the following screen shot:</span></span>
  
    ![Entitás hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    <span data-ttu-id="69064-186">Ellenőrizheti, hogy az entitás felvette a szakaszban ismertetett lépések [beolvasása egyetlen entitás](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="69064-186">You can verify that the entity was added by following the steps in the section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="69064-187">Használhatja a [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) a táblák összes entitás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="69064-187">You can also use the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to view all the entities for your tables.</span></span>

## <a name="add-a-batch-of-entities-to-a-table"></a><span data-ttu-id="69064-188">Egy teljes entitásköteget hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="69064-188">Add a batch of entities to a table</span></span>

<span data-ttu-id="69064-189">Nem csak [vehető fel olyan entitás egy táblához egyszerre](#add-an-entity-to-a-table), azt is megteheti entitások kötegben.</span><span class="sxs-lookup"><span data-stu-id="69064-189">In addition to being able to [add an entity to a table one at a time](#add-an-entity-to-a-table), you can also add entities in batch.</span></span> <span data-ttu-id="69064-190">Entitás hozzáadása a kötegelt csökkenti a kódot és az Azure table szolgáltatás közötti üzenetváltások számát.</span><span class="sxs-lookup"><span data-stu-id="69064-190">Adding entities in batch reduces the number of round-trips between your code and the Azure table service.</span></span> <span data-ttu-id="69064-191">A következő lépések bemutatják, hogyan adhat több entitás egyetlen táblához insert művelet:</span><span class="sxs-lookup"><span data-stu-id="69064-191">The following steps illustrate how to add multiple entities to a table with a single insert operation:</span></span>

> [!NOTE]
> 
> <span data-ttu-id="69064-192">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment).</span><span class="sxs-lookup"><span data-stu-id="69064-192">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment).</span></span>

1. <span data-ttu-id="69064-193">Nyissa meg az `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="69064-193">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="69064-194">Adja hozzá a hívott metódus **AddEntities** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="69064-194">Add a method called **AddEntities** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult AddEntities()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="69064-195">Belül a **AddEntities** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="69064-195">Within the **AddEntities** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="69064-196">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="69064-196">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="69064-197">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="69064-197">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="69064-198">Első egy **CloudTable** hivatkozni kell a tábla, amelyhez kívánja hozzáadni az új entitásokat képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-198">Get a **CloudTable** object that represents a reference to the table to which you are going to add the new entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="69064-199">Egyes felhasználói objektumok alapján hozható létre a **CustomerEntity** osztály a szakaszban bemutatott minta [vehető fel olyan entitás egy táblához](#add-an-entity-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="69064-199">Instantiate some customer objects based on the **CustomerEntity** model class presented in the section, [Add an entity to a table](#add-an-entity-to-a-table).</span></span>

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. <span data-ttu-id="69064-200">Első egy **TableBatchOperation** objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-200">Get a **TableBatchOperation** object.</span></span>

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. <span data-ttu-id="69064-201">Entitás hozzáadása a kötegelt beszúrási művelet objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-201">Add entities to the batch insert operation object.</span></span>

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. <span data-ttu-id="69064-202">A kötegelt beszúrási művelet végrehajtása meghívásával a **CloudTable.ExecuteBatch** metódust.</span><span class="sxs-lookup"><span data-stu-id="69064-202">Execute the batch insert operation by calling the **CloudTable.ExecuteBatch** method.</span></span>   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. <span data-ttu-id="69064-203">A **CloudTable.ExecuteBatch** metódus listáját adja vissza **TableResult** objektumok ahol minden **TableResult** objektum is vizsgálja meg a sikeres vagy sikertelen volt-e minden egyes művelethez.</span><span class="sxs-lookup"><span data-stu-id="69064-203">The **CloudTable.ExecuteBatch** method returns a list of **TableResult** objects where each **TableResult** object can be examined to determine the success or failure of each individual operation.</span></span> <span data-ttu-id="69064-204">Ennél a példánál adja át a listában egy nézetre, és lehetővé teszik a nézetben minden egyes művelet eredményének megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="69064-204">For this example, pass the list to a view and let the view display the results of each operation.</span></span> 
 
    ```csharp
    return View(results);
    ```

1. <span data-ttu-id="69064-205">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **táblák**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="69064-205">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="69064-206">Az a **nézet hozzáadása** párbeszédpanelen adja meg **AddEntities** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-206">On the **Add View** dialog, enter **AddEntities** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="69064-207">Nyissa meg `AddEntities.cshtml`, és módosítsa azt, hogy azt a következőhöz hasonló.</span><span class="sxs-lookup"><span data-stu-id="69064-207">Open `AddEntities.cshtml`, and modify it so that it looks like the following.</span></span>

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

1. <span data-ttu-id="69064-208">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="69064-208">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="69064-209">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="69064-209">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. <span data-ttu-id="69064-210">Futtassa az alkalmazást, és válassza ki **új entitásokat** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="69064-210">Run the application, and select **Add entities** to see results similar to the following screen shot:</span></span>
  
    ![Entitások hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    <span data-ttu-id="69064-212">Ellenőrizheti, hogy az entitás felvette a szakaszban ismertetett lépések [beolvasása egyetlen entitás](#get-a-single-entity).</span><span class="sxs-lookup"><span data-stu-id="69064-212">You can verify that the entity was added by following the steps in the section, [Get a single entity](#get-a-single-entity).</span></span> <span data-ttu-id="69064-213">Használhatja a [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) a táblák összes entitás megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="69064-213">You can also use the [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) to view all the entities for your tables.</span></span>

## <a name="get-a-single-entity"></a><span data-ttu-id="69064-214">Egyetlen entitás beolvasása</span><span class="sxs-lookup"><span data-stu-id="69064-214">Get a single entity</span></span>

<span data-ttu-id="69064-215">Ez a szakasz bemutatja, hogyan egyetlen entitás lekérése egy táblához, az entitás sorkulcsa és a partíciós kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="69064-215">This section illustrates how to get a single entity from a table using the entity's row key and partition key.</span></span> 

> [!NOTE]
> 
> <span data-ttu-id="69064-216">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment), és adatokat használ [egy teljes entitásköteget hozzáadása a táblához](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="69064-216">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="69064-217">Nyissa meg az `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="69064-217">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="69064-218">Adja hozzá a hívott metódus **GetSingle** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="69064-218">Add a method called **GetSingle** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetSingle()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="69064-219">Belül a **GetSingle** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="69064-219">Within the **GetSingle** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="69064-220">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="69064-220">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="69064-221">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="69064-221">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="69064-222">Első egy **CloudTable** hivatkozni kell a táblázat, amelyből az entitás keres képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-222">Get a **CloudTable** object that represents a reference to the table from which you are retrieving the entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="69064-223">Hozzon létre egy lekérése művelet objektumot, amely származó entitásobjektumra **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="69064-223">Create a retrieve operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="69064-224">Az első paraméter az *partitionKey*, és a második paraméter a *rowKey*.</span><span class="sxs-lookup"><span data-stu-id="69064-224">The first parameter is the *partitionKey*, and the second parameter is the *rowKey*.</span></span> <span data-ttu-id="69064-225">Használja a **CustomerEntity** osztály- és a szakaszban bemutatott [egy teljes entitásköteget hozzáadása a táblához](#add-a-batch-of-entities-to-a-table), a következő kódrészletet lekérdezi a tábla egy **CustomerEntity**entitás egy *partitionKey* "Smith" értékének és egy *rowKey* "Ben" értékét:</span><span class="sxs-lookup"><span data-stu-id="69064-225">Using the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table), the following code snippet queries the table for a **CustomerEntity** entity with a *partitionKey* value of "Smith" and a *rowKey* value of "Ben":</span></span>

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. <span data-ttu-id="69064-226">A beolvasási műveletet végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="69064-226">Execute the retrieve operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. <span data-ttu-id="69064-227">A nézetben megjelenítendő át az eredményt.</span><span class="sxs-lookup"><span data-stu-id="69064-227">Pass the result to the view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="69064-228">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **táblák**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="69064-228">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="69064-229">Az a **nézet hozzáadása** párbeszédpanelen adja meg **GetSingle** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-229">On the **Add View** dialog, enter **GetSingle** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="69064-230">Nyissa meg `GetSingle.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="69064-230">Open `GetSingle.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="69064-231">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="69064-231">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="69064-232">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="69064-232">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. <span data-ttu-id="69064-233">Futtassa az alkalmazást, és válassza ki **beolvasása egyetlen** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="69064-233">Run the application, and select **Get Single** to see results similar to the following screen shot:</span></span>
  
    ![Egyetlen beolvasása](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a><span data-ttu-id="69064-235">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="69064-235">Get all entities in a partition</span></span>

<span data-ttu-id="69064-236">A szakaszban említett [vehető fel olyan entitás egy táblához](#add-an-entity-to-a-table), egy partíciót és egy sorkulcsa együttesen egyedi módon azonosítja az entitást egy tábla.</span><span class="sxs-lookup"><span data-stu-id="69064-236">As mentioned in the section, [Add an entity to a table](#add-an-entity-to-a-table), the combination of a partition and a row key uniquely identify an entity in a table.</span></span> <span data-ttu-id="69064-237">Az azonos partíciókulcsú entitások gyorsabban entitások lehet lekérdezni a különböző partíciókulcsúak.</span><span class="sxs-lookup"><span data-stu-id="69064-237">Entities with the same partition key can be queried faster than entities with different partition keys.</span></span> <span data-ttu-id="69064-238">Ez a szakasz bemutatja, hogyan egy táblából egy partíció megadott származó összes entitás.</span><span class="sxs-lookup"><span data-stu-id="69064-238">This section illustrates how to query a table for all the entities from a specified partition.</span></span>  

> [!NOTE]
> 
> <span data-ttu-id="69064-239">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment), és adatokat használ [egy teljes entitásköteget hozzáadása a táblához](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="69064-239">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="69064-240">Nyissa meg az `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="69064-240">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="69064-241">Adja hozzá a hívott metódus **GetPartition** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="69064-241">Add a method called **GetPartition** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult GetPartition()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="69064-242">Belül a **GetPartition** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="69064-242">Within the **GetPartition** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="69064-243">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="69064-243">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="69064-244">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="69064-244">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="69064-245">Első egy **CloudTable** hivatkozni kell a táblázat, amelyből keres az entitások képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-245">Get a **CloudTable** object that represents a reference to the table from which you are retrieving the entities.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="69064-246">Hozható létre egy **TableQuery** objektum a lekérdezést a **ahol** záradékban.</span><span class="sxs-lookup"><span data-stu-id="69064-246">Instantiate a **TableQuery** object specifying the query in the **Where** clause.</span></span> <span data-ttu-id="69064-247">Használja a **CustomerEntity** osztály- és a szakaszban bemutatott [egy teljes entitásköteget hozzáadása a táblához](#add-a-batch-of-entities-to-a-table), a következő kódrészletet lekérdezi a tábla összes entitásra vonatkozó ahol a  **PartitionKey** (az ügyfél utolsó neve) értéke "Smith":</span><span class="sxs-lookup"><span data-stu-id="69064-247">Using the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table), the following code snippet queries the table for a all entities where the **PartitionKey** (customer's last name) has a value of "Smith":</span></span>

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. <span data-ttu-id="69064-248">A hurkon belül hívja a **CloudTable.ExecuteQuerySegmented** módszert a lekérdezés objektum példánya létre, ha az előző lépésben átadásakor.</span><span class="sxs-lookup"><span data-stu-id="69064-248">Within a loop, call the **CloudTable.ExecuteQuerySegmented** method passing the query object you instantiated in the previous step.</span></span>  <span data-ttu-id="69064-249">A **CloudTable.ExecuteQuerySegmented** metódus értéket ad vissza egy **TableContinuationToken** által - amikor **null** -azt jelzi, hogy nincsenek-e további entitások beolvasása.</span><span class="sxs-lookup"><span data-stu-id="69064-249">The **CloudTable.ExecuteQuerySegmented** method returns a **TableContinuationToken** object that - when **null** - indicates that there are no more entities to retrieve.</span></span> <span data-ttu-id="69064-250">A hurkon belül használja egy másik hurok az ismétlés a visszaadott entitás.</span><span class="sxs-lookup"><span data-stu-id="69064-250">Within the loop, use another loop to iterate over the returned entities.</span></span> <span data-ttu-id="69064-251">Az alábbi példakódban minden visszaadott entitás hozzáadandó listáját.</span><span class="sxs-lookup"><span data-stu-id="69064-251">In the following code example, each returned entity is added to a list.</span></span> <span data-ttu-id="69064-252">Ha a hurok véget ér, a lista nézet megjelenítendő objektumnak átadott:</span><span class="sxs-lookup"><span data-stu-id="69064-252">Once the loop ends, the list is passed to a view for display:</span></span> 

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

1. <span data-ttu-id="69064-253">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **táblák**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="69064-253">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="69064-254">Az a **nézet hozzáadása** párbeszédpanelen adja meg **GetPartition** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-254">On the **Add View** dialog, enter **GetPartition** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="69064-255">Nyissa meg `GetPartition.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="69064-255">Open `GetPartition.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="69064-256">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="69064-256">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="69064-257">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="69064-257">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. <span data-ttu-id="69064-258">Futtassa az alkalmazást, és válassza ki **első partíció** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="69064-258">Run the application, and select **Get Partition** to see results similar to the following screen shot:</span></span>
  
    ![Partíció beolvasása](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a><span data-ttu-id="69064-260">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="69064-260">Delete an entity</span></span>

<span data-ttu-id="69064-261">Ez a szakasz bemutatja, hogyan entitás törlése a táblázatból.</span><span class="sxs-lookup"><span data-stu-id="69064-261">This section illustrates how to delete an entity from a table.</span></span>

> [!NOTE]
> 
> <span data-ttu-id="69064-262">Ez a szakasz azt feltételezi, hogy végrehajtotta a [beállította a fejlesztőkörnyezetet](#set-up-the-development-environment), és adatokat használ [egy teljes entitásköteget hozzáadása a táblához](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="69064-262">This section assumes you have completed the steps in [Set up the development environment](#set-up-the-development-environment), and uses data from [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> 

1. <span data-ttu-id="69064-263">Nyissa meg az `TablesController.cs` fájlt.</span><span class="sxs-lookup"><span data-stu-id="69064-263">Open the `TablesController.cs` file.</span></span>

1. <span data-ttu-id="69064-264">Adja hozzá a hívott metódus **DeleteEntity** , amely visszaadja az **ActionResult**.</span><span class="sxs-lookup"><span data-stu-id="69064-264">Add a method called **DeleteEntity** that returns an **ActionResult**.</span></span>

    ```csharp
    public ActionResult DeleteEntity()
    {
        // The code in this section goes here.

        return View();
    }
    ```

1. <span data-ttu-id="69064-265">Belül a **DeleteEntity** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="69064-265">Within the **DeleteEntity** method, get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="69064-266">A tárolási kapcsolati karakterlánc és tárfiókadatok beolvasása az Azure szolgáltatás konfigurációs az alábbi kód használatával: (módosítása  *&lt;tárfióknév >* elérni az Azure storage-fiók nevére.)</span><span class="sxs-lookup"><span data-stu-id="69064-266">Use the following code to get the storage connection string and storage account information from the Azure service configuration: (Change *&lt;storage-account-name>* to the name of the Azure storage account you're accessing.)</span></span>
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. <span data-ttu-id="69064-267">Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.</span><span class="sxs-lookup"><span data-stu-id="69064-267">Get a **CloudTableClient** object represents a table service client.</span></span>
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. <span data-ttu-id="69064-268">Első egy **CloudTable** hivatkozni kell a táblázat, amelyből törli az entitást képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="69064-268">Get a **CloudTable** object that represents a reference to the table from which you are deleting the entity.</span></span> 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. <span data-ttu-id="69064-269">Hozzon létre egy törlési művelet objektumot, amely fogad, származtatott felületnek entitásobjektumot **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="69064-269">Create a delete operation object that takes an entity object derived from **TableEntity**.</span></span> <span data-ttu-id="69064-270">Ebben az esetben használjuk a **CustomerEntity** osztály- és a szakaszban bemutatott [egy teljes entitásköteget hozzáadása a táblához](#add-a-batch-of-entities-to-a-table).</span><span class="sxs-lookup"><span data-stu-id="69064-270">In this case, we use the **CustomerEntity** class and data presented in the section [Add a batch of entities to a table](#add-a-batch-of-entities-to-a-table).</span></span> <span data-ttu-id="69064-271">Az entitás **ETag** érvényes értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="69064-271">The entity's **ETag** must be set to a valid value.</span></span>  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. <span data-ttu-id="69064-272">A törlési művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="69064-272">Execute the delete operation.</span></span>   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. <span data-ttu-id="69064-273">A nézetben megjelenítendő át az eredményt.</span><span class="sxs-lookup"><span data-stu-id="69064-273">Pass the result to the view for display.</span></span>

    ```csharp
    return View(result);
    ```

1. <span data-ttu-id="69064-274">A a **Solution Explorer**, bontsa ki a **nézetek** mappát, kattintson a jobb gombbal **táblák**, és válassza a helyi menüben a **Hozzáadás -> nézet**.</span><span class="sxs-lookup"><span data-stu-id="69064-274">In the **Solution Explorer**, expand the **Views** folder, right-click **Tables**, and from the context menu, select **Add->View**.</span></span>

1. <span data-ttu-id="69064-275">Az a **nézet hozzáadása** párbeszédpanelen adja meg **DeleteEntity** a nézet nevét, majd válassza a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="69064-275">On the **Add View** dialog, enter **DeleteEntity** for the view name, and select **Add**.</span></span>

1. <span data-ttu-id="69064-276">Nyissa meg `DeleteEntity.cshtml`, és módosítsa úgy, hogy például a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="69064-276">Open `DeleteEntity.cshtml`, and modify it so that it looks like the following code snippet:</span></span>

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

1. <span data-ttu-id="69064-277">Az a **Megoldáskezelőben**, bontsa ki a **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="69064-277">In the **Solution Explorer**, expand the **Views->Shared** folder, and open `_Layout.cshtml`.</span></span>

1. <span data-ttu-id="69064-278">Után utolsó **Html.ActionLink**, adja hozzá a következő **Html.ActionLink**:</span><span class="sxs-lookup"><span data-stu-id="69064-278">After the last **Html.ActionLink**, add the following **Html.ActionLink**:</span></span>

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. <span data-ttu-id="69064-279">Futtassa az alkalmazást, és válassza ki **entitás törlése** az alábbi képernyőfelvételhez hasonló eredmények megtekintése érdekében:</span><span class="sxs-lookup"><span data-stu-id="69064-279">Run the application, and select **Delete entity** to see results similar to the following screen shot:</span></span>
  
    ![Egyetlen beolvasása](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a><span data-ttu-id="69064-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69064-281">Next steps</span></span>
<span data-ttu-id="69064-282">Az Azure-ban való adattárolás további lehetőségeiről tekintse meg a többi szolgáltatás-útmutatót.</span><span class="sxs-lookup"><span data-stu-id="69064-282">View more feature guides to learn about additional options for storing data in Azure.</span></span>

  * [<span data-ttu-id="69064-283">Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="69064-283">Get started with Azure blob storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-blobs.md)
  * [<span data-ttu-id="69064-284">Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="69064-284">Get started with Azure queue storage and Visual Studio Connected Services (ASP.NET)</span></span>](./vs-storage-aspnet-getting-started-queues.md)

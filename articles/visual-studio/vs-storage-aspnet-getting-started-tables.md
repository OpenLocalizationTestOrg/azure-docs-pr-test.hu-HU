---
title: "az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET) lépései aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának az Azure table storage egy ASP.NET-projekt, a Visual Studio használó Visual Studio kapcsolódó szolgáltatások tooa tárfiókot kapcsolódás után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/21/2016
ms.author: kraigb
ms.openlocfilehash: 04f79db7aad60ca51c3c866da1f4b01d9e11ac52
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Áttekintés

Az Azure Table storage lehetővé teszi nagy mennyiségű toostore strukturált adatok. hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.

Ez az oktatóanyag bemutatja, hogyan toowrite ASP.NET olyan gyakori forgatókönyveket tartalmaz, az Azure table storage entitások használata helykódja. Ilyen például, egy tábla létrehozása és hozzáadása, lekérdezése és tábla entitások törlése. 

##<a name="prerequisites"></a>Előfeltételek

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Azure Storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/vs-storage-aspnet-getting-started-create-azure-account.md)]

[!INCLUDE [storage-development-environment-include](../../includes/vs-storage-aspnet-getting-started-setup-dev-env.md)]

### <a name="create-an-mvc-controller"></a>Hozzon létre az MVC-vezérlő 

1. A hello **Megoldáskezelőben**, kattintson a jobb gombbal **tartományvezérlők**, és hello helyi menüből válassza ki a **Hozzáadás -> tartományvezérlő**.

    ![A vezérlő tooan ASP.NET MVC alkalmazás hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-controller-menu.png)

1. A hello **hozzáadása Scaffold** párbeszédablakban válassza **MVC 5 vezérlő - üres**, és válassza ki **Hozzáadás**.

    ![Adja meg az MVC-vezérlő típusa](./media/vs-storage-aspnet-getting-started-tables/add-controller.png)

1. A hello **vezérlő hozzáadása** párbeszédpanelen neve hello vezérlő *TablesController*, és válassza ki **Hozzáadás**.

    ![Hello MVC-vezérlő neve](./media/vs-storage-aspnet-getting-started-tables/add-controller-name.png)

1. Adja hozzá a következő hello *használatával* irányelvek toohello `TablesController.cs` fájlt:

    ```csharp
    using Microsoft.Azure;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;
    using Microsoft.WindowsAzure.Storage.Table;
    ```

### <a name="create-a-model-class"></a>Hozzon létre egy modellosztállyal

Számos hello példák ezen cikk használja egy **TableEntity**-származtatott osztályt **CustomerEntity**. a lépéseket követve hello végigvezeti Önt az osztályhoz tartozik, mint egy modellosztállyal deklaráló:

1. A hello **Megoldáskezelőben**, kattintson a jobb gombbal **modellek**, és hello helyi menüből válassza ki a **Hozzáadás -> osztály**.

1. A hello **új elem hozzáadása** párbeszédpanelen name hello osztály **CustomerEntity**.

1. Nyissa meg hello `CustomerEntity.cs` fájlt, és adja hozzá a következő hello **használatával** irányelv:

    ```csharp
    using Microsoft.WindowsAzure.Storage.Table;
    ```

1. Módosítsa a hello osztály, hogy a befejezésekor hello osztály hasonlóan a következő kód hello van deklarálva. hello osztály deklarál egy entitásosztályt nevű **CustomerEntity** , hogy használ hello az ügyfél első hello sor kulcsként és vezetékneve hello partíció kulcsként.

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

## <a name="create-a-table"></a>Tábla létrehozása

hello következő lépések bemutatják, hogyan toocreate egy táblázatot:

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment). 

1. Nyissa meg hello `TablesController.cs` fájlt.

1. Adja hozzá a hívott metódus **CreateTable** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult CreateTable()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **CreateTable** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Első egy **CloudTable** egy hivatkozás toohello kívánt tábla nevét képviselő objektum. Hello **CloudTableClient.GetTableReference** metódus nem tesz, a table storage egy kérelmet. hello hivatkozás hello tábla létezik-e adja vissza. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Hello hívás **CloudTable.CreateIfNotExists** metódus toocreate hello tábla, ha még nem létezik. Hello **CloudTable.CreateIfNotExists** metódus beolvasása **igaz** Ha hello tábla nem létezik, és sikeresen létrejött. Ellenkező esetben **hamis** adja vissza.    

    ```csharp
    ViewBag.Success = table.CreateIfNotExists();
    ```

1. Frissítés hello **ViewBag** hello nevű hello tábla.

    ```csharp
    ViewBag.TableName = table.Name;
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **CreateTable** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `CreateTable.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "Create Table";
    }
    
    <h2>Create Table results</h2>

    Creation of @ViewBag.TableName @(ViewBag.Success == true ? "succeeded" : "failed")
    ```

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Create table", "CreateTable", "Tables")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **tábla létrehozása** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![tábla létrehozása](./media/vs-storage-aspnet-getting-started-tables/create-table-results.png)

    Ahogy korábban említettük hello **CloudTable.CreateIfNotExists** metódus beolvasása **igaz** csak hello tábla nem létezik és jön létre. Ezért, ha futtatja a hello app hello tábla létezik, hello metódus visszaadja **hamis**. toorun hello alkalmazás többször, törölnie kell hello tábla hello app ismételt futtatása előtt. Törlése hello tábla megteheti a hello **CloudTable.Delete** metódust. Hello tábla hello segítségével törölheti is [Azure-portálon](http://go.microsoft.com/fwlink/p/?LinkID=525040) vagy hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md).  

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása

*Entitások* tooC leképezése\# származtatott egyéni osztály használatával objektumok **TableEntity**. egy entitás tooa tábla tooadd hozzon létre egy osztályt, amely meghatározza az entitás hello tulajdonságait. Ebben a szakaszban láthatja, hogyan hello toodefine egy entitásosztályt, amely használja, az ügyfél első hello sor kulcsként és vezetékneve hello partíció kulcsként. Együtt egy entitás partíció- és sorkulcsa egyértelműen hello entitás hello táblában. Az azonos partíciókulcsú entitások gyorsabban lekérdezhetők, mint a különböző partíciókulcsúak, de az eltérő partíciókulcsok használata a párhuzamos műveletek nagyobb fokú méretezhetőségét teszi lehetővé. Bármely tulajdonság hello table szolgáltatásban tárolni hello tulajdonság egy egyaránt és értékek beolvasása tévő támogatott típus nyilvános tulajdonságának kell lennie.
hello entitásosztályt *kell* deklarálható nyilvános, paraméter nélküli konstruktora.

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment).

1. Nyissa meg hello `TablesController.cs` fájlt.

1. Adja hozzá a következő direktíva, amely hello hello lévő kódot úgy hello `TablesController.cs` fájl férhetnek hozzá hello **CustomerEntity** osztály:

    ```csharp
    using StorageAspnet.Models;
    ```

1. Adja hozzá a hívott metódus **AddEntity** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult AddEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **AddEntity** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Első egy **CloudTable** tooadd hello új entitás fog hivatkozás toohello tábla toowhich képviselő objektum. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Példányt létrehozni és inicializálni a hello **CustomerEntity** osztály.

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    ```

1. Hozzon létre egy **TableOperation** objektum, amely beszúrja az ügyfélentitást hello.

    ```csharp
    TableOperation insertOperation = TableOperation.Insert(customer1);
    ```

1. Hello beszúrási művelet végrehajtása a hívó hello által **CloudTable.Execute** metódust. Hello művelet eredménye hello hello vizsgálatával ellenőrizheti **TableResult.HttpStatusCode** tulajdonság. 2xx állapotkódot azt jelzi, hogy hello ügyfél által kért hello művelet feldolgozása sikeres volt. Az új entitások sikeres Beszúrások például egy HTTP-állapotkód: 204, amely hello művelet feldolgozása sikeresen megtörtént, és hello kiszolgáló nem adott vissza a tartalom a eredményez.

    ```csharp
    TableResult result = table.Execute(insertOperation);
    ```

1. Frissítés hello **ViewBag** hello táblanév és hello beszúrási művelet hello eredményeit.

    ```csharp
    ViewBag.TableName = table.Name;
    ViewBag.Result = result.HttpStatusCode;
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **AddEntity** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `AddEntity.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

    ```csharp
    @{
        ViewBag.Title = "Add entity";
    }
    
    <h2>Add entity results</h2>

    Insert of entity into @ViewBag.TableName @(ViewBag.Result == 204 ? "succeeded" : "failed")
    ```
1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entity", "AddEntity", "Tables")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **entitás** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Entitás hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-entity-results.png)

    Hello entitás hozzáadásának hello területen hello lépéseket követve ellenőrizheti [beolvasása egyetlen entitás](#get-a-single-entity). Is használhatja a hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview összes hello entitások a táblákhoz.

## <a name="add-a-batch-of-entities-tooa-table"></a>Egy entitás tooa tábla kötegelt hozzáadása

Továbbá toobeing képes a túl[egy entitás tooa tábla egyik felvenni egy időben](#add-an-entity-to-a-table), azt is megteheti entitások kötegben. Entitás hozzáadása a kötegelt csökkenti az üzenetváltások a kódot és az Azure table szolgáltatás hello közötti hello számát. hello lépések bemutatják, hogyan tooadd több entitások tooa tábla egy egyetlen insert művelet:

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment).

1. Nyissa meg hello `TablesController.cs` fájlt.

1. Adja hozzá a hívott metódus **AddEntities** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult AddEntities()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **AddEntities** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Első egy **CloudTable** folyamatos tooadd hello új entitások áll hivatkozás toohello tábla toowhich képviselő objektum. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Néhány hello alapján ügyfélobjektumot példányosítható **CustomerEntity** osztály hello a szakaszban bemutatott minta [hozzáadása egy entitás tooa tábla](#add-an-entity-to-a-table).

    ```csharp
    CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
    customer1.Email = "Jeff@contoso.com";

    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.Email = "Ben@contoso.com";
    ```

1. Első egy **TableBatchOperation** objektum.

    ```csharp
    TableBatchOperation batchOperation = new TableBatchOperation();
    ```

1. Entitások toohello kötegelt beszúrási művelet objektum hozzáadása.

    ```csharp
    batchOperation.Insert(customer1);
    batchOperation.Insert(customer2);
    ```

1. Hello kötegelt beszúrási művelet végrehajtása a hívó hello által **CloudTable.ExecuteBatch** metódust.   

    ```csharp
    IList<TableResult> results = table.ExecuteBatch(batchOperation);
    ```

1. Hello **CloudTable.ExecuteBatch** metódus listáját adja vissza **TableResult** objektumok ahol minden **TableResult** objektum vizsgálni toodetermine hello sikeres vagy sikertelen lehet. az egyes műveleteket. Ehhez a példához hello tooa listanézet továbbítja, és lehetővé teszik a hello nézetben minden egyes művelet hello eredmények megjelenítéséhez. 
 
    ```csharp
    return View(results);
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **AddEntities** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `AddEntities.cshtml`, és módosítsa azt, hogy azt a következőhöz hasonló hello.

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

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Add entities", "AddEntities", "Tables")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **új entitásokat** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Entitások hozzáadása](./media/vs-storage-aspnet-getting-started-tables/add-entities-results.png)

    Hello entitás hozzáadásának hello területen hello lépéseket követve ellenőrizheti [beolvasása egyetlen entitás](#get-a-single-entity). Is használhatja a hello [Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) tooview összes hello entitások a táblákhoz.

## <a name="get-a-single-entity"></a>Egyetlen entitás beolvasása

Ez a szakasz bemutatja, hogyan tooget egy tábla használatával egyetlen entitás hello entitás sorkulcsa és partíciós kulcs. 

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment), és adatokat használ [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table). 

1. Nyissa meg hello `TablesController.cs` fájlt.

1. Adja hozzá a hívott metódus **GetSingle** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult GetSingle()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **GetSingle** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Első egy **CloudTable** , amelyből hello entity keres referencia toohello tábla képviselő objektum. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Hozzon létre egy lekérése művelet objektumot, amely származó entitásobjektumra **TableEntity**. hello első paramétere hello *partitionKey*, és a második paraméter hello hello *rowKey*. Hello segítségével **CustomerEntity** hello szakaszban bemutatott osztály- és [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table), kód részlet lekérdezések hello tábla a következő hello egy **CustomerEntity** entitás egy *partitionKey* "Smith" értékének és egy *rowKey* "Ben" értékét:

    ```csharp
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");
    ```

1. Hello lekérése művelet végrehajtása.   

    ```csharp
    TableResult result = table.Execute(retrieveOperation);
    ```

1. Hello eredmény toohello nézetben megjelenített átadni.

    ```csharp
    return View(result);
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **GetSingle** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `GetSingle.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

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

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get single", "GetSingle", "Tables")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **beolvasása egyetlen** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Egyetlen beolvasása](./media/vs-storage-aspnet-getting-started-tables/get-single-results.png)

## <a name="get-all-entities-in-a-partition"></a>Egy partíció összes entitásának lekérése

Hello szakaszban említett [hozzáadása egy entitás tooa tábla](#add-an-entity-to-a-table), egy partíciót, és a sorkulcs hello kombinációja egyedi módon azonosítja az entitás egy tábla. Az azonos partíciókulcsú entitások gyorsabban entitások lehet lekérdezni a különböző partíciókulcsúak. Ez a szakasz bemutatja, hogyan tooquery megadott partícióról entitásokhoz hello tábla.  

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment), és adatokat használ [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table). 

1. Nyissa meg hello `TablesController.cs` fájlt.

1. Adja hozzá a hívott metódus **GetPartition** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult GetPartition()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **GetPartition** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Első egy **CloudTable** egy toohello referenciatábla, amelyből keres hello entitásokat képviselő objektum. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Hozható létre egy **TableQuery** hello hello lekérdezést objektum **ahol** záradékban. Hello segítségével **CustomerEntity** hello szakaszban bemutatott osztály- és [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table), hello következő részlet lekérdezések hello tábla összes entitásra vonatkozó kódot, ha hello  **PartitionKey** (az ügyfél utolsó neve) értéke "Smith":

    ```csharp
    TableQuery<CustomerEntity> query = 
        new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));
    ```

1. A hurkon belül hívás hello **CloudTable.ExecuteQuerySegmented** hello lekérdezés objektum példánya létre, hogy átadja hello előző lépésben metódust.  Hello **CloudTable.ExecuteQuerySegmented** metódus értéket ad vissza egy **TableContinuationToken** által - amikor **null** -azt jelzi, hogy nincsenek-e további entitások tooretrieve. Hello hurkon belül használ az egy másik hurok tooiterate hello entitást adott vissza. Az alábbi kódpéldát hello minden visszaadott entitás tooa listája jelenik meg. Egyszer hello hurok véget ér, hello lista átadása tooa nézetének megjelenítése: 

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

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **GetPartition** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `GetPartition.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

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

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Get partition", "GetPartition", "Tables")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **első partíció** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Partíció beolvasása](./media/vs-storage-aspnet-getting-started-tables/get-partition-results.png)

## <a name="delete-an-entity"></a>Entitás törlése

Ez a szakasz bemutatja, hogyan toodelete egy entitás egy táblázatban.

> [!NOTE]
> 
> Ez a szakasz azt feltételezi, hogy végrehajtotta hello a [hello fejlesztési környezet beállítása](#set-up-the-development-environment), és adatokat használ [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table). 

1. Nyissa meg hello `TablesController.cs` fájlt.

1. Adja hozzá a hívott metódus **DeleteEntity** , amely visszaadja az **ActionResult**.

    ```csharp
    public ActionResult DeleteEntity()
    {
        // hello code in this section goes here.

        return View();
    }
    ```

1. Hello belül **DeleteEntity** módszer, lekérni egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Használjon hello következő kódot tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait hello Azure szolgáltatáskonfiguráció: (módosítása  *&lt;-tárfióknév >* hello az Azure storage toohello neve -fiók éri el.)
   
    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
       CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
    ```

1. Első egy **CloudTableClient** objektum által jelképezett a table szolgáltatásügyfél.
   
    ```csharp
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

1. Első egy **CloudTable** egy toohello referenciatábla hello entitás törli, amelyből képviselő objektum. 
   
    ```csharp
    CloudTable table = tableClient.GetTableReference("TestTable");
    ```

1. Hozzon létre egy törlési művelet objektumot, amely fogad, származtatott felületnek entitásobjektumot **TableEntity**. Ebben az esetben használjuk hello **CustomerEntity** hello szakaszban bemutatott osztály- és [hozzáadása egy kötegelt entitások tooa tábla](#add-a-batch-of-entities-to-a-table). entitás hello **ETag** tooa érvényes értéket kell állítani.  

    ```csharp
    TableOperation deleteOperation = 
        TableOperation.Delete(new CustomerEntity("Smith", "Ben") { ETag = "*" } );
    ```

1. Hello delete műveletet végrehajtani.   

    ```csharp
    TableResult result = table.Execute(deleteOperation);
    ```

1. Hello eredmény toohello nézetben megjelenített átadni.

    ```csharp
    return View(result);
    ```

1. A hello **Solution Explorer**, bontsa ki a hello **nézetek** mappát, kattintson a jobb gombbal **táblák**, hello helyi menüből válassza ki a **Hozzáadás -> nézet**.

1. A hello **nézet hozzáadása** párbeszédpanelen adja meg **DeleteEntity** hello nézet nevét, és válassza ki a **Hozzáadás**.

1. Nyissa meg `DeleteEntity.cshtml`, és módosítsa azt, hogy a következő kódrészletet hello hasonlítson:

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

1. A hello **Megoldáskezelőben**, bontsa ki a hello **Nézet -> megosztott** mappát, majd nyissa meg `_Layout.cshtml`.

1. Hello után utolsó **Html.ActionLink**, adja hozzá a következő hello **Html.ActionLink**:

    ```html
    <li>@Html.ActionLink("Delete entity", "DeleteEntity", "Tables")</li>
    ```

1. Hello alkalmazás futtatásához, és válassza ki **entitás törlése** toosee eredménye a következő képernyőfelvétel hasonló toohello:
  
    ![Egyetlen beolvasása](./media/vs-storage-aspnet-getting-started-tables/delete-entity-results.png)

## <a name="next-steps"></a>Következő lépések
Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.

  * [Ismerkedés az Azure blob storage és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)](../storage/vs-storage-aspnet-getting-started-blobs.md)
  * [Ismerkedés az Azure várólista-tároló és a Visual Studio kapcsolódó szolgáltatások (ASP.NET)](../storage/vs-storage-aspnet-getting-started-queues.md)

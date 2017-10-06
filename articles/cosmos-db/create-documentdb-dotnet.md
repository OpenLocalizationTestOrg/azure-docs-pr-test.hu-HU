---
title: "Azure Cosmos DB: A .NET webalkalmazás létrehozása és a DocumentDB API hello |} Microsoft Docs"
description: "Megadja a .NET kódminta, használhatja a tooconnect tooand lekérdezés hello Azure Cosmos DB DocumentDB API"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: 35517e35d80c48662a51a99814652ffa1121fc5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-documentdb-api-web-app-with-net-and-hello-azure-portal"></a>Azure Cosmos DB: Hozza létre a DocumentDB API webes alkalmazás a .NET, és hello Azure-portálon

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon. Kell majd létrehozása és központi telepítése egy hello épülő teendőlistát megvalósító webes alkalmazás [DocumentDB .NET API](documentdb-sdk-dotnet.md), ahogy az alábbi képernyőfelvétel a hello. 

![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a>Előfeltételek

Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a>Gyűjtemény hozzáadása

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Mintaadatok hozzáadása

Most tooyour új adatgyűjtés adatkezelő használatával adhat hozzá.

1. Új adatbázis hello Data Explorer hello Gyűjtemények ablaktáblán jelenik meg. Bontsa ki a hello **feladatok** adatbázisra, és bontsa ki a hello **elemek** gyűjtemény, kattintson a **dokumentumok**, és kattintson a **új dokumentumok**. 

   ![Hozzon létre új dokumentumok az adatkezelő a hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. A struktúra a következő hello mostantól hozzáadhatja azok toohello dokumentumgyűjteményt.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Hello json toohello hozzáadása után **dokumentumok** lapra, majd **mentése**.

    ![Másolja át json-adatokat, és kattintson a Mentés gombra az adatok Explorer hello Azure-portálon](./media/create-documentdb-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Hozzon létre és mentsen egy egyedi értéket hello helyezze egy további dokumentum `id` tulajdonság, és a változás hello más tulajdonságok, ahogyan szeretné. Mivel az Azure Cosmos DB nem kötelezi egy adott adatséma használatára, új dokumentumaihoz bármilyen struktúrát választhat.

     Használhatja az adatkezelő tooretrieve lekérdezések adatait. Alapértelmezés szerint adatkezelő használja `SELECT * FROM c` összes hello gyűjtemény, de a dokumentumok tooretrieve módosíthatja, hogy különböző tooa [SQL-lekérdezés](documentdb-sql-query.md), például a `SELECT * FROM c ORDER BY c._ts DESC`, csökkenő sorrendben minden hello dokumentum alapján tooreturn az időbélyegző.
 
     Is használhatja adatkezelő toocreate tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooperform kiszolgálóoldali üzleti logikát is mint méretezési átviteli sebesség. Adatkezelő mutatja meg az összes hello beépített programozott adatelérési hello API-k érhető el, de hello Azure-portálon található egyszerű a hozzáférés tooyour adatokat biztosít.

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük kapcsoló tooworking kóddal. Most klónozza a Githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt a DocumentDB API-alkalmazásba. Láthatja, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `CD` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Ezután nyissa meg az hello todo megoldásfájlt a Visual Studio. 

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello DocumentDBRepository.cs fájlt, és, hogy ezek a sorok, a kód létrehozása hello Azure Cosmos DB erőforrások találhat. 

* hello DocumentClient sor 73 inicializálva van.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* A rendszer létrehozza az új adatbázist a 88. sorban.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* A rendszer létrehozza az új gyűjteményt a 107. sorban.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**. Hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs hello web.config fájlba hello következő lépésben fogja használni.

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-dotnet/keys.png)

2. Nyissa meg a Visual Studio 2017 hello web.config fájlt. 

3. Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello a Web.config fájlban hello végpont kulcs értékét. 

    `<add key="endpoint" value="FILLME" />`

4. Ezután másolja az elsődleges kulcs-érték hello portálról, és teszi hello értékének hello authKey a Web.config fájlban. Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-hello-web-app"></a>Hello webes alkalmazás futtatása
1. A Visual Studióban, kattintson a jobb gombbal a hello projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**. 

2. A hello NuGet **Tallózás** mezőbe írja be *DocumentDB*.

3. Hello eredmények közül telepítse a hello **Microsoft.Azure.DocumentDB** könyvtárban. Ez telepíti a hello Microsoft.Azure.DocumentDB csomagot, valamint az összes függősége.

4. Kattintson a CTRL + F5 toorun hello alkalmazás. Az alkalmazás megjelenik a böngészőben. 

5. Kattintson a **hozzon létre új** a böngésző hello és néhány új feladatot létrehozni az tennivaló alkalmazásban.

   ![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-dotnet/azure-comosdb-todo-app-list.png)

Mostantól tooData Explorer visszaléphet, és tekintse meg a lekérdezés, módosítása, és ezekkel az új adatokkal. 

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello adatkezelő használatával, és futtassa a webes alkalmazás. További adatok tooyour Cosmos DB fiókot most importálhatja. 

> [!div class="nextstepaction"]
> [Adatok importálása az Azure Cosmos DB-be](import-data.md)



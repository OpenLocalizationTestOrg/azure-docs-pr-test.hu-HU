---
title: "egy Azure Cosmos DB .NET alkalmazást, amely aaaBuild hello tábla API |} Microsoft Docs"
description: "Bevezetés az Azure Cosmos DB Table API-jának .NET-alapú használatába"
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/22/2017
ms.author: arramac
ms.openlocfilehash: bdd4f8ec45407962b3d2cb26aa814a20cfc62173
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-net-application-using-hello-table-api"></a>Az Azure Cosmos DB: Hello tábla API használatával .NET-alkalmazás létrehozása

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezési bemutatja, hogyan toocreate egy Azure Cosmos DB fiókot, és hozzon létre egy táblát a fiókban hello Azure-portál használatával. Akkor lesz majd írási kód tooinsert, update és delete entitásokat és egyes lekérdezések futtatása használatával hello új [Windows Azure Storage prémium tábla](https://aka.ms/premiumtablenuget) NuGet-csomagot (előzetes verzió). Ez a függvénytár hello azonos osztályok és hello nyilvános metódus-aláírása [Windows Azure Storage szolgáltatás SDK](https://www.nuget.org/packages/WindowsAzure.Storage), de hello képességét tooconnect tooAzure Cosmos DB fiókok használatával hello [tábla API](table-introduction.md) (előzetes verzió). 

## <a name="prerequisites"></a>Előfeltételek

Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Tábla hozzáadása

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

## <a name="add-sample-data"></a>Mintaadatok hozzáadása

Most tooyour új adattábla adatok kezelővel (előzetes verzió) használatával adhat hozzá.

1. Az Adatkezelőben bontsa ki a **minta tábla** pontot, és kattintson az **Entitások**, ezután pedig az **Entitás hozzáadása** lehetőségre.

   ![Hozzon létre új entitások az adatkezelő a hello Azure-portálon](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Most adatok toohello PartitionKey érték és RowKey érték jelölését, és kattintson a **entitás hozzáadása**.

   ![Állítsa be partíciós kulcs és egy új entitás Sorkulcsa hello](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    Most több entitások tooyour tábla hozzáadásához, szerkesztéséhez az entitások vagy adatkezelő az adatok lekérdezése. Adatkezelő akkor is, ahol az átviteli sebesség méretezhető és tárolt eljárások, felhasználó által megadott függvények és eseményindítók tooyour tábla.

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük klónozza a githubból, állítsa be a hello kapcsolati karakterláncot, és futtassa azt egy tábla alkalmazást. Láthatja, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Ezután nyissa meg a hello megoldásfájlt a Visual Studio. 

## <a name="review-hello-code"></a>Tekintse át a hello kódot

Most Meggyőződünk arról, mi történik a hello app gyors áttekintése. Nyissa meg hello Program.cs fájlt, és, hogy ezek a sorok, a kód létrehozása hello Azure Cosmos DB erőforrások találhat. 

* hello CloudTableClient inicializálva van.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Új táblázat jön létre, ha még nem létezik.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Létrejön egy új táblázattároló. Megfigyelheti, hogy a kód nagyon hasonló tooregular Azure Table storage SDK. 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most azt frissítése hello kapcsolati karakterlánc adatokat, az alkalmazás tooAzure Cosmos DB működik. 

1. A Visual Studióban nyissa meg a hello app.config fájlt. 

2. A hello [Azure-portálon](http://portal.azure.com/), hello Azure Cosmos DB bal oldali navigációs menü, kattintson a **kapcsolati karakterlánc**. Hello új panelen kattintson a Másolás gombra hello hello kapcsolati karakterláncot kell megadnia. 

    ![Megtekintése és másolása hello végpont és a Fiókkulcsot hello kapcsolati karakterlánc panelen](./media/create-table-dotnet/keys.png)

3. Hello érték beillesztése hello app.config fájl hello PremiumStorageConnectionString hello értékeként. 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    Hello StandardStorageConnectionString, mert a hagyhatja.

Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 

## <a name="run-hello-web-app"></a>Hello webes alkalmazás futtatása

1. A Visual Studióban, kattintson a jobb gombbal a hello **PremiumTableGetStarted** a projekt **Megoldáskezelőben** majd **NuGet-csomagok kezelése**. 

2. A hello NuGet **Tallózás** mezőbe írja be *windowsazure.Storage kifejezésre-PremiumTable*.

3. Ellenőrizze a hello **közé tartoznak az előzetes** mezőbe. 

4. Hello eredmények közül telepítse a hello **windowsazure.Storage kifejezésre-PremiumTable** könyvtárban. Ez telepíti a hello preview Azure Cosmos DB tábla API csomag, valamint az összes függősége. Vegye figyelembe, hogy a csomag egy másik NuGet hello Windows Azure Storage csomag használják az Azure Table storage-nál. 

5. Kattintson a CTRL + F5 toorun hello alkalmazás.

    hello konzolablak hozzáadott, lekérését, lekérdezése, cseréje és hello táblából törölt hello adatokat jeleníthet meg. Hello parancsfájl befejezése után nyomja le az bármely főbb tooclose hello console ablakban. 
    
    ![Hello gyors üzembe helyezés a konzol kimeneti](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. Ha azt szeretné, hogy toosee hello új entitások Data Explorer, a program.cs fájlban, ezért azok nem törlődnek, 188-208 sorok csak megjegyzésbe futtassa újból hello minta. 

    Most lépjen vissza tooData Explorerben (Megoldáskezelőben) kattintson **frissítése**, bontsa ki a hello **személyek** táblázatban, majd kattintson **entitások**, és majd ezekkel az új adatokkal. 

    ![Új entitások az Adatkezelőben](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello: 

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson a létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy táblát hello adatkezelő használatával, és futtassa az alkalmazást.  Most már tudja kérdezni hello tábla API használata esetén az adatok.  

> [!div class="nextstepaction"]
> [A lekérdezés hello tábla API használatával](tutorial-query-table.md)


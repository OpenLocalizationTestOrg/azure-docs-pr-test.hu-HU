---
title: "Azure Cosmos DB: webalkalmazás fejlesztése Xamarin és Facebook-hitelesítés használatával | Microsoft Docs"
description: "Megadja a .NET kódminta tooconnect tooand használható Azure Cosmos DB lekérdezése"
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
ms.openlocfilehash: 5f71dddd2b2f5bd117e481c96c17915fc58d2119
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a>Azure Cosmos DB: webalkalmazás fejlesztése .NET, Xamarin és Facebook-hitelesítés használatával

Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása. Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése. 

A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon. Kell majd létrehozása és központi telepítése egy hello épülő teendőlistát megvalósító webes alkalmazás [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), és hello Azure Cosmos engedélyezési adatbázismotor. hello teendőlistát megvalósító webes alkalmazás megvalósítja a felhasználói adatok mintát, amely lehetővé teszi a felhasználók toologin Facebook-hitelesítés használatával, és a saját toodo elemek kezelése.

## <a name="prerequisites"></a>Előfeltételek

Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Adatbázisfiók létrehozása

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Gyűjtemény hozzáadása

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a>Klónozza a mintaalkalmazást hello

Most tegyük a githubból, klónozás egy DocumentDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt. Láthatja, milyen egyszerűen adatokkal toowork programozott módon. 

1. Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.  

2. Futtassa a következő parancs tooclone hello minta tárház hello. 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. Ezután nyissa meg a hello DocumentDBTodo.sln fájl, a Visual Studio hello samples/xamarin/UserItems/xamarin.forms mappából. 

## <a name="review-hello-code"></a>Tekintse át a hello kódot

hello Xamarin mappában hello kódot tartalmazza:

* Xamarin-alkalmazás. hello alkalmazás UserItems nevű particionált gyűjtemény hello felhasználói todo elemeket tárolja.
* Az erőforrás-jogkivonat közvetítőjének API-ja. Egy egyszerű ASP.NET Web API toobroker Azure Cosmos DB erőforrás bejelentkezett felhasználók hello alkalmazás toohello jogkivonatokat. Erőforrás-jogkivonatok rövid élettartamú jogkivonatot, amely hello app hello hozzáférés toohello bejelentkezett felhasználó adatait.

hello hitelesítési és adatfolyam mutatja be az alábbi ábrán hello.

* hello UserItems gyűjtemény jön létre a hello partíciós kulcs "/ userid'. Egy gyűjtemény partíciós kulcs lehetővé teszi, hogy Azure Cosmos DB tooscale végtelenül hello felhasználók és az elemek számának megadása növekszik.
* hello Xamarin-alkalmazás lehetővé teszi, hogy a felhasználók toologin Facebook hitelesítő adatokkal.
* hello Xamarin-alkalmazás Facebook access token tooauthenticate ResourceTokenBroker API-t használ.
* hello erőforrás token broker API App Service-hitelesítési funkcióval hello kérelem hitelesíti, és egy Azure Cosmos DB erőforrás-jogkivonat kéri az olvasási/írási hozzáférést tooall dokumentumok megosztása hello hitelesített felhasználó partíciós kulcs.
* Erőforrás-jogkivonat broker hello erőforrás token toohello ügyfélalkalmazás adja vissza.
* hello alkalmazás hozzáfér hello erőforrás tokent hello felhasználói todo elemeket.

![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a>A kapcsolati karakterlánc frissítése

Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.

1. A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**. Hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs hello Web.config fájlba hello következő lépésben fogja használni.

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-xamarin-dotnet/keys.png)

2. Nyissa meg a Visual Studio 2017 hello Web.config fájl hello azure-documentdb-dotnet/minták/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker mappában. 

3. Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello értékének hello accountUrl a Web.config fájlban. 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. Ezután másolja az elsődleges kulcs-érték hello portálról, és teszi hello hello accountKey Web.congif az értékét. 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval. 

## <a name="build-and-deploy-hello-web-app"></a>Hozza létre és hello webalkalmazás telepítése

1. Hello Azure-portálon hozzon létre egy App Service webhely toohost hello erőforrás token broker API.
2. Hello Azure-portálon nyissa meg a hello App beállítások panel hello erőforrás token broker API webhelyet. Töltse ki a következő beállítások hello:

    * accountUrl - hello Azure Cosmos DB fiók URL-címe hello kulcsok lapján Azure Cosmos DB fiókját.
    * accountKey - hello Azure Cosmos DB fiók főkulcs hello kulcsok lapján Azure Cosmos DB fiókját.
    * A létrehozott adatbázis és gyűjtemény databaseId és collectionId értékei

3. Tegye közzé hello ResourceTokenBroker megoldás tooyour webhely létrehozása.

4. Nyissa meg a hello Xamarin-projekthez, és keresse meg a tooTodoItemManager.cs. Töltse ki accountURL, a collectionId, a databaseid megadása, valamint resourceTokenBrokerURL hello értékeinek hello erőforrás token broker webhely hello alap https URL-címet.

5. Teljes hello [hogyan tooconfigure az App Service alkalmazás toouse Facebook bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) oktatóanyag toosetup Facebook hitelesítési és hello ResourceTokenBroker webhely konfigurálása.

    Hello Xamarin-alkalmazás futtatása.

## <a name="review-slas-in-hello-azure-portal"></a>Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello: 

1. A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson az imént létrehozott hello erőforrás hello nevét. 
2. Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.

## <a name="next-steps"></a>Következő lépések

A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello Data Explorer használatával és build és a Xamarin-alkalmazás telepítése. További adatok tooyour Cosmos DB fiókot most importálhatja. 

> [!div class="nextstepaction"]
> [Adatok importálása az Azure Cosmos DB-be](import-data.md)

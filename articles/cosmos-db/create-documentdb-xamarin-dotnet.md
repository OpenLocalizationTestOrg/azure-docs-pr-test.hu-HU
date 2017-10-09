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
# <a name="azure-cosmos-db-build-a-web-app-with-net-xamarin-and-facebook-authentication"></a><span data-ttu-id="3d3a9-103">Azure Cosmos DB: webalkalmazás fejlesztése .NET, Xamarin és Facebook-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="3d3a9-103">Azure Cosmos DB: Build a web app with .NET, Xamarin, and Facebook authentication</span></span>

<span data-ttu-id="3d3a9-104">Az Azure Cosmos DB a Microsoft globálisan elosztott többmodelles adatbázis-szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-104">Azure Cosmos DB is Microsoft’s globally distributed multi-model database service.</span></span> <span data-ttu-id="3d3a9-105">Gyorsan hozzon létre, és a dokumentum, a kulcs/érték és a graph adatbázisok, amelyek kihasználhassa hello globális terjesztési és horizontális skálázhatóságot képességekről az Azure-Cosmos adatbázis hello core lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-105">You can quickly create and query document, key/value, and graph databases, all of which benefit from hello global distribution and horizontal scale capabilities at hello core of Azure Cosmos DB.</span></span> 

<span data-ttu-id="3d3a9-106">A gyors üzembe helyezési bemutatja, hogyan toocreate Azure Cosmos DB fiókkal, a dokumentum-adatbázis és gyűjtemény használja hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-106">This quick start demonstrates how toocreate an Azure Cosmos DB account, document database, and collection using hello Azure portal.</span></span> <span data-ttu-id="3d3a9-107">Kell majd létrehozása és központi telepítése egy hello épülő teendőlistát megvalósító webes alkalmazás [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), és hello Azure Cosmos engedélyezési adatbázismotor.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-107">You'll then build and deploy a todo list web app built on hello [DocumentDB .NET API](documentdb-sdk-dotnet.md), [Xamarin](https://www.xamarin.com/), and hello Azure Cosmos DB authorization engine.</span></span> <span data-ttu-id="3d3a9-108">hello teendőlistát megvalósító webes alkalmazás megvalósítja a felhasználói adatok mintát, amely lehetővé teszi a felhasználók toologin Facebook-hitelesítés használatával, és a saját toodo elemek kezelése.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-108">hello todo list web app implements a per-user data pattern that enables users toologin using Facebook Auth and manage their own toodo items.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d3a9-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3d3a9-109">Prerequisites</span></span>

<span data-ttu-id="3d3a9-110">Ha még nincs telepítve a Visual Studio 2017, töltse le és használja a hello **szabad** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="3d3a9-110">If you don’t already have Visual Studio 2017 installed, you can download and use hello **free** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).</span></span> <span data-ttu-id="3d3a9-111">Győződjön meg arról, hogy engedélyezze **Azure fejlesztési** hello Visual Studio telepítése során.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-111">Make sure that you enable **Azure development** during hello Visual Studio setup.</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a><span data-ttu-id="3d3a9-112">Adatbázisfiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d3a9-112">Create a database account</span></span>

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a><span data-ttu-id="3d3a9-113">Gyűjtemény hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3d3a9-113">Add a collection</span></span>

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

## <a name="clone-hello-sample-application"></a><span data-ttu-id="3d3a9-114">Klónozza a mintaalkalmazást hello</span><span class="sxs-lookup"><span data-stu-id="3d3a9-114">Clone hello sample application</span></span>

<span data-ttu-id="3d3a9-115">Most tegyük a githubból, klónozás egy DocumentDB API app hello kapcsolati karakterlánc beállítása, és futtassa azt.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-115">Now let's clone a DocumentDB API app from github, set hello connection string, and run it.</span></span> <span data-ttu-id="3d3a9-116">Láthatja, milyen egyszerűen adatokkal toowork programozott módon.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-116">You'll see how easy it is toowork with data programmatically.</span></span> 

1. <span data-ttu-id="3d3a9-117">Nyisson meg egy git terminálablakot, például a git bash eszközt, és `cd` tooa munkakönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-117">Open a git terminal window, such as git bash, and `cd` tooa working directory.</span></span>  

2. <span data-ttu-id="3d3a9-118">Futtassa a következő parancs tooclone hello minta tárház hello.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-118">Run hello following command tooclone hello sample repository.</span></span> 

    ```bash
    git clone https://github.com/Azure/azure-documentdb-dotnet.git
    ```

3. <span data-ttu-id="3d3a9-119">Ezután nyissa meg a hello DocumentDBTodo.sln fájl, a Visual Studio hello samples/xamarin/UserItems/xamarin.forms mappából.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-119">Then open hello DocumentDBTodo.sln file from hello samples/xamarin/UserItems/xamarin.forms folder in Visual Studio.</span></span> 

## <a name="review-hello-code"></a><span data-ttu-id="3d3a9-120">Tekintse át a hello kódot</span><span class="sxs-lookup"><span data-stu-id="3d3a9-120">Review hello code</span></span>

<span data-ttu-id="3d3a9-121">hello Xamarin mappában hello kódot tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-121">hello code in hello Xamarin folder contains:</span></span>

* <span data-ttu-id="3d3a9-122">Xamarin-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-122">Xamarin app.</span></span> <span data-ttu-id="3d3a9-123">hello alkalmazás UserItems nevű particionált gyűjtemény hello felhasználói todo elemeket tárolja.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-123">hello app stores hello user's todo items in a partitioned collection named UserItems.</span></span>
* <span data-ttu-id="3d3a9-124">Az erőforrás-jogkivonat közvetítőjének API-ja.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-124">Resource token broker API.</span></span> <span data-ttu-id="3d3a9-125">Egy egyszerű ASP.NET Web API toobroker Azure Cosmos DB erőforrás bejelentkezett felhasználók hello alkalmazás toohello jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-125">A simple ASP.NET Web API toobroker Azure Cosmos DB resource tokens toohello logged in users of hello app.</span></span> <span data-ttu-id="3d3a9-126">Erőforrás-jogkivonatok rövid élettartamú jogkivonatot, amely hello app hello hozzáférés toohello bejelentkezett felhasználó adatait.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-126">Resource tokens are short-lived access tokens that provide hello app with hello access toohello logged in user's data.</span></span>

<span data-ttu-id="3d3a9-127">hello hitelesítési és adatfolyam mutatja be az alábbi ábrán hello.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-127">hello authentication and data flow is illustrated in hello diagram below.</span></span>

* <span data-ttu-id="3d3a9-128">hello UserItems gyűjtemény jön létre a hello partíciós kulcs "/ userid'.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-128">hello UserItems collection is created with hello partition key '/userid'.</span></span> <span data-ttu-id="3d3a9-129">Egy gyűjtemény partíciós kulcs lehetővé teszi, hogy Azure Cosmos DB tooscale végtelenül hello felhasználók és az elemek számának megadása növekszik.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-129">Specifying a partition key for a collection allows Azure Cosmos DB tooscale infinitely as hello number of users and items grows.</span></span>
* <span data-ttu-id="3d3a9-130">hello Xamarin-alkalmazás lehetővé teszi, hogy a felhasználók toologin Facebook hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-130">hello Xamarin app allows users toologin with Facebook credentials.</span></span>
* <span data-ttu-id="3d3a9-131">hello Xamarin-alkalmazás Facebook access token tooauthenticate ResourceTokenBroker API-t használ.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-131">hello Xamarin app uses Facebook access token tooauthenticate with ResourceTokenBroker API</span></span>
* <span data-ttu-id="3d3a9-132">hello erőforrás token broker API App Service-hitelesítési funkcióval hello kérelem hitelesíti, és egy Azure Cosmos DB erőforrás-jogkivonat kéri az olvasási/írási hozzáférést tooall dokumentumok megosztása hello hitelesített felhasználó partíciós kulcs.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-132">hello resource token broker API authenticates hello request using App Service Auth feature, and requests an Azure Cosmos DB resource token with read/write access tooall documents sharing hello authenticated user's partition key.</span></span>
* <span data-ttu-id="3d3a9-133">Erőforrás-jogkivonat broker hello erőforrás token toohello ügyfélalkalmazás adja vissza.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-133">Resource token broker returns hello resource token toohello client app.</span></span>
* <span data-ttu-id="3d3a9-134">hello alkalmazás hozzáfér hello erőforrás tokent hello felhasználói todo elemeket.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-134">hello app accesses hello user's todo items using hello resource token.</span></span>

![Teendőkezelő alkalmazás mintaadatokkal](./media/create-documentdb-xamarin-dotnet/tokenbroker.png)
    
## <a name="update-your-connection-string"></a><span data-ttu-id="3d3a9-136">A kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="3d3a9-136">Update your connection string</span></span>

<span data-ttu-id="3d3a9-137">Most lépjen vissza az Azure portál tooget toohello kapcsolati karakterlánc adatainak és hello alkalmazásba másolja.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-137">Now go back toohello Azure portal tooget your connection string information and copy it into hello app.</span></span>

1. <span data-ttu-id="3d3a9-138">A hello [Azure-portálon](http://portal.azure.com/), az Azure Cosmos DB a fiókot, kattintson a bal oldali navigációs hello **kulcsok**, és kattintson a **írható-olvasható kulcsok**.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-138">In hello [Azure portal](http://portal.azure.com/), in your Azure Cosmos DB account, in hello left navigation click **Keys**, and then click **Read-write Keys**.</span></span> <span data-ttu-id="3d3a9-139">Hello másolási gombok hello jobb oldalán hello képernyő toocopy hello URI és elsődleges kulcs hello Web.config fájlba hello következő lépésben fogja használni.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-139">You'll use hello copy buttons on hello right side of hello screen toocopy hello URI and Primary Key into hello Web.config file in hello next step.</span></span>

    ![Megtekintése és másolása egy hozzáférési kulcsot a hello Azure-portálon, a kulcsok panelen](./media/create-documentdb-xamarin-dotnet/keys.png)

2. <span data-ttu-id="3d3a9-141">Nyissa meg a Visual Studio 2017 hello Web.config fájl hello azure-documentdb-dotnet/minták/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker mappában.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-141">In Visual Studio 2017, open hello Web.config file in hello azure-documentdb-dotnet/samples/xamarin/UserItems/ResourceTokenBroker/ResourceTokenBroker folder.</span></span> 

3. <span data-ttu-id="3d3a9-142">Az URI értéket másol a portálról hello (hello Másolás gombra), és teszi hello értékének hello accountUrl a Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-142">Copy your URI value from hello portal (using hello copy button) and make it hello value of hello accountUrl in Web.config.</span></span> 

    `<add key="accountUrl" value="{Azure Cosmos DB account URL}"/>`

4. <span data-ttu-id="3d3a9-143">Ezután másolja az elsődleges kulcs-érték hello portálról, és teszi hello hello accountKey Web.congif az értékét.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-143">Then copy your PRIMARY KEY value from hello portal and make it hello value of hello accountKey in Web.congif.</span></span> 

    `<add key="accountKey" value="{Azure Cosmos DB secret}"/>`

<span data-ttu-id="3d3a9-144">Most már frissítette az alkalmazást az Azure Cosmos DB toocommunicate szükséges összes hello információval.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-144">You've now updated your app with all hello info it needs toocommunicate with Azure Cosmos DB.</span></span> 

## <a name="build-and-deploy-hello-web-app"></a><span data-ttu-id="3d3a9-145">Hozza létre és hello webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="3d3a9-145">Build and deploy hello web app</span></span>

1. <span data-ttu-id="3d3a9-146">Hello Azure-portálon hozzon létre egy App Service webhely toohost hello erőforrás token broker API.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-146">In hello Azure portal, create an App Service website toohost hello Resource token broker API.</span></span>
2. <span data-ttu-id="3d3a9-147">Hello Azure-portálon nyissa meg a hello App beállítások panel hello erőforrás token broker API webhelyet.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-147">In hello Azure portal, open hello App Settings blade of hello Resource token broker API website.</span></span> <span data-ttu-id="3d3a9-148">Töltse ki a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-148">Fill in hello following app settings:</span></span>

    * <span data-ttu-id="3d3a9-149">accountUrl - hello Azure Cosmos DB fiók URL-címe hello kulcsok lapján Azure Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-149">accountUrl - hello Azure Cosmos DB account URL from hello Keys tab of your Azure Cosmos DB account.</span></span>
    * <span data-ttu-id="3d3a9-150">accountKey - hello Azure Cosmos DB fiók főkulcs hello kulcsok lapján Azure Cosmos DB fiókját.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-150">accountKey - hello Azure Cosmos DB account master key from hello Keys tab of your Azure Cosmos DB account.</span></span>
    * <span data-ttu-id="3d3a9-151">A létrehozott adatbázis és gyűjtemény databaseId és collectionId értékei</span><span class="sxs-lookup"><span data-stu-id="3d3a9-151">databaseId and collectionId of your created database and collection</span></span>

3. <span data-ttu-id="3d3a9-152">Tegye közzé hello ResourceTokenBroker megoldás tooyour webhely létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-152">Publish hello ResourceTokenBroker solution tooyour created website.</span></span>

4. <span data-ttu-id="3d3a9-153">Nyissa meg a hello Xamarin-projekthez, és keresse meg a tooTodoItemManager.cs.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-153">Open hello Xamarin project, and navigate tooTodoItemManager.cs.</span></span> <span data-ttu-id="3d3a9-154">Töltse ki accountURL, a collectionId, a databaseid megadása, valamint resourceTokenBrokerURL hello értékeinek hello erőforrás token broker webhely hello alap https URL-címet.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-154">Fill in hello values for accountURL, collectionId, databaseId, as well as resourceTokenBrokerURL as hello base https url for hello resource token broker website.</span></span>

5. <span data-ttu-id="3d3a9-155">Teljes hello [hogyan tooconfigure az App Service alkalmazás toouse Facebook bejelentkezési](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) oktatóanyag toosetup Facebook hitelesítési és hello ResourceTokenBroker webhely konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-155">Complete hello [How tooconfigure your App Service application toouse Facebook login](../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md) tutorial toosetup Facebook authentication and configure hello ResourceTokenBroker website.</span></span>

    <span data-ttu-id="3d3a9-156">Hello Xamarin-alkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-156">Run hello Xamarin app.</span></span>

## <a name="review-slas-in-hello-azure-portal"></a><span data-ttu-id="3d3a9-157">Tekintse át a szolgáltatásiszint-szerződések a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3d3a9-157">Review SLAs in hello Azure portal</span></span>

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a><span data-ttu-id="3d3a9-158">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="3d3a9-158">Clean up resources</span></span>

<span data-ttu-id="3d3a9-159">Toocontinue toouse az alkalmazás nem fog, ha törli az összes erőforrást hozta létre a gyors üzembe helyezés hello az Azure-portálon az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="3d3a9-159">If you're not going toocontinue toouse this app, delete all resources created by this quickstart in hello Azure portal with hello following steps:</span></span> 

1. <span data-ttu-id="3d3a9-160">A hello hello Azure-portálon a bal oldali menüből, kattintson az **erőforráscsoportok** és kattintson az imént létrehozott hello erőforrás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-160">From hello left-hand menu in hello Azure portal, click **Resource groups** and then click hello name of hello resource you just created.</span></span> 
2. <span data-ttu-id="3d3a9-161">Az erőforrás csoport lapján kattintson a **törlése**, írja be a hello szövegmező hello erőforrás toodelete hello nevét, és kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-161">On your resource group page, click **Delete**, type hello name of hello resource toodelete in hello text box, and then click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d3a9-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d3a9-162">Next steps</span></span>

<span data-ttu-id="3d3a9-163">A gyors üzembe helyezés hogy megtanulta, hogyan toocreate Azure Cosmos DB adatait, hozzon létre egy gyűjteményt hello Data Explorer használatával és build és a Xamarin-alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-163">In this quickstart, you've learned how toocreate an Azure Cosmos DB account, create a collection using hello Data Explorer, and build and deploy a Xamarin app.</span></span> <span data-ttu-id="3d3a9-164">További adatok tooyour Cosmos DB fiókot most importálhatja.</span><span class="sxs-lookup"><span data-stu-id="3d3a9-164">You can now import additional data tooyour Cosmos DB account.</span></span> 

> [!div class="nextstepaction"]
> [<span data-ttu-id="3d3a9-165">Adatok importálása az Azure Cosmos DB-be</span><span class="sxs-lookup"><span data-stu-id="3d3a9-165">Import data into Azure Cosmos DB</span></span>](import-data.md)

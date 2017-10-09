---
title: "aaaGet használatába API Apps és az ASP.NET, az App Service szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate, és felhasználhatják az ASP.NET API-alkalmazás az Azure App Service szolgáltatásban a Visual Studio 2015 használatával."
services: app-service\api
documentationcenter: .net
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: ddc028b2-cde0-4567-a6ee-32cb264a830a
ms.service: app-service-api
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: hero-article
ms.date: 09/20/2016
ms.author: alkarche
ms.openlocfilehash: d3e90f1585907d183b0435c6cafc5585bc1e29ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-api-apps-aspnet-and-swagger-in-azure-app-service"></a><span data-ttu-id="bb124-103">Az Azure App Service szolgáltatásban elérhető API Apps, az ASP.NET és a Swagger használatának megismerése</span><span class="sxs-lookup"><span data-stu-id="bb124-103">Get started with API Apps, ASP.NET, and Swagger in Azure App Service</span></span>
[!INCLUDE [selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="bb124-104">Ez az hello először egy sorozat része oktatóprogramot kínál, amelyek megjelenítése hogyan toouse részeit, az Azure App Service, amelyek hasznosak a fejlesztéséhez és üzemeltetéséhez a RESTful API-kat.</span><span class="sxs-lookup"><span data-stu-id="bb124-104">This is hello first in a series of tutorials that show how toouse features of Azure App Service that are helpful for developing and hosting RESTful APIs.</span></span>  <span data-ttu-id="bb124-105">Ebben az oktatóanyagban Swagger formátumú API-metaadatok használatát vesszük alapul.</span><span class="sxs-lookup"><span data-stu-id="bb124-105">This tutorial covers support for API metadata in Swagger format.</span></span>

<span data-ttu-id="bb124-106">Az oktatóanyagból a következőket sajátíthatja el:</span><span class="sxs-lookup"><span data-stu-id="bb124-106">You'll learn:</span></span>

* <span data-ttu-id="bb124-107">Hogyan toocreate és központi telepítése [API-alkalmazások](app-service-api-apps-why-best-platform.md) az Azure App Service szolgáltatásban a Visual Studio 2015 beépített eszközei segítségével.</span><span class="sxs-lookup"><span data-stu-id="bb124-107">How toocreate and deploy [API apps](app-service-api-apps-why-best-platform.md) in Azure App Service by using tools built into Visual Studio 2015.</span></span>
* <span data-ttu-id="bb124-108">Hogyan tooautomate API felderítési hello a Swashbuckle NuGet csomag toodynamically generálása Swagger API-metaadatok.</span><span class="sxs-lookup"><span data-stu-id="bb124-108">How tooautomate API discovery by using hello Swashbuckle NuGet package toodynamically generate Swagger API metadata.</span></span>
* <span data-ttu-id="bb124-109">Hogyan a Swagger API-metaadatok tooautomatically toouse generálhat ügyfélkódot az API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bb124-109">How toouse Swagger API metadata tooautomatically generate client code for an API app.</span></span>

## <a name="sample-application-overview"></a><span data-ttu-id="bb124-110">Mintaalkalmazás áttekintése</span><span class="sxs-lookup"><span data-stu-id="bb124-110">Sample application overview</span></span>
<span data-ttu-id="bb124-111">Ebben az oktatóanyagban egy egyszerű tennivalólista típusú mintaalkalmazáson keresztül mutatjuk be a szolgáltatás funkcióit.</span><span class="sxs-lookup"><span data-stu-id="bb124-111">In this tutorial, you work with a simple to-do list sample application.</span></span> <span data-ttu-id="bb124-112">hello alkalmazás egyoldalas alkalmazások (SPA) előtér egy ASP.NET Web API középső réteg és egy ASP.NET Web API adatréteggel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="bb124-112">hello application has a single-page application (SPA) front end, an ASP.NET Web API middle tier, and an ASP.NET Web API data tier.</span></span>

![Az API Apps mintaalkalmazás-diagramja](./media/app-service-api-dotnet-get-started/noauthdiagram.png)

<span data-ttu-id="bb124-114">Íme egy képernyőkép hello [AngularJS](https://angularjs.org/) előtér.</span><span class="sxs-lookup"><span data-stu-id="bb124-114">Here's a screen shot of hello [AngularJS](https://angularjs.org/) front end.</span></span>

![API-alkalmazások minta toodo Alkalmazáslista](./media/app-service-api-dotnet-get-started/todospa.png)

<span data-ttu-id="bb124-116">hello Visual Studio megoldás három projektet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="bb124-116">hello Visual Studio solution includes three projects:</span></span>

![](./media/app-service-api-dotnet-get-started/projectsinse.png)

* <span data-ttu-id="bb124-117">**ToDoListAngular** -hello előtér: az AngularJS SPA, amely behívja a középső réteg hello.</span><span class="sxs-lookup"><span data-stu-id="bb124-117">**ToDoListAngular** - hello front end: an AngularJS SPA that calls hello middle tier.</span></span>
* <span data-ttu-id="bb124-118">**ToDoListAPI** -hello középső réteg: egy ASP.NET Web API-projektet, amely behívja hello adatrétegbeli tooperform CRUD-műveleteknek a Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="bb124-118">**ToDoListAPI** - hello middle tier: an ASP.NET Web API project that calls hello data tier tooperform CRUD operations on to-do items.</span></span>
* <span data-ttu-id="bb124-119">**ToDoListDataAPI** -hello adatréteg: egy ASP.NET Web API-projektet, amely elvégzi a CRUD-műveleteknek a Tennivalólista elemein.</span><span class="sxs-lookup"><span data-stu-id="bb124-119">**ToDoListDataAPI** - hello data tier:  an ASP.NET Web API project that performs CRUD operations on to-do items.</span></span>

<span data-ttu-id="bb124-120">hello háromrétegű architektúra csupán egy azok közül, az API Apps segítségével Megvalósíthat, és itt csak bemutató célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="bb124-120">hello three-tier architecture is one of many architectures that you can implement by using API Apps and is used here only for demonstration purposes.</span></span> <span data-ttu-id="bb124-121">az egyes rétegekbe hello kód más dolga, mint lehetséges toodemonstrate API Apps funkcióinak; például hello adatrétegbeli használ adatbázis helyett a kiszolgáló memória adatmegőrzési mechanizmusként.</span><span class="sxs-lookup"><span data-stu-id="bb124-121">hello code in each tier is as simple as possible toodemonstrate API Apps features; for example, hello data tier uses server memory rather than a database as its persistence mechanism.</span></span>

<span data-ttu-id="bb124-122">Az oktatóanyag elvégzését hello két Web API- projektje fog az App Service API apps hello felhőben futó kell.</span><span class="sxs-lookup"><span data-stu-id="bb124-122">On completing this tutorial, you'll have hello two Web API projects up and running in hello cloud in App Service API apps.</span></span>

<span data-ttu-id="bb124-123">hello hello sorozat következő oktatóanyaga hello SPA előtér toohello felhő telepíti.</span><span class="sxs-lookup"><span data-stu-id="bb124-123">hello next tutorial in hello series deploys hello SPA front end toohello cloud.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb124-124">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bb124-124">Prerequisites</span></span>
* <span data-ttu-id="bb124-125">Az ASP.NET Web API - hello oktatóanyagban szereplő utasítások során feltételezzük, hogy a vonatkozó általános ismeretekre ASP.NET toowork [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="bb124-125">ASP.NET Web API - hello tutorial instructions assume you have a basic knowledge of how toowork with ASP.NET [Web API 2](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) in Visual Studio.</span></span>
* <span data-ttu-id="bb124-126">Azure-fiók – [Nyisson ingyenes Azure-fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) vagy [használja ki a Visual Studio-előfizetők számára elérhető előnyöket](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="bb124-126">Azure account - You can [Open an Azure account for free](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) or [Activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>
  
    <span data-ttu-id="bb124-127">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépések tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/).</span><span class="sxs-lookup"><span data-stu-id="bb124-127">If you want tooget started with Azure App Service before you sign up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/).</span></span> <span data-ttu-id="bb124-128">Itt azonnal létrehozhat egy ideiglenes, kezdő szintű alkalmazást az App Service szolgáltatásban – kötelezettségek vállalása, és **bankkártyaadatok megadása nélkül**.</span><span class="sxs-lookup"><span data-stu-id="bb124-128">There, you can immediately create a short-lived starter app in App Service — **no credit card required**, and no commitments.</span></span>
* <span data-ttu-id="bb124-129">A Visual Studio 2015 a következővel hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) -hello SDK telepíti a Visual Studio 2015 automatikusan még nincs.</span><span class="sxs-lookup"><span data-stu-id="bb124-129">Visual Studio 2015 with hello [Azure SDK for .NET](https://azure.microsoft.com/downloads/archive-net-downloads/) - hello SDK installs Visual Studio 2015 automatically if you don't already have it.</span></span>
  
  * <span data-ttu-id="bb124-130">A Visual Studióban kattintson a Súgó -> A Microsoft Visual Studio névjegye elemre, és győződjön meg arról, hogy az Azure App Service Tools 2.9.1-es vagy újabb verziója van telepítve.</span><span class="sxs-lookup"><span data-stu-id="bb124-130">In Visual Studio, click Help -> About Microsoft Visual Studio and ensure that you have "Azure App Service Tools v2.9.1" or higher installed.</span></span>
    
    ![Azure App Tools-verzió](./media/app-service-api-dotnet-get-started/apiversion.png)
    
    > [!NOTE]
    > <span data-ttu-id="bb124-132">Attól függően, hogy hány SDK-függőség hello már van a számítógépen a telepítése hello SDK tooa mint fél óráig vagy tovább néhány perctől akár hosszú ideig eltarthat.</span><span class="sxs-lookup"><span data-stu-id="bb124-132">Depending on how many of hello SDK dependencies you already have on your machine, installing hello SDK could take a long time, from several minutes tooa half hour or more.</span></span>
    > 
    > 

## <a name="download-hello-sample-application"></a><span data-ttu-id="bb124-133">Hello mintaalkalmazás letöltése</span><span class="sxs-lookup"><span data-stu-id="bb124-133">Download hello sample application</span></span>
1. <span data-ttu-id="bb124-134">Töltse le a hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) tárházba.</span><span class="sxs-lookup"><span data-stu-id="bb124-134">Download hello [Azure-Samples/app-service-api-dotnet-to-do-list](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) repository.</span></span>
   
    <span data-ttu-id="bb124-135">Kattinthat a hello **töltse le a ZIP-** hello gombra vagy a klónozott tárház a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bb124-135">You can click hello **Download ZIP** button or clone hello repository on your local machine.</span></span>
2. <span data-ttu-id="bb124-136">Nyissa meg a hello ToDoList megoldást a Visual Studio 2015-öt vagy 2013.</span><span class="sxs-lookup"><span data-stu-id="bb124-136">Open hello ToDoList solution in Visual Studio 2015 or 2013.</span></span>
   
   1. <span data-ttu-id="bb124-137">Szüksége lesz tootrust egyes megoldások.</span><span class="sxs-lookup"><span data-stu-id="bb124-137">You will need tootrust each solution.</span></span>
         <span data-ttu-id="bb124-138">![Biztonsági figyelmeztetés](./media/app-service-api-dotnet-get-started/securitywarning.png)</span><span class="sxs-lookup"><span data-stu-id="bb124-138">![Security Warning](./media/app-service-api-dotnet-get-started/securitywarning.png)</span></span>
3. <span data-ttu-id="bb124-139">Build hello (CTRL + SHIFT + B) megoldás toorestore hello NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="bb124-139">Build hello solution (CTRL + SHIFT + B)  toorestore hello NuGet packages.</span></span>
   
    <span data-ttu-id="bb124-140">Ha szeretné, a művelet toosee hello alkalmazás telepítése előtt, helyileg is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="bb124-140">If you want toosee hello application in operation before you deploy it, you can run it locally.</span></span> <span data-ttu-id="bb124-141">Győződjön meg arról, hogy ToDoListDataAPI a kezdőprojekt és futtatási hello megoldás.</span><span class="sxs-lookup"><span data-stu-id="bb124-141">Make sure that ToDoListDataAPI is your startup project and run hello solution.</span></span> <span data-ttu-id="bb124-142">A böngészőben számíthat toosee egy HTTP 403-as hiba.</span><span class="sxs-lookup"><span data-stu-id="bb124-142">You should expect toosee a HTTP 403 error in your browser.</span></span>

## <a name="use-swagger-api-metadata-and-ui"></a><span data-ttu-id="bb124-143">A Swagger API-metaadatok és felhasználói felület használata</span><span class="sxs-lookup"><span data-stu-id="bb124-143">Use Swagger API metadata and UI</span></span>
<span data-ttu-id="bb124-144">Az Azure App Service beépített támogatást tartalmaz a [Swagger](http://swagger.io/) 2.0 API-metaadatokhoz.</span><span class="sxs-lookup"><span data-stu-id="bb124-144">Support for [Swagger](http://swagger.io/) 2.0 API metadata is built into Azure App Service.</span></span> <span data-ttu-id="bb124-145">Minden API-alkalmazás megadhat egy URL-végpontot, amely Swagger JSON formátumban ad vissza hello API metaadatait.</span><span class="sxs-lookup"><span data-stu-id="bb124-145">Each API app can specify a URL endpoint that returns metadata for hello API in Swagger JSON format.</span></span> <span data-ttu-id="bb124-146">Hello, hogy a végpont által visszaadott metaadatok lehet toogenerate ügyfél használja.</span><span class="sxs-lookup"><span data-stu-id="bb124-146">hello metadata returned from that endpoint can be used toogenerate client code.</span></span>

<span data-ttu-id="bb124-147">Egy ASP.NET Web API-projekt lehet dinamikusan létrehozni a Swagger-metaadatok hello segítségével [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="bb124-147">An ASP.NET Web API project can dynamically generate Swagger metadata by using hello [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet package.</span></span> <span data-ttu-id="bb124-148">hello Swashbuckle NuGet-csomag már telepítve van a hello ToDoListDataAPI és ToDoListAPI projektek letöltött.</span><span class="sxs-lookup"><span data-stu-id="bb124-148">hello Swashbuckle NuGet package is already installed in hello ToDoListDataAPI and ToDoListAPI projects that you downloaded.</span></span>

<span data-ttu-id="bb124-149">Ebben a szakaszban hello oktatóanyag hello generált Swagger 2.0 metaadatok tekinti meg, és próbálkozzon egy próba felhasználói felület hello Swagger-metaadatok alapján.</span><span class="sxs-lookup"><span data-stu-id="bb124-149">In this section of hello tutorial, you look at hello generated Swagger 2.0 metadata, and then you try out a test UI that is based on hello Swagger metadata.</span></span>

1. <span data-ttu-id="bb124-150">Állítsa be a hello ToDoListDataAPI projektet (**nem** hello ToDoListAPI projektet) hello indítási projektként.</span><span class="sxs-lookup"><span data-stu-id="bb124-150">Set hello ToDoListDataAPI project (**not** hello ToDoListAPI project) as hello startup project.</span></span>
   
    ![A ToDoDataAPI beállítása kezdőprojektként](./media/app-service-api-dotnet-get-started/startupproject.png)
2. <span data-ttu-id="bb124-152">Nyomja le az F5 billentyűt, vagy kattintson a **Debug > Start Debugging** toorun hello projekt hibakeresési módban.</span><span class="sxs-lookup"><span data-stu-id="bb124-152">Press F5 or click **Debug > Start Debugging** toorun hello project in debug mode.</span></span>
   
    <span data-ttu-id="bb124-153">hello böngészőben megnyílik, és HTTP 403 Hibaoldal hello mutatja.</span><span class="sxs-lookup"><span data-stu-id="bb124-153">hello browser opens and shows hello HTTP 403 error page.</span></span>
3. <span data-ttu-id="bb124-154">A böngésző címsorában adja hozzá `swagger/docs/v1` hello sort, és nyomja le az ENTER toohello végét.</span><span class="sxs-lookup"><span data-stu-id="bb124-154">In your browser address bar, add `swagger/docs/v1` toohello end of hello line, and then press Return.</span></span> <span data-ttu-id="bb124-155">(URL-cím hello `http://localhost:45914/swagger/docs/v1`.)</span><span class="sxs-lookup"><span data-stu-id="bb124-155">(hello URL is `http://localhost:45914/swagger/docs/v1`.)</span></span>
   
    <span data-ttu-id="bb124-156">Ez a hello alapértelmezett URL-cím hello API Swashbuckle tooreturn Swagger 2.0 JSON-metaadatok által használt.</span><span class="sxs-lookup"><span data-stu-id="bb124-156">This is hello default URL used by Swashbuckle tooreturn Swagger 2.0 JSON metadata for hello API.</span></span>
   
    <span data-ttu-id="bb124-157">Ha az Internet Explorer használata esetén hello böngésző felszólítja toodownload egy *v1.json* fájlt.</span><span class="sxs-lookup"><span data-stu-id="bb124-157">If you're using Internet Explorer, hello browser prompts you toodownload a *v1.json* file.</span></span>
   
    ![JSON-metaadatok letöltése az Internet Explorerben](./media/app-service-api-dotnet-get-started/iev1json.png)
   
    <span data-ttu-id="bb124-159">Ha Chrome, Firefox vagy Microsoft Edge használata esetén hello böngésző hello JSON hello böngészőablakban jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-159">If you're using Chrome, Firefox, or Edge, hello browser displays hello JSON in hello browser window.</span></span> <span data-ttu-id="bb124-160">A különböző böngészők eltérően kezelik a JSON és a böngészőablakot nem hasonlíthat pontosan hello példa.</span><span class="sxs-lookup"><span data-stu-id="bb124-160">Different browsers handle JSON differently, and your browser window may not look exactly like hello example.</span></span>
   
    ![JSON-metaadatok a Chrome-ban](./media/app-service-api-dotnet-get-started/chromev1json.png)
   
    <span data-ttu-id="bb124-162">hello következő példa bemutatja hello Swagger-metaadatokat az API-t hello első szakasza hello hello hello-definícióval Get metódus.</span><span class="sxs-lookup"><span data-stu-id="bb124-162">hello following sample shows hello first section of hello Swagger metadata for hello API, with hello definition for hello Get method.</span></span> <span data-ttu-id="bb124-163">A metaadatok milyen meghajtók hello Swagger felhasználói Felületet, hogy a lépéseket követve hello segítségével, és egy későbbi szakasz ismerteti a hello oktatóanyag tooautomatically használhatja az Ügyfélkód generálásához.</span><span class="sxs-lookup"><span data-stu-id="bb124-163">This metadata is what drives hello Swagger UI that you use in hello following steps, and you use it in a later section of hello tutorial tooautomatically generate client code.</span></span>
   
        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ToDoListDataAPI"
          },
          "host": "localhost:45914",
          "schemes": [ "http" ],
          "paths": {
            "/api/ToDoList": {
              "get": {
                "tags": [ "ToDoList" ],
                "operationId": "ToDoList_GetByOwner",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "parameters": [
                  {
                    "name": "owner",
                    "in": "query",
                    "required": true,
                    "type": "string"
                  }
                ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/ToDoItem" }
                    }
                  }
                },
                "deprecated": false
              },
4. <span data-ttu-id="bb124-164">Zárja be hello böngészőt, és állítsa le a Visual Studio hibakeresési módját.</span><span class="sxs-lookup"><span data-stu-id="bb124-164">Close hello browser and stop Visual Studio debugging.</span></span>
5. <span data-ttu-id="bb124-165">A projekt hello ToDoListDataAPI **Megoldáskezelőben**, nyissa meg hello *App_Start\SwaggerConfig.cs* fájlt, majd görgessen lefelé tooline 174, és állítsa vissza a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="bb124-165">In hello ToDoListDataAPI project in **Solution Explorer**, open hello *App_Start\SwaggerConfig.cs* file, then scroll down tooline 174 and uncomment hello following code.</span></span>
   
        /*
            })
        .EnableSwaggerUi(c =>
            {
        */
   
    <span data-ttu-id="bb124-166">Hello *SwaggerConfig.cs* fájl jön létre a projektben hello Swashbuckle csomag telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="bb124-166">hello *SwaggerConfig.cs* file is created when you install hello Swashbuckle package in a project.</span></span> <span data-ttu-id="bb124-167">hello fájl számos módon tooconfigure Swashbuckle.</span><span class="sxs-lookup"><span data-stu-id="bb124-167">hello file provides a number of ways tooconfigure Swashbuckle.</span></span>
   
    <span data-ttu-id="bb124-168">hello kódrész lehetővé teszi, hogy hello Swagger felhasználói felület, a következő hello használó lépések.</span><span class="sxs-lookup"><span data-stu-id="bb124-168">hello code you've uncommented enables hello Swagger UI that you use in hello following steps.</span></span> <span data-ttu-id="bb124-169">Amikor hello API-alkalmazás projektsablon használatával hoz létre Web API-projektet, ez a kód megjegyzésbe van alapértelmezés szerint biztonsági intézkedésként.</span><span class="sxs-lookup"><span data-stu-id="bb124-169">When you create a Web API project by using hello API app project template, this code is commented out by default as a security measure.</span></span>
6. <span data-ttu-id="bb124-170">Futtassa újra a hello projektet.</span><span class="sxs-lookup"><span data-stu-id="bb124-170">Run hello project again.</span></span>
7. <span data-ttu-id="bb124-171">A böngésző címsorában adja hozzá `swagger` hello sort, és nyomja le az ENTER toohello végét.</span><span class="sxs-lookup"><span data-stu-id="bb124-171">In your browser address bar, add `swagger` toohello end of hello line, and then press Return.</span></span> <span data-ttu-id="bb124-172">(URL-cím hello `http://localhost:45914/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="bb124-172">(hello URL is `http://localhost:45914/swagger`.)</span></span>
8. <span data-ttu-id="bb124-173">Ha hello Swagger felhasználói felületben kattintson **ToDoList** toosee hello módszer áll rendelkezésre.</span><span class="sxs-lookup"><span data-stu-id="bb124-173">When hello Swagger UI page appears, click **ToDoList** toosee hello methods available.</span></span>
   
    ![Swagger felhasználói felület, elérhető metódusok](./media/app-service-api-dotnet-get-started/methods.png)
9. <span data-ttu-id="bb124-175">Először kattintson hello **beolvasása** hello listában gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-175">Click hello first **Get** button in hello list.</span></span>
10. <span data-ttu-id="bb124-176">A hello **paraméterek** területen írjon be csillag hello hello értékeként `owner` paraméter, és kattintson **próbálja ki**.</span><span class="sxs-lookup"><span data-stu-id="bb124-176">In hello **Parameters** section, enter an asterisk as hello value of hello `owner` parameter, and then click **Try it out**.</span></span>
    
    <span data-ttu-id="bb124-177">Hitelesítés a későbbi részei hozzáadásakor a hello középső réteg biztosítani fogja hello tényleges felhasználói Azonosítót toohello adatrétegbeli.</span><span class="sxs-lookup"><span data-stu-id="bb124-177">When you add authentication in later tutorials, hello middle tier will provide hello actual user ID toohello data tier.</span></span> <span data-ttu-id="bb124-178">Most ezért minden feladatnál csillag helyettesítő a Tulajdonosazonosítót közben hello alkalmazás hitelesítés nélkül fut.</span><span class="sxs-lookup"><span data-stu-id="bb124-178">For now, all tasks will have asterisk as their owner ID while hello application runs without authentication enabled.</span></span>
    
    ![Swagger felhasználói felület, kipróbálás funkció](./media/app-service-api-dotnet-get-started/gettryitout1.png)
    
    <span data-ttu-id="bb124-180">Swagger felhasználói felület hello hello ToDoList Get metódust hívja, és hello válaszkódot és a JSON-eredményeket jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-180">hello Swagger UI calls hello ToDoList Get method and displays hello response code and JSON results.</span></span>
    
    ![Swagger felhasználói felület, kipróbálás, eredmények](./media/app-service-api-dotnet-get-started/gettryitout.png)
11. <span data-ttu-id="bb124-182">Kattintson a **Post**, és kattintson a hello csoportban **Modellsémát**.</span><span class="sxs-lookup"><span data-stu-id="bb124-182">Click **Post**, and then click hello box under **Model Schema**.</span></span>
    
    <span data-ttu-id="bb124-183">Kattintson a hello modell séma prefills hello beviteli mezőt hello paraméter értéke Post metódussal hello megadására.</span><span class="sxs-lookup"><span data-stu-id="bb124-183">Clicking hello model schema prefills hello input box where you can specify hello parameter value for hello Post method.</span></span> <span data-ttu-id="bb124-184">(Ha ez sem működik, az Internet Explorerben, próbálkozzon másik böngészővel, vagy manuálisan adja meg hello paraméter értéke hello következő lépésben.)</span><span class="sxs-lookup"><span data-stu-id="bb124-184">(If this doesn't work in Internet Explorer, use a different browser or enter hello parameter value manually in hello next step.)</span></span>  
    
    ![Swagger felhasználói felület, kipróbálás funkció, Post metódus](./media/app-service-api-dotnet-get-started/post.png)
12. <span data-ttu-id="bb124-186">Változás hello JSON a hello `todo` paraméter beviteli mezőjében úgy, hogy a következő példa hello úgy tűnik, vagy helyettesítse a saját leíró szöveg:</span><span class="sxs-lookup"><span data-stu-id="bb124-186">Change hello JSON in hello `todo` parameter input box so that it looks like hello following example, or substitute your own description text:</span></span>
    
        {
          "ID": 2,
          "Description": "buy hello dog a toy",
          "Owner": "*"
        }
13. <span data-ttu-id="bb124-187">Kattintson a **Try it out** (Kipróbálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="bb124-187">Click **Try it out**.</span></span>
    
    <span data-ttu-id="bb124-188">hello ToDoList API sikerességét jelző HTTP 204-es válasz kódot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="bb124-188">hello ToDoList API returns an HTTP 204 response code that indicates success.</span></span>
14. <span data-ttu-id="bb124-189">Először kattintson hello **beolvasása** gombra, majd hello oldal ezen részében hello **próbálja ki** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-189">Click hello first **Get** button, and then in that section of hello page click hello **Try it out** button.</span></span>
    
    <span data-ttu-id="bb124-190">hello Get metódusra adott válasz mostantól tartalmazza a hello új toodo elemet.</span><span class="sxs-lookup"><span data-stu-id="bb124-190">hello Get method response now includes hello new toodo item.</span></span>
15. <span data-ttu-id="bb124-191">Nem kötelező: Is hello Put, Delete, próbálja meg, és azonosító módszerekkel beolvasása.</span><span class="sxs-lookup"><span data-stu-id="bb124-191">Optional: Try also hello Put, Delete, and Get by ID methods.</span></span>
16. <span data-ttu-id="bb124-192">Zárja be hello böngészőt, és állítsa le a Visual Studio hibakeresési módját.</span><span class="sxs-lookup"><span data-stu-id="bb124-192">Close hello browser and stop Visual Studio debugging.</span></span>

<span data-ttu-id="bb124-193">A Swashbuckle bármelyik ASP.NET Web API-projekttel működik.</span><span class="sxs-lookup"><span data-stu-id="bb124-193">Swashbuckle works with any ASP.NET Web API project.</span></span> <span data-ttu-id="bb124-194">Ha tooadd Swagger metaadatainak generálása tooan meglévő projektet, egyszerűen telepítse hello Swashbuckle csomagot.</span><span class="sxs-lookup"><span data-stu-id="bb124-194">If you want tooadd Swagger metadata generation tooan existing project, just install hello Swashbuckle package.</span></span>

> [!NOTE]
> <span data-ttu-id="bb124-195">A Swagger-metaadatokban minden API-művelet saját egyedi azonosítót kap.</span><span class="sxs-lookup"><span data-stu-id="bb124-195">Swagger metadata includes a unique ID for each API operation.</span></span> <span data-ttu-id="bb124-196">Alapértelmezés szerint előfordulhat, hogy a Swashbuckle duplikált Swagger-műveleti azonosítókat hoz létre a Web API-vezérlő metódusokhoz.</span><span class="sxs-lookup"><span data-stu-id="bb124-196">By default, Swashbuckle may generate duplicate Swagger operation IDs for your Web API controller methods.</span></span> <span data-ttu-id="bb124-197">Ez akkor fordul elő, ha a vezérlőben túlterhelt HTTP-metódusok találhatók, például: `Get()` vagy `Get(id)`.</span><span class="sxs-lookup"><span data-stu-id="bb124-197">This happens if your controller has overloaded HTTP methods, such as `Get()` and `Get(id)`.</span></span> <span data-ttu-id="bb124-198">Hogyan toohandle túlterhelések kapcsolatos információkért lásd: [testreszabása Swashbuckle által generált API-definíciók](app-service-api-dotnet-swashbuckle-customize.md).</span><span class="sxs-lookup"><span data-stu-id="bb124-198">For information about how toohandle overloads, see [Customize Swashbuckle-generated API definitions](app-service-api-dotnet-swashbuckle-customize.md).</span></span> <span data-ttu-id="bb124-199">Ha Web API-projektet a Visual Studio hello Azure API App sablon segítségével hoz létre, egyedi műveleti azonosítókat generáló kódot automatikusan hozzáadott toohello *SwaggerConfig.cs* fájlt.</span><span class="sxs-lookup"><span data-stu-id="bb124-199">If you create a Web API project in Visual Studio by using hello Azure API App template, code that generates unique operation IDs is automatically added toohello *SwaggerConfig.cs* file.</span></span>  
> 
> 

## <span data-ttu-id="bb124-200"><a id="createapiapp"></a>API-alkalmazás létrehozása az Azure-ban, és a kód tooit telepítése</span><span class="sxs-lookup"><span data-stu-id="bb124-200"><a id="createapiapp"></a> Create an API app in Azure and deploy code tooit</span></span>
<span data-ttu-id="bb124-201">Ebben a szakaszban használhatja az Azure-eszközök Visual Studio hello integrált **webhely közzététele** varázsló toocreate egy olyan új API alkalmazást az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="bb124-201">In this section, you use Azure tools that are integrated into hello Visual Studio **Publish Web** wizard toocreate a new API app in Azure.</span></span> <span data-ttu-id="bb124-202">Ezután hello ToDoListDataAPI projektet toohello új API alkalmazást, és hello API hívása hello Swagger felhasználói felület futtatásával.</span><span class="sxs-lookup"><span data-stu-id="bb124-202">Then you deploy hello ToDoListDataAPI project toohello new API app and call hello API by running hello Swagger UI.</span></span>

1. <span data-ttu-id="bb124-203">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello ToDoListDataAPI projektet, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="bb124-203">In **Solution Explorer**, right-click hello ToDoListDataAPI project, and then click **Publish**.</span></span>
   
    ![A Publish (Közzététel) elem a Visual Studióban](./media/app-service-api-dotnet-get-started/pubinmenu.png)
2. <span data-ttu-id="bb124-205">A hello **profil** hello lépését **webhely közzététele** varázsló, kattintson a **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="bb124-205">In hello **Profile** step of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
   
   ![Az Azure App Service elem kiválasztása a Publish Web (Weboldal közzététele) varázslóban](./media/app-service-api-dotnet-get-started/selectappservice.png)
3. <span data-ttu-id="bb124-207">Jelentkezzen be Azure-fiók tooyour, ha még nem tette meg, vagy frissítse a hitelesítő adatokat, ha kijelentkeztette.</span><span class="sxs-lookup"><span data-stu-id="bb124-207">Sign in tooyour Azure account if you have not already done so, or refresh your credentials if they're expired.</span></span>
4. <span data-ttu-id="bb124-208">Az App Service párbeszédpanelen hello, válassza a hello Azure **előfizetés** toouse szeretne, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="bb124-208">In hello App Service dialog box, choose hello Azure **Subscription** you want toouse, and then click **New**.</span></span>
   
    ![A New (Új) elem kiválasztása az App Service párbeszédpanelen](./media/app-service-api-dotnet-get-started/clicknew.png)
   
    <span data-ttu-id="bb124-210">Hello **üzemeltetési** hello lapján **létrehozása az App Service** párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-210">hello **Hosting** tab of hello **Create App Service** dialog box appears.</span></span>
   
    <span data-ttu-id="bb124-211">Mivel, amelyhez telepített Swashbuckle tartozik Web API-projektet telepít, a Visual Studio feltételezi, hogy toocreate API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bb124-211">Because you're deploying a Web API project that has Swashbuckle installed, Visual Studio assumes that you want toocreate an API App.</span></span> <span data-ttu-id="bb124-212">Ez jelzi hello **API App Name** title és által hello tényt, hogy hello **típusának módosítása** legördülő lista túl van-e állítva**API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bb124-212">This is indicated by hello **API App Name** title and by hello fact that hello **Change Type** drop-down list is set too**API App**.</span></span>
   
    ![Alkalmazás típusának megadása az App Service párbeszédpanelen](./media/app-service-api-dotnet-get-started/apptype.png)
5. <span data-ttu-id="bb124-214">Adjon meg egy **API App Name** a hello egyedi *azurewebsites.net* tartomány.</span><span class="sxs-lookup"><span data-stu-id="bb124-214">Enter an **API App Name** that is unique in hello *azurewebsites.net* domain.</span></span> <span data-ttu-id="bb124-215">Elfogadhatja a Visual Studio által ajánlott hello alapértelmezett nevet.</span><span class="sxs-lookup"><span data-stu-id="bb124-215">You can accept hello default name that Visual Studio proposes.</span></span>
   
    <span data-ttu-id="bb124-216">Ha megad egy nevet, hogy valaki más már használatban van, megjelenik egy piros felkiáltójel toohello jobbra.</span><span class="sxs-lookup"><span data-stu-id="bb124-216">If you enter a name that someone else has already used, you see a red exclamation mark toohello right.</span></span>
   
    <span data-ttu-id="bb124-217">hello hello API-alkalmazás URL-CÍMÉT lesz `{API app name}.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="bb124-217">hello URL of hello API app will be `{API app name}.azurewebsites.net`.</span></span>
6. <span data-ttu-id="bb124-218">A hello **erőforráscsoport** legördülő menüben kattintson **új**, majd adja meg "ToDoListGroup" nevet, vagy ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="bb124-218">In hello **Resource Group** drop-down, click **New**, and then enter "ToDoListGroup" or another name if you prefer.</span></span>
   
    <span data-ttu-id="bb124-219">Az erőforráscsoportok Azure-erőforrások (például API Apps, adatbázisok, virtuális gépek stb.) gyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="bb124-219">A resource group is a collection of Azure resources such as API apps, databases, VMs, and so forth.</span></span>    <span data-ttu-id="bb124-220">Ebben az oktatóanyagban esetén ajánlott toocreate egy új erőforráscsoportot mivel így később könnyen toodelete összes hello Azure-erőforrások hello az oktatóanyaghoz létrehozott egy lépésben.</span><span class="sxs-lookup"><span data-stu-id="bb124-220">For this tutorial, it's best toocreate a new resource group because that makes it easy toodelete in one step all hello Azure resources that you create for hello tutorial.</span></span>
   
    <span data-ttu-id="bb124-221">Ebben a mezőben kiválaszthat egy meglévő [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md), vagy újat is létrehozhat. Új csoport létrehozásához írjon be egy olyan nevet, amely eltér az előfizetéshez tartozó többi erőforráscsoport nevétől.</span><span class="sxs-lookup"><span data-stu-id="bb124-221">This box lets you select an existing [resource group](../azure-resource-manager/resource-group-overview.md) or create a new one by typing in a name that is different from any existing resource group in your subscription.</span></span>
7. <span data-ttu-id="bb124-222">Kattintson a hello **új** gomb következő toohello **App Service-csomag** legördülő listán.</span><span class="sxs-lookup"><span data-stu-id="bb124-222">Click hello **New** button next toohello **App Service Plan** drop-down.</span></span>
   
    <span data-ttu-id="bb124-223">hello a képernyőfelvételen látható tartalmaz **API App Name**, **előfizetés**, és **erőforráscsoport** --a eltérőek lesznek.</span><span class="sxs-lookup"><span data-stu-id="bb124-223">hello screen shot shows sample values for **API App Name**, **Subscription**, and **Resource Group** -- your values will be different.</span></span>
   
    ![A Create App Service (App Service létrehozása) párbeszédpanel](./media/app-service-api-dotnet-get-started/createas.png)
   
    <span data-ttu-id="bb124-225">Az alábbi lépésekkel hello hello új erőforráscsoporthoz tartozó App Service-csomagot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="bb124-225">In hello following steps you create an App Service plan for hello new resource group.</span></span> <span data-ttu-id="bb124-226">Az App Service-csomag határozza meg, amely az API-alkalmazás fut hello számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="bb124-226">An App Service plan specifies hello compute resources that your API app runs on.</span></span> <span data-ttu-id="bb124-227">Például ha hello ingyenes csomagot választja, az API-alkalmazás fut a megosztott virtuális gépeken, míg egyes fizetett szinteken dedikált virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="bb124-227">For example, if you choose hello free tier, your API app runs on shared VMs, while for some paid tiers it runs on dedicated VMs.</span></span> <span data-ttu-id="bb124-228">Az App Service-csomagokkal kapcsolatban további információkért lásd a következő cikket: [App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) (App Service-csomagok áttekintése).</span><span class="sxs-lookup"><span data-stu-id="bb124-228">For information about App Service plans, see [App Service plans overview](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).</span></span>
8. <span data-ttu-id="bb124-229">A hello **konfigurálása App Service-csomag** párbeszédpanelen adja meg "ToDoListPlan", vagy egy másik nevet, ha szeretné.</span><span class="sxs-lookup"><span data-stu-id="bb124-229">In hello **Configure App Service Plan** dialog, enter "ToDoListPlan" or another name if you prefer.</span></span>
9. <span data-ttu-id="bb124-230">A hello **hely** legördülő menüben válassza ki a legközelebbi tooyou hello helyre.</span><span class="sxs-lookup"><span data-stu-id="bb124-230">In hello **Location** drop-down list, choose hello location that is closest tooyou.</span></span>
   
    <span data-ttu-id="bb124-231">Ez a beállítás azt határozza meg, hogy melyik Azure-adatközpontban fog futni az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb124-231">This setting specifies which Azure datacenter your app will run in.</span></span> <span data-ttu-id="bb124-232">Válasszon egy helyet, zárja be tooyou toominimize [késés](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).</span><span class="sxs-lookup"><span data-stu-id="bb124-232">Choose a location close tooyou toominimize [latency](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090).</span></span>
10. <span data-ttu-id="bb124-233">A hello **mérete** legördülő menüben kattintson **szabad**.</span><span class="sxs-lookup"><span data-stu-id="bb124-233">In hello **Size** drop-down, click **Free**.</span></span>
    
    <span data-ttu-id="bb124-234">Ebben az oktatóanyagban hello szabad IP-címek is megfelelő teljesítményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="bb124-234">For this tutorial, hello free pricing tier will provide sufficient performance.</span></span>
11. <span data-ttu-id="bb124-235">A hello **konfigurálása App Service-csomag** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb124-235">In hello **Configure App Service Plan** dialog, click **OK**.</span></span>
    
    ![Az OK gombra kattintás a Configure App Service Plan (App Service-csomag konfigurálása) párbeszédpanelen](./media/app-service-api-dotnet-get-started/configasp.png)
12. <span data-ttu-id="bb124-237">A hello **létrehozása az App Service** párbeszédpanel, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="bb124-237">In hello **Create App Service** dialog box, click **Create**.</span></span>
    
    ![A Create (Létrehozás) gombra kattintás a Create App Service (App Service létrehozása) párbeszédpanelen](./media/app-service-api-dotnet-get-started/clickcreate.png)
    
    <span data-ttu-id="bb124-239">A Visual Studio létrehozza a hello API-alkalmazást és egy közzétételi profilt, amely rendelkezik az összes szükséges hello beállítások hello API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb124-239">Visual Studio creates hello API app and a publish profile that has all of hello required settings for hello API app.</span></span> <span data-ttu-id="bb124-240">Ezt követően megnyitja hello **webhely közzététele** varázslót, amely azt ismertetjük toodeploy hello projekt.</span><span class="sxs-lookup"><span data-stu-id="bb124-240">Then it opens hello **Publish Web** wizard, which you'll use toodeploy hello project.</span></span>
    
    <span data-ttu-id="bb124-241">Hello **webhely közzététele** megnyílik a varázsló hello **kapcsolat** (lásd alább) fülre.</span><span class="sxs-lookup"><span data-stu-id="bb124-241">hello **Publish Web** wizard opens on hello **Connection** tab (shown below).</span></span>
    
    <span data-ttu-id="bb124-242">A hello **kapcsolat** lap hello **Server** és **helynév** beállítások pont tooyour API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="bb124-242">On hello **Connection** tab, hello **Server** and **Site name** settings point tooyour API app.</span></span> <span data-ttu-id="bb124-243">Hello **felhasználónév** és **jelszó** által Azure létrehozott telepítési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="bb124-243">hello **User name** and **Password** are deployment credentials that Azure creates for you.</span></span> <span data-ttu-id="bb124-244">A központi telepítést követően megnyílik a Visual Studio egy böngésző toohello **URL-címre** (célja hello csak a **cél URL-címe**).</span><span class="sxs-lookup"><span data-stu-id="bb124-244">After deployment, Visual Studio opens a browser toohello **Destination URL** (that's hello only purpose for **Destination URL**).</span></span>  
13. <span data-ttu-id="bb124-245">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-245">Click **Next**.</span></span>
    
    ![Kattintás a Next (Tovább) gombra a Publish Web (Weboldal közzététele) varázsló Connection (Kapcsolat) lapján](./media/app-service-api-dotnet-get-started/connnext.png)
    
    <span data-ttu-id="bb124-247">hello következő lap hello **beállítások** (lásd alább) fülre.</span><span class="sxs-lookup"><span data-stu-id="bb124-247">hello next tab is hello **Settings** tab (shown below).</span></span> <span data-ttu-id="bb124-248">Itt módosíthatja hello build konfigurációs lapon toodeploy hibakeresési buildet [távoli hibakeresés](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).</span><span class="sxs-lookup"><span data-stu-id="bb124-248">Here you can change hello build configuration tab toodeploy a debug build for [remote debugging](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug).</span></span> <span data-ttu-id="bb124-249">hello lapon is biztosít, több **Fájlközzétételi beállítás**:</span><span class="sxs-lookup"><span data-stu-id="bb124-249">hello tab also offers several **File Publish Options**:</span></span>
    
    * <span data-ttu-id="bb124-250">Remove additional files at destination (További fájlok eltávolítása a célhelyen)</span><span class="sxs-lookup"><span data-stu-id="bb124-250">Remove additional files at destination</span></span>
    * <span data-ttu-id="bb124-251">Precompile during publishing (Előfordítás a közzététel során)</span><span class="sxs-lookup"><span data-stu-id="bb124-251">Precompile during publishing</span></span>
    * <span data-ttu-id="bb124-252">Fájlok kizárása hello App_Data mappájában</span><span class="sxs-lookup"><span data-stu-id="bb124-252">Exclude files from hello App_Data folder</span></span>
    
    <span data-ttu-id="bb124-253">Ehhez az oktatóanyagokhoz ezek egyikét sem kell használnia.</span><span class="sxs-lookup"><span data-stu-id="bb124-253">For this tutorial you don't need any of these.</span></span> <span data-ttu-id="bb124-254">Ezek használatáról a következő cikkben talál részletes leírást: [How to: Deploy a Web Project Using One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx) (Útmutató: Webes projekt telepítése a Visual Studio Közzététel egyetlen kattintással funkciójával).</span><span class="sxs-lookup"><span data-stu-id="bb124-254">For detailed explanations of what they do, see [How to: Deploy a Web Project Using One-Click Publish in Visual Studio](https://msdn.microsoft.com/library/dd465337.aspx).</span></span>
14. <span data-ttu-id="bb124-255">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-255">Click **Next**.</span></span>
    
    ![Kattintás a Next (Tovább) gombra a Publish Web (Weboldal közzététele) varázsló Settings (Beállítások) lapján](./media/app-service-api-dotnet-get-started/settingsnext.png)
    
    <span data-ttu-id="bb124-257">Ezután a hello van **előzetes** lapon (lásd alább), amely lehetővé teszi az olyan lehetőség toosee, mely fájlokat fogja másolni a projekt toohello API-alkalmazás toobe.</span><span class="sxs-lookup"><span data-stu-id="bb124-257">Next is hello **Preview** tab (shown below), which gives you an opportunity toosee what files are going toobe copied from your project toohello API app.</span></span> <span data-ttu-id="bb124-258">Ha egy már telepített tooearlier projekt tooan API-alkalmazást telepít, csak a módosult fájlokat másolja.</span><span class="sxs-lookup"><span data-stu-id="bb124-258">When you're deploying a project tooan API app that you already deployed tooearlier, only changed files are copied.</span></span> <span data-ttu-id="bb124-259">Ha azt szeretné, hogy toosee másolandó listáját, kattintson a hello **Start Preview** gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-259">If you want toosee a list of what will be copied, you can click hello **Start Preview** button.</span></span>
15. <span data-ttu-id="bb124-260">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-260">Click **Publish**.</span></span>
    
    ![Kattintás a Publish (Közzététel) gombra a Publish Web (Webes közzététel) varázsló Preview (Előnézet) lapján](./media/app-service-api-dotnet-get-started/clickpublish.png)
    
    <span data-ttu-id="bb124-262">Visual Studio hello ToDoListDataAPI projektet toohello új API alkalmazást telepíti.</span><span class="sxs-lookup"><span data-stu-id="bb124-262">Visual Studio deploys hello ToDoListDataAPI project toohello new API app.</span></span> <span data-ttu-id="bb124-263">Hello **kimeneti** ablak naplózza a sikeres telepítést, és a "sikeres létrehozásról" tájékoztató oldal jelenik meg a böngészőben megnyílik toohello URL-címében hello API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb124-263">hello **Output** window logs successful deployment, and a "successfully created" page appears in a browser window opened toohello URL of hello API app.</span></span>
    
    ![Output (Kimenet) ablak, sikeres telepítés](./media/app-service-api-dotnet-get-started/deploymentoutput.png)
    
    ![Az új API-alkalmazás sikeres létrehozásáról tájékoztató oldal](./media/app-service-api-dotnet-get-started/appcreated.png)
16. <span data-ttu-id="bb124-266">Hello böngésző címsorában adja hozzá "swagger" toohello URL-címet, majd nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="bb124-266">Add "swagger" toohello URL in hello browser's address bar, and then press Enter.</span></span> <span data-ttu-id="bb124-267">(URL-cím hello `http://{apiappname}.azurewebsites.net/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="bb124-267">(hello URL is `http://{apiappname}.azurewebsites.net/swagger`.)</span></span>
    
    <span data-ttu-id="bb124-268">hello böngésző Swagger felhasználói felület, amely a korábban, de most fut. hello felhő hello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-268">hello browser displays hello same Swagger UI that you saw earlier, but now it's running in hello cloud.</span></span> <span data-ttu-id="bb124-269">Próbálja ki hello Get metódust, és láthatja, hogy Ön hátsó toohello 2 alapértelmezett tennivaló.</span><span class="sxs-lookup"><span data-stu-id="bb124-269">Try out hello Get method, and you see that you're back toohello default 2 to-do items.</span></span> <span data-ttu-id="bb124-270">hello elvégzett módosításokat hello helyi gép memóriájába mentette.</span><span class="sxs-lookup"><span data-stu-id="bb124-270">hello changes you made earlier were saved in memory in hello local machine.</span></span>
17. <span data-ttu-id="bb124-271">Nyissa meg hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="bb124-271">Open hello [Azure portal](https://portal.azure.com/).</span></span>
    
    <span data-ttu-id="bb124-272">hello Azure portál egy webes felület, például az API-alkalmazások kezelése az Azure erőforrások.</span><span class="sxs-lookup"><span data-stu-id="bb124-272">hello Azure portal is a web interface for managing Azure resources such as API apps.</span></span>
18. <span data-ttu-id="bb124-273">Kattintson a **További szolgáltatások > App Services** elemre.</span><span class="sxs-lookup"><span data-stu-id="bb124-273">Click **More Services > App Services**.</span></span>
    
    ![Tallózás az Alkalmazásszolgáltatások között](./media/app-service-api-dotnet-get-started/browseas.png)
19. <span data-ttu-id="bb124-275">A hello **alkalmazásszolgáltatások** panelen keresse meg és kattintson az új API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb124-275">In hello **App Services** blade, find and click your new API app.</span></span> <span data-ttu-id="bb124-276">(Hello Azure-portálon, a megnyíló ablakokat toohello jobb neve *paneleken*.)</span><span class="sxs-lookup"><span data-stu-id="bb124-276">(In hello Azure portal, windows that open toohello right are called *blades*.)</span></span>
    
    ![Az App Services (Alkalmazásszolgáltatások) panel](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)
    
    <span data-ttu-id="bb124-278">Két panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-278">Two blades open.</span></span> <span data-ttu-id="bb124-279">Az egyik panelen hello API-alkalmazás áttekintése, pedig egy megtekinthetik és módosíthatják a beállításokat listája túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="bb124-279">One blade has an overview of hello API app, and one has a long list of settings that you can view and change.</span></span>
20. <span data-ttu-id="bb124-280">A hello **beállítások** panelen, a keresés hello **API** szakaszt, és kattintson **API-definíció**.</span><span class="sxs-lookup"><span data-stu-id="bb124-280">In hello **Settings** blade, find hello **API** section and click **API Definition**.</span></span>
    
    ![Az API Definition (API-definíció) elem a Settings (Beállítások) panelen](./media/app-service-api-dotnet-get-started/apidefinsettings.png)
    
    <span data-ttu-id="bb124-282">Hello **API-definíció** panelen adhatja meg a hello URL-címet, amely Swagger 2.0-metaadatokat adja vissza JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="bb124-282">hello **API Definition** blade lets you specify hello URL that returns Swagger 2.0 metadata in JSON format.</span></span> <span data-ttu-id="bb124-283">Visual Studio létrehozza a hello API-alkalmazást, amikor hello API definition URL-cím toohello alapértelmezett értéket állítja a a Swashbuckle által generált metaadatokban, amely a korábban, amely hello API-alkalmazás csomagazonosítóját alap URL-cím plusz `/swagger/docs/v1`.</span><span class="sxs-lookup"><span data-stu-id="bb124-283">When Visual Studio creates hello API app, it sets hello API definition URL toohello default value for Swashbuckle-generated metadata that you saw earlier, which is hello API app's base URL plus `/swagger/docs/v1`.</span></span>
    
    ![Az API-definíció URL-címe](./media/app-service-api-dotnet-get-started/apidefurl.png)
    
    <span data-ttu-id="bb124-285">Ez az API app toogenerate Ügyfélkód választva, a Visual Studio lekéri a hello metaadatait a URL-címet.</span><span class="sxs-lookup"><span data-stu-id="bb124-285">When you select an API app toogenerate client code for it, Visual Studio retrieves hello metadata from this URL.</span></span>

## <span data-ttu-id="bb124-286"><a id="codegen"></a>Hello adatrétegbeli Ügyfélkód generálása</span><span class="sxs-lookup"><span data-stu-id="bb124-286"><a id="codegen"></a> Generate client code for hello data tier</span></span>
<span data-ttu-id="bb124-287">A Swagger integrálása az Azure API apps előnyeit hello egyik kódok automatikus létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bb124-287">One of hello advantages of integrating Swagger into Azure API apps is automatic code generation.</span></span> <span data-ttu-id="bb124-288">A generált révén könnyebben toowrite kódot, amely hívja az API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bb124-288">Generated client classes make it easier toowrite code that calls an API app.</span></span>

<span data-ttu-id="bb124-289">hello a ToDoListAPI projekt már hello generált ügyfélkódot, de a lépéseket követve hello bemutatjuk törölje azt, hogyan toodo hello kód generálása a toosee ismét.</span><span class="sxs-lookup"><span data-stu-id="bb124-289">hello ToDoListAPI project already has hello generated client code, but in hello following steps you'll delete it and regenerate it toosee how toodo hello code generation.</span></span>

1. <span data-ttu-id="bb124-290">A Visual Studio **Megoldáskezelőben**, a hello ToDoListAPI projektre, törölje a hello *ToDoListDataAPI* mappa.</span><span class="sxs-lookup"><span data-stu-id="bb124-290">In Visual Studio **Solution Explorer**, in hello ToDoListAPI project, delete hello *ToDoListDataAPI* folder.</span></span> <span data-ttu-id="bb124-291">**Figyelmeztetés: Csak hello mappa törléséhez hello ToDoListDataAPI projektet.**</span><span class="sxs-lookup"><span data-stu-id="bb124-291">**Caution: Delete only hello folder, not hello ToDoListDataAPI project.**</span></span>
   
    ![Generált ügyfélkód törlése](./media/app-service-api-dotnet-get-started/deletecodegen.png)
   
    <span data-ttu-id="bb124-293">Ez a mappa, amely körülbelül toogo keresztül hello kódgenerálási folyamat segítségével hozta létre.</span><span class="sxs-lookup"><span data-stu-id="bb124-293">This folder was created by using hello code generation process that you're about toogo through.</span></span>
2. <span data-ttu-id="bb124-294">Kattintson a jobb gombbal a hello ToDoListAPI projektet, és kattintson **Hozzáadás > REST API-ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="bb124-294">Right-click hello ToDoListAPI project, and then click **Add > REST API Client**.</span></span>
   
    ![REST API-ügyfél hozzáadása a Visual Studióban](./media/app-service-api-dotnet-get-started/codegenmenu.png)
3. <span data-ttu-id="bb124-296">A hello **REST API-ügyfél hozzáadása** párbeszédpanel, kattintson a **Swagger URL**, és kattintson a **Azure eszköz kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="bb124-296">In hello **Add REST API Client** dialog box, click **Swagger URL**, and then click **Select Azure Asset**.</span></span>
   
    ![Azure-objektum kiválasztása](./media/app-service-api-dotnet-get-started/codegenbrowse.png)
4. <span data-ttu-id="bb124-298">A hello **App Service** párbeszédpanelen bontsa ki a jelen oktatóanyag használata hello erőforráscsoportot, és válassza ki az API-alkalmazás, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb124-298">In hello **App Service** dialog box, expand hello resource group you're using for this tutorial and select your API app, and then click **OK**.</span></span>
   
    ![API-alkalmazás kiválasztása a kódgeneráláshoz](./media/app-service-api-dotnet-get-started/codegenselect.png)
   
    <span data-ttu-id="bb124-300">Figyelje meg, hogy amikor visszatér toohello **REST API-ügyfél hozzáadása** párbeszédpanelen hello szövegmező rendszer kitöltötte hello API definition hello portálon korábban látott URL-címével.</span><span class="sxs-lookup"><span data-stu-id="bb124-300">Notice that when you return toohello **Add REST API Client** dialog, hello text box has been filled in with hello API definition URL value that you saw earlier in hello portal.</span></span>
   
    ![API-definíció URL-címe](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)
   
   > [!TIP]
   > <span data-ttu-id="bb124-302">Egy másik módja tooget kód generálására metaadatai tooenter hello URL-címet közvetlenül tett lépéseket megkerülve hello tallózási párbeszédpanelre.</span><span class="sxs-lookup"><span data-stu-id="bb124-302">An alternative way tooget metadata for code generation is tooenter hello URL directly instead of going through hello browse dialog.</span></span> <span data-ttu-id="bb124-303">Vagy ha toogenerate Ügyfélkód tooAzure telepítése előtt, sikerült hello Web API-projekt helyi futtatásra, nyissa meg hello Swagger JSON-fájlt mentse hello fájlt biztosító toohello URL-címet, és hello használata **válasszon egy meglévő Swagger-metaadatfájl**lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bb124-303">Or if you want toogenerate client code before deploying tooAzure, you could run hello Web API project locally, go toohello URL that provides hello Swagger JSON file, save hello file, and use hello **Select an existing Swagger metadata file** option.</span></span>
   > 
   > 
5. <span data-ttu-id="bb124-304">A hello **REST API-ügyfél hozzáadása** párbeszédpanel, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb124-304">In hello **Add REST API Client** dialog box, click **OK**.</span></span>
   
    <span data-ttu-id="bb124-305">A Visual Studio létrehoz egy után hello API app nevű mappát, és elvégzi az ügyfélosztályok generálását.</span><span class="sxs-lookup"><span data-stu-id="bb124-305">Visual Studio creates a folder named after hello API app and generates client classes.</span></span>
   
    ![A generált ügyfélhez tartozó kódfájlok](./media/app-service-api-dotnet-get-started/codegenfiles.png)
6. <span data-ttu-id="bb124-307">Hello ToDoListAPI projektben nyissa meg *Controllers\ToDoListController.cs* toosee hello kód sor 40 hello API hello generált ügyfél segítségével behívó.</span><span class="sxs-lookup"><span data-stu-id="bb124-307">In hello ToDoListAPI project, open *Controllers\ToDoListController.cs* toosee hello code at line 40  that calls hello API by using hello generated client.</span></span>
   
    <span data-ttu-id="bb124-308">a következő kódrészletet hello bemutatja, hogyan hello kód példányosítja hello objektumot, és hívások hello Get metódus.</span><span class="sxs-lookup"><span data-stu-id="bb124-308">hello following snippet shows how hello code instantiates hello client object and calls hello Get method.</span></span>
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));
            return client;
        }
   
        public async Task<IEnumerable<ToDoItem>> Get()
        {
            using (var client = NewDataAPIClient())
            {
                var results = await client.ToDoList.GetByOwnerAsync(owner);
                return results.Select(m => new ToDoItem
                {
                    Description = m.Description,
                    ID = (int)m.ID,
                    Owner = m.Owner
                });
            }
        }
   
    <span data-ttu-id="bb124-309">hello konstruktorparamétert hello végponti URL-cím lekérése hello `toDoListDataAPIURL` Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="bb124-309">hello constructor parameter gets hello endpoint URL from  hello `toDoListDataAPIURL` app setting.</span></span> <span data-ttu-id="bb124-310">Hello Web.config fájlban, hogy értéke set toohello helyi IIS Express URL-címe hello API projektre, hogy hello alkalmazás helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="bb124-310">In hello Web.config file, that value is set toohello local IIS Express URL of hello API project so that you can run hello application locally.</span></span> <span data-ttu-id="bb124-311">Ha hello konstruktor paraméter nincs megadva, hello alapértelmezett végpont: hello kódot generálta hello URL-cím.</span><span class="sxs-lookup"><span data-stu-id="bb124-311">If you omit hello constructor parameter, hello default endpoint is hello URL that you generated hello code from.</span></span>
7. <span data-ttu-id="bb124-312">Ön egy eltérő nevű az API-alkalmazás nevének; alapján jön létre hello kód módosítható *Controllers\ToDoListController.cs* , hogy hello típusnév generáltnak a projektben.</span><span class="sxs-lookup"><span data-stu-id="bb124-312">Your client class will be generated with a different name based on your API app name; change hello code in *Controllers\ToDoListController.cs* so that hello type name matches what was generated in your project.</span></span> <span data-ttu-id="bb124-313">Ha például az API-alkalmazás neve ToDoListDataAPI071316, a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="bb124-313">For example, if you named your API App ToDoListDataAPI071316, you would change this code:</span></span>
   
        private static ToDoListDataAPI NewDataAPIClient()
        {
            var client = new ToDoListDataAPI(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));

<span data-ttu-id="bb124-314">toothis:</span><span class="sxs-lookup"><span data-stu-id="bb124-314">toothis:</span></span>

        private static ToDoListDataAPI071316 NewDataAPIClient()
        {
            var client = new ToDoListDataAPI071316(new Uri(ConfigurationManager.AppSettings["toDoListDataAPIURL"]));


## <a name="create-an-api-app-toohost-hello-middle-tier"></a><span data-ttu-id="bb124-315">Az API app toohost hello középső réteg létrehozása</span><span class="sxs-lookup"><span data-stu-id="bb124-315">Create an API app toohost hello middle tier</span></span>
<span data-ttu-id="bb124-316">Korábban, [hello API-alkalmazás adatrétegét létrehozott és telepített kód tooit](#createapiapp).</span><span class="sxs-lookup"><span data-stu-id="bb124-316">Earlier you [created hello data tier API app and deployed code tooit](#createapiapp).</span></span>  <span data-ttu-id="bb124-317">Hajtsa végre a hello most ugyanezt az eljárást hello középső rétegbeli API-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bb124-317">Now you follow hello same procedure for hello middle tier API app.</span></span>

1. <span data-ttu-id="bb124-318">A **Megoldáskezelőben**, kattintson a jobb gombbal hello középső rétegbeli ToDoListAPI projektre (nem hello adatrétegbeli ToDoListDataAPI), és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="bb124-318">In **Solution Explorer**, right-click hello middle tier ToDoListAPI  project (not hello data tier ToDoListDataAPI), and then click **Publish**.</span></span>
   
    ![A Publish (Közzététel) elem a Visual Studióban](./media/app-service-api-dotnet-get-started/pubinmenu2.png)
2. <span data-ttu-id="bb124-320">A hello **profil** hello lapján **webhely közzététele** varázsló, kattintson a **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="bb124-320">In hello **Profile** tab of hello **Publish Web** wizard, click **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="bb124-321">A hello **App Service** párbeszédpanel, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="bb124-321">In hello **App Service** dialog box, click **New**.</span></span>
4. <span data-ttu-id="bb124-322">A hello **üzemeltetési** hello lapján **létrehozása az App Service** párbeszédpanel mezőben fogadja el az alapértelmezett hello **API App Name** vagy adjon meg egy nevet, amely egyedi a hello  *azurewebsites.NET* tartomány.</span><span class="sxs-lookup"><span data-stu-id="bb124-322">In hello **Hosting** tab of hello **Create App Service** dialog box, accept hello default **API App Name** or enter a name that is unique in hello *azurewebsites.net* domain.</span></span>
5. <span data-ttu-id="bb124-323">Válassza ki a hello Azure **előfizetés** használja.</span><span class="sxs-lookup"><span data-stu-id="bb124-323">Choose hello Azure **Subscription** you have been using.</span></span>
6. <span data-ttu-id="bb124-324">A hello **erőforráscsoport** legördülő listából válassza ki a hello ugyanazt az a korábban létrehozott erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="bb124-324">In hello **Resource Group** drop-down, choose hello same resource group you created earlier.</span></span>
7. <span data-ttu-id="bb124-325">A hello **App Service-csomag** legördülő listából válassza ki a hello korábban létrehozott csomagot.</span><span class="sxs-lookup"><span data-stu-id="bb124-325">In hello **App Service Plan** drop-down, choose hello same plan you created earlier.</span></span> <span data-ttu-id="bb124-326">Az toothat értéke alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="bb124-326">It will default toothat value.</span></span>
8. <span data-ttu-id="bb124-327">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-327">Click **Create**.</span></span>
   
    <span data-ttu-id="bb124-328">A Visual Studio létrehozza hello API-alkalmazást, a közzétételi profilt hozza létre és hello megjeleníti **kapcsolat** hello lépését **webhely közzététele** varázsló.</span><span class="sxs-lookup"><span data-stu-id="bb124-328">Visual Studio creates hello API app, creates a publish profile for it, and displays hello **Connection** step of hello **Publish Web** wizard.</span></span>
9. <span data-ttu-id="bb124-329">A hello **kapcsolat** hello lépését **webhely közzététele** varázsló, kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="bb124-329">In hello **Connection** step of hello **Publish Web** wizard, click **Publish**.</span></span>
   
   <span data-ttu-id="bb124-330">Visual Studio telepíti a hello ToDoListAPI projekt toohello új API-alkalmazást, és megnyitja a böngésző toohello URL-t hello API-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="bb124-330">Visual Studio deploys hello ToDoListAPI project toohello new API app and opens a browser toohello URL of hello API app.</span></span> <span data-ttu-id="bb124-331">"sikeresen létrejött" Hello lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-331">hello "successfully created" page appears.</span></span>

## <a name="configure-hello-middle-tier-toocall-hello-data-tier"></a><span data-ttu-id="bb124-332">Hello középső réteg toocall hello adatszint konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bb124-332">Configure hello middle tier toocall hello data tier</span></span>
<span data-ttu-id="bb124-333">Ha most nevű hello középső rétegbeli API-alkalmazást, azt próbálná toocall hello adatrétegbeli hello localhost URL-cím használatával, amely még hello Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="bb124-333">If you called hello middle tier API app now, it would try toocall hello data tier using hello localhost URL that is still in hello Web.config file.</span></span> <span data-ttu-id="bb124-334">Ebben a szakaszban a hello adatok réteg API alkalmazás URL-CÍMÉT a hello középső réteg API-alkalmazásának egyik környezeti beállításánál meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-334">In this section you enter hello data tier API app URL into an environment setting in hello middle tier API app.</span></span> <span data-ttu-id="bb124-335">Hello hello középső rétegbeli API-alkalmazás kódja lekéri hello adatok adatréteg URL-beállításait, amikor a hello környezeti beállítás felülírja a hello Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="bb124-335">When hello code in hello middle tier API app retrieves hello data tier URL setting, hello environment setting will override what's in hello Web.config file.</span></span>

1. <span data-ttu-id="bb124-336">Nyissa meg toohello [Azure-portálon](https://portal.azure.com/), és navigáljon a toohello **API-alkalmazás** toohost hello TodoListAPI (középső réteg) projekt létrehozott hello API-alkalmazás paneljén.</span><span class="sxs-lookup"><span data-stu-id="bb124-336">Go toohello [Azure portal](https://portal.azure.com/), and then navigate toohello **API App** blade for hello API app that you created toohost hello TodoListAPI (middle tier) project.</span></span>
2. <span data-ttu-id="bb124-337">Az API-alkalmazás hello **beállítások** panelen kattintson a **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="bb124-337">In hello API App's **Settings** blade, click **Application settings**.</span></span>
3. <span data-ttu-id="bb124-338">Az API-alkalmazás hello **Alkalmazásbeállítások** panelen görgessen lefelé toohello **Alkalmazásbeállítások** szakaszt, és adja hozzá a következő hello kulcs-érték.</span><span class="sxs-lookup"><span data-stu-id="bb124-338">In hello API App's **Application Settings** blade, scroll down toohello **App settings** section and add hello following key and value.</span></span> <span data-ttu-id="bb124-339">hello értéket fogja hello hello első API App ebben az oktatóanyagban közzétett URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bb124-339">hello value will be hello URL of hello first API App you published in this tutorial.</span></span>
   
   | <span data-ttu-id="bb124-340">**Kulcs**</span><span class="sxs-lookup"><span data-stu-id="bb124-340">**Key**</span></span> | <span data-ttu-id="bb124-341">toDoListDataAPIURL</span><span class="sxs-lookup"><span data-stu-id="bb124-341">toDoListDataAPIURL</span></span> |
   | --- | --- |
   | <span data-ttu-id="bb124-342">**Érték**</span><span class="sxs-lookup"><span data-stu-id="bb124-342">**Value**</span></span> |<span data-ttu-id="bb124-343">https://{az adatréteghez tartozó API-alkalmazás neve}.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="bb124-343">https://{your data tier API app name}.azurewebsites.net</span></span> |
   | <span data-ttu-id="bb124-344">**Példa**</span><span class="sxs-lookup"><span data-stu-id="bb124-344">**Example**</span></span> |<span data-ttu-id="bb124-345">https://todolistdataapi.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="bb124-345">https://todolistdataapi.azurewebsites.net</span></span> |
4. <span data-ttu-id="bb124-346">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="bb124-346">Click **Save**.</span></span>
   
    ![Kattintás a Mentés gombra az Alkalmazásbeállítások menüben](./media/app-service-api-dotnet-get-started/asinportal.png)
   
    <span data-ttu-id="bb124-348">Amikor hello kód lefut az Azure-ban, ez az érték most felülírják hello localhost URL-cím hello Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="bb124-348">When hello code runs in Azure, this value will now override hello localhost URL that is in hello Web.config file.</span></span>

## <a name="test"></a><span data-ttu-id="bb124-349">Tesztelés</span><span class="sxs-lookup"><span data-stu-id="bb124-349">Test</span></span>
1. <span data-ttu-id="bb124-350">Egy böngészőablakban keresse meg a hello új középső rétegbeli API-alkalmazás, amelyet most hozott létre a ToDoListAPI toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="bb124-350">In a browser window, browse toohello URL of hello new middle tier API app that you just created for ToDoListAPI.</span></span> <span data-ttu-id="bb124-351">Ehhez úgy juthat el hello hello portál fő paneljén hello API-alkalmazás URL-CÍMRE kattintva.</span><span class="sxs-lookup"><span data-stu-id="bb124-351">You can get there by clicking hello URL in hello API app's main blade in hello portal.</span></span>
2. <span data-ttu-id="bb124-352">Hello böngésző címsorában adja hozzá "swagger" toohello URL-címet, majd nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="bb124-352">Add "swagger" toohello URL in hello browser's address bar, and then press Enter.</span></span> <span data-ttu-id="bb124-353">(URL-cím hello `http://{apiappname}.azurewebsites.net/swagger`.)</span><span class="sxs-lookup"><span data-stu-id="bb124-353">(hello URL is `http://{apiappname}.azurewebsites.net/swagger`.)</span></span>
   
    <span data-ttu-id="bb124-354">hello böngésző megjeleníti hello Swagger felhasználói felület, amely korábban már látta a ToDoListDataAPI, de most `owner` megadása nem kötelező hello Get művelethez, mert hello középső rétegbeli API-alkalmazást, hogy érték toohello API-alkalmazás adatrétegét küld Önnek.</span><span class="sxs-lookup"><span data-stu-id="bb124-354">hello browser displays hello same Swagger UI that you saw earlier for ToDoListDataAPI, but now `owner` is not a required field for hello Get operation, because hello middle tier API app is sending that value toohello data tier API app for you.</span></span> <span data-ttu-id="bb124-355">(Ha a hitelesítési oktatóanyagok hello, hello középső réteg valós felhasználói azonosítókat a hello fog küldeni `owner` paraméter; most azt fix kódolású csillag.)</span><span class="sxs-lookup"><span data-stu-id="bb124-355">(When you do hello authentication tutorials, hello middle tier will send actual user IDs for hello `owner` parameter; for now it is hard-coding an asterisk.)</span></span>
3. <span data-ttu-id="bb124-356">Próbálja ki a Get metódust hello és hello egyéb módszerek toovalidate, amely hello középső rétegbeli API-alkalmazás sikeresen behívja hello adatréteg API-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="bb124-356">Try out hello Get method and hello other methods toovalidate that hello middle tier API app is successfully calling hello data tier API app.</span></span>
   
    ![Swagger felhasználói felület, Get metódus](./media/app-service-api-dotnet-get-started/midtierget.png)

## <a name="troubleshooting"></a><span data-ttu-id="bb124-358">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="bb124-358">Troubleshooting</span></span>
<span data-ttu-id="bb124-359">Ha az oktatóanyag lépéseinek elvégzése közben hibákba ütközne, olvassa el az alábbi hibakeresési tippeket:</span><span class="sxs-lookup"><span data-stu-id="bb124-359">In case you run into a problem as you go through this tutorial here are some troubleshooting ideas:</span></span>

* <span data-ttu-id="bb124-360">Győződjön meg arról, hogy hello hello legújabb verzióját használja [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).</span><span class="sxs-lookup"><span data-stu-id="bb124-360">Make sure that you're using hello latest version of hello [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003).</span></span>
* <span data-ttu-id="bb124-361">Hello projekt neve kettő hasonlít egymáshoz (ToDoListAPI, ToDoListDataAPI).</span><span class="sxs-lookup"><span data-stu-id="bb124-361">Two of hello project names are similar (ToDoListAPI, ToDoListDataAPI).</span></span> <span data-ttu-id="bb124-362">Ha dolgot nem utasításokban leírtak hello utasításokat egy dolgozva, ellenőrizze, hogy hello megfelelő projektet nyitotta meg.</span><span class="sxs-lookup"><span data-stu-id="bb124-362">If things don't look as described in hello instructions when you are working with a project, make sure you have opened hello correct project.</span></span>
* <span data-ttu-id="bb124-363">Ha egy vállalati hálózat és próbál toodeploy tooAzure App Service a tűzfalon keresztül, győződjön meg arról, hogy 443-as és 8172-es port nyitva a Web Deploy keretrendszert.</span><span class="sxs-lookup"><span data-stu-id="bb124-363">If you're on a corporate network and are trying toodeploy tooAzure App Service through a firewall, make sure that ports 443 and 8172 are open for Web Deploy.</span></span> <span data-ttu-id="bb124-364">Ha nem tudja megnyitni ezeket a portokat, használjon más telepítési módszert.</span><span class="sxs-lookup"><span data-stu-id="bb124-364">If you can't open those ports, you can use other deployment methods.</span></span>  <span data-ttu-id="bb124-365">Lásd: [telepítheti az alkalmazást tooAzure App Service](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="bb124-365">See [Deploy your app tooAzure App Service](../app-service-web/web-sites-deploy.md).</span></span>
* <span data-ttu-id="bb124-366">"Útvonalneveknek egyedieknek kell lenniük" hibák sikerült kap ezek Ha véletlenül a hello megfelelő projektet tooan API app telepíti, később központilag terjesztheti hello helyes-e egy tooit.</span><span class="sxs-lookup"><span data-stu-id="bb124-366">"Route names must be unique" errors -- you could get these if you accidentally deploy hello wrong project tooan API app and then later deploy hello correct one tooit.</span></span> <span data-ttu-id="bb124-367">toocorrect a, helyezze üzembe újra hello megfelelő projektet toohello API app hello a **beállítások** hello lapján **webhely közzététele** varázsló select **célhelyentovábbifájlokeltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="bb124-367">toocorrect this, redeploy hello correct project toohello API app, and on hello **Settings** tab of hello **Publish Web** wizard select **Remove additional files at destination**.</span></span>

<span data-ttu-id="bb124-368">Miután az ASP.NET API-alkalmazás futtatása az Azure App Service-ben, érdemes lehet további információk a Visual Studio hibaelhárítást egyszerűsítő szolgáltatásairól toolearn.</span><span class="sxs-lookup"><span data-stu-id="bb124-368">After you have your ASP.NET API app running in Azure App Service, you may want toolearn more about Visual Studio features that simplify troubleshooting.</span></span> <span data-ttu-id="bb124-369">A naplózással, a távoli hibakereséssel és a többi funkcióval kapcsolatban további információkért lásd: [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) (Azure App Service alkalmazások hibakeresése a Visual Studióban).</span><span class="sxs-lookup"><span data-stu-id="bb124-369">For information about logging, remote debugging, and more, see  [Troubleshooting Azure App Service apps in Visual Studio](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb124-370">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bb124-370">Next steps</span></span>
<span data-ttu-id="bb124-371">Megtudhatta, hogyan toodeploy meglévő Web API projektek tooAPI alkalmazások, generálhat ügyfélkódot az API apps, és a .NET-ügyfelekről az API-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="bb124-371">You've seen how toodeploy existing Web API projects tooAPI apps, generate client code for API apps, and consume API apps from .NET clients.</span></span> <span data-ttu-id="bb124-372">a sorozat következő oktatóanyaga hello bemutatja, hogyan túl[használhatja a CORS tooconsume API alkalmazásokat JavaScript-ügyfelekből](app-service-api-cors-consume-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="bb124-372">hello next tutorial in this series shows how too[use CORS tooconsume API apps from JavaScript clients](app-service-api-cors-consume-javascript.md).</span></span>

<span data-ttu-id="bb124-373">Ügyfélkód generálásával kapcsolatban további információkért lásd: hello [Azure/AutoRest](https://github.com/azure/autorest) github.com tárházba. Hello generált ügyfél segítségével problémák kapcsolatban nyisson meg egy [hello AutoRest tárházban probléma](https://github.com/azure/autorest/issues).</span><span class="sxs-lookup"><span data-stu-id="bb124-373">For more information about client code generation, see hello [Azure/AutoRest](https://github.com/azure/autorest) repository on GitHub.com. For help with problems using hello generated client, open an [issue in hello AutoRest repository](https://github.com/azure/autorest/issues).</span></span>

<span data-ttu-id="bb124-374">Ha azt szeretné, hogy toocreate új API app projektet teljesen új, használja a hello **Azure API App** sablont.</span><span class="sxs-lookup"><span data-stu-id="bb124-374">If you want toocreate new API app projects from scratch, use hello **Azure API App** template.</span></span>

![API App sablon a Visual Studióban](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

<span data-ttu-id="bb124-376">Hello **Azure API App** projektsablon egyenértékű toochoosing hello **üres** ASP.NET 4.5.2 sablont, kattintással hello jelölőnégyzetet tooadd Web API támogatását, és telepíti a Swashbuckle NuGet csomagot hello.</span><span class="sxs-lookup"><span data-stu-id="bb124-376">hello **Azure API App** project template is equivalent toochoosing hello **Empty** ASP.NET 4.5.2 template, clicking hello check box tooadd Web API support, and installing hello Swashbuckle NuGet package.</span></span> <span data-ttu-id="bb124-377">Ezenkívül hello sablon hozzáadása némi Swashbuckle konfigurációs kódot is, amely tooprevent hello létrehozása duplikált Swagger műveleti azonosítók.</span><span class="sxs-lookup"><span data-stu-id="bb124-377">In addition, hello template adds some Swashbuckle configuration code designed tooprevent hello creation of duplicate Swagger operation IDs.</span></span> <span data-ttu-id="bb124-378">Ha létrehozta az API App projektet, ezután telepítheti azt tooan API app hello ebben az oktatóanyagban megtudhatta azonos módon.</span><span class="sxs-lookup"><span data-stu-id="bb124-378">Once you've created an API App project, you can deploy it tooan API app hello same way you saw in this tutorial.</span></span>


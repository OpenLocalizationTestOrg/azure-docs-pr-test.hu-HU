---
title: "A Mobile Apps használata a .NET-háttérrendszer server SDK |} Microsoft Docs"
description: "Megtudhatja, hogyan használható a .NET-háttérrendszer server SDK az Azure App Service Mobile Apps a."
keywords: "az App service, a azure app service, a mobilalkalmazás, a mobilszolgáltatást, a méretezési, méretezhető, központi telepítését, az azure app alkalmazástelepítés"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 0620554f-9590-40a8-9f47-61c48c21076b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 657fea16e47c15efd262c86d6a150a721476134a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="work-with-the-net-backend-server-sdk-for-azure-mobile-apps"></a><span data-ttu-id="f67b8-104">Az Azure Mobile Appshoz készült .NET háttérkiszolgáló-SDK használata</span><span class="sxs-lookup"><span data-stu-id="f67b8-104">Work with the .NET backend server SDK for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="f67b8-105">Ez a témakör bemutatja, hogyan használhatja a .NET háttérkiszolgáló SDK az Azure App Service Mobile Apps főbb forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="f67b8-105">This topic shows you how to use the .NET backend server SDK in key Azure App Service Mobile Apps scenarios.</span></span> <span data-ttu-id="f67b8-106">Az Azure Mobile Apps SDK segítséget nyújt az ASP.NET-alkalmazás a mobil ügyfelek dolgozni.</span><span class="sxs-lookup"><span data-stu-id="f67b8-106">The Azure Mobile Apps SDK helps you work with mobile clients from your ASP.NET application.</span></span>

> [!TIP]
> <span data-ttu-id="f67b8-107">A [.NET server az Azure Mobile Apps SDK] [ 2] nyílt forráskódú a Githubon.</span><span class="sxs-lookup"><span data-stu-id="f67b8-107">The [.NET server SDK for Azure Mobile Apps][2] is open source on GitHub.</span></span> <span data-ttu-id="f67b8-108">A tárház tartalmazza az összes többek között a teljes kiszolgáló SDK egység tesztcsomag és néhány mintaprojektjeit.</span><span class="sxs-lookup"><span data-stu-id="f67b8-108">The repository contains all source code including the entire server SDK unit test suite and some sample projects.</span></span>
>
>

## <a name="reference-documentation"></a><span data-ttu-id="f67b8-109">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="f67b8-109">Reference documentation</span></span>
<span data-ttu-id="f67b8-110">A kiszolgáló SDK dokumentációját a következő helyen található: [Azure Mobile Apps .NET hivatkozás][1].</span><span class="sxs-lookup"><span data-stu-id="f67b8-110">The reference documentation for the server SDK is located here: [Azure Mobile Apps .NET Reference][1].</span></span>

## <span data-ttu-id="f67b8-111"><a name="create-app"></a>Hogyan: .NET Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f67b8-111"><a name="create-app"></a>How to: Create a .NET Mobile App backend</span></span>
<span data-ttu-id="f67b8-112">Ha a számítógépet egy új projektet, létrehozhat egy App Service alkalmazás segítségével a [Azure-portálon] vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f67b8-112">If you are starting a new project, you can create an App Service application using either the [Azure portal] or Visual Studio.</span></span> <span data-ttu-id="f67b8-113">Az App Service alkalmazás helyileg történő futtatása, vagy a projekt közzététele a felhőalapú App Service mobilalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="f67b8-113">You can run the App Service application locally or publish the project to your cloud-based App Service mobile app.</span></span>

<span data-ttu-id="f67b8-114">Ha egy meglévő projektjébe hozzáadni mobil funkciókkal, tekintse meg a [töltse le és inicializálja az SDK-](#install-sdk) szakasz.</span><span class="sxs-lookup"><span data-stu-id="f67b8-114">If you are adding mobile capabilities to an existing project, see the [Download and initialize the SDK](#install-sdk) section.</span></span>

### <a name="create-a-net-backend-using-the-azure-portal"></a><span data-ttu-id="f67b8-115">Az Azure portál használatával a .NET-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f67b8-115">Create a .NET backend using the Azure portal</span></span>
<span data-ttu-id="f67b8-116">Az App Service mobil-háttéralkalmazás létrehozása, vagy hajtsa végre a [gyors üzembe helyezési útmutató] [ 3] vagy kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f67b8-116">To create an App Service mobile backend, either follow the [Quickstart tutorial][3] or follow these steps:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="f67b8-117">Vissza a *Ismerkedés* panelen, a **tábla API létrehozása**, válassza a **C#** , a **háttéralkalmazás-nyelv**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-117">Back in the *Get started* blade, under **Create a table API**, choose **C#** as your **Backend language**.</span></span> <span data-ttu-id="f67b8-118">Kattintson a **letöltése**, bontsa ki a tömörített projektfájlokat a helyi számítógépen, és nyissa meg a megoldást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f67b8-118">Click **Download**, extract the compressed project files to your local computer, and open the solution in Visual Studio.</span></span>

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a><span data-ttu-id="f67b8-119">Visual Studio 2013 és a Visual Studio 2015-öt használó .NET-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f67b8-119">Create a .NET backend using Visual Studio 2013 and Visual Studio 2015</span></span>
<span data-ttu-id="f67b8-120">Telepítse a [Azure SDK for .NET] [ 4] (2.9.0 verzió vagy újabb) az Azure Mobile Apps-projekt létrehozása a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="f67b8-120">Install the [Azure SDK for .NET][4] (version 2.9.0 or later) to create an Azure Mobile Apps project in Visual Studio.</span></span> <span data-ttu-id="f67b8-121">Az SDK telepítése után az alábbi lépéseket követve ASP.NET-alkalmazás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="f67b8-121">Once you have installed the SDK, create an ASP.NET application using the following steps:</span></span>

1. <span data-ttu-id="f67b8-122">Nyissa meg a **új projekt** párbeszédpanelen (a *fájl* > **új** > **projekt …** ).</span><span class="sxs-lookup"><span data-stu-id="f67b8-122">Open the **New Project** dialog (from *File* > **New** > **Project...**).</span></span>
2. <span data-ttu-id="f67b8-123">Bontsa ki a **sablonok** > **Visual C#**, és válassza ki **webes**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-123">Expand **Templates** > **Visual C#**, and select **Web**.</span></span>
3. <span data-ttu-id="f67b8-124">Válassza ki **ASP.NET webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-124">Select **ASP.NET Web Application**.</span></span>
4. <span data-ttu-id="f67b8-125">Töltse ki a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="f67b8-125">Fill in the project name.</span></span> <span data-ttu-id="f67b8-126">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f67b8-126">Then click **OK**.</span></span>
5. <span data-ttu-id="f67b8-127">A *ASP.NET 4.5.2 sablont*, jelölje be **Azure Mobile Apps**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-127">Under *ASP.NET 4.5.2 Templates*, select **Azure Mobile App**.</span></span> <span data-ttu-id="f67b8-128">Ellenőrizze **a felhőben lévő gazdagéphez** a mobil-háttéralkalmazás létrehozása, amelyre a projekt közzéteheti a felhőben.</span><span class="sxs-lookup"><span data-stu-id="f67b8-128">Check **Host in the cloud** to create a mobile backend in the cloud to which you can publish this project.</span></span>
6. <span data-ttu-id="f67b8-129">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f67b8-129">Click **OK**.</span></span>

## <span data-ttu-id="f67b8-130"><a name="install-sdk"></a>Hogyan: Töltse le és inicializálja az SDK</span><span class="sxs-lookup"><span data-stu-id="f67b8-130"><a name="install-sdk"></a>How to: Download and initialize the SDK</span></span>
<span data-ttu-id="f67b8-131">Az SDK nem érhető el a [NuGet.org].</span><span class="sxs-lookup"><span data-stu-id="f67b8-131">The SDK is available on [NuGet.org].</span></span> <span data-ttu-id="f67b8-132">Ez a csomag tartalmazza az SDK használatának megkezdéséhez szükséges alapvető funkciót.</span><span class="sxs-lookup"><span data-stu-id="f67b8-132">This package includes the base functionality required to get started using the SDK.</span></span> <span data-ttu-id="f67b8-133">Inicializálja az SDK, elvégezheti a kapcsolódó műveleteket kell a **HttpConfiguration** objektum.</span><span class="sxs-lookup"><span data-stu-id="f67b8-133">To initialize the SDK, you need to perform actions on the **HttpConfiguration** object.</span></span>

### <a name="install-the-sdk"></a><span data-ttu-id="f67b8-134">Az SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="f67b8-134">Install the SDK</span></span>
<span data-ttu-id="f67b8-135">Az SDK telepítéséhez kattintson a jobb gombbal a kiszolgáló projektre a Visual Studio, válassza ki a **NuGet-csomagok kezelése**, keresse meg a [Microsoft.Azure.Mobile.Server] csomagot, majd kattintson az **telepítése** .</span><span class="sxs-lookup"><span data-stu-id="f67b8-135">To install the SDK, right-click on the server project in Visual Studio, select **Manage NuGet Packages**, search for the [Microsoft.Azure.Mobile.Server] package, then click **Install**.</span></span>

### <span data-ttu-id="f67b8-136"><a name="server-project-setup"></a>A projekt inicializálása</span><span class="sxs-lookup"><span data-stu-id="f67b8-136"><a name="server-project-setup"></a> Initialize the server project</span></span>
<span data-ttu-id="f67b8-137">Egy .NET-háttérrendszer kiszolgálóprojektet inicializálva van más ASP.NET projektek hasonló OWIN indítási osztály-ot.</span><span class="sxs-lookup"><span data-stu-id="f67b8-137">A .NET backend server project is initialized similar to other ASP.NET projects, by including an OWIN startup class.</span></span> <span data-ttu-id="f67b8-138">Győződjön meg arról, hogy rendelkezik-e hivatkozott a NuGet-csomag `Microsoft.Owin.Host.SystemWeb`.</span><span class="sxs-lookup"><span data-stu-id="f67b8-138">Ensure that you have referenced the NuGet package `Microsoft.Owin.Host.SystemWeb`.</span></span> <span data-ttu-id="f67b8-139">Ez az osztály a Visual Studio hozzáadásához kattintson a jobb gombbal a kiszolgáló projektre, és válassza ki **Hozzáadás** >
**új elem**, majd **webes**  >  ** Általános** > **OWIN indítási osztály**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-139">To add this class in Visual Studio, right-click on your server project and select **Add** >
**New Item**, then **Web** > **General** > **OWIN Startup class**.</span></span>  <span data-ttu-id="f67b8-140">Egy osztály hoz létre a következő attribútumot:</span><span class="sxs-lookup"><span data-stu-id="f67b8-140">A class is generated with the following attribute:</span></span>

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

<span data-ttu-id="f67b8-141">Az a `Configuration()` az OWIN indítási osztályt, használja a metódus egy **HttpConfiguration** objektum az Azure Mobile Apps-környezet konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f67b8-141">In the `Configuration()` method of your OWIN startup class, use an **HttpConfiguration** object to configure the Azure Mobile Apps environment.</span></span>
<span data-ttu-id="f67b8-142">Az alábbi példa inicializálja a projekt nem hozzáadott szolgáltatásokkal:</span><span class="sxs-lookup"><span data-stu-id="f67b8-142">The following example initializes the server project with no added features:</span></span>

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

<span data-ttu-id="f67b8-143">Egyes funkciók engedélyezéséhez meg kell hívnia kiterjesztésmetódusok a **MobileAppConfiguration** hívása előtt objektum **ApplyTo**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-143">To enable individual features, you must call extension methods on the **MobileAppConfiguration** object before calling **ApplyTo**.</span></span> <span data-ttu-id="f67b8-144">Például a következő kódot hozzáadja az alapértelmezett útvonalakat a attribútummal rendelkező összes API-vezérlő `[MobileAppController]` inicializálása közben:</span><span class="sxs-lookup"><span data-stu-id="f67b8-144">For example, the following code adds the default routes to all API controllers that have the attribute `[MobileAppController]` during initialization:</span></span>

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

<span data-ttu-id="f67b8-145">A kiszolgáló gyors üzembe helyezés az Azure portál hívást **UseDefaultConfiguration()**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-145">The server quickstart from the Azure portal calls **UseDefaultConfiguration()**.</span></span> <span data-ttu-id="f67b8-146">Ez egyenértékű a következő telepítési:</span><span class="sxs-lookup"><span data-stu-id="f67b8-146">This equivalent to the following setup:</span></span>

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from the Home package
            .MapApiControllers()
            .AddTables(                               // from the Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from the Entity package
                )
            .AddPushNotifications()                   // from the Notifications package
            .MapLegacyCrossDomainController()         // from the CrossDomain package
            .ApplyTo(config);

<span data-ttu-id="f67b8-147">A bővítmény használt módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="f67b8-147">The extension methods used are:</span></span>

* <span data-ttu-id="f67b8-148">`AddMobileAppHomeController()`az alapértelmezett Azure Mobile Apps kezdőlap biztosít.</span><span class="sxs-lookup"><span data-stu-id="f67b8-148">`AddMobileAppHomeController()` provides the default Azure Mobile Apps home page.</span></span>
* <span data-ttu-id="f67b8-149">`MapApiControllers()`WebAPI tartományvezérlők attribútummal rendelkező egyéni API képességeket nyújt a `[MobileAppController]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="f67b8-149">`MapApiControllers()` provides custom API capabilities for WebAPI controllers decorated with the `[MobileAppController]` attribute.</span></span>
* <span data-ttu-id="f67b8-150">`AddTables()`egy táblázatot a biztosít a `/tables` tábla tartományvezérlők végpontok.</span><span class="sxs-lookup"><span data-stu-id="f67b8-150">`AddTables()` provides a mapping of the `/tables` endpoints to table controllers.</span></span>
* <span data-ttu-id="f67b8-151">`AddTablesWithEntityFramework()`egy rövid az aktuális leképezés van a `/tables` használó Entity Framework végpontok alapú tartományvezérlők.</span><span class="sxs-lookup"><span data-stu-id="f67b8-151">`AddTablesWithEntityFramework()` is a short-hand for mapping the `/tables` endpoints using Entity Framework based controllers.</span></span>
* <span data-ttu-id="f67b8-152">`AddPushNotifications()`eszközök regisztrálása a Notification Hubs egy egyszerű módszert kínál.</span><span class="sxs-lookup"><span data-stu-id="f67b8-152">`AddPushNotifications()` provides a simple method of registering devices for Notification Hubs.</span></span>
* <span data-ttu-id="f67b8-153">`MapLegacyCrossDomainController()`standard CORS fejlécek biztosít helyi fejlesztési.</span><span class="sxs-lookup"><span data-stu-id="f67b8-153">`MapLegacyCrossDomainController()` provides standard CORS headers for local development.</span></span>

### <a name="sdk-extensions"></a><span data-ttu-id="f67b8-154">SDK-bővítmények</span><span class="sxs-lookup"><span data-stu-id="f67b8-154">SDK extensions</span></span>
<span data-ttu-id="f67b8-155">A következő NuGet-alapú bővítmény csomagok az alkalmazás által használható különböző mobil funkciókat biztosítanak.</span><span class="sxs-lookup"><span data-stu-id="f67b8-155">The following NuGet-based extension packages provide various mobile features that can be used by your application.</span></span> <span data-ttu-id="f67b8-156">Bővítmények inicializálásakor használatával engedélyezheti a **MobileAppConfiguration** objektum.</span><span class="sxs-lookup"><span data-stu-id="f67b8-156">You enable extensions during initialization by using the **MobileAppConfiguration** object.</span></span>

* <span data-ttu-id="f67b8-157">[Microsoft.Azure.Mobile.Server.Quickstart] a Mobile Apps alapbeállítások támogatja.</span><span class="sxs-lookup"><span data-stu-id="f67b8-157">[Microsoft.Azure.Mobile.Server.Quickstart] Supports the basic Mobile Apps setup.</span></span> <span data-ttu-id="f67b8-158">Meghívásával része a konfigurációnak a **UseDefaultConfiguration** kiterjesztésmetódus inicializálása során.</span><span class="sxs-lookup"><span data-stu-id="f67b8-158">Added to the configuration by calling the **UseDefaultConfiguration** extension method during initialization.</span></span> <span data-ttu-id="f67b8-159">Ezt a bővítményt tartalmazza a következő kiterjesztések: értesítések, a hitelesítés, az entitás, a táblák, a tartományok közötti és otthoni csomagok.</span><span class="sxs-lookup"><span data-stu-id="f67b8-159">This extension includes following extensions: Notifications, Authentication, Entity, Tables, Cross-domain, and Home packages.</span></span> <span data-ttu-id="f67b8-160">Ez a csomag használja a Mobile Apps gyors üzembe helyezés az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="f67b8-160">This package is used by the Mobile Apps Quickstart available on the Azure portal.</span></span>
* <span data-ttu-id="f67b8-161">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) valósítja meg az alapértelmezett *a mobilalkalmazás megfelelően működik, és lap* a webhely gyökeréhez.</span><span class="sxs-lookup"><span data-stu-id="f67b8-161">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) Implements the default *this mobile app is up and running page* for the web site root.</span></span> <span data-ttu-id="f67b8-162">Meghívásával hozzáadni a konfigurációhoz a **AddMobileAppHomeController** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-162">Add to the configuration by calling the **AddMobileAppHomeController** extension method.</span></span>
* <span data-ttu-id="f67b8-163">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) adatok osztályokat tartalmazza, és a készlet létrehozása az adatok feldolgozási sor.</span><span class="sxs-lookup"><span data-stu-id="f67b8-163">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) includes classes for working with data and sets-up the data pipeline.</span></span> <span data-ttu-id="f67b8-164">Meghívásával hozzáadni a konfigurációhoz a **AddTables** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-164">Add to the configuration by calling the **AddTables** extension method.</span></span>
* <span data-ttu-id="f67b8-165">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) lehetővé teszi, hogy az entitás-keretrendszer access adatokat az SQL-adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="f67b8-165">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Enables the Entity Framework to access data in the SQL Database.</span></span> <span data-ttu-id="f67b8-166">Meghívásával hozzáadni a konfigurációhoz a **AddTablesWithEntityFramework** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-166">Add to the configuration by calling the **AddTablesWithEntityFramework** extension method.</span></span>
* <span data-ttu-id="f67b8-167">[Microsoft.Azure.Mobile.Server.Authentication] engedélyezi a hitelesítést és a készlet létrehozása az OWIN köztes jogkivonatainak érvényesítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="f67b8-167">[Microsoft.Azure.Mobile.Server.Authentication] Enables authentication and sets-up the OWIN middleware used to validate tokens.</span></span> <span data-ttu-id="f67b8-168">Meghívásával hozzáadni a konfigurációhoz a **AddAppServiceAuthentication** és **IAppBuilder**. **UseAppServiceAuthentication** kiterjesztésmetódusok.</span><span class="sxs-lookup"><span data-stu-id="f67b8-168">Add to the configuration by calling the **AddAppServiceAuthentication** and **IAppBuilder**.**UseAppServiceAuthentication** extension methods.</span></span>
* <span data-ttu-id="f67b8-169">[Microsoft.Azure.Mobile.Server.Notifications] lehetővé teszi a leküldéses értesítések és egy leküldéses regisztrációs végpontot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f67b8-169">[Microsoft.Azure.Mobile.Server.Notifications] Enables push notifications and defines a push registration endpoint.</span></span> <span data-ttu-id="f67b8-170">Meghívásával hozzáadni a konfigurációhoz a **AddPushNotifications** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-170">Add to the configuration by calling the **AddPushNotifications** extension method.</span></span>
* <span data-ttu-id="f67b8-171">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) egy tartományvezérlőre, amely az adatok a mobileszköz-alkalmazás is szolgál az örökölt webböngészők hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f67b8-171">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data to legacy web browsers from your Mobile App.</span></span> <span data-ttu-id="f67b8-172">Meghívásával hozzáadni a konfigurációhoz a **MapLegacyCrossDomainController** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-172">Add to the configuration by calling the **MapLegacyCrossDomainController** extension method.</span></span>
* <span data-ttu-id="f67b8-173">[Microsoft.Azure.Mobile.Server.Login] a AppServiceLoginHandler.CreateToken() metódust biztosít, amelyet egy egyéni hitelesítési forgatókönyvek során használt statikus metódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-173">[Microsoft.Azure.Mobile.Server.Login] Provides the AppServiceLoginHandler.CreateToken() method, which is a static method used during custom authentication scenarios.</span></span>

## <span data-ttu-id="f67b8-174"><a name="publish-server-project"></a>Útmutató: a kiszolgáló-projekt közzététele</span><span class="sxs-lookup"><span data-stu-id="f67b8-174"><a name="publish-server-project"></a>How to: Publish the server project</span></span>
<span data-ttu-id="f67b8-175">Ez a szakasz bemutatja, hogyan tehet közzé a Visual Studio .NET-háttérprojekt.</span><span class="sxs-lookup"><span data-stu-id="f67b8-175">This section shows you how to publish your .NET backend project from Visual Studio.</span></span> <span data-ttu-id="f67b8-176">A háttérprojekt Git használatával is telepítheti, vagy a más módszereket ismerteti a [Azure App Service üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="f67b8-176">You can also deploy your backend project using Git or any of the other methods covered in the [Azure App Service deployment documentation](../app-service-web/web-sites-deploy.md).</span></span>

1. <span data-ttu-id="f67b8-177">A Visual Studio NuGet-csomagok visszaállítására a projekt újraépítése.</span><span class="sxs-lookup"><span data-stu-id="f67b8-177">In Visual Studio, rebuild the project to restore NuGet packages.</span></span>
2. <span data-ttu-id="f67b8-178">A Megoldáskezelőben kattintson a jobb gombbal a projektre, kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-178">In Solution Explorer, right-click the project, click **Publish**.</span></span> <span data-ttu-id="f67b8-179">Beállíthatja, először kell a közzétételi profil meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="f67b8-179">The first time you publish, you need to define a publishing profile.</span></span> <span data-ttu-id="f67b8-180">Ha már rendelkezik egy definiált profil, válassza ki azt, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-180">When you already have a profile defined, you can select it and click **Publish**.</span></span>
3. <span data-ttu-id="f67b8-181">A közzétételi célként válassza ki, kattintson **Microsoft Azure App Service** > **következő**, majd (ha szükséges) jelentkezzen be Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="f67b8-181">If asked to select a publish target, click **Microsoft Azure App Service** > **Next**, then (if needed) sign-in with your Azure credentials.</span></span>
   <span data-ttu-id="f67b8-182">A Visual Studio letölti és tárolja biztonságos helyen a közzétételi beállítások közvetlenül az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="f67b8-182">Visual Studio downloads and securely stores your publish settings directly from Azure.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. <span data-ttu-id="f67b8-183">Válassza ki a **előfizetés**, jelölje be **erőforrástípus** a **nézet**, bontsa ki a **mobilalkalmazás**, és kattintson a mobil-háttéralkalmazást, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-183">Choose your **Subscription**, select **Resource Type** from **View**, expand **Mobile App**, and click your Mobile App backend, then click **OK**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. <span data-ttu-id="f67b8-184">Ellenőrizze a közzétételi profil adatait, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-184">Verify the publish profile information and click **Publish**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    <span data-ttu-id="f67b8-185">Ha a mobilalkalmazás háttérrendszerének közzététele sikeresen befejeződött, megjelenik a sikert jelző kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="f67b8-185">When your Mobile App backend has published successfully, you see a landing page indicating success.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <span data-ttu-id="f67b8-186"><a name="define-table-controller"></a>Útmutató: egy tábla vezérlő megadása</span><span class="sxs-lookup"><span data-stu-id="f67b8-186"><a name="define-table-controller"></a> How to: Define a table controller</span></span>
<span data-ttu-id="f67b8-187">Adja meg egy tábla vezérlő teszi közzé a mobil ügyfelek SQL tábla.</span><span class="sxs-lookup"><span data-stu-id="f67b8-187">Define a Table Controller to expose a SQL table to mobile clients.</span></span>  <span data-ttu-id="f67b8-188">Egy tábla Controller konfigurálása három lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="f67b8-188">Configuring a Table Controller requires three steps:</span></span>

1. <span data-ttu-id="f67b8-189">Hozzon létre egy adatok átvitele objektum (DTO) osztályt.</span><span class="sxs-lookup"><span data-stu-id="f67b8-189">Create a Data Transfer Object (DTO) class.</span></span>
2. <span data-ttu-id="f67b8-190">Adja meg a Mobile DbContext osztályt a táblahivatkozás.</span><span class="sxs-lookup"><span data-stu-id="f67b8-190">Configure a table reference in the Mobile DbContext class.</span></span>
3. <span data-ttu-id="f67b8-191">Hozzon létre egy tábla tartományvezérlőre.</span><span class="sxs-lookup"><span data-stu-id="f67b8-191">Create a table controller.</span></span>

<span data-ttu-id="f67b8-192">Egy adatok átvitele objektum (DTO) mező egy egyszerű C#-objektum, amely örökli `EntityData`.</span><span class="sxs-lookup"><span data-stu-id="f67b8-192">A Data Transfer Object (DTO) is a plain C# object that inherits from `EntityData`.</span></span>  <span data-ttu-id="f67b8-193">Példa:</span><span class="sxs-lookup"><span data-stu-id="f67b8-193">For example:</span></span>

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

<span data-ttu-id="f67b8-194">A DTO a tábla az SQL-adatbázis azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="f67b8-194">The DTO is used to define the table within the SQL database.</span></span>  <span data-ttu-id="f67b8-195">Az adatbázis-bejegyzés létrehozása, vegye fel a `DbSet<>` használ DbContext tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="f67b8-195">To create the database entry, add a `DbSet<>` property to the DbContext you are using.</span></span>  <span data-ttu-id="f67b8-196">Az Azure Mobile Apps alapértelmezett projektsablon a DbContext neve `Models\MobileServiceContext.cs`:</span><span class="sxs-lookup"><span data-stu-id="f67b8-196">In the default project template for Azure Mobile Apps, the DbContext is called `Models\MobileServiceContext.cs`:</span></span>

    public class MobileServiceContext : DbContext
    {
        private const string connectionStringName = "Name=MS_TableConnectionString";

        public MobileServiceContext() : base(connectionStringName)
        {

        }

        public DbSet<TodoItem> TodoItems { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Add(
                new AttributeToColumnAnnotationConvention<TableColumnAttribute, string>(
                    "ServiceColumnTable", (property, attributes) => attributes.Single().ColumnType.ToString()));
        }
    }

<span data-ttu-id="f67b8-197">Ha az Azure SDK telepítve van, most létrehozhat egy sablont table vezérlő az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f67b8-197">If you have the Azure SDK installed, you can now create a template table controller as follows:</span></span>

1. <span data-ttu-id="f67b8-198">Kattintson a jobb gombbal a tartományvezérlők mappára, és válassza ki **Hozzáadás** > **vezérlő...** .</span><span class="sxs-lookup"><span data-stu-id="f67b8-198">Right-click on the Controllers folder and select **Add** > **Controller...**.</span></span>
2. <span data-ttu-id="f67b8-199">Válassza ki a **Azure Mobile Apps Table vezérlő** lehetőséget, majd kattintson az **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-199">Select the **Azure Mobile Apps Table Controller** option, then click **Add**.</span></span>
3. <span data-ttu-id="f67b8-200">Az a **adja hozzá a tartományvezérlő** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="f67b8-200">In the **Add Controller** dialog:</span></span>
   * <span data-ttu-id="f67b8-201">Az a **Model class** legördülő menüben válassza ki az új DTO.</span><span class="sxs-lookup"><span data-stu-id="f67b8-201">In the **Model class** dropdown, select your new DTO.</span></span>
   * <span data-ttu-id="f67b8-202">Az a **DbContext** legördülő menüben válassza ki a Mobile Service DbContext osztályt.</span><span class="sxs-lookup"><span data-stu-id="f67b8-202">In the **DbContext** dropdown, select the Mobile Service DbContext class.</span></span>
   * <span data-ttu-id="f67b8-203">A tartományvezérlő neve akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="f67b8-203">The Controller name is created for you.</span></span>
4. <span data-ttu-id="f67b8-204">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f67b8-204">Click **Add**.</span></span>

<span data-ttu-id="f67b8-205">A gyors üzembe helyezés kiszolgálóprojektet egy egyszerű példa tartalmaz **TodoItemController**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-205">The quickstart server project contains an example for a simple **TodoItemController**.</span></span>

### <span data-ttu-id="f67b8-206"><a name="adjust-pagesize"></a>Útmutató: a tábla a lapozófájl méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="f67b8-206"><a name="adjust-pagesize"></a>How to: Adjust the table paging size</span></span>
<span data-ttu-id="f67b8-207">Alapértelmezés szerint az Azure Mobile Apps kérelmenként 50 bejegyzést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="f67b8-207">By default, Azure Mobile Apps returns 50 records per request.</span></span>  <span data-ttu-id="f67b8-208">Lapozófájl biztosítja, hogy az ügyfél nem nagy terhelést jelent a felhasználói felület szálán, sem a kiszolgáló túl sokáig a jó felhasználói élményt biztosítva.</span><span class="sxs-lookup"><span data-stu-id="f67b8-208">Paging ensures that the client does not tie up their UI thread nor the server for too long, ensuring a good user experience.</span></span> <span data-ttu-id="f67b8-209">Módosítsa a táblázat a lapozófájl méretét, növelje a kiszolgálóoldali "engedélyezett a lekérdezés mérete", és az ügyféloldali oldalméret a kiszolgálóoldali "engedélyezett a lekérdezés mérete" úgy kell beállítani, használja a `EnableQuery` attribútum:</span><span class="sxs-lookup"><span data-stu-id="f67b8-209">To change the table paging size, increase the server side "allowed query size" and the client-side page size The server side "allowed query size" is adjusted using the `EnableQuery` attribute:</span></span>

    [EnableQuery(PageSize = 500)]

<span data-ttu-id="f67b8-210">Győződjön meg arról a pageSize együttes értéke azonos vagy nagyobb, mint az ügyfél által kért méret.</span><span class="sxs-lookup"><span data-stu-id="f67b8-210">Ensure the PageSize is the same or larger than the size requested by the client.</span></span>  <span data-ttu-id="f67b8-211">Tekintse meg az adott ügyfél útmutató dokumentációjában talál részletes információt az ügyfél Oldalméret módosítása.</span><span class="sxs-lookup"><span data-stu-id="f67b8-211">Refer to the specific client HOWTO documentation for details on changing the client page size.</span></span>

## <a name="how-to-define-a-custom-api-controller"></a><span data-ttu-id="f67b8-212">Hogyan: Adja meg egy egyéni API-vezérlő</span><span class="sxs-lookup"><span data-stu-id="f67b8-212">How to: Define a custom API controller</span></span>
<span data-ttu-id="f67b8-213">Az egyéni API-vezérlőben a Mobile Apps-háttéralkalmazás a legalapvetőbb lehetőségeket kínál a jelentkezik, mintha a végpont.</span><span class="sxs-lookup"><span data-stu-id="f67b8-213">The custom API controller provides the most basic functionality to your Mobile App backend by exposing an endpoint.</span></span> <span data-ttu-id="f67b8-214">Egy mobileszköz-specifikus API-vezérlőben, a [MobileAppController] attribútum használatával regisztrálhatja.</span><span class="sxs-lookup"><span data-stu-id="f67b8-214">You can register a mobile-specific API controller using the [MobileAppController] attribute.</span></span> <span data-ttu-id="f67b8-215">A `MobileAppController` attribútum regisztrálja az útvonal, a Mobile Apps JSON-szerializáló beállítja és bekapcsolja a [ügyfél verzióellenőrzés](app-service-mobile-client-and-server-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="f67b8-215">The `MobileAppController` attribute registers the route, sets up the Mobile Apps JSON serializer, and turns on [client version checking](app-service-mobile-client-and-server-versioning.md).</span></span>

1. <span data-ttu-id="f67b8-216">A Visual Studióban, kattintson a jobb gombbal a tartományvezérlők mappát, majd kattintson **Hozzáadás** > **vezérlő**, jelölje be **Web API 2-es vezérlőhöz&mdash;üres** és Kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-216">In Visual Studio, right-click the Controllers folder, then click **Add** > **Controller**, select **Web API 2 Controller&mdash;Empty** and click **Add**.</span></span>
2. <span data-ttu-id="f67b8-217">Adjon meg egy **tartományvezérlő neve**, például `CustomController`, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-217">Supply a **Controller name**, such as `CustomController`, and click **Add**.</span></span>
3. <span data-ttu-id="f67b8-218">Az új tartományvezérlő osztály fájlban adja hozzá a következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="f67b8-218">In the new controller class file, add the following using statement:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
4. <span data-ttu-id="f67b8-219">Alkalmazza a **[MobileAppController]** attribútum segítségével az API vezérlő osztálydefiníciót, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f67b8-219">Apply the **[MobileAppController]** attribute to the API controller class definition, as in the following example:</span></span>

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. <span data-ttu-id="f67b8-220">App_Start/Startup.MobileApp.cs fájlban adja hozzá a következőt hívja a **MapApiControllers** kiterjesztésmetódus, az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="f67b8-220">In App_Start/Startup.MobileApp.cs file, add a call to the **MapApiControllers** extension method, as in the following example:</span></span>

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

<span data-ttu-id="f67b8-221">Használhatja a `UseDefaultConfiguration()` kiterjesztésmetódus helyett `MapApiControllers()`.</span><span class="sxs-lookup"><span data-stu-id="f67b8-221">You can also use the `UseDefaultConfiguration()` extension method instead of `MapApiControllers()`.</span></span> <span data-ttu-id="f67b8-222">Minden futtassa, amelynek nincs **MobileAppControllerAttribute** alkalmazza az ügyfelek által továbbra is elérhető, de előfordulhat, hogy nem megfelelően védelmekor ügyfelek bármely Mobile Apps-ügyfél SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="f67b8-222">Any controller that does not have **MobileAppControllerAttribute** applied can still be accessed by clients, but it may not be correctly consumed by clients using any Mobile App client SDK.</span></span>

## <a name="how-to-work-with-authentication"></a><span data-ttu-id="f67b8-223">Útmutató: hitelesítés</span><span class="sxs-lookup"><span data-stu-id="f67b8-223">How to: Work with authentication</span></span>
<span data-ttu-id="f67b8-224">Az Azure Mobile Apps használja az App Service hitelesítési / engedélyezési biztonságossá tételéhez a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f67b8-224">Azure Mobile Apps uses App Service Authentication / Authorization to secure your mobile backend.</span></span>  <span data-ttu-id="f67b8-225">Ez a szakasz bemutatja, hogyan hajthat végre a következő hitelesítési kapcsolatos feladatokat a .NET server háttérprojekt:</span><span class="sxs-lookup"><span data-stu-id="f67b8-225">This section shows you how to perform the following authentication-related tasks in your .NET backend server project:</span></span>

* [<span data-ttu-id="f67b8-226">Útmutató: hitelesítés hozzáadása egy kiszolgálóprojektet</span><span class="sxs-lookup"><span data-stu-id="f67b8-226">How to: Add authentication to a server project</span></span>](#add-auth)
* [<span data-ttu-id="f67b8-227">Útmutató: az alkalmazás egyéni hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="f67b8-227">How to: Use custom authentication for your application</span></span>](#custom-auth)
* [<span data-ttu-id="f67b8-228">Hogyan: lekérése hitelesített felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="f67b8-228">How to: Retrieve authenticated user information</span></span>](#user-info)
* [<span data-ttu-id="f67b8-229">Útmutató: a jogosult felhasználók adatokhoz való hozzáférést</span><span class="sxs-lookup"><span data-stu-id="f67b8-229">How to: Restrict data access for authorized users</span></span>](#authorize)

### <span data-ttu-id="f67b8-230"><a name="add-auth"></a>Útmutató: hitelesítés hozzáadása egy kiszolgálóprojektet</span><span class="sxs-lookup"><span data-stu-id="f67b8-230"><a name="add-auth"></a>How to: Add authentication to a server project</span></span>
<span data-ttu-id="f67b8-231">Kiterjesztésével server projektjéhez hitelesítési is hozzáadhat a **MobileAppConfiguration** objektum és OWIN köztes konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f67b8-231">You can add authentication to your server project by extending the **MobileAppConfiguration** object and configuring OWIN middleware.</span></span> <span data-ttu-id="f67b8-232">Amikor telepíti a [Microsoft.Azure.Mobile.Server.Quickstart] csomag- és hívás a **UseDefaultConfiguration** kiterjesztésmetódus, ugorjon a 3. lépés.</span><span class="sxs-lookup"><span data-stu-id="f67b8-232">When you install the [Microsoft.Azure.Mobile.Server.Quickstart] package and call the **UseDefaultConfiguration** extension method, you can skip to step 3.</span></span>

1. <span data-ttu-id="f67b8-233">A Visual Studio telepíti a [Microsoft.Azure.Mobile.Server.Authentication] csomag.</span><span class="sxs-lookup"><span data-stu-id="f67b8-233">In Visual Studio, install the [Microsoft.Azure.Mobile.Server.Authentication] package.</span></span>
2. <span data-ttu-id="f67b8-234">A Startup.cs projekt fájlban adja hozzá a következő kódsort elején a **konfigurációs** módszert:</span><span class="sxs-lookup"><span data-stu-id="f67b8-234">In the Startup.cs project file, add the following line of code at the beginning of the **Configuration** method:</span></span>

        app.UseAppServiceAuthentication(config);

    <span data-ttu-id="f67b8-235">Az OWIN köztes összetevő ellenőrzi a társított App Service-átjáró által kiállított jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="f67b8-235">This OWIN middleware component validates tokens issued by the associated App Service gateway.</span></span>
3. <span data-ttu-id="f67b8-236">Adja hozzá a `[Authorize]` bármely tartományvezérlő vagy a metódust, amelyhez hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="f67b8-236">Add the `[Authorize]` attribute to any controller or method that requires authentication.</span></span>

<span data-ttu-id="f67b8-237">Hitelesíti az ügyfeleket a Mobile Apps-háttéralkalmazásának kapcsolatos további tudnivalókért lásd: [hitelesítés hozzáadása az alkalmazáshoz](app-service-mobile-ios-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="f67b8-237">To learn about how to authenticate clients to your Mobile Apps backend, see [Add authentication to your app](app-service-mobile-ios-get-started-users.md).</span></span>

### <span data-ttu-id="f67b8-238"><a name="custom-auth"></a>Útmutató: az alkalmazás egyéni hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="f67b8-238"><a name="custom-auth"></a>How to: Use custom authentication for your application</span></span>
<span data-ttu-id="f67b8-239">Ha nem szeretné az App Service hitelesítési/engedélyezési szolgáltatók egyikét kell használnia, a saját bejelentkezési rendszer valósíthat meg.</span><span class="sxs-lookup"><span data-stu-id="f67b8-239">If you do not wish to use one of the App Service Authentication/Authorization providers, you can implement your own login system.</span></span> <span data-ttu-id="f67b8-240">Telepítse a [Microsoft.Azure.Mobile.Server.Login] csomag elősegítve ezzel a hitelesítési jogkivonatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="f67b8-240">Install the [Microsoft.Azure.Mobile.Server.Login] package to assist with authentication token generation.</span></span>  <span data-ttu-id="f67b8-241">Adja meg a saját kódot a felhasználó hitelesítő adatainak ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="f67b8-241">Provide your own code for validating user credentials.</span></span> <span data-ttu-id="f67b8-242">Például ellenőrizze sózott és kivonatolt jelszavakat adatbázisban ellen.</span><span class="sxs-lookup"><span data-stu-id="f67b8-242">For example, you might check against salted and hashed passwords in a database.</span></span> <span data-ttu-id="f67b8-243">Az alábbi példában a `isValidAssertion()` (határozni) metódus felelős az ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="f67b8-243">In the example below, the `isValidAssertion()` method (defined elsewhere) is responsible for these checks.</span></span>

<span data-ttu-id="f67b8-244">Az egyéni hitelesítési fel van fedve egy ApiController létrehozásával, és az ilyen `register` és `login` műveletek.</span><span class="sxs-lookup"><span data-stu-id="f67b8-244">The custom authentication is exposed by creating an ApiController and exposing `register` and `login` actions.</span></span> <span data-ttu-id="f67b8-245">Az ügyfél által használandó egyéni felhasználói Felületet a Információgyűjtés a felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="f67b8-245">The client should use a custom UI to collect the information from the user.</span></span>  <span data-ttu-id="f67b8-246">Az információkat majd szabványos HTTP POST hívja az API számára.</span><span class="sxs-lookup"><span data-stu-id="f67b8-246">The information is then submitted to the API with a standard HTTP POST call.</span></span> <span data-ttu-id="f67b8-247">Ha a kiszolgáló ellenőrzi a helyességi feltétel, jogkivonat használatával kiadott a `AppServiceLoginHandler.CreateToken()` metódust.</span><span class="sxs-lookup"><span data-stu-id="f67b8-247">Once the server validates the assertion, a token is issued using the `AppServiceLoginHandler.CreateToken()` method.</span></span>  <span data-ttu-id="f67b8-248">A ApiController **nem kell** használja a `[MobileAppController]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="f67b8-248">The ApiController **should not** use the `[MobileAppController]` attribute.</span></span>

<span data-ttu-id="f67b8-249">Példa `login` művelet:</span><span class="sxs-lookup"><span data-stu-id="f67b8-249">An example `login` action:</span></span>

        public IHttpActionResult Post([FromBody] JObject assertion)
        {
            if (isValidAssertion(assertion)) // user-defined function, checks against a database
            {
                JwtSecurityToken token = AppServiceLoginHandler.CreateToken(new Claim[] { new Claim(JwtRegisteredClaimNames.Sub, assertion["username"]) },
                    mySigningKey,
                    myAppURL,
                    myAppURL,
                    TimeSpan.FromHours(24) );
                return Ok(new LoginResult()
                {
                    AuthenticationToken = token.RawData,
                    User = new LoginResultUser() { UserId = userName.ToString() }
                });
            }
            else // user assertion was not valid
            {
                return this.Request.CreateUnauthorizedResponse();
            }
        }

<span data-ttu-id="f67b8-250">Az előző példában a LoginResult és LoginResultUser kötelező tulajdonságok feltáró szerializálható objektumok.</span><span class="sxs-lookup"><span data-stu-id="f67b8-250">In the preceding example, LoginResult and LoginResultUser are serializable objects exposing required properties.</span></span> <span data-ttu-id="f67b8-251">Az ügyfél vár a válaszokat, vissza kell adni az űrlap JSON-objektumként:</span><span class="sxs-lookup"><span data-stu-id="f67b8-251">The client expects login responses to be returned as JSON objects of the form:</span></span>

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

<span data-ttu-id="f67b8-252">A `AppServiceLoginHandler.CreateToken()` metódust tartalmaz egy *célközönség* és egy *kibocsátó* paraméter.</span><span class="sxs-lookup"><span data-stu-id="f67b8-252">The `AppServiceLoginHandler.CreateToken()` method includes an *audience* and an *issuer* parameter.</span></span> <span data-ttu-id="f67b8-253">Mindkét paraméter beállítása az URL-CÍMÉT az alkalmazás gyökereként, a HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="f67b8-253">Both of these parameters are set to the URL of your application root, using the HTTPS scheme.</span></span> <span data-ttu-id="f67b8-254">Hasonló módon állítsa be *secretKey* kell lennie az értéket az alkalmazás csomagazonosítóját aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="f67b8-254">Similarly you should set *secretKey* to be the value of your application's signing key.</span></span> <span data-ttu-id="f67b8-255">Az aláírási kulcs ügyfélprogram ne ossza el a kulcsok Mentaízű és megszemélyesíthet felhasználókat is használható.</span><span class="sxs-lookup"><span data-stu-id="f67b8-255">Do not distribute the signing key in a client as it can be used to mint keys and impersonate users.</span></span> <span data-ttu-id="f67b8-256">Ezt úgy szerezheti be az aláírási kulcs közben Vezérlőpultjának az App Service-ben üzemeltetett a *webhely\_AUTH\_aláírás\_kulcs* környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="f67b8-256">You can obtain the signing key while hosted in App Service by referencing the *WEBSITE\_AUTH\_SIGNING\_KEY* environment variable.</span></span> <span data-ttu-id="f67b8-257">Ha szükséges a helyi hibakeresési környezetben, kövesse az utasításokat a a [hitelesítéssel helyi hibakeresés](#local-debug) lekérni a kulcsot, és alkalmazás-beállítás szerint tárolja a szakaszban található.</span><span class="sxs-lookup"><span data-stu-id="f67b8-257">If needed in a local debugging context, follow the instructions in the [Local debugging with authentication](#local-debug) section to retrieve the key and store it as an application setting.</span></span>

<span data-ttu-id="f67b8-258">A kibocsátott jogkivonat más jogcímeket és lejárati dátummal is.</span><span class="sxs-lookup"><span data-stu-id="f67b8-258">The issued token may also include other claims and an expiry date.</span></span>  <span data-ttu-id="f67b8-259">A minimális követelmény, a kibocsátott jogkivonat tartalmaznia kell a tulajdonos (**sub**) jogcímek.</span><span class="sxs-lookup"><span data-stu-id="f67b8-259">Minimally, the issued token must include a subject (**sub**) claim.</span></span>

<span data-ttu-id="f67b8-260">A szabványos ügyfél támogathatja `loginAsync()` metódus által a hitelesítési útvonal terhelve.</span><span class="sxs-lookup"><span data-stu-id="f67b8-260">You can support the standard client `loginAsync()` method by overloading the authentication route.</span></span>  <span data-ttu-id="f67b8-261">Ha az ügyfél meghívja a `client.loginAsync('custom');` a bejelentkezéshez, az útvonal lehet `/.auth/login/custom`.</span><span class="sxs-lookup"><span data-stu-id="f67b8-261">If the client calls `client.loginAsync('custom');` to log in, your route must be `/.auth/login/custom`.</span></span>  <span data-ttu-id="f67b8-262">Beállíthatja, hogy az egyéni vezérlő használatával útvonal `MapHttpRoute()`:</span><span class="sxs-lookup"><span data-stu-id="f67b8-262">You can set the route for the custom authentication controller using `MapHttpRoute()`:</span></span>

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> <span data-ttu-id="f67b8-263">Használja a `loginAsync()` megközelítés biztosítja, hogy a hitelesítési jogkivonat csatolva van-e a szolgáltatás minden ezt követő hívásakor.</span><span class="sxs-lookup"><span data-stu-id="f67b8-263">Using the `loginAsync()` approach ensures that the authentication token is attached to every subsequent call to the service.</span></span>
>
>

### <span data-ttu-id="f67b8-264"><a name="user-info"></a>Hogyan: lekérése hitelesített felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="f67b8-264"><a name="user-info"></a>How to: Retrieve authenticated user information</span></span>
<span data-ttu-id="f67b8-265">Amikor a felhasználó hitelesítése az App Service, a hozzárendelt felhasználói Azonosítót és egyéb információkat hozzáférhet a .NET-háttérrendszer kódban.</span><span class="sxs-lookup"><span data-stu-id="f67b8-265">When a user is authenticated by App Service, you can access the assigned user ID and other information in your .NET backend code.</span></span> <span data-ttu-id="f67b8-266">A felhasználói adatok használható hitelesítési döntések meghozatala során a háttérben.</span><span class="sxs-lookup"><span data-stu-id="f67b8-266">The user information can be used for making authorization decisions in the backend.</span></span> <span data-ttu-id="f67b8-267">A következő kódot a felhasználói Azonosítóját a kérelemhez társított szerzi be:</span><span class="sxs-lookup"><span data-stu-id="f67b8-267">The following code obtains the user ID associated with a request:</span></span>

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

<span data-ttu-id="f67b8-268">A SID származó szolgáltatói felhasználói Azonosítóját, és egy adott felhasználó és a bejelentkezés-szolgáltató statikus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-268">The SID is derived from the provider-specific user ID and is static for a given user and login provider.</span></span>  <span data-ttu-id="f67b8-269">A SID értéke érvénytelen a hitelesítési tokenek null.</span><span class="sxs-lookup"><span data-stu-id="f67b8-269">The SID is null for invalid authentication tokens.</span></span>

<span data-ttu-id="f67b8-270">App Service lehetővé teszi a bejelentkezési szolgáltató által adott jogcímek igénylésére.</span><span class="sxs-lookup"><span data-stu-id="f67b8-270">App Service also lets you request specific claims from your login provider.</span></span> <span data-ttu-id="f67b8-271">Minden egyes identitásszolgáltató az identitásszolgáltató SDK használatával további információk megadására.</span><span class="sxs-lookup"><span data-stu-id="f67b8-271">Each identity provider can provide more information using the identity provider SDK.</span></span>  <span data-ttu-id="f67b8-272">Például használhatja a Facebook Graph API-t ismerősök információt.</span><span class="sxs-lookup"><span data-stu-id="f67b8-272">For example, you can use the Facebook Graph API for friends information.</span></span>  <span data-ttu-id="f67b8-273">A szolgáltató panel az Azure portálon is megadhat igényelt jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="f67b8-273">You can specify claims that are requested in the provider blade in the Azure portal.</span></span> <span data-ttu-id="f67b8-274">Egyes jogcímek a identitásszolgáltatóval további beállításokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="f67b8-274">Some claims require additional configuration with the identity provider.</span></span>

<span data-ttu-id="f67b8-275">A következő kódot a hívások a **GetAppServiceIdentityAsync** kiterjesztésmetódus a bejelentkezési hitelesítő adatokat, többek között a hozzáférési jogkivonatának beszerzéséhez szükséges kéréseket a meghatározott a Facebook Graph API:</span><span class="sxs-lookup"><span data-stu-id="f67b8-275">The following code calls the **GetAppServiceIdentityAsync** extension method to get the login credentials, which include the access token needed to make requests against the Facebook Graph API:</span></span>

    // Get the credentials for the logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with the Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request the current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with the Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

<span data-ttu-id="f67b8-276">Adja hozzá egy utasítást `System.Security.Principal` biztosításához a **GetAppServiceIdentityAsync** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="f67b8-276">Add a using statement for `System.Security.Principal` to provide the **GetAppServiceIdentityAsync** extension method.</span></span>

### <span data-ttu-id="f67b8-277"><a name="authorize"></a>Útmutató: a jogosult felhasználók adatokhoz való hozzáférést</span><span class="sxs-lookup"><span data-stu-id="f67b8-277"><a name="authorize"></a>How to: Restrict data access for authorized users</span></span>
<span data-ttu-id="f67b8-278">Az előző szakaszban azt bemutatta, hogyan lehet lekérni a hitelesített felhasználók felhasználói Azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="f67b8-278">In the previous section, we showed how to retrieve the user ID of an authenticated user.</span></span> <span data-ttu-id="f67b8-279">Adatokhoz és más erőforrások, ez az érték alapján korlátozhatja a hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="f67b8-279">You can restrict access to data and other resources based on this value.</span></span> <span data-ttu-id="f67b8-280">Például userId oszlop hozzáadásával táblák és a lekérdezés eredményeit a felhasználói azonosító szűrés módja a egyszerű visszaadott adatok csak a jogosult felhasználókra korlátozzák.</span><span class="sxs-lookup"><span data-stu-id="f67b8-280">For example, adding a userId column to tables and filtering the query results by the user ID is a simple way to limit returned data only to authorized users.</span></span> <span data-ttu-id="f67b8-281">A következő kódot adja vissza adatok csak akkor, ha a biztonsági azonosítója megegyezik-e a TodoItem tábla UserId oszlopban:</span><span class="sxs-lookup"><span data-stu-id="f67b8-281">The following code returns data rows only when the SID matches the value in the UserId column on the TodoItem table:</span></span>

    // Get the SID of the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong to the current user.
    return Query().Where(t => t.UserId == sid);

<span data-ttu-id="f67b8-282">A `Query()` metódus értéket ad vissza egy `IQueryable` , kezelni a szűrés LINQ kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="f67b8-282">The `Query()` method returns an `IQueryable` that can be manipulated by LINQ to handle filtering.</span></span>

## <a name="how-to-add-push-notifications-to-a-server-project"></a><span data-ttu-id="f67b8-283">Hogyan: adhat leküldéses értesítések küldéséhez egy kiszolgálóprojektet</span><span class="sxs-lookup"><span data-stu-id="f67b8-283">How to: Add push notifications to a server project</span></span>
<span data-ttu-id="f67b8-284">Leküldéses értesítések hozzáadása a projekt kiterjesztésével a **MobileAppConfiguration** objektum és a Notification Hubs ügyfelet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f67b8-284">Add push notifications to your server project by extending the **MobileAppConfiguration** object and creating a Notification Hubs client.</span></span>

1. <span data-ttu-id="f67b8-285">A Visual Studióban, kattintson a jobb gombbal a projekt, és kattintson a **NuGet-csomagok kezelése**, keressen `Microsoft.Azure.Mobile.Server.Notifications`, majd kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-285">In Visual Studio, right-click the server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.Mobile.Server.Notifications`, then click **Install**.</span></span>
2. <span data-ttu-id="f67b8-286">Telepítse ezt a lépést ismételje meg a `Microsoft.Azure.NotificationHubs` csomag, amely tartalmazza a Notification Hubs ügyféloldali kódtárára.</span><span class="sxs-lookup"><span data-stu-id="f67b8-286">Repeat this step to install the `Microsoft.Azure.NotificationHubs` package, which includes the Notification Hubs client library.</span></span>
3. <span data-ttu-id="f67b8-287">A App_Start/Startup.MobileApp.cs és adjon hozzá egy a **AddPushNotifications()** kiterjesztésmetódus inicializálása közben:</span><span class="sxs-lookup"><span data-stu-id="f67b8-287">In App_Start/Startup.MobileApp.cs, and add a call to the **AddPushNotifications()** extension method during initialization:</span></span>

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. <span data-ttu-id="f67b8-288">Az alábbi kódot, amely létrehozza a Notification Hubs-ügyfél hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f67b8-288">Add the following code that creates a Notification Hubs client:</span></span>

        // Get the settings for the server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get the Notification Hubs credentials for the Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

<span data-ttu-id="f67b8-289">A Notification Hubs-ügyfél leküldéses értesítések küldésére regisztrált eszközöket használhatja.</span><span class="sxs-lookup"><span data-stu-id="f67b8-289">You can now use the Notification Hubs client to send push notifications to registered devices.</span></span> <span data-ttu-id="f67b8-290">További információkért lásd: [leküldéses értesítések hozzáadása az alkalmazáshoz](app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="f67b8-290">For more information, see [Add push notifications to your app](app-service-mobile-ios-get-started-push.md).</span></span> <span data-ttu-id="f67b8-291">Notification hubs használatával kapcsolatos további információkért lásd: [Notification Hubs – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="f67b8-291">To learn more about Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

## <span data-ttu-id="f67b8-292"><a name="tags"></a>Hogyan: engedélyezése megcélzott ügyfélleküldéses címkék használatával</span><span class="sxs-lookup"><span data-stu-id="f67b8-292"><a name="tags"></a>How to: Enable targeted push using Tags</span></span>
<span data-ttu-id="f67b8-293">A Notification Hubs lehetővé teszi célzott értesítések küldése adott regisztrációk címkék használatával.</span><span class="sxs-lookup"><span data-stu-id="f67b8-293">Notification Hubs lets you send targeted notifications to specific registrations by using tags.</span></span> <span data-ttu-id="f67b8-294">Több címkék jönnek létre automatikusan:</span><span class="sxs-lookup"><span data-stu-id="f67b8-294">Several tags are created automatically:</span></span>

* <span data-ttu-id="f67b8-295">A telepítési azonosító azonosítja az adott eszköz.</span><span class="sxs-lookup"><span data-stu-id="f67b8-295">The Installation ID identifies a specific device.</span></span>
* <span data-ttu-id="f67b8-296">A felhasználói azonosítóját, a hitelesített SID azonosítója alapján azonosítja az egy adott felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f67b8-296">The User Id based on the authenticated SID identifies a specific user.</span></span>

<span data-ttu-id="f67b8-297">A telepítési azonosító érhetők el a **végrehajtott** tulajdonságát a **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-297">The installation ID can be accessed from the **installationId** property on the **MobileServiceClient**.</span></span>  <span data-ttu-id="f67b8-298">A következő példa bemutatja, hogyan használja a telepítési Azonosítót a Notification Hubs egy adott telepítési egy címke hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="f67b8-298">The following example shows how to use an installation ID to add a tag to a specific installation in Notification Hubs:</span></span>

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

<span data-ttu-id="f67b8-299">Leküldéses értesítés regisztrálása során az ügyfél által megadott címkéket figyelmen kívül hagyja a kiszolgáló létrehozása a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="f67b8-299">Any tags supplied by the client during push notification registration are ignored by the backend when creating the installation.</span></span> <span data-ttu-id="f67b8-300">Ahhoz, hogy egy ügyfél címkék hozzáadása a telepítés, létre kell hoznia egy egyéni API-t, amely az előző minta használatával címkék.</span><span class="sxs-lookup"><span data-stu-id="f67b8-300">To enable a client to add tags to the installation, you must create a custom API that adds tags using the preceding pattern.</span></span>

<span data-ttu-id="f67b8-301">Lásd: [ügyfél által hozzáadott leküldéses értesítések címkék] [ 5] például egy App Service Mobile Apps befejezett gyors üzembe helyezési minta.</span><span class="sxs-lookup"><span data-stu-id="f67b8-301">See [Client-added push notification tags][5] in the App Service Mobile Apps completed quickstart sample for an example.</span></span>

## <span data-ttu-id="f67b8-302"><a name="push-user"></a>Hogyan: leküldéses értesítések küldése a hitelesített felhasználó</span><span class="sxs-lookup"><span data-stu-id="f67b8-302"><a name="push-user"></a>How to: Send push notifications to an authenticated user</span></span>
<span data-ttu-id="f67b8-303">Amikor a hitelesített felhasználó regisztrálja a leküldéses értesítések, a felhasználói azonosító címke automatikusan hozzáadódik a regisztráció.</span><span class="sxs-lookup"><span data-stu-id="f67b8-303">When an authenticated user registers for push notifications, a user ID tag is automatically added to the registration.</span></span> <span data-ttu-id="f67b8-304">Ezt a címkét használatával leküldéses értesítéseket küldhet az adott személy által regisztrált összes eszközre.</span><span class="sxs-lookup"><span data-stu-id="f67b8-304">By using this tag, you can send push notifications to all devices registered by that person.</span></span> <span data-ttu-id="f67b8-305">Az alábbi kód lekérdezi a kérelmező felhasználó biztonsági azonosítója és minden eszközregisztráció személyre sablon leküldéses értesítést küld:</span><span class="sxs-lookup"><span data-stu-id="f67b8-305">The following code gets the SID of user making the request and sends a template push notification to every device registration for that person:</span></span>

    // Get the current user SID and create a tag for the current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for the template with the item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification to the user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

<span data-ttu-id="f67b8-306">Amikor regisztrál egy hitelesített ügyfél leküldéses értesítésekhez, győződjön meg arról, hogy a hitelesítési teljes regisztrációs megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="f67b8-306">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span> <span data-ttu-id="f67b8-307">További információkért lásd: [felhasználók leküldése] [ 6] .NET-háttérrendszer az App Service Mobile Apps befejezett gyors üzembe helyezési minta.</span><span class="sxs-lookup"><span data-stu-id="f67b8-307">For more information, see [Push to users][6] in the App Service Mobile Apps completed quickstart sample for .NET backend.</span></span>

## <a name="how-to-debug-and-troubleshoot-the-net-server-sdk"></a><span data-ttu-id="f67b8-308">Hogyan: hibakeresését és a .NET Server SDK hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="f67b8-308">How to: Debug and troubleshoot the .NET Server SDK</span></span>
<span data-ttu-id="f67b8-309">Az Azure App Service számos Hibakeresés és hibaelhárítási módszerekről az ASP.NET-alkalmazások biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f67b8-309">Azure App Service provides several debugging and troubleshooting techniques for ASP.NET applications:</span></span>

* [<span data-ttu-id="f67b8-310">Az Azure App Service figyelése</span><span class="sxs-lookup"><span data-stu-id="f67b8-310">Monitoring an Azure App Service</span></span>](../app-service-web/web-sites-monitor.md)
* [<span data-ttu-id="f67b8-311">Az Azure App Service diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f67b8-311">Enable Diagnostic Logging in Azure App Service</span></span>](../app-service-web/web-sites-enable-diagnostic-log.md)
* [<span data-ttu-id="f67b8-312">A Visual Studio egy Azure App Service hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="f67b8-312">Troubleshoot an Azure App Service in Visual Studio</span></span>](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a><span data-ttu-id="f67b8-313">Naplózás</span><span class="sxs-lookup"><span data-stu-id="f67b8-313">Logging</span></span>
<span data-ttu-id="f67b8-314">App Service diagnosztikai naplók írni az ASP.NET nyomkövetési írását normál használatával.</span><span class="sxs-lookup"><span data-stu-id="f67b8-314">You can write to App Service diagnostic logs by using the standard ASP.NET trace writing.</span></span> <span data-ttu-id="f67b8-315">Ahhoz, hogy a naplókba, engedélyeznie kell a Mobile Apps-háttéralkalmazás diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="f67b8-315">Before you can write to the logs, you must enable diagnostics in your Mobile App backend.</span></span>

<span data-ttu-id="f67b8-316">Diagnosztika engedélyezése, és a naplófájlok írásához:</span><span class="sxs-lookup"><span data-stu-id="f67b8-316">To enable diagnostics and write to the logs:</span></span>

1. <span data-ttu-id="f67b8-317">Kövesse a [diagnosztika engedélyezésével](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span><span class="sxs-lookup"><span data-stu-id="f67b8-317">Follow the steps in [How to enable diagnostics](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span></span>
2. <span data-ttu-id="f67b8-318">Adja hozzá a következő using utasítást a kód fájlban:</span><span class="sxs-lookup"><span data-stu-id="f67b8-318">Add the following using statement in your code file:</span></span>

        using System.Web.Http.Tracing;
3. <span data-ttu-id="f67b8-319">Hozzon létre egy nyomkövetési író írni a .NET-háttérrendszer a diagnosztikai naplók, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f67b8-319">Create a trace writer to write from the .NET backend to the diagnostic logs, as follows:</span></span>

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. <span data-ttu-id="f67b8-320">A projekt közzé, és hozzáférni a Mobile Apps-háttéralkalmazás a kód útvonalat a naplózási végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="f67b8-320">Republish your server project, and access the Mobile App backend to execute the code path with the logging.</span></span>
5. <span data-ttu-id="f67b8-321">Töltse le és kiértékelheti a naplókat, a [hogyan: naplók letöltéséhez](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span><span class="sxs-lookup"><span data-stu-id="f67b8-321">Download and evaluate the logs, as described in [How to: Download logs](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span></span>

### <span data-ttu-id="f67b8-322"><a name="local-debug"></a>Helyi hitelesítéssel hibakeresés</span><span class="sxs-lookup"><span data-stu-id="f67b8-322"><a name="local-debug"></a>Local debugging with authentication</span></span>
<span data-ttu-id="f67b8-323">Az alkalmazás helyileg tesztelje a módosításokat a felhő közzététel előtt is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="f67b8-323">You can run your application locally to test changes before publishing them to the cloud.</span></span> <span data-ttu-id="f67b8-324">Nyomja le az Azure Mobile Apps-háttérkiszolgálókon legtöbb, *F5* a Visual Studio során.</span><span class="sxs-lookup"><span data-stu-id="f67b8-324">For most Azure Mobile Apps backends, press *F5* while in Visual Studio.</span></span> <span data-ttu-id="f67b8-325">Van azonban néhány további szempontok hitelesítése során.</span><span class="sxs-lookup"><span data-stu-id="f67b8-325">However, there are some additional considerations when using authentication.</span></span>

<span data-ttu-id="f67b8-326">Rendelkeznie kell egy felhőalapú mobilalkalmazást az App Service hitelesítési/engedélyezési konfigurálva, és az ügyfél a megadott másodlagos bejelentkezési állomásaként felhő végpontot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="f67b8-326">You must have a cloud-based mobile app with App Service Authentication/Authorization configured, and your client must have the cloud endpoint specified as the alternate login host.</span></span> <span data-ttu-id="f67b8-327">A szükséges lépéseit ügyfélplatform dokumentációjában olvashatók.</span><span class="sxs-lookup"><span data-stu-id="f67b8-327">See the documentation for your client platform for the specific steps required.</span></span>

<span data-ttu-id="f67b8-328">Győződjön meg arról, hogy rendelkezik-e a mobil-háttéralkalmazást [Microsoft.Azure.Mobile.Server.Authentication] telepítve.</span><span class="sxs-lookup"><span data-stu-id="f67b8-328">Ensure that your mobile backend has [Microsoft.Azure.Mobile.Server.Authentication] installed.</span></span> <span data-ttu-id="f67b8-329">Ezt követően az alkalmazás OWIN indítási osztályt, adja hozzá a következő után `MobileAppConfiguration` telepítve van a `HttpConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="f67b8-329">Then, in your application's OWIN startup class, add the following, after `MobileAppConfiguration` has been applied to your `HttpConfiguration`:</span></span>

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

<span data-ttu-id="f67b8-330">A fenti példában, konfigurálnia kell a *authAudience* és *authIssuer* Alkalmazásbeállítások belül a Web.config fájl az egyes URL-CÍMÉT az alkalmazás gyökereként használja a HTTPS sémát kell.</span><span class="sxs-lookup"><span data-stu-id="f67b8-330">In the preceding example, you should configure the *authAudience* and *authIssuer* application settings within your Web.config file to each be the URL of your application root, using the HTTPS scheme.</span></span> <span data-ttu-id="f67b8-331">Hasonló módon állítsa be *authSigningKey* kell lennie az értéket az alkalmazás csomagazonosítóját aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="f67b8-331">Similarly you should set *authSigningKey* to be the value of your application's signing key.</span></span>
<span data-ttu-id="f67b8-332">Az aláírási kulcs beszerzése:</span><span class="sxs-lookup"><span data-stu-id="f67b8-332">To obtain the signing key:</span></span>

1. <span data-ttu-id="f67b8-333">Keresse meg az alkalmazás belül a [Azure-portálon]</span><span class="sxs-lookup"><span data-stu-id="f67b8-333">Navigate to your app within the [Azure portal]</span></span>
2. <span data-ttu-id="f67b8-334">Kattintson a **eszközök**, **Kudu**, **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-334">Click **Tools**, **Kudu**, **Go**.</span></span>
3. <span data-ttu-id="f67b8-335">A Kudu felügyeleti hely, kattintson a **környezet**.</span><span class="sxs-lookup"><span data-stu-id="f67b8-335">In the Kudu Management site, click **Environment**.</span></span>
4. <span data-ttu-id="f67b8-336">Keresse meg az értéket a *webhely\_AUTH\_aláírás\_kulcs*.</span><span class="sxs-lookup"><span data-stu-id="f67b8-336">Find the value for *WEBSITE\_AUTH\_SIGNING\_KEY*.</span></span>

<span data-ttu-id="f67b8-337">Az aláírási kulcs használata a *authSigningKey* paraméter a helyi alkalmazások Config.</span><span class="sxs-lookup"><span data-stu-id="f67b8-337">Use the signing key for the *authSigningKey* parameter in your local application config.</span></span>  <span data-ttu-id="f67b8-338">A mobil-háttéralkalmazást most már rendelkezik a érvényesíthet jogkivonatokat, a helyi futtatás során, amelyek az ügyfél a felhőalapú végpontról a jogkivonat szerzik be.</span><span class="sxs-lookup"><span data-stu-id="f67b8-338">Your mobile backend is now equipped to validate tokens when running locally, which the client obtains the token from the cloud-based endpoint.</span></span>

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
<span data-ttu-id="f67b8-339">[Azure-portálon]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="f67b8-339">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="f67b8-340">[NuGet.org]: http://www.nuget.org/</span><span class="sxs-lookup"><span data-stu-id="f67b8-340">[NuGet.org]: http://www.nuget.org/</span></span>
<span data-ttu-id="f67b8-341">[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/</span><span class="sxs-lookup"><span data-stu-id="f67b8-341">[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/</span></span>
<span data-ttu-id="f67b8-342">[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/</span><span class="sxs-lookup"><span data-stu-id="f67b8-342">[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/</span></span>
<span data-ttu-id="f67b8-343">[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/</span><span class="sxs-lookup"><span data-stu-id="f67b8-343">[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/</span></span>
<span data-ttu-id="f67b8-344">[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/</span><span class="sxs-lookup"><span data-stu-id="f67b8-344">[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/</span></span>
<span data-ttu-id="f67b8-345">[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/</span><span class="sxs-lookup"><span data-stu-id="f67b8-345">[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/</span></span>
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

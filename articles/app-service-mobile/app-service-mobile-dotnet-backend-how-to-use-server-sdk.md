---
title: "aaaHow toowork hello .NET háttérrendszer kiszolgálóval a Mobile Apps SDK |} Microsoft Docs"
description: "Ismerje meg, hogyan toowork az Azure App Service Mobile Apps háttérkiszolgáló .NET SDK hello."
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
ms.openlocfilehash: 2946c5ba4424565ac764e2ce5597bf42362fcedf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="work-with-hello-net-backend-server-sdk-for-azure-mobile-apps"></a><span data-ttu-id="1cdf3-104">Az Azure Mobile Apps hello .NET háttérkiszolgáló SDK használata</span><span class="sxs-lookup"><span data-stu-id="1cdf3-104">Work with hello .NET backend server SDK for Azure Mobile Apps</span></span>
[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

<span data-ttu-id="1cdf3-105">Ez a témakör bemutatja, hogyan toouse hello .NET háttérkiszolgáló SDK az Azure App Service Mobile Apps főbb forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-105">This topic shows you how toouse hello .NET backend server SDK in key Azure App Service Mobile Apps scenarios.</span></span> <span data-ttu-id="1cdf3-106">hello Azure Mobile Apps SDK segítséget nyújt az ASP.NET-alkalmazás a mobil ügyfelek dolgozni.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-106">hello Azure Mobile Apps SDK helps you work with mobile clients from your ASP.NET application.</span></span>

> [!TIP]
> <span data-ttu-id="1cdf3-107">Hello [.NET server az Azure Mobile Apps SDK] [ 2] nyílt forráskódú a Githubon.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-107">hello [.NET server SDK for Azure Mobile Apps][2] is open source on GitHub.</span></span> <span data-ttu-id="1cdf3-108">hello tárház tartalmazza az összes hello teljes server SDK egység tesztcsomag és néhány mintaprojektjeit beleértve.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-108">hello repository contains all source code including hello entire server SDK unit test suite and some sample projects.</span></span>
>
>

## <a name="reference-documentation"></a><span data-ttu-id="1cdf3-109">Segédanyagok</span><span class="sxs-lookup"><span data-stu-id="1cdf3-109">Reference documentation</span></span>
<span data-ttu-id="1cdf3-110">hello server SDK hello referenciadokumentációt tartalmaz a következő helyen található: [Azure Mobile Apps .NET hivatkozás][1].</span><span class="sxs-lookup"><span data-stu-id="1cdf3-110">hello reference documentation for hello server SDK is located here: [Azure Mobile Apps .NET Reference][1].</span></span>

## <span data-ttu-id="1cdf3-111"><a name="create-app"></a>Hogyan: .NET Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-111"><a name="create-app"></a>How to: Create a .NET Mobile App backend</span></span>
<span data-ttu-id="1cdf3-112">Ha a számítógépet egy új projektet, létrehozhat egy App Service-alkalmazást vagy hello [Azure-portálon] vagy a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-112">If you are starting a new project, you can create an App Service application using either hello [Azure portal] or Visual Studio.</span></span> <span data-ttu-id="1cdf3-113">Hello App Service alkalmazás helyileg történő futtatása, vagy hello projekt tooyour felhőalapú App Service mobile alkalmazás közzététele.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-113">You can run hello App Service application locally or publish hello project tooyour cloud-based App Service mobile app.</span></span>

<span data-ttu-id="1cdf3-114">Ha mobil funkciókkal tooan létező projekt hozzáadni, lásd: hello [töltse le és hello SDK inicializálása](#install-sdk) szakasz.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-114">If you are adding mobile capabilities tooan existing project, see hello [Download and initialize hello SDK](#install-sdk) section.</span></span>

### <a name="create-a-net-backend-using-hello-azure-portal"></a><span data-ttu-id="1cdf3-115">Hello Azure-portál használatával a .NET-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-115">Create a .NET backend using hello Azure portal</span></span>
<span data-ttu-id="1cdf3-116">az App Service mobil-háttéralkalmazást toocreate, vagy hajtsa végre a hello [gyors üzembe helyezési útmutató] [ 3] vagy kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-116">toocreate an App Service mobile backend, either follow hello [Quickstart tutorial][3] or follow these steps:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

<span data-ttu-id="1cdf3-117">Vissza a hello *Ismerkedés* panelen, a **tábla API létrehozása**, válassza a **C#** , a **háttéralkalmazás-nyelv**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-117">Back in hello *Get started* blade, under **Create a table API**, choose **C#** as your **Backend language**.</span></span> <span data-ttu-id="1cdf3-118">Kattintson a **letöltése**, bontsa ki a tömörített project fájlok tooyour helyi számítógépen, és nyissa meg a hello megoldást a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-118">Click **Download**, extract the compressed project files tooyour local computer, and open hello solution in Visual Studio.</span></span>

### <a name="create-a-net-backend-using-visual-studio-2013-and-visual-studio-2015"></a><span data-ttu-id="1cdf3-119">Visual Studio 2013 és a Visual Studio 2015-öt használó .NET-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-119">Create a .NET backend using Visual Studio 2013 and Visual Studio 2015</span></span>
<span data-ttu-id="1cdf3-120">Telepítse a hello [Azure SDK for .NET] [ 4] (2.9.0 verzió vagy újabb) toocreate egy Azure Mobile Apps-projektet a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-120">Install hello [Azure SDK for .NET][4] (version 2.9.0 or later) toocreate an Azure Mobile Apps project in Visual Studio.</span></span> <span data-ttu-id="1cdf3-121">Hello SDK telepítése után hozzon létre egy ASP.NET alkalmazást hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-121">Once you have installed hello SDK, create an ASP.NET application using hello following steps:</span></span>

1. <span data-ttu-id="1cdf3-122">Nyissa meg hello **új projekt** párbeszédpanelen (a *fájl* > **új** > **projekt …** ).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-122">Open hello **New Project** dialog (from *File* > **New** > **Project...**).</span></span>
2. <span data-ttu-id="1cdf3-123">Bontsa ki a **sablonok** > **Visual C#**, és válassza ki **webes**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-123">Expand **Templates** > **Visual C#**, and select **Web**.</span></span>
3. <span data-ttu-id="1cdf3-124">Válassza ki **ASP.NET webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-124">Select **ASP.NET Web Application**.</span></span>
4. <span data-ttu-id="1cdf3-125">Töltse ki hello projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-125">Fill in hello project name.</span></span> <span data-ttu-id="1cdf3-126">Ezután kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-126">Then click **OK**.</span></span>
5. <span data-ttu-id="1cdf3-127">A *ASP.NET 4.5.2 sablont*, jelölje be **Azure Mobile Apps**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-127">Under *ASP.NET 4.5.2 Templates*, select **Azure Mobile App**.</span></span> <span data-ttu-id="1cdf3-128">Ellenőrizze **hello felhőben lévő gazdagéphez** toocreate hello a mobil-háttéralkalmazást cloud toowhich közzéteheti ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-128">Check **Host in hello cloud** toocreate a mobile backend in hello cloud toowhich you can publish this project.</span></span>
6. <span data-ttu-id="1cdf3-129">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-129">Click **OK**.</span></span>

## <span data-ttu-id="1cdf3-130"><a name="install-sdk"></a>Hogyan: Töltse le és hello SDK inicializálása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-130"><a name="install-sdk"></a>How to: Download and initialize hello SDK</span></span>
<span data-ttu-id="1cdf3-131">hello SDK nem érhető el a [NuGet.org]. Ez a csomag hello szükséges alapvető funkciók tooget hello SDK használatának tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-131">hello SDK is available on [NuGet.org]. This package includes hello base functionality required tooget started using hello SDK.</span></span> <span data-ttu-id="1cdf3-132">tooinitialize hello SDK, hello tooperform műveleteket kell **HttpConfiguration** objektum.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-132">tooinitialize hello SDK, you need tooperform actions on hello **HttpConfiguration** object.</span></span>

### <a name="install-hello-sdk"></a><span data-ttu-id="1cdf3-133">Hello SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="1cdf3-133">Install hello SDK</span></span>
<span data-ttu-id="1cdf3-134">tooinstall hello SDK-t, kattintson a jobb gombbal a hello server projektre a Visual Studio kiválasztása **NuGet-csomagok kezelése**, keressen a hello [Microsoft.Azure.Mobile.Server] csomagot, majd kattintson az  **Telepítés**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-134">tooinstall hello SDK, right-click on hello server project in Visual Studio, select **Manage NuGet Packages**, search for hello [Microsoft.Azure.Mobile.Server] package, then click **Install**.</span></span>

### <span data-ttu-id="1cdf3-135"><a name="server-project-setup"></a>Hello kiszolgálóprojektet inicializálása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-135"><a name="server-project-setup"></a> Initialize hello server project</span></span>
<span data-ttu-id="1cdf3-136">Egy .NET-háttérrendszer kiszolgálóprojektet inicializált hasonló tooother ASP.NET projektek, az OWIN indítási osztály-ot.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-136">A .NET backend server project is initialized similar tooother ASP.NET projects, by including an OWIN startup class.</span></span> <span data-ttu-id="1cdf3-137">Győződjön meg arról, hogy rendelkezik-e hivatkozott hello NuGet-csomag `Microsoft.Owin.Host.SystemWeb`.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-137">Ensure that you have referenced hello NuGet package `Microsoft.Owin.Host.SystemWeb`.</span></span> <span data-ttu-id="1cdf3-138">tooadd Ez az osztály a Visual Studióban, kattintson a jobb gombbal a kiszolgáló-projektet, majd válassza ki a **Hozzáadás** >
**új elem**, majd **webes**  >  ** Általános** > **OWIN indítási osztály**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-138">tooadd this class in Visual Studio, right-click on your server project and select **Add** >
**New Item**, then **Web** > **General** > **OWIN Startup class**.</span></span>  <span data-ttu-id="1cdf3-139">Egy osztály hoz létre a következő attribútum hello:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-139">A class is generated with hello following attribute:</span></span>

    [assembly: OwinStartup(typeof(YourServiceName.YourStartupClassName))]

<span data-ttu-id="1cdf3-140">A hello `Configuration()` az OWIN indítási osztályt, használja a metódus egy **HttpConfiguration** objektum tooconfigure hello Azure Mobile Apps-környezetben.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-140">In hello `Configuration()` method of your OWIN startup class, use an **HttpConfiguration** object tooconfigure hello Azure Mobile Apps environment.</span></span>
<span data-ttu-id="1cdf3-141">a következő példa hello inicializálja hello kiszolgálóprojektet semmilyen hozzáadott szolgáltatásokkal:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-141">hello following example initializes hello server project with no added features:</span></span>

    // in OWIN startup class
    public void Configuration(IAppBuilder app)
    {
        HttpConfiguration config = new HttpConfiguration();

        new MobileAppConfiguration()
            // no added features
            .ApplyTo(config);

        app.UseWebApi(config);
    }

<span data-ttu-id="1cdf3-142">tooenable egyes funkciókat, meg kell hívnia kiterjesztésmetódusok a hello **MobileAppConfiguration** hívása előtt objektum **ApplyTo**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-142">tooenable individual features, you must call extension methods on hello **MobileAppConfiguration** object before calling **ApplyTo**.</span></span> <span data-ttu-id="1cdf3-143">Például létrehozza a következő kód hello hello alapértelmezett útvonalak tooall API tartományvezérlők hello attribútummal rendelkező `[MobileAppController]` inicializálása közben:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-143">For example, hello following code adds hello default routes tooall API controllers that have hello attribute `[MobileAppController]` during initialization:</span></span>

    new MobileAppConfiguration()
        .MapApiControllers()
        .ApplyTo(config);

<span data-ttu-id="1cdf3-144">hello server gyors üzembe helyezés az Azure portál hívások hello **UseDefaultConfiguration()**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-144">hello server quickstart from hello Azure portal calls **UseDefaultConfiguration()**.</span></span> <span data-ttu-id="1cdf3-145">A telepítő a következő egyenértékű toohello:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-145">This equivalent toohello following setup:</span></span>

        new MobileAppConfiguration()
            .AddMobileAppHomeController()             // from hello Home package
            .MapApiControllers()
            .AddTables(                               // from hello Tables package
                new MobileAppTableConfiguration()
                    .MapTableControllers()
                    .AddEntityFramework()             // from hello Entity package
                )
            .AddPushNotifications()                   // from hello Notifications package
            .MapLegacyCrossDomainController()         // from hello CrossDomain package
            .ApplyTo(config);

<span data-ttu-id="1cdf3-146">használt hello kiegészítő módszerek a következők:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-146">hello extension methods used are:</span></span>

* <span data-ttu-id="1cdf3-147">`AddMobileAppHomeController()`hello alapértelmezett Azure Mobile Apps kezdőlap biztosít.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-147">`AddMobileAppHomeController()` provides hello default Azure Mobile Apps home page.</span></span>
* <span data-ttu-id="1cdf3-148">`MapApiControllers()`egyéni API képességeket biztosít a hello attribútummal WebAPI tartományvezérlők `[MobileAppController]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-148">`MapApiControllers()` provides custom API capabilities for WebAPI controllers decorated with hello `[MobileAppController]` attribute.</span></span>
* <span data-ttu-id="1cdf3-149">`AddTables()`biztosítja a leképezést, hello `/tables` végpontok tootable tartományvezérlők.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-149">`AddTables()` provides a mapping of hello `/tables` endpoints tootable controllers.</span></span>
* <span data-ttu-id="1cdf3-150">`AddTablesWithEntityFramework()`leképezési hello egy rövid aktuális van `/tables` használó Entity Framework végpontok alapú tartományvezérlők.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-150">`AddTablesWithEntityFramework()` is a short-hand for mapping hello `/tables` endpoints using Entity Framework based controllers.</span></span>
* <span data-ttu-id="1cdf3-151">`AddPushNotifications()`eszközök regisztrálása a Notification Hubs egy egyszerű módszert kínál.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-151">`AddPushNotifications()` provides a simple method of registering devices for Notification Hubs.</span></span>
* <span data-ttu-id="1cdf3-152">`MapLegacyCrossDomainController()`standard CORS fejlécek biztosít helyi fejlesztési.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-152">`MapLegacyCrossDomainController()` provides standard CORS headers for local development.</span></span>

### <a name="sdk-extensions"></a><span data-ttu-id="1cdf3-153">SDK-bővítmények</span><span class="sxs-lookup"><span data-stu-id="1cdf3-153">SDK extensions</span></span>
<span data-ttu-id="1cdf3-154">a következő NuGet-alapú bővítménycsomagok hello adja meg az alkalmazás által használható különböző mobil funkciókat.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-154">hello following NuGet-based extension packages provide various mobile features that can be used by your application.</span></span> <span data-ttu-id="1cdf3-155">Bővítmények inicializálásakor használatával engedélyezheti hello **MobileAppConfiguration** objektum.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-155">You enable extensions during initialization by using hello **MobileAppConfiguration** object.</span></span>

* <span data-ttu-id="1cdf3-156">[Microsoft.Azure.Mobile.Server.Quickstart] támogatja hello alapvető Mobile Apps beállítási.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-156">[Microsoft.Azure.Mobile.Server.Quickstart] Supports hello basic Mobile Apps setup.</span></span> <span data-ttu-id="1cdf3-157">Hívó hello által hozzáadott toohello konfigurációs **UseDefaultConfiguration** kiterjesztésmetódus inicializálása során.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-157">Added toohello configuration by calling hello **UseDefaultConfiguration** extension method during initialization.</span></span> <span data-ttu-id="1cdf3-158">Ezt a bővítményt tartalmazza a következő kiterjesztések: értesítések, a hitelesítés, az entitás, a táblák, a tartományok közötti és otthoni csomagok.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-158">This extension includes following extensions: Notifications, Authentication, Entity, Tables, Cross-domain, and Home packages.</span></span> <span data-ttu-id="1cdf3-159">Ez a csomag hello Mobile Apps gyorsindítási hello Azure-portálon elérhető használják.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-159">This package is used by hello Mobile Apps Quickstart available on hello Azure portal.</span></span>
* <span data-ttu-id="1cdf3-160">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) hello alapértelmezett megvalósítja *a mobilalkalmazás megfelelően működik, és lap* hello webhely gyökér.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-160">[Microsoft.Azure.Mobile.Server.Home](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Home/) Implements hello default *this mobile app is up and running page* for hello web site root.</span></span> <span data-ttu-id="1cdf3-161">Meghívásával toohello konfiguráció hozzáadása a **AddMobileAppHomeController** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-161">Add toohello configuration by calling the **AddMobileAppHomeController** extension method.</span></span>
* <span data-ttu-id="1cdf3-162">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) adatok és a készletek felfelé hello adatok csővezeték való munkához osztályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-162">[Microsoft.Azure.Mobile.Server.Tables](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Tables/) includes classes for working with data and sets-up hello data pipeline.</span></span> <span data-ttu-id="1cdf3-163">Toohello konfigurációs hozzá hívó hello **AddTables** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-163">Add toohello configuration by calling hello **AddTables** extension method.</span></span>
* <span data-ttu-id="1cdf3-164">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) lehetővé teszi, hogy hello Entity Framework tooaccess hello SQL-adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-164">[Microsoft.Azure.Mobile.Server.Entity](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Entity/) Enables hello Entity Framework tooaccess data in hello SQL Database.</span></span> <span data-ttu-id="1cdf3-165">Toohello konfigurációs hozzá hívó hello **AddTablesWithEntityFramework** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-165">Add toohello configuration by calling hello **AddTablesWithEntityFramework** extension method.</span></span>
* <span data-ttu-id="1cdf3-166">[Microsoft.Azure.Mobile.Server.Authentication] lehetővé teszi, hogy hitelesítést és a készletek felfelé hello OWIN köztes használt toovalidate jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-166">[Microsoft.Azure.Mobile.Server.Authentication] Enables authentication and sets-up hello OWIN middleware used toovalidate tokens.</span></span> <span data-ttu-id="1cdf3-167">Toohello konfigurációs hozzá hívó hello **AddAppServiceAuthentication** és **IAppBuilder**. **UseAppServiceAuthentication** kiterjesztésmetódusok.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-167">Add toohello configuration by calling hello **AddAppServiceAuthentication** and **IAppBuilder**.**UseAppServiceAuthentication** extension methods.</span></span>
* <span data-ttu-id="1cdf3-168">[Microsoft.Azure.Mobile.Server.Notifications] lehetővé teszi a leküldéses értesítések és egy leküldéses regisztrációs végpontot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-168">[Microsoft.Azure.Mobile.Server.Notifications] Enables push notifications and defines a push registration endpoint.</span></span> <span data-ttu-id="1cdf3-169">Toohello konfigurációs hozzá hívó hello **AddPushNotifications** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-169">Add toohello configuration by calling hello **AddPushNotifications** extension method.</span></span>
* <span data-ttu-id="1cdf3-170">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) hoz létre, amely adatok toolegacy szolgál a Mobile Apps a webböngésző vezérlő.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-170">[Microsoft.Azure.Mobile.Server.CrossDomain](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.CrossDomain/) Creates a controller that serves data toolegacy web browsers from your Mobile App.</span></span> <span data-ttu-id="1cdf3-171">Meghívásával toohello konfiguráció hozzáadása a **MapLegacyCrossDomainController** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-171">Add toohello configuration by calling the **MapLegacyCrossDomainController** extension method.</span></span>
* <span data-ttu-id="1cdf3-172">[Microsoft.Azure.Mobile.Server.Login] egyéni hitelesítési forgatókönyvek során használt statikus metódus hello AppServiceLoginHandler.CreateToken() módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-172">[Microsoft.Azure.Mobile.Server.Login] Provides hello AppServiceLoginHandler.CreateToken() method, which is a static method used during custom authentication scenarios.</span></span>

## <span data-ttu-id="1cdf3-173"><a name="publish-server-project"></a>Hogyan: hello server projekt közzététele</span><span class="sxs-lookup"><span data-stu-id="1cdf3-173"><a name="publish-server-project"></a>How to: Publish hello server project</span></span>
<span data-ttu-id="1cdf3-174">Ez a szakasz bemutatja, hogyan toopublish a .NET-háttérrendszer-projektet a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-174">This section shows you how toopublish your .NET backend project from Visual Studio.</span></span> <span data-ttu-id="1cdf3-175">A háttérprojekt Git használatával is telepítheti, vagy bármely más hello ismertetett módszerek hello [Azure App Service üzembe helyezési dokumentációja](../app-service-web/web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-175">You can also deploy your backend project using Git or any of hello other methods covered in hello [Azure App Service deployment documentation](../app-service-web/web-sites-deploy.md).</span></span>

1. <span data-ttu-id="1cdf3-176">A Visual Studio újraépítése hello projekt toorestore NuGet-csomagok.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-176">In Visual Studio, rebuild hello project toorestore NuGet packages.</span></span>
2. <span data-ttu-id="1cdf3-177">A Megoldáskezelőben kattintson a jobb gombbal hello projektben kattintson **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-177">In Solution Explorer, right-click hello project, click **Publish**.</span></span> <span data-ttu-id="1cdf3-178">hello közzéteszi, amikor első alkalommal kell toodefine egy közzétételi profilt.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-178">hello first time you publish, you need toodefine a publishing profile.</span></span> <span data-ttu-id="1cdf3-179">Ha már rendelkezik egy definiált profil, válassza ki azt, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-179">When you already have a profile defined, you can select it and click **Publish**.</span></span>
3. <span data-ttu-id="1cdf3-180">Tooselect egy közzétételi célként, kattintson az **Microsoft Azure App Service** > **következő**, majd (ha szükséges) jelentkezzen be Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-180">If asked tooselect a publish target, click **Microsoft Azure App Service** > **Next**, then (if needed) sign-in with your Azure credentials.</span></span>
   <span data-ttu-id="1cdf3-181">A Visual Studio letölti és tárolja biztonságos helyen a közzétételi beállítások közvetlenül az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-181">Visual Studio downloads and securely stores your publish settings directly from Azure.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-1.png)
4. <span data-ttu-id="1cdf3-182">Válassza ki a **előfizetés**, jelölje be **erőforrástípus** a **nézet**, bontsa ki a **mobilalkalmazás**, és kattintson a mobil-háttéralkalmazást, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-182">Choose your **Subscription**, select **Resource Type** from **View**, expand **Mobile App**, and click your Mobile App backend, then click **OK**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-2.png)
5. <span data-ttu-id="1cdf3-183">Ellenőrizze a hello profiladatok közzététele, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-183">Verify hello publish profile information and click **Publish**.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-wizard-3.png)

    <span data-ttu-id="1cdf3-184">Ha a mobilalkalmazás háttérrendszerének közzététele sikeresen befejeződött, megjelenik a sikert jelző kezdőlapja.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-184">When your Mobile App backend has published successfully, you see a landing page indicating success.</span></span>

    ![](./media/app-service-mobile-dotnet-backend-how-to-use-server-sdk/publish-success.png)

## <span data-ttu-id="1cdf3-185"><a name="define-table-controller"></a>Útmutató: egy tábla vezérlő megadása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-185"><a name="define-table-controller"></a> How to: Define a table controller</span></span>
<span data-ttu-id="1cdf3-186">Adja meg egy tábla vezérlő tooexpose egy SQL-tábla toomobile ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-186">Define a Table Controller tooexpose a SQL table toomobile clients.</span></span>  <span data-ttu-id="1cdf3-187">Egy tábla Controller konfigurálása három lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-187">Configuring a Table Controller requires three steps:</span></span>

1. <span data-ttu-id="1cdf3-188">Hozzon létre egy adatok átvitele objektum (DTO) osztályt.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-188">Create a Data Transfer Object (DTO) class.</span></span>
2. <span data-ttu-id="1cdf3-189">Adja meg egy táblahivatkozás hello Mobile DbContext osztályt.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-189">Configure a table reference in hello Mobile DbContext class.</span></span>
3. <span data-ttu-id="1cdf3-190">Hozzon létre egy tábla tartományvezérlőre.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-190">Create a table controller.</span></span>

<span data-ttu-id="1cdf3-191">Egy adatok átvitele objektum (DTO) mező egy egyszerű C#-objektum, amely örökli `EntityData`.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-191">A Data Transfer Object (DTO) is a plain C# object that inherits from `EntityData`.</span></span>  <span data-ttu-id="1cdf3-192">Példa:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-192">For example:</span></span>

    public class TodoItem : EntityData
    {
        public string Text { get; set; }
        public bool Complete {get; set;}
    }

<span data-ttu-id="1cdf3-193">hello DTO használt toodefine hello tábla hello SQL-adatbázis belül.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-193">hello DTO is used toodefine hello table within hello SQL database.</span></span>  <span data-ttu-id="1cdf3-194">toocreate hello bejegyzés adatbázisra, és adja hozzá a `DbSet<>` hello használata DbContext tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-194">toocreate hello database entry, add a `DbSet<>` property to hello DbContext you are using.</span></span>  <span data-ttu-id="1cdf3-195">A hello alapértelmezett projekt sablon az Azure Mobile Apps, hello DbContext nevezik `Models\MobileServiceContext.cs`:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-195">In hello default project template for Azure Mobile Apps, hello DbContext is called `Models\MobileServiceContext.cs`:</span></span>

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

<span data-ttu-id="1cdf3-196">Ha hello Azure SDK telepítve van, most létrehozhat egy sablont table vezérlő az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-196">If you have hello Azure SDK installed, you can now create a template table controller as follows:</span></span>

1. <span data-ttu-id="1cdf3-197">Kattintson a jobb gombbal a hello, tartományvezérlői mappa, és válassza ki **Hozzáadás** > **vezérlő...** .</span><span class="sxs-lookup"><span data-stu-id="1cdf3-197">Right-click on hello Controllers folder and select **Add** > **Controller...**.</span></span>
2. <span data-ttu-id="1cdf3-198">Jelölje be hello **Azure Mobile Apps Table vezérlő** lehetőséget, majd kattintson az **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-198">Select hello **Azure Mobile Apps Table Controller** option, then click **Add**.</span></span>
3. <span data-ttu-id="1cdf3-199">A hello **adja hozzá a tartományvezérlő** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-199">In hello **Add Controller** dialog:</span></span>
   * <span data-ttu-id="1cdf3-200">A hello **Model class** legördülő menüben válassza ki az új DTO.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-200">In hello **Model class** dropdown, select your new DTO.</span></span>
   * <span data-ttu-id="1cdf3-201">A hello **DbContext** legördülő menüben válassza hello Mobile Service DbContext osztályt.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-201">In hello **DbContext** dropdown, select hello Mobile Service DbContext class.</span></span>
   * <span data-ttu-id="1cdf3-202">hello vezérlőnév akkor jön létre.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-202">hello Controller name is created for you.</span></span>
4. <span data-ttu-id="1cdf3-203">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-203">Click **Add**.</span></span>

<span data-ttu-id="1cdf3-204">hello gyors üzembe helyezés kiszolgálóprojektet egy egyszerű példa tartalmaz **TodoItemController**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-204">hello quickstart server project contains an example for a simple **TodoItemController**.</span></span>

### <span data-ttu-id="1cdf3-205"><a name="adjust-pagesize"></a>Hogyan: hello tábla a lapozófájl méretének módosítása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-205"><a name="adjust-pagesize"></a>How to: Adjust hello table paging size</span></span>
<span data-ttu-id="1cdf3-206">Alapértelmezés szerint az Azure Mobile Apps kérelmenként 50 bejegyzést ad vissza.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-206">By default, Azure Mobile Apps returns 50 records per request.</span></span>  <span data-ttu-id="1cdf3-207">Lapozófájl biztosítja, hogy hello az ügyfél nem köti a felhasználói felület és a szál nem hello kiszolgáló túl sokáig a jó felhasználói élményt biztosítva.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-207">Paging ensures that hello client does not tie up their UI thread nor hello server for too long, ensuring a good user experience.</span></span> <span data-ttu-id="1cdf3-208">toochange hello táblázat a lapozófájl méretét, növelje hello kiszolgálóoldali "engedélyezett lekérdezés mérete", és hello ügyféloldali oldal méretét hello kiszolgálóoldali "engedélyezett a lekérdezés mérete" módosul hello segítségével `EnableQuery` attribútum:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-208">toochange hello table paging size, increase hello server side "allowed query size" and hello client-side page size hello server side "allowed query size" is adjusted using hello `EnableQuery` attribute:</span></span>

    [EnableQuery(PageSize = 500)]

<span data-ttu-id="1cdf3-209">Helyezze a pageSize értékének hello hello azonos vagy nagyobb hello hello ügyfél által kért.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-209">Ensure hello PageSize is hello same or larger than hello size requested by hello client.</span></span>  <span data-ttu-id="1cdf3-210">Hello ügyfél Oldalméret módosítása a részletekért tekintse meg a toohello adott ügyfél útmutató dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-210">Refer toohello specific client HOWTO documentation for details on changing hello client page size.</span></span>

## <a name="how-to-define-a-custom-api-controller"></a><span data-ttu-id="1cdf3-211">Hogyan: Adja meg egy egyéni API-vezérlő</span><span class="sxs-lookup"><span data-stu-id="1cdf3-211">How to: Define a custom API controller</span></span>
<span data-ttu-id="1cdf3-212">hello egyéni API-vezérlőben hello legalapvetőbb funkció tooyour Mobile Apps-háttéralkalmazás jelentkezik, mintha a végpont biztosítja.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-212">hello custom API controller provides hello most basic functionality tooyour Mobile App backend by exposing an endpoint.</span></span> <span data-ttu-id="1cdf3-213">A mobileszköz-specifikus API-vezérlőben hello [MobileAppController] attribútum használatával regisztrálhatja.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-213">You can register a mobile-specific API controller using hello [MobileAppController] attribute.</span></span> <span data-ttu-id="1cdf3-214">Hello `MobileAppController` attribútum hello útvonal regisztrálja, hello Mobile Apps JSON szerializáló beállítja és bekapcsolja a [ügyfél verzióellenőrzés](app-service-mobile-client-and-server-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-214">hello `MobileAppController` attribute registers hello route, sets up hello Mobile Apps JSON serializer, and turns on [client version checking](app-service-mobile-client-and-server-versioning.md).</span></span>

1. <span data-ttu-id="1cdf3-215">A Visual Studióban, kattintson a jobb gombbal hello tartományvezérlők mappát, majd kattintson a **Hozzáadás** > **vezérlő**, jelölje be **Web API 2-es vezérlőhöz&mdash;üres** és Kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-215">In Visual Studio, right-click hello Controllers folder, then click **Add** > **Controller**, select **Web API 2 Controller&mdash;Empty** and click **Add**.</span></span>
2. <span data-ttu-id="1cdf3-216">Adjon meg egy **tartományvezérlő neve**, például `CustomController`, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-216">Supply a **Controller name**, such as `CustomController`, and click **Add**.</span></span>
3. <span data-ttu-id="1cdf3-217">Hello új tartományvezérlő osztály fájlban adja hozzá a hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-217">In hello new controller class file, add hello following using statement:</span></span>

        using Microsoft.Azure.Mobile.Server.Config;
4. <span data-ttu-id="1cdf3-218">Hello alkalmazása **[MobileAppController]** toohello API vezérlő osztálydefiníció, mint például a következő hello attribútum:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-218">Apply hello **[MobileAppController]** attribute toohello API controller class definition, as in hello following example:</span></span>

        [MobileAppController]
        public class CustomController : ApiController
        {
              //...
        }
5. <span data-ttu-id="1cdf3-219">App_Start/Startup.MobileApp.cs fájlban adja hozzá a hívás toohello **MapApiControllers** kiterjesztésmetódus, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-219">In App_Start/Startup.MobileApp.cs file, add a call toohello **MapApiControllers** extension method, as in hello following example:</span></span>

        new MobileAppConfiguration()
            .MapApiControllers()
            .ApplyTo(config);

<span data-ttu-id="1cdf3-220">Is használhatja a hello `UseDefaultConfiguration()` kiterjesztésmetódus helyett `MapApiControllers()`.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-220">You can also use hello `UseDefaultConfiguration()` extension method instead of `MapApiControllers()`.</span></span> <span data-ttu-id="1cdf3-221">Minden futtassa, amelynek nincs **MobileAppControllerAttribute** alkalmazza az ügyfelek által továbbra is elérhető, de előfordulhat, hogy nem megfelelően védelmekor ügyfelek bármely Mobile Apps-ügyfél SDK használatával.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-221">Any controller that does not have **MobileAppControllerAttribute** applied can still be accessed by clients, but it may not be correctly consumed by clients using any Mobile App client SDK.</span></span>

## <a name="how-to-work-with-authentication"></a><span data-ttu-id="1cdf3-222">Útmutató: hitelesítés</span><span class="sxs-lookup"><span data-stu-id="1cdf3-222">How to: Work with authentication</span></span>
<span data-ttu-id="1cdf3-223">Az Azure Mobile Apps használja az App Service hitelesítés / engedélyezés toosecure a mobil-háttéralkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-223">Azure Mobile Apps uses App Service Authentication / Authorization toosecure your mobile backend.</span></span>  <span data-ttu-id="1cdf3-224">Ez a szakasz bemutatja, hogyan tooperform hello hitelesítési modulhoz kapcsolódó feladatokat a .NET háttérrendszer kiszolgálóprojektet a következő:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-224">This section shows you how tooperform hello following authentication-related tasks in your .NET backend server project:</span></span>

* [<span data-ttu-id="1cdf3-225">Útmutató: hitelesítés tooa projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-225">How to: Add authentication tooa server project</span></span>](#add-auth)
* [<span data-ttu-id="1cdf3-226">Útmutató: az alkalmazás egyéni hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="1cdf3-226">How to: Use custom authentication for your application</span></span>](#custom-auth)
* [<span data-ttu-id="1cdf3-227">Hogyan: lekérése hitelesített felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="1cdf3-227">How to: Retrieve authenticated user information</span></span>](#user-info)
* [<span data-ttu-id="1cdf3-228">Útmutató: a jogosult felhasználók adatokhoz való hozzáférést</span><span class="sxs-lookup"><span data-stu-id="1cdf3-228">How to: Restrict data access for authorized users</span></span>](#authorize)

### <span data-ttu-id="1cdf3-229"><a name="add-auth"></a>Útmutató: hitelesítés tooa projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-229"><a name="add-auth"></a>How to: Add authentication tooa server project</span></span>
<span data-ttu-id="1cdf3-230">Hello kiterjesztésével is hozzáadhat a hitelesítési tooyour kiszolgálóprojektet **MobileAppConfiguration** objektum és OWIN köztes konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-230">You can add authentication tooyour server project by extending hello **MobileAppConfiguration** object and configuring OWIN middleware.</span></span> <span data-ttu-id="1cdf3-231">Hello telepítésekor [Microsoft.Azure.Mobile.Server.Quickstart] csomag- és hívás hello **UseDefaultConfiguration** kiterjesztésmetódus, kihagyhatja a toostep 3.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-231">When you install hello [Microsoft.Azure.Mobile.Server.Quickstart] package and call hello **UseDefaultConfiguration** extension method, you can skip toostep 3.</span></span>

1. <span data-ttu-id="1cdf3-232">A Visual Studio telepítése hello [Microsoft.Azure.Mobile.Server.Authentication] csomag.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-232">In Visual Studio, install hello [Microsoft.Azure.Mobile.Server.Authentication] package.</span></span>
2. <span data-ttu-id="1cdf3-233">Hello Startup.cs projekt fájlban adja hozzá a következő kódsort hello hello elején hello **konfigurációs** módszert:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-233">In hello Startup.cs project file, add hello following line of code at hello beginning of hello **Configuration** method:</span></span>

        app.UseAppServiceAuthentication(config);

    <span data-ttu-id="1cdf3-234">Az OWIN köztes összetevő hello tartozó App Service-átjáró által kiállított jogkivonatokat ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-234">This OWIN middleware component validates tokens issued by hello associated App Service gateway.</span></span>
3. <span data-ttu-id="1cdf3-235">Adja hozzá a hello `[Authorize]` attribútum tooany vezérlő vagy metódust, amelyhez hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-235">Add hello `[Authorize]` attribute tooany controller or method that requires authentication.</span></span>

<span data-ttu-id="1cdf3-236">arról, hogyan toolearn tooauthenticate ügyfelek tooyour Mobile Apps-háttéralkalmazás, lásd: [Add authentication tooyour alkalmazás](app-service-mobile-ios-get-started-users.md).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-236">toolearn about how tooauthenticate clients tooyour Mobile Apps backend, see [Add authentication tooyour app](app-service-mobile-ios-get-started-users.md).</span></span>

### <span data-ttu-id="1cdf3-237"><a name="custom-auth"></a>Útmutató: az alkalmazás egyéni hitelesítés használata</span><span class="sxs-lookup"><span data-stu-id="1cdf3-237"><a name="custom-auth"></a>How to: Use custom authentication for your application</span></span>
<span data-ttu-id="1cdf3-238">Ha nem kíván toouse hello App Service hitelesítési/engedélyezési szolgáltatók egyikét, a saját bejelentkezési rendszer valósíthat meg.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-238">If you do not wish toouse one of hello App Service Authentication/Authorization providers, you can implement your own login system.</span></span> <span data-ttu-id="1cdf3-239">Telepítse a hello [Microsoft.Azure.Mobile.Server.Login] csomag tooassist a hitelesítési jogkivonatok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-239">Install hello [Microsoft.Azure.Mobile.Server.Login] package tooassist with authentication token generation.</span></span>  <span data-ttu-id="1cdf3-240">Adja meg a saját kódot a felhasználó hitelesítő adatainak ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-240">Provide your own code for validating user credentials.</span></span> <span data-ttu-id="1cdf3-241">Például ellenőrizze sózott és kivonatolt jelszavakat adatbázisban ellen.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-241">For example, you might check against salted and hashed passwords in a database.</span></span> <span data-ttu-id="1cdf3-242">Hello az alábbi példában a hello `isValidAssertion()` (határozni) metódus felelős az ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-242">In hello example below, hello `isValidAssertion()` method (defined elsewhere) is responsible for these checks.</span></span>

<span data-ttu-id="1cdf3-243">egyéni hitelesítési hello tesz elérhetővé, és adjon meg egy ApiController létrehozása `register` és `login` műveletek.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-243">hello custom authentication is exposed by creating an ApiController and exposing `register` and `login` actions.</span></span> <span data-ttu-id="1cdf3-244">hello ügyfélnek használnia kell, egy egyéni felhasználói felületi toocollect hello információ hello felhasználótól.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-244">hello client should use a custom UI toocollect hello information from hello user.</span></span>  <span data-ttu-id="1cdf3-245">hello információt nem egy normál HTTP POST az elküldött toohello API hívása.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-245">hello information is then submitted toohello API with a standard HTTP POST call.</span></span> <span data-ttu-id="1cdf3-246">Miután hello kiszolgáló hello helyességi feltétel érvényesíti, jogkivonat használatával hello kiadott `AppServiceLoginHandler.CreateToken()` metódust.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-246">Once hello server validates hello assertion, a token is issued using hello `AppServiceLoginHandler.CreateToken()` method.</span></span>  <span data-ttu-id="1cdf3-247">hello ApiController **nem kell** hello használata `[MobileAppController]` attribútum.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-247">hello ApiController **should not** use hello `[MobileAppController]` attribute.</span></span>

<span data-ttu-id="1cdf3-248">Példa `login` művelet:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-248">An example `login` action:</span></span>

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

<span data-ttu-id="1cdf3-249">A fenti példa hello LoginResult és LoginResultUser a szükséges tulajdonságok feltáró szerializálható objektumok.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-249">In hello preceding example, LoginResult and LoginResultUser are serializable objects exposing required properties.</span></span> <span data-ttu-id="1cdf3-250">hello ügyfél bejelentkezési válaszok toobe adja vissza a JSON-objektumok hello űrlap vár:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-250">hello client expects login responses toobe returned as JSON objects of hello form:</span></span>

        {
            "authenticationToken": "<token>",
            "user": {
                "userId": "<userId>"
            }
        }

<span data-ttu-id="1cdf3-251">Hello `AppServiceLoginHandler.CreateToken()` metódust tartalmaz egy *célközönség* és egy *kibocsátó* paraméter.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-251">hello `AppServiceLoginHandler.CreateToken()` method includes an *audience* and an *issuer* parameter.</span></span> <span data-ttu-id="1cdf3-252">Mindkét paraméter beállított toohello URL-CÍMÉT az alkalmazás gyökérkönyvtára hello HTTPS protokollt használ.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-252">Both of these parameters are set toohello URL of your application root, using hello HTTPS scheme.</span></span> <span data-ttu-id="1cdf3-253">Hasonló módon állítsa be *secretKey* toobe hello értéket az alkalmazás csomagazonosítóját aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-253">Similarly you should set *secretKey* toobe hello value of your application's signing key.</span></span> <span data-ttu-id="1cdf3-254">Ne ossza el a hello aláírókulcsának ügyfélprogram használt toomint kulcsok kell és felhasználók megszemélyesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-254">Do not distribute hello signing key in a client as it can be used toomint keys and impersonate users.</span></span> <span data-ttu-id="1cdf3-255">Ezt úgy szerezheti be aláírókulcsának közben hello Vezérlőpultjának az App Service-ben üzemeltetett hello *webhely\_AUTH\_aláírás\_kulcs* környezeti változó.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-255">You can obtain hello signing key while hosted in App Service by referencing hello *WEBSITE\_AUTH\_SIGNING\_KEY* environment variable.</span></span> <span data-ttu-id="1cdf3-256">Szükség esetén egy helyi hibakeresési környezetben – kövesse a hello hello utasításait [hitelesítéssel helyi hibakeresés](#local-debug) tooretrieve hello kulcs szakaszt, és tárolja, az alkalmazás-beállítás.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-256">If needed in a local debugging context, follow hello instructions in hello [Local debugging with authentication](#local-debug) section tooretrieve hello key and store it as an application setting.</span></span>

<span data-ttu-id="1cdf3-257">hello kiállított jogkivonat más jogcímeket és lejárati dátummal is.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-257">hello issued token may also include other claims and an expiry date.</span></span>  <span data-ttu-id="1cdf3-258">A minimális követelmény, hello kiállított jogkivonat tartalmaznia kell a tulajdonos (**sub**) jogcímek.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-258">Minimally, hello issued token must include a subject (**sub**) claim.</span></span>

<span data-ttu-id="1cdf3-259">Hello szabványos ügyfél támogathatja `loginAsync()` metódus által túl van terhelve hello hitelesítési útvonal.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-259">You can support hello standard client `loginAsync()` method by overloading hello authentication route.</span></span>  <span data-ttu-id="1cdf3-260">Ha a hello ügyfél `client.loginAsync('custom');` a toolog, az útvonal lehet `/.auth/login/custom`.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-260">If hello client calls `client.loginAsync('custom');` toolog in, your route must be `/.auth/login/custom`.</span></span>  <span data-ttu-id="1cdf3-261">Hello útvonal a hello egyéni vezérlő használatával állíthatja be `MapHttpRoute()`:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-261">You can set hello route for hello custom authentication controller using `MapHttpRoute()`:</span></span>

    config.Routes.MapHttpRoute("custom", ".auth/login/custom", new { controller = "CustomAuth" });

> [!TIP]
> <span data-ttu-id="1cdf3-262">Hello segítségével `loginAsync()` megközelítés biztosítja, hogy hello hitelesítésére szolgáló jogkivonat csatolt tooevery későbbi hívás toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-262">Using hello `loginAsync()` approach ensures that hello authentication token is attached tooevery subsequent call toohello service.</span></span>
>
>

### <span data-ttu-id="1cdf3-263"><a name="user-info"></a>Hogyan: lekérése hitelesített felhasználói adatok</span><span class="sxs-lookup"><span data-stu-id="1cdf3-263"><a name="user-info"></a>How to: Retrieve authenticated user information</span></span>
<span data-ttu-id="1cdf3-264">Amikor a felhasználó hitelesítése az App Service, hozzárendelt felhasználói Azonosítót és egyéb információkat a .NET-háttéralkalmazás kódjának hello végezheti el.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-264">When a user is authenticated by App Service, you can access hello assigned user ID and other information in your .NET backend code.</span></span> <span data-ttu-id="1cdf3-265">hello felhasználói adatok a hitelesítési döntések meghozatala során a hello háttérbeli használhatók.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-265">hello user information can be used for making authorization decisions in hello backend.</span></span> <span data-ttu-id="1cdf3-266">hello alábbira szerzi be a kérelemhez társított hello Felhasználóazonosító:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-266">hello following code obtains hello user ID associated with a request:</span></span>

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

<span data-ttu-id="1cdf3-267">hello SID hello szolgáltatóhoz tartozó felhasználói azonosító származik, és egy adott felhasználó és a bejelentkezés-szolgáltató statikus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-267">hello SID is derived from hello provider-specific user ID and is static for a given user and login provider.</span></span>  <span data-ttu-id="1cdf3-268">hello SID értéke érvénytelen a hitelesítési tokenek null.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-268">hello SID is null for invalid authentication tokens.</span></span>

<span data-ttu-id="1cdf3-269">App Service lehetővé teszi a bejelentkezési szolgáltató által adott jogcímek igénylésére.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-269">App Service also lets you request specific claims from your login provider.</span></span> <span data-ttu-id="1cdf3-270">Minden egyes identitásszolgáltató az identitásszolgáltató SDK használatával további információk megadására.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-270">Each identity provider can provide more information using the identity provider SDK.</span></span>  <span data-ttu-id="1cdf3-271">Például ismerősök információt hello Facebook Graph API-t is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-271">For example, you can use hello Facebook Graph API for friends information.</span></span>  <span data-ttu-id="1cdf3-272">Hello szolgáltató panel az Azure-portálon hello igényelt jogcímeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-272">You can specify claims that are requested in hello provider blade in hello Azure portal.</span></span> <span data-ttu-id="1cdf3-273">Egyes jogcímek hello identitásszolgáltatóval további beállításokra van szükség.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-273">Some claims require additional configuration with hello identity provider.</span></span>

<span data-ttu-id="1cdf3-274">hello következő hívások hello kódot **GetAppServiceIdentityAsync** bővítmény metódus tooget hello bejelentkezési hitelesítő adatok, többek között hello hozzáférési jogkivonat szükséges toomake kérelmek hello Facebook Graph API szemben:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-274">hello following code calls hello **GetAppServiceIdentityAsync** extension method tooget hello login credentials, which include hello access token needed toomake requests against hello Facebook Graph API:</span></span>

    // Get hello credentials for hello logged-in user.
    var credentials =
        await this.User
        .GetAppServiceIdentityAsync<FacebookCredentials>(this.Request);

    if (credentials.Provider == "Facebook")
    {
        // Create a query string with hello Facebook access token.
        var fbRequestUrl = "https://graph.facebook.com/me/feed?access_token="
            + credentials.AccessToken;

        // Create an HttpClient request.
        var client = new System.Net.Http.HttpClient();

        // Request hello current user info from Facebook.
        var resp = await client.GetAsync(fbRequestUrl);
        resp.EnsureSuccessStatusCode();

        // Do something here with hello Facebook user information.
        var fbInfo = await resp.Content.ReadAsStringAsync();
    }

<span data-ttu-id="1cdf3-275">Adja hozzá egy utasítást `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** kiterjesztésmetódus.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-275">Add a using statement for `System.Security.Principal` tooprovide hello **GetAppServiceIdentityAsync** extension method.</span></span>

### <span data-ttu-id="1cdf3-276"><a name="authorize"></a>Útmutató: a jogosult felhasználók adatokhoz való hozzáférést</span><span class="sxs-lookup"><span data-stu-id="1cdf3-276"><a name="authorize"></a>How to: Restrict data access for authorized users</span></span>
<span data-ttu-id="1cdf3-277">Hello előző szakaszban azt lehet bemutatta, hogyan tooretrieve hello egy hitelesített felhasználó felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-277">In hello previous section, we showed how tooretrieve hello user ID of an authenticated user.</span></span> <span data-ttu-id="1cdf3-278">Korlátozhatja a hozzáférést toodata és más erőforrások, ez az érték alapján.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-278">You can restrict access toodata and other resources based on this value.</span></span> <span data-ttu-id="1cdf3-279">Például egy userId oszlop tootables hozzáadása, és a szűrés hello lekérdezési eredmények hello felhasználói azonosító nem egy adott vissza az adatokat csak tooauthorized felhasználók egyszerűen toolimit.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-279">For example, adding a userId column tootables and filtering hello query results by hello user ID is a simple way toolimit returned data only tooauthorized users.</span></span> <span data-ttu-id="1cdf3-280">hello alábbira sorát adja vissza adatok csak akkor, ha hello biztonsági azonosítója megegyezik-e a hello UserId oszlopban hello TodoItem tábla:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-280">hello following code returns data rows only when hello SID matches the value in hello UserId column on hello TodoItem table:</span></span>

    // Get hello SID of hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;

    // Only return data rows that belong toohello current user.
    return Query().Where(t => t.UserId == sid);

<span data-ttu-id="1cdf3-281">Hello `Query()` metódus értéket ad vissza egy `IQueryable` , amely a LINQ-toohandle szűréssel kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-281">hello `Query()` method returns an `IQueryable` that can be manipulated by LINQ toohandle filtering.</span></span>

## <a name="how-to-add-push-notifications-tooa-server-project"></a><span data-ttu-id="1cdf3-282">Hogyan: leküldéses értesítések tooa server projekt hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-282">How to: Add push notifications tooa server project</span></span>
<span data-ttu-id="1cdf3-283">Adja hozzá a leküldéses értesítések tooyour kiszolgálóprojektet hello kiterjesztésével **MobileAppConfiguration** objektum és a Notification Hubs ügyfelet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-283">Add push notifications tooyour server project by extending hello **MobileAppConfiguration** object and creating a Notification Hubs client.</span></span>

1. <span data-ttu-id="1cdf3-284">A Visual Studióban, kattintson a jobb gombbal a projekt hello, és kattintson **NuGet-csomagok kezelése**, keressen `Microsoft.Azure.Mobile.Server.Notifications`, majd kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-284">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for `Microsoft.Azure.Mobile.Server.Notifications`, then click **Install**.</span></span>
2. <span data-ttu-id="1cdf3-285">Ismételje meg ezt a lépést tooinstall hello `Microsoft.Azure.NotificationHubs` csomagot, amely tartalmazza a Notification Hubs ügyfélkódtár hello.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-285">Repeat this step tooinstall hello `Microsoft.Azure.NotificationHubs` package, which includes hello Notification Hubs client library.</span></span>
3. <span data-ttu-id="1cdf3-286">A App_Start/Startup.MobileApp.cs és adja hozzá a hívás toohello **AddPushNotifications()** kiterjesztésmetódus inicializálása közben:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-286">In App_Start/Startup.MobileApp.cs, and add a call toohello **AddPushNotifications()** extension method during initialization:</span></span>

        new MobileAppConfiguration()
            // other features...
            .AddPushNotifications()
            .ApplyTo(config);
4. <span data-ttu-id="1cdf3-287">Adja hozzá a következő kódot, amely létrehozza a Notification Hubs-ügyfél hello:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-287">Add hello following code that creates a Notification Hubs client:</span></span>

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            config.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create a new Notification Hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

<span data-ttu-id="1cdf3-288">Most már használhatja a hello Notification Hubs toosend leküldéses értesítések tooregistered ügyféleszközökön.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-288">You can now use hello Notification Hubs client toosend push notifications tooregistered devices.</span></span> <span data-ttu-id="1cdf3-289">További információkért lásd: [Hozzáadás leküldéses értesítések tooyour app](app-service-mobile-ios-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-289">For more information, see [Add push notifications tooyour app](app-service-mobile-ios-get-started-push.md).</span></span> <span data-ttu-id="1cdf3-290">toolearn Notification hubs használatával kapcsolatos további információkért lásd: [Notification Hubs – áttekintés](../notification-hubs/notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-290">toolearn more about Notification Hubs, see [Notification Hubs Overview](../notification-hubs/notification-hubs-push-notification-overview.md).</span></span>

## <span data-ttu-id="1cdf3-291"><a name="tags"></a>Hogyan: engedélyezése megcélzott ügyfélleküldéses címkék használatával</span><span class="sxs-lookup"><span data-stu-id="1cdf3-291"><a name="tags"></a>How to: Enable targeted push using Tags</span></span>
<span data-ttu-id="1cdf3-292">A Notification Hubs lehetővé teszi a címkék segítségével toospecific regisztrációk célzott értesítéseket küldeni.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-292">Notification Hubs lets you send targeted notifications toospecific registrations by using tags.</span></span> <span data-ttu-id="1cdf3-293">Több címkék jönnek létre automatikusan:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-293">Several tags are created automatically:</span></span>

* <span data-ttu-id="1cdf3-294">Telepítési Azonosítót hello azonosítja az egy adott eszközhöz.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-294">hello Installation ID identifies a specific device.</span></span>
* <span data-ttu-id="1cdf3-295">hello felhasználói azonosító alapján hitelesített hello SID azonosítja egy megadott felhasználó.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-295">hello User Id based on hello authenticated SID identifies a specific user.</span></span>

<span data-ttu-id="1cdf3-296">telepítési azonosító hello elérhető hello **végrehajtott** hello tulajdonságának **MobileServiceClient**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-296">hello installation ID can be accessed from hello **installationId** property on hello **MobileServiceClient**.</span></span>  <span data-ttu-id="1cdf3-297">hello következő példa bemutatja, hogyan egy telepítési azonosító tooadd használatára egy címke tooa adott telepítési a Notification hubs használatával:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-297">hello following example shows how to use an installation ID tooadd a tag tooa specific installation in Notification Hubs:</span></span>

    hub.PatchInstallation("my-installation-id", new[]
    {
        new PartialUpdateOperation
        {
            Operation = UpdateOperationType.Add,
            Path = "/tags",
            Value = "{my-tag}"
        }
    });

<span data-ttu-id="1cdf3-298">Hello ügyfél leküldéses értesítési regisztráció során megadott címkéket figyelmen kívül hagyják hello háttér hello telepítési létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-298">Any tags supplied by hello client during push notification registration are ignored by hello backend when creating hello installation.</span></span> <span data-ttu-id="1cdf3-299">egy ügyfél tooadd tooenable toohello telepítési címkéket, létre kell hoznia egy egyéni API-t, amely mintát megelőző hello segítségével címkék.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-299">tooenable a client tooadd tags toohello installation, you must create a custom API that adds tags using hello preceding pattern.</span></span>

<span data-ttu-id="1cdf3-300">Lásd: [ügyfél által hozzáadott leküldéses értesítések címkék] [ 5] hello App Service Mobile Apps befejezett gyors üzembe helyezési minta példát.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-300">See [Client-added push notification tags][5] in hello App Service Mobile Apps completed quickstart sample for an example.</span></span>

## <span data-ttu-id="1cdf3-301"><a name="push-user"></a>Hogyan: küldés leküldéses értesítések tooan hitelesített felhasználó</span><span class="sxs-lookup"><span data-stu-id="1cdf3-301"><a name="push-user"></a>How to: Send push notifications tooan authenticated user</span></span>
<span data-ttu-id="1cdf3-302">Amikor a hitelesített felhasználó regisztrálja a leküldéses értesítések, a felhasználói azonosító címke automatikusan kerül toohello regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-302">When an authenticated user registers for push notifications, a user ID tag is automatically added toohello registration.</span></span> <span data-ttu-id="1cdf3-303">Ezt a címkét használatával küldhet leküldéses értesítések tooall eszközök adott személy által regisztrált.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-303">By using this tag, you can send push notifications tooall devices registered by that person.</span></span> <span data-ttu-id="1cdf3-304">hello alábbira lekérdezi a kérelmező felhasználó biztonsági azonosítója hello és elküldi a sablon leküldéses értesítési tooevery eszközregisztráció személyre:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-304">hello following code gets hello SID of user making the request and sends a template push notification tooevery device registration for that person:</span></span>

    // Get hello current user SID and create a tag for hello current user.
    var claimsPrincipal = this.User as ClaimsPrincipal;
    string sid = claimsPrincipal.FindFirst(ClaimTypes.NameIdentifier).Value;
    string userTag = "_UserId:" + sid;

    // Build a dictionary for hello template with hello item message text.
    var notification = new Dictionary<string, string> { { "message", item.Text } };

    // Send a template notification toohello user ID.
    await hub.SendTemplateNotificationAsync(notification, userTag);

<span data-ttu-id="1cdf3-305">Amikor regisztrál egy hitelesített ügyfél leküldéses értesítésekhez, győződjön meg arról, hogy a hitelesítési teljes regisztrációs megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-305">When registering for push notifications from an authenticated client, make sure that authentication is complete before attempting registration.</span></span> <span data-ttu-id="1cdf3-306">További információkért lásd: [toousers leküldéses] [ 6] hello App Service Mobile Apps befejezett gyors üzembe helyezési minta .NET-háttérrendszer.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-306">For more information, see [Push toousers][6] in hello App Service Mobile Apps completed quickstart sample for .NET backend.</span></span>

## <a name="how-to-debug-and-troubleshoot-hello-net-server-sdk"></a><span data-ttu-id="1cdf3-307">Hogyan: Hibakeresés és hibaelhárítás hello .NET SDK-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="1cdf3-307">How to: Debug and troubleshoot hello .NET Server SDK</span></span>
<span data-ttu-id="1cdf3-308">Az Azure App Service számos Hibakeresés és hibaelhárítási módszerekről az ASP.NET-alkalmazások biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-308">Azure App Service provides several debugging and troubleshooting techniques for ASP.NET applications:</span></span>

* [<span data-ttu-id="1cdf3-309">Az Azure App Service figyelése</span><span class="sxs-lookup"><span data-stu-id="1cdf3-309">Monitoring an Azure App Service</span></span>](../app-service-web/web-sites-monitor.md)
* [<span data-ttu-id="1cdf3-310">Az Azure App Service diagnosztikai naplózás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1cdf3-310">Enable Diagnostic Logging in Azure App Service</span></span>](../app-service-web/web-sites-enable-diagnostic-log.md)
* [<span data-ttu-id="1cdf3-311">A Visual Studio egy Azure App Service hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="1cdf3-311">Troubleshoot an Azure App Service in Visual Studio</span></span>](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)

### <a name="logging"></a><span data-ttu-id="1cdf3-312">Naplózás</span><span class="sxs-lookup"><span data-stu-id="1cdf3-312">Logging</span></span>
<span data-ttu-id="1cdf3-313">Írhat tooApp szolgáltatás diagnosztikai naplók hello szabványos az ASP.NET nyomkövetési írás használatával.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-313">You can write tooApp Service diagnostic logs by using hello standard ASP.NET trace writing.</span></span> <span data-ttu-id="1cdf3-314">Ahhoz, hogy toohello naplókat, engedélyeznie kell a Mobile Apps-háttéralkalmazás diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-314">Before you can write toohello logs, you must enable diagnostics in your Mobile App backend.</span></span>

<span data-ttu-id="1cdf3-315">tooenable diagnostics és az írási toohello naplói:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-315">tooenable diagnostics and write toohello logs:</span></span>

1. <span data-ttu-id="1cdf3-316">Hello kövesse [hogyan tooenable diagnosztika](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-316">Follow hello steps in [How tooenable diagnostics](../app-service-web/web-sites-enable-diagnostic-log.md#enablediag).</span></span>
2. <span data-ttu-id="1cdf3-317">Adja hozzá hello következő using utasítást a kód fájlban:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-317">Add hello following using statement in your code file:</span></span>

        using System.Web.Http.Tracing;
3. <span data-ttu-id="1cdf3-318">Hozzon létre egy nyomkövetési író toowrite hello .NET háttérrendszer toohello diagnosztikai naplók, a az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-318">Create a trace writer toowrite from hello .NET backend toohello diagnostic logs, as follows:</span></span>

        ITraceWriter traceWriter = this.Configuration.Services.GetTraceWriter();
        traceWriter.Info("Hello, World");
4. <span data-ttu-id="1cdf3-319">A projekt ismételt közzététele, és elérési hello mobilalkalmazás háttér tooexecute hello kód hello naplózással.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-319">Republish your server project, and access hello Mobile App backend tooexecute hello code path with hello logging.</span></span>
5. <span data-ttu-id="1cdf3-320">Töltse le és hello naplókat, kiértékelheti a [hogyan: naplók letöltéséhez](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span><span class="sxs-lookup"><span data-stu-id="1cdf3-320">Download and evaluate hello logs, as described in [How to: Download logs](../app-service-web/web-sites-enable-diagnostic-log.md#download).</span></span>

### <span data-ttu-id="1cdf3-321"><a name="local-debug"></a>Helyi hitelesítéssel hibakeresés</span><span class="sxs-lookup"><span data-stu-id="1cdf3-321"><a name="local-debug"></a>Local debugging with authentication</span></span>
<span data-ttu-id="1cdf3-322">Az alkalmazás futtatása helyben tootest toohello felhő közzététel előtt változik.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-322">You can run your application locally tootest changes before publishing them toohello cloud.</span></span> <span data-ttu-id="1cdf3-323">Nyomja le az Azure Mobile Apps-háttérkiszolgálókon legtöbb, *F5* a Visual Studio során.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-323">For most Azure Mobile Apps backends, press *F5* while in Visual Studio.</span></span> <span data-ttu-id="1cdf3-324">Van azonban néhány további szempontok hitelesítése során.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-324">However, there are some additional considerations when using authentication.</span></span>

<span data-ttu-id="1cdf3-325">Rendelkeznie kell egy felhőalapú mobilalkalmazást az App Service hitelesítési/engedélyezési konfigurálva, és az ügyfél hello másodlagos bejelentkezési gazdagépként megadott hello felhő végpontot kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-325">You must have a cloud-based mobile app with App Service Authentication/Authorization configured, and your client must have hello cloud endpoint specified as hello alternate login host.</span></span> <span data-ttu-id="1cdf3-326">Hello lépéseit szükséges ügyfélplatform hello dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-326">See hello documentation for your client platform for hello specific steps required.</span></span>

<span data-ttu-id="1cdf3-327">Győződjön meg arról, hogy rendelkezik-e a mobil-háttéralkalmazást [Microsoft.Azure.Mobile.Server.Authentication] telepítve.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-327">Ensure that your mobile backend has [Microsoft.Azure.Mobile.Server.Authentication] installed.</span></span> <span data-ttu-id="1cdf3-328">Ezt követően az alkalmazás OWIN indítási osztályt, adja hozzá hello következő után `MobileAppConfiguration` lett alkalmazott tooyour `HttpConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-328">Then, in your application's OWIN startup class, add hello following, after `MobileAppConfiguration` has been applied tooyour `HttpConfiguration`:</span></span>

        app.UseAppServiceAuthentication(new AppServiceAuthenticationOptions()
        {
            SigningKey = ConfigurationManager.AppSettings["authSigningKey"],
            ValidAudiences = new[] { ConfigurationManager.AppSettings["authAudience"] },
            ValidIssuers = new[] { ConfigurationManager.AppSettings["authIssuer"] },
            TokenHandler = config.GetAppServiceTokenHandler()
        });

<span data-ttu-id="1cdf3-329">A fenti példa hello, konfigurálnia kell a hello *authAudience* és *authIssuer* Alkalmazásbeállítások belül a Web.config fájl tooeach kell az URL-CÍMÉT az alkalmazás gyökérkönyvtára hello HTTPS használatával a rendszer.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-329">In hello preceding example, you should configure hello *authAudience* and *authIssuer* application settings within your Web.config file tooeach be the URL of your application root, using hello HTTPS scheme.</span></span> <span data-ttu-id="1cdf3-330">Hasonló módon állítsa be *authSigningKey* toobe hello értéket az alkalmazás csomagazonosítóját aláírási kulcs.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-330">Similarly you should set *authSigningKey* toobe hello value of your application's signing key.</span></span>
<span data-ttu-id="1cdf3-331">tooobtain hello aláírási kulcs:</span><span class="sxs-lookup"><span data-stu-id="1cdf3-331">tooobtain hello signing key:</span></span>

1. <span data-ttu-id="1cdf3-332">Keresse meg a tooyour app belül hello [Azure-portálon]</span><span class="sxs-lookup"><span data-stu-id="1cdf3-332">Navigate tooyour app within hello [Azure portal]</span></span>
2. <span data-ttu-id="1cdf3-333">Kattintson a **eszközök**, **Kudu**, **Ugrás**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-333">Click **Tools**, **Kudu**, **Go**.</span></span>
3. <span data-ttu-id="1cdf3-334">Hello Kudu felügyeleti hely, kattintson a **környezet**.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-334">In hello Kudu Management site, click **Environment**.</span></span>
4. <span data-ttu-id="1cdf3-335">Keresse meg a hello értéket *webhely\_AUTH\_aláírás\_kulcs*.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-335">Find hello value for *WEBSITE\_AUTH\_SIGNING\_KEY*.</span></span>

<span data-ttu-id="1cdf3-336">Aláírási kulcs hello használata hello *authSigningKey* paraméter a helyi alkalmazások Config.  A mobil-háttéralkalmazást már felszerelt toovalidate jogkivonatokat, a helyi futtatás során, mely hello ügyfél kapja hello token hello felhőalapú végpont.</span><span class="sxs-lookup"><span data-stu-id="1cdf3-336">Use hello signing key for hello *authSigningKey* parameter in your local application config.  Your mobile backend is now equipped toovalidate tokens when running locally, which hello client obtains hello token from hello cloud-based endpoint.</span></span>

[1]: https://msdn.microsoft.com/library/azure/dn961176.aspx
[2]: https://github.com/Azure/azure-mobile-apps-net-server
[3]: app-service-mobile-ios-get-started.md
[4]: https://azure.microsoft.com/downloads/
[5]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#client-added-push-notification-tags
[6]: https://github.com/Azure-Samples/app-service-mobile-dotnet-backend-quickstart/blob/master/README.md#push-to-users
[Azure-portálon]: https://portal.azure.com
[NuGet.org]: http://www.nuget.org/
[Microsoft.Azure.Mobile.Server]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/
[Microsoft.Azure.Mobile.Server.Quickstart]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Quickstart/
[Microsoft.Azure.Mobile.Server.Authentication]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/
[Microsoft.Azure.Mobile.Server.Login]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Login/
[Microsoft.Azure.Mobile.Server.Notifications]: http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Notifications/
[MapHttpAttributeRoutes]: https://msdn.microsoft.com/library/dn479134(v=vs.118).aspx

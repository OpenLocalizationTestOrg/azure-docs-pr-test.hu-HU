---
title: "az ASP.NET MVC 5 aaaDeploy mobil webalkalmazás az Azure App Service-ben"
description: "Ez az oktatóanyag útmutatást ad, hogy hogyan toodeploy egy webes alkalmazás tooAzure használatával mobile App Service szolgáltatások ASP.NET mvc 5 webalkalmazás."
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 0752c802-8609-4956-a755-686116913645
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/12/2016
ms.author: cephalin
ms.openlocfilehash: 01119c07246c0252fd357562774a2e90b3ef77d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a><span data-ttu-id="b4d59-103">Az ASP.NET MVC 5 mobil webalkalmazás az Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="b4d59-103">Deploy an ASP.NET MVC 5 mobile web app in Azure App Service</span></span>
<span data-ttu-id="b4d59-104">Ez az oktatóanyag fog mutatja meg, hogyan toobuild egy ASP.NET MVC 5 webalkalmazás, amely mobilbarát alapjait hello és telepítheti az App Service tooAzure.</span><span class="sxs-lookup"><span data-stu-id="b4d59-104">This tutorial will teach you hello basics of how toobuild an ASP.NET MVC 5 web app that is mobile-friendly and deploy it tooAzure App Service.</span></span> <span data-ttu-id="b4d59-105">Ebben az oktatóanyagban kell [Visual Studio Express 2013 for Web alkalmazásokat] [ Visual Studio Express 2013] vagy hello Ha már rendelkezik, amely a Visual Studio professional Edition kiadását.</span><span class="sxs-lookup"><span data-stu-id="b4d59-105">For this tutorial, you need [Visual Studio Express 2013 for Web][Visual Studio Express 2013] or hello professional edition of Visual Studio if you already have that.</span></span> <span data-ttu-id="b4d59-106">Használhat [Visual Studio 2015] , de hello képernyőképek eltérőek lesznek, és hello az ASP.NET 4.x sablonok kell használnia.</span><span class="sxs-lookup"><span data-stu-id="b4d59-106">You can use [Visual Studio 2015] but hello screen shots will be different and you must use hello ASP.NET 4.x templates.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a><span data-ttu-id="b4d59-107">Mit kell összeállítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-107">What You'll Build</span></span>
<span data-ttu-id="b4d59-108">Ebben az oktatóanyagban a hello fogja hozzáadni mobil szolgáltatások toohello egyszerű konferencia-listát alkalmazás biztosított [alapszintű projekt][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="b4d59-108">For this tutorial, you'll add mobile features toohello simple conference-listing application that's provided in hello [starter project][StarterProject].</span></span> <span data-ttu-id="b4d59-109">hello alábbi képernyőfelvételen látható hello ASP.NET munkamenetek befejeződött hello alkalmazásban, az Internet Explorer 11 F12 fejlesztői eszközök hello böngésző emulátor látható módon.</span><span class="sxs-lookup"><span data-stu-id="b4d59-109">hello following screenshot shows hello ASP.NET sessions in hello completed application, as seen in hello browser emulator in Internet Explorer 11 F12 developer tools.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="b4d59-110">Használhatja az Internet Explorer 11 F12 hello Fejlesztőeszközök és hello [Fiddler eszköz] [ Fiddler] toohelp az alkalmazás hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="b4d59-110">You can use hello Internet Explorer 11 F12 developer tools and hello [Fiddler tool][Fiddler] toohelp debug your application.</span></span> 

## <a name="skills-youll-learn"></a><span data-ttu-id="b4d59-111">Megtudhatja, képességek</span><span class="sxs-lookup"><span data-stu-id="b4d59-111">Skills You'll Learn</span></span>
<span data-ttu-id="b4d59-112">Az itt ismertetett témák:</span><span class="sxs-lookup"><span data-stu-id="b4d59-112">Here's what you'll learn:</span></span>

* <span data-ttu-id="b4d59-113">Hogyan toouse Visual Studio 2013 toopublish a webes alkalmazás közvetlenül tooa webalkalmazást az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="b4d59-113">How toouse Visual Studio 2013 toopublish your web application directly tooa web app in Azure App Service.</span></span>
* <span data-ttu-id="b4d59-114">Hogyan hello ASP.NET MVC 5 sablonok használatával hello CSS rendszerindítási keretrendszer javíthatja a mobileszközök megjelenítési</span><span class="sxs-lookup"><span data-stu-id="b4d59-114">How hello ASP.NET MVC 5 templates use hello CSS Bootstrap framework to improve display on mobile devices</span></span>
* <span data-ttu-id="b4d59-115">Hogyan toocreate mobile-specifikus megtekinti tootarget adott mobilböngészők, például a hello iPhone- és Android rendszerhez</span><span class="sxs-lookup"><span data-stu-id="b4d59-115">How toocreate mobile-specific views tootarget specific mobile browsers, such as hello iPhone and Android</span></span>
* <span data-ttu-id="b4d59-116">Hogyan toocreate rugalmas nézetek (nézetek toodifferent böngészők válaszolnak az eszközön)</span><span class="sxs-lookup"><span data-stu-id="b4d59-116">How toocreate responsive views (views that respond toodifferent browsers across devices)</span></span>

## <a name="set-up-hello-development-environment"></a><span data-ttu-id="b4d59-117">Hello fejlesztési környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-117">Set up hello development environment</span></span>
<span data-ttu-id="b4d59-118">Állítsa be a fejlesztési környezetet telepítésével hello Azure SDK for .NET 2.5.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="b4d59-118">Set up your development environment by installing hello Azure SDK for .NET 2.5.1 or later.</span></span> 

1. <span data-ttu-id="b4d59-119">tooinstall hello Azure SDK for .NET, hello hivatkozásra kattintva.</span><span class="sxs-lookup"><span data-stu-id="b4d59-119">tooinstall hello Azure SDK for .NET, click hello link below.</span></span> <span data-ttu-id="b4d59-120">Ha nincs még telepítve a Visual Studio 2013, az hello csatlakozásonkénti települ.</span><span class="sxs-lookup"><span data-stu-id="b4d59-120">If you don't have Visual Studio 2013 installed yet, it will be installed by hello link.</span></span> <span data-ttu-id="b4d59-121">Ez az oktatóanyag a Visual Studio 2013 van szükség.</span><span class="sxs-lookup"><span data-stu-id="b4d59-121">This tutorial requires Visual Studio 2013.</span></span> <span data-ttu-id="b4d59-122">[Az Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span><span class="sxs-lookup"><span data-stu-id="b4d59-122">[Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span></span>
2. <span data-ttu-id="b4d59-123">Hello Webplatform-telepítő ablakában kattintson **telepítése** és hello a telepítés folytatásához.</span><span class="sxs-lookup"><span data-stu-id="b4d59-123">In hello Web Platform Installer window, click **Install** and proceed with hello installation.</span></span>

<span data-ttu-id="b4d59-124">Egy mobil böngésző emulátor is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="b4d59-124">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="b4d59-125">Hello következő működnek:</span><span class="sxs-lookup"><span data-stu-id="b4d59-125">Any of hello following will work:</span></span>

* <span data-ttu-id="b4d59-126">A böngésző emulátor [Internet Explorer 11 F12 fejlesztői eszközök] [ EmulatorIE11] (az összes mobil böngésző képernyőképeket szerepel).</span><span class="sxs-lookup"><span data-stu-id="b4d59-126">Browser Emulator in [Internet Explorer 11 F12 developer tools][EmulatorIE11] (used in all mobile browser screenshots).</span></span> <span data-ttu-id="b4d59-127">A Windows Phone 8, Windows Phone 7 és Apple iPad felhasználói ügynök karakterlánca készletek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b4d59-127">It has user agent string presets for Windows Phone 8, Windows Phone 7, and Apple iPad.</span></span>
* <span data-ttu-id="b4d59-128">A böngésző emulátor [Google Chrome DevTools][EmulatorChrome].</span><span class="sxs-lookup"><span data-stu-id="b4d59-128">Browser Emulator in [Google Chrome DevTools][EmulatorChrome].</span></span> <span data-ttu-id="b4d59-129">Számos Android-eszközök, valamint Apple iPhone, Apple iPad és Amazon Kindle tűz készletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b4d59-129">It contains presets for numerous Android devices, as well as Apple iPhone, Apple iPad, and Amazon Kindle Fire.</span></span> <span data-ttu-id="b4d59-130">Azt is emulálja touch események.</span><span class="sxs-lookup"><span data-stu-id="b4d59-130">It also emulates touch events.</span></span>
* <span data-ttu-id="b4d59-131">[Opera Mobilemulátoron][EmulatorOpera]</span><span class="sxs-lookup"><span data-stu-id="b4d59-131">[Opera Mobile Emulator][EmulatorOpera]</span></span>

<span data-ttu-id="b4d59-132">A Visual Studio-projektek C\# forráskód elérhető tooaccompany ebben a témakörben vannak:</span><span class="sxs-lookup"><span data-stu-id="b4d59-132">Visual Studio projects with C\# source code are available tooaccompany this topic:</span></span>

* <span data-ttu-id="b4d59-133">[Alapszintű projekt letöltése][StarterProject]</span><span class="sxs-lookup"><span data-stu-id="b4d59-133">[Starter project download][StarterProject]</span></span>
* <span data-ttu-id="b4d59-134">[Projekt letöltése befejeződött][CompletedProject]</span><span class="sxs-lookup"><span data-stu-id="b4d59-134">[Completed project download][CompletedProject]</span></span>

## <span data-ttu-id="b4d59-135"><a name="bkmk_DeployStarterProject"></a>Hello alapszintű projekt tooan Azure webalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="b4d59-135"><a name="bkmk_DeployStarterProject"></a>Deploy hello starter project tooan Azure web app</span></span>
1. <span data-ttu-id="b4d59-136">Töltse le a hello konferencia-listát alkalmazás [alapszintű projekt][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="b4d59-136">Download hello conference-listing application [starter project][StarterProject].</span></span>
2. <span data-ttu-id="b4d59-137">Ezután a Windows Intézőt, kattintson a jobb gombbal a hello letöltött ZIP-fájl, és válassza a *tulajdonságok*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-137">Then in Windows Explorer, right-click hello downloaded ZIP file and choose *Properties*.</span></span>
3. <span data-ttu-id="b4d59-138">A hello **tulajdonságok** párbeszédpanelen válassza ki a hello **Unblock** gombra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-138">In hello **Properties** dialog box, choose hello **Unblock** button.</span></span> <span data-ttu-id="b4d59-139">(Blokkolásának feloldása megakadályozza, hogy a biztonsági figyelmeztetést, amely akkor fordul elő, amikor megpróbál toouse egy *.zip* hello webről letöltött fájl.)</span><span class="sxs-lookup"><span data-stu-id="b4d59-139">(Unblocking prevents a security warning that occurs when you try toouse a *.zip* file that you've downloaded from hello web.)</span></span>
4. <span data-ttu-id="b4d59-140">Kattintson a jobb gombbal a hello ZIP-fájl, és válassza ki **összes kibontása** hello fájl kibontásához.</span><span class="sxs-lookup"><span data-stu-id="b4d59-140">Right-click hello ZIP file and select **Extract All** to unzip hello file.</span></span> 
5. <span data-ttu-id="b4d59-141">A Visual Studióban nyissa meg a hello *C#\Mvc5Mobile.sln* fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-141">In Visual Studio, open hello *C#\Mvc5Mobile.sln* file.</span></span>
6. <span data-ttu-id="b4d59-142">A Megoldáskezelőben kattintson a jobb gombbal a projekt hello, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-142">In Solution Explorer, right-click hello project and click **Publish**.</span></span>
   
   ![][DeployClickPublish]
7. <span data-ttu-id="b4d59-143">A webhely közzététele kattintson **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-143">In Publish Web, click **Microsoft Azure App Service**.</span></span>
   
   ![][DeployClickWebSites]
8. <span data-ttu-id="b4d59-144">Ha még nem jelentkezett be Azure, kattintson a **vegyen fel egy fiókot**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-144">If you haven't already logged into Azure, click **Add an account**.</span></span>
   
   ![][DeploySignIn]
9. <span data-ttu-id="b4d59-145">Hajtsa végre a hello kér toolog be Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="b4d59-145">Follow hello prompts toolog into your Azure account.</span></span>
10. <span data-ttu-id="b4d59-146">App Service párbeszédpanelen hello most meg kell jelennie, aláírt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-146">hello App Service dialog should now show you as signed in.</span></span> <span data-ttu-id="b4d59-147">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="b4d59-147">Click **New**.</span></span>
    
    ![][DeployNewWebsite]  
11. <span data-ttu-id="b4d59-148">A hello **webalkalmazásnév** mezőben adja meg egy egyedi alkalmazás nevének előtagja.</span><span class="sxs-lookup"><span data-stu-id="b4d59-148">In hello **Web App Name** field, specify a unique app name prefix.</span></span> <span data-ttu-id="b4d59-149">A webalkalmazás teljesen minősített neve lesz  *&lt;előtag >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="b4d59-149">Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net.</span></span> <span data-ttu-id="b4d59-150">Válassza ki vagy adjon meg egy új erőforráscsoport neve az is, **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-150">Also, select or specify a new resource group name in **Resource group**.</span></span> <span data-ttu-id="b4d59-151">Kattintson a **új** toocreate egy új App Service-csomagot.</span><span class="sxs-lookup"><span data-stu-id="b4d59-151">Then, click **New** toocreate a new App Service plan.</span></span>
    
    ![][DeploySiteSettings]
12. <span data-ttu-id="b4d59-152">Hello új App Service-csomag konfigurálása és **OK**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-152">Configure hello new App Service plan and click **OK**.</span></span> 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. <span data-ttu-id="b4d59-153">Vissza a hello létrehozása az App Service párbeszédpanelen kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-153">Back in hello Create App Service dialog, click **Create**.</span></span>
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. <span data-ttu-id="b4d59-154">Hello Azure-erőforrások létrehozása után hello webhely közzététele párbeszédpanelen beírja az új alkalmazás hello-beállítások.</span><span class="sxs-lookup"><span data-stu-id="b4d59-154">After hello Azure resources are created, hello Publish Web dialog will be filled with hello settings for your new app.</span></span> <span data-ttu-id="b4d59-155">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-155">Click **Publish**.</span></span>
    
    ![][DeployPublishSite]
    
    <span data-ttu-id="b4d59-156">Miután a Visual Studio befejezi a közzétételi hello alapszintű projekt toohello Azure web app, hello asztali böngésző megnyitja toodisplay hello élő webalkalmazását.</span><span class="sxs-lookup"><span data-stu-id="b4d59-156">Once Visual Studio finishes publishing hello starter project toohello Azure web app, hello desktop browser opens toodisplay hello live web app.</span></span>
15. <span data-ttu-id="b4d59-157">Indítsa el a mobil böngésző emulátor, és hello konferencia alkalmazás hello URL-Címének másolása (*<prefix>*. azurewebsites.net) történő hello emulátor, majd kattintson a jobb felső gombra, és válassza ki **kódcímkeKeressemeg**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-157">Start your mobile browser emulator, copy hello URL for hello conference application (*<prefix>*.azurewebsites.net) into hello emulator, and then click the top-right button and select **Browse by tag**.</span></span> <span data-ttu-id="b4d59-158">Ha az Internet Explorer 11 hello az alapértelmezett böngészőt használ, csupán be kell tootype `F12`, majd `Ctrl+8`, majd módosítsa túl hello browser-profilt**Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-158">If you are using Internet Explorer 11 as hello default browser, you just need tootype `F12`, then `Ctrl+8`, and then change hello browser profile too**Windows Phone**.</span></span> <span data-ttu-id="b4d59-159">Az alábbi képen láthatók hello *AllTags* nézet álló módban (válasszon **kódcímke Tallózás**).</span><span class="sxs-lookup"><span data-stu-id="b4d59-159">The image below shows hello *AllTags* view in portrait mode (from choosing **Browse by tag**).</span></span>
    
    ![][AllTags]

> [!TIP]
> <span data-ttu-id="b4d59-160">Visual Studio az MVC 5 alkalmazás megoldhassuk, amíg a webes alkalmazás tooAzure közzéteheti újra tooverify hello élő webalkalmazását-ről a mobil böngésző vagy a böngésző emulátor.</span><span class="sxs-lookup"><span data-stu-id="b4d59-160">While you can debug your MVC 5 application from within Visual Studio, you can publish your web app tooAzure again tooverify hello live web app directly from your mobile browser or a browser emulator.</span></span>
> 
> 

<span data-ttu-id="b4d59-161">hello megjelenítési akkor nagyon olvasható mobil eszközön.</span><span class="sxs-lookup"><span data-stu-id="b4d59-161">hello display is very readable on a mobile device.</span></span> <span data-ttu-id="b4d59-162">Néhány hello vizuális hatások hello rendszerindítási CSS keretrendszer által alkalmazott már is látható.</span><span class="sxs-lookup"><span data-stu-id="b4d59-162">You can also already see some of hello visual effects applied by hello Bootstrap CSS framework.</span></span>
<span data-ttu-id="b4d59-163">Kattintson a hello **ASP.NET** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-163">Click hello **ASP.NET** link.</span></span>

![][SessionsByTagASP.NET]

<span data-ttu-id="b4d59-164">ASP.NET címke nézet hello nagyítás felszerelt toohello képernyő, amely rendszerindítási automatikusan elvégzi az Ön.</span><span class="sxs-lookup"><span data-stu-id="b4d59-164">hello ASP.NET tag view is zoom-fitted toohello screen, which Bootstrap does for you automatically.</span></span> <span data-ttu-id="b4d59-165">Azonban a nézet toobetter színből hello mobil böngésző javíthatja.</span><span class="sxs-lookup"><span data-stu-id="b4d59-165">However, you can improve this view toobetter suit hello mobile browser.</span></span> <span data-ttu-id="b4d59-166">Például hello **dátum** oszlop nehezen olvasható.</span><span class="sxs-lookup"><span data-stu-id="b4d59-166">For example, hello **Date** column is difficult to read.</span></span> <span data-ttu-id="b4d59-167">Hello oktatóanyag későbbi részében módosítani fogjuk hello *AllTags* toomake megtekintése mobilbarát azt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-167">Later in hello tutorial you'll change hello *AllTags* view toomake it mobile-friendly.</span></span>

## <span data-ttu-id="b4d59-168"><a name="bkmk_bootstrap"></a>A rendszerindítási CSS-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="b4d59-168"><a name="bkmk_bootstrap"></a> Bootstrap CSS Framework</span></span>
<span data-ttu-id="b4d59-169">Új az MVC 5 hello sablon rendszer beépített rendszer-indításkori támogatja.</span><span class="sxs-lookup"><span data-stu-id="b4d59-169">New in hello MVC 5 template is built-in Bootstrap support.</span></span> <span data-ttu-id="b4d59-170">Hogyan azonnal javítja hello különböző nézeteket az alkalmazás már láthatta.</span><span class="sxs-lookup"><span data-stu-id="b4d59-170">You have already seen how it immediately improves hello different views in your application.</span></span> <span data-ttu-id="b4d59-171">Például hello navigációs sáv felső hello esetén automatikusan összecsukható hello böngésző szélesség kisebb.</span><span class="sxs-lookup"><span data-stu-id="b4d59-171">For example, hello navigation bar at hello top is automatically collapsible when hello browser width is smaller.</span></span> <span data-ttu-id="b4d59-172">Hello a böngésző asztali próbálja hello böngészőablak átméretezése, és hogyan hello navigációs sáv változása a Megjelenés és működés.</span><span class="sxs-lookup"><span data-stu-id="b4d59-172">On hello desktop browser, try resizing hello browser window and see how hello navigation bar changes its look and feel.</span></span> <span data-ttu-id="b4d59-173">Ez a rendszerindítási beépített hello rugalmas Webtervezés.</span><span class="sxs-lookup"><span data-stu-id="b4d59-173">This is hello responsive web design that is built into Bootstrap.</span></span>

<span data-ttu-id="b4d59-174">toosee hogyan hello webalkalmazás lenne nélkül rendszerindítási, nyissa meg a *App\_Start\\BundleConfig.cs* és hello sorokat tartalmazó megjegyzésbe *bootstrap.js* és *bootstrap.css*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-174">toosee how hello Web app would look without Bootstrap, open *App\_Start\\BundleConfig.cs* and comment out hello lines that contain *bootstrap.js* and *bootstrap.css*.</span></span> <span data-ttu-id="b4d59-175">hello következő kód bemutatja, hello hello utolsó két állapotkimutatások `RegisterBundles` metódus hello módosítás után:</span><span class="sxs-lookup"><span data-stu-id="b4d59-175">hello following code shows hello last two statements of hello `RegisterBundles` method after hello change:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

<span data-ttu-id="b4d59-176">Nyomja le az `Ctrl+F5` toorun hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b4d59-176">Press `Ctrl+F5` toorun hello application.</span></span>

<span data-ttu-id="b4d59-177">Megfigyelheti, hogy hello összecsukható navigációs sáv most csak egy szokásos rendezetlen lista.</span><span class="sxs-lookup"><span data-stu-id="b4d59-177">Observe that hello collapsible navigation bar is now just an ordinary unordered list.</span></span> <span data-ttu-id="b4d59-178">Kattintson a **kódcímke Tallózás** újra, majd kattintson a **ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-178">Click **Browse by tag** again, then click **ASP.NET**.</span></span>
<span data-ttu-id="b4d59-179">Hello mobilemulátoron nézetben tekintheti meg most, hogy már nem nagyítás felszerelt toohello képernyő, és kell rendelés toosee hello jobb oldalán hello tábla oldalirányba görgessen.</span><span class="sxs-lookup"><span data-stu-id="b4d59-179">In hello mobile emulator view, you can see now that it is no longer zoom-fitted toohello screen, and you must scroll sideways in order toosee hello right side of hello table.</span></span>

![][SessionsByTagASP.NETNoBootstrap]

<span data-ttu-id="b4d59-180">Visszavonja a módosításokat, és frissítse a hello mobil böngésző tooverify hello mobilbarát megjelenítési helyreállt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-180">Undo your changes and refresh hello mobile browser tooverify that hello mobile-friendly display has been restored.</span></span>

<span data-ttu-id="b4d59-181">Rendszerindítási nincs meghatározott tooASP.NET MVC 5, és veheti hasznát ezeket a szolgáltatásokat bármely webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b4d59-181">Bootstrap is not specific tooASP.NET MVC 5, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="b4d59-182">De most épített ASP.NET MVC 5 projektsablon, így az MVC 5-webalkalmazás kihasználhatja a rendszerindítás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b4d59-182">But it is now built into the ASP.NET MVC 5 project template, so that your MVC 5 Web application can take advantage of Bootstrap by default.</span></span>

<span data-ttu-id="b4d59-183">A rendszerindítási kapcsolatos további információkért lásd a toothe [rendszerindítási] [ BootstrapSite] hely.</span><span class="sxs-lookup"><span data-stu-id="b4d59-183">For more information about Bootstrap, go toothe [Bootstrap][BootstrapSite] site.</span></span>

<span data-ttu-id="b4d59-184">A következő szakaszban hello látni fogja, hogyan tooprovide mobile-böngésző bizonyos nézeteket.</span><span class="sxs-lookup"><span data-stu-id="b4d59-184">In hello next section you'll see how tooprovide mobile-browser specific views.</span></span>

## <span data-ttu-id="b4d59-185"><a name="bkmk_overrideviews"></a>Bírálja felül a hello nézetek, elrendezés és részleges nézetek</span><span class="sxs-lookup"><span data-stu-id="b4d59-185"><a name="bkmk_overrideviews"></a> Override hello Views, Layouts, and Partial Views</span></span>
<span data-ttu-id="b4d59-186">Általában az egyes mobil böngésző vagy adott böngészők mobilböngészők (beleértve a elrendezések és a részleges nézetek) nézeten felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="b4d59-186">You can override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="b4d59-187">tooprovide egy mobileszköz-specifikus megtekintéséhez másolja át a nézet fájlt, és adja hozzá *. Mobile* toohello fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="b4d59-187">tooprovide a mobile-specific view, you can copy a view file and add *.Mobile* toohello file name.</span></span> <span data-ttu-id="b4d59-188">Például egy mobileszköz toocreate *Index* másolhatja nézet, *nézetek\\Home\\Index.cshtml* való *nézetek\\kezdőlap\\ Index.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-188">For example, toocreate a mobile *Index* view, you can copy *Views\\Home\\Index.cshtml* to *Views\\Home\\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="b4d59-189">Ebben a szakaszban létre fog hozni egy mobileszköz-specifikus fájlt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-189">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="b4d59-190">toostart, másolása *nézetek\\megosztott\\\_Layout.cshtml* való *nézetek\\megosztott\\\_Layout.Mobile.cshtml* .</span><span class="sxs-lookup"><span data-stu-id="b4d59-190">toostart, copy *Views\\Shared\\\_Layout.cshtml* to *Views\\Shared\\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="b4d59-191">Nyissa meg  *\_Layout.Mobile.cshtml* , és módosítsa a hello cím **MVC5 alkalmazás** túl**MVC5-alkalmazás (mobil)**.</span><span class="sxs-lookup"><span data-stu-id="b4d59-191">Open *\_Layout.Mobile.cshtml* and change hello title from **MVC5 Application** too**MVC5 Application (Mobile)**.</span></span>

<span data-ttu-id="b4d59-192">Az egyes `Html.ActionLink` hello navigációs sáv hívja, akkor távolítsa el a "Tallózás alapja" minden hivatkozás *ActionLink*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-192">In each `Html.ActionLink` call for hello navigation bar, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="b4d59-193">hello következő kód bemutatja befejeződött hello `<ul class="nav navbar-nav">` hello mobil elrendezés fájl címke.</span><span class="sxs-lookup"><span data-stu-id="b4d59-193">hello following code shows hello completed `<ul class="nav navbar-nav">` tag of hello mobile layout file.</span></span>

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

<span data-ttu-id="b4d59-194">Másolás hello *nézetek\\Home\\AllTags.cshtml* fájl *nézetek\\Home\\AllTags.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-194">Copy hello *Views\\Home\\AllTags.cshtml* file to *Views\\Home\\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="b4d59-195">Nyissa meg hello új fájlt, és módosítsa a `<h2>` "Címkék" elem túl "címkék (M)":</span><span class="sxs-lookup"><span data-stu-id="b4d59-195">Open hello new file and change the `<h2>` element from "Tags" too"Tags (M)":</span></span>

    <h2>Tags (M)</h2>

<span data-ttu-id="b4d59-196">Keresse meg a toohello Címkék lap egy asztali böngészőt használ, és a mobil böngésző emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="b4d59-196">Browse toohello tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="b4d59-197">hello mobil böngésző emulátor hello két végrehajtott változtatásokat mutatja (a cím hello  *\_Layout.Mobile.cshtml* és hello cím és *AllTags.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b4d59-197">hello mobile browser emulator shows hello two changes you made (hello title from *\_Layout.Mobile.cshtml* and hello title from *AllTags.Mobile.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobile]

<span data-ttu-id="b4d59-198">Ezzel ellentétben nem változott hello asztali megjelenítési (a címei  *\_Layout.cshtml* és *AllTags.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b4d59-198">In contrast, hello desktop display has not changed (with titles from *\_Layout.cshtml* and *AllTags.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobileDesktop]

## <span data-ttu-id="b4d59-199"><a name="bkmk_browserviews"></a>Böngésző-specifikus nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4d59-199"><a name="bkmk_browserviews"></a> Create Browser-Specific Views</span></span>
<span data-ttu-id="b4d59-200">Továbbá toomobile- és asztali-specifikus nézetek, létrehozhat egy egyedi böngésző nézetek.</span><span class="sxs-lookup"><span data-stu-id="b4d59-200">In addition toomobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="b4d59-201">Például létrehozhat nézeteket, amelyek a hello iPhone vagy hello Android böngészőhöz.</span><span class="sxs-lookup"><span data-stu-id="b4d59-201">For example, you can create views that are specifically for hello iPhone or hello Android browser.</span></span> <span data-ttu-id="b4d59-202">Ebben a szakaszban létrehozunk egy elrendezés hello iPhone-böngésző és egy iPhone verzióját hello *AllTags* nézet.</span><span class="sxs-lookup"><span data-stu-id="b4d59-202">In this section, you'll create a layout for hello iPhone browser and an iPhone version of hello *AllTags* view.</span></span>

<span data-ttu-id="b4d59-203">Nyissa meg hello *Global.asax* fájlt, és adja hozzá a következő kód toohello alsó részén hello a `Application_Start` metódust.</span><span class="sxs-lookup"><span data-stu-id="b4d59-203">Open hello *Global.asax* file and add hello following code toohello bottom of the `Application_Start` method.</span></span>

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

<span data-ttu-id="b4d59-204">Ez a kód határozza meg egy új, "iPhone", amely az egyes bejövő kérelmek elleni megfeleltetésének nevű megjelenítési mód.</span><span class="sxs-lookup"><span data-stu-id="b4d59-204">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="b4d59-205">Ha hello bejövő kérelem megfelel a következő feltételt (ha hello felhasználói ügynök tartalmaz hello karakterlánc "iPhone") megadott, az ASP.NET MVC nézetek, amelynek a neve tartalmazza a "iPhone" utótag fogja keresni.</span><span class="sxs-lookup"><span data-stu-id="b4d59-205">If hello incoming request matches the condition you defined (that is, if hello user agent contains hello string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

> [!NOTE]
> <span data-ttu-id="b4d59-206">Ha mobil böngésző-specifikus hozzáadása megjelenítési módok, például iPhone és Android rendszerhez készült, túl lehet, hogy tooset hello első argumentum`0` (Beszúrás hello lista hello tetején) toomake meg arról, hogy hello böngésző-specifikus üzemmód elsőbbséget élvez hello mobil sablon (*. Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="b4d59-206">When adding mobile browser-specific display modes, such as for iPhone and Android, be sure tooset hello first argument too`0` (insert at hello top of hello list) toomake sure that hello browser-specific mode takes precedence over hello mobile template (*.Mobile.cshtml).</span></span> <span data-ttu-id="b4d59-207">Ha hello mobil sablon hello lista hello tetején helyette, ki lesz jelölve a kívánt megjelenítési mód (hello első egyezés a wins, és megfelelő összes mobilböngészők hello mobil sablon).</span><span class="sxs-lookup"><span data-stu-id="b4d59-207">If hello mobile template is at hello top of hello list instead, it will be selected over your intended display mode (hello first match wins, and hello mobile template matches all mobile browsers).</span></span> 
> 
> 

<span data-ttu-id="b4d59-208">Hello kódban, kattintson a jobb gombbal `DefaultDisplayMode`, válassza a **megoldásához**, és válassza a `using System.Web.WebPages;`.</span><span class="sxs-lookup"><span data-stu-id="b4d59-208">In hello code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="b4d59-209">Ezzel hozzáad egy hivatkozást toothe `System.Web.WebPages` névtér, amely akkor, ha a `DisplayModeProvider` és `DefaultDisplayMode` van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="b4d59-209">This adds a reference toothe `System.Web.WebPages` namespace, which is where the `DisplayModeProvider` and `DefaultDisplayMode` types are defined.</span></span>

![][ResolveDefaultDisplayMode]

<span data-ttu-id="b4d59-210">Azt is megteheti, csak kézzel is felveheti a következő sor toothe hello `using` hello fájl szakaszában.</span><span class="sxs-lookup"><span data-stu-id="b4d59-210">Alternatively, you can just manually add hello following line toothe `using` section of hello file.</span></span>

    using System.Web.WebPages;

<span data-ttu-id="b4d59-211">Hello módosítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4d59-211">Save hello changes.</span></span> <span data-ttu-id="b4d59-212">Másolás a *nézetek\\megosztott\\\_Layout.Mobile.cshtml* fájl *nézetek\\megosztott\\\_Layout.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-212">Copy the *Views\\Shared\\\_Layout.Mobile.cshtml* file to *Views\\Shared\\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="b4d59-213">Nyissa meg hello új fájlt, és módosítsa a hello cím `MVC5 Application (Mobile)` való `MVC5 Application (iPhone)`.</span><span class="sxs-lookup"><span data-stu-id="b4d59-213">Open hello new file and then change hello title from `MVC5 Application (Mobile)` to `MVC5 Application (iPhone)`.</span></span>

<span data-ttu-id="b4d59-214">Másolás hello *nézetek\\Home\\AllTags.Mobile.cshtml* fájl *nézetek\\Home\\AllTags.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-214">Copy hello *Views\\Home\\AllTags.Mobile.cshtml* file to *Views\\Home\\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="b4d59-215">Hello új fájlba, módosítsa a hello `<h2>` elem a "címkék (M)" túl "címkét (iPhone)".</span><span class="sxs-lookup"><span data-stu-id="b4d59-215">In hello new file, change hello `<h2>` element from "Tags (M)" too"Tags (iPhone)".</span></span>

<span data-ttu-id="b4d59-216">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b4d59-216">Run hello application.</span></span> <span data-ttu-id="b4d59-217">Futtassa a mobil böngésző emulátor, győződjön meg arról, hogy a felhasználói ügynök értéke túl "iPhone", és keresse meg a toohello *AllTags* nézet.</span><span class="sxs-lookup"><span data-stu-id="b4d59-217">Run a mobile browser emulator, make sure its user agent is set too"iPhone", and browse toohello *AllTags* view.</span></span> <span data-ttu-id="b4d59-218">Ha hello emulátor használ az Internet Explorer 11 F12 fejlesztői eszközök, állítsa be az emuláció toohello következőket:</span><span class="sxs-lookup"><span data-stu-id="b4d59-218">If you are using hello emulator in Internet Explorer 11 F12 developer tools, configure emulation toohello following:</span></span>

* <span data-ttu-id="b4d59-219">Böngésző profil = **Windows Phone**</span><span class="sxs-lookup"><span data-stu-id="b4d59-219">Browser profile = **Windows Phone**</span></span>
* <span data-ttu-id="b4d59-220">Felhasználói ügynök karakterlánca = **egyéni**</span><span class="sxs-lookup"><span data-stu-id="b4d59-220">User agent string =  **Custom**</span></span>
* <span data-ttu-id="b4d59-221">Egyéni karakterláncot = **Apple-iPhone5C1/1001.525**</span><span class="sxs-lookup"><span data-stu-id="b4d59-221">Custom string = **Apple-iPhone5C1/1001.525**</span></span>

<span data-ttu-id="b4d59-222">hello alábbi képernyőfelvételen látható hello *AllTags* nézet megjelenítése az emulátorban, az Internet Explorer 11 F12 fejlesztői eszközök hello egyéni felhasználói ügynök karakterlánca (Ez az egy iPhone 5 C felhasználói ügynök karakterlánca).</span><span class="sxs-lookup"><span data-stu-id="b4d59-222">hello following screenshot shows hello *AllTags* view rendered in the emulator in Internet Explorer 11 F12 developer tools with hello custom user agent string (this is an iPhone 5C user agent string).</span></span>

![][AllTagsIPhone_LayoutIPhone]

<span data-ttu-id="b4d59-223">Hello mobil böngészőben, válassza ki a hello **hangszórók** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-223">In hello mobile browser, select hello **Speakers** link.</span></span> <span data-ttu-id="b4d59-224">Mivel nincs mobil nézet (*AllSpeakers.Mobile.cshtml*), hello alapértelmezett hangszórók megtekintése (*AllSpeakers.cshtml*) használatával hello mobil elrendezés nézet megjelenítése ( *\_ Layout.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b4d59-224">Because there's not a mobile view (*AllSpeakers.Mobile.cshtml*), hello default speakers view (*AllSpeakers.cshtml*) is rendered using hello mobile layout view (*\_Layout.Mobile.cshtml*).</span></span> <span data-ttu-id="b4d59-225">Az alábbi módon hello cím **MVC5-alkalmazás (mobil)** meghatározott  *\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-225">As shown below, hello title **MVC5 Application (Mobile)** is defined in *\_Layout.Mobile.cshtml*.</span></span>

![][AllSpeakers_LayoutMobile]

<span data-ttu-id="b4d59-226">Úgy, hogy a megjelenítési belül mobil elrendezés alapértelmezett (nem mobil) nézet globálisan letilthatja `RequireConsistentDisplayMode` való `true` a hello *nézetek\\\_ViewStart.cshtml* fájl ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="b4d59-226">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in hello *Views\\\_ViewStart.cshtml* file, like this:</span></span>

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

<span data-ttu-id="b4d59-227">Ha `RequireConsistentDisplayMode` értéke túl`true`, hello mobil elrendezés (*\_Layout.Mobile.cshtml*) csak mobil nézetek használatos (azaz hello űrlap a nézet fájl esetén  ***Nézet_neve** . Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="b4d59-227">When `RequireConsistentDisplayMode` is set too`true`, hello mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views (i.e. when the view file is of hello form ***ViewName**.Mobile.cshtml*).</span></span> <span data-ttu-id="b4d59-228">Érdemes lehet tooset `RequireConsistentDisplayMode` túl`true` Ha a mobil elrendezés és a nem mobil nézetek nem működik.</span><span class="sxs-lookup"><span data-stu-id="b4d59-228">You might want tooset `RequireConsistentDisplayMode` too`true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="b4d59-229">hello az alábbi képernyőfelvételen hogyan hello *hangszórók* lap Renderelés mikor `RequireConsistentDisplayMode` értéke túl`true` (nélkül hello "(mobil)" hello felső navigációs sáv hello karakterlánc).</span><span class="sxs-lookup"><span data-stu-id="b4d59-229">hello screenshot below shows how hello *Speakers* page renders when `RequireConsistentDisplayMode` is set too`true` (without hello string "(Mobile)" in hello navigational bar at hello top).</span></span>

![][AllSpeakers_LayoutMobileOverridden]

<span data-ttu-id="b4d59-230">Adott nézetben konzisztens megjelenítési mód beállításával letilthatja `RequireConsistentDisplayMode` túl`false` hello nézet fájlban.</span><span class="sxs-lookup"><span data-stu-id="b4d59-230">You can disable consistent display mode in a specific view by setting `RequireConsistentDisplayMode` too`false` in hello view file.</span></span> <span data-ttu-id="b4d59-231">A következő kódban a hello *nézetek\\Home\\AllSpeakers.cshtml* beállítása fájl `RequireConsistentDisplayMode` túl`false`:</span><span class="sxs-lookup"><span data-stu-id="b4d59-231">The following markup in hello *Views\\Home\\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` too`false`:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

<span data-ttu-id="b4d59-232">Ebben a szakaszban is láttuk hogyan toocreate mobil elrendezések és a nézetek, és hogyan toocreate elrendezések és konkrét eszközöket, többek között nézeteket hello iPhone.</span><span class="sxs-lookup"><span data-stu-id="b4d59-232">In this section we've seen how toocreate mobile layouts and views and how toocreate layouts and views for specific devices such as hello iPhone.</span></span>
<span data-ttu-id="b4d59-233">Azonban hello fő hello rendszerindítási CSS keretrendszer előnye a megfelelően rugalmas elrendezést, ami azt jelenti, hogy egyetlen stíluslap asztali, telefon és táblagép böngészők toocreate egy egységes megjelenést és működést alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="b4d59-233">However, hello main advantage of hello Bootstrap CSS framework is the responsive layout, which means that a single stylesheet can be applied across desktop, phone, and tablet browsers toocreate a consistent look and feel.</span></span> <span data-ttu-id="b4d59-234">Hello a következő szakaszban láthatja, hogyan tooleverage Bootstrap toocreate mobilbarát nézetek.</span><span class="sxs-lookup"><span data-stu-id="b4d59-234">In hello next section you'll see how tooleverage Bootstrap toocreate mobile-friendly views.</span></span>

## <span data-ttu-id="b4d59-235"><a name="bkmk_Improvespeakerslist"></a>Hello hangszórók lista javítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-235"><a name="bkmk_Improvespeakerslist"></a> Improve hello Speakers List</span></span>
<span data-ttu-id="b4d59-236">Ekkor csak a böngészőben, hello *hangszórók* nézet olvasható, de hello hivatkozások kicsi, és nehéz tootap mobil eszközön.</span><span class="sxs-lookup"><span data-stu-id="b4d59-236">As you just saw, hello *Speakers* view is readable, but hello links are small and are difficult tootap on a mobile device.</span></span> <span data-ttu-id="b4d59-237">Ebben a szakaszban meg kell győződnie hello *AllSpeakers* nézet mobilbarát, amely nagy, könnyen koppintson mutató hivatkozások megjelennek, és tartalmazza a keresési mezőbe tooquickly hangszórók található.</span><span class="sxs-lookup"><span data-stu-id="b4d59-237">In this section, you'll make hello *AllSpeakers* view mobile-friendly, which displays large, easy-to-tap links and contains a search box tooquickly find speakers.</span></span>

<span data-ttu-id="b4d59-238">Használhatja a rendszerindítási hello [csatolt csoportja] [ linked list group] stílusbeállításokat hello javítására *hangszórók* nézet.</span><span class="sxs-lookup"><span data-stu-id="b4d59-238">You can use hello Bootstrap [linked list group][linked list group] styling to improve hello *Speakers* view.</span></span> <span data-ttu-id="b4d59-239">A *nézetek\\Home\\AllSpeakers.cshtml*, hello hello Razor fájl tartalmának lecserélése hello kódot.</span><span class="sxs-lookup"><span data-stu-id="b4d59-239">In *Views\\Home\\AllSpeakers.cshtml*, replace hello contents of hello Razor file with hello code below.</span></span>

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

<span data-ttu-id="b4d59-240">Hello `class="list-group"` hello attribútum `<div>` címke alkalmazza a rendszer-indításkori lista stíluselemekkel és hello `class="input-group-item"` attribútum Bootstrap lista stílusbeállításokat tooeach hivatkozás vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4d59-240">hello `class="list-group"` attribute in hello `<div>` tag applies the Bootstrap list styling, and hello `class="input-group-item"` attribute applies Bootstrap list item styling tooeach link.</span></span>

<span data-ttu-id="b4d59-241">Frissítse a hello mobil böngészőt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-241">Refresh hello mobile browser.</span></span> <span data-ttu-id="b4d59-242">hello nézet néz frissítése:</span><span class="sxs-lookup"><span data-stu-id="b4d59-242">hello updated view looks like this:</span></span>

![][AllSpeakersFixed]

<span data-ttu-id="b4d59-243">hello rendszerindítási [csatolt csoportja] [ linked list group] stílusbeállításokat teszi hello teljes csoportban minden hivatkozás kattintható, amely sok jobb felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-243">hello Bootstrap [linked list group][linked list group] styling makes hello entire box for each link clickable, which is a much better user experience.</span></span> <span data-ttu-id="b4d59-244">Váltás toothe asztali nézetben, és tekintse meg az hello egységes megjelenést és működést.</span><span class="sxs-lookup"><span data-stu-id="b4d59-244">Switch toothe desktop view and observe hello consistent look and feel.</span></span>

![][AllSpeakersFixedDesktop]

<span data-ttu-id="b4d59-245">Hello mobil böngésző nézet javult, bár nehezen hello hangszórók hosszú listáját.</span><span class="sxs-lookup"><span data-stu-id="b4d59-245">Although hello mobile browser view has improved, it's difficult to navigate hello long list of speakers.</span></span> <span data-ttu-id="b4d59-246">Rendszerindítási nem biztosít egy keresési szűrő funkciót az a-kész, de néhány sornyi kódot is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b4d59-246">Bootstrap doesn't provide a search filter functionality out-of-the-box, but you can add it with a few lines of code.</span></span> <span data-ttu-id="b4d59-247">Akkor lesz először a keresési mezőbe toohello nézet hozzáadása, majd hello hello szűrőfüggvény JavaScript-kódot a fájlkiszolgálófürtöt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-247">You will first add a search box toohello view, then hook up with hello JavaScript code for hello filter function.</span></span> <span data-ttu-id="b4d59-248">A *nézetek\\Home\\AllSpeakers.cshtml*, vegye fel a \<űrlap\> címke után hello \<h2\> címke, a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="b4d59-248">In *Views\\Home\\AllSpeakers.cshtml*, add a \<form\> tag just after hello \<h2\> tag, as shown below:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

<span data-ttu-id="b4d59-249">Figyelje meg, hogy hello `<form>` és `<input>` mindkét címkék hello alkalmazott Bootstrap stílusok toothem rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b4d59-249">Notice that hello `<form>` and `<input>` tags both have hello Bootstrap styles applied toothem.</span></span> <span data-ttu-id="b4d59-250">Hello `<span>` elem hozzáadása a rendszerindítási [glyphicon] [ glyphicon] toothe keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="b4d59-250">hello `<span>` element adds a Bootstrap [glyphicon][glyphicon] toothe search box.</span></span>

<span data-ttu-id="b4d59-251">A hello *parancsfájlok* mappát, adja hozzá a JavaScript-fájl neve *filter.js*.</span><span class="sxs-lookup"><span data-stu-id="b4d59-251">In hello *Scripts* folder, add a JavaScript file called *filter.js*.</span></span> <span data-ttu-id="b4d59-252">Nyissa meg hello fájlt, és illessze be a kódot bele a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b4d59-252">Open hello file and paste hello following code into it:</span></span>

    $(function () {

        // reset hello search form when hello page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up hello events toohello <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with hello typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

<span data-ttu-id="b4d59-253">A regisztrált kötegek tooinclude filter.js kell.</span><span class="sxs-lookup"><span data-stu-id="b4d59-253">You also need tooinclude filter.js in your registered bundles.</span></span> <span data-ttu-id="b4d59-254">Nyissa meg *App\_Start\\BundleConfig.cs* , és módosítsa az első hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="b4d59-254">Open *App\_Start\\BundleConfig.cs* and change hello first bundles.</span></span> <span data-ttu-id="b4d59-255">Módosítsa az első `bundles.Add` utasítás (a hello **jquery** csomagot) tooinclude *parancsfájlok\\filter.js*, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4d59-255">Change the first `bundles.Add` statement (for hello **jquery** bundle) tooinclude *Scripts\\filter.js*, as follows:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

<span data-ttu-id="b4d59-256">Hello **jquery** hello alapértelmezés szerint már megjelenítve a nyaláb  *\_elrendezés* nézet.</span><span class="sxs-lookup"><span data-stu-id="b4d59-256">hello **jquery** bundle is already rendered by hello default *\_Layout* view.</span></span> <span data-ttu-id="b4d59-257">Később, hello használhatja ugyanazt a JavaScript-kód tooapply a szűrő funkció tooother nézetek.</span><span class="sxs-lookup"><span data-stu-id="b4d59-257">Later, you can utilize hello same JavaScript code tooapply the filter functionality tooother list views.</span></span>

<span data-ttu-id="b4d59-258">Frissítse a hello mobil böngésző, és nyissa meg toohello *AllSpeakers* nézet.</span><span class="sxs-lookup"><span data-stu-id="b4d59-258">Refresh hello mobile browser and go toohello *AllSpeakers* view.</span></span> <span data-ttu-id="b4d59-259">A keresési mezőbe írja be az "sc".</span><span class="sxs-lookup"><span data-stu-id="b4d59-259">In the search box, type "sc".</span></span> <span data-ttu-id="b4d59-260">hello hangszórók lista most szűri az tooyour keresési karakterlánc alapján történik.</span><span class="sxs-lookup"><span data-stu-id="b4d59-260">hello speakers list should now be filtered according tooyour search string.</span></span>

![][AllSpeakersFixedSearchBySC]

## <span data-ttu-id="b4d59-261"><a name="bkmk_improvetags"></a>Címkelista hello javítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-261"><a name="bkmk_improvetags"></a> Improve hello Tags List</span></span>
<span data-ttu-id="b4d59-262">Például a hello *hangszórók* megtekintéséhez hello *címkék* nézet olvasható, de hello hivatkozások kis- és nehéz a mobileszköz tootap.</span><span class="sxs-lookup"><span data-stu-id="b4d59-262">Like hello *Speakers* view, hello *Tags* view is readable, but hello links are small and difficult tootap on a mobile device.</span></span> <span data-ttu-id="b4d59-263">Ezt úgy javíthatja ki hello *címkék* nézet hello azonos módon hello megoldása *hangszórók* megtekintése, ha korábban, de hello következőre leírt hello kódmódosításokat `Html.ActionLink` metódus szintaxist  *Nézetek\\Home\\AllTags.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b4d59-263">You can fix hello *Tags* view hello same way you fix hello *Speakers* view, if you use hello code changes described earlier, but with hello following `Html.ActionLink` method syntax in *Views\\Home\\AllTags.cshtml*:</span></span>

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="b4d59-264">hello frissítésének asztali böngészőn megkeresi az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4d59-264">hello refreshed desktop browser looks as follows:</span></span>

![][AllTagsFixedDesktop]

<span data-ttu-id="b4d59-265">És hello frissülnek mobil böngésző megkeresi az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="b4d59-265">And hello refreshed mobile browser looks as follows:</span></span> 

![][AllTagsFixed]

> [!NOTE]
> <span data-ttu-id="b4d59-266">Észlel bennük hello eredeti lista formázást van továbbra is a mobil böngésző hello és milyen történt tooyour töltött Bootstrap stílusbeállításokat wonder, ez az összetevő a korábbi művelet toocreate mobil adott nézetek.</span><span class="sxs-lookup"><span data-stu-id="b4d59-266">If you notice that hello original list formatting is still there in hello mobile browser and wonder what happened tooyour nice Bootstrap styling, this is an artifact of your earlier action toocreate mobile specific views.</span></span> <span data-ttu-id="b4d59-267">Most, hogy hello rendszerindítási CSS keretrendszer toocreate rugalmas Webtervezés használ, lépjen a head, és távolítsa el a mobileszköz-specifikus és nézeteket hello mobile-specifikus elrendezés.</span><span class="sxs-lookup"><span data-stu-id="b4d59-267">However, now that you are using hello Bootstrap CSS framework toocreate a responsive web design, go head and remove these mobile-specific views and hello mobile-specific layout views.</span></span> <span data-ttu-id="b4d59-268">Ezzel végzett, hello frissíteni mobil böngésző hello Bootstrap stílusbeállításokat fognak megjelenni.</span><span class="sxs-lookup"><span data-stu-id="b4d59-268">Once you have done so, hello refreshed mobile browser will show hello Bootstrap styling.</span></span>
> 
> 

## <span data-ttu-id="b4d59-269"><a name="bkmk_improvedates"></a>Hello dátumok lista javítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-269"><a name="bkmk_improvedates"></a> Improve hello Dates List</span></span>
<span data-ttu-id="b4d59-270">Javíthatja a hello *dátumok* megtekintése, például hogy továbbfejlesztett hello *hangszórók* és *címkék* megtekinti hello kódmódosításokat korábban, de a következő hello leírt használatakor`Html.ActionLink` metódus szintaxist *nézetek\\Home\\AllDates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="b4d59-270">You can improve hello *Dates* view like you improved hello *Speakers* and *Tags* views if you use hello code changes described earlier, but with hello following `Html.ActionLink` method syntax in *Views\\Home\\AllDates.cshtml*:</span></span>

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="b4d59-271">A frissített mobil böngésző nézet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b4d59-271">You will get a refreshed mobile browser view like this:</span></span>

![][AllDatesFixed]

<span data-ttu-id="b4d59-272">Tovább növelhető a hello *dátumok* nézet hello dátum-idő értékek dátum szerint rendezésével.</span><span class="sxs-lookup"><span data-stu-id="b4d59-272">You can further improve hello *Dates* view by organizing hello date-time values by date.</span></span> <span data-ttu-id="b4d59-273">Ezt megteheti a rendszerindítási hello [panelek] [ panels] frizurakészítő.</span><span class="sxs-lookup"><span data-stu-id="b4d59-273">This can be done with hello Bootstrap [panels][panels] styling.</span></span> <span data-ttu-id="b4d59-274">Cserélje le a hello hello tartalmát *nézetek\\Home\\AllDates.cshtml* fájl a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="b4d59-274">Replace hello contents of hello *Views\\Home\\AllDates.cshtml* file with the following code:</span></span>

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

<span data-ttu-id="b4d59-275">Ez a kód létrehoz egy különálló `<div class="panel panel-primary">` címke az egyes különálló dátumának hello listában, és használja hello [csatolt csoportja] [ linked list group] számára a megfelelő csatolja azt korábban.</span><span class="sxs-lookup"><span data-stu-id="b4d59-275">This code creates a separate `<div class="panel panel-primary">` tag for each distinct date in hello list, and uses hello [linked list group][linked list group] for the respective links as before.</span></span> <span data-ttu-id="b4d59-276">Nyilvántartásunk szerint, amikor a kód lefut hello mobil böngésző itt található:</span><span class="sxs-lookup"><span data-stu-id="b4d59-276">Here's what hello mobile browser looks like when this code runs:</span></span>

![][AllDatesFixed2]

<span data-ttu-id="b4d59-277">Kapcsoló toohello asztali böngészőn.</span><span class="sxs-lookup"><span data-stu-id="b4d59-277">Switch toohello desktop browser.</span></span> <span data-ttu-id="b4d59-278">Ebben az esetben Megjegyzés hello egységes megjelenést.</span><span class="sxs-lookup"><span data-stu-id="b4d59-278">Again, note hello consistent look.</span></span>

![][AllDatesFixed2Desktop]

## <span data-ttu-id="b4d59-279"><a name="bkmk_improvesessionstable"></a>Hello SessionsTable nézet javítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-279"><a name="bkmk_improvesessionstable"></a> Improve hello SessionsTable View</span></span>
<span data-ttu-id="b4d59-280">Ebben a szakaszban meg kell győződnie hello *SessionsTable* mobilbarát további megtekintése.</span><span class="sxs-lookup"><span data-stu-id="b4d59-280">In this section, you'll make hello *SessionsTable* view more mobile-friendly.</span></span> <span data-ttu-id="b4d59-281">Ez a változás szélesebb körű hello korábbi módosítások.</span><span class="sxs-lookup"><span data-stu-id="b4d59-281">This change is more extensive hello previous changes.</span></span>

<span data-ttu-id="b4d59-282">Hello mobil böngészőben, koppintson a hello **címke** gombra, majd adja meg `asp` be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="b4d59-282">In hello mobile browser, tap hello **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="b4d59-283">Koppintson a hello **ASP.NET** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-283">Tap hello **ASP.NET** link.</span></span>

![][SessionsTableTagASP.NET]

<span data-ttu-id="b4d59-284">Ahogy látja, hello megjelenítési táblaként, amely jelenleg tervezett toobe hello asztali böngészőben megtekintett van formázva.</span><span class="sxs-lookup"><span data-stu-id="b4d59-284">As you can see, hello display is formatted as a table, which is currently designed toobe viewed in hello desktop browser.</span></span> <span data-ttu-id="b4d59-285">Azonban egy kicsit nehéz tooread mobil böngésző.</span><span class="sxs-lookup"><span data-stu-id="b4d59-285">However, it's a little bit difficult tooread on a mobile browser.</span></span> <span data-ttu-id="b4d59-286">toofix a, nyissa meg *nézetek\\Home\\SessionsTable.cshtml* és majd a fájl tartalmát hello cserélje le a következő kód hello:</span><span class="sxs-lookup"><span data-stu-id="b4d59-286">toofix this, open *Views\\Home\\SessionsTable.cshtml* and then replace hello contents of the file with hello following code:</span></span>

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

<span data-ttu-id="b4d59-287">hello kód 3 dolgot hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="b4d59-287">hello code does 3 things:</span></span>

* <span data-ttu-id="b4d59-288">használja a rendszerindítási hello [egyéni csatolt csoportja] [ custom linked list group] tooformat hello munkamenetadatai függőleges, így ezt az információt nem olvasható a mobil böngészőt (osztályok, mint a lista-csoport-elem-szövege)</span><span class="sxs-lookup"><span data-stu-id="b4d59-288">uses hello Bootstrap [custom linked list group][custom linked list group] tooformat hello session information vertically, so that all this information is readable on a mobile browser (using classes such as list-group-item-text)</span></span>
* <span data-ttu-id="b4d59-289">hello vonatkozik [rács rendszer] [ grid system] toothe elrendezés, így hello munkamenetben elemek vízszintes a hello asztali böngészőn kívül függőleges hello mobil böngészőben (hello oszlop-md-4 osztály használatával)</span><span class="sxs-lookup"><span data-stu-id="b4d59-289">applies hello [grid system][grid system] toothe layout, so that hello session items flow horizontally in hello desktop browser and vertically in hello mobile browser (using hello col-md-4 class)</span></span>
* <span data-ttu-id="b4d59-290">felhasználási hello [rugalmas segédprogramok] [ responsive utilities] hello munkamenet címkék (hello rejtett xs osztály használatával) hello mobil böngészőben megtekintve elrejtése</span><span class="sxs-lookup"><span data-stu-id="b4d59-290">uses hello [responsive utilities][responsive utilities] to hide hello session tags when viewed in hello mobile browser (using hello hidden-xs class)</span></span>

<span data-ttu-id="b4d59-291">A cím hivatkozás toogo toohello megfelelő munkamenet is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="b4d59-291">You can also tap a title link toogo toohello respective session.</span></span> <span data-ttu-id="b4d59-292">az alábbi képen hello hello kódmódosításokat tükrözi.</span><span class="sxs-lookup"><span data-stu-id="b4d59-292">hello image below reflects hello code changes.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="b4d59-293">hello Bootstrap rács rendszer, amely automatikusan alkalmazza a munkamenetek függőleges böngészőben hello mobil rendezi.</span><span class="sxs-lookup"><span data-stu-id="b4d59-293">hello Bootstrap grid system that you applied automatically arranges the sessions vertically in hello mobile browser.</span></span> <span data-ttu-id="b4d59-294">Figyelje meg is, hogy hello címkék nem láthatók.</span><span class="sxs-lookup"><span data-stu-id="b4d59-294">Also, notice that hello tags are not shown.</span></span> <span data-ttu-id="b4d59-295">Kapcsoló toohello asztali böngészőn.</span><span class="sxs-lookup"><span data-stu-id="b4d59-295">Switch toohello desktop browser.</span></span>

![][SessionsTableFixedTagASP.NETDesktop]

<span data-ttu-id="b4d59-296">Hello asztali böngészőben figyelje meg, hogy hello címkék jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b4d59-296">In hello desktop browser, notice that hello tags are now displayed.</span></span> <span data-ttu-id="b4d59-297">Ezenkívül látható, hogy a hello Bootstrap rács rendszer alkalmazott hello munkamenet elemek két oszlop rendezése.</span><span class="sxs-lookup"><span data-stu-id="b4d59-297">Also, you can see that hello Bootstrap grid system you applied arranges hello session items in two columns.</span></span> <span data-ttu-id="b4d59-298">Ha nagyítása a böngészőben, látni fogja, hogy a hello elrendezéssel toothree oszlopok megváltozik.</span><span class="sxs-lookup"><span data-stu-id="b4d59-298">If you enlarge the browser, you will see that hello arrangement changes toothree columns.</span></span>

## <span data-ttu-id="b4d59-299"><a name="bkmk_improvesessionbycode"></a>Hello SessionByCode nézet javítása</span><span class="sxs-lookup"><span data-stu-id="b4d59-299"><a name="bkmk_improvesessionbycode"></a> Improve hello SessionByCode View</span></span>
<span data-ttu-id="b4d59-300">Végezetül hello fogjuk kijavítani *SessionByCode* toomake megtekintése mobilbarát azt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-300">Finally, you'll fix hello *SessionByCode* view toomake it mobile-friendly.</span></span>

<span data-ttu-id="b4d59-301">Hello mobil böngészőben, koppintson a hello **címke** gombra, majd adja meg `asp` be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="b4d59-301">In hello mobile browser, tap hello **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="b4d59-302">Koppintson a hello **ASP.NET** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-302">Tap hello **ASP.NET** link.</span></span> <span data-ttu-id="b4d59-303">Hello ASP.NET címke munkamenetek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b4d59-303">Sessions for hello ASP.NET tag are displayed.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="b4d59-304">Válassza ki a hello **létrehozása egyetlen lap alkalmazást az ASP.NET és az AngularJS** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="b4d59-304">Choose hello **Building a Single Page Application with ASP.NET and AngularJS** link.</span></span>

![][SessionByCode3-644]

<span data-ttu-id="b4d59-305">hello alapértelmezett asztali nézetben megfelelően működik, de könnyen javíthatja a hello tekintse meg a bizonyos rendszerindítási GUI-összetevők.</span><span class="sxs-lookup"><span data-stu-id="b4d59-305">hello default desktop view is fine, but you can improve hello look easily by using some Bootstrap GUI components.</span></span>

<span data-ttu-id="b4d59-306">Nyissa meg *nézetek\\Home\\SessionByCode.cshtml* és hello tartalma cserélje le a következő markup hello:</span><span class="sxs-lookup"><span data-stu-id="b4d59-306">Open *Views\\Home\\SessionByCode.cshtml* and replace hello contents with hello following markup:</span></span>

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

<span data-ttu-id="b4d59-307">hello új markup tooimprove hello mobil nézet frizurakészítő Bootstrap panelek használja.</span><span class="sxs-lookup"><span data-stu-id="b4d59-307">hello new markup uses Bootstrap panels styling tooimprove hello mobile view.</span></span> 

<span data-ttu-id="b4d59-308">Frissítse a hello mobil böngészőt.</span><span class="sxs-lookup"><span data-stu-id="b4d59-308">Refresh hello mobile browser.</span></span> <span data-ttu-id="b4d59-309">hello példánycsoportokat módosításoknak hello kód csak végrehajtott:</span><span class="sxs-lookup"><span data-stu-id="b4d59-309">hello following image reflects hello code changes that you just made:</span></span>

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a><span data-ttu-id="b4d59-310">Burkolja és felülvizsgálata</span><span class="sxs-lookup"><span data-stu-id="b4d59-310">Wrap Up and Review</span></span>
<span data-ttu-id="b4d59-311">Ez az oktatóanyag azt mutatja, hogy hogyan toouse ASP.NET MVC 5 toodevelop mobilbarát webes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="b4d59-311">This tutorial has shown you how toouse ASP.NET MVC 5 toodevelop mobile-friendly Web applications.</span></span> <span data-ttu-id="b4d59-312">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="b4d59-312">These include:</span></span>

* <span data-ttu-id="b4d59-313">Az ASP.NET MVC 5 alkalmazás tooan App Service-webalkalmazások telepítése</span><span class="sxs-lookup"><span data-stu-id="b4d59-313">Deploy an ASP.NET MVC 5 application tooan App Service web app</span></span>
* <span data-ttu-id="b4d59-314">A rendszerindítási toocreate az MVC 5 alkalmazás rugalmas webes formátum használata</span><span class="sxs-lookup"><span data-stu-id="b4d59-314">Use Bootstrap toocreate responsive web layout in your MVC 5 application</span></span>
* <span data-ttu-id="b4d59-315">Bírálja felül a elrendezését, a nézeteket és a részleges nézetek globálisan és az egyes nézet</span><span class="sxs-lookup"><span data-stu-id="b4d59-315">Override layout, views, and partial views, both globally and for an individual view</span></span>
* <span data-ttu-id="b4d59-316">Vezérlő elrendezése és a részleges felülbírálása kényszerítési használatával a `RequireConsistentDisplayMode` tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4d59-316">Control layout and partial override enforcement using the `RequireConsistentDisplayMode` property</span></span>
* <span data-ttu-id="b4d59-317">Bizonyos böngészők, például hello iPhone böngésző célzó nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4d59-317">Create views that target specific browsers, such as hello iPhone browser</span></span>
* <span data-ttu-id="b4d59-318">Alkalmazza a rendszer-indításkori stílusbeállításokat Razor-kódban</span><span class="sxs-lookup"><span data-stu-id="b4d59-318">Apply Bootstrap styling in Razor code</span></span>

## <a name="see-also"></a><span data-ttu-id="b4d59-319">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b4d59-319">See Also</span></span>
* [<span data-ttu-id="b4d59-320">9-es alapelvek webes rugalmas kialakítás</span><span class="sxs-lookup"><span data-stu-id="b4d59-320">9 basic principles of responsive web design</span></span>](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* <span data-ttu-id="b4d59-321">[Rendszerindítási][BootstrapSite]</span><span class="sxs-lookup"><span data-stu-id="b4d59-321">[Bootstrap][BootstrapSite]</span></span>
* <span data-ttu-id="b4d59-322">[A rendszerindítási hivatalos blogja][Official Bootstrap Blog]</span><span class="sxs-lookup"><span data-stu-id="b4d59-322">[Official Bootstrap Blog][Official Bootstrap Blog]</span></span>
* <span data-ttu-id="b4d59-323">[Twitter Bootstrap oktatóprogram oktatóanyag Köztársaság][Twitter Bootstrap Tutorial from Tutorial Republic]</span><span class="sxs-lookup"><span data-stu-id="b4d59-323">[Twitter Bootstrap Tutorial from Tutorial Republic][Twitter Bootstrap Tutorial from Tutorial Republic]</span></span>
* <span data-ttu-id="b4d59-324">[hello rendszerindítási Playground][hello Bootstrap Playground]</span><span class="sxs-lookup"><span data-stu-id="b4d59-324">[hello Bootstrap Playground][hello Bootstrap Playground]</span></span>
* <span data-ttu-id="b4d59-325">[W3C javaslat mobil webes alkalmazás gyakorlati tanácsok][W3C Recommendation Mobile Web Application Best Practices]</span><span class="sxs-lookup"><span data-stu-id="b4d59-325">[W3C Recommendation Mobile Web Application Best Practices][W3C Recommendation Mobile Web Application Best Practices]</span></span>
* <span data-ttu-id="b4d59-326">[W3C jelölt javaslat media lekérdezések][W3C Candidate Recommendation for media queries]</span><span class="sxs-lookup"><span data-stu-id="b4d59-326">[W3C Candidate Recommendation for media queries][W3C Candidate Recommendation for media queries]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="b4d59-327">A változások</span><span class="sxs-lookup"><span data-stu-id="b4d59-327">What's changed</span></span>
* <span data-ttu-id="b4d59-328">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b4d59-328">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- Internal Links -->
[Deploy hello starter project tooan Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override hello Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve hello Speakers List]: #bkmk_Improvespeakerslist
[Improve hello Tags List]: #bkmk_improvetags
[Improve hello Dates List]: #bkmk_improvedates
[Improve hello SessionsTable View]: #bkmk_improvesessionstable
[Improve hello SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[hello Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png


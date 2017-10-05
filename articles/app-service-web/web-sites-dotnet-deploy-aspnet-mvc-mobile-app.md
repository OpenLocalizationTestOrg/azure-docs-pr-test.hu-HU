---
title: "Az ASP.NET MVC 5 mobil webalkalmazás az Azure App Service telepítése"
description: "Ez az oktatóanyag útmutatást ad meg a webes alkalmazás telepítése az Azure App Service ASP.NET MVC 5 webalkalmazás mobil szolgáltatásainak használata."
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
ms.openlocfilehash: c98e9b485c52a82e5be5c0f6b0b67912d1e890b9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-an-aspnet-mvc-5-mobile-web-app-in-azure-app-service"></a><span data-ttu-id="6c244-103">Az ASP.NET MVC 5 mobil webalkalmazás az Azure App Service telepítése</span><span class="sxs-lookup"><span data-stu-id="6c244-103">Deploy an ASP.NET MVC 5 mobile web app in Azure App Service</span></span>
<span data-ttu-id="6c244-104">Ez az oktatóanyag fog tartalmazza az ASP.NET MVC 5 webes alkalmazást mobilbarát, és központilag telepítenie kell az Azure App Service alapjait.</span><span class="sxs-lookup"><span data-stu-id="6c244-104">This tutorial will teach you the basics of how to build an ASP.NET MVC 5 web app that is mobile-friendly and deploy it to Azure App Service.</span></span> <span data-ttu-id="6c244-105">Ebben az oktatóanyagban kell [Visual Studio Express 2013 for Web alkalmazásokat] [ Visual Studio Express 2013] , vagy ha már rendelkezik, amely a Visual Studio professional kiadása.</span><span class="sxs-lookup"><span data-stu-id="6c244-105">For this tutorial, you need [Visual Studio Express 2013 for Web][Visual Studio Express 2013] or the professional edition of Visual Studio if you already have that.</span></span> <span data-ttu-id="6c244-106">Használhat [Visual Studio 2015] , de a képernyőképek eltérőek lesznek, és az ASP.NET 4.x sablonok kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6c244-106">You can use [Visual Studio 2015] but the screen shots will be different and you must use the ASP.NET 4.x templates.</span></span>

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## <a name="what-youll-build"></a><span data-ttu-id="6c244-107">Mit kell összeállítása</span><span class="sxs-lookup"><span data-stu-id="6c244-107">What You'll Build</span></span>
<span data-ttu-id="6c244-108">Ebben az oktatóanyagban az egyszerű konferencia-listát-alkalmazáshoz megadott mobil szolgáltatásokat fogja hozzáadni a [alapszintű projekt][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="6c244-108">For this tutorial, you'll add mobile features to the simple conference-listing application that's provided in the [starter project][StarterProject].</span></span> <span data-ttu-id="6c244-109">A böngésző-emulátor az Internet Explorer 11 F12 fejlesztői eszközök látható módon, akkor az alábbi képernyőfelvételen látható az ASP.NET munkamenet a kész alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6c244-109">The following screenshot shows the ASP.NET sessions in the completed application, as seen in the browser emulator in Internet Explorer 11 F12 developer tools.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="6c244-110">Az Internet Explorer 11 F12 fejlesztői eszközök és a [Fiddler eszköz] [ Fiddler] érdekében, hogy az alkalmazás hibakeresését.</span><span class="sxs-lookup"><span data-stu-id="6c244-110">You can use the Internet Explorer 11 F12 developer tools and the [Fiddler tool][Fiddler] to help debug your application.</span></span> 

## <a name="skills-youll-learn"></a><span data-ttu-id="6c244-111">Megtudhatja, képességek</span><span class="sxs-lookup"><span data-stu-id="6c244-111">Skills You'll Learn</span></span>
<span data-ttu-id="6c244-112">Az itt ismertetett témák:</span><span class="sxs-lookup"><span data-stu-id="6c244-112">Here's what you'll learn:</span></span>

* <span data-ttu-id="6c244-113">Hogyan használhatja a Visual Studio 2013 a webes alkalmazás közvetlenül a webalkalmazás közzététele az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="6c244-113">How to use Visual Studio 2013 to publish your web application directly to a web app in Azure App Service.</span></span>
* <span data-ttu-id="6c244-114">Az ASP.NET MVC 5 sablonok használatát a CSS rendszerindítási keretrendszer mobileszközökön megjelenítési javítása érdekében</span><span class="sxs-lookup"><span data-stu-id="6c244-114">How the ASP.NET MVC 5 templates use the CSS Bootstrap framework to improve display on mobile devices</span></span>
* <span data-ttu-id="6c244-115">A célként megadott mobilböngészők, például az iPhone- és Android mobilalkalmazás-specifikus nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c244-115">How to create mobile-specific views to target specific mobile browsers, such as the iPhone and Android</span></span>
* <span data-ttu-id="6c244-116">Rugalmas nézetek (nézetek, amelyek válaszolnak az eszközön a különböző böngészők) létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c244-116">How to create responsive views (views that respond to different browsers across devices)</span></span>

## <a name="set-up-the-development-environment"></a><span data-ttu-id="6c244-117">A fejlesztési környezet kialakítása</span><span class="sxs-lookup"><span data-stu-id="6c244-117">Set up the development environment</span></span>
<span data-ttu-id="6c244-118">Állítsa be a fejlesztési környezetet telepítésével, az Azure SDK for .NET 2.5.1-es vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6c244-118">Set up your development environment by installing the Azure SDK for .NET 2.5.1 or later.</span></span> 

1. <span data-ttu-id="6c244-119">.NET-keretrendszerhez készült Azure SDK telepítéséhez kattintson az alábbi hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6c244-119">To install the Azure SDK for .NET, click the link below.</span></span> <span data-ttu-id="6c244-120">Ha nincs még telepítve a Visual Studio 2013, akkor a kapcsolat által telepíti.</span><span class="sxs-lookup"><span data-stu-id="6c244-120">If you don't have Visual Studio 2013 installed yet, it will be installed by the link.</span></span> <span data-ttu-id="6c244-121">Ez az oktatóanyag a Visual Studio 2013 van szükség.</span><span class="sxs-lookup"><span data-stu-id="6c244-121">This tutorial requires Visual Studio 2013.</span></span> <span data-ttu-id="6c244-122">[Az Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span><span class="sxs-lookup"><span data-stu-id="6c244-122">[Azure SDK for Visual Studio 2013][AzureSDKVs2013]</span></span>
2. <span data-ttu-id="6c244-123">Kattintson a Webplatform-telepítő ablakban **telepítése** és a telepítés folytatásához.</span><span class="sxs-lookup"><span data-stu-id="6c244-123">In the Web Platform Installer window, click **Install** and proceed with the installation.</span></span>

<span data-ttu-id="6c244-124">Egy mobil böngésző emulátor is szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="6c244-124">You will also need a mobile browser emulator.</span></span> <span data-ttu-id="6c244-125">Az alábbi működnek:</span><span class="sxs-lookup"><span data-stu-id="6c244-125">Any of the following will work:</span></span>

* <span data-ttu-id="6c244-126">A böngésző emulátor [Internet Explorer 11 F12 fejlesztői eszközök] [ EmulatorIE11] (az összes mobil böngésző képernyőképeket szerepel).</span><span class="sxs-lookup"><span data-stu-id="6c244-126">Browser Emulator in [Internet Explorer 11 F12 developer tools][EmulatorIE11] (used in all mobile browser screenshots).</span></span> <span data-ttu-id="6c244-127">A Windows Phone 8, Windows Phone 7 és Apple iPad felhasználói ügynök karakterlánca készletek rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6c244-127">It has user agent string presets for Windows Phone 8, Windows Phone 7, and Apple iPad.</span></span>
* <span data-ttu-id="6c244-128">A böngésző emulátor [Google Chrome DevTools][EmulatorChrome].</span><span class="sxs-lookup"><span data-stu-id="6c244-128">Browser Emulator in [Google Chrome DevTools][EmulatorChrome].</span></span> <span data-ttu-id="6c244-129">Számos Android-eszközök, valamint Apple iPhone, Apple iPad és Amazon Kindle tűz készletet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6c244-129">It contains presets for numerous Android devices, as well as Apple iPhone, Apple iPad, and Amazon Kindle Fire.</span></span> <span data-ttu-id="6c244-130">Azt is emulálja touch események.</span><span class="sxs-lookup"><span data-stu-id="6c244-130">It also emulates touch events.</span></span>
* <span data-ttu-id="6c244-131">[Opera Mobilemulátoron][EmulatorOpera]</span><span class="sxs-lookup"><span data-stu-id="6c244-131">[Opera Mobile Emulator][EmulatorOpera]</span></span>

<span data-ttu-id="6c244-132">A Visual Studio-projektek C\# forráskód érhetők el ebben a témakörben kísérő:</span><span class="sxs-lookup"><span data-stu-id="6c244-132">Visual Studio projects with C\# source code are available to accompany this topic:</span></span>

* <span data-ttu-id="6c244-133">[Alapszintű projekt letöltése][StarterProject]</span><span class="sxs-lookup"><span data-stu-id="6c244-133">[Starter project download][StarterProject]</span></span>
* <span data-ttu-id="6c244-134">[Projekt letöltése befejeződött][CompletedProject]</span><span class="sxs-lookup"><span data-stu-id="6c244-134">[Completed project download][CompletedProject]</span></span>

## <span data-ttu-id="6c244-135"><a name="bkmk_DeployStarterProject"></a>Az alapszintű projekt telepítése az Azure-webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="6c244-135"><a name="bkmk_DeployStarterProject"></a>Deploy the starter project to an Azure web app</span></span>
1. <span data-ttu-id="6c244-136">Töltse le a konferencia-listát alkalmazás [alapszintű projekt][StarterProject].</span><span class="sxs-lookup"><span data-stu-id="6c244-136">Download the conference-listing application [starter project][StarterProject].</span></span>
2. <span data-ttu-id="6c244-137">Ezután a Windows Intézőt, kattintson a jobb gombbal a letöltött ZIP-fájl, és válassza a *tulajdonságok*.</span><span class="sxs-lookup"><span data-stu-id="6c244-137">Then in Windows Explorer, right-click the downloaded ZIP file and choose *Properties*.</span></span>
3. <span data-ttu-id="6c244-138">Az a **tulajdonságok** párbeszédpanelen válassza ki a **Unblock** gombra.</span><span class="sxs-lookup"><span data-stu-id="6c244-138">In the **Properties** dialog box, choose the **Unblock** button.</span></span> <span data-ttu-id="6c244-139">(Letiltásuk feloldására, amely akkor fordul elő, amikor próbálja használni a biztonsági figyelmeztetést megakadályozza, hogy egy *.zip* az internetről letöltött fájl.)</span><span class="sxs-lookup"><span data-stu-id="6c244-139">(Unblocking prevents a security warning that occurs when you try to use a *.zip* file that you've downloaded from the web.)</span></span>
4. <span data-ttu-id="6c244-140">Kattintson a jobb gombbal a ZIP-fájl, és válassza ki **összes kibontása** és csomagolja ki a fájlt.</span><span class="sxs-lookup"><span data-stu-id="6c244-140">Right-click the ZIP file and select **Extract All** to unzip the file.</span></span> 
5. <span data-ttu-id="6c244-141">A Visual Studióban nyissa meg a *C#\Mvc5Mobile.sln* fájlt.</span><span class="sxs-lookup"><span data-stu-id="6c244-141">In Visual Studio, open the *C#\Mvc5Mobile.sln* file.</span></span>
6. <span data-ttu-id="6c244-142">A Megoldáskezelőben kattintson a jobb gombbal a projektre, és kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="6c244-142">In Solution Explorer, right-click the project and click **Publish**.</span></span>
   
   ![][DeployClickPublish]
7. <span data-ttu-id="6c244-143">A webhely közzététele kattintson **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="6c244-143">In Publish Web, click **Microsoft Azure App Service**.</span></span>
   
   ![][DeployClickWebSites]
8. <span data-ttu-id="6c244-144">Ha még nem jelentkezett be Azure, kattintson a **vegyen fel egy fiókot**.</span><span class="sxs-lookup"><span data-stu-id="6c244-144">If you haven't already logged into Azure, click **Add an account**.</span></span>
   
   ![][DeploySignIn]
9. <span data-ttu-id="6c244-145">Kövesse a megjelenő utasításokat az Azure-fiók be tudjon jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="6c244-145">Follow the prompts to log into your Azure account.</span></span>
10. <span data-ttu-id="6c244-146">Az App Service párbeszédpanelen most meg kell jelennie, aláírt.</span><span class="sxs-lookup"><span data-stu-id="6c244-146">The App Service dialog should now show you as signed in.</span></span> <span data-ttu-id="6c244-147">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6c244-147">Click **New**.</span></span>
    
    ![][DeployNewWebsite]  
11. <span data-ttu-id="6c244-148">Az a **webalkalmazásnév** mezőben adja meg egy egyedi alkalmazás nevének előtagja.</span><span class="sxs-lookup"><span data-stu-id="6c244-148">In the **Web App Name** field, specify a unique app name prefix.</span></span> <span data-ttu-id="6c244-149">A webalkalmazás teljesen minősített neve lesz  *&lt;előtag >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="6c244-149">Your fully-qualified web app name will be *&lt;prefix>*.azurewebsites.net.</span></span> <span data-ttu-id="6c244-150">Válassza ki vagy adjon meg egy új erőforráscsoport neve az is, **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="6c244-150">Also, select or specify a new resource group name in **Resource group**.</span></span> <span data-ttu-id="6c244-151">Kattintson a **új** egy új App Service-csomag létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6c244-151">Then, click **New** to create a new App Service plan.</span></span>
    
    ![][DeploySiteSettings]
12. <span data-ttu-id="6c244-152">Az új App Service-csomag konfigurálása és **OK**.</span><span class="sxs-lookup"><span data-stu-id="6c244-152">Configure the new App Service plan and click **OK**.</span></span> 
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7a.png)
13. <span data-ttu-id="6c244-153">Az App Service létrehozása párbeszédpanel, kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6c244-153">Back in the Create App Service dialog, click **Create**.</span></span>
    
    ![](./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7b.png) 
14. <span data-ttu-id="6c244-154">Miután az Azure erőforrások jönnek létre, a webhely közzététele párbeszédpanelen beírja a beállításokat az új alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6c244-154">After the Azure resources are created, the Publish Web dialog will be filled with the settings for your new app.</span></span> <span data-ttu-id="6c244-155">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="6c244-155">Click **Publish**.</span></span>
    
    ![][DeployPublishSite]
    
    <span data-ttu-id="6c244-156">Visual Studio befejezi a alapszintű projekt közzététele az Azure web app, ha az asztali böngésző megnyitja jelenítheti meg az élő webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6c244-156">Once Visual Studio finishes publishing the starter project to the Azure web app, the desktop browser opens to display the live web app.</span></span>
15. <span data-ttu-id="6c244-157">Indítsa el a mobil böngésző emulátor, és másolja az URL-címet a konferencia alkalmazás (*<prefix>*. azurewebsites.net) azokat az emulátorban, majd kattintson a jobb felső gombra, és válassza ki **kódcímke Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="6c244-157">Start your mobile browser emulator, copy the URL for the conference application (*<prefix>*.azurewebsites.net) into the emulator, and then click the top-right button and select **Browse by tag**.</span></span> <span data-ttu-id="6c244-158">Ha az alapértelmezett böngésző használ az Internet Explorer 11, csupán be kell beírnia `F12`, majd `Ctrl+8`, majd állítsa át a böngésző-profilhoz, amellyel **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="6c244-158">If you are using Internet Explorer 11 as the default browser, you just need to type `F12`, then `Ctrl+8`, and then change the browser profile to **Windows Phone**.</span></span> <span data-ttu-id="6c244-159">Az alábbi kép a *AllTags* nézet álló módban (válasszon **kódcímke Tallózás**).</span><span class="sxs-lookup"><span data-stu-id="6c244-159">The image below shows the *AllTags* view in portrait mode (from choosing **Browse by tag**).</span></span>
    
    ![][AllTags]

> [!TIP]
> <span data-ttu-id="6c244-160">Visual Studio az MVC 5 alkalmazás megoldhassuk, amíg az Azure-bA ismét ellenőrizze az élő webalkalmazását-ről a mobil böngésző vagy a böngésző emulátor közzéteheti a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6c244-160">While you can debug your MVC 5 application from within Visual Studio, you can publish your web app to Azure again to verify the live web app directly from your mobile browser or a browser emulator.</span></span>
> 
> 

<span data-ttu-id="6c244-161">A megjelenített akkor nagyon olvasható mobil eszközön.</span><span class="sxs-lookup"><span data-stu-id="6c244-161">The display is very readable on a mobile device.</span></span> <span data-ttu-id="6c244-162">A vizuális hatások a rendszerindítási CSS-keretrendszer által alkalmazott némelyike már is látható.</span><span class="sxs-lookup"><span data-stu-id="6c244-162">You can also already see some of the visual effects applied by the Bootstrap CSS framework.</span></span>
<span data-ttu-id="6c244-163">Kattintson a **ASP.NET** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6c244-163">Click the **ASP.NET** link.</span></span>

![][SessionsByTagASP.NET]

<span data-ttu-id="6c244-164">Az ASP.NET címke nézet Nagyítás felszerelt meg automatikusan elvégzi a rendszerindítás, a képernyőhöz.</span><span class="sxs-lookup"><span data-stu-id="6c244-164">The ASP.NET tag view is zoom-fitted to the screen, which Bootstrap does for you automatically.</span></span> <span data-ttu-id="6c244-165">Azonban ez a nézet jobban megfeleljenek a mobil böngésző javíthatja.</span><span class="sxs-lookup"><span data-stu-id="6c244-165">However, you can improve this view to better suit the mobile browser.</span></span> <span data-ttu-id="6c244-166">Például a **dátum** oszlop nehezen olvasható.</span><span class="sxs-lookup"><span data-stu-id="6c244-166">For example, the **Date** column is difficult to read.</span></span> <span data-ttu-id="6c244-167">Az oktatóanyag későbbi részében fogja módosítani a *AllTags* abba, hogy mobilbarát nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-167">Later in the tutorial you'll change the *AllTags* view to make it mobile-friendly.</span></span>

## <span data-ttu-id="6c244-168"><a name="bkmk_bootstrap"></a>A rendszerindítási CSS-keretrendszer</span><span class="sxs-lookup"><span data-stu-id="6c244-168"><a name="bkmk_bootstrap"></a> Bootstrap CSS Framework</span></span>
<span data-ttu-id="6c244-169">Új az MVC 5 sablon rendszer beépített rendszer-indításkori támogatja.</span><span class="sxs-lookup"><span data-stu-id="6c244-169">New in the MVC 5 template is built-in Bootstrap support.</span></span> <span data-ttu-id="6c244-170">Hogyan azonnal javítja a különböző nézetek az alkalmazás már láthatta.</span><span class="sxs-lookup"><span data-stu-id="6c244-170">You have already seen how it immediately improves the different views in your application.</span></span> <span data-ttu-id="6c244-171">Például a felső navigációs sáv esetén automatikusan összecsukható a böngésző szélesség kisebb.</span><span class="sxs-lookup"><span data-stu-id="6c244-171">For example, the navigation bar at the top is automatically collapsible when the browser width is smaller.</span></span> <span data-ttu-id="6c244-172">Az asztali böngészőn próbálkozzon a böngészőablak átméretezése, és hogyan a navigációs sáv változása a Megjelenés és működés.</span><span class="sxs-lookup"><span data-stu-id="6c244-172">On the desktop browser, try resizing the browser window and see how the navigation bar changes its look and feel.</span></span> <span data-ttu-id="6c244-173">Ez a rendszerindítási beépített rugalmas Webtervezés.</span><span class="sxs-lookup"><span data-stu-id="6c244-173">This is the responsive web design that is built into Bootstrap.</span></span>

<span data-ttu-id="6c244-174">Hogyan a webes alkalmazás nélkül rendszerindítási lenne megtekintéséhez nyissa meg a *App\_Start\\BundleConfig.cs* és tartalmazó sorok megjegyzésbe *bootstrap.js* és *bootstrap.css*.</span><span class="sxs-lookup"><span data-stu-id="6c244-174">To see how the Web app would look without Bootstrap, open *App\_Start\\BundleConfig.cs* and comment out the lines that contain *bootstrap.js* and *bootstrap.css*.</span></span> <span data-ttu-id="6c244-175">A következő kód bemutatja az utolsó két állapotkimutatások a `RegisterBundles` metódus a módosítás után:</span><span class="sxs-lookup"><span data-stu-id="6c244-175">The following code shows the last two statements of the `RegisterBundles` method after the change:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

<span data-ttu-id="6c244-176">Nyomja le az `Ctrl+F5` az alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="6c244-176">Press `Ctrl+F5` to run the application.</span></span>

<span data-ttu-id="6c244-177">Figyelje meg, hogy a összecsukható navigációs sáv jelenleg csak egy szokásos rendezetlen lista.</span><span class="sxs-lookup"><span data-stu-id="6c244-177">Observe that the collapsible navigation bar is now just an ordinary unordered list.</span></span> <span data-ttu-id="6c244-178">Kattintson a **kódcímke Tallózás** újra, majd kattintson a **ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6c244-178">Click **Browse by tag** again, then click **ASP.NET**.</span></span>
<span data-ttu-id="6c244-179">A mobilemulátoron nézetben tekintheti meg most, hogy már nincs nagyítás felszerelt a képernyőre, és kell ahhoz, hogy a tábla jobb oldalán talál oldalirányba görgessen.</span><span class="sxs-lookup"><span data-stu-id="6c244-179">In the mobile emulator view, you can see now that it is no longer zoom-fitted to the screen, and you must scroll sideways in order to see the right side of the table.</span></span>

![][SessionsByTagASP.NETNoBootstrap]

<span data-ttu-id="6c244-180">Visszavonja a módosításokat, és frissítse a mobil böngészőablakot győződjön meg arról, hogy a mobilbarát megjelenítési helyre lett állítva.</span><span class="sxs-lookup"><span data-stu-id="6c244-180">Undo your changes and refresh the mobile browser to verify that the mobile-friendly display has been restored.</span></span>

<span data-ttu-id="6c244-181">Rendszerindítási nem csak az ASP.NET MVC 5, és veheti hasznát ezeket a szolgáltatásokat bármely webalkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6c244-181">Bootstrap is not specific to ASP.NET MVC 5, and you can take advantage of these features in any web application.</span></span> <span data-ttu-id="6c244-182">De most épített ASP.NET MVC 5 projektsablon, így az MVC 5-webalkalmazás kihasználhatja a rendszerindítás alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="6c244-182">But it is now built into the ASP.NET MVC 5 project template, so that your MVC 5 Web application can take advantage of Bootstrap by default.</span></span>

<span data-ttu-id="6c244-183">Rendszerindítási kapcsolatos további információkért látogasson el a [rendszerindítási] [ BootstrapSite] hely.</span><span class="sxs-lookup"><span data-stu-id="6c244-183">For more information about Bootstrap, go to the [Bootstrap][BootstrapSite] site.</span></span>

<span data-ttu-id="6c244-184">A következő szakaszban láthatja, hogyan mobile-böngésző meghatározott nézetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="6c244-184">In the next section you'll see how to provide mobile-browser specific views.</span></span>

## <span data-ttu-id="6c244-185"><a name="bkmk_overrideviews"></a>A nézetek, elrendezés és részleges nézetek felülbírálása</span><span class="sxs-lookup"><span data-stu-id="6c244-185"><a name="bkmk_overrideviews"></a> Override the Views, Layouts, and Partial Views</span></span>
<span data-ttu-id="6c244-186">Általában az egyes mobil böngésző vagy adott böngészők mobilböngészők (beleértve a elrendezések és a részleges nézetek) nézeten felülbírálható.</span><span class="sxs-lookup"><span data-stu-id="6c244-186">You can override any view (including layouts and partial views) for mobile browsers in general, for an individual mobile browser, or for any specific browser.</span></span> <span data-ttu-id="6c244-187">Mobileszköz-specifikus megjelenítésére szolgáló, nézet fájl másolása és hozzáadása *. Mobile* fájlneve.</span><span class="sxs-lookup"><span data-stu-id="6c244-187">To provide a mobile-specific view, you can copy a view file and add *.Mobile* to the file name.</span></span> <span data-ttu-id="6c244-188">Ahhoz például, hogy hozzon létre egy mobile *Index* másolhatja nézet, *nézetek\\Home\\Index.cshtml* való *nézetek\\Home\\Index.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c244-188">For example, to create a mobile *Index* view, you can copy *Views\\Home\\Index.cshtml* to *Views\\Home\\Index.Mobile.cshtml*.</span></span>

<span data-ttu-id="6c244-189">Ebben a szakaszban létre fog hozni egy mobileszköz-specifikus fájlt.</span><span class="sxs-lookup"><span data-stu-id="6c244-189">In this section, you'll create a mobile-specific layout file.</span></span>

<span data-ttu-id="6c244-190">Elindításához másolása *nézetek\\megosztott\\\_Layout.cshtml* való *nézetek\\megosztott\\\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c244-190">To start, copy *Views\\Shared\\\_Layout.cshtml* to *Views\\Shared\\\_Layout.Mobile.cshtml*.</span></span> <span data-ttu-id="6c244-191">Nyissa meg  *\_Layout.Mobile.cshtml* , és módosítsa a cím és **MVC5 alkalmazás** való **MVC5-alkalmazás (mobil)**.</span><span class="sxs-lookup"><span data-stu-id="6c244-191">Open *\_Layout.Mobile.cshtml* and change the title from **MVC5 Application** to **MVC5 Application (Mobile)**.</span></span>

<span data-ttu-id="6c244-192">Az egyes `Html.ActionLink` a navigációs sáv hívja, akkor távolítsa el a "Tallózás alapja" minden hivatkozás *ActionLink*.</span><span class="sxs-lookup"><span data-stu-id="6c244-192">In each `Html.ActionLink` call for the navigation bar, remove "Browse by" in each link *ActionLink*.</span></span> <span data-ttu-id="6c244-193">A következő kód bemutatja a befejezett `<ul class="nav navbar-nav">` címkéjének a mobil fájlt.</span><span class="sxs-lookup"><span data-stu-id="6c244-193">The following code shows the completed `<ul class="nav navbar-nav">` tag of the mobile layout file.</span></span>

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

<span data-ttu-id="6c244-194">Másolás a *nézetek\\Home\\AllTags.cshtml* fájl *nézetek\\Home\\AllTags.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c244-194">Copy the *Views\\Home\\AllTags.cshtml* file to *Views\\Home\\AllTags.Mobile.cshtml*.</span></span> <span data-ttu-id="6c244-195">Nyissa meg az új fájlt, és módosítsa a `<h2>` elem "Címkék" a "címkék (M)":</span><span class="sxs-lookup"><span data-stu-id="6c244-195">Open the new file and change the `<h2>` element from "Tags" to "Tags (M)":</span></span>

    <h2>Tags (M)</h2>

<span data-ttu-id="6c244-196">Tallózással keresse meg a Címkék lap egy asztali böngészőt használ, és a mobil böngésző emulator használatával.</span><span class="sxs-lookup"><span data-stu-id="6c244-196">Browse to the tags page using a desktop browser and using mobile browser emulator.</span></span> <span data-ttu-id="6c244-197">A mobil böngésző emulátor mutatja a két végrehajtott (a cím és  *\_Layout.Mobile.cshtml* és a cím és *AllTags.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c244-197">The mobile browser emulator shows the two changes you made (the title from *\_Layout.Mobile.cshtml* and the title from *AllTags.Mobile.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobile]

<span data-ttu-id="6c244-198">Ezzel szemben az asztal nem változott (a címei  *\_Layout.cshtml* és *AllTags.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c244-198">In contrast, the desktop display has not changed (with titles from *\_Layout.cshtml* and *AllTags.cshtml*).</span></span>

![][AllTagsMobile_LayoutMobileDesktop]

## <span data-ttu-id="6c244-199"><a name="bkmk_browserviews"></a>Böngésző-specifikus nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c244-199"><a name="bkmk_browserviews"></a> Create Browser-Specific Views</span></span>
<span data-ttu-id="6c244-200">Mobileszköz- és asztali-specifikus nézetek mellett egy egyedi böngésző nézeteket is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="6c244-200">In addition to mobile-specific and desktop-specific views, you can create views for an individual browser.</span></span> <span data-ttu-id="6c244-201">Például kimondottan az iPhone vagy az Android böngészőhöz nézeteket is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="6c244-201">For example, you can create views that are specifically for the iPhone or the Android browser.</span></span> <span data-ttu-id="6c244-202">Ebben a szakaszban létrehozunk egy elrendezést a iPhone-böngésző és egy iPhone verzióját a *AllTags* nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-202">In this section, you'll create a layout for the iPhone browser and an iPhone version of the *AllTags* view.</span></span>

<span data-ttu-id="6c244-203">Nyissa meg a *Global.asax* fájlt, és adja hozzá a következő kódot alján a `Application_Start` metódust.</span><span class="sxs-lookup"><span data-stu-id="6c244-203">Open the *Global.asax* file and add the following code to the bottom of the `Application_Start` method.</span></span>

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

<span data-ttu-id="6c244-204">Ez a kód határozza meg egy új, "iPhone", amely az egyes bejövő kérelmek elleni megfeleltetésének nevű megjelenítési mód.</span><span class="sxs-lookup"><span data-stu-id="6c244-204">This code defines a new display mode named "iPhone" that will be matched against each incoming request.</span></span> <span data-ttu-id="6c244-205">Ha a bejövő kérelem megfelel a következő feltételt (Ha ez azt jelenti, hogy a felhasználói ügynök tartalmazza a karakterláncot "iPhone") megadott, az ASP.NET MVC nézetek, amelynek a neve tartalmazza a "iPhone" utótag fogja keresni.</span><span class="sxs-lookup"><span data-stu-id="6c244-205">If the incoming request matches the condition you defined (that is, if the user agent contains the string "iPhone"), ASP.NET MVC will look for views whose name contains the "iPhone" suffix.</span></span>

> [!NOTE]
> <span data-ttu-id="6c244-206">Mobil böngésző-specifikus megjelenítési módok való hozzáadásakor, például iPhone-és Android, ügyeljen arra, hogy az első argumentum beállítása `0` (szúrja be a lista elején) Győződjön meg arról, hogy a böngésző-specifikus mód elsőbbséget élvez a mobil sablon (*. Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="6c244-206">When adding mobile browser-specific display modes, such as for iPhone and Android, be sure to set the first argument to `0` (insert at the top of the list) to make sure that the browser-specific mode takes precedence over the mobile template (*.Mobile.cshtml).</span></span> <span data-ttu-id="6c244-207">Ha a mobil sablon a lista tetején helyette, akkor lesz kiválasztva a kívánt megjelenítési mód (az első egyező wins, és a mobil sablon megegyezik a hordozható böngészők).</span><span class="sxs-lookup"><span data-stu-id="6c244-207">If the mobile template is at the top of the list instead, it will be selected over your intended display mode (the first match wins, and the mobile template matches all mobile browsers).</span></span> 
> 
> 

<span data-ttu-id="6c244-208">A kódban, kattintson a jobb gombbal `DefaultDisplayMode`, válassza a **megoldásához**, és válassza a `using System.Web.WebPages;`.</span><span class="sxs-lookup"><span data-stu-id="6c244-208">In the code, right-click `DefaultDisplayMode`, choose **Resolve**, and then choose `using System.Web.WebPages;`.</span></span> <span data-ttu-id="6c244-209">Ez hozzáad egy hivatkozást a `System.Web.WebPages` névtér, amely akkor, ha a `DisplayModeProvider` és `DefaultDisplayMode` van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="6c244-209">This adds a reference to the `System.Web.WebPages` namespace, which is where the `DisplayModeProvider` and `DefaultDisplayMode` types are defined.</span></span>

![][ResolveDefaultDisplayMode]

<span data-ttu-id="6c244-210">Másik lehetőségként csak manuálisan adhat hozzá a következő sort a `using` a fájl a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6c244-210">Alternatively, you can just manually add the following line to the `using` section of the file.</span></span>

    using System.Web.WebPages;

<span data-ttu-id="6c244-211">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="6c244-211">Save the changes.</span></span> <span data-ttu-id="6c244-212">Másolás a *nézetek\\megosztott\\\_Layout.Mobile.cshtml* fájl *nézetek\\megosztott\\\_Layout.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c244-212">Copy the *Views\\Shared\\\_Layout.Mobile.cshtml* file to *Views\\Shared\\\_Layout.iPhone.cshtml*.</span></span> <span data-ttu-id="6c244-213">Nyissa meg az új fájlt, és módosítsa a címét, majd `MVC5 Application (Mobile)` való `MVC5 Application (iPhone)`.</span><span class="sxs-lookup"><span data-stu-id="6c244-213">Open the new file and then change the title from `MVC5 Application (Mobile)` to `MVC5 Application (iPhone)`.</span></span>

<span data-ttu-id="6c244-214">Másolás a *nézetek\\Home\\AllTags.Mobile.cshtml* fájl *nézetek\\Home\\AllTags.iPhone.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c244-214">Copy the *Views\\Home\\AllTags.Mobile.cshtml* file to *Views\\Home\\AllTags.iPhone.cshtml*.</span></span> <span data-ttu-id="6c244-215">Az új fájl, módosítsa a `<h2>` "Címkék (iPhone)" a "címkék (M)" elemet.</span><span class="sxs-lookup"><span data-stu-id="6c244-215">In the new file, change the `<h2>` element from "Tags (M)" to "Tags (iPhone)".</span></span>

<span data-ttu-id="6c244-216">Futtassa az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="6c244-216">Run the application.</span></span> <span data-ttu-id="6c244-217">Futtassa a mobil böngésző emulátor, győződjön meg arról, hogy a felhasználói ügynök "iPhone" értékre van állítva, és keresse meg a *AllTags* nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-217">Run a mobile browser emulator, make sure its user agent is set to "iPhone", and browse to the *AllTags* view.</span></span> <span data-ttu-id="6c244-218">Az emulátor használatakor az Internet Explorer 11 F12 fejlesztői eszközök konfigurálása emuláció a következőhöz:</span><span class="sxs-lookup"><span data-stu-id="6c244-218">If you are using the emulator in Internet Explorer 11 F12 developer tools, configure emulation to the following:</span></span>

* <span data-ttu-id="6c244-219">Böngésző profil = **Windows Phone**</span><span class="sxs-lookup"><span data-stu-id="6c244-219">Browser profile = **Windows Phone**</span></span>
* <span data-ttu-id="6c244-220">Felhasználói ügynök karakterlánca = **egyéni**</span><span class="sxs-lookup"><span data-stu-id="6c244-220">User agent string =  **Custom**</span></span>
* <span data-ttu-id="6c244-221">Egyéni karakterláncot = **Apple-iPhone5C1/1001.525**</span><span class="sxs-lookup"><span data-stu-id="6c244-221">Custom string = **Apple-iPhone5C1/1001.525**</span></span>

<span data-ttu-id="6c244-222">Az alábbi képernyőfelvételen látható a *AllTags* nézet megjelenítése az emulátorban, az Internet Explorer 11 F12 fejlesztői eszközök, az egyéni felhasználói ügynök karakterlánca (Ez az egy iPhone 5 C felhasználói ügynök karakterlánca).</span><span class="sxs-lookup"><span data-stu-id="6c244-222">The following screenshot shows the *AllTags* view rendered in the emulator in Internet Explorer 11 F12 developer tools with the custom user agent string (this is an iPhone 5C user agent string).</span></span>

![][AllTagsIPhone_LayoutIPhone]

<span data-ttu-id="6c244-223">A mobil böngészőben, válassza ki a **hangszórók** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6c244-223">In the mobile browser, select the **Speakers** link.</span></span> <span data-ttu-id="6c244-224">Mivel nincs mobil nézet (*AllSpeakers.Mobile.cshtml*), az alapértelmezett hangszórók megtekintése (*AllSpeakers.cshtml*) használata a mobil nézet megjelenítése (*\_Layout.Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c244-224">Because there's not a mobile view (*AllSpeakers.Mobile.cshtml*), the default speakers view (*AllSpeakers.cshtml*) is rendered using the mobile layout view (*\_Layout.Mobile.cshtml*).</span></span> <span data-ttu-id="6c244-225">Az alábbi módon a cím **MVC5-alkalmazás (mobil)** meghatározott  *\_Layout.Mobile.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6c244-225">As shown below, the title **MVC5 Application (Mobile)** is defined in *\_Layout.Mobile.cshtml*.</span></span>

![][AllSpeakers_LayoutMobile]

<span data-ttu-id="6c244-226">Úgy, hogy a megjelenítési belül mobil elrendezés alapértelmezett (nem mobil) nézet globálisan letilthatja `RequireConsistentDisplayMode` való `true` a a *nézetek\\\_ViewStart.cshtml* fájl ehhez hasonló:</span><span class="sxs-lookup"><span data-stu-id="6c244-226">You can globally disable a default (non-mobile) view from rendering inside a mobile layout by setting `RequireConsistentDisplayMode` to `true` in the *Views\\\_ViewStart.cshtml* file, like this:</span></span>

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

<span data-ttu-id="6c244-227">Ha `RequireConsistentDisplayMode` értéke `true`, a mobil elrendezés (*\_Layout.Mobile.cshtml*) csak mobil nézetek használatos (azaz az űrlap a nézet fájl esetén  ***Nézet_neve**. Mobile.cshtml*).</span><span class="sxs-lookup"><span data-stu-id="6c244-227">When `RequireConsistentDisplayMode` is set to `true`, the mobile layout (*\_Layout.Mobile.cshtml*) is used only for mobile views (i.e. when the view file is of the form ***ViewName**.Mobile.cshtml*).</span></span> <span data-ttu-id="6c244-228">Előfordulhat, hogy szeretné állítani `RequireConsistentDisplayMode` való `true` Ha a mobil elrendezés és a nem mobil nézetek nem működik.</span><span class="sxs-lookup"><span data-stu-id="6c244-228">You might want to set `RequireConsistentDisplayMode` to `true` if your mobile layout doesn't work well with your non-mobile views.</span></span> <span data-ttu-id="6c244-229">Az alábbi képernyőfelvételen az *hangszórók* lap Renderelés mikor `RequireConsistentDisplayMode` értékre van állítva `true` (nélkül a karakterlánc "(mobil)" a felső navigációs sáv).</span><span class="sxs-lookup"><span data-stu-id="6c244-229">The screenshot below shows how the *Speakers* page renders when `RequireConsistentDisplayMode` is set to `true` (without the string "(Mobile)" in the navigational bar at the top).</span></span>

![][AllSpeakers_LayoutMobileOverridden]

<span data-ttu-id="6c244-230">Adott nézetben konzisztens megjelenítési mód beállításával letilthatja `RequireConsistentDisplayMode` való `false` a nézet fájlban.</span><span class="sxs-lookup"><span data-stu-id="6c244-230">You can disable consistent display mode in a specific view by setting `RequireConsistentDisplayMode` to `false` in the view file.</span></span> <span data-ttu-id="6c244-231">A következő kódban a *nézetek\\Home\\AllSpeakers.cshtml* beállítása fájl `RequireConsistentDisplayMode` való `false`:</span><span class="sxs-lookup"><span data-stu-id="6c244-231">The following markup in the *Views\\Home\\AllSpeakers.cshtml* file sets `RequireConsistentDisplayMode` to `false`:</span></span>

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

<span data-ttu-id="6c244-232">Ebben a szakaszban is láttuk mobil elrendezések és nézetek létrehozása és elrendezések és a meghatározott eszközök, például az iPhone nézetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6c244-232">In this section we've seen how to create mobile layouts and views and how to create layouts and views for specific devices such as the iPhone.</span></span>
<span data-ttu-id="6c244-233">Azonban a fő a rendszerindítási CSS-keretrendszer előnye a megfelelően rugalmas elrendezést, ami azt jelenti, hogy egyetlen stíluslap alkalmazható asztali, telefon és táblagép böngészők egy egységes megjelenést és működést létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6c244-233">However, the main advantage of the Bootstrap CSS framework is the responsive layout, which means that a single stylesheet can be applied across desktop, phone, and tablet browsers to create a consistent look and feel.</span></span> <span data-ttu-id="6c244-234">A következő szakaszban láthatja, hogyan használhatók ki a rendszerindítási mobilbarát nézetek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6c244-234">In the next section you'll see how to leverage Bootstrap to create mobile-friendly views.</span></span>

## <span data-ttu-id="6c244-235"><a name="bkmk_Improvespeakerslist"></a>A hangszórók lista javítása</span><span class="sxs-lookup"><span data-stu-id="6c244-235"><a name="bkmk_Improvespeakerslist"></a> Improve the Speakers List</span></span>
<span data-ttu-id="6c244-236">Ekkor csak a böngészőben, a *hangszórók* nézet olvasható, de a hivatkozások kicsi, és nehéz a mobileszköz koppintson.</span><span class="sxs-lookup"><span data-stu-id="6c244-236">As you just saw, the *Speakers* view is readable, but the links are small and are difficult to tap on a mobile device.</span></span> <span data-ttu-id="6c244-237">Ebben a szakaszban meg kell győződnie a *AllSpeakers* mobilbarát, amely nagy, könnyen koppintson mutató hivatkozások megjelennek, és tartalmazza a keresőmezőbe gyorsan hangszórók megtekintése.</span><span class="sxs-lookup"><span data-stu-id="6c244-237">In this section, you'll make the *AllSpeakers* view mobile-friendly, which displays large, easy-to-tap links and contains a search box to quickly find speakers.</span></span>

<span data-ttu-id="6c244-238">Használhatja a rendszerindítási [csatolt csoportja] [ linked list group] stílusbeállításokat javítása érdekében a *hangszórók* nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-238">You can use the Bootstrap [linked list group][linked list group] styling to improve the *Speakers* view.</span></span> <span data-ttu-id="6c244-239">A *nézetek\\Home\\AllSpeakers.cshtml*, cserélje le a Razor fájl tartalmát az alábbi kódra.</span><span class="sxs-lookup"><span data-stu-id="6c244-239">In *Views\\Home\\AllSpeakers.cshtml*, replace the contents of the Razor file with the code below.</span></span>

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

<span data-ttu-id="6c244-240">A `class="list-group"` attribútumnak a `<div>` címke alkalmazza a rendszer-indításkori lista stíluselemekkel és a `class="input-group-item"` attribútum Bootstrap lista elem stílusbeállításokat vonatkozik minden hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="6c244-240">The `class="list-group"` attribute in the `<div>` tag applies the Bootstrap list styling, and the `class="input-group-item"` attribute applies Bootstrap list item styling to each link.</span></span>

<span data-ttu-id="6c244-241">Frissítse a mobil böngészőablakot.</span><span class="sxs-lookup"><span data-stu-id="6c244-241">Refresh the mobile browser.</span></span> <span data-ttu-id="6c244-242">A frissített nézet így néz ki:</span><span class="sxs-lookup"><span data-stu-id="6c244-242">The updated view looks like this:</span></span>

![][AllSpeakersFixed]

<span data-ttu-id="6c244-243">A rendszerindítási [csatolt csoportja] [ linked list group] stílusbeállításokat lehetővé teszi a teljes csoportban kattintható, minden hivatkozás, amely sok jobb felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="6c244-243">The Bootstrap [linked list group][linked list group] styling makes the entire box for each link clickable, which is a much better user experience.</span></span> <span data-ttu-id="6c244-244">Az asztali nézetet, és tekintse meg az egységes megjelenést és működést.</span><span class="sxs-lookup"><span data-stu-id="6c244-244">Switch to the desktop view and observe the consistent look and feel.</span></span>

![][AllSpeakersFixedDesktop]

<span data-ttu-id="6c244-245">Bár a mobil böngésző nézet javult, akkor nehezen hangszórók listája túl hosszú.</span><span class="sxs-lookup"><span data-stu-id="6c244-245">Although the mobile browser view has improved, it's difficult to navigate the long list of speakers.</span></span> <span data-ttu-id="6c244-246">Rendszerindítási nem biztosít egy keresési szűrő funkciót az a-kész, de néhány sornyi kódot is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="6c244-246">Bootstrap doesn't provide a search filter functionality out-of-the-box, but you can add it with a few lines of code.</span></span> <span data-ttu-id="6c244-247">Akkor lesz először a keresőmező a nézethez történő hozzáadáshoz, majd a szűrő függvény JavaScript kóddal a számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="6c244-247">You will first add a search box to the view, then hook up with the JavaScript code for the filter function.</span></span> <span data-ttu-id="6c244-248">A *nézetek\\Home\\AllSpeakers.cshtml*, vegye fel a \<űrlap\> címke után csak a \<h2\> címke, a lent látható módon:</span><span class="sxs-lookup"><span data-stu-id="6c244-248">In *Views\\Home\\AllSpeakers.cshtml*, add a \<form\> tag just after the \<h2\> tag, as shown below:</span></span>

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

<span data-ttu-id="6c244-249">Figyelje meg, hogy a `<form>` és `<input>` mindkét címkék rendelkezik vonatkoznak annak biztosítása érdekében a rendszer-indításkori stílusok.</span><span class="sxs-lookup"><span data-stu-id="6c244-249">Notice that the `<form>` and `<input>` tags both have the Bootstrap styles applied to them.</span></span> <span data-ttu-id="6c244-250">A `<span>` elem hozzáadása a rendszerindítási [glyphicon] [ glyphicon] a keresési mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6c244-250">The `<span>` element adds a Bootstrap [glyphicon][glyphicon] to the search box.</span></span>

<span data-ttu-id="6c244-251">Az a *parancsfájlok* mappát, adja hozzá a JavaScript-fájl neve *filter.js*.</span><span class="sxs-lookup"><span data-stu-id="6c244-251">In the *Scripts* folder, add a JavaScript file called *filter.js*.</span></span> <span data-ttu-id="6c244-252">Nyissa meg a fájlt, és illessze be az alábbi:</span><span class="sxs-lookup"><span data-stu-id="6c244-252">Open the file and paste the following code into it:</span></span>

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
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

<span data-ttu-id="6c244-253">Szükség filter.js szerepeljenek a regisztrált csomagokat.</span><span class="sxs-lookup"><span data-stu-id="6c244-253">You also need to include filter.js in your registered bundles.</span></span> <span data-ttu-id="6c244-254">Nyissa meg *App\_Start\\BundleConfig.cs* , és módosítsa az első csomagokat.</span><span class="sxs-lookup"><span data-stu-id="6c244-254">Open *App\_Start\\BundleConfig.cs* and change the first bundles.</span></span> <span data-ttu-id="6c244-255">Módosítsa az első `bundles.Add` utasítás (az a **jquery** csomagot) felvenni *parancsfájlok\\filter.js*, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6c244-255">Change the first `bundles.Add` statement (for the **jquery** bundle) to include *Scripts\\filter.js*, as follows:</span></span>

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

<span data-ttu-id="6c244-256">A **jquery** köteg már megjelenítése a alapértelmezés szerint  *\_elrendezés* nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-256">The **jquery** bundle is already rendered by the default *\_Layout* view.</span></span> <span data-ttu-id="6c244-257">Később használhatja a szűrés funkciót alkalmazandó más nézetek azonos JavaScript-kódot.</span><span class="sxs-lookup"><span data-stu-id="6c244-257">Later, you can utilize the same JavaScript code to apply the filter functionality to other list views.</span></span>

<span data-ttu-id="6c244-258">Frissítse a mobil böngészőt, és navigáljon a *AllSpeakers* nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-258">Refresh the mobile browser and go to the *AllSpeakers* view.</span></span> <span data-ttu-id="6c244-259">A keresési mezőbe írja be az "sc".</span><span class="sxs-lookup"><span data-stu-id="6c244-259">In the search box, type "sc".</span></span> <span data-ttu-id="6c244-260">A hangszórók lista most szűri megfelelően a keresési karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="6c244-260">The speakers list should now be filtered according to your search string.</span></span>

![][AllSpeakersFixedSearchBySC]

## <span data-ttu-id="6c244-261"><a name="bkmk_improvetags"></a>A címkék lista javítása</span><span class="sxs-lookup"><span data-stu-id="6c244-261"><a name="bkmk_improvetags"></a> Improve the Tags List</span></span>
<span data-ttu-id="6c244-262">Például a *hangszórók* nézet, a *címkék* nézet olvasható, de a hivatkozások a kis- és nehéz a mobileszköz koppintson.</span><span class="sxs-lookup"><span data-stu-id="6c244-262">Like the *Speakers* view, the *Tags* view is readable, but the links are small and difficult to tap on a mobile device.</span></span> <span data-ttu-id="6c244-263">Ezt úgy javíthatja ki a *címkék* meg ugyanúgy megtekintheti a *hangszórók* megtekintése, ha korábban, de a következő leírt kódmódosításokat `Html.ActionLink` metódus szintaxist *nézetek\\Home\\AllTags.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c244-263">You can fix the *Tags* view the same way you fix the *Speakers* view, if you use the code changes described earlier, but with the following `Html.ActionLink` method syntax in *Views\\Home\\AllTags.cshtml*:</span></span>

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="6c244-264">A frissített asztali böngészőn a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="6c244-264">The refreshed desktop browser looks as follows:</span></span>

![][AllTagsFixedDesktop]

<span data-ttu-id="6c244-265">És a frissített mobil böngészőben a következőképpen néz ki:</span><span class="sxs-lookup"><span data-stu-id="6c244-265">And the refreshed mobile browser looks as follows:</span></span> 

![][AllTagsFixed]

> [!NOTE]
> <span data-ttu-id="6c244-266">Ha azt észleli, hogy az eredeti lista formázás továbbra is van a mobil böngészőben, és mi történt a töltött Bootstrap stílusbeállításokat wonder, ez az összetevő a korábbi művelet mobil adott nézetek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6c244-266">If you notice that the original list formatting is still there in the mobile browser and wonder what happened to your nice Bootstrap styling, this is an artifact of your earlier action to create mobile specific views.</span></span> <span data-ttu-id="6c244-267">Most, hogy a rendszerindítási CSS-keretrendszer használatával hozzon létre egy rugalmas Webtervezés, nyissa meg a head, és távolítsa el a mobileszköz-specifikus elrendezés és a mobileszköz-specifikus nézeteket.</span><span class="sxs-lookup"><span data-stu-id="6c244-267">However, now that you are using the Bootstrap CSS framework to create a responsive web design, go head and remove these mobile-specific views and the mobile-specific layout views.</span></span> <span data-ttu-id="6c244-268">Ezzel végzett, a frissített mobil böngészőt a rendszer-indításkori stílusbeállításokat fognak megjelenni.</span><span class="sxs-lookup"><span data-stu-id="6c244-268">Once you have done so, the refreshed mobile browser will show the Bootstrap styling.</span></span>
> 
> 

## <span data-ttu-id="6c244-269"><a name="bkmk_improvedates"></a>A dátumok lista javítása</span><span class="sxs-lookup"><span data-stu-id="6c244-269"><a name="bkmk_improvedates"></a> Improve the Dates List</span></span>
<span data-ttu-id="6c244-270">Javíthatja a *dátumok* megtekintése, nagyobb, mint a *hangszórók* és *címkék* megtekinti a korábban, de a következő leírt kódmódosításokat használatakor `Html.ActionLink` metódus szintaxist *nézetek\\Home\\AllDates.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="6c244-270">You can improve the *Dates* view like you improved the *Speakers* and *Tags* views if you use the code changes described earlier, but with the following `Html.ActionLink` method syntax in *Views\\Home\\AllDates.cshtml*:</span></span>

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

<span data-ttu-id="6c244-271">A frissített mobil böngésző nézet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="6c244-271">You will get a refreshed mobile browser view like this:</span></span>

![][AllDatesFixed]

<span data-ttu-id="6c244-272">Tovább javíthatja a *dátumok* nézet dátum-idő értékek dátum szerint rendezésével.</span><span class="sxs-lookup"><span data-stu-id="6c244-272">You can further improve the *Dates* view by organizing the date-time values by date.</span></span> <span data-ttu-id="6c244-273">Ezt megteheti a rendszerindítási rendelkező [panelek] [ panels] frizurakészítő.</span><span class="sxs-lookup"><span data-stu-id="6c244-273">This can be done with the Bootstrap [panels][panels] styling.</span></span> <span data-ttu-id="6c244-274">Cserélje le a tartalmát a *nézetek\\Home\\AllDates.cshtml* fájl a következő kóddal:</span><span class="sxs-lookup"><span data-stu-id="6c244-274">Replace the contents of the *Views\\Home\\AllDates.cshtml* file with the following code:</span></span>

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

<span data-ttu-id="6c244-275">Ez a kód létrehoz egy különálló `<div class="panel panel-primary">` címkét az egyes különálló dátumának elemet a listában, és használja a [csatolt csoportja] [ linked list group] számára a megfelelő csatolja azt korábban.</span><span class="sxs-lookup"><span data-stu-id="6c244-275">This code creates a separate `<div class="panel panel-primary">` tag for each distinct date in the list, and uses the [linked list group][linked list group] for the respective links as before.</span></span> <span data-ttu-id="6c244-276">Ez a mobil böngésző néz ezt a kódot futtatásakor:</span><span class="sxs-lookup"><span data-stu-id="6c244-276">Here's what the mobile browser looks like when this code runs:</span></span>

![][AllDatesFixed2]

<span data-ttu-id="6c244-277">Az asztali böngészőn váltani.</span><span class="sxs-lookup"><span data-stu-id="6c244-277">Switch to the desktop browser.</span></span> <span data-ttu-id="6c244-278">Újra vegye figyelembe összhangban megjelenését.</span><span class="sxs-lookup"><span data-stu-id="6c244-278">Again, note the consistent look.</span></span>

![][AllDatesFixed2Desktop]

## <span data-ttu-id="6c244-279"><a name="bkmk_improvesessionstable"></a>A SessionsTable nézet javítása</span><span class="sxs-lookup"><span data-stu-id="6c244-279"><a name="bkmk_improvesessionstable"></a> Improve the SessionsTable View</span></span>
<span data-ttu-id="6c244-280">Ebben a szakaszban meg kell győződnie a *SessionsTable* mobilbarát további megtekintése.</span><span class="sxs-lookup"><span data-stu-id="6c244-280">In this section, you'll make the *SessionsTable* view more mobile-friendly.</span></span> <span data-ttu-id="6c244-281">Ez a változás szélesebb körű a korábbi módosítások.</span><span class="sxs-lookup"><span data-stu-id="6c244-281">This change is more extensive the previous changes.</span></span>

<span data-ttu-id="6c244-282">A mobil böngészőben, koppintson a **címke** gombra, majd adja meg `asp` be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="6c244-282">In the mobile browser, tap the **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="6c244-283">Koppintson a **ASP.NET** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6c244-283">Tap the **ASP.NET** link.</span></span>

![][SessionsTableTagASP.NET]

<span data-ttu-id="6c244-284">Ahogy látja, a megjelenítési táblaként, amely jelenleg tekinthető meg az asztali böngészőn van formázva.</span><span class="sxs-lookup"><span data-stu-id="6c244-284">As you can see, the display is formatted as a table, which is currently designed to be viewed in the desktop browser.</span></span> <span data-ttu-id="6c244-285">Azonban érdemes valamivel nehezen olvasható mobil böngésző.</span><span class="sxs-lookup"><span data-stu-id="6c244-285">However, it's a little bit difficult to read on a mobile browser.</span></span> <span data-ttu-id="6c244-286">Nyissa meg a javításhoz *nézetek\\Home\\SessionsTable.cshtml* majd cserélje le a fájl tartalmát az alábbira:</span><span class="sxs-lookup"><span data-stu-id="6c244-286">To fix this, open *Views\\Home\\SessionsTable.cshtml* and then replace the contents of the file with the following code:</span></span>

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

<span data-ttu-id="6c244-287">A kód 3 dolgot hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="6c244-287">The code does 3 things:</span></span>

* <span data-ttu-id="6c244-288">használja a rendszerindítási [egyéni csatolt csoportja] [ custom linked list group] formázandó a munkamenet-információk függőleges, úgy, hogy ezt az információt olvasható egy mobil böngésző (például a lista-csoport-elem szövege osztályok használatával)</span><span class="sxs-lookup"><span data-stu-id="6c244-288">uses the Bootstrap [custom linked list group][custom linked list group] to format the session information vertically, so that all this information is readable on a mobile browser (using classes such as list-group-item-text)</span></span>
* <span data-ttu-id="6c244-289">alkalmazza a [rács rendszer] [ grid system] elrendezését, így a munkamenet-elemek vízszintes az asztali böngészőn és függőlegesen a mobil böngészőben (az oszlop-md-4 osztály használatával)</span><span class="sxs-lookup"><span data-stu-id="6c244-289">applies the [grid system][grid system] to the layout, so that the session items flow horizontally in the desktop browser and vertically in the mobile browser (using the col-md-4 class)</span></span>
* <span data-ttu-id="6c244-290">használja a [rugalmas segédprogramok] [ responsive utilities] elrejtheti a munkamenet címkék, ha a mobil böngészőben (a rejtett xs osztály használatával)</span><span class="sxs-lookup"><span data-stu-id="6c244-290">uses the [responsive utilities][responsive utilities] to hide the session tags when viewed in the mobile browser (using the hidden-xs class)</span></span>

<span data-ttu-id="6c244-291">A cím mutató hivatkozásra a megfelelő munkamenet is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="6c244-291">You can also tap a title link to go to the respective session.</span></span> <span data-ttu-id="6c244-292">Az alábbi képen tükrözi a kód módosítására.</span><span class="sxs-lookup"><span data-stu-id="6c244-292">The image below reflects the code changes.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="6c244-293">A rendszer-indításkori rács rendszer, amely automatikusan alkalmazza a munkamenetek függőlegesen a mobil böngészőben rendezi.</span><span class="sxs-lookup"><span data-stu-id="6c244-293">The Bootstrap grid system that you applied automatically arranges the sessions vertically in the mobile browser.</span></span> <span data-ttu-id="6c244-294">Továbbá figyelje meg, hogy a címkék nem láthatók.</span><span class="sxs-lookup"><span data-stu-id="6c244-294">Also, notice that the tags are not shown.</span></span> <span data-ttu-id="6c244-295">Az asztali böngészőn váltani.</span><span class="sxs-lookup"><span data-stu-id="6c244-295">Switch to the desktop browser.</span></span>

![][SessionsTableFixedTagASP.NETDesktop]

<span data-ttu-id="6c244-296">Az asztali böngészőn figyelje meg, hogy a címkék jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6c244-296">In the desktop browser, notice that the tags are now displayed.</span></span> <span data-ttu-id="6c244-297">Ezenkívül látható, hogy a rendszer-indításkori rács rendszer alkalmazott a munkamenet elemek két oszlop rendezése a.</span><span class="sxs-lookup"><span data-stu-id="6c244-297">Also, you can see that the Bootstrap grid system you applied arranges the session items in two columns.</span></span> <span data-ttu-id="6c244-298">Ha nagyítása a böngészőben, látni fogja, hogy a megállapodás vált három oszlopot.</span><span class="sxs-lookup"><span data-stu-id="6c244-298">If you enlarge the browser, you will see that the arrangement changes to three columns.</span></span>

## <span data-ttu-id="6c244-299"><a name="bkmk_improvesessionbycode"></a>A SessionByCode nézet javítása</span><span class="sxs-lookup"><span data-stu-id="6c244-299"><a name="bkmk_improvesessionbycode"></a> Improve the SessionByCode View</span></span>
<span data-ttu-id="6c244-300">Végezetül fogjuk kijavítani a *SessionByCode* abba, hogy mobilbarát nézet.</span><span class="sxs-lookup"><span data-stu-id="6c244-300">Finally, you'll fix the *SessionByCode* view to make it mobile-friendly.</span></span>

<span data-ttu-id="6c244-301">A mobil böngészőben, koppintson a **címke** gombra, majd adja meg `asp` be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="6c244-301">In the mobile browser, tap the **Tag** button, then enter `asp` in the search box.</span></span>

![][AllTagsFixedSearchByASP]

<span data-ttu-id="6c244-302">Koppintson a **ASP.NET** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6c244-302">Tap the **ASP.NET** link.</span></span> <span data-ttu-id="6c244-303">Az ASP.NET címke munkamenetek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="6c244-303">Sessions for the ASP.NET tag are displayed.</span></span>

![][FixedSessionsByTag]

<span data-ttu-id="6c244-304">Válassza ki a **létrehozása egyetlen lap alkalmazást az ASP.NET és az AngularJS** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="6c244-304">Choose the **Building a Single Page Application with ASP.NET and AngularJS** link.</span></span>

![][SessionByCode3-644]

<span data-ttu-id="6c244-305">Az alapértelmezett nézet megfelelően működik, de javíthatja a hely könnyen néhány rendszerindítási GUI-összetevők.</span><span class="sxs-lookup"><span data-stu-id="6c244-305">The default desktop view is fine, but you can improve the look easily by using some Bootstrap GUI components.</span></span>

<span data-ttu-id="6c244-306">Nyissa meg *nézetek\\Home\\SessionByCode.cshtml* , és cserélje ki annak tartalmát a következő kódban:</span><span class="sxs-lookup"><span data-stu-id="6c244-306">Open *Views\\Home\\SessionByCode.cshtml* and replace the contents with the following markup:</span></span>

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

<span data-ttu-id="6c244-307">Az új markup Bootstrap panelek frizurakészítő a mobil nézet javítására használja.</span><span class="sxs-lookup"><span data-stu-id="6c244-307">The new markup uses Bootstrap panels styling to improve the mobile view.</span></span> 

<span data-ttu-id="6c244-308">Frissítse a mobil böngészőablakot.</span><span class="sxs-lookup"><span data-stu-id="6c244-308">Refresh the mobile browser.</span></span> <span data-ttu-id="6c244-309">Az alábbi képen a kód végrehajtott módosítások el csak tükrözi:</span><span class="sxs-lookup"><span data-stu-id="6c244-309">The following image reflects the code changes that you just made:</span></span>

![][SessionByCodeFixed3-644]

## <a name="wrap-up-and-review"></a><span data-ttu-id="6c244-310">Burkolja és felülvizsgálata</span><span class="sxs-lookup"><span data-stu-id="6c244-310">Wrap Up and Review</span></span>
<span data-ttu-id="6c244-311">Ez az oktatóanyag azt mutatja, hogyan használható az ASP.NET MVC 5 mobilbarát webes alkalmazások fejlesztéséhez.</span><span class="sxs-lookup"><span data-stu-id="6c244-311">This tutorial has shown you how to use ASP.NET MVC 5 to develop mobile-friendly Web applications.</span></span> <span data-ttu-id="6c244-312">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="6c244-312">These include:</span></span>

* <span data-ttu-id="6c244-313">Egy App Service webalkalmazásba ASP.NET MVC 5 alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6c244-313">Deploy an ASP.NET MVC 5 application to an App Service web app</span></span>
* <span data-ttu-id="6c244-314">Az MVC 5-alkalmazás rugalmas webes elrendezés létrehozásához használja a rendszerindítási</span><span class="sxs-lookup"><span data-stu-id="6c244-314">Use Bootstrap to create responsive web layout in your MVC 5 application</span></span>
* <span data-ttu-id="6c244-315">Bírálja felül a elrendezését, a nézeteket és a részleges nézetek globálisan és az egyes nézet</span><span class="sxs-lookup"><span data-stu-id="6c244-315">Override layout, views, and partial views, both globally and for an individual view</span></span>
* <span data-ttu-id="6c244-316">Vezérlő elrendezése és a részleges felülbírálása kényszerítési használatával a `RequireConsistentDisplayMode` tulajdonság</span><span class="sxs-lookup"><span data-stu-id="6c244-316">Control layout and partial override enforcement using the `RequireConsistentDisplayMode` property</span></span>
* <span data-ttu-id="6c244-317">Bizonyos böngészők, például a iPhone böngésző célzó nézetek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c244-317">Create views that target specific browsers, such as the iPhone browser</span></span>
* <span data-ttu-id="6c244-318">Alkalmazza a rendszer-indításkori stílusbeállításokat Razor-kódban</span><span class="sxs-lookup"><span data-stu-id="6c244-318">Apply Bootstrap styling in Razor code</span></span>

## <a name="see-also"></a><span data-ttu-id="6c244-319">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="6c244-319">See Also</span></span>
* [<span data-ttu-id="6c244-320">9-es alapelvek webes rugalmas kialakítás</span><span class="sxs-lookup"><span data-stu-id="6c244-320">9 basic principles of responsive web design</span></span>](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
* <span data-ttu-id="6c244-321">[Rendszerindítási][BootstrapSite]</span><span class="sxs-lookup"><span data-stu-id="6c244-321">[Bootstrap][BootstrapSite]</span></span>
* <span data-ttu-id="6c244-322">[A rendszerindítási hivatalos blogja][Official Bootstrap Blog]</span><span class="sxs-lookup"><span data-stu-id="6c244-322">[Official Bootstrap Blog][Official Bootstrap Blog]</span></span>
* <span data-ttu-id="6c244-323">[Twitter Bootstrap oktatóprogram oktatóanyag Köztársaság][Twitter Bootstrap Tutorial from Tutorial Republic]</span><span class="sxs-lookup"><span data-stu-id="6c244-323">[Twitter Bootstrap Tutorial from Tutorial Republic][Twitter Bootstrap Tutorial from Tutorial Republic]</span></span>
* <span data-ttu-id="6c244-324">[A rendszer-indításkori Playground][The Bootstrap Playground]</span><span class="sxs-lookup"><span data-stu-id="6c244-324">[The Bootstrap Playground][The Bootstrap Playground]</span></span>
* <span data-ttu-id="6c244-325">[W3C javaslat mobil webes alkalmazás gyakorlati tanácsok][W3C Recommendation Mobile Web Application Best Practices]</span><span class="sxs-lookup"><span data-stu-id="6c244-325">[W3C Recommendation Mobile Web Application Best Practices][W3C Recommendation Mobile Web Application Best Practices]</span></span>
* <span data-ttu-id="6c244-326">[W3C jelölt javaslat media lekérdezések][W3C Candidate Recommendation for media queries]</span><span class="sxs-lookup"><span data-stu-id="6c244-326">[W3C Candidate Recommendation for media queries][W3C Candidate Recommendation for media queries]</span></span>

## <a name="whats-changed"></a><span data-ttu-id="6c244-327">A változások</span><span class="sxs-lookup"><span data-stu-id="6c244-327">What's changed</span></span>
* <span data-ttu-id="6c244-328">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6c244-328">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

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
[The Bootstrap Playground]: http://www.bootply.com/
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


---
title: "Üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez |} Microsoft Docs"
description: "Megtudhatja, hogyan üzleti alkalmazás létrehozása az Azure App Service-ben, amely hitelesíti a helyszíni STS. Ez az oktatóanyag a helyszíni STS-ként AD FS célozza."
services: app-service\web
documentationcenter: .net
author: cephalin
manager: erikre
editor: 
ms.assetid: 0fa9f7a1-37bd-4d11-845f-aeff6fc9e4ca
ms.service: app-service-web
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 08/31/2016
ms.author: cephalin
ms.openlocfilehash: f9a8984400378d154a504af8a41609900128d052
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a><span data-ttu-id="43873-104">Üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="43873-104">Create a line-of-business Azure app with AD FS authentication</span></span>
<span data-ttu-id="43873-105">A cikkből megtudhatja, hogyan hozhat létre egy ASP.NET MVC üzleti alkalmazást a [Azure App Service](../app-service/app-service-value-prop-what-is.md) használatával egy helyszíni [Active Directory összevonási szolgáltatások](http://technet.microsoft.com/library/hh831502.aspx) az identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="43873-105">This article shows you how to create an ASP.NET MVC line-of-business application in [Azure App Service](../app-service/app-service-value-prop-what-is.md) using an on-premises [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) as the identity provider.</span></span> <span data-ttu-id="43873-106">Ebben a forgatókönyvben is működik, amikor az Azure App Service-üzleti alkalmazások létrehozásához, de a szervezet megköveteli a címtáradatok helyszínen tárolni.</span><span class="sxs-lookup"><span data-stu-id="43873-106">This scenario can work when you want to create line-of-business applications in Azure App Service but your organization requires directory data to be stored on-site.</span></span>

> [!NOTE]
> <span data-ttu-id="43873-107">A különböző vállalati hitelesítési és engedélyezési beállítások az Azure App Service-áttekintését lásd: [hitelesítés a helyszíni Active Directoryval, az Azure alkalmazásban](web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="43873-107">For an overview of the different enterprise authentication and authorization options for Azure App Service, see [Authenticate with on-premises Active Directory in your Azure app](web-sites-authentication-authorization.md).</span></span>
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="43873-108">Mit fog létrehozni</span><span class="sxs-lookup"><span data-stu-id="43873-108">What you will build</span></span>
<span data-ttu-id="43873-109">Egy egyszerű ASP.NET-alkalmazások az Azure App Service Web Apps a következő funkciók fog létrehozni:</span><span class="sxs-lookup"><span data-stu-id="43873-109">You will build a basic ASP.NET application in Azure App Service Web Apps with the following features:</span></span>

* <span data-ttu-id="43873-110">Hitelesíti a felhasználókat az AD FS ellen</span><span class="sxs-lookup"><span data-stu-id="43873-110">Authenticates users against AD FS</span></span>
* <span data-ttu-id="43873-111">Használja a `[Authorize]` engedélyezése a felhasználóknak a különféle műveletek</span><span class="sxs-lookup"><span data-stu-id="43873-111">Uses `[Authorize]` to authorize users for different actions</span></span>
* <span data-ttu-id="43873-112">A Visual Studio hibakeresési, mind az App Service Web Apps történő közzététel statikus konfigurációját (csak egyszer konfigurálnia, hibakeresését és bármikor közzététele)</span><span class="sxs-lookup"><span data-stu-id="43873-112">Static configuration for both debugging in Visual Studio and publishing to App Service Web Apps (configure once, debug and publish anytime)</span></span>  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="43873-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="43873-113">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="43873-114">Az oktatóanyag teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="43873-114">You need the following to complete this tutorial:</span></span>

* <span data-ttu-id="43873-115">Egy helyszíni AD FS üzembe helyezése (ebben az oktatóanyagban használt tesztkörnyezet végpont útmutatót lásd: [tesztlabor: az AD FS az Azure virtuális gép (csak tesztelési) önálló STS](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span><span class="sxs-lookup"><span data-stu-id="43873-115">An on-premises AD FS deployment (for an end-to-end walkthrough of the test lab used in this tutorial, see [Test Lab: Standalone STS with AD FS in Azure VM (for test only)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span></span>
* <span data-ttu-id="43873-116">Függő létrehozásához szükséges engedélyek fél AD FS felügyelet bizalmi kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="43873-116">Permissions to create relying party trusts in AD FS Management</span></span>
* <span data-ttu-id="43873-117">A Visual Studio 2013 Update 4 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="43873-117">Visual Studio 2013 Update 4 or later</span></span>
* <span data-ttu-id="43873-118">[Az Azure SDK 2.8.1-es verziójának](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="43873-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) or later</span></span>

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a><span data-ttu-id="43873-119">Mintaalkalmazást használ üzleti sablon</span><span class="sxs-lookup"><span data-stu-id="43873-119">Use sample application for line-of-business template</span></span>
<span data-ttu-id="43873-120">Ebben az oktatóanyagban a mintaalkalmazáshoz [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), az Azure Active Directory csapata hozza létre.</span><span class="sxs-lookup"><span data-stu-id="43873-120">The sample application in this tutorial, [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), is created by the Azure Active Directory team.</span></span> <span data-ttu-id="43873-121">Mivel az AD FS támogatja a WS-Federation, használhatja azt sablonként üzleti alkalmazások létrehozása a könnyű kezelés.</span><span class="sxs-lookup"><span data-stu-id="43873-121">Since AD FS supports WS-Federation, you can use it as a template to create line-of-business applications with ease.</span></span> <span data-ttu-id="43873-122">A következő jellemzőkkel rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="43873-122">It has the following features:</span></span>

* <span data-ttu-id="43873-123">Használja a [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) való hitelesítéshez szükséges egy helyszíni AD FS üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="43873-123">Uses [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) to authenticate with an on-premises AD FS deployment</span></span>
* <span data-ttu-id="43873-124">Be- és kijelentkezési funkció</span><span class="sxs-lookup"><span data-stu-id="43873-124">Sign-in and sign-out functionality</span></span>
* <span data-ttu-id="43873-125">Használja a [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (a Windows Identity Foundation), és nem olyan a jövőbeli az ASP.NET és jóval egyszerűbb, hitelesítési és engedélyezési mint WIF beállítása</span><span class="sxs-lookup"><span data-stu-id="43873-125">Uses [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (instead of Windows Identity Foundation), which is the future of ASP.NET and much simpler to set up for authentication and authorization than WIF</span></span>

<a name="bkmk_setup"></a>

## <a name="set-up-the-sample-application"></a><span data-ttu-id="43873-126">A mintaalkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="43873-126">Set up the sample application</span></span>
1. <span data-ttu-id="43873-127">Klónozza, vagy töltse le a minta megoldást a következő [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) a helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="43873-127">Clone or download the sample solution at [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) to your local directory.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="43873-128">Található utasítások segítségével: [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) megtudhatja, hogyan állíthat be az alkalmazás az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="43873-128">The instructions at [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) show you how to set up the application with Azure Active Directory.</span></span> <span data-ttu-id="43873-129">Azonban ebben az oktatóanyagban, állítsa be az AD FS-sel, ezért a lépések az itt helyette.</span><span class="sxs-lookup"><span data-stu-id="43873-129">But in this tutorial, you set it up with AD FS, so follow the steps here instead.</span></span>
   > 
   > 
2. <span data-ttu-id="43873-130">Nyissa meg a megoldást, és nyissa meg a Controllers\AccountController.cs a **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="43873-130">Open the solution, and then open Controllers\AccountController.cs in the **Solution Explorer**.</span></span>
   
   <span data-ttu-id="43873-131">Látni fogja, hogy a kód egy hitelesítési kérdés hitelesíteni a felhasználót, WS-Federation használatával egyszerűen ad ki.</span><span class="sxs-lookup"><span data-stu-id="43873-131">You will see that the code simply issues an authentication challenge to authenticate the user using WS-Federation.</span></span> <span data-ttu-id="43873-132">Minden hitelesítési App_Start\Startup.Auth.cs van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="43873-132">All authentication is configured in App_Start\Startup.Auth.cs.</span></span>
3. <span data-ttu-id="43873-133">Nyissa meg a App_Start\Startup.Auth.cs.</span><span class="sxs-lookup"><span data-stu-id="43873-133">Open App_Start\Startup.Auth.cs.</span></span> <span data-ttu-id="43873-134">Az a `ConfigureAuth` metódus, vegye figyelembe a sor:</span><span class="sxs-lookup"><span data-stu-id="43873-134">In the `ConfigureAuth` method, note the line:</span></span>
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   <span data-ttu-id="43873-135">OWIN világában a részlet valójában az operációs rendszer minimális kell konfigurálnia a WS-Federation hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="43873-135">In the OWIN world, this snippet is really the bare minimum you need to configure WS-Federation authentication.</span></span> <span data-ttu-id="43873-136">Már jóval egyszerűbb, több elegáns, mint a WIF, ahol Web.config van beszúrta XML hely számos országában dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="43873-136">It is much simpler and more elegant than WIF, where Web.config is injected with XML all over the place.</span></span> <span data-ttu-id="43873-137">Információ szüksége a függő entitás (RP) azonosítóját és az AD FS szolgáltatást metaadatait tartalmazó fájl URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="43873-137">The only information you need is the relying party's (RP) identifier and the URL of your AD FS service's metadata file.</span></span> <span data-ttu-id="43873-138">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="43873-138">Here's an example:</span></span>
   
   * <span data-ttu-id="43873-139">Függő Entitás azonosítója:`https://contoso.com/MyLOBApp`</span><span class="sxs-lookup"><span data-stu-id="43873-139">RP identifier: `https://contoso.com/MyLOBApp`</span></span>
   * <span data-ttu-id="43873-140">Metaadatok címe:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span><span class="sxs-lookup"><span data-stu-id="43873-140">Metadata address: `http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span></span>
4. <span data-ttu-id="43873-141">App_Start\Startup.Auth.cs módosítsa a következő statikus karakterlánc-definíciók:</span><span class="sxs-lookup"><span data-stu-id="43873-141">In App_Start\Startup.Auth.cs, change the following static string definitions:</span></span>  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. <span data-ttu-id="43873-142">Most végezze el a megfelelő módosításokat a Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="43873-142">Now, make the corresponding changes in Web.config.</span></span> <span data-ttu-id="43873-143">Nyissa meg a Web.config fájl, és módosítsa a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="43873-143">Open the Web.config and modify the following app settings:</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter the App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter the relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter the FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   <span data-ttu-id="43873-144">Töltse ki a megfelelő környezet alapján értékek.</span><span class="sxs-lookup"><span data-stu-id="43873-144">Fill in the key values based on your respective environment.</span></span>
6. <span data-ttu-id="43873-145">Győződjön meg arról, hogy nincsenek hibák az alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="43873-145">Build the application to make sure there are no errors.</span></span>

<span data-ttu-id="43873-146">Ennyi az egész.</span><span class="sxs-lookup"><span data-stu-id="43873-146">That's it.</span></span> <span data-ttu-id="43873-147">Most már a mintaalkalmazáshoz az AD FS működik készen áll.</span><span class="sxs-lookup"><span data-stu-id="43873-147">Now the sample application is ready to work with AD FS.</span></span> <span data-ttu-id="43873-148">Továbbra is szeretné egy függő Entitás megbízhatósági kapcsolat beállítása az AD FS újabb alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="43873-148">You still need to configure an RP trust with this application in AD FS later.</span></span>

<a name="bkmk_deploy"></a>

## <a name="deploy-the-sample-application-to-azure-app-service-web-apps"></a><span data-ttu-id="43873-149">Az Azure App Service Web Apps a mintaalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="43873-149">Deploy the sample application to Azure App Service Web Apps</span></span>
<span data-ttu-id="43873-150">Itt akkor tegye közzé az alkalmazást egy webes alkalmazás az App Service Web Apps a hibakeresési környezetben adatainak megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="43873-150">Here, you publish the application to a web app in App Service Web Apps while preserving the debug environment.</span></span> <span data-ttu-id="43873-151">Vegye figyelembe, hogy az alkalmazás közzététele előtt azt egy függő Entitás megbízhatósági kapcsolattal rendelkezik azzal az AD FS-ben, a hitelesítés továbbra sem működik még fog.</span><span class="sxs-lookup"><span data-stu-id="43873-151">Note that you're going to publish the application before it has an RP trust with AD FS, so authentication still doesn't work yet.</span></span> <span data-ttu-id="43873-152">Azonban ha így tesz, akkor most lehet a webes alkalmazás URL-CÍMÉT, melyekkel később konfigurálhatja a függő Entitás megbízhatóságát.</span><span class="sxs-lookup"><span data-stu-id="43873-152">However, if you do it now you can have the web app URL that you can use to configure the RP trust later.</span></span>

1. <span data-ttu-id="43873-153">Kattintson jobb gombbal a projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="43873-153">Right-click your project and select **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. <span data-ttu-id="43873-154">Válassza ki **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="43873-154">Select **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="43873-155">Ha még nem jelentkezett be az Azure-ba, kattintson a **bejelentkezés** és az Azure-előfizetéshez a Microsoft-fiók használatával jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="43873-155">If you haven't signed in to Azure, click **Sign In** and use the Microsoft account for your Azure subscription to sign in.</span></span>
4. <span data-ttu-id="43873-156">Miután bejelentkezett, kattintson a **új** egy webalkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="43873-156">Once signed in, click **New** to create a web app.</span></span>
5. <span data-ttu-id="43873-157">Töltse ki az összes kötelező mezőt.</span><span class="sxs-lookup"><span data-stu-id="43873-157">Fill in all required fields.</span></span> <span data-ttu-id="43873-158">Csatlakozzon a helyszíni adatok később, a webalkalmazás-adatbázis létrehozása nem kívánja.</span><span class="sxs-lookup"><span data-stu-id="43873-158">You are going to connect to on-premises data later, so don't create a database for this web app.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. <span data-ttu-id="43873-159">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="43873-159">Click **Create**.</span></span> <span data-ttu-id="43873-160">A webalkalmazás létrehozása után a webhely közzététele párbeszédpanelen van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="43873-160">Once the web app is created, the Publish Web dialog is opened.</span></span>
7. <span data-ttu-id="43873-161">A **URL-címre**, módosítsa **http** való **https**.</span><span class="sxs-lookup"><span data-stu-id="43873-161">In **Destination URL**, change **http** to **https**.</span></span> <span data-ttu-id="43873-162">Másolja a teljes URL-címet egy szövegszerkesztőben későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="43873-162">Copy the entire URL to a text editor for later use.</span></span> <span data-ttu-id="43873-163">Kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="43873-163">Then, click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. <span data-ttu-id="43873-164">A Visual Studióban nyissa meg a **Web.Release.config** a projektben.</span><span class="sxs-lookup"><span data-stu-id="43873-164">In Visual Studio, open **Web.Release.config** in your project.</span></span> <span data-ttu-id="43873-165">Helyezze be a következő XML a `<configuration>` címkét, és cserélje le a kulcs értékét a közzététel webes alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="43873-165">Insert the following XML into the `<configuration>` tag, and replace the key value with your publish web app's URL.</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

<span data-ttu-id="43873-166">Amikor elkészült, hogy a projekt, a Visual Studio hibakeresési környezetben, és egy a közzétett webalkalmazás az Azure-ban beállított két RP azonosítót.</span><span class="sxs-lookup"><span data-stu-id="43873-166">When you're done, you have two RP identifiers configured in your project, one for your debug environment in Visual Studio, and one for the published web app in Azure.</span></span> <span data-ttu-id="43873-167">Az egyes a két környezet között, az AD FS fog beállítani egy függő Entitás megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="43873-167">You will set up an RP trust for each of the two environments in AD FS.</span></span> <span data-ttu-id="43873-168">Hibakeresési, a beállítások a Web.config fájlban szolgálja, hogy a **Debug** az AD FS konfigurációs munka.</span><span class="sxs-lookup"><span data-stu-id="43873-168">During debugging, the app settings in Web.config are used to make your **Debug** configuration work with AD FS.</span></span> <span data-ttu-id="43873-169">Ha közzé van téve (alapértelmezés szerint a **kiadás** konfigurációs közzé van téve), átalakított Web.config tölt fel, amely magában foglalja a Web.Release.config app beállítás változásait.</span><span class="sxs-lookup"><span data-stu-id="43873-169">When it's published (by default, the **Release** configuration is published), a transformed Web.config is uploaded that incorporates the app setting changes in Web.Release.config.</span></span>

<span data-ttu-id="43873-170">Ha azt szeretné, hogy a közzétett webalkalmazás az Azure szolgáltatásban csatlakoztatni a hibakereső (vagyis fel kell töltenie a kód a közzétett webes alkalmazás hibakeresési szimbólumok), a hibakeresési konfiguráció Azure hibakereséshez, de a saját egyéni Web.config transzformálása (pl. Web.AzureDebug.config), amely az alkalmazás beállításai Web.Release.config másolat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="43873-170">If you want to attach the published web app in Azure to the debugger (i.e. you must upload debug symbols of your code in the published web app), you can create a clone of the Debug configuration for Azure debugging, but with its own custom Web.config transform (e.g. Web.AzureDebug.config) that uses the app settings from Web.Release.config.</span></span> <span data-ttu-id="43873-171">Ez lehetővé teszi, hogy statikus karbantartása a különböző környezetek között.</span><span class="sxs-lookup"><span data-stu-id="43873-171">This allows you to maintain a static configuration across the different environments.</span></span>

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a><span data-ttu-id="43873-172">Megbízható függő entitások AD FS felügyelet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="43873-172">Configure relying party trusts in AD FS Management</span></span>
<span data-ttu-id="43873-173">Most kell egy függő Entitás megbízhatóságának AD FS felügyeleti konfigurálása előtt használja a mintaalkalmazást, és az AD FS ténylegesen hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="43873-173">Now you need to configure an RP trust in AD FS Management before you can use your sample application and actually authenticate with AD FS.</span></span> <span data-ttu-id="43873-174">Szüksége lesz a két külön RP bizalmi kapcsolatok, a hibakeresés környezethez és az egyikben a közzétett webalkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="43873-174">You will need to set up two separate RP trusts, one for your debug environment and one for your published web app.</span></span>

> [!NOTE]
> <span data-ttu-id="43873-175">Győződjön meg arról, hogy mindkét a környezetek esetében ismételje meg az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="43873-175">Make sure that you repeat the following steps for both of your environments.</span></span>
> 
> 

1. <span data-ttu-id="43873-176">Az AD FS kiszolgálón jelentkezzen be az AD FS felügyeleti jogosultságokkal rendelkező hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="43873-176">On your AD FS server, log in with credentials that have management rights to AD FS.</span></span>
2. <span data-ttu-id="43873-177">Nyissa meg az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="43873-177">Open AD FS Management.</span></span> <span data-ttu-id="43873-178">Kattintson a jobb gombbal **AD FS\Trusted Relationships\Relying entitások Megbízhatóságai** válassza **függő entitás megbízhatóságának hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="43873-178">Right-click **AD FS\Trusted Relationships\Relying Party Trusts** and select **Add Relying Party Trust**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. <span data-ttu-id="43873-179">Az a **adatforrás kiválasztása** lapon, hogy melyik **függő entitással kapcsolatos adatok manuális megadása**.</span><span class="sxs-lookup"><span data-stu-id="43873-179">In the **Select Data Source** page, select **Enter data about the relying party manually**.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. <span data-ttu-id="43873-180">Az a **megjelenítendő név megadása** lapon adjon meg egy megjelenítési nevet az alkalmazáshoz, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="43873-180">In the **Specify Display Name** page, type a display name for the application and click **Next**.</span></span>
5. <span data-ttu-id="43873-181">Az a **válasszon protokoll** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="43873-181">In the **Choose Protocol** page, click **Next**.</span></span>
6. <span data-ttu-id="43873-182">Az a **tanúsítvány konfigurálása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="43873-182">In the **Configure Certificate** page, click **Next**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="43873-183">Mivel kell használni HTTPS már, titkosított jogkivonatok nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="43873-183">Since you should be using HTTPS already, encrypted tokens are optional.</span></span> <span data-ttu-id="43873-184">Ha valóban szeretné titkosítani az ezen az oldalon az AD FS jogkivonatokat, jogkivonat-visszafejtési logikát is hozzá kell adnia a kódot.</span><span class="sxs-lookup"><span data-stu-id="43873-184">If you really want to encrypt tokens from AD FS on this page, you must also add token-decrypting logic in your code.</span></span> <span data-ttu-id="43873-185">További információkért lásd: [manuálisan titkosítja az OWIN WS-Federation köztes konfigurálása és elfogadása jogkivonatok](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span><span class="sxs-lookup"><span data-stu-id="43873-185">For more information, see [Manually configuring OWIN WS-Federation middleware and accepting encrypted tokens](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span></span>
   > 
   > 
7. <span data-ttu-id="43873-186">Helyezze át a következő lépés az alakzatot, meg kell információ a Visual Studio projektből.</span><span class="sxs-lookup"><span data-stu-id="43873-186">Before you move onto the next step, you need one piece of information from your Visual Studio project.</span></span> <span data-ttu-id="43873-187">A projekt tulajdonságait, jegyezze fel a **SSL URL-cím** az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="43873-187">In the project properties, note the **SSL URL** of the application.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. <span data-ttu-id="43873-188">AD FS felügyelet, a biztonsági másolatot a **URL konfigurálása** oldalán a **függő entitás megbízhatóságának hozzáadása varázsló**, jelölje be **a WS-Federation passzív protokoll támogatásának engedélyezése** és az SSL URL-cím típusú-e a Visual Studio-projektet az előző lépésben feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="43873-188">Back in AD FS Management, in the **Configure URL** page of the **Add Relying Party Trust Wizard**, select **Enable support for the WS-Federation Passive protocol** and type in the SSL URL of your Visual Studio project that you noted in the previous step.</span></span> <span data-ttu-id="43873-189">Ezután kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="43873-189">Then, click **Next**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > <span data-ttu-id="43873-190">URL-CÍMÉT határozza meg, hova küldje az ügyfél a sikeres hitelesítést követően.</span><span class="sxs-lookup"><span data-stu-id="43873-190">URL specifies where to send the client after authentication succeeds.</span></span> <span data-ttu-id="43873-191">A hibakeresési környezetben kell <code>https://localhost:&lt;port&gt;/</code>.</span><span class="sxs-lookup"><span data-stu-id="43873-191">For the debug environment, it should be <code>https://localhost:&lt;port&gt;/</code>.</span></span> <span data-ttu-id="43873-192">A közzétett webes alkalmazás esetén a webes alkalmazás URL-CÍMÉT kell lennie.</span><span class="sxs-lookup"><span data-stu-id="43873-192">For the published web app, it should be the web app URL.</span></span>
   > 
   > 
9. <span data-ttu-id="43873-193">Az a **azonosítók konfigurálása** lapon ellenőrizze, hogy a projekt SSL URL-cím már szerepel-e, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="43873-193">In the **Configure Identifiers** page, verify that your project SSL URL is already listed and click **Next**.</span></span> <span data-ttu-id="43873-194">Kattintson a **következő** egészen az alapértelmezett beállításokat a varázsló befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="43873-194">Click **Next** all the way to the end of the wizard with default selections.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="43873-195">A App_Start\Startup.Auth.cs a Visual Studio-projekt, ez az azonosító értékének elleni egyezik <code>WsFederationAuthenticationOptions.Wtrealm</code> összevont hitelesítés során.</span><span class="sxs-lookup"><span data-stu-id="43873-195">In App_Start\Startup.Auth.cs of your Visual Studio project, this identifier is matched against the value of <code>WsFederationAuthenticationOptions.Wtrealm</code> during federated authentication.</span></span> <span data-ttu-id="43873-196">Alapértelmezés szerint az előző lépésben az alkalmazás URL-címe meg van adva egy függő Entitás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="43873-196">By default, the application's URL from the previous step is added as an RP identifier.</span></span>
   > 
   > 
10. <span data-ttu-id="43873-197">Most már befejezte a projekt a RP-alkalmazások konfigurálása az AD FS-ben.</span><span class="sxs-lookup"><span data-stu-id="43873-197">You have now finished configuring the RP application for your project in AD FS.</span></span> <span data-ttu-id="43873-198">Ezután állítsa be az alkalmazás a jogcímek, az alkalmazás által igényelt küldésére.</span><span class="sxs-lookup"><span data-stu-id="43873-198">Next, you configure this application to send the claims needed by your application.</span></span> <span data-ttu-id="43873-199">A **Jogcímszabályok szerkesztése** párbeszédpanelen alapértelmezés szerint megnyílik az Ön a varázsló végén, és azonnal megkezdje.</span><span class="sxs-lookup"><span data-stu-id="43873-199">The **Edit Claim Rules** dialog is opened by default for you at the end of the wizard so you can start immediately.</span></span> <span data-ttu-id="43873-200">Pedig konfiguráljuk legalább a következő jogcímeket (zárójelben sémák):</span><span class="sxs-lookup"><span data-stu-id="43873-200">Let's configure at least the following claims (with schemas in parentheses):</span></span>
    
    * <span data-ttu-id="43873-201">Az ASP.NET által használt hydrate neve (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - `User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="43873-201">Name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - used by ASP.NET to hydrate `User.Identity.Name`.</span></span>
    * <span data-ttu-id="43873-202">Egyszerű felhasználónév (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) – a szervezethez tartozó felhasználók egyedi azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="43873-202">User principal name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - used to uniquely identify users in the organization.</span></span>
    * <span data-ttu-id="43873-203">A csoporttagságokat, szerepkörök (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - használható `[Authorize(Roles="role1, role2,...")]` decoration tartományvezérlők/műveletek engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="43873-203">Group memberships as roles (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - can be used with `[Authorize(Roles="role1, role2,...")]` decoration to authorize controllers/actions.</span></span> <span data-ttu-id="43873-204">A valóságban ez a módszer nem lehet a legtöbb performant a szerepkör-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="43873-204">In reality, this approach may not be the most performant for role authorization.</span></span> <span data-ttu-id="43873-205">Ha az AD-felhasználók biztonsági csoport több száz tartozik, több száz szerepkör jogcím szerepel a SAML-jogkivonat válnak.</span><span class="sxs-lookup"><span data-stu-id="43873-205">If your AD users belong to hundreds of security groups, they become hundreds of role claims in the SAML token.</span></span> <span data-ttu-id="43873-206">Egy másik módszert-hoz feltételes attól függően, hogy a felhasználó tagságát szerepkör egyetlen jogcímet küld ki, egy adott csoport.</span><span class="sxs-lookup"><span data-stu-id="43873-206">An alternative approach is to send a single role claim conditionally depending on the user's membership in a particular group.</span></span> <span data-ttu-id="43873-207">Azonban szavatoljuk az egyszerű ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="43873-207">However, we'll keep it simple for this tutorial.</span></span>
    * <span data-ttu-id="43873-208">Azonosító (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) Name,-hamisítás érvényesítéshez használható.</span><span class="sxs-lookup"><span data-stu-id="43873-208">Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - can be used for anti-forgery validation.</span></span> <span data-ttu-id="43873-209">Hamisítás érvényesítéssel használható legyen kapcsolatos további információkért lásd: a **üzleti funkciókkal** szakasza [üzleti Azure-alkalmazás létrehozása az Azure Active Directory hitelesítési](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span><span class="sxs-lookup"><span data-stu-id="43873-209">For more information on how to make it work with anti-forgery validation, see the **Add line-of-business functionality** section of [Create a line-of-business Azure app with Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="43873-210">A jogcímtípusok, konfigurálnia kell az alkalmazás az alkalmazás igényeinek határozza meg.</span><span class="sxs-lookup"><span data-stu-id="43873-210">The claim types you need to configure for your application is determined by your application's needs.</span></span> <span data-ttu-id="43873-211">Azure Active Directory-alkalmazások (azaz a függő Entitás megbízhatósági kapcsolatok) által támogatott jogcímek listája, például: [támogatott jogkivonat és jogcímtípusok](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span><span class="sxs-lookup"><span data-stu-id="43873-211">For the list of claims supported by Azure Active Directory applications (i.e. RP trusts), for example, see [Supported Token and Claim Types](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span></span>
    > 
    > 
11. <span data-ttu-id="43873-212">Kattintson a Jogcímszabályok szerkesztése párbeszédpanel **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="43873-212">In the Edit Claim Rules dialog, click **Add Rule**.</span></span>
12. <span data-ttu-id="43873-213">Konfigurálja a nevét, az egyszerű felhasználónév és a szerepkör jogcím, ahogy a képernyőkép, majd kattintson az **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="43873-213">Configure the name, UPN, and role claims as shown in the screenshot and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    <span data-ttu-id="43873-214">Ezután hozzon létre egy átmeneti nevet azonosító jogcím bemutatott lépések [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="43873-214">Next, you create a transient name ID claim using the steps demonstrated in [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
13. <span data-ttu-id="43873-215">Kattintson a **szabály hozzáadása** újra.</span><span class="sxs-lookup"><span data-stu-id="43873-215">Click **Add Rule** again.</span></span>
14. <span data-ttu-id="43873-216">Válassza ki **jogcímek küldése egyéni szabály segítségével** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="43873-216">Select **Send Claims Using a Custom Rule** and click **Next**.</span></span>
15. <span data-ttu-id="43873-217">Illessze be a következő nyelvének azokat a **egyéni szabály** mezőbe a szabály neve **munkamenet azonosítója** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="43873-217">Paste the following rule language into the **Custom rule** box, name the rule **Per Session Identifier** and click **Finish**.</span></span>  
    
    <pre class="prettyprint">
    c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"] &amp;&amp;
    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant"]
     => add(
         store = "_OpaqueIdStore",
         types = ("<mark>http://contoso.com/internal/sessionid</mark>"),
         query = "{0};{1};{2};{3};{4}",
         param = "useEntropy",
         param = c1.Value,
         param = c1.OriginalIssuer,
         param = "",
         param = c2.Value);
    </pre>
    
    <span data-ttu-id="43873-218">Az egyéni szabály ezen a képernyőfelvételen látható hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="43873-218">Your custom rule should look like this screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. <span data-ttu-id="43873-219">Kattintson a **szabály hozzáadása** újra.</span><span class="sxs-lookup"><span data-stu-id="43873-219">Click **Add Rule** again.</span></span>
17. <span data-ttu-id="43873-220">Válassza ki **bejövő jogcím átalakítása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="43873-220">Select **Transform an Incoming Claim** and click **Next**.</span></span>
18. <span data-ttu-id="43873-221">A szabály konfigurálása (a létrehozott egyéni szabály jogcímtípus használatával) a képernyőfelvételen látható módon, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="43873-221">Configure the rule as shown in the screenshot (using the claim type you created in the custom rule) and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    <span data-ttu-id="43873-222">Az átmeneti Névazonosító jogcímet lépéseinek részletes információkért lásd: [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="43873-222">For detailed information on the steps for the transient Name ID claim, see [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
19. <span data-ttu-id="43873-223">Kattintson a **alkalmaz** a a **Jogcímszabályok szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="43873-223">Click **Apply** in the **Edit Claim Rules** dialog.</span></span> <span data-ttu-id="43873-224">Mostantól az alábbi képernyőfelvételen hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="43873-224">It should now look like the following screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > <span data-ttu-id="43873-225">Ebben az esetben győződjön meg arról, hogy a hibakeresési környezetben és a közzétett webes alkalmazás ismételje meg ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="43873-225">Again, make sure that you repeat these steps for both your debug environment and published web app.</span></span>
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a><span data-ttu-id="43873-226">Az alkalmazás összevont hitelesítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="43873-226">Test federated authentication for your application</span></span>
<span data-ttu-id="43873-227">Készen áll az alkalmazás hitelesítési logikát szemben az AD FS tesztelése.</span><span class="sxs-lookup"><span data-stu-id="43873-227">You are ready to test your application's authentication logic against AD FS.</span></span> <span data-ttu-id="43873-228">Az AD FS laborkörnyezetben, amelyhez tartozik egy tesztcsoportot, az Active Directory (AD) tesztfelhasználó van.</span><span class="sxs-lookup"><span data-stu-id="43873-228">In my AD FS lab environment, I have a test user that belongs to a test group in Active Directory (AD).</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

<span data-ttu-id="43873-229">A hibakereső hitelesítés teszteléséhez mindössze annyit kell tennie most típus `F5`.</span><span class="sxs-lookup"><span data-stu-id="43873-229">To test authentication in the debugger, all you need to do now is type `F5`.</span></span> <span data-ttu-id="43873-230">Ha szeretné a közzétett webes alkalmazás alapuló hitelesítésének tesztelése, keresse meg az URL-címet.</span><span class="sxs-lookup"><span data-stu-id="43873-230">If you want to test authentication in the published web app, navigate to the URL.</span></span>

<span data-ttu-id="43873-231">A webalkalmazás betöltése után kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="43873-231">After the web application loads, click **Sign In**.</span></span> <span data-ttu-id="43873-232">Most szerezheti be bejelentkezési vagy a bejelentkezési oldal AD FS-ben az AD FS által választott hitelesítési módszertől függően szolgálja ki.</span><span class="sxs-lookup"><span data-stu-id="43873-232">You should now get either a login dialog or the login page served by AD FS, depending on the authentication method chosen by AD FS.</span></span> <span data-ttu-id="43873-233">Ez mit jelenik meg az Internet Explorer 11.</span><span class="sxs-lookup"><span data-stu-id="43873-233">Here's what I get in Internet Explorer 11.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

<span data-ttu-id="43873-234">Az AD-tartomány az AD FS központi telepítés a felhasználó jelentkezik, ha meg kell jelennie a kezdőlapon újra **Hello, <User Name>!**</span><span class="sxs-lookup"><span data-stu-id="43873-234">Once you log in with a user in the AD domain of the AD FS deployment, you should now see the homepage again with **Hello, <User Name>!**</span></span> <span data-ttu-id="43873-235">a sarokban.</span><span class="sxs-lookup"><span data-stu-id="43873-235">in the corner.</span></span> <span data-ttu-id="43873-236">Ez mit jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="43873-236">Here's what I get.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

<span data-ttu-id="43873-237">Az eddigi sikeresen a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="43873-237">So far, you've succeeded in the following ways:</span></span>

* <span data-ttu-id="43873-238">Az alkalmazás sikeresen elérte az AD FS és a megfelelő függő Entitás azonosítója az AD FS adatbázisában található</span><span class="sxs-lookup"><span data-stu-id="43873-238">Your application has successfully reached AD FS and a matching RP identifier is found in the AD FS database</span></span>
* <span data-ttu-id="43873-239">Az AD-felhasználó és az alkalmazás kezdőlapja biztonsági átirányítási AD FS sikeresen hitelesítette.</span><span class="sxs-lookup"><span data-stu-id="43873-239">AD FS has successfully authenticated an AD user and redirect you back to the application's homepage</span></span>
* <span data-ttu-id="43873-240">AD FS Szolgáltatásban sikeresen elküldte a jogcím nevét (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) az alkalmazás, ahogy azt a tényt, hogy a felhasználó neve sarokban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="43873-240">AD FS as successfully sent the name claim (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) to your application, as indicated by the fact that the user name is displayed in the corner.</span></span> 

<span data-ttu-id="43873-241">Ha a jogcím nevét hiányzik, akkor amint láthatta **Hello,!**.</span><span class="sxs-lookup"><span data-stu-id="43873-241">If the name claim is missing, you would have seen **Hello, !**.</span></span> <span data-ttu-id="43873-242">Ha megnézzük Views\Shared\_LoginPartial.cshtml, találja, hogy használja `User.Identity.Name` megjelenítése a felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="43873-242">If you look at Views\Shared\_LoginPartial.cshtml, you find that it uses `User.Identity.Name` to display the user name.</span></span> <span data-ttu-id="43873-243">Ahogy korábban említettük, ha a név jogcímet a hitelesített felhasználó érhető el a SAML-jogkivonat ASP.NET hydrates Ez a tulajdonság azt.</span><span class="sxs-lookup"><span data-stu-id="43873-243">As mentioned previously, if the name claim of the authenticated user is available in the SAML token, ASP.NET hydrates this property with it.</span></span> <span data-ttu-id="43873-244">A jogcímek az AD FS által küldött megtekintéséhez be töréspont Controllers\HomeController.cs, Index művelet metódus.</span><span class="sxs-lookup"><span data-stu-id="43873-244">To see all the claims that are sent by AD FS, put a breakpoint in Controllers\HomeController.cs, in the Index action method.</span></span> <span data-ttu-id="43873-245">A felhasználó hitelesítése után vizsgálja meg a `System.Security.Claims.Current.Claims` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="43873-245">After the user is authenticated, inspect the `System.Security.Claims.Current.Claims` collection.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a><span data-ttu-id="43873-246">Adott vezérlőket vagy a műveletek a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="43873-246">Authorize users for specific controllers or actions</span></span>
<span data-ttu-id="43873-247">Csoporttagságok szerepkör jogcímekként bevont a függő Entitás szintű bizalmi kapcsolatainak konfigurációja, mivel most már használhatja azokat közvetlenül a `[Authorize(Roles="...")]` decoration tartományvezérlőket és a műveletek.</span><span class="sxs-lookup"><span data-stu-id="43873-247">Since you have included group memberships as role claims in your RP trust configuration, you can now use them directly in the `[Authorize(Roles="...")]` decoration for controllers and actions.</span></span> <span data-ttu-id="43873-248">A létrehozás-olvasás-Módosítás-Törlés (CRUD) mintával üzleti alkalmazásokban minden művelet érhető el az egyes szerepkörök engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="43873-248">In a line-of-business application with the Create-Read-Update-Delete (CRUD) pattern, you can authorize specific roles to access each action.</span></span> <span data-ttu-id="43873-249">Most akkor lesz csak próbálja ki ezt a funkciót a meglévő otthoni vezérlő.</span><span class="sxs-lookup"><span data-stu-id="43873-249">For now, you will just try out this feature on the existing Home controller.</span></span>

1. <span data-ttu-id="43873-250">Nyissa meg a Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="43873-250">Open Controllers\HomeController.cs.</span></span>
2. <span data-ttu-id="43873-251">Adja a `About` és `Contact` hasonló az alábbi kód, használja-e biztonsági műveletmetódusokhoz csoporttagságok a hitelesített felhasználó rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="43873-251">Decorate the `About` and `Contact` action methods similar to the following code, using security group memberships that your authenticated user has.</span></span>  
   
    <pre class="prettyprint">
    <mark>[Authorize(Roles="Test Group")]</mark>
    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";
   
        return View();
    }
   
    <mark>[Authorize(Roles="Domain Admins")]</mark>
    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";
   
        return View();
    }
    </pre>
   
    <span data-ttu-id="43873-252">Óta hozzáadott **tesztelése felhasználói** való **Tesztcsoport** a az AD FS laborkörnyezetben engedélyezési tesztelése a Tesztcsoport fogok használni `About`.</span><span class="sxs-lookup"><span data-stu-id="43873-252">Since I added **Test User** to **Test Group** in my AD FS lab environment, I'll use Test Group to test authorization on `About`.</span></span> <span data-ttu-id="43873-253">A `Contact`, a negatív gyorsítás esetében is tesztelni fogja **Tartománygazdák**található amely **tesztelése felhasználói** nem tartozik.</span><span class="sxs-lookup"><span data-stu-id="43873-253">For `Contact`, I'll test the negative case of **Domain Admins**, to which **Test User** doesn't belong.</span></span>
3. <span data-ttu-id="43873-254">Indítsa el a hibakereső beírásával `F5` jelentkezzen be, majd kattintson a **kapcsolatos**.</span><span class="sxs-lookup"><span data-stu-id="43873-254">Start the debugger by typing `F5` and sign in, then click **About**.</span></span> <span data-ttu-id="43873-255">Ön most kell megtekintése a `~/About/Index` lapon sikeres legyen, ha a hitelesített felhasználó jogosult-e az adott művelethez.</span><span class="sxs-lookup"><span data-stu-id="43873-255">You should now be viewing the `~/About/Index` page successfully, if your authenticated user is authorized for that action.</span></span>
4. <span data-ttu-id="43873-256">Most kattintson **forduljon**, ami abban az esetben, ha nem engedélyezhetik **tesztfelhasználó** művelethez.</span><span class="sxs-lookup"><span data-stu-id="43873-256">Now click **Contact**, which in my case should not authorize **Test User** for the action.</span></span> <span data-ttu-id="43873-257">Azonban a böngésző átirányítja az AD FS, végül megjelenítheti az ezt az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="43873-257">However, the browser is redirected to AD FS, which eventually shows this message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    <span data-ttu-id="43873-258">Az AD FS-kiszolgálón az eseménynaplóban hibára vizsgálja meg, a következő kivétel üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="43873-258">If you investigate this error in Event Viewer on the AD FS server, you see this exception message:</span></span>  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>The same client browser session has made '6' requests in the last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    <span data-ttu-id="43873-259">Ez a hiba oka, hogy alapértelmezés szerint MVC esetén ad vissza a 401-es nem engedélyezett a felhasználói szerepkörök nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="43873-259">The reason for this error is that by default, MVC returns a 401 Unauthorized when a user's roles are not authorized.</span></span> <span data-ttu-id="43873-260">Ezzel elindítja az identitásszolgáltató (AD FS) újrahitelesítés kérést.</span><span class="sxs-lookup"><span data-stu-id="43873-260">This triggers a reauthentication request to your identity provider (AD FS).</span></span> <span data-ttu-id="43873-261">Mivel a felhasználó már hitelesítve van, az AD FS visszatér ugyanazon az oldalon, majd állít ki egy másik 401-es, amely egy átirányítási jöjjön létre.</span><span class="sxs-lookup"><span data-stu-id="43873-261">Since the user is already authenticated, AD FS returns to the same page, which then issues another 401, creating a redirect loop.</span></span> <span data-ttu-id="43873-262">AuthorizeAttribute meg fog felülbírálása `HandleUnauthorizedRequest` egyszerű logikával valami megjelenítendő helyett az átirányítási hurok Folytatás legjobb módszer.</span><span class="sxs-lookup"><span data-stu-id="43873-262">You will override AuthorizeAttribute's `HandleUnauthorizedRequest` method with simple logic to show something that makes sense instead of continuing the redirect loop.</span></span>
5. <span data-ttu-id="43873-263">A projekt AuthorizeAttribute.cs nevű fájl létrehozása, és az alábbi kód beillesztése.</span><span class="sxs-lookup"><span data-stu-id="43873-263">Create a file in the project called AuthorizeAttribute.cs, and paste the following code into it.</span></span>
   
        using System;
        using System.Web.Mvc;
        using System.Web.Routing;
   
        namespace WebApp_WSFederation_DotNet
        {
            [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
            public class AuthorizeAttribute : System.Web.Mvc.AuthorizeAttribute
            {
                protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
                {
                    if (filterContext.HttpContext.Request.IsAuthenticated)
                    {
                        filterContext.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
                    }
                    else
                    {
                        base.HandleUnauthorizedRequest(filterContext);
                    }
                }
            }
        }
   
    <span data-ttu-id="43873-264">A felülbírálás kódot küld egy HTTP 403 (tiltott) helyett HTTP 401-es (nem engedélyezett) hitelesített, de nem engedélyezett esetekben.</span><span class="sxs-lookup"><span data-stu-id="43873-264">The override code sends an HTTP 403 (Forbidden) instead of HTTP 401 (Unauthorized) in  authenticated but unauthorized cases.</span></span>
6. <span data-ttu-id="43873-265">Futtassa újra a hibakereső `F5`.</span><span class="sxs-lookup"><span data-stu-id="43873-265">Run the debugger again with `F5`.</span></span> <span data-ttu-id="43873-266">Kattintson a **forduljon** most egy még informatívabbá (ámbár unattractive) hibaüzenetet jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="43873-266">Clicking **Contact** now shows a more informative (albeit unattractive) error message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. <span data-ttu-id="43873-267">Tegye közzé az alkalmazást újra az Azure App Service Web Apps, és az élő alkalmazás működésének teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="43873-267">Publish the application to Azure App Service Web Apps again, and test the behavior of the live application.</span></span>

<a name="bkmk_data"></a>

## <a name="connect-to-on-premises-data"></a><span data-ttu-id="43873-268">Csatlakozás helyszíni adatokhoz</span><span class="sxs-lookup"><span data-stu-id="43873-268">Connect to on-premises data</span></span>
<span data-ttu-id="43873-269">Egy, hogy valósítja meg az üzleti alkalmazás Azure Active Directory helyett AD FS-sel történő kívánt oka szervezeti adatok helyi tartásával megfelelőségi problémákat.</span><span class="sxs-lookup"><span data-stu-id="43873-269">A reason that you would want to implement your line-of-business application with AD FS instead of Azure Active Directory is compliance issues with keeping organization data off-premise.</span></span> <span data-ttu-id="43873-270">Ez azt is jelenti, hogy a webalkalmazás az Azure-ban helyszíni adatbázisokat kell hozzáférni, mert a használata nem engedélyezett [SQL-adatbázis](/services/sql-database/) , az adatok rétegként a webes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="43873-270">This may also mean that your web app in Azure must access on-premises databases, since you are not allowed to use [SQL Database](/services/sql-database/) as the data tier for your web apps.</span></span>

<span data-ttu-id="43873-271">Az Azure App Service Web Apps férnek hozzá a helyszíni adatbázisok esetén két módszer támogatja: [hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és [virtuális hálózatok](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="43873-271">Azure App Service Web Apps supports accessing on-premises databases with two approaches: [Hybrid Connections](../biztalk-services/integration-hybrid-connection-overview.md) and [Virtual Networks](web-sites-integrate-with-vnet.md).</span></span> <span data-ttu-id="43873-272">További információkért lásd: [használó hálózatok integráció és az Azure App Service Web Apps hibrid kapcsolatok](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span><span class="sxs-lookup"><span data-stu-id="43873-272">For more information, see [Using VNET integration and Hybrid connections with Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="43873-273">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="43873-273">Further resources</span></span>
* [<span data-ttu-id="43873-274">A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="43873-274">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="43873-275">Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="43873-275">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>](web-sites-dotnet-lob-application-azure-ad.md)
* [<span data-ttu-id="43873-276">A Visual Studio 2013 ASP.NET használni a helyszíni szervezeti hitelesítési lehetőséget (AD FS)</span><span class="sxs-lookup"><span data-stu-id="43873-276">Use the On-Premises Organizational Authentication Option (ADFS) With ASP.NET in Visual Studio 2013</span></span>](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [<span data-ttu-id="43873-277">VS2013 webes projektet át WIF Katana</span><span class="sxs-lookup"><span data-stu-id="43873-277">Migrate a VS2013 Web Project From WIF to Katana</span></span>](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [<span data-ttu-id="43873-278">Az Active Directory összevonási szolgáltatások áttekintése</span><span class="sxs-lookup"><span data-stu-id="43873-278">Active Directory Federation Services Overview</span></span>](http://technet.microsoft.com/library/hh831502.aspx)
* [<span data-ttu-id="43873-279">WS-Federation 1.1 szabvány</span><span class="sxs-lookup"><span data-stu-id="43873-279">WS-Federation 1.1 specification</span></span>](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)


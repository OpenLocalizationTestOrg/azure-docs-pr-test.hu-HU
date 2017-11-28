---
title: "az AD FS-hitelesítéshez üzleti az Azure alkalmazás aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy üzleti alkalmazást az Azure App Service szolgáltatásban, amely hitelesíti a helyszíni STS. Ez az oktatóanyag az AD FS, a helyszíni STS hello célozza."
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
ms.openlocfilehash: cfd6f14837642fbf58ca5da9dee72db69cd5ab7f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-line-of-business-azure-app-with-ad-fs-authentication"></a><span data-ttu-id="28b56-104">Üzleti Azure-alkalmazás létrehozása az AD FS-hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="28b56-104">Create a line-of-business Azure app with AD FS authentication</span></span>
<span data-ttu-id="28b56-105">Ez a cikk bemutatja, hogyan toocreate egy ASP.NET MVC-üzleti alkalmazás [Azure App Service](../app-service/app-service-value-prop-what-is.md) használatával egy helyszíni [Active Directory összevonási szolgáltatások](http://technet.microsoft.com/library/hh831502.aspx) hello identitás-szolgáltatóként.</span><span class="sxs-lookup"><span data-stu-id="28b56-105">This article shows you how toocreate an ASP.NET MVC line-of-business application in [Azure App Service](../app-service/app-service-value-prop-what-is.md) using an on-premises [Active Directory Federation Services](http://technet.microsoft.com/library/hh831502.aspx) as hello identity provider.</span></span> <span data-ttu-id="28b56-106">Ebben a forgatókönyvben úgy dolgozhatnak, ha azt szeretné, hogy toocreate-üzletági alkalmazások az Azure App Service szolgáltatásban a szervezet azonban előírja directory adatok toobe helyszínen tárolja.</span><span class="sxs-lookup"><span data-stu-id="28b56-106">This scenario can work when you want toocreate line-of-business applications in Azure App Service but your organization requires directory data toobe stored on-site.</span></span>

> [!NOTE]
> <span data-ttu-id="28b56-107">Hello különböző vállalati hitelesítési és engedélyezési az Azure App Service-beállítások áttekintéséhez lásd: [hitelesítés a helyszíni Active Directoryval, az Azure alkalmazásban](web-sites-authentication-authorization.md).</span><span class="sxs-lookup"><span data-stu-id="28b56-107">For an overview of hello different enterprise authentication and authorization options for Azure App Service, see [Authenticate with on-premises Active Directory in your Azure app](web-sites-authentication-authorization.md).</span></span>
> 
> 

<a name="bkmk_build"></a>

## <a name="what-you-will-build"></a><span data-ttu-id="28b56-108">Mit fog létrehozni</span><span class="sxs-lookup"><span data-stu-id="28b56-108">What you will build</span></span>
<span data-ttu-id="28b56-109">A szolgáltatások a következő hello fog létrehozni egy egyszerű ASP.NET-alkalmazás az Azure App Service Web Apps:</span><span class="sxs-lookup"><span data-stu-id="28b56-109">You will build a basic ASP.NET application in Azure App Service Web Apps with hello following features:</span></span>

* <span data-ttu-id="28b56-110">Hitelesíti a felhasználókat az AD FS ellen</span><span class="sxs-lookup"><span data-stu-id="28b56-110">Authenticates users against AD FS</span></span>
* <span data-ttu-id="28b56-111">Használja a `[Authorize]` tooauthorize felhasználók a különböző műveletek</span><span class="sxs-lookup"><span data-stu-id="28b56-111">Uses `[Authorize]` tooauthorize users for different actions</span></span>
* <span data-ttu-id="28b56-112">Statikus konfigurálásához a Visual Studio hibakeresési és közzétételi tooApp Service Web Apps (csak egyszer konfigurálnia, hibakeresését és bármikor közzététele)</span><span class="sxs-lookup"><span data-stu-id="28b56-112">Static configuration for both debugging in Visual Studio and publishing tooApp Service Web Apps (configure once, debug and publish anytime)</span></span>  

<a name="bkmk_need"></a>

## <a name="what-you-need"></a><span data-ttu-id="28b56-113">Mi szükséges</span><span class="sxs-lookup"><span data-stu-id="28b56-113">What you need</span></span>
[!INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

<span data-ttu-id="28b56-114">Meg kell hello toocomplete követően ez az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="28b56-114">You need hello following toocomplete this tutorial:</span></span>

* <span data-ttu-id="28b56-115">Egy helyszíni AD FS üzembe helyezése (ebben az oktatóanyagban használt hello tesztkörnyezet végpont útmutatót lásd: [tesztlabor: az AD FS az Azure virtuális gép (csak tesztelési) önálló STS](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span><span class="sxs-lookup"><span data-stu-id="28b56-115">An on-premises AD FS deployment (for an end-to-end walkthrough of hello test lab used in this tutorial, see [Test Lab: Standalone STS with AD FS in Azure VM (for test only)](https://blogs.msdn.microsoft.com/cephalin/2014/12/21/test-lab-standalone-sts-with-ad-fs-in-azure-vm-for-test-only/))</span></span>
* <span data-ttu-id="28b56-116">Engedélyek toocreate függő entitás megbízhatóságai AD FS felügyelet</span><span class="sxs-lookup"><span data-stu-id="28b56-116">Permissions toocreate relying party trusts in AD FS Management</span></span>
* <span data-ttu-id="28b56-117">A Visual Studio 2013 Update 4 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="28b56-117">Visual Studio 2013 Update 4 or later</span></span>
* <span data-ttu-id="28b56-118">[Az Azure SDK 2.8.1-es verziójának](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="28b56-118">[Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) or later</span></span>

<a name="bkmk_sample"></a>

## <a name="use-sample-application-for-line-of-business-template"></a><span data-ttu-id="28b56-119">Mintaalkalmazást használ üzleti sablon</span><span class="sxs-lookup"><span data-stu-id="28b56-119">Use sample application for line-of-business template</span></span>
<span data-ttu-id="28b56-120">Ebben az oktatóanyagban mintaalkalmazás hello [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), hello Azure Active Directory csapata hozza létre.</span><span class="sxs-lookup"><span data-stu-id="28b56-120">hello sample application in this tutorial, [WebApp-WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), is created by hello Azure Active Directory team.</span></span> <span data-ttu-id="28b56-121">Mivel az AD FS támogatja a WS-Federation, használhatja egy sablon toocreate üzleti alkalmazásokként alkalmazásaiba.</span><span class="sxs-lookup"><span data-stu-id="28b56-121">Since AD FS supports WS-Federation, you can use it as a template toocreate line-of-business applications with ease.</span></span> <span data-ttu-id="28b56-122">A következő funkciók hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="28b56-122">It has hello following features:</span></span>

* <span data-ttu-id="28b56-123">Használja a [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate egy helyszíni AD FS üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="28b56-123">Uses [WS-Federation](http://msdn.microsoft.com/library/bb498017.aspx) tooauthenticate with an on-premises AD FS deployment</span></span>
* <span data-ttu-id="28b56-124">Be- és kijelentkezési funkció</span><span class="sxs-lookup"><span data-stu-id="28b56-124">Sign-in and sign-out functionality</span></span>
* <span data-ttu-id="28b56-125">Használja a [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (nem Windows Identity Foundation), amely van hello jövőbeli az ASP.NET és a hitelesítési és engedélyezési feliratkozott jóval egyszerűbb tooset WIF-nál</span><span class="sxs-lookup"><span data-stu-id="28b56-125">Uses [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (instead of Windows Identity Foundation), which is hello future of ASP.NET and much simpler tooset up for authentication and authorization than WIF</span></span>

<a name="bkmk_setup"></a>

## <a name="set-up-hello-sample-application"></a><span data-ttu-id="28b56-126">Hello mintaalkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="28b56-126">Set up hello sample application</span></span>
1. <span data-ttu-id="28b56-127">Klónozza, vagy töltse le a hello minta megoldást a következő [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="28b56-127">Clone or download hello sample solution at [WebApp-WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) tooyour local directory.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="28b56-128">útmutatás hello [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) mutatja be tooset mentése hello alkalmazás Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="28b56-128">hello instructions at [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) show you how tooset up hello application with Azure Active Directory.</span></span> <span data-ttu-id="28b56-129">Azonban ebben az oktatóanyagban, állítsa be az AD FS-sel, ezért a lépések hello itt helyette.</span><span class="sxs-lookup"><span data-stu-id="28b56-129">But in this tutorial, you set it up with AD FS, so follow hello steps here instead.</span></span>
   > 
   > 
2. <span data-ttu-id="28b56-130">Nyissa meg a hello megoldás, és nyissa meg a hello Controllers\AccountController.cs **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="28b56-130">Open hello solution, and then open Controllers\AccountController.cs in hello **Solution Explorer**.</span></span>
   
   <span data-ttu-id="28b56-131">Látni fogja, hogy egyszerűen hello kódot állít ki egy hitelesítési kérdés tooauthenticate hello felhasználó WS-Federation használatával.</span><span class="sxs-lookup"><span data-stu-id="28b56-131">You will see that hello code simply issues an authentication challenge tooauthenticate hello user using WS-Federation.</span></span> <span data-ttu-id="28b56-132">Minden hitelesítési App_Start\Startup.Auth.cs van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="28b56-132">All authentication is configured in App_Start\Startup.Auth.cs.</span></span>
3. <span data-ttu-id="28b56-133">Nyissa meg a App_Start\Startup.Auth.cs.</span><span class="sxs-lookup"><span data-stu-id="28b56-133">Open App_Start\Startup.Auth.cs.</span></span> <span data-ttu-id="28b56-134">A hello `ConfigureAuth` metódust, Megjegyzés hello sor:</span><span class="sxs-lookup"><span data-stu-id="28b56-134">In hello `ConfigureAuth` method, note hello line:</span></span>
   
       app.UseWsFederationAuthentication(
           new WsFederationAuthenticationOptions
           {
               Wtrealm = realm,
               MetadataAddress = metadata                                      
           });
   
   <span data-ttu-id="28b56-135">Hello OWIN világában a részlet valóban operációs rendszer minimálisan szükséges tooconfigure WS-Federation hitelesítési hello.</span><span class="sxs-lookup"><span data-stu-id="28b56-135">In hello OWIN world, this snippet is really hello bare minimum you need tooconfigure WS-Federation authentication.</span></span> <span data-ttu-id="28b56-136">Már jóval egyszerűbb, több elegáns, mint a WIF, ahol Web.config van beszúrta XML hello hely számos országában dolgoznak.</span><span class="sxs-lookup"><span data-stu-id="28b56-136">It is much simpler and more elegant than WIF, where Web.config is injected with XML all over hello place.</span></span> <span data-ttu-id="28b56-137">csak a szükséges információkat hello megbízó fél (RP) hello azonosítója és hello URL-címe az AD FS szolgáltatást metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="28b56-137">hello only information you need is hello relying party's (RP) identifier and hello URL of your AD FS service's metadata file.</span></span> <span data-ttu-id="28b56-138">Íme egy példa:</span><span class="sxs-lookup"><span data-stu-id="28b56-138">Here's an example:</span></span>
   
   * <span data-ttu-id="28b56-139">Függő Entitás azonosítója:`https://contoso.com/MyLOBApp`</span><span class="sxs-lookup"><span data-stu-id="28b56-139">RP identifier: `https://contoso.com/MyLOBApp`</span></span>
   * <span data-ttu-id="28b56-140">Metaadatok címe:`http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span><span class="sxs-lookup"><span data-stu-id="28b56-140">Metadata address: `http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`</span></span>
4. <span data-ttu-id="28b56-141">App_Start\Startup.Auth.cs módosítsa a következő statikus karakterlánc-definíciók hello:</span><span class="sxs-lookup"><span data-stu-id="28b56-141">In App_Start\Startup.Auth.cs, change hello following static string definitions:</span></span>  
   
   <pre class="prettyprint">
   private static string realm = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
   <mark><del>private static string aadInstance = ConfigurationManager.AppSettings["ida:AADInstance"];</del></mark>
   <mark><del>private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];</del></mark>
   <mark><del>private static string metadata = string.Format("{0}/{1}/federationmetadata/2007-06/federationmetadata.xml", aadInstance, tenant);</del></mark>
   <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>
   
   <mark><del>string authority = String.Format(CultureInfo.InvariantCulture, aadInstance, tenant);</del></mark>
   </pre>
5. <span data-ttu-id="28b56-142">Most módosításokat hello megfelelő a Web.config fájlban. Nyissa meg a hello Web.config, és módosítsa a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="28b56-142">Now, make hello corresponding changes in Web.config. Open hello Web.config and modify hello following app settings:</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="webpages:Version" value="3.0.0.0" /&gt;
   &lt;add key="webpages:Enabled" value="false" /&gt;
   &lt;add key="ClientValidationEnabled" value="true" /&gt;
   &lt;add key="UnobtrusiveJavaScriptEnabled" value="true" /&gt;
   <mark><del>&lt;add key="ida:Wtrealm" value="[Enter hello App ID URI of WebApp-WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet]" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:AADInstance" value="https://login.microsoftonline.com" /&gt;</del></mark>
   <mark><del>&lt;add key="ida:Tenant" value="[Enter tenant name, e.g. contoso.onmicrosoft.com]" /&gt;</del></mark>
   <mark>&lt;add key="ida:RPIdentifier" value="[Enter hello relying party identifier as configured in AD FS, e.g. https://localhost:44320/]" /&gt;</mark>
   <mark>&lt;add key="ida:ADFS" value="[Enter hello FQDN of AD FS service, e.g. adfs.contoso.com]" /&gt;</mark>
   
   &lt;/appSettings&gt;
   </pre>
   
   <span data-ttu-id="28b56-143">Töltse ki a megfelelő környezet alapján hello kulcsértékei.</span><span class="sxs-lookup"><span data-stu-id="28b56-143">Fill in hello key values based on your respective environment.</span></span>
6. <span data-ttu-id="28b56-144">Build hello alkalmazás toomake meg arról, hogy nincsenek hibák.</span><span class="sxs-lookup"><span data-stu-id="28b56-144">Build hello application toomake sure there are no errors.</span></span>

<span data-ttu-id="28b56-145">Ennyi az egész.</span><span class="sxs-lookup"><span data-stu-id="28b56-145">That's it.</span></span> <span data-ttu-id="28b56-146">Hello mintaalkalmazást most már készen áll a toowork az AD FS áll.</span><span class="sxs-lookup"><span data-stu-id="28b56-146">Now hello sample application is ready toowork with AD FS.</span></span> <span data-ttu-id="28b56-147">Szüksége van egy függő Entitás megbízhatósági kapcsolatban áll az alkalmazás később az AD FS tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="28b56-147">You still need tooconfigure an RP trust with this application in AD FS later.</span></span>

<a name="bkmk_deploy"></a>

## <a name="deploy-hello-sample-application-tooazure-app-service-web-apps"></a><span data-ttu-id="28b56-148">Hello minta alkalmazás tooAzure App Service Web Apps telepítése</span><span class="sxs-lookup"><span data-stu-id="28b56-148">Deploy hello sample application tooAzure App Service Web Apps</span></span>
<span data-ttu-id="28b56-149">Itt közzéteszi hello alkalmazás tooa webalkalmazást az App Service Web Apps hello hibakeresési környezetben adatainak megőrzése mellett.</span><span class="sxs-lookup"><span data-stu-id="28b56-149">Here, you publish hello application tooa web app in App Service Web Apps while preserving hello debug environment.</span></span> <span data-ttu-id="28b56-150">Vegye figyelembe, hogy fog toopublish hello alkalmazás rendelkezik egy függő Entitás megbízhatóság az AD FS-sel, így a hitelesítés továbbra sem működik még előtt.</span><span class="sxs-lookup"><span data-stu-id="28b56-150">Note that you're going toopublish hello application before it has an RP trust with AD FS, so authentication still doesn't work yet.</span></span> <span data-ttu-id="28b56-151">Azonban ha így tesz, akkor most lehet hello webes URL-címet, hogy tooconfigure hello RP megbízhatósági később használhatja.</span><span class="sxs-lookup"><span data-stu-id="28b56-151">However, if you do it now you can have hello web app URL that you can use tooconfigure hello RP trust later.</span></span>

1. <span data-ttu-id="28b56-152">Kattintson jobb gombbal a projektre, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="28b56-152">Right-click your project and select **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)
2. <span data-ttu-id="28b56-153">Válassza ki **Microsoft Azure App Service**.</span><span class="sxs-lookup"><span data-stu-id="28b56-153">Select **Microsoft Azure App Service**.</span></span>
3. <span data-ttu-id="28b56-154">Ha még nem jelentkezett tooAzure, kattintson a **bejelentkezés** és az Azure-előfizetés toosign a hello Microsoft-fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="28b56-154">If you haven't signed in tooAzure, click **Sign In** and use hello Microsoft account for your Azure subscription toosign in.</span></span>
4. <span data-ttu-id="28b56-155">Miután bejelentkezett, kattintson a **új** toocreate egy webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="28b56-155">Once signed in, click **New** toocreate a web app.</span></span>
5. <span data-ttu-id="28b56-156">Töltse ki az összes kötelező mezőt.</span><span class="sxs-lookup"><span data-stu-id="28b56-156">Fill in all required fields.</span></span> <span data-ttu-id="28b56-157">Tooconnect tooon helyszíni adatokhoz készül később, ezért ne hozzon létre egy adatbázist a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="28b56-157">You are going tooconnect tooon-premises data later, so don't create a database for this web app.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)
6. <span data-ttu-id="28b56-158">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="28b56-158">Click **Create**.</span></span> <span data-ttu-id="28b56-159">Hello webalkalmazás létrehozása után hello webhely közzététele párbeszédpanelen van megnyitva.</span><span class="sxs-lookup"><span data-stu-id="28b56-159">Once hello web app is created, hello Publish Web dialog is opened.</span></span>
7. <span data-ttu-id="28b56-160">A **URL-címre**, módosítsa **http** túl**https**.</span><span class="sxs-lookup"><span data-stu-id="28b56-160">In **Destination URL**, change **http** too**https**.</span></span> <span data-ttu-id="28b56-161">Másolja a hello teljes URL-cím tooa szövegszerkesztőben későbbi használatra.</span><span class="sxs-lookup"><span data-stu-id="28b56-161">Copy hello entire URL tooa text editor for later use.</span></span> <span data-ttu-id="28b56-162">Kattintson a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="28b56-162">Then, click **Publish**.</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)
8. <span data-ttu-id="28b56-163">A Visual Studióban nyissa meg a **Web.Release.config** a projektben.</span><span class="sxs-lookup"><span data-stu-id="28b56-163">In Visual Studio, open **Web.Release.config** in your project.</span></span> <span data-ttu-id="28b56-164">Helyezze be a hello XML hello be a következő `<configuration>` címkét, és hello kulcsérték cserélje le a közzétételi webes alkalmazás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="28b56-164">Insert hello following XML into hello `<configuration>` tag, and replace hello key value with your publish web app's URL.</span></span>  
   
   <pre class="prettyprint">
   &lt;appSettings&gt;
   &lt;add key="ida:RPIdentifier" value="<mark>[e.g. https://mylobapp.azurewebsites.net/]</mark>" xdt:Transform="SetAttributes" xdt:Locator="Match(key)" /&gt;
   &lt;/appSettings&gt;</pre>

<span data-ttu-id="28b56-165">Amikor elkészült, a projekt egy Visual Studio, a hibakeresési környezetben a konfigurált két RP azonosítók rendelkezik, és egy hello a közzétett webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="28b56-165">When you're done, you have two RP identifiers configured in your project, one for your debug environment in Visual Studio, and one for hello published web app in Azure.</span></span> <span data-ttu-id="28b56-166">Az egyes hello két környezetet az AD FS fog beállítani egy függő Entitás megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="28b56-166">You will set up an RP trust for each of hello two environments in AD FS.</span></span> <span data-ttu-id="28b56-167">Hibakeresési, hello Alkalmazásbeállítások a Web.config fájlban használt toomake a **Debug** az AD FS konfigurációs munka.</span><span class="sxs-lookup"><span data-stu-id="28b56-167">During debugging, hello app settings in Web.config are used toomake your **Debug** configuration work with AD FS.</span></span> <span data-ttu-id="28b56-168">Ha közzé van téve (alapértelmezés szerint hello **kiadás** konfigurációs van közzétéve), átalakított Web.config tölt fel, amely magában foglalja a hello app beállítás változása Web.Release.config.</span><span class="sxs-lookup"><span data-stu-id="28b56-168">When it's published (by default, hello **Release** configuration is published), a transformed Web.config is uploaded that incorporates hello app setting changes in Web.Release.config.</span></span>

<span data-ttu-id="28b56-169">Ha a kívánt tooattach hello a közzétett webes alkalmazás Azure toohello hibakereső (vagyis fel kell töltenie a kód hello a közzétett webes alkalmazás hibakeresési szimbólumok), akkor is klónozhatja hello hibakeresése az Azure-hibakeresés konfigurációs, de a saját egyéni Web.config átalakítás) pl. Web.AzureDebug.config) használó Web.Release.config hello alkalmazás beállításait. Ez lehetővé teszi toomaintain statikus hello különböző környezetek között.</span><span class="sxs-lookup"><span data-stu-id="28b56-169">If you want tooattach hello published web app in Azure toohello debugger (i.e. you must upload debug symbols of your code in hello published web app), you can create a clone of hello Debug configuration for Azure debugging, but with its own custom Web.config transform (e.g. Web.AzureDebug.config) that uses hello app settings from Web.Release.config. This allows you toomaintain a static configuration across hello different environments.</span></span>

<a name="bkmk_rptrusts"></a>

## <a name="configure-relying-party-trusts-in-ad-fs-management"></a><span data-ttu-id="28b56-170">Megbízható függő entitások AD FS felügyelet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="28b56-170">Configure relying party trusts in AD FS Management</span></span>
<span data-ttu-id="28b56-171">A továbbiakban létre kell egy függő Entitás megbízhatóságának AD FS felügyeleti tooconfigure előtt használja a mintaalkalmazást, és az AD FS ténylegesen hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="28b56-171">Now you need tooconfigure an RP trust in AD FS Management before you can use your sample application and actually authenticate with AD FS.</span></span> <span data-ttu-id="28b56-172">Szüksége lesz a tooset két külön RP bizalmi kapcsolatok mentése, egy, a hibakeresés környezethez és egy közzétett webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="28b56-172">You will need tooset up two separate RP trusts, one for your debug environment and one for your published web app.</span></span>

> [!NOTE]
> <span data-ttu-id="28b56-173">Győződjön meg arról, hogy mind a környezetek lépések hello ismételje.</span><span class="sxs-lookup"><span data-stu-id="28b56-173">Make sure that you repeat hello following steps for both of your environments.</span></span>
> 
> 

1. <span data-ttu-id="28b56-174">Az AD FS-kiszolgálón jelentkezzen be a hitelesítő adatokat, amelyek felügyeleti jogok tooAD FS.</span><span class="sxs-lookup"><span data-stu-id="28b56-174">On your AD FS server, log in with credentials that have management rights tooAD FS.</span></span>
2. <span data-ttu-id="28b56-175">Nyissa meg az AD FS felügyeleti konzolt.</span><span class="sxs-lookup"><span data-stu-id="28b56-175">Open AD FS Management.</span></span> <span data-ttu-id="28b56-176">Kattintson a jobb gombbal **AD FS\Trusted Relationships\Relying entitások Megbízhatóságai** válassza **függő entitás megbízhatóságának hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="28b56-176">Right-click **AD FS\Trusted Relationships\Relying Party Trusts** and select **Add Relying Party Trust**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)
3. <span data-ttu-id="28b56-177">A hello **adatforrás kiválasztása** lapon, hogy melyik **hello függő entitással kapcsolatos adatok manuális megadása**.</span><span class="sxs-lookup"><span data-stu-id="28b56-177">In hello **Select Data Source** page, select **Enter data about hello relying party manually**.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)
4. <span data-ttu-id="28b56-178">A hello **megjelenítendő név megadása** lapon adjon meg egy megjelenítési nevet hello alkalmazáshoz, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="28b56-178">In hello **Specify Display Name** page, type a display name for hello application and click **Next**.</span></span>
5. <span data-ttu-id="28b56-179">A hello **válasszon protokoll** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="28b56-179">In hello **Choose Protocol** page, click **Next**.</span></span>
6. <span data-ttu-id="28b56-180">A hello **tanúsítvány konfigurálása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="28b56-180">In hello **Configure Certificate** page, click **Next**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="28b56-181">Mivel kell használni HTTPS már, titkosított jogkivonatok nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="28b56-181">Since you should be using HTTPS already, encrypted tokens are optional.</span></span> <span data-ttu-id="28b56-182">Ha valóban tooencrypt jogkivonatok az AD FS ezen a lapon, jogkivonat-visszafejtési logikát is hozzá kell adnia a kódot.</span><span class="sxs-lookup"><span data-stu-id="28b56-182">If you really want tooencrypt tokens from AD FS on this page, you must also add token-decrypting logic in your code.</span></span> <span data-ttu-id="28b56-183">További információkért lásd: [manuálisan titkosítja az OWIN WS-Federation köztes konfigurálása és elfogadása jogkivonatok](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span><span class="sxs-lookup"><span data-stu-id="28b56-183">For more information, see [Manually configuring OWIN WS-Federation middleware and accepting encrypted tokens](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx).</span></span>
   > 
   > 
7. <span data-ttu-id="28b56-184">Mielőtt alakzatot hello következő lépésre viszi, a Visual Studio-projektet a kell információ.</span><span class="sxs-lookup"><span data-stu-id="28b56-184">Before you move onto hello next step, you need one piece of information from your Visual Studio project.</span></span> <span data-ttu-id="28b56-185">Hello projekt tulajdonságai, vegye figyelembe a hello **SSL URL-cím** hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="28b56-185">In hello project properties, note hello **SSL URL** of hello application.</span></span> 
   
   ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)
8. <span data-ttu-id="28b56-186">Vissza az AD FS-kezelés hello **URL konfigurálása** hello oldalán **függő entitás megbízhatóságának hozzáadása varázsló**, jelölje be **hello WS-Federation passzív protokoll támogatásának engedélyezése a** és Írja be a hello SSL URL-címet, a Visual Studio-projekt, hogy hello előző lépésben feljegyzett.</span><span class="sxs-lookup"><span data-stu-id="28b56-186">Back in AD FS Management, in hello **Configure URL** page of hello **Add Relying Party Trust Wizard**, select **Enable support for hello WS-Federation Passive protocol** and type in hello SSL URL of your Visual Studio project that you noted in hello previous step.</span></span> <span data-ttu-id="28b56-187">Ezután kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="28b56-187">Then, click **Next**.</span></span>
   
   ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)
   
   > [!NOTE]
   > <span data-ttu-id="28b56-188">URL-CÍMÉT határozza meg, ahol toosend hello ügyfél hitelesítést követően sikeres lesz.</span><span class="sxs-lookup"><span data-stu-id="28b56-188">URL specifies where toosend hello client after authentication succeeds.</span></span> <span data-ttu-id="28b56-189">Hello hibakeresés környezethez kell <code>https://localhost:&lt;port&gt;/</code>.</span><span class="sxs-lookup"><span data-stu-id="28b56-189">For hello debug environment, it should be <code>https://localhost:&lt;port&gt;/</code>.</span></span> <span data-ttu-id="28b56-190">Hello közzétett webalkalmazás hello webes alkalmazás URL-CÍMÉT kell lennie.</span><span class="sxs-lookup"><span data-stu-id="28b56-190">For hello published web app, it should be hello web app URL.</span></span>
   > 
   > 
9. <span data-ttu-id="28b56-191">A hello **azonosítók konfigurálása** lapon ellenőrizze, hogy a projekt SSL URL-cím már szerepel-e, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="28b56-191">In hello **Configure Identifiers** page, verify that your project SSL URL is already listed and click **Next**.</span></span> <span data-ttu-id="28b56-192">Kattintson a **következő** összes hello módon toohello vége hello varázsló az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="28b56-192">Click **Next** all hello way toohello end of hello wizard with default selections.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="28b56-193">Ez az azonosító App_Start\Startup.Auth.cs a Visual Studio-projekt, a elleni hello értéke egyezik <code>WsFederationAuthenticationOptions.Wtrealm</code> összevont hitelesítés során.</span><span class="sxs-lookup"><span data-stu-id="28b56-193">In App_Start\Startup.Auth.cs of your Visual Studio project, this identifier is matched against hello value of <code>WsFederationAuthenticationOptions.Wtrealm</code> during federated authentication.</span></span> <span data-ttu-id="28b56-194">Alapértelmezés szerint hello előző lépésben hello alkalmazás URL-címe meg van adva egy függő Entitás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="28b56-194">By default, hello application's URL from hello previous step is added as an RP identifier.</span></span>
   > 
   > 
10. <span data-ttu-id="28b56-195">A projekthez RP alkalmazás hello beállítása az AD FS most után.</span><span class="sxs-lookup"><span data-stu-id="28b56-195">You have now finished configuring hello RP application for your project in AD FS.</span></span> <span data-ttu-id="28b56-196">Ezután állítsa be az alkalmazás toosend hello az alkalmazás által igényelt jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="28b56-196">Next, you configure this application toosend hello claims needed by your application.</span></span> <span data-ttu-id="28b56-197">Hello **Jogcímszabályok szerkesztése** párbeszédpanelen alapértelmezés szerint megnyílik az Ön hello varázsló hello végén, és azonnal megkezdje.</span><span class="sxs-lookup"><span data-stu-id="28b56-197">hello **Edit Claim Rules** dialog is opened by default for you at hello end of hello wizard so you can start immediately.</span></span> <span data-ttu-id="28b56-198">Pedig konfiguráljuk legalább hello követően a jogcímeket (zárójelben sémák):</span><span class="sxs-lookup"><span data-stu-id="28b56-198">Let's configure at least hello following claims (with schemas in parentheses):</span></span>
    
    * <span data-ttu-id="28b56-199">ASP.NET toohydrate által használt név (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - `User.Identity.Name`.</span><span class="sxs-lookup"><span data-stu-id="28b56-199">Name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - used by ASP.NET toohydrate `User.Identity.Name`.</span></span>
    * <span data-ttu-id="28b56-200">Felhasználó egyszerű felhasználónév (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - használt toouniquely hello szervezethez tartozó felhasználók azonosításához.</span><span class="sxs-lookup"><span data-stu-id="28b56-200">User principal name (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - used toouniquely identify users in hello organization.</span></span>
    * <span data-ttu-id="28b56-201">A csoporttagságokat, szerepkörök (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - használható `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize tartományvezérlők/művelet.</span><span class="sxs-lookup"><span data-stu-id="28b56-201">Group memberships as roles (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - can be used with `[Authorize(Roles="role1, role2,...")]` decoration tooauthorize controllers/actions.</span></span> <span data-ttu-id="28b56-202">A valóságban ez a megközelítés lehet, hogy nem kell hello legtöbb performant a szerepkör-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="28b56-202">In reality, this approach may not be hello most performant for role authorization.</span></span> <span data-ttu-id="28b56-203">Ha az AD-felhasználók biztonsági csoportjainak toohundreds tartozik, több száz szerepkör jogcím szerepel hello SAML-jogkivonat válnak.</span><span class="sxs-lookup"><span data-stu-id="28b56-203">If your AD users belong toohundreds of security groups, they become hundreds of role claims in hello SAML token.</span></span> <span data-ttu-id="28b56-204">Egy másik módszert toosend egyetlen szerepkör jogcím feltételesen attól függően, hogy egy adott csoport hello felhasználó tagja.</span><span class="sxs-lookup"><span data-stu-id="28b56-204">An alternative approach is toosend a single role claim conditionally depending on hello user's membership in a particular group.</span></span> <span data-ttu-id="28b56-205">Azonban szavatoljuk az egyszerű ehhez az oktatóanyaghoz.</span><span class="sxs-lookup"><span data-stu-id="28b56-205">However, we'll keep it simple for this tutorial.</span></span>
    * <span data-ttu-id="28b56-206">Azonosító (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) Name,-hamisítás érvényesítéshez használható.</span><span class="sxs-lookup"><span data-stu-id="28b56-206">Name ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - can be used for anti-forgery validation.</span></span> <span data-ttu-id="28b56-207">A toomake azt működése hamisítás érvényesítéssel további információkért lásd: hello **üzleti funkciókkal** szakasza [üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés ](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span><span class="sxs-lookup"><span data-stu-id="28b56-207">For more information on how toomake it work with anti-forgery validation, see hello **Add line-of-business functionality** section of [Create a line-of-business Azure app with Azure Active Directory authentication](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="28b56-208">hello jogcím típusokat, tooconfigure szükséges az alkalmazás határozza meg az alkalmazás igényeinek.</span><span class="sxs-lookup"><span data-stu-id="28b56-208">hello claim types you need tooconfigure for your application is determined by your application's needs.</span></span> <span data-ttu-id="28b56-209">Azure Active Directory-alkalmazások (azaz a függő Entitás megbízhatósági kapcsolatok) által támogatott jogcímek hello listája, például: [támogatott jogkivonat és jogcímtípusok](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span><span class="sxs-lookup"><span data-stu-id="28b56-209">For hello list of claims supported by Azure Active Directory applications (i.e. RP trusts), for example, see [Supported Token and Claim Types](http://msdn.microsoft.com/library/azure/dn195587.aspx).</span></span>
    > 
    > 
11. <span data-ttu-id="28b56-210">Hello Jogcímszabályok szerkesztése párbeszédpanelen kattintson a **szabály hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="28b56-210">In hello Edit Claim Rules dialog, click **Add Rule**.</span></span>
12. <span data-ttu-id="28b56-211">Hello nevét, az egyszerű felhasználónév és a szerepkör jogcím konfigurálása hello képernyőfelvételen látható módon, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="28b56-211">Configure hello name, UPN, and role claims as shown in hello screenshot and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)
    
    <span data-ttu-id="28b56-212">Ezután hozzon létre egy átmeneti nevet azonosító jogcím használatával hello lépéseket mutatja be [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="28b56-212">Next, you create a transient name ID claim using hello steps demonstrated in [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
13. <span data-ttu-id="28b56-213">Kattintson a **szabály hozzáadása** újra.</span><span class="sxs-lookup"><span data-stu-id="28b56-213">Click **Add Rule** again.</span></span>
14. <span data-ttu-id="28b56-214">Válassza ki **jogcímek küldése egyéni szabály segítségével** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="28b56-214">Select **Send Claims Using a Custom Rule** and click **Next**.</span></span>
15. <span data-ttu-id="28b56-215">Beillesztés hello következő nyelvi szabály be hello **egyéni szabály** mezőbe, a név hello szabály **munkamenet azonosítója** kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="28b56-215">Paste hello following rule language into hello **Custom rule** box, name hello rule **Per Session Identifier** and click **Finish**.</span></span>  
    
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
    
    <span data-ttu-id="28b56-216">Az egyéni szabály ezen a képernyőfelvételen látható hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="28b56-216">Your custom rule should look like this screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)
16. <span data-ttu-id="28b56-217">Kattintson a **szabály hozzáadása** újra.</span><span class="sxs-lookup"><span data-stu-id="28b56-217">Click **Add Rule** again.</span></span>
17. <span data-ttu-id="28b56-218">Válassza ki **bejövő jogcím átalakítása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="28b56-218">Select **Transform an Incoming Claim** and click **Next**.</span></span>
18. <span data-ttu-id="28b56-219">Hello szabály konfigurálása (használatával létrehozott egyéni szabály hello hello jogcímtípus) hello képernyőfelvételen látható módon, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="28b56-219">Configure hello rule as shown in hello screenshot (using hello claim type you created in hello custom rule) and click **Finish**.</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)
    
    <span data-ttu-id="28b56-220">A hello lépésekre hello átmeneti Névazonosító jogcímet részletes információkért lásd: [azonosítókat a SAML helyességi feltételek](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span><span class="sxs-lookup"><span data-stu-id="28b56-220">For detailed information on hello steps for hello transient Name ID claim, see [Name Identifiers in SAML assertions](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx).</span></span>
19. <span data-ttu-id="28b56-221">Kattintson a **alkalmaz** a hello **Jogcímszabályok szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="28b56-221">Click **Apply** in hello **Edit Claim Rules** dialog.</span></span> <span data-ttu-id="28b56-222">Mostantól a következő képernyőkép hello hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="28b56-222">It should now look like hello following screenshot:</span></span>
    
    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)
    
    > [!NOTE]
    > <span data-ttu-id="28b56-223">Ebben az esetben győződjön meg arról, hogy a hibakeresési környezetben és a közzétett webes alkalmazás ismételje meg ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="28b56-223">Again, make sure that you repeat these steps for both your debug environment and published web app.</span></span>
    > 
    > 

<a name="bkmk_test"></a>

## <a name="test-federated-authentication-for-your-application"></a><span data-ttu-id="28b56-224">Az alkalmazás összevont hitelesítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="28b56-224">Test federated authentication for your application</span></span>
<span data-ttu-id="28b56-225">Akkor van kész tootest az alkalmazás hitelesítési logikát szemben az AD FS.</span><span class="sxs-lookup"><span data-stu-id="28b56-225">You are ready tootest your application's authentication logic against AD FS.</span></span> <span data-ttu-id="28b56-226">Az AD FS laborkörnyezetben tooa teszt csoportot az Active Directory (AD) tartozó tesztfelhasználó van.</span><span class="sxs-lookup"><span data-stu-id="28b56-226">In my AD FS lab environment, I have a test user that belongs tooa test group in Active Directory (AD).</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

<span data-ttu-id="28b56-227">hello hibakeresőben, most szüksége toodo tootest hitelesítési típus `F5`.</span><span class="sxs-lookup"><span data-stu-id="28b56-227">tootest authentication in hello debugger, all you need toodo now is type `F5`.</span></span> <span data-ttu-id="28b56-228">Ha azt szeretné, hogy a közzétett webalkalmazás hello tootest hitelesítési, keresse meg a toohello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="28b56-228">If you want tootest authentication in hello published web app, navigate toohello URL.</span></span>

<span data-ttu-id="28b56-229">Hello webalkalmazás betöltése után kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="28b56-229">After hello web application loads, click **Sign In**.</span></span> <span data-ttu-id="28b56-230">Most vagy egy bejelentkezési párbeszédpanel vagy hello bejelentkezési oldalt az AD FS által megadott hello hitelesítési módszertől függően az AD FS által kiszolgált szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="28b56-230">You should now get either a login dialog or hello login page served by AD FS, depending on hello authentication method chosen by AD FS.</span></span> <span data-ttu-id="28b56-231">Ez mit jelenik meg az Internet Explorer 11.</span><span class="sxs-lookup"><span data-stu-id="28b56-231">Here's what I get in Internet Explorer 11.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

<span data-ttu-id="28b56-232">Hello AD-tartomány hello AD FS központi telepítés a felhasználó jelentkezik, ha meg kell jelennie hello kezdőlap újra **Hello, <User Name>!**</span><span class="sxs-lookup"><span data-stu-id="28b56-232">Once you log in with a user in hello AD domain of hello AD FS deployment, you should now see hello homepage again with **Hello, <User Name>!**</span></span> <span data-ttu-id="28b56-233">hello sarokban.</span><span class="sxs-lookup"><span data-stu-id="28b56-233">in hello corner.</span></span> <span data-ttu-id="28b56-234">Ez mit jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="28b56-234">Here's what I get.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

<span data-ttu-id="28b56-235">Az eddigi sikeresen hello a következő módon:</span><span class="sxs-lookup"><span data-stu-id="28b56-235">So far, you've succeeded in hello following ways:</span></span>

* <span data-ttu-id="28b56-236">Az alkalmazás sikeresen elérte az AD FS és a megfelelő függő Entitás azonosítója hello AD FS adatbázisában található</span><span class="sxs-lookup"><span data-stu-id="28b56-236">Your application has successfully reached AD FS and a matching RP identifier is found in hello AD FS database</span></span>
* <span data-ttu-id="28b56-237">AD FS sikeresen hitelesítette egy AD-felhasználó és az átirányítási biztonsági toohello alkalmazás kezdőlap</span><span class="sxs-lookup"><span data-stu-id="28b56-237">AD FS has successfully authenticated an AD user and redirect you back toohello application's homepage</span></span>
* <span data-ttu-id="28b56-238">Az AD FS elküldése sikerült hello neveként (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour alkalmazás, jogcím-hello tény jelöli, hogy hello a felhasználó neve hello sarokban jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="28b56-238">AD FS as successfully sent hello name claim (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) tooyour application, as indicated by hello fact that hello user name is displayed in hello corner.</span></span> 

<span data-ttu-id="28b56-239">Ha hello név jogcímet hiányzik, akkor amint láthatta **Hello,!**.</span><span class="sxs-lookup"><span data-stu-id="28b56-239">If hello name claim is missing, you would have seen **Hello, !**.</span></span> <span data-ttu-id="28b56-240">Ha megnézzük Views\Shared\_LoginPartial.cshtml, találja, hogy használja `User.Identity.Name` toodisplay hello felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="28b56-240">If you look at Views\Shared\_LoginPartial.cshtml, you find that it uses `User.Identity.Name` toodisplay hello user name.</span></span> <span data-ttu-id="28b56-241">Ahogy korábban említettük, ha a felhasználó SAML-jogkivonat hello érhető el a hitelesített hello hello neve jogcím ASP.NET hydrates Ez a tulajdonság azt.</span><span class="sxs-lookup"><span data-stu-id="28b56-241">As mentioned previously, if hello name claim of hello authenticated user is available in hello SAML token, ASP.NET hydrates this property with it.</span></span> <span data-ttu-id="28b56-242">a jogcímek összes hello toosee indexhez tartozó műveleti módszer küldött Controllers\HomeController.cs, a hello állíthasson be az AD FS által.</span><span class="sxs-lookup"><span data-stu-id="28b56-242">toosee all hello claims that are sent by AD FS, put a breakpoint in Controllers\HomeController.cs, in hello Index action method.</span></span> <span data-ttu-id="28b56-243">Hello felhasználó hitelesítése után vizsgálja meg a hello `System.Security.Claims.Current.Claims` gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="28b56-243">After hello user is authenticated, inspect hello `System.Security.Claims.Current.Claims` collection.</span></span>

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>

## <a name="authorize-users-for-specific-controllers-or-actions"></a><span data-ttu-id="28b56-244">Adott vezérlőket vagy a műveletek a felhasználók hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="28b56-244">Authorize users for specific controllers or actions</span></span>
<span data-ttu-id="28b56-245">Csoporttagságok szerepkör jogcímekként bevont a függő Entitás szintű bizalmi kapcsolatainak konfigurációja, mivel most már használhatja azokat közvetlenül a hello `[Authorize(Roles="...")]` decoration tartományvezérlőket és a műveletek.</span><span class="sxs-lookup"><span data-stu-id="28b56-245">Since you have included group memberships as role claims in your RP trust configuration, you can now use them directly in hello `[Authorize(Roles="...")]` decoration for controllers and actions.</span></span> <span data-ttu-id="28b56-246">Hello létrehozás-olvasás-Módosítás-Törlés (CRUD) mintával üzleti alkalmazásokban engedélyezheti egyes szerepkörök tooaccess egyes műveletek során.</span><span class="sxs-lookup"><span data-stu-id="28b56-246">In a line-of-business application with hello Create-Read-Update-Delete (CRUD) pattern, you can authorize specific roles tooaccess each action.</span></span> <span data-ttu-id="28b56-247">Most meg fog csak próbálja ki ezt a funkciót hello meglévő otthoni vezérlő.</span><span class="sxs-lookup"><span data-stu-id="28b56-247">For now, you will just try out this feature on hello existing Home controller.</span></span>

1. <span data-ttu-id="28b56-248">Nyissa meg a Controllers\HomeController.cs.</span><span class="sxs-lookup"><span data-stu-id="28b56-248">Open Controllers\HomeController.cs.</span></span>
2. <span data-ttu-id="28b56-249">Adja a hello `About` és `Contact` művelet módszerek hasonló toohello a következő kódot, biztonsági csoporttagság, amely a hitelesített felhasználó rendelkezik-e használatával.</span><span class="sxs-lookup"><span data-stu-id="28b56-249">Decorate hello `About` and `Contact` action methods similar toohello following code, using security group memberships that your authenticated user has.</span></span>  
   
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
   
    <span data-ttu-id="28b56-250">Mivel a hozzáadott **tesztfelhasználó** túl**Tesztcsoport** a az AD FS laborkörnyezetben, a teszt csoportot tootest engedélyezési fogok használni `About`.</span><span class="sxs-lookup"><span data-stu-id="28b56-250">Since I added **Test User** too**Test Group** in my AD FS lab environment, I'll use Test Group tootest authorization on `About`.</span></span> <span data-ttu-id="28b56-251">A `Contact`, hello negatív gyorsítás esetében is tesztelni fogja **Tartománygazdák**, toowhich **tesztelése felhasználói** nem tartozik.</span><span class="sxs-lookup"><span data-stu-id="28b56-251">For `Contact`, I'll test hello negative case of **Domain Admins**, toowhich **Test User** doesn't belong.</span></span>
3. <span data-ttu-id="28b56-252">Indítsa el a hello hibakereső beírásával `F5` jelentkezzen be, majd kattintson a **kapcsolatos**.</span><span class="sxs-lookup"><span data-stu-id="28b56-252">Start hello debugger by typing `F5` and sign in, then click **About**.</span></span> <span data-ttu-id="28b56-253">Meg kell most tekintjük hello `~/About/Index` lapon sikeres legyen, ha a hitelesített felhasználó jogosult-e az adott művelethez.</span><span class="sxs-lookup"><span data-stu-id="28b56-253">You should now be viewing hello `~/About/Index` page successfully, if your authenticated user is authorized for that action.</span></span>
4. <span data-ttu-id="28b56-254">Most kattintson **forduljon**, ami abban az esetben, ha nem engedélyezhetik **tesztfelhasználó** hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="28b56-254">Now click **Contact**, which in my case should not authorize **Test User** for hello action.</span></span> <span data-ttu-id="28b56-255">Hello böngésző azonban átirányított tooAD FS, ami végül jeleníti meg ezt az üzenetet:</span><span class="sxs-lookup"><span data-stu-id="28b56-255">However, hello browser is redirected tooAD FS, which eventually shows this message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)
   
    <span data-ttu-id="28b56-256">Ez a hiba az eseménynaplóban az AD FS-kiszolgáló hello vizsgálja meg, a következő kivétel üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="28b56-256">If you investigate this error in Event Viewer on hello AD FS server, you see this exception message:</span></span>  
   
    <pre class="prettyprint">
    Microsoft.IdentityServer.Web.InvalidRequestException: MSIS7042: <mark>hello same client browser session has made '6' requests in hello last '11' seconds.</mark> Contact your administrator for details.
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>
   
    <span data-ttu-id="28b56-257">hello Ez a hiba oka, hogy alapértelmezés szerint MVC esetén ad vissza a 401-es nem engedélyezett a felhasználói szerepkörök nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="28b56-257">hello reason for this error is that by default, MVC returns a 401 Unauthorized when a user's roles are not authorized.</span></span> <span data-ttu-id="28b56-258">Ezzel elindítja a újrahitelesítés kérelem tooyour identitásszolgáltató (AD FS).</span><span class="sxs-lookup"><span data-stu-id="28b56-258">This triggers a reauthentication request tooyour identity provider (AD FS).</span></span> <span data-ttu-id="28b56-259">Hello felhasználó már hitelesítve van, mivel az AD FS adja vissza toohello ugyanazon az oldalon, amely majd kiadja a másik 401, egy átirányítási jöjjön létre.</span><span class="sxs-lookup"><span data-stu-id="28b56-259">Since hello user is already authenticated, AD FS returns toohello same page, which then issues another 401, creating a redirect loop.</span></span> <span data-ttu-id="28b56-260">AuthorizeAttribute meg fog felülbírálása `HandleUnauthorizedRequest` egyszerű logikai tooshow metódust használ, amelyet érdemes folyamatos hello átirányítási hurok helyett.</span><span class="sxs-lookup"><span data-stu-id="28b56-260">You will override AuthorizeAttribute's `HandleUnauthorizedRequest` method with simple logic tooshow something that makes sense instead of continuing hello redirect loop.</span></span>
5. <span data-ttu-id="28b56-261">Hozzon létre egy fájlt AuthorizeAttribute.cs, és a Beillesztés hello kód bele a következő nevű hello projektben.</span><span class="sxs-lookup"><span data-stu-id="28b56-261">Create a file in hello project called AuthorizeAttribute.cs, and paste hello following code into it.</span></span>
   
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
   
    <span data-ttu-id="28b56-262">hello bírálja felül a kód elküldi egy HTTP 403 (tiltott) helyett HTTP 401-es (nem engedélyezett) azokban az esetekben, hitelesített, de nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="28b56-262">hello override code sends an HTTP 403 (Forbidden) instead of HTTP 401 (Unauthorized) in  authenticated but unauthorized cases.</span></span>
6. <span data-ttu-id="28b56-263">Futtassa újra hello hibakereső `F5`.</span><span class="sxs-lookup"><span data-stu-id="28b56-263">Run hello debugger again with `F5`.</span></span> <span data-ttu-id="28b56-264">Kattintson a **forduljon** most egy még informatívabbá (ámbár unattractive) hibaüzenetet jelenít meg:</span><span class="sxs-lookup"><span data-stu-id="28b56-264">Clicking **Contact** now shows a more informative (albeit unattractive) error message:</span></span>
   
    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)
7. <span data-ttu-id="28b56-265">Tegye közzé újra hello alkalmazás tooAzure App Service Web Apps, és tesztelje a hello élő alkalmazás hello jellemzőit.</span><span class="sxs-lookup"><span data-stu-id="28b56-265">Publish hello application tooAzure App Service Web Apps again, and test hello behavior of hello live application.</span></span>

<a name="bkmk_data"></a>

## <a name="connect-tooon-premises-data"></a><span data-ttu-id="28b56-266">Csatlakozás tooon helyszíni adatokhoz</span><span class="sxs-lookup"><span data-stu-id="28b56-266">Connect tooon-premises data</span></span>
<span data-ttu-id="28b56-267">Miért érdemes tooimplement az üzleti alkalmazás az Azure Active Directory helyett AD FS fogja szervezeti adatok helyi tartásával megfelelőségi problémák.</span><span class="sxs-lookup"><span data-stu-id="28b56-267">A reason that you would want tooimplement your line-of-business application with AD FS instead of Azure Active Directory is compliance issues with keeping organization data off-premise.</span></span> <span data-ttu-id="28b56-268">Ez azt is jelenti, hogy a webalkalmazás az Azure-ban helyszíni adatbázisokat kell hozzáférni, mert nem szerepelhet toouse [SQL-adatbázis](/services/sql-database/) , hello adatok rétegként a webes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="28b56-268">This may also mean that your web app in Azure must access on-premises databases, since you are not allowed toouse [SQL Database](/services/sql-database/) as hello data tier for your web apps.</span></span>

<span data-ttu-id="28b56-269">Az Azure App Service Web Apps férnek hozzá a helyszíni adatbázisok esetén két módszer támogatja: [hibrid kapcsolatok](../biztalk-services/integration-hybrid-connection-overview.md) és [virtuális hálózatok](web-sites-integrate-with-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="28b56-269">Azure App Service Web Apps supports accessing on-premises databases with two approaches: [Hybrid Connections](../biztalk-services/integration-hybrid-connection-overview.md) and [Virtual Networks](web-sites-integrate-with-vnet.md).</span></span> <span data-ttu-id="28b56-270">További információkért lásd: [használó hálózatok integráció és az Azure App Service Web Apps hibrid kapcsolatok](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span><span class="sxs-lookup"><span data-stu-id="28b56-270">For more information, see [Using VNET integration and Hybrid connections with Azure App Service Web Apps](https://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/).</span></span>

<a name="bkmk_resources"></a>

## <a name="further-resources"></a><span data-ttu-id="28b56-271">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="28b56-271">Further resources</span></span>
* [<span data-ttu-id="28b56-272">A helyszíni Active Directoryval az Azure alkalmazás hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="28b56-272">Authenticate with on-premises Active Directory in your Azure app</span></span>](web-sites-authentication-authorization.md)
* [<span data-ttu-id="28b56-273">Üzleti Azure-alkalmazás létrehozása az Azure Active Directory-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="28b56-273">Create a line-of-business Azure app with Azure Active Directory authentication</span></span>](web-sites-dotnet-lob-application-azure-ad.md)
* [<span data-ttu-id="28b56-274">A helyszíni szervezeti hitelesítés lehetőséget (AD FS) az ASP.NET hello használja a Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="28b56-274">Use hello On-Premises Organizational Authentication Option (ADFS) With ASP.NET in Visual Studio 2013</span></span>](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
* [<span data-ttu-id="28b56-275">A webes projektet a WIF VS2013 tooKatana áttelepítése</span><span class="sxs-lookup"><span data-stu-id="28b56-275">Migrate a VS2013 Web Project From WIF tooKatana</span></span>](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
* [<span data-ttu-id="28b56-276">Az Active Directory összevonási szolgáltatások áttekintése</span><span class="sxs-lookup"><span data-stu-id="28b56-276">Active Directory Federation Services Overview</span></span>](http://technet.microsoft.com/library/hh831502.aspx)
* [<span data-ttu-id="28b56-277">WS-Federation 1.1 szabvány</span><span class="sxs-lookup"><span data-stu-id="28b56-277">WS-Federation 1.1 specification</span></span>](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)


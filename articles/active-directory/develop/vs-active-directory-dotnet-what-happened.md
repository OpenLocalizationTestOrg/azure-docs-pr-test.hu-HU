---
title: "végrehajtott aaaChanges tooa MVC projekt tooAzure AD csatlakoztatásakor |} Microsoft Docs"
description: "Ismerteti, mi történik tooyour MVC projekt tooAzure AD Visual Studio kapcsolódó szolgáltatások"
services: active-directory
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 8b24adde-547e-4ffe-824a-2029ba210216
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 5e6d4ce5331eacca5fc83429017ae454fadcc8e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-mvc-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="8a0e8-103">Milyen történt toomy MVC projekt (a Visual Studio Azure Active Directory szolgáltatás csatlakozik)?</span><span class="sxs-lookup"><span data-stu-id="8a0e8-103">What happened toomy MVC project (Visual Studio Azure Active Directory connected service)?</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="8a0e8-104">Első lépések</span><span class="sxs-lookup"><span data-stu-id="8a0e8-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="8a0e8-105">mi történt</span><span class="sxs-lookup"><span data-stu-id="8a0e8-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="8a0e8-106">Hivatkozások vannak adva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="8a0e8-107">NuGet-csomag hivatkozások</span><span class="sxs-lookup"><span data-stu-id="8a0e8-107">NuGet package references</span></span>
* <span data-ttu-id="8a0e8-108">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-108">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="8a0e8-109">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-109">**Microsoft.Owin**</span></span>
* <span data-ttu-id="8a0e8-110">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-110">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="8a0e8-111">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-111">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="8a0e8-112">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-112">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="8a0e8-113">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-113">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="8a0e8-114">**Owin**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-114">**Owin**</span></span>
* <span data-ttu-id="8a0e8-115">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-115">**System.IdentityModel.Tokens.Jwt**</span></span>

### <a name="net-references"></a><span data-ttu-id="8a0e8-116">.NET-hivatkozások</span><span class="sxs-lookup"><span data-stu-id="8a0e8-116">.NET references</span></span>
* <span data-ttu-id="8a0e8-117">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-117">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="8a0e8-118">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-118">**Microsoft.Owin**</span></span>
* <span data-ttu-id="8a0e8-119">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-119">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="8a0e8-120">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-120">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="8a0e8-121">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-121">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="8a0e8-122">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-122">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="8a0e8-123">**Owin**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-123">**Owin**</span></span>
* <span data-ttu-id="8a0e8-124">**System.IdentityModel**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-124">**System.IdentityModel**</span></span>
* <span data-ttu-id="8a0e8-125">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-125">**System.IdentityModel.Tokens.Jwt**</span></span>
* <span data-ttu-id="8a0e8-126">**System.Runtime.Serialization**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-126">**System.Runtime.Serialization**</span></span>

## <a name="code-has-been-added"></a><span data-ttu-id="8a0e8-127">Kód hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-127">Code has been added</span></span>
### <a name="code-files-were-added-tooyour-project"></a><span data-ttu-id="8a0e8-128">Kód hozzáadott tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="8a0e8-128">Code files were added tooyour project</span></span>
<span data-ttu-id="8a0e8-129">Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** tooyour projektet, amely tartalmazza az Azure AD-alapú hitelesítés ügyfélindítási logikája lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-129">An authentication startup class, **App_Start/Startup.Auth.cs** was added tooyour project containing startup logic for Azure AD authentication.</span></span> <span data-ttu-id="8a0e8-130">Emellett vezérlőosztály, Controllers/AccountController.cs hozzá lett adva tartalmazó **SignIn()** és **SignOut()** módszerek.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-130">Also, a controller class, Controllers/AccountController.cs was added which contains **SignIn()** and **SignOut()** methods.</span></span> <span data-ttu-id="8a0e8-131">Végezetül részleges nézet **Views/Shared/_LoginPartial.cshtml** hozzá lett adva egy művelet hivatkozására tartalmazó SignIn/SignOut.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-131">Finally, a partial view, **Views/Shared/_LoginPartial.cshtml** was added containing an action link for SignIn/SignOut.</span></span>

### <a name="startup-code-was-added-tooyour-project"></a><span data-ttu-id="8a0e8-132">Indítási kóddal tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="8a0e8-132">Startup code was added tooyour project</span></span>
<span data-ttu-id="8a0e8-133">Ha előzőleg már egy indítási osztályt a projekthez, hello **konfigurációs** metódus lett frissített tooinclude hívása túl**ConfigureAuth(app)**.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-133">If you already had a Startup class in your project, hello **Configuration** method was updated tooinclude a call too**ConfigureAuth(app)**.</span></span> <span data-ttu-id="8a0e8-134">Ellenkező esetben indítási osztály tooyour projekt lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-134">Otherwise, a Startup class was added tooyour project.</span></span>

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a><span data-ttu-id="8a0e8-135">Új konfigurációs értéket tartalmaz a az App.config fájlt vagy a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="8a0e8-135">Your app.config or web.config has new configuration values</span></span>
<span data-ttu-id="8a0e8-136">a következő konfigurációs bejegyzéseket hello hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-136">hello following configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="hello selected Azure AD Domain" />
        <add key="ida:TenantId" value="hello Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a><span data-ttu-id="8a0e8-137">Egy Azure Active Directory (AD) alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="8a0e8-137">An Azure Active Directory (AD) App was created</span></span>
<span data-ttu-id="8a0e8-138">Az Azure AD-alkalmazások létrehozásának hello hello varázslóban megadott könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-138">An Azure AD Application was created in hello directory that you selected in hello wizard.</span></span>

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="8a0e8-139">Ha ellenőrizni szeretnék *tiltsa le az egyes felhasználói fiókok hitelesítési*, további változásait toomy projektet?</span><span class="sxs-lookup"><span data-stu-id="8a0e8-139">If I checked *disable Individual User Accounts authentication*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="8a0e8-140">Hivatkozások a NuGet csomag eltávolítása, és fájlok lettek eltávolítva, a biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-140">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="8a0e8-141">Attól függően, hogy a projekt hello állapotát előfordulhat, hogy távolítsa el a további hivatkozások vagy fájlokat, vagy módosítsa a megfelelő kód toomanually.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-141">Depending on hello state of your project, you may have toomanually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="8a0e8-142">NuGet csomag hivatkozást eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="8a0e8-142">NuGet package references removed (for those present)</span></span>
* <span data-ttu-id="8a0e8-143">**Microsoft.AspNet.Identity.Core**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-143">**Microsoft.AspNet.Identity.Core**</span></span>
* <span data-ttu-id="8a0e8-144">**Microsoft.AspNet.Identity.EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-144">**Microsoft.AspNet.Identity.EntityFramework**</span></span>
* <span data-ttu-id="8a0e8-145">**Microsoft.AspNet.Identity.Owin**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-145">**Microsoft.AspNet.Identity.Owin**</span></span>

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="8a0e8-146">Fájlok biztonsági mentése és eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="8a0e8-146">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="8a0e8-147">Alábbi fájlok biztonsági mentése és hello projekt távolítva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-147">Each of following files was backed up and removed from hello project.</span></span> <span data-ttu-id="8a0e8-148">A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-148">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* <span data-ttu-id="8a0e8-149">**App_Start\IdentityConfig.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-149">**App_Start\IdentityConfig.cs**</span></span>
* <span data-ttu-id="8a0e8-150">**Controllers\ManageController.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-150">**Controllers\ManageController.cs**</span></span>
* <span data-ttu-id="8a0e8-151">**Models\IdentityModels.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-151">**Models\IdentityModels.cs**</span></span>
* <span data-ttu-id="8a0e8-152">**Models\ManageViewModels.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-152">**Models\ManageViewModels.cs**</span></span>

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="8a0e8-153">Kód fájlok biztonsági mentése (a jelen)</span><span class="sxs-lookup"><span data-stu-id="8a0e8-153">Code files backed up (for those present)</span></span>
<span data-ttu-id="8a0e8-154">Alábbi fájlok készült biztonsági másolat cseréje előtt.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-154">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="8a0e8-155">A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-155">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* <span data-ttu-id="8a0e8-156">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-156">**Startup.cs**</span></span>
* <span data-ttu-id="8a0e8-157">**App_Start\Startup.auth.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-157">**App_Start\Startup.Auth.cs**</span></span>
* <span data-ttu-id="8a0e8-158">**Controllers\AccountController.cs**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-158">**Controllers\AccountController.cs**</span></span>
* <span data-ttu-id="8a0e8-159">**Views\Shared\_LoginPartial.cshtml**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-159">**Views\Shared\_LoginPartial.cshtml**</span></span>

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="8a0e8-160">Ha ellenőrizni szeretnék *címtáradatok olvasása*, további változásait toomy projektet?</span><span class="sxs-lookup"><span data-stu-id="8a0e8-160">If I checked *Read directory data*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="8a0e8-161">További hivatkozások kerültek.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-161">Additional references have been added.</span></span>

### <a name="additional-nuget-package-references"></a><span data-ttu-id="8a0e8-162">NuGet-csomag további hivatkozások</span><span class="sxs-lookup"><span data-stu-id="8a0e8-162">Additional NuGet package references</span></span>
* <span data-ttu-id="8a0e8-163">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-163">**EntityFramework**</span></span>
* <span data-ttu-id="8a0e8-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="8a0e8-165">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-165">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="8a0e8-166">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-166">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="8a0e8-167">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-167">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="8a0e8-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="8a0e8-169">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-169">**System.Spatial**</span></span>

### <a name="additional-net-references"></a><span data-ttu-id="8a0e8-170">.NET kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="8a0e8-170">Additional .NET references</span></span>
* <span data-ttu-id="8a0e8-171">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-171">**EntityFramework**</span></span>
* <span data-ttu-id="8a0e8-172">**EntityFramework.SqlServer**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-172">**EntityFramework.SqlServer**</span></span>
* <span data-ttu-id="8a0e8-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="8a0e8-174">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-174">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="8a0e8-175">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-175">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="8a0e8-176">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-176">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="8a0e8-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="8a0e8-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span></span>
* <span data-ttu-id="8a0e8-179">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="8a0e8-179">**System.Spatial**</span></span>

### <a name="additional-code-files-were-added-tooyour-project"></a><span data-ttu-id="8a0e8-180">További kód hozzáadott tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="8a0e8-180">Additional Code files were added tooyour project</span></span>
<span data-ttu-id="8a0e8-181">Két hozzáadott toosupport token-gyorsítótárazási: **Models\ADALTokenCache.cs** és **Models\ApplicationDbContext.cs**.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-181">Two files were added toosupport token caching: **Models\ADALTokenCache.cs** and **Models\ApplicationDbContext.cs**.</span></span>  <span data-ttu-id="8a0e8-182">Egy további tartományvezérlő és a nézet hozzáadott tooillustrate fér hozzá a felhasználói profillal kapcsolatos információk az Azure graph API-k használatával.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-182">An additional controller and view were added tooillustrate accessing user profile information using Azure graph APIs.</span></span>  <span data-ttu-id="8a0e8-183">Ezek a fájlok **Controllers\UserProfileController.cs** és **Views\UserProfile\Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-183">These files are **Controllers\UserProfileController.cs** and **Views\UserProfile\Index.cshtml**.</span></span>

### <a name="additional-startup-code-was-added-tooyour-project"></a><span data-ttu-id="8a0e8-184">További indítási kóddal tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="8a0e8-184">Additional Startup code was added tooyour project</span></span>
<span data-ttu-id="8a0e8-185">A hello **startup.auth.cs** fájlba, egy új **OpenIdConnectAuthenticationNotifications** objektum lett hozzáadva toohello **értesítések** hello tagja  **OpenIdConnectAuthenticationOptions**.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-185">In hello **startup.auth.cs** file, a new **OpenIdConnectAuthenticationNotifications** object was added toohello **Notifications** member of hello **OpenIdConnectAuthenticationOptions**.</span></span>  <span data-ttu-id="8a0e8-186">Ez a tooenable hello OAuth kód fogadására, és azt a hozzáférési token cseréjét.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-186">This is tooenable receiving hello OAuth code and exchanging it for an access token.</span></span>

### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a><span data-ttu-id="8a0e8-187">További módosítások történtek, tooyour app.config vagy a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="8a0e8-187">Additional changes were made tooyour app.config or web.config</span></span>
<span data-ttu-id="8a0e8-188">hello következő további konfigurációs bejegyzés hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-188">hello following additional configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

<span data-ttu-id="8a0e8-189">hello következő konfigurációs szakaszokat és a kapcsolati karakterlánc bővült.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-189">hello following configuration sections and connection string have been added.</span></span>

    <configSections>
        <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
        <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
    </configSections>
    <connectionStrings>
        <add name="DefaultConnection" connectionString="Data Source=(localdb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-[AppName + Generated Id].mdf;Initial Catalog=aspnet-[AppName + Generated Id];Integrated Security=True" providerName="System.Data.SqlClient" />
    </connectionStrings>
    <entityFramework>
        <defaultConnectionFactory type="System.Data.Entity.Infrastructure.LocalDbConnectionFactory, EntityFramework">
          <parameters>
            <parameter value="mssqllocaldb" />
          </parameters>
        </defaultConnectionFactory>
        <providers>
          <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
        </providers>
    </entityFramework>


### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="8a0e8-190">Az Azure Active Directory-alkalmazás frissült</span><span class="sxs-lookup"><span data-stu-id="8a0e8-190">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="8a0e8-191">Az Azure Active Directory-alkalmazás lett frissített tooinclude hello *címtáradatok olvasása* engedéllyel és kulcsot hozták létre, amely majd hello volt megadva *ida: ClientSecret* a hello  **Web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="8a0e8-191">Your Azure Active Directory App was updated tooinclude hello *Read directory data* permission and an additional key was created which was then used as hello *ida:ClientSecret* in hello **web.config** file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8a0e8-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8a0e8-192">Next steps</span></span>
- [<span data-ttu-id="8a0e8-193">További tudnivalók az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="8a0e8-193">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)


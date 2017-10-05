---
title: "Változások a MVC projekt csatlakoztatása az Azure AD |} Microsoft Docs"
description: "Ismerteti, mi történik a MVC projekt, amikor csatlakozik a Visual Studio szolgáltatásainak a használatával az Azure AD connect"
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
ms.openlocfilehash: 095411a7fc854f4dce11921adb0f57c5389a8e13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-mvc-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="c9fb7-103">Mi történt a MVC projekt (a Visual Studio Azure Active Directory szolgáltatás csatlakozik)?</span><span class="sxs-lookup"><span data-stu-id="c9fb7-103">What happened to my MVC project (Visual Studio Azure Active Directory connected service)?</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c9fb7-104">Első lépések</span><span class="sxs-lookup"><span data-stu-id="c9fb7-104">Getting Started</span></span>](vs-active-directory-dotnet-getting-started.md)
> * [<span data-ttu-id="c9fb7-105">mi történt</span><span class="sxs-lookup"><span data-stu-id="c9fb7-105">What Happened</span></span>](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="c9fb7-106">Hivatkozások vannak adva.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="c9fb7-107">NuGet-csomag hivatkozások</span><span class="sxs-lookup"><span data-stu-id="c9fb7-107">NuGet package references</span></span>
* <span data-ttu-id="c9fb7-108">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-108">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="c9fb7-109">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-109">**Microsoft.Owin**</span></span>
* <span data-ttu-id="c9fb7-110">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-110">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="c9fb7-111">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-111">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="c9fb7-112">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-112">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="c9fb7-113">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-113">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="c9fb7-114">**Owin**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-114">**Owin**</span></span>
* <span data-ttu-id="c9fb7-115">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-115">**System.IdentityModel.Tokens.Jwt**</span></span>

### <a name="net-references"></a><span data-ttu-id="c9fb7-116">.NET-hivatkozások</span><span class="sxs-lookup"><span data-stu-id="c9fb7-116">.NET references</span></span>
* <span data-ttu-id="c9fb7-117">**Microsoft.IdentityModel.Protocol.Extensions**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-117">**Microsoft.IdentityModel.Protocol.Extensions**</span></span>
* <span data-ttu-id="c9fb7-118">**Microsoft.Owin**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-118">**Microsoft.Owin**</span></span>
* <span data-ttu-id="c9fb7-119">**Microsoft.Owin.Host.SystemWeb**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-119">**Microsoft.Owin.Host.SystemWeb**</span></span>
* <span data-ttu-id="c9fb7-120">**Microsoft.Owin.Security**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-120">**Microsoft.Owin.Security**</span></span>
* <span data-ttu-id="c9fb7-121">**Microsoft.Owin.Security.Cookies**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-121">**Microsoft.Owin.Security.Cookies**</span></span>
* <span data-ttu-id="c9fb7-122">**Microsoft.Owin.Security.OpenIdConnect**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-122">**Microsoft.Owin.Security.OpenIdConnect**</span></span>
* <span data-ttu-id="c9fb7-123">**Owin**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-123">**Owin**</span></span>
* <span data-ttu-id="c9fb7-124">**System.IdentityModel**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-124">**System.IdentityModel**</span></span>
* <span data-ttu-id="c9fb7-125">**System.IdentityModel.Tokens.Jwt**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-125">**System.IdentityModel.Tokens.Jwt**</span></span>
* <span data-ttu-id="c9fb7-126">**System.Runtime.Serialization**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-126">**System.Runtime.Serialization**</span></span>

## <a name="code-has-been-added"></a><span data-ttu-id="c9fb7-127">Kód hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-127">Code has been added</span></span>
### <a name="code-files-were-added-to-your-project"></a><span data-ttu-id="c9fb7-128">Kód hozzáadott a projekthez</span><span class="sxs-lookup"><span data-stu-id="c9fb7-128">Code files were added to your project</span></span>
<span data-ttu-id="c9fb7-129">Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** lett hozzáadva a projekthez az Azure AD-alapú hitelesítés ügyfélindítási logikája tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-129">An authentication startup class, **App_Start/Startup.Auth.cs** was added to your project containing startup logic for Azure AD authentication.</span></span> <span data-ttu-id="c9fb7-130">Emellett vezérlőosztály, Controllers/AccountController.cs hozzá lett adva tartalmazó **SignIn()** és **SignOut()** módszerek.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-130">Also, a controller class, Controllers/AccountController.cs was added which contains **SignIn()** and **SignOut()** methods.</span></span> <span data-ttu-id="c9fb7-131">Végezetül részleges nézet **Views/Shared/_LoginPartial.cshtml** hozzá lett adva egy művelet hivatkozására tartalmazó SignIn/SignOut.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-131">Finally, a partial view, **Views/Shared/_LoginPartial.cshtml** was added containing an action link for SignIn/SignOut.</span></span>

### <a name="startup-code-was-added-to-your-project"></a><span data-ttu-id="c9fb7-132">Indítási kóddal a projekthez</span><span class="sxs-lookup"><span data-stu-id="c9fb7-132">Startup code was added to your project</span></span>
<span data-ttu-id="c9fb7-133">Ha a projektben már volt indítási osztály a **konfigurációs** metódus hívása lett frissítve **ConfigureAuth(app)**.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-133">If you already had a Startup class in your project, the **Configuration** method was updated to include a call to **ConfigureAuth(app)**.</span></span> <span data-ttu-id="c9fb7-134">Ellenkező esetben egy indítási osztályt a projekthez lett adva.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-134">Otherwise, a Startup class was added to your project.</span></span>

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a><span data-ttu-id="c9fb7-135">Új konfigurációs értéket tartalmaz a az App.config fájlt vagy a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="c9fb7-135">Your app.config or web.config has new configuration values</span></span>
<span data-ttu-id="c9fb7-136">A következő konfigurációs bejegyzés hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-136">The following configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="The selected Azure AD Domain" />
        <add key="ida:TenantId" value="The Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a><span data-ttu-id="c9fb7-137">Egy Azure Active Directory (AD) alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="c9fb7-137">An Azure Active Directory (AD) App was created</span></span>
<span data-ttu-id="c9fb7-138">Az Azure AD-alkalmazások létrehozásának, a varázslóban megadott könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-138">An Azure AD Application was created in the directory that you selected in the wizard.</span></span>

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="c9fb7-139">Ha ellenőrizni szeretnék *tiltsa le az egyes felhasználói fiókok hitelesítési*, milyen további módosítások történtek a projektben?</span><span class="sxs-lookup"><span data-stu-id="c9fb7-139">If I checked *disable Individual User Accounts authentication*, what additional changes were made to my project?</span></span>
<span data-ttu-id="c9fb7-140">Hivatkozások a NuGet csomag eltávolítása, és fájlok lettek eltávolítva, a biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-140">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="c9fb7-141">A projekt állapotától függően előfordulhat, hogy manuálisan távolítsa el a további hivatkozások vagy fájlokat, vagy szükség szerint kód módosítása.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-141">Depending on the state of your project, you may have to manually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="c9fb7-142">NuGet csomag hivatkozást eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="c9fb7-142">NuGet package references removed (for those present)</span></span>
* <span data-ttu-id="c9fb7-143">**Microsoft.AspNet.Identity.Core**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-143">**Microsoft.AspNet.Identity.Core**</span></span>
* <span data-ttu-id="c9fb7-144">**Microsoft.AspNet.Identity.EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-144">**Microsoft.AspNet.Identity.EntityFramework**</span></span>
* <span data-ttu-id="c9fb7-145">**Microsoft.AspNet.Identity.Owin**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-145">**Microsoft.AspNet.Identity.Owin**</span></span>

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="c9fb7-146">Fájlok biztonsági mentése és eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="c9fb7-146">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="c9fb7-147">Alábbi fájlok biztonsági mentése és a projekt távolítva.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-147">Each of following files was backed up and removed from the project.</span></span> <span data-ttu-id="c9fb7-148">A biztonságimásolat-fájlok a "Biztonsági mentés" mappával a gyökérkönyvtárban, a projekt könyvtárában találhatók.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-148">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* <span data-ttu-id="c9fb7-149">**App_Start\IdentityConfig.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-149">**App_Start\IdentityConfig.cs**</span></span>
* <span data-ttu-id="c9fb7-150">**Controllers\ManageController.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-150">**Controllers\ManageController.cs**</span></span>
* <span data-ttu-id="c9fb7-151">**Models\IdentityModels.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-151">**Models\IdentityModels.cs**</span></span>
* <span data-ttu-id="c9fb7-152">**Models\ManageViewModels.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-152">**Models\ManageViewModels.cs**</span></span>

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="c9fb7-153">Kód fájlok biztonsági mentése (a jelen)</span><span class="sxs-lookup"><span data-stu-id="c9fb7-153">Code files backed up (for those present)</span></span>
<span data-ttu-id="c9fb7-154">Alábbi fájlok készült biztonsági másolat cseréje előtt.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-154">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="c9fb7-155">A biztonságimásolat-fájlok a "Biztonsági mentés" mappával a gyökérkönyvtárban, a projekt könyvtárában találhatók.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-155">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* <span data-ttu-id="c9fb7-156">**Startup.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-156">**Startup.cs**</span></span>
* <span data-ttu-id="c9fb7-157">**App_Start\Startup.auth.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-157">**App_Start\Startup.Auth.cs**</span></span>
* <span data-ttu-id="c9fb7-158">**Controllers\AccountController.cs**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-158">**Controllers\AccountController.cs**</span></span>
* <span data-ttu-id="c9fb7-159">**Views\Shared\_LoginPartial.cshtml**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-159">**Views\Shared\_LoginPartial.cshtml**</span></span>

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="c9fb7-160">Ha ellenőrizni szeretnék *címtáradatok olvasása*, milyen további módosítások történtek a projektben?</span><span class="sxs-lookup"><span data-stu-id="c9fb7-160">If I checked *Read directory data*, what additional changes were made to my project?</span></span>
<span data-ttu-id="c9fb7-161">További hivatkozások kerültek.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-161">Additional references have been added.</span></span>

### <a name="additional-nuget-package-references"></a><span data-ttu-id="c9fb7-162">NuGet-csomag további hivatkozások</span><span class="sxs-lookup"><span data-stu-id="c9fb7-162">Additional NuGet package references</span></span>
* <span data-ttu-id="c9fb7-163">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-163">**EntityFramework**</span></span>
* <span data-ttu-id="c9fb7-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-164">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="c9fb7-165">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-165">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="c9fb7-166">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-166">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="c9fb7-167">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-167">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="c9fb7-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-168">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="c9fb7-169">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-169">**System.Spatial**</span></span>

### <a name="additional-net-references"></a><span data-ttu-id="c9fb7-170">.NET kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="c9fb7-170">Additional .NET references</span></span>
* <span data-ttu-id="c9fb7-171">**EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-171">**EntityFramework**</span></span>
* <span data-ttu-id="c9fb7-172">**EntityFramework.SqlServer**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-172">**EntityFramework.SqlServer**</span></span>
* <span data-ttu-id="c9fb7-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-173">**Microsoft.Azure.ActiveDirectory.GraphClient**</span></span>
* <span data-ttu-id="c9fb7-174">**Microsoft.Data.Edm**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-174">**Microsoft.Data.Edm**</span></span>
* <span data-ttu-id="c9fb7-175">**Microsoft.Data.OData**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-175">**Microsoft.Data.OData**</span></span>
* <span data-ttu-id="c9fb7-176">**Microsoft.Data.Services.Client**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-176">**Microsoft.Data.Services.Client**</span></span>
* <span data-ttu-id="c9fb7-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-177">**Microsoft.IdentityModel.Clients.ActiveDirectory**</span></span>
* <span data-ttu-id="c9fb7-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-178">**Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**</span></span>
* <span data-ttu-id="c9fb7-179">**System.Spatial**</span><span class="sxs-lookup"><span data-stu-id="c9fb7-179">**System.Spatial**</span></span>

### <a name="additional-code-files-were-added-to-your-project"></a><span data-ttu-id="c9fb7-180">További kód hozzáadott a projekthez</span><span class="sxs-lookup"><span data-stu-id="c9fb7-180">Additional Code files were added to your project</span></span>
<span data-ttu-id="c9fb7-181">Két hozzáadott támogatja a token-gyorsítótárazási: **Models\ADALTokenCache.cs** és **Models\ApplicationDbContext.cs**.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-181">Two files were added to support token caching: **Models\ADALTokenCache.cs** and **Models\ApplicationDbContext.cs**.</span></span>  <span data-ttu-id="c9fb7-182">Egy további tartományvezérlő, és tekintse meg a rendszer adott elérése során felhasználói profillal kapcsolatos információk az Azure graph API-k segítségével mutatja be.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-182">An additional controller and view were added to illustrate accessing user profile information using Azure graph APIs.</span></span>  <span data-ttu-id="c9fb7-183">Ezek a fájlok **Controllers\UserProfileController.cs** és **Views\UserProfile\Index.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-183">These files are **Controllers\UserProfileController.cs** and **Views\UserProfile\Index.cshtml**.</span></span>

### <a name="additional-startup-code-was-added-to-your-project"></a><span data-ttu-id="c9fb7-184">További indítási kóddal a projekthez</span><span class="sxs-lookup"><span data-stu-id="c9fb7-184">Additional Startup code was added to your project</span></span>
<span data-ttu-id="c9fb7-185">Az a **startup.auth.cs** fájlba, egy új **OpenIdConnectAuthenticationNotifications** objektumot hozzáadni az **értesítések** tagja a  **OpenIdConnectAuthenticationOptions**.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-185">In the **startup.auth.cs** file, a new **OpenIdConnectAuthenticationNotifications** object was added to the **Notifications** member of the **OpenIdConnectAuthenticationOptions**.</span></span>  <span data-ttu-id="c9fb7-186">Ez egy, az OAuth-kód fogadása és cseréjét azt a hozzáférési token engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-186">This is to enable receiving the OAuth code and exchanging it for an access token.</span></span>

### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a><span data-ttu-id="c9fb7-187">További módosítások történtek az App.config fájlt vagy a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="c9fb7-187">Additional changes were made to your app.config or web.config</span></span>
<span data-ttu-id="c9fb7-188">A következő további konfigurációs bejegyzés hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-188">The following additional configuration entries have been added.</span></span>

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

<span data-ttu-id="c9fb7-189">A következő konfigurációs szakaszokat és a kapcsolati karakterlánc bővült.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-189">The following configuration sections and connection string have been added.</span></span>

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


### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="c9fb7-190">Az Azure Active Directory-alkalmazás frissült</span><span class="sxs-lookup"><span data-stu-id="c9fb7-190">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="c9fb7-191">Az Azure Active Directory-alkalmazás lett frissítve a *címtáradatok olvasása* engedéllyel és kulcsot hozták létre, amely majd használata a *ida: ClientSecret* a a  **Web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9fb7-191">Your Azure Active Directory App was updated to include the *Read directory data* permission and an additional key was created which was then used as the *ida:ClientSecret* in the **web.config** file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c9fb7-192">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9fb7-192">Next steps</span></span>
- [<span data-ttu-id="c9fb7-193">További tudnivalók az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="c9fb7-193">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)


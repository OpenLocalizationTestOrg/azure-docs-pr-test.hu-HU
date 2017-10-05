---
title: "Változások a WebApi projekt csatlakoztatása az Azure AD |} Microsoft Docs"
description: "Ismerteti, mi történik a WebApi-projektet a Visual Studio segítségével az Azure AD connect"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 57630aee-26a2-4326-9dbb-ea2a66daa8b0
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: 086e5a9622cad681cd282345d97e0c28ee7de2fa
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="what-happened-to-my-webapi-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="f767d-103">Mi történt a WebApi project (a Visual Studio Azure Active Directory szolgáltatás csatlakozik)</span><span class="sxs-lookup"><span data-stu-id="f767d-103">What happened to my WebApi project (Visual Studio Azure Active Directory connected service)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f767d-104">Első lépések</span><span class="sxs-lookup"><span data-stu-id="f767d-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="f767d-105">mi történt</span><span class="sxs-lookup"><span data-stu-id="f767d-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="f767d-106">Hivatkozások vannak adva.</span><span class="sxs-lookup"><span data-stu-id="f767d-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="f767d-107">NuGet-csomag hivatkozások</span><span class="sxs-lookup"><span data-stu-id="f767d-107">NuGet package references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a><span data-ttu-id="f767d-108">.NET-hivatkozások</span><span class="sxs-lookup"><span data-stu-id="f767d-108">.NET references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a><span data-ttu-id="f767d-109">Kódmódosítások</span><span class="sxs-lookup"><span data-stu-id="f767d-109">Code changes</span></span>
### <a name="code-files-were-added-to-your-project"></a><span data-ttu-id="f767d-110">Kód hozzáadott a projekthez</span><span class="sxs-lookup"><span data-stu-id="f767d-110">Code files were added to your project</span></span>
<span data-ttu-id="f767d-111">Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** lett hozzáadva a projekthez az Azure AD-alapú hitelesítés ügyfélindítási logikája tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="f767d-111">An authentication startup class, **App_Start/Startup.Auth.cs** was added to your project containing startup logic for Azure AD authentication.</span></span>

### <a name="startup-code-was-added-to-your-project"></a><span data-ttu-id="f767d-112">Indítási kóddal a projekthez</span><span class="sxs-lookup"><span data-stu-id="f767d-112">Startup code was added to your project</span></span>
<span data-ttu-id="f767d-113">Ha a projektben már volt indítási osztály a **konfigurációs** metódus hívása lett frissítve `ConfigureAuth(app)`.</span><span class="sxs-lookup"><span data-stu-id="f767d-113">If you already had a Startup class in your project, the **Configuration** method was updated to include a call to `ConfigureAuth(app)`.</span></span> <span data-ttu-id="f767d-114">Ellenkező esetben egy indítási osztályt a projekthez lett adva.</span><span class="sxs-lookup"><span data-stu-id="f767d-114">Otherwise, a Startup class was added to your project.</span></span>

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a><span data-ttu-id="f767d-115">Az App.config fájlt vagy a web.config fájl új konfigurációs értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f767d-115">Your app.config or web.config file has new configuration values.</span></span>
<span data-ttu-id="f767d-116">A következő konfigurációs bejegyzés hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="f767d-116">The following configuration entries have been added.</span></span>

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from the new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="The App ID Uri from the wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a><span data-ttu-id="f767d-117">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f767d-117">An Azure AD App was created</span></span>
<span data-ttu-id="f767d-118">Az Azure AD-alkalmazások létrehozásának, a varázslóban megadott könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="f767d-118">An Azure AD Application was created in the directory that you selected in the wizard.</span></span>

[<span data-ttu-id="f767d-119">További tudnivalók az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="f767d-119">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="f767d-120">Ha ellenőrizni szeretnék *tiltsa le az egyes felhasználói fiókok hitelesítési*, milyen további módosítások történtek a projektben?</span><span class="sxs-lookup"><span data-stu-id="f767d-120">If I checked *disable Individual User Accounts authentication*, what additional changes were made to my project?</span></span>
<span data-ttu-id="f767d-121">Hivatkozások a NuGet csomag eltávolítása, és fájlok lettek eltávolítva, a biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="f767d-121">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="f767d-122">A projekt állapotától függően előfordulhat, hogy manuálisan távolítsa el a további hivatkozások vagy fájlokat, vagy szükség szerint kód módosítása.</span><span class="sxs-lookup"><span data-stu-id="f767d-122">Depending on the state of your project, you may have to manually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="f767d-123">NuGet csomag hivatkozást eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="f767d-123">NuGet package references removed (for those present)</span></span>
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="f767d-124">Fájlok biztonsági mentése és eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="f767d-124">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="f767d-125">Alábbi fájlok biztonsági mentése és a projekt távolítva.</span><span class="sxs-lookup"><span data-stu-id="f767d-125">Each of following files was backed up and removed from the project.</span></span> <span data-ttu-id="f767d-126">A biztonságimásolat-fájlok a "Biztonsági mentés" mappával a gyökérkönyvtárban, a projekt könyvtárában találhatók.</span><span class="sxs-lookup"><span data-stu-id="f767d-126">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="f767d-127">Kód fájlok biztonsági mentése (a jelen)</span><span class="sxs-lookup"><span data-stu-id="f767d-127">Code files backed up (for those present)</span></span>
<span data-ttu-id="f767d-128">Alábbi fájlok készült biztonsági másolat cseréje előtt.</span><span class="sxs-lookup"><span data-stu-id="f767d-128">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="f767d-129">A biztonságimásolat-fájlok a "Biztonsági mentés" mappával a gyökérkönyvtárban, a projekt könyvtárában találhatók.</span><span class="sxs-lookup"><span data-stu-id="f767d-129">Backup files are located in a 'Backup' folder at the root of the project's directory.</span></span>

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-to-my-project"></a><span data-ttu-id="f767d-130">Ha ellenőrizni szeretnék *címtáradatok olvasása*, milyen további módosítások történtek a projektben?</span><span class="sxs-lookup"><span data-stu-id="f767d-130">If I checked *Read directory data*, what additional changes were made to my project?</span></span>
### <a name="additional-changes-were-made-to-your-appconfig-or-webconfig"></a><span data-ttu-id="f767d-131">További módosítások történtek az App.config fájlt vagy a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="f767d-131">Additional changes were made to your app.config or web.config</span></span>
<span data-ttu-id="f767d-132">A következő további konfigurációs bejegyzés hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="f767d-132">The following additional configuration entries have been added.</span></span>

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="f767d-133">Az Azure Active Directory-alkalmazás frissült</span><span class="sxs-lookup"><span data-stu-id="f767d-133">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="f767d-134">Az Azure Active Directory-alkalmazás lett frissítve a *címtáradatok olvasása* engedéllyel és kulcsot hozták létre, amely majd használata a *ida: jelszó* a a `web.config` fájlt.</span><span class="sxs-lookup"><span data-stu-id="f767d-134">Your Azure Active Directory App was updated to include the *Read directory data* permission and an additional key was created which was then used as the *ida:Password* in the `web.config` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f767d-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f767d-135">Next steps</span></span>
- [<span data-ttu-id="f767d-136">További tudnivalók az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="f767d-136">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)


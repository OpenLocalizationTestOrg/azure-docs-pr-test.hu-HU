---
title: "végrehajtott aaaChanges tooa WebApi projekt tooAzure AD csatlakoztatásakor |} Microsoft Docs"
description: "Ismerteti, mi történik tooyour WebApi projekt tooAzure AD Visual Studio használatával"
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
ms.openlocfilehash: 1ea77b6c75b2dc273219fa6c43f02c7a7c5312ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-webapi-project-visual-studio-azure-active-directory-connected-service"></a><span data-ttu-id="6a6be-103">Milyen történt toomy WebApi project (a Visual Studio Azure Active Directory szolgáltatás csatlakozik)</span><span class="sxs-lookup"><span data-stu-id="6a6be-103">What happened toomy WebApi project (Visual Studio Azure Active Directory connected service)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6a6be-104">Első lépések</span><span class="sxs-lookup"><span data-stu-id="6a6be-104">Getting Started</span></span>](vs-active-directory-webapi-getting-started.md)
> * [<span data-ttu-id="6a6be-105">mi történt</span><span class="sxs-lookup"><span data-stu-id="6a6be-105">What Happened</span></span>](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a><span data-ttu-id="6a6be-106">Hivatkozások vannak adva.</span><span class="sxs-lookup"><span data-stu-id="6a6be-106">References have been added</span></span>
### <a name="nuget-package-references"></a><span data-ttu-id="6a6be-107">NuGet-csomag hivatkozások</span><span class="sxs-lookup"><span data-stu-id="6a6be-107">NuGet package references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a><span data-ttu-id="6a6be-108">.NET-hivatkozások</span><span class="sxs-lookup"><span data-stu-id="6a6be-108">.NET references</span></span>
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a><span data-ttu-id="6a6be-109">Kódmódosítások</span><span class="sxs-lookup"><span data-stu-id="6a6be-109">Code changes</span></span>
### <a name="code-files-were-added-tooyour-project"></a><span data-ttu-id="6a6be-110">Kód hozzáadott tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="6a6be-110">Code files were added tooyour project</span></span>
<span data-ttu-id="6a6be-111">Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** tooyour projektet, amely tartalmazza az Azure AD-alapú hitelesítés ügyfélindítási logikája lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="6a6be-111">An authentication startup class, **App_Start/Startup.Auth.cs** was added tooyour project containing startup logic for Azure AD authentication.</span></span>

### <a name="startup-code-was-added-tooyour-project"></a><span data-ttu-id="6a6be-112">Indítási kóddal tooyour projekt</span><span class="sxs-lookup"><span data-stu-id="6a6be-112">Startup code was added tooyour project</span></span>
<span data-ttu-id="6a6be-113">Ha előzőleg már egy indítási osztályt a projekthez, hello **konfigurációs** metódus lett frissített tooinclude hívása túl`ConfigureAuth(app)`.</span><span class="sxs-lookup"><span data-stu-id="6a6be-113">If you already had a Startup class in your project, hello **Configuration** method was updated tooinclude a call too`ConfigureAuth(app)`.</span></span> <span data-ttu-id="6a6be-114">Ellenkező esetben indítási osztály tooyour projekt lett hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="6a6be-114">Otherwise, a Startup class was added tooyour project.</span></span>

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a><span data-ttu-id="6a6be-115">Az App.config fájlt vagy a web.config fájl új konfigurációs értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6a6be-115">Your app.config or web.config file has new configuration values.</span></span>
<span data-ttu-id="6a6be-116">a következő konfigurációs bejegyzéseket hello hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="6a6be-116">hello following configuration entries have been added.</span></span>

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="hello App ID Uri from hello wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a><span data-ttu-id="6a6be-117">Az Azure AD-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6a6be-117">An Azure AD App was created</span></span>
<span data-ttu-id="6a6be-118">Az Azure AD-alkalmazások létrehozásának hello hello varázslóban megadott könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="6a6be-118">An Azure AD Application was created in hello directory that you selected in hello wizard.</span></span>

[<span data-ttu-id="6a6be-119">További tudnivalók az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="6a6be-119">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="6a6be-120">Ha ellenőrizni szeretnék *tiltsa le az egyes felhasználói fiókok hitelesítési*, további változásait toomy projektet?</span><span class="sxs-lookup"><span data-stu-id="6a6be-120">If I checked *disable Individual User Accounts authentication*, what additional changes were made toomy project?</span></span>
<span data-ttu-id="6a6be-121">Hivatkozások a NuGet csomag eltávolítása, és fájlok lettek eltávolítva, a biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="6a6be-121">NuGet package references were removed, and files were removed and backed up.</span></span> <span data-ttu-id="6a6be-122">Attól függően, hogy a projekt hello állapotát előfordulhat, hogy távolítsa el a további hivatkozások vagy fájlokat, vagy módosítsa a megfelelő kód toomanually.</span><span class="sxs-lookup"><span data-stu-id="6a6be-122">Depending on hello state of your project, you may have toomanually remove additional references or files, or modify code as appropriate.</span></span>

### <a name="nuget-package-references-removed-for-those-present"></a><span data-ttu-id="6a6be-123">NuGet csomag hivatkozást eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="6a6be-123">NuGet package references removed (for those present)</span></span>
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a><span data-ttu-id="6a6be-124">Fájlok biztonsági mentése és eltávolítani (a jelen)</span><span class="sxs-lookup"><span data-stu-id="6a6be-124">Code files backed up and removed (for those present)</span></span>
<span data-ttu-id="6a6be-125">Alábbi fájlok biztonsági mentése és hello projekt távolítva.</span><span class="sxs-lookup"><span data-stu-id="6a6be-125">Each of following files was backed up and removed from hello project.</span></span> <span data-ttu-id="6a6be-126">A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.</span><span class="sxs-lookup"><span data-stu-id="6a6be-126">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a><span data-ttu-id="6a6be-127">Kód fájlok biztonsági mentése (a jelen)</span><span class="sxs-lookup"><span data-stu-id="6a6be-127">Code files backed up (for those present)</span></span>
<span data-ttu-id="6a6be-128">Alábbi fájlok készült biztonsági másolat cseréje előtt.</span><span class="sxs-lookup"><span data-stu-id="6a6be-128">Each of following files was backed up before being replaced.</span></span> <span data-ttu-id="6a6be-129">A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.</span><span class="sxs-lookup"><span data-stu-id="6a6be-129">Backup files are located in a 'Backup' folder at hello root of hello project's directory.</span></span>

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a><span data-ttu-id="6a6be-130">Ha ellenőrizni szeretnék *címtáradatok olvasása*, további változásait toomy projektet?</span><span class="sxs-lookup"><span data-stu-id="6a6be-130">If I checked *Read directory data*, what additional changes were made toomy project?</span></span>
### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a><span data-ttu-id="6a6be-131">További módosítások történtek, tooyour app.config vagy a Web.config fájlban</span><span class="sxs-lookup"><span data-stu-id="6a6be-131">Additional changes were made tooyour app.config or web.config</span></span>
<span data-ttu-id="6a6be-132">hello következő további konfigurációs bejegyzés hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="6a6be-132">hello following additional configuration entries have been added.</span></span>

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a><span data-ttu-id="6a6be-133">Az Azure Active Directory-alkalmazás frissült</span><span class="sxs-lookup"><span data-stu-id="6a6be-133">Your Azure Active Directory App was updated</span></span>
<span data-ttu-id="6a6be-134">Az Azure Active Directory-alkalmazás lett frissített tooinclude hello *címtáradatok olvasása* engedéllyel és kulcsot hozták létre, amely majd hello volt megadva *ida: jelszó* a hello `web.config` fájlt.</span><span class="sxs-lookup"><span data-stu-id="6a6be-134">Your Azure Active Directory App was updated tooinclude hello *Read directory data* permission and an additional key was created which was then used as hello *ida:Password* in hello `web.config` file.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a6be-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6a6be-135">Next steps</span></span>
- [<span data-ttu-id="6a6be-136">További tudnivalók az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="6a6be-136">Learn more about Azure Active Directory</span></span>](https://azure.microsoft.com/services/active-directory/)


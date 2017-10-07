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
# <a name="what-happened-toomy-webapi-project-visual-studio-azure-active-directory-connected-service"></a>Milyen történt toomy WebApi project (a Visual Studio Azure Active Directory szolgáltatás csatlakozik)
> [!div class="op_single_selector"]
> * [Első lépések](vs-active-directory-webapi-getting-started.md)
> * [mi történt](vs-active-directory-webapi-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Hivatkozások vannak adva.
### <a name="nuget-package-references"></a>NuGet-csomag hivatkozások
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

### <a name="net-references"></a>.NET-hivatkozások
* `Microsoft.Owin`
* `Microsoft.Owin.Host.SystemWeb`
* `Microsoft.Owin.Security`
* `Microsoft.Owin.Security.ActiveDirectory`
* `Microsoft.Owin.Security.Jwt`
* `Microsoft.Owin.Security.OAuth`
* `Owin`
* `System.IdentityModel.Tokens.Jwt`

## <a name="code-changes"></a>Kódmódosítások
### <a name="code-files-were-added-tooyour-project"></a>Kód hozzáadott tooyour projekt
Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** tooyour projektet, amely tartalmazza az Azure AD-alapú hitelesítés ügyfélindítási logikája lett hozzáadva.

### <a name="startup-code-was-added-tooyour-project"></a>Indítási kóddal tooyour projekt
Ha előzőleg már egy indítási osztályt a projekthez, hello **konfigurációs** metódus lett frissített tooinclude hívása túl`ConfigureAuth(app)`. Ellenkező esetben indítási osztály tooyour projekt lett hozzáadva.

### <a name="your-appconfig-or-webconfig-file-has-new-configuration-values"></a>Az App.config fájlt vagy a web.config fájl új konfigurációs értékeket tartalmaz.
a következő konfigurációs bejegyzéseket hello hozzá lett adva.

```
    <appSettings>
            <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
            <add key="ida:Tenant" value="Your selected Azure AD Tenant" />
            <add key="ida:Audience" value="hello App ID Uri from hello wizard" />
    </appSettings>`
```

### <a name="an-azure-ad-app-was-created"></a>Az Azure AD-alkalmazás létrehozása
Az Azure AD-alkalmazások létrehozásának hello hello varázslóban megadott könyvtárban található.

[További tudnivalók az Azure Active Directoryban](https://azure.microsoft.com/services/active-directory/)

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a>Ha ellenőrizni szeretnék *tiltsa le az egyes felhasználói fiókok hitelesítési*, további változásait toomy projektet?
Hivatkozások a NuGet csomag eltávolítása, és fájlok lettek eltávolítva, a biztonsági mentése. Attól függően, hogy a projekt hello állapotát előfordulhat, hogy távolítsa el a további hivatkozások vagy fájlokat, vagy módosítsa a megfelelő kód toomanually.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet csomag hivatkozást eltávolítani (a jelen)
* `Microsoft.AspNet.Identity.Core`
* `Microsoft.AspNet.Identity.EntityFramework`
* `Microsoft.AspNet.Identity.Owin`

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Fájlok biztonsági mentése és eltávolítani (a jelen)
Alábbi fájlok biztonsági mentése és hello projekt távolítva. A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.

* `App_Start\IdentityConfig.cs`
* `Controllers\AccountController.cs`
* `Controllers\ManageController.cs`
* `Models\IdentityModels.cs`
* `Providers\ApplicationOAuthProvider.cs`

### <a name="code-files-backed-up-for-those-present"></a>Kód fájlok biztonsági mentése (a jelen)
Alábbi fájlok készült biztonsági másolat cseréje előtt. A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.

* `Startup.cs`
* `App_Start\Startup.Auth.cs`

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a>Ha ellenőrizni szeretnék *címtáradatok olvasása*, további változásait toomy projektet?
### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a>További módosítások történtek, tooyour app.config vagy a Web.config fájlban
hello következő további konfigurációs bejegyzés hozzáadva.

```
    <appSettings>
        <add key="ida:Password" value="Your Azure AD App's new password" />
    </appSettings>`
```

### <a name="your-azure-active-directory-app-was-updated"></a>Az Azure Active Directory-alkalmazás frissült
Az Azure Active Directory-alkalmazás lett frissített tooinclude hello *címtáradatok olvasása* engedéllyel és kulcsot hozták létre, amely majd hello volt megadva *ida: jelszó* a hello `web.config` fájlt.

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók az Azure Active Directoryban](https://azure.microsoft.com/services/active-directory/)


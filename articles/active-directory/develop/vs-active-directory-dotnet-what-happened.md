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
# <a name="what-happened-toomy-mvc-project-visual-studio-azure-active-directory-connected-service"></a>Milyen történt toomy MVC projekt (a Visual Studio Azure Active Directory szolgáltatás csatlakozik)?
> [!div class="op_single_selector"]
> * [Első lépések](vs-active-directory-dotnet-getting-started.md)
> * [mi történt](vs-active-directory-dotnet-what-happened.md)
> 
> 

## <a name="references-have-been-added"></a>Hivatkozások vannak adva.
### <a name="nuget-package-references"></a>NuGet-csomag hivatkozások
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel.Tokens.Jwt**

### <a name="net-references"></a>.NET-hivatkozások
* **Microsoft.IdentityModel.Protocol.Extensions**
* **Microsoft.Owin**
* **Microsoft.Owin.Host.SystemWeb**
* **Microsoft.Owin.Security**
* **Microsoft.Owin.Security.Cookies**
* **Microsoft.Owin.Security.OpenIdConnect**
* **Owin**
* **System.IdentityModel**
* **System.IdentityModel.Tokens.Jwt**
* **System.Runtime.Serialization**

## <a name="code-has-been-added"></a>Kód hozzá lett adva.
### <a name="code-files-were-added-tooyour-project"></a>Kód hozzáadott tooyour projekt
Egy hitelesítési indítási osztály **App_Start/Startup.Auth.cs** tooyour projektet, amely tartalmazza az Azure AD-alapú hitelesítés ügyfélindítási logikája lett hozzáadva. Emellett vezérlőosztály, Controllers/AccountController.cs hozzá lett adva tartalmazó **SignIn()** és **SignOut()** módszerek. Végezetül részleges nézet **Views/Shared/_LoginPartial.cshtml** hozzá lett adva egy művelet hivatkozására tartalmazó SignIn/SignOut.

### <a name="startup-code-was-added-tooyour-project"></a>Indítási kóddal tooyour projekt
Ha előzőleg már egy indítási osztályt a projekthez, hello **konfigurációs** metódus lett frissített tooinclude hívása túl**ConfigureAuth(app)**. Ellenkező esetben indítási osztály tooyour projekt lett hozzáadva.

### <a name="your-appconfig-or-webconfig-has-new-configuration-values"></a>Új konfigurációs értéket tartalmaz a az App.config fájlt vagy a Web.config fájlban
a következő konfigurációs bejegyzéseket hello hozzá lett adva.

    <appSettings>
        <add key="ida:ClientId" value="ClientId from hello new Azure AD App" />
        <add key="ida:AADInstance" value="https://login.microsoftonline.com/" />
        <add key="ida:Domain" value="hello selected Azure AD Domain" />
        <add key="ida:TenantId" value="hello Id of your selected Azure AD Tenant" />
        <add key="ida:PostLogoutRedirectUri" value="Your project start page" />
    </appSettings>

### <a name="an-azure-active-directory-ad-app-was-created"></a>Egy Azure Active Directory (AD) alkalmazás létrehozása
Az Azure AD-alkalmazások létrehozásának hello hello varázslóban megadott könyvtárban található.

## <a name="if-i-checked-disable-individual-user-accounts-authentication-what-additional-changes-were-made-toomy-project"></a>Ha ellenőrizni szeretnék *tiltsa le az egyes felhasználói fiókok hitelesítési*, további változásait toomy projektet?
Hivatkozások a NuGet csomag eltávolítása, és fájlok lettek eltávolítva, a biztonsági mentése. Attól függően, hogy a projekt hello állapotát előfordulhat, hogy távolítsa el a további hivatkozások vagy fájlokat, vagy módosítsa a megfelelő kód toomanually.

### <a name="nuget-package-references-removed-for-those-present"></a>NuGet csomag hivatkozást eltávolítani (a jelen)
* **Microsoft.AspNet.Identity.Core**
* **Microsoft.AspNet.Identity.EntityFramework**
* **Microsoft.AspNet.Identity.Owin**

### <a name="code-files-backed-up-and-removed-for-those-present"></a>Fájlok biztonsági mentése és eltávolítani (a jelen)
Alábbi fájlok biztonsági mentése és hello projekt távolítva. A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.

* **App_Start\IdentityConfig.cs**
* **Controllers\ManageController.cs**
* **Models\IdentityModels.cs**
* **Models\ManageViewModels.cs**

### <a name="code-files-backed-up-for-those-present"></a>Kód fájlok biztonsági mentése (a jelen)
Alábbi fájlok készült biztonsági másolat cseréje előtt. A biztonságimásolat-fájlok hello gyökerében hello projekt könyvtár "Biztonsági mentés" mappában található.

* **Startup.cs**
* **App_Start\Startup.auth.cs**
* **Controllers\AccountController.cs**
* **Views\Shared\_LoginPartial.cshtml**

## <a name="if-i-checked-read-directory-data-what-additional-changes-were-made-toomy-project"></a>Ha ellenőrizni szeretnék *címtáradatok olvasása*, további változásait toomy projektet?
További hivatkozások kerültek.

### <a name="additional-nuget-package-references"></a>NuGet-csomag további hivatkozások
* **EntityFramework**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **System.Spatial**

### <a name="additional-net-references"></a>.NET kapcsolódó témakörök
* **EntityFramework**
* **EntityFramework.SqlServer**
* **Microsoft.Azure.ActiveDirectory.GraphClient**
* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.IdentityModel.Clients.ActiveDirectory**
* **Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms**
* **System.Spatial**

### <a name="additional-code-files-were-added-tooyour-project"></a>További kód hozzáadott tooyour projekt
Két hozzáadott toosupport token-gyorsítótárazási: **Models\ADALTokenCache.cs** és **Models\ApplicationDbContext.cs**.  Egy további tartományvezérlő és a nézet hozzáadott tooillustrate fér hozzá a felhasználói profillal kapcsolatos információk az Azure graph API-k használatával.  Ezek a fájlok **Controllers\UserProfileController.cs** és **Views\UserProfile\Index.cshtml**.

### <a name="additional-startup-code-was-added-tooyour-project"></a>További indítási kóddal tooyour projekt
A hello **startup.auth.cs** fájlba, egy új **OpenIdConnectAuthenticationNotifications** objektum lett hozzáadva toohello **értesítések** hello tagja  **OpenIdConnectAuthenticationOptions**.  Ez a tooenable hello OAuth kód fogadására, és azt a hozzáférési token cseréjét.

### <a name="additional-changes-were-made-tooyour-appconfig-or-webconfig"></a>További módosítások történtek, tooyour app.config vagy a Web.config fájlban
hello következő további konfigurációs bejegyzés hozzáadva.

    <appSettings>
        <add key="ida:ClientSecret" value="Your Azure AD App's new client secret" />
    </appSettings>

hello következő konfigurációs szakaszokat és a kapcsolati karakterlánc bővült.

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


### <a name="your-azure-active-directory-app-was-updated"></a>Az Azure Active Directory-alkalmazás frissült
Az Azure Active Directory-alkalmazás lett frissített tooinclude hello *címtáradatok olvasása* engedéllyel és kulcsot hozták létre, amely majd hello volt megadva *ida: ClientSecret* a hello  **Web.config** fájlt.

## <a name="next-steps"></a>Következő lépések
- [További tudnivalók az Azure Active Directoryban](https://azure.microsoft.com/services/active-directory/)


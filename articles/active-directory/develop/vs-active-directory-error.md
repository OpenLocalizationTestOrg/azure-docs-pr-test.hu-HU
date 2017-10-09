---
title: "aaaHow toodiagnose hibák hello Azure Active Directory kapcsolat varázsló"
description: "hello active directory kapcsolat varázsló nem kompatibilis hitelesítési típust észlelt"
services: active-directory
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: dd89ea63-4e45-4da1-9642-645b9309670a
ms.service: active-directory
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 03/05/2017
ms.author: kraigb
ms.custom: aaddev
ms.openlocfilehash: f71c5b41457c0c8db05042e8d5f723e58ad11844
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnosing-errors-with-hello-azure-active-directory-connection-wizard"></a>Az Azure Active Directory kapcsolat varázsló hello hibák diagnosztizálása
Előző hitelesítési kód észlelése, közben hello varázsló észlelte a nem kompatibilis hitelesítési típus.   

## <a name="what-is-being-checked"></a>Mi ellenőrzés?
**Megjegyzés:** toocorrectly észlelheti az előző hitelesítési kód egy projekt, hello projekt kell kialakítani.  Ha ezt a hibát észlelt, és nem kell egy előző hitelesítési kódot a projektben, építse újra, és próbálkozzon újra.

### <a name="project-types"></a>Projekttípusok
hello varázsló ellenőrzi a projektet, azt is hello megfelelő hitelesítési logikát behelyezése hello projekt kidolgozása hello típusú.  Ha minden tartományvezérlő abból származó `ApiController` hello projektben hello projekt WebAPI projekt számít.  Ha csak azok a tartományvezérlők, amelyek a `MVC.Controller` hello projektben hello projekt egy MVC projekt számít.  Bármi más hello varázsló nem támogatja.

### <a name="compatible-authentication-code"></a>Kompatibilis hitelesítési kód
hello varázsló hello varázslóval korábban konfigurált vagy hello varázsló kompatibilis hitelesítési beállításait is keres.  Ha minden beállítás találhatók, akkor tekinthető párhuzamosan eset és hello megnyílik hello-beállítások megjelenítése.  Ha csak bizonyos hello beállításait meg adva, hibás esetnek minősül.

MVC projekt hello varázsló ellenőrzi a következő beállításokat, amelyeket az előző használatából hello varázsló hello bármelyikét:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

Ezenkívül hello varázsló ellenőrzi, bármely hello Web API-projektet, a beállítást, amellyel előző használatából hello varázsló a következő:

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a>Nem kompatibilis hitelesítési kód
Végezetül hello varázsló megpróbál toodetect hitelesítési kódot azon verzióit, amelyek a Visual Studio korábbi verziói vannak konfigurálva. Ha ezt a hibát kapott, az azt jelenti, a projekt nem kompatibilis hitelesítési típust tartalmaz. hello varázsló észleli a következő típusú hitelesítés Visual Studio korábbi verziói hello:

* Windows-hitelesítés 
* Egyéni felhasználói fiókok 
* Munkahelyi és iskolai fiókok 

Windows-hitelesítés toodetect MVC projekt, hello varázsló megkeresi hello `authentication` elem a **web.config** fájlt.

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

Windows-hitelesítés toodetect a Web API-projektet, a hello varázsló megkeresi hello `IISExpressWindowsAuthentication` a projekt elem **.csproj** fájlt:

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

toodetect egyes felhasználói fiókok hitelesítési, a hello varázsló hello csomag elem keresi a **Packages.config** fájlt.

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

toodetect egy szervezeti fiók hitelesítése régi formája, hello varázsló megkeresi a következő elem hello **web.config**:

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

toochange hello hitelesítés típusa, távolítsa el a hello nem kompatibilis hitelesítés típusa, és futtassa újra hello varázslót.

További információkért lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).

#<a name="next-steps"></a>Következő lépések
- [Hitelesítési forgatókönyvek az Azure AD-hez](active-directory-authentication-scenarios.md)
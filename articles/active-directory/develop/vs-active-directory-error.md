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
# <a name="diagnosing-errors-with-hello-azure-active-directory-connection-wizard"></a><span data-ttu-id="7cbfb-103">Az Azure Active Directory kapcsolat varázsló hello hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="7cbfb-103">Diagnosing errors with hello Azure Active Directory Connection Wizard</span></span>
<span data-ttu-id="7cbfb-104">Előző hitelesítési kód észlelése, közben hello varázsló észlelte a nem kompatibilis hitelesítési típus.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-104">While detecting previous authentication code, hello wizard detected an incompatible authentication type.</span></span>   

## <a name="what-is-being-checked"></a><span data-ttu-id="7cbfb-105">Mi ellenőrzés?</span><span class="sxs-lookup"><span data-stu-id="7cbfb-105">What is being checked?</span></span>
<span data-ttu-id="7cbfb-106">**Megjegyzés:** toocorrectly észlelheti az előző hitelesítési kód egy projekt, hello projekt kell kialakítani.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-106">**Note:** toocorrectly detect previous authentication code in a project, hello project must be built.</span></span>  <span data-ttu-id="7cbfb-107">Ha ezt a hibát észlelt, és nem kell egy előző hitelesítési kódot a projektben, építse újra, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-107">If you encountered this error and you don't have a previous authentication code in your project, rebuild and try again.</span></span>

### <a name="project-types"></a><span data-ttu-id="7cbfb-108">Projekttípusok</span><span class="sxs-lookup"><span data-stu-id="7cbfb-108">Project Types</span></span>
<span data-ttu-id="7cbfb-109">hello varázsló ellenőrzi a projektet, azt is hello megfelelő hitelesítési logikát behelyezése hello projekt kidolgozása hello típusú.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-109">hello wizard checks hello type of project you’re developing so it can inject hello right authentication logic into hello project.</span></span>  <span data-ttu-id="7cbfb-110">Ha minden tartományvezérlő abból származó `ApiController` hello projektben hello projekt WebAPI projekt számít.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-110">If there is any controller that derives from `ApiController` in hello project, hello project is considered a WebAPI project.</span></span>  <span data-ttu-id="7cbfb-111">Ha csak azok a tartományvezérlők, amelyek a `MVC.Controller` hello projektben hello projekt egy MVC projekt számít.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-111">If there are only controllers that derive from `MVC.Controller` in hello project, hello project is considered an MVC project.</span></span>  <span data-ttu-id="7cbfb-112">Bármi más hello varázsló nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-112">Anything else is not supported by hello wizard.</span></span>

### <a name="compatible-authentication-code"></a><span data-ttu-id="7cbfb-113">Kompatibilis hitelesítési kód</span><span class="sxs-lookup"><span data-stu-id="7cbfb-113">Compatible Authentication Code</span></span>
<span data-ttu-id="7cbfb-114">hello varázsló hello varázslóval korábban konfigurált vagy hello varázsló kompatibilis hitelesítési beállításait is keres.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-114">hello wizard also checks for authentication settings that have been previously configured with hello wizard or are compatible with hello wizard.</span></span>  <span data-ttu-id="7cbfb-115">Ha minden beállítás találhatók, akkor tekinthető párhuzamosan eset és hello megnyílik hello-beállítások megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-115">If all settings are present, it is considered a re-entrant case, and hello wizard opens display hello settings.</span></span>  <span data-ttu-id="7cbfb-116">Ha csak bizonyos hello beállításait meg adva, hibás esetnek minősül.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-116">If only some of hello settings are present, it is considered an error case.</span></span>

<span data-ttu-id="7cbfb-117">MVC projekt hello varázsló ellenőrzi a következő beállításokat, amelyeket az előző használatából hello varázsló hello bármelyikét:</span><span class="sxs-lookup"><span data-stu-id="7cbfb-117">In an MVC project, hello wizard checks for any of hello following settings, which result from previous use of hello wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

<span data-ttu-id="7cbfb-118">Ezenkívül hello varázsló ellenőrzi, bármely hello Web API-projektet, a beállítást, amellyel előző használatából hello varázsló a következő:</span><span class="sxs-lookup"><span data-stu-id="7cbfb-118">In addition, hello wizard checks for any of hello following settings in a Web API project, which result from previous use of hello wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a><span data-ttu-id="7cbfb-119">Nem kompatibilis hitelesítési kód</span><span class="sxs-lookup"><span data-stu-id="7cbfb-119">Incompatible Authentication Code</span></span>
<span data-ttu-id="7cbfb-120">Végezetül hello varázsló megpróbál toodetect hitelesítési kódot azon verzióit, amelyek a Visual Studio korábbi verziói vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-120">Finally, hello wizard attempts toodetect versions of authentication code that have been configured with previous versions of Visual Studio.</span></span> <span data-ttu-id="7cbfb-121">Ha ezt a hibát kapott, az azt jelenti, a projekt nem kompatibilis hitelesítési típust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-121">If you received this error, it means your project contains an incompatible authentication type.</span></span> <span data-ttu-id="7cbfb-122">hello varázsló észleli a következő típusú hitelesítés Visual Studio korábbi verziói hello:</span><span class="sxs-lookup"><span data-stu-id="7cbfb-122">hello wizard detects hello following types of authentication from previous versions of Visual Studio:</span></span>

* <span data-ttu-id="7cbfb-123">Windows-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="7cbfb-123">Windows Authentication</span></span> 
* <span data-ttu-id="7cbfb-124">Egyéni felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="7cbfb-124">Individual User Accounts</span></span> 
* <span data-ttu-id="7cbfb-125">Munkahelyi és iskolai fiókok</span><span class="sxs-lookup"><span data-stu-id="7cbfb-125">Organizational Accounts</span></span> 

<span data-ttu-id="7cbfb-126">Windows-hitelesítés toodetect MVC projekt, hello varázsló megkeresi hello `authentication` elem a **web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-126">toodetect Windows Authentication in an MVC project, hello wizard looks for hello `authentication` element from your **web.config** file.</span></span>

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="7cbfb-127">Windows-hitelesítés toodetect a Web API-projektet, a hello varázsló megkeresi hello `IISExpressWindowsAuthentication` a projekt elem **.csproj** fájlt:</span><span class="sxs-lookup"><span data-stu-id="7cbfb-127">toodetect Windows Authentication in a Web API project, hello wizard looks for hello `IISExpressWindowsAuthentication` element from your project's **.csproj** file:</span></span>

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

<span data-ttu-id="7cbfb-128">toodetect egyes felhasználói fiókok hitelesítési, a hello varázsló hello csomag elem keresi a **Packages.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-128">toodetect Individual User Accounts authentication, hello wizard looks for hello package element from your **Packages.config** file.</span></span>

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

<span data-ttu-id="7cbfb-129">toodetect egy szervezeti fiók hitelesítése régi formája, hello varázsló megkeresi a következő elem hello **web.config**:</span><span class="sxs-lookup"><span data-stu-id="7cbfb-129">toodetect an old form of Organizational Account authentication, hello wizard looks for hello following element from **web.config**:</span></span>

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="7cbfb-130">toochange hello hitelesítés típusa, távolítsa el a hello nem kompatibilis hitelesítés típusa, és futtassa újra hello varázslót.</span><span class="sxs-lookup"><span data-stu-id="7cbfb-130">toochange hello authentication type, remove hello incompatible authentication type and run hello wizard again.</span></span>

<span data-ttu-id="7cbfb-131">További információkért lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="7cbfb-131">For more information, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).</span></span>

#<a name="next-steps"></a><span data-ttu-id="7cbfb-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cbfb-132">Next steps</span></span>
- [<span data-ttu-id="7cbfb-133">Hitelesítési forgatókönyvek az Azure AD-hez</span><span class="sxs-lookup"><span data-stu-id="7cbfb-133">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)
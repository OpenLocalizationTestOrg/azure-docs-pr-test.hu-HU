---
title: "Az Azure Active Directory kapcsolat varázsló hibák diagnosztizálása"
description: "Az active directory kapcsolat varázsló nem kompatibilis hitelesítési típust észlelt"
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
ms.openlocfilehash: 4f29f62b2996cae98b02c1ed5fcb59eca09301ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="diagnosing-errors-with-the-azure-active-directory-connection-wizard"></a><span data-ttu-id="ccb49-103">Az Azure Active Directory kapcsolat varázsló hibák diagnosztizálása</span><span class="sxs-lookup"><span data-stu-id="ccb49-103">Diagnosing errors with the Azure Active Directory Connection Wizard</span></span>
<span data-ttu-id="ccb49-104">Történt észlelésekor előző hitelesítési kódot, a varázsló nem kompatibilis hitelesítési típust észlelt.</span><span class="sxs-lookup"><span data-stu-id="ccb49-104">While detecting previous authentication code, the wizard detected an incompatible authentication type.</span></span>   

## <a name="what-is-being-checked"></a><span data-ttu-id="ccb49-105">Mi ellenőrzés?</span><span class="sxs-lookup"><span data-stu-id="ccb49-105">What is being checked?</span></span>
<span data-ttu-id="ccb49-106">**Megjegyzés:** megfelelően azonosíthatók a projekt korábbi hitelesítési kódot, a projekt kell kialakítani.</span><span class="sxs-lookup"><span data-stu-id="ccb49-106">**Note:** To correctly detect previous authentication code in a project, the project must be built.</span></span>  <span data-ttu-id="ccb49-107">Ha ezt a hibát észlelt, és nem kell egy előző hitelesítési kódot a projektben, építse újra, és próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="ccb49-107">If you encountered this error and you don't have a previous authentication code in your project, rebuild and try again.</span></span>

### <a name="project-types"></a><span data-ttu-id="ccb49-108">Projekttípusok</span><span class="sxs-lookup"><span data-stu-id="ccb49-108">Project Types</span></span>
<span data-ttu-id="ccb49-109">A varázsló ellenőrzi, így azt a megfelelő hitelesítési logikát is szúrjon be a projekt kidolgozása projekt típusától.</span><span class="sxs-lookup"><span data-stu-id="ccb49-109">The wizard checks the type of project you’re developing so it can inject the right authentication logic into the project.</span></span>  <span data-ttu-id="ccb49-110">Ha minden tartományvezérlő abból származó `ApiController` a projektben a projekt WebAPI projekt számít.</span><span class="sxs-lookup"><span data-stu-id="ccb49-110">If there is any controller that derives from `ApiController` in the project, the project is considered a WebAPI project.</span></span>  <span data-ttu-id="ccb49-111">Ha csak azok a tartományvezérlők, amelyek a `MVC.Controller` a projektben a projekt egy MVC projekt számít.</span><span class="sxs-lookup"><span data-stu-id="ccb49-111">If there are only controllers that derive from `MVC.Controller` in the project, the project is considered an MVC project.</span></span>  <span data-ttu-id="ccb49-112">Bármi más a varázsló által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="ccb49-112">Anything else is not supported by the wizard.</span></span>

### <a name="compatible-authentication-code"></a><span data-ttu-id="ccb49-113">Kompatibilis hitelesítési kód</span><span class="sxs-lookup"><span data-stu-id="ccb49-113">Compatible Authentication Code</span></span>
<span data-ttu-id="ccb49-114">A varázsló is ellenőrzi a hitelesítési beállításokat, amelyek a varázsló a korábban konfigurált vagy kompatibilisek-e a varázslót.</span><span class="sxs-lookup"><span data-stu-id="ccb49-114">The wizard also checks for authentication settings that have been previously configured with the wizard or are compatible with the wizard.</span></span>  <span data-ttu-id="ccb49-115">Ha minden beállítás, akkor tekinthető párhuzamosan eset, és megnyílik a varázsló megjeleníti a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ccb49-115">If all settings are present, it is considered a re-entrant case, and the wizard opens display the settings.</span></span>  <span data-ttu-id="ccb49-116">Ha csak néhány beállítás találhatók, hibás esetnek minősül.</span><span class="sxs-lookup"><span data-stu-id="ccb49-116">If only some of the settings are present, it is considered an error case.</span></span>

<span data-ttu-id="ccb49-117">MVC projekt a varázsló ellenőrzi, az alábbi beállításokat, amelyeket a varázsló előző használatából bármelyikéhez:</span><span class="sxs-lookup"><span data-stu-id="ccb49-117">In an MVC project, the wizard checks for any of the following settings, which result from previous use of the wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:AADInstance" value="" />
    <add key="ida:PostLogoutRedirectUri" value="" />

<span data-ttu-id="ccb49-118">Emellett a varázsló ellenőrzi a Web API-projektet, a következő beállítások a varázsló előző használatából fakadó:</span><span class="sxs-lookup"><span data-stu-id="ccb49-118">In addition, the wizard checks for any of the following settings in a Web API project, which result from previous use of the wizard:</span></span>

    <add key="ida:ClientId" value="" />
    <add key="ida:Tenant" value="" />
    <add key="ida:Audience" value="" />

### <a name="incompatible-authentication-code"></a><span data-ttu-id="ccb49-119">Nem kompatibilis hitelesítési kód</span><span class="sxs-lookup"><span data-stu-id="ccb49-119">Incompatible Authentication Code</span></span>
<span data-ttu-id="ccb49-120">Végezetül a varázsló megpróbálja észlelni a hitelesítési kód vannak konfigurálva a Visual Studio korábbi verziói-verziók.</span><span class="sxs-lookup"><span data-stu-id="ccb49-120">Finally, the wizard attempts to detect versions of authentication code that have been configured with previous versions of Visual Studio.</span></span> <span data-ttu-id="ccb49-121">Ha ezt a hibát kapott, az azt jelenti, a projekt nem kompatibilis hitelesítési típust tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ccb49-121">If you received this error, it means your project contains an incompatible authentication type.</span></span> <span data-ttu-id="ccb49-122">A varázsló észleli a következő típusú hitelesítés a Visual Studio korábbi verzióiról:</span><span class="sxs-lookup"><span data-stu-id="ccb49-122">The wizard detects the following types of authentication from previous versions of Visual Studio:</span></span>

* <span data-ttu-id="ccb49-123">Windows-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="ccb49-123">Windows Authentication</span></span> 
* <span data-ttu-id="ccb49-124">Egyéni felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="ccb49-124">Individual User Accounts</span></span> 
* <span data-ttu-id="ccb49-125">Munkahelyi és iskolai fiókok</span><span class="sxs-lookup"><span data-stu-id="ccb49-125">Organizational Accounts</span></span> 

<span data-ttu-id="ccb49-126">MVC projekt észleli a Windows-hitelesítést, a varázsló megkeresi a `authentication` elem a **web.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ccb49-126">To detect Windows Authentication in an MVC project, the wizard looks for the `authentication` element from your **web.config** file.</span></span>

<pre>
    &lt;configuration&gt;
        &lt;system.web&gt;
            <span style="background-color: yellow">&lt;authentication mode="Windows" /&gt;</span>
        &lt;/system.web&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="ccb49-127">Windows-hitelesítés azonosíthatók a Web API-projektet, a varázsló megkeresi a `IISExpressWindowsAuthentication` a projekt elem **.csproj** fájlt:</span><span class="sxs-lookup"><span data-stu-id="ccb49-127">To detect Windows Authentication in a Web API project, the wizard looks for the `IISExpressWindowsAuthentication` element from your project's **.csproj** file:</span></span>

<pre>
    &lt;Project&gt;
        &lt;PropertyGroup&gt;
            <span style="background-color: yellow">&lt;IISExpressWindowsAuthentication&gt;enabled&lt;/IISExpressWindowsAuthentication&gt;</span>
        &lt;/PropertyGroup>
    &lt;/Project&gt;
</pre>

<span data-ttu-id="ccb49-128">Egyes felhasználói fiókok hitelesítési észlelését, a varázsló megkeresi a csomag elem a **Packages.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ccb49-128">To detect Individual User Accounts authentication, the wizard looks for the package element from your **Packages.config** file.</span></span>

<pre>
    &lt;packages&gt;
        <span style="background-color: yellow">&lt;package id="Microsoft.AspNet.Identity.EntityFramework" version="2.1.0" targetFramework="net45" /&gt;</span>
    &lt;/packages&gt;
</pre>

<span data-ttu-id="ccb49-129">Egy szervezeti fiók hitelesítése régi formája észlelését, a varázsló megkeresi a következő elem **web.config**:</span><span class="sxs-lookup"><span data-stu-id="ccb49-129">To detect an old form of Organizational Account authentication, the wizard looks for the following element from **web.config**:</span></span>

<pre>
    &lt;configuration&gt;
        &lt;appSettings&gt;
            <span style="background-color: yellow">&lt;add key="ida:Realm" value="***" /&gt;</span>
        &lt;/appSettings&gt;
    &lt;/configuration&gt;
</pre>

<span data-ttu-id="ccb49-130">A hitelesítési típus módosításához távolítsa el a nem kompatibilis hitelesítés típusát, és futtassa újra a varázslót.</span><span class="sxs-lookup"><span data-stu-id="ccb49-130">To change the authentication type, remove the incompatible authentication type and run the wizard again.</span></span>

<span data-ttu-id="ccb49-131">További információkért lásd: [hitelesítési forgatókönyvek az Azure AD](active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="ccb49-131">For more information, see [Authentication Scenarios for Azure AD](active-directory-authentication-scenarios.md).</span></span>

#<a name="next-steps"></a><span data-ttu-id="ccb49-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ccb49-132">Next steps</span></span>
- [<span data-ttu-id="ccb49-133">Hitelesítési forgatókönyvek az Azure AD-hez</span><span class="sxs-lookup"><span data-stu-id="ccb49-133">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)
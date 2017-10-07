---
title: "melyik alkalmazás toouse írja be, amikor egy alkalmazás hozzáadása aaaHow toochoose |} Microsoft Docs"
description: "Hello támogatott ismerteti az alkalmazások integrálva az Azure AD és a kapcsolódó konfigurációs beállítások"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 46e3672e7f5048b0fa54171f0fc169362c9d5ac6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochoose-which-application-type-toouse-when-adding-an-application"></a><span data-ttu-id="c8789-103">Hogyan toochoose melyik alkalmazás toouse írja be, amikor egy alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8789-103">How toochoose which application type toouse when adding an application</span></span>

<span data-ttu-id="c8789-104">Ez a cikk segítséget toounderstand hello négy fő típusú alkalmazások integrálható az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="c8789-104">This article help you toounderstand hello four main types of applications you can integrate with Azure AD:</span></span>

* <span data-ttu-id="c8789-105">Támogatott források és műveletek szerint</span><span class="sxs-lookup"><span data-stu-id="c8789-105">What is supported by each of them</span></span>
* <span data-ttu-id="c8789-106">Miért akkor érdemes választania, melyik alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c8789-106">Why you might choose which application</span></span>
* <span data-ttu-id="c8789-107">Hogyan tooconfigure ezen alkalmazás core tulajdonságai, például a felhasználók **kiépített**, vagy mi **egyszeri bejelentkezés** technológia toouse.</span><span class="sxs-lookup"><span data-stu-id="c8789-107">How tooconfigure those application’s core properties, like how users are **provisioned**, or what **single sign-on** technology toouse.</span></span>

## <a name="supported-application-types-in-azure-ad"></a><span data-ttu-id="c8789-108">Az Azure ad-ben támogatott alkalmazástípusok</span><span class="sxs-lookup"><span data-stu-id="c8789-108">Supported application types in Azure AD</span></span>

<span data-ttu-id="c8789-109">Az Azure AD által támogatott négy fő típusokat adhat hozzá hello **Hozzáadás** szolgáltatás alatt található **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8789-109">Azure AD supports four main application types that you can add using hello **Add** feature found under **Enterprise Applications**.</span></span> <span data-ttu-id="c8789-110">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="c8789-110">These include:</span></span>

-   <span data-ttu-id="c8789-111">**Azure AD-gyűjtemény alkalmazások** –, amelyet az egyszeri bejelentkezés az Azure ad-vel előre integrált alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c8789-111">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

-   <span data-ttu-id="c8789-112">**Alkalmazás Proxy alkalmazások** – a helyszíni környezetben futó tooprovide biztonságos egyszeri bejelentkezést tooexternally kívánt alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c8789-112">**Application Proxy Applications** – An application running in your on-premises environment that you want tooprovide secure single-sign on tooexternally.</span></span>

-   <span data-ttu-id="c8789-113">**Egyéni által fejlesztett alkalmazások** – olyan alkalmazás, amely a szervezet által toodevelop a hello Azure AD alkalmazás-fejlesztő Platform, de, amely nem még létezik.</span><span class="sxs-lookup"><span data-stu-id="c8789-113">**Custom-developed Applications** – An application that your organization wishes toodevelop on hello Azure AD Application Development Platform, but that may not exist yet.</span></span>

-   <span data-ttu-id="c8789-114">**Nem-gyűjtemény alkalmazások** – kapcsolja a saját alkalmazásai!</span><span class="sxs-lookup"><span data-stu-id="c8789-114">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="c8789-115">Bármely kívánt webes hivatkozást, vagy egy felhasználónevet és jelszót a mezőben, amely alkalmazás SAML vagy az OpenID Connect protokollt támogatja, vagy az egyszeri bejelentkezés az Azure ad-val toointegrate verzióváltáshoz SCIM támogatja.</span><span class="sxs-lookup"><span data-stu-id="c8789-115">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish toointegrate for single sign-on with Azure AD.</span></span>

## <a name="features-and-capabilities-supported-by-all-hello-above-application-types"></a><span data-ttu-id="c8789-116">Funkciók és képességek összes hello fent alkalmazástípusok által támogatott</span><span class="sxs-lookup"><span data-stu-id="c8789-116">Features and capabilities supported by all hello above application types</span></span>

<span data-ttu-id="c8789-117">hello következő szolgáltatások támogatottak bármelyik fenti 4 alkalmazástípusok hello Azure AD-ben:</span><span class="sxs-lookup"><span data-stu-id="c8789-117">hello following features are supported by any of hello above 4 application types in Azure AD:</span></span>

-   <span data-ttu-id="c8789-118">**Gyors üzembe helyezési** – induláshoz alkalmazással gyorsan következő [egyszerű üzembe helyezés lépései](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span><span class="sxs-lookup"><span data-stu-id="c8789-118">**Quick start** – get going with an application quickly by following [simple deployment steps](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span></span>

-   <span data-ttu-id="c8789-119">**Általános tulajdonságok felügyeleti** – beszerzése egy [közvetlen mélyhivatkozás](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan alkalmazás [hello branding testreszabása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) egy alkalmazás vagy [hello alkalmazásletiltása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) az összes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="c8789-119">**General properties management** – get a [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) tooan application, [customize hello branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) of an application, or [disable hello application](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) for all users.</span></span>

-   <span data-ttu-id="c8789-120">**Felhasználók és csoportok kezelése** – [hozzárendelése](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) vagy [eltávolítása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) felhasználók és csoportok tooan alkalmazás, és szükség esetén hozzárendelése hello az adott alkalmazást szerepkörök a felhasználók és csoportok hozzáférése</span><span class="sxs-lookup"><span data-stu-id="c8789-120">**User and group management** – [assign](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) or [remove](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) users and groups tooan application, and optionally assign hello specific application roles these users and groups have access to</span></span>

-   <span data-ttu-id="c8789-121">**Önkiszolgáló alkalmazás-hozzáférés** – engedélyezése a felhasználók toorequest [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan alkalmazás az alkalmazás-hozzáférés a panelek vagy egy alkalmazás közvetlenül hozzáadásával vagy [ Csatlakozás egy önkiszolgáló engedélyezett csoporthoz](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), opcionálisan mentén hello üzleti jóváhagyásra van szükség módja</span><span class="sxs-lookup"><span data-stu-id="c8789-121">**Self-service application access** – enable your users toorequest [self-service application access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) tooan application from their Application Access Panels either by adding an application directly or [joining a self-service enabled group](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), optionally requiring business approval along hello way</span></span>

-   <span data-ttu-id="c8789-122">**Bejelentkezési naplók** – lásd: [összes hello bejelentkezések tooan alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), vagy az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c8789-122">**Sign-in logs** – see [all hello sign-ins tooan application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), or all your applications</span></span>

-   <span data-ttu-id="c8789-123">**A naplók** – lásd: [naplók módosítások tooan alkalmazással kapcsolatos részletes](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), vagy tooall az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c8789-123">**Audit logs** – see [detailed audit logs about modifications tooan application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), or tooall your applications</span></span>

-   <span data-ttu-id="c8789-124">**Feltételes és kockázati-alapú hozzáférés** – hatékony beállított [feltétel-alapú hozzáférési szabályok](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , amely akkor lépnek érvénybe, ha a felhasználó toosign tooa adott alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="c8789-124">**Conditional and risk-based access** – set powerful [condition-based access rules](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) that are enforced when users attempt toosign in tooa specific application</span></span>

-   <span data-ttu-id="c8789-125">**Engedélyek megtekintése** – hello bármelyikét megtekintése [OAuth2 engedélyek](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) alkalmazás rendelkezik hozzáféréssel tooin a címtár egyetlen helyéről</span><span class="sxs-lookup"><span data-stu-id="c8789-125">**Permissions view** – view any of hello [OAuth2 permissions](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) an application has access tooin your directory from a single place</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="c8789-126">Egyszeri bejelentkezés és a létesítési módot támogat. az adott alkalmazást típusok</span><span class="sxs-lookup"><span data-stu-id="c8789-126">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="c8789-127">hello az alábbi táblázat hello különböző egyszeri bejelentkezést és a létesítési módot támogat az egyes alkalmazástípusok fent hello által.</span><span class="sxs-lookup"><span data-stu-id="c8789-127">hello table below describes hello different single sign-on and provisioning modes supported by each of hello above application types.</span></span> <span data-ttu-id="c8789-128">Használhatja a tábla toohelp toounderstand, melyik alkalmazás szüksége tooadd toosupport adott cél.</span><span class="sxs-lookup"><span data-stu-id="c8789-128">You can use this table toohelp you toounderstand which application you need tooadd toosupport a specific goal.</span></span>

  ![Alkalmazás típusú tábla](./media/application-tables/table1.png)

## <a name="how-toochoose-a-single-sign-on-mode"></a><span data-ttu-id="c8789-130">Hogyan toochoose a egyszeri bejelentkezés módját</span><span class="sxs-lookup"><span data-stu-id="c8789-130">How toochoose a single sign-on mode</span></span>

<span data-ttu-id="c8789-131">hello támogatott **egyszeri bejelentkezés** módjainak Azure AD-alkalmazások az alábbiak.</span><span class="sxs-lookup"><span data-stu-id="c8789-131">hello supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="c8789-132">**Az Azure AD az egyszeri bejelentkezés le van tiltva** – válassza ki az Azure AD az egyszeri bejelentkezés le van tiltva **egyszeri bejelentkezés mód** Ha még nem áll készen toointegrate ehhez az alkalmazáshoz az egyszeri bejelentkezés az Azure ad-vel, vagy egyszerűen csak tesztelés kimenő</span><span class="sxs-lookup"><span data-stu-id="c8789-132">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready toointegrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="c8789-133">**Bejelentkezés kapcsolódó** – hello válasszon [társított bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha valamelyik alkalmazásnál, amely a már meglévő egyszeri bejelentkezési megoldással már csatlakoztatva van, vagy ha csak a felhasználók a hivatkozás egy egyszerű toopublish azok [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) vagy [Office 365 alkalmazásindító](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="c8789-133">**Linked Sign-on** – choose hello [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want toopublish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="c8789-134">**Jelszó alapú bejelentkezés** – hello válasszon [jelszóalapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha az alkalmazás Renderelés egy HTML-felhasználónév és jelszó mezőt, és azt szeretné, hogy toostore, felhasználónév és jelszó biztonságosan toobe újból később toohello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c8789-134">**Password-based Sign-on** – choose hello [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want toostore that username and password securely toobe replayed toohello application later</span></span>

-   <span data-ttu-id="c8789-135">**SAML-alapú bejelentkezés** – hello válasszon [SAML-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) mód, ha az alkalmazás hello SAML vagy az OpenID Connect protokollt támogatja, vagy azt szeretné, hogy toobe képes toomap felhasználók egyszeri bejelentkezés toospecific alkalmazás szerepkörök alapján a SAML meghatározott szabályok jogcímek *</span><span class="sxs-lookup"><span data-stu-id="c8789-135">**SAML-based Sign-on** – choose hello [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports hello SAML or OpenID Connect protocols, or you want toobe able toomap users toospecific application roles based on rules you define in your SAML claims *</span></span>

   >[!NOTE]
   ><span data-ttu-id="c8789-136">Ez a beállítás nem érhető el, ha hello alkalmazásproxy úgy van konfigurálva, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c8789-136">This option is not available when hello application proxy is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="c8789-137">**Fejléc-alapú bejelentkezés** – válassza ezt a [fejléc-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) egyetlen bejelentkezés módja Ha valamely alkalmazás használatával, amely támogatja a HTTP-fejléc PingAccess alapján kívánja tooperform egyszeri bejelentkezést a túl hitelesítés</span><span class="sxs-lookup"><span data-stu-id="c8789-137">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish tooperform single-sign on too</span></span>

   >[!NOTE]
   ><span data-ttu-id="c8789-138">Ez a beállítás csak érhető el, ha hello proxy és PingAccess úgy van konfigurálva, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c8789-138">This option is only available when hello application proxy and PingAccess is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="c8789-139">**Integrált Windows-hitelesítés** – hello válasszon [integrált Windows-hitelesítés](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) túl a kívánja tooperform egyszeri bejelentkezést a helyszíni WIA alkalmazás kitettségének üzemmódot egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8789-139">**Integrated Windows Authentication** – choose hello [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish tooperform single-sign on too</span></span>

   >[!NOTE]
   ><span data-ttu-id="c8789-140">Ez a beállítás csak érhető el, ha hello alkalmazásproxy úgy van konfigurálva, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c8789-140">This option is only available when hello application proxy is configured for an application.</span></span>
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="c8789-141">Egyszeri bejelentkezés módjainak egyéni által fejlesztett alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c8789-141">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="c8789-142">Egyéni hello használatával fejlesztett alkalmazások rendelkeznek [egyéni által fejlesztett alkalmazás](#_Custom-Developed_Applications) élmény is további egyszeri bejelentkezés módot támogat fent nem említett.</span><span class="sxs-lookup"><span data-stu-id="c8789-142">Applications you have custom developed through hello [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="c8789-143">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="c8789-143">These include:</span></span>

-   <span data-ttu-id="c8789-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="c8789-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="c8789-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="c8789-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="c8789-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="c8789-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="c8789-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="c8789-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="c8789-148">Olvasási hello [Azure Active Directory fejlesztői útmutatója](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn hogyan egyéni által fejlesztett toocreate alkalmazást, amely támogatja ezeket egyszeri bejelentkezési módok többet.</span><span class="sxs-lookup"><span data-stu-id="c8789-148">Read hello [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) toolearn more about how toocreate a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-tooset-an-applications-single-sign-on-mode"></a><span data-ttu-id="c8789-149">Hogyan tooset alkalmazás csomagazonosítóját egyszeri bejelentkezés mód</span><span class="sxs-lookup"><span data-stu-id="c8789-149">How tooset an application’s single sign-on mode</span></span>

<span data-ttu-id="c8789-150">tooset alkalmazás **egyszeri bejelentkezés** mód, kövesse az alábbi hello utasítások:</span><span class="sxs-lookup"><span data-stu-id="c8789-150">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="c8789-151">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="c8789-151">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="c8789-152">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="c8789-152">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c8789-153">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="c8789-153">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c8789-154">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="c8789-154">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="c8789-155">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c8789-155">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="c8789-156">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="c8789-156">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="c8789-157">Válassza ki legyen tooconfigure egyszeri bejelentkezés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8789-157">Select hello application for which you want tooconfigure single sign-on.</span></span>

7.  <span data-ttu-id="c8789-158">Amikor hello alkalmazás betölt, kattintson a **egyszeri bejelentkezés** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="c8789-158">Once hello application loads, click **Single sign-on** from hello application’s left hand navigation menu.</span></span>

## <a name="how-toochoose-a-provisioning-mode"></a><span data-ttu-id="c8789-159">Hogyan toochoose a létesítés</span><span class="sxs-lookup"><span data-stu-id="c8789-159">How toochoose a provisioning mode</span></span>

-   <span data-ttu-id="c8789-160">**Manuális létesítési** – hello válasszon [manuális](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) kiépítési üzemmódban, ha meglévő fiókokat, vagy kívül az Azure AD alkalmazás toomanage fiókok kívánja.</span><span class="sxs-lookup"><span data-stu-id="c8789-160">**Manual Provisioning** – choose hello [Manual](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) provisioning mode if you have existing accounts, or wish toomanage accounts for this application outside of Azure AD.</span></span>

-   <span data-ttu-id="c8789-161">**Automatikus kiépítés** – hello válasszon [automatikus](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **létesítés** felhasználói fiókot toothis, ha azt szeretné, hogy tooenable automatikus API-alapú üzembe helyezés és/vagy a megszüntetést alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c8789-161">**Automatic Provisioning** – choose hello [Automatic](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **provisioning mode** if you want tooenable automatic API-based provisioning and/or de-provisioning of user accounts toothis application</span></span> 

   >[!NOTE]
   ><span data-ttu-id="c8789-162">Ez a beállítás csak hello alkalmazást érhető **kiemelt** hello kategóriáját [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="c8789-162">This option is available only for applications within hello **featured** category of hello [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span></span>
   >
   >

-   <span data-ttu-id="c8789-163">**SCIM-alapú automatikus üzembe helyezés** – használható [SCIM-alapú automatikus üzembe helyezés](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Ha az alkalmazás támogatja-e a módosítások toousers és a csoport, amelyek a módosítások automatikusan kibocsátott hello SCIM protokoll az Azure ad-vel integrált tooany alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c8789-163">**SCIM-based Automatic Provisioning** – use [SCIM-based Automatic Provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) if your application supports hello SCIM protocol for detecting changes toousers and groups, which are automatically emitted for changes tooany application integrated with Azure AD</span></span> 

   >[!NOTE]
   ><span data-ttu-id="c8789-164">Ez a beállítás nem szerepel egy üzembe helyezési mód, de minden olyan alkalmazásnál, az Azure ad-vel integrált alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="c8789-164">This option is not listed as a specific provisioning mode, but is enabled by default for all applications that are integrated with Azure AD.</span></span>
   >
   >

## <a name="how-tooset-an-applications-provisioning-mode"></a><span data-ttu-id="c8789-165">Hogyan a tooset az alkalmazás csomagazonosítóját kiépítési üzemmódban</span><span class="sxs-lookup"><span data-stu-id="c8789-165">How tooset an application’s provisioning mode</span></span>

<span data-ttu-id="c8789-166">tooset alkalmazás **kiépítés** mód, kövesse az alábbi hello utasítások:</span><span class="sxs-lookup"><span data-stu-id="c8789-166">tooset an application’s **provisioning** mode, follow hello instructions below:</span></span>

<span data-ttu-id="c8789-167">tooset alkalmazás **egyszeri bejelentkezés** mód, kövesse az alábbi hello utasítások:</span><span class="sxs-lookup"><span data-stu-id="c8789-167">tooset an application’s **single sign-on** mode, follow hello instructions below:</span></span>

1.  <span data-ttu-id="c8789-168">Nyitott hello [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="c8789-168">Open hello [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="c8789-169">Nyissa meg hello **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** hello fő bal oldali navigációs menü hello alján.</span><span class="sxs-lookup"><span data-stu-id="c8789-169">Open hello **Azure Active Directory Extension** by clicking **More services** at hello bottom of hello main left hand navigation menu.</span></span>

3.  <span data-ttu-id="c8789-170">Írja be a **"Azure Active Directory**" hello szűrő keresési mezőbe, és válassza hello **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="c8789-170">Type in **“Azure Active Directory**” in hello filter search box and select hello **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="c8789-171">Kattintson a **vállalati alkalmazások** hello Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="c8789-171">click **Enterprise Applications** from hello Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="c8789-172">Kattintson a **összes alkalmazás** tooview az alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="c8789-172">click **All Applications** tooview a list of all your applications.</span></span>

  * <span data-ttu-id="c8789-173">Ha hello alkalmazás jelenik itt meg nem látja, akkor hello **szűrő** hello hello tetején vezérlő **összes alkalmazások listáját** és set hello **megjelenítése** beállítás túl **Minden alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="c8789-173">If you do not see hello application you want show up here, use hello **Filter** control at hello top of hello **All Applications List** and set hello **Show** option too**All Applications.**</span></span>

6.  <span data-ttu-id="c8789-174">Válassza ki a kívánt tooconfigure kiépítés hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8789-174">Select hello application for which you want tooconfigure provisioning.</span></span>

7.  <span data-ttu-id="c8789-175">Amikor hello alkalmazás betölt, kattintson a **kiépítési** hello alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="c8789-175">Once hello application loads, click **Provisioning** from hello application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c8789-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c8789-176">Next steps</span></span>
[<span data-ttu-id="c8789-177">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="c8789-177">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)

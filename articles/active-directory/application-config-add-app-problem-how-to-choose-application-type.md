---
title: "Alkalmazás típust használja, ha egy alkalmazás hozzáadása kiválasztása |} Microsoft Docs"
description: "A támogatott ismerteti az alkalmazások integrálva az Azure AD és a kapcsolódó konfigurációs beállítások"
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
ms.openlocfilehash: e0d41d1933531c2c633613bcbc1bbcbf075d6a69
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-choose-which-application-type-to-use-when-adding-an-application"></a><span data-ttu-id="8d14a-103">Alkalmazás típust használja, ha egy alkalmazás hozzáadása kiválasztása</span><span class="sxs-lookup"><span data-stu-id="8d14a-103">How to choose which application type to use when adding an application</span></span>

<span data-ttu-id="8d14a-104">Ez a cikk segítenek megérteni a négy fő típusú alkalmazások integrálható az Azure AD:</span><span class="sxs-lookup"><span data-stu-id="8d14a-104">This article help you to understand the four main types of applications you can integrate with Azure AD:</span></span>

* <span data-ttu-id="8d14a-105">Támogatott források és műveletek szerint</span><span class="sxs-lookup"><span data-stu-id="8d14a-105">What is supported by each of them</span></span>
* <span data-ttu-id="8d14a-106">Miért akkor érdemes választania, melyik alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8d14a-106">Why you might choose which application</span></span>
* <span data-ttu-id="8d14a-107">Ezen alkalmazás core tulajdonságai, például a felhasználók konfigurálásával **kiépített**, vagy mi **egyszeri bejelentkezés** technológiát használatára.</span><span class="sxs-lookup"><span data-stu-id="8d14a-107">How to configure those application’s core properties, like how users are **provisioned**, or what **single sign-on** technology to use.</span></span>

## <a name="supported-application-types-in-azure-ad"></a><span data-ttu-id="8d14a-108">Az Azure ad-ben támogatott alkalmazástípusok</span><span class="sxs-lookup"><span data-stu-id="8d14a-108">Supported application types in Azure AD</span></span>

<span data-ttu-id="8d14a-109">Az Azure AD támogatja négy fő típusokat adhat hozzá a **Hozzáadás** szolgáltatás alatt található **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8d14a-109">Azure AD supports four main application types that you can add using the **Add** feature found under **Enterprise Applications**.</span></span> <span data-ttu-id="8d14a-110">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="8d14a-110">These include:</span></span>

-   <span data-ttu-id="8d14a-111">**Azure AD-gyűjtemény alkalmazások** –, amelyet az egyszeri bejelentkezés az Azure ad-vel előre integrált alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d14a-111">**Azure AD Gallery Applications** – An application that has been pre-integrated for single sign-on with Azure AD.</span></span>

-   <span data-ttu-id="8d14a-112">**Alkalmazás Proxy alkalmazások** – szeretne biztosítani a biztonságos egyszeri bejelentkezést a külsőleg a helyszíni környezetben futó alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8d14a-112">**Application Proxy Applications** – An application running in your on-premises environment that you want to provide secure single-sign on to externally.</span></span>

-   <span data-ttu-id="8d14a-113">**Egyéni által fejlesztett alkalmazások** – az alkalmazás, amely a szervezet által platformon az Azure AD alkalmazás fejlesztési kialakításához, de, amely nem még létezik.</span><span class="sxs-lookup"><span data-stu-id="8d14a-113">**Custom-developed Applications** – An application that your organization wishes to develop on the Azure AD Application Development Platform, but that may not exist yet.</span></span>

-   <span data-ttu-id="8d14a-114">**Nem-gyűjtemény alkalmazások** – kapcsolja a saját alkalmazásai!</span><span class="sxs-lookup"><span data-stu-id="8d14a-114">**Non-Gallery Applications** – Bring your own applications!</span></span> <span data-ttu-id="8d14a-115">Bármely kívánt webes hivatkozást, vagy egy felhasználónevet és jelszót a mezőben, amely alkalmazás SAML vagy az OpenID Connect protokollt támogatja, vagy az egyszeri bejelentkezés az Azure ad-vel integrálni kívánt SCIM támogatja.</span><span class="sxs-lookup"><span data-stu-id="8d14a-115">Any web link you want, or any application which renders a username and password field, supports SAML or OpenID Connect protocols, or supports SCIM which you wish to integrate for single sign-on with Azure AD.</span></span>

## <a name="features-and-capabilities-supported-by-all-the-above-application-types"></a><span data-ttu-id="8d14a-116">Funkciók és képességek az összes fenti alkalmazástípusok által támogatott</span><span class="sxs-lookup"><span data-stu-id="8d14a-116">Features and capabilities supported by all the above application types</span></span>

<span data-ttu-id="8d14a-117">Az alábbi szolgáltatások az Azure AD által a fenti 4 alkalmazás típusok támogatottak:</span><span class="sxs-lookup"><span data-stu-id="8d14a-117">The following features are supported by any of the above 4 application types in Azure AD:</span></span>

-   <span data-ttu-id="8d14a-118">**Gyors üzembe helyezési** – induláshoz alkalmazással gyorsan következő [egyszerű üzembe helyezés lépései](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span><span class="sxs-lookup"><span data-stu-id="8d14a-118">**Quick start** – get going with an application quickly by following [simple deployment steps](https://docs.microsoft.com/azure/active-directory/active-directory-integrating-applications-getting-started)</span></span>

-   <span data-ttu-id="8d14a-119">**Általános tulajdonságok felügyeleti** – lekérni egy [közvetlen mélyhivatkozás](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) egy alkalmazást, [testre szabhatja a márkajelzési](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) egy alkalmazás vagy [az alkalmazás letiltására](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) az összes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="8d14a-119">**General properties management** – get a [direct deeplink](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users) to an application, [customize the branding](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-change-app-logo-user-azure-portal) of an application, or [disable the application](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) for all users.</span></span>

-   <span data-ttu-id="8d14a-120">**Felhasználók és csoportok kezelése** – [hozzárendelése](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) vagy [eltávolítása](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) felhasználókat és csoportokat egy alkalmazást, és ezek a felhasználók az adott alkalmazás szerepköröket rendelhet, és a csoportok fér hozzá a</span><span class="sxs-lookup"><span data-stu-id="8d14a-120">**User and group management** – [assign](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) or [remove](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) users and groups to an application, and optionally assign the specific application roles these users and groups have access to</span></span>

-   <span data-ttu-id="8d14a-121">**Önkiszolgáló alkalmazás-hozzáférés** – lehetővé teszi a felhasználók kéréséhez [önkiszolgáló alkalmazás-hozzáférés](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) az alkalmazás hozzáférési panelek vagy hozzáadásával egy alkalmazás közvetlenül az alkalmazáshoz való vagy [önkiszolgáló engedélyezett csoporthoz való csatlakozásra](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), opcionálisan menet üzleti jóváhagyásra van szükség</span><span class="sxs-lookup"><span data-stu-id="8d14a-121">**Self-service application access** – enable your users to request [self-service application access](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) to an application from their Application Access Panels either by adding an application directly or [joining a self-service enabled group](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management), optionally requiring business approval along the way</span></span>

-   <span data-ttu-id="8d14a-122">**Bejelentkezési naplók** – lásd: [minden a bejelentkezéseket egy alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), vagy az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8d14a-122">**Sign-in logs** – see [all the sign-ins to an application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins), or all your applications</span></span>

-   <span data-ttu-id="8d14a-123">**A naplók** – lásd: [naplók kapcsolatos módosítások az alkalmazás részletes](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), vagy az alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8d14a-123">**Audit logs** – see [detailed audit logs about modifications to an application](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs), or to all your applications</span></span>

-   <span data-ttu-id="8d14a-124">**Feltételes és kockázati-alapú hozzáférés** – hatékony beállított [feltétel-alapú hozzáférési szabályok](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) , amely akkor lépnek érvénybe, amikor a felhasználók próbálnak bejelentkezni az adott alkalmazást</span><span class="sxs-lookup"><span data-stu-id="8d14a-124">**Conditional and risk-based access** – set powerful [condition-based access rules](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access) that are enforced when users attempt to sign in to a specific application</span></span>

-   <span data-ttu-id="8d14a-125">**Engedélyek megtekintése** – bármely megtekintése a [OAuth2 engedélyek](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) hozzáfér egy alkalmazás helyezendő ugyanahhoz a könyvtárban</span><span class="sxs-lookup"><span data-stu-id="8d14a-125">**Permissions view** – view any of the [OAuth2 permissions](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent) an application has access to in your directory from a single place</span></span>

## <a name="single-sign-on-and-provisioning-modes-supported-by-specific-application-types"></a><span data-ttu-id="8d14a-126">Egyszeri bejelentkezés és a létesítési módot támogat. az adott alkalmazást típusok</span><span class="sxs-lookup"><span data-stu-id="8d14a-126">Single sign-on and provisioning modes supported by specific application types</span></span>

<span data-ttu-id="8d14a-127">Az alábbi táblázat ismerteti a különböző egyszeri bejelentkezést és a fenti alkalmazástípusok által támogatott üzembe helyezési mód.</span><span class="sxs-lookup"><span data-stu-id="8d14a-127">The table below describes the different single sign-on and provisioning modes supported by each of the above application types.</span></span> <span data-ttu-id="8d14a-128">Ez a táblázat segítségével segítenek megérteni, melyik alkalmazás, akkor hozzon létre egy adott cél támogatásához.</span><span class="sxs-lookup"><span data-stu-id="8d14a-128">You can use this table to help you to understand which application you need to add to support a specific goal.</span></span>

  ![Alkalmazás típusú tábla](./media/application-tables/table1.png)

## <a name="how-to-choose-a-single-sign-on-mode"></a><span data-ttu-id="8d14a-130">Egyszeri bejelentkezés mód kiválasztása</span><span class="sxs-lookup"><span data-stu-id="8d14a-130">How to choose a single sign-on mode</span></span>

<span data-ttu-id="8d14a-131">A támogatott **egyszeri bejelentkezés** módjainak Azure AD-alkalmazások az alábbiak.</span><span class="sxs-lookup"><span data-stu-id="8d14a-131">The supported **single sign-on** modes for Azure AD applications are listed below.</span></span>

-   <span data-ttu-id="8d14a-132">**Az Azure AD az egyszeri bejelentkezés le van tiltva** – válassza ki az Azure AD az egyszeri bejelentkezés le van tiltva **egyszeri bejelentkezés mód** Ha még nem állnak készen áll az alkalmazás integrálható az egyszeri bejelentkezés az Azure ad-vel, vagy egyszerűen csak tesztelés kimenő</span><span class="sxs-lookup"><span data-stu-id="8d14a-132">**Azure AD single sign-on disabled** – choose Azure AD single sign-on disabled **single sign-on mode** if you are not yet ready to integrate this application with single sign-on with Azure AD, or are simply testing it out</span></span>

-   <span data-ttu-id="8d14a-133">**Bejelentkezés kapcsolódó** – válassza ki a [társított bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha valamelyik alkalmazásnál, amely a már meglévő egyszeri bejelentkezési megoldással már csatlakoztatva van, vagy ha szeretné a felhasználóknak egy egyszerű hivatkozás közzététele a [alkalmazás hozzáférési Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) vagy [Office 365 alkalmazásindító](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span><span class="sxs-lookup"><span data-stu-id="8d14a-133">**Linked Sign-on** – choose the [Linked Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if you have an application that is already connected with an existing single sign-on solution, or if you just want to publish a simple link for your users in their [Application Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) or [Office 365 application launcher](https://login.microsoftonline.com/common/oauth2/authorize?response_mode=form_post&response_type=id_token&scope=openid&nonce=d508a995-f6d6-4b8a-81b8-825c71f1be46.636253878097046923&state=https%3a%2f%2fsupport.office.com%2farticle%2fMeet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a%3fui%3den-US%26rs%3den-US%26ad%3dUS&client_id=4b233688-031c-404b-9a80-a4f3f2351f90&redirect_uri=https%3a%2f%2fsupport.office.com%2fauth%2fsignin&login_hint=asteen%40microsoft.com&prompt=none)</span></span>

-   <span data-ttu-id="8d14a-134">**Jelszó alapú bejelentkezés** – válassza ki a [jelszóalapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **egyszeri bejelentkezés mód** Ha az alkalmazás Renderelés egy HTML-felhasználónév és jelszó mezőt, és szeretné tárolni, hogy felhasználónevet és jelszót biztonságos helyen kell újból az alkalmazást később</span><span class="sxs-lookup"><span data-stu-id="8d14a-134">**Password-based Sign-on** – choose the [Password-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) **single sign-on mode** if your application renders an HTML username and password field and you want to store that username and password securely to be replayed to the application later</span></span>

-   <span data-ttu-id="8d14a-135">**SAML-alapú bejelentkezés** – válassza ki a [SAML-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) egyszeri bejelentkezést a módot, ha az alkalmazás támogatja az SAML- vagy OpenID Connect protokoll segítségével a felhasználók adott alkalmazás szerepköreihez a SAML-jogcímek meghatározott szabályok alapján tudja kívánt, vagy *</span><span class="sxs-lookup"><span data-stu-id="8d14a-135">**SAML-based Sign-on** – choose the [SAML-based Sign-on](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#how-does-single-sign-on-with-azure-active-directory-work) single-sign on mode if your application supports the SAML or OpenID Connect protocols, or you want to be able to map users to specific application roles based on rules you define in your SAML claims *</span></span>

   >[!NOTE]
   ><span data-ttu-id="8d14a-136">Ez a beállítás nem érhető el, ha a proxy van konfigurálva, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d14a-136">This option is not available when the application proxy is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="8d14a-137">**Fejléc-alapú bejelentkezés** – válassza ezt a [fejléc-alapú bejelentkezés](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) egyszeri bejelentkezés módja Ha valamely alkalmazás használatával, amely támogatja a HTTP-fejléc PingAccess alapján hitelesítés egyszeri bejelentkezést a végrehajtani kívánt</span><span class="sxs-lookup"><span data-stu-id="8d14a-137">**Header-based Sign-on** – choose this [Header-based Sign-on](https://docs.microsoft.com/azure/active-directory/application-proxy-ping-access#what-is-pingaccess-for-azure-ad) single sign-on mode if you have an application using PingAccess that supports HTTP-header based authentication that you wish to perform single-sign on to</span></span> 

   >[!NOTE]
   ><span data-ttu-id="8d14a-138">Ez a beállítás csak érhető el, ha a proxy- és PingAccess úgy van konfigurálva, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d14a-138">This option is only available when the application proxy and PingAccess is configured for an application.</span></span>
   >
   >

-   <span data-ttu-id="8d14a-139">**Integrált Windows-hitelesítés** – válassza ki a [integrált Windows-hitelesítés](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) egyszeri bejelentkezést a elvégezni kívánt helyszíni WIA alkalmazás kitettségének üzemmódot egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8d14a-139">**Integrated Windows Authentication** – choose the [Integrated Windows Authentication](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-sso-using-kcd) single-sign on mode when exposing an on-premises WIA application that you wish to perform single-sign on to</span></span> 

   >[!NOTE]
   ><span data-ttu-id="8d14a-140">Ez a beállítás csak érhető el, ha a proxy van konfigurálva, az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8d14a-140">This option is only available when the application proxy is configured for an application.</span></span>
   >
   >

## <a name="single-sign-on-modes-for-custom-developed-applications"></a><span data-ttu-id="8d14a-141">Egyszeri bejelentkezés módjainak egyéni által fejlesztett alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8d14a-141">Single sign-on modes for custom-developed applications</span></span>

<span data-ttu-id="8d14a-142">Egyéni használatával fejlesztett alkalmazások rendelkeznek a [egyéni által fejlesztett alkalmazás](#_Custom-Developed_Applications) élmény is további egyszeri bejelentkezés módot támogat fent nem említett.</span><span class="sxs-lookup"><span data-stu-id="8d14a-142">Applications you have custom developed through the [Custom-developed application](#_Custom-Developed_Applications) experience also support additional single sign-on modes not listed above.</span></span> <span data-ttu-id="8d14a-143">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="8d14a-143">These include:</span></span>

-   <span data-ttu-id="8d14a-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="8d14a-144">[OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) based sign-on</span></span>

-   <span data-ttu-id="8d14a-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="8d14a-145">[OpenID Connect 1.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) based sign-on</span></span>

-   <span data-ttu-id="8d14a-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="8d14a-146">[WS-Federation 1.2](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html) based sign-on</span></span>

-   <span data-ttu-id="8d14a-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) bejelentkezés alapján</span><span class="sxs-lookup"><span data-stu-id="8d14a-147">[SAML 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-protocol-reference) based sign-on</span></span>

<span data-ttu-id="8d14a-148">Olvassa el a [Azure Active Directory fejlesztői útmutatója](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) további egyéni által fejlesztett alkalmazás, amely támogatja az egyszeri bejelentkezés módokban létrehozásával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8d14a-148">Read the [Azure Active Directory developer’s guide](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide) to learn more about how to create a custom-developed application which supports these single sign-on modes.</span></span>

## <a name="how-to-set-an-applications-single-sign-on-mode"></a><span data-ttu-id="8d14a-149">Egy alkalmazás egyszeri bejelentkezés mód beállítása</span><span class="sxs-lookup"><span data-stu-id="8d14a-149">How to set an application’s single sign-on mode</span></span>

<span data-ttu-id="8d14a-150">Az alkalmazás beállítására **egyszeri bejelentkezés** mód, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="8d14a-150">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="8d14a-151">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="8d14a-151">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="8d14a-152">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="8d14a-152">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="8d14a-153">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="8d14a-153">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="8d14a-154">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="8d14a-154">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="8d14a-155">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="8d14a-155">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="8d14a-156">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="8d14a-156">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="8d14a-157">Válassza ki az alkalmazást, amelyekhez az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8d14a-157">Select the application for which you want to configure single sign-on.</span></span>

7.  <span data-ttu-id="8d14a-158">Ha az alkalmazás betölt, kattintson **egyszeri bejelentkezés** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="8d14a-158">Once the application loads, click **Single sign-on** from the application’s left hand navigation menu.</span></span>

## <a name="how-to-choose-a-provisioning-mode"></a><span data-ttu-id="8d14a-159">A létesítés kiválasztása</span><span class="sxs-lookup"><span data-stu-id="8d14a-159">How to choose a provisioning mode</span></span>

-   <span data-ttu-id="8d14a-160">**Manuális létesítési** – válassza ki a [manuális](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) kiépítési üzemmódban, ha van meglévő fiókokat, vagy szeretné ezt az Azure AD-en kívüli alkalmazáshoz tartozó fiókok kezelése.</span><span class="sxs-lookup"><span data-stu-id="8d14a-160">**Manual Provisioning** – choose the [Manual](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#provisioning-modes) provisioning mode if you have existing accounts, or wish to manage accounts for this application outside of Azure AD.</span></span>

-   <span data-ttu-id="8d14a-161">**Automatikus kiépítés** – válassza ki a [automatikus](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **létesítés** Ha később engedélyezni kívánja automatikus API-alapú üzembe helyezés és/vagy az alkalmazás felhasználói fiókokat megszüntetést</span><span class="sxs-lookup"><span data-stu-id="8d14a-161">**Automatic Provisioning** – choose the [Automatic](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-manage-provisioning#configuring-automatic-user-account-provisioning) **provisioning mode** if you want to enable automatic API-based provisioning and/or de-provisioning of user accounts to this application</span></span> 

   >[!NOTE]
   ><span data-ttu-id="8d14a-162">Ez a beállítás csak a alkalmazások belül érhető el a **kiemelt** kategóriáját a [az Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span><span class="sxs-lookup"><span data-stu-id="8d14a-162">This option is available only for applications within the **featured** category of the [Azure AD Application Gallery](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-apps-whats-new-azure-portal#the-new-and-improved-application-gallery).</span></span>
   >
   >

-   <span data-ttu-id="8d14a-163">**SCIM-alapú automatikus üzembe helyezés** – használható [SCIM-alapú automatikus üzembe helyezés](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) Ha az alkalmazás támogatja-e a SCIM protokoll, az Azure ad integrált felhasználókat és csoportokat, amelyek bármely alkalmazás módosítások automatikusan kibocsátott módosításai észlelése</span><span class="sxs-lookup"><span data-stu-id="8d14a-163">**SCIM-based Automatic Provisioning** – use [SCIM-based Automatic Provisioning](https://docs.microsoft.com/azure/active-directory/active-directory-scim-provisioning) if your application supports the SCIM protocol for detecting changes to users and groups, which are automatically emitted for changes to any application integrated with Azure AD</span></span> 

   >[!NOTE]
   ><span data-ttu-id="8d14a-164">Ez a beállítás nem szerepel egy üzembe helyezési mód, de minden olyan alkalmazásnál, az Azure ad-vel integrált alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="8d14a-164">This option is not listed as a specific provisioning mode, but is enabled by default for all applications that are integrated with Azure AD.</span></span>
   >
   >

## <a name="how-to-set-an-applications-provisioning-mode"></a><span data-ttu-id="8d14a-165">Az alkalmazás beállítása a kiépítési üzemmódban</span><span class="sxs-lookup"><span data-stu-id="8d14a-165">How to set an application’s provisioning mode</span></span>

<span data-ttu-id="8d14a-166">Az alkalmazás beállítására **kiépítés** mód, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="8d14a-166">To set an application’s **provisioning** mode, follow the instructions below:</span></span>

<span data-ttu-id="8d14a-167">Az alkalmazás beállítására **egyszeri bejelentkezés** mód, kövesse az alábbi utasításokat:</span><span class="sxs-lookup"><span data-stu-id="8d14a-167">To set an application’s **single sign-on** mode, follow the instructions below:</span></span>

1.  <span data-ttu-id="8d14a-168">Nyissa meg a [ **Azure Portal** ](https://portal.azure.com/) , és jelentkezzen be egy **globális rendszergazda** vagy **Co-rendszergazda segítségét.**</span><span class="sxs-lookup"><span data-stu-id="8d14a-168">Open the [**Azure Portal**](https://portal.azure.com/) and sign in as a **Global Administrator** or **Co-admin.**</span></span>

2.  <span data-ttu-id="8d14a-169">Nyissa meg a **Azure Active Directory-bővítmény** kattintva **további szolgáltatások** a fő bal oldali navigációs menü alján.</span><span class="sxs-lookup"><span data-stu-id="8d14a-169">Open the **Azure Active Directory Extension** by clicking **More services** at the bottom of the main left hand navigation menu.</span></span>

3.  <span data-ttu-id="8d14a-170">Írja be a **"Azure Active Directory**" a szűrő keresési mezőbe, és válasszon a **Azure Active Directory** elemet.</span><span class="sxs-lookup"><span data-stu-id="8d14a-170">Type in **“Azure Active Directory**” in the filter search box and select the **Azure Active Directory** item.</span></span>

4.  <span data-ttu-id="8d14a-171">Kattintson a **vállalati alkalmazások** az Azure Active Directory bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="8d14a-171">click **Enterprise Applications** from the Azure Active Directory left hand navigation menu.</span></span>

5.  <span data-ttu-id="8d14a-172">Kattintson a **összes alkalmazás** az alkalmazások listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="8d14a-172">click **All Applications** to view a list of all your applications.</span></span>

  * <span data-ttu-id="8d14a-173">Ha azt szeretné, hogy itt megjelennek az alkalmazás nem látja, használja a **szűrő** vezérlő tetején a **összes alkalmazások listáját** és állítsa be a **megjelenítése** lehetőséggel **összes alkalmazást.**</span><span class="sxs-lookup"><span data-stu-id="8d14a-173">If you do not see the application you want show up here, use the **Filter** control at the top of the **All Applications List** and set the **Show** option to **All Applications.**</span></span>

6.  <span data-ttu-id="8d14a-174">Válassza ki az alkalmazást, amelynek konfigurálása kiosztás keresi.</span><span class="sxs-lookup"><span data-stu-id="8d14a-174">Select the application for which you want to configure provisioning.</span></span>

7.  <span data-ttu-id="8d14a-175">Ha az alkalmazás betölt, kattintson **kiépítési** az alkalmazás bal oldali navigációs menüjében.</span><span class="sxs-lookup"><span data-stu-id="8d14a-175">Once the application loads, click **Provisioning** from the application’s left hand navigation menu.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d14a-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8d14a-176">Next steps</span></span>
[<span data-ttu-id="8d14a-177">Alkalmazások kezelése az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="8d14a-177">Managing Applications with Azure Active Directory</span></span>](active-directory-enable-sso-scenario.md)

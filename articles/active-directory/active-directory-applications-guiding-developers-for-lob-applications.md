---
title: "Az Azure AD alkalmazások fejlesztéséhez |} Microsoft Docs"
description: "Az informatikai szakembereknek készült Ez a cikk útmutatást nyújt a Azure-alkalmazások integrálása az Active Directoryval."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6b119be9c06d8c1ccc8e747168429e6c2d2e7a8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a><span data-ttu-id="6e62a-103">Az Azure Active Directory-üzleti alkalmazások fejlesztéséhez</span><span class="sxs-lookup"><span data-stu-id="6e62a-103">Develop line-of-business apps for Azure Active Directory</span></span>
<span data-ttu-id="6e62a-104">Ez az útmutató áttekintést fejlesztése az üzletági (LoB) alkalmazások az Azure Active Directory (AD). A célközönség Active Directory vagy Office 365 globális rendszergazdák.</span><span class="sxs-lookup"><span data-stu-id="6e62a-104">This guide provides an overview of developing line-of-business (LoB) applications for Azure Active Directory (AD).The intended audience is Active Directory/Office 365 global administrators.</span></span>

## <a name="overview"></a><span data-ttu-id="6e62a-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="6e62a-105">Overview</span></span>
<span data-ttu-id="6e62a-106">Az Azure AD integrált alkalmazások lehetőséget ad a felhasználók a szervezet egyszeri bejelentkezéshez és az Office 365 a.</span><span class="sxs-lookup"><span data-stu-id="6e62a-106">Building applications integrated with Azure AD gives users in your organization single sign-on with Office 365.</span></span> <span data-ttu-id="6e62a-107">Mivel az alkalmazás a hitelesítési házirend alkalmazására vonatkozó vezérlésére biztosít az Azure AD lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6e62a-107">Having the application in Azure AD gives you control over the authentication policy for the application.</span></span> <span data-ttu-id="6e62a-108">További információt a feltételes hozzáférés és a többtényezős hitelesítés (MFA) tekintse meg az alkalmazások védelme [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="6e62a-108">To learn more about conditional access and how to protect apps with multi-factor authentication (MFA) see [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

<span data-ttu-id="6e62a-109">Regisztrálja az alkalmazást az Azure Active Directoryt.</span><span class="sxs-lookup"><span data-stu-id="6e62a-109">Register your application to use Azure Active Directory.</span></span> <span data-ttu-id="6e62a-110">Az alkalmazás regisztrálása, az azt jelenti, hogy a fejlesztők használhatják-e az Azure AD hitelesíti a felhasználókat, és kérjen hozzáférést a felhasználói erőforrások, például az e-mailek, naptár és dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="6e62a-110">Registering the application means that your developers can use Azure AD to authenticate users and request access to user resources such as email, calendar, and documents.</span></span>

<span data-ttu-id="6e62a-111">Bármelyik tag a könyvtár (nem vendégek) regisztrálhatja az alkalmazás, más néven a *alkalmazásobjektum létrehozása*.</span><span class="sxs-lookup"><span data-stu-id="6e62a-111">Any member of your directory (not guests) can register an application, otherwise known as *creating an application object*.</span></span>

<span data-ttu-id="6e62a-112">Az alkalmazásnak lehetővé teszi, hogy a felhasználók úgy tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="6e62a-112">Registering an application allows any user to do the following:</span></span>

* <span data-ttu-id="6e62a-113">Az alkalmazás számára az Azure AD felismerhető identitás lekérése</span><span class="sxs-lookup"><span data-stu-id="6e62a-113">Get an identity for their application that Azure AD recognizes</span></span>
* <span data-ttu-id="6e62a-114">Egy vagy több titkok/kulcsot, az alkalmazás segítségével hitelesíti magát az AD beolvasása</span><span class="sxs-lookup"><span data-stu-id="6e62a-114">Get one or more secrets/keys that the application can use to authenticate itself to AD</span></span>
* <span data-ttu-id="6e62a-115">Az alkalmazás az Azure portálon egy egyéni nevét, emblémáját, stb. arculatát.</span><span class="sxs-lookup"><span data-stu-id="6e62a-115">Brand the application in the Azure portal with a custom name, logo, etc.</span></span>
* <span data-ttu-id="6e62a-116">Az Azure AD hitelesítési szolgáltatások alkalmazni az alkalmazásra, többek között:</span><span class="sxs-lookup"><span data-stu-id="6e62a-116">Apply Azure AD authorization features to their app, including:</span></span>

  * <span data-ttu-id="6e62a-117">Szerepköralapú hozzáférés-vezérlés (RBAC)</span><span class="sxs-lookup"><span data-stu-id="6e62a-117">Role-Based Access Control (RBAC)</span></span>
  * <span data-ttu-id="6e62a-118">Az Azure Active Directory oAuth-engedélyezési kiszolgálóként (az API-k által az alkalmazás biztonságos)</span><span class="sxs-lookup"><span data-stu-id="6e62a-118">Azure Active Directory as oAuth authorization server (secure an API exposed by the application)</span></span>
* <span data-ttu-id="6e62a-119">Deklarálja az alkalmazás működéséhez szükséges szükséges engedélyekkel, vártnak, beleértve:</span><span class="sxs-lookup"><span data-stu-id="6e62a-119">Declare required permissions necessary for the application to function as expected, including:</span></span>

      - <span data-ttu-id="6e62a-120">Alkalmazásengedélyek (csak a globális rendszergazdák).</span><span class="sxs-lookup"><span data-stu-id="6e62a-120">App permissions (global administrators only).</span></span> <span data-ttu-id="6e62a-121">Példa: egy másik Azure AD alkalmazás vagy szerepkör tagsági képest az Azure erőforrás, erőforráscsoport, vagy előfizetés szerepkör tagjának kell lennie</span><span class="sxs-lookup"><span data-stu-id="6e62a-121">For example: Role membership in another Azure AD application or role membership relative to an Azure Resource, Resource Group, or Subscription</span></span>
      - <span data-ttu-id="6e62a-122">Delegált engedélyek (felhasználói).</span><span class="sxs-lookup"><span data-stu-id="6e62a-122">Delegated permissions (any user).</span></span> <span data-ttu-id="6e62a-123">Például: az Azure AD-bejelentkezés, és olvasási profil</span><span class="sxs-lookup"><span data-stu-id="6e62a-123">For example: Azure AD, Sign-in, and Read Profile</span></span>

> [!NOTE]
> <span data-ttu-id="6e62a-124">Alapértelmezés szerint minden tag kérelmet tud regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="6e62a-124">By default, any member can register an application.</span></span> <span data-ttu-id="6e62a-125">A regisztrációhoz meghatározott tagjainak alkalmazások engedélyek korlátozása, lásd: [hogyan alkalmazások felvétele az Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span><span class="sxs-lookup"><span data-stu-id="6e62a-125">To learn how to restrict permissions for registering applications to specific members, see [How applications are added to Azure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span></span>
>
>

<span data-ttu-id="6e62a-126">Mi a globális rendszergazdának kell tennie segítségével a fejlesztők legyen az alkalmazás készen áll a termelési itt található:</span><span class="sxs-lookup"><span data-stu-id="6e62a-126">Here’s what you, the global administrator, need to do to help developers make their application ready for production:</span></span>

* <span data-ttu-id="6e62a-127">Hozzáférési szabályok (hozzáférési házirend vagy MFA) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e62a-127">Configure access rules (access policy/MFA)</span></span>
* <span data-ttu-id="6e62a-128">Az alkalmazás felhasználói kiosztása és felhasználók hozzárendelése konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e62a-128">Configure the app to require user assignment and assign users</span></span>
* <span data-ttu-id="6e62a-129">Ne jelenjen meg többé az alapértelmezett felhasználói hozzájárulás élmény</span><span class="sxs-lookup"><span data-stu-id="6e62a-129">Suppress the default user consent experience</span></span>

## <a name="configure-access-rules"></a><span data-ttu-id="6e62a-130">Hozzáférési szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e62a-130">Configure access rules</span></span>
<span data-ttu-id="6e62a-131">A Szolgáltatottszoftver-alkalmazásoknál alkalmazás hozzáférési szabályok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6e62a-131">Configure per-application access rules to your SaaS apps.</span></span> <span data-ttu-id="6e62a-132">Például többtényezős hitelesítés megkövetelése, vagy csak felhasználók férhessenek hozzá a megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="6e62a-132">For example, you can require MFA or only allow access to users on trusted networks.</span></span> <span data-ttu-id="6e62a-133">Ez a részletes érhetők el a dokumentum [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="6e62a-133">The details for this are available in the document [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a><span data-ttu-id="6e62a-134">Az alkalmazás felhasználói kiosztása és felhasználók hozzárendelése konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6e62a-134">Configure the app to require user assignment and assign users</span></span>
<span data-ttu-id="6e62a-135">Alapértelmezés szerint felhasználók folyik a hozzárendelése nem férhet hozzá az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="6e62a-135">By default, users can access applications without being assigned.</span></span> <span data-ttu-id="6e62a-136">Ha az alkalmazás elérhetővé teszi a szerepkörök, vagy ha azt szeretné, hogy az alkalmazás a felhasználó hozzáférési panel megjeleníteni, felhasználó-hozzárendelés elvégzéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="6e62a-136">However, if the application exposes roles or if you want the application to appear on a user’s access panel, you should require user assignment.</span></span>

[<span data-ttu-id="6e62a-137">Felhasználó-hozzárendelés igénylése</span><span class="sxs-lookup"><span data-stu-id="6e62a-137">Requiring user assignment</span></span>](active-directory-applications-guiding-developers-requiring-user-assignment.md)

<span data-ttu-id="6e62a-138">Ha az Azure AD Premium vagy a nagyvállalati mobilitási csomag (EMS) előfizető, erősen ajánlott csoportok használatával.</span><span class="sxs-lookup"><span data-stu-id="6e62a-138">If you’re an Azure AD Premium or Enterprise Mobility Suite (EMS) subscriber, we strongly recommend using groups.</span></span> <span data-ttu-id="6e62a-139">Csoportok hozzárendelése az alkalmazás lehetővé teszi a csoport folyamatos hozzáférés-kezelés a tulajdonosra delegálására.</span><span class="sxs-lookup"><span data-stu-id="6e62a-139">Assigning groups to the application allows you to delegate ongoing access management to the owner of the group.</span></span> <span data-ttu-id="6e62a-140">A csoport létrehozásához, vagy kérje meg a felelős fél létrehozni a csoportot, a csoport felügyeleti funkció segítségével a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="6e62a-140">You can create the group or ask the responsible party in your organization to create the group using your group management facility.</span></span>

[<span data-ttu-id="6e62a-141">Felhasználók hozzárendelése egy alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="6e62a-141">Assigning users to an application</span></span>](active-directory-applications-guiding-developers-assigning-users.md)  
[<span data-ttu-id="6e62a-142">Csoportok hozzárendelése egy alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="6e62a-142">Assigning groups to an application</span></span>](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a><span data-ttu-id="6e62a-143">Felhasználói hozzájárulás mellőzése</span><span class="sxs-lookup"><span data-stu-id="6e62a-143">Suppress user consent</span></span>
<span data-ttu-id="6e62a-144">Alapértelmezés szerint minden felhasználó végighalad a hozzájárulási élmény való bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="6e62a-144">By default, each user goes through a consent experience to sign in.</span></span> <span data-ttu-id="6e62a-145">Lehet, hogy a hozzájárulási élmény kérni a felhasználókat adhat engedélyeket egy alkalmazást, disconcerting felhasználók számára nem ismeri az ilyen döntések meghozatalában.</span><span class="sxs-lookup"><span data-stu-id="6e62a-145">The consent experience, asking users to grant permissions to an application, can be disconcerting for users who are unfamiliar with making such decisions.</span></span>

<span data-ttu-id="6e62a-146">A megbízható alkalmazások egyszerűbbé teszik a felhasználói élmény hozzájárul ahhoz, hogy a szervezet nevében az alkalmazás által.</span><span class="sxs-lookup"><span data-stu-id="6e62a-146">For applications that you trust, you can simplify the user experience by consenting to the application on behalf of your organization.</span></span>

<span data-ttu-id="6e62a-147">További információ a felhasználói hozzájárulás és a hozzájárulásukat adják észlel, az Azure-ban, a következő témakörben: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="6e62a-147">For more information about user consent and the consent experience in Azure, see [Integrating Applications with Azure Active Directory](active-directory-integrating-applications.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="6e62a-148">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="6e62a-148">Related Articles</span></span>
* [<span data-ttu-id="6e62a-149">Az Azure AD alkalmazásproxy a helyszíni alkalmazások biztonságos távoli hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6e62a-149">Enable secure remote access to on-premises applications with Azure AD Application Proxy</span></span>](active-directory-application-proxy-get-started.md)
* [<span data-ttu-id="6e62a-150">Azure feltételes hozzáférés előzetes verziója SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="6e62a-150">Azure Conditional Access Preview for SaaS Apps</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="6e62a-151">Az Azure ad-val alkalmazásokhoz való hozzáférés kezelése</span><span class="sxs-lookup"><span data-stu-id="6e62a-151">Managing access to apps with Azure AD</span></span>](active-directory-managing-access-to-apps.md)
* [<span data-ttu-id="6e62a-152">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="6e62a-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

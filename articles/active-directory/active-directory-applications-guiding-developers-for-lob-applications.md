---
title: "az Azure AD-alkalmazások aaaDevelop |} Microsoft Docs"
description: "Az informatikai szakembereknek hello verziójához, ez a cikk útmutatást nyújt a Azure-alkalmazások integrálása az Active Directoryval."
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
ms.openlocfilehash: d2924be752af0be2843b1d9b74d9ec446d3fe1ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a><span data-ttu-id="017f0-103">Az Azure Active Directory-üzleti alkalmazások fejlesztéséhez</span><span class="sxs-lookup"><span data-stu-id="017f0-103">Develop line-of-business apps for Azure Active Directory</span></span>
<span data-ttu-id="017f0-104">Ez az útmutató áttekintést az Azure Active Directory (AD) .hello-üzletági (LoB) alkalmazások fejlesztéséhez szánt célközönségét az Active Directory vagy Office 365 globális rendszergazdák.</span><span class="sxs-lookup"><span data-stu-id="017f0-104">This guide provides an overview of developing line-of-business (LoB) applications for Azure Active Directory (AD).hello intended audience is Active Directory/Office 365 global administrators.</span></span>

## <a name="overview"></a><span data-ttu-id="017f0-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="017f0-105">Overview</span></span>
<span data-ttu-id="017f0-106">Az Azure AD integrált alkalmazások lehetőséget ad a felhasználók a szervezet egyszeri bejelentkezéshez és az Office 365 a.</span><span class="sxs-lookup"><span data-stu-id="017f0-106">Building applications integrated with Azure AD gives users in your organization single sign-on with Office 365.</span></span> <span data-ttu-id="017f0-107">Mivel hello alkalmazás hello hitelesítési házirend hello alkalmazás vezérlésére biztosít az Azure AD lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="017f0-107">Having hello application in Azure AD gives you control over hello authentication policy for hello application.</span></span> <span data-ttu-id="017f0-108">További információk a feltételes hozzáférés, és hogyan tooprotect alkalmazások és a többtényezős hitelesítés (MFA): toolearn [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="017f0-108">toolearn more about conditional access and how tooprotect apps with multi-factor authentication (MFA) see [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

<span data-ttu-id="017f0-109">Az alkalmazás toouse Azure Active Directoryban regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="017f0-109">Register your application toouse Azure Active Directory.</span></span> <span data-ttu-id="017f0-110">Hello alkalmazás regisztrálása azt jelenti, hogy a fejlesztők az Azure AD tooauthenticate felhasználóinak használja-e, és kérjen hozzáférési toouser erőforrásokat, például az e-mailek, naptár és dokumentumok.</span><span class="sxs-lookup"><span data-stu-id="017f0-110">Registering hello application means that your developers can use Azure AD tooauthenticate users and request access toouser resources such as email, calendar, and documents.</span></span>

<span data-ttu-id="017f0-111">Bármelyik tag a könyvtár (nem vendégek) regisztrálhatja az alkalmazás, más néven a *alkalmazásobjektum létrehozása*.</span><span class="sxs-lookup"><span data-stu-id="017f0-111">Any member of your directory (not guests) can register an application, otherwise known as *creating an application object*.</span></span>

<span data-ttu-id="017f0-112">Az alkalmazásnak lehetővé teszi, hogy bármely felhasználó toodo hello következő:</span><span class="sxs-lookup"><span data-stu-id="017f0-112">Registering an application allows any user toodo hello following:</span></span>

* <span data-ttu-id="017f0-113">Az alkalmazás számára az Azure AD felismerhető identitás lekérése</span><span class="sxs-lookup"><span data-stu-id="017f0-113">Get an identity for their application that Azure AD recognizes</span></span>
* <span data-ttu-id="017f0-114">Egy vagy több titkok/kulcsok alkalmazás hello is használja a maga tooauthenticate tooAD</span><span class="sxs-lookup"><span data-stu-id="017f0-114">Get one or more secrets/keys that hello application can use tooauthenticate itself tooAD</span></span>
* <span data-ttu-id="017f0-115">Hello alkalmazás márka hello Azure portál, amely egy egyéni nevet, embléma, stb.</span><span class="sxs-lookup"><span data-stu-id="017f0-115">Brand hello application in hello Azure portal with a custom name, logo, etc.</span></span>
* <span data-ttu-id="017f0-116">Alkalmazza az Azure AD hitelesítési szolgáltatások tootheir alkalmazás, beleértve:</span><span class="sxs-lookup"><span data-stu-id="017f0-116">Apply Azure AD authorization features tootheir app, including:</span></span>

  * <span data-ttu-id="017f0-117">Szerepköralapú hozzáférés-vezérlés (RBAC)</span><span class="sxs-lookup"><span data-stu-id="017f0-117">Role-Based Access Control (RBAC)</span></span>
  * <span data-ttu-id="017f0-118">Az Azure Active Directory oAuth-engedélyezési kiszolgálóként (biztonságos hello alkalmazás által az API-k)</span><span class="sxs-lookup"><span data-stu-id="017f0-118">Azure Active Directory as oAuth authorization server (secure an API exposed by hello application)</span></span>
* <span data-ttu-id="017f0-119">Szükséges engedélyekkel, beleértve a várt módon hello alkalmazás toofunction szükséges deklarálható:</span><span class="sxs-lookup"><span data-stu-id="017f0-119">Declare required permissions necessary for hello application toofunction as expected, including:</span></span>

      - <span data-ttu-id="017f0-120">Alkalmazásengedélyek (csak a globális rendszergazdák).</span><span class="sxs-lookup"><span data-stu-id="017f0-120">App permissions (global administrators only).</span></span> <span data-ttu-id="017f0-121">Példa: egy másik Azure AD alkalmazás vagy szerepkör tagsági relatív tooan Azure Resource, erőforráscsoport, szerepkör tagjának kell lennie, vagy az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="017f0-121">For example: Role membership in another Azure AD application or role membership relative tooan Azure Resource, Resource Group, or Subscription</span></span>
      - <span data-ttu-id="017f0-122">Delegált engedélyek (felhasználói).</span><span class="sxs-lookup"><span data-stu-id="017f0-122">Delegated permissions (any user).</span></span> <span data-ttu-id="017f0-123">Például: az Azure AD-bejelentkezés, és olvasási profil</span><span class="sxs-lookup"><span data-stu-id="017f0-123">For example: Azure AD, Sign-in, and Read Profile</span></span>

> [!NOTE]
> <span data-ttu-id="017f0-124">Alapértelmezés szerint minden tag kérelmet tud regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="017f0-124">By default, any member can register an application.</span></span> <span data-ttu-id="017f0-125">Hogyan toorestrict engedélyek alkalmazások toospecific tagok, a regisztrációhoz: toolearn [hogyan alkalmazások felvétele tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span><span class="sxs-lookup"><span data-stu-id="017f0-125">toolearn how toorestrict permissions for registering applications toospecific members, see [How applications are added tooAzure AD](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).</span></span>
>
>

<span data-ttu-id="017f0-126">Ez milyen Ön hello globális rendszergazda, kell toodo toohelp fejlesztők ellenőrizze az alkalmazás készen áll a termelési:</span><span class="sxs-lookup"><span data-stu-id="017f0-126">Here’s what you, hello global administrator, need toodo toohelp developers make their application ready for production:</span></span>

* <span data-ttu-id="017f0-127">Hozzáférési szabályok (hozzáférési házirend vagy MFA) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="017f0-127">Configure access rules (access policy/MFA)</span></span>
* <span data-ttu-id="017f0-128">Hello app toorequire felhasználói hozzárendelésének konfigurálása és a felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="017f0-128">Configure hello app toorequire user assignment and assign users</span></span>
* <span data-ttu-id="017f0-129">Ne jelenjen meg többé hello alapértelmezett felhasználói hozzájárulás élmény</span><span class="sxs-lookup"><span data-stu-id="017f0-129">Suppress hello default user consent experience</span></span>

## <a name="configure-access-rules"></a><span data-ttu-id="017f0-130">Hozzáférési szabályok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="017f0-130">Configure access rules</span></span>
<span data-ttu-id="017f0-131">Alkalmazásonkénti hozzáférési szabályok tooyour SaaS-alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="017f0-131">Configure per-application access rules tooyour SaaS apps.</span></span> <span data-ttu-id="017f0-132">Például többtényezős hitelesítés megkövetelése, vagy a hozzáférés toousers engedélyezése csak a megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="017f0-132">For example, you can require MFA or only allow access toousers on trusted networks.</span></span> <span data-ttu-id="017f0-133">Ez hello részletei érhetők el hello dokumentum [konfigurálása hozzáférési szabályok](active-directory-conditional-access-azuread-connected-apps.md).</span><span class="sxs-lookup"><span data-stu-id="017f0-133">hello details for this are available in hello document [Configuring access rules](active-directory-conditional-access-azuread-connected-apps.md).</span></span>

## <a name="configure-hello-app-toorequire-user-assignment-and-assign-users"></a><span data-ttu-id="017f0-134">Hello app toorequire felhasználói hozzárendelésének konfigurálása és a felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="017f0-134">Configure hello app toorequire user assignment and assign users</span></span>
<span data-ttu-id="017f0-135">Alapértelmezés szerint felhasználók folyik a hozzárendelése nem férhet hozzá az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="017f0-135">By default, users can access applications without being assigned.</span></span> <span data-ttu-id="017f0-136">Ha hello alkalmazás közzététele a szerepköröket, vagy ha azt szeretné, hogy a felhasználó hozzáférési panel hello alkalmazás tooappear, felhasználó-hozzárendelés elvégzéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="017f0-136">However, if hello application exposes roles or if you want hello application tooappear on a user’s access panel, you should require user assignment.</span></span>

[<span data-ttu-id="017f0-137">Felhasználó-hozzárendelés igénylése</span><span class="sxs-lookup"><span data-stu-id="017f0-137">Requiring user assignment</span></span>](active-directory-applications-guiding-developers-requiring-user-assignment.md)

<span data-ttu-id="017f0-138">Ha az Azure AD Premium vagy a nagyvállalati mobilitási csomag (EMS) előfizető, erősen ajánlott csoportok használatával.</span><span class="sxs-lookup"><span data-stu-id="017f0-138">If you’re an Azure AD Premium or Enterprise Mobility Suite (EMS) subscriber, we strongly recommend using groups.</span></span> <span data-ttu-id="017f0-139">Csoportok toohello alkalmazás hozzárendelése lehetővé teszi toodelegate folyamatban lévő access management toohello hello csoport tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="017f0-139">Assigning groups toohello application allows you toodelegate ongoing access management toohello owner of hello group.</span></span> <span data-ttu-id="017f0-140">Hello csoport létrehozása, vagy kérje meg a hello felelős fél a szervezet toocreate hello csoportjában a csoport felügyeleti funkció segítségével.</span><span class="sxs-lookup"><span data-stu-id="017f0-140">You can create hello group or ask hello responsible party in your organization toocreate hello group using your group management facility.</span></span>

[<span data-ttu-id="017f0-141">Felhasználók hozzárendelése tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="017f0-141">Assigning users tooan application</span></span>](active-directory-applications-guiding-developers-assigning-users.md)  
[<span data-ttu-id="017f0-142">Csoportok tooan alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="017f0-142">Assigning groups tooan application</span></span>](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a><span data-ttu-id="017f0-143">Felhasználói hozzájárulás mellőzése</span><span class="sxs-lookup"><span data-stu-id="017f0-143">Suppress user consent</span></span>
<span data-ttu-id="017f0-144">Alapértelmezés szerint minden felhasználó végighalad a hozzájárulási élmény toosign a.</span><span class="sxs-lookup"><span data-stu-id="017f0-144">By default, each user goes through a consent experience toosign in.</span></span> <span data-ttu-id="017f0-145">hello hozzájárulási, kérni a felhasználó toogrant engedélyeivel tooan alkalmazás lehet disconcerting felhasználók számára nem ismeri az ilyen döntések meghozatalában.</span><span class="sxs-lookup"><span data-stu-id="017f0-145">hello consent experience, asking users toogrant permissions tooan application, can be disconcerting for users who are unfamiliar with making such decisions.</span></span>

<span data-ttu-id="017f0-146">A megbízható alkalmazások egyszerűbbé teheti az hello felhasználói élmény küldőnek toohello alkalmazás által a szervezet nevében.</span><span class="sxs-lookup"><span data-stu-id="017f0-146">For applications that you trust, you can simplify hello user experience by consenting toohello application on behalf of your organization.</span></span>

<span data-ttu-id="017f0-147">További információ a felhasználói hozzájárulás és hello hozzájárulási tapasztalhat az Azure-ban, lásd: [alkalmazások integrálása az Azure Active Directoryval](active-directory-integrating-applications.md).</span><span class="sxs-lookup"><span data-stu-id="017f0-147">For more information about user consent and hello consent experience in Azure, see [Integrating Applications with Azure Active Directory](active-directory-integrating-applications.md).</span></span>

## <a name="related-articles"></a><span data-ttu-id="017f0-148">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="017f0-148">Related Articles</span></span>
* [<span data-ttu-id="017f0-149">Biztonságos távoli hozzáférés tooon helyszíni alkalmazások az Azure AD alkalmazásproxy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="017f0-149">Enable secure remote access tooon-premises applications with Azure AD Application Proxy</span></span>](active-directory-application-proxy-get-started.md)
* [<span data-ttu-id="017f0-150">Azure feltételes hozzáférés előzetes verziója SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="017f0-150">Azure Conditional Access Preview for SaaS Apps</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="017f0-151">Az Azure AD hozzáférési tooapps kezelése</span><span class="sxs-lookup"><span data-stu-id="017f0-151">Managing access tooapps with Azure AD</span></span>](active-directory-managing-access-to-apps.md)
* [<span data-ttu-id="017f0-152">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="017f0-152">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)

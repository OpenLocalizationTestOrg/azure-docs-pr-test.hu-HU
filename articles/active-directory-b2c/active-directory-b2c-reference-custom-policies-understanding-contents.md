---
title: "Az Azure Active Directory B2C: Az alapszintű csomag egyéni házirendjeinek ismertetése |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 9847bcfcc139a769847678c1cca6a8b9c3a30e93
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understanding-the-custom-policies-of-the-azure-ad-b2c-custom-policy-starter-pack"></a><span data-ttu-id="33fe6-103">Az Azure AD B2C egyéni házirend alapszintű csomag egyéni házirendjeinek ismertetése</span><span class="sxs-lookup"><span data-stu-id="33fe6-103">Understanding the custom policies of the Azure AD B2C Custom Policy starter pack</span></span>

<span data-ttu-id="33fe6-104">Ez a rész felsorolja a B2C_1A_base szabályzat részeként elérhető összes core elem a **alapszintű csomag** és, hogy megfelelő saját házirendeket a örökléssel tartalomkészítéshez elkészítéséhez használja a *B2C_1A_base_extensions házirend* .</span><span class="sxs-lookup"><span data-stu-id="33fe6-104">This section lists all the core elements of the B2C_1A_base policy that comes with the **Starter Pack** and that is leveraged for authoring your own policies through the inheritance of the *B2C_1A_base_extensions policy*.</span></span>

<span data-ttu-id="33fe6-105">Mint ilyen akkor különösen mutatja be a már definiált jogcímtípusok, a jogcímek átalakítása, tartalom definíciók, az a műszaki profil és a központi felhasználói utak jogcímszolgáltatóktól.</span><span class="sxs-lookup"><span data-stu-id="33fe6-105">As such, it more particularly focusses on the already defined claim types, claims transformations, content definitions, claims providers with their technical profile(s), and the core user journeys.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33fe6-106">A Microsoft nem vállal sem kifejezett szavatosságot, a továbbiakban megadott adatai.</span><span class="sxs-lookup"><span data-stu-id="33fe6-106">Microsoft makes no warranties, express or implied, with respect to the information provided hereafter.</span></span> <span data-ttu-id="33fe6-107">GA idő, illetve után a módosítások vihetők bármikor GA időpont előtt.</span><span class="sxs-lookup"><span data-stu-id="33fe6-107">Changes may be introduced at any time, before GA time, at GA time, or after.</span></span>

<span data-ttu-id="33fe6-108">Megfelelő saját házirendeket, mind a B2C_1A_base_extensions házirend felülírja ezeket a definíciókat, és a szülő házirend kiterjesztése újak megadásával, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="33fe6-108">Both your own policies and the B2C_1A_base_extensions policy can override these definitions and extend this parent policy by providing additional ones as needed.</span></span>

<span data-ttu-id="33fe6-109">A fő elemei a *B2C_1A_base házirend* jogcímtípusok, a jogcímek átalakítása és a tartalom definíciókat.</span><span class="sxs-lookup"><span data-stu-id="33fe6-109">The core elements of the *B2C_1A_base policy* are claim types, claims transformations, and content definitions.</span></span> <span data-ttu-id="33fe6-110">Ezeket az elemeket is ki vannak téve a megfelelő saját házirendeket is és a lehet hivatkozni a *B2C_1A_base_extensions házirend*.</span><span class="sxs-lookup"><span data-stu-id="33fe6-110">These elements can susceptible to be referenced in your own policies as well as in the *B2C_1A_base_extensions policy*.</span></span>

## <a name="claims-schemas"></a><span data-ttu-id="33fe6-111">Jogcímek sémák</span><span class="sxs-lookup"><span data-stu-id="33fe6-111">Claims schemas</span></span>

<span data-ttu-id="33fe6-112">A jogcím-sémák három részből áll:</span><span class="sxs-lookup"><span data-stu-id="33fe6-112">This claims schemas is divided into three sections:</span></span>

1.  <span data-ttu-id="33fe6-113">Első szakasz, amely felsorolja a minimális jogcímet a felhasználó utak helyes működéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="33fe6-113">A first section that lists the minimum claims that are required for the user journeys to work properly.</span></span>
2.  <span data-ttu-id="33fe6-114">A jogcímek listája második szakasz szükséges lekérdezési karakterlánc és más különleges paraméterei más jogcímszolgáltatóktól, különösen a hitelesítéshez login.microsoftonline.com átadandó.</span><span class="sxs-lookup"><span data-stu-id="33fe6-114">A second section that lists the claims required for query string parameters and other special parameters to be passed to other claims providers, especially login.microsoftonline.com for authentication.</span></span> <span data-ttu-id="33fe6-115">**Ne módosítsa ezeket a jogcímeket**.</span><span class="sxs-lookup"><span data-stu-id="33fe6-115">**Please do not modify these claims**.</span></span>
3.  <span data-ttu-id="33fe6-116">És végül a harmadik szakasz további, opcionális jogcímeket a felhasználó a gyűjtendő felsoroló a könyvtárban tárolja, és a bejelentkezés során küldött jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="33fe6-116">And eventually a third section that lists any additional, optional claims that can be collected from the user, stored in the directory and sent in tokens during sign in.</span></span> <span data-ttu-id="33fe6-117">A felhasználó gyűjtött vagy a token küldött új jogcím típusa ebben a szakaszban lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="33fe6-117">New claims type to be collected from the user and/or sent in the token can be added in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="33fe6-118">A jogcímek séma egyes jogcímek, például a jelszavak és a felhasználónevek korlátozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="33fe6-118">The claims schema contains restrictions on certain claims such as passwords and usernames.</span></span> <span data-ttu-id="33fe6-119">A megbízható keretrendszer (TF) házirend más jogcím-szolgáltató kezeli az Azure AD és a korlátozások vannak modellezni a prémium szintű házirendben.</span><span class="sxs-lookup"><span data-stu-id="33fe6-119">The Trust Framework (TF) policy treats Azure AD as any other claims provider and all its restrictions are modelled in the premium policy.</span></span> <span data-ttu-id="33fe6-120">További korlátozások hozzáadása, vagy egy másik jogcím-szolgáltató használata a hitelesítő adatok tárolása, amelynek a saját korlátozások házirend volt módosítani.</span><span class="sxs-lookup"><span data-stu-id="33fe6-120">A policy could be modified to add more restrictions, or use another claims provider for credential storage which will have its own restrictions.</span></span>

<span data-ttu-id="33fe6-121">A rendelkezésre álló jogcím-típusok listája látható.</span><span class="sxs-lookup"><span data-stu-id="33fe6-121">The available claim types are listed below.</span></span>

### <a name="claims-that-are-required-for-the-user-journeys"></a><span data-ttu-id="33fe6-122">A felhasználó utak szükséges jogcímeket</span><span class="sxs-lookup"><span data-stu-id="33fe6-122">Claims that are required for the user journeys</span></span>

<span data-ttu-id="33fe6-123">A következő jogcímek felhasználói utak helyes működéséhez szükségesek:</span><span class="sxs-lookup"><span data-stu-id="33fe6-123">The following claims are required for user journeys to work properly:</span></span>

| <span data-ttu-id="33fe6-124">Jogcím típusa</span><span class="sxs-lookup"><span data-stu-id="33fe6-124">Claims type</span></span> | <span data-ttu-id="33fe6-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-125">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="33fe6-126">*Felhasználói azonosítóját*</span><span class="sxs-lookup"><span data-stu-id="33fe6-126">*UserId*</span></span> | <span data-ttu-id="33fe6-127">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="33fe6-127">Username</span></span> |
| <span data-ttu-id="33fe6-128">*signInName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-128">*signInName*</span></span> | <span data-ttu-id="33fe6-129">Jelentkezzen be neve</span><span class="sxs-lookup"><span data-stu-id="33fe6-129">Sign in name</span></span> |
| <span data-ttu-id="33fe6-130">*a tenantId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-130">*tenantId*</span></span> | <span data-ttu-id="33fe6-131">Bérlő azonosítóját az Azure AD B2C-támogatás a felhasználói objektum</span><span class="sxs-lookup"><span data-stu-id="33fe6-131">Tenant identifier (ID) of the user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="33fe6-132">*Objektumazonosító*</span><span class="sxs-lookup"><span data-stu-id="33fe6-132">*objectId*</span></span> | <span data-ttu-id="33fe6-133">Objektumazonosító (ID) az Azure AD B2C-támogatás a felhasználói objektum</span><span class="sxs-lookup"><span data-stu-id="33fe6-133">Object identifier (ID) of the user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="33fe6-134">*jelszó*</span><span class="sxs-lookup"><span data-stu-id="33fe6-134">*password*</span></span> | <span data-ttu-id="33fe6-135">Jelszó</span><span class="sxs-lookup"><span data-stu-id="33fe6-135">Password</span></span> |
| <span data-ttu-id="33fe6-136">*ÚjJelszó*</span><span class="sxs-lookup"><span data-stu-id="33fe6-136">*newPassword*</span></span> | |
| <span data-ttu-id="33fe6-137">*reenterPassword*</span><span class="sxs-lookup"><span data-stu-id="33fe6-137">*reenterPassword*</span></span> | |
| <span data-ttu-id="33fe6-138">*passwordPolicies*</span><span class="sxs-lookup"><span data-stu-id="33fe6-138">*passwordPolicies*</span></span> | <span data-ttu-id="33fe6-139">A jelszóházirendek az Azure AD B2C prémium alapján határozzák meg a jelszó erőssége, a lejárat, stb.</span><span class="sxs-lookup"><span data-stu-id="33fe6-139">Password policies used by Azure AD B2C Premium to determine password strength, expiry, etc.</span></span> |
| <span data-ttu-id="33fe6-140">*Sub*</span><span class="sxs-lookup"><span data-stu-id="33fe6-140">*sub*</span></span> | |
| <span data-ttu-id="33fe6-141">*alternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-141">*alternativeSecurityId*</span></span> | |
| <span data-ttu-id="33fe6-142">*identityProvider*</span><span class="sxs-lookup"><span data-stu-id="33fe6-142">*identityProvider*</span></span> | |
| <span data-ttu-id="33fe6-143">*displayName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-143">*displayName*</span></span> | |
| <span data-ttu-id="33fe6-144">*strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="33fe6-144">*strongAuthenticationPhoneNumber*</span></span> | <span data-ttu-id="33fe6-145">A felhasználó telefonszáma</span><span class="sxs-lookup"><span data-stu-id="33fe6-145">User's telephone number</span></span> |
| <span data-ttu-id="33fe6-146">*Verified.strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="33fe6-146">*Verified.strongAuthenticationPhoneNumber*</span></span> | |
| <span data-ttu-id="33fe6-147">*e-mailek*</span><span class="sxs-lookup"><span data-stu-id="33fe6-147">*email*</span></span> | <span data-ttu-id="33fe6-148">Kapcsolattartás a felhasználókkal használható e-mail cím</span><span class="sxs-lookup"><span data-stu-id="33fe6-148">Email address that can be used to contact the user</span></span> |
| <span data-ttu-id="33fe6-149">*signInNamesInfo.emailAddress*</span><span class="sxs-lookup"><span data-stu-id="33fe6-149">*signInNamesInfo.emailAddress*</span></span> | <span data-ttu-id="33fe6-150">A felhasználó használhatja-e bejelentkezni az e-mail címet</span><span class="sxs-lookup"><span data-stu-id="33fe6-150">Email address that the user can use to sign in</span></span> |
| <span data-ttu-id="33fe6-151">*otherMails*</span><span class="sxs-lookup"><span data-stu-id="33fe6-151">*otherMails*</span></span> | <span data-ttu-id="33fe6-152">Kapcsolattartás a felhasználókkal használt e-mail címek</span><span class="sxs-lookup"><span data-stu-id="33fe6-152">Email addresses that can be used to contact the user</span></span> |
| <span data-ttu-id="33fe6-153">*userPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-153">*userPrincipalName*</span></span> | <span data-ttu-id="33fe6-154">Az Azure AD B2C prémium tárolt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="33fe6-154">Username as stored in the Azure AD B2C Premium</span></span> |
| <span data-ttu-id="33fe6-155">*upnUserName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-155">*upnUserName*</span></span> | <span data-ttu-id="33fe6-156">Egyszerű felhasználónév létrehozásához felhasználónév</span><span class="sxs-lookup"><span data-stu-id="33fe6-156">Username for creating user principal name</span></span> |
| <span data-ttu-id="33fe6-157">*mailNickName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-157">*mailNickName*</span></span> | <span data-ttu-id="33fe6-158">Mail nick felhasználónév tárolt az Azure AD B2C-támogatás</span><span class="sxs-lookup"><span data-stu-id="33fe6-158">User's mail nick name as stored in the Azure AD B2C Premium</span></span> |
| <span data-ttu-id="33fe6-159">*Új_felhasználó*</span><span class="sxs-lookup"><span data-stu-id="33fe6-159">*newUser*</span></span> | |
| <span data-ttu-id="33fe6-160">*végre SelfAsserted-bemenet*</span><span class="sxs-lookup"><span data-stu-id="33fe6-160">*executed-SelfAsserted-Input*</span></span> | <span data-ttu-id="33fe6-161">Amely meghatározza, hogy attribútumok gyűjtötte a program a felhasználói jogcímek</span><span class="sxs-lookup"><span data-stu-id="33fe6-161">Claim that specifies whether attributes were collected from the user</span></span> |
| <span data-ttu-id="33fe6-162">*végre PhoneFactor-bemenet*</span><span class="sxs-lookup"><span data-stu-id="33fe6-162">*executed-PhoneFactor-Input*</span></span> | <span data-ttu-id="33fe6-163">Jogcímet, amely megadja, hogy egy új telefonszámot gyűjtötte a program a felhasználó részéről</span><span class="sxs-lookup"><span data-stu-id="33fe6-163">Claim that specifies whether a new phone number was collected from the user</span></span> |
| <span data-ttu-id="33fe6-164">*authenticationSource*</span><span class="sxs-lookup"><span data-stu-id="33fe6-164">*authenticationSource*</span></span> | <span data-ttu-id="33fe6-165">Megadja, hogy a felhasználó hitelesítési közösségi identitásszolgáltató, login.microsoftonline.com vagy helyi fiók</span><span class="sxs-lookup"><span data-stu-id="33fe6-165">Specifies whether the user was authenticated at Social Identity Provider, login.microsoftonline.com, or local account</span></span> |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a><span data-ttu-id="33fe6-166">A lekérdezési karakterlánc és más különleges paraméterei szükséges jogcímeket</span><span class="sxs-lookup"><span data-stu-id="33fe6-166">Claims required for query string parameters and other special parameters</span></span>

<span data-ttu-id="33fe6-167">A következő jogcímeket más jogcímszolgáltatóktól kell továbbítani a Speciális paraméterek (beleértve az egyes lekérdezési karakterlánc paraméterek):</span><span class="sxs-lookup"><span data-stu-id="33fe6-167">The following claims are required to pass on special parameters (including some query string parameters) to other claims providers:</span></span>

| <span data-ttu-id="33fe6-168">Jogcím típusa</span><span class="sxs-lookup"><span data-stu-id="33fe6-168">Claims type</span></span> | <span data-ttu-id="33fe6-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-169">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="33fe6-170">*nux*</span><span class="sxs-lookup"><span data-stu-id="33fe6-170">*nux*</span></span> | <span data-ttu-id="33fe6-171">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-171">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-172">*a hálózati csatlakozási Segéd*</span><span class="sxs-lookup"><span data-stu-id="33fe6-172">*nca*</span></span> | <span data-ttu-id="33fe6-173">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-173">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-174">*parancssor*</span><span class="sxs-lookup"><span data-stu-id="33fe6-174">*prompt*</span></span> | <span data-ttu-id="33fe6-175">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-175">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-176">*mkt*</span><span class="sxs-lookup"><span data-stu-id="33fe6-176">*mkt*</span></span> | <span data-ttu-id="33fe6-177">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-177">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-178">*LC*</span><span class="sxs-lookup"><span data-stu-id="33fe6-178">*lc*</span></span> | <span data-ttu-id="33fe6-179">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-179">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-180">*grant_type*</span><span class="sxs-lookup"><span data-stu-id="33fe6-180">*grant_type*</span></span> | <span data-ttu-id="33fe6-181">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-181">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-182">*hatókör*</span><span class="sxs-lookup"><span data-stu-id="33fe6-182">*scope*</span></span> | <span data-ttu-id="33fe6-183">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-183">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-184">*client_id*</span><span class="sxs-lookup"><span data-stu-id="33fe6-184">*client_id*</span></span> | <span data-ttu-id="33fe6-185">Speciális paramétert a helyi fiók hitelesítési login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33fe6-185">Special parameter passed for local account authentication to login.microsoftonline.com</span></span> |
| <span data-ttu-id="33fe6-186">*objectIdFromSession*</span><span class="sxs-lookup"><span data-stu-id="33fe6-186">*objectIdFromSession*</span></span> | <span data-ttu-id="33fe6-187">Az alapértelmezett munkamenet felügyeleti szolgáltató annak jelzésére, hogy az egyszeri bejelentkezési munkamenet objektumazonosító beolvasása a megadott paraméter</span><span class="sxs-lookup"><span data-stu-id="33fe6-187">Parameter provided by the default session management provider to indicate that the object id has been retrieved from an SSO session</span></span> |
| <span data-ttu-id="33fe6-188">*isActiveMFASession*</span><span class="sxs-lookup"><span data-stu-id="33fe6-188">*isActiveMFASession*</span></span> | <span data-ttu-id="33fe6-189">A többtényezős hitelesítés annak jelzésére, hogy a felhasználó rendelkezik-e többtényezős hitelesítés aktív munkamenet munkamenet-kezelés által biztosított paraméter</span><span class="sxs-lookup"><span data-stu-id="33fe6-189">Parameter provided by the MFA session management to indicate that the user has an active MFA session</span></span> |

### <a name="additional-optional-claims-that-can-be-collected"></a><span data-ttu-id="33fe6-190">További (nem kötelező) jogcímeket is</span><span class="sxs-lookup"><span data-stu-id="33fe6-190">Additional (optional) claims that can be collected</span></span>

<span data-ttu-id="33fe6-191">A következő jogcímek további gyűjtött felhasználói, a címtárban tárolt, és a token küldi.</span><span class="sxs-lookup"><span data-stu-id="33fe6-191">The following claims are additional claims that can be collected from the users, stored in the directory, and sent in the token.</span></span> <span data-ttu-id="33fe6-192">Mielőtt leírtak további jogcímeket is hozzáadhatók erre a listára.</span><span class="sxs-lookup"><span data-stu-id="33fe6-192">As outlined before, additional claims can be added to this list.</span></span>

| <span data-ttu-id="33fe6-193">Jogcím típusa</span><span class="sxs-lookup"><span data-stu-id="33fe6-193">Claims type</span></span> | <span data-ttu-id="33fe6-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-194">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="33fe6-195">*givenName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-195">*givenName*</span></span> | <span data-ttu-id="33fe6-196">A megadott felhasználónév (más néven Keresztnév)</span><span class="sxs-lookup"><span data-stu-id="33fe6-196">User's given name (also known as first name)</span></span> |
| <span data-ttu-id="33fe6-197">*Vezetéknév*</span><span class="sxs-lookup"><span data-stu-id="33fe6-197">*surname*</span></span> | <span data-ttu-id="33fe6-198">Felhasználó vezetékneve (más néven Családnév vagy vezetéknevet)</span><span class="sxs-lookup"><span data-stu-id="33fe6-198">User's surname (also known as family name or last name)</span></span> |
| <span data-ttu-id="33fe6-199">*Extension_picture*</span><span class="sxs-lookup"><span data-stu-id="33fe6-199">*Extension_picture*</span></span> | <span data-ttu-id="33fe6-200">Felhasználó társadalombiztosítási kép</span><span class="sxs-lookup"><span data-stu-id="33fe6-200">User's picture from social</span></span> |

## <a name="claim-transformations"></a><span data-ttu-id="33fe6-201">A jogcímek átalakításához</span><span class="sxs-lookup"><span data-stu-id="33fe6-201">Claim transformations</span></span>

<span data-ttu-id="33fe6-202">A rendelkezésre álló a jogcímek átalakításához alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="33fe6-202">The available claim transformations are listed below.</span></span>

| <span data-ttu-id="33fe6-203">Jogcím-átalakítást</span><span class="sxs-lookup"><span data-stu-id="33fe6-203">Claim transformation</span></span> | <span data-ttu-id="33fe6-204">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-204">Description</span></span> |
|----------------------|-------------|
| <span data-ttu-id="33fe6-205">*CreateOtherMailsFromEmail*</span><span class="sxs-lookup"><span data-stu-id="33fe6-205">*CreateOtherMailsFromEmail*</span></span> | |
| <span data-ttu-id="33fe6-206">*CreateRandomUPNUserName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-206">*CreateRandomUPNUserName*</span></span> | |
| <span data-ttu-id="33fe6-207">*CreateUserPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="33fe6-207">*CreateUserPrincipalName*</span></span> | |
| <span data-ttu-id="33fe6-208">*CreateSubjectClaimFromObjectID*</span><span class="sxs-lookup"><span data-stu-id="33fe6-208">*CreateSubjectClaimFromObjectID*</span></span> | |
| <span data-ttu-id="33fe6-209">*CreateSubjectClaimFromAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-209">*CreateSubjectClaimFromAlternativeSecurityId*</span></span> | |
| <span data-ttu-id="33fe6-210">*CreateAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-210">*CreateAlternativeSecurityId*</span></span> | |

## <a name="content-definitions"></a><span data-ttu-id="33fe6-211">Tartalom definíciók</span><span class="sxs-lookup"><span data-stu-id="33fe6-211">Content definitions</span></span>

<span data-ttu-id="33fe6-212">Ez a szakasz ismerteti a tartalom-definíciók már deklarálva a *B2C_1A_base* házirend.</span><span class="sxs-lookup"><span data-stu-id="33fe6-212">This section describes the content definitions already declared in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="33fe6-213">A tartalom definíciók lehet hivatkozni, felül, illetve szükség esetén a megfelelő saját házirendeket, valamint hasonlóan a kiterjesztett ki vannak téve a *B2C_1A_base_extensions* házirend.</span><span class="sxs-lookup"><span data-stu-id="33fe6-213">These content definitions are susceptible to be referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="33fe6-214">Jogcím-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="33fe6-214">Claims provider</span></span> | <span data-ttu-id="33fe6-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-215">Description</span></span> |
|-----------------|-------------|
| <span data-ttu-id="33fe6-216">*Facebook*</span><span class="sxs-lookup"><span data-stu-id="33fe6-216">*Facebook*</span></span> | |
| <span data-ttu-id="33fe6-217">*Helyi fiók SignIn*</span><span class="sxs-lookup"><span data-stu-id="33fe6-217">*Local Account SignIn*</span></span> | |
| <span data-ttu-id="33fe6-218">*PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="33fe6-218">*PhoneFactor*</span></span> | |
| <span data-ttu-id="33fe6-219">*Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="33fe6-219">*Azure Active Directory*</span></span> | |
| <span data-ttu-id="33fe6-220">*Önkiszolgáló magas*</span><span class="sxs-lookup"><span data-stu-id="33fe6-220">*Self Asserted*</span></span> | |
| <span data-ttu-id="33fe6-221">*Helyi fiók*</span><span class="sxs-lookup"><span data-stu-id="33fe6-221">*Local Account*</span></span> | |
| <span data-ttu-id="33fe6-222">*Munkamenet-kezelés*</span><span class="sxs-lookup"><span data-stu-id="33fe6-222">*Session Management*</span></span> | |
| <span data-ttu-id="33fe6-223">*A házirendmotor Trustframework*</span><span class="sxs-lookup"><span data-stu-id="33fe6-223">*Trustframework Policy Engine*</span></span> | |
| <span data-ttu-id="33fe6-224">*TechnicalProfiles*</span><span class="sxs-lookup"><span data-stu-id="33fe6-224">*TechnicalProfiles*</span></span> | |
| <span data-ttu-id="33fe6-225">*Jogkivonatot kibocsátó*</span><span class="sxs-lookup"><span data-stu-id="33fe6-225">*Token Issuer*</span></span> | |

## <a name="technical-profiles"></a><span data-ttu-id="33fe6-226">Műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-226">Technical profiles</span></span>

<span data-ttu-id="33fe6-227">Ez a szakasz mutatja be a műszaki profilok száma a jogcímszolgáltató már deklarálva a *B2C_1A_base* házirend.</span><span class="sxs-lookup"><span data-stu-id="33fe6-227">This section depicts the technical profiles already declared per claim provider in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="33fe6-228">A műszaki profilok lehet további hivatkozott, felül, illetve szükség esetén a megfelelő saját házirendeket, valamint hasonlóan a kiterjesztett ki vannak téve a *B2C_1A_base_extensions* házirend.</span><span class="sxs-lookup"><span data-stu-id="33fe6-228">These technical profiles are susceptible to be further referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

### <a name="technical-profiles-for-facebook"></a><span data-ttu-id="33fe6-229">Műszaki profilokat a Facebook-on</span><span class="sxs-lookup"><span data-stu-id="33fe6-229">Technical profiles for Facebook</span></span>

| <span data-ttu-id="33fe6-230">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-230">Technical profile</span></span> | <span data-ttu-id="33fe6-231">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-231">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-232">*Facebook-OAUTH*</span><span class="sxs-lookup"><span data-stu-id="33fe6-232">*Facebook-OAUTH*</span></span> | |

### <a name="technical-profiles-for-local-account-signin"></a><span data-ttu-id="33fe6-233">A helyi fiókkal bejelentkezik műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-233">Technical profiles for Local Account Signin</span></span>

| <span data-ttu-id="33fe6-234">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-234">Technical profile</span></span> | <span data-ttu-id="33fe6-235">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-235">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-236">*Bejelentkezési nem interaktív*</span><span class="sxs-lookup"><span data-stu-id="33fe6-236">*Login-NonInteractive*</span></span> | |

### <a name="technical-profiles-for-phone-factor"></a><span data-ttu-id="33fe6-237">A Phone Factor műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-237">Technical profiles for Phone Factor</span></span>

| <span data-ttu-id="33fe6-238">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-238">Technical profile</span></span> | <span data-ttu-id="33fe6-239">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-239">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-240">*PhoneFactor-bemenet*</span><span class="sxs-lookup"><span data-stu-id="33fe6-240">*PhoneFactor-Input*</span></span> | |
| <span data-ttu-id="33fe6-241">*PhoneFactor-InputOrVerify*</span><span class="sxs-lookup"><span data-stu-id="33fe6-241">*PhoneFactor-InputOrVerify*</span></span> | |
| <span data-ttu-id="33fe6-242">*PhoneFactor-ellenőrzése*</span><span class="sxs-lookup"><span data-stu-id="33fe6-242">*PhoneFactor-Verify*</span></span> | |

### <a name="technical-profiles-for-azure-active-directory"></a><span data-ttu-id="33fe6-243">Az Azure Active Directory műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-243">Technical profiles for Azure Active Directory</span></span>

| <span data-ttu-id="33fe6-244">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-244">Technical profile</span></span> | <span data-ttu-id="33fe6-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-245">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-246">*Az AAD-közös*</span><span class="sxs-lookup"><span data-stu-id="33fe6-246">*AAD-Common*</span></span> | <span data-ttu-id="33fe6-247">A más AAD-xxx műszaki profil része műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-247">Technical profile included by the other AAD-xxx technical profiles</span></span> |
| <span data-ttu-id="33fe6-248">*Az AAD-UserWriteUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-248">*AAD-UserWriteUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="33fe6-249">A közösségi bejelentkezések során műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-249">Technical profile for social logins</span></span> |
| <span data-ttu-id="33fe6-250">*Az AAD-UserReadUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-250">*AAD-UserReadUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="33fe6-251">A közösségi bejelentkezések során műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-251">Technical profile for social logins</span></span> |
| <span data-ttu-id="33fe6-252">*Az AAD-UserReadUsingAlternativeSecurityId-NoError*</span><span class="sxs-lookup"><span data-stu-id="33fe6-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span></span> | <span data-ttu-id="33fe6-253">A közösségi bejelentkezések során műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-253">Technical profile for social logins</span></span> |
| <span data-ttu-id="33fe6-254">*Az AAD-UserWritePasswordUsingLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="33fe6-254">*AAD-UserWritePasswordUsingLogonEmail*</span></span> | <span data-ttu-id="33fe6-255">A helyi fiókok műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-255">Technical profile for local accounts</span></span> |
| <span data-ttu-id="33fe6-256">*Az AAD-UserReadUsingEmailAddress*</span><span class="sxs-lookup"><span data-stu-id="33fe6-256">*AAD-UserReadUsingEmailAddress*</span></span> | <span data-ttu-id="33fe6-257">A helyi fiókok műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-257">Technical profile for local accounts</span></span> |
| <span data-ttu-id="33fe6-258">*Az AAD-UserWriteProfileUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-258">*AAD-UserWriteProfileUsingObjectId*</span></span> | <span data-ttu-id="33fe6-259">Műszaki profil használatával objectId felhasználói rekord frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="33fe6-259">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="33fe6-260">*Az AAD-UserWritePhoneNumberUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-260">*AAD-UserWritePhoneNumberUsingObjectId*</span></span> | <span data-ttu-id="33fe6-261">Műszaki profil használatával objectId felhasználói rekord frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="33fe6-261">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="33fe6-262">*Az AAD-UserWritePasswordUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-262">*AAD-UserWritePasswordUsingObjectId*</span></span> | <span data-ttu-id="33fe6-263">Műszaki profil használatával objectId felhasználói rekord frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="33fe6-263">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="33fe6-264">*Az AAD-UserReadUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="33fe6-264">*AAD-UserReadUsingObjectId*</span></span> | <span data-ttu-id="33fe6-265">Műszaki profil segítségével adatokat olvasni, miután a felhasználók hitelesítése</span><span class="sxs-lookup"><span data-stu-id="33fe6-265">Technical profile is used to read data after user authenticates</span></span> |

### <a name="technical-profiles-for-self-asserted"></a><span data-ttu-id="33fe6-266">Az önkiszolgáló magas műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-266">Technical profiles for Self Asserted</span></span>

| <span data-ttu-id="33fe6-267">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-267">Technical profile</span></span> | <span data-ttu-id="33fe6-268">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-268">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-269">*SelfAsserted-társadalombiztosítási*</span><span class="sxs-lookup"><span data-stu-id="33fe6-269">*SelfAsserted-Social*</span></span> | |
| <span data-ttu-id="33fe6-270">*SelfAsserted-ProfileUpdate*</span><span class="sxs-lookup"><span data-stu-id="33fe6-270">*SelfAsserted-ProfileUpdate*</span></span> | |

### <a name="technical-profiles-for-local-account"></a><span data-ttu-id="33fe6-271">Helyi fiók műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-271">Technical profiles for Local Account</span></span>

| <span data-ttu-id="33fe6-272">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-272">Technical profile</span></span> | <span data-ttu-id="33fe6-273">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-273">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-274">*LocalAccountSignUpWithLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="33fe6-274">*LocalAccountSignUpWithLogonEmail*</span></span> | |

### <a name="technical-profiles-for-session-management"></a><span data-ttu-id="33fe6-275">Műszaki profilokat a munkamenet-kezelés</span><span class="sxs-lookup"><span data-stu-id="33fe6-275">Technical profiles for Session Management</span></span>

| <span data-ttu-id="33fe6-276">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-276">Technical profile</span></span> | <span data-ttu-id="33fe6-277">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-277">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-278">*SM-Noop*</span><span class="sxs-lookup"><span data-stu-id="33fe6-278">*SM-Noop*</span></span> | |
| <span data-ttu-id="33fe6-279">*SM-AAD-BEN*</span><span class="sxs-lookup"><span data-stu-id="33fe6-279">*SM-AAD*</span></span> | |
| <span data-ttu-id="33fe6-280">*SM-SocialSignup*</span><span class="sxs-lookup"><span data-stu-id="33fe6-280">*SM-SocialSignup*</span></span> | <span data-ttu-id="33fe6-281">Elem egyértelműségének biztosításához AAD munkamenet között jelentkezzen be, és jelentkezzen be a profil neve használatban van</span><span class="sxs-lookup"><span data-stu-id="33fe6-281">Profile name is being used to disambiguate AAD session between sign up and sign in</span></span> |
| <span data-ttu-id="33fe6-282">*SM-SocialLogin*</span><span class="sxs-lookup"><span data-stu-id="33fe6-282">*SM-SocialLogin*</span></span> | |
| <span data-ttu-id="33fe6-283">*SM-MFA*</span><span class="sxs-lookup"><span data-stu-id="33fe6-283">*SM-MFA*</span></span> | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a><span data-ttu-id="33fe6-284">Trustframework házirend motor TechnicalProfiles műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-284">Technical profiles for Trustframework Policy Engine TechnicalProfiles</span></span>

<span data-ttu-id="33fe6-285">Jelenleg nincsenek technikai profilok meghatározása a **Trustframework házirend motor TechnicalProfiles** jogcím-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="33fe6-285">Currently, no technical profiles are defined for the **Trustframework Policy Engine TechnicalProfiles** claims provider.</span></span>

### <a name="technical-profiles-for-token-issuer"></a><span data-ttu-id="33fe6-286">Jogkivonatot kibocsátó műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="33fe6-286">Technical profiles for Token Issuer</span></span>

| <span data-ttu-id="33fe6-287">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="33fe6-287">Technical profile</span></span> | <span data-ttu-id="33fe6-288">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-288">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="33fe6-289">*JwtIssuer*</span><span class="sxs-lookup"><span data-stu-id="33fe6-289">*JwtIssuer*</span></span> | |

## <a name="user-journeys"></a><span data-ttu-id="33fe6-290">Felhasználói utak</span><span class="sxs-lookup"><span data-stu-id="33fe6-290">User journeys</span></span>

<span data-ttu-id="33fe6-291">Ez a szakasz mutatja be a felhasználó utak már deklarálva a *B2C_1A_base* házirend.</span><span class="sxs-lookup"><span data-stu-id="33fe6-291">This section depicts the user journeys already declared in the *B2C_1A_base* policy.</span></span> <span data-ttu-id="33fe6-292">Ezek az felhasználói utak amelyek ki vannak téve lehet további hivatkozott, felül, és/vagy szükség esetén a megfelelő saját házirendeket, valamint hasonlóan a kiterjesztett a *B2C_1A_base_extensions* házirend.</span><span class="sxs-lookup"><span data-stu-id="33fe6-292">These user journeys are susceptible to be further referenced, overridden, and/or extended as needed in your own policies as well as in the *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="33fe6-293">Felhasználói út</span><span class="sxs-lookup"><span data-stu-id="33fe6-293">User journey</span></span> | <span data-ttu-id="33fe6-294">Leírás</span><span class="sxs-lookup"><span data-stu-id="33fe6-294">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="33fe6-295">*Regisztráció*</span><span class="sxs-lookup"><span data-stu-id="33fe6-295">*SignUp*</span></span> | |
| <span data-ttu-id="33fe6-296">*Bejelentkezés*</span><span class="sxs-lookup"><span data-stu-id="33fe6-296">*SignIn*</span></span> | |
| <span data-ttu-id="33fe6-297">*SignUpOrSignIn*</span><span class="sxs-lookup"><span data-stu-id="33fe6-297">*SignUpOrSignIn*</span></span> | |
| <span data-ttu-id="33fe6-298">*EditProfile*</span><span class="sxs-lookup"><span data-stu-id="33fe6-298">*EditProfile*</span></span> | |
| <span data-ttu-id="33fe6-299">*PasswordReset*</span><span class="sxs-lookup"><span data-stu-id="33fe6-299">*PasswordReset*</span></span> | |

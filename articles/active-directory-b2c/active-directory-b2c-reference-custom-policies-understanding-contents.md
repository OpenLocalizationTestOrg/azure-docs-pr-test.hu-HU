---
title: "Az Azure Active Directory B2C: Hello alapszintű csomag egyéni házirendjeinek ismertetése |} Microsoft Docs"
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
ms.openlocfilehash: 3484e8cc6fa6a9d57c2aa14c0cc9616065892d10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-custom-policies-of-hello-azure-ad-b2c-custom-policy-starter-pack"></a><span data-ttu-id="256b7-103">Egyéni házirendek hello hello Azure AD B2C egyéni házirend alapszintű csomag ismertetése</span><span class="sxs-lookup"><span data-stu-id="256b7-103">Understanding hello custom policies of hello Azure AD B2C Custom Policy starter pack</span></span>

<span data-ttu-id="256b7-104">Ez a rész felsorolja a hello hello B2C_1A_base házirend minden hello core elemei **alapszintű csomag** és, hogy a rendszer elkészítéséhez használja megfelelő saját házirendeket a hello hello örökléssel tartalomkészítéshez *B2C_1A_base_ bővítmények házirend*.</span><span class="sxs-lookup"><span data-stu-id="256b7-104">This section lists all hello core elements of hello B2C_1A_base policy that comes with hello **Starter Pack** and that is leveraged for authoring your own policies through hello inheritance of hello *B2C_1A_base_extensions policy*.</span></span>

<span data-ttu-id="256b7-105">Azt több különösen mutatja be már meg van adva hello jogcím-típusokat, a jogcímek átalakítása, tartalom definíciók, az a műszaki profil Jogcímszolgáltatók, és alapvető felhasználói utak hello.</span><span class="sxs-lookup"><span data-stu-id="256b7-105">As such, it more particularly focusses on hello already defined claim types, claims transformations, content definitions, claims providers with their technical profile(s), and hello core user journeys.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="256b7-106">A Microsoft nem vállal semmilyen kifejezett, kifejezett vagy vélelmezett, továbbiakban megadott tekintetben toohello adatokkal.</span><span class="sxs-lookup"><span data-stu-id="256b7-106">Microsoft makes no warranties, express or implied, with respect toohello information provided hereafter.</span></span> <span data-ttu-id="256b7-107">GA idő, illetve után a módosítások vihetők bármikor GA időpont előtt.</span><span class="sxs-lookup"><span data-stu-id="256b7-107">Changes may be introduced at any time, before GA time, at GA time, or after.</span></span>

<span data-ttu-id="256b7-108">Saját házirendek és a hello B2C_1A_base_extensions házirend felülírja ezeket a definíciókat, és a szülő házirend kiterjesztése újak megadásával, igény szerint.</span><span class="sxs-lookup"><span data-stu-id="256b7-108">Both your own policies and hello B2C_1A_base_extensions policy can override these definitions and extend this parent policy by providing additional ones as needed.</span></span>

<span data-ttu-id="256b7-109">alapvető összetevőjét hello hello *B2C_1A_base házirend* jogcímtípusok, a jogcímek átalakítása és a tartalom definíciókat.</span><span class="sxs-lookup"><span data-stu-id="256b7-109">hello core elements of hello *B2C_1A_base policy* are claim types, claims transformations, and content definitions.</span></span> <span data-ttu-id="256b7-110">Ezeket az elemeket is ki vannak téve toobe hello mellett a saját a házirendeket, valamint a hivatkozott *B2C_1A_base_extensions házirend*.</span><span class="sxs-lookup"><span data-stu-id="256b7-110">These elements can susceptible toobe referenced in your own policies as well as in hello *B2C_1A_base_extensions policy*.</span></span>

## <a name="claims-schemas"></a><span data-ttu-id="256b7-111">Jogcímek sémák</span><span class="sxs-lookup"><span data-stu-id="256b7-111">Claims schemas</span></span>

<span data-ttu-id="256b7-112">A jogcím-sémák három részből áll:</span><span class="sxs-lookup"><span data-stu-id="256b7-112">This claims schemas is divided into three sections:</span></span>

1.  <span data-ttu-id="256b7-113">Első szakasz hello minimális jogcímeket, amelyek szükségesek a hello felhasználói utak toowork megfelelően felsoroló.</span><span class="sxs-lookup"><span data-stu-id="256b7-113">A first section that lists hello minimum claims that are required for hello user journeys toowork properly.</span></span>
2.  <span data-ttu-id="256b7-114">Második szakasz listák hello lekérdezési karakterlánc paraméter szükséges jogcímeket és a más különleges paraméterek toobe átadott tooother Jogcímszolgáltatók, különösen a hitelesítéshez login.microsoftonline.com.</span><span class="sxs-lookup"><span data-stu-id="256b7-114">A second section that lists hello claims required for query string parameters and other special parameters toobe passed tooother claims providers, especially login.microsoftonline.com for authentication.</span></span> <span data-ttu-id="256b7-115">**Ne módosítsa ezeket a jogcímeket**.</span><span class="sxs-lookup"><span data-stu-id="256b7-115">**Please do not modify these claims**.</span></span>
3.  <span data-ttu-id="256b7-116">És végül a hello felhasználótól összegyűjthetők további, opcionális jogcímeket felsoroló harmadik szakasz hello könyvtárban tárolja, és a bejelentkezés során küldött jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="256b7-116">And eventually a third section that lists any additional, optional claims that can be collected from hello user, stored in hello directory and sent in tokens during sign in.</span></span> <span data-ttu-id="256b7-117">Új jogcím típusa toobe hello felhasználói gyűjtött és/vagy küldi hello token ebben a szakaszban lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="256b7-117">New claims type toobe collected from hello user and/or sent in hello token can be added in this section.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="256b7-118">hello jogcímek séma egyes jogcímek, például a jelszavak és a felhasználónevek korlátozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="256b7-118">hello claims schema contains restrictions on certain claims such as passwords and usernames.</span></span> <span data-ttu-id="256b7-119">hello megbízható keretrendszer (TF) házirend más jogcím-szolgáltató kezeli az Azure AD és a korlátozások vannak modellezni hello prémium házirendben.</span><span class="sxs-lookup"><span data-stu-id="256b7-119">hello Trust Framework (TF) policy treats Azure AD as any other claims provider and all its restrictions are modelled in hello premium policy.</span></span> <span data-ttu-id="256b7-120">Egy házirend módosított tooadd további korlátozásokat, vagy egy másik jogcímszolgáltató használandó hitelesítő adatok tárolása, amelynek a saját korlátozások.</span><span class="sxs-lookup"><span data-stu-id="256b7-120">A policy could be modified tooadd more restrictions, or use another claims provider for credential storage which will have its own restrictions.</span></span>

<span data-ttu-id="256b7-121">hello elérhető jogcímtípusok alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="256b7-121">hello available claim types are listed below.</span></span>

### <a name="claims-that-are-required-for-hello-user-journeys"></a><span data-ttu-id="256b7-122">A hello felhasználói utak által igényelt jogcímeket</span><span class="sxs-lookup"><span data-stu-id="256b7-122">Claims that are required for hello user journeys</span></span>

<span data-ttu-id="256b7-123">a következő jogcímeket hello szükségesek a felhasználó utak toowork megfelelően:</span><span class="sxs-lookup"><span data-stu-id="256b7-123">hello following claims are required for user journeys toowork properly:</span></span>

| <span data-ttu-id="256b7-124">Jogcím típusa</span><span class="sxs-lookup"><span data-stu-id="256b7-124">Claims type</span></span> | <span data-ttu-id="256b7-125">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-125">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="256b7-126">*Felhasználói azonosítóját*</span><span class="sxs-lookup"><span data-stu-id="256b7-126">*UserId*</span></span> | <span data-ttu-id="256b7-127">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="256b7-127">Username</span></span> |
| <span data-ttu-id="256b7-128">*signInName*</span><span class="sxs-lookup"><span data-stu-id="256b7-128">*signInName*</span></span> | <span data-ttu-id="256b7-129">Jelentkezzen be neve</span><span class="sxs-lookup"><span data-stu-id="256b7-129">Sign in name</span></span> |
| <span data-ttu-id="256b7-130">*a tenantId*</span><span class="sxs-lookup"><span data-stu-id="256b7-130">*tenantId*</span></span> | <span data-ttu-id="256b7-131">Bérlő azonosítóját az Azure AD B2C prémium hello felhasználói objektum</span><span class="sxs-lookup"><span data-stu-id="256b7-131">Tenant identifier (ID) of hello user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="256b7-132">*Objektumazonosító*</span><span class="sxs-lookup"><span data-stu-id="256b7-132">*objectId*</span></span> | <span data-ttu-id="256b7-133">Objektumazonosító (ID) az Azure AD B2C prémium hello felhasználói objektum</span><span class="sxs-lookup"><span data-stu-id="256b7-133">Object identifier (ID) of hello user object in Azure AD B2C Premium</span></span> |
| <span data-ttu-id="256b7-134">*jelszó*</span><span class="sxs-lookup"><span data-stu-id="256b7-134">*password*</span></span> | <span data-ttu-id="256b7-135">Jelszó</span><span class="sxs-lookup"><span data-stu-id="256b7-135">Password</span></span> |
| <span data-ttu-id="256b7-136">*ÚjJelszó*</span><span class="sxs-lookup"><span data-stu-id="256b7-136">*newPassword*</span></span> | |
| <span data-ttu-id="256b7-137">*reenterPassword*</span><span class="sxs-lookup"><span data-stu-id="256b7-137">*reenterPassword*</span></span> | |
| <span data-ttu-id="256b7-138">*passwordPolicies*</span><span class="sxs-lookup"><span data-stu-id="256b7-138">*passwordPolicies*</span></span> | <span data-ttu-id="256b7-139">A jelszóházirendek használják az Azure AD B2C prémium toodetermine jelszó erőssége, a lejárat, stb.</span><span class="sxs-lookup"><span data-stu-id="256b7-139">Password policies used by Azure AD B2C Premium toodetermine password strength, expiry, etc.</span></span> |
| <span data-ttu-id="256b7-140">*Sub*</span><span class="sxs-lookup"><span data-stu-id="256b7-140">*sub*</span></span> | |
| <span data-ttu-id="256b7-141">*alternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="256b7-141">*alternativeSecurityId*</span></span> | |
| <span data-ttu-id="256b7-142">*identityProvider*</span><span class="sxs-lookup"><span data-stu-id="256b7-142">*identityProvider*</span></span> | |
| <span data-ttu-id="256b7-143">*displayName*</span><span class="sxs-lookup"><span data-stu-id="256b7-143">*displayName*</span></span> | |
| <span data-ttu-id="256b7-144">*strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="256b7-144">*strongAuthenticationPhoneNumber*</span></span> | <span data-ttu-id="256b7-145">A felhasználó telefonszáma</span><span class="sxs-lookup"><span data-stu-id="256b7-145">User's telephone number</span></span> |
| <span data-ttu-id="256b7-146">*Verified.strongAuthenticationPhoneNumber*</span><span class="sxs-lookup"><span data-stu-id="256b7-146">*Verified.strongAuthenticationPhoneNumber*</span></span> | |
| <span data-ttu-id="256b7-147">*e-mailek*</span><span class="sxs-lookup"><span data-stu-id="256b7-147">*email*</span></span> | <span data-ttu-id="256b7-148">E-mail cím használható használt toocontact hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="256b7-148">Email address that can be used toocontact hello user</span></span> |
| <span data-ttu-id="256b7-149">*signInNamesInfo.emailAddress*</span><span class="sxs-lookup"><span data-stu-id="256b7-149">*signInNamesInfo.emailAddress*</span></span> | <span data-ttu-id="256b7-150">E-mail címet, amely a felhasználó hello használhatja a toosign</span><span class="sxs-lookup"><span data-stu-id="256b7-150">Email address that hello user can use toosign in</span></span> |
| <span data-ttu-id="256b7-151">*otherMails*</span><span class="sxs-lookup"><span data-stu-id="256b7-151">*otherMails*</span></span> | <span data-ttu-id="256b7-152">E-mail címek, amelyek lehetnek használt toocontact hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="256b7-152">Email addresses that can be used toocontact hello user</span></span> |
| <span data-ttu-id="256b7-153">*userPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="256b7-153">*userPrincipalName*</span></span> | <span data-ttu-id="256b7-154">Az Azure AD B2C prémium hello tárolt felhasználónév</span><span class="sxs-lookup"><span data-stu-id="256b7-154">Username as stored in hello Azure AD B2C Premium</span></span> |
| <span data-ttu-id="256b7-155">*upnUserName*</span><span class="sxs-lookup"><span data-stu-id="256b7-155">*upnUserName*</span></span> | <span data-ttu-id="256b7-156">Egyszerű felhasználónév létrehozásához felhasználónév</span><span class="sxs-lookup"><span data-stu-id="256b7-156">Username for creating user principal name</span></span> |
| <span data-ttu-id="256b7-157">*mailNickName*</span><span class="sxs-lookup"><span data-stu-id="256b7-157">*mailNickName*</span></span> | <span data-ttu-id="256b7-158">Mail nick felhasználónév tárolt hello Azure AD B2C-támogatás</span><span class="sxs-lookup"><span data-stu-id="256b7-158">User's mail nick name as stored in hello Azure AD B2C Premium</span></span> |
| <span data-ttu-id="256b7-159">*Új_felhasználó*</span><span class="sxs-lookup"><span data-stu-id="256b7-159">*newUser*</span></span> | |
| <span data-ttu-id="256b7-160">*végre SelfAsserted-bemenet*</span><span class="sxs-lookup"><span data-stu-id="256b7-160">*executed-SelfAsserted-Input*</span></span> | <span data-ttu-id="256b7-161">Jogcímet, amely megadja, hogy attribútumok gyűjtötte a program a hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="256b7-161">Claim that specifies whether attributes were collected from hello user</span></span> |
| <span data-ttu-id="256b7-162">*végre PhoneFactor-bemenet*</span><span class="sxs-lookup"><span data-stu-id="256b7-162">*executed-PhoneFactor-Input*</span></span> | <span data-ttu-id="256b7-163">Jogcímet, amely megadja, hogy egy új telefonszámot gyűjtötte a program a hello felhasználó</span><span class="sxs-lookup"><span data-stu-id="256b7-163">Claim that specifies whether a new phone number was collected from hello user</span></span> |
| <span data-ttu-id="256b7-164">*authenticationSource*</span><span class="sxs-lookup"><span data-stu-id="256b7-164">*authenticationSource*</span></span> | <span data-ttu-id="256b7-165">Megadja, hogy hello felhasználó közösségi identitásszolgáltató, login.microsoftonline.com vagy helyi fiók hitelesítési</span><span class="sxs-lookup"><span data-stu-id="256b7-165">Specifies whether hello user was authenticated at Social Identity Provider, login.microsoftonline.com, or local account</span></span> |

### <a name="claims-required-for-query-string-parameters-and-other-special-parameters"></a><span data-ttu-id="256b7-166">A lekérdezési karakterlánc és más különleges paraméterei szükséges jogcímeket</span><span class="sxs-lookup"><span data-stu-id="256b7-166">Claims required for query string parameters and other special parameters</span></span>

<span data-ttu-id="256b7-167">hello következő jogcímek olyan speciális paramétert (beleértve az egyes lekérdezési karakterlánc paraméterek) tooother Jogcímszolgáltatók a szükséges toopass:</span><span class="sxs-lookup"><span data-stu-id="256b7-167">hello following claims are required toopass on special parameters (including some query string parameters) tooother claims providers:</span></span>

| <span data-ttu-id="256b7-168">Jogcím típusa</span><span class="sxs-lookup"><span data-stu-id="256b7-168">Claims type</span></span> | <span data-ttu-id="256b7-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-169">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="256b7-170">*nux*</span><span class="sxs-lookup"><span data-stu-id="256b7-170">*nux*</span></span> | <span data-ttu-id="256b7-171">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-171">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-172">*a hálózati csatlakozási Segéd*</span><span class="sxs-lookup"><span data-stu-id="256b7-172">*nca*</span></span> | <span data-ttu-id="256b7-173">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-173">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-174">*parancssor*</span><span class="sxs-lookup"><span data-stu-id="256b7-174">*prompt*</span></span> | <span data-ttu-id="256b7-175">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-175">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-176">*mkt*</span><span class="sxs-lookup"><span data-stu-id="256b7-176">*mkt*</span></span> | <span data-ttu-id="256b7-177">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-177">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-178">*LC*</span><span class="sxs-lookup"><span data-stu-id="256b7-178">*lc*</span></span> | <span data-ttu-id="256b7-179">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-179">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-180">*grant_type*</span><span class="sxs-lookup"><span data-stu-id="256b7-180">*grant_type*</span></span> | <span data-ttu-id="256b7-181">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-181">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-182">*hatókör*</span><span class="sxs-lookup"><span data-stu-id="256b7-182">*scope*</span></span> | <span data-ttu-id="256b7-183">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-183">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-184">*client_id*</span><span class="sxs-lookup"><span data-stu-id="256b7-184">*client_id*</span></span> | <span data-ttu-id="256b7-185">Speciális paramétert a helyi fiók hitelesítési toologin.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="256b7-185">Special parameter passed for local account authentication toologin.microsoftonline.com</span></span> |
| <span data-ttu-id="256b7-186">*objectIdFromSession*</span><span class="sxs-lookup"><span data-stu-id="256b7-186">*objectIdFromSession*</span></span> | <span data-ttu-id="256b7-187">Hello alapértelmezett munkamenet felügyeleti szolgáltató tooindicate, objektumazonosító: hello által biztosított paraméter egy egyszeri bejelentkezési munkamenet beolvasása</span><span class="sxs-lookup"><span data-stu-id="256b7-187">Parameter provided by hello default session management provider tooindicate that hello object id has been retrieved from an SSO session</span></span> |
| <span data-ttu-id="256b7-188">*isActiveMFASession*</span><span class="sxs-lookup"><span data-stu-id="256b7-188">*isActiveMFASession*</span></span> | <span data-ttu-id="256b7-189">Hello MFA munkamenet felügyeleti tooindicate által biztosított paraméter hello rendelkezik aktív MFA munkamenet</span><span class="sxs-lookup"><span data-stu-id="256b7-189">Parameter provided by hello MFA session management tooindicate that hello user has an active MFA session</span></span> |

### <a name="additional-optional-claims-that-can-be-collected"></a><span data-ttu-id="256b7-190">További (nem kötelező) jogcímeket is</span><span class="sxs-lookup"><span data-stu-id="256b7-190">Additional (optional) claims that can be collected</span></span>

<span data-ttu-id="256b7-191">hello következő hello felhasználóktól összegyűjthetők további jogcímeket hello könyvtárban tárolja, és hello tokent küldött jogcímek.</span><span class="sxs-lookup"><span data-stu-id="256b7-191">hello following claims are additional claims that can be collected from hello users, stored in hello directory, and sent in hello token.</span></span> <span data-ttu-id="256b7-192">Mielőtt leírtak további jogcímek toothis lista lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="256b7-192">As outlined before, additional claims can be added toothis list.</span></span>

| <span data-ttu-id="256b7-193">Jogcím típusa</span><span class="sxs-lookup"><span data-stu-id="256b7-193">Claims type</span></span> | <span data-ttu-id="256b7-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-194">Description</span></span> |
|-------------|-------------|
| <span data-ttu-id="256b7-195">*givenName*</span><span class="sxs-lookup"><span data-stu-id="256b7-195">*givenName*</span></span> | <span data-ttu-id="256b7-196">A megadott felhasználónév (más néven Keresztnév)</span><span class="sxs-lookup"><span data-stu-id="256b7-196">User's given name (also known as first name)</span></span> |
| <span data-ttu-id="256b7-197">*Vezetéknév*</span><span class="sxs-lookup"><span data-stu-id="256b7-197">*surname*</span></span> | <span data-ttu-id="256b7-198">Felhasználó vezetékneve (más néven Családnév vagy vezetéknevet)</span><span class="sxs-lookup"><span data-stu-id="256b7-198">User's surname (also known as family name or last name)</span></span> |
| <span data-ttu-id="256b7-199">*Extension_picture*</span><span class="sxs-lookup"><span data-stu-id="256b7-199">*Extension_picture*</span></span> | <span data-ttu-id="256b7-200">Felhasználó társadalombiztosítási kép</span><span class="sxs-lookup"><span data-stu-id="256b7-200">User's picture from social</span></span> |

## <a name="claim-transformations"></a><span data-ttu-id="256b7-201">A jogcímek átalakításához</span><span class="sxs-lookup"><span data-stu-id="256b7-201">Claim transformations</span></span>

<span data-ttu-id="256b7-202">hello érhető el a jogcímek átalakításához alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="256b7-202">hello available claim transformations are listed below.</span></span>

| <span data-ttu-id="256b7-203">Jogcím-átalakítást</span><span class="sxs-lookup"><span data-stu-id="256b7-203">Claim transformation</span></span> | <span data-ttu-id="256b7-204">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-204">Description</span></span> |
|----------------------|-------------|
| <span data-ttu-id="256b7-205">*CreateOtherMailsFromEmail*</span><span class="sxs-lookup"><span data-stu-id="256b7-205">*CreateOtherMailsFromEmail*</span></span> | |
| <span data-ttu-id="256b7-206">*CreateRandomUPNUserName*</span><span class="sxs-lookup"><span data-stu-id="256b7-206">*CreateRandomUPNUserName*</span></span> | |
| <span data-ttu-id="256b7-207">*CreateUserPrincipalName*</span><span class="sxs-lookup"><span data-stu-id="256b7-207">*CreateUserPrincipalName*</span></span> | |
| <span data-ttu-id="256b7-208">*CreateSubjectClaimFromObjectID*</span><span class="sxs-lookup"><span data-stu-id="256b7-208">*CreateSubjectClaimFromObjectID*</span></span> | |
| <span data-ttu-id="256b7-209">*CreateSubjectClaimFromAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="256b7-209">*CreateSubjectClaimFromAlternativeSecurityId*</span></span> | |
| <span data-ttu-id="256b7-210">*CreateAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="256b7-210">*CreateAlternativeSecurityId*</span></span> | |

## <a name="content-definitions"></a><span data-ttu-id="256b7-211">Tartalom definíciók</span><span class="sxs-lookup"><span data-stu-id="256b7-211">Content definitions</span></span>

<span data-ttu-id="256b7-212">Ez a szakasz ismerteti a definíciók tartalom hello hello már deklarálva *B2C_1A_base* házirend.</span><span class="sxs-lookup"><span data-stu-id="256b7-212">This section describes hello content definitions already declared in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="256b7-213">A tartalom definíciók téve toobe hivatkozott, felül, illetve szükség esetén hello mellett a saját a házirendeket, valamint a kiterjesztett *B2C_1A_base_extensions* házirend.</span><span class="sxs-lookup"><span data-stu-id="256b7-213">These content definitions are susceptible toobe referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="256b7-214">Jogcím-szolgáltató</span><span class="sxs-lookup"><span data-stu-id="256b7-214">Claims provider</span></span> | <span data-ttu-id="256b7-215">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-215">Description</span></span> |
|-----------------|-------------|
| <span data-ttu-id="256b7-216">*Facebook*</span><span class="sxs-lookup"><span data-stu-id="256b7-216">*Facebook*</span></span> | |
| <span data-ttu-id="256b7-217">*Helyi fiók SignIn*</span><span class="sxs-lookup"><span data-stu-id="256b7-217">*Local Account SignIn*</span></span> | |
| <span data-ttu-id="256b7-218">*PhoneFactor*</span><span class="sxs-lookup"><span data-stu-id="256b7-218">*PhoneFactor*</span></span> | |
| <span data-ttu-id="256b7-219">*Azure Active Directory*</span><span class="sxs-lookup"><span data-stu-id="256b7-219">*Azure Active Directory*</span></span> | |
| <span data-ttu-id="256b7-220">*Önkiszolgáló magas*</span><span class="sxs-lookup"><span data-stu-id="256b7-220">*Self Asserted*</span></span> | |
| <span data-ttu-id="256b7-221">*Helyi fiók*</span><span class="sxs-lookup"><span data-stu-id="256b7-221">*Local Account*</span></span> | |
| <span data-ttu-id="256b7-222">*Munkamenet-kezelés*</span><span class="sxs-lookup"><span data-stu-id="256b7-222">*Session Management*</span></span> | |
| <span data-ttu-id="256b7-223">*A házirendmotor Trustframework*</span><span class="sxs-lookup"><span data-stu-id="256b7-223">*Trustframework Policy Engine*</span></span> | |
| <span data-ttu-id="256b7-224">*TechnicalProfiles*</span><span class="sxs-lookup"><span data-stu-id="256b7-224">*TechnicalProfiles*</span></span> | |
| <span data-ttu-id="256b7-225">*Jogkivonatot kibocsátó*</span><span class="sxs-lookup"><span data-stu-id="256b7-225">*Token Issuer*</span></span> | |

## <a name="technical-profiles"></a><span data-ttu-id="256b7-226">Műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-226">Technical profiles</span></span>

<span data-ttu-id="256b7-227">Ez a szakasz ábrázol hello műszaki profilok / hello a jogcímszolgáltató már deklarálva *B2C_1A_base* házirend.</span><span class="sxs-lookup"><span data-stu-id="256b7-227">This section depicts hello technical profiles already declared per claim provider in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="256b7-228">A műszaki profilok téve toobe további hivatkozott, felül, illetve szükség esetén hello mellett a saját a házirendeket, valamint a kiterjesztett *B2C_1A_base_extensions* házirend.</span><span class="sxs-lookup"><span data-stu-id="256b7-228">These technical profiles are susceptible toobe further referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

### <a name="technical-profiles-for-facebook"></a><span data-ttu-id="256b7-229">Műszaki profilokat a Facebook-on</span><span class="sxs-lookup"><span data-stu-id="256b7-229">Technical profiles for Facebook</span></span>

| <span data-ttu-id="256b7-230">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-230">Technical profile</span></span> | <span data-ttu-id="256b7-231">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-231">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-232">*Facebook-OAUTH*</span><span class="sxs-lookup"><span data-stu-id="256b7-232">*Facebook-OAUTH*</span></span> | |

### <a name="technical-profiles-for-local-account-signin"></a><span data-ttu-id="256b7-233">A helyi fiókkal bejelentkezik műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-233">Technical profiles for Local Account Signin</span></span>

| <span data-ttu-id="256b7-234">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-234">Technical profile</span></span> | <span data-ttu-id="256b7-235">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-235">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-236">*Bejelentkezési nem interaktív*</span><span class="sxs-lookup"><span data-stu-id="256b7-236">*Login-NonInteractive*</span></span> | |

### <a name="technical-profiles-for-phone-factor"></a><span data-ttu-id="256b7-237">A Phone Factor műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-237">Technical profiles for Phone Factor</span></span>

| <span data-ttu-id="256b7-238">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-238">Technical profile</span></span> | <span data-ttu-id="256b7-239">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-239">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-240">*PhoneFactor-bemenet*</span><span class="sxs-lookup"><span data-stu-id="256b7-240">*PhoneFactor-Input*</span></span> | |
| <span data-ttu-id="256b7-241">*PhoneFactor-InputOrVerify*</span><span class="sxs-lookup"><span data-stu-id="256b7-241">*PhoneFactor-InputOrVerify*</span></span> | |
| <span data-ttu-id="256b7-242">*PhoneFactor-ellenőrzése*</span><span class="sxs-lookup"><span data-stu-id="256b7-242">*PhoneFactor-Verify*</span></span> | |

### <a name="technical-profiles-for-azure-active-directory"></a><span data-ttu-id="256b7-243">Az Azure Active Directory műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-243">Technical profiles for Azure Active Directory</span></span>

| <span data-ttu-id="256b7-244">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-244">Technical profile</span></span> | <span data-ttu-id="256b7-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-245">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-246">*Az AAD-közös*</span><span class="sxs-lookup"><span data-stu-id="256b7-246">*AAD-Common*</span></span> | <span data-ttu-id="256b7-247">Műszaki profil benne hello más AAD-xxx műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-247">Technical profile included by hello other AAD-xxx technical profiles</span></span> |
| <span data-ttu-id="256b7-248">*Az AAD-UserWriteUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="256b7-248">*AAD-UserWriteUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="256b7-249">A közösségi bejelentkezések során műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-249">Technical profile for social logins</span></span> |
| <span data-ttu-id="256b7-250">*Az AAD-UserReadUsingAlternativeSecurityId*</span><span class="sxs-lookup"><span data-stu-id="256b7-250">*AAD-UserReadUsingAlternativeSecurityId*</span></span> | <span data-ttu-id="256b7-251">A közösségi bejelentkezések során műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-251">Technical profile for social logins</span></span> |
| <span data-ttu-id="256b7-252">*Az AAD-UserReadUsingAlternativeSecurityId-NoError*</span><span class="sxs-lookup"><span data-stu-id="256b7-252">*AAD-UserReadUsingAlternativeSecurityId-NoError*</span></span> | <span data-ttu-id="256b7-253">A közösségi bejelentkezések során műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-253">Technical profile for social logins</span></span> |
| <span data-ttu-id="256b7-254">*Az AAD-UserWritePasswordUsingLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="256b7-254">*AAD-UserWritePasswordUsingLogonEmail*</span></span> | <span data-ttu-id="256b7-255">A helyi fiókok műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-255">Technical profile for local accounts</span></span> |
| <span data-ttu-id="256b7-256">*Az AAD-UserReadUsingEmailAddress*</span><span class="sxs-lookup"><span data-stu-id="256b7-256">*AAD-UserReadUsingEmailAddress*</span></span> | <span data-ttu-id="256b7-257">A helyi fiókok műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-257">Technical profile for local accounts</span></span> |
| <span data-ttu-id="256b7-258">*Az AAD-UserWriteProfileUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="256b7-258">*AAD-UserWriteProfileUsingObjectId*</span></span> | <span data-ttu-id="256b7-259">Műszaki profil használatával objectId felhasználói rekord frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="256b7-259">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="256b7-260">*Az AAD-UserWritePhoneNumberUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="256b7-260">*AAD-UserWritePhoneNumberUsingObjectId*</span></span> | <span data-ttu-id="256b7-261">Műszaki profil használatával objectId felhasználói rekord frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="256b7-261">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="256b7-262">*Az AAD-UserWritePasswordUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="256b7-262">*AAD-UserWritePasswordUsingObjectId*</span></span> | <span data-ttu-id="256b7-263">Műszaki profil használatával objectId felhasználói rekord frissítéséhez</span><span class="sxs-lookup"><span data-stu-id="256b7-263">Technical profile for updating user record using objectId</span></span> |
| <span data-ttu-id="256b7-264">*Az AAD-UserReadUsingObjectId*</span><span class="sxs-lookup"><span data-stu-id="256b7-264">*AAD-UserReadUsingObjectId*</span></span> | <span data-ttu-id="256b7-265">Műszaki profil az használt tooread adatai után a felhasználók hitelesítése</span><span class="sxs-lookup"><span data-stu-id="256b7-265">Technical profile is used tooread data after user authenticates</span></span> |

### <a name="technical-profiles-for-self-asserted"></a><span data-ttu-id="256b7-266">Az önkiszolgáló magas műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-266">Technical profiles for Self Asserted</span></span>

| <span data-ttu-id="256b7-267">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-267">Technical profile</span></span> | <span data-ttu-id="256b7-268">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-268">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-269">*SelfAsserted-társadalombiztosítási*</span><span class="sxs-lookup"><span data-stu-id="256b7-269">*SelfAsserted-Social*</span></span> | |
| <span data-ttu-id="256b7-270">*SelfAsserted-ProfileUpdate*</span><span class="sxs-lookup"><span data-stu-id="256b7-270">*SelfAsserted-ProfileUpdate*</span></span> | |

### <a name="technical-profiles-for-local-account"></a><span data-ttu-id="256b7-271">Helyi fiók műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-271">Technical profiles for Local Account</span></span>

| <span data-ttu-id="256b7-272">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-272">Technical profile</span></span> | <span data-ttu-id="256b7-273">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-273">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-274">*LocalAccountSignUpWithLogonEmail*</span><span class="sxs-lookup"><span data-stu-id="256b7-274">*LocalAccountSignUpWithLogonEmail*</span></span> | |

### <a name="technical-profiles-for-session-management"></a><span data-ttu-id="256b7-275">Műszaki profilokat a munkamenet-kezelés</span><span class="sxs-lookup"><span data-stu-id="256b7-275">Technical profiles for Session Management</span></span>

| <span data-ttu-id="256b7-276">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-276">Technical profile</span></span> | <span data-ttu-id="256b7-277">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-277">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-278">*SM-Noop*</span><span class="sxs-lookup"><span data-stu-id="256b7-278">*SM-Noop*</span></span> | |
| <span data-ttu-id="256b7-279">*SM-AAD-BEN*</span><span class="sxs-lookup"><span data-stu-id="256b7-279">*SM-AAD*</span></span> | |
| <span data-ttu-id="256b7-280">*SM-SocialSignup*</span><span class="sxs-lookup"><span data-stu-id="256b7-280">*SM-SocialSignup*</span></span> | <span data-ttu-id="256b7-281">Profil neve alatt használt toodisambiguate AAD munkamenet között bejelentkezési fel, és jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="256b7-281">Profile name is being used toodisambiguate AAD session between sign up and sign in</span></span> |
| <span data-ttu-id="256b7-282">*SM-SocialLogin*</span><span class="sxs-lookup"><span data-stu-id="256b7-282">*SM-SocialLogin*</span></span> | |
| <span data-ttu-id="256b7-283">*SM-MFA*</span><span class="sxs-lookup"><span data-stu-id="256b7-283">*SM-MFA*</span></span> | |

### <a name="technical-profiles-for-trustframework-policy-engine-technicalprofiles"></a><span data-ttu-id="256b7-284">Trustframework házirend motor TechnicalProfiles műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-284">Technical profiles for Trustframework Policy Engine TechnicalProfiles</span></span>

<span data-ttu-id="256b7-285">Jelenleg nincsenek technikai profilok hello definiált **Trustframework házirend motor TechnicalProfiles** jogcím-szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="256b7-285">Currently, no technical profiles are defined for hello **Trustframework Policy Engine TechnicalProfiles** claims provider.</span></span>

### <a name="technical-profiles-for-token-issuer"></a><span data-ttu-id="256b7-286">Jogkivonatot kibocsátó műszaki profilok</span><span class="sxs-lookup"><span data-stu-id="256b7-286">Technical profiles for Token Issuer</span></span>

| <span data-ttu-id="256b7-287">Műszaki profil</span><span class="sxs-lookup"><span data-stu-id="256b7-287">Technical profile</span></span> | <span data-ttu-id="256b7-288">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-288">Description</span></span> |
|-------------------|-------------|
| <span data-ttu-id="256b7-289">*JwtIssuer*</span><span class="sxs-lookup"><span data-stu-id="256b7-289">*JwtIssuer*</span></span> | |

## <a name="user-journeys"></a><span data-ttu-id="256b7-290">Felhasználói utak</span><span class="sxs-lookup"><span data-stu-id="256b7-290">User journeys</span></span>

<span data-ttu-id="256b7-291">Ez a szakasz ábrázol hello felhasználói utak hello már deklarálva *B2C_1A_base* házirend.</span><span class="sxs-lookup"><span data-stu-id="256b7-291">This section depicts hello user journeys already declared in hello *B2C_1A_base* policy.</span></span> <span data-ttu-id="256b7-292">Ezek az felhasználói utak téve toobe további hivatkozott, felül, illetve szükség esetén hello mellett a saját a házirendeket, valamint a kiterjesztett *B2C_1A_base_extensions* házirend.</span><span class="sxs-lookup"><span data-stu-id="256b7-292">These user journeys are susceptible toobe further referenced, overridden, and/or extended as needed in your own policies as well as in hello *B2C_1A_base_extensions* policy.</span></span>

| <span data-ttu-id="256b7-293">Felhasználói út</span><span class="sxs-lookup"><span data-stu-id="256b7-293">User journey</span></span> | <span data-ttu-id="256b7-294">Leírás</span><span class="sxs-lookup"><span data-stu-id="256b7-294">Description</span></span> |
|--------------|-------------|
| <span data-ttu-id="256b7-295">*Regisztráció*</span><span class="sxs-lookup"><span data-stu-id="256b7-295">*SignUp*</span></span> | |
| <span data-ttu-id="256b7-296">*Bejelentkezés*</span><span class="sxs-lookup"><span data-stu-id="256b7-296">*SignIn*</span></span> | |
| <span data-ttu-id="256b7-297">*SignUpOrSignIn*</span><span class="sxs-lookup"><span data-stu-id="256b7-297">*SignUpOrSignIn*</span></span> | |
| <span data-ttu-id="256b7-298">*EditProfile*</span><span class="sxs-lookup"><span data-stu-id="256b7-298">*EditProfile*</span></span> | |
| <span data-ttu-id="256b7-299">*PasswordReset*</span><span class="sxs-lookup"><span data-stu-id="256b7-299">*PasswordReset*</span></span> | |

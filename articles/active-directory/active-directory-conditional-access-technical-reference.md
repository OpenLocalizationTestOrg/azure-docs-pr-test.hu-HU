---
title: "technikai útmutató az Active Directory feltételes hozzáférés aaaAzure |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi hello megadott feltételek hello felhasználói hitelesítés során, és mielőtt engedélyezi a hozzáférést toohello alkalmazás kiválasztása. Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve."
services: active-directory.
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 56a5bade-7dcc-4dcf-8092-a7d4bf5df3c1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/22/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: ee201405d1d17f130607a95bf455b60cd222dd0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a><span data-ttu-id="cc2af-104">Az Azure Active Directory feltételes hozzáférést biztosító műszaki útmutatója</span><span class="sxs-lookup"><span data-stu-id="cc2af-104">Azure Active Directory Conditional Access technical reference</span></span>

## <a name="services-enabled-with-conditional-access"></a><span data-ttu-id="cc2af-105">Szolgáltatás engedélyezve van a feltételes hozzáféréssel</span><span class="sxs-lookup"><span data-stu-id="cc2af-105">Services enabled with conditional access</span></span>

<span data-ttu-id="cc2af-106">Feltételes hozzáférési szabályok több Azure AD-alkalmazás különféle is támogatott.</span><span class="sxs-lookup"><span data-stu-id="cc2af-106">Conditional Access rules are supported across various Azure AD application types.</span></span> <span data-ttu-id="cc2af-107">A lista a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="cc2af-107">This list includes:</span></span>


* <span data-ttu-id="cc2af-108">Hello Azure Application Proxy regisztrált alkalmazások</span><span class="sxs-lookup"><span data-stu-id="cc2af-108">Applications registered with hello Azure Application Proxy</span></span>
* <span data-ttu-id="cc2af-109">Azure távoli alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cc2af-109">Azure Remote App</span></span>
* <span data-ttu-id="cc2af-110">Fejlett üzletági és az Azure ad-vel regisztrált több-bérlős alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="cc2af-110">Developed line of business and multi-tenant applications registered with Azure AD</span></span>
* <span data-ttu-id="cc2af-111">Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="cc2af-111">Dynamics CRM</span></span>
* <span data-ttu-id="cc2af-112">Összevont alkalmazásokhoz hello Azure AD alkalmazás-galériából</span><span class="sxs-lookup"><span data-stu-id="cc2af-112">Federated applications from hello Azure AD application gallery</span></span>
* <span data-ttu-id="cc2af-113">A Microsoft Office 365 Yammer</span><span class="sxs-lookup"><span data-stu-id="cc2af-113">Microsoft Office 365 Yammer</span></span>
* <span data-ttu-id="cc2af-114">A Microsoft Office 365 Exchange online-hoz</span><span class="sxs-lookup"><span data-stu-id="cc2af-114">Microsoft Office 365 Exchange Online</span></span>
* <span data-ttu-id="cc2af-115">A Microsoft Office 365 SharePoint online-hoz (tartalmazza a onedrive vállalati verzió)</span><span class="sxs-lookup"><span data-stu-id="cc2af-115">Microsoft Office 365 SharePoint Online (includes OneDrive for Business)</span></span>
* <span data-ttu-id="cc2af-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="cc2af-116">Microsoft Power BI</span></span> 
* <span data-ttu-id="cc2af-117">Jelszó SSO alkalmazások hello Azure AD alkalmazás-galériából</span><span class="sxs-lookup"><span data-stu-id="cc2af-117">Password SSO applications from hello Azure AD application gallery</span></span>
* <span data-ttu-id="cc2af-118">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="cc2af-118">Visual Studio Team Services</span></span>
* <span data-ttu-id="cc2af-119">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="cc2af-119">Microsoft Teams</span></span>









## <a name="enable-access-rules"></a><span data-ttu-id="cc2af-120">Engedélyezze a hozzáférési szabályok</span><span class="sxs-lookup"><span data-stu-id="cc2af-120">Enable access rules</span></span>
<span data-ttu-id="cc2af-121">Minden egyes szabály is engedélyezhető vagy letiltható a egy alkalmazás körrel száma.</span><span class="sxs-lookup"><span data-stu-id="cc2af-121">Each rule can be enabled or disabled on a per application bases.</span></span> <span data-ttu-id="cc2af-122">Amikor a szabályok lettek **ON** azok engedélyezve lesz, és érvényesíti a hello alkalmazás elérő felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="cc2af-122">When rules are **ON** they will be enabled and enforced for users accessing hello application.</span></span> <span data-ttu-id="cc2af-123">Ha vannak **OFF** nem lesz és fog nem gyakorolt hatás hello jelentkeznek élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="cc2af-123">When they are **OFF** they will not be used and will not impact hello users sign in experience.</span></span>

## <a name="applying-rules-toospecific-users"></a><span data-ttu-id="cc2af-124">Szabályok toospecific felhasználók alkalmazása</span><span class="sxs-lookup"><span data-stu-id="cc2af-124">Applying rules toospecific users</span></span>
<span data-ttu-id="cc2af-125">A szabályok lehetnek úgy, hogy a biztonsági csoport alapján alkalmazott toospecific felhasználócsoportokhoz **érvényes**.</span><span class="sxs-lookup"><span data-stu-id="cc2af-125">Rules can be applied toospecific sets of users based on security group by setting **Apply To**.</span></span> <span data-ttu-id="cc2af-126">**Érvényes** túl beállítható**minden felhasználó** vagy **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cc2af-126">**Apply To** can be set too**All Users** or **Groups**.</span></span> <span data-ttu-id="cc2af-127">Ha értéke túl**minden felhasználó** hello szabályokat alkalmazza tooany felhasználói hozzáférés toohello alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="cc2af-127">When set too**All Users** hello rules will apply tooany user with access toohello application.</span></span> <span data-ttu-id="cc2af-128">Hello **csoportok** beállítás lehetővé teszi, hogy bizonyos biztonsági és terjesztési csoportok toobe kijelölve, csak sikertelenek lehetnek ezekhez a csoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="cc2af-128">hello **Groups** option allows specific security and distribution groups toobe selected, rules will only be enforced for these groups.</span></span>

<span data-ttu-id="cc2af-129">Szabály való telepítése esetén közös toofirst alkalmazhatja azt csak bizonyos felhasználók, a tesztelés csoportok tagjai.</span><span class="sxs-lookup"><span data-stu-id="cc2af-129">When deploying a rule,  it is common toofirst apply it a limited set of users, that are members of a piloting groups.</span></span> <span data-ttu-id="cc2af-130">Miután a teljes hello szabály túl is alkalmazható**minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cc2af-130">Once complete hello rule can be applied too**All Users**.</span></span> <span data-ttu-id="cc2af-131">Ennek hatására hello szabály toobe érvényesíti a hello szervezet összes felhasználója számára.</span><span class="sxs-lookup"><span data-stu-id="cc2af-131">This will cause hello rule toobe enforced for all users in hello organization.</span></span>

<span data-ttu-id="cc2af-132">Csoportok is mentesíthetők hello használatával **kivéve** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cc2af-132">Select groups may also be exempted from policy using hello **Except** option.</span></span> <span data-ttu-id="cc2af-133">Ezeket a csoportokat tagok fog kell sorolni, még akkor is, ha egy befoglalt csoportban szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="cc2af-133">Any members of these groups will be exempted even if they appear in an included group.</span></span>

## <a name="at-work-networks"></a><span data-ttu-id="cc2af-134">"Munkahelyi" hálózatok</span><span class="sxs-lookup"><span data-stu-id="cc2af-134">“At work” networks</span></span>
<span data-ttu-id="cc2af-135">Feltételes hozzáférési szabályok, amelyek egy "munkahelyi" hálózati támaszkodnak az Azure Active Directoryban beállított megbízható IP-címtartományok vagy a "belső vállalati hálózat" hello jogcímek az AD FS.</span><span class="sxs-lookup"><span data-stu-id="cc2af-135">Conditional access rules that use an “At work” network, rely on trusted IP address ranges that have been configured in Azure AD, or use of hello "inside corpnet" claim from AD FS.</span></span> <span data-ttu-id="cc2af-136">A rendeletek az alábbiakat tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="cc2af-136">These rules include:</span></span>

* <span data-ttu-id="cc2af-137">Ha nem munkahelyi többtényezős hitelesítést</span><span class="sxs-lookup"><span data-stu-id="cc2af-137">Require multi-factor authentication when not at work</span></span>
* <span data-ttu-id="cc2af-138">Letiltja a hozzáférést, ha nem a munkahelyi hálózatban</span><span class="sxs-lookup"><span data-stu-id="cc2af-138">Block access when not at work</span></span>

<span data-ttu-id="cc2af-139">LDAP beállításai "munkahelyi" hálózatok</span><span class="sxs-lookup"><span data-stu-id="cc2af-139">Options for specifiying “at work” networks</span></span>

1. <span data-ttu-id="cc2af-140">Konfigurálja a megbízható IP-címtartományok hello [többtényezős hitelesítés konfigurálása lapon](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="cc2af-140">Configure trusted IP address ranges in hello [multi-factor authentication configuration page](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span></span> <span data-ttu-id="cc2af-141">Feltételes hozzáférési házirend minden egyes hitelesítési kérelmek és -tokennel kiállítási tooevaluate szabályok konfigurált hello címtartományokat fogja használni.</span><span class="sxs-lookup"><span data-stu-id="cc2af-141">Conditional Access policy will use hello configured ranges on each authentication request and token issuance tooevaluate rules.</span></span> 
2. <span data-ttu-id="cc2af-142">Corpnet jogcím belül hello konfigurálhatja, ez a beállítás használható összevont könyvtárak, használja az AD FS szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cc2af-142">Configure use of hello inside corpnet claim, this option can be used with federated directories, using AD FS.</span></span> <span data-ttu-id="cc2af-143">toolearn belül corpnet jogcímek hello kapcsolatos további információkért lásd: [Tusted IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="cc2af-143">toolearn more about hello inside corpnet claims, see [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="rules-based-on-application-sensitivity"></a><span data-ttu-id="cc2af-144">Alkalmazás érzékenysége alapján</span><span class="sxs-lookup"><span data-stu-id="cc2af-144">Rules based on application sensitivity</span></span>
<span data-ttu-id="cc2af-145">A szabályok konfigurálva vannak, így hello értékes szolgáltatások toobe tooother szolgáltatást befolyásolása nélkül védett alkalmazásonként.</span><span class="sxs-lookup"><span data-stu-id="cc2af-145">Rules are configured per application allowing hello high value services toobe secured without impacting access tooother services.</span></span> <span data-ttu-id="cc2af-146">Feltételes hozzáférési szabályai lehet megadni a hello **konfigurálása** hello alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="cc2af-146">Conditional access rules can be configured on hello  **Configure** tab of hello application.</span></span> 

<span data-ttu-id="cc2af-147">A szabályok jelenleg érhető el:</span><span class="sxs-lookup"><span data-stu-id="cc2af-147">Rules currently offered:</span></span>

* <span data-ttu-id="cc2af-148">**Többtényezős hitelesítés megkövetelése**</span><span class="sxs-lookup"><span data-stu-id="cc2af-148">**Require multi-factor authentication**</span></span>
  
  * <span data-ttu-id="cc2af-149">Minden felhasználó, hogy ez a házirend-e az alkalmazott toowill szükséges tooauthenticate legalább egyszer a többtényezős hitelesítéssel kell.</span><span class="sxs-lookup"><span data-stu-id="cc2af-149">All users that this policy is applied toowill be required tooauthenticate via multi-factor authentication at least once.</span></span>
* <span data-ttu-id="cc2af-150">**Ha nem munkahelyi többtényezős hitelesítést**</span><span class="sxs-lookup"><span data-stu-id="cc2af-150">**Require multi-factor authentication when not at work**</span></span>
  
  * <span data-ttu-id="cc2af-151">Ha a házirend érvényben van, minden felhasználó legalább egyszer végre szükséges toohave a multi-factor authentication lesz, ha munkaidőn kívüli távolról elérni azok hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cc2af-151">If this policy is applied, all users will be required toohave performed multi-factor authentication at least once if they access hello service from a non-work remote location.</span></span> <span data-ttu-id="cc2af-152">Ha munkahelyi tooremote helyről helyezi át, fogják szükséges tooperform többtényezős hitelesítést, hello szolgáltatás elérésekor.</span><span class="sxs-lookup"><span data-stu-id="cc2af-152">If they move from a work tooremote location, they will be required tooperform multifactor authentication when accessing hello service.</span></span>
* <span data-ttu-id="cc2af-153">**Letiltja a hozzáférést, ha nem a munkahelyi hálózatban**</span><span class="sxs-lookup"><span data-stu-id="cc2af-153">**Block access when not at work**</span></span> 
  
  * <span data-ttu-id="cc2af-154">Ha a felhasználók munkahelyi tooa távoli helyről, le lesznek tiltva Ha hello "Letiltja a hozzáférést, ha nem munkahelyi" házirend alkalmazott toothem.</span><span class="sxs-lookup"><span data-stu-id="cc2af-154">When users move from work tooa remote location, they will be blocked if hello "Block access when not at work" policy is applied toothem.</span></span>  <span data-ttu-id="cc2af-155">Ezek újra kapnak hozzáférést, a munkahelyi helyre.</span><span class="sxs-lookup"><span data-stu-id="cc2af-155">They will be re-allowed access when at a work location.</span></span>

## <a name="related-topics"></a><span data-ttu-id="cc2af-156">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="cc2af-156">Related topics</span></span>
* [<span data-ttu-id="cc2af-157">Az Active Directory tooAzure tooOffice 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele csatlakoztatva</span><span class="sxs-lookup"><span data-stu-id="cc2af-157">Securing access tooOffice 365 and other apps connected tooAzure Active Directory</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="cc2af-158">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="cc2af-158">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)


---
title: "Az Azure Active Directory feltételes hozzáférést biztosító műszaki útmutatója |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlést az Azure Active Directory ellenőrzi a megadott feltételek, ha a felhasználó hitelesítése és az alkalmazáshoz való hozzáférés előtt válasszon. Ha ezek a feltételek teljesülnek, a felhasználó hitelesítése és hozzáférni az alkalmazáshoz engedélyezett."
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
ms.openlocfilehash: ca16a5399f94fd1ab267e0798cade3fd83f75b13
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-conditional-access-technical-reference"></a><span data-ttu-id="882ca-104">Az Azure Active Directory feltételes hozzáférést biztosító műszaki útmutatója</span><span class="sxs-lookup"><span data-stu-id="882ca-104">Azure Active Directory Conditional Access technical reference</span></span>

## <a name="services-enabled-with-conditional-access"></a><span data-ttu-id="882ca-105">Szolgáltatás engedélyezve van a feltételes hozzáféréssel</span><span class="sxs-lookup"><span data-stu-id="882ca-105">Services enabled with conditional access</span></span>

<span data-ttu-id="882ca-106">Feltételes hozzáférési szabályok több Azure AD-alkalmazás különféle is támogatott.</span><span class="sxs-lookup"><span data-stu-id="882ca-106">Conditional Access rules are supported across various Azure AD application types.</span></span> <span data-ttu-id="882ca-107">A lista a következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="882ca-107">This list includes:</span></span>


* <span data-ttu-id="882ca-108">Az Azure alkalmazásproxyval regisztrált alkalmazások</span><span class="sxs-lookup"><span data-stu-id="882ca-108">Applications registered with the Azure Application Proxy</span></span>
* <span data-ttu-id="882ca-109">Azure távoli alkalmazás</span><span class="sxs-lookup"><span data-stu-id="882ca-109">Azure Remote App</span></span>
* <span data-ttu-id="882ca-110">Fejlett üzletági és az Azure ad-vel regisztrált több-bérlős alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="882ca-110">Developed line of business and multi-tenant applications registered with Azure AD</span></span>
* <span data-ttu-id="882ca-111">Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="882ca-111">Dynamics CRM</span></span>
* <span data-ttu-id="882ca-112">Összevont alkalmazásokhoz az Azure AD alkalmazás-galériából</span><span class="sxs-lookup"><span data-stu-id="882ca-112">Federated applications from the Azure AD application gallery</span></span>
* <span data-ttu-id="882ca-113">A Microsoft Office 365 Yammer</span><span class="sxs-lookup"><span data-stu-id="882ca-113">Microsoft Office 365 Yammer</span></span>
* <span data-ttu-id="882ca-114">A Microsoft Office 365 Exchange online-hoz</span><span class="sxs-lookup"><span data-stu-id="882ca-114">Microsoft Office 365 Exchange Online</span></span>
* <span data-ttu-id="882ca-115">A Microsoft Office 365 SharePoint online-hoz (tartalmazza a onedrive vállalati verzió)</span><span class="sxs-lookup"><span data-stu-id="882ca-115">Microsoft Office 365 SharePoint Online (includes OneDrive for Business)</span></span>
* <span data-ttu-id="882ca-116">Microsoft Power BI</span><span class="sxs-lookup"><span data-stu-id="882ca-116">Microsoft Power BI</span></span> 
* <span data-ttu-id="882ca-117">Jelszó SSO alkalmazások az Azure AD alkalmazás-galériából</span><span class="sxs-lookup"><span data-stu-id="882ca-117">Password SSO applications from the Azure AD application gallery</span></span>
* <span data-ttu-id="882ca-118">Visual Studio Team Services</span><span class="sxs-lookup"><span data-stu-id="882ca-118">Visual Studio Team Services</span></span>
* <span data-ttu-id="882ca-119">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="882ca-119">Microsoft Teams</span></span>









## <a name="enable-access-rules"></a><span data-ttu-id="882ca-120">Engedélyezze a hozzáférési szabályok</span><span class="sxs-lookup"><span data-stu-id="882ca-120">Enable access rules</span></span>
<span data-ttu-id="882ca-121">Minden egyes szabály is engedélyezhető vagy letiltható a egy alkalmazás körrel száma.</span><span class="sxs-lookup"><span data-stu-id="882ca-121">Each rule can be enabled or disabled on a per application bases.</span></span> <span data-ttu-id="882ca-122">Amikor a szabályok lettek **ON** azok engedélyezve lesz, és a felhasználók számára az alkalmazás elérésének érvényesíti.</span><span class="sxs-lookup"><span data-stu-id="882ca-122">When rules are **ON** they will be enabled and enforced for users accessing the application.</span></span> <span data-ttu-id="882ca-123">Ha vannak **OFF** nem fogja használni, és nem befolyásolja a felhasználók bejelentkezési élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="882ca-123">When they are **OFF** they will not be used and will not impact the users sign in experience.</span></span>

## <a name="applying-rules-to-specific-users"></a><span data-ttu-id="882ca-124">Szabályok alkalmazása adott felhasználókra</span><span class="sxs-lookup"><span data-stu-id="882ca-124">Applying rules to specific users</span></span>
<span data-ttu-id="882ca-125">Szabályok alkalmazhatók a biztonsági csoport alapján úgy, hogy a felhasználók adott részhalmazához **érvényes**.</span><span class="sxs-lookup"><span data-stu-id="882ca-125">Rules can be applied to specific sets of users based on security group by setting **Apply To**.</span></span> <span data-ttu-id="882ca-126">**Érvényes** megadható **minden felhasználó** vagy **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="882ca-126">**Apply To** can be set to **All Users** or **Groups**.</span></span> <span data-ttu-id="882ca-127">Ha beállítása **minden felhasználó** szabályok minden olyan felhasználó, az alkalmazás települ.</span><span class="sxs-lookup"><span data-stu-id="882ca-127">When set to **All Users** the rules will apply to any user with access to the application.</span></span> <span data-ttu-id="882ca-128">A **csoportok** beállítással meghatározott biztonsági és terjesztési csoportot ki kell választani, csak sikertelenek lehetnek ezekhez a csoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="882ca-128">The **Groups** option allows specific security and distribution groups to be selected, rules will only be enforced for these groups.</span></span>

<span data-ttu-id="882ca-129">Egy szabály telepítésekor esetében gyakori, először alkalmazni a korlátozott állítva, a felhasználók, amelyek a tesztelés csoportok tagjai.</span><span class="sxs-lookup"><span data-stu-id="882ca-129">When deploying a rule,  it is common to first apply it a limited set of users, that are members of a piloting groups.</span></span> <span data-ttu-id="882ca-130">Miután befejeződött a szabály is alkalmazható **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="882ca-130">Once complete the rule can be applied to **All Users**.</span></span> <span data-ttu-id="882ca-131">Ennek hatására a szabály, amely érvényesíti a szervezet összes felhasználója számára.</span><span class="sxs-lookup"><span data-stu-id="882ca-131">This will cause the rule to be enforced for all users in the organization.</span></span>

<span data-ttu-id="882ca-132">Csoportok is mentesíthetők házirendekkel a **kivéve** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="882ca-132">Select groups may also be exempted from policy using the **Except** option.</span></span> <span data-ttu-id="882ca-133">Ezeket a csoportokat tagok fog kell sorolni, még akkor is, ha egy befoglalt csoportban szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="882ca-133">Any members of these groups will be exempted even if they appear in an included group.</span></span>

## <a name="at-work-networks"></a><span data-ttu-id="882ca-134">"Munkahelyi" hálózatok</span><span class="sxs-lookup"><span data-stu-id="882ca-134">“At work” networks</span></span>
<span data-ttu-id="882ca-135">Feltételes hozzáférési szabályok, amelyek egy "munkahelyi" hálózati támaszkodnak az Azure Active Directoryban beállított megbízható IP-címtartományok vagy a "belső vállalati hálózat" jogcímek az AD FS használatára.</span><span class="sxs-lookup"><span data-stu-id="882ca-135">Conditional access rules that use an “At work” network, rely on trusted IP address ranges that have been configured in Azure AD, or use of the "inside corpnet" claim from AD FS.</span></span> <span data-ttu-id="882ca-136">A rendeletek az alábbiakat tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="882ca-136">These rules include:</span></span>

* <span data-ttu-id="882ca-137">Ha nem munkahelyi többtényezős hitelesítést</span><span class="sxs-lookup"><span data-stu-id="882ca-137">Require multi-factor authentication when not at work</span></span>
* <span data-ttu-id="882ca-138">Letiltja a hozzáférést, ha nem a munkahelyi hálózatban</span><span class="sxs-lookup"><span data-stu-id="882ca-138">Block access when not at work</span></span>

<span data-ttu-id="882ca-139">LDAP beállításai "munkahelyi" hálózatok</span><span class="sxs-lookup"><span data-stu-id="882ca-139">Options for specifiying “at work” networks</span></span>

1. <span data-ttu-id="882ca-140">Konfigurálja a megbízható IP-címtartományok a [többtényezős hitelesítés konfigurálása lapon](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span><span class="sxs-lookup"><span data-stu-id="882ca-140">Configure trusted IP address ranges in the [multi-factor authentication configuration page](../multi-factor-authentication/multi-factor-authentication-whats-next.md).</span></span> <span data-ttu-id="882ca-141">Feltételes hozzáférési házirend használatával fogja a beállított tartomány minden egyes hitelesítési kérelmek és a hitelesítési karakterlánc-kiállítási szabályok kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="882ca-141">Conditional Access policy will use the configured ranges on each authentication request and token issuance to evaluate rules.</span></span> 
2. <span data-ttu-id="882ca-142">Konfigurálhatja a belső vállalati hálózat jogcím, ez a beállítás használható összevont könyvtárak, használja az AD FS szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="882ca-142">Configure use of the inside corpnet claim, this option can be used with federated directories, using AD FS.</span></span> <span data-ttu-id="882ca-143">További információ a belső vállalati hálózat jogcímeket, lásd: [Tusted IP-címek](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span><span class="sxs-lookup"><span data-stu-id="882ca-143">To learn more about the inside corpnet claims, see [Tusted IPs](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips).</span></span>


## <a name="rules-based-on-application-sensitivity"></a><span data-ttu-id="882ca-144">Alkalmazás érzékenysége alapján</span><span class="sxs-lookup"><span data-stu-id="882ca-144">Rules based on application sensitivity</span></span>
<span data-ttu-id="882ca-145">A szabályok konfigurálva vannak alkalmazásonként engedélyezése a nagy értékű szolgáltatásokat érintő egyéb szolgáltatásokhoz való hozzáférés nélkül védeni.</span><span class="sxs-lookup"><span data-stu-id="882ca-145">Rules are configured per application allowing the high value services to be secured without impacting access to other services.</span></span> <span data-ttu-id="882ca-146">Feltételes hozzáférési szabályai konfigurálhatja a **konfigurálása** az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="882ca-146">Conditional access rules can be configured on the  **Configure** tab of the application.</span></span> 

<span data-ttu-id="882ca-147">A szabályok jelenleg érhető el:</span><span class="sxs-lookup"><span data-stu-id="882ca-147">Rules currently offered:</span></span>

* <span data-ttu-id="882ca-148">**Többtényezős hitelesítés megkövetelése**</span><span class="sxs-lookup"><span data-stu-id="882ca-148">**Require multi-factor authentication**</span></span>
  
  * <span data-ttu-id="882ca-149">Ez a házirend vonatkozik az összes felhasználó legalább egyszer a többtényezős hitelesítéssel hitelesíteni kell.</span><span class="sxs-lookup"><span data-stu-id="882ca-149">All users that this policy is applied to will be required to authenticate via multi-factor authentication at least once.</span></span>
* <span data-ttu-id="882ca-150">**Ha nem munkahelyi többtényezős hitelesítést**</span><span class="sxs-lookup"><span data-stu-id="882ca-150">**Require multi-factor authentication when not at work**</span></span>
  
  * <span data-ttu-id="882ca-151">Ha a házirend érvényben van, minden felhasználó lesz szükség legalább egy alkalommal a multi-factor authentication végeztek, ha azok munkaidőn kívüli távolról elérni a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="882ca-151">If this policy is applied, all users will be required to have performed multi-factor authentication at least once if they access the service from a non-work remote location.</span></span> <span data-ttu-id="882ca-152">Ha azok áthelyezése egy távoli helyre, akkor le kell többtényezős hitelesítést végezni, ha a szolgáltatás elérésével.</span><span class="sxs-lookup"><span data-stu-id="882ca-152">If they move from a work to remote location, they will be required to perform multifactor authentication when accessing the service.</span></span>
* <span data-ttu-id="882ca-153">**Letiltja a hozzáférést, ha nem a munkahelyi hálózatban**</span><span class="sxs-lookup"><span data-stu-id="882ca-153">**Block access when not at work**</span></span> 
  
  * <span data-ttu-id="882ca-154">Ha a felhasználók a munkahelyen egy távoli helyre, le lesznek tiltva Ha a "Letiltja a hozzáférést, ha nem munkahelyi" házirend alkalmazását.</span><span class="sxs-lookup"><span data-stu-id="882ca-154">When users move from work to a remote location, they will be blocked if the "Block access when not at work" policy is applied to them.</span></span>  <span data-ttu-id="882ca-155">Ezek újra kapnak hozzáférést, a munkahelyi helyre.</span><span class="sxs-lookup"><span data-stu-id="882ca-155">They will be re-allowed access when at a work location.</span></span>

## <a name="related-topics"></a><span data-ttu-id="882ca-156">Kapcsolódó témakörök</span><span class="sxs-lookup"><span data-stu-id="882ca-156">Related topics</span></span>
* [<span data-ttu-id="882ca-157">Az Azure Active Directoryhoz csatlakoztatott Office 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele</span><span class="sxs-lookup"><span data-stu-id="882ca-157">Securing access to Office 365 and other apps connected to Azure Active Directory</span></span>](active-directory-conditional-access.md)
* [<span data-ttu-id="882ca-158">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="882ca-158">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)


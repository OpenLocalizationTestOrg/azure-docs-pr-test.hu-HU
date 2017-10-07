---
title: "a klasszikus Azure portálon hello aaaConditional hozzáférés |} Microsoft Docs"
description: "Feltételes hozzáférés-vezérlés az Azure klasszikus portál toocheck hello a megadott feltételek hitelesítéséhez használt a hozzáférési tooapplications."
services: active-directory
keywords: "feltételes hozzáférés tooapps, feltételes hozzáférés az Azure AD-vel biztonságos hozzáférés toocompany erőforrásokat, a feltételes hozzáférési házirendek"
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: da3f0a44-1399-4e0b-aefb-03a826ae4ead
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/07/2017
ms.author: markvi
ms.reviewer: calebb
ms.custom: oldportal
ms.openlocfilehash: 049bd3f6ec9e35479319ee7608b007b9a1e16647
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-in-hello-azure-classic-portal"></a><span data-ttu-id="73060-104">A klasszikus Azure portálon hello feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="73060-104">Conditional access in hello Azure classic portal</span></span>

<span data-ttu-id="73060-105">Ez a témakör tárgya feltételes hozzáférés a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="73060-105">This topic is about conditional access in hello Azure classic portal.</span></span> <span data-ttu-id="73060-106">Feltételes hozzáférés az Azure Active Directory hello információ legutóbbi hello,: [feltételes hozzáférés az Azure Active Directoryban](active-directory-conditional-access-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="73060-106">For hello most recent information about conditional access in hello Azure Active Directory, see [Conditional access in Azure Active Directory](active-directory-conditional-access-azure-portal.md).</span></span>


<span data-ttu-id="73060-107">hello vezérlés képességeinek az Azure Active Directory (Azure AD) feltételes hozzáférés ajánlatot egyszerű módon toohelp biztonságos hello felhőalapú és helyszíni erőforrások.</span><span class="sxs-lookup"><span data-stu-id="73060-107">hello control capabilities in Azure Active Directory (Azure AD) conditional access offer simple ways toohelp secure resources in hello cloud and on-premises.</span></span> <span data-ttu-id="73060-108">Feltételes hozzáférési szabályzatok, például többtényezős hitelesítés szemben biztosítanak védelmet hello kockázatát ellopják és phished hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="73060-108">Conditional access policies like multi-factor authentication can help protect against hello risk of stolen and phished credentials.</span></span> <span data-ttu-id="73060-109">Más feltételes hozzáférési házirendek segítségével a szervezet adatai biztonságát.</span><span class="sxs-lookup"><span data-stu-id="73060-109">Other conditional access policies can help keep your organization's data safe.</span></span> <span data-ttu-id="73060-110">Például továbbá toorequiring hitelesítő adatokat, lehetséges, hogy egy házirendet, hogy csak regisztrált eszközök esetében a mobileszköz-kezelési rendszerbe például a Microsoft Intune férhetnek hozzá a szervezet bizalmas szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="73060-110">For example, in addition toorequiring credentials, you might have a policy that only devices that are enrolled in a mobile device management system like Microsoft Intune can access your organization's sensitive services.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73060-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73060-111">Prerequisites</span></span>
<span data-ttu-id="73060-112">Azure AD feltételes hozzáférés csak a [Azure Active Directory Premium](http://www.microsoft.com/identity).</span><span class="sxs-lookup"><span data-stu-id="73060-112">Azure AD conditional access is a feature of [Azure Active Directory Premium](http://www.microsoft.com/identity).</span></span> <span data-ttu-id="73060-113">Minden felhasználó, aki hozzáfér a feltételes hozzáférési házirendeket alkalmaztak alkalmazások az Azure AD Premium licenccel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="73060-113">Each user who accesses an application that has conditional access policies applied must have an Azure AD Premium license.</span></span> <span data-ttu-id="73060-114">További tudnivalók a licenckövetelmények vonatkoznak [licenccel nem rendelkező felhasználó jelentés](https://aka.ms/utc5ix).</span><span class="sxs-lookup"><span data-stu-id="73060-114">You can learn more about license requirements in [Unlicensed user report](https://aka.ms/utc5ix).</span></span>

## <a name="how-is-conditional-access-control-enforced"></a><span data-ttu-id="73060-115">Hogyan kényszeríti ki a feltételes hozzáférés-vezérlést?</span><span class="sxs-lookup"><span data-stu-id="73060-115">How is conditional access control enforced?</span></span>
<span data-ttu-id="73060-116">Feltételes hozzáférés-vezérléssel helyen, az Azure AD hello adott feltételeket ellenőrzi állítja be a felhasználó tooaccess kérelmet.</span><span class="sxs-lookup"><span data-stu-id="73060-116">With conditional access control in place, Azure AD checks for hello specific conditions you set for a user tooaccess an application.</span></span> <span data-ttu-id="73060-117">Miután a hozzáférési követelmények teljesülnek, a hello felhasználó van hitelesítve, és hello alkalmazást érheti el.</span><span class="sxs-lookup"><span data-stu-id="73060-117">After access requirements are met, hello user is authenticated and can access hello application.</span></span>  

![Feltételes hozzáférés – áttekintés](./media/active-directory-conditional-access/conditionalaccess-overview.png)

## <a name="conditions"></a><span data-ttu-id="73060-119">Feltételek</span><span class="sxs-lookup"><span data-stu-id="73060-119">Conditions</span></span>
<span data-ttu-id="73060-120">Ezek azok a feltételeket, amelyek is megadhat a feltételes hozzáférési szabályzatot:</span><span class="sxs-lookup"><span data-stu-id="73060-120">These are conditions that you can include in a conditional access policy:</span></span>

* <span data-ttu-id="73060-121">**Csoporttagságát**.</span><span class="sxs-lookup"><span data-stu-id="73060-121">**Group membership**.</span></span> <span data-ttu-id="73060-122">A csoport tagsága alapján a felhasználói hozzáférés szabályozása.</span><span class="sxs-lookup"><span data-stu-id="73060-122">Control a user's access based on membership in a group.</span></span>
* <span data-ttu-id="73060-123">**Hely**.</span><span class="sxs-lookup"><span data-stu-id="73060-123">**Location**.</span></span> <span data-ttu-id="73060-124">Hello hely hello felhasználói tootrigger multi-factor Authentication használata, és a blokk-vezérlők használata, ha egy felhasználó nem megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="73060-124">Use hello location of hello user tootrigger multi-factor authentication, and use block controls when a user is not on a trusted network.</span></span>
* <span data-ttu-id="73060-125">**Eszközplatform**.</span><span class="sxs-lookup"><span data-stu-id="73060-125">**Device platform**.</span></span> <span data-ttu-id="73060-126">Használja a hello eszközplatform, például az iOS, Android, Windows Mobile vagy Windows-házirend alkalmazására vonatkozó feltétele.</span><span class="sxs-lookup"><span data-stu-id="73060-126">Use hello device platform, such as iOS, Android, Windows Mobile, or Windows, as a condition for applying policy.</span></span>
* <span data-ttu-id="73060-127">**Eszköz-kompatibilis**.</span><span class="sxs-lookup"><span data-stu-id="73060-127">**Device-enabled**.</span></span> <span data-ttu-id="73060-128">Az eszköz állapotát, hogy engedélyezve van vagy le van tiltva, érvényesítése eszköz házirend kiértékelése közben.</span><span class="sxs-lookup"><span data-stu-id="73060-128">Device state, whether enabled or disabled, is validated during device policy evaluation.</span></span> <span data-ttu-id="73060-129">Ha letilt egy elveszett vagy ellopott eszközt hello könyvtárban, akkor már nem felel meg házirend követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="73060-129">If you disable a lost or stolen device in hello directory, it can no longer satisfy policy requirements.</span></span>
* <span data-ttu-id="73060-130">**Be- és felhasználói kockázati**.</span><span class="sxs-lookup"><span data-stu-id="73060-130">**Sign-in and user risk**.</span></span> <span data-ttu-id="73060-131">Használhat [Azure AD Identity Protection](active-directory-identityprotection.md) a feltételes hozzáférési kockázat házirendekhez.</span><span class="sxs-lookup"><span data-stu-id="73060-131">You can use [Azure AD Identity Protection](active-directory-identityprotection.md) for conditional access risk policies.</span></span> <span data-ttu-id="73060-132">Feltételes hozzáférés kockázat házirendek segítenek, hogy a szervezet előzetes védelme a kockázati eseményekről és a szokatlan bejelentkezési tevékenység alapján.</span><span class="sxs-lookup"><span data-stu-id="73060-132">Conditional access risk policies help give your organization advance protection based on risk events and unusual sign-in activities.</span></span>

## <a name="controls"></a><span data-ttu-id="73060-133">Vezérlők</span><span class="sxs-lookup"><span data-stu-id="73060-133">Controls</span></span>
<span data-ttu-id="73060-134">Használható tooenforce feltételes hozzáférési házirend vezérlőelemek az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="73060-134">These are controls that you can use tooenforce a conditional access policy:</span></span>

* <span data-ttu-id="73060-135">**A multi-factor authentication**.</span><span class="sxs-lookup"><span data-stu-id="73060-135">**Multi-factor authentication**.</span></span> <span data-ttu-id="73060-136">Erős hitelesítés, a többtényezős hitelesítés megkövetelése</span><span class="sxs-lookup"><span data-stu-id="73060-136">You can require strong authentication through multi-factor authentication.</span></span> <span data-ttu-id="73060-137">A multi-factor authentication használható Azure multi-factor Authentication szolgáltatással vagy a helyszíni többtényezős hitelesítési szolgáltató, kombinálja az Active Directory összevonási szolgáltatások (AD FS) használatával.</span><span class="sxs-lookup"><span data-stu-id="73060-137">You can use multi-factor authentication with Azure Multi-Factor Authentication or by using an on-premises multi-factor authentication provider, combined with Active Directory Federation Services (AD FS).</span></span> <span data-ttu-id="73060-138">A multi-factor authentication segítségével erőforrások védelme a fent konfigurált egy jogosulatlan felhasználó, aki előfordulhat, hogy rendelkezik kiszolgálószoftvertől toohello érvényes felhasználó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="73060-138">Using multi-factor authentication helps protect resources from being accessed by an unauthorized user who might have gained access toohello credentials of a valid user.</span></span>
* <span data-ttu-id="73060-139">**Blokk**.</span><span class="sxs-lookup"><span data-stu-id="73060-139">**Block**.</span></span> <span data-ttu-id="73060-140">Feltételek, például a felhasználó helye tooblock felhasználói hozzáférés is alkalmazhatja.</span><span class="sxs-lookup"><span data-stu-id="73060-140">You can apply conditions like user location tooblock user access.</span></span> <span data-ttu-id="73060-141">Letilthatja például hozzáférést, ha egy felhasználó nem megbízható hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="73060-141">For example, you can block access when a user is not on a trusted network.</span></span>
* <span data-ttu-id="73060-142">**Az előírásoknak megfelelő eszközök**.</span><span class="sxs-lookup"><span data-stu-id="73060-142">**Compliant devices**.</span></span> <span data-ttu-id="73060-143">Feltételes hozzáférési házirendek hello eszköz szinten állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="73060-143">You can set conditional access policies at hello device level.</span></span> <span data-ttu-id="73060-144">Előfordulhat, hogy beállította egy házirendet, hogy csak a tartományhoz csatlakoztatott számítógépek vagy mobileszközök vannak léptetve egy mobileszköz-kezelési alkalmazás elérhessék a szervezeti erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="73060-144">You might set up a policy so that only computers that are domain-joined, or mobile devices that are enrolled in a mobile device management application, can access your organization's resources.</span></span> <span data-ttu-id="73060-145">Például használja az Intune toocheck az eszköz megfelelőségét, és jelentést készít az tooAzure kényszerített AD amikor hello felhasználó próbál tooaccess egy alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="73060-145">For example, you can use Intune toocheck device compliance, and then report it tooAzure AD for enforcement when hello user attempts tooaccess an application.</span></span> <span data-ttu-id="73060-146">Hogyan toouse Intune tooprotect alkalmazásokat és adatokat, lásd: tartalmaznak részletes útmutatást [alkalmazások és a Microsoft Intune-nal adatainak védelme](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="73060-146">For detailed guidance about how toouse Intune tooprotect apps and data, see [Protect apps and data with Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/protect-apps-and-data-with-microsoft-intune).</span></span> <span data-ttu-id="73060-147">Intune tooenforce adatvédelem elveszett vagy ellopott eszközök is használja.</span><span class="sxs-lookup"><span data-stu-id="73060-147">You also can use Intune tooenforce data protection for lost or stolen devices.</span></span> <span data-ttu-id="73060-148">További információkért lásd: [az adatok védelme teljes vagy szelektív törléssel a Microsoft Intune használatával](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).</span><span class="sxs-lookup"><span data-stu-id="73060-148">For more information, see [Help protect your data with full or selective wipe using Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune).</span></span>

## <a name="applications"></a><span data-ttu-id="73060-149">Alkalmazások</span><span class="sxs-lookup"><span data-stu-id="73060-149">Applications</span></span>
<span data-ttu-id="73060-150">Kényszerítheti a feltételes hozzáférési szabályzat hello alkalmazás szintjén.</span><span class="sxs-lookup"><span data-stu-id="73060-150">You can enforce a conditional access policy at hello application level.</span></span> <span data-ttu-id="73060-151">Állítsa be a hozzáférési szintek az alkalmazások és szolgáltatások hello felhőalapú vagy helyszíni.</span><span class="sxs-lookup"><span data-stu-id="73060-151">Set access levels for applications and services in hello cloud or on-premises.</span></span> <span data-ttu-id="73060-152">hello házirend alkalmazott közvetlenül toohello webhelyre vagy szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="73060-152">hello policy is applied directly toohello website or service.</span></span> <span data-ttu-id="73060-153">az access toohello böngésző és hello szolgáltatás elérő tooapplications hello szabályzat érvényes.</span><span class="sxs-lookup"><span data-stu-id="73060-153">hello policy is enforced for access toohello browser, and tooapplications that access hello service.</span></span>

## <a name="device-based-conditional-access"></a><span data-ttu-id="73060-154">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="73060-154">Device-based conditional access</span></span>
<span data-ttu-id="73060-155">Korlátozhatja a hozzáférést tooapplications eszközökről, amely regisztrálva van az Azure ad-val, és amelyek megfelelnek a megadott feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="73060-155">You can restrict access tooapplications from devices that are registered with Azure AD, and which meet specific conditions.</span></span> <span data-ttu-id="73060-156">Eszközalapú feltételes hozzáférési egy szervezet erőforrásaihoz kísérlet tooaccess hello erőforrások a felhasználók nem használhatják:</span><span class="sxs-lookup"><span data-stu-id="73060-156">Device-based conditional access protects an organization's resources from users who attempt tooaccess hello resources from:</span></span>

* <span data-ttu-id="73060-157">Ismeretlen vagy nem felügyelt eszközökön.</span><span class="sxs-lookup"><span data-stu-id="73060-157">Unknown or unmanaged devices.</span></span>
* <span data-ttu-id="73060-158">Azok az eszközök, amelyek nem felelnek meg hello biztonsági házirendek beállítása a szervezet.</span><span class="sxs-lookup"><span data-stu-id="73060-158">Devices that don’t meet hello security policies your organization set up.</span></span>

<span data-ttu-id="73060-159">Ezen követelmények alapján házirendek állíthatja be:</span><span class="sxs-lookup"><span data-stu-id="73060-159">You can set policies based on these requirements:</span></span>

* <span data-ttu-id="73060-160">**Tartományhoz csatlakozó eszközök**.</span><span class="sxs-lookup"><span data-stu-id="73060-160">**Domain-joined devices**.</span></span> <span data-ttu-id="73060-161">Állítsa be a házirend toorestrict hozzáférés toodevices, a helyszíni Active Directory-tartományhoz csatlakoztatott tooan, és, amely is van regisztrálva az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="73060-161">Set a policy toorestrict access toodevices that are joined tooan on-premises Active Directory domain, and that also are registered with Azure AD.</span></span> <span data-ttu-id="73060-162">Ez a házirend tooWindows asztali gépek, laptopok és vállalati táblagépeken vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="73060-162">This policy applies tooWindows desktops, laptops, and enterprise tablets.</span></span>
  <span data-ttu-id="73060-163">További információ az Azure ad-vel, a tartományhoz csatlakoztatott eszközök automatikus regisztrálása mentése tooset lásd: [beállítása a Windows Azure Active Directory tartományhoz csatlakoztatott eszközök automatikus regisztrálása](active-directory-conditional-access-automatic-device-registration-setup.md).</span><span class="sxs-lookup"><span data-stu-id="73060-163">For more information about how tooset up automatic registration of domain-joined devices with Azure AD, see [Set up automatic registration of Windows domain-joined devices with Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).</span></span>
* <span data-ttu-id="73060-164">**Az előírásoknak megfelelő eszközök**.</span><span class="sxs-lookup"><span data-stu-id="73060-164">**Compliant devices**.</span></span> <span data-ttu-id="73060-165">Állítsa be a házirend toorestrict hozzáférés toodevices megjelölt **megfelelő** hello felügyeleti rendszer könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="73060-165">Set a policy toorestrict access toodevices that are marked **compliant** in hello management system directory.</span></span> <span data-ttu-id="73060-166">Ez a házirend biztosítja, hogy csak a biztonsági házirendek, például a kényszerítése az eszközön a fájltitkosítást teljesítő eszközöket hozzáférhetnek.</span><span class="sxs-lookup"><span data-stu-id="73060-166">This policy ensures that only devices that meet security policies such as enforcing file encryption on a device are allowed access.</span></span> <span data-ttu-id="73060-167">A házirend toorestrict hozzáférés a következő eszközök hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="73060-167">You can use this policy toorestrict access from hello following devices:</span></span>
  
  * <span data-ttu-id="73060-168">**Windows-tartományhoz csatlakoztatott eszközök**.</span><span class="sxs-lookup"><span data-stu-id="73060-168">**Windows domain-joined devices**.</span></span> <span data-ttu-id="73060-169">A System Center Configuration Manager által felügyelt (a hello aktuális ág) hibrid konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="73060-169">Managed by System Center Configuration Manager (in hello current branch) deployed in a hybrid configuration.</span></span>
  * <span data-ttu-id="73060-170">**Windows 10 Mobile munkahelyi vagy személyes eszközök**.</span><span class="sxs-lookup"><span data-stu-id="73060-170">**Windows 10 Mobile work or personal devices**.</span></span> <span data-ttu-id="73060-171">Intune vagy egy támogatott harmadik fél mobileszköz felügyeleti rendszer kezeli.</span><span class="sxs-lookup"><span data-stu-id="73060-171">Managed by Intune or by a supported third-party mobile device management system.</span></span>
  * <span data-ttu-id="73060-172">**iOS és Android-eszközök**.</span><span class="sxs-lookup"><span data-stu-id="73060-172">**iOS and Android devices**.</span></span> <span data-ttu-id="73060-173">Intune által felügyelt.</span><span class="sxs-lookup"><span data-stu-id="73060-173">Managed by Intune.</span></span>

<span data-ttu-id="73060-174">Azok a felhasználók számára, akik egy eszköz-alapú által védett alkalmazások, certification authority házirend hello alkalmazás férni egy eszközről, amely megfelel a házirend követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="73060-174">Users who access applications that are protected by a device-based, certification authority policy must access hello application from a device that meets this policy's requirements.</span></span> <span data-ttu-id="73060-175">Hozzáférés megtagadva, ha a kísérlet történt egy eszközön, amely nem felel meg a házirend követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="73060-175">Access is denied if attempted on a device that doesn’t meet policy requirements.</span></span>

<span data-ttu-id="73060-176">További információ a hogyan tooconfigure eszköz alapú, az Azure AD-hitelesítésszolgáltató irányelvét: [eszközalapú feltételes hozzáférési házirend, az Azure Active Directory-kompatibilis alkalmazásokat](active-directory-conditional-access-policy-connected-applications.md).</span><span class="sxs-lookup"><span data-stu-id="73060-176">For information about how tooconfigure a device-based, certification authority policy in Azure AD, see [Set device-based conditional access policy for Azure Active Directory-connected applications](active-directory-conditional-access-policy-connected-applications.md).</span></span>

## <a name="resources"></a><span data-ttu-id="73060-177">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="73060-177">Resources</span></span>
<span data-ttu-id="73060-178">Tekintse meg a következő erőforrás kategóriák és a cikkek toolearn a szervezet feltételes hozzáférés beállításáról további hello.</span><span class="sxs-lookup"><span data-stu-id="73060-178">See hello following resource categories and articles toolearn more about setting conditional access for your organization.</span></span>

### <a name="multi-factor-authentication-and-location-policies"></a><span data-ttu-id="73060-179">Többtényezős hitelesítés és a hely házirendek</span><span class="sxs-lookup"><span data-stu-id="73060-179">Multi-factor authentication and location policies</span></span>
* [<span data-ttu-id="73060-180">Ismerkedés a feltételes hozzáférés tooAzure AD-kompatibilis alkalmazások csoport, a hely és a többtényezős hitelesítési házirendek alapján</span><span class="sxs-lookup"><span data-stu-id="73060-180">Getting started with conditional access tooAzure AD-connected apps based on group, location, and multi-factor authentication policies</span></span>](active-directory-conditional-access-azuread-connected-apps.md)
* [<span data-ttu-id="73060-181">Alkalmazások és a támogatott böngészők</span><span class="sxs-lookup"><span data-stu-id="73060-181">Applications and browsers that are supported</span></span>](active-directory-conditional-access-supported-apps.md)

### <a name="device-based-conditional-access"></a><span data-ttu-id="73060-182">Eszközalapú feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="73060-182">Device-based conditional access</span></span>
* [<span data-ttu-id="73060-183">Állítsa be a hozzáférési eszközalapú feltételes hozzáférési házirend vezérlő tooAzure Active Directory-kompatibilis alkalmazásokat</span><span class="sxs-lookup"><span data-stu-id="73060-183">Set device-based conditional access policy for access control tooAzure Active Directory-connected applications</span></span>](active-directory-conditional-access-policy-connected-applications.md)
* [<span data-ttu-id="73060-184">Állítsa be az automatikus regisztráció, a Windows Azure Active Directory tartományhoz csatlakozó eszközök</span><span class="sxs-lookup"><span data-stu-id="73060-184">Set up automatic registration of Windows domain-joined devices with Azure Active Directory</span></span>](active-directory-conditional-access-automatic-device-registration-setup.md)
* [<span data-ttu-id="73060-185">Azure Active Directory-hozzáférési problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="73060-185">Troubleshooting for Azure Active Directory access issues</span></span>](active-directory-conditional-access-device-remediation.md)
* [<span data-ttu-id="73060-186">Elveszett vagy ellopott eszközökön lévő adatok védelme a Microsoft Intune segítségével</span><span class="sxs-lookup"><span data-stu-id="73060-186">Help protect data on lost or stolen devices by using Microsoft Intune</span></span>](https://docs.microsoft.com/intune/deploy-use/use-remote-wipe-to-help-protect-data-using-microsoft-intune)

### <a name="protect-resources-based-on-sign-in-risk"></a><span data-ttu-id="73060-187">A bejelentkezési kockázat alapján erőforrások védelme</span><span class="sxs-lookup"><span data-stu-id="73060-187">Protect resources based on sign-in risk</span></span>
* [<span data-ttu-id="73060-188">Az Azure AD identity protection</span><span class="sxs-lookup"><span data-stu-id="73060-188">Azure AD identity protection</span></span>](active-directory-identityprotection.md)

### <a name="next-steps"></a><span data-ttu-id="73060-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73060-189">Next steps</span></span>
* [<span data-ttu-id="73060-190">Feltételes hozzáférés – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="73060-190">Conditional access FAQs</span></span>](active-directory-conditional-faqs.md)
* [<span data-ttu-id="73060-191">Technikai útmutató</span><span class="sxs-lookup"><span data-stu-id="73060-191">Technical reference</span></span>](active-directory-conditional-access-technical-reference.md)

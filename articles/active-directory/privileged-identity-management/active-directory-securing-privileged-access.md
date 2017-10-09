---
title: az Azure AD Privileged Access aaaSecuring |} Microsoft Docs
description: "Ez a témakör azt ismerteti, hello megközelíti Azure, az Azure Active Directory és a Microsoft Online Services rendszerjogosultságú hozzáférés biztosítása érdekében."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: mwahl
ms.assetid: 235a0ce9-1daf-4433-8f65-9c6afcd64d08
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: kgremban
ms.custom: pim
ms.openlocfilehash: 694835dc5c41640673dbd996d44b0d1f217220de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a><span data-ttu-id="d09a6-103">Az Azure Active Directory biztonságossá tétele a privilegizált hozzáférési jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="d09a6-103">Securing privileged access in Azure AD</span></span>
<span data-ttu-id="d09a6-104">Biztonságossá tétele a privileged access kritikus az első lépés toohelp modern szervezet üzleti eszközök védelme.</span><span class="sxs-lookup"><span data-stu-id="d09a6-104">Securing privileged access is a critical first step toohelp protect business assets in a modern organization.</span></span> <span data-ttu-id="d09a6-105">Kiemelt jogosultságú fiókok azok a fiókok, felügyelheti és kezelheti az informatikai rendszerek.</span><span class="sxs-lookup"><span data-stu-id="d09a6-105">Privileged accounts are accounts that administer and manage IT systems.</span></span> <span data-ttu-id="d09a6-106">A támadók számítógépes ezen fiókok toogain hozzáférés tooan szervezeti adatok és rendszerek célként.</span><span class="sxs-lookup"><span data-stu-id="d09a6-106">Cyber-attackers target these accounts toogain access tooan organization’s data and systems.</span></span> <span data-ttu-id="d09a6-107">toosecure emelt szintű hozzáférés, hello fiókokat kell elkülönítése, és rendszerek hello kockázata, hogy a feltárt tooa rosszindulatú felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d09a6-107">toosecure privileged access, you should isolate hello accounts and systems from hello risk of being exposed tooa malicious user.</span></span>

<span data-ttu-id="d09a6-108">További felhasználók indító tooget emelt szintű hozzáférés felhőalapú szolgáltatások segítségével.</span><span class="sxs-lookup"><span data-stu-id="d09a6-108">More users are starting tooget privileged access through cloud services.</span></span> <span data-ttu-id="d09a6-109">Ilyen lehet például az Office365 globális rendszergazdák, Azure-előfizetés rendszergazdák és felhasználók rendszergazdai hozzáféréssel rendelkező virtuális gépek vagy SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d09a6-109">This can include global administrators of Office365, Azure subscription administrators, and users who have administrative access in VMs or on SaaS apps.</span></span>

<span data-ttu-id="d09a6-110">A Microsoft azt javasolja, hogy kövesse a terv [emelt szintű hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx).</span><span class="sxs-lookup"><span data-stu-id="d09a6-110">Microsoft recommends you follow this roadmap on [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx).</span></span>

<span data-ttu-id="d09a6-111">A felhasználó Azure Active Directory, Office 365, vagy más Microsoft-szolgáltatások és alkalmazások esetén az alapelvek alkalmazni, hogy a felhasználói fiókok által kezelt és hitelesítése az Active Directory vagy az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="d09a6-111">For customers using Azure Active Directory, Office 365, or other Microsoft services and applications, these principles apply whether user accounts are managed and authenticated by Active Directory or in Azure Active Directory.</span></span> <span data-ttu-id="d09a6-112">hello következő szakaszokban további információt az emelt szintű hozzáférés biztonságossá tétele az Azure AD-szolgáltatások toosupport.</span><span class="sxs-lookup"><span data-stu-id="d09a6-112">hello following sections provide more information on Azure AD features toosupport securing privileged access.</span></span>

## <a name="azure-multi-factor-authentication"></a><span data-ttu-id="d09a6-113">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="d09a6-113">Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="d09a6-114">rendszergazda hitelesítés tooincrease hello biztonságát, érdemes beállítani kétlépéses ellenőrzés jogosultságok megadása előtt.</span><span class="sxs-lookup"><span data-stu-id="d09a6-114">tooincrease hello security of administrator authentication, you should require two-step verification before granting privileges.</span></span> <span data-ttu-id="d09a6-115">Kétlépéses ellenőrzés egy metódust, akik áll, amelyek ellenőrzése csupán felhasználónévvel és jelszóval hello használatát igényli.</span><span class="sxs-lookup"><span data-stu-id="d09a6-115">Two-step verification is a method of verifying who you are that requires hello use of more than just a username and password.</span></span> <span data-ttu-id="d09a6-116">Biztonsági toouser bejelentkezéseket és tranzakciókat második réteget biztosít.</span><span class="sxs-lookup"><span data-stu-id="d09a6-116">It provides a second layer of security toouser sign-ins and transactions.</span></span>

<span data-ttu-id="d09a6-117">Az Azure multi-factor Authentication (MFA) egy a Microsoft kétlépéses ellenőrzés megoldás, mely segítségével megakadályozhatja hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint.</span><span class="sxs-lookup"><span data-stu-id="d09a6-117">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution, which helps safeguard access toodata and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="d09a6-118">Erős hitelesítés, többek között könnyen ellenőrzési lehetőségek széles keresztül biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d09a6-118">It delivers strong authentication via a range of easy verification options including:</span></span>

- <span data-ttu-id="d09a6-119">telefonhívásokat</span><span class="sxs-lookup"><span data-stu-id="d09a6-119">phone calls</span></span>
- <span data-ttu-id="d09a6-120">Szöveges üzenetek</span><span class="sxs-lookup"><span data-stu-id="d09a6-120">text messages</span></span>
- <span data-ttu-id="d09a6-121">mobilalkalmazás-értesítések</span><span class="sxs-lookup"><span data-stu-id="d09a6-121">mobile app notifications</span></span>
- <span data-ttu-id="d09a6-122">mobilalkalmazás ellenőrző kódok kezelésére</span><span class="sxs-lookup"><span data-stu-id="d09a6-122">mobile app verification codes</span></span>
- <span data-ttu-id="d09a6-123">Harmadik féltől származó OATH jogkivonatokkal</span><span class="sxs-lookup"><span data-stu-id="d09a6-123">third-party OATH tokens</span></span>

<span data-ttu-id="d09a6-124">Azure multi-factor Authentication működése áttekintését lásd a következő videó hello:</span><span class="sxs-lookup"><span data-stu-id="d09a6-124">For an overview of how Azure Multi-Factor Authentication works see hello following video:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

<span data-ttu-id="d09a6-125">További információkért lásd: [többtényezős hitelesítés az Office 365 és az Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span><span class="sxs-lookup"><span data-stu-id="d09a6-125">For more information, see [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span></span>

## <a name="time-bound-privileges"></a><span data-ttu-id="d09a6-126">Időhöz kötött jogosultságokkal</span><span class="sxs-lookup"><span data-stu-id="d09a6-126">Time-bound privileges</span></span>
<span data-ttu-id="d09a6-127">Egyes szervezetek tapasztalhatja, hogy túl sok felhasználó rendelkeznek magas szintű jogosultsággal rendelkező szerepkört.</span><span class="sxs-lookup"><span data-stu-id="d09a6-127">Some organizations may find that they have too many users in highly privileged roles.</span></span> <span data-ttu-id="d09a6-128">A felhasználó esetleg hozzá vannak adva egy adott tevékenységet, például az egy service szolgáltatáshoz toosign toohello szerepkör, de nem a gyakran ezt követően ezeket az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="d09a6-128">A user might have been added toohello role for a particular activity, like toosign up for a service, but didn't use those privileges frequently afterward.</span></span>

<span data-ttu-id="d09a6-129">toolower hello kitettség idő jogosultságokat, és növelheti a használatukat korlát felhasználók tooonly véve a jogosultságait az "igény szerint" láthatósága (JIT), ha egy feladat tooperform van szükségük.</span><span class="sxs-lookup"><span data-stu-id="d09a6-129">toolower hello exposure time of privileges and increase your visibility into their use, limit users tooonly taking on their privileges "just in time" (JIT), when they need tooperform a task.</span></span> <span data-ttu-id="d09a6-130">Az Azure Active Directory és a Microsoft Online Services használhatja [az Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span><span class="sxs-lookup"><span data-stu-id="d09a6-130">For Azure Active Directory and Microsoft Online Services, you can use [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span></span>

![A PIM irányítópult][2]

## <a name="attack-detection"></a><span data-ttu-id="d09a6-132">Észlelt támadás</span><span class="sxs-lookup"><span data-stu-id="d09a6-132">Attack detection</span></span>
<span data-ttu-id="d09a6-133">[Az Azure Active Directory Identity Protection](../active-directory-identityprotection.md) a kockázati eseményekről és a szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="d09a6-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) provides a consolidated view into risk events and potential vulnerabilities affecting your organization’s identities.</span></span> <span data-ttu-id="d09a6-134">Kockázati események alapján Identity Protection számítja ki a felhasználó kockázati szint minden felhasználóhoz, így már tooconfigure kockázati-alapú házirendek tooautomatically védelme hello identitások a szervezet.</span><span class="sxs-lookup"><span data-stu-id="d09a6-134">Based on risk events, Identity Protection calculates a user risk level for each user, enabling you tooconfigure risk-based policies tooautomatically protect hello identities of your organization.</span></span> <span data-ttu-id="d09a6-135">Ezek a házirendek más feltételes hozzáférés-szabályozási EMS, és az Azure Active Directory által biztosított automatikusan hello felhasználó blokkolása vagy ad javaslatokat, amelyek tartalmazzák a jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="d09a6-135">These policies, along with other conditional access controls provided by Azure Active Directory and EMS, can automatically block hello user or offer suggestions that include password resets and multi-factor authentication enforcement.</span></span>

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a><span data-ttu-id="d09a6-137">Feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="d09a6-137">Conditional access</span></span>
<span data-ttu-id="d09a6-138">Feltételes hozzáférés-vezérlést Azure Active Directory hello megadott feltételek úgy dönt, hogy a felhasználó hitelesítéséhez access tooan alkalmazás engedélyezése előtt ellenőrzi.</span><span class="sxs-lookup"><span data-stu-id="d09a6-138">With conditional access control, Azure Active Directory checks hello specific conditions you choose when authenticating a user, before allowing access tooan application.</span></span> <span data-ttu-id="d09a6-139">Ha ezek a feltételek teljesülnek, hello felhasználó hitelesítése és hozzáférési toohello alkalmazás engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="d09a6-139">Once those conditions are met, hello user is authenticated and allowed access toohello application.</span></span>

![Feltételes hozzáférési szabályok a multi-factor Authentication szolgáltatás beállítása][4]

## <a name="related-articles"></a><span data-ttu-id="d09a6-141">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="d09a6-141">Related articles</span></span>
* <span data-ttu-id="d09a6-142">Engedélyezése [Azure többtényezős hitelesítés](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="d09a6-142">Enable [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span></span>
* <span data-ttu-id="d09a6-143">Engedélyezése [az Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span><span class="sxs-lookup"><span data-stu-id="d09a6-143">Enable [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span></span>
* <span data-ttu-id="d09a6-144">Engedélyezése [Azure AD Identity Protection](../active-directory-identityprotection.md)</span><span class="sxs-lookup"><span data-stu-id="d09a6-144">Enable [Azure AD Identity Protection](../active-directory-identityprotection.md)</span></span>
* <span data-ttu-id="d09a6-145">Engedélyezése [feltételes hozzáférés-vezérlést](../active-directory-conditional-access.md)</span><span class="sxs-lookup"><span data-stu-id="d09a6-145">Enable [conditional access controls](../active-directory-conditional-access.md)</span></span>

<span data-ttu-id="d09a6-146">További információk egy teljes biztonsági terv, című hello "ügyfél feladatkörei és terv" hello [vállalati fejlesztők a Microsoft Cloud biztonsági](http://aka.ms/securecustomer) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d09a6-146">For more information on building a complete security roadmap, see hello “Customer responsibilities and roadmap” section of hello [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) document.</span></span> <span data-ttu-id="d09a6-147">A Microsoft-szolgáltatások tooassist egyetlen ezek a témakörök végezhet további tájékoztatásért forduljon a Microsoft képviselőjével, vagy keresse fel a [számítógépes megoldások lap](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span><span class="sxs-lookup"><span data-stu-id="d09a6-147">For more information on engaging Microsoft services tooassist with any of these topics, contact your Microsoft representative or visit our [Cybersecurity solutions page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span></span>

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png

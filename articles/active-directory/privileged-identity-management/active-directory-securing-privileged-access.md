---
title: "Emelt szintű hozzáférés biztosítása az Azure AD |} Microsoft Docs"
description: "Ez a témakör ismerteti a privileged access Azure, az Azure Active Directory és a Microsoft Online Services védelmének."
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
ms.openlocfilehash: acd1c11cecfa37f607fe5d82a76a40647093f43f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="securing-privileged-access-in-azure-ad"></a><span data-ttu-id="0b915-103">Az Azure Active Directory biztonságossá tétele a privilegizált hozzáférési jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="0b915-103">Securing privileged access in Azure AD</span></span>
<span data-ttu-id="0b915-104">Emelt szintű hozzáférés biztonságossá tétele Ez kritikus első lépés egy modern szervezet üzleti eszközök védelme érdekében.</span><span class="sxs-lookup"><span data-stu-id="0b915-104">Securing privileged access is a critical first step to help protect business assets in a modern organization.</span></span> <span data-ttu-id="0b915-105">Kiemelt jogosultságú fiókok azok a fiókok, felügyelheti és kezelheti az informatikai rendszerek.</span><span class="sxs-lookup"><span data-stu-id="0b915-105">Privileged accounts are accounts that administer and manage IT systems.</span></span> <span data-ttu-id="0b915-106">A támadók számítógépes ezeket a fiókokat ahhoz, hogy hozzáférjenek a szervezetek adatokhoz és rendszerek célként.</span><span class="sxs-lookup"><span data-stu-id="0b915-106">Cyber-attackers target these accounts to gain access to an organization’s data and systems.</span></span> <span data-ttu-id="0b915-107">Emelt szintű hozzáférés biztonságossá tételéhez, a fiókok és a kockázat, hogy egy rosszindulatú felhasználó rendszert kell különíteni.</span><span class="sxs-lookup"><span data-stu-id="0b915-107">To secure privileged access, you should isolate the accounts and systems from the risk of being exposed to a malicious user.</span></span>

<span data-ttu-id="0b915-108">További felhasználók emelt szintű hozzáférés keresztül felhőszolgáltatások eléréséhez fizet.</span><span class="sxs-lookup"><span data-stu-id="0b915-108">More users are starting to get privileged access through cloud services.</span></span> <span data-ttu-id="0b915-109">Ilyen lehet például az Office365 globális rendszergazdák, Azure-előfizetés rendszergazdák és felhasználók rendszergazdai hozzáféréssel rendelkező virtuális gépek vagy SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="0b915-109">This can include global administrators of Office365, Azure subscription administrators, and users who have administrative access in VMs or on SaaS apps.</span></span>

<span data-ttu-id="0b915-110">A Microsoft azt javasolja, hogy kövesse a terv [emelt szintű hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx).</span><span class="sxs-lookup"><span data-stu-id="0b915-110">Microsoft recommends you follow this roadmap on [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx).</span></span>

<span data-ttu-id="0b915-111">A felhasználó Azure Active Directory, Office 365, vagy más Microsoft-szolgáltatások és alkalmazások esetén az alapelvek alkalmazni, hogy a felhasználói fiókok által kezelt és hitelesítése az Active Directory vagy az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="0b915-111">For customers using Azure Active Directory, Office 365, or other Microsoft services and applications, these principles apply whether user accounts are managed and authenticated by Active Directory or in Azure Active Directory.</span></span> <span data-ttu-id="0b915-112">Az alábbi szakaszokban további információk az Azure AD-funkciókat támogató emelt szintű hozzáférés biztonságossá tétele.</span><span class="sxs-lookup"><span data-stu-id="0b915-112">The following sections provide more information on Azure AD features to support securing privileged access.</span></span>

## <a name="azure-multi-factor-authentication"></a><span data-ttu-id="0b915-113">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="0b915-113">Azure Multi-Factor Authentication</span></span>
<span data-ttu-id="0b915-114">A rendszergazdai hitelesítési biztonság növelése érdekében jogosultságok megadása előtt a kétlépéses ellenőrzés elvégzéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b915-114">To increase the security of administrator authentication, you should require two-step verification before granting privileges.</span></span> <span data-ttu-id="0b915-115">Kétlépéses ellenőrzés egy metódus ellenőrzésére, akik meg, amelyek nem csak a felhasználónév és jelszó használatát igényli.</span><span class="sxs-lookup"><span data-stu-id="0b915-115">Two-step verification is a method of verifying who you are that requires the use of more than just a username and password.</span></span> <span data-ttu-id="0b915-116">A második biztonsági szintként, a felhasználói bejelentkezéseket és tranzakciókat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0b915-116">It provides a second layer of security to user sign-ins and transactions.</span></span>

<span data-ttu-id="0b915-117">Az Azure multi-factor Authentication (MFA) a Microsoft kétlépéses ellenőrzés megoldás, mely segít hozzáférés biztonságossá tételét adatokhoz és alkalmazásokhoz a felhasználói betartása mellett egy egyszerű bejelentkezési folyamat keresletének.</span><span class="sxs-lookup"><span data-stu-id="0b915-117">Azure Multi-Factor Authentication (MFA) is Microsoft's two-step verification solution, which helps safeguard access to data and applications while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="0b915-118">Erős hitelesítés, többek között könnyen ellenőrzési lehetőségek széles keresztül biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0b915-118">It delivers strong authentication via a range of easy verification options including:</span></span>

- <span data-ttu-id="0b915-119">telefonhívásokat</span><span class="sxs-lookup"><span data-stu-id="0b915-119">phone calls</span></span>
- <span data-ttu-id="0b915-120">Szöveges üzenetek</span><span class="sxs-lookup"><span data-stu-id="0b915-120">text messages</span></span>
- <span data-ttu-id="0b915-121">mobilalkalmazás-értesítések</span><span class="sxs-lookup"><span data-stu-id="0b915-121">mobile app notifications</span></span>
- <span data-ttu-id="0b915-122">mobilalkalmazás ellenőrző kódok kezelésére</span><span class="sxs-lookup"><span data-stu-id="0b915-122">mobile app verification codes</span></span>
- <span data-ttu-id="0b915-123">Harmadik féltől származó OATH jogkivonatokkal</span><span class="sxs-lookup"><span data-stu-id="0b915-123">third-party OATH tokens</span></span>

<span data-ttu-id="0b915-124">Tekintse át az alábbi videó az Azure multi-factor Authentication működésének áttekintése:</span><span class="sxs-lookup"><span data-stu-id="0b915-124">For an overview of how Azure Multi-Factor Authentication works see the following video:</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]

<span data-ttu-id="0b915-125">További információkért lásd: [többtényezős hitelesítés az Office 365 és az Azure MFA](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span><span class="sxs-lookup"><span data-stu-id="0b915-125">For more information, see [MFA for Office 365 and MFA for Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).</span></span>

## <a name="time-bound-privileges"></a><span data-ttu-id="0b915-126">Időhöz kötött jogosultságokkal</span><span class="sxs-lookup"><span data-stu-id="0b915-126">Time-bound privileges</span></span>
<span data-ttu-id="0b915-127">Egyes szervezetek tapasztalhatja, hogy túl sok felhasználó rendelkeznek magas szintű jogosultsággal rendelkező szerepkört.</span><span class="sxs-lookup"><span data-stu-id="0b915-127">Some organizations may find that they have too many users in highly privileged roles.</span></span> <span data-ttu-id="0b915-128">A felhasználó előfordulhat, hogy érhetőek el a szerepkör egy adott tevékenységet, például a Feliratkozás a szolgáltatás azonban nem használja ezeket az engedélyeket gyakran ezt követően.</span><span class="sxs-lookup"><span data-stu-id="0b915-128">A user might have been added to the role for a particular activity, like to sign up for a service, but didn't use those privileges frequently afterward.</span></span>

<span data-ttu-id="0b915-129">Jogosultságokat a kitettség idő csökkentése, és növelje a használatukat lássák, korlátozza a felhasználók számára csak véve a jogosultságait az "igény szerint" (JIT), ha a feladat végrehajtásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="0b915-129">To lower the exposure time of privileges and increase your visibility into their use, limit users to only taking on their privileges "just in time" (JIT), when they need to perform a task.</span></span> <span data-ttu-id="0b915-130">Az Azure Active Directory és a Microsoft Online Services használhatja [az Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span><span class="sxs-lookup"><span data-stu-id="0b915-130">For Azure Active Directory and Microsoft Online Services, you can use [Azure AD Privileged Identity Management (PIM)](http://aka.ms/AzurePIM).</span></span>

![A PIM irányítópult][2]

## <a name="attack-detection"></a><span data-ttu-id="0b915-132">Észlelt támadás</span><span class="sxs-lookup"><span data-stu-id="0b915-132">Attack detection</span></span>
<span data-ttu-id="0b915-133">[Az Azure Active Directory Identity Protection](../active-directory-identityprotection.md) a kockázati eseményekről és a szervezet identitásait érintő lehetséges biztonsági rések egyesített nézetét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="0b915-133">[Azure Active Directory Identity Protection](../active-directory-identityprotection.md) provides a consolidated view into risk events and potential vulnerabilities affecting your organization’s identities.</span></span> <span data-ttu-id="0b915-134">Identity Protection kockázati események alapján számítja ki minden felhasználó, amely lehetővé teszi, kockázati-alapú házirendek automatikusan védő a szervezete egy felhasználó kockázati szintjét.</span><span class="sxs-lookup"><span data-stu-id="0b915-134">Based on risk events, Identity Protection calculates a user risk level for each user, enabling you to configure risk-based policies to automatically protect the identities of your organization.</span></span> <span data-ttu-id="0b915-135">Ezek a házirendek más feltételes hozzáférés-szabályozási EMS, és az Azure Active Directory által biztosított automatikusan a felhasználó letiltásához vagy ad javaslatokat, amelyek tartalmazzák a jelszó alaphelyzetbe állítását és a többtényezős hitelesítés kényszerítése.</span><span class="sxs-lookup"><span data-stu-id="0b915-135">These policies, along with other conditional access controls provided by Azure Active Directory and EMS, can automatically block the user or offer suggestions that include password resets and multi-factor authentication enforcement.</span></span>

![Azure AD Identity Protection][3]

## <a name="conditional-access"></a><span data-ttu-id="0b915-137">Feltételes hozzáférés</span><span class="sxs-lookup"><span data-stu-id="0b915-137">Conditional access</span></span>
<span data-ttu-id="0b915-138">Feltételes hozzáférés-vezérlést Azure Active Directory ellenőrzi a megadott feltételek úgy dönt, hogy a felhasználó hitelesítéséhez az alkalmazáshoz való hozzáférés engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="0b915-138">With conditional access control, Azure Active Directory checks the specific conditions you choose when authenticating a user, before allowing access to an application.</span></span> <span data-ttu-id="0b915-139">Ha ezek a feltételek teljesülnek, a felhasználó hitelesítése és hozzáférni az alkalmazáshoz engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="0b915-139">Once those conditions are met, the user is authenticated and allowed access to the application.</span></span>

![Feltételes hozzáférési szabályok a multi-factor Authentication szolgáltatás beállítása][4]

## <a name="related-articles"></a><span data-ttu-id="0b915-141">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="0b915-141">Related articles</span></span>
* <span data-ttu-id="0b915-142">Engedélyezése [Azure többtényezős hitelesítés](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="0b915-142">Enable [Azure Multi-Factor Authentication](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)</span></span>
* <span data-ttu-id="0b915-143">Engedélyezése [az Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span><span class="sxs-lookup"><span data-stu-id="0b915-143">Enable [Azure AD Privileged Identity Management](../active-directory-privileged-identity-management-configure.md)</span></span>
* <span data-ttu-id="0b915-144">Engedélyezése [Azure AD Identity Protection](../active-directory-identityprotection.md)</span><span class="sxs-lookup"><span data-stu-id="0b915-144">Enable [Azure AD Identity Protection](../active-directory-identityprotection.md)</span></span>
* <span data-ttu-id="0b915-145">Engedélyezése [feltételes hozzáférés-vezérlést](../active-directory-conditional-access.md)</span><span class="sxs-lookup"><span data-stu-id="0b915-145">Enable [conditional access controls](../active-directory-conditional-access.md)</span></span>

<span data-ttu-id="0b915-146">Egy teljes biztonsági terv felépítésével további információkért lásd: az "ügyfél feladatkörei és terv" részében a [vállalati fejlesztők a Microsoft Cloud biztonsági](http://aka.ms/securecustomer) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="0b915-146">For more information on building a complete security roadmap, see the “Customer responsibilities and roadmap” section of the [Microsoft Cloud Security for Enterprise Architects](http://aka.ms/securecustomer) document.</span></span> <span data-ttu-id="0b915-147">Bővebben módon vegye fel a Microsoft-szolgáltatások az alábbi témakörök egyik segít, forduljon a Microsoft képviselőjével, vagy keresse fel a [számítógépes megoldások lap](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span><span class="sxs-lookup"><span data-stu-id="0b915-147">For more information on engaging Microsoft services to assist with any of these topics, contact your Microsoft representative or visit our [Cybersecurity solutions page](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx).</span></span>

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png

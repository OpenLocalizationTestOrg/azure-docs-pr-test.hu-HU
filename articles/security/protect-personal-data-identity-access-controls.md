---
title: "személyes adatok aaaProtect Azure identitások és hozzáférések vezérlőkkel |} Microsoft Docs"
description: "Azure identitás- és hozzáférés-vezérlők toohelp használatával, a személyes adatok védelme"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 3132c2af25f86662668e5b555eab1d81de7f2e6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a><span data-ttu-id="7c779-103">Az Azure Active Directory és a multi-factor Authentication: identitások és hozzáférések vezérlőkkel személyes adatok védelme</span><span class="sxs-lookup"><span data-stu-id="7c779-103">Azure Active Directory and Multi-Factor Authentication: Protect personal data with identity and access controls</span></span>

<span data-ttu-id="7c779-104">Ez a cikk ismerteti, használhatja a tooprotect személyes adatokat az Azure Active Directory és a többtényezős hitelesítési biztonsági szolgáltatások és szolgáltatások használatával.</span><span class="sxs-lookup"><span data-stu-id="7c779-104">This article provides information and procedures you can use tooprotect personal data using Azure Active Directory and Multi-factor authentication security features and services.</span></span>

## <a name="scenario"></a><span data-ttu-id="7c779-105">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="7c779-105">Scenario</span></span>

<span data-ttu-id="7c779-106">A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik.</span><span class="sxs-lookup"><span data-stu-id="7c779-106">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="7c779-107">toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit</span><span class="sxs-lookup"><span data-stu-id="7c779-107">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span> 

<span data-ttu-id="7c779-108">hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja.</span><span class="sxs-lookup"><span data-stu-id="7c779-108">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="7c779-109">Ez magában foglalja a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait.</span><span class="sxs-lookup"><span data-stu-id="7c779-109">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="7c779-110">Minden helyen hagyományos emberi erőforrások adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7c779-110">It also includes traditional Human Resources information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="7c779-111">hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.</span><span class="sxs-lookup"><span data-stu-id="7c779-111">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="7c779-112">Vállalati alkalmazottak hozzáférési hello hálózati hello vállalati távoli iroda és utazás ügynökök található hello világ rendelkezik toosome vállalati erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="7c779-112">Corporate employees access hello network from hello company’s remote offices and travel agents located around hello world have access toosome company resources.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="7c779-113">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="7c779-113">Problem statement</span></span>

<span data-ttu-id="7c779-114">hello vállalati kell védelmében hello ügyfelek és az alkalmazottak a személyes adatok a támadóktól sérült toouse identitások toogain hozzáférést kér.</span><span class="sxs-lookup"><span data-stu-id="7c779-114">hello company must protect hello privacy of customers’ and employees’ personal data from attackers seeking toouse compromised identities toogain access.</span></span> <span data-ttu-id="7c779-115">Is biztosítaniuk kell, hogy jogosult felhasználók adatait csak azok, akik szükség lenne rá toodo korlátozott hozzáférés toopersonal a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="7c779-115">They also must ensure that access toopersonal data by legitimate users is restricted to only those who need it toodo their jobs.</span></span>

## <a name="company-goal"></a><span data-ttu-id="7c779-116">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="7c779-116">Company goal</span></span>

<span data-ttu-id="7c779-117">hello vállalati célja toopersonal adatokat elérő tooensure szigorú vezérlését.</span><span class="sxs-lookup"><span data-stu-id="7c779-117">hello company’s goal is tooensure that access toopersonal data is strictly controlled.</span></span> <span data-ttu-id="7c779-118">Fontos, hogy a felhasználók hozzáférést toopersonal adatokkal azonosítói erős hitelesítés védi.</span><span class="sxs-lookup"><span data-stu-id="7c779-118">It is essential that identities of users with access toopersonal data be protected by strong authentication.</span></span> <span data-ttu-id="7c779-119">A házirend a [minimális jogosultság] (https://en.wikipedia.org/wiki/Principle_of_least_privilege), hogy jogos felhasználója csak hello szintű hozzáférést a szükséges, és nem több kényszerítettnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="7c779-119">A policy of [least privilege] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) must be enforced so that legitimate users have only hello level of  access they need, and no more.</span></span>

## <a name="solutions"></a><span data-ttu-id="7c779-120">Megoldások</span><span class="sxs-lookup"><span data-stu-id="7c779-120">Solutions</span></span>

<span data-ttu-id="7c779-121">A Microsoft Azure access tooresources személyes adatokat tartalmazó rendelkező toohelp vállalatok felügyeletét biztosítja, identitás- és hozzáférés-felügyeleti eszközök.</span><span class="sxs-lookup"><span data-stu-id="7c779-121">Microsoft Azure provides identity and access management tools toohelp companies control who has access tooresources that contain personal data.</span></span>

### <a name="azure-active-directory"></a><span data-ttu-id="7c779-122">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7c779-122">Azure Active Directory</span></span>

<span data-ttu-id="7c779-123">[Az Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) identitások kezeli, és szabályozza a hozzáférést tooAzure, valamint egyéb a helyszíni és más felhőalapú erőforrásokat, adatokhoz és alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="7c779-123">[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) manages identities and controls access tooAzure as well as other on-premises and other cloud resources, data, and applications.</span></span> <span data-ttu-id="7c779-124">[Az Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) segít az Azure rendszergazdák toominimize hello személyek, akiknek toocertain adatok eléréséhez például a személyes adatok száma.</span><span class="sxs-lookup"><span data-stu-id="7c779-124">[Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) helps Azure administrators toominimize hello number of people who have access toocertain information such as personal data.</span></span> <span data-ttu-id="7c779-125">Lehetővé teszi őket toodiscover korlátozása és figyelheti a kiemelt jogosultságú identitások és azok hozzáférés tooresources, és ideiglenes, tooassign fordítója (JIT) rendszergazdai jogosultságokkal tooeligible felhasználók.</span><span class="sxs-lookup"><span data-stu-id="7c779-125">It enables them toodiscover, restrict, and monitor privileged identities and their access tooresources, and tooassign temporary, Just-In-Time (JIT) administrative rights tooeligible users.</span></span> <span data-ttu-id="7c779-126">Emellett biztosítja az AAD-rendszergazdai jogosultságokkal rendelkező személyek betekintést.</span><span class="sxs-lookup"><span data-stu-id="7c779-126">It also provides insight into those who have AAD administrative privileges.</span></span>

<span data-ttu-id="7c779-127">az AAD PIM használatával járó hello tevékenységei közé tartoznak:</span><span class="sxs-lookup"><span data-stu-id="7c779-127">hello activities involved in using AAD PIM include:</span></span>

- <span data-ttu-id="7c779-128">A Privileged Identity Management engedélyezése a címtáron</span><span class="sxs-lookup"><span data-stu-id="7c779-128">Enabling Privileged Identity Management for your directory</span></span>

- <span data-ttu-id="7c779-129">A Privileged Identity Management felügyeleti Irányítópult toosee fontos információk segítségével egy pillanat alatt</span><span class="sxs-lookup"><span data-stu-id="7c779-129">Using Privileged Identity Management admin dashboard toosee important information at a glance</span></span>

- <span data-ttu-id="7c779-130">Hello emelt szintű identitások (rendszergazdák) hozzáadásával vagy eltávolításával a rendszergazdák állandó vagy jogosult tooeach szerepkör kezelése</span><span class="sxs-lookup"><span data-stu-id="7c779-130">Managing hello privileged identities (administrators) by adding or removing permanent or eligible administrators tooeach role</span></span>

- <span data-ttu-id="7c779-131">Hello szerepkör aktiválási beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c779-131">Configuring hello role activation settings</span></span>

- <span data-ttu-id="7c779-132">Szerepkörök aktiválása</span><span class="sxs-lookup"><span data-stu-id="7c779-132">Activating roles</span></span>

- <span data-ttu-id="7c779-133">Szerepkör tevékenység áttekintése</span><span class="sxs-lookup"><span data-stu-id="7c779-133">Reviewing role activity</span></span>

#### <a name="how-do-i-enable-aad-pim"></a><span data-ttu-id="7c779-134">Hogyan engedélyezhető az aad-ben PIM?</span><span class="sxs-lookup"><span data-stu-id="7c779-134">How do I enable AAD PIM?</span></span>

<span data-ttu-id="7c779-135">a PIM használatát a címtár toostart hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7c779-135">toostart using PIM for your directory, do hello following:</span></span>

1. <span data-ttu-id="7c779-136">Jelentkezzen be toohello Azure-portálon a címtár globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7c779-136">Sign in toohello Azure portal as a global administrator of your directory.</span></span>

2. <span data-ttu-id="7c779-137">Ha a szervezet több címtárral rendelkezik, válassza ki a felhasználónév hello jobb felső sarokban a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7c779-137">If your organization has more than one directory, select your username in hello upper right-hand corner of hello Azure portal.</span></span> <span data-ttu-id="7c779-138">Válassza ki a hello directory, ahol az Azure AD Privileged Identity Management szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="7c779-138">Select hello directory where you will use Azure AD Privileged Identity Management.</span></span>

3. <span data-ttu-id="7c779-139">Válassza ki **további szolgáltatások** és hello **szűrő** szövegmező toosearch az Azure AD Privileged Identity Management.</span><span class="sxs-lookup"><span data-stu-id="7c779-139">Select **More services** and use hello **Filter** textbox toosearch for Azure AD Privileged Identity Management.</span></span>

4. <span data-ttu-id="7c779-140">Ellenőrizze **PIN-kód toodashboard** majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7c779-140">Check **Pin toodashboard** and then click **Create**.</span></span> <span data-ttu-id="7c779-141">Megnyílik a Privileged Identity Management alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="7c779-141">hello Privileged Identity Management application opens.</span></span>

<span data-ttu-id="7c779-142">Azure AD Privileged Identity Management beállítása után hello navigációs panelen láthatja hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="7c779-142">Once Azure AD Privileged Identity Management is set up, you see hello navigation blade whenever you open hello application.</span></span>

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

<span data-ttu-id="7c779-143">További információkat és utasításokat Ismerkedés az aad-ben PIM [Start használata az Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span><span class="sxs-lookup"><span data-stu-id="7c779-143">For more information and instructions on getting started with AAD PIM, see [Start Using Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)</span></span>

### <a name="azure-role-based-access-control"></a><span data-ttu-id="7c779-144">Azure szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="7c779-144">Azure Role-based Access Control</span></span>

<span data-ttu-id="7c779-145">[Szerepköralapú hozzáférés-vezérlés az Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) hozzáférési tooAzure erőforrások kezelése hello nyújtása hello felhasználó hozzárendelt szerepkörön alapuló hozzáférés engedélyezésével az Azure rendszergazdáit segíti.</span><span class="sxs-lookup"><span data-stu-id="7c779-145">[Azure Role-Based Access Control](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) (RBAC) helps Azure administrators manage access tooAzure resources by enabling hello granting of access based on hello user’s assigned role.</span></span> <span data-ttu-id="7c779-146">Elkülönítse a csapaton belüli feladatokat, és adja meg csak hello mennyisége hozzáférés toousers, csoportok és alkalmazások, hogy be kell tooperform a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="7c779-146">You can segregate duties within a team and grant only hello amount of access toousers, groups and applications that they need tooperform their jobs.</span></span>

<span data-ttu-id="7c779-147">Szerepköralapú hozzáférés-toousers hello Azure-portálon, az Azure parancssori eszközöket vagy Azure szolgáltatásfelügyeleti API használatával engedélyezhetők.</span><span class="sxs-lookup"><span data-stu-id="7c779-147">Role-based access can be granted toousers using hello Azure portal, Azure Command-Line tools or Azure Management APIs.</span></span>

<span data-ttu-id="7c779-148">Azure RBAC alapokkal kapcsolatos további információkért lásd: [Ismerkedés a szerepköralapú hozzáférés-vezérlés a hello Azure portálon.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span><span class="sxs-lookup"><span data-stu-id="7c779-148">For more information about Azure RBAC basics, see [Get started with Role-Based Access Control in hello Azure Portal.](https://docs.microsoft.com/active-directory/role-based-access-control-what-is)</span></span>

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a><span data-ttu-id="7c779-149">Hogyan kezelhető az Azure RBAC a PowerShell segítségével?</span><span class="sxs-lookup"><span data-stu-id="7c779-149">How do I manage Azure RBAC with PowerShell?</span></span>

<span data-ttu-id="7c779-150">Használhatja a PowerShell-parancsmagok toomanage Azure RBAC, beleértve a következő felügyeleti feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="7c779-150">You can use PowerShell cmdlets toomanage Azure RBAC, including hello following management tasks:</span></span>

- <span data-ttu-id="7c779-151">Lista szerepkörök</span><span class="sxs-lookup"><span data-stu-id="7c779-151">List roles</span></span>

- <span data-ttu-id="7c779-152">Lásd a kinek van hozzáférése:</span><span class="sxs-lookup"><span data-stu-id="7c779-152">See who has access</span></span>

- <span data-ttu-id="7c779-153">Hozzáférés biztosítása</span><span class="sxs-lookup"><span data-stu-id="7c779-153">Grant access</span></span>

- <span data-ttu-id="7c779-154">Megszünteti a hozzáférést</span><span class="sxs-lookup"><span data-stu-id="7c779-154">Remove access</span></span>

- <span data-ttu-id="7c779-155">Egyéni szerepkör létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c779-155">Create a custom role</span></span>

- <span data-ttu-id="7c779-156">Egy erőforrás-szolgáltató műveleteinek beolvasása</span><span class="sxs-lookup"><span data-stu-id="7c779-156">Get Actions for a Resource Provider</span></span>

- <span data-ttu-id="7c779-157">Egyéni szerepkör módosítása</span><span class="sxs-lookup"><span data-stu-id="7c779-157">Modify a custom role</span></span>

- <span data-ttu-id="7c779-158">Egyéni szerepkör törléséhez</span><span class="sxs-lookup"><span data-stu-id="7c779-158">Delete a custom role</span></span>

- <span data-ttu-id="7c779-159">Egyéni szerepkörök listája</span><span class="sxs-lookup"><span data-stu-id="7c779-159">List custom roles</span></span>

<span data-ttu-id="7c779-160">Hogyan toomanage a PowerShell használatával, az Azure RBAC: kapcsolatos utasításokat [kezelése szerepköralapú hozzáférés az Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span><span class="sxs-lookup"><span data-stu-id="7c779-160">For instructions on how toomanage Azure RBAC with PowerShell, see [Manage Role-based Access with Azure PowerShell](https://docs.microsoft.com/azure/active-directory/role-based-access-control-manage-access-powershell).</span></span>

### <a name="azure-multi-factor-authentication"></a><span data-ttu-id="7c779-161">Azure Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="7c779-161">Azure Multi-Factor Authentication</span></span>

<span data-ttu-id="7c779-162">[Az Azure multi-factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) egy kétlépéses ellenőrzés megoldás, amely segít a biztonságos működés érdekében hozzáférés toodata és az alkalmazások, egyszerű bejelentkezési folyamatot a felhasználó igény szerint betartása mellett.</span><span class="sxs-lookup"><span data-stu-id="7c779-162">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA) is a two-step verification solution that helps safeguard access toodata and applications, while meeting user demand for a simple sign-in process.</span></span> <span data-ttu-id="7c779-163">Számos (például telefonos megerősítést, szöveges üzenetet vagy mobilalkalmazást használó) ellenőrzési módszerének köszönhetően erős hitelesítést biztosít.</span><span class="sxs-lookup"><span data-stu-id="7c779-163">It delivers strong authentication via a range of verification methods, including phone call, text message, or mobile app verification.</span></span>

<span data-ttu-id="7c779-164">toofirst szüksége toodeploy MFA hello Azure felhőben, az engedélyezéshez, és kapcsolja be a kétlépéses ellenőrzést, a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="7c779-164">toodeploy MFA in hello Azure cloud, you need toofirst enable it and then turn on two-step verification for users.</span></span>

#### <a name="how-do-i-enable-azure-toouse-mfa"></a><span data-ttu-id="7c779-165">Hogyan engedélyezhető az Azure toouse MFA?</span><span class="sxs-lookup"><span data-stu-id="7c779-165">How do I enable Azure toouse MFA?</span></span>

<span data-ttu-id="7c779-166">Ha a felhasználók, amely tartalmazza az Azure multi-factor Authentication licencek, nincs szükség, hogy kell-e az Azure MFA toodo tooturn.</span><span class="sxs-lookup"><span data-stu-id="7c779-166">If your users have licenses that include Azure Multi-Factor Authentication, there's nothing that you need toodo tooturn on Azure MFA.</span></span> <span data-ttu-id="7c779-167">Ha nem, a multi-factor Auth provider toocreate a könyvtárban van szüksége.</span><span class="sxs-lookup"><span data-stu-id="7c779-167">If not, you need toocreate a Multi-Factor Auth provider in your directory.</span></span> <span data-ttu-id="7c779-168">toodo, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="7c779-168">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="7c779-169">Válassza ki **Active Directory** a hello (bejelentkezve rendszergazdaként) a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7c779-169">Select **Active Directory** in hello Azure classic portal (logged on as an administrator).</span></span>

2. <span data-ttu-id="7c779-170">Válassza ki **többtényezős hitelesítési szolgáltatók.**</span><span class="sxs-lookup"><span data-stu-id="7c779-170">Select **Multi-Factor Authentication Providers.**</span></span>

3. <span data-ttu-id="7c779-171">Válassza ki **új** majd a **alkalmazásszolgáltatások** kiválasztása **többtényezős hitelesítésszolgáltató.**</span><span class="sxs-lookup"><span data-stu-id="7c779-171">Select **New,** and then under **App Services,** select **Multi-Factor Auth Provider.**</span></span>

4. <span data-ttu-id="7c779-172">Válassza ki **Gyorslétrehozás.**</span><span class="sxs-lookup"><span data-stu-id="7c779-172">Select **Quick Create.**</span></span>

5. <span data-ttu-id="7c779-173">Hello neve mezőbe, és válassza ki a használati modell (engedélyezett felhasználónkénti vagy hitelesítésenkénti).</span><span class="sxs-lookup"><span data-stu-id="7c779-173">Fill in hello name field and select a usage model (per authentication or per enabled user).</span></span>

6. <span data-ttu-id="7c779-174">Kijelöl egy könyvtárat, mely hello MFA-szolgáltatóra társítva.</span><span class="sxs-lookup"><span data-stu-id="7c779-174">Designate a directory with which hello MFA Provider is associated.</span></span>

7. <span data-ttu-id="7c779-175">Kattintson a hello **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c779-175">Click hello **Create** button.</span></span>

![](media/protect-personal-data-identity-access-controls/quick-create.png)

<span data-ttu-id="7c779-176">További útmutatást toomanage a többtényezős hitelesítésszolgáltató, lásd: [első lépések az Azure multi-factor Auth Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span><span class="sxs-lookup"><span data-stu-id="7c779-176">For more instructions on how toomanage your Multi-Factor Auth Provider, see [Getting Started with an Azure Multi-Factor Auth Provider.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)</span></span>

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a><span data-ttu-id="7c779-177">Hogyan kapcsolja a kétlépéses ellenőrzést, a felhasználók számára?</span><span class="sxs-lookup"><span data-stu-id="7c779-177">How do I turn on two-step verification for users?</span></span>

<span data-ttu-id="7c779-178">Kényszerítheti a kétlépéses ellenőrzést az összes bejelentkezéseket, vagy hozhat létre feltételes hozzáférési házirendek toorequire kétlépéses ellenőrzést, csak akkor, ha a megadott feltétel teljesül.</span><span class="sxs-lookup"><span data-stu-id="7c779-178">You can enforce two-step verification for all sign-ins, or you can create conditional access policies toorequire two-step verification only when specific conditions apply.</span></span>

<span data-ttu-id="7c779-179">Azure MFA engedélyezése a felhasználói állapotok módosításával egy hagyományos megközelítés hello megkövetelő a kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="7c779-179">Enabling Azure MFA by changing user states is hello traditional approach for requiring two-step verification.</span></span> <span data-ttu-id="7c779-180">Minden hello felhasználónak, amely engedélyezi a hello azonos követelmény tooperform kétlépéses ellenőrzést, minden alkalommal, amikor bejelentkeznek az.</span><span class="sxs-lookup"><span data-stu-id="7c779-180">All hello users that you enable will have hello same requirement tooperform two-step verification every time they sign in.</span></span> <span data-ttu-id="7c779-181">A feltételes hozzáférési szabályzatok, amelyek befolyásolhatják, hogy a felhasználó engedélyezése egy felhasználó felülbírálja.</span><span class="sxs-lookup"><span data-stu-id="7c779-181">Enabling a user overrides any conditional access policies that may affect that user.</span></span>

<span data-ttu-id="7c779-182">Azure MFA engedélyezésével a feltételes hozzáférési házirenddel egy olyan rugalmasabb megközelítés megkövetelő a kétlépéses ellenőrzést.</span><span class="sxs-lookup"><span data-stu-id="7c779-182">Enabling Azure MFA with a conditional access policy is a more flexible approach for requiring two-step verification.</span></span> <span data-ttu-id="7c779-183">Toogroups, valamint az egyes felhasználókra vonatkozó feltételes hozzáférési házirendeket is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7c779-183">You can create conditional access policies that apply toogroups as well as individual users.</span></span> <span data-ttu-id="7c779-184">Magas kockázatú csoportok adható meg több korlátozás mint alacsony kockázat csoportok, vagy a kétlépéses ellenőrzés csak magas kockázatú felhőalkalmazások szükséges, és kihagyja a alacsony kockázat néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="7c779-184">High-risk groups can be given more restrictions than low-risk groups, or two-step verification can be required only for high-risk cloud apps and skipped for low-risk ones.</span></span> <span data-ttu-id="7c779-185">Feltételes hozzáférés azonban egy fizetős az Azure Active Directory szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7c779-185">However, conditional access is a paid feature of Azure Active Directory.</span></span>

<span data-ttu-id="7c779-186">tooenable MFA felhasználói állapot módosításával hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7c779-186">tooenable MFA by changing user state, do hello following:</span></span>

1. <span data-ttu-id="7c779-187">Jelentkezzen be toohello Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7c779-187">Sign in toohello Azure portal as an administrator.</span></span>
2. <span data-ttu-id="7c779-188">Nyissa meg túl**Azure Active Directory \> felhasználók és csoportok \> minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7c779-188">Go too**Azure Active Directory \> Users and groups \> All users**.</span></span>
3. <span data-ttu-id="7c779-189">Válassza ki **a multi-factor Authentication**.</span><span class="sxs-lookup"><span data-stu-id="7c779-189">Select **Multi-Factor Authentication**.</span></span>
4. <span data-ttu-id="7c779-190">Keresés hello felhasználói tooenable szeretné az Azure MFA számára.</span><span class="sxs-lookup"><span data-stu-id="7c779-190">Find hello user that you want tooenable for Azure MFA.</span></span> <span data-ttu-id="7c779-191">Előfordulhat, hogy toochange hello nézet hello tetején.</span><span class="sxs-lookup"><span data-stu-id="7c779-191">You may need toochange hello view at hello top.</span></span>
5. <span data-ttu-id="7c779-192">Ellenőrizze a hello mezőben következő toohello felhasználó nevét.</span><span class="sxs-lookup"><span data-stu-id="7c779-192">Check hello box next toohello user’s name.</span></span>
6. <span data-ttu-id="7c779-193">A jobb oldali Gyorsműveletek a hello válassza **engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="7c779-193">On hello right, under quick steps, choose **Enable**.</span></span>

   ![](media/protect-personal-data-identity-access-controls/quick-create.png)

7. <span data-ttu-id="7c779-194">Erősítse meg választását a hello előugró ablakban.</span><span class="sxs-lookup"><span data-stu-id="7c779-194">Confirm your selection in hello pop-up window that opens.</span></span>  <span data-ttu-id="7c779-195">Felhasználók, akik számára engedélyezve van az MFA kell adnia tooregister hello következő bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="7c779-195">Users for whom MFA has been enabled will be asked tooregister hello next time they sign in.</span></span>

<span data-ttu-id="7c779-196">a feltételes hozzáférési házirend, az Azure MFA tooenable hello a következő:</span><span class="sxs-lookup"><span data-stu-id="7c779-196">tooenable Azure MFA with a conditional access policy, do hello following:</span></span>

1. <span data-ttu-id="7c779-197">Jelentkezzen be toohello Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7c779-197">Sign in toohello Azure portal as an administrator.</span></span>

2. <span data-ttu-id="7c779-198">Nyissa meg túl**Azure Active Directory \> feltételes hozzáférés**.</span><span class="sxs-lookup"><span data-stu-id="7c779-198">Go too**Azure Active Directory \> Conditional access**.</span></span>

3. <span data-ttu-id="7c779-199">Válassza ki **új házirend**.</span><span class="sxs-lookup"><span data-stu-id="7c779-199">Select **New policy**.</span></span>

4. <span data-ttu-id="7c779-200">A **hozzárendelések**, jelölje be **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7c779-200">Under **Assignments**, select **Users and groups**.</span></span> <span data-ttu-id="7c779-201">Használjon hello **Include** és **kizárása** lapokon toospecify mely felhasználók és csoportok hello házirend fogja kezelni.</span><span class="sxs-lookup"><span data-stu-id="7c779-201">Use hello **Include** and     **Exclude** tabs toospecify which users and groups will be managed by hello policy.</span></span>

5. <span data-ttu-id="7c779-202">A **hozzárendelések**, jelölje be **felhőalapú alkalmazásokba.**</span><span class="sxs-lookup"><span data-stu-id="7c779-202">Under **Assignments**, select **Cloud apps.**</span></span> <span data-ttu-id="7c779-203">Válassza ki a túl**tartoznak azok az összes felhő alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7c779-203">Choose too**include All cloud apps**.</span></span>
6.  <span data-ttu-id="7c779-204">A **hozzáférés-szabályozási**, jelölje be **Grant**.</span><span class="sxs-lookup"><span data-stu-id="7c779-204">Under **Access controls**, select **Grant**.</span></span> <span data-ttu-id="7c779-205">Válasszon **többtényezős hitelesítést**.</span><span class="sxs-lookup"><span data-stu-id="7c779-205">Choose **Require multi-factor authentication**.</span></span>
7.  <span data-ttu-id="7c779-206">Kapcsolja be **házirend engedélyezése** túl**a** majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7c779-206">Turn **Enable policy** too**On** and then select **Save**.</span></span>

<span data-ttu-id="7c779-207">Információk hogyan tooconfigure Azure MFA beállítások tooset csalás figyelmeztetéseket, az egyszeri Mellőzés létrehozni, használjon egyedi hangüzenetek, gyorsítótár konfigurálása, adja meg a megbízható IP-címek, alkalmazásjelszavak létrehozásának, jelszóelőzmények eszközök, felhasználók megbízik, és válassza ki az MFA engedélyezése az ellenőrzési módszereket, tekintse meg [Azure multi-factor Authentication beállításainak konfigurálása.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span><span class="sxs-lookup"><span data-stu-id="7c779-207">For information on how tooconfigure Azure MFA settings tooset up fraud alerts, create a one-time bypass, use custom voice messages, configure caching, specify trusted IPs, create app passwords, enable remembering MFA for devices that users trust, and select verification methods, see [Configure Azure Multi-Factor Authentication Settings.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)</span></span>

## <a name="next-steps"></a><span data-ttu-id="7c779-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7c779-208">Next steps</span></span>

- [<span data-ttu-id="7c779-209">Az Azure Active Directory biztonságossá tétele a privilegizált hozzáférési jogosultsága.</span><span class="sxs-lookup"><span data-stu-id="7c779-209">Securing privileged access in Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [<span data-ttu-id="7c779-210">Azure Multi-Factor Authentication – gyakran ismételt kérdések</span><span class="sxs-lookup"><span data-stu-id="7c779-210">Frequently asked questions about Azure Multi-Factor Authentication</span></span>](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [<span data-ttu-id="7c779-211">Szerepköralapú hozzáférés-vezérlés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7c779-211">Role-based Access Control troubleshooting</span></span>](https://docs.microsoft.com/azure/active-directory/role-based-access-control-troubleshooting)

- [<span data-ttu-id="7c779-212">Az Azure Active Directory azonosító adatok védelmét</span><span class="sxs-lookup"><span data-stu-id="7c779-212">Azure Active Directory Identity Protection</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

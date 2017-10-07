---
title: aaaAzure Active Directory reporting |} Microsoft Docs
description: "Az Azure Active Directory jelentéskészítés általános áttekintését nyújtja."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6141a333-38db-478a-927e-526f1e7614f4
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/13/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: c91813acbdc4b0bfcd164169b0b575accac227d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-reporting"></a><span data-ttu-id="3c31d-103">Jelentéskészítés az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="3c31d-103">Azure Active Directory reporting</span></span>

<span data-ttu-id="3c31d-104">Az Azure Active Directory jelentéskészítéssel betekintést nyerhet a környezet működésébe.</span><span class="sxs-lookup"><span data-stu-id="3c31d-104">With Azure Active Directory reporting, you can gain insights into how your environment is doing.</span></span>  
<span data-ttu-id="3c31d-105">a megadott hello adatok teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="3c31d-105">hello provided data enables you to:</span></span>

- <span data-ttu-id="3c31d-106">Meghatározhatja, hogy a felhasználók hogyan használják az alkalmazásokat és szolgáltatásokat</span><span class="sxs-lookup"><span data-stu-id="3c31d-106">Determine how your apps and services are utilized by your users</span></span>
- <span data-ttu-id="3c31d-107">A környezet állapotának hello érintő kockázatok észlelése</span><span class="sxs-lookup"><span data-stu-id="3c31d-107">Detect potential risks affecting hello health of your environment</span></span>
- <span data-ttu-id="3c31d-108">Elháríthatja azokat a hibákat, amelyek megakadályozzák a felhasználók munkavégzését</span><span class="sxs-lookup"><span data-stu-id="3c31d-108">Troubleshoot issues preventing your users from getting their work done</span></span>  

<span data-ttu-id="3c31d-109">hello jelentéskészítési architektúra két fő oszlopok támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="3c31d-109">hello reporting architecture relies on two main pillars:</span></span>

- <span data-ttu-id="3c31d-110">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="3c31d-110">Security reports</span></span>
- <span data-ttu-id="3c31d-111">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="3c31d-111">Activity reports</span></span>

![Jelentéskészítés](./media/active-directory-reporting-azure-portal/01.png)



## <a name="security-reports"></a><span data-ttu-id="3c31d-113">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="3c31d-113">Security reports</span></span>

<span data-ttu-id="3c31d-114">az Azure Active Directoryban hello biztonsági jelentések segítségével meg tooprotect a szervezet identitásait.</span><span class="sxs-lookup"><span data-stu-id="3c31d-114">hello security reports in Azure Active Directory help you tooprotect your organization's identities.</span></span>  
<span data-ttu-id="3c31d-115">Az Azure Active Directoryban két típusú biztonsági jelentés létezik:</span><span class="sxs-lookup"><span data-stu-id="3c31d-115">There are two types of security reports in Azure Active Directory:</span></span>

- <span data-ttu-id="3c31d-116">**Felhasználók meg van jelölve, a kockázat** – hello a [felhasználók meg van jelölve, a kockázat biztonsági jelentés](active-directory-reporting-security-user-at-risk.md), felhasználói fiókokat, előfordulhat, hogy sérült áttekintést kaphat.</span><span class="sxs-lookup"><span data-stu-id="3c31d-116">**Users flagged for risk** - From hello [users flagged for risk security report](active-directory-reporting-security-user-at-risk.md), you get an overview of user accounts that might have been compromised.</span></span>

- <span data-ttu-id="3c31d-117">**Kockázatos bejelentkezések** – hello a [kockázatos bejelentkezési biztonsági jelentés](active-directory-reporting-security-risky-sign-ins.md), a bejelentkezési kísérletek előfordulhat, hogy valaki, aki elvégezte nem hello egy felhasználói fiókot jogos tulajdonosa kap mutató.</span><span class="sxs-lookup"><span data-stu-id="3c31d-117">**Risky sign-ins** - With hello [risky sign-in security report](active-directory-reporting-security-risky-sign-ins.md), you get an indicator for sign-in attempts that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> 

<span data-ttu-id="3c31d-118">**Milyen az Azure AD licencet kell tooaccess biztonsági jelentést?**</span><span class="sxs-lookup"><span data-stu-id="3c31d-118">**What Azure AD license do you need tooaccess a security report?**</span></span>  
<span data-ttu-id="3c31d-119">Az Azure Active Directory minden kiadása biztosítja a kockázatosként megjelölt felhasználók és a kockázatos bejelentkezések jelentéseit.</span><span class="sxs-lookup"><span data-stu-id="3c31d-119">All editions of Azure Active Directory provide you with users flagged for risk and risky sign-ins reports.</span></span>  
<span data-ttu-id="3c31d-120">Azonban a jelentések részletességét hello szintű platformjától függően eltérő hello verziója esetén:</span><span class="sxs-lookup"><span data-stu-id="3c31d-120">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="3c31d-121">A hello **Azure Active Directory szabad és alapszintű kiadásai**, kockázat és kockázatos bejelentkezések megjelölt felhasználók listájának már beolvasása.</span><span class="sxs-lookup"><span data-stu-id="3c31d-121">In hello **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk and risky sign-ins.</span></span> 

- <span data-ttu-id="3c31d-122">Hello **Azure Active Directory Premium 1** edition kiterjeszti a modell is így már tooexamine néhány hello az alapul szolgáló kockázati eseményekről, amelyek az egyes jelentések vonatkozóan nem észlelhetők.</span><span class="sxs-lookup"><span data-stu-id="3c31d-122">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="3c31d-123">Hello **Azure Active Directory Premium 2** kiadás való hello legtöbb részletes információkat az alapul szolgáló kockázati események hello és emellett lehetővé teszi a tooconfigure biztonsági házirendek, amelyek automatikusan reagálnak tooconfigured tartalmazza kockázati szintek.</span><span class="sxs-lookup"><span data-stu-id="3c31d-123">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about hello underlying risk events and it also enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>


## <a name="activity-reports"></a><span data-ttu-id="3c31d-124">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="3c31d-124">Activity reports</span></span>

<span data-ttu-id="3c31d-125">Az Azure Active Directoryban két típusú tevékenységjelentés létezik:</span><span class="sxs-lookup"><span data-stu-id="3c31d-125">There are two types of activity reports in Azure Active Directory:</span></span>

- <span data-ttu-id="3c31d-126">**A naplók** – hello [auditnaplókat tevékenységgel kapcsolatos jelentés](active-directory-reporting-activity-audit-logs.md) biztosít hozzáférést toohello előzményeit minden feladat az Ön bérelt szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="3c31d-126">**Audit logs** - hello [audit logs activity report](active-directory-reporting-activity-audit-logs.md) provides you with access toohello history of every task performed in your tenant.</span></span>

- <span data-ttu-id="3c31d-127">**Bejelentkezések** – hello a [bejelentkezések tevékenységgel kapcsolatos jelentés](active-directory-reporting-activity-sign-ins.md), meghatározhatja, akik hello naplózási naplók jelentés által jelentett hello feladatok hajtott végre.</span><span class="sxs-lookup"><span data-stu-id="3c31d-127">**Sign-ins** -  With hello [sign-ins activity report](active-directory-reporting-activity-sign-ins.md), you can determine, who has performed hello tasks reported by hello audit logs report.</span></span>



<span data-ttu-id="3c31d-128">Hello **auditnaplókat jelentés** biztosít megfelelőségi rendszer tevékenységeket rögzíti.</span><span class="sxs-lookup"><span data-stu-id="3c31d-128">hello **audit logs report** provides you with records of system activities for compliance.</span></span>
<span data-ttu-id="3c31d-129">Többek között a hello megadott adatok lehetővé teszi tooaddress gyakori forgatókönyvek például:</span><span class="sxs-lookup"><span data-stu-id="3c31d-129">Amongst others, hello provided data enables you tooaddress common scenarios such as:</span></span>

- <span data-ttu-id="3c31d-130">Valaki hozzáfér a bérlők a kapott hozzáférési tooan felügyeleti csoportot.</span><span class="sxs-lookup"><span data-stu-id="3c31d-130">Someone in my tenant got access tooan admin group.</span></span> <span data-ttu-id="3c31d-131">Ki biztosított neki hozzáférést?</span><span class="sxs-lookup"><span data-stu-id="3c31d-131">Who gave them access?</span></span> 

- <span data-ttu-id="3c31d-132">Szeretném tooknow hello azoknak a felhasználóknak jelentkezik be egy adott alkalmazást szeretnék óta nemrég előkészítve hello app és tooknow szeretné, ha a helyes műveletet</span><span class="sxs-lookup"><span data-stu-id="3c31d-132">I want tooknow hello list of users signing into a specific app since I recently onboarded hello app and want tooknow if it’s doing well</span></span>

- <span data-ttu-id="3c31d-133">A kívánt tooknow hány jelszó alaphelyzetbe állítása folyamatban van a bérlő</span><span class="sxs-lookup"><span data-stu-id="3c31d-133">I want tooknow how many password resets are happening in my tenant</span></span>


<span data-ttu-id="3c31d-134">**Milyen az Azure AD licencet igényelnek tooaccess hello vizsgálati naplók jelentést?**</span><span class="sxs-lookup"><span data-stu-id="3c31d-134">**What Azure AD license do you need tooaccess hello audit logs report?**</span></span>  
<span data-ttu-id="3c31d-135">hello naplózási naplók jelentés érhető el szolgáltatásokat, amelyhez licenccel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3c31d-135">hello audit logs report is available for features for which you have licenses.</span></span> <span data-ttu-id="3c31d-136">Ha egy adott funkcióhoz licenccel rendelkezik, akkor is napló naplóadatok az access toohello.</span><span class="sxs-lookup"><span data-stu-id="3c31d-136">If you have a license for a specific feature, you also have access toohello audit log information for it.</span></span>

<span data-ttu-id="3c31d-137">További részletekért lásd: **hello ingyenes, a Basic és a Premium kiadásainak általánosan elérhető szolgáltatások összehasonlítása** a [Azure Active Directory szolgáltatásokat és képességeket](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span><span class="sxs-lookup"><span data-stu-id="3c31d-137">For more details, see **Comparing generally available features of hello Free, Basic, and Premium editions** in [Azure Active Directory features and capabilities](https://www.microsoft.com/cloud-platform/azure-active-directory-features).</span></span>   



<span data-ttu-id="3c31d-138">Hello **bejelentkezések tevékenységgel kapcsolatos jelentés** lehetővé teszi, hogy tootoofind megválaszolja tooquestions, mint:</span><span class="sxs-lookup"><span data-stu-id="3c31d-138">hello **sign-ins activity report** enables tootoofind answers tooquestions such as:</span></span>

- <span data-ttu-id="3c31d-139">Mi az a hello bejelentkezési minta egy olyan felhasználó?</span><span class="sxs-lookup"><span data-stu-id="3c31d-139">What is hello sign-in pattern of a user?</span></span>
- <span data-ttu-id="3c31d-140">Hány felhasználó jelentkezett be egy adott héten?</span><span class="sxs-lookup"><span data-stu-id="3c31d-140">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="3c31d-141">Mi az a bejelentkezéseket hello állapotának?</span><span class="sxs-lookup"><span data-stu-id="3c31d-141">What’s hello status of these sign-ins?</span></span>


<span data-ttu-id="3c31d-142">**Milyen az Azure AD licencet elvégzi kell tooaccess hello bejelentkezések tevékenységgel kapcsolatos jelentés?**</span><span class="sxs-lookup"><span data-stu-id="3c31d-142">**What Azure AD license do you need tooaccess hello sign-ins activity report?**</span></span>  
<span data-ttu-id="3c31d-143">tooaccess hello bejelentkezések tevékenységgel kapcsolatos jelentés, a bérlő rendelkeznie kell egy hozzá társított Azure AD Premium licenchez.</span><span class="sxs-lookup"><span data-stu-id="3c31d-143">tooaccess hello sign-ins activity report, your tenant must have an Azure AD Premium license associated with it.</span></span>


## <a name="programmatic-access"></a><span data-ttu-id="3c31d-144">Szoftveres hozzáférés</span><span class="sxs-lookup"><span data-stu-id="3c31d-144">Programmatic access</span></span>

<span data-ttu-id="3c31d-145">Továbbá toohello felhasználói felületén, Azure Active Directory reporting is biztosít [programozott hozzáférés](active-directory-reporting-api-getting-started-azure-portal.md) toohello jelentésadatait.</span><span class="sxs-lookup"><span data-stu-id="3c31d-145">In addition toohello user interface, Azure Active Directory reporting also provides you with [programmatic access](active-directory-reporting-api-getting-started-azure-portal.md) toohello reporting data.</span></span> <span data-ttu-id="3c31d-146">Ezek a jelentések adatainak hello lehet nagyon hasznos tooyour alkalmazások, például a SIEM rendszerek, naplózási és üzleti intelligencia eszközök.</span><span class="sxs-lookup"><span data-stu-id="3c31d-146">hello data of these reports can be very useful tooyour applications, such as SIEM systems, audit, and business intelligence tools.</span></span> <span data-ttu-id="3c31d-147">hello Azure AD jelentéskészítési API-k olyan programozott hozzáférés toohello adatokat REST-alapú API-készlet.</span><span class="sxs-lookup"><span data-stu-id="3c31d-147">hello Azure AD reporting APIs provide programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="3c31d-148">Különböző programnyelvekkel és eszközökkel hívhatja ezeket az API-kat.</span><span class="sxs-lookup"><span data-stu-id="3c31d-148">You can call these APIs from a variety of programming languages and tools.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="3c31d-149">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c31d-149">Next steps</span></span>

<span data-ttu-id="3c31d-150">Ha többet hello tooknow különböző jelentéstípusok az Azure Active Directoryban, lásd:</span><span class="sxs-lookup"><span data-stu-id="3c31d-150">If you want tooknow more about hello various report types in Azure Active Directory, see:</span></span>

- [<span data-ttu-id="3c31d-151">Kockázatosként megjelölt felhasználók jelentés</span><span class="sxs-lookup"><span data-stu-id="3c31d-151">Users flagged for risk report</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="3c31d-152">Kockázatos bejelentkezések jelentés</span><span class="sxs-lookup"><span data-stu-id="3c31d-152">Risky sign-ins report</span></span>](active-directory-reporting-security-risky-sign-ins.md)
- [<span data-ttu-id="3c31d-153">Naplók jelentés</span><span class="sxs-lookup"><span data-stu-id="3c31d-153">Audit logs report</span></span>](active-directory-reporting-activity-audit-logs.md)
- [<span data-ttu-id="3c31d-154">Bejelentkezési naplók jelentés</span><span class="sxs-lookup"><span data-stu-id="3c31d-154">Sign-ins logs report</span></span>](active-directory-reporting-activity-sign-ins.md)

<span data-ttu-id="3c31d-155">Ha azt szeretné, hogy a jelentés adatainak hello reporting API használatával hello hello elérésével kapcsolatos további tooknow, lásd:</span><span class="sxs-lookup"><span data-stu-id="3c31d-155">If you want tooknow more about accessing hello hello reporting data using hello reporting API, see:</span></span> 

- [<span data-ttu-id="3c31d-156">Ismerkedés az Azure Active Directory reporting API-val hello</span><span class="sxs-lookup"><span data-stu-id="3c31d-156">Getting started with hello Azure Active Directory reporting API</span></span>](active-directory-reporting-api-getting-started-azure-portal.md)


<!--Image references-->
[1]: ./media/active-directory-reporting-azure-portal/ic195031.png
---
title: "aaaRisky bejelentkezések jelentés hello Azure Active Directory portálon |} Microsoft Docs"
description: "További tudnivalók: hello kockázatos bejelentkezések jelentés hello Azure Active Directory portálon"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: d8df5cafea6b38f3e364c24a6aff599abe088e88
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="risky-sign-ins-report-in-hello-azure-active-directory-portal"></a><span data-ttu-id="47834-103">Kockázatos bejelentkezések jelentés hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="47834-103">Risky sign-ins report in hello Azure Active Directory portal</span></span>

<span data-ttu-id="47834-104">Hello biztonsági jelentések segítségével az Azure Active Directory (Azure AD) akkor is kaphat a hello valószínűségértékének inverzét feltört felhasználói fiókok a környezetben.</span><span class="sxs-lookup"><span data-stu-id="47834-104">With hello security reports in Azure Active Directory (Azure AD) you can gain insights into hello probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="47834-105">Az Azure AD kapcsolódó tooyour felhasználói fiókok gyanús műveleteket észleli.</span><span class="sxs-lookup"><span data-stu-id="47834-105">Azure AD detects suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="47834-106">A rendszer minden egyes észlelt tevékenység esetében létrehoz egy *kockázati esemény* nevű rekordot.</span><span class="sxs-lookup"><span data-stu-id="47834-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="47834-107">További részletek: [Az Azure Active Directory kockázati eseményei](active-directory-identity-protection-risk-events.md).</span><span class="sxs-lookup"><span data-stu-id="47834-107">For more details, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="47834-108">hello észlelt kockázati események használt toocalculate:</span><span class="sxs-lookup"><span data-stu-id="47834-108">hello detected risk events are used toocalculate:</span></span>

- <span data-ttu-id="47834-109">**Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója.</span><span class="sxs-lookup"><span data-stu-id="47834-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="47834-110">További részletek: [Kockázatos bejelentkezések](active-directory-identityprotection.md#risky-sign-ins).</span><span class="sxs-lookup"><span data-stu-id="47834-110">For more details, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="47834-111">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="47834-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="47834-112">További részletek: [Kockázatosként megjelölt felhasználók](active-directory-identityprotection.md#users-flagged-for-risk).</span><span class="sxs-lookup"><span data-stu-id="47834-112">For more details, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="47834-113">A [hello Azure-portálon](https://portal.azure.com), hello biztonsági jelentések hello található **Azure Active Directory** hello paneljén **biztonsági** szakasz.</span><span class="sxs-lookup"><span data-stu-id="47834-113">In [hello Azure portal](https://portal.azure.com), you can find hello security reports on hello **Azure Active Directory** blade in hello **Security** section.</span></span> 

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/10.png)


## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a><span data-ttu-id="47834-115">Milyen az Azure AD licencet kell tooaccess biztonsági jelentést?</span><span class="sxs-lookup"><span data-stu-id="47834-115">What Azure AD license do you need tooaccess a security report?</span></span>  

<span data-ttu-id="47834-116">Az Azure Active Directory minden kiadása biztosítja a kockázatos bejelentkezések jelentéseit.</span><span class="sxs-lookup"><span data-stu-id="47834-116">All editions of Azure Active Directory provide you with risky sign-ins reports.</span></span>  
<span data-ttu-id="47834-117">Azonban a jelentések részletességét hello szintű platformjától függően eltérő hello verziója esetén:</span><span class="sxs-lookup"><span data-stu-id="47834-117">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="47834-118">A hello **Azure Active Directory szabad és alapszintű kiadásai**, kockázatos bejelentkezések listájának már beolvasása.</span><span class="sxs-lookup"><span data-stu-id="47834-118">In hello **Azure Active Directory Free and Basic editions**, you already get a list of risky sign-ins.</span></span> 

- <span data-ttu-id="47834-119">Hello **Azure Active Directory Premium 1** edition kiterjeszti a modell is így már tooexamine néhány hello az alapul szolgáló kockázati eseményekről, amelyek az egyes jelentések vonatkozóan nem észlelhetők.</span><span class="sxs-lookup"><span data-stu-id="47834-119">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="47834-120">Hello **Azure Active Directory Premium 2** kiadás hello a leginkább részletes információk minden alapul szolgáló kockázati eseményekről, és emellett lehetővé teszi a tooconfigure biztonsági házirendek, amelyek automatikusan reagálnak tooconfigured tartalmazza kockázati szintek.</span><span class="sxs-lookup"><span data-stu-id="47834-120">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about all underlying risk events and it also enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="47834-121">Azure Active Directory – ingyenes és alapszintű kiadások</span><span class="sxs-lookup"><span data-stu-id="47834-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="47834-122">Azure Active Directory ingyenes hello és alapszintű kiadásai biztosít kockázatos bejelentkezések észlelt listáját a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="47834-122">hello Azure Active Directory free and basic editions provide you with a list of risky sign-ins that have been detected for your users.</span></span> <span data-ttu-id="47834-123">Ez a jelentés a következőket listázza:</span><span class="sxs-lookup"><span data-stu-id="47834-123">This report lists:</span></span>

- <span data-ttu-id="47834-124">**Felhasználói** – hello hello bejelentkezési művelet során használt hello felhasználó neve</span><span class="sxs-lookup"><span data-stu-id="47834-124">**User** - hello name of hello user that was used during hello sign-in operation</span></span>
- <span data-ttu-id="47834-125">**IP** -hello használt tooconnect tooAzure Active Directory hello eszköz IP-címe</span><span class="sxs-lookup"><span data-stu-id="47834-125">**IP** - hello IP address of hello device that was used tooconnect tooAzure Active Directory</span></span>
- <span data-ttu-id="47834-126">**Hely** -hello helyet használja az Active Directory tooconnect tooAzure</span><span class="sxs-lookup"><span data-stu-id="47834-126">**Location** - hello location used tooconnect tooAzure Active Directory</span></span>
- <span data-ttu-id="47834-127">**Bejelentkezés idejét** -hello idő, amikor hello bejelentkezés történt meg</span><span class="sxs-lookup"><span data-stu-id="47834-127">**Sign-in time** - hello time when hello sign-in was performed</span></span>
- <span data-ttu-id="47834-128">**Állapot** -hello hello bejelentkezés állapota</span><span class="sxs-lookup"><span data-stu-id="47834-128">**Status** - hello status of hello sign-in</span></span>


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/01.png)

<span data-ttu-id="47834-130">A vizsgálni hello kockázatos bejelentkezés alapján, megadhatja a visszajelzés tooAzure Active Directory-hello műveletek a következő formában:</span><span class="sxs-lookup"><span data-stu-id="47834-130">Based on your investigation of hello risky sign-in, you can provide feedback tooAzure Active Directory in form of hello following actions:</span></span>

- <span data-ttu-id="47834-131">Feloldás</span><span class="sxs-lookup"><span data-stu-id="47834-131">Resolve</span></span>
- <span data-ttu-id="47834-132">Megjelölés téves riasztásként</span><span class="sxs-lookup"><span data-stu-id="47834-132">Mark as false positive</span></span>
- <span data-ttu-id="47834-133">Kihagyás</span><span class="sxs-lookup"><span data-stu-id="47834-133">Ignore</span></span>
- <span data-ttu-id="47834-134">Újraaktiválás</span><span class="sxs-lookup"><span data-stu-id="47834-134">Reactivate</span></span>

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/21.png)

<span data-ttu-id="47834-136">További részletek: [Kockázati események manuális lezárása](active-directory-identityprotection.md#closing-risk-events-manually).</span><span class="sxs-lookup"><span data-stu-id="47834-136">For more details, see [Closing risk events manually](active-directory-identityprotection.md#closing-risk-events-manually).</span></span>

<span data-ttu-id="47834-137">Ez a jelentés a következő lehetőségeket kínálja:</span><span class="sxs-lookup"><span data-stu-id="47834-137">This report provides you with an option to:</span></span>

- <span data-ttu-id="47834-138">Erőforrások keresése</span><span class="sxs-lookup"><span data-stu-id="47834-138">Searh resources</span></span>
- <span data-ttu-id="47834-139">Hello jelentés adatok letöltése</span><span class="sxs-lookup"><span data-stu-id="47834-139">Download hello report data</span></span>


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="47834-141">Azure Active Directory – prémium szintű kiadások</span><span class="sxs-lookup"><span data-stu-id="47834-141">Azure Active Directory premium editions</span></span>

<span data-ttu-id="47834-142">hello kockázatos bejelentkezések jelentés hello Azure Active Directory prémium kiadásaiban biztosítja:</span><span class="sxs-lookup"><span data-stu-id="47834-142">hello risky sign-ins report in hello Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="47834-143">Hello kapcsolatos információk összesítése [eseménytípusok kockázati](active-directory-identity-protection-risk-events.md) észlelt</span><span class="sxs-lookup"><span data-stu-id="47834-143">Aggregated information about hello [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="47834-144">Egy beállítás toodownload hello jelentés</span><span class="sxs-lookup"><span data-stu-id="47834-144">An option toodownload hello report</span></span>


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/456.png)


<span data-ttu-id="47834-146">Egy kockázati esemény kiválasztásakor megkapja az esemény részletes jelentési nézetét, amely a következőket teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="47834-146">When you select a risk event, you get a detailed report view for this risk event that enables you to:</span></span>

- <span data-ttu-id="47834-147">Egy beállítás tooconfigure egy [felhasználói kockázat szervizelési házirend](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="47834-147">An option tooconfigure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  

- <span data-ttu-id="47834-148">Tekintse át a hello észlelési ütemtervét hello kockázat esemény</span><span class="sxs-lookup"><span data-stu-id="47834-148">Review hello detection timeline for hello risk event</span></span>  

- <span data-ttu-id="47834-149">Azon felhasználók listájának áttekintését, amelyeknél a rendszer kockázati esemény észlelt</span><span class="sxs-lookup"><span data-stu-id="47834-149">Review a list of users for which this risk event has been detected</span></span>

- <span data-ttu-id="47834-150">[A kockázati események manuális bezárását](active-directory-identityprotection.md#closing-risk-events-manually) és a manuálisan bezárt kockázati események újbóli aktiválását.</span><span class="sxs-lookup"><span data-stu-id="47834-150">[Manually close risk events](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/457.png)

<span data-ttu-id="47834-152">Egy felhasználó kiválasztásakor megkapja a felhasználó részletes jelentési nézetét, amely a következőket teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="47834-152">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="47834-153">Nyissa meg a hello összes bejelentkezések megtekintése</span><span class="sxs-lookup"><span data-stu-id="47834-153">Open hello All sign-ins view</span></span>

- <span data-ttu-id="47834-154">Hello felhasználói jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="47834-154">Reset hello user's password</span></span>

- <span data-ttu-id="47834-155">Az összes esemény elvetését</span><span class="sxs-lookup"><span data-stu-id="47834-155">Dismiss all events</span></span>

- <span data-ttu-id="47834-156">Vizsgálja meg a hello felhasználóhoz jelentett kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="47834-156">Investigate reported risk events for hello user.</span></span> 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/324.png)


<span data-ttu-id="47834-158">a kockázat esemény tooinvestigate válasszon egyet a hello listából.</span><span class="sxs-lookup"><span data-stu-id="47834-158">tooinvestigate a risk event, select one from hello list.</span></span>  
<span data-ttu-id="47834-159">Ekkor megnyílik a hello **részletek** panel kockázat eseményhez.</span><span class="sxs-lookup"><span data-stu-id="47834-159">This opens hello **Details** blade for this risk event.</span></span> <span data-ttu-id="47834-160">A hello **részletek** panelen hello beállítás tooeither rendelkezik [kézzel zárja be a kockázati események](active-directory-identityprotection.md#closing-risk-events-manually) vagy egy manuális lezárt kockázat esemény újraaktiválásához.</span><span class="sxs-lookup"><span data-stu-id="47834-160">On hello **Details** blade, you have hello option tooeither [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-risky-sign-ins/325.png)





## <a name="next-steps"></a><span data-ttu-id="47834-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47834-162">Next steps</span></span>

- <span data-ttu-id="47834-163">Az Azure Active Directory Identity Protectionnel kapcsolatos további részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="47834-163">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>


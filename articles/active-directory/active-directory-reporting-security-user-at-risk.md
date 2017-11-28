---
title: "aaaUsers meg van jelölve, a kockázat biztonsági jelentés hello Azure Active Directory portálon |} Microsoft Docs"
description: "További tudnivalók: hello felhasználók megjelölt kockázat biztonsági jelentés hello Azure Active Directory portálon"
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 5077cd61d6119745a85ed712623904633a151331
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="users-flagged-for-risk-security-report-in-hello-azure-active-directory-portal"></a><span data-ttu-id="3923a-103">Felhasználók meg van jelölve, a kockázat biztonsági jelentés hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="3923a-103">Users flagged for risk security report in hello Azure Active Directory portal</span></span>

<span data-ttu-id="3923a-104">Hello biztonsági jelentésekkel hello Azure Active Directory (Azure AD) akkor is kaphat a hello valószínűségértékének inverzét feltört felhasználói fiókok a környezetben.</span><span class="sxs-lookup"><span data-stu-id="3923a-104">With hello security reports in hello Azure Active Directory (Azure AD), you can gain insights into hello probability of compromised user accounts in your environment.</span></span> 

<span data-ttu-id="3923a-105">Az Azure Active Directory kapcsolódó tooyour felhasználói fiókok gyanús műveleteket észleli.</span><span class="sxs-lookup"><span data-stu-id="3923a-105">Azure Active Directory detects suspicious actions that are related tooyour user accounts.</span></span> <span data-ttu-id="3923a-106">A rendszer minden egyes észlelt tevékenység esetében létrehoz egy *kockázati esemény* nevű rekordot.</span><span class="sxs-lookup"><span data-stu-id="3923a-106">For each detected action, a record called *risk event* is created.</span></span> <span data-ttu-id="3923a-107">További információkért tekintse át [Az Azure Active Directory kockázati eseményeivel](active-directory-identity-protection-risk-events.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="3923a-107">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span> 

<span data-ttu-id="3923a-108">hello észlelt kockázati események használt toocalculate:</span><span class="sxs-lookup"><span data-stu-id="3923a-108">hello detected risk events are used toocalculate:</span></span>

- <span data-ttu-id="3923a-109">**Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója.</span><span class="sxs-lookup"><span data-stu-id="3923a-109">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="3923a-110">További információkért lásd a [kockázatos bejelentkezésekkel](active-directory-identityprotection.md#risky-sign-ins) foglalkozó részt.</span><span class="sxs-lookup"><span data-stu-id="3923a-110">For more information, see [Risky sign-ins](active-directory-identityprotection.md#risky-sign-ins).</span></span> 

- <span data-ttu-id="3923a-111">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="3923a-111">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="3923a-112">További információkért lásd a [kockázatosként megjelölt felhasználókkal](active-directory-identityprotection.md#users-flagged-for-risk) foglalkozó részt.</span><span class="sxs-lookup"><span data-stu-id="3923a-112">For more information, see [Users flagged for risk](active-directory-identityprotection.md#users-flagged-for-risk).</span></span>  

<span data-ttu-id="3923a-113">Hello Azure-portálon, az hello biztonsági jelentések hello megtalálhatja **Azure Active Directory** hello paneljén **biztonsági** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3923a-113">In hello Azure portal, you can find hello security reports on hello **Azure Active Directory** blade in hello **Security** section.</span></span>  

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/10.png)



## <a name="what-azure-ad-license-do-you-need-tooaccess-a-security-report"></a><span data-ttu-id="3923a-115">Milyen az Azure AD licencet kell tooaccess biztonsági jelentést?</span><span class="sxs-lookup"><span data-stu-id="3923a-115">What Azure AD license do you need tooaccess a security report?</span></span>  

<span data-ttu-id="3923a-116">Az Azure Active Directory minden kiadása biztosítja a kockázatosként megjelölt felhasználók jelentéseit.</span><span class="sxs-lookup"><span data-stu-id="3923a-116">All editions of Azure Active Directory provide you with users flagged for risk reports.</span></span>  
<span data-ttu-id="3923a-117">Azonban a jelentések részletességét hello szintű platformjától függően eltérő hello verziója esetén:</span><span class="sxs-lookup"><span data-stu-id="3923a-117">However, hello level of report granularity varies between hello editions:</span></span> 

- <span data-ttu-id="3923a-118">A hello **Azure Active Directory szabad és alapszintű kiadásai**, elérhetővé már meg van jelölve a kockázat felhasználók listáját.</span><span class="sxs-lookup"><span data-stu-id="3923a-118">In hello **Azure Active Directory Free and Basic editions**, you already get a list of users flagged for risk.</span></span> 

- <span data-ttu-id="3923a-119">Hello **Azure Active Directory Premium 1** edition kiterjeszti a modell is így már tooexamine néhány hello az alapul szolgáló kockázati eseményekről, amelyek az egyes jelentések vonatkozóan nem észlelhetők.</span><span class="sxs-lookup"><span data-stu-id="3923a-119">hello **Azure Active Directory Premium 1** edition extends this model by also enabling you tooexamine some of hello underlying risk events that have been detected for each report.</span></span> 

- <span data-ttu-id="3923a-120">Hello **Azure Active Directory Premium 2** kiadás tartalmazza a hello minden alapul szolgáló kockázati események részletesebb információt, és lehetővé teszi a tooconfigure biztonsági házirendek, amelyek automatikusan reagálnak tooconfigured kockázata szintek.</span><span class="sxs-lookup"><span data-stu-id="3923a-120">hello **Azure Active Directory Premium 2** edition provides you with hello most detailed information about all underlying risk events and enables you tooconfigure security policies that automatically respond tooconfigured risk levels.</span></span>



## <a name="azure-active-directory-free-and-basic-edition"></a><span data-ttu-id="3923a-121">Azure Active Directory – ingyenes és alapszintű kiadások</span><span class="sxs-lookup"><span data-stu-id="3923a-121">Azure Active Directory free and basic edition</span></span>

<span data-ttu-id="3923a-122">meg van jelölve a kockázat jelentés hello Azure Active Directory ingyenes és alapszintű kiadásai hello felhasználók tesz lehetővé a felhasználói fiókokat, előfordulhat, hogy sérült listáját.</span><span class="sxs-lookup"><span data-stu-id="3923a-122">hello users flagged for risk report in hello Azure Active Directory free and basic editions provides you with a list of user accounts that may have been compromised.</span></span> 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/03.png)

<span data-ttu-id="3923a-124">Felhasználó megadásával hello kapcsolódó felhasználói adatok paneljének megnyitása.</span><span class="sxs-lookup"><span data-stu-id="3923a-124">Selecting a user opens hello related user data blade.</span></span>
<span data-ttu-id="3923a-125">A felhasználók számára, az érintettek áttekintheti a hello felhasználó bejelentkezési előzményei és hello jelszó alaphelyzetbe állítása, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="3923a-125">For users that are at risk, you can review hello user’s sign-in history and reset hello password if necessary.</span></span>

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/46.png)


<span data-ttu-id="3923a-127">Ez a párbeszédablak a következő lehetőségeket kínálja:</span><span class="sxs-lookup"><span data-stu-id="3923a-127">This dialog provides you with an option to:</span></span>

- <span data-ttu-id="3923a-128">Töltse le a hello jelentés</span><span class="sxs-lookup"><span data-stu-id="3923a-128">Download hello report</span></span>

- <span data-ttu-id="3923a-129">Felhasználók keresése</span><span class="sxs-lookup"><span data-stu-id="3923a-129">Search users</span></span>

![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/16.png)


## <a name="azure-active-directory-premium-editions"></a><span data-ttu-id="3923a-131">Azure Active Directory – prémium szintű kiadások</span><span class="sxs-lookup"><span data-stu-id="3923a-131">Azure Active Directory premium editions</span></span>

<span data-ttu-id="3923a-132">meg van jelölve a kockázat jelentés hello Azure Active Directory prémium kiadásaiban hello felhasználók biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3923a-132">hello users flagged for risk report in hello Azure Active Directory premium editions provides you with:</span></span>

- <span data-ttu-id="3923a-133">Egy [lista azokról a felhasználói fiókokról](active-directory-identityprotection.md#users-flagged-for-risk), amelyeknek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="3923a-133">A [list of user accounts](active-directory-identityprotection.md#users-flagged-for-risk) that may have been compromised</span></span> 

- <span data-ttu-id="3923a-134">Hello kapcsolatos információk összesítése [eseménytípusok kockázati](active-directory-identity-protection-risk-events.md) észlelt</span><span class="sxs-lookup"><span data-stu-id="3923a-134">Aggregated information about hello [risk event types](active-directory-identity-protection-risk-events.md) that have been detected</span></span>

- <span data-ttu-id="3923a-135">Egy beállítás toodownload hello jelentés</span><span class="sxs-lookup"><span data-stu-id="3923a-135">An option toodownload hello report</span></span>

- <span data-ttu-id="3923a-136">Egy beállítás tooconfigure egy [felhasználói kockázat szervizelési házirend](active-directory-identityprotection.md#user-risk-security-policy)</span><span class="sxs-lookup"><span data-stu-id="3923a-136">An option tooconfigure a [user risk remediation policy](active-directory-identityprotection.md#user-risk-security-policy)</span></span>  


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/71.png)

<span data-ttu-id="3923a-138">Egy felhasználó kiválasztásakor megkapja a felhasználó részletes jelentési nézetét, amely a következőket teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="3923a-138">When you select a user, you get a detailed report view for this user that enables you to:</span></span>

- <span data-ttu-id="3923a-139">Nyissa meg a hello összes bejelentkezések megtekintése</span><span class="sxs-lookup"><span data-stu-id="3923a-139">Open hello All sign-ins view</span></span>

- <span data-ttu-id="3923a-140">Hello felhasználói jelszó alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="3923a-140">Reset hello user's password</span></span>

- <span data-ttu-id="3923a-141">Az összes esemény elvetését</span><span class="sxs-lookup"><span data-stu-id="3923a-141">Dismiss all events</span></span>

- <span data-ttu-id="3923a-142">Vizsgálja meg a hello felhasználóhoz jelentett kockázati eseményekről.</span><span class="sxs-lookup"><span data-stu-id="3923a-142">Investigate reported risk events for hello user.</span></span> 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/324.png)


<span data-ttu-id="3923a-144">a kockázat esemény tooinvestigate válasszon egyet a hello lista tooopen hello **részletek** panel kockázat eseményhez.</span><span class="sxs-lookup"><span data-stu-id="3923a-144">tooinvestigate a risk event, select one from hello list tooopen hello **Details** blade for this risk event.</span></span> <span data-ttu-id="3923a-145">A hello **részletek** panelen hello beállítás tooeither rendelkezik [kézzel zárja be a kockázati események](active-directory-identityprotection.md#closing-risk-events-manually) vagy egy manuális lezárt kockázat esemény újraaktiválásához.</span><span class="sxs-lookup"><span data-stu-id="3923a-145">On hello **Details** blade, you have hello option tooeither [manually close a risk event](active-directory-identityprotection.md#closing-risk-events-manually) or reactivate a manually closed risk event.</span></span> 


![Kockázatos bejelentkezések](./media/active-directory-reporting-security-user-at-risk/325.png)



## <a name="next-steps"></a><span data-ttu-id="3923a-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3923a-147">Next steps</span></span>

- <span data-ttu-id="3923a-148">Az Azure Active Directory Identity Protectionnel kapcsolatos további részletekért lásd: [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span><span class="sxs-lookup"><span data-stu-id="3923a-148">For more information about Azure Active Directory Identity Protection, see [Azure Active Directory Identity Protection](active-directory-identityprotection.md).</span></span>


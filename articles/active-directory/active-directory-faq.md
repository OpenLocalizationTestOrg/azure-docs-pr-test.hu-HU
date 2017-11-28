---
title: "aaaAzure Active Directory – gyakori kérdések |} Microsoft Docs"
description: "Az Azure Active Directory GYIK hogyan tooaccess Azure és Azure Active Directory, a jelszókezelés és alkalmazás-hozzáférési kapcsolatos kérdésekre ad."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: b8207760-9714-4871-93d5-f9893de31c8f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/16/2017
ms.author: markvi
ms.openlocfilehash: 63c30c4aeda4551bf02c6b968f98cded5a3b2c16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-faq"></a><span data-ttu-id="b1fc7-103">Azure Active Directory – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b1fc7-103">Azure Active Directory FAQ</span></span>
<span data-ttu-id="b1fc7-104">Az Azure Active Directory (Azure AD) egy átfogó szolgáltatott identitási (IDaaS) megoldás, amely az identitások, a hozzáférés-kezelés és a biztonság minden szempontját lefedi.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-104">Azure Active Directory (Azure AD) is a comprehensive identity as a service (IDaaS) solution that spans all aspects of identity, access management, and security.</span></span>

<span data-ttu-id="b1fc7-105">További információkért lásd: [Mi az az Azure Active Directory?](active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-105">For more information, see [What is Azure Active Directory?](active-directory-whatis.md).</span></span>


## <a name="access-azure-and-azure-active-directory"></a><span data-ttu-id="b1fc7-106">Az Azure és az Azure Active Directory elérése</span><span class="sxs-lookup"><span data-stu-id="b1fc7-106">Access Azure and Azure Active Directory</span></span>
<span data-ttu-id="b1fc7-107">**K: Miért kapok "nem találhatók előfizetések" jelenik meg, hogy a klasszikus Azure portálon hello Azure AD tooaccess?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-107">**Q: Why do I get “No subscriptions found” when I try tooaccess Azure AD in hello Azure classic portal?**</span></span>

<span data-ttu-id="b1fc7-108">**V:** tooaccess hello a klasszikus Azure portálon, minden felhasználó számára szükséges engedélyek Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-108">**A:** tooaccess hello Azure classic portal, each user needs permissions with an Azure subscription.</span></span> <span data-ttu-id="b1fc7-109">Ha fizetős Office 365 vagy Azure AD-előfizetéssel rendelkezik, nyissa meg túl[http://aka.ms/accessAAD](http://aka.ms/accessAAD) egy egyszeri aktiváláshoz a.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-109">If you have a paid Office 365 or Azure AD subscription, go too[http://aka.ms/accessAAD](http://aka.ms/accessAAD) for a one-time activation step.</span></span> <span data-ttu-id="b1fc7-110">Ellenkező esetben szüksége lesz egy ingyenes tooactivate [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) vagy egy fizetős előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-110">Otherwise, you will need tooactivate a free [Azure account](https://azure.microsoft.com/pricing/free-trial/) or a paid subscription.</span></span>

<span data-ttu-id="b1fc7-111">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b1fc7-111">For more information, see:</span></span>

* [<span data-ttu-id="b1fc7-112">How Azure subscriptions are associated with Active Directory? (Hogyan kapcsolódnak az Azure-előfizetések az Azure Active Directory-hoz?)</span><span class="sxs-lookup"><span data-stu-id="b1fc7-112">How Azure subscriptions are associated with Azure Active Directory</span></span>](active-directory-how-subscriptions-associated-directory.md)
* [<span data-ttu-id="b1fc7-113">Az Office 365-előfizetéshez az Azure-ban hello címtár kezelése</span><span class="sxs-lookup"><span data-stu-id="b1fc7-113">Manage hello directory for your Office 365 subscription in Azure</span></span>](active-directory-manage-o365-subscription.md)

- - -
<span data-ttu-id="b1fc7-114">**K: Mi az hello kapcsolat az Azure AD között az Office 365 és Azure?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-114">**Q: What’s hello relationship between Azure AD, Office 365, and Azure?**</span></span>

<span data-ttu-id="b1fc7-115">**V:** az Azure AD biztosít közös identitás- és hozzáférés lehetőségeket tooall webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-115">**A:** Azure AD provides you with common identity and access capabilities tooall web services.</span></span> <span data-ttu-id="b1fc7-116">Akár az Office 365, Microsoft Azure, Intune-ban, vagy mások számára, akkor most már használja az Azure AD toohelp kapcsolja be a szolgáltatások a bejelentkezéshez és a hozzáférés-kezelés.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-116">Whether you are using Office 365, Microsoft Azure, Intune, or others, you're already using Azure AD toohelp turn on sign-on and access management for all these services.</span></span>

<span data-ttu-id="b1fc7-117">Minden felhasználó, aki toouse webszolgáltatások be vannak állítva az egy vagy több Azure AD-példányban található felhasználói fiókok vannak meghatározva.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-117">All users who are set up toouse web services are defined as user accounts in one or more Azure AD instances.</span></span> <span data-ttu-id="b1fc7-118">Beállíthatja ezeket a fiókokat az ingyenes Azure AD-képességekhez, például a felhőalkalmazások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-118">You can set up these accounts for free Azure AD capabilities like cloud application access.</span></span>

<span data-ttu-id="b1fc7-119">A fizetős Azure AD szolgáltatások, például az Enterprise Mobility + Security, átfogó vállalati méretű felügyeleti és biztonsági megoldásokkal egészítenek ki más webes szolgáltatásokat, például az Office 365-öt és a Microsoft Azure-t.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-119">Azure AD paid services like Enterprise Mobility + Security complement other web services like Office 365 and Microsoft Azure with comprehensive enterprise-scale management and security solutions.</span></span>
- - -
<span data-ttu-id="b1fc7-120">**K: miért lehet jelentkezzen be Azure-portálon toohello azonban nem hello a klasszikus Azure portálon?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-120">**Q:  Why can I sign in toohello Azure portal but not hello Azure classic portal?**</span></span>

<span data-ttu-id="b1fc7-121">**V:** hello Azure-portál nem érvényes előfizetés szükséges, és a klasszikus portálon hello érvényes előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-121">**A:**  hello Azure portal does not require a valid subscription, and hello classic portal does require a valid subscription.</span></span>  <span data-ttu-id="b1fc7-122">Ha nem rendelkezik előfizetéssel, nem tud bejelentkezni a klasszikus portálon toohello.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-122">If you do not have a subscription, you can't sign in toohello classic portal.</span></span>
- - -
<span data-ttu-id="b1fc7-123">**K: Mik előfizetési rendszergazda, és a Directory-rendszergazda hello különbségei?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-123">**Q:  What are hello differences between Subscription Administrator and Directory Administrator?**</span></span>

<span data-ttu-id="b1fc7-124">**V:** alapértelmezés szerint akkor hello előfizetés rendszergazdai szerepkörrel feliratkozás az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-124">**A:** By default, you are assigned hello Subscription Administrator role when you sign up for Azure.</span></span> <span data-ttu-id="b1fc7-125">Előfizetés-adminisztrátor használhatja, vagy a Microsoft-fiókkal, vagy a munkahelyi vagy iskolai fiókkal, amely az Azure-előfizetés hello hello könyvtárból társítva.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-125">A subscription admin can use either a Microsoft account or a work or school account from hello directory that hello Azure subscription is associated with.</span></span>  <span data-ttu-id="b1fc7-126">A szerepkör engedélyezett toomanage szolgáltatások hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-126">This role is authorized toomanage services in hello Azure portal.</span></span>

<span data-ttu-id="b1fc7-127">Ha mások toosign az kell, és szolgáltatások által használatával hello ugyanahhoz az előfizetéshez, mint a társadminisztrátoroknak hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-127">If others need toosign in and access services by using hello same subscription, you can add them as co-admins.</span></span> <span data-ttu-id="b1fc7-128">Ez a szerepkör rendelkezik hello azonos hello szolgáltatásadminisztrátoréval hozzáférési jogosultsággal, de nem módosíthatják az előfizetések tooAzure könyvtárak hello hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-128">This role has hello same access privileges as hello service admin, but can’t change hello association of subscriptions tooAzure directories.</span></span>  <span data-ttu-id="b1fc7-129">Az előfizetés rendszergazdái további információkért lásd: [hogyan tooadd, vagy módosítsa az Azure rendszergazdai szerepkörök](../billing-add-change-azure-subscription-administrator.md) és [kapcsolódnak hogyan Azure-előfizetések az Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-129">For additional information on subscription admins, see [How tooadd or change Azure administrator roles](../billing-add-change-azure-subscription-administrator.md) and [How Azure subscriptions are associated with Azure Active Directory](active-directory-how-subscriptions-associated-directory.md).</span></span>


<span data-ttu-id="b1fc7-130">Az Azure AD rendszergazdai szerepkörök toomanage hello directory és az identitás-kapcsolatos funkciók különböző szabálykészleteket rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-130">Azure AD has a different set of admin roles toomanage hello directory and identity-related features.</span></span>  <span data-ttu-id="b1fc7-131">Ezeket a rendszergazdák a fog hello Azure-portálon hozzáférési toovarious funkciókat tartalmaz, vagy a klasszikus Azure portálon hello.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-131">These admins will have access toovarious features in hello Azure portal or hello Azure classic portal.</span></span> <span data-ttu-id="b1fc7-132">Üdvözöljük a rendszergazdákat szerepkör meghatározza, hogy mit tehet, hasonló hozzon létre vagy szerkessze a felhasználók, tooothers rendszergazdai szerepkörök hozzárendelése, felhasználók új jelszavainak létrehozására, felhasználói licencek kezelése vagy tartományok kezelése.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-132">hello admin's role determines what they can do, like create or edit users, assign administrative roles tooothers, reset user passwords, manage user licenses, or manage domains.</span></span>  <span data-ttu-id="b1fc7-133">Az Azure AD címtárrendszergazdáival és azok szerepköreivel kapcsolatos tovább információkért lásd: [Rendszergazdai szerepkörök hozzárendelése az Azure Active Directoryban](active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-133">For additional information on Azure AD directory admins and their roles, see [Assigning administrator roles in Azure Active Directory](active-directory-assign-admin-roles.md).</span></span>

<span data-ttu-id="b1fc7-134">Ezenkívül a fizetős Azure AD szolgáltatások, például az Enterprise Mobility + Security, átfogó vállalati méretű felügyeleti és biztonsági megoldásokkal egészítenek ki más webes szolgáltatásokat, például az Office 365-öt és a Microsoft Azure-t.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-134">Additionally, Azure AD paid services like Enterprise Mobility + Security complement other web services, such as Office 365 and Microsoft Azure, with comprehensive enterprise-scale management and security solutions.</span></span>

- - -
<span data-ttu-id="b1fc7-135">**K: Létezik olyan jelentés, amely megmutatja, hogy mikor járnak le az Azure AD-beli felhasználói licenceim?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-135">**Q: Is there a report that shows when my Azure AD user licenses will expire?**</span></span>

<span data-ttu-id="b1fc7-136">**V.:** Nem.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-136">**A:** No.</span></span>  <span data-ttu-id="b1fc7-137">Ez jelenleg nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-137">This is not currently available.</span></span>

- - -

## <a name="get-started-with-hybrid-azure-ad"></a><span data-ttu-id="b1fc7-138">Első lépések a Hybrid Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b1fc7-138">Get started with Hybrid Azure AD</span></span>


<span data-ttu-id="b1fc7-139">**K: Hogyan hagyhatok el egy bérlőt, amikor közreműködőként vagyok hozzáadva?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-139">**Q: How do I leave a tenant when I am added as a collaborator?**</span></span>

<span data-ttu-id="b1fc7-140">**V:** tooanother szervezet bérlője kerülnek, közreműködő, használhatja hello "bérlői kapcsoló" hello felső jobb tooswitch a bérlők között.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-140">**A:** When you are added tooanother organization's tenant as a collaborator, you can use hello "tenant switcher" in hello upper right tooswitch between tenants.</span></span>  <span data-ttu-id="b1fc7-141">Jelenleg nincs módja tooleave hello felkéri a szervezet van, és a Microsoft dolgozik a funkció.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-141">Currently, there is no way tooleave hello inviting organization, and Microsoft is working on providing this functionality.</span></span>  <span data-ttu-id="b1fc7-142">Amíg ez a funkció nem érhető el, megkérheti a szervezet tooremove meghívása hello megtilthatja a bérlőben.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-142">Until this feature is available, you can ask hello inviting organization tooremove you from their tenant.</span></span>
- - -
<span data-ttu-id="b1fc7-143">**K: hogyan kapcsolódhatnak a helyszíni címtár tooAzure AD?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-143">**Q: How can I connect my on-premises directory tooAzure AD?**</span></span>

<span data-ttu-id="b1fc7-144">**V:** a helyszíni címtár tooAzure AD az Azure AD Connect használatával képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-144">**A:** You can connect your on-premises directory tooAzure AD by using Azure AD Connect.</span></span>

<span data-ttu-id="b1fc7-145">További információkért lásd: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-145">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="b1fc7-146">**K: Hogyan állíthatok be egyszeri bejelentkezést a helyszíni címtár és a felhőalkalmazásaim között?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-146">**Q: How do I set up SSO between my on-premises directory and my cloud applications?**</span></span>

<span data-ttu-id="b1fc7-147">**V:** csak akkor kell tooset be egyszeri bejelentkezést (SSO) a helyszíni címtár és az Azure AD között.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-147">**A:** You only need tooset up single sign-on (SSO) between your on-premises directory and Azure AD.</span></span> <span data-ttu-id="b1fc7-148">Mindaddig, amíg az Azure AD keresztül fér hozzá a felhőalapú alkalmazásokhoz, a hello szolgáltatás automatikusan átirányítja a felhasználókat, a helyszíni hitelesítő adataikkal hitelesítést toocorrectly.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-148">As long as you access your cloud applications through Azure AD, hello service automatically drives your users toocorrectly authenticate with their on-premises credentials.</span></span>

<span data-ttu-id="b1fc7-149">Az egyszeri bejelentkezés helyszínről végzett implementálása könnyen elérhető olyan összevonási megoldásokkal, mint az Active Directory összevonási szolgáltatások (AD FS), illetve a jelszókivonat-szinkronizálás konfigurálásával. Könnyedén telepítheti mindkét lehetőség hello Azure AD Connect konfigurációs varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-149">Implementing SSO from on-premises can be easily achieved with federation solutions such as Active Directory Federation Services (AD FS), or by configuring password hash sync. You can easily deploy both options by using hello Azure AD Connect configuration wizard.</span></span>

<span data-ttu-id="b1fc7-150">További információkért lásd: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-150">For more information, see [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>

- - -
<span data-ttu-id="b1fc7-151">**K: Az Azure AD-ben elérhető önkiszolgáló portál a szervezetemben lévő felhasználók számára?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-151">**Q: Does Azure AD provide a self-service portal for users in my organization?**</span></span>

<span data-ttu-id="b1fc7-152">**V:** Igen, az Azure AD biztosít hello [Azure AD hozzáférési Panel](http://myapps.microsoft.com) az önkiszolgáló felhasználók és az alkalmazás-hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-152">**A:** Yes, Azure AD provides you with hello [Azure AD Access Panel](http://myapps.microsoft.com) for user self-service and application access.</span></span> <span data-ttu-id="b1fc7-153">Ha Ön Office 365-felhasználó, található hello számos ugyanazokat a képességeket a hello Office 365 portál.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-153">If you are an Office 365 customer, you can find many of hello same capabilities in hello Office 365 portal.</span></span>

<span data-ttu-id="b1fc7-154">További információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-154">For more information, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

- - -
<span data-ttu-id="b1fc7-155">**K: Az Azure AD segít a helyszíni infrastruktúrám kezelésében?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-155">**Q: Does Azure AD help me manage my on-premises infrastructure?**</span></span>

<span data-ttu-id="b1fc7-156">**V:** Igen.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-156">**A:** Yes.</span></span> <span data-ttu-id="b1fc7-157">hello Azure AD prémium kiadás tartalmazza az Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-157">hello Azure AD Premium edition provides you with Azure AD Connect Health.</span></span> <span data-ttu-id="b1fc7-158">Az Azure AD Connect Health segít a figyelheti, és betekintést a helyszíni identitás-infrastruktúra és hello szinkronizálási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-158">Azure AD Connect Health helps you monitor and gain insight into your on-premises identity infrastructure and hello synchronization services.</span></span>  

<span data-ttu-id="b1fc7-159">További információkért lásd: [figyelheti a helyszíni identitás infrastruktúra és a szinkronizálási szolgáltatások hello felhő](active-directory-aadconnect-health.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-159">For more information, see [Monitor your on-premises identity infrastructure and synchronization services in hello cloud](active-directory-aadconnect-health.md).</span></span>  

- - -
## <a name="password-management"></a><span data-ttu-id="b1fc7-160">Jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="b1fc7-160">Password management</span></span>
<span data-ttu-id="b1fc7-161">**K: Használhatom az Azure AD jelszóvisszaírást jelszó-szinkronizálás nélkül? (Ebben a forgatókönyvben az azt lehetséges toouse az Azure AD az önkiszolgáló jelszó-változtatási (SSPR) jelszó késleltetve visszaírt és tárolja-jelszavakkal működő hello felhő?)**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-161">**Q: Can I use Azure AD password write-back without password sync? (In this scenario, is it possible toouse Azure AD self-service password reset (SSPR) with password write-back and not store passwords in hello cloud?)**</span></span>

<span data-ttu-id="b1fc7-162">**V:** nem kell toosynchronize az Active Directory jelszavak tooAzure AD tooenable késleltetve visszaírt.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-162">**A:** You do not need toosynchronize your Active Directory passwords tooAzure AD tooenable write-back.</span></span> <span data-ttu-id="b1fc7-163">Összevont környezetben az Azure AD az egyszeri bejelentkezés (SSO) támaszkodik hello a helyszíni címtár tooauthenticate hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-163">In a federated environment, Azure AD single sign-on (SSO) relies on hello on-premises directory tooauthenticate hello user.</span></span> <span data-ttu-id="b1fc7-164">Ez a forgatókönyv nem igényel hello helyszíni jelszó toobe nyomon követheti az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-164">This scenario does not require hello on-premises password toobe tracked in Azure AD.</span></span>

- - -
<span data-ttu-id="b1fc7-165">**K: mennyi ideig tart a egy jelszó toobe visszaírását tooActive címtár a helyszínen?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-165">**Q: How long does it take for a password toobe written back tooActive Directory on-premises?**</span></span>

<span data-ttu-id="b1fc7-166">**V:** A jelszavak visszaírása valós időben történik.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-166">**A:** Password write-back operates in real time.</span></span>

<span data-ttu-id="b1fc7-167">További részletekért lásd: [A jelszókezelés első lépései](active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-167">For more information, see [Getting started with password management](active-directory-passwords-getting-started.md).</span></span>

- - -
<span data-ttu-id="b1fc7-168">**K: Használhatok jelszóvisszaírást rendszergazda által kezelt jelszavakhoz?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-168">**Q: Can I use password write-back with passwords that are managed by an admin?**</span></span>

<span data-ttu-id="b1fc7-169">**V:** Igen, ha jelszóvisszaírás engedélyezve van, egy rendszergazda által végrehajtott hello jelszó műveletek írt vissza tooyour helyszíni környezetben.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-169">**A:** Yes, if you have password write-back enabled, hello password operations performed by an admin are written back tooyour on-premises environment.</span></span>  

<span data-ttu-id="b1fc7-170">Adott további válaszokért lásd toopassword kapcsolatos kérdések: [jelszókezeléssel kapcsolatos gyakori kérdések](active-directory-passwords-faq.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-170">For more answers toopassword-related questions, see [Password management frequently asked questions](active-directory-passwords-faq.md).</span></span>
- - -
<span data-ttu-id="b1fc7-171">**K: Mi a teendő, ha a meglévő Office 365 vagy az Azure AD-jelszó nem emlékszem a jelszavam toochange közben?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-171">**Q:  What can I do if I can't remember my existing Office 365/Azure AD password while trying toochange my password?**</span></span>

<span data-ttu-id="b1fc7-172">**V:** Ilyen helyzetben több lehetőség közül választhat.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-172">**A:** For this type of situation, there are a couple of options.</span></span>  <span data-ttu-id="b1fc7-173">Használjon önkiszolgáló jelszó-visszaállítást (SSPR), ha elérhető.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-173">Use self-service password reset (SSPR) if it's available.</span></span>  <span data-ttu-id="b1fc7-174">Az SSPR konfigurációjától függ, hogy működik-e.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-174">Whether SSPR works depends on how it's configured.</span></span>  <span data-ttu-id="b1fc7-175">További információkért lásd: [hogyan nem hello jelszó nullázza a munkahelyi portál](active-directory-passwords-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-175">For more information, see [How does hello password reset portal work](active-directory-passwords-best-practices.md).</span></span>

<span data-ttu-id="b1fc7-176">Az Office 365-felhasználók, a rendszergazda is jelszó alaphelyzetbe állítása hello leírt lépéseket hello segítségével [felhasználói jelszavak átállítása](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-176">For Office 365 users, your admin can reset hello password by using hello steps outlined in [Reset user passwords](https://support.office.com/en-us/article/Admins-Reset-user-passwords-7A5D073B-7FAE-4AA5-8F96-9ECD041ABA9C?ui=en-US&rs=en-US&ad=US).</span></span>

<span data-ttu-id="b1fc7-177">Azure AD-fiókok, a rendszergazda alaphelyzetbe állíthatja a jelszavakat hello következő használatával:</span><span class="sxs-lookup"><span data-stu-id="b1fc7-177">For Azure AD accounts, admins can reset passwords by using one of hello following:</span></span>

- [<span data-ttu-id="b1fc7-178">Alaphelyzetbe állítja a fiókok a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b1fc7-178">Reset accounts in hello Azure portal</span></span>](active-directory-users-reset-password-azure-portal.md)
- [<span data-ttu-id="b1fc7-179">A klasszikus portálon hello fiókok alaphelyzetbe állítása</span><span class="sxs-lookup"><span data-stu-id="b1fc7-179">Reset accounts in hello classic portal</span></span>](active-directory-create-users-reset-password.md)
- [<span data-ttu-id="b1fc7-180">A PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="b1fc7-180">Using PowerShell</span></span>](/powershell/module/msonline/set-msoluserpassword?view=azureadps-1.0)


- - -
## <a name="security"></a><span data-ttu-id="b1fc7-181">Biztonság</span><span class="sxs-lookup"><span data-stu-id="b1fc7-181">Security</span></span>
<span data-ttu-id="b1fc7-182">**K: Zárolja a rendszer a fiókokat egy adott számú hibás kísérlet után, vagy ennél kifinomultabb stratégiát alkalmaz?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-182">**Q: Are accounts locked after a specific number of failed attempts or is there a more sophisticated strategy used?**</span></span></br>
<span data-ttu-id="b1fc7-183">Egy kifinomultabb stratégia toolock fiókok használjuk.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-183">We use a more sophisticated strategy toolock accounts.</span></span>  <span data-ttu-id="b1fc7-184">Ez a hello IP hello kérelem és a beírt jelszavak hello alapul.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-184">This is based on hello IP of hello request and hello passwords entered.</span></span> <span data-ttu-id="b1fc7-185">hello kizárás időtartamát hello is növekszik hello valószínűsége, hogy a rendszer a támadás alapján.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-185">hello duration of hello lockout also increases based on hello likelihood that it is an attack.</span></span>  

<span data-ttu-id="b1fc7-186">**K: bizonyos (általános) jelszavak beolvasása elutasítva hello üzenetek "ezt a jelszót használt toomany alkalommal újrapróbálkozott", nem ez tekintse meg a jelenlegi active Directoryban hello használt toopasswords?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-186">**Q:  Certain (common) passwords get rejected with hello messages ‘this password has been used toomany times’, does this refer toopasswords used in hello current active directory?**</span></span></br>
<span data-ttu-id="b1fc7-187">Az adott globálisan, például a "Password" változatának és a "123456" toopasswords utal.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-187">This refers toopasswords that are globally common, such as any variants of “Password” and “123456”.</span></span>

<span data-ttu-id="b1fc7-188">**K: Blokkolja a rendszer a kétes forrásokból (botnetek, TOR-végpontok) érkező bejelentkezési kéréseket a B2C-bérlőkön, vagy ehhez alap- vagy prémium szintű bérlőre van szükség?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-188">**Q: Will a sign-in request from dubious sources (botnets, tor endpoint) be blocked in a B2C tenant or does this require a Basic or Premium edition tenant?**</span></span></br>
<span data-ttu-id="b1fc7-189">Az átjárónk szűri a kéréseket és bizonyos fokú védelmet biztosít a botnetek ellen. Ez minden B2C-bérlőre vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-189">We do have a gateway that filters requests and provides some protection from botnets, and is applied for all B2C tenants.</span></span>

## <a name="application-access"></a><span data-ttu-id="b1fc7-190">Alkalmazás-hozzáférés</span><span class="sxs-lookup"><span data-stu-id="b1fc7-190">Application access</span></span>
<span data-ttu-id="b1fc7-191">**K: Hol találom az Azure Ad-vel előre integrált alkalmazások és azok képességeinek listáját?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-191">**Q: Where can I find a list of applications that are pre-integrated with Azure AD and their capabilities?**</span></span>

<span data-ttu-id="b1fc7-192">**V:** Az Azure AD a Microsoft vállalat, az alkalmazásszolgáltatók és a partnerek több mint 2600 előre integrált alkalmazásával rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-192">**A:** Azure AD has more than 2,600 pre-integrated applications from Microsoft, application service providers, and partners.</span></span> <span data-ttu-id="b1fc7-193">Mindegyik előre integrált alkalmazás támogatja az egyszeri bejelentkezést (SSO).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-193">All pre-integrated applications support single sign-on (SSO).</span></span> <span data-ttu-id="b1fc7-194">Egyszeri bejelentkezés lehetővé teszi a szervezeti hitelesítő adatok tooaccess az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-194">SSO lets you use your organizational credentials tooaccess your apps.</span></span> <span data-ttu-id="b1fc7-195">Hello alkalmazások egy részének támogatja az automatizált üzembe helyezést és megszüntetést is.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-195">Some of hello applications also support automated provisioning and de-provisioning.</span></span>

<span data-ttu-id="b1fc7-196">Hello előre integrált alkalmazások teljes listáját lásd: hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-196">For a complete list of hello pre-integrated applications, see hello [Active Directory Marketplace](https://azure.microsoft.com/marketplace/active-directory/).</span></span>

- - -
<span data-ttu-id="b1fc7-197">**K: Mit tegyek, ha hello van szükségem az alkalmazás nincs hello Azure AD Marketplace-en?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-197">**Q: What if hello application I need is not in hello Azure AD marketplace?**</span></span>

<span data-ttu-id="b1fc7-198">**V:** Az Azure AD Premiumban bármely alkalmazást felveheti és konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-198">**A:** With Azure AD Premium, you can add and configure any application that you want.</span></span> <span data-ttu-id="b1fc7-199">Az alkalmazás képességeitől és a beállításaitól függően konfigurálhat egyszeri bejelentkezést és automatikus üzembe helyezést.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-199">Depending on your application’s capabilities and your preferences, you can configure SSO and automated provisioning.</span></span>  

<span data-ttu-id="b1fc7-200">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b1fc7-200">For more information, see:</span></span>

* [<span data-ttu-id="b1fc7-201">Egyszeri bejelentkezés tooapplications, amelyek nincsenek hello Azure Active Directory alkalmazáskatalógusában konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b1fc7-201">Configuring single sign-on tooapplications that are not in hello Azure Active Directory application gallery</span></span>](active-directory-saas-custom-apps.md)
* [<span data-ttu-id="b1fc7-202">SCIM tooenable automatikus kiépítés a felhasználók és csoportok az Azure Active Directory tooapplications használatával</span><span class="sxs-lookup"><span data-stu-id="b1fc7-202">Using SCIM tooenable automatic provisioning of users and groups from Azure Active Directory tooapplications</span></span>](active-directory-scim-provisioning.md)

- - -
<span data-ttu-id="b1fc7-203">**K: hogyan tegye jelentkeznek be tooapplications Azure AD használatával?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-203">**Q: How do users sign in tooapplications by using Azure AD?**</span></span>

<span data-ttu-id="b1fc7-204">**V:** az Azure AD számos lehetőséget biztosít a felhasználók tooview, és az alkalmazások, például a eléréséhez:</span><span class="sxs-lookup"><span data-stu-id="b1fc7-204">**A:** Azure AD provides several ways for users tooview and access their applications, such as:</span></span>

* <span data-ttu-id="b1fc7-205">hello Azure AD hozzáférési panel</span><span class="sxs-lookup"><span data-stu-id="b1fc7-205">hello Azure AD access panel</span></span>
* <span data-ttu-id="b1fc7-206">hello Office 365 alkalmazásindító</span><span class="sxs-lookup"><span data-stu-id="b1fc7-206">hello Office 365 application launcher</span></span>
* <span data-ttu-id="b1fc7-207">Közvetlen bejelentkezés toofederated alkalmazások</span><span class="sxs-lookup"><span data-stu-id="b1fc7-207">Direct sign-in toofederated apps</span></span>
* <span data-ttu-id="b1fc7-208">Mélyhivatkozással toofederated, jelszóalapú, vagy meglévő alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="b1fc7-208">Deep links toofederated, password-based, or existing apps</span></span>

<span data-ttu-id="b1fc7-209">További információkért lásd: [üzembe helyezés az Azure AD integrált alkalmazások toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-209">For more information, see [Deploying Azure AD integrated applications toousers](active-directory-appssoaccess-whatis.md#deploying-azure-ad-integrated-applications-to-users).</span></span>

- - -
<span data-ttu-id="b1fc7-210">**K: milyen módon hello Azure AD lehetővé teszi a hitelesítés és egyszeri bejelentkezés tooapplications?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-210">**Q: What are hello different ways Azure AD enables authentication and single sign-on tooapplications?**</span></span>

<span data-ttu-id="b1fc7-211">**V:** Az Azure AD számos szabványos protokollt támogat a hitelesítéshez és az engedélyezéshez, például ilyen a SAML 2.0, az OpenID Connect, az OAuth 2.0 és a WS-Federation.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-211">**A:** Azure AD supports many standardized protocols for authentication and authorization, such as SAML 2.0, OpenID Connect, OAuth 2.0, and WS-Federation.</span></span> <span data-ttu-id="b1fc7-212">Az Azure AD a jelszótárolást és az automatikus bejelentkezési képességeket is támogatja olyan alkalmazásoknál, amelyek csak az űrlapalapú hitelesítést támogatják.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-212">Azure AD also supports password vaulting and automated sign-in capabilities for apps that only support forms-based authentication.</span></span>  

<span data-ttu-id="b1fc7-213">További információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="b1fc7-213">For more information, see:</span></span>

* [<span data-ttu-id="b1fc7-214">Hitelesítési forgatókönyvek az Azure AD-hez</span><span class="sxs-lookup"><span data-stu-id="b1fc7-214">Authentication Scenarios for Azure AD</span></span>](active-directory-authentication-scenarios.md)
* [<span data-ttu-id="b1fc7-215">Az Active Directory hitelesítési protokolljai</span><span class="sxs-lookup"><span data-stu-id="b1fc7-215">Active Directory authentication protocols</span></span>](https://msdn.microsoft.com/library/azure/dn151124.aspx)
* [<span data-ttu-id="b1fc7-216">Hogyan működik az egyszeri bejelentkezés az Azure Active Directoryval?</span><span class="sxs-lookup"><span data-stu-id="b1fc7-216">How does single sign-on with Azure Active Directory work?</span></span>](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

- - -
<span data-ttu-id="b1fc7-217">**K: Felvehetek helyszínen futtatott alkalmazásokat?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-217">**Q: Can I add applications I’m running on-premises?**</span></span>

<span data-ttu-id="b1fc7-218">**V:** az Azure AD-alkalmazásproxy egyszerű és biztonságos hozzáférést biztosít az Ön által tooon helyszíni webalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-218">**A:** Azure AD Application Proxy provides you with easy and secure access tooon-premises web applications that you choose.</span></span> <span data-ttu-id="b1fc7-219">Ezeket az alkalmazásokat hello végezheti el, hogy Ön a szoftver kiadva magát bejusson egy szolgáltatott szoftverként (SaaS) alkalmazások az Azure AD azonos módon.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-219">You can access these applications in hello same way that you access your software as a service (SaaS) apps in Azure AD.</span></span> <span data-ttu-id="b1fc7-220">Nincs szükség a VPN-és toochange a hálózati infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-220">There is no need for a VPN or toochange your network infrastructure.</span></span>  

<span data-ttu-id="b1fc7-221">További információkért lásd: [hogyan tooprovide biztonságos távoli hozzáférés tooon helyszíni alkalmazások](active-directory-application-proxy-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-221">For more information, see [How tooprovide secure remote access tooon-premises applications](active-directory-application-proxy-get-started.md).</span></span>

- - -
<span data-ttu-id="b1fc7-222">**K: Hogyan követelhetem meg a többtényezős hitelesítést az adott alkalmazásokhoz hozzáféréssel rendelkező felhasználóknál?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-222">**Q: How do I require multi-factor authentication for users who access a particular application?**</span></span>

<span data-ttu-id="b1fc7-223">**V:** Az Azure AD feltételes hozzáférésével minden alkalmazáshoz egyedi hozzáférési házirendet rendelhet.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-223">**A:** With Azure AD conditional access, you can assign a unique access policy for each application.</span></span> <span data-ttu-id="b1fc7-224">A házirend a multi-factor authentication mindig megkövetelheti, vagy ha a felhasználók nem rendelkeznek helyi hálózathoz csatlakoztatott toohello.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-224">In your policy, you can require multi-factor authentication always, or when users are not connected toohello local network.</span></span>  

<span data-ttu-id="b1fc7-225">További információkért lásd: [tooOffice 365 és az egyéb alkalmazások hozzáférésének biztonságossá tétele az Active Directory tooAzure csatlakoztatott](active-directory-conditional-access.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-225">For more information, see [Securing access tooOffice 365 and other apps connected tooAzure Active Directory](active-directory-conditional-access.md).</span></span>

- - -
<span data-ttu-id="b1fc7-226">**K: Mi a SaaS-alkalmazások automatizált felhasználókiépítése?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-226">**Q: What is automated user provisioning for SaaS apps?**</span></span>

<span data-ttu-id="b1fc7-227">**V:** használja az Azure AD tooautomate hello létrehozását, karbantartási és eltávolítását számos népszerű felhőalapú Szolgáltatottszoftver-alkalmazásoknál felhasználói identitások.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-227">**A:** Use Azure AD tooautomate hello creation, maintenance, and removal of user identities in many popular cloud SaaS apps.</span></span>

<span data-ttu-id="b1fc7-228">További információkért lásd: [automatizálhatja a felhasználó kiépítésének és megszüntetésének biztosítása tooSaaS alkalmazásokat az Azure Active Directoryval](active-directory-saas-app-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="b1fc7-228">For more information, see [Automate user provisioning and deprovisioning tooSaaS applications with Azure Active Directory](active-directory-saas-app-provisioning.md).</span></span>

- - -
<span data-ttu-id="b1fc7-229">**K: Állíthatok be biztonságos LDAP-kapcsolatot az Azure AD-vel?**</span><span class="sxs-lookup"><span data-stu-id="b1fc7-229">**Q:  Can I set up a secure LDAP connection with Azure AD?**</span></span>

<span data-ttu-id="b1fc7-230">**V.:** Nem.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-230">**A:**  No.</span></span> <span data-ttu-id="b1fc7-231">Az Azure AD nem támogatja a hello LDAP protokollt.</span><span class="sxs-lookup"><span data-stu-id="b1fc7-231">Azure AD does not support hello LDAP protocol.</span></span>

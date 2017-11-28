---
title: "aaaFind tevékenység jelentései hello Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan toofind Azure Active Directory-tevékenység jelentései hello Azure-portálon."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f8d7a968403e10ccc5319f27fedad38b1553ded0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-activity-reports-in-hello-azure-portal"></a><span data-ttu-id="76ee0-103">Tevékenységjelentések található hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="76ee0-103">Find activity reports in hello Azure portal</span></span>

<span data-ttu-id="76ee0-104">Ha hello Azure klasszikus portál toohello az Azure-portálon, kap egy új tevékenységi naplóit Azure Active Directory (Azure AD) nézze meg.</span><span class="sxs-lookup"><span data-stu-id="76ee0-104">If you are moving from hello Azure classic portal toohello Azure portal, you get a new look at Azure Active Directory (Azure AD) activity logs.</span></span> <span data-ttu-id="76ee0-105">Az egy nemrég végrehajtott [blogbejegyzés](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), azt ismertetik, hogyan láthatja, tevékenység dolgozik a hello Azure-portálon hello erőforrás hello környezetében naplózza.</span><span class="sxs-lookup"><span data-stu-id="76ee0-105">In a recent [blog post](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), we explain how you can see activity logs in hello context of hello resource you are working on in hello Azure portal.</span></span> <span data-ttu-id="76ee0-106">Ez a cikk azt ismerteti hogyan toofind jelent a hello hello Azure-portálon a klasszikus Azure portálon használt.</span><span class="sxs-lookup"><span data-stu-id="76ee0-106">In this article, we describe how toofind reports that you used in hello Azure classic portal in hello Azure portal.</span></span>

## <a name="whats-new"></a><span data-ttu-id="76ee0-107">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="76ee0-107">What's new</span></span>

<span data-ttu-id="76ee0-108">A klasszikus Azure portálon hello jelentések kategóriába oszthatók:</span><span class="sxs-lookup"><span data-stu-id="76ee0-108">Reports in hello Azure classic portal are separated into categories:</span></span>

1.  <span data-ttu-id="76ee0-109">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="76ee0-109">Security reports</span></span>
2.  <span data-ttu-id="76ee0-110">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="76ee0-110">Activity reports</span></span>
3.  <span data-ttu-id="76ee0-111">Integrált alkalmazás jelentések</span><span class="sxs-lookup"><span data-stu-id="76ee0-111">Integrated app reports</span></span>

### <a name="activity-and-integrated-app-reports"></a><span data-ttu-id="76ee0-112">Tevékenység és integrált alkalmazás jelentések</span><span class="sxs-lookup"><span data-stu-id="76ee0-112">Activity and integrated app reports</span></span>

<span data-ttu-id="76ee0-113">Környezetfüggő jelentéseihez hello Azure-portálon, a meglévő jelentések egyesülnek egyetlen nézetben.</span><span class="sxs-lookup"><span data-stu-id="76ee0-113">For context-based reporting in hello Azure portal, existing reports are merged into a single view.</span></span> <span data-ttu-id="76ee0-114">Egyetlen, a mögöttes API hello adatok toohello nézetét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="76ee0-114">A single, underlying API provides hello data toohello view.</span></span>

<span data-ttu-id="76ee0-115">e nézet, a hello toosee **Azure Active Directory** panel alatt **tevékenység**, jelölje be **naplók**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-115">toosee this view, on hello **Azure Active Directory** blade, under **ACTIVITY**, select **Audit logs**.</span></span>

<span data-ttu-id="76ee0-116">![Naplók](./media/active-directory-reporting-migration/482.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="76ee0-116">![Audit logs](./media/active-directory-reporting-migration/482.png "Audit logs")</span></span>

<span data-ttu-id="76ee0-117">a következő jelentések hello ebben a nézetben van összevonva:</span><span class="sxs-lookup"><span data-stu-id="76ee0-117">hello following reports are consolidated in this view:</span></span>

-   <span data-ttu-id="76ee0-118">Ellenőrzési jelentés</span><span class="sxs-lookup"><span data-stu-id="76ee0-118">Audit report</span></span>
-   <span data-ttu-id="76ee0-119">Jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="76ee0-119">Password reset activity</span></span>
-   <span data-ttu-id="76ee0-120">Jelszó-visszaállítási regisztrációs tevékenység</span><span class="sxs-lookup"><span data-stu-id="76ee0-120">Password reset registration activity</span></span>
-   <span data-ttu-id="76ee0-121">Önkiszolgáló csoportok tevékenységéről</span><span class="sxs-lookup"><span data-stu-id="76ee0-121">Self-service groups activity</span></span>
-   <span data-ttu-id="76ee0-122">Office365 csoport nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="76ee0-122">Office365 Group Name Changes</span></span>
-   <span data-ttu-id="76ee0-123">Tevékenység-kiépítési</span><span class="sxs-lookup"><span data-stu-id="76ee0-123">Account provisioning activity</span></span>
-   <span data-ttu-id="76ee0-124">Jelszó-helyettesítő állapota</span><span class="sxs-lookup"><span data-stu-id="76ee0-124">Password rollover status</span></span>
-   <span data-ttu-id="76ee0-125">Alkalmazás-kiépítési hibák</span><span class="sxs-lookup"><span data-stu-id="76ee0-125">Account provisioning errors</span></span>


<span data-ttu-id="76ee0-126">Alkalmazás használati jelentés hello bővült, és szerepel a hello **bejelentkezések** nézet.</span><span class="sxs-lookup"><span data-stu-id="76ee0-126">hello Application Usage report has been enhanced and is included in hello **Sign-ins** view.</span></span> <span data-ttu-id="76ee0-127">e nézet, a hello toosee **Azure Active Directory** panel alatt **tevékenység**, jelölje be **bejelentkezések**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-127">toosee this view, on hello **Azure Active Directory** blade, under **ACTIVITY**, select **Sign-ins**.</span></span>

<span data-ttu-id="76ee0-128">![Bejelentkezések nézet](./media/active-directory-reporting-migration/483.png "bejelentkezések megtekintése")</span><span class="sxs-lookup"><span data-stu-id="76ee0-128">![Sign-ins view](./media/active-directory-reporting-migration/483.png "Sign-ins view")</span></span>

<span data-ttu-id="76ee0-129">Hello **bejelentkezések** nézet tartalmazza az összes felhasználói bejelentkezéseket. Használhatja az információk tooget alkalmazás használati adatait.</span><span class="sxs-lookup"><span data-stu-id="76ee0-129">hello **Sign-ins** view includes all user sign-ins. You can use this information tooget application usage information.</span></span> <span data-ttu-id="76ee0-130">Is megtekintheti az alkalmazásadatok használati hello **vállalati alkalmazások** áttekintése, a hello **kezelése** szakasz.</span><span class="sxs-lookup"><span data-stu-id="76ee0-130">You also can view application usage information in hello **Enterprise applications** overview, in hello **MANAGE** section.</span></span>

<span data-ttu-id="76ee0-131">![Vállalati alkalmazások](./media/active-directory-reporting-migration/484.png "vállalati alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="76ee0-131">![Enterprise applications](./media/active-directory-reporting-migration/484.png "Enterprise applications")</span></span>

## <a name="access-a-specific-report"></a><span data-ttu-id="76ee0-132">Hozzáférés egy adott jelentés</span><span class="sxs-lookup"><span data-stu-id="76ee0-132">Access a specific report</span></span>

<span data-ttu-id="76ee0-133">Bár az Azure-portálon hello egyetlen nézetben kínál, is vessen egy pillantást konkrét jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="76ee0-133">Although hello Azure portal offers a single view, you also can look at specific reports.</span></span>

### <a name="audit-logs"></a><span data-ttu-id="76ee0-134">Naplók</span><span class="sxs-lookup"><span data-stu-id="76ee0-134">Audit logs</span></span>

<span data-ttu-id="76ee0-135">A válasz toocustomer visszajelzést hello Azure-portálon, használhatja a speciális szűrési tooaccess hello kívánt adatokat is.</span><span class="sxs-lookup"><span data-stu-id="76ee0-135">In response toocustomer feedback, in hello Azure portal, you can use advanced filtering tooaccess hello data you want.</span></span> <span data-ttu-id="76ee0-136">Használhat egy szűrő egy *tevékenységkategóriákkal*, hello különböző típusú tevékenység sorolja fel, amelyek naplózza az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="76ee0-136">One filter you can use is an *activity category*, which lists hello different types of activity logs in Azure AD.</span></span> <span data-ttu-id="76ee0-137">toonarrow eredmények toowhat keres, kiválaszthatja, hogy egy kategóriát.</span><span class="sxs-lookup"><span data-stu-id="76ee0-137">toonarrow results toowhat you are looking for, you can select a category.</span></span>

<span data-ttu-id="76ee0-138">Például ha érdekli, csak a tevékenységek kapcsolódó tooself-szolgáltatás jelszó-átállításra, választhat hello **önkiszolgáló jelszókezelés** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="76ee0-138">For example, if you are interested only in activities related tooself-service password resets, you can choose hello **Self-service Password Management** category.</span></span> <span data-ttu-id="76ee0-139">hello kategóriák látja dolgozik hello erőforrás alapulnak.</span><span class="sxs-lookup"><span data-stu-id="76ee0-139">hello categories you see are based on hello resource you are working in.</span></span>  

<span data-ttu-id="76ee0-140">![Kategória beállítások hello szűrő naplók lapon](./media/active-directory-reporting-migration/06.png "hello szűrő naplók lapon kategória beállításai")</span><span class="sxs-lookup"><span data-stu-id="76ee0-140">![Category options on hello Filter Audit Logs page](./media/active-directory-reporting-migration/06.png "Category options on hello Filter Audit Logs page")</span></span>

<span data-ttu-id="76ee0-141">Tevékenységkategóriák a következők:</span><span class="sxs-lookup"><span data-stu-id="76ee0-141">Activity categories include:</span></span>

- <span data-ttu-id="76ee0-142">Alapvető könyvtár</span><span class="sxs-lookup"><span data-stu-id="76ee0-142">Core Directory</span></span>
- <span data-ttu-id="76ee0-143">Önkiszolgáló jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="76ee0-143">Self-service Password Management</span></span>
- <span data-ttu-id="76ee0-144">Önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="76ee0-144">Self-service Group Management</span></span>
- <span data-ttu-id="76ee0-145">Fiók kiépítése</span><span class="sxs-lookup"><span data-stu-id="76ee0-145">Account Provisioning</span></span>

### <a name="application-usage"></a><span data-ttu-id="76ee0-146">Az alkalmazás használatának</span><span class="sxs-lookup"><span data-stu-id="76ee0-146">Application usage</span></span>

<span data-ttu-id="76ee0-147">az összes olyan alkalmazáshoz, vagy egy önálló alkalmazás, az alkalmazások használatára vonatkozó a részletek tooview **tevékenység**, jelölje be **bejelentkezések**. toonarrow hello eredmény elérése érdekében szűrheti a felhasználó vagy alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="76ee0-147">tooview details about application usage for all apps or for a single app, under **ACTIVITY**, select **Sign-ins**. toonarrow hello results, you can filter on user name or application name.</span></span>

<span data-ttu-id="76ee0-148">![Szűrő bejelentkezési események lapot](./media/active-directory-reporting-migration/07.png "bejelentkezési események szűrése lap")</span><span class="sxs-lookup"><span data-stu-id="76ee0-148">![Filter Sign-In Events page](./media/active-directory-reporting-migration/07.png "Filter Sign-In Events page")</span></span>

### <a name="security-reports"></a><span data-ttu-id="76ee0-149">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="76ee0-149">Security reports</span></span>

#### <a name="azure-ad-anomalous-activity-reports"></a><span data-ttu-id="76ee0-150">Az Azure AD rendellenes tevékenységet jelentések</span><span class="sxs-lookup"><span data-stu-id="76ee0-150">Azure AD anomalous activity reports</span></span>

<span data-ttu-id="76ee0-151">Az Azure AD rendellenes tevékenységet biztonsági jelentéseit a klasszikus Azure portálon hello konszolidált tooprovide volt, egy, a központi nézettel.</span><span class="sxs-lookup"><span data-stu-id="76ee0-151">Azure AD anomalous activity security reports from hello Azure classic portal have been consolidated tooprovide you with one, central view.</span></span> <span data-ttu-id="76ee0-152">Ebben a nézetben látható az összes biztonsági kockázati eseményekről, hogy az Azure AD képesek észlelni és jelentést.</span><span class="sxs-lookup"><span data-stu-id="76ee0-152">This view shows all security-related risk events that Azure AD can detect and report on.</span></span>

<span data-ttu-id="76ee0-153">hello a következő táblázat felsorolja a hello Azure AD rendellenes tevékenységet biztonsági jelentések és a megfelelő kockázat eseménytípusok hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="76ee0-153">hello following table lists hello Azure AD anomalous activity security reports, and corresponding risk event types in hello Azure portal.</span></span>

| <span data-ttu-id="76ee0-154">Az Azure AD rendellenes tevékenységgel kapcsolatos jelentés</span><span class="sxs-lookup"><span data-stu-id="76ee0-154">Azure AD anomalous activity report</span></span> |  <span data-ttu-id="76ee0-155">Identity protection kockázati esemény típusa</span><span class="sxs-lookup"><span data-stu-id="76ee0-155">Identity protection risk event type</span></span>|
| :--- | :--- |
| <span data-ttu-id="76ee0-156">Felhasználók, akiknek kiszivárogtak a hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="76ee0-156">Users with leaked credentials</span></span> | <span data-ttu-id="76ee0-157">Kiszivárgott hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="76ee0-157">Leaked credentials</span></span> |
| <span data-ttu-id="76ee0-158">Rendszertelen bejelentkezési tevékenység</span><span class="sxs-lookup"><span data-stu-id="76ee0-158">Irregular sign-in activity</span></span> | <span data-ttu-id="76ee0-159">Lehetetlen odautazás tooatypical helyek</span><span class="sxs-lookup"><span data-stu-id="76ee0-159">Impossible travel tooatypical locations</span></span> |
| <span data-ttu-id="76ee0-160">Bejelentkezések potenciálisan fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="76ee0-160">Sign-ins from possibly infected devices</span></span> | <span data-ttu-id="76ee0-161">Bejelentkezések fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="76ee0-161">Sign-ins from infected devices</span></span>|
| <span data-ttu-id="76ee0-162">Bejelentkezések ismeretlen forrásokról</span><span class="sxs-lookup"><span data-stu-id="76ee0-162">Sign-ins from unknown sources</span></span> | <span data-ttu-id="76ee0-163">Névtelen IP-címről történő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="76ee0-163">Sign-ins from anonymous IP addresses</span></span> |
| <span data-ttu-id="76ee0-164">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="76ee0-164">Sign-ins from IP addresses with suspicious activity</span></span> | <span data-ttu-id="76ee0-165">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="76ee0-165">Sign-ins from IP addresses with suspicious activity</span></span> |
| - | <span data-ttu-id="76ee0-166">Ismeretlen helyekről történt bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="76ee0-166">Sign-ins from unfamiliar locations</span></span> |

<span data-ttu-id="76ee0-167">az Azure AD rendellenes tevékenységet biztonsági következő hello jelentések nem tartoznak kockázati események hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="76ee0-167">hello following Azure AD anomalous activity security reports are not included as risk events in hello Azure portal:</span></span>

* <span data-ttu-id="76ee0-168">Több hibát követő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="76ee0-168">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="76ee0-169">Bejelentkezések különböző földrajzi régiókból</span><span class="sxs-lookup"><span data-stu-id="76ee0-169">Sign-ins from multiple geographies</span></span>

<span data-ttu-id="76ee0-170">Ezek a jelentések továbbra is elérhetőek hello a klasszikus Azure portálon, de azok elavulttá válik a jövőbeli hello valamikor.</span><span class="sxs-lookup"><span data-stu-id="76ee0-170">These reports are still available in hello Azure classic portal, but they will be deprecated at some time in hello future.</span></span>

<span data-ttu-id="76ee0-171">További információkért tekintse át [Az Azure Active Directory kockázati eseményeivel](active-directory-identity-protection-risk-events.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="76ee0-171">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>  


#### <a name="detected-risk-events"></a><span data-ttu-id="76ee0-172">Észlelt kockázati események</span><span class="sxs-lookup"><span data-stu-id="76ee0-172">Detected risk events</span></span>

<span data-ttu-id="76ee0-173">Hello Azure-portálon, Ön hozzáférhet hello észlelt kockázati események kapcsolatos jelentéseket **Azure Active Directory** panel alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-173">In hello Azure portal, you can access reports about detected risk events on hello **Azure Active Directory** blade, under **SECURITY**.</span></span> <span data-ttu-id="76ee0-174">A következő jelentések hello észlelt kockázati események követi:</span><span class="sxs-lookup"><span data-stu-id="76ee0-174">Detected risk events are tracked in hello following reports:</span></span>   

- <span data-ttu-id="76ee0-175">Felhasználók veszélyben</span><span class="sxs-lookup"><span data-stu-id="76ee0-175">Users at Risk</span></span>
- <span data-ttu-id="76ee0-176">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="76ee0-176">Risky Sign-ins</span></span>

<span data-ttu-id="76ee0-177">![Biztonsági jelentések](./media/active-directory-reporting-migration/04.png "biztonsági jelentések")</span><span class="sxs-lookup"><span data-stu-id="76ee0-177">![Security reports](./media/active-directory-reporting-migration/04.png "Security reports")</span></span>

<span data-ttu-id="76ee0-178">Biztonsági jelentésekkel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="76ee0-178">For more information about security reports, see:</span></span>

- [<span data-ttu-id="76ee0-179">A kockázat biztonsági jelentés hello Azure Active Directory portálon felhasználója</span><span class="sxs-lookup"><span data-stu-id="76ee0-179">Users at Risk security report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="76ee0-180">Kockázatos bejelentkezések jelentés hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="76ee0-180">Risky Sign-ins report in hello Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-hello-azure-classic-portal-vs-hello-azure-portal"></a><span data-ttu-id="76ee0-181">Tevékenységjelentések a hello hello Azure-portál és a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="76ee0-181">Activity reports in hello Azure classic portal vs. hello Azure portal</span></span>

<span data-ttu-id="76ee0-182">hello ebben a szakaszban szereplő táblázat hello a klasszikus Azure portálon a meglévő jelentéseken.</span><span class="sxs-lookup"><span data-stu-id="76ee0-182">hello table in this section lists existing reports in hello Azure classic portal.</span></span> <span data-ttu-id="76ee0-183">Azt is bemutatja, hogyan férhetnek hello ugyanazokat az információkat a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="76ee0-183">It also describes how you can get hello same information in hello Azure portal.</span></span>

<span data-ttu-id="76ee0-184">minden tooview adatok, a hello naplózása **Azure Active Directory** panelen, a **tevékenység**, lépjen túl**a naplók**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-184">tooview all auditing data, on hello **Azure Active Directory** blade, under **ACTIVITY**, go too**Audit logs**.</span></span>

<span data-ttu-id="76ee0-185">![Naplók](./media/active-directory-reporting-migration/61.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="76ee0-185">![Audit logs](./media/active-directory-reporting-migration/61.png "Audit logs")</span></span>

| <span data-ttu-id="76ee0-186">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="76ee0-186">Azure classic portal</span></span>                 | <span data-ttu-id="76ee0-187">az Azure-portálon hello toofind</span><span class="sxs-lookup"><span data-stu-id="76ee0-187">toofind in hello Azure portal</span></span>                                                         |
| ---                                  | ---                                                                        |
| <span data-ttu-id="76ee0-188">Naplók</span><span class="sxs-lookup"><span data-stu-id="76ee0-188">Audit logs</span></span>                           | <span data-ttu-id="76ee0-189">A **Tevékenységkategóriákkal**, jelölje be **Core Directory**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-189">For **Activity Category**, select **Core Directory**.</span></span>                       |
| <span data-ttu-id="76ee0-190">Jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="76ee0-190">Password reset activity</span></span>              | <span data-ttu-id="76ee0-191">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-191">For **Activity Category**, select **Self-service Password Management**.</span></span> |
| <span data-ttu-id="76ee0-192">Jelszó-visszaállítási regisztrációs tevékenység</span><span class="sxs-lookup"><span data-stu-id="76ee0-192">Password reset registration activity</span></span> | <span data-ttu-id="76ee0-193">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-193">For **Activity Category**, select **Self-service Password Management**.</span></span>     |
| <span data-ttu-id="76ee0-194">Önkiszolgáló csoportok tevékenységéről</span><span class="sxs-lookup"><span data-stu-id="76ee0-194">Self-service groups activity</span></span>         | <span data-ttu-id="76ee0-195">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló csoportkezelési**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-195">For **Activity Category**, select **Self-service Group Management**.</span></span>        |
| <span data-ttu-id="76ee0-196">Tevékenység-kiépítési</span><span class="sxs-lookup"><span data-stu-id="76ee0-196">Account provisioning activity</span></span>        | <span data-ttu-id="76ee0-197">A **Tevékenységkategóriákkal**, jelölje be **fiók a felhasználók átadása**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-197">For **Activity Category**, select **Account User Provisioning**.</span></span>         |
| <span data-ttu-id="76ee0-198">Jelszó-helyettesítő állapota</span><span class="sxs-lookup"><span data-stu-id="76ee0-198">Password rollover status</span></span>             | <span data-ttu-id="76ee0-199">A **Tevékenységkategóriákkal**, jelölje be **automatikus App jelszó helyettesítő**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-199">For **Activity Category**, select **Automatic App Password Rollover**.</span></span>      |
| <span data-ttu-id="76ee0-200">Alkalmazás-kiépítési hibák</span><span class="sxs-lookup"><span data-stu-id="76ee0-200">Account provisioning errors</span></span>          | <span data-ttu-id="76ee0-201">A **Tevékenységkategóriákkal**, jelölje be **fiók a felhasználók átadása**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-201">For **Activity Category**, select **Account User Provisioning**.</span></span>        |
| <span data-ttu-id="76ee0-202">Office365 csoport nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="76ee0-202">Office365 Group Name Changes</span></span>         | <span data-ttu-id="76ee0-203">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-203">For **Activity Category**, select **Self-service Password Management**.</span></span> <span data-ttu-id="76ee0-204">A **tevékenység erőforrástípus**, jelölje be **csoport**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-204">For **Activity Resource Type**, select **Group**.</span></span> <span data-ttu-id="76ee0-205">A **tevékenység forrása**, jelölje be **Office 365-csoportok**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-205">For **Activity Source**, select **O365 groups**.</span></span>|

<span data-ttu-id="76ee0-206">tooview hello **Alkalmazáshasználat** jelentésére hello **Azure Active Directory** panel alatt **kezelése**, jelölje be **vállalati alkalmazások**, majd válassza ki **bejelentkezések**.</span><span class="sxs-lookup"><span data-stu-id="76ee0-206">tooview hello **Application Usage** report, on hello **Azure Active Directory** blade, under **MANAGE**, select **Enterprise Applications**, and then select **Sign-ins**.</span></span>


<span data-ttu-id="76ee0-207">![Vállalati alkalmazások bejelentkezések jelentés](./media/active-directory-reporting-migration/199.png "vállalati alkalmazások bejelentkezések jelentés")</span><span class="sxs-lookup"><span data-stu-id="76ee0-207">![Enterprise Applications Sign-Ins report](./media/active-directory-reporting-migration/199.png "Enterprise Applications Sign-Ins report")</span></span>

## <a name="next-steps"></a><span data-ttu-id="76ee0-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="76ee0-208">Next steps</span></span>

<span data-ttu-id="76ee0-209">Jelentéskészítési áttekintését lásd: hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="76ee0-209">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>

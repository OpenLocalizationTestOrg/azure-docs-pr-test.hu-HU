---
title: "Az Azure portálon található Tevékenységjelentések |} Microsoft Docs"
description: "Útmutató: Azure Active Directory Tevékenységjelentések az Azure portálon található."
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
ms.openlocfilehash: f1875582476c3817b9eb0082b6548cc15043cb98
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="find-activity-reports-in-the-azure-portal"></a><span data-ttu-id="3b71f-103">Az Azure portálon található Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-103">Find activity reports in the Azure portal</span></span>

<span data-ttu-id="3b71f-104">Ha a klasszikus Azure portálon az Azure portálra, kap egy új Azure Active Directory (Azure AD) tevékenységi naplóit pillantást.</span><span class="sxs-lookup"><span data-stu-id="3b71f-104">If you are moving from the Azure classic portal to the Azure portal, you get a new look at Azure Active Directory (Azure AD) activity logs.</span></span> <span data-ttu-id="3b71f-105">Az egy nemrég végrehajtott [blogbejegyzés](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), azt ismertetik, hogyan láthatja, tevékenység naplózza az Azure-portálon dolgoznak erőforrás környezetében.</span><span class="sxs-lookup"><span data-stu-id="3b71f-105">In a recent [blog post](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), we explain how you can see activity logs in the context of the resource you are working on in the Azure portal.</span></span> <span data-ttu-id="3b71f-106">Ez a cikk azt ismerteti, hogyan használt jelentéseket a klasszikus Azure-portálon az Azure portálon található.</span><span class="sxs-lookup"><span data-stu-id="3b71f-106">In this article, we describe how to find reports that you used in the Azure classic portal in the Azure portal.</span></span>

## <a name="whats-new"></a><span data-ttu-id="3b71f-107">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="3b71f-107">What's new</span></span>

<span data-ttu-id="3b71f-108">A klasszikus Azure portálon jelentések kategóriába oszthatók:</span><span class="sxs-lookup"><span data-stu-id="3b71f-108">Reports in the Azure classic portal are separated into categories:</span></span>

1.  <span data-ttu-id="3b71f-109">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-109">Security reports</span></span>
2.  <span data-ttu-id="3b71f-110">Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-110">Activity reports</span></span>
3.  <span data-ttu-id="3b71f-111">Integrált alkalmazás jelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-111">Integrated app reports</span></span>

### <a name="activity-and-integrated-app-reports"></a><span data-ttu-id="3b71f-112">Tevékenység és integrált alkalmazás jelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-112">Activity and integrated app reports</span></span>

<span data-ttu-id="3b71f-113">Környezetfüggő jelentéskészítés az Azure portálon, a meglévő jelentések egyesülnek egyetlen nézetben.</span><span class="sxs-lookup"><span data-stu-id="3b71f-113">For context-based reporting in the Azure portal, existing reports are merged into a single view.</span></span> <span data-ttu-id="3b71f-114">Egyetlen, a nézet az adatokat az alapul szolgáló API nyújt.</span><span class="sxs-lookup"><span data-stu-id="3b71f-114">A single, underlying API provides the data to the view.</span></span>

<span data-ttu-id="3b71f-115">Ebben a nézetben tekintheti meg a **Azure Active Directory** panel alatt **tevékenység**, jelölje be **naplók**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-115">To see this view, on the **Azure Active Directory** blade, under **ACTIVITY**, select **Audit logs**.</span></span>

<span data-ttu-id="3b71f-116">![Naplók](./media/active-directory-reporting-migration/482.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="3b71f-116">![Audit logs](./media/active-directory-reporting-migration/482.png "Audit logs")</span></span>

<span data-ttu-id="3b71f-117">Az alábbi jelentések az ebben a nézetben van összevonva:</span><span class="sxs-lookup"><span data-stu-id="3b71f-117">The following reports are consolidated in this view:</span></span>

-   <span data-ttu-id="3b71f-118">Ellenőrzési jelentés</span><span class="sxs-lookup"><span data-stu-id="3b71f-118">Audit report</span></span>
-   <span data-ttu-id="3b71f-119">Jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="3b71f-119">Password reset activity</span></span>
-   <span data-ttu-id="3b71f-120">Jelszó-visszaállítási regisztrációs tevékenység</span><span class="sxs-lookup"><span data-stu-id="3b71f-120">Password reset registration activity</span></span>
-   <span data-ttu-id="3b71f-121">Önkiszolgáló csoportok tevékenységéről</span><span class="sxs-lookup"><span data-stu-id="3b71f-121">Self-service groups activity</span></span>
-   <span data-ttu-id="3b71f-122">Office365 csoport nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="3b71f-122">Office365 Group Name Changes</span></span>
-   <span data-ttu-id="3b71f-123">Tevékenység-kiépítési</span><span class="sxs-lookup"><span data-stu-id="3b71f-123">Account provisioning activity</span></span>
-   <span data-ttu-id="3b71f-124">Jelszó-helyettesítő állapota</span><span class="sxs-lookup"><span data-stu-id="3b71f-124">Password rollover status</span></span>
-   <span data-ttu-id="3b71f-125">Alkalmazás-kiépítési hibák</span><span class="sxs-lookup"><span data-stu-id="3b71f-125">Account provisioning errors</span></span>


<span data-ttu-id="3b71f-126">Az alkalmazás használati jelentés bővült, és szerepel a **bejelentkezések** nézet.</span><span class="sxs-lookup"><span data-stu-id="3b71f-126">The Application Usage report has been enhanced and is included in the **Sign-ins** view.</span></span> <span data-ttu-id="3b71f-127">Ebben a nézetben tekintheti meg a **Azure Active Directory** panel alatt **tevékenység**, jelölje be **bejelentkezések**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-127">To see this view, on the **Azure Active Directory** blade, under **ACTIVITY**, select **Sign-ins**.</span></span>

<span data-ttu-id="3b71f-128">![Bejelentkezések nézet](./media/active-directory-reporting-migration/483.png "bejelentkezések megtekintése")</span><span class="sxs-lookup"><span data-stu-id="3b71f-128">![Sign-ins view](./media/active-directory-reporting-migration/483.png "Sign-ins view")</span></span>

<span data-ttu-id="3b71f-129">A **bejelentkezések** nézet tartalmazza az összes felhasználói bejelentkezéseket.</span><span class="sxs-lookup"><span data-stu-id="3b71f-129">The **Sign-ins** view includes all user sign-ins.</span></span> <span data-ttu-id="3b71f-130">Ez az információ segítségével alkalmazás használati adatok.</span><span class="sxs-lookup"><span data-stu-id="3b71f-130">You can use this information to get application usage information.</span></span> <span data-ttu-id="3b71f-131">Az alkalmazás használati adatai is megtekintheti a **vállalati alkalmazások** áttekintése, a a **kezelése** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3b71f-131">You also can view application usage information in the **Enterprise applications** overview, in the **MANAGE** section.</span></span>

<span data-ttu-id="3b71f-132">![Vállalati alkalmazások](./media/active-directory-reporting-migration/484.png "vállalati alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="3b71f-132">![Enterprise applications](./media/active-directory-reporting-migration/484.png "Enterprise applications")</span></span>

## <a name="access-a-specific-report"></a><span data-ttu-id="3b71f-133">Hozzáférés egy adott jelentés</span><span class="sxs-lookup"><span data-stu-id="3b71f-133">Access a specific report</span></span>

<span data-ttu-id="3b71f-134">Bár az Azure-portálon egyetlen nézetben kínál, is vessen egy pillantást konkrét jelentéseket.</span><span class="sxs-lookup"><span data-stu-id="3b71f-134">Although the Azure portal offers a single view, you also can look at specific reports.</span></span>

### <a name="audit-logs"></a><span data-ttu-id="3b71f-135">Naplók</span><span class="sxs-lookup"><span data-stu-id="3b71f-135">Audit logs</span></span>

<span data-ttu-id="3b71f-136">Az ügyfelek visszajelzései alapján, az Azure portálon használhatja a speciális szűrési férhetnek hozzá a kívánt adathoz.</span><span class="sxs-lookup"><span data-stu-id="3b71f-136">In response to customer feedback, in the Azure portal, you can use advanced filtering to access the data you want.</span></span> <span data-ttu-id="3b71f-137">Használhat egy szűrő egy *tevékenységkategóriákkal*, amely felsorolja a különböző típusú tevékenység naplózza az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3b71f-137">One filter you can use is an *activity category*, which lists the different types of activity logs in Azure AD.</span></span> <span data-ttu-id="3b71f-138">Éppen megtekintett eredmények szűkítéséhez is válasszon egy kategóriát.</span><span class="sxs-lookup"><span data-stu-id="3b71f-138">To narrow results to what you are looking for, you can select a category.</span></span>

<span data-ttu-id="3b71f-139">Például ha érdekli, csak az önkiszolgáló jelszó-átállításra kapcsolatos tevékenységeit, választhatja a **önkiszolgáló jelszókezelés** kategóriát.</span><span class="sxs-lookup"><span data-stu-id="3b71f-139">For example, if you are interested only in activities related to self-service password resets, you can choose the **Self-service Password Management** category.</span></span> <span data-ttu-id="3b71f-140">A kategóriák, megjelenik az erőforrás-használata alapulnak.</span><span class="sxs-lookup"><span data-stu-id="3b71f-140">The categories you see are based on the resource you are working in.</span></span>  

<span data-ttu-id="3b71f-141">![A szűrő naplók oldalon kategória beállítások](./media/active-directory-reporting-migration/06.png "szűrő Naplók lapján található kategória beállítások")</span><span class="sxs-lookup"><span data-stu-id="3b71f-141">![Category options on the Filter Audit Logs page](./media/active-directory-reporting-migration/06.png "Category options on the Filter Audit Logs page")</span></span>

<span data-ttu-id="3b71f-142">Tevékenységkategóriák a következők:</span><span class="sxs-lookup"><span data-stu-id="3b71f-142">Activity categories include:</span></span>

- <span data-ttu-id="3b71f-143">Alapvető könyvtár</span><span class="sxs-lookup"><span data-stu-id="3b71f-143">Core Directory</span></span>
- <span data-ttu-id="3b71f-144">Önkiszolgáló jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="3b71f-144">Self-service Password Management</span></span>
- <span data-ttu-id="3b71f-145">Önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="3b71f-145">Self-service Group Management</span></span>
- <span data-ttu-id="3b71f-146">Fiók kiépítése</span><span class="sxs-lookup"><span data-stu-id="3b71f-146">Account Provisioning</span></span>

### <a name="application-usage"></a><span data-ttu-id="3b71f-147">Az alkalmazás használatának</span><span class="sxs-lookup"><span data-stu-id="3b71f-147">Application usage</span></span>

<span data-ttu-id="3b71f-148">Az összes olyan alkalmazáshoz, vagy egy önálló alkalmazás, az alkalmazás használati adatainak megtekintéséhez az **tevékenység**, jelölje be **bejelentkezések**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-148">To view details about application usage for all apps or for a single app, under **ACTIVITY**, select **Sign-ins**.</span></span> <span data-ttu-id="3b71f-149">Az eredmények szűkítéséhez szűrheti a felhasználó vagy alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="3b71f-149">To narrow the results, you can filter on user name or application name.</span></span>

<span data-ttu-id="3b71f-150">![Szűrő bejelentkezési események lapot](./media/active-directory-reporting-migration/07.png "bejelentkezési események szűrése lap")</span><span class="sxs-lookup"><span data-stu-id="3b71f-150">![Filter Sign-In Events page](./media/active-directory-reporting-migration/07.png "Filter Sign-In Events page")</span></span>

### <a name="security-reports"></a><span data-ttu-id="3b71f-151">Biztonsági jelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-151">Security reports</span></span>

#### <a name="azure-ad-anomalous-activity-reports"></a><span data-ttu-id="3b71f-152">Az Azure AD rendellenes tevékenységet jelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-152">Azure AD anomalous activity reports</span></span>

<span data-ttu-id="3b71f-153">Az Azure AD rendellenes tevékenységet biztonsági biztosítja, hogy a központi nézet egy, az engedélyezés jelentéseit a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3b71f-153">Azure AD anomalous activity security reports from the Azure classic portal have been consolidated to provide you with one, central view.</span></span> <span data-ttu-id="3b71f-154">Ebben a nézetben látható az összes biztonsági kockázati eseményekről, hogy az Azure AD képesek észlelni és jelentést.</span><span class="sxs-lookup"><span data-stu-id="3b71f-154">This view shows all security-related risk events that Azure AD can detect and report on.</span></span>

<span data-ttu-id="3b71f-155">A következő tábla listák az Azure AD rendellenes tevékenységet biztonsági jelentések, és a kockázati esemény típusával azonos Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="3b71f-155">The following table lists the Azure AD anomalous activity security reports, and corresponding risk event types in the Azure portal.</span></span>

| <span data-ttu-id="3b71f-156">Az Azure AD rendellenes tevékenységgel kapcsolatos jelentés</span><span class="sxs-lookup"><span data-stu-id="3b71f-156">Azure AD anomalous activity report</span></span> |  <span data-ttu-id="3b71f-157">Identity protection kockázati esemény típusa</span><span class="sxs-lookup"><span data-stu-id="3b71f-157">Identity protection risk event type</span></span>|
| :--- | :--- |
| <span data-ttu-id="3b71f-158">Felhasználók, akiknek kiszivárogtak a hitelesítő adatai</span><span class="sxs-lookup"><span data-stu-id="3b71f-158">Users with leaked credentials</span></span> | <span data-ttu-id="3b71f-159">Kiszivárgott hitelesítő adatok</span><span class="sxs-lookup"><span data-stu-id="3b71f-159">Leaked credentials</span></span> |
| <span data-ttu-id="3b71f-160">Rendszertelen bejelentkezési tevékenység</span><span class="sxs-lookup"><span data-stu-id="3b71f-160">Irregular sign-in activity</span></span> | <span data-ttu-id="3b71f-161">Bejelentkezés szokatlan helyekről</span><span class="sxs-lookup"><span data-stu-id="3b71f-161">Impossible travel to atypical locations</span></span> |
| <span data-ttu-id="3b71f-162">Bejelentkezések potenciálisan fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="3b71f-162">Sign-ins from possibly infected devices</span></span> | <span data-ttu-id="3b71f-163">Bejelentkezések fertőzött eszközökről</span><span class="sxs-lookup"><span data-stu-id="3b71f-163">Sign-ins from infected devices</span></span>|
| <span data-ttu-id="3b71f-164">Bejelentkezések ismeretlen forrásokról</span><span class="sxs-lookup"><span data-stu-id="3b71f-164">Sign-ins from unknown sources</span></span> | <span data-ttu-id="3b71f-165">Névtelen IP-címről történő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3b71f-165">Sign-ins from anonymous IP addresses</span></span> |
| <span data-ttu-id="3b71f-166">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="3b71f-166">Sign-ins from IP addresses with suspicious activity</span></span> | <span data-ttu-id="3b71f-167">Bejelentkezések gyanús tevékenységeket mutató IP-címekkel</span><span class="sxs-lookup"><span data-stu-id="3b71f-167">Sign-ins from IP addresses with suspicious activity</span></span> |
| - | <span data-ttu-id="3b71f-168">Ismeretlen helyekről történt bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3b71f-168">Sign-ins from unfamiliar locations</span></span> |

<span data-ttu-id="3b71f-169">A következő Azure AD rendellenes tevékenységet biztonsági jelentések nem tartoznak kockázati eseményekről az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="3b71f-169">The following Azure AD anomalous activity security reports are not included as risk events in the Azure portal:</span></span>

* <span data-ttu-id="3b71f-170">Több hibát követő bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3b71f-170">Sign-ins after multiple failures</span></span>
* <span data-ttu-id="3b71f-171">Bejelentkezések különböző földrajzi régiókból</span><span class="sxs-lookup"><span data-stu-id="3b71f-171">Sign-ins from multiple geographies</span></span>

<span data-ttu-id="3b71f-172">Ezek a jelentések továbbra is elérhetők, a klasszikus Azure portálon, de azok a jövőben elavulttá válik.</span><span class="sxs-lookup"><span data-stu-id="3b71f-172">These reports are still available in the Azure classic portal, but they will be deprecated at some time in the future.</span></span>

<span data-ttu-id="3b71f-173">További információkért tekintse át [Az Azure Active Directory kockázati eseményeivel](active-directory-identity-protection-risk-events.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="3b71f-173">For more information, see [Azure Active Directory risk events](active-directory-identity-protection-risk-events.md).</span></span>  


#### <a name="detected-risk-events"></a><span data-ttu-id="3b71f-174">Észlelt kockázati események</span><span class="sxs-lookup"><span data-stu-id="3b71f-174">Detected risk events</span></span>

<span data-ttu-id="3b71f-175">Az Azure-portálon hozzáférhet észlelt kockázati események jelentéseket a a **Azure Active Directory** panel alatt **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-175">In the Azure portal, you can access reports about detected risk events on the **Azure Active Directory** blade, under **SECURITY**.</span></span> <span data-ttu-id="3b71f-176">Az alábbi jelentések észlelt kockázati események követi:</span><span class="sxs-lookup"><span data-stu-id="3b71f-176">Detected risk events are tracked in the following reports:</span></span>   

- <span data-ttu-id="3b71f-177">Felhasználók veszélyben</span><span class="sxs-lookup"><span data-stu-id="3b71f-177">Users at Risk</span></span>
- <span data-ttu-id="3b71f-178">Kockázatos bejelentkezések</span><span class="sxs-lookup"><span data-stu-id="3b71f-178">Risky Sign-ins</span></span>

<span data-ttu-id="3b71f-179">![Biztonsági jelentések](./media/active-directory-reporting-migration/04.png "biztonsági jelentések")</span><span class="sxs-lookup"><span data-stu-id="3b71f-179">![Security reports](./media/active-directory-reporting-migration/04.png "Security reports")</span></span>

<span data-ttu-id="3b71f-180">Biztonsági jelentésekkel kapcsolatos további információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="3b71f-180">For more information about security reports, see:</span></span>

- [<span data-ttu-id="3b71f-181">Kockázati biztonsági jelentést az Azure Active Directory portálon a felhasználók</span><span class="sxs-lookup"><span data-stu-id="3b71f-181">Users at Risk security report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-user-at-risk.md)
- [<span data-ttu-id="3b71f-182">Az Azure Active Directory portálon kockázatos bejelentkezések jelentés</span><span class="sxs-lookup"><span data-stu-id="3b71f-182">Risky Sign-ins report in the Azure Active Directory portal</span></span>](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-the-azure-classic-portal-vs-the-azure-portal"></a><span data-ttu-id="3b71f-183">Az Azure-portál és a klasszikus Azure portálon Tevékenységjelentések</span><span class="sxs-lookup"><span data-stu-id="3b71f-183">Activity reports in the Azure classic portal vs. the Azure portal</span></span>

<span data-ttu-id="3b71f-184">Ez a szakasz a táblázat meglévő jelentések a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3b71f-184">The table in this section lists existing reports in the Azure classic portal.</span></span> <span data-ttu-id="3b71f-185">Azt is bemutatja, hogyan férhetnek ugyanazokat az információkat az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="3b71f-185">It also describes how you can get the same information in the Azure portal.</span></span>

<span data-ttu-id="3b71f-186">Összes naplózási adatok megtekintéséhez a **Azure Active Directory** panelen, a **tevékenység**, és **naplók**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-186">To view all auditing data, on the **Azure Active Directory** blade, under **ACTIVITY**, go to **Audit logs**.</span></span>

<span data-ttu-id="3b71f-187">![Naplók](./media/active-directory-reporting-migration/61.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="3b71f-187">![Audit logs](./media/active-directory-reporting-migration/61.png "Audit logs")</span></span>

| <span data-ttu-id="3b71f-188">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="3b71f-188">Azure classic portal</span></span>                 | <span data-ttu-id="3b71f-189">Az Azure portálon található</span><span class="sxs-lookup"><span data-stu-id="3b71f-189">To find in the Azure portal</span></span>                                                         |
| ---                                  | ---                                                                        |
| <span data-ttu-id="3b71f-190">Naplók</span><span class="sxs-lookup"><span data-stu-id="3b71f-190">Audit logs</span></span>                           | <span data-ttu-id="3b71f-191">A **Tevékenységkategóriákkal**, jelölje be **Core Directory**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-191">For **Activity Category**, select **Core Directory**.</span></span>                       |
| <span data-ttu-id="3b71f-192">Jelszó-visszaállítási tevékenység</span><span class="sxs-lookup"><span data-stu-id="3b71f-192">Password reset activity</span></span>              | <span data-ttu-id="3b71f-193">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-193">For **Activity Category**, select **Self-service Password Management**.</span></span> |
| <span data-ttu-id="3b71f-194">Jelszó-visszaállítási regisztrációs tevékenység</span><span class="sxs-lookup"><span data-stu-id="3b71f-194">Password reset registration activity</span></span> | <span data-ttu-id="3b71f-195">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-195">For **Activity Category**, select **Self-service Password Management**.</span></span>     |
| <span data-ttu-id="3b71f-196">Önkiszolgáló csoportok tevékenységéről</span><span class="sxs-lookup"><span data-stu-id="3b71f-196">Self-service groups activity</span></span>         | <span data-ttu-id="3b71f-197">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló csoportkezelési**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-197">For **Activity Category**, select **Self-service Group Management**.</span></span>        |
| <span data-ttu-id="3b71f-198">Tevékenység-kiépítési</span><span class="sxs-lookup"><span data-stu-id="3b71f-198">Account provisioning activity</span></span>        | <span data-ttu-id="3b71f-199">A **Tevékenységkategóriákkal**, jelölje be **fiók a felhasználók átadása**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-199">For **Activity Category**, select **Account User Provisioning**.</span></span>         |
| <span data-ttu-id="3b71f-200">Jelszó-helyettesítő állapota</span><span class="sxs-lookup"><span data-stu-id="3b71f-200">Password rollover status</span></span>             | <span data-ttu-id="3b71f-201">A **Tevékenységkategóriákkal**, jelölje be **automatikus App jelszó helyettesítő**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-201">For **Activity Category**, select **Automatic App Password Rollover**.</span></span>      |
| <span data-ttu-id="3b71f-202">Alkalmazás-kiépítési hibák</span><span class="sxs-lookup"><span data-stu-id="3b71f-202">Account provisioning errors</span></span>          | <span data-ttu-id="3b71f-203">A **Tevékenységkategóriákkal**, jelölje be **fiók a felhasználók átadása**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-203">For **Activity Category**, select **Account User Provisioning**.</span></span>        |
| <span data-ttu-id="3b71f-204">Office365 csoport nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="3b71f-204">Office365 Group Name Changes</span></span>         | <span data-ttu-id="3b71f-205">A **Tevékenységkategóriákkal**, jelölje be **önkiszolgáló jelszókezelés**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-205">For **Activity Category**, select **Self-service Password Management**.</span></span> <span data-ttu-id="3b71f-206">A **tevékenység erőforrástípus**, jelölje be **csoport**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-206">For **Activity Resource Type**, select **Group**.</span></span> <span data-ttu-id="3b71f-207">A **tevékenység forrása**, jelölje be **Office 365-csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-207">For **Activity Source**, select **O365 groups**.</span></span>|

<span data-ttu-id="3b71f-208">Megtekintéséhez a **Alkalmazáshasználat** jelentés, az a **Azure Active Directory** panelen, a **kezelése**, jelölje be **vállalati alkalmazások**, Válassza ki és **bejelentkezések**.</span><span class="sxs-lookup"><span data-stu-id="3b71f-208">To view the **Application Usage** report, on the **Azure Active Directory** blade, under **MANAGE**, select **Enterprise Applications**, and then select **Sign-ins**.</span></span>


<span data-ttu-id="3b71f-209">![Vállalati alkalmazások bejelentkezések jelentés](./media/active-directory-reporting-migration/199.png "vállalati alkalmazások bejelentkezések jelentés")</span><span class="sxs-lookup"><span data-stu-id="3b71f-209">![Enterprise Applications Sign-Ins report](./media/active-directory-reporting-migration/199.png "Enterprise Applications Sign-Ins report")</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b71f-210">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3b71f-210">Next steps</span></span>

<span data-ttu-id="3b71f-211">A jelentéskészítés áttekintéséért lásd: [Jelentéskészítés az Azure Active Directoryban](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3b71f-211">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>

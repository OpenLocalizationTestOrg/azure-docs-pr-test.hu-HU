---
title: "Naplózott tevékenységekre vonatkozó jelentések az Azure Active Directory portálon | Microsoft Docs"
description: "Naplózott tevékenységekre vonatkozó jelentések az Azure Active Directory portálon – bevezetés"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a1f93126-77d1-4345-ab7d-561066041161
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f2d0332d815c82d7d47625e020de2e9c5099deeb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="audit-activity-reports-in-the-azure-active-directory-portal"></a><span data-ttu-id="cba41-103">Naplózott tevékenységekre vonatkozó jelentések az Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="cba41-103">Audit activity reports in the Azure Active Directory portal</span></span> 

<span data-ttu-id="cba41-104">Az Azure Portalon az Azure Active Directory (Azure AD) jelentéskészítési funkciójával minden szükséges információhoz hozzájuthat a környezetével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="cba41-104">With reporting in Azure Active Directory (Azure AD), you can get the information you need to determine how your environment is doing.</span></span>

<span data-ttu-id="cba41-105">Az Azure AD jelentéskészítési architektúrája a következő elemekből áll:</span><span class="sxs-lookup"><span data-stu-id="cba41-105">The reporting architecture in Azure AD consists of the following components:</span></span>

- <span data-ttu-id="cba41-106">**Tevékenység**</span><span class="sxs-lookup"><span data-stu-id="cba41-106">**Activity**</span></span> 
    - <span data-ttu-id="cba41-107">**Bejelentkezési tevékenységek** – A felügyelt alkalmazások használatával és a felhasználók bejelentkezési tevékenységeivel kapcsolatos információk</span><span class="sxs-lookup"><span data-stu-id="cba41-107">**Sign-in activities** – Information about the usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="cba41-108">**Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="cba41-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="cba41-109">**Biztonság**</span><span class="sxs-lookup"><span data-stu-id="cba41-109">**Security**</span></span> 
    - <span data-ttu-id="cba41-110">**Kockázatos bejelentkezések** – A kockázatos bejelentkezés egy olyan bejelentkezési kísérletet jelöl, amelyet elképzelhető, hogy olyan személy hajtott végre, aki nem a felhasználói fiók jogos tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="cba41-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> <span data-ttu-id="cba41-111">További részletek: Kockázatos bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="cba41-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="cba41-112">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="cba41-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="cba41-113">További részletek: Kockázatosként megjelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="cba41-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="cba41-114">Ezen témakör áttekintést nyújt a naplózási tevékenységekről.</span><span class="sxs-lookup"><span data-stu-id="cba41-114">This topic gives you an overview of the audit activities.</span></span>
 
## <a name="who-can-access-the-data"></a><span data-ttu-id="cba41-115">Ki férhet hozzá az adatokhoz?</span><span class="sxs-lookup"><span data-stu-id="cba41-115">Who can access the data?</span></span>
* <span data-ttu-id="cba41-116">A biztonsági rendszergazda vagy biztonsági olvasó szerepkörű felhasználók</span><span class="sxs-lookup"><span data-stu-id="cba41-116">Users in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="cba41-117">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="cba41-117">Global Admins</span></span>
* <span data-ttu-id="cba41-118">Az egyedi (nem rendszergazda jogosultságú) felhasználók csak a saját tevékenységüket láthatják</span><span class="sxs-lookup"><span data-stu-id="cba41-118">Individual users (non-admins) can see their own activities</span></span>


## <a name="audit-logs"></a><span data-ttu-id="cba41-119">Naplók</span><span class="sxs-lookup"><span data-stu-id="cba41-119">Audit logs</span></span>

<span data-ttu-id="cba41-120">Az Azure Active Directory naplói a rendszertevékenységek rekordjait tartalmazzák megfelelőségi célokból.</span><span class="sxs-lookup"><span data-stu-id="cba41-120">The audit logs in Azure Active Directory provide records of system activities for compliance.</span></span>  
<span data-ttu-id="cba41-121">A **Naplók** menüponton át vezet az út az összes naplózott adathoz – a menüpont az **Azure Active Directory** **Tevékenység** szakaszában található.</span><span class="sxs-lookup"><span data-stu-id="cba41-121">Your first entry point to all auditing data is **Audit logs** in the **Activity** section of **Azure Active Directory**.</span></span>

<span data-ttu-id="cba41-122">![Naplók](./media/active-directory-reporting-activity-audit-logs/61.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-122">![Audit logs](./media/active-directory-reporting-activity-audit-logs/61.png "Audit logs")</span></span>

<span data-ttu-id="cba41-123">Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="cba41-123">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="cba41-124">az előfordulás dátuma és időpontját</span><span class="sxs-lookup"><span data-stu-id="cba41-124">the date and time of the occurrence</span></span>
- <span data-ttu-id="cba41-125">a tevékenység kezdeményezőjét / szereplőjét (*ki?*)</span><span class="sxs-lookup"><span data-stu-id="cba41-125">the initiator / actor (*who*) of an activity</span></span> 
- <span data-ttu-id="cba41-126">a tevékenységet (*mi?*)</span><span class="sxs-lookup"><span data-stu-id="cba41-126">the activity (*what*)</span></span> 
- <span data-ttu-id="cba41-127">a célt</span><span class="sxs-lookup"><span data-stu-id="cba41-127">the target</span></span>

<span data-ttu-id="cba41-128">![Naplók](./media/active-directory-reporting-activity-audit-logs/18.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-128">![Audit logs](./media/active-directory-reporting-activity-audit-logs/18.png "Audit logs")</span></span>

<span data-ttu-id="cba41-129">A listanézetet az eszköztár **Oszlopok** elemére kattintva lehet testre szabni.</span><span class="sxs-lookup"><span data-stu-id="cba41-129">You can customize the list view by clicking **Columns** in the toolbar.</span></span>

<span data-ttu-id="cba41-130">![Naplók](./media/active-directory-reporting-activity-audit-logs/19.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-130">![Audit logs](./media/active-directory-reporting-activity-audit-logs/19.png "Audit logs")</span></span>

<span data-ttu-id="cba41-131">További mezőket jeleníthet meg, vagy eltávolíthatja a már megjelenített mezőket.</span><span class="sxs-lookup"><span data-stu-id="cba41-131">This enables you to display additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="cba41-132">![Naplók](./media/active-directory-reporting-activity-audit-logs/21.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-132">![Audit logs](./media/active-directory-reporting-activity-audit-logs/21.png "Audit logs")</span></span>


<span data-ttu-id="cba41-133">A listanézet egyik elemére kattintva megtekintheti annak elérhető összes részletét.</span><span class="sxs-lookup"><span data-stu-id="cba41-133">By clicking an item in the list view, you get all available details about it.</span></span>

<span data-ttu-id="cba41-134">![Naplók](./media/active-directory-reporting-activity-audit-logs/22.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-134">![Audit logs](./media/active-directory-reporting-activity-audit-logs/22.png "Audit logs")</span></span>


## <a name="filtering-audit-logs"></a><span data-ttu-id="cba41-135">Auditnaplók szűrése</span><span class="sxs-lookup"><span data-stu-id="cba41-135">Filtering audit logs</span></span>

<span data-ttu-id="cba41-136">A jelentésben lévő adatok megfelelő szintű szűkítéséhez az alábbi mezőkkel szűrheti a naplózott adatokat:</span><span class="sxs-lookup"><span data-stu-id="cba41-136">To narrow down the reported data to a level that works for you, you can filter the audit data using the following fields:</span></span>

- <span data-ttu-id="cba41-137">Dátumtartomány</span><span class="sxs-lookup"><span data-stu-id="cba41-137">Date range</span></span>
- <span data-ttu-id="cba41-138">Kezdeményező (Szereplő)</span><span class="sxs-lookup"><span data-stu-id="cba41-138">Initiated by (Actor)</span></span>
- <span data-ttu-id="cba41-139">Kategória</span><span class="sxs-lookup"><span data-stu-id="cba41-139">Category</span></span>
- <span data-ttu-id="cba41-140">Tevékenység erőforrástípusa</span><span class="sxs-lookup"><span data-stu-id="cba41-140">Activity resource type</span></span>
- <span data-ttu-id="cba41-141">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="cba41-141">Activity</span></span>

<span data-ttu-id="cba41-142">![Naplók](./media/active-directory-reporting-activity-audit-logs/23.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-142">![Audit logs](./media/active-directory-reporting-activity-audit-logs/23.png "Audit logs")</span></span>


<span data-ttu-id="cba41-143">A **dátumtartomány** szűrővel időkeretet lehet meghatározni a visszaadott adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="cba41-143">The **date range** filter enables to you to define a timeframe for the returned data.</span></span>  
<span data-ttu-id="cba41-144">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="cba41-144">Possible values are:</span></span>

- <span data-ttu-id="cba41-145">1 hónap</span><span class="sxs-lookup"><span data-stu-id="cba41-145">1 month</span></span>
- <span data-ttu-id="cba41-146">7 nap</span><span class="sxs-lookup"><span data-stu-id="cba41-146">7 days</span></span>
- <span data-ttu-id="cba41-147">24 óra</span><span class="sxs-lookup"><span data-stu-id="cba41-147">24 hours</span></span>
- <span data-ttu-id="cba41-148">Egyéni</span><span class="sxs-lookup"><span data-stu-id="cba41-148">Custom</span></span>

<span data-ttu-id="cba41-149">Egyéni időkeret kiválasztásakor beállíthatja a kezdő és a záró időpontot.</span><span class="sxs-lookup"><span data-stu-id="cba41-149">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="cba41-150">A **Kezdeményező** szűrő lehetővé teszi egy szereplő nevének vagy UPN-jének megadását.</span><span class="sxs-lookup"><span data-stu-id="cba41-150">The **initiated by** filter enables you to define an actor's name or its universal principal name (UPN).</span></span>

<span data-ttu-id="cba41-151">A **kategória** szűrővel az alábbi szűrők egyikét választhatja ki:</span><span class="sxs-lookup"><span data-stu-id="cba41-151">The **category** filter enables you to select one of the following filter:</span></span>

- <span data-ttu-id="cba41-152">Összes</span><span class="sxs-lookup"><span data-stu-id="cba41-152">All</span></span>
- <span data-ttu-id="cba41-153">Alapvető kategória</span><span class="sxs-lookup"><span data-stu-id="cba41-153">Core category</span></span>
- <span data-ttu-id="cba41-154">Alapvető könyvtár</span><span class="sxs-lookup"><span data-stu-id="cba41-154">Core directory</span></span>
- <span data-ttu-id="cba41-155">Önkiszolgáló jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="cba41-155">Self-service password management</span></span>
- <span data-ttu-id="cba41-156">Önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="cba41-156">Self-service group management</span></span>
- <span data-ttu-id="cba41-157">Fiók kiépítése – Automatizált jelszóváltás</span><span class="sxs-lookup"><span data-stu-id="cba41-157">Account provisioning- Automated password rollover</span></span>
- <span data-ttu-id="cba41-158">Meghívott felhasználók</span><span class="sxs-lookup"><span data-stu-id="cba41-158">Invited users</span></span>
- <span data-ttu-id="cba41-159">MIM szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="cba41-159">MIM service</span></span>
- <span data-ttu-id="cba41-160">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="cba41-160">Identity Protection</span></span>
- <span data-ttu-id="cba41-161">B2C</span><span class="sxs-lookup"><span data-stu-id="cba41-161">B2C</span></span>

<span data-ttu-id="cba41-162">A **tevékenység erőforrástípusa** szűrővel az alábbi szűrők egyikét választhatja ki:</span><span class="sxs-lookup"><span data-stu-id="cba41-162">The **activity resource type** filter enables you to select one of the following filters:</span></span>

- <span data-ttu-id="cba41-163">Összes</span><span class="sxs-lookup"><span data-stu-id="cba41-163">All</span></span> 
- <span data-ttu-id="cba41-164">Csoport</span><span class="sxs-lookup"><span data-stu-id="cba41-164">Group</span></span>
- <span data-ttu-id="cba41-165">Címtár</span><span class="sxs-lookup"><span data-stu-id="cba41-165">Directory</span></span>
- <span data-ttu-id="cba41-166">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="cba41-166">User</span></span>
- <span data-ttu-id="cba41-167">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="cba41-167">Application</span></span>
- <span data-ttu-id="cba41-168">Szabályzat</span><span class="sxs-lookup"><span data-stu-id="cba41-168">Policy</span></span>
- <span data-ttu-id="cba41-169">Eszköz</span><span class="sxs-lookup"><span data-stu-id="cba41-169">Device</span></span>
- <span data-ttu-id="cba41-170">Egyéb</span><span class="sxs-lookup"><span data-stu-id="cba41-170">Other</span></span>

<span data-ttu-id="cba41-171">Ha a **Csoport** elemet választja a **tevékenység erőforrástípusaként**, kap egy kiegészítő szűrőkategóriát, amelyben megadhatja a **forrást**:</span><span class="sxs-lookup"><span data-stu-id="cba41-171">When you select **Group** as **activity resource type**, you get an additional filter category that enables you to also provide a **Source**:</span></span>

- <span data-ttu-id="cba41-172">Azure AD</span><span class="sxs-lookup"><span data-stu-id="cba41-172">Azure AD</span></span>
- <span data-ttu-id="cba41-173">O365</span><span class="sxs-lookup"><span data-stu-id="cba41-173">O365</span></span>


<span data-ttu-id="cba41-174">A **tevékenység** szűrő a kiválasztott kategórián és tevékenység-erőforrástípuson alapul.</span><span class="sxs-lookup"><span data-stu-id="cba41-174">The **activity** filter is based on the category and Activity resource type selection you make.</span></span> <span data-ttu-id="cba41-175">Választhat egy adott tevékenységet, amelyet meg szeretne tekinteni, vagy kiválaszthatja az összeset.</span><span class="sxs-lookup"><span data-stu-id="cba41-175">You can select a specific activity you want to see or choose all.</span></span> 

<span data-ttu-id="cba41-176">A Graph API (https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta) használatával lekérheti az összes naplózási tevékenység listáját, ahol a $tenantdomain a tartománynév, vagy tekintse meg a [naplózási jelentési eseményekkel kapcsolatos](active-directory-reporting-audit-events.md) cikket.</span><span class="sxs-lookup"><span data-stu-id="cba41-176">You can get the list of all Audit Activities using the Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, where $tenantdomain = your domain name or refer to the article [audit report events](active-directory-reporting-audit-events.md).</span></span>


## <a name="audit-logs-shortcuts"></a><span data-ttu-id="cba41-177">Rövidebb utak a naplók eléréséhez</span><span class="sxs-lookup"><span data-stu-id="cba41-177">Audit logs shortcuts</span></span>

<span data-ttu-id="cba41-178">Az **Azure Active Directory** mellett az Azure Portal két további lehetőséget biztosít a naplózási adatok elérésére:</span><span class="sxs-lookup"><span data-stu-id="cba41-178">In addition to **Azure Active Directory**, the Azure portal provides you with two additional entry points to audit data:</span></span>

- <span data-ttu-id="cba41-179">Felhasználók és csoportok</span><span class="sxs-lookup"><span data-stu-id="cba41-179">Users and groups</span></span>
- <span data-ttu-id="cba41-180">Vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="cba41-180">Enterprise applications</span></span>

### <a name="users-and-groups-audit-logs"></a><span data-ttu-id="cba41-181">Felhasználók és csoportok auditnaplói</span><span class="sxs-lookup"><span data-stu-id="cba41-181">Users and groups audit logs</span></span>

<span data-ttu-id="cba41-182">A felhasználó- és csoportalapú naplózási jelentésekkel az alábbi kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="cba41-182">With user and group-based audit reports, you can get answers to questions such as:</span></span>

- <span data-ttu-id="cba41-183">Milyen típusú frissítéseket telepítettek a felhasználók?</span><span class="sxs-lookup"><span data-stu-id="cba41-183">What types of updates have been applied the users?</span></span>

- <span data-ttu-id="cba41-184">Hány felhasználó lett módosítva?</span><span class="sxs-lookup"><span data-stu-id="cba41-184">How many users were changed?</span></span>

- <span data-ttu-id="cba41-185">Hány jelszó lett módosítva?</span><span class="sxs-lookup"><span data-stu-id="cba41-185">How many passwords were changed?</span></span>

- <span data-ttu-id="cba41-186">Mit csinált a rendszergazda egy adott címtárban?</span><span class="sxs-lookup"><span data-stu-id="cba41-186">What has an administrator done in a directory?</span></span>

- <span data-ttu-id="cba41-187">Mely csoportok lettek hozzáadva?</span><span class="sxs-lookup"><span data-stu-id="cba41-187">What are the groups that have been added?</span></span>

- <span data-ttu-id="cba41-188">Történt-e tagsági változás valamelyik csoportban?</span><span class="sxs-lookup"><span data-stu-id="cba41-188">Are there groups with membership changes?</span></span>

- <span data-ttu-id="cba41-189">Változtak-e a csoportok tulajdonosai?</span><span class="sxs-lookup"><span data-stu-id="cba41-189">Have the owners of group been changed?</span></span>

- <span data-ttu-id="cba41-190">Milyen licencek lettek hozzárendelve egy adott csoporthoz vagy felhasználóhoz?</span><span class="sxs-lookup"><span data-stu-id="cba41-190">What licenses have been assigned to a group or a user?</span></span>

<span data-ttu-id="cba41-191">Ha csak át szeretné tekinteni a felhasználókhoz és csoportokhoz kapcsolódó naplózási adatokat, megnyithat egy szűrt nézetet az **Auditnaplók** menüpontból, amely a **Felhasználók és csoportok** **Tevékenység** szakaszában található.</span><span class="sxs-lookup"><span data-stu-id="cba41-191">If you just want to review auditing data that is related to users and groups, you can find a filtered view under **Audit logs** in the **Activity** section of the **Users and Groups**.</span></span> <span data-ttu-id="cba41-192">Ennél a lehetőségnél a **Felhasználók és csoportok** van előre kiválasztva **tevékenység-erőforrástípusként**.</span><span class="sxs-lookup"><span data-stu-id="cba41-192">This entry point has **Users and groups** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="cba41-193">![Naplók](./media/active-directory-reporting-activity-audit-logs/93.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-193">![Audit logs](./media/active-directory-reporting-activity-audit-logs/93.png "Audit logs")</span></span>

### <a name="enterprise-applications-audit-logs"></a><span data-ttu-id="cba41-194">Vállalati alkalmazások naplói</span><span class="sxs-lookup"><span data-stu-id="cba41-194">Enterprise applications audit logs</span></span>

<span data-ttu-id="cba41-195">Az alkalmazásalapú naplózási jelentésekkel az alábbi kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="cba41-195">With application-based audit reports, you can get answers to questions such as:</span></span>

* <span data-ttu-id="cba41-196">Mely alkalmazások lettek hozzáadva vagy frissítve?</span><span class="sxs-lookup"><span data-stu-id="cba41-196">What are the applications that have been added or updated?</span></span>
* <span data-ttu-id="cba41-197">Mely alkalmazások lettek eltávolítva?</span><span class="sxs-lookup"><span data-stu-id="cba41-197">What are the applications that have been removed?</span></span>
* <span data-ttu-id="cba41-198">Megváltozott valamelyik alkalmazás egyszerű szolgáltatásneve?</span><span class="sxs-lookup"><span data-stu-id="cba41-198">Has a service principle for an application changed?</span></span>
* <span data-ttu-id="cba41-199">Történt változás az alkalmazások nevében?</span><span class="sxs-lookup"><span data-stu-id="cba41-199">Have the names of applications been changed?</span></span>
* <span data-ttu-id="cba41-200">Ki hagyott jóvá egy adott alkalmazást?</span><span class="sxs-lookup"><span data-stu-id="cba41-200">Who gave consent to an application?</span></span>

<span data-ttu-id="cba41-201">Ha csak át szeretné tekinteni az alkalmazásaihoz kapcsolódó naplózási adatokat, megnyithat egy szűrt nézetet az **Auditnaplók** menüpontból, amely a **Vállalati alkalmazások** panel **Tevékenység** szakaszában található.</span><span class="sxs-lookup"><span data-stu-id="cba41-201">If you just want to review auditing data that is related to your applications, you can find a filtered view under **Audit logs** in the **Activity** section of the **Enterprise applications** blade.</span></span> <span data-ttu-id="cba41-202">Ennél a lehetőségnél a **Vállalati alkalmazások** van előre kiválasztva **tevékenység-erőforrástípusként**.</span><span class="sxs-lookup"><span data-stu-id="cba41-202">This entry point has **Enterprise applications** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="cba41-203">![Naplók](./media/active-directory-reporting-activity-audit-logs/134.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-203">![Audit logs](./media/active-directory-reporting-activity-audit-logs/134.png "Audit logs")</span></span>

<span data-ttu-id="cba41-204">A nézetet tovább szűrheti csak a **csoportok** vagy csak a **felhasználók** megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cba41-204">You can filter this view further down to just **groups** or just **users**.</span></span>

<span data-ttu-id="cba41-205">![Naplók](./media/active-directory-reporting-activity-audit-logs/25.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="cba41-205">![Audit logs](./media/active-directory-reporting-activity-audit-logs/25.png "Audit logs")</span></span>


## <a name="next-steps"></a><span data-ttu-id="cba41-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cba41-206">Next steps</span></span>

<span data-ttu-id="cba41-207">A jelentéskészítés áttekintéséért lásd: [Jelentéskészítés az Azure Active Directoryban](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cba41-207">For an overview of reporting, see the [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>


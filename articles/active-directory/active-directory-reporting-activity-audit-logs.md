---
title: "aaaAudit tevékenység jelentések hello Azure Active Directory portálon |} Microsoft Docs"
description: "Bevezetés toohello naplózási Tevékenységjelentések hello Azure Active Directory portálon"
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
ms.openlocfilehash: 1567673f5030fc707b017c069f2ba7587962e5cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="audit-activity-reports-in-hello-azure-active-directory-portal"></a><span data-ttu-id="5edd5-103">Naplózási Tevékenységjelentések hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="5edd5-103">Audit activity reports in hello Azure Active Directory portal</span></span> 

<span data-ttu-id="5edd5-104">A jelentéskészítés az Azure Active Directory (Azure AD), toodetermine hogyan működik a környezetében szükséges hello információkat kaphat.</span><span class="sxs-lookup"><span data-stu-id="5edd5-104">With reporting in Azure Active Directory (Azure AD), you can get hello information you need toodetermine how your environment is doing.</span></span>

<span data-ttu-id="5edd5-105">a következő összetevők hello architektúra az Azure AD reporting hello foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="5edd5-105">hello reporting architecture in Azure AD consists of hello following components:</span></span>

- <span data-ttu-id="5edd5-106">**Tevékenység**</span><span class="sxs-lookup"><span data-stu-id="5edd5-106">**Activity**</span></span> 
    - <span data-ttu-id="5edd5-107">**Bejelentkezési tevékenységek** – kezelt alkalmazások és a felhasználói bejelentkezési tevékenységek hello használatával kapcsolatos információért</span><span class="sxs-lookup"><span data-stu-id="5edd5-107">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="5edd5-108">**Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="5edd5-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="5edd5-109">**Biztonság**</span><span class="sxs-lookup"><span data-stu-id="5edd5-109">**Security**</span></span> 
    - <span data-ttu-id="5edd5-110">**Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója.</span><span class="sxs-lookup"><span data-stu-id="5edd5-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="5edd5-111">További részletek: Kockázatos bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="5edd5-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="5edd5-112">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="5edd5-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="5edd5-113">További részletek: Kockázatosként megjelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="5edd5-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="5edd5-114">Ez a témakör áttekintést nyújt a hello naplózási tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="5edd5-114">This topic gives you an overview of hello audit activities.</span></span>
 
## <a name="who-can-access-hello-data"></a><span data-ttu-id="5edd5-115">Hello adatok hozzáférő felhasználók?</span><span class="sxs-lookup"><span data-stu-id="5edd5-115">Who can access hello data?</span></span>
* <span data-ttu-id="5edd5-116">Hello biztonsági rendszergazda vagy a biztonsági olvasó szerepet betöltő felhasználók</span><span class="sxs-lookup"><span data-stu-id="5edd5-116">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="5edd5-117">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="5edd5-117">Global Admins</span></span>
* <span data-ttu-id="5edd5-118">Az egyedi (nem rendszergazda jogosultságú) felhasználók csak a saját tevékenységüket láthatják</span><span class="sxs-lookup"><span data-stu-id="5edd5-118">Individual users (non-admins) can see their own activities</span></span>


## <a name="audit-logs"></a><span data-ttu-id="5edd5-119">Naplók</span><span class="sxs-lookup"><span data-stu-id="5edd5-119">Audit logs</span></span>

<span data-ttu-id="5edd5-120">hello naplók az Azure Active Directoryban adja meg a rendszer tevékenységek megfelelőségi rögzíti.</span><span class="sxs-lookup"><span data-stu-id="5edd5-120">hello audit logs in Azure Active Directory provide records of system activities for compliance.</span></span>  
<span data-ttu-id="5edd5-121">Az első belépési pont tooall adatok naplózása van **naplók** a hello **tevékenység** szakasza **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5edd5-121">Your first entry point tooall auditing data is **Audit logs** in hello **Activity** section of **Azure Active Directory**.</span></span>

<span data-ttu-id="5edd5-122">![Naplók](./media/active-directory-reporting-activity-audit-logs/61.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-122">![Audit logs](./media/active-directory-reporting-activity-audit-logs/61.png "Audit logs")</span></span>

<span data-ttu-id="5edd5-123">Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="5edd5-123">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="5edd5-124">hello dátum és idő hello előfordulás</span><span class="sxs-lookup"><span data-stu-id="5edd5-124">hello date and time of hello occurrence</span></span>
- <span data-ttu-id="5edd5-125">hello kezdeményező / aktor (*ki*) tevékenység</span><span class="sxs-lookup"><span data-stu-id="5edd5-125">hello initiator / actor (*who*) of an activity</span></span> 
- <span data-ttu-id="5edd5-126">tevékenység hello (*mi*)</span><span class="sxs-lookup"><span data-stu-id="5edd5-126">hello activity (*what*)</span></span> 
- <span data-ttu-id="5edd5-127">hello cél</span><span class="sxs-lookup"><span data-stu-id="5edd5-127">hello target</span></span>

<span data-ttu-id="5edd5-128">![Naplók](./media/active-directory-reporting-activity-audit-logs/18.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-128">![Audit logs](./media/active-directory-reporting-activity-audit-logs/18.png "Audit logs")</span></span>

<span data-ttu-id="5edd5-129">Testre szabhatja hello listanézet kattintva **oszlopok** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="5edd5-129">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>

<span data-ttu-id="5edd5-130">![Naplók](./media/active-directory-reporting-activity-audit-logs/19.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-130">![Audit logs](./media/active-directory-reporting-activity-audit-logs/19.png "Audit logs")</span></span>

<span data-ttu-id="5edd5-131">Ez lehetővé teszi a toodisplay további mezőket, vagy távolítsa el a mezőket, amelyeknek már jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5edd5-131">This enables you toodisplay additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="5edd5-132">![Naplók](./media/active-directory-reporting-activity-audit-logs/21.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-132">![Audit logs](./media/active-directory-reporting-activity-audit-logs/21.png "Audit logs")</span></span>


<span data-ttu-id="5edd5-133">Hello listanézet elemére kattintva kap az összes rendelkezésre álló részleteit.</span><span class="sxs-lookup"><span data-stu-id="5edd5-133">By clicking an item in hello list view, you get all available details about it.</span></span>

<span data-ttu-id="5edd5-134">![Naplók](./media/active-directory-reporting-activity-audit-logs/22.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-134">![Audit logs](./media/active-directory-reporting-activity-audit-logs/22.png "Audit logs")</span></span>


## <a name="filtering-audit-logs"></a><span data-ttu-id="5edd5-135">Auditnaplók szűrése</span><span class="sxs-lookup"><span data-stu-id="5edd5-135">Filtering audit logs</span></span>

<span data-ttu-id="5edd5-136">lefelé hello toonarrow jelentette, hogy az Ön működik, végezhet hello naplózási adatok használatával a következő mezők hello tooa szintnek:</span><span class="sxs-lookup"><span data-stu-id="5edd5-136">toonarrow down hello reported data tooa level that works for you, you can filter hello audit data using hello following fields:</span></span>

- <span data-ttu-id="5edd5-137">Dátumtartomány</span><span class="sxs-lookup"><span data-stu-id="5edd5-137">Date range</span></span>
- <span data-ttu-id="5edd5-138">Kezdeményező (Szereplő)</span><span class="sxs-lookup"><span data-stu-id="5edd5-138">Initiated by (Actor)</span></span>
- <span data-ttu-id="5edd5-139">Kategória</span><span class="sxs-lookup"><span data-stu-id="5edd5-139">Category</span></span>
- <span data-ttu-id="5edd5-140">Tevékenység erőforrástípusa</span><span class="sxs-lookup"><span data-stu-id="5edd5-140">Activity resource type</span></span>
- <span data-ttu-id="5edd5-141">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="5edd5-141">Activity</span></span>

<span data-ttu-id="5edd5-142">![Naplók](./media/active-directory-reporting-activity-audit-logs/23.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-142">![Audit logs](./media/active-directory-reporting-activity-audit-logs/23.png "Audit logs")</span></span>


<span data-ttu-id="5edd5-143">Hello **dátumtartományának** szűrő lehetővé teszi, hogy tooyou toodefine a hello időkereteket adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="5edd5-143">hello **date range** filter enables tooyou toodefine a timeframe for hello returned data.</span></span>  
<span data-ttu-id="5edd5-144">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="5edd5-144">Possible values are:</span></span>

- <span data-ttu-id="5edd5-145">1 hónap</span><span class="sxs-lookup"><span data-stu-id="5edd5-145">1 month</span></span>
- <span data-ttu-id="5edd5-146">7 nap</span><span class="sxs-lookup"><span data-stu-id="5edd5-146">7 days</span></span>
- <span data-ttu-id="5edd5-147">24 óra</span><span class="sxs-lookup"><span data-stu-id="5edd5-147">24 hours</span></span>
- <span data-ttu-id="5edd5-148">Egyéni</span><span class="sxs-lookup"><span data-stu-id="5edd5-148">Custom</span></span>

<span data-ttu-id="5edd5-149">Egyéni időkeret kiválasztásakor beállíthatja a kezdő és a záró időpontot.</span><span class="sxs-lookup"><span data-stu-id="5edd5-149">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="5edd5-150">Hello **által kezdeményezett** szűrő lehetővé teszi, hogy Ön toodefine egy szereplő nevét vagy annak univerzális egyszerű felhasználónév (UPN).</span><span class="sxs-lookup"><span data-stu-id="5edd5-150">hello **initiated by** filter enables you toodefine an actor's name or its universal principal name (UPN).</span></span>

<span data-ttu-id="5edd5-151">Hello **kategória** szűrő tooselect a következő szűrő hello lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="5edd5-151">hello **category** filter enables you tooselect one of hello following filter:</span></span>

- <span data-ttu-id="5edd5-152">Összes</span><span class="sxs-lookup"><span data-stu-id="5edd5-152">All</span></span>
- <span data-ttu-id="5edd5-153">Alapvető kategória</span><span class="sxs-lookup"><span data-stu-id="5edd5-153">Core category</span></span>
- <span data-ttu-id="5edd5-154">Alapvető könyvtár</span><span class="sxs-lookup"><span data-stu-id="5edd5-154">Core directory</span></span>
- <span data-ttu-id="5edd5-155">Önkiszolgáló jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="5edd5-155">Self-service password management</span></span>
- <span data-ttu-id="5edd5-156">Önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="5edd5-156">Self-service group management</span></span>
- <span data-ttu-id="5edd5-157">Fiók kiépítése – Automatizált jelszóváltás</span><span class="sxs-lookup"><span data-stu-id="5edd5-157">Account provisioning- Automated password rollover</span></span>
- <span data-ttu-id="5edd5-158">Meghívott felhasználók</span><span class="sxs-lookup"><span data-stu-id="5edd5-158">Invited users</span></span>
- <span data-ttu-id="5edd5-159">MIM szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5edd5-159">MIM service</span></span>
- <span data-ttu-id="5edd5-160">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="5edd5-160">Identity Protection</span></span>
- <span data-ttu-id="5edd5-161">B2C</span><span class="sxs-lookup"><span data-stu-id="5edd5-161">B2C</span></span>

<span data-ttu-id="5edd5-162">Hello **tevékenység erőforrástípus** szűrő lehetővé teszi tooselect hello következő szűrése:</span><span class="sxs-lookup"><span data-stu-id="5edd5-162">hello **activity resource type** filter enables you tooselect one of hello following filters:</span></span>

- <span data-ttu-id="5edd5-163">Összes</span><span class="sxs-lookup"><span data-stu-id="5edd5-163">All</span></span> 
- <span data-ttu-id="5edd5-164">Csoport</span><span class="sxs-lookup"><span data-stu-id="5edd5-164">Group</span></span>
- <span data-ttu-id="5edd5-165">Címtár</span><span class="sxs-lookup"><span data-stu-id="5edd5-165">Directory</span></span>
- <span data-ttu-id="5edd5-166">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="5edd5-166">User</span></span>
- <span data-ttu-id="5edd5-167">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5edd5-167">Application</span></span>
- <span data-ttu-id="5edd5-168">Szabályzat</span><span class="sxs-lookup"><span data-stu-id="5edd5-168">Policy</span></span>
- <span data-ttu-id="5edd5-169">Eszköz</span><span class="sxs-lookup"><span data-stu-id="5edd5-169">Device</span></span>
- <span data-ttu-id="5edd5-170">Egyéb</span><span class="sxs-lookup"><span data-stu-id="5edd5-170">Other</span></span>

<span data-ttu-id="5edd5-171">Ha bejelöli **csoport** , **tevékenység erőforrástípus**, tooalso kap egy kiegészítő szűrőt kategóriát, amely lehetővé teszi, adjon meg egy **forrás**:</span><span class="sxs-lookup"><span data-stu-id="5edd5-171">When you select **Group** as **activity resource type**, you get an additional filter category that enables you tooalso provide a **Source**:</span></span>

- <span data-ttu-id="5edd5-172">Azure AD</span><span class="sxs-lookup"><span data-stu-id="5edd5-172">Azure AD</span></span>
- <span data-ttu-id="5edd5-173">O365</span><span class="sxs-lookup"><span data-stu-id="5edd5-173">O365</span></span>


<span data-ttu-id="5edd5-174">Hello **tevékenység** szűrő hello kategória és tevékenység erőforrás típusa választott ki alapul.</span><span class="sxs-lookup"><span data-stu-id="5edd5-174">hello **activity** filter is based on hello category and Activity resource type selection you make.</span></span> <span data-ttu-id="5edd5-175">Kiválaszthatja, hogy egy adott tevékenységre, válassza ki az összes vagy toosee szeretné.</span><span class="sxs-lookup"><span data-stu-id="5edd5-175">You can select a specific activity you want toosee or choose all.</span></span> 

<span data-ttu-id="5edd5-176">Kaphat az összes naplózási tevékenység hello listája tenantdomain/tevékenységek/auditActivityTypes hello Graph API https://graph.windows.net/$ használatával? api-version = beta, ahol $tenantdomain = a tartomány nevét, vagy tekintse meg a toohello cikk [ellenőrzési jelentés események](active-directory-reporting-audit-events.md).</span><span class="sxs-lookup"><span data-stu-id="5edd5-176">You can get hello list of all Audit Activities using hello Graph API https://graph.windows.net/$tenantdomain/activities/auditActivityTypes?api-version=beta, where $tenantdomain = your domain name or refer toohello article [audit report events](active-directory-reporting-audit-events.md).</span></span>


## <a name="audit-logs-shortcuts"></a><span data-ttu-id="5edd5-177">Rövidebb utak a naplók eléréséhez</span><span class="sxs-lookup"><span data-stu-id="5edd5-177">Audit logs shortcuts</span></span>

<span data-ttu-id="5edd5-178">Ezenkívül túl**Azure Active Directory**, hello Azure-portál lehetővé teszi két további belépési pontok tooaudit adatokat:</span><span class="sxs-lookup"><span data-stu-id="5edd5-178">In addition too**Azure Active Directory**, hello Azure portal provides you with two additional entry points tooaudit data:</span></span>

- <span data-ttu-id="5edd5-179">Felhasználók és csoportok</span><span class="sxs-lookup"><span data-stu-id="5edd5-179">Users and groups</span></span>
- <span data-ttu-id="5edd5-180">Vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="5edd5-180">Enterprise applications</span></span>

### <a name="users-and-groups-audit-logs"></a><span data-ttu-id="5edd5-181">Felhasználók és csoportok auditnaplói</span><span class="sxs-lookup"><span data-stu-id="5edd5-181">Users and groups audit logs</span></span>

<span data-ttu-id="5edd5-182">A felhasználó és csoport alapuló naplózási jelentések kaphat a válaszok tooquestions többek között:</span><span class="sxs-lookup"><span data-stu-id="5edd5-182">With user and group-based audit reports, you can get answers tooquestions such as:</span></span>

- <span data-ttu-id="5edd5-183">Milyen típusú frissítések lettek alkalmazott hello felhasználók?</span><span class="sxs-lookup"><span data-stu-id="5edd5-183">What types of updates have been applied hello users?</span></span>

- <span data-ttu-id="5edd5-184">Hány felhasználó lett módosítva?</span><span class="sxs-lookup"><span data-stu-id="5edd5-184">How many users were changed?</span></span>

- <span data-ttu-id="5edd5-185">Hány jelszó lett módosítva?</span><span class="sxs-lookup"><span data-stu-id="5edd5-185">How many passwords were changed?</span></span>

- <span data-ttu-id="5edd5-186">Mit csinált a rendszergazda egy adott címtárban?</span><span class="sxs-lookup"><span data-stu-id="5edd5-186">What has an administrator done in a directory?</span></span>

- <span data-ttu-id="5edd5-187">Mik azok a hozzáadott hello csoportok?</span><span class="sxs-lookup"><span data-stu-id="5edd5-187">What are hello groups that have been added?</span></span>

- <span data-ttu-id="5edd5-188">Történt-e tagsági változás valamelyik csoportban?</span><span class="sxs-lookup"><span data-stu-id="5edd5-188">Are there groups with membership changes?</span></span>

- <span data-ttu-id="5edd5-189">Megváltoztatta a csoport tulajdonosainak hello?</span><span class="sxs-lookup"><span data-stu-id="5edd5-189">Have hello owners of group been changed?</span></span>

- <span data-ttu-id="5edd5-190">Milyen licencek tooa csoport vagy felhasználó hozzárendelt?</span><span class="sxs-lookup"><span data-stu-id="5edd5-190">What licenses have been assigned tooa group or a user?</span></span>

<span data-ttu-id="5edd5-191">Ha csak naplózási adatokat, amelyek kapcsolódó toousers és csoportok tooreview, egy szűrt nézet alatt található **naplók** a hello **tevékenység** hello szakasza **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5edd5-191">If you just want tooreview auditing data that is related toousers and groups, you can find a filtered view under **Audit logs** in hello **Activity** section of hello **Users and Groups**.</span></span> <span data-ttu-id="5edd5-192">Ennél a lehetőségnél a **Felhasználók és csoportok** van előre kiválasztva **tevékenység-erőforrástípusként**.</span><span class="sxs-lookup"><span data-stu-id="5edd5-192">This entry point has **Users and groups** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="5edd5-193">![Naplók](./media/active-directory-reporting-activity-audit-logs/93.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-193">![Audit logs](./media/active-directory-reporting-activity-audit-logs/93.png "Audit logs")</span></span>

### <a name="enterprise-applications-audit-logs"></a><span data-ttu-id="5edd5-194">Vállalati alkalmazások naplói</span><span class="sxs-lookup"><span data-stu-id="5edd5-194">Enterprise applications audit logs</span></span>

<span data-ttu-id="5edd5-195">Az alkalmazás-alapú naplózási jelentések, akkor létrehozhat válaszok tooquestions többek között:</span><span class="sxs-lookup"><span data-stu-id="5edd5-195">With application-based audit reports, you can get answers tooquestions such as:</span></span>

* <span data-ttu-id="5edd5-196">Mik azok a hozzáadott vagy frissített hello alkalmazások?</span><span class="sxs-lookup"><span data-stu-id="5edd5-196">What are hello applications that have been added or updated?</span></span>
* <span data-ttu-id="5edd5-197">Mik azok a hello alkalmazások, amelyek el lettek távolítva?</span><span class="sxs-lookup"><span data-stu-id="5edd5-197">What are hello applications that have been removed?</span></span>
* <span data-ttu-id="5edd5-198">Megváltozott valamelyik alkalmazás egyszerű szolgáltatásneve?</span><span class="sxs-lookup"><span data-stu-id="5edd5-198">Has a service principle for an application changed?</span></span>
* <span data-ttu-id="5edd5-199">Megváltoztatta a kérelmek hello nevek?</span><span class="sxs-lookup"><span data-stu-id="5edd5-199">Have hello names of applications been changed?</span></span>
* <span data-ttu-id="5edd5-200">Hozzájárulás tooan alkalmazás számára megadott?</span><span class="sxs-lookup"><span data-stu-id="5edd5-200">Who gave consent tooan application?</span></span>

<span data-ttu-id="5edd5-201">Ha csak naplózási adatokat, amelyek kapcsolódó tooyour alkalmazások tooreview, egy szűrt nézet alatt található **naplók** a hello **tevékenység** hello szakasza **vállalati alkalmazások**  panelen.</span><span class="sxs-lookup"><span data-stu-id="5edd5-201">If you just want tooreview auditing data that is related tooyour applications, you can find a filtered view under **Audit logs** in hello **Activity** section of hello **Enterprise applications** blade.</span></span> <span data-ttu-id="5edd5-202">Ennél a lehetőségnél a **Vállalati alkalmazások** van előre kiválasztva **tevékenység-erőforrástípusként**.</span><span class="sxs-lookup"><span data-stu-id="5edd5-202">This entry point has **Enterprise applications** as preselected **Activity Resource Type**.</span></span>

<span data-ttu-id="5edd5-203">![Naplók](./media/active-directory-reporting-activity-audit-logs/134.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-203">![Audit logs](./media/active-directory-reporting-activity-audit-logs/134.png "Audit logs")</span></span>

<span data-ttu-id="5edd5-204">Szűrheti a nézet további le toojust **csoportok** vagy csak **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="5edd5-204">You can filter this view further down toojust **groups** or just **users**.</span></span>

<span data-ttu-id="5edd5-205">![Naplók](./media/active-directory-reporting-activity-audit-logs/25.png "Naplók")</span><span class="sxs-lookup"><span data-stu-id="5edd5-205">![Audit logs](./media/active-directory-reporting-activity-audit-logs/25.png "Audit logs")</span></span>


## <a name="next-steps"></a><span data-ttu-id="5edd5-206">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5edd5-206">Next steps</span></span>

<span data-ttu-id="5edd5-207">Jelentéskészítési áttekintését lásd: hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5edd5-207">For an overview of reporting, see hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).</span></span>


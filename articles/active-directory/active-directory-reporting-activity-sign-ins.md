---
title: "jelentések aaaSign tevékenységet hello Azure Active Directory portálon |} Microsoft Docs"
description: "Bevezetés toosign tevékenységet jelentések hello Azure Active Directory portálon"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 49590d625a08d7dc189a629b89bab2261c2b4780
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-activity-reports-in-hello-azure-active-directory-portal"></a><span data-ttu-id="809b0-103">Bejelentkezési tevékenység jelentések hello Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="809b0-103">Sign-in activity reports in hello Azure Active Directory portal</span></span>

<span data-ttu-id="809b0-104">Az Azure Active Directory (Azure AD) jelentéskészítés hello [Azure-portálon](https://portal.azure.com), hogyan működik a környezet toodetermine szükséges hello információkat kaphat.</span><span class="sxs-lookup"><span data-stu-id="809b0-104">With Azure Active Directory (Azure AD) reporting in hello [Azure portal](https://portal.azure.com), you can get hello information you need toodetermine how your environment is doing.</span></span>

<span data-ttu-id="809b0-105">a következő összetevők hello architektúra az Azure Active Directory reporting hello foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="809b0-105">hello reporting architecture in Azure Active Directory consists of hello following components:</span></span>

- <span data-ttu-id="809b0-106">**Tevékenység**</span><span class="sxs-lookup"><span data-stu-id="809b0-106">**Activity**</span></span> 
    - <span data-ttu-id="809b0-107">**Bejelentkezési tevékenységek** – kezelt alkalmazások és a felhasználói bejelentkezési tevékenységek hello használatával kapcsolatos információért</span><span class="sxs-lookup"><span data-stu-id="809b0-107">**Sign-in activities** – Information about hello usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="809b0-108">**Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="809b0-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="809b0-109">**Biztonság**</span><span class="sxs-lookup"><span data-stu-id="809b0-109">**Security**</span></span> 
    - <span data-ttu-id="809b0-110">**Kockázatos bejelentkezések** -kockázatos bejelentkezés egy bejelentkezési kísérlet, amely előfordulhat, hogy nincs egy felhasználói fiókot hello jogos tulajdonosa, aki elvégezte mutatója.</span><span class="sxs-lookup"><span data-stu-id="809b0-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not hello legitimate owner of a user account.</span></span> <span data-ttu-id="809b0-111">További részletek: Kockázatos bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="809b0-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="809b0-112">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="809b0-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="809b0-113">További részletek: Kockázatosként megjelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="809b0-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="809b0-114">Ez a témakör áttekintést nyújt a hello bejelentkezési tevékenységek.</span><span class="sxs-lookup"><span data-stu-id="809b0-114">This topic gives you an overview of hello sign-in activities.</span></span>

## <a name="pre-requisite"></a><span data-ttu-id="809b0-115">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="809b0-115">Pre-requisite</span></span>

### <a name="who-can-access-hello-data"></a><span data-ttu-id="809b0-116">Hello adatok hozzáférő felhasználók?</span><span class="sxs-lookup"><span data-stu-id="809b0-116">Who can access hello data?</span></span>
* <span data-ttu-id="809b0-117">Hello biztonsági rendszergazda vagy a biztonsági olvasó szerepet betöltő felhasználók</span><span class="sxs-lookup"><span data-stu-id="809b0-117">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="809b0-118">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="809b0-118">Global Admins</span></span>
* <span data-ttu-id="809b0-119">Bármely (nem rendszergazda jogosultságú) felhasználó hozzáfér a saját bejelentkezéseihez</span><span class="sxs-lookup"><span data-stu-id="809b0-119">Any user (non-admins) can access their own sign-ins</span></span> 

### <a name="what-azure-ad-license-do-you-need-tooaccess-sign-in-activity"></a><span data-ttu-id="809b0-120">Milyen az Azure AD licencet kell tooaccess bejelentkezési tevékenységet?</span><span class="sxs-lookup"><span data-stu-id="809b0-120">What Azure AD license do you need tooaccess sign-in activity?</span></span>
* <span data-ttu-id="809b0-121">A bérlő rendelkeznie kell egy hozzá társított Azure AD Premium licenc toosee hello akár az összes bejelentkezési tevékenység jelentés</span><span class="sxs-lookup"><span data-stu-id="809b0-121">Your tenant must have an Azure AD Premium license associated with it toosee hello all up sign-in activity report</span></span>


## <a name="signs-in-activities"></a><span data-ttu-id="809b0-122">Bejelentkezési tevékenységek</span><span class="sxs-lookup"><span data-stu-id="809b0-122">Signs-in activities</span></span>

<span data-ttu-id="809b0-123">Felhasználói bejelentkezési jelentés hello hello információk megtalálhatja válaszok tooquestions többek között:</span><span class="sxs-lookup"><span data-stu-id="809b0-123">With hello information provided by hello user sign-in report, you find answers tooquestions such as:</span></span>

* <span data-ttu-id="809b0-124">Mi az a hello bejelentkezési minta egy olyan felhasználó?</span><span class="sxs-lookup"><span data-stu-id="809b0-124">What is hello sign-in pattern of a user?</span></span>
* <span data-ttu-id="809b0-125">Hány felhasználó jelentkezett be egy adott héten?</span><span class="sxs-lookup"><span data-stu-id="809b0-125">How many users have users signed in over a week?</span></span>
* <span data-ttu-id="809b0-126">Mi az a bejelentkezéseket hello állapotának?</span><span class="sxs-lookup"><span data-stu-id="809b0-126">What’s hello status of these sign-ins?</span></span>

<span data-ttu-id="809b0-127">Az első belépési pont tooall bejelentkezési tevékenységek adatok **bejelentkezések** hello tevékenység részében **Azure Active**.</span><span class="sxs-lookup"><span data-stu-id="809b0-127">Your first entry point tooall sign-in activities data is **Sign-ins** in hello Activity section of **Azure Active**.</span></span>


<span data-ttu-id="809b0-128">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-128">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")</span></span>


<span data-ttu-id="809b0-129">Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="809b0-129">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="809b0-130">hello kapcsolódó felhasználói</span><span class="sxs-lookup"><span data-stu-id="809b0-130">hello related user</span></span>
- <span data-ttu-id="809b0-131">hello alkalmazás hello felhasználó van bejelentkezve a</span><span class="sxs-lookup"><span data-stu-id="809b0-131">hello application hello user has signed-in to</span></span>
- <span data-ttu-id="809b0-132">hello bejelentkezési állapot</span><span class="sxs-lookup"><span data-stu-id="809b0-132">hello sign-in status</span></span>
- <span data-ttu-id="809b0-133">hello bejelentkezési idő</span><span class="sxs-lookup"><span data-stu-id="809b0-133">hello sign-in time</span></span>

<span data-ttu-id="809b0-134">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-134">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-135">Testre szabhatja hello listanézet kattintva **oszlopok** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="809b0-135">You can customize hello list view by clicking **Columns** in hello toolbar.</span></span>

<span data-ttu-id="809b0-136">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-136">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-137">Ez lehetővé teszi a toodisplay további mezőket, vagy távolítsa el a mezőket, amelyeknek már jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="809b0-137">This enables you toodisplay additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="809b0-138">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-138">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-139">Hello listanézet elemére kattintva kap az összes rendelkezésre álló részleteit.</span><span class="sxs-lookup"><span data-stu-id="809b0-139">By clicking an item in hello list view, you get all available details about it.</span></span>

<span data-ttu-id="809b0-140">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-140">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")</span></span>


## <a name="filtering-sign-in-activities"></a><span data-ttu-id="809b0-141">A bejelentkezési tevékenységek szűrése</span><span class="sxs-lookup"><span data-stu-id="809b0-141">Filtering sign-in activities</span></span>

<span data-ttu-id="809b0-142">lefelé hello toonarrow jelentette, hogy az Ön működik, végezhet hello bejelentkezések adatokat, és a következő mezők hello tooa szintnek:</span><span class="sxs-lookup"><span data-stu-id="809b0-142">toonarrow down hello reported data tooa level that works for you, you can filter hello sign-ins data using hello following fields:</span></span>

- <span data-ttu-id="809b0-143">Időintervallum</span><span class="sxs-lookup"><span data-stu-id="809b0-143">Time interval</span></span>
- <span data-ttu-id="809b0-144">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="809b0-144">User</span></span>
- <span data-ttu-id="809b0-145">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="809b0-145">Application</span></span>
- <span data-ttu-id="809b0-146">Ügyfél</span><span class="sxs-lookup"><span data-stu-id="809b0-146">Client</span></span>
- <span data-ttu-id="809b0-147">Bejelentkezési állapot</span><span class="sxs-lookup"><span data-stu-id="809b0-147">Sign-in status</span></span>

<span data-ttu-id="809b0-148">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-148">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")</span></span>


<span data-ttu-id="809b0-149">Hello **alatt az időtartam alatt** szűrő lehetővé teszi, hogy tooyou toodefine a hello időkereteket adatokat adott vissza.</span><span class="sxs-lookup"><span data-stu-id="809b0-149">hello **time interval** filter enables tooyou toodefine a timeframe for hello returned data.</span></span>  
<span data-ttu-id="809b0-150">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="809b0-150">Possible values are:</span></span>

- <span data-ttu-id="809b0-151">1 hónap</span><span class="sxs-lookup"><span data-stu-id="809b0-151">1 month</span></span>
- <span data-ttu-id="809b0-152">7 nap</span><span class="sxs-lookup"><span data-stu-id="809b0-152">7 days</span></span>
- <span data-ttu-id="809b0-153">24 óra</span><span class="sxs-lookup"><span data-stu-id="809b0-153">24 hours</span></span>
- <span data-ttu-id="809b0-154">Egyéni</span><span class="sxs-lookup"><span data-stu-id="809b0-154">Custom</span></span>

<span data-ttu-id="809b0-155">Egyéni időkeret kiválasztásakor beállíthatja a kezdő és a záró időpontot.</span><span class="sxs-lookup"><span data-stu-id="809b0-155">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="809b0-156">Hello **felhasználói** szűrő lehetővé teszi a toospecify hello nevét vagy hello egyszerű felhasználónév (UPN) az Ön számára legfontosabb hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="809b0-156">hello **user** filter enables you toospecify hello name or hello user principal name (UPN) of hello user you care about.</span></span>

<span data-ttu-id="809b0-157">Hello **alkalmazás** szűrő lehetővé teszi az Ön számára legfontosabb hello alkalmazás toospecify hello nevét.</span><span class="sxs-lookup"><span data-stu-id="809b0-157">hello **application** filter enables you toospecify hello name of hello application you care about.</span></span>

<span data-ttu-id="809b0-158">Hello **ügyfél** szűrő lehetővé teszi az Ön számára legfontosabb hello eszköz toospecify adatai.</span><span class="sxs-lookup"><span data-stu-id="809b0-158">hello **client** filter enables you toospecify information about hello device you care about.</span></span>

<span data-ttu-id="809b0-159">Hello **bejelentkezési állapot** szűrő tooselect a következő szűrő hello lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="809b0-159">hello **sign-in status** filter enables you tooselect one of hello following filter:</span></span>

- <span data-ttu-id="809b0-160">Összes</span><span class="sxs-lookup"><span data-stu-id="809b0-160">All</span></span>
- <span data-ttu-id="809b0-161">Sikeres</span><span class="sxs-lookup"><span data-stu-id="809b0-161">Success</span></span>
- <span data-ttu-id="809b0-162">Hiba</span><span class="sxs-lookup"><span data-stu-id="809b0-162">Failure</span></span>


## <a name="sign-in-activities-shortcuts"></a><span data-ttu-id="809b0-163">Bejelentkezési tevékenységek parancsikonjai</span><span class="sxs-lookup"><span data-stu-id="809b0-163">Sign-in activities shortcuts</span></span>

<span data-ttu-id="809b0-164">Ezenkívül tooAzure Active Directory, hello Azure-portálon biztosít két további belépési pontok toosign a tevékenységek adatok:</span><span class="sxs-lookup"><span data-stu-id="809b0-164">In addition tooAzure Active Directory, hello Azure portal provides you with two additional entry points toosign-in activities data:</span></span>

- <span data-ttu-id="809b0-165">Felhasználók és csoportok</span><span class="sxs-lookup"><span data-stu-id="809b0-165">Users and groups</span></span>
- <span data-ttu-id="809b0-166">Vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="809b0-166">Enterprise applications</span></span>


### <a name="users-and-groups-sign-ins-activities"></a><span data-ttu-id="809b0-167">Felhasználók és csoportok bejelentkezési tevékenységei</span><span class="sxs-lookup"><span data-stu-id="809b0-167">Users and groups sign-ins activities</span></span>

<span data-ttu-id="809b0-168">Felhasználói bejelentkezési jelentés hello hello információk megtalálhatja válaszok tooquestions többek között:</span><span class="sxs-lookup"><span data-stu-id="809b0-168">With hello information provided by hello user sign-in report, you find answers tooquestions such as:</span></span>

- <span data-ttu-id="809b0-169">Mi az a hello bejelentkezési minta egy olyan felhasználó?</span><span class="sxs-lookup"><span data-stu-id="809b0-169">What is hello sign-in pattern of a user?</span></span>
- <span data-ttu-id="809b0-170">Hány felhasználó jelentkezett be egy adott héten?</span><span class="sxs-lookup"><span data-stu-id="809b0-170">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="809b0-171">Mi az a bejelentkezéseket hello állapotának?</span><span class="sxs-lookup"><span data-stu-id="809b0-171">What’s hello status of these sign-ins?</span></span>



<span data-ttu-id="809b0-172">A belépési pont toothis adatok hello felhasználói bejelentkezési diagram hello **áttekintése** szakaszában **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="809b0-172">Your entry point toothis data is hello user sign-in graph in hello **Overview** section under **Users and groups**.</span></span>

<span data-ttu-id="809b0-173">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-173">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-174">hello felhasználói bejelentkezési grafikon azt ábrázolja, bejelentkezési heti összesítéseinek modulok az összes felhasználó számára egy adott időszakban.</span><span class="sxs-lookup"><span data-stu-id="809b0-174">hello user sign-in graph shows weekly aggregations of sign ins for all users in a given time period.</span></span> <span data-ttu-id="809b0-175">hello hello időszak alapértelmezés 30 nap.</span><span class="sxs-lookup"><span data-stu-id="809b0-175">hello default for hello time period is 30 days.</span></span>

<span data-ttu-id="809b0-176">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-176">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-177">Egy napon hello bejelentkezési Graph kattintáskor hello bejelentkezési tevékenységek részletes listájának lekérése a nap.</span><span class="sxs-lookup"><span data-stu-id="809b0-177">When you click on a day in hello sign-in graph, you get a detailed list of hello sign-in activities for this day.</span></span>

<span data-ttu-id="809b0-178">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-178">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-179">Minden egyes sorára hello bejelentkezési tevékenységek listája ad meg hello kijelölt hello bejelentkezhet például részletes információkat:</span><span class="sxs-lookup"><span data-stu-id="809b0-179">Each row in hello sign-in activities list gives you hello detailed information about hello selected sign-in such as:</span></span>

* <span data-ttu-id="809b0-180">Ki jelentkezett be?</span><span class="sxs-lookup"><span data-stu-id="809b0-180">Who has signed in?</span></span>
* <span data-ttu-id="809b0-181">Mi volt hello kapcsolódó UPN?</span><span class="sxs-lookup"><span data-stu-id="809b0-181">What was hello related UPN?</span></span>
* <span data-ttu-id="809b0-182">Milyen alkalmazás hello célja hello bejelentkezés volt?</span><span class="sxs-lookup"><span data-stu-id="809b0-182">What application was hello target of hello sign-in?</span></span>
* <span data-ttu-id="809b0-183">Mi az a hello IP-címe hello bejelentkezés?</span><span class="sxs-lookup"><span data-stu-id="809b0-183">What is hello IP address of hello sign-in?</span></span>
* <span data-ttu-id="809b0-184">Mi volt a hello állapotának hello bejelentkezés?</span><span class="sxs-lookup"><span data-stu-id="809b0-184">What was hello status of hello sign-in?</span></span>

<span data-ttu-id="809b0-185">Hello **bejelentkezések** beállítás lehetővé teszi az összes felhasználói bejelentkezések teljes körű áttekintést.</span><span class="sxs-lookup"><span data-stu-id="809b0-185">hello **Sign-ins** option gives you a complete overview of all user sign-ins.</span></span>

<span data-ttu-id="809b0-186">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-186">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")</span></span>



## <a name="usage-of-managed-applications"></a><span data-ttu-id="809b0-187">Felügyelt alkalmazások használati adatai</span><span class="sxs-lookup"><span data-stu-id="809b0-187">Usage of managed applications</span></span>

<span data-ttu-id="809b0-188">A bejelentkezési információk alkalmazás-központú nézetével az alábbi kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="809b0-188">With an application-centric view of your sign-in data, you can answer questions such as:</span></span>

* <span data-ttu-id="809b0-189">Ki használja az alkalmazásaimat?</span><span class="sxs-lookup"><span data-stu-id="809b0-189">Who is using my applications?</span></span>
* <span data-ttu-id="809b0-190">Mik azok a hello legfontosabb 3 alkalmazást a szervezetében?</span><span class="sxs-lookup"><span data-stu-id="809b0-190">What are hello top 3 applications in your organization?</span></span>
* <span data-ttu-id="809b0-191">Nemrégiben új alkalmazást adtam ki.</span><span class="sxs-lookup"><span data-stu-id="809b0-191">I have recently rolled out an application.</span></span> <span data-ttu-id="809b0-192">Mennyire sikeres?</span><span class="sxs-lookup"><span data-stu-id="809b0-192">How is it doing?</span></span>

<span data-ttu-id="809b0-193">A belépési pont toothis adatok hello legfontosabb 3 alkalmazást a szervezeten belül hello utolsó 30 nap jelentést a hello **áttekintése** szakaszában **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="809b0-193">Your entry point toothis data is hello top 3 applications in your organization within hello last 30 days report in hello **Overview** section under **Enterprise applications**.</span></span>

<span data-ttu-id="809b0-194">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-194">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-195">hello alkalmazás használati diagramon heti összesítéseinek bejelentkezések az első 3 alkalmazások egy adott időszakban.</span><span class="sxs-lookup"><span data-stu-id="809b0-195">hello app usage graph weekly aggregations of sign ins for your top 3 applications in a given time period.</span></span> <span data-ttu-id="809b0-196">hello hello időszak alapértelmezés 30 nap.</span><span class="sxs-lookup"><span data-stu-id="809b0-196">hello default for hello time period is 30 days.</span></span>

<span data-ttu-id="809b0-197">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-197">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")</span></span>

<span data-ttu-id="809b0-198">Ha szeretné, amelyen beállíthatja hello fókusz egy adott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="809b0-198">If you want to, you can set hello focus on a specific application.</span></span>


<span data-ttu-id="809b0-199">![Jelentéskészítés](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Jelentéskészítés")</span><span class="sxs-lookup"><span data-stu-id="809b0-199">![Reporting](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")</span></span>

<span data-ttu-id="809b0-200">Gombra a hello alkalmazás használati diagramot egy napon kapott hello bejelentkezési tevékenységek részletes listáját.</span><span class="sxs-lookup"><span data-stu-id="809b0-200">When you click on a day in hello app usage graph, you get a detailed list of hello sign-in activities.</span></span>


<span data-ttu-id="809b0-201">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-201">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")</span></span>


<span data-ttu-id="809b0-202">Hello **bejelentkezések** lehetőséget ad bejelentkezési események tooyour alkalmazások teljes áttekintése.</span><span class="sxs-lookup"><span data-stu-id="809b0-202">hello **Sign-ins** option gives you a complete overview of all sign-in events tooyour applications.</span></span>

<span data-ttu-id="809b0-203">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="809b0-203">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")</span></span>



## <a name="next-steps"></a><span data-ttu-id="809b0-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="809b0-204">Next steps</span></span>

<span data-ttu-id="809b0-205">Ha azt szeretné, hogy a bejelentkezési tevékenység hibakódokkal kapcsolatban további tooknow, tekintse meg a hello [bejelentkezési tevékenység jelentés hibakódok hello Azure Active Directory portálon](active-directory-reporting-activity-sign-ins-errors.md).</span><span class="sxs-lookup"><span data-stu-id="809b0-205">If you want tooknow more about sign-in activity error codes, see hello [Sign-in activity report error codes in hello Azure Active Directory portal](active-directory-reporting-activity-sign-ins-errors.md).</span></span>


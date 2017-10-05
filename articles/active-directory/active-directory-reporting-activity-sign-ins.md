---
title: "Bejelentkezési tevékenységre vonatkozó jelentések az Azure Active Directory portálon | Microsoft Docs"
description: "A bejelentkezési tevékenységre vonatkozó jelentések az Azure Active Directory portálon – bevezetés"
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
ms.openlocfilehash: b9e61950654ba427b09dd608d354589a0804aaa5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sign-in-activity-reports-in-the-azure-active-directory-portal"></a><span data-ttu-id="8cb29-103">Bejelentkezési tevékenységre vonatkozó jelentések az Azure Active Directory portálon</span><span class="sxs-lookup"><span data-stu-id="8cb29-103">Sign-in activity reports in the Azure Active Directory portal</span></span>

<span data-ttu-id="8cb29-104">Az [Azure Portalon](https://portal.azure.com) az Azure Active Directory (Azure AD) jelentéskészítési funkciójával minden szükséges információhoz hozzájuthat a környezetével kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8cb29-104">With Azure Active Directory (Azure AD) reporting in the [Azure portal](https://portal.azure.com), you can get the information you need to determine how your environment is doing.</span></span>

<span data-ttu-id="8cb29-105">Az Azure Active Directory jelentéskészítési architektúrája a következő elemekből áll:</span><span class="sxs-lookup"><span data-stu-id="8cb29-105">The reporting architecture in Azure Active Directory consists of the following components:</span></span>

- <span data-ttu-id="8cb29-106">**Tevékenység**</span><span class="sxs-lookup"><span data-stu-id="8cb29-106">**Activity**</span></span> 
    - <span data-ttu-id="8cb29-107">**Bejelentkezési tevékenységek** – A felügyelt alkalmazások használatával és a felhasználók bejelentkezési tevékenységeivel kapcsolatos információk</span><span class="sxs-lookup"><span data-stu-id="8cb29-107">**Sign-in activities** – Information about the usage of managed applications and user sign-in activities</span></span>
    - <span data-ttu-id="8cb29-108">**Naplók** – Rendszertevékenység információk a felhasználó- és csoportfelügyeletre, valamint a felügyelt alkalmazásokra és a címtártevékenységekre vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="8cb29-108">**Audit logs** - System activity information about users and group management, your managed applications and directory activities.</span></span>
- <span data-ttu-id="8cb29-109">**Biztonság**</span><span class="sxs-lookup"><span data-stu-id="8cb29-109">**Security**</span></span> 
    - <span data-ttu-id="8cb29-110">**Kockázatos bejelentkezések** – A kockázatos bejelentkezés egy olyan bejelentkezési kísérletet jelöl, amelyet elképzelhető, hogy olyan személy hajtott végre, aki nem a felhasználói fiók jogos tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="8cb29-110">**Risky sign-ins** - A risky sign-in is an indicator for a sign-in attempt that might have been performed by someone who is not the legitimate owner of a user account.</span></span> <span data-ttu-id="8cb29-111">További részletek: Kockázatos bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="8cb29-111">For more details, see Risky sign-ins.</span></span>
    - <span data-ttu-id="8cb29-112">**Kockázatosként megjelölt felhasználók** – A kockázatos felhasználó egy olyan felhasználói fiókot jelöl, amelynek elképzelhető, hogy sérült a biztonsága.</span><span class="sxs-lookup"><span data-stu-id="8cb29-112">**Users flagged for risk** - A risky user is an indicator for a user account that might have been compromised.</span></span> <span data-ttu-id="8cb29-113">További részletek: Kockázatosként megjelölt felhasználók.</span><span class="sxs-lookup"><span data-stu-id="8cb29-113">For more details, see Users flagged for risk.</span></span>

<span data-ttu-id="8cb29-114">Ez a témakör áttekintést nyújt a bejelentkezési tevékenységekről.</span><span class="sxs-lookup"><span data-stu-id="8cb29-114">This topic gives you an overview of the sign-in activities.</span></span>

## <a name="pre-requisite"></a><span data-ttu-id="8cb29-115">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="8cb29-115">Pre-requisite</span></span>

### <a name="who-can-access-the-data"></a><span data-ttu-id="8cb29-116">Ki férhet hozzá az adatokhoz?</span><span class="sxs-lookup"><span data-stu-id="8cb29-116">Who can access the data?</span></span>
* <span data-ttu-id="8cb29-117">A biztonsági rendszergazda vagy biztonsági olvasó szerepkörű felhasználók</span><span class="sxs-lookup"><span data-stu-id="8cb29-117">Users in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="8cb29-118">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="8cb29-118">Global Admins</span></span>
* <span data-ttu-id="8cb29-119">Bármely (nem rendszergazda jogosultságú) felhasználó hozzáfér a saját bejelentkezéseihez</span><span class="sxs-lookup"><span data-stu-id="8cb29-119">Any user (non-admins) can access their own sign-ins</span></span> 

### <a name="what-azure-ad-license-do-you-need-to-access-sign-in-activity"></a><span data-ttu-id="8cb29-120">Milyen Azure AD-licencre van szükség a bejelentkezési tevékenységhez való hozzáféréshez?</span><span class="sxs-lookup"><span data-stu-id="8cb29-120">What Azure AD license do you need to access sign-in activity?</span></span>
* <span data-ttu-id="8cb29-121">A bérlőjének prémium szintű Azure AD-licenccel kell rendelkeznie az összes bejelentkezési tevékenység jelentésének megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="8cb29-121">Your tenant must have an Azure AD Premium license associated with it to see the all up sign-in activity report</span></span>


## <a name="signs-in-activities"></a><span data-ttu-id="8cb29-122">Bejelentkezési tevékenységek</span><span class="sxs-lookup"><span data-stu-id="8cb29-122">Signs-in activities</span></span>

<span data-ttu-id="8cb29-123">A felhasználók bejelentkezési jelentésében szereplő információkból az alábbi kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="8cb29-123">With the information provided by the user sign-in report, you find answers to questions such as:</span></span>

* <span data-ttu-id="8cb29-124">Milyen egy adott felhasználó bejelentkezési mintázata?</span><span class="sxs-lookup"><span data-stu-id="8cb29-124">What is the sign-in pattern of a user?</span></span>
* <span data-ttu-id="8cb29-125">Hány felhasználó jelentkezett be egy adott héten?</span><span class="sxs-lookup"><span data-stu-id="8cb29-125">How many users have users signed in over a week?</span></span>
* <span data-ttu-id="8cb29-126">Milyen állapotúak ezek a bejelentkezések?</span><span class="sxs-lookup"><span data-stu-id="8cb29-126">What’s the status of these sign-ins?</span></span>

<span data-ttu-id="8cb29-127">A **Bejelentkezések** menüponton át vezet az út a bejelentkezési tevékenység adataihoz, a menüpont az **Azure Active** Tevékenység szakaszában található.</span><span class="sxs-lookup"><span data-stu-id="8cb29-127">Your first entry point to all sign-in activities data is **Sign-ins** in the Activity section of **Azure Active**.</span></span>


<span data-ttu-id="8cb29-128">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-128">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/61.png "Sign-in activity")</span></span>


<span data-ttu-id="8cb29-129">Az auditnapló alapértelmezett listanézete az alábbi adatokat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="8cb29-129">An audit log has a default list view that shows:</span></span>

- <span data-ttu-id="8cb29-130">a kapcsolódó felhasználó</span><span class="sxs-lookup"><span data-stu-id="8cb29-130">the related user</span></span>
- <span data-ttu-id="8cb29-131">az alkalmazás, amelybe a felhasználó bejelentkezett</span><span class="sxs-lookup"><span data-stu-id="8cb29-131">the application the user has signed-in to</span></span>
- <span data-ttu-id="8cb29-132">a bejelentkezési állapot</span><span class="sxs-lookup"><span data-stu-id="8cb29-132">the sign-in status</span></span>
- <span data-ttu-id="8cb29-133">a bejelentkezési időpont</span><span class="sxs-lookup"><span data-stu-id="8cb29-133">the sign-in time</span></span>

<span data-ttu-id="8cb29-134">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-134">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-135">A listanézetet az eszköztár **Oszlopok** elemére kattintva lehet testre szabni.</span><span class="sxs-lookup"><span data-stu-id="8cb29-135">You can customize the list view by clicking **Columns** in the toolbar.</span></span>

<span data-ttu-id="8cb29-136">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-136">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/19.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-137">További mezőket jeleníthet meg, vagy eltávolíthatja a már megjelenített mezőket.</span><span class="sxs-lookup"><span data-stu-id="8cb29-137">This enables you to display additional fields or remove fields that are already displayed.</span></span>

<span data-ttu-id="8cb29-138">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-138">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/42.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-139">A listanézet egyik elemére kattintva megtekintheti annak elérhető összes részletét.</span><span class="sxs-lookup"><span data-stu-id="8cb29-139">By clicking an item in the list view, you get all available details about it.</span></span>

<span data-ttu-id="8cb29-140">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-140">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/43.png "Sign-in activity")</span></span>


## <a name="filtering-sign-in-activities"></a><span data-ttu-id="8cb29-141">A bejelentkezési tevékenységek szűrése</span><span class="sxs-lookup"><span data-stu-id="8cb29-141">Filtering sign-in activities</span></span>

<span data-ttu-id="8cb29-142">A jelentésben lévő adatok megfelelő szintű szűkítéséhez az alábbi mezőkkel szűrheti a bejelentkezési adatokat:</span><span class="sxs-lookup"><span data-stu-id="8cb29-142">To narrow down the reported data to a level that works for you, you can filter the sign-ins data using the following fields:</span></span>

- <span data-ttu-id="8cb29-143">Időintervallum</span><span class="sxs-lookup"><span data-stu-id="8cb29-143">Time interval</span></span>
- <span data-ttu-id="8cb29-144">Felhasználó</span><span class="sxs-lookup"><span data-stu-id="8cb29-144">User</span></span>
- <span data-ttu-id="8cb29-145">Alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8cb29-145">Application</span></span>
- <span data-ttu-id="8cb29-146">Ügyfél</span><span class="sxs-lookup"><span data-stu-id="8cb29-146">Client</span></span>
- <span data-ttu-id="8cb29-147">Bejelentkezési állapot</span><span class="sxs-lookup"><span data-stu-id="8cb29-147">Sign-in status</span></span>

<span data-ttu-id="8cb29-148">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-148">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/44.png "Sign-in activity")</span></span>


<span data-ttu-id="8cb29-149">Az **Időintervallum** szűrővel időkeretet lehet meghatározni a visszaadott adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="8cb29-149">The **time interval** filter enables to you to define a timeframe for the returned data.</span></span>  
<span data-ttu-id="8cb29-150">Lehetséges értékek:</span><span class="sxs-lookup"><span data-stu-id="8cb29-150">Possible values are:</span></span>

- <span data-ttu-id="8cb29-151">1 hónap</span><span class="sxs-lookup"><span data-stu-id="8cb29-151">1 month</span></span>
- <span data-ttu-id="8cb29-152">7 nap</span><span class="sxs-lookup"><span data-stu-id="8cb29-152">7 days</span></span>
- <span data-ttu-id="8cb29-153">24 óra</span><span class="sxs-lookup"><span data-stu-id="8cb29-153">24 hours</span></span>
- <span data-ttu-id="8cb29-154">Egyéni</span><span class="sxs-lookup"><span data-stu-id="8cb29-154">Custom</span></span>

<span data-ttu-id="8cb29-155">Egyéni időkeret kiválasztásakor beállíthatja a kezdő és a záró időpontot.</span><span class="sxs-lookup"><span data-stu-id="8cb29-155">When you select a custom timeframe, you can configure a start time and an end time.</span></span>

<span data-ttu-id="8cb29-156">A **Felhasználó** szűrővel egy konkrét felhasználó nevét vagy egyszerű felhasználónevét adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="8cb29-156">The **user** filter enables you to specify the name or the user principal name (UPN) of the user you care about.</span></span>

<span data-ttu-id="8cb29-157">Az **Alkalmazás** szűrővel egy konkrét alkalmazás nevét adhatja meg.</span><span class="sxs-lookup"><span data-stu-id="8cb29-157">The **application** filter enables you to specify the name of the application you care about.</span></span>

<span data-ttu-id="8cb29-158">Az **Ügyfél** szűrővel egy konkrét eszközhöz tartozó információt adhat meg.</span><span class="sxs-lookup"><span data-stu-id="8cb29-158">The **client** filter enables you to specify information about the device you care about.</span></span>

<span data-ttu-id="8cb29-159">A **Bejelentkezési állapot** szűrővel az alábbi szűrők egyikét választhatja ki:</span><span class="sxs-lookup"><span data-stu-id="8cb29-159">The **sign-in status** filter enables you to select one of the following filter:</span></span>

- <span data-ttu-id="8cb29-160">Összes</span><span class="sxs-lookup"><span data-stu-id="8cb29-160">All</span></span>
- <span data-ttu-id="8cb29-161">Sikeres</span><span class="sxs-lookup"><span data-stu-id="8cb29-161">Success</span></span>
- <span data-ttu-id="8cb29-162">Hiba</span><span class="sxs-lookup"><span data-stu-id="8cb29-162">Failure</span></span>


## <a name="sign-in-activities-shortcuts"></a><span data-ttu-id="8cb29-163">Bejelentkezési tevékenységek parancsikonjai</span><span class="sxs-lookup"><span data-stu-id="8cb29-163">Sign-in activities shortcuts</span></span>

<span data-ttu-id="8cb29-164">Az Azure Active Directory mellett az Azure Portal két további lehetőséget biztosít a bejelentkezési tevékenységek adatainak elérésére:</span><span class="sxs-lookup"><span data-stu-id="8cb29-164">In addition to Azure Active Directory, the Azure portal provides you with two additional entry points to sign-in activities data:</span></span>

- <span data-ttu-id="8cb29-165">Felhasználók és csoportok</span><span class="sxs-lookup"><span data-stu-id="8cb29-165">Users and groups</span></span>
- <span data-ttu-id="8cb29-166">Vállalati alkalmazások</span><span class="sxs-lookup"><span data-stu-id="8cb29-166">Enterprise applications</span></span>


### <a name="users-and-groups-sign-ins-activities"></a><span data-ttu-id="8cb29-167">Felhasználók és csoportok bejelentkezési tevékenységei</span><span class="sxs-lookup"><span data-stu-id="8cb29-167">Users and groups sign-ins activities</span></span>

<span data-ttu-id="8cb29-168">A felhasználók bejelentkezési jelentésében szereplő információkból az alábbi kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="8cb29-168">With the information provided by the user sign-in report, you find answers to questions such as:</span></span>

- <span data-ttu-id="8cb29-169">Milyen egy adott felhasználó bejelentkezési mintázata?</span><span class="sxs-lookup"><span data-stu-id="8cb29-169">What is the sign-in pattern of a user?</span></span>
- <span data-ttu-id="8cb29-170">Hány felhasználó jelentkezett be egy adott héten?</span><span class="sxs-lookup"><span data-stu-id="8cb29-170">How many users have users signed in over a week?</span></span>
- <span data-ttu-id="8cb29-171">Milyen állapotúak ezek a bejelentkezések?</span><span class="sxs-lookup"><span data-stu-id="8cb29-171">What’s the status of these sign-ins?</span></span>



<span data-ttu-id="8cb29-172">Az adatok megtekintését a felhasználók bejelentkezési grafikonjával kezdheti, amely az **Áttekintés** szakaszban, a **Felhasználók és csoportok** területen.</span><span class="sxs-lookup"><span data-stu-id="8cb29-172">Your entry point to this data is the user sign-in graph in the **Overview** section under **Users and groups**.</span></span>

<span data-ttu-id="8cb29-173">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-173">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/45.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-174">A felhasználók bejelentkezési grafikonja az összes felhasználó bejelentkezéseinek összesítését ábrázolja egy adott időszakban.</span><span class="sxs-lookup"><span data-stu-id="8cb29-174">The user sign-in graph shows weekly aggregations of sign ins for all users in a given time period.</span></span> <span data-ttu-id="8cb29-175">Az alapértelmezett időszak 30 nap.</span><span class="sxs-lookup"><span data-stu-id="8cb29-175">The default for the time period is 30 days.</span></span>

<span data-ttu-id="8cb29-176">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-176">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/46.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-177">A bejelentkezési grafikon egyik napjára kattintva részletes listát kap az adott nap bejelentkezési tevékenységeiről.</span><span class="sxs-lookup"><span data-stu-id="8cb29-177">When you click on a day in the sign-in graph, you get a detailed list of the sign-in activities for this day.</span></span>

<span data-ttu-id="8cb29-178">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-178">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/41.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-179">A bejelentkezési tevékenységek listájának minden sora részletes információkat tartalmaz a kijelölt bejelentkezésről:</span><span class="sxs-lookup"><span data-stu-id="8cb29-179">Each row in the sign-in activities list gives you the detailed information about the selected sign-in such as:</span></span>

* <span data-ttu-id="8cb29-180">Ki jelentkezett be?</span><span class="sxs-lookup"><span data-stu-id="8cb29-180">Who has signed in?</span></span>
* <span data-ttu-id="8cb29-181">Mi volt a kapcsolódó egyszerű felhasználónév?</span><span class="sxs-lookup"><span data-stu-id="8cb29-181">What was the related UPN?</span></span>
* <span data-ttu-id="8cb29-182">Melyik alkalmazás volt a bejelentkezés célja?</span><span class="sxs-lookup"><span data-stu-id="8cb29-182">What application was the target of the sign-in?</span></span>
* <span data-ttu-id="8cb29-183">Mi a bejelentkezéshez tartozó IP-cím?</span><span class="sxs-lookup"><span data-stu-id="8cb29-183">What is the IP address of the sign-in?</span></span>
* <span data-ttu-id="8cb29-184">Mi volt a bejelentkezés állapota?</span><span class="sxs-lookup"><span data-stu-id="8cb29-184">What was the status of the sign-in?</span></span>

<span data-ttu-id="8cb29-185">A **Bejelentkezések** lehetőség teljes körű áttekintést biztosít az összes felhasználói bejelentkezésről.</span><span class="sxs-lookup"><span data-stu-id="8cb29-185">The **Sign-ins** option gives you a complete overview of all user sign-ins.</span></span>

<span data-ttu-id="8cb29-186">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-186">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/51.png "Sign-in activity")</span></span>



## <a name="usage-of-managed-applications"></a><span data-ttu-id="8cb29-187">Felügyelt alkalmazások használati adatai</span><span class="sxs-lookup"><span data-stu-id="8cb29-187">Usage of managed applications</span></span>

<span data-ttu-id="8cb29-188">A bejelentkezési információk alkalmazás-központú nézetével az alábbi kérdésekre kaphat választ:</span><span class="sxs-lookup"><span data-stu-id="8cb29-188">With an application-centric view of your sign-in data, you can answer questions such as:</span></span>

* <span data-ttu-id="8cb29-189">Ki használja az alkalmazásaimat?</span><span class="sxs-lookup"><span data-stu-id="8cb29-189">Who is using my applications?</span></span>
* <span data-ttu-id="8cb29-190">Melyik a szervezet 3 legnépszerűbb alkalmazása?</span><span class="sxs-lookup"><span data-stu-id="8cb29-190">What are the top 3 applications in your organization?</span></span>
* <span data-ttu-id="8cb29-191">Nemrégiben új alkalmazást adtam ki.</span><span class="sxs-lookup"><span data-stu-id="8cb29-191">I have recently rolled out an application.</span></span> <span data-ttu-id="8cb29-192">Mennyire sikeres?</span><span class="sxs-lookup"><span data-stu-id="8cb29-192">How is it doing?</span></span>

<span data-ttu-id="8cb29-193">Az adatok megtekintését a szervezet az elmúlt 30 nap alatt legnépszerűbb 3 alkalmazásáról szóló jelentéssel kezdheti az **Áttekintés** szakaszban, a **Vállalati alkalmazások** területen.</span><span class="sxs-lookup"><span data-stu-id="8cb29-193">Your entry point to this data is the top 3 applications in your organization within the last 30 days report in the **Overview** section under **Enterprise applications**.</span></span>

<span data-ttu-id="8cb29-194">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-194">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/64.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-195">A 3 legnépszerűbb alkalmazásba való bejelentkezések heti összesítő grafikonja egy adott időszak során.</span><span class="sxs-lookup"><span data-stu-id="8cb29-195">The app usage graph weekly aggregations of sign ins for your top 3 applications in a given time period.</span></span> <span data-ttu-id="8cb29-196">Az alapértelmezett időszak 30 nap.</span><span class="sxs-lookup"><span data-stu-id="8cb29-196">The default for the time period is 30 days.</span></span>

<span data-ttu-id="8cb29-197">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-197">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/47.png "Sign-in activity")</span></span>

<span data-ttu-id="8cb29-198">Igény esetén egy adott alkalmazást is kiemelhet.</span><span class="sxs-lookup"><span data-stu-id="8cb29-198">If you want to, you can set the focus on a specific application.</span></span>


<span data-ttu-id="8cb29-199">![Jelentéskészítés](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Jelentéskészítés")</span><span class="sxs-lookup"><span data-stu-id="8cb29-199">![Reporting](./media/active-directory-reporting-activity-sign-ins/single_spp_usage_graph.png "Reporting")</span></span>

<span data-ttu-id="8cb29-200">Az alkalmazáshasználati grafikon egyik napjára kattintva részletes listát kap a bejelentkezési tevékenységekről.</span><span class="sxs-lookup"><span data-stu-id="8cb29-200">When you click on a day in the app usage graph, you get a detailed list of the sign-in activities.</span></span>


<span data-ttu-id="8cb29-201">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-201">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/48.png "Sign-in activity")</span></span>


<span data-ttu-id="8cb29-202">A **Bejelentkezések** lehetőség az alkalmazások összes bejelentkezési eseményének teljes körű áttekintését biztosítja.</span><span class="sxs-lookup"><span data-stu-id="8cb29-202">The **Sign-ins** option gives you a complete overview of all sign-in events to your applications.</span></span>

<span data-ttu-id="8cb29-203">![Bejelentkezési tevékenység](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")</span><span class="sxs-lookup"><span data-stu-id="8cb29-203">![Sign-in activity](./media/active-directory-reporting-activity-sign-ins/49.png "Sign-in activity")</span></span>



## <a name="next-steps"></a><span data-ttu-id="8cb29-204">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8cb29-204">Next steps</span></span>

<span data-ttu-id="8cb29-205">Ha többet szeretne megtudni a bejelentkezési tevékenységek hibakódjairól, lásd: [Bejelentkezési tevékenységekre vonatkozó jelentések hibakódjai az Azure Active Directory portálon](active-directory-reporting-activity-sign-ins-errors.md).</span><span class="sxs-lookup"><span data-stu-id="8cb29-205">If you want to know more about sign-in activity error codes, see the [Sign-in activity report error codes in the Azure Active Directory portal](active-directory-reporting-activity-sign-ins-errors.md).</span></span>


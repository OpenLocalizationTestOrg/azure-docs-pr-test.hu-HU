---
title: "Az Azure Active Directory naplózási API-referencia |} Microsoft Docs"
description: "Ismerkedés az Azure Active Directory naplózási API-hoz"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 44e46be8-09e5-4981-be2b-d474aaa92792
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/05/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 573e940c5390e7b990d889681eb37b73c5b253d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-active-directory-audit-api-reference"></a><span data-ttu-id="5c8fb-103">Az Azure Active Directory naplózási API-referencia</span><span class="sxs-lookup"><span data-stu-id="5c8fb-103">Azure Active Directory audit API reference</span></span>
<span data-ttu-id="5c8fb-104">Ez a témakör az Azure Active Directory reporting API-val kapcsolatos témakörök gyűjteményét részét képezi.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-104">This topic is part of a collection of topics about the Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="5c8fb-105">Az Azure AD jelentéskészítési lehetőséget biztosít az API-k, amely lehetővé teszi a kód vagy a kapcsolódó eszközök naplózási adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-105">Azure AD reporting provides you with an API that enables you to access audit data using code or related tools.</span></span>
<span data-ttu-id="5c8fb-106">Ez a témakör a hatóköre biztosításához kapcsolatos útmutatót a **API naplózási**.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-106">The scope of this topic is to provide you with reference information about the **audit API**.</span></span>

<span data-ttu-id="5c8fb-107">Lásd:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-107">See:</span></span>

* <span data-ttu-id="5c8fb-108">[A naplók](active-directory-reporting-azure-portal.md#activity-reports) további információ</span><span class="sxs-lookup"><span data-stu-id="5c8fb-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>

* <span data-ttu-id="5c8fb-109">[Bevezetés az Azure Active Directory Reporting API használatába](active-directory-reporting-api-getting-started.md) a reporting API-val kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-109">[Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about the reporting API.</span></span>


<span data-ttu-id="5c8fb-110">Esetén:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-110">For:</span></span>

- <span data-ttu-id="5c8fb-111">Gyakori kérdések, olvassa el a [– gyakori kérdések](active-directory-reporting-faq.md)</span><span class="sxs-lookup"><span data-stu-id="5c8fb-111">Frequently asked questions, read our [FAQ](active-directory-reporting-faq.md)</span></span> 

- <span data-ttu-id="5c8fb-112">Adjon ki [fájlt egy támogatási jegy](active-directory-troubleshooting-support-howto.md)</span><span class="sxs-lookup"><span data-stu-id="5c8fb-112">Issues, please [file a support ticket](active-directory-troubleshooting-support-howto.md)</span></span> 


## <a name="who-can-access-the-data"></a><span data-ttu-id="5c8fb-113">Ki férhet hozzá az adatokhoz?</span><span class="sxs-lookup"><span data-stu-id="5c8fb-113">Who can access the data?</span></span>
* <span data-ttu-id="5c8fb-114">A biztonsági rendszergazda vagy biztonsági olvasó szerepkörű felhasználók</span><span class="sxs-lookup"><span data-stu-id="5c8fb-114">Users in the Security Admin or Security Reader role</span></span>
* <span data-ttu-id="5c8fb-115">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="5c8fb-115">Global Admins</span></span>
* <span data-ttu-id="5c8fb-116">Bármely alkalmazás, amely rendelkezik hozzáférési az API-t (alkalmazás engedélyezési is lehet a telepítő csak a globális rendszergazdai engedély alapján)</span><span class="sxs-lookup"><span data-stu-id="5c8fb-116">Any app that has authorization to access the API (app authorization can be setup only based on Global Admin’s permission)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c8fb-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5c8fb-117">Prerequisites</span></span>
<span data-ttu-id="5c8fb-118">Ez a jelentés eléréséhez a Reporting API-n keresztül, ha rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-118">In order to access this report through the Reporting API, you must have:</span></span>

* <span data-ttu-id="5c8fb-119">Egy [Azure Active Directory ingyenes vagy jobb edition](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="5c8fb-119">An [Azure Active Directory Free or better edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="5c8fb-120">Befejeződött a [Előfeltételek az Azure AD reporting API eléréséhez](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="5c8fb-120">Completed the [prerequisites to access the Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-the-api"></a><span data-ttu-id="5c8fb-121">Az API elérése</span><span class="sxs-lookup"><span data-stu-id="5c8fb-121">Accessing the API</span></span>
<span data-ttu-id="5c8fb-122">Vagy az API keresztül hozzáférhet a [Graph Explorer](https://graphexplorer2.cloudapp.net) vagy programozott módon, például PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-122">You can either access this API through the [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="5c8fb-123">Ahhoz, hogy megfelelően értelmezni az OData-szűrőszintaxisának AAD Graph REST-hívásokban használt PowerShell, a backtick kell használnia (más néven: aposztróf) "karaktert" a $ karaktert.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-123">In order for PowerShell to correctly interpret the OData filter syntax used in AAD Graph REST calls, you must use the backtick (aka: grave accent) character to “escape” the $ character.</span></span> <span data-ttu-id="5c8fb-124">A backtick karakter funkcionál [PowerShell escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé téve a $ karaktert a literális értelmezésének, és elkerülheti a PowerShell változó neveként zavaró, PowerShell (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="5c8fb-124">The backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell to do a literal interpretation of the $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="5c8fb-125">A jelen témakör elsősorban a Graph Explorer.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-125">The focus of this topic is on the Graph Explorer.</span></span> <span data-ttu-id="5c8fb-126">A PowerShell például megjelenik ez [PowerShell-parancsfájl](active-directory-reporting-api-audit-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="5c8fb-126">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-audit-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="5c8fb-127">API-végpont</span><span class="sxs-lookup"><span data-stu-id="5c8fb-127">API Endpoint</span></span>
<span data-ttu-id="5c8fb-128">Ez az API a következő URI használatával érhető el:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-128">You can access this API using the following URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

<span data-ttu-id="5c8fb-129">Az Azure AD naplózási API-t (OData tördelési használatával) által visszaadott rekordok száma korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-129">There is no limit on the number of records returned by the Azure AD audit API (using OData pagination).</span></span>
<span data-ttu-id="5c8fb-130">A jelentés adatainak megőrzési-korlátok, tekintse meg [adatmegőrzési Reporting](active-directory-reporting-retention.md).</span><span class="sxs-lookup"><span data-stu-id="5c8fb-130">For retention limits on reporting data, check out [Reporting Retention Policies](active-directory-reporting-retention.md).</span></span>

<span data-ttu-id="5c8fb-131">Ez a hívás kötegekben adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-131">This call returns the data in batches.</span></span> <span data-ttu-id="5c8fb-132">Minden egyes legfeljebb 1000 rekord rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-132">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="5c8fb-133">A következő mérési adatköteget a rekordok megtekintéséhez használja a következő hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-133">To get the next batch of records, use the Next link.</span></span> <span data-ttu-id="5c8fb-134">A skiptoken adatok lehívása az első visszaadott rekordok készletét.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-134">Get the skiptoken information from the first set of returned records.</span></span> <span data-ttu-id="5c8fb-135">A kihagyási lexikális elem az eredmény végén értéke lesz.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-135">The skip token will be at the end of the result set.</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a><span data-ttu-id="5c8fb-136">Támogatott szűrők</span><span class="sxs-lookup"><span data-stu-id="5c8fb-136">Supported filters</span></span>
<span data-ttu-id="5c8fb-137">Az API-k által visszaadott rekordok számának megadásával szűkíthető hívható meg egy szűrő formában.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-137">You can narrow down the number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="5c8fb-138">Bejelentkezés az API-hoz kapcsolódó adatok, a következő szűrőket támogatottak:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-138">For sign-in API related data, the following filters are supported:</span></span>

* <span data-ttu-id="5c8fb-139">**$top =\<visszaadott száma\>**  - visszaadott rekordok számát.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-139">**$top=\<number of records to be returned\>** - to limit the number of returned records.</span></span> <span data-ttu-id="5c8fb-140">Ez az egy drága művelet.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-140">This is an expensive operation.</span></span> <span data-ttu-id="5c8fb-141">Ha szeretne visszaállítani az objektumok több ezer ne használjon a szűrőt.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-141">You should not use this filter if you want to return thousands of objects.</span></span>     
* <span data-ttu-id="5c8fb-142">**$filter =\<a szűrő utasítás\>**  – megadhatja, támogatott szűrő mezők alapján az Önt érdeklő rekordok</span><span class="sxs-lookup"><span data-stu-id="5c8fb-142">**$filter=\<your filter statement\>** - to specify, on the basis of supported filter fields, the type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="5c8fb-143">Támogatott Szűrő mezőket és operátorok</span><span class="sxs-lookup"><span data-stu-id="5c8fb-143">Supported filter fields and operators</span></span>
<span data-ttu-id="5c8fb-144">Adja meg az Önt érdeklő rekordok, egy szűrő utasítást, amely egy vagy a következő szűrő mező tartalmazhat hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-144">To specify the type of records you care about, you can build a filter statement that can contain either one or a combination of the following filter fields:</span></span>

* <span data-ttu-id="5c8fb-145">[activityDate](#activitydate) -dátum vagy dátumtartomány meghatározása</span><span class="sxs-lookup"><span data-stu-id="5c8fb-145">[activityDate](#activitydate)  - defines a date or date range</span></span>
* <span data-ttu-id="5c8fb-146">[kategória](#category) -határozza meg a szűrni kívánt kategóriát.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-146">[category](#category) - defines the category you want to filter on.</span></span>
* <span data-ttu-id="5c8fb-147">[activityStatus](#activitystatus) -tevékenység állapotát határozza meg</span><span class="sxs-lookup"><span data-stu-id="5c8fb-147">[activityStatus](#activitystatus) - defines the status of an activity</span></span>
* <span data-ttu-id="5c8fb-148">[az activityType](#activitytype) -tevékenység típusát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="5c8fb-148">[activityType](#activitytype)  - defines the type of an activity</span></span>
* <span data-ttu-id="5c8fb-149">[tevékenység](#activity) -karakterláncot határozza meg a tevékenység</span><span class="sxs-lookup"><span data-stu-id="5c8fb-149">[activity](#activity) - defines the activity as string</span></span>  
* <span data-ttu-id="5c8fb-150">[aktor/name](#actorname) -határozza meg a szereplő a szereplő név formátumban</span><span class="sxs-lookup"><span data-stu-id="5c8fb-150">[actor/name](#actorname) -   defines the actor in form of the actor's name</span></span>
* <span data-ttu-id="5c8fb-151">[aktor/objectid](#actorobjectid) -határozza meg a szereplő képernyőn a szereplő azonosító</span><span class="sxs-lookup"><span data-stu-id="5c8fb-151">[actor/objectid](#actorobjectid) - defines the actor in form of the actor's ID</span></span>   
* <span data-ttu-id="5c8fb-152">[aktor/upn](#actorupn) -határozza meg a szereplő a szereplő elv felhasználónév (UPN) formájában</span><span class="sxs-lookup"><span data-stu-id="5c8fb-152">[actor/upn](#actorupn)  - defines the actor in form of the actor's user principle name (UPN)</span></span> 
* <span data-ttu-id="5c8fb-153">[cél/neve](#targetname) -határozza meg a cél a szereplő név formátumban</span><span class="sxs-lookup"><span data-stu-id="5c8fb-153">[target/name](#targetname)  - defines the target in form of the actor's name</span></span>
* <span data-ttu-id="5c8fb-154">[cél/objectid](#targetobjectid) -határozza meg a cél a cél azonosítójának formában</span><span class="sxs-lookup"><span data-stu-id="5c8fb-154">[target/objectid](#targetobjectid) - defines the target in form of the target's ID</span></span>  
* <span data-ttu-id="5c8fb-155">[cél/upn](#targetupn) -határozza meg a szereplő a szereplő elv felhasználónév (UPN) formájában</span><span class="sxs-lookup"><span data-stu-id="5c8fb-155">[target/upn](#targetupn) - defines the actor in form of the actor's user principle name (UPN)</span></span>   

- - -
### <a name="activitydate"></a><span data-ttu-id="5c8fb-156">activityDate</span><span class="sxs-lookup"><span data-stu-id="5c8fb-156">activityDate</span></span>
<span data-ttu-id="5c8fb-157">**Támogatott operátorok**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="5c8fb-157">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="5c8fb-158">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-158">**Example**:</span></span>

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

<span data-ttu-id="5c8fb-159">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-159">**Notes**:</span></span>

<span data-ttu-id="5c8fb-160">dátum és idő (UTC) formátumban kell megadni</span><span class="sxs-lookup"><span data-stu-id="5c8fb-160">datetime should be in UTC format</span></span>

- - -
### <a name="category"></a><span data-ttu-id="5c8fb-161">category</span><span class="sxs-lookup"><span data-stu-id="5c8fb-161">category</span></span>

<span data-ttu-id="5c8fb-162">**Támogatott értékek**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-162">**Supported values**:</span></span>

| <span data-ttu-id="5c8fb-163">Kategória</span><span class="sxs-lookup"><span data-stu-id="5c8fb-163">Category</span></span>                         | <span data-ttu-id="5c8fb-164">Érték</span><span class="sxs-lookup"><span data-stu-id="5c8fb-164">Value</span></span>     |
| :--                              | ---       |
| <span data-ttu-id="5c8fb-165">Alapvető könyvtár</span><span class="sxs-lookup"><span data-stu-id="5c8fb-165">Core Directory</span></span>                   | <span data-ttu-id="5c8fb-166">Címtár</span><span class="sxs-lookup"><span data-stu-id="5c8fb-166">Directory</span></span> |
| <span data-ttu-id="5c8fb-167">Önkiszolgáló jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="5c8fb-167">Self-service Password Management</span></span> | <span data-ttu-id="5c8fb-168">SSPR</span><span class="sxs-lookup"><span data-stu-id="5c8fb-168">SSPR</span></span>      |
| <span data-ttu-id="5c8fb-169">Önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="5c8fb-169">Self-service Group Management</span></span>    | <span data-ttu-id="5c8fb-170">SSGM</span><span class="sxs-lookup"><span data-stu-id="5c8fb-170">SSGM</span></span>      |
| <span data-ttu-id="5c8fb-171">Fiók kiépítése</span><span class="sxs-lookup"><span data-stu-id="5c8fb-171">Account Provisioning</span></span>             | <span data-ttu-id="5c8fb-172">Sync</span><span class="sxs-lookup"><span data-stu-id="5c8fb-172">Sync</span></span>      |
| <span data-ttu-id="5c8fb-173">Automatizált jelszóváltás</span><span class="sxs-lookup"><span data-stu-id="5c8fb-173">Automated Password Rollover</span></span>      | <span data-ttu-id="5c8fb-174">Automatizált jelszóváltás</span><span class="sxs-lookup"><span data-stu-id="5c8fb-174">Automated Password Rollover</span></span> |
| <span data-ttu-id="5c8fb-175">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="5c8fb-175">Identity Protection</span></span>              | <span data-ttu-id="5c8fb-176">IdentityProtection</span><span class="sxs-lookup"><span data-stu-id="5c8fb-176">IdentityProtection</span></span> |
| <span data-ttu-id="5c8fb-177">Meghívott felhasználók</span><span class="sxs-lookup"><span data-stu-id="5c8fb-177">Invited Users</span></span>                    | <span data-ttu-id="5c8fb-178">Meghívott felhasználók</span><span class="sxs-lookup"><span data-stu-id="5c8fb-178">Invited Users</span></span> |
| <span data-ttu-id="5c8fb-179">MIM szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5c8fb-179">MIM Service</span></span>                      | <span data-ttu-id="5c8fb-180">MIM szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="5c8fb-180">MIM Service</span></span> |



<span data-ttu-id="5c8fb-181">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="5c8fb-181">**Supported operators**: eq</span></span>

<span data-ttu-id="5c8fb-182">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-182">**Example**:</span></span>

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a><span data-ttu-id="5c8fb-183">ActivityStatus</span><span class="sxs-lookup"><span data-stu-id="5c8fb-183">activityStatus</span></span>

<span data-ttu-id="5c8fb-184">**Támogatott értékek**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-184">**Supported values**:</span></span>

| <span data-ttu-id="5c8fb-185">A tevékenység állapota</span><span class="sxs-lookup"><span data-stu-id="5c8fb-185">Activity Status</span></span> | <span data-ttu-id="5c8fb-186">Érték</span><span class="sxs-lookup"><span data-stu-id="5c8fb-186">Value</span></span> |
| :--             | ---   |
| <span data-ttu-id="5c8fb-187">Sikeres</span><span class="sxs-lookup"><span data-stu-id="5c8fb-187">Success</span></span>         | <span data-ttu-id="5c8fb-188">0</span><span class="sxs-lookup"><span data-stu-id="5c8fb-188">0</span></span>     |
| <span data-ttu-id="5c8fb-189">Hiba</span><span class="sxs-lookup"><span data-stu-id="5c8fb-189">Failure</span></span>         | <span data-ttu-id="5c8fb-190">- 1</span><span class="sxs-lookup"><span data-stu-id="5c8fb-190">- 1</span></span>   |

<span data-ttu-id="5c8fb-191">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="5c8fb-191">**Supported operators**: eq</span></span>

<span data-ttu-id="5c8fb-192">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-192">**Example**:</span></span>

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a><span data-ttu-id="5c8fb-193">az ActivityType tulajdonság</span><span class="sxs-lookup"><span data-stu-id="5c8fb-193">activityType</span></span>
<span data-ttu-id="5c8fb-194">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="5c8fb-194">**Supported operators**: eq</span></span>

<span data-ttu-id="5c8fb-195">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-195">**Example**:</span></span>

    $filter=activityType eq 'User'    

<span data-ttu-id="5c8fb-196">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-196">**Notes**:</span></span>

<span data-ttu-id="5c8fb-197">kis-és nagybetűket</span><span class="sxs-lookup"><span data-stu-id="5c8fb-197">case-sensitive</span></span>

- - -
### <a name="activity"></a><span data-ttu-id="5c8fb-198">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="5c8fb-198">activity</span></span>
<span data-ttu-id="5c8fb-199">**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="5c8fb-199">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="5c8fb-200">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-200">**Example**:</span></span>

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

<span data-ttu-id="5c8fb-201">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-201">**Notes**:</span></span>

<span data-ttu-id="5c8fb-202">kis-és nagybetűket</span><span class="sxs-lookup"><span data-stu-id="5c8fb-202">case-sensitive</span></span>

- - -
### <a name="actorname"></a><span data-ttu-id="5c8fb-203">aktor/neve</span><span class="sxs-lookup"><span data-stu-id="5c8fb-203">actor/name</span></span>
<span data-ttu-id="5c8fb-204">**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="5c8fb-204">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="5c8fb-205">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-205">**Example**:</span></span>

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

<span data-ttu-id="5c8fb-206">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-206">**Notes**:</span></span>

<span data-ttu-id="5c8fb-207">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="5c8fb-207">case-insensitive</span></span>

- - -
### <a name="actorobjectid"></a><span data-ttu-id="5c8fb-208">aktor/objectId</span><span class="sxs-lookup"><span data-stu-id="5c8fb-208">actor/objectId</span></span>
<span data-ttu-id="5c8fb-209">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="5c8fb-209">**Supported operators**: eq</span></span>

<span data-ttu-id="5c8fb-210">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-210">**Example**:</span></span>

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a><span data-ttu-id="5c8fb-211">cél/neve</span><span class="sxs-lookup"><span data-stu-id="5c8fb-211">target/name</span></span>
<span data-ttu-id="5c8fb-212">**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="5c8fb-212">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="5c8fb-213">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-213">**Example**:</span></span>

    $filter=targets/any(t: t/name eq 'some name')    

<span data-ttu-id="5c8fb-214">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-214">**Notes**:</span></span>

<span data-ttu-id="5c8fb-215">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="5c8fb-215">Case-insensitive</span></span>

- - -
### <a name="targetupn"></a><span data-ttu-id="5c8fb-216">cél/egyszerű felhasználónév</span><span class="sxs-lookup"><span data-stu-id="5c8fb-216">target/upn</span></span>
<span data-ttu-id="5c8fb-217">**Támogatott operátorok**: eq, startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="5c8fb-217">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="5c8fb-218">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-218">**Example**:</span></span>

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

<span data-ttu-id="5c8fb-219">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-219">**Notes**:</span></span>

* <span data-ttu-id="5c8fb-220">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="5c8fb-220">Case-insensitive</span></span>
* <span data-ttu-id="5c8fb-221">Hozzá kell adnia a teljes névtér Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity lekérdezésekor</span><span class="sxs-lookup"><span data-stu-id="5c8fb-221">You need to add the full namespace when querying  Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span></span>

- - -
### <a name="targetobjectid"></a><span data-ttu-id="5c8fb-222">cél/objectId</span><span class="sxs-lookup"><span data-stu-id="5c8fb-222">target/objectId</span></span>
<span data-ttu-id="5c8fb-223">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="5c8fb-223">**Supported operators**: eq</span></span>

<span data-ttu-id="5c8fb-224">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-224">**Example**:</span></span>

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a><span data-ttu-id="5c8fb-225">aktor/egyszerű felhasználónév</span><span class="sxs-lookup"><span data-stu-id="5c8fb-225">actor/upn</span></span>
<span data-ttu-id="5c8fb-226">**Támogatott operátorok**: eq, startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="5c8fb-226">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="5c8fb-227">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-227">**Example**:</span></span>

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

<span data-ttu-id="5c8fb-228">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="5c8fb-228">**Notes**:</span></span>

* <span data-ttu-id="5c8fb-229">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="5c8fb-229">Case-insensitive</span></span> 
* <span data-ttu-id="5c8fb-230">Hozzá kell adnia a teljes névtér Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity lekérdezésekor</span><span class="sxs-lookup"><span data-stu-id="5c8fb-230">You need to add the full namespace when querying Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="5c8fb-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c8fb-231">Next Steps</span></span>
* <span data-ttu-id="5c8fb-232">Meg szeretné tekinteni a szűrt rendszertevékenységét példák?</span><span class="sxs-lookup"><span data-stu-id="5c8fb-232">Do you want to see examples for filtered system activities?</span></span> <span data-ttu-id="5c8fb-233">Tekintse meg a [Azure Active Directory naplózási API minták](active-directory-reporting-api-audit-samples.md).</span><span class="sxs-lookup"><span data-stu-id="5c8fb-233">Check out the [Azure Active Directory audit API samples](active-directory-reporting-api-audit-samples.md).</span></span>
* <span data-ttu-id="5c8fb-234">Meg szeretné ismerni az Azure AD reporting API-val kapcsolatos?</span><span class="sxs-lookup"><span data-stu-id="5c8fb-234">Do you want to know more about the Azure AD reporting API?</span></span> <span data-ttu-id="5c8fb-235">Lásd: [Bevezetés az Azure Active Directory Reporting API használatába](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="5c8fb-235">See [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>


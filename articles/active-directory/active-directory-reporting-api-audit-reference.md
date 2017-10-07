---
title: "aaaAzure Active Directory naplózási API-referencia |} Microsoft Docs"
description: "Hogyan tooget használatába hello Azure Active Directory naplózási API"
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
ms.openlocfilehash: 5f33b62ede9be445f35704739e328580dc454368
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-audit-api-reference"></a><span data-ttu-id="78722-103">Az Azure Active Directory naplózási API-referencia</span><span class="sxs-lookup"><span data-stu-id="78722-103">Azure Active Directory audit API reference</span></span>
<span data-ttu-id="78722-104">Ez a témakör hello Azure Active Directoryval kapcsolatos témakörök gyűjteményét része reporting API-val.</span><span class="sxs-lookup"><span data-stu-id="78722-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="78722-105">Az Azure AD jelentéskészítési lehetőséget biztosít, amely lehetővé teszi az API-k tooaccess naplózási adatok használata a kódban, illetve a kapcsolódó eszközök.</span><span class="sxs-lookup"><span data-stu-id="78722-105">Azure AD reporting provides you with an API that enables you tooaccess audit data using code or related tools.</span></span>
<span data-ttu-id="78722-106">hello ebben a témakörben hatóköre tooprovide kapcsolatos információk hello **API naplózási**.</span><span class="sxs-lookup"><span data-stu-id="78722-106">hello scope of this topic is tooprovide you with reference information about hello **audit API**.</span></span>

<span data-ttu-id="78722-107">Lásd:</span><span class="sxs-lookup"><span data-stu-id="78722-107">See:</span></span>

* <span data-ttu-id="78722-108">[A naplók](active-directory-reporting-azure-portal.md#activity-reports) további információ</span><span class="sxs-lookup"><span data-stu-id="78722-108">[Audit logs](active-directory-reporting-azure-portal.md#activity-reports)  for more conceptual information</span></span>

* <span data-ttu-id="78722-109">[Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) hello reporting API-val kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="78722-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


<span data-ttu-id="78722-110">Esetén:</span><span class="sxs-lookup"><span data-stu-id="78722-110">For:</span></span>

- <span data-ttu-id="78722-111">Gyakori kérdések, olvassa el a [– gyakori kérdések](active-directory-reporting-faq.md)</span><span class="sxs-lookup"><span data-stu-id="78722-111">Frequently asked questions, read our [FAQ](active-directory-reporting-faq.md)</span></span> 

- <span data-ttu-id="78722-112">Adjon ki [fájlt egy támogatási jegy](active-directory-troubleshooting-support-howto.md)</span><span class="sxs-lookup"><span data-stu-id="78722-112">Issues, please [file a support ticket](active-directory-troubleshooting-support-howto.md)</span></span> 


## <a name="who-can-access-hello-data"></a><span data-ttu-id="78722-113">Hello adatok hozzáférő felhasználók?</span><span class="sxs-lookup"><span data-stu-id="78722-113">Who can access hello data?</span></span>
* <span data-ttu-id="78722-114">Hello biztonsági rendszergazda vagy a biztonsági olvasó szerepet betöltő felhasználók</span><span class="sxs-lookup"><span data-stu-id="78722-114">Users in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="78722-115">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="78722-115">Global Admins</span></span>
* <span data-ttu-id="78722-116">Bármely alkalmazás, amely rendelkezik engedélyezési tooaccess hello API (alkalmazás engedélyezési is lehet a telepítő csak a globális rendszergazdai engedély alapján)</span><span class="sxs-lookup"><span data-stu-id="78722-116">Any app that has authorization tooaccess hello API (app authorization can be setup only based on Global Admin’s permission)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78722-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="78722-117">Prerequisites</span></span>
<span data-ttu-id="78722-118">Rendelés tooaccess ezt a jelentést a Reporting API hello, kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="78722-118">In order tooaccess this report through hello Reporting API, you must have:</span></span>

* <span data-ttu-id="78722-119">Egy [Azure Active Directory ingyenes vagy jobb edition](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="78722-119">An [Azure Active Directory Free or better edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="78722-120">Befejezett hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="78722-120">Completed hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-hello-api"></a><span data-ttu-id="78722-121">Hello API elérése</span><span class="sxs-lookup"><span data-stu-id="78722-121">Accessing hello API</span></span>
<span data-ttu-id="78722-122">Vagy érheti el az API hello [Graph Explorer](https://graphexplorer2.cloudapp.net) vagy programozott módon, például PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="78722-122">You can either access this API through hello [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="78722-123">Ahhoz, a PowerShell toocorrectly értelmezhetők hello AAD Graph REST-hívásokban használt OData-szintaxist kell használnia hello backtick (más néven: aposztróf) karakter túl "karaktert" hello $.</span><span class="sxs-lookup"><span data-stu-id="78722-123">In order for PowerShell toocorrectly interpret hello OData filter syntax used in AAD Graph REST calls, you must use hello backtick (aka: grave accent) character too“escape” hello $ character.</span></span> <span data-ttu-id="78722-124">hello backtick karakter funkcionál [PowerShell escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé téve a PowerShell toodo hello $ karaktert a literális értelmezésének, és elkerülheti a zavaró, a PowerShell változó neveként (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="78722-124">hello backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell toodo a literal interpretation of hello $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="78722-125">hello Ez a témakör elsősorban hello Graph Explorer.</span><span class="sxs-lookup"><span data-stu-id="78722-125">hello focus of this topic is on hello Graph Explorer.</span></span> <span data-ttu-id="78722-126">A PowerShell például megjelenik ez [PowerShell-parancsfájl](active-directory-reporting-api-audit-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="78722-126">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-audit-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="78722-127">API-végpont</span><span class="sxs-lookup"><span data-stu-id="78722-127">API Endpoint</span></span>
<span data-ttu-id="78722-128">Ez az API a következő URI hello segítségével érhető el:</span><span class="sxs-lookup"><span data-stu-id="78722-128">You can access this API using hello following URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

<span data-ttu-id="78722-129">Hello hello Azure AD naplózási API (OData tördelési használatával) által visszaadott rekordok száma korlátozva van.</span><span class="sxs-lookup"><span data-stu-id="78722-129">There is no limit on hello number of records returned by hello Azure AD audit API (using OData pagination).</span></span>
<span data-ttu-id="78722-130">A jelentés adatainak megőrzési-korlátok, tekintse meg [adatmegőrzési Reporting](active-directory-reporting-retention.md).</span><span class="sxs-lookup"><span data-stu-id="78722-130">For retention limits on reporting data, check out [Reporting Retention Policies](active-directory-reporting-retention.md).</span></span>

<span data-ttu-id="78722-131">Ez a hívás kötegekben hello adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="78722-131">This call returns hello data in batches.</span></span> <span data-ttu-id="78722-132">Minden egyes legfeljebb 1000 rekord rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="78722-132">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="78722-133">tooget hello következő köteget a rekordok, hello tovább kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="78722-133">tooget hello next batch of records, use hello Next link.</span></span> <span data-ttu-id="78722-134">Információk hello skiptoken visszaadott rekordok hello első készletből.</span><span class="sxs-lookup"><span data-stu-id="78722-134">Get hello skiptoken information from hello first set of returned records.</span></span> <span data-ttu-id="78722-135">hello kihagyási lexikális elem hello eredménykészlet hello végén lesz.</span><span class="sxs-lookup"><span data-stu-id="78722-135">hello skip token will be at hello end of hello result set.</span></span>  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a><span data-ttu-id="78722-136">Támogatott szűrők</span><span class="sxs-lookup"><span data-stu-id="78722-136">Supported filters</span></span>
<span data-ttu-id="78722-137">Azt jelzi, hogy az API-k által visszaadott hello számának megadásával szűkíthető hívható meg egy szűrő formában.</span><span class="sxs-lookup"><span data-stu-id="78722-137">You can narrow down hello number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="78722-138">Bejelentkezés az API-hoz kapcsolódó adatokat, a következő szűrők hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="78722-138">For sign-in API related data, hello following filters are supported:</span></span>

* <span data-ttu-id="78722-139">**$top =\<visszaadott rekordok toobe száma\>**  -toolimit hello visszaadott rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="78722-139">**$top=\<number of records toobe returned\>** - toolimit hello number of returned records.</span></span> <span data-ttu-id="78722-140">Ez az egy drága művelet.</span><span class="sxs-lookup"><span data-stu-id="78722-140">This is an expensive operation.</span></span> <span data-ttu-id="78722-141">Ha azt szeretné, hogy az objektumok több ezer tooreturn ne használjon a szűrőt.</span><span class="sxs-lookup"><span data-stu-id="78722-141">You should not use this filter if you want tooreturn thousands of objects.</span></span>     
* <span data-ttu-id="78722-142">**$filter =\<a szűrő utasítás\>**  -toospecify, az Ön számára legfontosabb bejegyzés hello típusát, a támogatott szűrő mezők hello alapján</span><span class="sxs-lookup"><span data-stu-id="78722-142">**$filter=\<your filter statement\>** - toospecify, on hello basis of supported filter fields, hello type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="78722-143">Támogatott Szűrő mezőket és operátorok</span><span class="sxs-lookup"><span data-stu-id="78722-143">Supported filter fields and operators</span></span>
<span data-ttu-id="78722-144">az Ön számára legfontosabb rekordok toospecify hello típusú hozhat létre egy szűrő utasítást, amely egy vagy a Szűrő mezőket a következő hello kombinációja szerepelhet:</span><span class="sxs-lookup"><span data-stu-id="78722-144">toospecify hello type of records you care about, you can build a filter statement that can contain either one or a combination of hello following filter fields:</span></span>

* <span data-ttu-id="78722-145">[activityDate](#activitydate) -dátum vagy dátumtartomány meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-145">[activityDate](#activitydate)  - defines a date or date range</span></span>
* <span data-ttu-id="78722-146">[kategória](#category) -határozza meg a kívánt toofilter hello kategóriát.</span><span class="sxs-lookup"><span data-stu-id="78722-146">[category](#category) - defines hello category you want toofilter on.</span></span>
* <span data-ttu-id="78722-147">[activityStatus](#activitystatus) -tevékenység hello állapotának meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-147">[activityStatus](#activitystatus) - defines hello status of an activity</span></span>
* <span data-ttu-id="78722-148">[az activityType](#activitytype) -tevékenység hello típusának definiálása</span><span class="sxs-lookup"><span data-stu-id="78722-148">[activityType](#activitytype)  - defines hello type of an activity</span></span>
* <span data-ttu-id="78722-149">[tevékenység](#activity) -karakterlánc hello tevékenység meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-149">[activity](#activity) - defines hello activity as string</span></span>  
* <span data-ttu-id="78722-150">[aktor/name](#actorname) -hello szereplő név formájában hello szereplő meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-150">[actor/name](#actorname) -   defines hello actor in form of hello actor's name</span></span>
* <span data-ttu-id="78722-151">[aktor/objectid](#actorobjectid) -hello szereplő képernyőn hello szereplő azonosító meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-151">[actor/objectid](#actorobjectid) - defines hello actor in form of hello actor's ID</span></span>   
* <span data-ttu-id="78722-152">[aktor/upn](#actorupn) -hello szereplő meghatározása hello szereplő elv felhasználónév (UPN) formájában</span><span class="sxs-lookup"><span data-stu-id="78722-152">[actor/upn](#actorupn)  - defines hello actor in form of hello actor's user principle name (UPN)</span></span> 
* <span data-ttu-id="78722-153">[cél/neve](#targetname) -hello szereplő név formájában hello cél meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-153">[target/name](#targetname)  - defines hello target in form of hello actor's name</span></span>
* <span data-ttu-id="78722-154">[cél/objectid](#targetobjectid) -hello cél azonosítójának formában hello cél meghatározása</span><span class="sxs-lookup"><span data-stu-id="78722-154">[target/objectid](#targetobjectid) - defines hello target in form of hello target's ID</span></span>  
* <span data-ttu-id="78722-155">[cél/upn](#targetupn) -hello szereplő meghatározása hello szereplő elv felhasználónév (UPN) formájában</span><span class="sxs-lookup"><span data-stu-id="78722-155">[target/upn](#targetupn) - defines hello actor in form of hello actor's user principle name (UPN)</span></span>   

- - -
### <a name="activitydate"></a><span data-ttu-id="78722-156">activityDate</span><span class="sxs-lookup"><span data-stu-id="78722-156">activityDate</span></span>
<span data-ttu-id="78722-157">**Támogatott operátorok**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="78722-157">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="78722-158">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-158">**Example**:</span></span>

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

<span data-ttu-id="78722-159">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-159">**Notes**:</span></span>

<span data-ttu-id="78722-160">dátum és idő (UTC) formátumban kell megadni</span><span class="sxs-lookup"><span data-stu-id="78722-160">datetime should be in UTC format</span></span>

- - -
### <a name="category"></a><span data-ttu-id="78722-161">category</span><span class="sxs-lookup"><span data-stu-id="78722-161">category</span></span>

<span data-ttu-id="78722-162">**Támogatott értékek**:</span><span class="sxs-lookup"><span data-stu-id="78722-162">**Supported values**:</span></span>

| <span data-ttu-id="78722-163">Kategória</span><span class="sxs-lookup"><span data-stu-id="78722-163">Category</span></span>                         | <span data-ttu-id="78722-164">Érték</span><span class="sxs-lookup"><span data-stu-id="78722-164">Value</span></span>     |
| :--                              | ---       |
| <span data-ttu-id="78722-165">Alapvető könyvtár</span><span class="sxs-lookup"><span data-stu-id="78722-165">Core Directory</span></span>                   | <span data-ttu-id="78722-166">Címtár</span><span class="sxs-lookup"><span data-stu-id="78722-166">Directory</span></span> |
| <span data-ttu-id="78722-167">Önkiszolgáló jelszókezelés</span><span class="sxs-lookup"><span data-stu-id="78722-167">Self-service Password Management</span></span> | <span data-ttu-id="78722-168">SSPR</span><span class="sxs-lookup"><span data-stu-id="78722-168">SSPR</span></span>      |
| <span data-ttu-id="78722-169">Önkiszolgáló csoportkezelés</span><span class="sxs-lookup"><span data-stu-id="78722-169">Self-service Group Management</span></span>    | <span data-ttu-id="78722-170">SSGM</span><span class="sxs-lookup"><span data-stu-id="78722-170">SSGM</span></span>      |
| <span data-ttu-id="78722-171">Fiók kiépítése</span><span class="sxs-lookup"><span data-stu-id="78722-171">Account Provisioning</span></span>             | <span data-ttu-id="78722-172">Sync</span><span class="sxs-lookup"><span data-stu-id="78722-172">Sync</span></span>      |
| <span data-ttu-id="78722-173">Automatizált jelszóváltás</span><span class="sxs-lookup"><span data-stu-id="78722-173">Automated Password Rollover</span></span>      | <span data-ttu-id="78722-174">Automatizált jelszóváltás</span><span class="sxs-lookup"><span data-stu-id="78722-174">Automated Password Rollover</span></span> |
| <span data-ttu-id="78722-175">Identity Protection</span><span class="sxs-lookup"><span data-stu-id="78722-175">Identity Protection</span></span>              | <span data-ttu-id="78722-176">IdentityProtection</span><span class="sxs-lookup"><span data-stu-id="78722-176">IdentityProtection</span></span> |
| <span data-ttu-id="78722-177">Meghívott felhasználók</span><span class="sxs-lookup"><span data-stu-id="78722-177">Invited Users</span></span>                    | <span data-ttu-id="78722-178">Meghívott felhasználók</span><span class="sxs-lookup"><span data-stu-id="78722-178">Invited Users</span></span> |
| <span data-ttu-id="78722-179">MIM szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="78722-179">MIM Service</span></span>                      | <span data-ttu-id="78722-180">MIM szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="78722-180">MIM Service</span></span> |



<span data-ttu-id="78722-181">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="78722-181">**Supported operators**: eq</span></span>

<span data-ttu-id="78722-182">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-182">**Example**:</span></span>

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a><span data-ttu-id="78722-183">ActivityStatus</span><span class="sxs-lookup"><span data-stu-id="78722-183">activityStatus</span></span>

<span data-ttu-id="78722-184">**Támogatott értékek**:</span><span class="sxs-lookup"><span data-stu-id="78722-184">**Supported values**:</span></span>

| <span data-ttu-id="78722-185">A tevékenység állapota</span><span class="sxs-lookup"><span data-stu-id="78722-185">Activity Status</span></span> | <span data-ttu-id="78722-186">Érték</span><span class="sxs-lookup"><span data-stu-id="78722-186">Value</span></span> |
| :--             | ---   |
| <span data-ttu-id="78722-187">Sikeres</span><span class="sxs-lookup"><span data-stu-id="78722-187">Success</span></span>         | <span data-ttu-id="78722-188">0</span><span class="sxs-lookup"><span data-stu-id="78722-188">0</span></span>     |
| <span data-ttu-id="78722-189">Hiba</span><span class="sxs-lookup"><span data-stu-id="78722-189">Failure</span></span>         | <span data-ttu-id="78722-190">- 1</span><span class="sxs-lookup"><span data-stu-id="78722-190">- 1</span></span>   |

<span data-ttu-id="78722-191">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="78722-191">**Supported operators**: eq</span></span>

<span data-ttu-id="78722-192">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-192">**Example**:</span></span>

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a><span data-ttu-id="78722-193">az ActivityType tulajdonság</span><span class="sxs-lookup"><span data-stu-id="78722-193">activityType</span></span>
<span data-ttu-id="78722-194">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="78722-194">**Supported operators**: eq</span></span>

<span data-ttu-id="78722-195">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-195">**Example**:</span></span>

    $filter=activityType eq 'User'    

<span data-ttu-id="78722-196">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-196">**Notes**:</span></span>

<span data-ttu-id="78722-197">kis-és nagybetűket</span><span class="sxs-lookup"><span data-stu-id="78722-197">case-sensitive</span></span>

- - -
### <a name="activity"></a><span data-ttu-id="78722-198">Tevékenység</span><span class="sxs-lookup"><span data-stu-id="78722-198">activity</span></span>
<span data-ttu-id="78722-199">**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="78722-199">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="78722-200">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-200">**Example**:</span></span>

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

<span data-ttu-id="78722-201">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-201">**Notes**:</span></span>

<span data-ttu-id="78722-202">kis-és nagybetűket</span><span class="sxs-lookup"><span data-stu-id="78722-202">case-sensitive</span></span>

- - -
### <a name="actorname"></a><span data-ttu-id="78722-203">aktor/neve</span><span class="sxs-lookup"><span data-stu-id="78722-203">actor/name</span></span>
<span data-ttu-id="78722-204">**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="78722-204">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="78722-205">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-205">**Example**:</span></span>

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

<span data-ttu-id="78722-206">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-206">**Notes**:</span></span>

<span data-ttu-id="78722-207">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="78722-207">case-insensitive</span></span>

- - -
### <a name="actorobjectid"></a><span data-ttu-id="78722-208">aktor/objectId</span><span class="sxs-lookup"><span data-stu-id="78722-208">actor/objectId</span></span>
<span data-ttu-id="78722-209">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="78722-209">**Supported operators**: eq</span></span>

<span data-ttu-id="78722-210">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-210">**Example**:</span></span>

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a><span data-ttu-id="78722-211">cél/neve</span><span class="sxs-lookup"><span data-stu-id="78722-211">target/name</span></span>
<span data-ttu-id="78722-212">**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="78722-212">**Supported operators**: eq, contains, startsWith</span></span>

<span data-ttu-id="78722-213">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-213">**Example**:</span></span>

    $filter=targets/any(t: t/name eq 'some name')    

<span data-ttu-id="78722-214">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-214">**Notes**:</span></span>

<span data-ttu-id="78722-215">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="78722-215">Case-insensitive</span></span>

- - -
### <a name="targetupn"></a><span data-ttu-id="78722-216">cél/egyszerű felhasználónév</span><span class="sxs-lookup"><span data-stu-id="78722-216">target/upn</span></span>
<span data-ttu-id="78722-217">**Támogatott operátorok**: eq, startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="78722-217">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="78722-218">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-218">**Example**:</span></span>

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

<span data-ttu-id="78722-219">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-219">**Notes**:</span></span>

* <span data-ttu-id="78722-220">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="78722-220">Case-insensitive</span></span>
* <span data-ttu-id="78722-221">Szüksége van a tooadd hello teljes névtér Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity lekérdezésekor</span><span class="sxs-lookup"><span data-stu-id="78722-221">You need tooadd hello full namespace when querying  Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity</span></span>

- - -
### <a name="targetobjectid"></a><span data-ttu-id="78722-222">cél/objectId</span><span class="sxs-lookup"><span data-stu-id="78722-222">target/objectId</span></span>
<span data-ttu-id="78722-223">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="78722-223">**Supported operators**: eq</span></span>

<span data-ttu-id="78722-224">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-224">**Example**:</span></span>

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a><span data-ttu-id="78722-225">aktor/egyszerű felhasználónév</span><span class="sxs-lookup"><span data-stu-id="78722-225">actor/upn</span></span>
<span data-ttu-id="78722-226">**Támogatott operátorok**: eq, startswith elemnek</span><span class="sxs-lookup"><span data-stu-id="78722-226">**Supported operators**: eq, startsWith</span></span>

<span data-ttu-id="78722-227">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="78722-227">**Example**:</span></span>

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

<span data-ttu-id="78722-228">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="78722-228">**Notes**:</span></span>

* <span data-ttu-id="78722-229">Nem betűérzékeny</span><span class="sxs-lookup"><span data-stu-id="78722-229">Case-insensitive</span></span> 
* <span data-ttu-id="78722-230">Szüksége van a tooadd hello teljes névtér Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity lekérdezésekor</span><span class="sxs-lookup"><span data-stu-id="78722-230">You need tooadd hello full namespace when querying Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="78722-231">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78722-231">Next Steps</span></span>
* <span data-ttu-id="78722-232">Szeretné toosee példák szűrt rendszer tevékenységek?</span><span class="sxs-lookup"><span data-stu-id="78722-232">Do you want toosee examples for filtered system activities?</span></span> <span data-ttu-id="78722-233">Tekintse meg a hello [Azure Active Directory naplózási API minták](active-directory-reporting-api-audit-samples.md).</span><span class="sxs-lookup"><span data-stu-id="78722-233">Check out hello [Azure Active Directory audit API samples](active-directory-reporting-api-audit-samples.md).</span></span>
* <span data-ttu-id="78722-234">További információk hello Azure AD jelentéskészítési API tooknow szeretne?</span><span class="sxs-lookup"><span data-stu-id="78722-234">Do you want tooknow more about hello Azure AD reporting API?</span></span> <span data-ttu-id="78722-235">Lásd: [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="78722-235">See [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>


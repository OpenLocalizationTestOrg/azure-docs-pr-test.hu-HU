---
title: "az Active Directory-bejelentkezés aaaAzure tevékenységgel kapcsolatos jelentés API-referencia |} Microsoft Docs"
description: "Azure Active Directory-bejelentkezés hello tevékenységgel kapcsolatos jelentés API referenciája"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ddcd9ae0-f6b7-4f13-a5e1-6cbf51a25634
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 8123f308a291503f2c61ac5de26696806c6402ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a><span data-ttu-id="00098-103">Az Azure Active Directory bejelentkezési tevékenység jelentés API-referencia</span><span class="sxs-lookup"><span data-stu-id="00098-103">Azure Active Directory sign-in activity report API reference</span></span>
<span data-ttu-id="00098-104">Ez a témakör hello Azure Active Directoryval kapcsolatos témakörök gyűjteményét része reporting API-val.</span><span class="sxs-lookup"><span data-stu-id="00098-104">This topic is part of a collection of topics about hello Azure Active Directory reporting API.</span></span>  
<span data-ttu-id="00098-105">Az Azure AD jelentéskészítési lehetőséget biztosít, amely lehetővé teszi az API-k tooaccess bejelentkezési tevékenység jelentésadatokat kód vagy a kapcsolódó eszközök használatával.</span><span class="sxs-lookup"><span data-stu-id="00098-105">Azure AD reporting provides you with an API that enables you tooaccess sign-in activity report data using code or related tools.</span></span>
<span data-ttu-id="00098-106">hello ebben a témakörben hatóköre tooprovide kapcsolatos információk hello **bejelentkezés tevékenység jelentés API**.</span><span class="sxs-lookup"><span data-stu-id="00098-106">hello scope of this topic is tooprovide you with reference information about hello **sign-in activity report API**.</span></span>

<span data-ttu-id="00098-107">Lásd:</span><span class="sxs-lookup"><span data-stu-id="00098-107">See:</span></span>

* <span data-ttu-id="00098-108">[Bejelentkezési tevékenységek](active-directory-reporting-azure-portal.md#activity-reports) további információ</span><span class="sxs-lookup"><span data-stu-id="00098-108">[Sign-in activities](active-directory-reporting-azure-portal.md#activity-reports) for more conceptual information</span></span>
* <span data-ttu-id="00098-109">[Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) hello reporting API-val kapcsolatos további információk.</span><span class="sxs-lookup"><span data-stu-id="00098-109">[Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md) for more information about hello reporting API.</span></span>


## <a name="who-can-access-hello-api-data"></a><span data-ttu-id="00098-110">Hello API adatok hozzáférő felhasználók?</span><span class="sxs-lookup"><span data-stu-id="00098-110">Who can access hello API data?</span></span>
* <span data-ttu-id="00098-111">Felhasználók és a biztonsági rendszergazda vagy a biztonsági olvasó szerepkört hello Szolgáltatásnevekről</span><span class="sxs-lookup"><span data-stu-id="00098-111">Users and Service Principals in hello Security Admin or Security Reader role</span></span>
* <span data-ttu-id="00098-112">A globális rendszergazdák</span><span class="sxs-lookup"><span data-stu-id="00098-112">Global Admins</span></span>
* <span data-ttu-id="00098-113">Bármely alkalmazás, amely rendelkezik engedélyezési tooaccess hello API (alkalmazás engedélyezési is lehet a telepítő csak a globális rendszergazdai engedély alapján)</span><span class="sxs-lookup"><span data-stu-id="00098-113">Any app that has authorization tooaccess hello API (app authorization can be setup only based on Global Admin’s permission)</span></span>

<span data-ttu-id="00098-114">egy alkalmazás tooaccess biztonsági API-k signin események, például PowerShell tooadd hello alkalmazások egyszerű hello olvasó biztonsági szerepkörhöz a következő használatát hello tooconfigure hozzáférés</span><span class="sxs-lookup"><span data-stu-id="00098-114">tooconfigure access for an application tooaccess security APIs such as signin events, use hello following PowerShell tooadd hello applications Service Principal into hello Security Reader role</span></span>

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a><span data-ttu-id="00098-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00098-115">Prerequisites</span></span>
<span data-ttu-id="00098-116">Ez a jelentés tooaccess hello jelentéskészítési API, kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="00098-116">tooaccess this report through hello reporting API, you must have:</span></span>

* <span data-ttu-id="00098-117">Egy [Azure Active Directory Premium P1 és P2 edition](active-directory-editions.md)</span><span class="sxs-lookup"><span data-stu-id="00098-117">An [Azure Active Directory Premium P1 or P2 edition](active-directory-editions.md)</span></span>
* <span data-ttu-id="00098-118">Befejezett hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md).</span><span class="sxs-lookup"><span data-stu-id="00098-118">Completed hello [prerequisites tooaccess hello Azure AD reporting API](active-directory-reporting-api-prerequisites.md).</span></span> 

## <a name="accessing-hello-api"></a><span data-ttu-id="00098-119">Hello API elérése</span><span class="sxs-lookup"><span data-stu-id="00098-119">Accessing hello API</span></span>
<span data-ttu-id="00098-120">Vagy érheti el az API hello [Graph Explorer](https://graphexplorer2.cloudapp.net) vagy programozott módon, például PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="00098-120">You can either access this API through hello [Graph Explorer](https://graphexplorer2.cloudapp.net) or programmatically using, for example, PowerShell.</span></span> <span data-ttu-id="00098-121">Ahhoz, a PowerShell toocorrectly értelmezhetők hello AAD Graph REST-hívásokban használt OData-szintaxist kell használnia hello backtick (más néven: aposztróf) karakter túl "karaktert" hello $.</span><span class="sxs-lookup"><span data-stu-id="00098-121">In order for PowerShell toocorrectly interpret hello OData filter syntax used in AAD Graph REST calls, you must use hello backtick (aka: grave accent) character too“escape” hello $ character.</span></span> <span data-ttu-id="00098-122">hello backtick karakter funkcionál [PowerShell escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé téve a PowerShell toodo hello $ karaktert a literális értelmezésének, és elkerülheti a zavaró, a PowerShell változó neveként (ie: $filter).</span><span class="sxs-lookup"><span data-stu-id="00098-122">hello backtick character serves as [PowerShell’s escape character](https://technet.microsoft.com/library/hh847755.aspx), allowing PowerShell toodo a literal interpretation of hello $ character, and avoid confusing it as a PowerShell variable name (ie: $filter).</span></span>

<span data-ttu-id="00098-123">hello Ez a témakör elsősorban hello Graph Explorer.</span><span class="sxs-lookup"><span data-stu-id="00098-123">hello focus of this topic is on hello Graph Explorer.</span></span> <span data-ttu-id="00098-124">A PowerShell például megjelenik ez [PowerShell-parancsfájl](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span><span class="sxs-lookup"><span data-stu-id="00098-124">For a PowerShell example, see this [PowerShell script](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).</span></span>

## <a name="api-endpoint"></a><span data-ttu-id="00098-125">API-végpont</span><span class="sxs-lookup"><span data-stu-id="00098-125">API Endpoint</span></span>
<span data-ttu-id="00098-126">Ez az API a következő alap URI-Azonosítójának hello segítségével érhető el:</span><span class="sxs-lookup"><span data-stu-id="00098-126">You can access this API using hello following base URI:</span></span>  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



<span data-ttu-id="00098-127">Lejáró toohello adatok mennyiségét az API rendelkezik maximális egymillió visszaadott rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="00098-127">Due toohello volume of data, this API has a limit of one million returned records.</span></span> 

<span data-ttu-id="00098-128">Ez a hívás kötegekben hello adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="00098-128">This call returns hello data in batches.</span></span> <span data-ttu-id="00098-129">Minden egyes legfeljebb 1000 rekord rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="00098-129">Each batch has a maximum of 1000 records.</span></span>  
<span data-ttu-id="00098-130">tooget hello következő köteget a rekordok, hello tovább kapcsolat használata.</span><span class="sxs-lookup"><span data-stu-id="00098-130">tooget hello next batch of records, use hello Next link.</span></span> <span data-ttu-id="00098-131">Első hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) hello visszaadott rekordok először készletének adatait.</span><span class="sxs-lookup"><span data-stu-id="00098-131">Get hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) information from hello first set of returned records.</span></span> <span data-ttu-id="00098-132">hello kihagyási lexikális elem hello eredménykészlet hello végén lesz.</span><span class="sxs-lookup"><span data-stu-id="00098-132">hello skip token will be at hello end of hello result set.</span></span>  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a><span data-ttu-id="00098-133">Támogatott szűrők</span><span class="sxs-lookup"><span data-stu-id="00098-133">Supported filters</span></span>
<span data-ttu-id="00098-134">Azt jelzi, hogy az API-k által visszaadott hello számának megadásával szűkíthető hívható meg egy szűrő formában.</span><span class="sxs-lookup"><span data-stu-id="00098-134">You can narrow down hello number of records that are returned by an API call in form of a filter.</span></span>  
<span data-ttu-id="00098-135">Bejelentkezés az API-hoz kapcsolódó adatokat, a következő szűrők hello támogatottak:</span><span class="sxs-lookup"><span data-stu-id="00098-135">For sign-in API related data, hello following filters are supported:</span></span>

* <span data-ttu-id="00098-136">**$top =\<visszaadott rekordok toobe száma\>**  -toolimit hello visszaadott rekordok száma.</span><span class="sxs-lookup"><span data-stu-id="00098-136">**$top=\<number of records toobe returned\>** - toolimit hello number of returned records.</span></span> <span data-ttu-id="00098-137">Ez az egy drága művelet.</span><span class="sxs-lookup"><span data-stu-id="00098-137">This is an expensive operation.</span></span> <span data-ttu-id="00098-138">Ha azt szeretné, hogy az objektumok több ezer tooreturn ne használjon a szűrőt.</span><span class="sxs-lookup"><span data-stu-id="00098-138">You should not use this filter if you want tooreturn thousands of objects.</span></span>  
* <span data-ttu-id="00098-139">**$filter =\<a szűrő utasítás\>**  -toospecify, az Ön számára legfontosabb bejegyzés hello típusát, a támogatott szűrő mezők hello alapján</span><span class="sxs-lookup"><span data-stu-id="00098-139">**$filter=\<your filter statement\>** - toospecify, on hello basis of supported filter fields, hello type of records you care about</span></span>

## <a name="supported-filter-fields-and-operators"></a><span data-ttu-id="00098-140">Támogatott Szűrő mezőket és operátorok</span><span class="sxs-lookup"><span data-stu-id="00098-140">Supported filter fields and operators</span></span>
<span data-ttu-id="00098-141">az Ön számára legfontosabb rekordok toospecify hello típusú hozhat létre egy szűrő utasítást, amely egy vagy a Szűrő mezőket a következő hello kombinációja szerepelhet:</span><span class="sxs-lookup"><span data-stu-id="00098-141">toospecify hello type of records you care about, you can build a filter statement that can contain either one or a combination of hello following filter fields:</span></span>

* <span data-ttu-id="00098-142">[signinDateTime](#signindatetime) -dátum vagy dátumtartomány meghatározása</span><span class="sxs-lookup"><span data-stu-id="00098-142">[signinDateTime](#signindatetime) - defines a date or date range</span></span>
* <span data-ttu-id="00098-143">[userId](#userid) -meghatározása egy adott felhasználó alapú hello felhasználói azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="00098-143">[userId](#userid) - defines a specific user based hello user's ID.</span></span>
* <span data-ttu-id="00098-144">[userPrincipalName](#userprincipalname) -meghatározása egy adott felhasználó alapú hello felhasználó egyszerű felhasználónév (UPN)</span><span class="sxs-lookup"><span data-stu-id="00098-144">[userPrincipalName](#userprincipalname) - defines a specific user based hello user's user principal name (UPN)</span></span>
* <span data-ttu-id="00098-145">[appId](#appid) -meghatározása egy adott alkalmazás alapú hello alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="00098-145">[appId](#appid) - defines a specific app based hello app's ID</span></span>
* <span data-ttu-id="00098-146">[appDisplayName](#appdisplayname) -egy adott alkalmazás alapú hello alkalmazás megjelenített név meghatározása</span><span class="sxs-lookup"><span data-stu-id="00098-146">[appDisplayName](#appdisplayname) - defines a specific app based hello app's display name</span></span>
* <span data-ttu-id="00098-147">[loginStatus](#loginStatus) -hello bejelentkezések hello állapotának meghatározása (sikeres vagy sikertelen)</span><span class="sxs-lookup"><span data-stu-id="00098-147">[loginStatus](#loginStatus) - defines hello status of hello logins (success / failure)</span></span>

> [!NOTE]
> <span data-ttu-id="00098-148">Graph Explorer használatakor Ön hello kell toouse hello helyes esetben minden levél a Szűrő mezőket.</span><span class="sxs-lookup"><span data-stu-id="00098-148">When using Graph Explorer, you hello need toouse hello correct case for each letter is your filter fields.</span></span>
> 
> 

<span data-ttu-id="00098-149">toonarrow le adatokat adott vissza hello hello hatókörét, hozhat létre hello támogatott szűrők és Szűrő mezők kombinációi.</span><span class="sxs-lookup"><span data-stu-id="00098-149">toonarrow down hello scope of hello returned data, you can build combinations of hello supported filters and filter fields.</span></span> <span data-ttu-id="00098-150">Például hello következő utasítást adja vissza hello felső 10 rekordot. július 1-jétől 2016 és a 6. 2016. július között:</span><span class="sxs-lookup"><span data-stu-id="00098-150">For example, hello following statement returns hello top 10 records between July 1st 2016 and July 6th 2016:</span></span>

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a><span data-ttu-id="00098-151">signinDateTime</span><span class="sxs-lookup"><span data-stu-id="00098-151">signinDateTime</span></span>
<span data-ttu-id="00098-152">**Támogatott operátorok**: eq, ge, le, gt, lt</span><span class="sxs-lookup"><span data-stu-id="00098-152">**Supported operators**: eq, ge, le, gt, lt</span></span>

<span data-ttu-id="00098-153">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="00098-153">**Example**:</span></span>

<span data-ttu-id="00098-154">Egy adott dátumot használatával</span><span class="sxs-lookup"><span data-stu-id="00098-154">Using a specific date</span></span>

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



<span data-ttu-id="00098-155">Dátumtartomány használatával</span><span class="sxs-lookup"><span data-stu-id="00098-155">Using a date range</span></span>    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


<span data-ttu-id="00098-156">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="00098-156">**Notes**:</span></span>

<span data-ttu-id="00098-157">hello datetime paraméter hello UTC formátumban kell megadni</span><span class="sxs-lookup"><span data-stu-id="00098-157">hello datetime parameter should be in hello UTC format</span></span> 

- - -
### <a name="userid"></a><span data-ttu-id="00098-158">Felhasználói azonosítóját</span><span class="sxs-lookup"><span data-stu-id="00098-158">userId</span></span>
<span data-ttu-id="00098-159">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="00098-159">**Supported operators**: eq</span></span>

<span data-ttu-id="00098-160">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="00098-160">**Example**:</span></span>

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

<span data-ttu-id="00098-161">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="00098-161">**Notes**:</span></span>

<span data-ttu-id="00098-162">hello userId értéke karakterlánc-értéke</span><span class="sxs-lookup"><span data-stu-id="00098-162">hello value of userId is a string value</span></span>

- - -
### <a name="userprincipalname"></a><span data-ttu-id="00098-163">UserPrincipalName</span><span class="sxs-lookup"><span data-stu-id="00098-163">userPrincipalName</span></span>
<span data-ttu-id="00098-164">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="00098-164">**Supported operators**: eq</span></span>

<span data-ttu-id="00098-165">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="00098-165">**Example**:</span></span>

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


<span data-ttu-id="00098-166">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="00098-166">**Notes**:</span></span>

<span data-ttu-id="00098-167">hello userPrincipalName értéke karakterlánc-értéke</span><span class="sxs-lookup"><span data-stu-id="00098-167">hello value of userPrincipalName is a string value</span></span>

- - -
### <a name="appid"></a><span data-ttu-id="00098-168">Alkalmazásazonosító</span><span class="sxs-lookup"><span data-stu-id="00098-168">appId</span></span>
<span data-ttu-id="00098-169">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="00098-169">**Supported operators**: eq</span></span>

<span data-ttu-id="00098-170">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="00098-170">**Example**:</span></span>

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



<span data-ttu-id="00098-171">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="00098-171">**Notes**:</span></span>

<span data-ttu-id="00098-172">hello appId értéke karakterlánc-értéke</span><span class="sxs-lookup"><span data-stu-id="00098-172">hello value of appId is a string value</span></span>

- - -
### <a name="appdisplayname"></a><span data-ttu-id="00098-173">appDisplayName</span><span class="sxs-lookup"><span data-stu-id="00098-173">appDisplayName</span></span>
<span data-ttu-id="00098-174">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="00098-174">**Supported operators**: eq</span></span>

<span data-ttu-id="00098-175">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="00098-175">**Example**:</span></span>

    $filter=appDisplayName+eq+'Azure+Portal' 


<span data-ttu-id="00098-176">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="00098-176">**Notes**:</span></span>

<span data-ttu-id="00098-177">hello appDisplayName értéke karakterlánc-értéke</span><span class="sxs-lookup"><span data-stu-id="00098-177">hello value of appDisplayName is a string value</span></span>

- - -
### <a name="loginstatus"></a><span data-ttu-id="00098-178">LoginStatus</span><span class="sxs-lookup"><span data-stu-id="00098-178">loginStatus</span></span>
<span data-ttu-id="00098-179">**Támogatott operátorok**: eq</span><span class="sxs-lookup"><span data-stu-id="00098-179">**Supported operators**: eq</span></span>

<span data-ttu-id="00098-180">**Példa**:</span><span class="sxs-lookup"><span data-stu-id="00098-180">**Example**:</span></span>

    $filter=loginStatus+eq+'1'  


<span data-ttu-id="00098-181">**Megjegyzések**:</span><span class="sxs-lookup"><span data-stu-id="00098-181">**Notes**:</span></span>

<span data-ttu-id="00098-182">Hello loginStatus esetén két lehetőség áll rendelkezésre: 0 – sikeres, 1 – hiba</span><span class="sxs-lookup"><span data-stu-id="00098-182">There are two options for hello loginStatus: 0 - success, 1 - failure</span></span>

- - -
## <a name="next-steps"></a><span data-ttu-id="00098-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00098-183">Next steps</span></span>
* <span data-ttu-id="00098-184">Szeretné toosee példák szűrt bejelentkezési tevékenységek?</span><span class="sxs-lookup"><span data-stu-id="00098-184">Do you want toosee examples for filtered sign-in activities?</span></span> <span data-ttu-id="00098-185">Tekintse meg a hello [Azure Active Directory bejelentkezési tevékenység jelentés API minták](active-directory-reporting-api-sign-in-activity-samples.md).</span><span class="sxs-lookup"><span data-stu-id="00098-185">Check out hello [Azure Active Directory sign-in activity report API samples](active-directory-reporting-api-sign-in-activity-samples.md).</span></span>
* <span data-ttu-id="00098-186">További információk hello Azure AD jelentéskészítési API tooknow szeretne?</span><span class="sxs-lookup"><span data-stu-id="00098-186">Do you want tooknow more about hello Azure AD reporting API?</span></span> <span data-ttu-id="00098-187">Lásd: [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="00098-187">See [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>


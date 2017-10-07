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
# <a name="azure-active-directory-audit-api-reference"></a>Az Azure Active Directory naplózási API-referencia
Ez a témakör hello Azure Active Directoryval kapcsolatos témakörök gyűjteményét része reporting API-val.  
Az Azure AD jelentéskészítési lehetőséget biztosít, amely lehetővé teszi az API-k tooaccess naplózási adatok használata a kódban, illetve a kapcsolódó eszközök.
hello ebben a témakörben hatóköre tooprovide kapcsolatos információk hello **API naplózási**.

Lásd:

* [A naplók](active-directory-reporting-azure-portal.md#activity-reports) további információ

* [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) hello reporting API-val kapcsolatos további információk.


Esetén:

- Gyakori kérdések, olvassa el a [– gyakori kérdések](active-directory-reporting-faq.md) 

- Adjon ki [fájlt egy támogatási jegy](active-directory-troubleshooting-support-howto.md) 


## <a name="who-can-access-hello-data"></a>Hello adatok hozzáférő felhasználók?
* Hello biztonsági rendszergazda vagy a biztonsági olvasó szerepet betöltő felhasználók
* A globális rendszergazdák
* Bármely alkalmazás, amely rendelkezik engedélyezési tooaccess hello API (alkalmazás engedélyezési is lehet a telepítő csak a globális rendszergazdai engedély alapján)

## <a name="prerequisites"></a>Előfeltételek
Rendelés tooaccess ezt a jelentést a Reporting API hello, kell rendelkeznie:

* Egy [Azure Active Directory ingyenes vagy jobb edition](active-directory-editions.md)
* Befejezett hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-hello-api"></a>Hello API elérése
Vagy érheti el az API hello [Graph Explorer](https://graphexplorer2.cloudapp.net) vagy programozott módon, például PowerShell használatával. Ahhoz, a PowerShell toocorrectly értelmezhetők hello AAD Graph REST-hívásokban használt OData-szintaxist kell használnia hello backtick (más néven: aposztróf) karakter túl "karaktert" hello $. hello backtick karakter funkcionál [PowerShell escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé téve a PowerShell toodo hello $ karaktert a literális értelmezésének, és elkerülheti a zavaró, a PowerShell változó neveként (ie: $filter).

hello Ez a témakör elsősorban hello Graph Explorer. A PowerShell például megjelenik ez [PowerShell-parancsfájl](active-directory-reporting-api-audit-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-végpont
Ez az API a következő URI hello segítségével érhető el:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

Hello hello Azure AD naplózási API (OData tördelési használatával) által visszaadott rekordok száma korlátozva van.
A jelentés adatainak megőrzési-korlátok, tekintse meg [adatmegőrzési Reporting](active-directory-reporting-retention.md).

Ez a hívás kötegekben hello adatait jeleníti meg. Minden egyes legfeljebb 1000 rekord rendelkezik.  
tooget hello következő köteget a rekordok, hello tovább kapcsolat használata. Információk hello skiptoken visszaadott rekordok hello első készletből. hello kihagyási lexikális elem hello eredménykészlet hello végén lesz.  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>Támogatott szűrők
Azt jelzi, hogy az API-k által visszaadott hello számának megadásával szűkíthető hívható meg egy szűrő formában.  
Bejelentkezés az API-hoz kapcsolódó adatokat, a következő szűrők hello támogatottak:

* **$top =\<visszaadott rekordok toobe száma\>**  -toolimit hello visszaadott rekordok száma. Ez az egy drága művelet. Ha azt szeretné, hogy az objektumok több ezer tooreturn ne használjon a szűrőt.     
* **$filter =\<a szűrő utasítás\>**  -toospecify, az Ön számára legfontosabb bejegyzés hello típusát, a támogatott szűrő mezők hello alapján

## <a name="supported-filter-fields-and-operators"></a>Támogatott Szűrő mezőket és operátorok
az Ön számára legfontosabb rekordok toospecify hello típusú hozhat létre egy szűrő utasítást, amely egy vagy a Szűrő mezőket a következő hello kombinációja szerepelhet:

* [activityDate](#activitydate) -dátum vagy dátumtartomány meghatározása
* [kategória](#category) -határozza meg a kívánt toofilter hello kategóriát.
* [activityStatus](#activitystatus) -tevékenység hello állapotának meghatározása
* [az activityType](#activitytype) -tevékenység hello típusának definiálása
* [tevékenység](#activity) -karakterlánc hello tevékenység meghatározása  
* [aktor/name](#actorname) -hello szereplő név formájában hello szereplő meghatározása
* [aktor/objectid](#actorobjectid) -hello szereplő képernyőn hello szereplő azonosító meghatározása   
* [aktor/upn](#actorupn) -hello szereplő meghatározása hello szereplő elv felhasználónév (UPN) formájában 
* [cél/neve](#targetname) -hello szereplő név formájában hello cél meghatározása
* [cél/objectid](#targetobjectid) -hello cél azonosítójának formában hello cél meghatározása  
* [cél/upn](#targetupn) -hello szereplő meghatározása hello szereplő elv felhasználónév (UPN) formájában   

- - -
### <a name="activitydate"></a>activityDate
**Támogatott operátorok**: eq, ge, le, gt, lt

**Példa**:

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=activityDate gt ' + $7daysago    

**Megjegyzések**:

dátum és idő (UTC) formátumban kell megadni

- - -
### <a name="category"></a>category

**Támogatott értékek**:

| Kategória                         | Érték     |
| :--                              | ---       |
| Alapvető könyvtár                   | Címtár |
| Önkiszolgáló jelszókezelés | SSPR      |
| Önkiszolgáló csoportkezelés    | SSGM      |
| Fiók kiépítése             | Sync      |
| Automatizált jelszóváltás      | Automatizált jelszóváltás |
| Identity Protection              | IdentityProtection |
| Meghívott felhasználók                    | Meghívott felhasználók |
| MIM szolgáltatás                      | MIM szolgáltatás |



**Támogatott operátorok**: eq

**Példa**:

    $filter=category eq 'SSPR'
- - -
### <a name="activitystatus"></a>ActivityStatus

**Támogatott értékek**:

| A tevékenység állapota | Érték |
| :--             | ---   |
| Sikeres         | 0     |
| Hiba         | - 1   |

**Támogatott operátorok**: eq

**Példa**:

    $filter=activityStatus eq -1    

---
### <a name="activitytype"></a>az ActivityType tulajdonság
**Támogatott operátorok**: eq

**Példa**:

    $filter=activityType eq 'User'    

**Megjegyzések**:

kis-és nagybetűket

- - -
### <a name="activity"></a>Tevékenység
**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek

**Példa**:

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')    

**Megjegyzések**:

kis-és nagybetűket

- - -
### <a name="actorname"></a>aktor/neve
**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek

**Példa**:

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')    

**Megjegyzések**:

Nem betűérzékeny

- - -
### <a name="actorobjectid"></a>aktor/objectId
**Támogatott operátorok**: eq

**Példa**:

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    


- - -
### <a name="targetname"></a>cél/neve
**Támogatott operátorok**: eq, tartalmaz, a startswith elemnek

**Példa**:

    $filter=targets/any(t: t/name eq 'some name')    

**Megjegyzések**:

Nem betűérzékeny

- - -
### <a name="targetupn"></a>cél/egyszerű felhasználónév
**Támogatott operátorok**: eq, startswith elemnek

**Példa**:

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc'))    

**Megjegyzések**:

* Nem betűérzékeny
* Szüksége van a tooadd hello teljes névtér Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity lekérdezésekor

- - -
### <a name="targetobjectid"></a>cél/objectId
**Támogatott operátorok**: eq

**Példa**:

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

- - -
### <a name="actorupn"></a>aktor/egyszerű felhasználónév
**Támogatott operátorok**: eq, startswith elemnek

**Példa**:

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')    

**Megjegyzések**:

* Nem betűérzékeny 
* Szüksége van a tooadd hello teljes névtér Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity lekérdezésekor

- - -
## <a name="next-steps"></a>Következő lépések
* Szeretné toosee példák szűrt rendszer tevékenységek? Tekintse meg a hello [Azure Active Directory naplózási API minták](active-directory-reporting-api-audit-samples.md).
* További információk hello Azure AD jelentéskészítési API tooknow szeretne? Lásd: [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).


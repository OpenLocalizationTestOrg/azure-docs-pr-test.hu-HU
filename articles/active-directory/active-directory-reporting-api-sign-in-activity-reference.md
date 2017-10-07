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
# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Az Azure Active Directory bejelentkezési tevékenység jelentés API-referencia
Ez a témakör hello Azure Active Directoryval kapcsolatos témakörök gyűjteményét része reporting API-val.  
Az Azure AD jelentéskészítési lehetőséget biztosít, amely lehetővé teszi az API-k tooaccess bejelentkezési tevékenység jelentésadatokat kód vagy a kapcsolódó eszközök használatával.
hello ebben a témakörben hatóköre tooprovide kapcsolatos információk hello **bejelentkezés tevékenység jelentés API**.

Lásd:

* [Bejelentkezési tevékenységek](active-directory-reporting-azure-portal.md#activity-reports) további információ
* [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md) hello reporting API-val kapcsolatos további információk.


## <a name="who-can-access-hello-api-data"></a>Hello API adatok hozzáférő felhasználók?
* Felhasználók és a biztonsági rendszergazda vagy a biztonsági olvasó szerepkört hello Szolgáltatásnevekről
* A globális rendszergazdák
* Bármely alkalmazás, amely rendelkezik engedélyezési tooaccess hello API (alkalmazás engedélyezési is lehet a telepítő csak a globális rendszergazdai engedély alapján)

egy alkalmazás tooaccess biztonsági API-k signin események, például PowerShell tooadd hello alkalmazások egyszerű hello olvasó biztonsági szerepkörhöz a következő használatát hello tooconfigure hozzáférés

```PowerShell
Connect-MsolService
$servicePrincipal = Get-MsolServicePrincipal -AppPrincipalId "<app client id>"
$role = Get-MsolRole | ? Name -eq "Security Reader"
Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $servicePrincipal.ObjectId
```

## <a name="prerequisites"></a>Előfeltételek
Ez a jelentés tooaccess hello jelentéskészítési API, kell rendelkeznie:

* Egy [Azure Active Directory Premium P1 és P2 edition](active-directory-editions.md)
* Befejezett hello [Előfeltételek tooaccess hello Azure AD jelentéskészítési API](active-directory-reporting-api-prerequisites.md). 

## <a name="accessing-hello-api"></a>Hello API elérése
Vagy érheti el az API hello [Graph Explorer](https://graphexplorer2.cloudapp.net) vagy programozott módon, például PowerShell használatával. Ahhoz, a PowerShell toocorrectly értelmezhetők hello AAD Graph REST-hívásokban használt OData-szintaxist kell használnia hello backtick (más néven: aposztróf) karakter túl "karaktert" hello $. hello backtick karakter funkcionál [PowerShell escape-karakter](https://technet.microsoft.com/library/hh847755.aspx), lehetővé téve a PowerShell toodo hello $ karaktert a literális értelmezésének, és elkerülheti a zavaró, a PowerShell változó neveként (ie: $filter).

hello Ez a témakör elsősorban hello Graph Explorer. A PowerShell például megjelenik ez [PowerShell-parancsfájl](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script).

## <a name="api-endpoint"></a>API-végpont
Ez az API a következő alap URI-Azonosítójának hello segítségével érhető el:  

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



Lejáró toohello adatok mennyiségét az API rendelkezik maximális egymillió visszaadott rekordok száma. 

Ez a hívás kötegekben hello adatait jeleníti meg. Minden egyes legfeljebb 1000 rekord rendelkezik.  
tooget hello következő köteget a rekordok, hello tovább kapcsolat használata. Első hello [skiptoken](https://msdn.microsoft.com/library/dd942121.aspx) hello visszaadott rekordok először készletének adatait. hello kihagyási lexikális elem hello eredménykészlet hello végén lesz.  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>Támogatott szűrők
Azt jelzi, hogy az API-k által visszaadott hello számának megadásával szűkíthető hívható meg egy szűrő formában.  
Bejelentkezés az API-hoz kapcsolódó adatokat, a következő szűrők hello támogatottak:

* **$top =\<visszaadott rekordok toobe száma\>**  -toolimit hello visszaadott rekordok száma. Ez az egy drága művelet. Ha azt szeretné, hogy az objektumok több ezer tooreturn ne használjon a szűrőt.  
* **$filter =\<a szűrő utasítás\>**  -toospecify, az Ön számára legfontosabb bejegyzés hello típusát, a támogatott szűrő mezők hello alapján

## <a name="supported-filter-fields-and-operators"></a>Támogatott Szűrő mezőket és operátorok
az Ön számára legfontosabb rekordok toospecify hello típusú hozhat létre egy szűrő utasítást, amely egy vagy a Szűrő mezőket a következő hello kombinációja szerepelhet:

* [signinDateTime](#signindatetime) -dátum vagy dátumtartomány meghatározása
* [userId](#userid) -meghatározása egy adott felhasználó alapú hello felhasználói azonosítóját.
* [userPrincipalName](#userprincipalname) -meghatározása egy adott felhasználó alapú hello felhasználó egyszerű felhasználónév (UPN)
* [appId](#appid) -meghatározása egy adott alkalmazás alapú hello alkalmazás azonosítója
* [appDisplayName](#appdisplayname) -egy adott alkalmazás alapú hello alkalmazás megjelenített név meghatározása
* [loginStatus](#loginStatus) -hello bejelentkezések hello állapotának meghatározása (sikeres vagy sikertelen)

> [!NOTE]
> Graph Explorer használatakor Ön hello kell toouse hello helyes esetben minden levél a Szűrő mezőket.
> 
> 

toonarrow le adatokat adott vissza hello hello hatókörét, hozhat létre hello támogatott szűrők és Szűrő mezők kombinációi. Például hello következő utasítást adja vissza hello felső 10 rekordot. július 1-jétől 2016 és a 6. 2016. július között:

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


- - -
### <a name="signindatetime"></a>signinDateTime
**Támogatott operátorok**: eq, ge, le, gt, lt

**Példa**:

Egy adott dátumot használatával

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z    



Dátumtartomány használatával    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**Megjegyzések**:

hello datetime paraméter hello UTC formátumban kell megadni 

- - -
### <a name="userid"></a>Felhasználói azonosítóját
**Támogatott operátorok**: eq

**Példa**:

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**Megjegyzések**:

hello userId értéke karakterlánc-értéke

- - -
### <a name="userprincipalname"></a>UserPrincipalName
**Támogatott operátorok**: eq

**Példa**:

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**Megjegyzések**:

hello userPrincipalName értéke karakterlánc-értéke

- - -
### <a name="appid"></a>Alkalmazásazonosító
**Támogatott operátorok**: eq

**Példa**:

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**Megjegyzések**:

hello appId értéke karakterlánc-értéke

- - -
### <a name="appdisplayname"></a>appDisplayName
**Támogatott operátorok**: eq

**Példa**:

    $filter=appDisplayName+eq+'Azure+Portal' 


**Megjegyzések**:

hello appDisplayName értéke karakterlánc-értéke

- - -
### <a name="loginstatus"></a>LoginStatus
**Támogatott operátorok**: eq

**Példa**:

    $filter=loginStatus+eq+'1'  


**Megjegyzések**:

Hello loginStatus esetén két lehetőség áll rendelkezésre: 0 – sikeres, 1 – hiba

- - -
## <a name="next-steps"></a>Következő lépések
* Szeretné toosee példák szűrt bejelentkezési tevékenységek? Tekintse meg a hello [Azure Active Directory bejelentkezési tevékenység jelentés API minták](active-directory-reporting-api-sign-in-activity-samples.md).
* További információk hello Azure AD jelentéskészítési API tooknow szeretne? Lásd: [Ismerkedés az Azure Active Directory Reporting API hello](active-directory-reporting-api-getting-started.md).


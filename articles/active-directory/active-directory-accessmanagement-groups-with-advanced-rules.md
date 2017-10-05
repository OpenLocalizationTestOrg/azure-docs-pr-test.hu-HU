---
title: "Az Azure Active Directoryban címtárobjektum-attribútumok alapján dinamikusan csoportok feltöltéséhez |} Microsoft Docs"
description: "Hogyan-a következőre csoport tagsági beleértve a támogatott szabályoperátorokat kifejezés és paraméterek speciális szabályok létrehozásához."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 04813a42-d40a-48d6-ae96-15b7e5025884
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: b9b5ddf42958a2b4e241d0252101d979009e7dc0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="populate-groups-dynamically-based-on-object-attributes"></a>Csoportok címtárobjektum-attribútumok alapján dinamikusan feltöltése
A klasszikus Azure portálon összetettebb Attribútumalapú dinamikus csoporttagságok az Azure Active Directory (Azure AD) csoport engedélyezése lehetőséget nyújt.  

Ha módosítja egy felhasználó vagy eszköz attribútuma, a rendszer kiértékeli az összes dinamikus csoport szabályokat annak ellenőrzéséhez, hogy a módosítás kiváltották bármely csoport hozzáadása vagy eltávolítása a könyvtárban található. Ha egy felhasználó vagy az eszköz megfelel a csoporton szabály, azokat hozzá szeretné adni a csoport tagjai. A szabály már nem megfelelnek eltávolítja.

> [!NOTE]
> - Biztonsági vagy Office 365-csoportok esetében dinamikustagság-szabály beállítására is lehetőség van.
>
> - Ez a szolgáltatás legalább egy dinamikus csoportba felvett felhasználói tagjaihoz tartozó Azure AD Premium P1-licencre van szükség.
>
> - Létrehozhat egy dinamikus csoportot eszközök vagy felhasználók számára, de nem hozható létre egy szabályt, amely a felhasználó és eszköz objektumokat is tartalmaz.

> - Jelenleg nincs lehetőség a felhasználói attribútumok a tulajdonos alapján eszköz csoport létrehozásához. Eszköz tagsági szabályok csak az eszköz a címtárban található objektumokhoz azonnali attribútumok hivatkozhat.

## <a name="to-create-an-advanced-rule"></a>Speciális szabály létrehozása
1. Az a [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.
2. Válassza ki a **csoportok** lapra, és nyissa meg a szerkeszteni kívánt csoportot.
3. Válassza ki a **konfigurálása** lapon jelölje be a **speciális szabály** lehetőséget, majd a speciális szabályt a szövegmezőbe írja be.

## <a name="constructing-the-body-of-an-advanced-rule"></a>A szervezet egy speciális szabály létrehozása
A speciális szabályt, a dinamikus tagságot csoportokat hozhat létre az lényegében egy bináris kifejezés, amely három részből áll, és IGAZ vagy hamis eredménye eredményez. A három részt a következők:

* Bal oldali paraméter
* Bináris operátor
* Jobb oldali állandó

A teljes speciális szabály néz: (leftParameter binaryOperator "RightConstant"), ahol a nyitó és záró zárójel szükségesek a teljes bináris kifejezésből, dupla idézőjelek között szükség a megfelelő állandó és a bal oldali paraméter szintaxisa a következő user.property. Speciális szabály állhat egynél több bináris kifejezések vesszővel elválasztva a - és, - vagy, és - nem logikai operátor.
A következő példák a megfelelően felépített speciális szabályt:

* (felhasználó.részleg - eq "Értékesítési") – vagy (felhasználó.részleg - eq "Marketing")
* (felhasználó.részleg - eq "Értékesítési")- és - nem (user.jobTitle-tartalmazza a "SDE")

Támogatott paraméterek és kifejezés szabályoperátorokat teljes listájáért tekintse meg az alábbi szakaszok.


Vegye figyelembe, hogy a tulajdonság a megfelelő objektumtípus lehet előtagként: felhasználó vagy eszköz.
Az alábbi szabály sikertelen lesz, az érvényesítés: az e-mail – ne null

A megfelelő szabályt a következő lesz:

User.mail – ne NULL értékű

A speciális szabály törzsét teljes hossza legfeljebb 2048 karakter hosszú lehet.

> [!NOTE]
> Karakterlánc és regex műveletek esetében-és nagybetűket.
> Idézőjeleket tartalmazó karakterláncok "használatával kell megjelölni" karakter, például felhasználó.részleg - eq \`"Értékesítési".
> Csak akkor használjon idézőjelet karakterlánc típusú értékeket, és csak az angol nyelvű idézőjelek használata.
>
>

## <a name="supported-expression-rule-operators"></a>Támogatott kifejezés szabály operátorok
Az alábbi táblázat a támogatott kifejezés szabályoperátorokat és szintaxisát a speciális szabály törzsét használhatók:

| Operátor | Szintaxis |
| --- | --- |
| Nem egyenlő |-ne |
| egyenlő |-eq |
| Nem kezdődik |-notStartsWith |
| Kezdődik |-startswith elemnek |
| Nem tartalmazza |-notContains |
| Contains |-tartalmaz |
| Nem egyeznek |-notMatch |
| Egyezés |-felel meg |
| A | -a |
| Nem található | -notIn |

## <a name="operator-precedence"></a>Precedencia

Minden operátorok a következők / élveznek a csökkenteni a magasabb, operátor szerepel ugyanabban a sorban szerepelnek egyenlő élvez-bármely - összes - vagy - és - nem - eq - ne - megadott módon kezdődő - notStartsWith-- notContains tartalmazza-– notMatch egyezik-- notIn a

Minden műveleteivel végrehajtható vagy kötőjel előtag nélkül.

Vegye figyelembe, hogy nem mindig szükséges zárójeleket tartalmazhat, csak kell hozzáadnia a zárójelek közé foglalva, amikor sorrendje nem felel meg a követelmények például:

   felhasználó.részleg-eq "Marketing" – és felhasználó.ország-eq "US"

az egyenértékű:

   (felhasználó.részleg-eq "Marketing") – és (felhasználó.ország-eq "US")

## <a name="using-the--in-and--notin-operators"></a>Használja az - a és - notIn operátorok

Ha számos különböző értéket elleni felhasználói attribútum értékének összehasonlítandó használhatja az - a vagy - notIn operátorokat. Példa használja a - operátorban:

    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]

Vegye figyelembe a használatát a "[" és "]" elején és értékek listájának végét. Ez a feltétel eredménye IGAZ érték felhasználó.részleg egyenlő a listában szereplő értékek közül.

## <a name="query-error-remediation"></a>Lekérdezési hiba szervizelés
A következő táblázat felsorolja a lehetséges hibákat, és hogyan javítsa ki azokat, ha az előfordulásuk időpontjában naplózhatja őket

| Lekérdezés-elemzési hiba | Hiba kihasználtsága | Javított kihasználtsága |
| --- | --- | --- |
| Hiba történt: Az attribútum nem támogatott. |(user.invalidProperty - eq "Érték") |(felhasználó.részleg - eq "érték")<br/>Tulajdonságot meg kell felelnie közül a [tulajdonságainak listája támogatott](#supported-properties). |
| Hiba: Operátor nem támogatott az attribútum. |(user.accountEnabled-igaz tartalmazza) |(user.accountEnabled - eq igaz)<br/>Tulajdonság logikai érték típusú. A támogatott operátorokkal (-eq vagy - ne) logikai típus a fenti listából. |
| Hiba: Lekérdezésfordítási hiba. |(felhasználó.részleg - eq "Értékesítési")- és (felhasználó.részleg - eq "Marketing") (user.userPrincipalName-match "*@domain.ext") |(felhasználó.részleg - eq "Értékesítési")- és (felhasználó.részleg - eq "Marketing")<br/>Logikai operátor meg kell felelnie egy, a támogatott tulajdonságok a fenti listából. (user.userPrincipalName-egyeznie ". *@domain.ext") vagy (user.userPrincipalName-egyeznie "@domain.ext$") hiba történt a reguláris kifejezés. |
| Hiba: Bináris kifejezés nem megfelelő formátumban van. |(felhasználó.részleg-eq "Értékesítési") (felhasználó.részleg - eq "Értékesítési") (felhasználó.részleg-eq "Értékesítési") |(user.accountEnabled - eq igaz) – és (user.userPrincipalName-tartalmaz "alias@domain")<br/>A lekérdezés több hibát tartalmaz. Nem megfelelő helyen zárójel. |
| Hiba: Ismeretlen hiba történt a dinamikus csoporttagságok beállítása során. |(user.accountEnabled - eq "True" és user.userPrincipalName-tartalmaz "alias@domain") |(user.accountEnabled - eq igaz) – és (user.userPrincipalName-tartalmaz "alias@domain")<br/>A lekérdezés több hibát tartalmaz. Nem megfelelő helyen zárójel. |

## <a name="supported-properties"></a>Támogatott tulajdonságok
A speciális szabály használható felhasználói tulajdonságok a következők:

### <a name="properties-of-type-boolean"></a>Logikai érték típusú tulajdonságairól
Engedélyezett operátorok

* -eq
* -ne

| Tulajdonságok | Megengedett értékek | Használat |
| --- | --- | --- |
| AccountEnabled |IGAZ, hamis |user.accountEnabled - eq igaz |
| DirSyncEnabled |IGAZ, hamis |user.dirSyncEnabled - eq igaz |

### <a name="properties-of-type-string"></a>Karakterlánc típusú tulajdonságok
Engedélyezett operátorok

* -eq
* -ne
* -notStartsWith
* -Startswith elemnek
* -tartalmaz
* -notContains
* -felel meg
* -notMatch
* -a
* -notIn

| Tulajdonságok | Megengedett értékek | Használat |
| --- | --- | --- |
| city |Bármilyen karakterlánc vagy $null |(user.city - eq "érték") |
| Ország |Bármilyen karakterlánc vagy $null |(felhasználó.ország - eq "érték") |
| Cégnév | Bármilyen karakterlánc vagy $null | (user.companyName - eq "érték") |
| Szervezeti egység |Bármilyen karakterlánc vagy $null |(felhasználó.részleg - eq "érték") |
| displayName |Bármilyen karakterlánc típusú értéket |(user.displayName - eq "érték") |
| facsimileTelephoneNumber |Bármilyen karakterlánc vagy $null |(user.facsimileTelephoneNumber - eq "érték") |
| givenName |Bármilyen karakterlánc vagy $null |(user.givenName - eq "érték") |
| Beosztás |Bármilyen karakterlánc vagy $null |(user.jobTitle - eq "érték") |
| mail |Bármilyen karakterlánc vagy $null (SMTP-cím felhasználó) |(user.mail - eq "érték") |
| mailNickName |Bármilyen karakterlánc típusú értéket (mail alias a felhasználó) |(user.mailNickName - eq "érték") |
| Mobileszköz |Bármilyen karakterlánc vagy $null |(user.mobile - eq "érték") |
| Objektumazonosító |A user objektum GUID-azonosítója |(user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | A helyi biztonsági azonosítóját (SID) a felhasználók számára a felhőbe a helyszíni szinkronizálva. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
| passwordPolicies |Nincs DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |(user.passwordPolicies - eq "DisableStrongPassword") |
| physicalDeliveryOfficeName |Bármilyen karakterlánc vagy $null |(user.physicalDeliveryOfficeName - eq "érték") |
| Irányítószám |Bármilyen karakterlánc vagy $null |(user.postalCode - eq "érték") |
| preferredLanguage |ISO 639-1 kódot |(user.preferredLanguage - eq "en-US") |
| sipProxyAddress |Bármilyen karakterlánc vagy $null |(user.sipProxyAddress - eq "érték") |
| state |Bármilyen karakterlánc vagy $null |(user.state - eq "érték") |
| StreetAddress |Bármilyen karakterlánc vagy $null |(user.streetAddress - eq "érték") |
| Vezetéknév |Bármilyen karakterlánc vagy $null |(user.surname - eq "érték") |
| TelephoneNumber |Bármilyen karakterlánc vagy $null |(user.telephoneNumber - eq "érték") |
| usageLocation |Két betűkkel országhívószám |(user.usageLocation - eq "US") |
| UserPrincipalName |Bármilyen karakterlánc típusú értéket |(user.userPrincipalName - eq "alias@domain") |
| UserType |tag Vendég $null |(user.userType - eq "Tag") |

### <a name="properties-of-type-string-collection"></a>Típusú karakterlánc gyűjtemény tulajdonságai
Engedélyezett operátorok

* -tartalmaz
* -notContains

| Tulajdonságok | Megengedett értékek | Használat |
| --- | --- | --- |
| otherMails |Bármilyen karakterlánc típusú értéket |(user.otherMails-tartalmaz "alias@domain") |
| proxyAddresses |SMTP: alias@domain smtp:alias@domain |(user.proxyAddresses-tartalmaz "SMTP: alias@domain") |

## <a name="multi-value-properties"></a>Többértékű tulajdonságai
Engedélyezett operátorok

* -a (kielégíteni, ha legalább a gyűjtemény több eleme egyezik a következő feltételt:)
* -az összes (teljesülnek, ha a gyűjtemény összes elemének felel meg a feltétel)

| Tulajdonságok | Értékek | Használat |
| --- | --- | --- |
| assignedPlans |A gyűjtemény minden vezérlőnek a következő karakterlánc tulajdonságai: capabilityStatus, szolgáltatás, servicePlanId |user.assignedPlans-bármely (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- és assignedPlan.capabilityStatus - eq "Engedélyezett") |

A rendszer ugyanolyan típusú objektumok gyűjteményeit adják többértékű tulajdonságokat. Használhat - bármely és - minden üzemeltetői számára, illetve egy vagy az összes elem a gyűjteményben, alkalmaz rá egy feltételt. Példa:

assignedPlans egy többértékű tulajdonság, amely tartalmazza a felhasználóhoz rendelt összes service-csomagokról. Az alábbi kifejezés fogja kiválasztani az Exchange Online (terv 2) service-csomag, amely egyúttal az engedélyezési állapotot rendelkező felhasználók:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(A Guid azonosító azonosítja az Exchange Online (terv 2) service-csomag).

> [!NOTE]
> Ez akkor hasznos, ha azt szeretné, hogy minden felhasználó azonosításához, amely az Office 365 (vagy más Microsoft Online Services szolgáltatással) funkció engedélyezve van, például, amelyekre őket egy bizonyos házirendcsoport.

Az alábbi kifejezés fogja kiválasztani az összes, akik rendelkeznek minden service-csomag, amely az Intune szolgáltatással (szolgáltatásnév "SCO" által azonosított) van társítva:
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Null értékek használatát

Egy szabály null értéket ad meg, "null" vagy a $null is használhat. Példa:

   User.mail – ne null értéke megegyezik user.mail – ne $null

## <a name="extension-attributes-and-custom-attributes"></a>Egyéni oszlopainál és a bővítmény
A bővítményattribútumokat és egyéni attribútumok dinamikus tagsági szabályok támogatottak.

Kiterjesztési attribútumot a helyszíni Windows Server AD szinkronizált, és tegye meg "ExtensionAttributeX", ahol X értéke 1 – 15 formátuma.
Egy bővítmény attribútumot használó szabály például lenne.

(user.extensionAttribute15 - eq "Marketing")

Az egyéni attribútumok szinkronizált a helyi Windows Server AD-ről vagy csatlakoztatott SaaS-alkalmazás és a formátuma "user.extension_[GUID]\__ [attribútum]" ahol [GUID] az az attribútum az aad-ben létrehozott alkalmazás az aad-ben az egyedi azonosítója, amely [attribútum] esetén az attribútum neve, mert azt létrehozták.
Példa egy szabályt, amely egy egyéni attribútumot használ:

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Az egyéni attribútum nevében található a könyvtárban a felhasználó lekérdezésével attribútum Graph Explorerrel és keres az attribútum nevét.

## <a name="direct-reports-rule"></a>"Közvetlen beosztottai" szabály
Létrehozhat egy Manager minden közvetlen beosztottai tartalmazó csoport. A kezelő közvetlen beosztottai később módosíthatja, ha a csoport tagsága automatikusan igazítani.

> [!NOTE]
> 1. A szabály érvényesítéséhez, ellenőrizze, hogy a **kezelő azonosítója** tulajdonság helyesen beállítva a felhasználók az Ön bérelt szolgáltatásának. Akkor is ellenőrizhesse a felhasználó aktuális értékét az **profil lapon**.
> 2. Ez a szabály csak **közvetlen** jelentéseket. Jelenleg nem lehetséges egy beágyazott hierarchiához, pl. tartalmazó csoport közvetlen jelentések és a csoport létrehozásához.

**A csoport konfigurálása**

1. Kövesse a 1-5 lépések szakaszából [a speciális szabály létrehozásához](#to-create-the-advanced-rule), és válassza ki egy **tagsági típusa** a **dinamikus felhasználói**.
2. Az a **dinamikus tagsági szabályok** panelen adja meg a szabály a következő szintaxissal:

    *A(z) "{obectID_of_manager}" közvetlen beosztottaik*

    Példa egy érvényes szabályt:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is the objectID of the manager. The object ID can be found on manager's **Profile tab**.
3. A szabály mentése után a megadott kezelő azonosítója értékkel rendelkező összes felhasználó bekerül a csoporthoz.

## <a name="using-attributes-to-create-rules-for-device-objects"></a>Attribútumok használata eszközobjektumok szabályok létrehozása
Olyan szabály, amely kijelöli a tagság eszközobjektumok egy csoportot is létrehozhat. A következő tulajdonságok használhatók:

| Tulajdonságok              | Megengedett értékek                  | Használat                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| AccountEnabled          | IGAZ, hamis                      | (device.accountEnabled - eq igaz)                            |
| displayName             | Bármilyen karakterlánc típusú értéket                | (device.displayName - eq "Rob Iphone")                       |
| DeviceOSType            | Bármilyen karakterlánc típusú értéket                | (device.deviceOSType - eq "IOS")                             |
| DeviceOSVersion         | Bármilyen karakterlánc típusú értéket                | (eszköz. OSVersion - eq "9.1")                                |
| deviceCategory          | Bármilyen karakterlánc típusú értéket                | (device.deviceCategory - eq "")                              |
| DeviceManufacturer      | Bármilyen karakterlánc típusú értéket                | (device.deviceManufacturer - eq "Microsoft")                 |
| DeviceModel             | Bármilyen karakterlánc típusú értéket                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | Bármilyen karakterlánc típusú értéket                | (device.deviceOwnership - eq "")                             |
| Tartománynév              | Bármilyen karakterlánc típusú értéket                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | Bármilyen karakterlánc típusú értéket                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | IGAZ, hamis                      | (device.deviceOSType - eq igaz)                              |
| managementType          | Bármilyen karakterlánc típusú értéket                | (device.managementType - eq "")                              |
| OrganizationalUnit      | Bármilyen karakterlánc típusú értéket                | (device.organizationalUnit - eq "")                          |
| deviceId                | egy érvényes deviceId                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d") |
| Objektumazonosító                | egy érvényes AAD objectId            | (device.objectId - eq "76ad43c9-32c5-45e8-a272-7b58b58f596d") |

> [!NOTE]
> Ezeket az eszköz szabályokat nem hozható létre, hogy a "egyszerű szabály" legördülő lista használatakor a klasszikus Azure portálon.
>
>

## <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [A csoportok dinamikus csoporttagságok hibaelhárítása](active-directory-accessmanagement-troubleshooting.md)
* [Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

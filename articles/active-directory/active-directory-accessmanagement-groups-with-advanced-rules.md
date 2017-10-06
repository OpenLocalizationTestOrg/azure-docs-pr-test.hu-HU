---
title: "aaaPopulate csoportokat dinamikusan az Azure Active Directoryban címtárobjektum-attribútumok alapján |} Microsoft Docs"
description: "Hogyan-tartozó toocreate speciális szabályok kifejezés szabályoperátorokat és paraméterek csoport tagsági többek között támogatott."
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
ms.openlocfilehash: fe22829118ed8f5137a619d93fa6f9bf80835863
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="populate-groups-dynamically-based-on-object-attributes"></a>Csoportok címtárobjektum-attribútumok alapján dinamikusan feltöltése
a klasszikus Azure portálon hello biztosít hello képességét tooenable összetettebb Attribútumalapú dinamikus csoporttagságok az Azure Active Directory (Azure AD) csoport.  

Ha bármely felhasználó vagy eszköz változás attribútumok, hello rendszer kiértékeli az összes dinamikus csoport szabályokat egy directory toosee a hello módosítás kiváltották, ha bármely csoport hozzáadása vagy eltávolítása. Ha egy felhasználó vagy az eszköz megfelel a csoporton szabály, azokat hozzá szeretné adni a csoport tagjai. Hello szabály már nem megfelelnek eltávolítja.

> [!NOTE]
> - Biztonsági vagy Office 365-csoportok esetében dinamikustagság-szabály beállítására is lehetőség van.
>
> - Ez a szolgáltatás minden egyes felhasználó tag hozzáadott tooat legalább egy dinamikus csoport Azure AD Premium P1-licencre van szükség.
>
> - Létrehozhat egy dinamikus csoportot eszközök vagy felhasználók számára, de nem hozható létre egy szabályt, amely a felhasználó és eszköz objektumokat is tartalmaz.

> - Hello jelenleg nincs a felhasználói attribútumok a tulajdonos alapján eszközcsoport lehetséges toocreate. Eszköz tagsági szabályok csak referencia azonnali attribútumok hello directory eszköz objektumokat is.

## <a name="toocreate-an-advanced-rule"></a>toocreate speciális szabály
1. A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd nyissa meg a munkahely címtárában.
2. Jelölje be hello **csoportok** fülre, és azt szeretné, hogy tooedit majd nyitott hello csoport.
3. Jelölje be hello **konfigurálása** lapra, jelölje be hello **speciális szabály** lehetőséget, és hello speciális szabály hello szövegmezőbe írja be.

## <a name="constructing-hello-body-of-an-advanced-rule"></a>Hello törzsét speciális szabály létrehozása
hello speciális szabályt is létrehozhat dinamikus tagságot hello csoportok lényegében egy bináris kifejezés, amely három részből áll, és IGAZ vagy hamis eredménye eredményez. hello három részek a következők:

* Bal oldali paraméter
* Bináris operátor
* Jobb oldali állandó

A teljes speciális szabály a következőhöz hasonló toothis: (leftParameter binaryOperator "RightConstant"), amennyiben a nyitó és záró zárójel hello hello teljes bináris kifejezés szükséges, dupla idézőjelek között szükség, hello szintaxisát, és jobb oldali állandó hello a hello a bal oldali paraméter user.property. Speciális szabály egynél több bináris kifejezések elválasztott hello állhat- és, - vagy, és - nem logikai operátor.
hello következő példákban a megfelelően felépített speciális szabályt:

* (felhasználó.részleg - eq "Értékesítési") – vagy (felhasználó.részleg - eq "Marketing")
* (felhasználó.részleg - eq "Értékesítési")- és - nem (user.jobTitle-tartalmazza a "SDE")

A támogatott paraméterek és kifejezés szabályoperátorokat hello teljes listáját az alábbi szakaszban talál.


Vegye figyelembe, hogy hello tulajdonság előtaggal kell a megfelelő objektumtípus hello: felhasználó vagy eszköz.
hello alatt szabály sikertelen lesz, hello érvényesítése: az e-mail – ne null

hello helyes-e a szabály a következő lesz:

User.mail – ne NULL értékű

a speciális szabály törzsét hello hello teljes hossza nem haladhatja meg a 2048 karakter.

> [!NOTE]
> Karakterlánc és regex műveletek esetében-és nagybetűket.
> Idézőjeleket tartalmazó karakterláncok "használatával kell megjelölni" karakter, például felhasználó.részleg - eq \`"Értékesítési".
> Csak akkor használjon idézőjelet karakterlánc típusú értékeket, és csak az angol nyelvű idézőjelek használata.
>
>

## <a name="supported-expression-rule-operators"></a>Támogatott kifejezés szabály operátorok
hello következő táblázat felsorolja az összes támogatott hello kifejezés szabályoperátorokat és a speciális szabály hello hello törzsét használt szintaxis toobe:

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

Minden operátorok a következők / élveznek a kisebb toohigher, operátor szerepel ugyanabban a sorban szerepelnek egyenlő élvez-bármely - összes - vagy - és - nem - eq - ne - megadott módon kezdődő - notStartsWith-- notContains tartalmazza-– notMatch egyezik-- notIn a

Minden műveleteivel végrehajtható vagy kötőjel előtag nélkül.

Vegye figyelembe, hogy zárójel nem mindig szükséges, ha sorrendje nem felel meg a követelmények például csak kell tooadd zárójel:

   felhasználó.részleg-eq "Marketing" – és felhasználó.ország-eq "US"

az egyenértékű:

   (felhasználó.részleg-eq "Marketing") – és (felhasználó.ország-eq "US")

## <a name="using-hello--in-and--notin-operators"></a>Hello segítségével – az és - notIn operátorok

Ha azt szeretné, hogy számos különböző értéket elleni felhasználói attribútum értékének toocompare hello hello - a vagy - notIn operátorokat. Íme egy példa segítségével hello - operátorban:

    user.department -In [ "50001", "50002", "50003", “50005”, “50006”, “50007”, “50008”, “50016”, “50020”, “50024”, “50038”, “50039”, “51100” ]

Vegye figyelembe a hello hello használata "[" és "]" hello elején és végén lévő hello értékekből álló listát:. A feltétel értéke tooTrue hello értékének felhasználó.részleg egyenlő egy hello értékek hello listában.

## <a name="query-error-remediation"></a>Lekérdezési hiba szervizelés
hello táblázatban a lehetséges hibákat, és hogyan toocorrect őket, ha az előfordulásuk időpontjában naplózhatja őket

| Lekérdezés-elemzési hiba | Hiba kihasználtsága | Javított kihasználtsága |
| --- | --- | --- |
| Hiba történt: Az attribútum nem támogatott. |(user.invalidProperty - eq "Érték") |(felhasználó.részleg - eq "érték")<br/>Tulajdonságot meg kell felelnie hello közül [tulajdonságainak listája támogatott](#supported-properties). |
| Hiba: Operátor nem támogatott az attribútum. |(user.accountEnabled-igaz tartalmazza) |(user.accountEnabled - eq igaz)<br/>Tulajdonság logikai érték típusú. Hello támogatott operátorokkal (-eq vagy - ne) logikai típusának hello lista fölött. |
| Hiba: Lekérdezésfordítási hiba. |(felhasználó.részleg - eq "Értékesítési")- és (felhasználó.részleg - eq "Marketing") (user.userPrincipalName-match "*@domain.ext") |(felhasználó.részleg - eq "Értékesítési")- és (felhasználó.részleg - eq "Marketing")<br/>Logikai operátor meg kell felelnie egy hello támogatott tulajdonságok a fenti listából. (user.userPrincipalName-egyeznie ". *@domain.ext") vagy (user.userPrincipalName-egyeznie "@domain.ext$") hiba történt a reguláris kifejezés. |
| Hiba: Bináris kifejezés nem megfelelő formátumban van. |(felhasználó.részleg-eq "Értékesítési") (felhasználó.részleg - eq "Értékesítési") (felhasználó.részleg-eq "Értékesítési") |(user.accountEnabled - eq igaz) – és (user.userPrincipalName-tartalmaz "alias@domain")<br/>A lekérdezés több hibát tartalmaz. Nem megfelelő helyen zárójel. |
| Hiba: Ismeretlen hiba történt a dinamikus csoporttagságok beállítása során. |(user.accountEnabled - eq "True" és user.userPrincipalName-tartalmaz "alias@domain") |(user.accountEnabled - eq igaz) – és (user.userPrincipalName-tartalmaz "alias@domain")<br/>A lekérdezés több hibát tartalmaz. Nem megfelelő helyen zárójel. |

## <a name="supported-properties"></a>Támogatott tulajdonságok
Az alábbiakban hello hello felhasználói tulajdonságok a speciális szabályt is használhatja:

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
| mail |Bármilyen karakterlánc vagy $null (SMTP-cím hello felhasználó) |(user.mail - eq "érték") |
| mailNickName |Bármilyen karakterlánc típusú értéket (mail alias hello felhasználó) |(user.mailNickName - eq "érték") |
| Mobileszköz |Bármilyen karakterlánc vagy $null |(user.mobile - eq "érték") |
| Objektumazonosító |Hello felhasználóobjektum GUID azonosítója |(user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| onPremisesSecurityIdentifier | A helyi biztonsági azonosítója (SID) a szinkronizált felhasználók helyszíni toohello felhő. |(user.onPremisesSecurityIdentifier - eq "S-1-1-11-1111111111-1111111111-1111111111-1111111") |
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

* -a (hello gyűjtemény legalább egy eleme megegyezik hello feltétel meg)
* -az összes (teljesülnek, ha hello gyűjtemény minden elemén hello feltétel felel meg)

| Tulajdonságok | Értékek | Használat |
| --- | --- | --- |
| assignedPlans |Minden hello gyűjtemény-vezérlőnek a következő karakterlánc tulajdonságai hello: capabilityStatus, szolgáltatás, servicePlanId |user.assignedPlans-bármely (assignedPlan.servicePlanId - eq "efb87545-963c-4e0d-99df-69c6916d9eb0"- és assignedPlan.capabilityStatus - eq "Engedélyezett") |

Többértékű tulajdonságok hello objektumainak gyűjteményei ugyanarra a típusra. Használhat - egy feltétel tooone vagy az összes hello elemek hello gyűjteményben, illetve bármely és - összes operátorok tooapply. Példa:

assignedPlans egy többértékű tulajdonság, amely tartalmazza az összes hozzárendelt toohello felhasználó service-csomagokról. alább kifejezés hello kiválaszt hello Exchange Online (terv 2) service-csomag, amely egyúttal az engedélyezési állapotot rendelkező felhasználók:

```
user.assignedPlans -any (assignedPlan.servicePlanId -eq "efb87545-963c-4e0d-99df-69c6916d9eb0" -and assignedPlan.capabilityStatus -eq "Enabled")
```

(hello Guid-azonosító azonosítja hello Exchange Online (terv 2) service-csomag).

> [!NOTE]
> Ez akkor hasznos, ha azt szeretné, tooidentify minden felhasználó, akinek az Office 365 (vagy más Microsoft Online Services szolgáltatással) funkció engedélyezve van, például tootarget házirendek bizonyos állítja be őket.

hello következő kifejezés fogja kiválasztani bármely service-csomag társított hello Intune-ba (szolgáltatásnév "SCO" által azonosított) rendelkező felhasználók:
```
user.assignedPlans -any (assignedPlan.service -eq "SCO" -and assignedPlan.capabilityStatus -eq "Enabled")
```

## <a name="use-of-null-values"></a>Null értékek használatát

toospecify null értéket a szabályban, "null" vagy a $null használja. Példa:

   User.mail – ne null értéke megegyezik user.mail – ne $null

## <a name="extension-attributes-and-custom-attributes"></a>Egyéni oszlopainál és a bővítmény
A bővítményattribútumokat és egyéni attribútumok dinamikus tagsági szabályok támogatottak.

Kiterjesztési attribútumot a helyszíni Windows Server AD szinkronizált, és tegye meg "ExtensionAttributeX", ahol X értéke 1 – 15 hello formátuma.
Egy bővítmény attribútumot használó szabály például lenne.

(user.extensionAttribute15 - eq "Marketing")

Az egyéni attribútumok a helyi Windows Server AD-ről vagy egy csatlakoztatott SaaS alkalmazás és hello hello formátuma szinkronizált "user.extension_[GUID]\__ [attribútum]", ahol a [GUID] hello alkalmazás egyedi azonosítója az aad-ben hello, az aad-ben és a [attribútum] létrehozott hello attribútum hello attribútum hello neve megegyezik hozták létre.
Példa egy szabályt, amely egy egyéni attribútumot használ:

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

hello egyéni attribútum nevében található hello directory lekérdezésével egy felhasználói attribútum Graph Explorerrel és hello attribútumnév keres.

## <a name="direct-reports-rule"></a>"Közvetlen beosztottai" szabály
Létrehozhat egy Manager minden közvetlen beosztottai tartalmazó csoport. Hello manager közvetlen jelentések módosításakor a jövőbeli hello hello csoport tagsága automatikusan igazítani.

> [!NOTE]
> 1. Hello szabály toowork, győződjön meg arról, hogy hello **Manager azonosító** tulajdonság helyesen beállítva a felhasználók az Ön bérelt szolgáltatásának. Akkor is ellenőrizhesse a felhasználó aktuális értéke hello azok **profil lapon**.
> 2. Ez a szabály csak **közvetlen** jelentéseket. Jelenleg nem lehetséges toocreate beágyazott hierarchiáját, pl. tartalmazó csoport közvetlen és a jelentések egy csoportot.

**tooconfigure hello csoport**

1. Hajtsa végre a 1-5 lépések szakaszából [toocreate hello speciális szabály](#to-create-the-advanced-rule), és válassza ki a **tagsági típusa** a **dinamikus felhasználói**.
2. A hello **dinamikus tagsági szabályok** panelen adja meg a hello szabály a hello szintaxisa a következő:

    *A(z) "{obectID_of_manager}" közvetlen beosztottaik*

    Példa egy érvényes szabályt:
```
                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863"
```
    where “62e19b97-8b3d-4d4a-a106-4ce66896a863” is hello objectID of hello manager. hello object ID can be found on manager's **Profile tab**.
3. Hello szabály mentése után minden felhasználó hello megadott toohello csoport Manager azonosító értéket ad hozzá.

## <a name="using-attributes-toocreate-rules-for-device-objects"></a>Az eszközobjektumok attribútumok toocreate szabályok használatával
Olyan szabály, amely kijelöli a tagság eszközobjektumok egy csoportot is létrehozhat. a következő eszközattribútumokon hello használhatók:

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
> Ezeket az eszköz szabályokat nem hozható létre, a klasszikus Azure portálon hello hello "egyszerű szabály" legördülő lista használatával.
>
>

## <a name="next-steps"></a>Következő lépések
E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.

* [A csoportok dinamikus csoporttagságok hibaelhárítása](active-directory-accessmanagement-troubleshooting.md)
* [Hozzáférés tooresources kezelése az Azure Active Directoryval](active-directory-manage-groups.md)
* [Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke](active-directory-apps-index.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)

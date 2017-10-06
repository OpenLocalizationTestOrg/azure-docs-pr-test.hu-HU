---
title: "Jogcím-leképezés az Azure Active Directoryban (nyilvános előzetes verzió) |} Microsoft Docs"
description: "Ez a lap leképezési Azure Active Directory jogcímszolgáltatói ismerteti."
services: active-directory
author: billmath
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: billmath
ms.openlocfilehash: ff07b9954d5c2ce71ab0ffd0db49fde15f323586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="claims-mapping-in-azure-active-directory-public-preview"></a>A jogcímek hozzárendelése az Azure Active Directoryban (nyilvános előzetes verzió)

>[!NOTE]
>Ez a szolgáltatás váltja fel, és írja felül a hello [testreszabási jogcímek](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) ma kínált hello portálon keresztül. Jogcímek hello portál testreszabásakor továbbá toohello Graph/PowerShell-módszer részletes ebben a dokumentumban a hello azonos alkalmazás, az alkalmazás figyelmen kívül hagyja hello konfigurációs hello portálon kiállított jogkivonatokat.
A jelen dokumentumban szereplő hello módszerrel végzett konfigurációk nem tükröződnek hello portálon.

Ez a funkció a bérlői rendszergazdák toocustomize hello jogcímeket egy adott alkalmazáshoz a bérlőben jogkivonatokba kibocsátott használják. A házirendek hozzárendelése jogcímek használata:

- Válassza ki, melyik jogkivonatok szerepelnek.
- Hozzon létre jogcímtípusokra vonatkozik, amelyeket már nem létezik.
- Válassza ki, vagy módosítsa az adott jogcímek kibocsátott hello adatforrás.

>[!NOTE]
>Ez a funkció jelenleg nyilvános előzetes verziójához. Készüljön toorevert, vagy távolítsa el a módosításokat. bármely Azure Active Directory (Azure AD) előfizetés nyilvános előzetes hello szolgáltatás érhető el. Amikor hello szolgáltatás általánosan elérhetővé válik, azonban bizonyos aspektusainak hello szolgáltatást szükség lehet az Azure Active Directory premium előfizetéssel.

## <a name="claims-mapping-policy-type"></a>A jogcímek leképezési házirend típusa
Az Azure AD egy **házirend** objektum által jelképezett szabályok lépnek érvénybe, az egyes alkalmazások vagy a szervezeten belüli összes alkalmazást. Mindegyik házirendtípusnál struktúrája egy egyedi, tulajdonságokhoz állítja be, majd hozzárendeli tooobjects toowhich alkalmazza.

A jogcím-házirend hozzárendelése egy olyan típusú **házirend** objektum, amely módosítja a hello jogcímek, az adott alkalmazásokra vonatkozó kiállított jogkivonatokat a kibocsátott.

## <a name="claim-sets"></a>Jogcímszabály-készletek
Nincsenek bizonyos-készletek, amelyek meghatározzák, hogy mikor és hogyan használhatók a jogkivonatok jogcímeket.

### <a name="core-claim-set"></a>Alapvető jogcímkészlethez
A hello core jogcím set minden jogkivonatban, függetlenül a házirend szerepelnek. Ezeket a jogcímeket is tartoznak, korlátozott, és nem módosítható.

### <a name="basic-claim-set"></a>Alapszintű jogcímek készletéhez
hello alapvető jogcím hello alapértelmezett jogkivonatokat (a hozzáadása toohello core jogcímkészlethez) által kibocsátott jogcímeket tartalmaz. Ezeket a jogcímeket nincs megadva, vagy módosította a házirendek hozzárendelése hello jogcímek használata.

### <a name="restricted-claim-set"></a>Korlátozott jogcímkészlethez
Korlátozott jogcímek házirend használatával nem módosítható. hello adatforrás nem módosítható, és átalakítás nélkül alkalmazza ezeket a jogcímeket létrehozásakor.

#### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>1. táblázat: JSON webes jogkivonat (JWT) korlátozott jogcímek készletéhez
|Jogcím típusa (név)|
| ----- |
|_claim_names|
|_claim_sources|
|access_token|
|account_type|
|ACR|
|Aktor|
|actortoken|
|aio|
|altsecid|
|AMR|
|app_chain|
|app_displayname|
|app_res|
|appctx|
|appctxsender|
|alkalmazásazonosító|
|appidacr|
|helyességi feltétel|
|at_hash|
|és|
|auth_data|
|auth_time|
|authorization_code|
|azp|
|azpacr|
|c_hash|
|ca_enf|
|Másolatot kap|
|cert_token_use|
|client_id|
|cloud_graph_host_name|
|cloud_instance_name|
|használható cnf paraméterrel|
|Kód|
|vezérlők|
|credential_keys|
|CSR|
|csr_type|
|eszközazonosító|
|dns_names|
|domain_dns_name|
|domain_netbios_name|
|e_exp|
|E-mailek|
|végpont|
|enfpolids|
|Exp|
|expires_on|
|grant_type|
|Diagram|
|group_sids|
|csoportok|
|hasgroups|
|hash_alg|
|home_oid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationInstant|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expired|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Name|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier|
|IAT|
|identityprovider|
|IDP|
|in_corp|
|Példány|
|IPADDR|
|isbrowserhostedapp|
|iss|
|jwk|
|key_id|
|key_type|
|mam_compliance_url|
|mam_enrollment_url|
|mam_terms_of_use_url|
|mdm_compliance_url|
|mdm_enrollment_url|
|mdm_terms_of_use_url|
|nameid|
|NBF|
|netbios_name|
|Nonce|
|OID|
|on_prem_id|
|onprem_sam_account_name|
|onprem_sid|
|openid2_id|
|jelszó|
|platf|
|polids|
|pop_jwk|
|preferred_username|
|previous_refresh_token|
|primary_sid|
|PUID azonosítója|
|pwd_exp|
|pwd_url|
|redirect_uri|
|refresh_token|
|refreshtoken|
|request_nonce|
|Erőforrás|
|Szerepkör|
|roles|
|Hatókör|
|Szolgáltatáskapcsolódási pont|
|biztonsági azonosító|
|Aláírás|
|signin_state|
|src1|
|src2|
|Sub|
|tbid|
|tenant_display_name|
|tenant_region_scope|
|thumbnail_photo|
|TID|
|tokenAutologonEnabled|
|trustedfordelegation|
|unique_name|
|egyszerű felhasználónév|
|user_setting_sync_url|
|felhasználónév|
|uti|
|ver|
|verified_primary_email|
|verified_secondary_email|
|wids|
|win_ver|

#### <a name="table-2-security-assertion-markup-language-saml-restricted-claim-set"></a>2. táblázat: Security Assertion Markup Language (SAML) korlátozott jogcímek készletéhez
|Jogcím típusa (URI)|
| ----- |
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expired|
|http://schemas.microsoft.com/Identity/Claims/accesstoken|
|http://schemas.microsoft.com/Identity/Claims/openid2_id|
|http://schemas.microsoft.com/Identity/Claims/identityprovider|
|http://schemas.microsoft.com/Identity/Claims/objectidentifier|
|http://schemas.microsoft.com/Identity/Claims/PUID|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier [MR1] |
|http://schemas.microsoft.com/Identity/Claims/tenantid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationInstant|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/accesscontrolservice/2010/07/Claims/identityprovider|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/groups|
|http://schemas.microsoft.com/Claims/groups.Link|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Role|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/wids|
|http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant|
|http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown|
|http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged|
|http://schemas.microsoft.com/2014/03/psso|
|http://schemas.microsoft.com/Claims/authnmethodsreferences|
|http://schemas.xmlsoap.org/ws/2009/09/Identity/Claims/Actor|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/samlissuername|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/confirmationkey|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsaccountname|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarysid|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/authorizationdecision|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Authentication|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SID|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarysid|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/denyonlysid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlywindowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdeviceclaim|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsfqbnversion|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowssubauthority|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsuserclaim|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/x500distinguishedname|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/GroupSID|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SPN|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/ispersistent|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/privatepersonalidentifier|
|http://schemas.microsoft.com/Identity/Claims/scope|

## <a name="claims-mapping-policy-properties"></a>A jogcímek leképezési házirend tulajdonságai
Egy leképezési házirend toocontrol melyik kibocsátott jogcímeket hello tulajdonságait, és ahol hello adatok származik. Nincs házirend be van állítva, hello rendszer hello core jogcímkészlethez tartalmazó jogkivonatokat állít ki, alapszintű hello jogcímek készletéhez és alkalmazás hello választható jogcímeket tooreceive választotta.

### <a name="include-basic-claim-set"></a>Alapszintű jogcímkészlethez tartalmazza

**Karakterlánc:** IncludeBasicClaimSet

**Adattípus:** logikai (IGAZ vagy hamis)

**Összefoglalás:** Ez a tulajdonság határozza meg, hogy hello alapvető jogcímek készletében van a házirend által érintett jogkivonatokba. 

- Ha a készlet tooTrue, minden hello alapvető jogcímek készletében lévő jogcímek egyikével kibocsátott hello szabályzat jogkivonatokba. 
- Ha set tooFalse, hello alapvető jogcímek készletében nem hello jogkivonatokat, kivéve, ha külön-külön hozzáadásuk után hello hello jogcímek séma tulajdonságában azonos házirend.

>[!NOTE] 
>A hello core jogcím set minden jogkivonatban, függetlenül attól, milyen Ez a tulajdonság értéke szerepelnek. 

### <a name="claims-schema"></a>Jogcímek séma

**Karakterlánc:** ClaimsSchema

**Adattípus:** JSON blob egy vagy több jogcím séma megadásával

**Összefoglalás:** Ez a tulajdonság határozza meg, milyen jogcímek szerepelnek hello szabályzat hello jogkivonatokat, továbbá az alapvető toohello jogcímek készletéhez és hello core jogcímek készletéhez.
Minden jogcím séma bejegyzéshez ebben a tulajdonságban meghatározott bizonyos információkra szükség. Meg kell adnia, ahol hello adatok származik (**érték** vagy **forrás vagy-azonosító pár**), és amely jogcím hello adatok kibocsátott (**jogcím típusa**).

### <a name="claim-schema-entry-elements"></a>Jogcím sémaelemek-bejegyzés

**Érték:** hello érték elem hello adatok toobe hello jogcímek kibocsátott, statikus értéket határoz meg.

**Forrás vagy-azonosító pár:** forrás hello és azonosító elemek határozza meg, ahol hello adatok hello jogcímek származik. 

hello forráselem hello következő tooone be kell állítani: 


- "user": hello hello adatok jogcím hello User objektum tulajdonság. 
- "alkalmazás": hello hello adatok jogcím hello (ügyfél) alkalmazás egyszerű egy tulajdonság. 
- "erőforrás": hello hello adatok jogcím hello erőforrás egyszerű egy tulajdonság.
- "célközönség": hello hello jogcím adatai egyik tulajdonságának hello szolgáltatás egyszerű, amely hello célközönség hello jogkivonat (vagy hello ügyfél vagy az erőforrás egyszerű szolgáltatásnév).
- "Vállalati": hello hello adatok jogcím egy tulajdonság hello erőforrás bérlői vállalati objektumnál.
- "átalakítása": hello hello adatok jogcím, a jogcímek átalakításáról (lásd a cikkben később hello "jogcímek átalakítása" szakaszt). 

Ha hello forrás átalakítása, hello **TransformationID** elem szerepelnie kell a jogcímdefiníciót.

hello "ID" elem. azonosítja, hogy melyik tulajdonság hello forrás hello értéket ad meg hello jogcímet. hello következő táblázatban hello értékek minden forrás értékének érvényes azonosító.

#### <a name="table-3-valid-id-values-per-source"></a>3. táblázat: Érvényes azonosító értékek forrásonként
|Forrás|ID (Azonosító)|Leírás|
|-----|-----|-----|
|Felhasználó|Vezetéknév|Család neve|
|Felhasználó|givenName|Utónév|
|Felhasználó|DisplayName|Megjelenített név|
|Felhasználó|objektumazonosító|Objektumazonosító|
|Felhasználó|mail|E-mail cím|
|Felhasználó|userPrincipalName|Egyszerű felhasználónév|
|Felhasználó|Szervezeti egység|Szervezeti egység|
|Felhasználó|onpremisessamaccountname|A helyi Sam-fiók neve|
|Felhasználó|netbiosname|NetBIOS-név|
|Felhasználó|tartománynév|DNS-tartománynév|
|Felhasználó|onpremisesecurityidentifier|a helyi biztonsági azonosítója|
|Felhasználó|Cégnév|Szervezet neve|
|Felhasználó|streetAddress|Utca, házszám|
|Felhasználó|Irányítószám|Irányítószám|
|Felhasználó|preferredlanguange|Választott nyelv|
|Felhasználó|onpremisesuserprincipalname|a helyszíni egyszerű Felhasználónévvel|
|Felhasználó|mailnickname|Mail becenév|
|Felhasználó|extensionAttribute1|Mellék attribútum 1|
|Felhasználó|extensionattribute2|Mellék attribútum 2|
|Felhasználó|extensionattribute3|Mellék attribútum 3|
|Felhasználó|extensionattribute4|Mellék attribútum 4|
|Felhasználó|extensionattribute5|Mellék attribútum 5|
|Felhasználó|extensionattribute6|Mellék attribútum 6|
|Felhasználó|extensionattribute7|Mellék attribútum 7|
|Felhasználó|extensionattribute8|Mellék attribútum 8|
|Felhasználó|extensionattribute9|Mellék attribútum 9|
|Felhasználó|extensionattribute10|Mellék attribútum 10|
|Felhasználó|extensionattribute11|Mellék attribútum 11|
|Felhasználó|extensionattribute12|Mellék attribútum 12|
|Felhasználó|extensionattribute13|Mellék attribútum 13|
|Felhasználó|extensionattribute14|Mellék attribútum 14|
|Felhasználó|extensionattribute15|Mellék attribútum 15|
|Felhasználó|othermail|Más E-mail|
|Felhasználó|Ország|Ország|
|Felhasználó|city|Város|
|Felhasználó|state|Állapot|
|Felhasználó|Beosztás|Beosztás|
|Felhasználó|EmployeeID|Alkalmazott azonosítója|
|Felhasználó|facsimiletelephonenumber|Fax Telefonszám|
|alkalmazás, erőforrás, a célközönség|DisplayName|Megjelenített név|
|alkalmazás, erőforrás, a célközönség|kifogásolt|Objektumazonosító|
|alkalmazás, erőforrás, a célközönség|tags|Szolgáltatás egyszerű címke|
|Cég|tenantcountry|Bérlő ország|

**TransformationID:** hello TransformationID elemnek kell lennie, csak ha hello túl van-e állítva a forráselem megadott "átalakítása".

- Ennek az elemnek meg kell egyeznie a hello "ID" elem. hello átalakítása bejegyzés hello **ClaimsTransformation** tulajdonság, amely meghatározza, hogyan generáljon a rendszer a jogcím hello adatait.

**Jogcím típusa:** hello **JwtClaimType** és **SamlClaimType** elemek határozza meg, amely jogcím, a jogcím-séma bejegyzés hivatkozik.

- hello JwtClaimType hello jogcím toobe JWTs a kibocsátott hello nevét kell tartalmaznia.
- hello SamlClaimType URI-jának hello toobe kibocsátott SAML-jogkivonatokat a jogcím hello kell tartalmaznia.

>[!NOTE]
>Nevek és URI-azonosítók jogcím szerepel hello korlátozott jogcím set hello jogcím típusú elemek esetében nem használható. További információkért lásd: hello "Kivételeket és korlátozások" című cikkben.

### <a name="claims-transformation"></a>Jogcím átalakítása

**Karakterlánc:** ClaimsTransformation

**Adattípus:** JSON blob egy vagy több átalakítása megadásával 

**Összefoglalás:** az tulajdonság tooapply közös átalakítások toosource adatok, a megadott jogcímek hello jogcímek séma toogenerate hello kimeneti adatokat.

**Azonosító:** hello azonosító elem tooreference hello TransformationID jogcímek séma bejegyzés átalakítása tételhez használja. Ez az érték átalakítása tételek belül a házirend egyedinek kell lennie.

**TransformationMethod:** hello TransformationMethod elem azonosít művelettől elvégezni toogenerate hello adatok hello jogcímet.

Alapján kiválasztott hello módszert, amelynek bemenetekhez és kimenetekhez várt. Ezek hello használatával definiáltak **InputClaims**, **InputParameters** és **OutputClaims** elemek.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>4. táblázat: Átalakítási metódusok és várt bemenetekhez és kimenetekhez
|TransformationMethod|Várt bemeneti|Várt kimenet|Leírás|
|-----|-----|-----|-----|
|Csatlakozás|karakterlánc1, karakterlánc2, elválasztó|outputClaim|Illesztések karakterláncok adjon meg egy elválasztó bejelentkezve használatával. Például: karakterlánc1: "foo@bar.com", karakterlánc2: "védőfal", az elválasztó: "." outputClaim eredményez: "foo@bar.com.sandbox"|
|ExtractMailPrefix|mail|outputClaim|Kibontja a hello helyi része egy e-mail címet. Például: mail: "foo@bar.com" outputClaim eredményez: "foo". Ha @ nem bejelentkezési jelen, majd hello orignal bemeneti karakterláncot ad vissza.|

**InputClaims:** egy InputClaims elem toopass hello adatait használják a jogcím-séma bejegyzés tooa átalakítást. Két attribútumot tartalmaz: **ClaimTypeReferenceId** és **TransformationClaimType**.

- **ClaimTypeReferenceId** hello jogcím séma bejegyzés toofind hello megfelelő bemeneti jogcím "ID" elem. kapcsolódik. 
- **TransformationClaimType** használt toogive egy egyedi nevet toothis bemeneti van. Ezt a nevet meg kell egyeznie egy várt hello bemenetek hello átalakítási metódus.

**InputParameters:** egy InputParameters elem toopass konstans tooa átalakítás használja. Két attribútumot tartalmaz: **érték** és **azonosító**.

- **Érték** hello tényleges konstans toobe átadása.
- **Azonosító** használt toogive egy egyedi nevet toothis bemeneti van. Ezt a nevet meg kell egyeznie egy várt hello bemenetek hello átalakítási metódus.

**OutputClaims:** egy OutputClaims elem toohold hello által létrehozott adatok átalakítás használja, és tooa jogcím séma bejegyzés összekötését azt. Két attribútumot tartalmaz: **ClaimTypeReferenceId** és **TransformationClaimType**.

- **ClaimTypeReferenceId** hello jogcím séma bejegyzés toofind hello megfelelő kimeneti jogcímek hello azonosítójú csatlakoztatva van.
- **TransformationClaimType** egy használt toogive egy egyedi nevet toothis kimenet. Ezt a nevet meg kell egyeznie egy várt hello kimenetek hello átalakítási metódus.

### <a name="exceptions-and-restrictions"></a>Kivételek és korlátozások

**SAML NameID és egyszerű felhasználónév:** megnyitása, amelyen a forrás hello NameID és UPN értékét, és hello átalakítások, amelyeknél engedélyezve van, jogcímek hello attribútumait korlátozottak.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>5. táblázat: Attribútumok SAML NameID adatforrásként engedélyezett
|Forrás|ID (Azonosító)|Leírás|
|-----|-----|-----|
|Felhasználó|mail|E-mail cím|
|Felhasználó|userPrincipalName|Egyszerű felhasználónév|
|Felhasználó|onpremisessamaccountname|A helyi Sam-fiók neve|
|Felhasználó|EmployeeID|Alkalmazott azonosítója|
|Felhasználó|extensionAttribute1|Mellék attribútum 1|
|Felhasználó|extensionattribute2|Mellék attribútum 2|
|Felhasználó|extensionattribute3|Mellék attribútum 3|
|Felhasználó|extensionattribute4|Mellék attribútum 4|
|Felhasználó|extensionattribute5|Mellék attribútum 5|
|Felhasználó|extensionattribute6|Mellék attribútum 6|
|Felhasználó|extensionattribute7|Mellék attribútum 7|
|Felhasználó|extensionattribute8|Mellék attribútum 8|
|Felhasználó|extensionattribute9|Mellék attribútum 9|
|Felhasználó|extensionattribute10|Mellék attribútum 10|
|Felhasználó|extensionattribute11|Mellék attribútum 11|
|Felhasználó|extensionattribute12|Mellék attribútum 12|
|Felhasználó|extensionattribute13|Mellék attribútum 13|
|Felhasználó|extensionattribute14|Mellék attribútum 14|
|Felhasználó|extensionattribute15|Mellék attribútum 15|

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>6. táblázat: Átalakítási metódusok SAML NameID engedélyezett
|TransformationMethod|Korlátozások|
| ----- | ----- |
|ExtractMailPrefix|None|
|Csatlakozás|hello utótag alatt csatlakoztatott hello erőforrás bérlő ellenőrzött tartományt kell lennie.|

### <a name="custom-signing-key"></a>Egyéni aláírási kulcs
Egy egyéni aláírási kulcs toohello szolgáltatás egyszerű objektum egy házirend tootake hatás leképezési jogcímek kell hozzárendelni. Minden olyan kiadott jogkivonatokat, amelyek rendelkezik hello házirend nincs hatással vannak aláírva ezt a kulcsot. Alkalmazások konfigurált tooaccept jogkivonatok aláírva, ezt a kulcsot kell lennie. Ez biztosítja, hogy módosult-e a jogkivonatok hello hello készítője által visszaigazolás jogcím-hozzárendelési házirendet. Alkalmazások megakadályozza kártevő szereplője által létrehozott házirendek hozzárendelése jogcímeket.

### <a name="cross-tenant-scenarios"></a>Több-bérlős forgatókönyvek
Leképezési házirendek nem lesznek alkalmazva tooguest felhasználói jogcímeket. Akkor a Vendég tooaccess egy alkalmazás, egy jogcímeket leképezési házirendje tooits szolgáltatás egyszerű, hello alapértelmezett tokent ad ki (hello házirend nincs hatása).

## <a name="claims-mapping-policy-assignment"></a>Házirend-hozzárendelés leképezési jogcímek
Jogcím-leképezés házirend csak rendelhető tooservice egyszerű objektumok.

### <a name="example-claims-mapping-policies"></a>Példa jogcím-leképezés házirendek

Az Azure ad-ben több forgatókönyv is előfordulhatnak, ha testre szabhatja az adott szolgáltatás rendszerbiztonsági tagoknak jogkivonatokba kibocsátott jogcímeket. Ez a szakasz azt végezze el néhány olyan gyakori forgatókönyvet, amelyik segíthet a megfogható hogyan toouse hello jogcím-e a leképezés típus.

#### <a name="prerequisites"></a>Előfeltételek
A következő példák hello akkor létrehozása, frissítése, hivatkozás és házirendek törlése szolgáltatás rendszerbiztonsági tagoknak. Ha új tooAzure AD, azt javasoljuk, hogy Ismerkedjen meg arról, hogyan tooget az Azure AD bérlői, mielőtt folytatná a példákat. 

elindult, tooget hello a következő lépéseket:


1. Töltse le a hello legújabb [Azure AD PowerShell modult nyilvános előzetes](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.127).
2.  Hello Connect parancs toosign futtatása tooyour az Azure AD rendszergazdai fiókot. Futtassa ezt a parancsot minden alkalommal, amikor új munkamenet indításához.
    
     ``` powershell
    Connect-AzureAD -Confirm
    
    ```
3.  a parancs, amelyet a szervezetében, futtassa a következő hello létrehozott összes házirend toosee. Azt javasoljuk, hogy a parancs futtatása után a legtöbb műveletei hello következő forgatókönyvek esetén, amely a házirendek létrehozása, toocheck várt.
   
    ``` powershell
        Get-AzureADPolicy
    
    ```
#### <a name="example-create-and-assign-a-policy-tooomit-hello-basic-claims-from-tokens-issued-tooa-service-principal"></a>Példa: Hozzon létre, és rendelje hozzá a házirend tooomit hello alapvető jogcím a kiállított jogkivonatokat tooa egyszerű szolgáltatásnév.
Ebben a példában létrehozhat olyan házirendet, amely alapszintű jogcímkészlethez hello eltávolítja a kiállított jogkivonatokat toolinked szolgáltatásnevekről.


1. Hozzon létre egy házirend leképezési jogcímeket. Ezt a házirendet, a csatolt toospecific szolgáltatásnevekről, hello alapvető jogcímkészlethez eltávolítása jogkivonatokat.
    1. toocreate hello házirend, futtassa a parancsot: 
    
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims” -Type "ClaimsMappingPolicy"
    ```
    2. az új házirend, és tooget hello házirend ObjectId, futtassa a következő hello parancs toosee:
    
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Rendelje hozzá a hello házirend tooyour egyszerű szolgáltatást. Meg kell, a szolgáltatás egyszerű ObjectId tooget hello is. 
    1.  toosee a szervezet szolgáltatásnevekről, a Microsoft Graph lekérdezhető. Vagy az Azure AD Graph Explorerben, jelentkezzen be tooyour Azure AD-fiókot.
    2.  Ha rendelkezik hello ObjectId, a szolgáltatás egyszerű, futtatási hello a következő parancsot:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
#### <a name="example-create-and-assign-a-policy-tooinclude-hello-employeeid-and-tenantcountry-as-claims-in-tokens-issued-tooa-service-principal"></a>Példa: Hozzon létre, és rendelje hozzá a házirend tooinclude hello EmployeeID és TenantCountry jogkivonatokat a jogcím a kiállított tooa egyszerű.
Ebben a példában, amely hello EmployeeID házirendet hoz létre, és TenantCountry tootokens kiadott toolinked szolgáltatásnevekről. hello EmployeeID kibocsátott hello neve jogcím típusa SAML-jogkivonatokat és JWTs is. hello TenantCountry kibocsátott, hello ország jogcím típusa SAML-jogkivonatokat és JWTs is. Ebben a példában a Folytatás tooinclude hello alapvető jogcímek hello jogkivonatokba beállítása.

1. Hozzon létre egy házirend leképezési jogcímeket. Ezt a házirendet, a csatolt toospecific szolgáltatásnevekről, hello EmployeeID és TenantCountry jogcímek tootokens hozzáadja.
    1. toocreate hello házirend, futtassa a parancsot:  
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":" tenantcountry ","SamlClaimType":" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country ","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. az új házirend, és tooget hello házirend ObjectId, futtassa a következő hello parancs toosee:
     
     ``` powershell  
    Get-AzureADPolicy
    ```
2.  Rendelje hozzá a hello házirend tooyour egyszerű szolgáltatást. Meg kell, a szolgáltatás egyszerű ObjectId tooget hello is. 
    1.  toosee a szervezet szolgáltatásnevekről, a Microsoft Graph lekérdezhető. Vagy az Azure AD Graph Explorerben, jelentkezzen be tooyour Azure AD-fiókot.
    2.  Ha rendelkezik hello ObjectId, a szolgáltatás egyszerű, futtatási hello a következő parancsot:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-tooa-service-principal"></a>Példa: Hozzon létre, és rendelje hozzá olyan házirendet, amely a kiállított jogkivonatokat tooa egyszerű szolgáltatásnév a jogcímek átalakításáról használja.
Ebben a példában hozzon létre egy megfelelően kibocsát egy egyéni tooJWTs toolinked szolgáltatásnevekről kiállított jogcímet "JoinedData" házirendet. A jogcím értéke a hello extensionattribute1 attribútum hello felhasználói objektum ".sandbox" hello adataihoz összeillesztésével lett létrehozva. Ebben a példában a Microsoft hello alapvető jogcímek hello jogkivonatokba beállítása zárhatja ki.


1. Hozzon létre egy házirend leképezési jogcímeket. Ezt a házirendet, a csatolt toospecific szolgáltatásnevekről, hello EmployeeID és TenantCountry jogcímek tootokens hozzáadja.
    1. toocreate hello házirend, futtassa a parancsot: 
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformation":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"Id":"string2","Value":"sandbox"},{"Id":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. az új házirend, és tooget hello házirend ObjectId, futtassa a következő hello parancs toosee: 
     
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Rendelje hozzá a hello házirend tooyour egyszerű szolgáltatást. Meg kell, a szolgáltatás egyszerű ObjectId tooget hello is. 
    1.  toosee a szervezet szolgáltatásnevekről, a Microsoft Graph lekérdezhető. Vagy az Azure AD Graph Explorerben, jelentkezzen be tooyour Azure AD-fiókot.
    2.  Ha rendelkezik hello ObjectId, a szolgáltatás egyszerű, futtatási hello a következő parancsot: 
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```

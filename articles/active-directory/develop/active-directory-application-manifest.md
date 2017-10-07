---
title: Azure Active Directory Application Manifest aaaUnderstanding hello |} Microsoft Docs
description: "Hello Azure Active Directory alkalmazásjegyzékének, egy alkalmazás identitását konfigurációját az Azure AD-bérlő jelöli, és a használt toofacilitate OAuth-engedélyezési és hozzájárulási élmény több részletes körét."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: 096c9d5501edbfc08731fea670cee559d4442ad1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-azure-active-directory-application-manifest"></a>Hello Azure Active Directory alkalmazásjegyzékének megismerése
Az Azure AD-bérlő, hello alkalmazás állandó identitáskonfigurációs biztosító alkalmazásokat, amelyek az Azure Active Directory (AD) integrálása szerepelnie kell. Ez a konfiguráció van megkeresett futásidőben, forgatókönyvek, amelyek lehetővé teszik az alkalmazás toooutsource és broker hitelesítési/engedélyezési keresztül az Azure AD engedélyezése. Az Azure AD hello alkalmazásmodell kapcsolatos további információkért lásd: hello [hozzáadása, frissítése és eltávolítása egy alkalmazás] [ ADD-UPD-RMV-APP] cikk.

## <a name="updating-an-applications-identity-configuration"></a>Az alkalmazás identitását konfigurációjának frissítése
Ténylegesen több lehetőség áll rendelkezésre hello tulajdonságainak egy identitás alkalmazást, a képességek és a probléma, beleértve a következőket hello fok változó frissítése:

* Hello  **[Azure portal] [ AZURE-PORTAL] webes felhasználói felület** tooupdate hello leggyakrabban használt alkalmazás tulajdonságainak lehetővé teszi. Ez a leggyorsabb hello és legalább hiba nagyon eséllyel fordulnak elő módon frissítésére a az alkalmazás tulajdonságait, de nem teljes tooall tulajdonságait, például a következő két módszer hello biztosítanak.
* Speciális forgatókönyvek esetén, amelyek nem érhetők el a klasszikus Azure portálon hello tooupdate tulajdonságok kell, módosíthatja a hello **alkalmazásjegyzék**. Ez hello célja az ebben a cikkben, és további részletes információkat hello a következő szakaszban indítása.
* Lehetőség arra is túl**az alkalmazás által használt hello [Graph API] [ GRAPH-API]**  tooupdate, amelyhez az alkalmazás hello legtöbb elérhető. Erre akkor lehet vonzó lehetőség azonban, ha felügyeleti szoftver ír, vagy tooupdate alkalmazástulajdonságok rendszeresen automatizált módon kell.

## <a name="using-hello-application-manifest-tooupdate-an-applications-identity-configuration"></a>Egy alkalmazás identitás-konfigurációt használni, hello application manifest tooupdate
Hello keresztül [Azure-portálon][AZURE-PORTAL], kezelheti az alkalmazás identitását konfigurációs szerkesztőjével hello beágyazott jegyzék hello alkalmazásjegyzék frissítésével. Letöltheti, és töltse fel az alkalmazásjegyzék hello JSON-fájlként. Nincs a fájl tényleges hello könyvtárban tárolódik. hello alkalmazásjegyzék csupán egy HTTP GET művelet hello Azure AD Graph API-alkalmazás entitáson, továbbá hello feltöltés hello alkalmazás entitás HTTP javítás művelet.

Ennek eredményeképpen a rendelés toounderstand hello formátum és hello alkalmazásjegyzék tulajdonságait, kell tooreference hello Graph API [alkalmazás entitás] [ APPLICATION-ENTITY] dokumentációját. Például, ha az alkalmazás jegyzékében feltöltés végrehajtható frissítések:

* **Deklarálja engedélyhatókörök (oauth2Permissions)** jelennek meg, ha a webes API-t. Hello "Exposing webes API-k tooOther alkalmazások" témaköre [alkalmazások integrálása az Azure Active Directoryval] [ INTEGRATING-APPLICATIONS-AAD] kapcsolatos felhasználói megszemélyesítés hello használata oauth2Permissions delegált engedéllyel hatókör. Ahogy korábban említettük, entitás alkalmazástulajdonságok hello Graph API ismertetett [entitás és a komplex típus] [ APPLICATION-ENTITY] áttekintésével foglalkozó cikkben, beleértve a hello oauth2Permissions tulajdonság, amely egy típusú gyűjtemény [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Deklarálja az alkalmazás által elérhetővé tett alkalmazás-szerepkörök (appRoles)**. hello alkalmazás entitás appRoles tulajdonság típusú gyűjteményhez [birtokolhat][APPLICATION-ENTITY-APP-ROLE]. Lásd: hello [szerepköralapú hozzáférés-vezérlés az Azure AD felhőalapú alkalmazások] [ RBAC-CLOUD-APPS-AZUREAD] cikk egy megvalósítási példát.
* **Deklarálja ismert ügyfél alkalmazások (knownClientApplications)**, amely toologically tie hello beleegyezése hello lehetővé teszik a megadott ügyfél alkalmazás(ok) toohello erőforrás vagy webes API.
* **Az Azure AD tooissue csoporttagságok jogcím kérelem** a bejelentkezett felhasználó (groupMembershipClaims) hello.  Ez lehet konfigurált tooissue állítások hello felhasználók directory szerepkörök tagságát is. Lásd: hello [felhőalapú alkalmazások AD-csoportok használata az engedélyezési] [ AAD-GROUPS-FOR-AUTHORIZATION] cikk egy megvalósítási példát.
* **Az alkalmazás OAuth 2.0 Implicit toosupport-támogatás engedélyezése** adatfolyamok (oauth2AllowImplicitFlow). Az ilyen típusú grant flow beágyazott JavaScript weblapok vagy a lap alkalmazások (SPA) használatos. Hello implicit hitelesítésengedélyezési további információkért lásd: [ismertetése hello OAuth2 implicit adja meg az Azure Active Directoryban folyamat][IMPLICIT-GRANT].
* **X509 használatának engedélyezése hello titkos kulcs tanúsítványokhoz** (keyCredentials). Lásd: hello [Office 365 szolgáltatás és -démon alkalmazások készítéséhez] [ O365-SERVICE-DAEMON-APPS] és [– fejlesztői útmutató tooauth az Azure Resource Manager API-JÁVAL] [ DEV-GUIDE-TO-AUTH-WITH-ARM] megvalósítási példa cikkek.
* **Adja hozzá egy új App ID URI** az alkalmazáshoz (identifierURIs[]). App ID URI-k használata toouniquely azonosítása során az Azure AD-bérlő (vagy több Azure AD-bérlőre, több-bérlős forgatókönyvek minősített ellenőrzött egyéni tartomány keresztül). Engedélyek tooa erőforrás alkalmazás kérésekor használhatók vagy az beszerzése egy hozzáférési jogkivonatot egy erőforrás-alkalmazáshoz. Ez az elem frissítésekor hello ugyanazt a frissítést legyen hello alkalmazás otthoni bérlői él toohello megfelelő szolgáltatás egyszerű servicePrincipalNames [] gyűjtemény.

hello alkalmazásjegyzék is biztosít egy jó módszer tootrack hello az alkalmazás regisztrációs állapotát. Mivel JSON formátumban, hello fájl megjelenítése a verziókövetési, valamint az alkalmazás forráskódjához való ellenőrizhető.

## <a name="step-by-step-example"></a>Lépés példa alapján lépés
Most már megadható, hogy az alkalmazás identitását konfigurációs hello alkalmazásjegyzék keresztül végezze el hello lépéseket szükséges tooupdate. Ki vannak emelve példák, megelőző hello egyik jelenít meg, hogyan toodeclare új engedély hatókör egy erőforrás-kérelem:

1. Jelentkezzen be toohello [Azure-portálon][AZURE-PORTAL].
2. Ön már hitelesítése után válassza ki az Azure AD-bérlő való hello jobb felső sarkában hello oldal kiválasztása.
3. Válassza ki **Azure Active Directory** kiterjesztést a bal oldali navigációs panelen, majd kattintson a hello **App regisztrációk**.
4. Szeretné, hogy tooupdate hello listában, és kattintson rá hello alkalmazás található.
5. Hello alkalmazás lapon kattintson **Manifest** tooopen hello beágyazott jegyzék szerkesztő. 
6. Közvetlenül szerkesztheti a szerkesztő hello jegyzéket. Vegye figyelembe, hogy hello jegyzékfájl követi hello hello sémája [alkalmazás entitás] [ APPLICATION-ENTITY] azt a korábban említett: például "Employees.Read.All" feltéve, hogy azt szeretnénk, ha tooimplement/elérhetővé tétele egy új engedély neve az erőforrás alkalmazás (API) akkor egyszerűen hozzáadhat egy új/másodperc elem toohello oauth2Permissions gyűjtemény ie:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow hello application tooaccess MyWebApplication on behalf of hello signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow hello application tooaccess MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow hello application toohave read-only access tooall Employee data.",
        "adminConsentDisplayName": "Read-only access tooEmployee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow hello application toohave read-only access tooyour Employee data.",
        "userConsentDisplayName": "Read-only access tooyour Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    hello bejegyzésnek egyedinek kell lennie, és ezért létre kell hoznia egy új globálisan egyedi Azonosítót (GUID) hello `"id"` tulajdonság. Ebben az esetben mivel azt a megadott `"type": "User"`, ezzel az engedéllyel is kell hozzájárult tooby hello által hitelesített bármely olyan fiók, mely hello erőforrás/API-alkalmazás van regisztrálva az Azure AD-bérlő. Ez biztosít hello ügyfél alkalmazás engedély tooaccess azt hello fiók nevében. hello leírását és a megjelenített karakterláncok során hozzájárulási és hello Azure-portálon megjelenítendő szolgálnak.
6. Ha befejezte a hello jegyzékfájl frissítése, kattintson a **mentése** toosave hello jegyzékben.  
   
Most, hogy hello jegyzékfájl mentése regisztrált ügyfél alkalmazás hozzáférés toohello új engedélyt adhat azt fent. Ezúttal használható hello hello ügyfél alkalmazás jegyzékfájlja szerkesztése helyett az Azure portál webes felhasználói felületén:  

1. Nyissa meg a toohello **beállítások** panelen található hello ügyfél alkalmazás toowhich kívánja tooadd hozzáférés toohello új API-t, kattintson a **szükséges engedélyek** válassza **API kiválasztása** .
2. Majd választhat hello bérlő regisztrált erőforrás-alkalmazások (API-k) hello listáját. Kattintson a hello erőforrás alkalmazás tooselect, vagy az alkalmazás hello keresőmezőbe hello hello nevét. Ha hello alkalmazás talált, kattintson **válasszon**.  
3. A rendszer ekkor toohello **Select engedélyeket** lap, amely Alkalmazásengedélyek és hello erőforrás alkalmazáshoz elérhető delegált engedélyek hello listája. Válassza ki a rendelés tooadd új engedély hello azt toohello ügyfél által kért az engedélyek listájához. Ez az új engedély hello ügyfél identitás alkalmazást, a hello "requiredResourceAccess" gyűjteménytulajdonság tárolódnak.


Ennyi az egész. Most már az alkalmazásokat az új identitás-konfigurációt használni fog futni.

## <a name="next-steps"></a>Következő lépések
* Az alkalmazás alkalmazás- és szolgáltatásnevet objektumok hello kapcsolatát a további részletekért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure AD][AAD-APP-OBJECTS].
* Lásd: hello [az Azure AD fejlesztői szószedet] [ AAD-DEVELOPER-GLOSSARY] néhány hello core Azure Active Directory (AD) fejlesztői fogalmak definícióját.

Hello Megjegyzések szakaszban tooprovide visszajelzést alább, és segítsen pontosítsa és a tartalom.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/


---
title: "Az Azure Active Directory Alkalmazásjegyzékének megismerése |} Microsoft Docs"
description: "Az Azure Active Directory alkalmazásjegyzék, amely jelöli az identitás alkalmazást az Azure AD-bérlő alkalmaz, és lehetővé teszi az OAuth hitelesítési, hozzájárulási élmény és további részletes körét."
services: active-directory
documentationcenter: 
author: sureshja
manager: mtillman
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
ms.openlocfilehash: f3284d4cbb15f21522549c678410815b54344744
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-application-manifest"></a>Az Azure Active Directory alkalmazásjegyzékének
Alkalmazásokat, amelyekbe beépül az Azure AD az Azure AD-bérlő regisztrálva kell lennie. Ez az alkalmazás az alkalmazás-jegyzékfájl (alatt az Azure AD panel) használatával konfigurálható a [Azure-portálon](https://portal.azure.com).

## <a name="manifest-reference"></a>Hivatkozás manifest

>[!div class="mx-tdBreakAll"]
>[!div class="mx-tdCol2BreakAll"]
|Kulcs  |Érték típusa |Példaérték  |Leírás  |
|---------|---------|---------|---------|
|Alkalmazásazonosító     |  Azonosító karakterlánca       |""|  Az alkalmazás-alkalmazásokhoz az Azure AD által hozzárendelt egyedi azonosítója.|
|appRoles     |    Tömb típusa     |[{<br>&emsp;"allowedMemberTypes": [<br>&emsp;&nbsp;&nbsp;&nbsp;"User"<br>&emsp;],<br>&emsp;"description": "Olvasás csak való hozzáférés eszközadatokat",<br>&emsp;"displayName": "Csak Olvasás",<br>&emsp;"id": guid,<br>&emsp;"isEnabled": igaz értéke esetén<br>&emsp;"a érték": "ReadOnly"<br>}]|Szerepkörök, amelyek az alkalmazás nyilváníthat gyűjteménye. Ezek a szerepkörök a felhasználók, csoportok vagy szolgáltatásnevekről rendelhetők.|
|AvailableToOtherTenants|Logikai érték|igaz|Ha ez a beállítás értéke igaz, az alkalmazás áll rendelkezésre más bérlők számára.  Ha FALSE értéke esetén az alkalmazás csak a bérlő mavenen van regisztrálva.  További információ:: [bármely Azure Active Directory (AD) felhasználó a több-bérlős alkalmazás minta használatával bejelentkezés](active-directory-devhowto-multi-tenant-overview.md). |
|displayName     |karakterlánc         |MyRegisteredApp         |A megjelenítési nevet az alkalmazásnak. |
|errorURL     |karakterlánc         |http:<i></i>//MyRegisteredAppError         |Hiba történt egy alkalmazás URL-CÍMÉT. |
|GroupMembershipClaims     |    karakterlánc     |    1     |   Bitmaszk, amely a kibocsátott "csoportok" jogcím egy felhasználó vagy az OAuth 2.0 hozzáférési jogkivonatot, amely az alkalmazás vár konfigurálja. A bitmaszk értékei: 0: None, 1: biztonsági csoportok és szerepkörök az Azure AD, 2: fenntartott, 4: foglalt. A bitmaszk beállítás 7 összes a biztonsági csoportok, a terjesztési csoportok és az Azure Active directory szerepkörök, amely a bejelentkezett felhasználó tagja megkapja.      |
|optionalClaims     |  karakterlánc       |     null    |    A jogkivonat a biztonsági jogkivonat-szolgáltatás az adott alkalmazás által visszaadott választható jogcímeket.     |
|acceptMappedClaims    |      Logikai érték   | igaz        |    Ha a beállítás értéke TRUE, lehetővé teszi az alkalmazás-leképezés jogcímek egy egyéni aláírási kulcs megadása nélkül.|
|Kezdőlap     |  karakterlánc       |http:<i></i>//MyRegistererdApp         |    Az alkalmazás kezdőlapját URL-CÍMÉT.     |
|identifierUris     |  Karakterlánc-tömbben       | http:<i></i>//MyRegistererdApp        |   Felhasználó által definiált URI(s), amely egyedileg azonosítja az egy webalkalmazást az Azure AD-bérlő vagy egyéni ellenőrzött tartományt, ha az alkalmazás több-bérlős.      |
|keyCredentials     |   Tömb típusa      |   [{<br>&nbsp;"customKeyIdentifier": null,<br>"endDate": "2018-09-13T00:00:00Z",<br>"keyId": "\<guid >",<br>"startDate": "2017-09-12T00:00:00Z",<br>"type": "AsymmetricX509Cert"<br>"használati": "Hitelesítés"<br>"a érték": NULL értékű<br>}]      |   Ez a tulajdonság tárolja a hitelesítő alkalmazás által hozzárendelt, a közös titkokat karakterlánc-alapú és az X.509-tanúsítványokat.  Ezek a hitelesítő adatok kerülnek play hozzáférési jogkivonatok kérésekor (ha az alkalmazás az ügyfél nevében jár el egy inkább, mint a forrás).     |
|knownClientApplications     |     Tömb típusa    |    [guid]     |     Az érték kötegelése szerezni a hozzájárulásukat szolgál, ha olyan megoldás, amely két részből áll, egy ügyfél-alkalmazás és egy egyéni webes API-alkalmazás tartalmazza. Az ügyfélalkalmazás appID ezt az értéket ad meg, ha a felhasználó csak kell egyszer hozzájárul az ügyfélalkalmazás. Megtudhatja, hogy az Azure AD, hogy hozzájárul ahhoz, hogy az ügyfél azt jelenti, amely implicit módon hozzájárul ahhoz, hogy a webes API-t, és automatikusan kiállít szolgáltatásnevekről az ügyfél és a webes API-t (az ügyfél és a webes API-alkalmazást regisztrálni kell az azonos egyszerre Bérlői).|
|logoutUrl     |   karakterlánc      |     http:<i></i>//MyRegisteredAppLogout    |   Jelentkezzen ki az alkalmazás URL-CÍMÉT.      |
|oauth2AllowImplicitFlow     |   Logikai érték      |  hamis       |       Meghatározza, hogy a webes alkalmazás kérhet OAuth2.0 implicit engedélyezési folyamat jogkivonatokat. Az alapértelmezett értéke false. Böngészőalapú alkalmazások, például Javascript egylapos alkalmazások szolgál. |
|oauth2AllowUrlPathMatching     |   Logikai érték      |  hamis       |   Megadja, hogy, az OAuth 2.0 token kérelmek részeként, az Azure AD engedélyezi az átirányítási URI-t az alkalmazás replyUrls szemben a megfelelő elérési útja. Az alapértelmezett értéke false.      |
|oauth2Permissions     | Tömb típusa         |      [{<br>"adminConsentDescription": "Engedélyezéséhez az alkalmazás a bejelentkezett felhasználó nevében erőforrások eléréséhez."<br>"adminConsentDisplayName": "Hozzáférési resource1",<br>"id": "\<guid >",<br>"isEnabled": igaz értéke esetén<br>"type": "User"<br>"userConsentDescription": "Engedélyezéséhez az alkalmazás az Ön nevében resource1 eléréséhez."<br>"userConsentDisplayName": "Hozzáférési resources",<br>"a érték": "user_impersonation"<br>}]   |  Az OAuth 2.0-engedélyhatókörök, amely a webes API (erőforrás) alkalmazás ügyfélalkalmazások számára elérhetővé teszi gyűjteménye. Ezek engedélyhatókörök hozzájárulási során adható ügyfélalkalmazások számára. |
|oauth2RequiredPostResponse     | Logikai érték        |    hamis     |      Megadja, hogy, az OAuth 2.0 token kérelmek részeként, az Azure AD engedélyezi POST-kérésnél az GET kérelmek szemben. Az alapértelmezett értéke false, amely megadja, hogy csak a GET-kérésekhez engedélyezett lesz.   
|Objektumazonosító     | Azonosító karakterlánca        |     ""    |    A könyvtárban az alkalmazás egyedi azonosítója.  Ez az azonosítója alapján határozza meg az alkalmazás minden protokoll tranzakcióban.  Felhasználói feladata a hivatkozik az objektumra directory lekérdezésekben.|
|passwordCredentials     | Tömb típusa        |   [{<br>"customKeyIdentifier": null,<br>"endDate": "2018-10-19T17:59:59.6521653Z",<br>"keyId": "\<guid >",<br>"startDate": "2016-10-19T17:59:59.6521653Z",<br>"a érték": NULL értékű<br>}]      |    Tekintse meg a keyCredentials tulajdonság leírását.     |
|PublicClient     |  Logikai érték       |      hamis   | Megadja, hogy egy alkalmazás egy nyilvános ügyfél (például egy telepített alkalmazás egy mobileszközön fut). Alapértelmezett értéke false.        |
|supportsConvergence     |  Logikai érték       |   hamis      | Ez a tulajdonság nem lehet szerkeszteni.  Fogadja el az alapértelmezett értéket.        |
|replyUrls     |  Karakterlánc-tömbben       |   http:<i></i>//localhost     |  Ez a többértékű tulajdonság tárolja, amely az Azure AD elfogadja, célként regisztrált redirect_uri értékek listájának amikor returining jogkivonatokat. |
|RequiredResourceAccess     |     Tömb típusa    |    [{<br>"resourceAppId": "00000002-0000-0000-c000-000000000000"<br>"resourceAccess": [{<br>&nbsp;&nbsp;&nbsp;&nbsp;"id": "311a71cc-e848-46a1-bdf8-97ff7156d8e6"<br>&nbsp;&nbsp;&nbsp;&nbsp;"type": "Hatókör"<br>&nbsp;&nbsp;}]<br>}]     |   Adja meg az ehhez az alkalmazáshoz való hozzáférés és az OAuth-engedélyhatókörök és alkalmazás-szerepkörök, az egyes erőforrások szükséges erőforrásokat. A szükséges erőforrások elérése előtti konfigurációját a hozzájárulási élmény meghajtók.|
|resourceAppId     |    Azonosító karakterlánca     |  ""      |   Az erőforrás, amely kell elérnie az alkalmazás egyedi azonosítója. Ez az érték a célalkalmazás erőforrás deklarált appId egyenlőnek kell lennie.     |
|resourceAccess     |  Tömb típusa       | Lásd: a példában a requiredResourceAccess tulajdonság értékét.        |   OAuth2.0 engedélyhatókörök és alkalmazás-szerepkörök, az alkalmazás által a megadott erőforrás (azonosító és a típus értékeket a megadott erőforrások tartalmazza)        |
|samlMetadataUrl|karakterlánc|http:<i></i>//MyRegisteredAppSAMLMetadata|Az alkalmazás SAML metaadatainak URL-CÍMÉT.| 

## <a name="next-steps"></a>Következő lépések
* Az alkalmazás alkalmazás- és szolgáltatásnevet objektumok közötti kapcsolatot további információkért lásd: [alkalmazás és szolgáltatás egyszerű objektumok az Azure AD][AAD-APP-OBJECTS].
* Tekintse meg a [az Azure AD fejlesztői szószedet] [ AAD-DEVELOPER-GLOSSARY] bizonyos Azure Active Directory (AD) fejlesztői alapfogalmakat-definíciók.

Az alábbi Megjegyzések szakasz segítségével visszajelzést, amellyel pontosíthatja és a tartalom.

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


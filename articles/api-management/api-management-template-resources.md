---
title: "az API Management sablonerőforrás aaaAzure |} Microsoft Docs"
description: "További információk a hello típusú erőforrás fejlesztői portál sablonok az Azure API Management használható."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 51a1b4c6-a9fd-4524-9e0e-03a9800c3e94
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 2221e3852986d485d13817b483e473dfe451d3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-template-resources"></a>Azure API Management-sablon erőforrások
Az Azure API Management a következő típusú hello developer portálon sablonokban használható erőforrás hello biztosít.  
  
-   [Karakterlánc-erőforrások](#strings)  
  
-   [A betűkép-erőforrások](#glyphs)  
  
##  <a name="strings"></a>Karakterlánc-erőforrások  
 Az API Management széles választékát erőforrásait hello developer portálon számára biztosít. Ezeket az erőforrásokat az API Management által támogatott hello nyelvek mindegyikét honosítva vannak. sablonok alapértelmezett készletét hello oldalfejlécben, a címkéket és az állandó karakterláncokkal hello developer portálon megjelenő ezeket az erőforrásokat használ. a sablonokat, a karakterlánc erőforrás toouse hello erőforrás karakterlánc előtagja hello karakterlánc neve, majd adja meg, ahogy az alábbi példa hello.  
  
```  
{% localized "Prefix|Name" %}  
  
```  
  
 hello alábbi példa hello termék lista sablonból, és megjeleníti **termékek** hello oldal hello tetején.  
  
```  
<h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>  
  
```  
  
 Tekintse meg a következő táblák hello karakterlánc erőforrások developer-portál sablonok használható toohello. Hello táblanév használják hello előtag a tábla hello erőforrásait.  
  
-   [ApisStrings](#ApisStrings)  
  
-   [ApplicationListStrings](#ApplicationListStrings)  
  
-   [AppDetailsStrings](#AppDetailsStrings)  
  
-   [AppStrings](#AppStrings)  
  
-   [CommonResources](#CommonResources)  
  
-   [CommonStrings](#CommonStrings)  
  
-   [Dokumentáció](#Documentation)  
  
-   [ErrorPageStrings](#ErrorPageStrings)  
  
-   [IssuesStrings](#IssuesStrings)  
  
-   [NotFoundStrings](#NotFoundStrings)  
  
-   [ProductDetailsStrings](#ProductDetailsStrings)  
  
-   [ProductsStrings](#ProductsStrings)  
  
-   [ProviderInfoStrings](#ProviderInfoStrings)  
  
-   [SigninResources](#SigninResources)  
  
-   [SigninStrings](#SigninStrings)  
  
-   [SignupStrings](#SignupStrings)  
  
-   [SubscriptionListStrings](#SubscriptionListStrings)  
  
-   [SubscriptionStrings](#SubscriptionStrings)  
  
-   [UpdateProfileStrings](#UpdateProfileStrings)  
  
-   [Felhasználói profil](#UserProfile)  
  
###  <a name="ApisStrings"></a>ApisStrings  
  
|Név|Szöveg|  
|----------|----------|  
|PageTitleApis|API-k|  
  
###  <a name="AppDetailsStrings"></a>AppDetailsStrings  
  
|Név|Szöveg|  
|----------|----------|  
|WebApplicationsDetailsTitle|Alkalmazás megtekintése|  
|WebApplicationsRequirementsHeader|Követelmények|  
|WebApplicationsScreenshotAlt|Képernyőfelvétel|  
|WebApplicationsScreenshotsHeader|Képernyőfelvételek|  
  
###  <a name="ApplicationListStrings"></a>ApplicationListStrings  
  
|Név|Szöveg|  
|----------|----------|  
|WebDevelopersAppDeleteConfirmation|Biztos, hogy szeretné-e tooremove alkalmazás?|  
|WebDevelopersAppNotPublished|Nincs közzétéve|  
|WebDevelopersAppNotSubminted|Nem elküldve|  
|WebDevelopersAppTableCategoryHeader|Kategória|  
|WebDevelopersAppTableNameHeader|Név|  
|WebDevelopersAppTableStateHeader|Állapot|  
|WebDevelopersEditLink|Szerkesztés|  
|WebDevelopersRegisterAppLink|Alkalmazás regisztrálása|  
|WebDevelopersRemoveLink|Eltávolítás|  
|WebDevelopersSubmitLink|Küldés|  
|WebDevelopersYourApplicationsHeader|Az alkalmazások|  
  
###  <a name="AppStrings"></a>AppStrings  
  
|Név|Szöveg|  
|----------|----------|  
|WebApplicationsHeader|Alkalmazások|  
  
###  <a name="CommonResources"></a>CommonResources  
  
|Név|Szöveg|  
|----------|----------|  
|NoItemsToDisplay|Nincs eredmény.|  
|GeneralExceptionMessage|Hiba található nem megfelelő. Ideiglenes működési hiba vagy egy hiba lehet. Kérjük próbálkozzon újra.|  
|GeneralJsonExceptionMessage|Hiba található nem megfelelő. Ideiglenes működési hiba vagy egy hiba lehet. Kérjük hello oldal újratöltéséhez, és próbálkozzon újra.|  
|ConfirmationMessageUnsavedChanges|Egyes nem mentett módosítások vannak. Biztos, hogy szeretné, hogy toocancel, és elveti a hello?|  
|AzureActiveDirectory|Azure Active Directory|  
|HttpLargeRequestMessage|HTTP kérelem törzse túl nagy.|  
  
###  <a name="CommonStrings"></a>CommonStrings  
  
|Név|Szöveg|  
|----------|----------|  
|ButtonLabelCancel|Mégse|  
|ButtonLabelSave|Mentés|  
|GeneralExceptionMessage|Hiba található nem megfelelő. Ideiglenes működési hiba vagy egy hiba lehet. Kérjük próbálkozzon újra.|  
|NoItemsToDisplay|Nincsenek nem elemek toodisplay.|  
|PagerButtonLabelFirst|Első|  
|PagerButtonLabelLast|utolsó|  
|PagerButtonLabelNext|Következő lépés|  
|PagerButtonLabelPrevious|Előző|  
|PagerLabelPageNOfM|{0} lap {1}|  
|PasswordTooShort|hello jelszó túl rövid|  
|EmailAsPassword|Ne használja az e-maileket a jelszót|  
|PasswordSameAsUserName|A jelszó nem tartalmazhatja a felhasználónevet|  
|PasswordTwoCharacterClasses|Használjon különböző karakterkészletnek osztályok|  
|PasswordTooManyRepetitions|Túl sok ismétlődését|  
|PasswordSequenceFound|A jelszó karaktersorozatokat tartalmaz|  
|PagerLabelPageSize|Oldalméret|  
|CurtainLabelLoading|Betöltése...|  
|TablePlaceholderNothingToDisplay|Nincs adat hello kijelölt időszak és hatókör|  
|ButtonLabelClose|Bezárás|  
  
###  <a name="Documentation"></a>Dokumentáció  
  
|Név|Szöveg|  
|----------|----------|  
|WebDocumentationInvalidHeaderErrorMessage|Érvénytelen fejléc "{0}"|  
|WebDocumentationInvalidRequestErrorMessage|Érvénytelen a kérelem URL-címe|  
|TextboxLabelAccessToken|Hozzáférési jogkivonat *|  
|DropdownOptionPrimaryKeyFormat|Elsődleges – {0}|  
|DropdownOptionSecondaryKeyFormat|Másodlagos-{0}|  
|WebDocumentationSubscriptionKeyText|Az Előfizetés kulcs|  
|WebDocumentationTemplatesAddHeaders|Kötelező HTTP-fejlécek hozzáadása|  
|WebDocumentationTemplatesBasicAuthSample|Alapszintű engedélyezési minta|  
|WebDocumentationTemplatesCurlForBasicAuth|Alapszintű engedélyezési használatra:--{username} felhasználó: {jelszó}|  
|WebDocumentationTemplatesCurlValuesForPath|Elérési út paraméterek ({...} útvonalként megjelenő), az Előfizetés kulcs és a lekérdezési paraméterek értékeit értékek megadása|  
|WebDocumentationTemplatesDeveloperKey|Adja meg az Előfizetés kulcs|  
|WebDocumentationTemplatesJavaApache|Ezt a mintát használ hello Apache HTTP-ügyfél a HTTP-összetevők (http://hc.apache.org/httpcomponents-client-ga/)|  
|WebDocumentationTemplatesOptionalParams|Adja meg a nem kötelező paraméter értékét, igény szerint|  
|WebDocumentationTemplatesPhpPackage|Ez a minta hello HTTP_Request2 csomagot fogja használni. (További információ: http://pear.php.net/package/HTTP_Request2)|  
|WebDocumentationTemplatesPythonValuesForPath|Adja meg az elérési út paraméterek ({...} útvonalként megjelenő) értékét, és a kérelem törzsében, szükség esetén|  
|WebDocumentationTemplatesRequestBody|Adja meg a kérelem törzsében|  
|WebDocumentationTemplatesRequiredParams|Adható meg érték a következő paramétereket hello|  
|WebDocumentationTemplatesValuesForPath|Adja meg az elérési út paraméterek ({...} útvonalként megjelenő) értékét|  
|OAuth2AuthorizationEndpointDescription|hello engedélyezési végpont használt toointeract hello erőforrás tulajdonos, és egy hitelesítésengedélyezési beszerzése.|  
|OAuth2AuthorizationEndpointName|engedélyezési végpont|  
|OAuth2TokenEndpointDescription|hello jogkivonat végpontjához használt szerint hello ügyfél tooobtain egy hozzáférési jogkivonat segítségével az engedélyt biztosít, vagy frissítse a jogkivonatot.|  
|OAuth2TokenEndpointName|token-végpont|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Description|< p\> hello ügyfél kezdeményez hello folyamata során irányítja hello erőforrás tulajdonosa felhasználói ügynök toohello engedélyezési végpont.  hello ügyfél tartalmazza az ügyfél-azonosítókra, a megadott hatókörben, a helyi állapota, és egy átirányítási URI toowhich hello engedélyezési kiszolgálóra küldi hello felhasználói ügynök, vissza, ha hozzáférést (engedélyezett vagy tiltott).     < /p\> < p\> hello engedélyezési kiszolgáló hitelesíti a hello erőforrás tulajdonosa (keresztül hello felhasználói ügynök), és hogy hello erőforrás tulajdonosa engedélyezi vagy megtagadja a hello ügyfél kérést hoz létre.     < /p\> < p\> feltételezve hello erőforrás tulajdonosa engedélyezi a hozzáférést, hello engedélyezési kiszolgáló átirányítja a felhasználókat hello felhasználói ügynök hátsó toohello ügyfél hello átirányítással megadott URI-Azonosítót (hello kérelem vagy a közötti tartalommegosztás korábbi t regisztráció).  hello átirányítási URI Azonosítót tartalmazza az engedélyezési kód és bármely helyi hello ügyfél által korábban meghatározott.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ErrorDescription|< p\> hello felhasználó megtagadja a hello hozzáférés kérésére, ha hello vonatkozó kérés érvénytelen, ha hello ügyfél értesítést kap a következő paraméterek hozzáadta a toohello átirányítási hello használata: < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_Name|Hitelesítési kérelem|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_RequestDescription|< p\> hello ügyfélalkalmazás hello felhasználói toohello engedélyezési végpont kell küldenie a rendelés tooinitiate hello OAuth folyamat.          Hello engedélyezési végpont hello felhasználó hitelesíti, és majd engedélyezi, vagy megtagadja a hozzáférést toohello alkalmazást.     < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_AuthorizationRequest_ResponseDescription|< p\> feltételezve hello erőforrás tulajdonosa engedélyezi a hozzáférést, engedélyezési kiszolgáló átirányítja a felhasználókat hello felhasználói ügynök hátsó toohello ügyfél hello átirányítással megadott URI-Azonosítót korábbi (hello kérelem vagy ügyfél-regisztrációk alatt).  hello átirányítási URI Azonosítót tartalmazza az engedélyezési kód és bármely helyi hello ügyfél által korábban meghatározott. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_Description|< p\> hello ügyfél olyan hozzáférési jogkivonatot kér hello engedélyezési kiszolgálótól "s token-végpont hello előző lépésben kapott hello engedélyezési kód-ot.  Amikor hello kérést hello ügyfél hello engedélyezési kiszolgáló hitelesíti.  hello ügyfél hello átirányítási URI használt tooobtain hello engedélyezési kód az ellenőrzéshez tartalmazza. < /p\> < p\> hello engedélyezési kiszolgáló hitelesíti a hello ügyfél, hello engedélyezési kódot, és biztosítja, hogy hello átirányítási URI megegyezik hello URI használt tooredirect hello ügyfél kapott (C) lépésben.  Érvényes, ha hello engedélyezési kiszolgáló válaszol, vissza olyan hozzáférési jogkivonatot, és opcionálisan egy frissítési jogkivonat. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ErrorDescription|< p\> hello kérelem ügyfél-hitelesítés sikertelen volt, vagy nem érvényes, ha hello engedélyezési kiszolgáló válaszol a HTTP 400 (hibás kérés) állapotkóddal (kivéve, ha másként), és a következő paraméterek hello válasz hello tartalmazza. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_RequestDescription|< p\> hello ügyfél a következő paraméterek hello "application/x-www-form-urlencoded" formátum használata egy karakter kódolás az UTF-8 a hello HTTP-kérelem-entitástörzsében hello elküldésével lehetővé teszi a kérelem toohello jogkivonat végpontjához. < /p\>|  
|OAuth2Flow_AuthorizationCodeGrant_Step_TokenRequest_ResponseDescription|< p\> hello engedélyezési kiszolgáló kiad egy hozzáférési jogkivonat és opcionális frissítési jogkivonat, és szerkezetek hello válasz a következő paraméterek-toohello-entitástörzsében hello HTTP-válaszok 200 (OK) állapotkóddal hello hozzáadásával. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_Description|< p\> hello ügyfél hello engedélyezési kiszolgáló hitelesíti, és egy hozzáférési jogkivonatot kér hello jogkivonat végpontjához. < /p\> < p\> hello engedélyezési kiszolgáló hello ügyfél hitelesíti, és érvényes, ha egy jogkivonatot állít ki. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> Ha hello kérelem sikertelen volt az ügyfél-hitelesítéshez, vagy érvénytelen hello engedélyezési kiszolgáló válaszol a HTTP 400 (hibás kérés) állapotkóddal (kivéve, ha másként), és a következő paraméterek hello válasz hello tartalmazza. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> hello ügyfél a következő paraméterek hello "application/x-www-form-urlencoded" formátum használata egy karakter kódolás az UTF-8 a hello HTTP-kérelem-entitástörzsében hello hozzáadásával lehetővé teszi egy kérelem toohello jogkivonat végpontjához. < /p\>|  
|OAuth2Flow_ClientCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> Ha hello hozzáférési token kérelme, mert az érvényes és meghatalmazott hello engedélyezési kiszolgáló kiad egy hozzáférési jogkivonat és opcionális frissítési jogkivonat és szerkezetek hello válasz a következő paraméterek toohello-törzsében hello HTTP hello hozzáadásával válasz 200 (OK) állapotkóddal. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_Description|< p\> hello ügyfél kezdeményez hello folyamata során irányítja hello erőforrás tulajdonosa "s felhasználói ügynök toohello engedélyezési végpont.  hello ügyfél tartalmazza az ügyfél-azonosítókra, a megadott hatókörben, a helyi állapota, és egy átirányítási URI toowhich hello engedélyezési kiszolgálóra küldi hello felhasználói ügynök, vissza, ha hozzáférést (engedélyezett vagy tiltott). < /p\> < p\> hello engedélyezési kiszolgáló hitelesíti a hello erőforrás tulajdonosa (keresztül hello felhasználói ügynök), és megállapítja, hogy hello erőforrás tulajdonosa engedélyezi vagy megtagadja a hello ügyfél "s hozzáférési kérelem. < /p\> < p\> feltételezve hello erőforrás tulajdonosa engedélyezi a hozzáférést, hello engedélyezési kiszolgáló átirányítja a felhasználókat hello felhasználói ügynök hátsó toohello ügyfél hello átirányítási URI korábban megadott használatával.  hello átirányítási URI hello URI-töredéket hello hozzáférési jogkivonatot tartalmazza. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ErrorDescription|< p\> Ha hello erőforrás tulajdonosa megtagadja hello hozzáférési kérelem, vagy ha hello kérelem sikertelen, ennek oka egy hiányzó vagy érvénytelen átirányítási URI Azonosítót nem, hello engedélyezési tájékoztassa hello ügyfél adja hozzá a következő paraméterek toohello fragme hello NT összetevője hello átirányítási URI használatával hello "application/x-www-form-urlencoded" formátumban. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_RequestDescription|< p\> hello ügyfélalkalmazás hello felhasználói toohello engedélyezési végpont kell küldenie a rendelés tooinitiate hello OAuth folyamat.      Hello engedélyezési végpont hello felhasználó hitelesíti, és majd engedélyezi, vagy megtagadja a hozzáférést toohello alkalmazást. < /p\>|  
|OAuth2Flow_ImplicitGrant_Step_AuthorizationRequest_ResponseDescription|< p\> hello erőforrás tulajdonos ruházza fel hello kérést, ha hello engedélyezési kiszolgáló egy jogkivonatot állít ki, és adja hozzá a következő paraméterek toohello fragment összetevőt hello átirányítási URI Azonosítót a hello toohello ügyfél továbbítja hello használata "a alkalmazásobjektum/x-www-form-urlencoded"formátumban. < /p\>|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Description|Hitelesítésikód-folyamata hello bizalmas kezelése mellett, a hitelesítő adataikat (például a webkiszolgáló alkalmazások megvalósították a PHP, Java, Python, Ruby, ASP.NET, stb.) fenntartásának alkalmas ügyfelek van optimalizálva.|  
|OAuth2Flow_ObtainAuthorization_AuthorizationCodeGrant_Name|Kód hitelesítésengedélyezési|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Description|Ügyfél-hitelesítő adatok folyamata megfelelő azokban az esetekben, ahol hello ügyfél (az alkalmazás) kér az ellenőrzése alatt toohello védett erőforrások eléréséhez. hello ügyfél erőforrás tulajdonosa, minősül, ezért nem végfelhasználói beavatkozásra szükség.|  
|OAuth2Flow_ObtainAuthorization_ClientCredentialsGrant_Name|Ügyfél-hitelesítő adatok megadása|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Description|Implicit engedélyezési folyamat hello bizalmas kezelése mellett, a hitelesítő adatok ismert toooperate egy adott átirányítási URI Azonosítót fenntartásának nem alkalmas ügyfelek van optimalizálva. Ezek az ügyfelek általában valósíthatók meg egy böngészőt, például a JavaScriptek programozási nyelv használatával.|  
|OAuth2Flow_ObtainAuthorization_ImplicitGrant_Name|Implicit támogatás|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Description|Erőforrás tulajdonosa jelszó hitelesítő adatok folyamata megfelelő azokban az esetekben ahol hello erőforrás tulajdonosa megbízhatósági kapcsolat hello ügyfél (az alkalmazás), például a hello eszköz-operációsrendszerek vagy egy magas szintű jogosultsággal rendelkező alkalmazást. Ez a folyamat alkalmas képes megszerezni hello erőforrás tulajdonos hitelesítő adatait (felhasználónévvel és jelszóval, általában az interaktív űrlap használatával).|  
|OAuth2Flow_ObtainAuthorization_ResourceOwnerPasswordCredentialsGrant_Name|Erőforrás tulajdonosa jelszó hitelesítő adatok megadása|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_Description|< p\> hello erőforrás tulajdonosa hello ügyfél biztosítja a felhasználónévvel és jelszóval. < /p\> < p\> hello ügyfél olyan hozzáférési jogkivonatot kér hello engedélyezési kiszolgálótól "s token-végpont hello erőforrás tulajdonosa kapott hello hitelesítő adatokkal együtt.  Amikor hello kérést hello ügyfél hello engedélyezési kiszolgáló hitelesíti. < /p\> < p\> hello engedélyezési kiszolgáló hello ügyfél hitelesíti, és érvényesíti a hello erőforrás tulajdonos hitelesítő adatait, és érvényes, ha egy jogkivonatot állít ki. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ErrorDescription|< p\> Ha hello kérelem sikertelen volt az ügyfél-hitelesítéshez, vagy érvénytelen hello engedélyezési kiszolgáló válaszol a HTTP 400 (hibás kérés) állapotkóddal (kivéve, ha másként), és a következő paraméterek hello válasz hello tartalmazza. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_RequestDescription|< p\> hello ügyfél a következő paraméterek hello "application/x-www-form-urlencoded" formátum használata egy karakter kódolás az UTF-8 a hello HTTP-kérelem-entitástörzsében hello hozzáadásával lehetővé teszi egy kérelem toohello jogkivonat végpontjához. < /p\>|  
|OAuth2Flow_ResourceOwnerPasswordCredentialsGrant_Step_TokenRequest_ResponseDescription|< p\> Ha hello hozzáférési token kérelme, mert az érvényes és meghatalmazott hello engedélyezési kiszolgáló kiad egy hozzáférési jogkivonat és opcionális frissítési jogkivonat és szerkezetek hello válasz a következő paraméterek toohello-törzsében hello HTTP respo hello hozzáadásával nse 200 (OK) állapotkóddal. < /p\>|  
|OAuth2Step_AccessTokenRequest_Name|Hozzáférési jogkivonat kérelem|  
|OAuth2Step_AuthorizationRequest_Name|Hitelesítési kérelem|  
|OAuth2AccessToken_AuthorizationCodeGrant_TokenResponse|SZÜKSÉGES. hello hozzáférési jogkivonat hello engedélyezési kiszolgáló bocsátott ki.|  
|OAuth2AccessToken_ClientCredentialsGrant_TokenResponse|SZÜKSÉGES. hello hozzáférési jogkivonat hello engedélyezési kiszolgáló bocsátott ki.|  
|OAuth2AccessToken_ImplicitGrant_AuthorizationResponse|SZÜKSÉGES. hello hozzáférési jogkivonat hello engedélyezési kiszolgáló bocsátott ki.|  
|OAuth2AccessToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|SZÜKSÉGES. hello hozzáférési jogkivonat hello engedélyezési kiszolgáló bocsátott ki.|  
|OAuth2ClientId_AuthorizationCodeGrant_AuthorizationRequest|SZÜKSÉGES. Ügyfél-azonosító.|  
|OAuth2ClientId_AuthorizationCodeGrant_TokenRequest|Ha hello ügyfél nem hitelesíti hello engedélyezési Server szükséges.|  
|OAuth2ClientId_ImplicitGrant_AuthorizationRequest|SZÜKSÉGES. ügyfél-azonosítókra hello.|  
|OAuth2Code_AuthorizationCodeGrant_AuthorizationResponse|SZÜKSÉGES. hello engedélyezési kód engedélyezési kiszolgáló hello által generált.|  
|OAuth2Code_AuthorizationCodeGrant_TokenRequest|SZÜKSÉGES. hello engedélyezési kód engedélyezési hello kiszolgálótól kapott.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_AuthorizationErrorResponse|NEM KÖTELEZŐ. Emberek számára olvasható ASCII szöveges kezeléséről további információt.|  
|OAuth2ErrorDescription_AuthorizationCodeGrant_TokenErrorResponse|NEM KÖTELEZŐ. Emberek számára olvasható ASCII szöveges kezeléséről további információt.|  
|OAuth2ErrorDescription_ClientCredentialsGrant_TokenErrorResponse|NEM KÖTELEZŐ. Emberek számára olvasható ASCII szöveges kezeléséről további információt.|  
|OAuth2ErrorDescription_ImplicitGrant_AuthorizationErrorResponse|NEM KÖTELEZŐ. Emberek számára olvasható ASCII szöveges kezeléséről további információt.|  
|OAuth2ErrorDescription_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|NEM KÖTELEZŐ. Emberek számára olvasható ASCII szöveges kezeléséről további információt.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_AuthorizationErrorResponse|NEM KÖTELEZŐ. Egy URI-emberek számára olvasható weblapon azonosítja hello hibával kapcsolatos információkat.|  
|OAuth2ErrorUri_AuthorizationCodeGrant_TokenErrorResponse|NEM KÖTELEZŐ. Egy URI-emberek számára olvasható weblapon azonosítja hello hibával kapcsolatos információkat.|  
|OAuth2ErrorUri_ClientCredentialsGrant_TokenErrorResponse|NEM KÖTELEZŐ. Egy URI-emberek számára olvasható weblapon azonosítja hello hibával kapcsolatos információkat.|  
|OAuth2ErrorUri_ImplicitGrant_AuthorizationErrorResponse|NEM KÖTELEZŐ. Egy URI-emberek számára olvasható weblapon azonosítja hello hibával kapcsolatos információkat.|  
|OAuth2ErrorUri_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|NEM KÖTELEZŐ. Egy URI-emberek számára olvasható weblapon azonosítja hello hibával kapcsolatos információkat.|  
|OAuth2Error_AuthorizationCodeGrant_AuthorizationErrorResponse|SZÜKSÉGES. Hello következő egyetlen ASCII hiba kódja: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_AuthorizationCodeGrant_TokenErrorResponse|SZÜKSÉGES. Hello következő egyetlen ASCII hiba kódja: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ClientCredentialsGrant_TokenErrorResponse|SZÜKSÉGES. Hello következő egyetlen ASCII hiba kódja: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2Error_ImplicitGrant_AuthorizationErrorResponse|SZÜKSÉGES. Hello következő egyetlen ASCII hiba kódja: invalid_request, unauthorized_client, access_denied, unsupported_response_type, invalid_scope, server_error, temporarily_unavailable.|  
|OAuth2Error_ResourceOwnerPasswordCredentialsGrant_TokenErrorResponse|SZÜKSÉGES. Hello következő egyetlen ASCII hiba kódja: invalid_request, invalid_client, invalid_grant, unauthorized_client, unsupported_grant_type, invalid_scope.|  
|OAuth2ExpiresIn_AuthorizationCodeGrant_TokenResponse|AJÁNLOTT. hello másodpercben hello hozzáférési jogkivonat.|  
|OAuth2ExpiresIn_ClientCredentialsGrant_TokenResponse|AJÁNLOTT. hello másodpercben hello hozzáférési jogkivonat.|  
|OAuth2ExpiresIn_ImplicitGrant_AuthorizationResponse|AJÁNLOTT. hello másodpercben hello hozzáférési jogkivonat.|  
|OAuth2ExpiresIn_ResourceOwnerPasswordCredentialsGrant_TokenResponse|AJÁNLOTT. hello másodpercben hello hozzáférési jogkivonat.|  
|OAuth2GrantType_AuthorizationCodeGrant_TokenRequest|SZÜKSÉGES. Érték túl állítható be "authorization_code".|  
|OAuth2GrantType_ClientCredentialsGrant_TokenRequest|SZÜKSÉGES. Érték túl állítható be "client_credentials".|  
|OAuth2GrantType_ResourceOwnerPasswordCredentialsGrant_TokenRequest|SZÜKSÉGES. Érték túl állítható be "password".|  
|OAuth2Password_ResourceOwnerPasswordCredentialsGrant_TokenRequest|SZÜKSÉGES. hello erőforrás tulajdonosi jelszó.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_AuthorizationRequest|NEM KÖTELEZŐ. hello átirányítási végpont URI abszolút URI-nak kell lennie.|  
|OAuth2RedirectUri_AuthorizationCodeGrant_TokenRequest|SZÜKSÉGES Ha hello "redirect_uri" paraméter hello engedélyezési kérelem szerepelt, és azok értékeit azonosnak kell lennie.|  
|OAuth2RedirectUri_ImplicitGrant_AuthorizationRequest|NEM KÖTELEZŐ. hello átirányítási végpont URI abszolút URI-nak kell lennie.|  
|OAuth2RefreshToken_AuthorizationCodeGrant_TokenResponse|NEM KÖTELEZŐ. hello frissítési jogkivonat, amely lehet használt tooobtain új jogkivonatot.|  
|OAuth2RefreshToken_ClientCredentialsGrant_TokenResponse|NEM KÖTELEZŐ. hello frissítési jogkivonat, amely lehet használt tooobtain új jogkivonatot.|  
|OAuth2RefreshToken_ResourceOwnerPasswordCredentialsGrant_TokenResponse|NEM KÖTELEZŐ. hello frissítési jogkivonat, amely lehet használt tooobtain új jogkivonatot.|  
|OAuth2ResponseType_AuthorizationCodeGrant_AuthorizationRequest|SZÜKSÉGES. Értéket kell megadni túl "code".|  
|OAuth2ResponseType_ImplicitGrant_AuthorizationRequest|SZÜKSÉGES. Érték túl "token" kell beállítani.|  
|OAuth2Scope_AuthorizationCodeGrant_AuthorizationRequest|NEM KÖTELEZŐ. hello hozzáférési kérelem hello hatókörét.|  
|OAuth2Scope_AuthorizationCodeGrant_TokenResponse|Nem kötelező, ha azonos toohello hatókör kérte hello ügyfél; Ellenkező esetben szükséges.|  
|OAuth2Scope_ClientCredentialsGrant_TokenRequest|NEM KÖTELEZŐ. hello hozzáférési kérelem hello hatókörét.|  
|OAuth2Scope_ClientCredentialsGrant_TokenResponse|Nem kötelező, ha azonos toohello hatókör hello ügyfél; által kért Ellenkező esetben szükséges.|  
|OAuth2Scope_ImplicitGrant_AuthorizationRequest|NEM KÖTELEZŐ. hello hozzáférési kérelem hello hatókörét.|  
|OAuth2Scope_ImplicitGrant_AuthorizationResponse|Nem kötelező, ha azonos toohello hatókör kérte hello ügyfél; Ellenkező esetben szükséges.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenRequest|NEM KÖTELEZŐ. hello hozzáférési kérelem hello hatókörét.|  
|OAuth2Scope_ResourceOwnerPasswordCredentialsGrant_TokenResponse|Nem kötelező, ha azonos toohello hatókör hello ügyfél; által kért Ellenkező esetben szükséges.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationErrorResponse|SZÜKSÉGES, ha hello "állapot" paraméter szerepelt hello ügyfél-hitelesítési kérést.  hello pontos érték hello ügyféltől kapott.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationRequest|AJÁNLOTT. Egy nem átlátszó érték toomaintain ügyfélállapot hello hello kérelem és a visszahívás között használják.  hello engedélyezési kiszolgáló például ezt az értéket, ha hello felhasználói ügynök hátsó toohello ügyfél átirányítása.  hello paraméter webhelyközi kérések hamisításának megakadályozása használható.|  
|OAuth2State_AuthorizationCodeGrant_AuthorizationResponse|SZÜKSÉGES, ha hello "állapot" paraméter szerepelt hello ügyfél-hitelesítési kérést.  hello pontos érték hello ügyféltől kapott.|  
|OAuth2State_ImplicitGrant_AuthorizationErrorResponse|SZÜKSÉGES, ha hello "állapot" paraméter szerepelt hello ügyfél-hitelesítési kérést.  hello pontos érték hello ügyféltől kapott.|  
|OAuth2State_ImplicitGrant_AuthorizationRequest|AJÁNLOTT. Egy nem átlátszó érték toomaintain ügyfélállapot hello hello kérelem és a visszahívás között használják.  hello engedélyezési kiszolgáló például ezt az értéket, ha hello felhasználói ügynök hátsó toohello ügyfél átirányítása.  hello paraméter webhelyközi kérések hamisításának megakadályozása használható.|  
|OAuth2State_ImplicitGrant_AuthorizationResponse|SZÜKSÉGES, ha hello "állapot" paraméter szerepelt hello ügyfél-hitelesítési kérést.  hello pontos érték hello ügyféltől kapott.|  
|OAuth2TokenType_AuthorizationCodeGrant_TokenResponse|SZÜKSÉGES. hello kiadott tokennek hello típusa.|  
|OAuth2TokenType_ClientCredentialsGrant_TokenResponse|SZÜKSÉGES. hello kiadott tokennek hello típusa.|  
|OAuth2TokenType_ImplicitGrant_AuthorizationResponse|SZÜKSÉGES. hello kiadott tokennek hello típusa.|  
|OAuth2TokenType_ResourceOwnerPasswordCredentialsGrant_TokenResponse|SZÜKSÉGES. hello kiadott tokennek hello típusa.|  
|OAuth2UserName_ResourceOwnerPasswordCredentialsGrant_TokenRequest|SZÜKSÉGES. hello erőforrás tulajdonosa felhasználónév.|  
|OAuth2UnsupportedTokenType|A jogkivonat típusa "{0}" nincs supporetd.|  
|OAuth2InvalidState|Érvénytelen válasz engedélyezési kiszolgálótól|  
|OAuth2GrantType_AuthorizationCode|Engedélyezési kód|  
|OAuth2GrantType_Implicit|Implicit|  
|OAuth2GrantType_ClientCredentials|Ügyfél hitelesítő adatait|  
|OAuth2GrantType_ResourceOwnerPassword|Erőforrás-tulajdonosi jelszó|  
|WebDocumentation302Code|302 található|  
|WebDocumentation400Code|400 (hibás kérés)|  
|OAuth2SendingMethod_AuthHeader|Engedélyezési fejléc|  
|OAuth2SendingMethod_QueryParam|Lekérdezési paraméter|  
|OAuth2AuthorizationServerGeneralException|Hiba történt az {0} hozzáférésének engedélyezése|  
|OAuth2AuthorizationServerCommunicationException|Nem hozható létre kapcsolat tooauthorization HTTP-kiszolgáló, vagy váratlanul megszűnt.|  
|WebDocumentationOAuth2GeneralErrorMessage|Váratlan hiba történt.|  
|AuthorizationServerCommunicationException|Engedélyezési kiszolgálói kommunikáció kivétel történt. Lépjen kapcsolatba a rendszergazdával.|  
|TextblockSubscriptionKeyHeaderDescription|Előfizetés-kulcsot, amely hozzáférési toothis API alkalmazást biztosít. Található a < a href = "/ fejlesztői"\>profil < /a\>.|  
|TextblockOAuthHeaderDescription|OAuth 2.0 hozzáférési jogkivonatot kapott < i\>{0} < /i\>. Támogatott grant típusok: < i\>{1} < /i\>.|  
|TextblockContentTypeHeaderDescription|A médiatípus toohello API küldi hello törzse.|  
|ErrorMessageApiNotAccessible|hello toocall próbált API jelenleg nem érhető el. Lépjen kapcsolatba a hello API publisher < a href = "/ problémák"\>itt < /a\>.|  
|ErrorMessageApiTimedout|hello toocall próbált API a vártnál normál tooget válasz vissza. Lépjen kapcsolatba a hello API publisher < a href = "/ problémák"\>itt < /a\>.|  
|BadRequestParameterExpected|"a"{0}"paramétert várt."|  
|TooltipTextDoubleClickToSelectAll|Kattintson duplán az összes tooselect.|  
|TooltipTextHideRevealSecret|Megjelenítése/elrejtése|  
|ButtonLinkOpenConsole|Kipróbálom|  
|SectionHeadingRequestBody|Kérés törzsében|  
|SectionHeadingRequestParameters|A kérelemben szereplő paraméterek|  
|SectionHeadingRequestUrl|Kérelem URL-címe|  
|SectionHeadingResponse|Válasz|  
|SectionHeadingRequestHeaders|Kérelem fejlécei|  
|FormLabelSubtextOptional|Nem kötelező|  
|SectionHeadingCodeSamples|Kódminták|  
|TextblockOpenidConnectHeaderDescription|OpenID Connect azonosító jogkivonatot kapott < i\>{0} < /i\>. Támogatott grant típusok: < i\>{1} < /i\>.|  
  
###  <a name="ErrorPageStrings"></a>ErrorPageStrings  
  
|Név|Szöveg|  
|----------|----------|  
|LinkLabelBack|biztonsági|  
|LinkLabelHomePage|Kezdőlap|  
|LinkLabelSendUsEmail|e-mail küldése|  
|PageTitleError|Sajnáljuk hiba történt a probléma szolgál hello kért oldalról|  
|TextblockPotentialCauseIntermittentIssue|Ennek az lehet, amely már nem látható időszakos adatok hozzáférési problémát.|  
|TextblockPotentialCauseOldLink|lehet, hogy hello hivatkozásra kattintott, a régi, és nem pont toohello többé kijavíthatja helyét.|  
|TextblockPotentialCauseTechnicalProblem|Az end műszaki probléma lehet.|  
|TextblockPotentialSolutionRefresh|Próbálja meg frissíteni a hello lap.|  
|TextblockPotentialSolutionStartOver|Kezdje újra a folyamatot a következőhöz: {0}.|  
|TextblockPotentialSolutionTryAgain|Nyissa meg {0}, és próbálja meg újra végrehajtani a hello művelet.|  
|TextReportProblem|a Microsoft és a hibával leíró {0} áttekintjük, hogy azt, amint azt is.|  
|TitlePotentialCause|Lehetséges ok|  
|TitlePotentialSolution|Valószínűleg csak egy ideiglenes probléma, néhány dolog tootry|  
  
###  <a name="IssuesStrings"></a>IssuesStrings  
  
|Név|Szöveg|  
|----------|----------|  
|WebIssuesIndexTitle|Hibák|  
|WebIssuesNoActiveSubscriptions|Nincs aktív előfizetéssel rendelkezik. A termék tooreport problémát toosubscribe van szükség.|  
|WebIssuesNotSignin|Nincs bejelentkezve. Kérjük, {0} tooreport problémát, vagy írjon egy megjegyzést.|  
|WebIssuesReportIssueButton|A jelentés probléma|  
|WebIssuesSignIn|bejelentkezés|  
|WebIssuesStatusReportedBy|Állapot: {0} &#124; {1} által jelentett|  
  
###  <a name="NotFoundStrings"></a>NotFoundStrings  
  
|Név|Szöveg|  
|----------|----------|  
|LinkLabelHomePage|Kezdőlap|  
|LinkLabelSendUsEmail|e-mail küldése|  
|PageTitleNotFound|Sajnos nem található a keresett hello lap|  
|TextblockPotentialCauseMisspelledUrl|Valószínűleg elírta hello URL-CÍMÉT írta be a Ha.|  
|TextblockPotentialCauseOldLink|lehet, hogy hello hivatkozásra kattintott, a régi, és nem pont toohello többé kijavíthatja helyét.|  
|TextblockPotentialSolutionRetype|Próbálja meg, hogy újra beírni a hello URL-CÍMÉT.|  
|TextblockPotentialSolutionStartOver|Kezdje újra a folyamatot a következőhöz: {0}.|  
|TextReportProblem|a Microsoft és a hibával leíró {0} áttekintjük, hogy azt, amint azt is.|  
|TitlePotentialCause|Lehetséges ok|  
|TitlePotentialSolution|Lehetséges megoldás|  
  
###  <a name="ProductDetailsStrings"></a>ProductDetailsStrings  
  
|Név|Szöveg|  
|----------|----------|  
|WebProductsAgreement|Ha túl feliratkozik {0} termék, toohello elfogadom `<a data-toggle='modal' href='#legal-terms'\>Terms of Use</a\>`.|  
|WebProductsLegalTermsLink|Használati feltételek|  
|WebProductsSubscribeButton|Feliratkozás|  
|WebProductsUsageLimitsHeader|Használati korlátok|  
|WebProductsYouAreNotSubscribed|Előfizetett toothis termék áll.|  
|WebProductsYouRequestedSubscription|A kért előfizetés toothis termék.|  
|ErrorYouNeedtoAgreeWithLegalTerms|A folytatás előtt el kell fogadnia a használati feltételeket toohello.|  
|ButtonLabelAddSubscription|Előfizetés hozzáadása|  
|LinkLabelChangeSubscriptionName|módosítás|  
|ButtonLabelConfirm|Erősítse meg|  
|TextblockMultipleSubscriptionsCount|{0} előfizetések toothis termék közül választhat:|  
|TextblockSingleSubscriptionsCount|{0} előfizetés toothis termék közül választhat:|  
|TextblockSingleApisCount|Ez a termék {0} API tartalmaz:|  
|TextblockMultipleApisCount|Ez a termék {0} API-kat tartalmaz:|  
|TextblockHeaderSubscribe|Előfizetés tooproduct|  
|TextblockSubscriptionDescription|Egy új előfizetés jön létre az alábbiak szerint:|  
|TextblockSubscriptionLimitReached|Előfizetési határértékét.|  
  
###  <a name="ProductsStrings"></a>ProductsStrings  
  
|Név|Szöveg|  
|----------|----------|  
|PageTitleProducts|Termékek|  
  
###  <a name="ProviderInfoStrings"></a>ProviderInfoStrings  
  
|Név|Szöveg|  
|----------|----------|  
|TextboxExternalIdentitiesDisabled|Bejelentkezés le van tiltva a hello rendszergazdái hello pillanatban.|  
|TextboxExternalIdentitiesSigninInvitation|Alternatív megoldásként jelentkezzen be|  
|TextboxExternalIdentitiesSigninInvitationPrimary|Jelentkezzen be:|  
  
###  <a name="SigninResources"></a>SigninResources  
  
|Név|Szöveg|  
|----------|----------|  
|PrincipalNotFound|Rendszerbiztonsági tag nem található vagy érvénytelen aláírása|  
|ErrorSsoAuthenticationFailed|Egyszeri bejelentkezési hitelesítés sikertelen volt|  
|ErrorSsoAuthenticationFailedDetailed|Érvénytelen a megadott jogkivonat vagy aláírás nem ellenőrizhető.|  
|ErrorSsoTokenInvalid|Egyszeri bejelentkezési token érvénytelen|  
|ValidationErrorSpecificEmailAlreadyExists|E-mailek "{0}" már regisztrálva van|  
|ValidationErrorSpecificEmailInvalid|E-mailek "{0}" érvénytelen.|  
|ValidationErrorPasswordInvalid|Jelszó érvénytelen. Javítsa ki az hello hibákat, és próbálkozzon újra.|  
|PropertyTooShort|{0} túl rövid|  
|WebAuthenticationAddresserEmailInvalidErrorMessage|Érvénytelen e-mail címet.|  
|ValidationMessageNewPasswordConfirmationRequired|Új jelszó megerősítése|  
|ValidationErrorPasswordConfirmationRequired|Ellenőrizze, hogy a jelszava üres|  
|WebAuthenticationEmailChangeNotice|Változás visszaigazoló e-mailben túl van hello {0}. Tooconfirm szereplő utasításokat kövesse az új e-mail címet. Ha hello e-mailek nem érkezik tooyour beérkezett hello a következő néhány percet, ellenőrizze a Levélszemét mappát.|  
|WebAuthenticationEmailChangeNoticeHeader|Az e-mailek változáskérés feldolgozása sikeresen megtörtént|  
|WebAuthenticationEmailChangeNoticeTitle|E-mailt kért módosítás|  
|WebAuthenticationEmailHasBeenRevertedNotice|Már létezik, e-mailt. Kérelem vissza lett állítva.|  
|ValidationErrorEmailAlreadyExists|E-mailek már létezik.|  
|ValidationErrorEmailInvalid|Érvénytelen e-mail cím|  
|TextboxLabelEmail|E-mail cím|  
|ValidationErrorEmailRequired|E-mailek szükség.|  
|WebAuthenticationErrorNoticeHeader|Hiba|  
|WebAuthenticationFieldLengthErrorMessage|{0} {1} legfeljebb ilyen hosszúnak kell lennie.|  
|TextboxLabelEmailFirstName|Utónév|  
|ValidationErrorFirstNameRequired|Keresztnév megadása szükséges.|  
|ValidationErrorFirstNameInvalid|Érvénytelen Utónév|  
|NoticeInvalidInvitationToken|Vegye figyelembe, hogy megerősítési hivatkozások érvényesek csak 48 órán keresztül. Ha továbbra is a időkereten belül, győződjön meg arról, hogy a hivatkozás megfelelő. Ha lejárt a hivatkozásra, majd ismételje meg tooconfirm próbált hello művelet.|  
|NoticeHeaderInvalidInvitationToken|Érvénytelen a meghívást jogkivonat|  
|NoticeTitleInvalidInvitationToken|Jóváhagyás hiba|  
|WebAuthenticationLastNameInvalidErrorMessage|Utolsó neve érvénytelen|  
|TextboxLabelEmailLastName|Vezetéknév|  
|ValidationErrorLastNameRequired|Vezetéknév megadása szükséges.|  
|WebAuthenticationLinkExpiredNotice|Küldött tooyou megerősítő hivatkozás érvényessége lejárt. `<a href={0}?token={1}>Resend confirmation email.</a\>`|  
|NoticePasswordResetLinkInvalidOrExpired|A jelszó-visszaállítási hivatkozásra értéke érvénytelen vagy lejárt.|  
|WebAuthenticationLinkExpiredNoticeTitle|Küldött hivatkozások|  
|WebAuthenticationNewPasswordLabel|Új jelszó|  
|ValidationMessageNewPasswordRequired|Meg kell adni az új jelszót.|  
|TextboxLabelNotificationsSenderEmail|Feladó e-mail értesítések|  
|TextboxLabelOrganizationName|Szervezet neve|  
|WebAuthenticationOrganizationRequiredErrorMessage|Szervezet neve nincs megadva|  
|WebAuthenticationPasswordChangedNotice|A jelszó frissítése sikeresen befejeződött.|  
|WebAuthenticationPasswordChangedNoticeTitle|Jelszó frissítése|  
|WebAuthenticationPasswordCompareErrorMessage|Jelszavak nem egyeznek|  
|WebAuthenticationPasswordConfirmLabel|Jelszó megerősítése|  
|ValidationErrorPasswordInvalidDetailed|A jelszava túl egyszerű.|  
|WebAuthenticationPasswordLabel|Jelszó|  
|ValidationErrorPasswordRequired|Meg kell adni a jelszót.|  
|WebAuthenticationPasswordResetSendNotice|Módosítsa jelszavát visszaigazoló e-mailben túl van hello {0}. Adjon követésével hello hello e-mail toocontinue belül a jelszómódosítási folyamat.|  
|WebAuthenticationPasswordResetSendNoticeHeader|A jelszó alaphelyzetbe állítása kérés feldolgozása sikeresen megtörtént|  
|WebAuthenticationPasswordResetSendNoticeTitle|Jelszó alaphelyzetbe állítása irányuló kérés.|  
|WebAuthenticationRequestNotFoundNotice|Nem található|  
|WebAuthenticationSenderEmailRequiredErrorMessage|Feladó e-mail értesítések értéke üres|  
|WebAuthenticationSigninPasswordLabel|Erősítse meg a jelszó megadásával hello módosítása|  
|WebAuthenticationSignupConfirmNotice|Regisztrációs visszaigazoló e-mailben hamarosan megérkezik túl {0}. < br /\> adjon kövesse az utasításokat belül hello e-mail tooactivate a fiókjához. < br /\> hello e-mailek nem érkezik a beérkezett belül hello tovább néhány percet, ha ellenőrizni a Levélszemét mappát.|  
|WebAuthenticationSignupConfirmNoticeHeader|A fiók sikeresen létrejött.|  
|WebAuthenticationSignupConfirmNoticeRepeatHeader|Regisztrációs visszaigazoló e-mailben küldött újra|  
|WebAuthenticationSignupConfirmNoticeTitle|Fiók létrehozása|  
|WebAuthenticationTokenRequiredErrorMessage|A token üres|  
|WebAuthenticationUserAlreadyRegisteredNotice|Úgy tűnik, hogy ezzel az e-mail a felhasználó már regisztrálva van a hello rendszer. Ha elfelejtette a jelszavát, próbálja meg toorestore, vagy forduljon a terméktámogatási csapathoz.|  
|WebAuthenticationUserAlreadyRegisteredNoticeHeader|A felhasználó már regisztrálva van|  
|WebAuthenticationUserAlreadyRegisteredNoticeTitle|Már regisztrálva van|  
|ButtonLabelChangePassword|Jelszó módosítása|  
|ButtonLabelChangeAccountInfo|Fiók adatainak módosítása|  
|ButtonLabelCloseAccount|Zárja be a fiók|  
|WebAuthenticationInvalidCaptchaErrorMessage|A megadott szöveg nem felel meg a hello kép szöveget. Kérjük, próbálkozzon újból.|  
|ValidationErrorCredentialsInvalid|E-mailek és a jelszó érvénytelen. Javítsa ki az hello hibákat, és próbálkozzon újra.|  
|WebAuthenticationRequestIsNotValid|Kérelem érvénytelen.|  
|WebAuthenticationUserIsNotConfirm|Ellenőrizze, hogy a regisztrációt a toosign megkísérlése előtt.|  
|WebAuthenticationInvalidEmailFormated|Érvénytelen e-mail: {0}|  
|WebAuthenticationUserNotFound|A felhasználó nem található|  
|WebAuthenticationTenantNotRegistered|A fiókja tagja tooa Azure Active Directory-bérlőt, ami nem engedélyezett tooaccess ezen a portálon.|  
|WebAuthenticationAuthenticationFailed|Nem sikerült a hitelesítés.|  
|WebAuthenticationGooglePlusNotEnabled|Nem sikerült a hitelesítés. Ha hello alkalmazás engedélyezett, akkor lépjen kapcsolatba a hello admin toomake meg arról, hogy, hogy a Google hitelesítési megfelelően van konfigurálva.|  
|ValidationErrorAllowedTenantIsRequired|Engedélyezett bérlői szükség|  
|ValidationErrorTenantIsNotValid|Érvénytelen hello Azure Active Directory-bérlőt "{0}".|  
|WebAuthenticationActiveDirectoryTitle|Azure Active Directory|  
|WebAuthenticationLoginUsingYourProvider|Jelentkezzen be a {0} fiókjával|  
|WebAuthenticationUserLimitNotice|Ez a szolgáltatás elérte a megengedett felhasználók maximális száma hello. Adjon `<a href="mailto:{0}"\>contact hello administrator</a\>` tooupgrade a szolgáltatást, majd engedélyezze újra a felhasználói regisztráció.|  
|WebAuthenticationUserLimitNoticeHeader|Felhasználói regisztráció letiltva|  
|WebAuthenticationUserLimitNoticeTitle|Felhasználói regisztráció letiltva|  
|WebAuthenticationUserRegistrationDisabledNotice|Felhasználók regisztrálása hello rendszergazda letiltotta. Kérjük, jelentkezzen be a külső identitásszolgáltatótól.|  
|WebAuthenticationUserRegistrationDisabledNoticeHeader|Felhasználói regisztráció letiltva|  
|WebAuthenticationUserRegistrationDisabledNoticeTitle|Felhasználói regisztráció letiltva|  
|WebAuthenticationSignupPendingConfirmationNotice|A Microsoft fejezheti be a fiók létrehozását hello igazolnia kell tooverify e-mail címét. Küldtünk egy e-mailt túl {0}. Adjon követésével hello hello e-mail tooactivate belül a fiókját. Ha az üdvözlő e-mail hello belül nem érkeznek, tovább néhány percet, ellenőrizze a Levélszemét mappát.|  
|WebAuthenticationSignupPendingConfirmationAccountFoundNotice|Nem megerősített fiók hello e-mail cím {0} észleltünk. a fiók e-mail címét kell tooverify toocomplete hello létrehozása. Küldtünk egy e-mailt túl {0}. Adjon követésével hello hello e-mail tooactivate belül a fiókját. Ha az üdvözlő e-mail hello belül nem érkezik a következő percekben, ellenőrizze a Levélszemét mappa|  
|WebAuthenticationSignupConfirmationAlmostDone|Majdnem kész|  
|WebAuthenticationSignupConfirmationEmailSent|Küldtünk egy e-mailt túl {0}. Adjon követésével hello hello e-mail tooactivate belül a fiókját. Ha az üdvözlő e-mail hello belül nem érkeznek, tovább néhány percet, ellenőrizze a Levélszemét mappát.|  
|WebAuthenticationEmailSentNotificationMessage|Túl által sikeresen küldött e-mail {0}|  
|WebAuthenticationNoAadTenantConfigured|Nincs konfigurálva hello szolgáltatáshoz Azure Active Directory-bérlő.|  
|CheckboxLabelUserRegistrationTermsConsentRequired|Elfogadom toohello `<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use</a\>`.|  
|TextblockUserRegistrationTermsProvided|Tekintse át`<a data-toggle="modal" href="#" data-target="#terms"\>Terms of Use.</a\>`|  
|DialogHeadingTermsOfUse|Használati feltételek|  
|ValidationMessageConsentNotAccepted|A folytatás előtt el kell fogadnia a használati feltételeket toohello.|  
  
###  <a name="SigninStrings"></a>SigninStrings  
  
|Név|Szöveg|  
|----------|----------|  
|WebAuthenticationForgotPassword|Elfelejtette a jelszavát?|  
|WebAuthenticationIfAdministrator|Ha rendszergazdaként jelentkezett be kell jelentkeznie a `<a href="{0}"\>here</a\>`.|  
|WebAuthenticationNotAMember|Nem tagja még? `<a href="/signup"\>Sign up now</a\>`|  
|WebAuthenticationRemember|Emlékezzen rám ezen a számítógépen|  
|WebAuthenticationSigininWithPassword|Jelentkezzen be a felhasználónevét és jelszavát|  
|WebAuthenticationSigninTitle|Bejelentkezés|  
|WebAuthenticationSignUpNow|Regisztráljon most|  
  
###  <a name="SignupStrings"></a>SignupStrings  
  
|Név|Szöveg|  
|----------|----------|  
|PageTitleSignup|Regisztráció|  
|WebAuthenticationAlreadyAMember|Egy tag már?|  
|WebAuthenticationCreateNewAccount|Új API Management-fiók létrehozása|  
|WebAuthenticationSigninNow|Jelentkezzen be|  
|ButtonLabelSignup|Regisztráció|  
  
###  <a name="SubscriptionListStrings"></a>SubscriptionListStrings  
  
|Név|Szöveg|  
|----------|----------|  
|SubscriptionCancelConfirmation|Biztos, hogy akkor toocancel ezt az előfizetést?|  
|SubscriptionRenewConfirmation|Biztos, hogy akkor toorenew ezt az előfizetést?|  
|WebDevelopersManageSubscriptions|Előfizetések kezelése|  
|WebDevelopersPrimaryKey|Elsődleges kulcs|  
|WebDevelopersRegenerateLink|Újragenerálás|  
|WebDevelopersSecondaryKey|Másodlagos kulcsát.|  
|ButtonLabelShowKey|Megjelenítés|  
|ButtonLabelRenewSubscription|Frissítés|  
|WebDevelopersSubscriptionReqested|A kért {0}|  
|WebDevelopersSubscriptionRequestedState|A kért|  
|WebDevelopersSubscriptionTableNameHeader|Név|  
|WebDevelopersSubscriptionTableStateHeader|Állapot|  
|WebDevelopersUsageStatisticsLink|Elemzési jelentések|  
|WebDevelopersYourSubscriptions|Az előfizetések|  
|SubscriptionPropertyLabelRequestedDate|A kért|  
|SubscriptionPropertyLabelStartedDate|A következőn:|  
|PageTitleRenameSubscription|Nevezze át az előfizetést|  
|SubscriptionPropertyLabelName|Előfizetés neve|  
  
###  <a name="SubscriptionStrings"></a>SubscriptionStrings  
  
|Név|Szöveg|  
|----------|----------|  
|SectionHeadingCloseAccount|Tooclose keresése a fiókját?|  
|PageTitleDeveloperProfile|Profil|  
|ButtonLabelHideKey|Elrejtése|  
|ButtonLabelRegenerateKey|Újragenerálás|  
|InformationMessageKeyWasRegenerated|Biztos, hogy Ön tooregenerate ezt a kulcsot?|  
|ButtonLabelShowKey|Megjelenítés|  
  
###  <a name="UpdateProfileStrings"></a>UpdateProfileStrings  
  
|Név|Szöveg|  
|----------|----------|  
|ButtonLabelUpdateProfile|Profil frissítése|  
|PageTitleUpdateProfile|Fiókadatok frissítése|  
  
###  <a name="UserProfile"></a>Felhasználói profil  
  
|Név|Szöveg|  
|----------|----------|  
|ButtonLabelChangeAccountInfo|Fiók adatainak módosítása|  
|ButtonLabelChangePassword|Jelszó módosítása|  
|ButtonLabelCloseAccount|Zárja be a fiók|  
|TextboxLabelEmail|E-mail cím|  
|TextboxLabelEmailFirstName|Utónév|  
|TextboxLabelEmailLastName|Vezetéknév|  
|TextboxLabelNotificationsSenderEmail|Feladó e-mail értesítések|  
|TextboxLabelOrganizationName|Szervezet neve|  
|SubscriptionStateActive|Aktív|  
|SubscriptionStateCancelled|Megszakítva|  
|SubscriptionStateExpired|Lejárt|  
|SubscriptionStateRejected|Elutasított|  
|SubscriptionStateRequested|A kért|  
|SubscriptionStateSuspended|Felfüggesztve|  
|DefaultSubscriptionNameTemplate|{0} (alapértelmezett)|  
|SubscriptionNameTemplate|Fejlesztői hozzáférést #{0}|  
|TextboxLabelSubscriptionName|Előfizetés neve|  
|ValidationMessageSubscriptionNameRequired|Előfizetés neve nem lehet üres.|  
|ApiManagementUserLimitReached|Ez a szolgáltatás elérte a megengedett felhasználók maximális száma hello. Frissítse az tooa magasabb tarifacsomagra vált.|  
  
##  <a name="glyphs"></a>A betűkép-erőforrások  
 Az API Management fejlesztői portál sablonok használhatja a hello Karaktertábla [a rendszerindítási Glyphicons](http://getbootstrap.com/components/#glyphicons). A glyphs elem tartalmaz több mint 250 Karaktertábla hello a betűtípus formátumban [Glyphicon](http://glyphicons.com/) Halflings beállítása. Ez egy eredeti toouse megadásához használja a következő szintaxist hello.  
  
```html  
<span class="glyphicon glyphicon-user">  
```  
  
 Hello teljes listája megtalálható a glyphs elem, [a rendszerindítási Glyphicons](http://getbootstrap.com/components/#glyphicons).

## <a name="next-steps"></a>Következő lépések
A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).

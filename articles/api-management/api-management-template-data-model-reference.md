---
title: "aaaAzure API Management-sablon adatok modellhez tartozó referencia |} Microsoft Docs"
description: "További információk a hello entitás és típus felelősséget közös elemek hello fejlesztői portál sablonok az Azure API Management hello adatmodellekben szerepel."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: b0ad7e15-9519-4517-bb73-32e593ed6380
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: 7d049d8ecc9e597cf48ce0c820c172798bcf86de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-api-management-template-data-model-reference"></a>Az Azure API Management sablon modell hivatkozás
Ez a témakör ismerteti a hello entitás és típus felelősséget közös elemek hello fejlesztői portál sablonok az Azure API Management hello adatmodellekben szerepel.  
  
 A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](https://azure.microsoft.com/documentation/articles/api-management-developer-portal-templates/).  
  
-   [API](#API)  
-   [API-összefoglalót](#APISummary)  
-   [Alkalmazás](#Application)  
-   [Melléklet](#Attachment)  
-   [Kódminta](#Sample)  
-   [Megjegyzés](#Comment)  
-   [Szűrés](#Filtering)  
-   [Fejléc](#Header)  
-   [HTTP-kérelem](#HTTPRequest)  
-   [HTTP-válasz](#HTTPResponse)  
-   [A probléma](#Issue)  
-   [Művelet](#Operation)  
-   [Művelet menü](#Menu)  
-   [A művelet menüpont](#MenuItem)  
-   [Lapozófájl](#Paging)  
-   [A paraméter](#Parameter)  
-   [A termék](#Product)  
-   [Szolgáltató](#Provider)  
-   [Megjelenítésre](#Representation)  
-   [Előfizetés](#Subscription)  
-   [Előfizetés összefoglaló](#SubscriptionSummary)  
-   [Felhasználói fiók adatainak](#UserAccountInfo)  
-   [Felhasználói bejelentkezés](#UseSignIn)  
-   [Felhasználói bejelentkezési](#UserSignUp)  
  
##  <a name="API"></a>API  
 Hello `API` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|id|Karakterlánc|Erőforrás-azonosítója. Hello API hello aktuális API Management szolgáltatáspéldány belül egyedi azonosítására szolgál. hello értéke hello formátuma érvényes relatív URL- `apis/{id}` ahol `{id}` API azonosítója. Ez a tulajdonság csak olvasható.|  
|név|Karakterlánc|Hello API neve. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|leírás|Karakterlánc|Hello API leírása. Nem lehet üres. HTML-címkék formázás is tartalmazhat. Hossza legfeljebb 1000 karakter lehet.|  
|serviceUrl|Karakterlánc|Ez az API végrehajtási hello háttérszolgáltatást abszolút URL-címe|  
|Elérési út|Karakterlánc|Egyedi azonosítása, ez az API és az összes belül hello API Management service-példány az erőforrás elérési utak relatív URL-címe. Azt a rendszer hozzáfűzi toohello API végpont alap URL-cím során megadott hello szolgáltatás példány létrehozása tooform egy nyilvános URL-címet ehhez az API.|  
|protokollok|a tömb szám|Ismerteti, hogy mely protokollok hello a műveletek az API-ban is elindítható. Két érték engedélyezett `1 - http` és `2 - https`, vagy mindkettőt.|  
|authenticationSettings|[Engedélyezési kiszolgáló hitelesítési beállítások](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-contract-reference#AuthenticationSettings)|Ez az API szerepel hitelesítési beállítások gyűjteménye.|  
|subscriptionKeyParameterNames|Objektum|Nem kötelező tulajdonság, amely használt toospecify egyéni nevek hello előfizetés kulcsot tartalmazó lekérdezés és/vagy a fejléc paraméter lehet. Ha ez a tulajdonság jelen hello két alábbi tulajdonságok közül legalább egy tartalmazhat.<br /><br /> `{   "subscriptionKeyParameterNames":   {     "query": “customQueryParameterName",     "header": “customHeaderParameterName"   } }`|  
  
##  <a name="APISummary"></a>API-összefoglalót  
 Hello `API summary` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|id|Karakterlánc|Erőforrás-azonosítója. Hello API hello aktuális API Management szolgáltatáspéldány belül egyedi azonosítására szolgál. hello értéke hello formátuma érvényes relatív URL- `apis/{id}` ahol `{id}` API azonosítója. Ez a tulajdonság csak olvasható.|  
|név|Karakterlánc|Hello API neve. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|leírás|Karakterlánc|Hello API leírása. Nem lehet üres. HTML-címkék formázás is tartalmazhat. Hossza legfeljebb 1000 karakter lehet.|  
  
##  <a name="Application"></a>Alkalmazás  
 Hello `application` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Karakterlánc|hello hello alkalmazás egyedi azonosítója.|  
|Cím|Karakterlánc|hello alkalmazás hello címe.|  
|Leírás|Karakterlánc|hello alkalmazás hello leírása.|  
|URL-cím|URI|hello URI hello alkalmazáshoz.|  
|Verzió|Karakterlánc|Hello alkalmazás fájlverzió-információkat.|  
|Követelmények|Karakterlánc|Hello alkalmazás leírását.|  
|Állapot|Szám|hello hello alkalmazás aktuális állapota.<br /><br /> -0 - regisztrálva<br /><br /> -1 - elküldése megtörtént<br /><br /> -Közzétett 2-<br /><br /> -3 - elutasítva<br /><br /> – 4 – közzé nem tett|  
|RegistrationDate|Dátum és idő|hello dátum és idő hello alkalmazás lett regisztrálva.|  
|CategoryId|Szám|hello kategória hello alkalmazás (pénzügyi, szórakozás stb.)|  
|DeveloperId|Karakterlánc|hello hello fejlesztői küldő hello alkalmazás egyedi azonosítója.|  
|Mellékletek|A gyűjtemény [melléklet](#Attachment) entitásokat.|A mellékletek például képernyőképeket vagy ikonok hello alkalmazáshoz.|  
|Ikon|[Melléklet](#Attachment)|hello ikon hello hello alkalmazáshoz.|  
  
##  <a name="Attachment"></a>Melléklet  
 Hello `attachment` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Egyedi azonosító|Karakterlánc|hello hello melléklet egyedi azonosítója.|  
|URL-cím|Karakterlánc|hello hello erőforrás URL-címe|  
|Típus|Karakterlánc|melléklet hello típusa|  
|A ContentType|Karakterlánc|adathordozó-típusát hello hello mellékletet.|  
  
##  <a name="Sample"></a>Kódminta  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Cím|Karakterlánc|hello művelet hello neve.|  
|részlet|Karakterlánc|Ez a tulajdonság elavult, és nem használható.|  
|Ecset|Karakterlánc|Hello kódminta megjelenítésekor használt sablon toobe színátmenetekhez szintaxis kódokat. Két érték engedélyezett `plain`, `php`, `java`, `xml`, `objc`, `python`, `ruby`, és `csharp`.|  
|sablon|Karakterlánc|a kód a minta sablon hello nevét.|  
|Törzs|Karakterlánc|Hello kód a minta része hello részlet helyőrzője.|  
|Módszer|Karakterlánc|hello hello művelet HTTP-metódus.|  
|séma|Karakterlánc|hello protokoll toouse hello művelet kérelem.|  
|Elérési út|Karakterlánc|hello művelet hello elérési útját.|  
|lekérdezés|Karakterlánc|Lekérdezési karakterlánc például a megadott paraméterekkel.|  
|állomás|Karakterlánc|hello URL-címe hello API-kezelés szolgáltatás átjáró hello API, amely tartalmazza ezt a műveletet.|  
|Fejlécek|A gyűjtemény [fejléc](#Header) entitásokat.|Fejlécek ehhez a művelethez.|  
|paraméterek|A gyűjtemény [paraméter](#Parameter) entitásokat.|Ehhez a művelethez megadott paraméterek.|  
  
##  <a name="Comment"></a>Megjegyzés  
 Hello `API` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Szám|hello Megjegyzés hello azonosítója.|  
|CommentText|Karakterlánc|hello Megjegyzés hello törzsét. HTML is tartalmazhat.|  
|DeveloperCompany|Karakterlánc|hello fejlesztői hello vállalat neve.|  
|PostedOn|Dátum és idő|hello dátum és idő hello Megjegyzés közzétéve.|  
  
##  <a name="Issue"></a>A probléma  
 Hello `issue` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Karakterlánc|hello hello probléma egyedi azonosítója.|  
|ApiID|Karakterlánc|hello azonosítója, amelynek a probléma történt a következő hello API.|  
|Cím|Karakterlánc|Hello probléma címét.|  
|Leírás|Karakterlánc|Hello probléma leírása.|  
|SubscriptionDeveloperName|Karakterlánc|Utónév hello fejlesztő hello hibát jelentett.|  
|IssueState|Karakterlánc|hello hello probléma aktuális állapotát. A lehetséges értékek a következők: Proposed, Opened, lezárva.|  
|ReportedOn|Dátum és idő|hello dátum és idő hello probléma történt.|  
|Megjegyzések|A gyűjtemény [Megjegyzés](#Comment) entitásokat.|A probléma a megjegyzéseit.|  
|Mellékletek|A gyűjtemény [melléklet](api-management-template-data-model-reference.md#Attachment) entitásokat.|Minden mellékletek toohello kapcsolatos probléma.|  
|Szolgáltatások|A gyűjtemény [API](#API) entitásokat.|API-k hello előfizetett tooby hello felhasználó hello probléma bejegyezve.|  
  
##  <a name="Filtering"></a>Szűrés  
 Hello `filtering` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Minta|Karakterlánc|aktuális keresési kifejezés hello; vagy `null` nincs keresési kifejezés esetén.|  
|Helyőrző|Karakterlánc|hello szöveg toodisplay hello keresőmezőbe nincs megadott keresési kifejezés esetén.|  
  
##  <a name="Header"></a>Fejléc  
 Ez a szakasz ismerteti a hello `parameter` ábrázolását.  
  
|Tulajdonság|Leírás|Típus|  
|--------------|-----------------|----------|  
|név|Karakterlánc|A paraméter neve.|  
|leírás|Karakterlánc|A paraméter leírását.|  
|érték|Karakterlánc|Fejléc értéke.|  
|A TypeName értéke|Karakterlánc|Állomásfejléc-érték adattípusa.|  
|beállítások|Karakterlánc|Beállítások.|  
|Szükséges|Logikai érték|E hello fejléc szükség.|  
|csak olvasható|Logikai érték|E hello fejléc csak olvasható.|  
  
##  <a name="HTTPRequest"></a>HTTP-kérelem  
 Ez a szakasz ismerteti a hello `request` ábrázolását.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|leírás|Karakterlánc|A művelet kérelem leírása.|  
|Fejlécek|a tömb [fejléc](#Header) entitásokat.|Kérelem fejlécei.|  
|paraméterek|a tömb [paraméter](#Parameter)|A művelet a kérelemben szereplő paraméterek gyűjteménye.|  
|felelősséget|a tömb [ábrázolása](#Representation)|Művelet kérelem felelősséget gyűjteménye.|  
  
##  <a name="HTTPResponse"></a>HTTP-válasz  
 Ez a szakasz ismerteti a hello `response` ábrázolását.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|statusCode|Pozitív egész szám|A művelet válasz állapotkódja.|  
|leírás|Karakterlánc|A művelet válasz leírása.|  
|felelősséget|a tömb [ábrázolása](#Representation)|Művelet válasz felelősséget gyűjteménye.|  
  
##  <a name="Operation"></a>Művelet  
 Hello `operation` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|id|Karakterlánc|Erőforrás-azonosítója. Hello művelet hello aktuális API Management szolgáltatáspéldány belül egyedi azonosítására szolgál. hello értéke hello formátuma érvényes relatív URL- `apis/{aid}/operations/{id}` ahol `{aid}` API azonosító és `{id}` művelet azonosítója. Ez a tulajdonság csak olvasható.|  
|név|Karakterlánc|Hello művelet neve. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|leírás|Karakterlánc|Hello művelet leírása. Nem lehet üres. HTML-címkék formázás is tartalmazhat. Hossza legfeljebb 1000 karakter lehet.|  
|séma|Karakterlánc|Ismerteti, hogy mely protokollok hello a műveletek az API-ban is elindítható. Két érték engedélyezett `http`, `https`, vagy mindkettőt `http` és `https`.|  
|uriTemplate|Karakterlánc|Relatív URL-cím sablon azonosítása hello célerőforrása ehhez a művelethez. Paraméterek is tartalmazhat. Példa:`customers/{cid}/orders/{oid}/?date={date}`|  
|állomás|Karakterlánc|hello API Management átjáró URL-címe, amelyen hello API.|  
|HttpMethod|Karakterlánc|Művelet HTTP-metódus.|  
|Kérelem|[HTTP-kérelem](#HTTPRequest)|Kérelem részleteit tartalmazó entitás.|  
|válaszok|a tömb [HTTP-válasz](#HTTPResponse)|Művelet tömbje [HTTP-válasz](#HTTPResponse) entitásokat.|  
  
##  <a name="Menu"></a>Művelet menü  
 Hello `operation menu` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|apiId|Karakterlánc|hello aktuális API hello azonosítója.|  
|CurrentOperationId|Karakterlánc|hello azonosítója hello aktuális műveletet.|  
|Műveletek|Karakterlánc|hello menüben írja be.|  
|MenuItems|A gyűjtemény [művelet menüpont](#MenuItem) entitásokat.|hello műveletek hello aktuális API-hoz.|  
  
##  <a name="MenuItem"></a>A művelet menüpont  
 Hello `operation menu item` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Karakterlánc|hello művelet hello azonosítója.|  
|Cím|Karakterlánc|hello művelet hello leírása.|  
|HttpMethod|Karakterlánc|hello hello művelet Http-metódus.|  
  
##  <a name="Paging"></a>Lapozófájl  
 Hello `paging` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Lap|Szám|aktuális oldalszám hello.|  
|PageSize együttes értéke|Szám|egy oldalon megjelenő eredmények maximális toobe hello.|  
|TotalItemCount|Szám|a megjelenített elemek hello száma.|  
|ShowAll|Logikai érték|E összes toosho okoz egy oldalon.|  
|PageCount|Szám|az eredmények lapok hello száma.|  
  
##  <a name="Parameter"></a>A paraméter  
 Ez a szakasz ismerteti a hello `parameter` ábrázolását.  
  
|Tulajdonság|Leírás|Típus|  
|--------------|-----------------|----------|  
|név|Karakterlánc|A paraméter neve.|  
|leírás|Karakterlánc|A paraméter leírását.|  
|érték|Karakterlánc|A paraméter értékét.|  
|beállítások|A karakterlánc tömbje|Lekérdezési paraméter értéke a definiált értékekkel.|  
|Szükséges|Logikai érték|Meghatározza, hogy paraméter szükség-e vagy sem.|  
|típusa|Szám|Hogy a paraméter egy elérési utat (1), vagy egy lekérdezési karakterlánc paraméter (2).|  
|A TypeName értéke|Karakterlánc|Paraméter típusa.|  
  
##  <a name="Product"></a>A termék  
 Hello `product` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Karakterlánc|Erőforrás-azonosítója. Hello termék aktuális API Management szolgáltatáspéldány hello belül egyedi azonosítására szolgál. hello értéke hello formátuma érvényes relatív URL- `products/{pid}` ahol `{pid}` termék azonosítója. Ez a tulajdonság csak olvasható.|  
|Cím|Karakterlánc|Hello termék nevét. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|Leírás|Karakterlánc|Hello termék leírása. Nem lehet üres. HTML-címkék formázás is tartalmazhat. Hossza legfeljebb 1000 karakter lehet.|  
|Fogalmak|Karakterlánc|A termék használati feltételeket. Toosubscribe toohello termék próbált fejlesztők számára jelenik meg, és a szükséges tooaccept hello előfizetési folyamat végrehajtása előtt ezeket a feltételeket.|  
|ProductState|Szám|Meghatározza, hogy hello termék van közzétéve, vagy nem. Közzétett termékeket felderíthető fejlesztők hello developer portálon. A nem közzétett termékek látható csak tooadministrators.<br /><br /> termék állapota hello megengedett értékei a következők:<br /><br /> - `0 - Not Published`<br /><br /> - `1 - Published`<br /><br /> - `2 - Deleted`|  
|AllowMultipleSubscriptions|Logikai érték|Megadja, hogy egy felhasználó több előfizetések toothis termék is rendelkeznek: hello ugyanannyi időt vesz igénybe.|  
|MultipleSubscriptionsCount|Szám|előfizetések toothis termék hello aktuális felhasználó hello száma.|  
  
##  <a name="Provider"></a>Szolgáltató  
 Hello `provider` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Tulajdonságok|karakterlánc szótár|A hitelesítésszolgáltató tulajdonságai.|  
|AuthenticationType|Karakterlánc|hello szolgáltató típusa. (Az azure Active Directory, bejelentkezés Facebook, Google-fiók, a Microsoft Account, Twitter).|  
|Felirat|Karakterlánc|Hello szolgáltató megjelenített neve.|  
  
##  <a name="Representation"></a>Megjelenítésre  
 Ez a szakasz ismerteti a `representation`.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|A ContentType|Karakterlánc|Adja meg a regisztrált vagy egyéni content-type a ábrázolását, pl. `application/xml`.|  
|minta|Karakterlánc|Példa hello ábrázolását.|  
  
##  <a name="Subscription"></a>Előfizetés  
 Hello `subscription` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Karakterlánc|Erőforrás-azonosítója. Hello előfizetés aktuális API Management szolgáltatáspéldány hello belül egyedi azonosítására szolgál. hello értéke hello formátuma érvényes relatív URL- `subscriptions/{sid}` ahol `{sid}` egy előfizetés-azonosító. Ez a tulajdonság csak olvasható.|  
|Termékazonosító|Karakterlánc|hello termék erőforrás-azonosítója hello termék előfizetője. hello értéke hello formátuma érvényes relatív URL- `products/{pid}` ahol `{pid}` termék azonosítója.|  
|ProductTitle|Karakterlánc|Hello termék nevét. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|ProductDescription|Karakterlánc|Hello termék leírása. Nem lehet üres. HTML-címkék formázás is tartalmazhat. Hossza legfeljebb 1000 karakter lehet.|  
|ProductDetailsUrl|Karakterlánc|Relatív URL-cím toohello termék részletei.|  
|state|Karakterlánc|hello előfizetés hello állapotát. Lehetséges állapota van:<br /><br /> - `0 - suspended`– hello előfizetés le van tiltva, és hello előfizető hello termék bármely API nem hívható meg.<br /><br /> - `1 - active`– hello előfizetés már aktív.<br /><br /> - `2 - expired`– hello előfizetés elérte a lejárat és inaktiváltuk.<br /><br /> - `3 - submitted`– hello előfizetés kérelem hello fejlesztő történt, de még nem jóváhagyták vagy elutasították.<br /><br /> - `4 - rejected`– hello előfizetés kérelem rendszergazda meg lett tagadva.<br /><br /> - `5 - cancelled`– hello előfizetés hello fejlesztői vagy a rendszergazda megszakította.|  
|displayName|Karakterlánc|Hello előfizetéshez tartozó megjelenített név.|  
|CreatedDate|Dátum és idő|hello dátum hello előfizetés létrejött, az ISO 8601 formátum: `2014-06-24T16:25:00Z`.|  
|CanBeCancelled|Logikai érték|E hello előfizetés törölni lehessen hello aktuális felhasználó.|  
|IsAwaitingApproval|Logikai érték|E hello előfizetés jóváhagyására vár.|  
|A StartDate|Dátum és idő|hello kezdetének hello az előfizetéshez, az ISO 8601 formátum: `2014-06-24T16:25:00Z`.|  
|ExpirationDate|Dátum és idő|hello lejárati dátum hello az előfizetéshez, ISO 8601 formátum: `2014-06-24T16:25:00Z`.|  
|NotificationDate|Dátum és idő|hello értesítési dátum hello az előfizetéshez, ISO 8601 formátum: `2014-06-24T16:25:00Z`.|  
|primaryKey|Karakterlánc|hello előfizetés elsődleges kulcs. Hossza legfeljebb 256 karakter lehet.|  
|másodlagos kulcs|Karakterlánc|hello másodlagos előfizetés kulcs. Hossza legfeljebb 256 karakter lehet.|  
|CanBeRenewed|Logikai érték|E hello előfizetés is megújítani az aktuális felhasználó hello.|  
|HasExpired|Logikai érték|E hello előfizetése lejárt.|  
|IsRejected|Logikai érték|E hello előfizetés kérelem visszautasítva.|  
|CancelUrl|Karakterlánc|hello relatív URL-cím toocancel hello előfizetés.|  
|RenewUrl|Karakterlánc|hello relatív URL-cím toorenew hello előfizetés.|  
  
##  <a name="SubscriptionSummary"></a>Előfizetés összefoglaló  
 Hello `subscription summary` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Azonosító|Karakterlánc|Erőforrás-azonosítója. Hello előfizetés aktuális API Management szolgáltatáspéldány hello belül egyedi azonosítására szolgál. hello értéke hello formátuma érvényes relatív URL- `subscriptions/{sid}` ahol `{sid}` egy előfizetés-azonosító. Ez a tulajdonság csak olvasható.|  
|displayName|Karakterlánc|hello megjelenített hello előfizetés neve|  
  
##  <a name="UserAccountInfo"></a>Felhasználói fiók adatainak  
 Hello `user account info` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|Utónév|Karakterlánc|Utónév. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|Vezetéknév|Karakterlánc|Utolsó neve. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|E-mail cím|Karakterlánc|E-mail cím. Nem lehet üres, és hello szolgáltatáspéldány belül egyedieknek kell lenniük. Maximális hossza 254 karakter.|  
|Jelszó|Karakterlánc|Felhasználói fiók jelszavát.|  
|NameIdentifier|Karakterlánc|Fiókazonosító, hello ugyanaz, mint a hello felhasználói e-mailt.|  
|ProviderName|Karakterlánc|Hitelesítési szolgáltató neve.|  
|IsBasicAccount|Logikai érték|Értéke TRUE, ha ez a fiók regisztrálták az e-mailek és a jelszó; hamis, ha hello fiók regisztrálták szolgáltató használatával.|  
  
##  <a name="UseSignIn"></a>Felhasználói bejelentkezés  
 Hello `user sign in` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|E-mail cím|Karakterlánc|E-mail cím. Nem lehet üres, és hello szolgáltatáspéldány belül egyedieknek kell lenniük. Maximális hossza 254 karakter.|  
|Jelszó|Karakterlánc|Felhasználói fiók jelszavát.|  
|ReturnUrl|Karakterlánc|hello lap URL-címe hello ahol hello felhasználó kattintott a bejelentkezés.|  
|RememberMe|Logikai érték|E toosave hello a jelenlegi felhasználó adatait.|  
|RegistrationEnabled|Logikai érték|Regisztrációs engedélyezve van-e.|  
|DelegationEnabled|Logikai érték|Delegált bejelentkezés engedélyezve van-e.|  
|DelegationUrl|Karakterlánc|hello meghatalmazott bejelentkezési URL-címben, ha engedélyezve van.|  
|SsoSignUpUrl|Karakterlánc|egyetlen hello bejelentkezéskor URL-cím hello felhasználó, ha van ilyen.|  
|AuxServiceUrl|Karakterlánc|Ha hello aktuális felhasználó a rendszergazda, hello klasszikus Azure portál egy hivatkozás toohello szolgáltatáspéldány.|  
|Szolgáltatók|A gyűjtemény [szolgáltató](#Provider) entitások|a felhasználó hello hitelesítésszolgáltatókat.|  
|UserRegistrationTerms|Karakterlánc|A felhasználó hozzá kell járulniuk bejelentkezés toobefore kifejezéssel.|  
|UserRegistrationTermsEnabled|Logikai érték|E kifejezések engedélyezettek.|  
  
##  <a name="UserSignUp"></a>Felhasználói bejelentkezési  
 Hello `user sign up` entitás hello alábbi tulajdonságokkal rendelkezik.  
  
|Tulajdonság|Típus|Leírás|  
|--------------|----------|-----------------|  
|PasswordConfirm|Logikai érték|Hello által használt érték [előfizetési](api-management-page-controls.md#sign-up)előfizetési vezérlő.|  
|Jelszó|Karakterlánc|Felhasználói fiók jelszavát.|  
|PasswordVerdictLevel|Szám|Hello által használt érték [előfizetési](api-management-page-controls.md#sign-up)előfizetési vezérlő.|  
|UserRegistrationTerms|Karakterlánc|A felhasználó hozzá kell járulniuk bejelentkezés toobefore kifejezéssel.|  
|UserRegistrationTermsOptions|Szám|Hello által használt érték [előfizetési](api-management-page-controls.md#sign-up)előfizetési vezérlő.|  
|ConsentAccepted|Logikai érték|Hello által használt érték [előfizetési](api-management-page-controls.md#sign-up)előfizetési vezérlő.|  
|E-mail cím|Karakterlánc|E-mail cím. Nem lehet üres, és hello szolgáltatáspéldány belül egyedieknek kell lenniük. Maximális hossza 254 karakter.|  
|Utónév|Karakterlánc|Utónév. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|Vezetéknév|Karakterlánc|Utolsó neve. Nem lehet üres. Hossza legfeljebb 100 karakter lehet.|  
|Felhasználói adatok|Karakterlánc|Hello által használt érték [előfizetési](api-management-page-controls.md#sign-up) vezérlő.|  
|NameIdentifier|Karakterlánc|Hello által használt érték [előfizetési](api-management-page-controls.md#sign-up)előfizetési vezérlő.|  
|ProviderName|Karakterlánc|Hitelesítési szolgáltató neve.|

## <a name="next-steps"></a>Következő lépések
A sablonok használatának kapcsolatos további információkért lásd: [hogyan toocustomize hello API Management fejlesztői portálján sablonokkal](api-management-developer-portal-templates.md).

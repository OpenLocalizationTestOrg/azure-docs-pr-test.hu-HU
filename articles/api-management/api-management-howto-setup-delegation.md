---
title: "aaaHow toodelegate felhasználói regisztráció és a termék előfizetés"
description: "Ismerje meg, hogyan toodelegate felhasználói regisztráció és a termék előfizetés tooa harmadik fél az Azure API Management."
services: api-management
documentationcenter: 
author: antonba
manager: erikre
editor: 
ms.assetid: 8b7ad5ee-a873-4966-a400-7e508bbbe158
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 406648db2d2f37c4dcda466294726d331cc0551b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodelegate-user-registration-and-product-subscription"></a>Hogyan toodelegate felhasználói regisztráció és a termék-előfizetéshez
Delegálás toouse lehetővé teszi a meglévő webhely, bejelentkezési-a/regisztrációs és az előfizetés tooproducts fejlesztői kezelése ellenezte toousing hello beépített funkcióval hello developer portálon. Ez lehetővé teszi, hogy a webhely tooown hello felhasználói adatokat, és ezeket a lépéseket hello ellenőrzés végrehajtása egy egyedi módon.

## <a name="delegate-signin-up"></a>Delegálása fejlesztői bejelentkezési és regisztrációs
toodelegate fejlesztői bejelentkezési és regisztrációs tooyour létező webhely toocreate egy különös delegálás végpont szüksége lesz a webhelyen, eleget kell hello hello API Management developer portálról kezdeményezett kérelem belépési pontját.

hello végső munkafolyamat a következők:

1. A hello hello bejelentkezés vagy regisztráció hivatkozására kattint fejlesztői API Management fejlesztői portálján
2. Böngésző átirányított toohello delegálás végpont
3. Delegálás végpont ismét tooor megadja felhasználói felület azzal a kérdéssel felhasználói toosign a vagy előfizetési átirányítja a felhasználókat
4. Siker hello felhasználói átirányított hátsó toohello API Management developer portálon a elkezdett

toobegin, most első beállításról API Management tooroute kérelmek delegálás végpontot keresztül. Hello API Management publisher portál, kattintson a **biztonsági** majd hello **delegálás** fülre. Kattintson a hello jelölőnégyzet tooenable "Delegált & regisztráció, bejelentkezés".

![Delegálási lapra][api-management-delegation-signin-up]

* Döntse el, mi a különleges delegálás végpont URL-címe hello fog kell, és írja be hello **delegálás végponti URL-cím** mező. 
* Hello belül **delegálás hitelesítési kulcs** mezőbe írja be a titkos kulcs, amely egy megadott aláírás tooyou az ellenőrzési tooensure, amely hello kérelem valóban érkezik Azure API Management használt toocompute lesz. Kattinthat a hello **készítése** gomb toohave API felügyeleti véletlenszerű előállításához a kulcsot meg.

Most toocreate hello **delegálás végpont**. Többféle lépéssel rendelkezik tooperform:

1. Egy kérést kap, a következő képernyő hello:
   
   > *{lap URL-címe forrás} http://www.yourwebsite.com/apimdelegation?Operation=SignIn&returnUrl= & védőérték = {karakterlánc} & sig = {karakterlánc}*
   > 
   > 
   
    Lekérdezési paraméterek hello / regisztrációs, bejelentkezési esethez:
   
   * **a művelet**: delegálás kérés – csak azok milyen típusú azonosítja **SignIn** ebben az esetben
   * **returnUrl**: hello ahol hello felhasználói bejelentkezés vagy regisztráció hivatkozásra kattintott hello lap URL-címe
   * **védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc
   * **SIG**: egy számított biztonsági kivonatoló toobe összehasonlító tooyour saját használt számított kivonata
2. Győződjön meg arról, hogy hello kérelem érkezik Azure API Management (nem kötelező, de erősen ajánlott a biztonság)
   
   * Egy karakterlánc alapján hello HMAC-SHA512 kivonatának számítási **returnUrl** és **védőérték** lekérdezési paramétert ([alábbi példakód]):
     
     > HMAC (**védőérték** + "\n" + **returnUrl**)
     > 
     > 
   * Hasonlítsa össze a hello fent számított toohello kivonatértéke hello **sig** lekérdezési paraméter. Hello két kivonatok megegyeznek, ha áthelyezni a következő lépés toohello, ellenkező esetben a hello kérelem elutasítása.
3. Győződjön meg arról, hogy fordulnak elő a bejelentkezési/sign up kérelmet: hello **művelet** lekérdezésparaméter lesz beállítva, túl "**SignIn**".
4. Jelen hello felhasználói vagy előfizetési toosign a felhasználói felületen
5. Ha hello felhasználó regisztrált toocreate megfelelő fiókkal rendelkező számukra az API Management. [Hozzon létre egy felhasználót] a hello API Management REST API-t. Ha így arról, hogy ugyanaz a felhasználókhoz tartozó tárolóban van, vagy akkor is nyomon követésére szolgáló tooan azonosító beállítva hello felhasználói azonosító toohello.
6. Ha a hello felhasználó sikeresen hitelesített:
   
   * [egyszeri bejelentkezéses (SSO) jogkivonatot kérni] hello API Management REST API-n keresztül
   * hozzáfűzése egy returnUrl lekérdezési paraméter toohello egyszeri bejelentkezési URL-címet, a fenti hello API-hívás érkezett:
     
     > pl. https://customer.portal.azure-api.net/signin-sso?token&returnUrl=/return/url 
     > 
     > 
   * az átirányítási hello felhasználói toohello fenti előállított URL-címe

A hozzáadása toohello **SignIn** művelet is végrehajthat fiókkezelés hello előző lépések és hello a következő műveletek egyikével.

* **A ChangePassword**
* **ChangeProfile**
* **CloseAccount**

A következő lekérdezési paraméterek a fiókkezelési műveletekhez hello kell átadni.

* **a művelet**: azonosítja (a ChangePassword, ChangeProfile vagy CloseAccount) delegálás kérelem típusa
* **userId**: hello fiók toomanage hello felhasználó azonosítója
* **védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc
* **SIG**: egy számított biztonsági kivonatoló toobe összehasonlító tooyour saját használt számított kivonata

## <a name="delegate-product-subscription"></a>Termék-előfizetéshez delegálása
Termék-előfizetéshez delegálása működik hasonlóképpen toodelegating felhasználói bejelentkezés/felfelé. hello végső munkafolyamata a következőképpen nézne ki:

1. Fejlesztői termék kiválasztása hello API Management developer portálon, és a hello előfizetés gombra kattint
2. Böngésző átirányított toohello delegálás végpont
3. Delegálás endpoint szükséges termék előfizetés lépéseket végzi el – ez tooyou működik, és átirányítása tooanother lap toorequest számlázási adatokat, további kérdések vagy hello információk tárolásának és felhasználói beavatkozást nem igénylő járhat

tooenable hello funkciói hello **delegálás** kattintson **termék-előfizetéshez delegálása**.

Gondoskodnia kell hello delegálás végpont hello a következő műveleteket hajtja végre:

1. Egy kérést kap, a következő képernyő hello:
   
   > *{művelet} http://www.yourwebsite.com/apimdelegation?Operation= & productId = {termék toosubscribe való} & userId = {a felhasználó kérést} & védőérték = {karakterlánc} & sig = {karakterlánc}*
   > 
   > 
   
    Lekérdezési paraméterek hello termék előfizetés esethez:
   
   * **a művelet**: azonosítja a delegálás kérelem milyen típusú legyen. A termék-előfizetéshez tartozó kérelmek hello érvényes lehetőségek közül választhat:
     * "Előfizetés": a kérés toosubscribe hello felhasználói tooa megadott termék megadott azonosító (lásd alább)
     * "Leiratkozhat": a kérés toounsubscribe a terméket felhasználó
     * "Megújítása": egy requst toorenew előfizetést (pl. is lejár)
   * **productId**: hello hello termék hello felhasználói Azonosítóját a toosubscribe kért
   * **userId**: hello hello felhasználó, akinek hello kérelem azonosítója
   * **védőérték**: biztonsági kivonatát kiszámításához használt különleges védőérték karakterlánc
   * **SIG**: egy számított biztonsági kivonatoló toobe összehasonlító tooyour saját használt számított kivonata
2. Győződjön meg arról, hogy hello kérelem érkezik Azure API Management (nem kötelező, de erősen ajánlott a biztonság)
   
   * Egy karakterlánc alapján hello HMAC-SHA512 számítási **productId**, **userId** és **védőérték** lekérdezési paramétert:
     
     > HMAC (**védőérték** + "\n" + **productId** + "\n" + **userId**)
     > 
     > 
   * Hasonlítsa össze a hello fent számított toohello kivonatértéke hello **sig** lekérdezési paraméter. Hello két kivonatok megegyeznek, ha áthelyezni a következő lépés toohello, ellenkező esetben a hello kérelem elutasítása.
3. A termék feldolgozási hello alapján a kért művelet végrehajtásához **művelet** – pl. számlázást, a további kérdésekre, stb.
4. Sikeresen előfizetés hello felhasználói toohello termék a oldalon, az előfizetés által hello felhasználói toohello API-felügyeleti termék [termék-előfizetéshez tartozó REST API-t hívó hello].

## <a name="delegate-example-code"></a> Példakód
Ezek az minták megjelenítése hogyan code tootake hello *delegálás érvényesítési kulcs*, melynek értéke hello publisher portál hello delegálás képernyőjén, egy HMAC, amely majd toocreate használt toovalidate hello aláírás igazolására hello hello érvényességét átadott returnUrl. hello ugyanazt a kódot működik hello productId és kis mértékben módosított rendelkező felhasználói azonosítóját.

**C# kóddal toogenerate kivonatának returnUrl**

```c#
using System.Security.Cryptography;

string key = "delegation validation key";
string returnUrl = "returnUrl query parameter";
string salt = "salt query parameter";
string signature;
using (var encoder = new HMACSHA512(Convert.FromBase64String(key)))
{
    signature = Convert.ToBase64String(encoder.ComputeHash(Encoding.UTF8.GetBytes(salt + "\n" + returnUrl)));
    // change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
    // compare signature toosig query parameter
}
```

**NodeJS code returnUrl toogenerate kivonata**

```
var crypto = require('crypto');

var key = 'delegation validation key'; 
var returnUrl = 'returnUrl query parameter';
var salt = 'salt query parameter';

var hmac = crypto.createHmac('sha512', new Buffer(key, 'base64'));
var digest = hmac.update(salt + '\n' + returnUrl).digest();
// change too(salt + "\n" + productId + "\n" + userId) when delegating product subscription
// compare signature toosig query parameter

var signature = digest.toString('base64');
```

## <a name="next-steps"></a>Következő lépések
Delegálás további információkért tekintse meg a következő videó hello.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Delegating-User-Authentication-and-Product-Subscription-to-a-3rd-Party-Site/player]
> 
> 

[Delegating developer sign-in and sign-up]: #delegate-signin-up
[Delegating product subscription]: #delegate-product-subscription
[egyszeri bejelentkezéses (SSO) jogkivonatot kérni]: http://go.microsoft.com/fwlink/?LinkId=507409
[felhasználó létrehozása]: http://go.microsoft.com/fwlink/?LinkId=507655#CreateUser
[termék-előfizetéshez tartozó REST API-t hívó hello]: http://go.microsoft.com/fwlink/?LinkId=507655#SSO
[Next steps]: #next-steps
[alábbi példakód]: #delegate-example-code

[api-management-delegation-signin-up]: ./media/api-management-howto-setup-delegation/api-management-delegation-signin-up.png 

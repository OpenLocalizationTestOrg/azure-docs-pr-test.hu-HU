---
title: "Az Azure Active Directory B2C: Használatával nyelvi testreszabási |} Microsoft Docs"
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: sama
ms.openlocfilehash: a8e4037014f5adf929dac7b5dc4db72ba0f5b292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Az Azure Active Directory B2C: Használatával nyelvi testreszabása

>[!NOTE] 
>A funkció jelenleg nyilvános előzetes verziójához.  Javasoljuk, hogy ez a funkció használata esetén használjon egy tesztelési bérlőn.  Nem tervezzük az hello előzetes toohello általánosan rendelkezésre álló legfrissebb módosításokat, de azt lefoglalni hello jobb toomake ilyen változások tooimprove hello szolgáltatás.  Miután egy alkalommal tootry funkciót, adja meg az Önnek nyújtott élményt visszajelzést, és hogyan tökéletesíthetjük az jobb.  Visszajelzést küldhet hello Azure-portálon keresztül hello arc arcfelismerési eszközzel a hello felső sarokban.   Ha vállalati követelmények miatt meg toogo élő ezzel a szolgáltatással hello preview fázisban, ossza meg velünk az esetek, és azt adja meg a megfelelő hello útmutatást és segítséget.  Akkor is lépjen velünk kapcsolatba [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).

Nyelvi testreszabási lehetővé teszi a felhasználó szállítás tooa különböző nyelvi toosuit a ügyféligények toochange.  Fordítások 36 nyelvhez nyújtunk (lásd: [további információt](#additional-information)).  Akkor is, ha a felhasználói élmény csak egyetlen nyelvet előírt, testre szabhatja hello lapok toosuit szöveget az igényeinek.  

## <a name="how-does-language-customization-work"></a>Hogyan működik a nyelvi testreszabási?
Nyelvi testreszabási lehetővé teszi a tooselect nyelveket a felhasználó használatában érhető el.  Hello szolgáltatás engedélyezése után hello lekérdezési karakterláncot, ui_locales, megadhatja az alkalmazásból.  Az Azure AD B2C hívásakor azt jelezte lap toohello helyi lefordítani.  Típusú konfiguráció használata lehetővé teszi teljes felügyeletet gyakorolhat hello nyelvek a a felhasználók utazás, és figyelmen kívül hagyja a hello ügyfél böngésző hello nyelvi beállításait. Azt is megteheti nem feltétlenül kell, hogy milyen nyelveket, az ügyfél talál jogokat.  Ha nem ad meg egy ui_locales paraméter, hello felhasználói élmény a böngésző beállításait határozza meg.  Akkor is ellenőrizheti, amely a felhasználó használatában nyelveket a lefordított tooby hozzáadná egy támogatott nyelv.  Ha egy ügyfél böngésző set tooshow nyelvet nem szeretné, hogy toosupport, majd egy alapértelmezett támogatott kulturális környezetek helyette ahogy kiválasztott hello nyelv.

1. **felhasználói felület – területi beállításokhoz megadott nyelvi** -nyelvi testreszabásának engedélyezése, a felhasználó használatában-e az itt megadott lefordított toohello nyelv
2. **Böngésző kért nyelv** -határozott meg nem ui-területi beállításokat, ha az eszköz-e toohello böngésző kért nyelv, **Ha szerepelt támogatott nyelvek**
3. **Házirend alapértelmezett nyelv** -hello böngésző nem adja meg a nyelvet, vagy adja meg egy nem támogatott, ha az eszköz-e toohello házirend alapértelmezett nyelv

>[!NOTE]
>Ha a felhasználói egyéni attribútumok használata esetén meg kell tooprovide saját fordítások. Lásd: "[testre szabhatja a karakterláncok](#customize-your-strings)" részleteiről.
>

## <a name="support-uilocales-requested-languages"></a>Támogatási ui_locales kért nyelvek 
Azáltal, hogy a "Nyelvi beállítások" házirend alapján, most vezérelheti hello nyelvi hello felhasználói út hello ui_locales paraméter hozzáadásával.
1. [Kövesse a lépéseket toonavigate toohello B2C funkciók panelje hello Azure-portálon.](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-b2c-settings)
2. Keresse meg a megjeleníteni kívánt tooenable fordítások tooa házirend.
3. Kattintson a **nyelvi testreszabási**.
4. Olvassa el figyelmesen figyelmeztetés hello.  Miután engedélyezte őket, nem kapcsolható ki "Nyelvi testreszabási".
5. Kapcsolja be hello szolgáltatást, és kattintson a **OK**.
6. A házirend mentése a hello bal felső sarkában a **házirend szerkesztése** panelen.

## <a name="select-which-languages-your-user-journey-supports"></a>Válassza ki a felhasználói használatában támogatja nyelveket 
Hozzon létre a felhasználó út toobe, amikor hello ui_locales paraméter nincs megadva lefordítva engedélyezett nyelvek listáját.
1. Győződjön meg arról, a házirend "Nyelvi testreszabási" az előző utasítások engedélyezve van.
2. Az a **házirend szerkesztése** panelen válassza **nyelvi testreszabási**.
3. A következő lépés az tooyour **támogatott nyelv** panelen.  Itt választhatja ki **nyelv hozzáadása**.
4. Minden hello nyelv ki lett választva támogatott toobe szeretné.  

>[!NOTE]
>Ha egy ui_locales paraméter nincs megadva, majd hello lap lefordított toohello az ügyfél a böngésző nyelvének csak akkor, ha szerepel a listán
>

5. Kattintson a **Ok** hello lap alján
6. Bezárás hello **nyelvi testreszabási** panel és **mentése** a házirendet.

## <a name="customize-your-strings"></a>A karakterlánc testreszabása
"Nyelvi testreszabási" lehetővé teszi a felhasználó út bármely karakterlánc toocustomize.
1. Győződjön meg arról, a házirend "Nyelvi testreszabási" hello előző utasítások engedélyezve van.
2. Az a **házirend szerkesztése** panelen válassza **nyelvi testreszabási**.
3. Válassza hello bal oldali navigációs menü **Tartalomletöltéshez**.
4. Válassza ki a kívánt tooedit hello lap.
5. Hello legördülő menüben válassza ki a tooedit kívánt hello nyelvet.
6. Kattintson a **Letöltés** gombra. 

Ezeket a lépéseket egy JSON-fájl használható a karakterlánc szerkesztése toostart biztosítják.

### <a name="changing-any-string-on-hello-page"></a>Bármilyen karakterlánc hello oldalon módosítása
1. Nyissa meg hello JSON-fájl egy JSON-szerkesztőben előző utasítások letölteni.
2. Keresés hello a toochange kívánt elemet.  Hello található `StringId` hello karakterlánc keres, vagy keressen a hello `Value` toochange szeretné.
3. Frissítés hello `Value` attribútum kívánt jelennek meg.
4. Hello fájl mentéséhez, majd töltse fel a módosításokat.

### <a name="changing-extension-attributes"></a>A bővítményattribútumokat módosítása
Ha toochange hello karakterláncot keres egy egyéni felhasználói attribútumot, vagy szeretné, hogy tooadd egy toohello JSON, hello formátuma a következő szerepel:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Ha egy karakterlánc toooverride van szüksége, győződjön meg arról, hogy tooset hello `Override` érték túl`true`.  Hello értéke nem változik meg, ha hello bejegyzés figyelmen kívül hagyja. 
>

Cserélje le `<ExtensionAttribute>` hello nevű az egyéni felhasználói attribútum.  
Cserélje le `<ExtensionAttributeValue>` hello jelenik meg új karakterlánc toobe együtt.

### <a name="using-localizedcollections"></a>LocalizedCollections használatával
A válaszok tooprovide meghatározott számítógéplistán szereplő értékek azt szeretné, ha szüksége van-e toocreate egy `LocalizedCollections`.  A `LocalizedCollections` tömbje `Name` és `Value` párokat.  Hello `Name` mi jelenik meg, és hello `Value` van hello jogcímek adott vissza.  tooadd egy `LocalizedCollections`, formátuma a következő hello rendelkezik:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": false,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```
>[!IMPORTANT]
>Ha egy karakterlánc toooverride van szüksége, győződjön meg arról, hogy tooset hello `Override` érték túl`true`.  Hello értéke nem változik meg, ha hello bejegyzés figyelmen kívül hagyja. 
>

* `ElementId`van hello felhasználói attribútumot, amelyhez a `LocalizedCollections` válasz
* `Name`hello érték látható toohello felhasználó
* `Value`eredményének hello jogcímek amikor ez a beállítás értéke

### <a name="upload-your-changes"></a>A módosítások feltöltéséhez
1. Hello módosítások tooyour JSON-fájl befejezése után lépjen vissza tooyour B2C-bérlő.
2. Az a **házirend szerkesztése** panelen válassza **nyelvi testreszabási**.
3. Válassza hello bal oldali navigációs menü **tartalom feltöltése**.
4. Válassza ki a használni kívánt tooupload a módosításokat a hello oldal.
5. Ha azt szeretné egy nyelv, amely a korábban megadott a JSON tooupload, toodelete hello előző bejegyzést kell.  Hello kattintva törölheti `...` a hello az adott nyelv jobbra, és válassza ki **törlése**.
6. Kattintson a **Hozzáadás** hello felső bal oldalon.
7. Válassza ki a JSON-fájl hello nyelvét.
8. Hello mappa gombra hello megfelelő, majd keresse meg a JSON-fájl.
9. Kattintson a hello **feltöltése** hello panel alján hello gombjára.
10. Lépjen vissza tooyour **házirend szerkesztése** panel megnyitásához, és kattintson **mentése**.

## <a name="additional-information"></a>További információ
### <a name="recommendations-for-language-customization"></a>A testreszabáshoz"nyelv" javaslatok
Azt javasoljuk, hogy csak helyezésének bejegyzések tooyour nyelvi erőforrások karakterláncok, explicit módon kívánja tooreplace.  Méretének korlátját toohello fájl kívül a JSON-fordítások fordítását, akkor azt kényszerítéséhez.  Ha a fájl túl nagy, a felhasználó használatában hello teljesítményének hatással van.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Lap felhasználói felületének testreszabása címkék "Nyelvi testreszabási" engedélyezése után törlődnek.
Ha engedélyezi a "Nyelv testreszabási", a korábbi módosítások címkék használatával lap felhasználói felületének testreszabása felhasználói egyéni attribútumok kivételével a rendszer törli.  Ez a változás történik, tooavoid ütközéseket a karakterláncok szerkesztésére.  Folytatás toochange a címkéket és egyéb karakterláncok nyelvi erőforrások "Nyelvi testreszabás" feltöltésével.
### <a name="microsoft-is-committed-tooprovide-hello-most-up-to-date-translations-for-your-use"></a>Microsoft véglegesített tooprovide hello legfrissebb fordításainak használata
Azt a rendszer folyamatosan fordítások javítására, és tartsa meg a megfelelőség.  Rendszer azonosítsa, hibákat és változások a globális terminológia és zökkenőmentesen teszi hello frissítéseket fog működni a felhasználó használatában.
### <a name="support-for-right-to-left-languages"></a>Jobbról balra író nyelvek támogatása
Nincs jobbról balra író nyelvek esetén nem támogatott, ha szüksége van a szolgáltatás adja ezt a funkciót szavazzon [Azure visszajelzés](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).
### <a name="social-identity-provider-translations"></a>Közösségi Identity provider fordítások
OIDC paraméter toosocial bejelentkezések hello ui_locales nyújtunk, de azok nem fogadja el a néhány közösségi Identitásszolgáltatók, beleértve a Facebook-on és a Google. 
### <a name="browser-behavior"></a>Böngésző viselkedése
Chrome Firefox mindkét változáskérés és a beállított nyelvhez tartozó, és ha egy támogatott nyelvi, hello alapértelmezett előtt megjelenik.  Peremhálózati jelenleg nem kér egy nyelvet, és egyenes toohello alapértelmezett nyelv kerül.
### <a name="known-issues"></a>Ismert problémák
* Nyelvi erőforrások hello többtényezős hitelesítés lap egy profil szerkesztése házirendben feltöltése jelenleg nem érhető el.
* `LocalizedCollections`értékek nem létre, amikor hello választípus miatt szükséges
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Mi történik, ha nem támogatott nyelven szeretnék?
Azt tervezi, ez a szolgáltatás, amely lehetővé teszi egy JSON-erőforrás "egyéni nyelvek" felé tooupload kiterjesztése tooprovide.  hello funkcióval toospecify hello nevét, és a nyelvi kód bármilyen nyelven, és adja meg *összes* hello fordítások az adott nyelven.  Ha ez a funkció van szüksége, küldjön nekünk a forgatókönyvet [ aadb2cpreview@microsoft.com ](mailto:aadb2cpreview@microsoft.com).  

### <a name="what-languages-are-supported"></a>Milyen nyelveket támogatja?

| Nyelv              | Nyelvkód |
|-----------------------|---------------|
| Bangla                | BN            |
| cseh                 | cs            |
| dán                | da            |
| német                | de            |
| görög                 | el            |
| Angol               | en            |
| spanyol               | es            |
| finn               | fi            |
| francia                | fr            |
| gudzsaráti              | Gu            |
| hindi                 | szia            |
| horvát              | HR            |
| magyar             | hu            |
| olasz               | it            |
| japán              | ja            |
| kannada               | KN            |
| koreai                | ko            |
| malajálam             | gépi tanulás            |
| marathi               | MR            |
| maláj                 | MS            |
| Norvég Bokmal      | nb            |
| holland                 | nl            |
| pandzsábi               | szolgáltatói            |
| lengyel                | pl            |
| Portugál - Brazília   | pt-br         |
| Portugál - Portugália | pt-pt         |
| román              | ro            |
| orosz               | ru            |
| szlovák                | SK            |
| svéd               | sv            |
| tamil                 | TA            |
| telugu                | Te            |
| thai                  | TH            |
| török               | TR            |
| Egyszerűsített kínai-  | zh-hans       |
| -Hagyományos kínai | zh-hant       |

---
title: "Az Azure Active Directory B2C: Hivatkozni: hello egyéni házirendek egy felhasználó út felhasználói felület testreszabása |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C egyéni házirendek"
services: active-directory-b2c
documentationcenter: 
author: rojasja
manager: krassk
editor: rojasja
ms.assetid: 
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: joroja
ms.openlocfilehash: 11f2a7575b95a186399d83266850fe44d650371b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hello-ui-of-a-user-journey-with-custom-policies"></a>Hello egyéni házirendek egy felhasználó út felhasználói felület testreszabása

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

> [!NOTE]
> Ez a cikk a felhasználói felület testreszabásával működése, és hogyan tooenable a B2C egyéni házirend használatával hello identitás élmény keretrendszer speciális leírása


Zökkenőmentes felhasználói élményt az kulcs minden üzleti egyéni megoldás. Zökkenőmentes felhasználói élmény, amelyet az eszköz vagy a böngésző, ha a szolgáltatás egy felhasználói út nem lehet megkülönböztetni hello szolgáltatást használják, amely azt jelenti élményt.

## <a name="understand-hello-cors-way-for-ui-customization"></a>A felhasználói felület testreszabásával hello CORS módon ismertetése

Az Azure AD B2C lehetővé teszi az Ön toocustomize hello megjelenését-és-működését felhasználói élmény (UX) hello, amely potenciálisan kiszolgált és az egyéni házirendek segítségével az Azure AD B2C által megjelenített különféle oldalakat.

Erre a célra az Azure AD B2C kód futtatása a felhasználó böngészőben, és használja hello modern és szabványos megközelítés [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/) tooload egyéni tartalmat egy adott URL-címet egy egyéni házirendek toopoint tooyour HTML5/CSS sablonok. A CORS egy olyan mechanizmus, amely lehetővé teszi a korlátozott erőforrásokat, például a betűtípusokat, a másik tartományban, amelyből származik hello erőforrás hello tartományon kívülről a kért weblap toobe.

Toohello régi hagyományos módon, ahol a sablon lap tulajdonosa hello megoldás, ha korlátozott szöveget a megadott és lemezképeket, ahol a elrendezés és érzetéhez korlátozott felügyeletét felkínált vezető toomore mint nehézségek tooachieve zökkenőmentes élményt, hello CORS összehasonlítása támogatja a HTML5-ös és a CSS és teszi lehetővé:

- Hello tartalom tárolásához és hello megoldás esetében az ügyféloldali parancsfájl használatával szabályozza.
- Minden képpont elrendezés és a kezelőfelület teljes hozzáféréssel rendelkeznek.

Megadhatja, hogy megfelelő HTML5/CSS fájlok létrehozásával tetszés szerinti számú tartalom oldal.

> [!NOTE]
> Biztonsági okokból hello JavaScript jelenleg blokkolva van a Testreszabás. a JavaScript használatát egy egyéni tartománynevet, az Azure AD B2C-bérlő toounblock van szükség.

Az egyes HTML5/CSS-sablonok, adja meg egy *horgonyzási* elem, amely megfelel a szükséges toohello `<div id=”api”>` elem hello HTML vagy hello tartalomlapokon, a továbbiakban bemutatására. Az Azure AD B2C megköveteli, hogy minden tartalom oldal az adott div.

```
<!DOCTYPE html>
<html>
  <head>
    <title>Your page content’s tile!</title>
  </head>
  <body>
    <div id="api"></div>
  </body>
</html>
```

Az Azure AD B2C kapcsolatos tartalom a hello lapon fog tölthető be a nézetmodellbe a div közben hello rest hello lap valóban az Öné toocontrol. az Azure AD B2C hello JavaScript-kód lekéri a tartalmat a, és a HTML be az adott div elem esetében. Az Azure AD B2C hello vezérlők szükség szerint a következő esetében: kiválasztásakor vezérlő, a bejelentkezési vezérlők, a multi-factor Authentication (jelenleg phone-alapú) vezérlők és az attribútum gyűjtemény vezérlők fiók. Az Azure AD B2C biztosítja, hogy minden hello vezérlő HTML5 megfelelő, és elérhető, vezérlőkhöz hello teljesen stílussal, és, hogy a vezérlő verzió nem lesz amelyikre.

hello egyesített tartalmat végül jelenik meg hello dinamikus dokumentum tooyour fogyasztói.

a fenti hello tooensure megfelelően működik-e, be kell:

- Győződjön meg arról, a tartalma HTML5 a szabályzatnak megfelelő és érhető el
- Győződjön meg arról, a tartalomkiszolgáló CORS engedélyezve van.
- HTTPS-KAPCSOLATON keresztül tartalmat szolgáltat.
- Használjon abszolút URL-CÍMEI, például a https://yourdomain/content hivatkozások és a CSS-tartalom.

> [!TIP]
> amely a tartalom üzemeltet a hely hello tooverify CORS engedélyezve van, és tesztelése a CORS-kérelmeket, hello hely http://test-cors.org/ használja. Köszönjük, hogy toothis hely, akkor egyszerűen vagy elküldhető hello CORS kérelem tooa távoli kiszolgálóhoz (ha támogatott CORS tootest), vagy az hello CORS kérelem tooa tesztkiszolgálón (tooexplore a CORS bizonyos funkcióinak).

> [!TIP]
> hello hely http://enable-cors.org/ is jelent a több, mint a CORS hasznos erőforrásokhoz.

Köszönhetően toothis CORS-alapú módszer, hello végfelhasználók majd lesz konzisztens lép az alkalmazás és az Azure AD B2C által kiszolgált hello lapok között.

## <a name="create-a-storage-account"></a>Create a storage account

Előfeltételként kell toocreate egy tárfiókot. Szüksége lesz egy Azure-előfizetés toocreate Azure Blob Storage-fiók. Regisztrálhat egy ingyenes próbaverziót: hello [Azure-webhelyen](https://azure.microsoft.com/en-us/pricing/free-trial/).

1. Nyissa meg a böngésző munkamenetet, és keresse meg a toohello [Azure-portálon](https://portal.azure.com).
2. Jelentkezzen be rendszergazdai hitelesítő adataival.
3. Kattintson a **új** > **adatok + tárolás** > **tárfiók**.  A **storage-fiók létrehozása** panel megnyílik.
4. A **neve**, nevezze el a hello tárfiókot, például *contoso369b2c*. Ez az érték későbbi fog hivatkozni, túl*storageAccountName*.
5. Válassza ki a megfelelő eltávolításra hello hello tarifacsomag, hello erőforráscsoport és hello előfizetés. Győződjön meg arról, hogy rendelkezik-e hello **PIN-kód tooStartboard** beállítás be van jelölve. Kattintson a **Create** (Létrehozás) gombra.
6. Lépjen vissza a Kezdőpulton toohello, és kattintson az újonnan létrehozott tárfiók hello.
7. A hello **szolgáltatások** kattintson **Blobok**. A **Blob szolgáltatás panelre** megnyílik.
8. Kattintson a **+ tároló**.
9. A **neve**, nevezze el a hello tárolóhoz, például *b2c*. Ez az érték későbbi hivatkozott tooas lesz *containerName*.
9. Válassza ki **Blob** , hello **hozzáférési típus**. Kattintson a **Create** (Létrehozás) gombra.
10. hello tároló létrehozott megjelennek a hello hello listában **Blob szolgáltatás panelre**.
11. Bezárás hello **Blobok** panelen.
12. A hello **storage-fiók panelen**, kattintson a hello **kulcs** ikonra. Egy **elérési kulcsok paneljén** megnyílik.  
13. Írja le hello értékének **key1**. Ez az érték későbbi fog hivatkozni *key1*.

## <a name="downloading-hello-helper-tool"></a>Hello segédeszköze letöltése

1.  Töltse le a hello segédeszköze [GitHub](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).
2.  Mentse a hello *B2C-AzureBlobStorage-ügyfél-master.zip* fájlt a helyi számítógépen.
3.  Bontsa ki a helyi lemezen, például a hello hello B2C-AzureBlobStorage-ügyfél-master.zip fájl hello tartalmat **UI-testreszabási-csomag** mappát. Ezzel létrehoz egy *B2C-AzureBlobStorage-ügyfél-főkiszolgáló* mindenképpen a mappában.
4.  Nyissa meg a mappát, és bontsa ki a hello archív fájl tartalma hello *B2CAzureStorageClient.zip* belül.

## <a name="upload-hello-ui-customization-pack-sample-files"></a>Hello UI-testreszabási-csomag minta fájlok feltöltése

1.  A Windows Intézővel keresse meg a toohello mappa *B2C-AzureBlobStorage-ügyfél-főkiszolgáló* alatt hello *UI-testreszabási-csomag* hello előző szakaszban létrehozott mappába.
2.  Futtassa a hello *B2CAzureStorageClient.exe* fájlt. A program egyszerűen fel kell töltenie valamennyi hello fájlt hello tooyour tárfiók adja meg, és engedélyezze a hozzáférést a CORS azokat a fájlokat.
3.  Amikor a rendszer kéri, adja meg: egy.  a tárfiók neve hello *storageAccountName*, például *contoso369b2c*.
    b.  hello elsődleges elérési kulcsát az azure blob storage *key1*, például *contoso369b2c*.
    c.  a blob storage tárolót, hello neve *containerName*, például *b2c*.
    d.  hello elérési útja hello *Starter-csomag* fájlok, például minta *... \B2CTemplates\wingtiptoys*.

Ha követte a fenti hello lépéseket, hello hello HTML5 és CSS fájlok *UI-testreszabási-csomag* hello fiktív vállalat **tartománynév** lesz most kell mutató tooyour tárfiók.  Ellenőrizheti, hogy hello tartalom feltöltötte megfelelően hello Azure-portálon hello kapcsolódó tároló panel megnyitásával. Másik lehetőségként ellenőrizheti, hogy hello tartalom feltöltötte megfelelően böngészőből hello lap elérésével. További információkért lásd: [Azure Active Directory B2C: egy segédeszköze használt toodemonstrate hello lap felhasználói felület (UI) testreszabási szolgáltatás](active-directory-b2c-reference-ui-customization-helper-tool.md).

## <a name="ensure-hello-storage-account-has-cors-enabled"></a>Győződjön meg arról hello storage-fiók rendelkezik a CORS engedélyezése

A CORS (eltérő eredetű erőforrások megosztása) kell lennie a végponton, az Azure AD B2C prémium tooload engedélyezve van a tartalom. Ennek oka az, a tartalmak tárolása más tartományban található, mint a hello tartományhoz az Azure AD B2C prémium fog szolgáltató hello oldal.

tooverify rendelkező hello tárolási üzemeltet a tartalmat a CORS engedélyezése mellett, folytassa a hello a következő lépéseket:

1. Nyissa meg a böngésző munkamenetet, és keresse meg a toohello lap *unified.html* hello teljes URL-cím segítségével a hely a tárfiókban lévő `https://<storageAccountName>.blob.core.windows.net/<containerName>/unified.html`. Például https://contoso369b2c.blob.core.windows.net/b2c/unified.html.
2. Keresse meg a toohttp://test-cors.org. Ez a hely lehetővé teszi, hogy Ön tooverify hello lap használ, rendelkezik a CORS engedélyezése.  
<!--
![test-cors.org](../../media/active-directory-b2c-customize-ui-of-a-user-journey/test-cors.png)
-->

3. A **távoli URL-cím**, adja meg a unified.html tartalom hello teljes URL-címet, és kattintson **kérés küldése**.
4. Győződjön meg arról, hogy a hello hello kimeneti **eredmények** szakasz *XHR állapota: 200*. Ez azt jelzi, hogy engedélyezve van-e a CORS.
<!--
![CORS enabled](../../media/active-directory-b2c-customize-ui-of-a-user-journey/cors-enabled.png)
-->
hello tárfiókot most tartalmaznia kell egy blob-tároló nevű *b2c* az ábrán látható, amely tartalmazza a tartománynév-sablonok követően – hello hello *Starter-csomag*.

<!--
![Correctly configured storage account](../../articles/active-directory-b2c/media/active-directory-b2c-reference-customize-ui-custom/storage-account-final.png)
-->

hello alábbi táblázat felett HTML5 lapok hello hello célját.

| HTML5-sablon | Leírás |
|----------------|-------------|
| *phonefactor.HTML* | Ezen a lapon a többtényezős hitelesítés lap sablonként használható. |
| *ResetPassword.HTML* | Ezen a lapon használható sablonként egy elfelejtett jelszó lap. |
| *selfasserted.HTML* | Ezen a lapon egy közösségi fiók regisztrációs oldalon, a helyi fiók előfizetéshez vagy egy helyi fiók bejelentkezési oldalának sablonként is használható. |
| *Unified.HTML* | Ezen a lapon egy egységes regisztráció vagy bejelentkezés lap használható sablonként. |
| *updateprofile.HTML* | Ezen a lapon egy profil frissítés lap használható sablonként. |

## <a name="add-a-link-tooyour-html5css-templates-tooyour-user-journey"></a>Hivatkozás tooyour HTML5/CSS sablonok tooyour felhasználói út hozzáadása

Egyéni házirendet közvetlenül szerkesztésével hivatkozás tooyour HTML5/CSS sablonok tooyour felhasználói út adhat hozzá.

hello egyéni HTML5/CSS sablonok toouse a felhasználó használatában a tartalom az adott felhasználó utak használható definíciók listájában megadott toobe rendelkezik. Erre a célra, egy nem kötelező  *<ContentDefinitions>*  XML-elem deklarálni kell a hello  *<BuildingBlocks>*  az egyéni XML-házirendfájl szakasza.

hello következő táblázat ismerteti hello ismeri fel az Azure AD B2C hello identitás élmény motor és a hello típusú lapok toothem kapcsolódó tartalom definition azonosítók.

| Tartalmi azonosító | Leírás |
|-----------------------|-------------|
| *API.Error* | **Hibalap**. Ezen a lapon megjelenik, ha kivétel, vagy hiba történt. |
| *API.idpselections* | **Identitás-szolgáltató kiválasztása lapon**. Ezen a lapon hello felhasználó-szolgáltatók közül választhatnak során bejelentkezési identitás listáját tartalmazza. Ezek a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiókok (e-mail cím vagy a felhasználó neve alapján). |
| *API.idpselections.Signup* | **Identity provider adatgyűjtésre vonatkozó felhasználói előfizetési**. Ezen a lapon hello felhasználó-szolgáltatók közül választhatnak regisztráció során identitás listáját tartalmazza. Ezek a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiókok (e-mail cím vagy a felhasználó neve alapján). |
| *API.localaccountpasswordreset* | **Elfelejtett jelszó lap**. Ez a lap tartalmaz egy űrlap adott hello felhasználó rendelkezik-e a jelszó-átállítási toofill tooinitiate.  |
| *API.localaccountsignin* | **Helyi fiók bejelentkezési oldalának**. Ez a lap tartalmaz egy bejelentkezési képernyő hello rendelkezik toofill amikor bejelentkezik egy helyi fiók, amely egy e-mail címet vagy egy felhasználónevet alapul. hello űrlap is tartalmazhat, egy szöveges beviteli mezőbe, majd a jelszó mező. |
| *API.localaccountsignup* | **Helyi fiók előfizetéshez**. Ez a lap tartalmazza-e a bejelentkezési űrlap kitöltése hello rendelkezik toofill amikor regisztrál a helyi fiók, amely egy e-mail címet vagy egy felhasználónevet alapul. hello űrlap szöveg beviteli mezőt, a jelszó beviteli mezője, a választógomb, a egyetlen legördülő listák és a többszörös kiválasztási jelölőnégyzetet például különböző bemeneti vezérlőket tartalmazhat. |
| *API.phonefactor* | **Többtényezős hitelesítés lap**. Ezen a lapon felhasználók regisztráció vagy bejelentkezés során ellenőrizheti a telefonszámok (szöveges vagy hangos használatával). |
| *API.selfasserted* | **Közösségi fiók bejelentkezési oldalának**. Ez a lap tartalmazza-e a bejelentkezési űrlap kitöltése hello rendelkezik a, ha regisztrál a használatával egy meglévő fiókot a Facebook-on vagy a Google + például közösségi identitásszolgáltató toofill. Ezen a lapon a hasonló toohello közösségi fiók regisztrációs oldalon hello jelszó számbeviteli mezők kivételével hello felett. |
| *API.selfasserted.profileupdate* | **Profil update lapon**. Ez a lap tartalmaz egy űrlap hello felhasználó használhatja tooupdate a profilját. Ezen a lapon a hasonló toohello közösségi fiók regisztrációs oldalon hello jelszó számbeviteli mezők kivételével hello felett. |
| *API.signuporsignin* | **Egyesített előfizetési vagy a bejelentkezési oldal**.  Ezen a lapon is előfizetési és bejelentkezés a felhasználók, akik vállalati identitás-szolgáltatóktól, például a Facebook-on vagy a Google + és helyi fiókok közösségi Identitásszolgáltatók kezeli.

## <a name="next-steps"></a>Következő lépések
[Hivatkozás: Megérteni, hogyan egyéni házirendek a hello B2C identitás élmény keretrendszere](active-directory-b2c-reference-custom-policies-understanding-contents.md)

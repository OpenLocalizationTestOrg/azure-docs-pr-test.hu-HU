---
title: "Egyéni házirendek – az Azure AD B2C használatával a felhasználói felület testreszabása |} Microsoft Docs"
description: "További tudnivalók a felhasználói felület (UI) testreszabása, miközben egyéni házirendekkel használhatja az Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6f00995e54c9f9ef27cc51e38f3de07cd5817cc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a>Az Azure Active Directory B2C: Egyéni házirendek konfigurálása a felhasználói felület testreszabása

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Ez a cikk befejezése után fog regisztráció és bejelentkezés egyéni házirendet a márka és megjelenését. Azure Active Directory B2C (az Azure AD B2C), kapott szinte teljes körű hozzáférést engedélyezzenek hello HTML- és CSS tartalom, amely toousers bemutatott. Egyéni házirend használata esetén konfigurálnia felhasználói felület testreszabásával XML-vezérlők használata az Azure-portálon hello helyett. 

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, fejezze be a [Ismerkedés az egyéni házirendek](active-directory-b2c-get-started-custom.md). A regisztrációt, és jelentkezzen be helyi fiókok működő egyéni házirendet kell rendelkeznie.

## <a name="page-ui-customization"></a>Lap felhasználói felületének testreszabása

Hello lap felhasználói felületének testreszabása szolgáltatás segítségével testre szabhatja a hello megjelenésének és arculatának bármilyen egyéni házirendet. Kezelheti a márka és egységes megjelenést az alkalmazás és az Azure AD B2C között is.

Hogyan működik ez: Azure AD B2C az ügyfél böngészőjében kód fut, és a modern megközelítés nevű [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/). Első lépésként ad meg URL-címet hello egyéni házirendekben a testreszabott HTML-tartalmakat. Az Azure AD B2C egyesíti a HTML-tartalmakat, amely be töltve az URL-címről, és megjeleníti hello lap toohello ügyfél hello felhasználói felületi elemei.

## <a name="create-your-html5-content"></a>A HTML5 tartalmának létrehozása

Hozzon létre HTML tartalom a termék márkáját nevű hello cím.

1. A következő HTML részlet hello másolja. Szabályos üres elem a HTML5 nevű  *\<div id = "api"\>\</div\>*  hello belül található  *\<törzs\>*  címkék. Ez az elem azt jelzi, ha az Azure AD B2C tartalom toobe beszúrni.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   >Biztonsági okokból hello JavaScript jelenleg blokkolva van a Testreszabás.

2. Illessze be a másolt hello részlet egy szövegszerkesztőben, és mentse a fájlt hello *testreszabása ui.html*.

## <a name="create-an-azure-blob-storage-account"></a>Egy Azure Blob storage-fiók létrehozása

>[!NOTE]
> Ebben a cikkben használjuk Azure Blob storage toohost tartalmat. Kiválaszthatja toohost a tartalmat a webkiszolgálón, de meg kell [CORS engedélyezése a webkiszolgálón](https://enable-cors.org/server.html).

toohost hello a Blob Storage tárolóban, a HTML-tartalmakat, a következő:

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. A hello **Hub** menüjében válassza **új** > **tárolási** > **tárfiók**.
3. Adjon meg egy egyedi **neve** a tárfiók.
4. **Telepítési modell** maradjanak **erőforrás-kezelő**.
5. Változás **fiók típusa** túl**Blob-tároló**.
6. **Teljesítmény** maradjanak **szabványos**.
7. **Replikációs** maradjanak **RA-GRS**.
8. **Hozzáférési szint** maradjanak **gyakran használt adatok**.
9. **Storage szolgáltatás titkosítási** maradjanak **letiltott**.
10. Válassza ki a **előfizetés** a tárfiók.
11. Hozzon létre egy **erőforráscsoport** vagy válasszon egy meglévőt.
12. Jelölje be hello **földrajzi hely** a tárfiók.
13. Kattintson a **létrehozása** toocreate hello tárfiók.  
    Hello központi telepítés befejezése után hello **tárfiók** panel nyílik meg automatikusan.

## <a name="create-a-container"></a>Tároló létrehozása

a Blob Storage tárolóban, nyilvános tárolókban toocreate hello a következő:

1. Kattintson a hello **áttekintése** fülre.
2. Kattintson a **tároló**.
3. A **neve**, típus **$root**.
4. Állítsa be **hozzáférési típus** túl**Blob**.
5. Kattintson a **$root** tooopen hello új tároló.
6. Kattintson a **Feltöltés** gombra.
7. Kattintson a hello ikonja tovább túl**válasszon ki egy fájlt**.
8. Nyissa meg túl**testreszabása ui.html**, amely hello korábban létrehozott [lap felhasználói felületének testreszabása](#the-page-ui-customization-feature) szakasz.
9. Kattintson a **Feltöltés** gombra.
10. Válassza ki a feltöltött hello testreszabása ui.html blob.
11. Következő túl**URL-cím**, kattintson a **másolási**.
12. A böngészőben illessze be a másolt hello URL-címet, és toohello található. Hello hely nem érhető el, ha állítson hello tároló hozzáférés típusa túl**blob**.

## <a name="configure-cors"></a>A CORS konfigurálása

A Blob storage konfigurálásához az eltérő eredetű erőforrások megosztása hello következő tevékenységek végrehajtásával:

>[!NOTE]
>Szeretné, hogy ki hello felhasználói felületi testreszabási szolgáltatás tootry minta HTML- és CSS tartalmat használatával? Mellékelt [egy egyszerű segédeszköze](active-directory-b2c-reference-ui-customization-helper-tool.md) , amely feltölti és konfigurálja a minta tartalmat a Blob storage-fiók. Ha hello eszközt használja, hagyja ki azokat, amelyek túl[módosítsa a regisztráció vagy bejelentkezés egyéni házirendet](#modify-your-sign-up-or-sign-in-custom-policy).

1. A hello **tárolási** panel alatt **beállítások**, nyissa meg **CORS**.
2. Kattintson az **Add** (Hozzáadás) parancsra.
3. A **engedélyezett eredeteket**, írja be egy csillag (\*).
4. A hello **engedélyezett műveletek** legördülő listából válassza ki, válassza ki mindkét **beolvasása** és **beállítások**.
5. A **engedélyezett fejlécek**, írja be egy csillag (\*).
6. A **fejlécek kitett**, írja be egy csillag (\*).
7. A **maximális élettartama (másodperc)**, típus **200**.
8. Kattintson az **Add** (Hozzáadás) parancsra.

## <a name="test-cors"></a>A CORS tesztelése

Ellenőrizze, hogy készen áll hello következő tevékenységek végrehajtásával:

1. Toohello Ugrás [teszt-cors.org](http://test-cors.org/) webhelyet, és beillesztés hello URL-címet hello **távoli URL-cím** mezőbe.
2. Kattintson a **kérés küldése**.  
    Ha hibaüzenetet kap, ellenőrizze, hogy a [CORS-beállítások](#configure-cors) helyes-e. Előfordulhat, hogy tooclear kell a gyorsítótárban, vagy nyisson meg egy InPrivate-böngészés munkamenet Ctrl + Shift + P lenyomásával.

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a>A regisztráció vagy bejelentkezés egyéni házirend módosítása

A legfelső szintű hello  *\<TrustFrameworkPolicy\>*  címke, keresse meg  *\<BuildingBlocks\>*  címke. Hello belül  *\<BuildingBlocks\>*  címkék, vegye fel a  *\<ContentDefinitions\>*  címke a következő példa hello másolásával. Cserélje le *your_storage_account* hello a tárfiók nevére.

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a>Töltse fel a frissített egyéni házirend

1. A hello [Azure-portálon](https://portal.azure.com), [hello keretében az Azure AD B2C-bérlő átváltani](active-directory-b2c-navigate-to-b2c-context.md), majd nyissa meg a hello **az Azure AD B2C** panelen.
2. Kattintson a **házirend**.
3. Kattintson a **házirend feltöltése**.
4. Töltse fel `SignUpOrSignin.xml` a hello  *\<ContentDefinitions\>*  címke korábban hozzáadott.

## <a name="test-hello-custom-policy-by-using-run-now"></a>Tesztelése hello egyéni házirend használatával **futtatása most**

1. A hello **az Azure AD B2C** panelen, lépjen túl**összes házirendek**.
2. Válassza ki a feltöltött hello egyéni házirendet, majd kattintson a hello **futtatása most** gombra.
3. Meg kell képes toosign fel e-mail cím használatával.

## <a name="reference"></a>Referencia

Minta sablonok a felhasználói felület testreszabásával itt található:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

hello sample_templates/wingtip mappa a következő HTML-fájlok hello tartalmazza:

| HTML5-sablon | Leírás |
|----------------|-------------|
| *phonefactor.HTML* | Használja ezt a fájlt egy sablont a többtényezős hitelesítés lap. |
| *ResetPassword.HTML* | Használja ezt a fájlt a sablont egy elfelejtett jelszó lap. |
| *selfasserted.HTML* | Használja ezt a fájlt egy közösségi fiók regisztrációs oldalon, a helyi fiók előfizetéshez vagy egy helyi fiók bejelentkezési oldalának sablont. |
| *Unified.HTML* | Használja ezt a fájlt egy egységes regisztráció vagy bejelentkezés lap sablont. |
| *updateprofile.HTML* | Használja ezt a fájlt egy profil frissítés lap sablont. |

A hello [módosítása a regisztráció vagy bejelentkezés egyéni házirend szakasz](#modify-your-sign-up-or-sign-in-custom-policy), konfigurálta a tartalom definíciója hello `api.idpselections`. hello teljes készletét tartalom definíció azonosítóját, amely felismeri a hello Azure AD B2C identitás élmény keretrendszer és azok leírásait tartalmazza a következő táblázat hello szerepelnek:

| Tartalmi azonosító | Leírás | 
|-----------------------|-------------|
| *API.Error* | **Hibalap**. Ezen a lapon megjelenik, ha kivétel, vagy hiba történt. |
| *API.idpselections* | **Identitás-szolgáltató kiválasztása lapon**. Ezen a lapon hello felhasználó-szolgáltatók közül választhatnak során bejelentkezési identitás listáját tartalmazza. Ezek a beállítások esetén a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiók. |
| *API.idpselections.Signup* | **Identity provider adatgyűjtésre vonatkozó felhasználói előfizetési**. Ezen a lapon hello felhasználó-szolgáltatók közül választhatnak regisztráció során identitás listáját tartalmazza. Ezek a beállítások esetén a vállalati identitás-szolgáltatóktól, például a Facebookhoz és a Google + közösségi Identitásszolgáltatók, vagy helyi fiók. |
| *API.localaccountpasswordreset* | **Elfelejtett jelszó lap**. Ez a lap tartalmaz egy űrlap hello felhasználó jelszó-létrehozási tooinitiate kell végeznie.  |
| *API.localaccountsignin* | **Helyi fiók bejelentkezési oldalának**. Ez a lap tartalmaz egy helyi fiók, az e-mail címet vagy egy felhasználónevet alapuló bejelentkezést a bejelentkezési űrlap. hello űrlap is tartalmazhat, egy szöveges beviteli mezőbe, majd a jelszó mező. |
| *API.localaccountsignup* | **Helyi fiók előfizetéshez**. Ezen a lapon található regisztrációs űrlap iratkozik fel egy helyi fiók, amely egy e-mail címet vagy egy felhasználónevet alapul. hello űrlap különböző bemeneti vezérlők, például szöveg beviteli mezőt, a jelszó mező, választógomb, egyetlen legördülő listák és a többszörös kiválasztási jelölőnégyzetek is tartalmazhat. |
| *API.phonefactor* | **Többtényezős hitelesítés lap**. Ezen a lapon, a felhasználók a telefonszámok (segítségével ellenőrizheti szöveges vagy hangos) regisztráció vagy bejelentkezés során. |
| *API.selfasserted* | **Közösségi fiók bejelentkezési oldalának**. Ezen a lapon, hogy a felhasználók kell végeznie, amikor azok az egy meglévő fiókkal a Facebook-on vagy a Google + például közösségi identitásszolgáltató regisztráljon regisztrációs űrlap tartalmazza. Ezen a lapon a hasonló toohello közösségi fiók előfizetéshez megelőző hello jelszó számbeviteli mezők kivételével. |
| *API.selfasserted.profileupdate* | **Profil update lapon**. Ezen a lapon, hogy felhasználók használhatják tooupdate a profilját űrlap tartalmazza. Ezen a lapon a hasonló toohello közösségi fiók bejelentkezési lapon hello jelszó számbeviteli mezők kivételével. |
| *API.signuporsignin* | **Egyesített előfizetési vagy a bejelentkezési oldal**. Ezen a lapon mindkét hello előfizetési és jelentkezzen be a felhasználók, akik vállalati identitás-szolgáltatóktól, például a Facebook-on vagy a Google + és helyi fiókok közösségi Identitásszolgáltatók kezeli.  |

## <a name="next-steps"></a>Következő lépések

További információ a testre szabható felhasználói felületi elemeket: [beépített házirendek felhasználói felület testreszabása a referencia-útmutató](active-directory-b2c-reference-ui-customization.md).

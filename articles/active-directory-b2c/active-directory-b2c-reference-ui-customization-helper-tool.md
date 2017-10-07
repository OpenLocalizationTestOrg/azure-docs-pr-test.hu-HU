---
title: "Az Azure Active Directory B2C: Lap felhasználói felületének testreszabása segédeszköze |} Microsoft Docs"
description: "Egy segédeszköze használt Azure Active Directory B2C toodemonstrate hello lap felhasználói felületi testreszabási szolgáltatás"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: ae935d52-3520-4a94-b66e-b35bb40e7514
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: swkrish
ms.openlocfilehash: 5137ac112019369b4244a925df1acb96fefb758f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-a-helper-tool-used-toodemonstrate-hello-page-user-interface-ui-customization-feature"></a>Az Azure Active Directory B2C: Egy segédeszköze használt toodemonstrate hello lap felhasználói felület (UI) testreszabási szolgáltatás
Ez a cikk egy kiegészítő toohello [fő felhasználói felület testreszabása cikk](active-directory-b2c-reference-ui-customization.md) az Azure Active Directory (Azure AD) B2C. hello következő lépések bemutatják, hogyan tooexercise hello minta HTML- és CSS tartalom nyújtunk a lap felhasználói felületi testreszabási szolgáltatás.

## <a name="get-an-azure-ad-b2c-tenant"></a>Az Azure AD B2C-bérlő beszerzése
Mielőtt bármilyen testreszabhatja, szüksége lesz túl[az Azure AD B2C-bérlő beszerzése](active-directory-b2c-get-started.md) Ha még nem rendelkezik.

## <a name="create-a-sign-up-or-sign-in-policy"></a>Regisztráció vagy bejelentkezés házirend létrehozása
hello mellékelt minta tartalom lehet a használt toocustomze két lapok egy [regisztráció vagy bejelentkezés házirend](active-directory-b2c-reference-policies.md): hello [egyesített bejelentkezési oldal](active-directory-b2c-reference-ui-customization.md) és hello [önálló szükséges attribútumok lapon](active-directory-b2c-reference-ui-customization.md). Ha [a regisztráció vagy bejelentkezés házirend létrehozása](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy), adja hozzá a helyi fiókkal (e-mail cím), a Facebook, a Google és a Microsoft részére **identitás-szolgáltatóktól**. A rendszer hello csak olyan IDPs, amely elfogadja a minta HTML-tartalmakat.  Ha is hozzáadhat a IDPs egy részét.

## <a name="register-an-application"></a>Egy alkalmazás regisztrálása
Szüksége lesz túl[alkalmazás regisztrálása](active-directory-b2c-app-registration.md) az a B2C-bérlőben, amely lehet használt tooexecute a házirendet. Az alkalmazás a regisztrálás után futtassa a regisztrációs szabályzatban tooactually használt néhány lehetőség közül választhat:

* Gyors üzembe helyezési alkalmazások szakaszában felsorolt hello "első lépések" hello Azure AD B2C egyik Build [jelentkezzen regisztrálása és beléptetése az alkalmazások a fogyasztói](active-directory-b2c-overview.md#get-started).
* Előzetesen elkészített használata hello [az Azure AD B2C Playground](https://aadb2cplayground.azurewebsites.net) alkalmazás. Ha úgy dönt, hogy toouse hello playground, regisztrálnia kell egy alkalmazást a B2C-bérlő hello segítségével a **átirányítási URI** `https://aadb2cplayground.azurewebsites.net/`.
* Használjon hello **Futtatás most** gombra a házirend a hello [Azure-portálon](https://portal.azure.com/).

## <a name="customize-your-policy"></a>Szabja testre a házirendet
toocustomize hello megjelenését és működését a házirend, kell toofirst hello adott egyezmények az Azure AD B2C használatával HTML- és a CSS-fájlok létrehozása. Majd feltöltheti a statikus tartalom tooa nyilvánosan elérhető helyre, hogy az Azure AD B2C-e hozzáférési engedélye. Ennek oka az lehet, a saját dedikált webkiszolgálón, Azure Blob Storage, Azure Content Delivery Network vagy bármely más statikus erőforrás-üzemeltetési szolgáltató. hello csak vonatkozó követelmények nincsenek, hogy a tartalom elérhető HTTPS-KAPCSOLATON keresztül, és CORS használatával érhető el. A statikus tartalom hello webhely már kapcsolódik, ha a házirend toopoint toothis hely és a jelen, a tartalom tooyour ügyfelek szerkesztheti. Hello [fő felhasználói felület testreszabása cikk](active-directory-b2c-reference-ui-customization.md) hello Azure AD B2C testreszabási szolgáltatás működését ismerteti részletesen.

Ez az oktatóanyag hello céljából már előre létrehozott néhány minta tartalom és üzemelteti az Azure Blob Storage. hello minta tartalma a fiktív vállalat, a "Wingtip Toys" hello témában nagyon egyszerű testreszabását. tootry azt ki a saját házirendjének, kövesse az alábbi lépéseket:

1. Jelentkezzen be a hello tooyour bérlői [Azure-portálon](https://portal.azure.com/) , és keresse meg a toohello B2C funkciók panelje.
2. Kattintson a **regisztráció vagy bejelentkezés házirendek** és kattintson a házirend (például "b2c\_1\_bejelentkezési\_mentése\_bejelentkezési\_a").
3. Kattintson a **lap felhasználói felületének testreszabása** , majd **egyesített előfizetési vagy a bejelentkezési oldal**.
4. Váltás hello **használata egyéni lap** túl kapcsoló**Igen**. A hello **egyéni lap URI** mezőbe írja be `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/unified.html`. Kattintson az **OK** gombra.
5. Kattintson a **helyi fiók előfizetéshez**. Váltás hello **egyéni sablon használata** túl kapcsoló**Igen**. A hello **egyéni lap URI** mezőbe írja be `https://wingtiptoysb2c.blob.core.windows.net/b2c/wingtip/selfasserted.html`.
6. Ugyanaz a hello lépés ismétlési hello **közösségi fiók bejelentkezési oldalának**.
   Kattintson a **OK** kétszer a tooclose hello felhasználói felület testreszabása paneleken.
7. Kattintson a **Save** (Mentés) gombra.

Most tekinthetők meg a testreszabott házirendet. Saját alkalmazás vagy hello Azure AD B2C playground használható, ha szeretné, de egyszerűen is kattinthat hello **Futtatás most** hello szabályzat paneljén parancsot. Hello legördülő listából válassza ki az alkalmazását, és válassza ki a hello megfelelő átirányítási URI-t. Kattintson a hello **futtatása most** gombra. Egy új böngészőlapon nyílik meg, és hello felhasználói élmény iratkozik fel az alkalmazás és a hely új tartalom hello segítségével futtathatja!

## <a name="upload-hello-sample-content-tooazure-blob-storage"></a>Hello minta tartalom tooAzure Blob Storage feltöltése
Ha azt szeretné, hogy toouse Azure Blob Storage toohost a tartalom, hozzon létre egy saját tárfiókot, és használhatja fájljait, a B2C segítő eszköz tooupload lap.

### <a name="create-a-storage-account"></a>Create a storage account
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).
2. Kattintson a **+ új** > **adatok + tárolás** > **tárfiók**. Szüksége lesz egy Azure-előfizetés toocreate Azure Blob Storage-fiók. Regisztrálhat egy ingyenes próbaverziót: hello [Azure-webhelyen](https://azure.microsoft.com/pricing/free-trial/).
3. Adjon meg egy **neve** hello storage-fiókok (például "contoso"), és mentse hello megfelelő eltávolításra **tarifacsomag**, **erőforráscsoport** és  **Előfizetés**. Győződjön meg arról, hogy rendelkezik-e hello **PIN-kód tooStartboard** beállítás be van jelölve. Kattintson a **Create** (Létrehozás) gombra.
4. Lépjen vissza a Kezdőpulton toohello, és kattintson az újonnan létrehozott tárfiók hello.
5. A hello **összegzés** kattintson **tárolók**, és kattintson a **+ Hozzáadás**.
6. Adjon meg egy **neve** hello tároló (például "b2c"), és válassza a **Blob** hello, **hozzáférési típus**. Kattintson az **OK** gombra.
7. hello tároló létrehozott megjelennek a hello hello listában **Blobok** panelen. Írja le hello tároló; hello URL-címe például az alábbihoz hasonló túl`https://contoso.blob.core.windows.net/b2c`. Bezárás hello **Blobok** panelen.
8. Hello storage-fiók paneljén kattintson **kulcsok** írja le hello hello értékének **Tárfióknév** és **elsődleges elérési kulcsot** mezőket.

> [!NOTE]
> **Elsődleges elérési kulcsot** van egy fontos biztonsági hitelesítő adatok.
> 
> 

### <a name="download-hello-helper-tool-and-sample-files"></a>Hello segítő eszközre és fájlok letöltése
Letöltheti a hello [Azure Blob Storage segítő eszközre és .zip-fájlként fájlok](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip) vagy klónozza a Githubról:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

Ebben a tárházban tartalmaz egy `sample_templates\wingtip` címtár, például HTML, CSS, és a képeket tartalmazó. Ezek a sablonok tooreference a saját Azure Blob Storage-fiók kell tooedit hello HTML-fájlok. Nyissa meg `unified.html` és `selfasserted.html` , és cserélje le minden példányát `https://localhost` saját tárolót, akkor az előző lépéseket hello hello URL-címével. Hello abszolút elérési útja hello HTML-fájlokat, mert hello HTML az Azure AD tartományi hello által kiszolgált ebben az esetben kell használnia `https://login.microsoftonline.com`.

### <a name="upload-hello-sample-files"></a>Hello minta fájlok feltöltése
Az adott adattár hello, csomagolja ki `B2CAzureStorageClient.zip` és futtatási hello `B2CAzureStorageClient.exe` fájlon belül. A program egyszerűen fel kell töltenie valamennyi hello fájlt hello tooyour tárfiók adja meg, és engedélyezze a hozzáférést a CORS azokat a fájlokat. Ha követte a fenti lépések hello, hello HTML és CSS fájlok lesznek most kell mutató tooyour tárfiók. Vegye figyelembe, hogy hello a tárfiók nevére része hello megelőző `blob.core.windows.net`, például `contoso`. Ellenőrizheti, hogy hello tartalom feltöltötte megfelelően úgy tooaccess `https://{storage-account-name}.blob.core.windows.net/{container-name}/wingtip/unified.html` egy böngésző. Is [http://test-cors.org/](http://test-cors.org/) toomake arról, hogy hello tartalom most CORS engedélyezése mellett. (Keressen "XHR állapota: 200" hello eredményben.)

### <a name="customize-your-policy-again"></a>A házirendet, újra testreszabása
Most, hogy hello minta tartalom tooyour saját tárfiók feltöltött, szerkesztenie kell a regisztrációs szabályzatban tooreference azt. Hello ismételje meg a hello ["Testreszabhatja az"](#customize-your-policy) a fenti szakaszban ezúttal a saját storage-fiók URL-címeket. Például hello helyét a `unified.html` fájl lenne `<url-of-your-container>/wingtip/unified.html`.

Mostantól a hello **Futtatás most** gombra vagy a saját alkalmazás tooexecute újra a házirendet. hello eredmény kell keresse meg szinte teljesen hello azonos--meg használt hello azonos Példa HTML és a CSS mindkét esetben. Azonban a házirend most hivatkoznak a saját Azure Blob Storage példányát, és szabad tooedit, és töltse fel újra, ha Ön hello fájlok adja. További információk a testreszabás, hello HTML- és CSS, részletek toohello [fő felhasználói felület testreszabása cikk](active-directory-b2c-reference-ui-customization.md).


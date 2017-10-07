---
title: "aaaHow tooUse hello JavaScript SDK az Azure Mobile Apps-alkalmazáshoz"
description: "Hogyan tooUse v Azure Mobile Apps-alkalmazáshoz"
services: app-service\mobile
documentationcenter: javascript
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 53b78965-caa3-4b22-bb67-5bd5c19d03c4
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: html
ms.devlang: javascript
ms.topic: article
ms.date: 10/30/2016
ms.author: glenga
ms.openlocfilehash: 3fcbb0c5bd6918a285bdafa1946ba0bd47bb21b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-hello-javascript-client-library-for-azure-mobile-apps"></a>Hogyan tooUse hello Azure Mobile Apps a JavaScript ügyféloldali kódtár
[!INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Ez az útmutató útmutatást ad, hogy tooperform szolgáltatást használó általános forgatókönyvhöz hello legújabb [JavaScript SDK az Azure Mobile Apps]. Ha új tooAzure Mobile Apps, először végezzen [Azure Mobile Apps gyors üzembe helyezés] toocreate háttérkiszolgálón és hozzon létre egy táblát. Az útmutató azt összpontosítanak hello mobil-háttéralkalmazások használatával HTML/JavaScript webes alkalmazásokhoz.

## <a name="supported-platforms"></a>A támogatott platformok
Azt böngésző támogatási toohello aktuális korlátjának növelését, és az utolsó hello verziói ismertebb böngésző: Google Chrome, a Microsoft Edge, a Microsoft Internet Explorer vagy a Mozilla Firefox.  Várhatóan hello SDK toofunction minden viszonylag modern böngészőben.

hello csomag terjesztése egy univerzális JavaScript-modult, így támogatja a globális változók globals, AMD, és az CommonJS formátumú.

## <a name="Setup"></a>A telepítő és Előfeltételek
Ez az útmutató feltételezi, hogy létrehozott egy táblát a háttérkiszolgálón. Ez az útmutató feltételezi, hogy hello táblához hello hello táblák ezen oktatóprogram az azonos sémából.

Hello Azure Mobile Apps JavaScript SDK telepítése megteheti a hello `npm` parancs:

```
npm install azure-mobile-apps-client --save
```

hello könyvtár egy ES2015 modul, például Browserify és Webpack és egy AMD tárként CommonJS környezetekben is használható.  Példa:

```
# For ECMAScript 5.1 CommonJS
var WindowsAzure = require('azure-mobile-apps-client');
# For ES2015 modules
import * as WindowsAzure from 'azure-mobile-apps-client';
```

Hello SDK egy előre elkészített verziója is használható, ha úgy, hogy letölti a CDN-ről:

```html
<script src="https://zumo.blob.core.windows.net/sdk/azure-mobile-apps-client.min.js"></script>
```

[!INCLUDE [app-service-mobile-html-js-library](../../includes/app-service-mobile-html-js-library.md)]

## <a name="auth"></a>Hogyan: hitelesíti a felhasználókat
Az Azure App Service támogat hitelesítése és engedélyezése a felhasználók alkalmazás különböző külső Identitásszolgáltatók: Facebook, Google, a Microsoft Account és Twitter. Beállíthatja engedélyeit a táblák toorestrict hozzáférési megadott műveletek tooonly hitelesített felhasználók. Használhatja a hitelesített felhasználók tooimplement az engedélyezési szabályok hello identitás server parancsfájlokban. További információkért lásd: hello [Bevezetés a hitelesítés használatába] oktatóanyag.

Két hitelesítési forgalom támogatottak: a kiszolgáló folyamata és egy ügyfél folyamatában.  hello kiszolgáló folyamata nyújt hello legegyszerűbb hitelesítési élmény hello szolgáltató webes hitelesítés felület támaszkodnak. hello ügyfél folyamata lehetővé teszi, hogy szorosabb integrációt eszközspecifikus képességeket például single-sign-on, szolgáltatói SDK-k támaszkodnak az.

[!INCLUDE [app-service-mobile-html-js-auth-library](../../includes/app-service-mobile-html-js-auth-library.md)]

### <a name="configure-external-redirect-urls"></a>Útmutató: a Mobile App Service konfigurálása külső átirányítási URL-címeket.
A JavaScript-alkalmazások számos különböző használja egy visszacsatolási funkció toohandle OAuth UI zajlik.  Ezek a képességek közé tartoznak:

* A szolgáltatás helyben fut
* Élő Újrabetöltés hello ionos keretrendszer használatával
* A hitelesítési szolgáltatás tooApp átirányítása.

Helyileg futó problémákat okozhat, mert alapértelmezés szerint csak az App Service tooallow a hozzáférést a Mobile Apps-háttéralkalmazását konfigurálva. Hello server a helyi futtatás során a következő lépéseket toochange hello App Service beállítások tooenable hitelesítési hello használata:

1. Jelentkezzen be toohello [Azure-portálon]
2. Lépjen a Mobile Apps-háttéralkalmazás tooyour.
3. Válassza ki **erőforrás-kezelő** a hello **FEJLESZTŐESZKÖZÖK** menü.
4. Kattintson a **Ugrás** tooopen hello erőforrás-kezelő a Mobile Apps-háttéralkalmazás egy új lapot vagy ablakban.
5. Bontsa ki a hello **config** > **authsettings** csomópont az alkalmazásra vonatkozóan.
6. Kattintson a hello **szerkesztése** tooenable hello erőforrás Szerkesztés gomb.
7. Hello található **allowedExternalRedirectUrls** elemmel rendelkezik, ami null értékűnek kell lennie. Adja hozzá az URL-címek tömbjét:

         "allowedExternalRedirectUrls": [
             "http://localhost:3000",
             "https://localhost:3000"
         ],

    Hello URL-címek hello tömb cserélje le a szolgáltatás, amely ebben a példában hello URL-címei `http://localhost:3000` hello helyi Node.js sample szolgáltatás. Is `http://localhost:4400` hello Fodrozás szolgáltatás vagy egy másik URL-cím, attól függően, hogy az alkalmazás konfigurálását.
8. Hello hello oldal tetején, kattintson a **olvasási/írási**, majd kattintson a **PUT** toosave a frissítéseket.

Szükség tooadd hello azonos visszacsatolási URL-címek toohello CORS engedélyezési lista beállításait:

1. Keresse meg a visszafelé toohello [Azure-portálon].
2. Lépjen a Mobile Apps-háttéralkalmazás tooyour.
3. Kattintson a **CORS** a hello **API** menü.
4. Adja meg minden egyes URL-címe üres hello **engedélyezett eredetet** szövegmezőben.  Egy új szövegmező jön létre.
5. Kattintson a **mentése**

Után hello háttér frissíti, az alkalmazás fogja tudni toouse hello új visszacsatolási URL-címeket.

<!-- URLs. -->
[Azure Mobile Apps gyors üzembe helyezés]: app-service-mobile-cordova-get-started.md
[Bevezetés a hitelesítés használatába]: app-service-mobile-cordova-get-started-users.md
[Add authentication tooyour app]: app-service-mobile-cordova-get-started-users.md

[Azure-portálon]: https://portal.azure.com/
[JavaScript SDK az Azure Mobile Apps]: https://www.npmjs.com/package/azure-mobile-apps-client
[Query object documentation]: https://msdn.microsoft.com/en-us/library/azure/jj613353.aspx

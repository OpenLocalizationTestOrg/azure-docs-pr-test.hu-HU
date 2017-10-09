---
title: "aaaAzure AD v2 JS SPA interaktív telepítés - konfigurálása (ARP) |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások az Azure Active Directory-v2 végpontja (ARP) hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 157f4e342cd684294e24da6ee1fad8a7c2fc266a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a>Hello alkalmazás regisztrációs információ tooyour alkalmazás hozzáadása

Ebben a lépésben kell tooconfigure hello átirányítási URL-CÍMÉT a alkalmazás regisztrációs adatait, és adja hozzá a hello alkalmazásazonosító tooyour JavaScript SPA-alkalmazáshoz.

### <a name="configure-redirect-url"></a>Az átirányítási URL-CÍMEK konfigurálása

Hello konfigurálása `Redirect URL` hello URL-címe alapján a webkiszolgáló index.html lapon a fenti mezőben, majd kattintson az *frissítés*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>A Visual Studio utasításokat átirányítási URL-cím beszerzése
> tooobtain az átirányítási URL-cím, kövesse az alábbi hello utasítások:
> 1.    A *Megoldáskezelőben*hello projekt között, nézze meg hello `Properties` ablakban (Ha nem látja a Tulajdonságok ablak, nyomja meg az `F4`)
> 2.    Másolja a hello értéket `URL` toohello vágólapra:<br/> ![Projekt tulajdonságai](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Illessze be a hello értéke megegyezik egy `Redirect URL` hello az oldal tetején, majd kattintson`Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Az átirányítási URL-ként Python
> Python hello a webtartalom-kiszolgáló portja parancssorból állíthatók be. Az interaktív telepítés használja a 8080-as porton hello referenciaként, de érzi, hogy a szabad toouse összes rendelkezésre álló portot. Minden esetben kövesse az alábbi tooset hello alkalmazás regisztrációs adatai egy átirányítási URL-cím mentése hello utasításokat:<br/>
> Állítsa be `http://localhost:8080/` , egy `Redirect URL` az oldal tetején hello, vagy használjon `http://localhost:[port]/` egyéni TCP-port használata (Ha *[port]* hello egyéni TCP-port száma), és kattintson a "Frissítés"

### <a name="configure-your-javascript-spa-application"></a>A JavaScript SPA-alkalmazások konfigurálása

1.  Hozzon létre egy fájlt `msalconfig.js` tartalmazó hello alkalmazás regisztrációs adatokat. Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`. Nevezze el`msalconfig.js`
2.  Adja hozzá a következő kód tooyour hello `msalconfig.js` fájlt:

```javascript
var msalconfig = {
    clientID: "[Enter hello application Id here]",
    redirectUri: location.origin
};
``` 

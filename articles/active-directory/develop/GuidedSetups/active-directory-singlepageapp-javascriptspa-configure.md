---
title: "aaaAzure AD v2 JS SPA interaktív telepítés – konfigurálása |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
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
ms.openlocfilehash: 1b93298d4bd4e17dd261dbb75502a122c30aac97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="register-your-application"></a>Alkalmazás regisztrálása

Nincs több módon toocreate egy alkalmazást, válasszon egyet a őket:

### <a name="option-1-register-your-application-express-mode"></a>1. lehetőség: Az alkalmazás (Expressz mód) regisztrálása
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:

1.  Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=singlePageApp&appTech=javascriptSpa&step=configure)
2.  Adjon meg egy nevet az alkalmazás és az e-maileket
3.  Győződjön meg arról, hogy hello lehetőséget *interaktív telepítés* be van jelölve
4.  Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot

### <a name="option-2-register-your-application-advanced-mode"></a>2. lehetőség: Az alkalmazás (speciális módban) regisztrálása

1. Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás
2. Adjon meg egy nevet az alkalmazás és az e-maileket 
3. Győződjön meg arról, hogy hello lehetőséget *interaktív telepítés* nincs bejelölve
4.  Kattintson a `Add Platform`, majd jelölje be`Web`
5. Adja hozzá a hello `Redirect URL` , amelyek megfelelnek a toohello alkalmazás URL-CÍMÉT, a webkiszolgáló alapján. Útmutatás hello szakaszokat meg, hogyan tooset / a Visual Studio és a Python hello átirányítási URL-cím beszerzése.
6. Kattintson a *mentése*

> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>A Visual Studio utasításokat átirányítási URL-cím beszerzése
> Kövesse az utasításokat tooobtain hello az átirányítási URL-cím:
> 1.    A *Megoldáskezelőben*hello projekt között, nézze meg hello `Properties` ablakban (Ha nem látja a Tulajdonságok ablak, nyomja meg az `F4`)
> 2.    Másolja a hello értéket `URL` toohello vágólapra:<br/> ![Projekt tulajdonságai](media/active-directory-singlepageapp-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Váltson vissza toohello *alkalmazásregisztrációs portálra* , majd illessze be a hello értéke megegyezik egy `Redirect URL` , és kattintson a "Mentés"

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Az átirányítási URL-ként Python
> Python hello a webtartalom-kiszolgáló portja parancssorból állíthatók be. Az interaktív telepítés használja a 8080-as porton hello referenciaként, de érzi, hogy a szabad toouse összes rendelkezésre álló portot. Minden esetben kövesse az alábbi tooset hello alkalmazás regisztrációs adatai egy átirányítási URL-cím mentése hello utasításokat:<br/>
> - Váltson vissza toohello *alkalmazásregisztrációs portálra* és állítsa be `http://localhost:8080/` , egy `Redirect URL`, vagy használjon `http://localhost:[port]/` egyéni TCP-port használata (ahol *[port]* hello egyéni TCP port szám), és kattintson a "Mentés"


#### <a name="configure-your-javascript-spa"></a>A JavaScript SPA konfigurálása

1.  Hozzon létre egy fájlt `msalconfig.js` tartalmazó hello alkalmazás regisztrációs adatokat. Ha használja a Visual Studio, válassza hello project (projekt gyökérmappájában), kattintson a jobb gombbal, és válassza ki: `Add`  >  `New Item`  >  `JavaScript File`. Nevezze el`msalconfig.js`
2.  Adja hozzá a következő kód tooyour hello `msalconfig.js` fájlt:

```javascript
var msalconfig = {
    clientID: "Enter_the_Application_Id_here",
    redirectUri: location.origin
};
```
<ol start="3">
<li>
Cserélje le <code>Enter_the_Application_Id_here</code> a hello csak regisztrált alkalmazás azonosítója
</li>
</ol>

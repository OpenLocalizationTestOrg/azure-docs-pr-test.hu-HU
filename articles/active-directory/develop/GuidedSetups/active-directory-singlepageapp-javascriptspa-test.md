---
title: "aaaAzure AD v2 JS SPA interaktív telepítés - teszt |} Microsoft Docs"
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
ms.openlocfilehash: b2339431a070b5c4ad4058e6c1a9b19b83c84c1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Tesztelheti a kódját

> ### <a name="testing-with-visual-studio"></a>A Visual Studio tesztelése
> Visual Studio használatakor nyomja le az `F5` toorun a projekthez: hello böngészőben megnyílik, és arra utasítja, hogy túl*http://localhost: {port}* hello megtapasztalhatja *Microsoft Graph API hívása* gombra.

<p/><!-- -->

> ### <a name="testing-with-python-or-another-web-server"></a>A Python vagy egy másik webkiszolgálón tesztelése
> Ha a Visual Studio nem használ, győződjön meg arról, a webkiszolgáló elindult, és van-e konfigurálva toolisten tooa TCP-port alapján hello mappát tartalmazó a *index.html* fájlt. A Python, megkezdheti figyel toohello port futtatásával hello hello parancsban Rákérdezés / terminál, hello app mappából:
> 
> ```bash
> python -m http.server 8080
> ```
>  Ezt követően hello böngésző megnyitásához és típus *8080* vagy *http://localhost: {port}* - Ha hello *port* toohello portot, amelyet a webkiszolgáló megfelel figyel. Hello tartalmát a hello index.html lapon kell megjelennie *Microsoft Graph API hívása* gombra.

## <a name="test-your-application"></a>Az alkalmazás tesztelése

Hello böngésző után betölti a *index.html*, kattintson a hello *Microsoft Graph API hívása* gombra. Ha hello első alkalommal, hello böngésző átirányítja, Microsoft Azure Active Directory v2 toohello végpont, ahol felszólítja a toosign.
 
![A minta képernyőkép](media/active-directory-singlepageapp-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="consent"></a>Hozzájárulás
hello első bejelentkezéskor tooyour alkalmazás, lehetősége lesz a hozzájárulási képernyő hasonló toohello következő, ahol tooaccept kell:

 ![Hozzájárulás képernyő](media/active-directory-singlepageapp-javascriptspa-test/javascriptspaconsent.png)


### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
Felhasználói profil adatait a Microsoft Graph API-hívás válasz hello által visszaadott kell megjelennie.
 
 ![Results (Eredmények)](media/active-directory-singlepageapp-javascriptspa-test/javascriptsparesults.png)

Hello szerzett hello token alapvető információkat is látni *Access Token* és *azonosító Token jogcímek* jelölőnégyzetéből.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették

hello Microsoft Graph API szükséges hello `user.read` tooread hello profil hatókörének. Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben. Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség. Például a Microsoft Graph-hatókör hello `Calendars.Read` van szükség toolist hello felhasználók naptáraiban. Sorrendben tooaccess hello hello környezetben egy alkalmazás felhasználói naptár, tooadd hello kell `Calendars.Read` delegált engedéllyel toohello alkalmazás regisztrációs adatait, és adja meg az hello `Calendars.Read` hatókör toohello `acquireTokenSilent` hívható meg. hello felhasználói további hozzájárulásokat azoktól hello hatókörök számának növelésével kérheti.

Ha egy háttér-API nem igényli a hatókör (nem ajánlott), használhatja a hello `clientId` hello hatóköreként a hello `acquireTokenSilent` és/vagy `acquireTokenRedirect` hívások.

<!--end-collapse-->

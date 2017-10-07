---
title: "aaaAzure AD v2 Windows asztali bevezetés - teszt |} Microsoft Docs"
description: "Hogyan Windows asztali .NET (XAML) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: 0ae9612e1585c54a3fe35ba9d18f92554099b2c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Tesztelheti a kódját

A rendezés tootest az alkalmazás, nyomja meg az `F5` toorun a projektre a Visual Studióban. A fő ablak kell megjelennie:

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-windowsdesktop-test/samplescreenshot.png)

Amikor készen áll a tootest, kattintson a *Microsoft Graph API hívása* , és használja a Microsoft Azure Active Directory (szervezeti fiók) vagy a Microsoft Account (live.com, outlook.com) fiók toosign a. Az első alkalommal hello, a felhasználó toosign kérő ablak jelenik meg:

![bejelentkezés](media/active-directory-mobileanddesktopapp-windowsdesktop-test/signinscreenshot.png)

### <a name="consent"></a>Hozzájárulás
hello első bejelentkezéskor tooyour alkalmazást, akkor számára jelenik meg a hozzájárulási képernyő hasonló toohello az alábbi, amennyiben szükséges tooexplicitly, fogadja el:

![Hozzájárulás képernyő](media/active-directory-mobileanddesktopapp-windowsdesktop-test/consentscreen.png)

### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
Felhasználói profil adatait hello API-hívás eredménye képernyőn hello Microsoft Graph API-hívás által visszaadott kell megjelennie.

Emellett meg kell jelennie keresztül szerzett hello token alapvető információkat `AcquireTokenAsync` vagy `AcquireTokenSilentAsync` hello Token adatait mezőbe:

|Tulajdonság  |Formátumban  |Leírás |
|---------|---------|---------|
|Név | {Felhasználó teljes neve} |hello felhasználói vezeték- és keresztneve|
|Felhasználónév |<span>user@domain.com</span> |hello felhasználónév használt tooidentify hello felhasználó|
|Jogkivonat lejár |{DateTime}         |hello idő mely hello jogkivonat lejár. MSAL fog hello lejárati dátum meghosszabbításához meg megújításával hello token szükség esetén|
|hozzáférési jogkivonat |{Karakterlánc}         |a lexikális elem karakterlánca hello küldött, küld egy engedélyezési fejléc igénylő tooHTTP kérelmek|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették
Graph API szükséges hello `user.read` tooread felhasználói profil hatókörének. Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben. Néhány egyéb Graph API-k, valamint a háttérkiszolgáló az egyéni API-k szükséges további hatókörökkel. Például a diagramot `Calendars.Read` van szükség toolist felhasználók naptáraiban. Sorrendben tooaccess hello kontextusban az alkalmazás felhasználó naptár, tooadd kell `Calendars.Read` meghatalmazott alkalmazás regisztrációs adatait, majd adja hozzá `Calendars.Read` toohello `AcquireTokenAsync` hívható meg. Felhasználó kérheti további hozzájárulásokat azoktól hello hatókörök számának növelésével.

<!--end-collapse-->




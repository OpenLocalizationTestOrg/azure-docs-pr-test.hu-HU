---
title: aaaAzure AD v2 iOS Getting Started - teszt |} Microsoft Docs
description: "Hogyan iOS (Swift) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
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
ms.date: 05/09/2017
ms.author: andret
ms.openlocfilehash: 98c73eddabf9664feb19ac6878e9d7315b9aa79b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-querying-hello-microsoft-graph-api-from-your-ios-application"></a>Az iOS-alkalmazásból lekérdező hello Microsoft Graph API tesztelése

Nyomja le az `Command`  +  `R` toorun hello kód hello szimulátor.

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-ios-test/iostestscreenshot.png)

Amikor készen áll a tootest, koppintson *"Hívható meg Microsoft Graph API"* és rákérdezéses tootype lesz a felhasználónevét és jelszavát.

### <a name="consent"></a>Hozzájárulás
hello első bejelentkezéskor tooyour alkalmazást, akkor számára jelenik meg a hozzájárulási képernyő hasonló toohello az alábbi, amennyiben szükséges tooexplicitly, fogadja el:

![Hozzájárulás képernyő](media/active-directory-mobileanddesktopapp-ios-test/iosconsentscreen.png)

### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
Felhasználói profil adatait a hello hello Microsoft Graph API-hívás által visszaadott kell megjelennie *naplózás* szakasz.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették

hello Microsoft Graph API szükséges hello `user.read` tooread hello profil hatókörének. Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben. Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség. Például a Microsoft Graph-hatókör hello `Calendars.Read` van szükség toolist hello felhasználók naptáraiban. Sorrendben tooaccess hello kontextusban az alkalmazás felhasználó naptár, tooadd hello kell `Calendars.Read` delegált engedéllyel toohello alkalmazás regisztrációs adatait, és adja meg az hello `Calendars.Read` hatókör toohello `acquireTokenSilent` hívható meg. hello felhasználói további hozzájárulásokat azoktól hello hatókörök számának növelésével kérheti.

<!--end-collapse-->




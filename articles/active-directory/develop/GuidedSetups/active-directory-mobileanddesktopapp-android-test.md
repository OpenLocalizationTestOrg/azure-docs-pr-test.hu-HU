---
title: "aaaAzure AD v2 Android bevezetés - teszt |} Microsoft Docs"
description: "Android-alkalmazás hogyan szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API-val vagy a hozzáférési jogkivonatok az Azure Active Directory v2 végpont igénylő API-k"
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
ms.openlocfilehash: 499f32b46fd44cca0e52179bced49b311135d8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="test-your-code"></a>Tesztelheti a kódját

1. A kód tooyour/emulátor eszköz telepítése.
2. Amikor készen áll a tootest, használja a Microsoft Azure Active Directory (szervezeti fiók) vagy a Microsoft Account (live.com, outlook.com) fiók toosign a. 

![A minta képernyőkép](media/active-directory-mobileanddesktopapp-android-test/mainwindow.png)
<br/><br/>
![Bejelentkezés](media/active-directory-mobileanddesktopapp-android-test/usernameandpassword.png)

### <a name="consent"></a>Hozzájárulás
hello első alkalommal a felhasználó bejelentkezik tooyour alkalmazás, azok számára jelenik meg a hozzájárulási képernyő hasonló toohello az alábbi, ahol tooexplicitly, fogadja el: 

![Hozzájárulás](media/active-directory-mobileanddesktopapp-android-test/androidconsent.png)


### <a name="expected-results"></a>Kívánt eredmény elérése érdekében
Megjelenik egy hívás tooMicrosoft Graph API hello eredménye "me" végpont használt tootooobtain hello felhasználói profil – https://graph.microsoft.com/v1.0/me. Általános Microsoft Graph-végpontok listáját lásd: a [cikk](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>További információ a hatókörök és delegált jogosultságokkal sikeresen telepítették

hello Microsoft Graph API szükséges hello `user.read` tooread hello profil hatókörének. Minden egyes a regisztrációs portál regisztrált alkalmazás alapértelmezés szerint automatikusan megjelenik az ebben a hatókörben. Néhány más Microsoft Graph API, továbbá egyéni API-k, a háttér-kiszolgáló további hatókörökkel lehet szükség. Például a Microsoft Graph-hatókör hello `Calendars.Read` van szükség toolist hello felhasználók naptáraiban. Sorrendben tooaccess hello kontextusban az alkalmazás felhasználó naptár, tooadd hello kell `Calendars.Read` delegált engedéllyel toohello alkalmazás regisztrációs adatait, és adja meg az hello `Calendars.Read` hatókör toohello `acquireTokenSilentAsync` hívható meg. hello felhasználói további hozzájárulásokat azoktól hello hatókörök számának növelésével kérheti.

<!--end-collapse-->

---
title: "aaaAzure AD v2 Android bevezetés - konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: e14796c37ab0c30d948b6f783dac80059375afa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Hozzon létre egy alkalmazást (Express)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)
2.  Adjon meg egy nevet az alkalmazás és az e-maileket
3.  Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.
4.  Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás
2. Adjon meg egy nevet az alkalmazás és az e-maileket 
3. Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve
4. Kattintson a `Add Platform`, majd jelölje be `Native Application` , majd kattintson a Mentés
5.  Nyissa meg `MainActivity` (alatt `app`  >  `java`  >   *`{host}.{namespace}`* )
6.  Cserélje le a hello *[hello alkalmazás azonosítója itt meg]* kezdve hello sorban `final static String CLIENT_ID` csak regisztrált hello alkalmazás azonosítójú:

```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Nyissa meg `AndroidManifest.xml` (alatt `app`  >  `manifests`) túl a következő tevékenység hozzáadása hello`manifest\application` csomópont. Ezzel a `BrowserTabActivity` tooallow hello OS tooresume hello hitelesítés végrehajtása után az alkalmazás:
</li>
</ol>

```xml
<!--Intent filter toocapture System Browser calling back tooour app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        
        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, hello scheme should be similar too'msal[appId]' -->
        <data android:scheme="msal[Enter hello application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```
<!-- Workaround for Docs conversion bug -->
<ol start="8">
<li>
A hello `BrowserTabActivity`, cserélje le `[Enter hello application Id here]` hello alkalmazásazonosítóval
</li>
</ol>

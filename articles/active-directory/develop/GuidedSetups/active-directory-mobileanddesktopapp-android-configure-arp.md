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
ms.openlocfilehash: eaa41805c92212154ee8d51d3eb3aee1202eef1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="add-hello-applications-registration-information-tooyour-app"></a>Hello alkalmazás regisztrációs információ tooyour alkalmazás hozzáadása

Ebben a lépésben kell tooadd hello ügyfél-azonosító tooyour projekt.

1.  Nyissa meg `MainActivity` (alatt `app`  >  `java`  >   *`{host}.{namespace}`* )
2.  Cserélje le a hello kezdetű sort `final static String CLIENT_ID` együtt:
```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
3. Nyissa meg:`app` > `manifests` > `AndroidManifest.xml`
4. Adja hozzá a következő tevékenység túl hello`manifest\application` csomópont. A regisztráció egy `BrowserTabActivity` tooallow hello OS tooresume hello hitelesítés végrehajtása után az alkalmazás:

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

### <a name="what-is-next"></a>Mi az a következő

[Tesztelése és érvényesítése](active-directory-mobileanddesktopapp-android-test.md)

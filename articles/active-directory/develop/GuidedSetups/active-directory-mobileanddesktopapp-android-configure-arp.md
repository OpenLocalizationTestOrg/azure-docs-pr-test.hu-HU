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
## <a name="add-hello-applications-registration-information-tooyour-app"></a><span data-ttu-id="e8da5-103">Hello alkalmazás regisztrációs információ tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e8da5-103">Add hello application’s registration information tooyour app</span></span>

<span data-ttu-id="e8da5-104">Ebben a lépésben kell tooadd hello ügyfél-azonosító tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="e8da5-104">In this step, you need tooadd hello Client ID tooyour project.</span></span>

1.  <span data-ttu-id="e8da5-105">Nyissa meg `MainActivity` (alatt `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="e8da5-105">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
2.  <span data-ttu-id="e8da5-106">Cserélje le a hello kezdetű sort `final static String CLIENT_ID` együtt:</span><span class="sxs-lookup"><span data-stu-id="e8da5-106">Replace hello line starting with `final static String CLIENT_ID` with:</span></span>
```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
3. <span data-ttu-id="e8da5-107">Nyissa meg:`app` > `manifests` > `AndroidManifest.xml`</span><span class="sxs-lookup"><span data-stu-id="e8da5-107">Open: `app` > `manifests` > `AndroidManifest.xml`</span></span>
4. <span data-ttu-id="e8da5-108">Adja hozzá a következő tevékenység túl hello`manifest\application` csomópont.</span><span class="sxs-lookup"><span data-stu-id="e8da5-108">Add hello following activity too`manifest\application` node.</span></span> <span data-ttu-id="e8da5-109">A regisztráció egy `BrowserTabActivity` tooallow hello OS tooresume hello hitelesítés végrehajtása után az alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="e8da5-109">This register a `BrowserTabActivity` tooallow hello OS tooresume your application after completing hello authentication:</span></span>

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

### <a name="what-is-next"></a><span data-ttu-id="e8da5-110">Mi az a következő</span><span class="sxs-lookup"><span data-stu-id="e8da5-110">What is Next</span></span>

[<span data-ttu-id="e8da5-111">Tesztelése és érvényesítése</span><span class="sxs-lookup"><span data-stu-id="e8da5-111">Test and Validate</span></span>](active-directory-mobileanddesktopapp-android-test.md)

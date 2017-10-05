---
title: "Az Azure AD v2 Android első lépések – konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: c09937582118ebcc5b8cbc1f43a0a2019f2f7a89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
## <a name="add-the-applications-registration-information-to-your-app"></a><span data-ttu-id="225d8-103">Az alkalmazás regisztrációs adatok hozzáadása az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="225d8-103">Add the application’s registration information to your app</span></span>

<span data-ttu-id="225d8-104">Ebben a lépésben kell hozzáadnia az ügyfél-Azonosítót a projekthez.</span><span class="sxs-lookup"><span data-stu-id="225d8-104">In this step, you need to add the Client ID to your project.</span></span>

1.  <span data-ttu-id="225d8-105">Nyissa meg `MainActivity` (alatt `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="225d8-105">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
2.  <span data-ttu-id="225d8-106">Cserélje le a kezdetű sort `final static String CLIENT_ID` együtt:</span><span class="sxs-lookup"><span data-stu-id="225d8-106">Replace the line starting with `final static String CLIENT_ID` with:</span></span>
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. <span data-ttu-id="225d8-107">Nyissa meg:`app` > `manifests` > `AndroidManifest.xml`</span><span class="sxs-lookup"><span data-stu-id="225d8-107">Open: `app` > `manifests` > `AndroidManifest.xml`</span></span>
4. <span data-ttu-id="225d8-108">Adja hozzá a következő tevékenységek `manifest\application` csomópont.</span><span class="sxs-lookup"><span data-stu-id="225d8-108">Add the following activity to `manifest\application` node.</span></span> <span data-ttu-id="225d8-109">A regisztráció egy `BrowserTabActivity` engedélyezi a hitelesítés befejezése után az alkalmazás folytatja az operációs rendszer:</span><span class="sxs-lookup"><span data-stu-id="225d8-109">This register a `BrowserTabActivity` to allow the OS to resume your application after completing the authentication:</span></span>

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

### <a name="what-is-next"></a><span data-ttu-id="225d8-110">Mi az a következő</span><span class="sxs-lookup"><span data-stu-id="225d8-110">What is Next</span></span>

[<span data-ttu-id="225d8-111">Tesztelése és érvényesítése</span><span class="sxs-lookup"><span data-stu-id="225d8-111">Test and Validate</span></span>](active-directory-mobileanddesktopapp-android-test.md)

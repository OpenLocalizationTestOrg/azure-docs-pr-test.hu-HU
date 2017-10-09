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
## <a name="create-an-application-express"></a><span data-ttu-id="603da-103">Hozzon létre egy alkalmazást (Express)</span><span class="sxs-lookup"><span data-stu-id="603da-103">Create an application (Express)</span></span>
<span data-ttu-id="603da-104">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="603da-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="603da-105">Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)</span><span class="sxs-lookup"><span data-stu-id="603da-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure)</span></span>
2.  <span data-ttu-id="603da-106">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="603da-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="603da-107">Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="603da-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="603da-108">Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot</span><span class="sxs-lookup"><span data-stu-id="603da-108">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="603da-109">Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)</span><span class="sxs-lookup"><span data-stu-id="603da-109">Add your application registration information tooyour solution (Advanced)</span></span>
<span data-ttu-id="603da-110">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="603da-110">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="603da-111">Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás</span><span class="sxs-lookup"><span data-stu-id="603da-111">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app) tooregister an application</span></span>
2. <span data-ttu-id="603da-112">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="603da-112">Enter a name for your application and your email</span></span> 
3. <span data-ttu-id="603da-113">Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve</span><span class="sxs-lookup"><span data-stu-id="603da-113">Make sure hello option for Guided Setup is unchecked</span></span>
4. <span data-ttu-id="603da-114">Kattintson a `Add Platform`, majd jelölje be `Native Application` , majd kattintson a Mentés</span><span class="sxs-lookup"><span data-stu-id="603da-114">Click `Add Platform`, then select `Native Application` and hit Save</span></span>
5.  <span data-ttu-id="603da-115">Nyissa meg `MainActivity` (alatt `app`  >  `java`  >   *`{host}.{namespace}`* )</span><span class="sxs-lookup"><span data-stu-id="603da-115">Open `MainActivity` (under `app` > `java` > *`{host}.{namespace}`*)</span></span>
6.  <span data-ttu-id="603da-116">Cserélje le a hello *[hello alkalmazás azonosítója itt meg]* kezdve hello sorban `final static String CLIENT_ID` csak regisztrált hello alkalmazás azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="603da-116">Replace hello *[Enter hello application Id here]* in hello line starting with `final static String CLIENT_ID` with hello application ID you just registered:</span></span>

```java
final static String CLIENT_ID = "[Enter hello application Id here]";
```
<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
<span data-ttu-id="603da-117">Nyissa meg `AndroidManifest.xml` (alatt `app`  >  `manifests`) túl a következő tevékenység hozzáadása hello`manifest\application` csomópont.</span><span class="sxs-lookup"><span data-stu-id="603da-117">Open `AndroidManifest.xml` (under `app` > `manifests`) Add hello following activity too`manifest\application` node.</span></span> <span data-ttu-id="603da-118">Ezzel a `BrowserTabActivity` tooallow hello OS tooresume hello hitelesítés végrehajtása után az alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="603da-118">This registers a `BrowserTabActivity` tooallow hello OS tooresume your application after completing hello authentication:</span></span>
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
<span data-ttu-id="603da-119">A hello `BrowserTabActivity`, cserélje le `[Enter hello application Id here]` hello alkalmazásazonosítóval</span><span class="sxs-lookup"><span data-stu-id="603da-119">In hello `BrowserTabActivity`, replace `[Enter hello application Id here]` with hello application ID.</span></span>
</li>
</ol>

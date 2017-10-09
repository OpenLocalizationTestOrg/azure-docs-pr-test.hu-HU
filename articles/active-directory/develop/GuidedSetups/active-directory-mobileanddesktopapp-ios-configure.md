---
title: "aaaAzure AD v2 iOS – első lépések – konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: 537cc7f0de6cd947fe340566c9e93f8bb08d57a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="d7f80-103">Hozzon létre egy alkalmazást (Express)</span><span class="sxs-lookup"><span data-stu-id="d7f80-103">Create an application (Express)</span></span>
<span data-ttu-id="d7f80-104">Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="d7f80-104">Now you need tooregister your application in hello *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="d7f80-105">Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span><span class="sxs-lookup"><span data-stu-id="d7f80-105">Register your application via hello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span></span>
2.  <span data-ttu-id="d7f80-106">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="d7f80-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="d7f80-107">Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="d7f80-107">Make sure hello option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="d7f80-108">Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot</span><span class="sxs-lookup"><span data-stu-id="d7f80-108">Follow hello instructions tooobtain hello application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a><span data-ttu-id="d7f80-109">Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)</span><span class="sxs-lookup"><span data-stu-id="d7f80-109">Add your application registration information tooyour solution (Advanced)</span></span>

1.  <span data-ttu-id="d7f80-110">Nyissa meg túl[Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app)</span><span class="sxs-lookup"><span data-stu-id="d7f80-110">Go too[Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app)</span></span>
2.  <span data-ttu-id="d7f80-111">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="d7f80-111">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="d7f80-112">Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve</span><span class="sxs-lookup"><span data-stu-id="d7f80-112">Make sure hello option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="d7f80-113">Kattintson a `Add Platform`, majd jelölje be `Native Application` kattintson`Save`</span><span class="sxs-lookup"><span data-stu-id="d7f80-113">Click `Add Platform`, then select `Native Application` and click `Save`</span></span>
5.  <span data-ttu-id="d7f80-114">Lépjen vissza a tooXcode.</span><span class="sxs-lookup"><span data-stu-id="d7f80-114">Go back tooXcode.</span></span> <span data-ttu-id="d7f80-115">A `ViewController.swift`, cserélje le a hello kezdetű sort "`let kClientID`" csak regisztrált hello alkalmazás azonosítójú:</span><span class="sxs-lookup"><span data-stu-id="d7f80-115">In `ViewController.swift`, replace hello line starting with '`let kClientID`' with hello application ID you just registered:</span></span>

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
<span data-ttu-id="d7f80-116">CTRL + kattintson <code>Info.plist</code> toobring hello helyi menü kialakításához, és kattintson: <code>Open As</code>> <code>Source Code</code>
</span><span class="sxs-lookup"><span data-stu-id="d7f80-116">Control+click <code>Info.plist</code> toobring up hello contextual menu, and then click: <code>Open As</code> > <code>Source Code</code>
</span></span></li>
<li>
<span data-ttu-id="d7f80-117">A hello <code>dict</code> gyökércsomópont, adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="d7f80-117">Under hello <code>dict</code> root node, add hello following:</span></span>
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
<span data-ttu-id="d7f80-118">Cserélje le <i> <code>[Your_Application_Id_Here]</code> </i> a hello csak regisztrált alkalmazás azonosítója</span><span class="sxs-lookup"><span data-stu-id="d7f80-118">Replace <i><code>[Your_Application_Id_Here]</code></i> with hello Application Id you just registered</span></span>
</li>
</ol>

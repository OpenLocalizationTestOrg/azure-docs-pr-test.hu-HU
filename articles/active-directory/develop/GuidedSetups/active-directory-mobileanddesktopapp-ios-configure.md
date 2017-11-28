---
title: "Bevezetés az Azure AD v2 iOS - konfigurálása |} Microsoft Docs"
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
ms.openlocfilehash: 0ebca65585fc87bd4a85ba092cd423fce9540f58
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
## <a name="create-an-application-express"></a><span data-ttu-id="e1d50-103">Hozzon létre egy alkalmazást (Express)</span><span class="sxs-lookup"><span data-stu-id="e1d50-103">Create an application (Express)</span></span>
<span data-ttu-id="e1d50-104">Most kell regisztrálnia az alkalmazást a *Microsoft alkalmazásregisztrációs portálra*:</span><span class="sxs-lookup"><span data-stu-id="e1d50-104">Now you need to register your application in the *Microsoft Application Registration Portal*:</span></span>
1. <span data-ttu-id="e1d50-105">Az alkalmazás regisztrálása a [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span><span class="sxs-lookup"><span data-stu-id="e1d50-105">Register your application via the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)</span></span>
2.  <span data-ttu-id="e1d50-106">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="e1d50-106">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="e1d50-107">Győződjön meg arról, hogy az interaktív telepítés beállítást</span><span class="sxs-lookup"><span data-stu-id="e1d50-107">Make sure the option for Guided Setup is checked</span></span>
4.  <span data-ttu-id="e1d50-108">Az utasítások beszerzéséhez, és illessze be a kódot</span><span class="sxs-lookup"><span data-stu-id="e1d50-108">Follow the instructions to obtain the application ID and paste it into your code</span></span>

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a><span data-ttu-id="e1d50-109">Az alkalmazás regisztrációs adatokat hozzáadni a megoldás (speciális)</span><span class="sxs-lookup"><span data-stu-id="e1d50-109">Add your application registration information to your solution (Advanced)</span></span>

1.  <span data-ttu-id="e1d50-110">Ugrás a [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app)</span><span class="sxs-lookup"><span data-stu-id="e1d50-110">Go to [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/portal/register-app)</span></span>
2.  <span data-ttu-id="e1d50-111">Adjon meg egy nevet az alkalmazás és az e-maileket</span><span class="sxs-lookup"><span data-stu-id="e1d50-111">Enter a name for your application and your email</span></span>
3.  <span data-ttu-id="e1d50-112">Győződjön meg arról, hogy az interaktív telepítés beállítás nincs bejelölve</span><span class="sxs-lookup"><span data-stu-id="e1d50-112">Make sure the option for Guided Setup is unchecked</span></span>
4.  <span data-ttu-id="e1d50-113">Kattintson a `Add Platform`, majd jelölje be `Native Application` kattintson`Save`</span><span class="sxs-lookup"><span data-stu-id="e1d50-113">Click `Add Platform`, then select `Native Application` and click `Save`</span></span>
5.  <span data-ttu-id="e1d50-114">Térjen vissza az xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="e1d50-114">Go back to Xcode.</span></span> <span data-ttu-id="e1d50-115">A `ViewController.swift`, cserélje le a kezdetű sort "`let kClientID`" az imént regisztrált alkalmazás azonosítójával:</span><span class="sxs-lookup"><span data-stu-id="e1d50-115">In `ViewController.swift`, replace the line starting with '`let kClientID`' with the application ID you just registered:</span></span>

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
<span data-ttu-id="e1d50-116">CTRL + kattintson <code>Info.plist</code> a helyi menü, és kattintson a: <code>Open As</code>> <code>Source Code</code>
</span><span class="sxs-lookup"><span data-stu-id="e1d50-116">Control+click <code>Info.plist</code> to bring up the contextual menu, and then click: <code>Open As</code> > <code>Source Code</code>
</span></span></li>
<li>
<span data-ttu-id="e1d50-117">Az a <code>dict</code> gyökércsomópont, adja hozzá a következő:</span><span class="sxs-lookup"><span data-stu-id="e1d50-117">Under the <code>dict</code> root node, add the following:</span></span>
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
<span data-ttu-id="e1d50-118">Cserélje le <i> <code>[Your_Application_Id_Here]</code> </i> az imént regisztrált alkalmazás azonosítójával</span><span class="sxs-lookup"><span data-stu-id="e1d50-118">Replace <i><code>[Your_Application_Id_Here]</code></i> with the Application Id you just registered</span></span>
</li>
</ol>

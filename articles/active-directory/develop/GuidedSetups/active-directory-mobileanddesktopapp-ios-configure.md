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
## <a name="create-an-application-express"></a>Hozzon létre egy alkalmazást (Express)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Adjon meg egy nevet az alkalmazás és az e-maileket
3.  Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.
4.  Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)

1.  Nyissa meg túl[Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app)
2.  Adjon meg egy nevet az alkalmazás és az e-maileket
3.  Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve
4.  Kattintson a `Add Platform`, majd jelölje be `Native Application` kattintson`Save`
5.  Lépjen vissza a tooXcode. A `ViewController.swift`, cserélje le a hello kezdetű sort "`let kClientID`" csak regisztrált hello alkalmazás azonosítójú:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
CTRL + kattintson <code>Info.plist</code> toobring hello helyi menü kialakításához, és kattintson: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
A hello <code>dict</code> gyökércsomópont, adja hozzá a következő hello:
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
Cserélje le <i> <code>[Your_Application_Id_Here]</code> </i> a hello csak regisztrált alkalmazás azonosítója
</li>
</ol>

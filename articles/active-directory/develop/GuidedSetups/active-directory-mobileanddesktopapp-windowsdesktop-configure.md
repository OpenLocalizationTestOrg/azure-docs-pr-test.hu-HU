---
title: "aaaAzure AD v2 Windows asztali bevezetés - Config |} Microsoft Docs"
description: "A Windows asztali .NET (XAML) alkalmazások hogyan szereznie egy hozzáférési jogkivonatot, és a hívja az API-k védi az Azure Active Directory v2 végpont. | A Microsoft Azure |} A Microsoft Azure"
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
ms.openlocfilehash: 39c257e3e0cb09491f6fe005877601bd46824d12
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
## <a name="create-an-application-express"></a>Hozzon létre egy alkalmazást (Express)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Hello keresztül az alkalmazás regisztrálása [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Adjon meg egy nevet az alkalmazás és az e-maileket
3.  Ellenőrizze, hogy az interaktív telepítés hello beállítás be van jelölve.
4.  Hajtsa végre a hello utasításokat tooobtain hello Alkalmazásazonosító, és illessze be a kódot

### <a name="add-your-application-registration-information-tooyour-solution-advanced"></a>Adja hozzá a regisztrációs adatokat tooyour megoldás (speciális)
Most tooregister hello az alkalmazás *Microsoft alkalmazásregisztrációs portálra*:
1. Nyissa meg toohello [Microsoft alkalmazásregisztrációs portálra](https://apps.dev.microsoft.com/portal/register-app) tooregister alkalmazás
2. Adjon meg egy nevet az alkalmazás és az e-maileket 
3. Győződjön meg arról, hogy az interaktív telepítés hello beállítás nincs bejelölve
4. Kattintson a `Add Platform`, majd jelölje be `Native Application` , majd kattintson a Mentés
5. Másoljon át hello GUID azonosítója, lépjen vissza tooVisual Studio, a nyílt `App.xaml.cs` , és cserélje le `your_client_id_here` az imént regisztrált Alkalmazásazonosító hello:

```csharp
private static string ClientId = "your_application_id_here";
```

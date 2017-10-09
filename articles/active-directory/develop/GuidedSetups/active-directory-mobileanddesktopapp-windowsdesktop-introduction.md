---
title: "aaaAzure AD v2 Windows asztali első lépések – bevezetés |} Microsoft Docs"
description: "Hogyan Windows asztali .NET (XAML) alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
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
ms.openlocfilehash: 89d98fc46190ba9e47b7c3095f91e32eca455fcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-windows-desktop-app"></a>Microsoft Graph API hello felelnek meg, és a Windows asztali alkalmazások

Ez az útmutató ismerteti, hogyan a natív Windows asztali .NET (XAML) alkalmazás szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k.

Ez az útmutató végén hello, az alkalmazás fogja kell tudni toocall egy védett API-t (például outlook.com, live.com és mások) személyes fiókok használatát, valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.  

> Ez az útmutató a Visual Studio 2015 Update 3 vagy a Visual Studio 2017 van szükség.  Nem rendelkezik? [A Visual Studio 2017 ingyenesen letölthető](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Ez az útmutató működése

![Ez az útmutató működése](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

Ez az útmutató által létrehozott hello mintaalkalmazás lehetővé teszi, hogy a Windows asztali alkalmazások tooquery Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el. Ebben a forgatókönyvben egy token hozzá tooHTTP kérelmek keresztül hello Authorization fejlécet. Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Webes API-k eléréséhez token beszerzési kezelése védett.

Miután hello felhasználó hitelesíti magát, hello mintaalkalmazás kap egy jogkivonatot, amely lehet használt tooquery Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.

API-k, mint például a Microsoft Graph szükséges egy adott erőforrások – például tooread elérése access token tooallow felhasználói profil felhasználó naptár eléréséhez, vagy egy e-mailek küldése. Az alkalmazás kérheti egy hozzáférési jogkivonat segítségével MSAL tooaccess ezeket az erőforrásokat API hatókörök megadásával. Ez a jogkivonat nem hozzáadott toohello HTTP Authorization fejlécet minden hívás hello ellen védett erőforrás. 

MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.


### <a name="nuget-packages"></a>NuGet-csomagok

Ez az útmutató a következő NuGet-csomagok hello használja:

|Részletes ismertetés|Leírás|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft hitelesítési könyvtár (MSAL)|


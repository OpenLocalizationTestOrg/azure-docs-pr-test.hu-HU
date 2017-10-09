---
title: "aaaAzure AD v2 iOS Getting Started – bevezetés |} Microsoft Docs"
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
ms.openlocfilehash: f40aebbb75490912e533aecc7eedfb2b2dcd8c6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-an-ios-app"></a>Az iOS-alkalmazásokból hello Microsoft Graph API hívása

Ez az útmutató ismerteti, hogyan olyan natív iOS-alkalmazás (Swift) access token beszerzése és hello Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k hívása.

Ez az útmutató végén hello, az alkalmazás fogja kell tudni toocall egy védett API-t (például outlook.com, live.com és mások) személyes fiókok használatát, valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.

> ### <a name="pre-requisites"></a>Előfeltételek
> - XCode 8.x elengedhetetlen ahhoz, hogy ez az útmutató. Letöltheti az XCode [innen](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode letöltési URL-cím").
> - [Carthage](https://github.com/Carthage/Carthage) a felügyeleti csomag

### <a name="how-this-guide-works"></a>Ez az útmutató működése

![Ez az útmutató működése](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

Ez az útmutató által létrehozott hello mintaalkalmazás lehetővé teszi, hogy az iOS alkalmazás tooquery hello Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el. Ebben a forgatókönyvben egy token hozzá tooHTTP kérelmek keresztül hello Authorization fejlécet. Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Webes API-k eléréséhez token beszerzési kezelése védett.

Miután hello felhasználó hitelesíti magát, hello mintaalkalmazás kap egy jogkivonatot, amely lehet használt tooquery hello Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.

API-k, mint például a Microsoft Graph szükséges egy adott erőforrások – például tooread elérése access token tooallow felhasználói profil felhasználó naptár eléréséhez, vagy egy e-mailek küldése. Az alkalmazás olyan hozzáférési jogkivonatot API hatókörök megadásával MSAL használatával kérhet. Ez a jogkivonat nem hozzáadott toohello HTTP Authorization fejlécet minden hívás hello ellen védett erőforrás.

MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.


### <a name="nuget-packages"></a>NuGet-csomagok

Ez az útmutató a következő NuGet-csomagok hello használja:

|Részletes ismertetés|Leírás|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|IOS rendszerhez készült Microsoft hitelesítési könyvtár előzetes verzió|


---
title: "aaaAzure AD v2 Android első lépések – bevezetés |} Microsoft Docs"
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
ms.openlocfilehash: 7c76ab8bbf1a6c934ab672cccf85ae82f03f601a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-an-android-app"></a>Hello Microsoft Graph API hívása az Android-alkalmazás

Ez az útmutató ismerteti, hogyan natív Android-alkalmazás szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k.

Ez az útmutató végén hello, az alkalmazás fogja kell tudni toocall egy védett API-t (például outlook.com, live.com és mások) személyes fiókok használatát, valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.  

### <a name="how-this-sample-works"></a>Ez a minta működése
![Ez a minta működése](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Ez az útmutató által létrehozott hello minta egy olyan forgatókönyvet, ahol egy Android-alkalmazás az használt tooquery egy webes API-t, amely az Azure Active Directory v2 végpont – ebben az esetben a Microsoft Graph API származó jogkivonatokat fogad alapul. Ebben a forgatókönyvben egy token hozzá tooHTTP kérelmek keresztül hello Authorization fejlécet. Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.

### <a name="pre-requisites"></a>Előfeltételek
* Ez az interaktív telepítés Android Studio összpontosít, de bármely olyan Android-alkalmazás fejlesztői környezetben is elfogadható. 
* Android SDK 21 vagy újabb rendszer szükséges (SDK 25 ajánlott).
* Google Chrome vagy egy webes böngésző egyéni lapok használatával szükség ebben a kiadásban Microsoft hitelesítési könyvtár (MSAL) Android rendszerhez.

> Megjegyzés: Google Chrome nem szerepel a Visual Studio-emulátor Android rendszerhez. Javasoljuk, hogy tootest ezt a kódot az API-25 vagy a kép az emulátor, API 21 vagy újabb Google Chrome telepített.


### <a name="how-toohandle-token-acquisition-tooaccess-a-protected-web-api"></a>Hogyan toohandle token beszerzési tooaccess egy védett webes API

Miután hello felhasználó hitelesíti magát, hello mintaalkalmazás kap egy jogkivonatot, amely lehet használt tooquery Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.

API-k, mint például a Microsoft Graph szükséges egy adott erőforrások – például tooread elérése access token tooallow felhasználói profil felhasználó naptár eléréséhez, vagy egy e-mailek küldése. Az alkalmazás kérheti egy hozzáférési jogkivonat segítségével MSAL tooaccess ezeket az erőforrásokat API hatókörök megadásával. Ez a jogkivonat nem hozzáadott toohello HTTP Authorization fejlécet minden hívás hello ellen védett erőforrás. 

MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.

### <a name="libraries"></a>Szalagtárak

Ez az útmutató a következő könyvtárak hello használja:

|Részletes ismertetés|Leírás|
|---|---|
|[com.microsoft.Identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft hitelesítési könyvtár (MSAL)|

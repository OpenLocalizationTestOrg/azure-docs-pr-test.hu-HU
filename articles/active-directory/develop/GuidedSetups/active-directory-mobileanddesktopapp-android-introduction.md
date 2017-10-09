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
# <a name="call-hello-microsoft-graph-api-from-an-android-app"></a><span data-ttu-id="3a575-103">Hello Microsoft Graph API hívása az Android-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3a575-103">Call hello Microsoft Graph API from an Android app</span></span>

<span data-ttu-id="3a575-104">Ez az útmutató ismerteti, hogyan natív Android-alkalmazás szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k.</span><span class="sxs-lookup"><span data-stu-id="3a575-104">This guide demonstrates how a native Android application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="3a575-105">Ez az útmutató végén hello, az alkalmazás fogja kell tudni toocall egy védett API-t (például outlook.com, live.com és mások) személyes fiókok használatát, valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="3a575-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

### <a name="how-this-sample-works"></a><span data-ttu-id="3a575-106">Ez a minta működése</span><span class="sxs-lookup"><span data-stu-id="3a575-106">How this sample works</span></span>
![Ez a minta működése](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

<span data-ttu-id="3a575-108">Ez az útmutató által létrehozott hello minta egy olyan forgatókönyvet, ahol egy Android-alkalmazás az használt tooquery egy webes API-t, amely az Azure Active Directory v2 végpont – ebben az esetben a Microsoft Graph API származó jogkivonatokat fogad alapul.</span><span class="sxs-lookup"><span data-stu-id="3a575-108">hello sample created by this guide is based on a scenario where an Android application is used tooquery a Web API that accepts tokens from Azure Active Directory v2 endpoint – in this case, Microsoft Graph API.</span></span> <span data-ttu-id="3a575-109">Ebben a forgatókönyvben egy token hozzá tooHTTP kérelmek keresztül hello Authorization fejlécet.</span><span class="sxs-lookup"><span data-stu-id="3a575-109">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="3a575-110">Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.</span><span class="sxs-lookup"><span data-stu-id="3a575-110">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>

### <a name="pre-requisites"></a><span data-ttu-id="3a575-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3a575-111">Pre-requisites</span></span>
* <span data-ttu-id="3a575-112">Ez az interaktív telepítés Android Studio összpontosít, de bármely olyan Android-alkalmazás fejlesztői környezetben is elfogadható.</span><span class="sxs-lookup"><span data-stu-id="3a575-112">This guided setup is focused on Android Studio, but any other Android application development environment is also acceptable.</span></span> 
* <span data-ttu-id="3a575-113">Android SDK 21 vagy újabb rendszer szükséges (SDK 25 ajánlott).</span><span class="sxs-lookup"><span data-stu-id="3a575-113">Android SDK 21 or newer is required (SDK 25 is recommended).</span></span>
* <span data-ttu-id="3a575-114">Google Chrome vagy egy webes böngésző egyéni lapok használatával szükség ebben a kiadásban Microsoft hitelesítési könyvtár (MSAL) Android rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="3a575-114">Google Chrome or a web browser using Custom Tabs is required for this release of Microsoft Authentication Library (MSAL) for Android.</span></span>

> <span data-ttu-id="3a575-115">Megjegyzés: Google Chrome nem szerepel a Visual Studio-emulátor Android rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="3a575-115">Note: Google Chrome is not included on Visual Studio Emulator for Android.</span></span> <span data-ttu-id="3a575-116">Javasoljuk, hogy tootest ezt a kódot az API-25 vagy a kép az emulátor, API 21 vagy újabb Google Chrome telepített.</span><span class="sxs-lookup"><span data-stu-id="3a575-116">We recommend you tootest this code on an Emulator with API 25 or an image with API 21 or newer that has with Google Chrome installed.</span></span>


### <a name="how-toohandle-token-acquisition-tooaccess-a-protected-web-api"></a><span data-ttu-id="3a575-117">Hogyan toohandle token beszerzési tooaccess egy védett webes API</span><span class="sxs-lookup"><span data-stu-id="3a575-117">How toohandle token acquisition tooaccess a protected Web API</span></span>

<span data-ttu-id="3a575-118">Miután hello felhasználó hitelesíti magát, hello mintaalkalmazás kap egy jogkivonatot, amely lehet használt tooquery Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.</span><span class="sxs-lookup"><span data-stu-id="3a575-118">After hello user authenticates, hello sample application receives a token that can be used tooquery Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="3a575-119">API-k, mint például a Microsoft Graph szükséges egy adott erőforrások – például tooread elérése access token tooallow felhasználói profil felhasználó naptár eléréséhez, vagy egy e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="3a575-119">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="3a575-120">Az alkalmazás kérheti egy hozzáférési jogkivonat segítségével MSAL tooaccess ezeket az erőforrásokat API hatókörök megadásával.</span><span class="sxs-lookup"><span data-stu-id="3a575-120">Your application can request an access token using MSAL tooaccess these resources by specifying API scopes.</span></span> <span data-ttu-id="3a575-121">Ez a jogkivonat nem hozzáadott toohello HTTP Authorization fejlécet minden hívás hello ellen védett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3a575-121">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span> 

<span data-ttu-id="3a575-122">MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3a575-122">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>

### <a name="libraries"></a><span data-ttu-id="3a575-123">Szalagtárak</span><span class="sxs-lookup"><span data-stu-id="3a575-123">Libraries</span></span>

<span data-ttu-id="3a575-124">Ez az útmutató a következő könyvtárak hello használja:</span><span class="sxs-lookup"><span data-stu-id="3a575-124">This guide uses hello following libraries:</span></span>

|<span data-ttu-id="3a575-125">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="3a575-125">Library</span></span>|<span data-ttu-id="3a575-126">Leírás</span><span class="sxs-lookup"><span data-stu-id="3a575-126">Description</span></span>|
|---|---|
|[<span data-ttu-id="3a575-127">com.microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="3a575-127">com.microsoft.identity.client</span></span>](http://javadoc.io/doc/com.microsoft.identity.client/msal)|<span data-ttu-id="3a575-128">Microsoft hitelesítési könyvtár (MSAL)</span><span class="sxs-lookup"><span data-stu-id="3a575-128">Microsoft Authentication Library (MSAL)</span></span>|

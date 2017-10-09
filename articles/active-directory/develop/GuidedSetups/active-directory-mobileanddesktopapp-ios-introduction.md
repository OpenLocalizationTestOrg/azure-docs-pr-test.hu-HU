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
# <a name="call-hello-microsoft-graph-api-from-an-ios-app"></a><span data-ttu-id="5ff00-103">Az iOS-alkalmazásokból hello Microsoft Graph API hívása</span><span class="sxs-lookup"><span data-stu-id="5ff00-103">Call hello Microsoft Graph API from an iOS app</span></span>

<span data-ttu-id="5ff00-104">Ez az útmutató ismerteti, hogyan olyan natív iOS-alkalmazás (Swift) access token beszerzése és hello Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="5ff00-104">This guide demonstrates how a native iOS application (Swift) can get an access token and call hello Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="5ff00-105">Ez az útmutató végén hello, az alkalmazás fogja kell tudni toocall egy védett API-t (például outlook.com, live.com és mások) személyes fiókok használatát, valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5ff00-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>

> ### <a name="pre-requisites"></a><span data-ttu-id="5ff00-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5ff00-106">Pre-requisites</span></span>
> - <span data-ttu-id="5ff00-107">XCode 8.x elengedhetetlen ahhoz, hogy ez az útmutató.</span><span class="sxs-lookup"><span data-stu-id="5ff00-107">XCode 8.x is required for this guide.</span></span> <span data-ttu-id="5ff00-108">Letöltheti az XCode [innen](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode letöltési URL-cím").</span><span class="sxs-lookup"><span data-stu-id="5ff00-108">You can download XCode [from here](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").</span></span>
> - <span data-ttu-id="5ff00-109">[Carthage](https://github.com/Carthage/Carthage) a felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="5ff00-109">[Carthage](https://github.com/Carthage/Carthage) for package management</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="5ff00-110">Ez az útmutató működése</span><span class="sxs-lookup"><span data-stu-id="5ff00-110">How this guide works</span></span>

![Ez az útmutató működése](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

<span data-ttu-id="5ff00-112">Ez az útmutató által létrehozott hello mintaalkalmazás lehetővé teszi, hogy az iOS alkalmazás tooquery hello Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="5ff00-112">hello sample application created by this guide enables an iOS application tooquery hello Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="5ff00-113">Ebben a forgatókönyvben egy token hozzá tooHTTP kérelmek keresztül hello Authorization fejlécet.</span><span class="sxs-lookup"><span data-stu-id="5ff00-113">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="5ff00-114">Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.</span><span class="sxs-lookup"><span data-stu-id="5ff00-114">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="5ff00-115">Webes API-k eléréséhez token beszerzési kezelése védett.</span><span class="sxs-lookup"><span data-stu-id="5ff00-115">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="5ff00-116">Miután hello felhasználó hitelesíti magát, hello mintaalkalmazás kap egy jogkivonatot, amely lehet használt tooquery hello Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.</span><span class="sxs-lookup"><span data-stu-id="5ff00-116">After hello user authenticates, hello sample application receives a token that can be used tooquery hello Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="5ff00-117">API-k, mint például a Microsoft Graph szükséges egy adott erőforrások – például tooread elérése access token tooallow felhasználói profil felhasználó naptár eléréséhez, vagy egy e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="5ff00-117">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="5ff00-118">Az alkalmazás olyan hozzáférési jogkivonatot API hatókörök megadásával MSAL használatával kérhet.</span><span class="sxs-lookup"><span data-stu-id="5ff00-118">Your application can request an access token using MSAL by specifying API scopes.</span></span> <span data-ttu-id="5ff00-119">Ez a jogkivonat nem hozzáadott toohello HTTP Authorization fejlécet minden hívás hello ellen védett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5ff00-119">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span>

<span data-ttu-id="5ff00-120">MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5ff00-120">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="5ff00-121">NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="5ff00-121">NuGet packages</span></span>

<span data-ttu-id="5ff00-122">Ez az útmutató a következő NuGet-csomagok hello használja:</span><span class="sxs-lookup"><span data-stu-id="5ff00-122">This guide uses hello following NuGet packages:</span></span>

|<span data-ttu-id="5ff00-123">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="5ff00-123">Library</span></span>|<span data-ttu-id="5ff00-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="5ff00-124">Description</span></span>|
|---|---|
|[<span data-ttu-id="5ff00-125">MSAL.framework</span><span class="sxs-lookup"><span data-stu-id="5ff00-125">MSAL.framework</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|<span data-ttu-id="5ff00-126">IOS rendszerhez készült Microsoft hitelesítési könyvtár előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="5ff00-126">Microsoft Authentication Library Preview for iOS</span></span>|


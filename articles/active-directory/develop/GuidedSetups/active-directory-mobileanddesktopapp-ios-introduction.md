---
title: "Az Azure AD v2 iOS Getting Started – bevezetés |} Microsoft Docs"
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
ms.openlocfilehash: 948693c8501ecc46a1508e5ea085846d0910783e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="call-the-microsoft-graph-api-from-an-ios-app"></a><span data-ttu-id="54f8f-103">Az iOS-alkalmazásoknak a Microsoft Graph API hívását</span><span class="sxs-lookup"><span data-stu-id="54f8f-103">Call the Microsoft Graph API from an iOS app</span></span>

<span data-ttu-id="54f8f-104">Ez az útmutató ismerteti, hogyan olyan natív iOS-alkalmazás (Swift) szereznie egy hozzáférési jogkivonatot és a Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="54f8f-104">This guide demonstrates how a native iOS application (Swift) can get an access token and call the Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="54f8f-105">Ez az útmutató végén az alkalmazás fogja tudni hívható meg egy védett API használatával személyes fiókok (például outlook.com, live.com és mások) valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="54f8f-105">At the end of this guide, your application will be able to call a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>

> ### <a name="pre-requisites"></a><span data-ttu-id="54f8f-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54f8f-106">Pre-requisites</span></span>
> - <span data-ttu-id="54f8f-107">XCode 8.x elengedhetetlen ahhoz, hogy ez az útmutató.</span><span class="sxs-lookup"><span data-stu-id="54f8f-107">XCode 8.x is required for this guide.</span></span> <span data-ttu-id="54f8f-108">Letöltheti az XCode [innen](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode letöltési URL-cím").</span><span class="sxs-lookup"><span data-stu-id="54f8f-108">You can download XCode [from here](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").</span></span>
> - <span data-ttu-id="54f8f-109">[Carthage](https://github.com/Carthage/Carthage) a felügyeleti csomag</span><span class="sxs-lookup"><span data-stu-id="54f8f-109">[Carthage](https://github.com/Carthage/Carthage) for package management</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="54f8f-110">Ez az útmutató működése</span><span class="sxs-lookup"><span data-stu-id="54f8f-110">How this guide works</span></span>

![Ez az útmutató működése](media/active-directory-mobileanddesktopapp-ios-introduction/iosintro.png)

<span data-ttu-id="54f8f-112">A mintaalkalmazás, ez az útmutató által létrehozott lehetővé teszi, hogy az iOS-alkalmazás lekérdezése a Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="54f8f-112">The sample application created by this guide enables an iOS application to query the Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="54f8f-113">Ebben az esetben jogkivonat adni a hitelesítési fejlécéhez via HTTP-kérelmekre.</span><span class="sxs-lookup"><span data-stu-id="54f8f-113">For this scenario, a token is added to HTTP requests via the Authorization header.</span></span> <span data-ttu-id="54f8f-114">Token beszerzése és -megújítás kezelése a Microsoft hitelesítési könyvtár (MSAL).</span><span class="sxs-lookup"><span data-stu-id="54f8f-114">Token acquisition and renewal is handled by the Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="54f8f-115">Webes API-k eléréséhez token beszerzési kezelése védett.</span><span class="sxs-lookup"><span data-stu-id="54f8f-115">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="54f8f-116">Miután a felhasználó hitelesíti magát, a mintaalkalmazást kap egy jogkivonatot, amely segítségével a Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.</span><span class="sxs-lookup"><span data-stu-id="54f8f-116">After the user authenticates, the sample application receives a token that can be used to query the Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="54f8f-117">Például a Microsoft Graph API-k igényelnek olyan hozzáférési jogkivonatot, hogy az adott erőforrások – például elérése egy profil, hozzáférés felhasználói naptár olvasni és e-mailt küldeni.</span><span class="sxs-lookup"><span data-stu-id="54f8f-117">APIs such as Microsoft Graph require an access token to allow accessing specific resources – for example, to read a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="54f8f-118">Az alkalmazás olyan hozzáférési jogkivonatot API hatókörök megadásával MSAL használatával kérhet.</span><span class="sxs-lookup"><span data-stu-id="54f8f-118">Your application can request an access token using MSAL by specifying API scopes.</span></span> <span data-ttu-id="54f8f-119">Ez a jogkivonat majd hozzáadódik a HTTP Authorization fejlécet minden hívás, szemben a védett erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="54f8f-119">This access token is then added to the HTTP Authorization header for every call made against the protected resource.</span></span>

<span data-ttu-id="54f8f-120">MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="54f8f-120">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="54f8f-121">NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="54f8f-121">NuGet packages</span></span>

<span data-ttu-id="54f8f-122">Ez az útmutató a következő NuGet-csomagok használja:</span><span class="sxs-lookup"><span data-stu-id="54f8f-122">This guide uses the following NuGet packages:</span></span>

|<span data-ttu-id="54f8f-123">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="54f8f-123">Library</span></span>|<span data-ttu-id="54f8f-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="54f8f-124">Description</span></span>|
|---|---|
|[<span data-ttu-id="54f8f-125">MSAL.framework</span><span class="sxs-lookup"><span data-stu-id="54f8f-125">MSAL.framework</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|<span data-ttu-id="54f8f-126">IOS rendszerhez készült Microsoft hitelesítési könyvtár előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="54f8f-126">Microsoft Authentication Library Preview for iOS</span></span>|


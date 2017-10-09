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
# <a name="call-hello-microsoft-graph-api-from-a-windows-desktop-app"></a><span data-ttu-id="258b9-103">Microsoft Graph API hello felelnek meg, és a Windows asztali alkalmazások</span><span class="sxs-lookup"><span data-stu-id="258b9-103">Call hello Microsoft Graph API from a Windows Desktop app</span></span>

<span data-ttu-id="258b9-104">Ez az útmutató ismerteti, hogyan a natív Windows asztali .NET (XAML) alkalmazás szereznie egy hozzáférési jogkivonatot és hívható meg Microsoft Graph API vagy egyéb Azure Active Directory v2 végpont a hozzáférési jogkivonatok igénylő API-k.</span><span class="sxs-lookup"><span data-stu-id="258b9-104">This guide demonstrates how a native Windows Desktop .NET (XAML) application can get an access token and call Microsoft Graph API or other APIs that require access tokens from Azure Active Directory v2 endpoint.</span></span>

<span data-ttu-id="258b9-105">Ez az útmutató végén hello, az alkalmazás fogja kell tudni toocall egy védett API-t (például outlook.com, live.com és mások) személyes fiókok használatát, valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="258b9-105">At hello end of this guide, your application will be able toocall a protected API using personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has Azure Active Directory.</span></span>  

> <span data-ttu-id="258b9-106">Ez az útmutató a Visual Studio 2015 Update 3 vagy a Visual Studio 2017 van szükség.</span><span class="sxs-lookup"><span data-stu-id="258b9-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="258b9-107">Nem rendelkezik?</span><span class="sxs-lookup"><span data-stu-id="258b9-107">Don’t have it?</span></span> [<span data-ttu-id="258b9-108">A Visual Studio 2017 ingyenesen letölthető</span><span class="sxs-lookup"><span data-stu-id="258b9-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a><span data-ttu-id="258b9-109">Ez az útmutató működése</span><span class="sxs-lookup"><span data-stu-id="258b9-109">How this guide works</span></span>

![Ez az útmutató működése](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

<span data-ttu-id="258b9-111">Ez az útmutató által létrehozott hello mintaalkalmazás lehetővé teszi, hogy a Windows asztali alkalmazások tooquery Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="258b9-111">hello sample application created by this guide enables a Windows Desktop Application tooquery Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="258b9-112">Ebben a forgatókönyvben egy token hozzá tooHTTP kérelmek keresztül hello Authorization fejlécet.</span><span class="sxs-lookup"><span data-stu-id="258b9-112">For this scenario, a token is added tooHTTP requests via hello Authorization header.</span></span> <span data-ttu-id="258b9-113">Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.</span><span class="sxs-lookup"><span data-stu-id="258b9-113">Token acquisition and renewal is handled by hello Microsoft Authentication Library (MSAL).</span></span>


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a><span data-ttu-id="258b9-114">Webes API-k eléréséhez token beszerzési kezelése védett.</span><span class="sxs-lookup"><span data-stu-id="258b9-114">Handling token acquisition for accessing protected Web APIs</span></span>

<span data-ttu-id="258b9-115">Miután hello felhasználó hitelesíti magát, hello mintaalkalmazás kap egy jogkivonatot, amely lehet használt tooquery Microsoft Graph API vagy egy Microsoft Azure Active Directory v2 által biztosított webes API.</span><span class="sxs-lookup"><span data-stu-id="258b9-115">After hello user authenticates, hello sample application receives a token that can be used tooquery Microsoft Graph API or a Web API secured by Microsoft Azure Active Directory v2.</span></span>

<span data-ttu-id="258b9-116">API-k, mint például a Microsoft Graph szükséges egy adott erőforrások – például tooread elérése access token tooallow felhasználói profil felhasználó naptár eléréséhez, vagy egy e-mailek küldése.</span><span class="sxs-lookup"><span data-stu-id="258b9-116">APIs such as Microsoft Graph require an access token tooallow accessing specific resources – for example, tooread a user’s profile, access user’s calendar or send an email.</span></span> <span data-ttu-id="258b9-117">Az alkalmazás kérheti egy hozzáférési jogkivonat segítségével MSAL tooaccess ezeket az erőforrásokat API hatókörök megadásával.</span><span class="sxs-lookup"><span data-stu-id="258b9-117">Your application can request an access token using MSAL tooaccess these resources by specifying API scopes.</span></span> <span data-ttu-id="258b9-118">Ez a jogkivonat nem hozzáadott toohello HTTP Authorization fejlécet minden hívás hello ellen védett erőforrás.</span><span class="sxs-lookup"><span data-stu-id="258b9-118">This access token is then added toohello HTTP Authorization header for every call made against hello protected resource.</span></span> 

<span data-ttu-id="258b9-119">MSAL kezeli, és a hozzáférési jogkivonatok frissítése, így nem kell az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="258b9-119">MSAL manages caching and refreshing access tokens for you, so your application doesn't need to.</span></span>


### <a name="nuget-packages"></a><span data-ttu-id="258b9-120">NuGet-csomagok</span><span class="sxs-lookup"><span data-stu-id="258b9-120">NuGet Packages</span></span>

<span data-ttu-id="258b9-121">Ez az útmutató a következő NuGet-csomagok hello használja:</span><span class="sxs-lookup"><span data-stu-id="258b9-121">This guide uses hello following NuGet packages:</span></span>

|<span data-ttu-id="258b9-122">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="258b9-122">Library</span></span>|<span data-ttu-id="258b9-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="258b9-123">Description</span></span>|
|---|---|
|[<span data-ttu-id="258b9-124">Microsoft.Identity.Client</span><span class="sxs-lookup"><span data-stu-id="258b9-124">Microsoft.Identity.Client</span></span>](https://www.nuget.org/packages/Microsoft.Identity.Client)|<span data-ttu-id="258b9-125">Microsoft hitelesítési könyvtár (MSAL)</span><span class="sxs-lookup"><span data-stu-id="258b9-125">Microsoft Authentication Library (MSAL)</span></span>|


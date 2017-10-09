---
title: "aaaAzure AD v2 ASP.NET Web Server első lépések – bevezetés |} Microsoft Docs"
description: "A Microsoft bejelentkezés végrehajtási egy ASP.NET-megoldás a hagyományos böngészőalapú webalkalmazás a szabványos OpenID Connect"
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
ms.openlocfilehash: d6449926af2bdad24cbc8e91f74885a08f909103
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-sign-in-with-microsoft-tooan-aspnet-web-app"></a><span data-ttu-id="f7ca7-103">Jelentkezzen be Microsoft tooan ASP.NET webalkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f7ca7-103">Add sign-in with Microsoft tooan ASP.NET web app</span></span>

<span data-ttu-id="f7ca7-104">Ez az útmutató bemutatja, hogyan tooimplement bejelentkezhet egy ASP.NET MVC megoldás használata egy hagyományos webböngésző-alapú webalkalmazás OpenID Connect használata a Microsofttal.</span><span class="sxs-lookup"><span data-stu-id="f7ca7-104">This guide demonstrates how tooimplement sign-in with Microsoft using an ASP.NET MVC solution with a traditional web browser-based application using OpenID Connect.</span></span> 

<span data-ttu-id="f7ca7-105">Ez az útmutató végén hello az alkalmazás kell tudni tooaccept bejelentkezési modulok a személyes fiókok (például outlook.com, live.com és mások), valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely az Azure Active Directoryval integrált.</span><span class="sxs-lookup"><span data-stu-id="f7ca7-105">At hello end of this guide, your application will be able tooaccept sign ins of personal accounts (including outlook.com, live.com, and others) as well as work and school accounts from any company or organization that has integrated with Azure Active Directory.</span></span> 

> <span data-ttu-id="f7ca7-106">Ez az útmutató a Visual Studio 2015 Update 3 vagy a Visual Studio 2017 van szükség.</span><span class="sxs-lookup"><span data-stu-id="f7ca7-106">This guide requires Visual Studio 2015 Update 3 or Visual Studio 2017.</span></span>  <span data-ttu-id="f7ca7-107">Nem rendelkezik?</span><span class="sxs-lookup"><span data-stu-id="f7ca7-107">Don’t have it?</span></span>  [<span data-ttu-id="f7ca7-108">A Visual Studio 2017 ingyenesen letölthető</span><span class="sxs-lookup"><span data-stu-id="f7ca7-108">Download Visual Studio 2017 for free</span></span>](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a><span data-ttu-id="f7ca7-109">Ez az útmutató működése</span><span class="sxs-lookup"><span data-stu-id="f7ca7-109">How this guide works</span></span>

![Ez az útmutató működése](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

<span data-ttu-id="f7ca7-111">Ez az útmutató alapján hello forgatókönyv, ahol a böngésző hozzáfér-e ASP.NET-webhely, a bejelentkezési gomb segítségével egy felhasználó tooauthenticate kér.</span><span class="sxs-lookup"><span data-stu-id="f7ca7-111">This guide is based on hello scenario where a browser accesses an ASP.NET web site, requesting a user tooauthenticate via a sign-in button.</span></span> <span data-ttu-id="f7ca7-112">Ebben a forgatókönyvben a legtöbb hello munkahelyi toorender hello weblap hello kiszolgáló oldalán következik be.</span><span class="sxs-lookup"><span data-stu-id="f7ca7-112">In this scenario, most of hello work toorender hello web page occurs on hello server side.</span></span>

## <a name="libraries"></a><span data-ttu-id="f7ca7-113">Szalagtárak</span><span class="sxs-lookup"><span data-stu-id="f7ca7-113">Libraries</span></span>

<span data-ttu-id="f7ca7-114">Ez az útmutató a következő könyvtárak hello használja:</span><span class="sxs-lookup"><span data-stu-id="f7ca7-114">This guide uses hello following libraries:</span></span>

|<span data-ttu-id="f7ca7-115">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="f7ca7-115">Library</span></span>|<span data-ttu-id="f7ca7-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="f7ca7-116">Description</span></span>|
|---|---|
|[<span data-ttu-id="f7ca7-117">Microsoft.Owin.Security.OpenIdConnect</span><span class="sxs-lookup"><span data-stu-id="f7ca7-117">Microsoft.Owin.Security.OpenIdConnect</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|<span data-ttu-id="f7ca7-118">Alkalmazott szoftver, amely lehetővé teszi, hogy egy alkalmazás toouse OpenIdConnect hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="f7ca7-118">Middleware that enables an application toouse OpenIdConnect for authentication</span></span>|
|[<span data-ttu-id="f7ca7-119">Microsoft.Owin.Security.Cookies</span><span class="sxs-lookup"><span data-stu-id="f7ca7-119">Microsoft.Owin.Security.Cookies</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|<span data-ttu-id="f7ca7-120">Alkalmazott szoftver, amely lehetővé teszi, hogy egy alkalmazás toomaintain felhasználói munkamenet cookie-k használata</span><span class="sxs-lookup"><span data-stu-id="f7ca7-120">Middleware that enables an application toomaintain user session using cookies</span></span>|
|[<span data-ttu-id="f7ca7-121">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="f7ca7-121">Microsoft.Owin.Host.SystemWeb</span></span>](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|<span data-ttu-id="f7ca7-122">Lehetővé teszi, hogy az IIS használatával hello ASP.NET kérelemfeldolgozási sorban OWIN-alapú alkalmazások toorun</span><span class="sxs-lookup"><span data-stu-id="f7ca7-122">Enables OWIN-based applications toorun on IIS using hello ASP.NET request pipeline</span></span>|


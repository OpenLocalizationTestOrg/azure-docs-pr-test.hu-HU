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
# <a name="add-sign-in-with-microsoft-tooan-aspnet-web-app"></a>Jelentkezzen be Microsoft tooan ASP.NET webalkalmazás hozzáadása

Ez az útmutató bemutatja, hogyan tooimplement bejelentkezhet egy ASP.NET MVC megoldás használata egy hagyományos webböngésző-alapú webalkalmazás OpenID Connect használata a Microsofttal. 

Ez az útmutató végén hello az alkalmazás kell tudni tooaccept bejelentkezési modulok a személyes fiókok (például outlook.com, live.com és mások), valamint a munkahelyi és iskolai fiókok bármely vállalat vagy szervezet, amely az Azure Active Directoryval integrált. 

> Ez az útmutató a Visual Studio 2015 Update 3 vagy a Visual Studio 2017 van szükség.  Nem rendelkezik?  [A Visual Studio 2017 ingyenesen letölthető](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Ez az útmutató működése

![Ez az útmutató működése](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Ez az útmutató alapján hello forgatókönyv, ahol a böngésző hozzáfér-e ASP.NET-webhely, a bejelentkezési gomb segítségével egy felhasználó tooauthenticate kér. Ebben a forgatókönyvben a legtöbb hello munkahelyi toorender hello weblap hello kiszolgáló oldalán következik be.

## <a name="libraries"></a>Szalagtárak

Ez az útmutató a következő könyvtárak hello használja:

|Részletes ismertetés|Leírás|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Alkalmazott szoftver, amely lehetővé teszi, hogy egy alkalmazás toouse OpenIdConnect hitelesítéshez|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Alkalmazott szoftver, amely lehetővé teszi, hogy egy alkalmazás toomaintain felhasználói munkamenet cookie-k használata|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Lehetővé teszi, hogy az IIS használatával hello ASP.NET kérelemfeldolgozási sorban OWIN-alapú alkalmazások toorun|


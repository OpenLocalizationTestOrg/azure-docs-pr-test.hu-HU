---
title: "Az Azure Active Directory v2.0-végponttól |} Microsoft Docs"
description: "Megismerkedhet az alkalmazások a Microsoft Account, mind az Azure Active Directory-bejelentkezés létrehozása."
services: active-directory
documentationcenter: 
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 2dee579f-fdf6-474b-bc2c-016c931eaa27
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: dastrock
ms.custom: aaddev
ms.openlocfilehash: 1e286044fb1a1b367fcac2dc14c47f68d5ed120d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a><span data-ttu-id="ebaa4-103">Bejelentkezés Microsoft-Account & egyetlen alkalmazást az Azure AD-felhasználók</span><span class="sxs-lookup"><span data-stu-id="ebaa4-103">Sign-in Microsoft Account & Azure AD users in a single app</span></span>
<span data-ttu-id="ebaa4-104">A múltban két külön rendszer integrálása az alkalmazás fejlesztőjének személyes Microsoft-fiókot is támogatja, és a munkahelyi fiókok az Azure Active Directory számára szükséges.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-104">In the past, an app developer who wanted to support both personal Microsoft accounts and work accounts from Azure Active Directory was required to integrate with two separate systems.</span></span>  <span data-ttu-id="ebaa4-105">A **az Azure AD v2.0-végponttól** bevezet egy új hitelesítési API-verzió, amely lehetővé teszi, hogy jelentkezzen be mindkét típusú fiókok használatával egy egyszerű integráció.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-105">The **Azure AD v2.0 endpoint** introduces a new authentication API version that enables you to sign in both types of accounts using one simple integration.</span></span>  <span data-ttu-id="ebaa4-106">A v2.0-végpontra használó alkalmazások is felhasználhat REST API-kat a [Microsoft Graph](https://graph.microsoft.io) bármely típusú fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-106">Apps that use the v2.0 endpoint can also consume REST APIs from the [Microsoft Graph](https://graph.microsoft.io) using either type of account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="ebaa4-107">Első lépések</span><span class="sxs-lookup"><span data-stu-id="ebaa4-107">Getting Started</span></span>
<span data-ttu-id="ebaa4-108">Válassza ki előnyben részesített platformját az alábbi listából a nyílt forráskódú szalagtárak & keretrendszerek használata az alkalmazás elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-108">Choose your favorite platform from the following list to build an app using our open source libraries & frameworks.</span></span>  <span data-ttu-id="ebaa4-109">Másik lehetőségként segítségével az OAuth 2.0-s & OpenID Connect-protokoll dokumentációját & küldése közvetlenül egy hitelesítési tár használata nélkül protokoll üzeneteket fogadni.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-109">Alternatively, you can use our OAuth 2.0 & OpenID Connect protocol documentation to send & receive protocol messages directly without using an auth library.</span></span>

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a><span data-ttu-id="ebaa4-110">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="ebaa4-110">What's New</span></span>
<span data-ttu-id="ebaa4-111">Itt az információk hasznosak bizonyulnak a Mi & Mi nem lehetséges a v2.0-végponttal ismertetése.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-111">The information here will be useful in understanding what is & what isn't possible with the v2.0 endpoint.</span></span>

* <span data-ttu-id="ebaa4-112">További tudnivalók a [típusú alkalmazásokat hozhat létre a v2.0-végponttal](active-directory-v2-flows.md).</span><span class="sxs-lookup"><span data-stu-id="ebaa4-112">Learn about the [types of apps you can build with the v2.0 endpoint](active-directory-v2-flows.md).</span></span>
* <span data-ttu-id="ebaa4-113">Megérteni a [korlátozások, korlátozások és megkötések](active-directory-v2-limitations.md) a v2.0-végponttal.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-113">Understand the [limitations, restrictions, and constraints](active-directory-v2-limitations.md) with the v2.0 endpoint.</span></span>
* <span data-ttu-id="ebaa4-114">Ez az áttekintő videó a v2.0-végpontra a kivétel:</span><span class="sxs-lookup"><span data-stu-id="ebaa4-114">Check out this overview video for the v2.0 endpoint:</span></span>

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a><span data-ttu-id="ebaa4-115">Referencia</span><span class="sxs-lookup"><span data-stu-id="ebaa4-115">Reference</span></span>
<span data-ttu-id="ebaa4-116">Ezek a hivatkozások akkor lehet hasznos, felderítését lehetővé tevő a platform részletesen:</span><span class="sxs-lookup"><span data-stu-id="ebaa4-116">These links will be useful for exploring the platform in depth:</span></span>

* [<span data-ttu-id="ebaa4-117">v2.0 protokoll referenciái</span><span class="sxs-lookup"><span data-stu-id="ebaa4-117">v2.0 Protocol Reference</span></span>](active-directory-v2-protocols.md)
* [<span data-ttu-id="ebaa4-118">v2.0 jogkivonat referenciái</span><span class="sxs-lookup"><span data-stu-id="ebaa4-118">v2.0 Token Reference</span></span>](active-directory-v2-tokens.md)
* [<span data-ttu-id="ebaa4-119">v2.0 kódtár – dokumentáció</span><span class="sxs-lookup"><span data-stu-id="ebaa4-119">v2.0 Library Reference</span></span>](active-directory-v2-libraries.md)
* [<span data-ttu-id="ebaa4-120">Hatókörök és a v2.0-végpontra jóváhagyása</span><span class="sxs-lookup"><span data-stu-id="ebaa4-120">Scopes and Consent in the v2.0 endpoint</span></span>](active-directory-v2-scopes.md)
* [<span data-ttu-id="ebaa4-121">A Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="ebaa4-121">The Microsoft Graph</span></span>](https://graph.microsoft.io)

## <a name="help--support"></a><span data-ttu-id="ebaa4-122">Súgó és támogatás</span><span class="sxs-lookup"><span data-stu-id="ebaa4-122">Help & Support</span></span>
<span data-ttu-id="ebaa4-123">Az Azure Active Directoryban történő fejlesztéshez ezeken a helyeken találhat segítséget.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-123">These are the best places to get help with developing on Azure Active Directory.</span></span>

* [<span data-ttu-id="ebaa4-124">A Stack Overflow `azure-active-directory` és `adal` címkéi</span><span class="sxs-lookup"><span data-stu-id="ebaa4-124">Stack Overflow's `azure-active-directory` and `adal` tags</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [<span data-ttu-id="ebaa4-125">Visszajelzés az Azure Active Directoryról</span><span class="sxs-lookup"><span data-stu-id="ebaa4-125">Feedback on Azure Active Directory</span></span>](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> <span data-ttu-id="ebaa4-126">Ha csak szeretné jelentkezzen be munkahelyi és iskolai fiókok az Azure Active Directoryból, kell kezdődnie, a [az Azure AD – fejlesztői útmutató](active-directory-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="ebaa4-126">If you only need to sign in work and school accounts from Azure Active Directory, you should start with our [Azure AD developer's guide](active-directory-developers-guide.md).</span></span>  <span data-ttu-id="ebaa4-127">A v2.0-végpontra a explicit módon be kell jelentkeznie személyes Microsoft-fiókok fejlesztőknek szól.</span><span class="sxs-lookup"><span data-stu-id="ebaa4-127">The v2.0 endpoint is intended for use by developers who explicitly need to sign in Microsoft personal accounts.</span></span>


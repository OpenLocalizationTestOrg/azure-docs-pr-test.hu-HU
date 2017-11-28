---
title: "az Active Directory v2.0-végponttól aaaAzure |} Microsoft Docs"
description: "Egy bevezető toobuilding-alkalmazások a Microsoft Account, mind az Azure Active Directory-bejelentkezés."
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
ms.openlocfilehash: ae5946b02c50ae5a6137dc1decae66c96647e875
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-microsoft-account--azure-ad-users-in-a-single-app"></a><span data-ttu-id="5cf98-103">Bejelentkezés Microsoft-Account & egyetlen alkalmazást az Azure AD-felhasználók</span><span class="sxs-lookup"><span data-stu-id="5cf98-103">Sign-in Microsoft Account & Azure AD users in a single app</span></span>
<span data-ttu-id="5cf98-104">A hello részhez, az alkalmazás fejlesztőjének számára toosupport személyes Microsoft-fiókot is, és az Azure Active Directory munkahelyi fiókok volt szükséges toointegrate két külön rendszerrel.</span><span class="sxs-lookup"><span data-stu-id="5cf98-104">In hello past, an app developer who wanted toosupport both personal Microsoft accounts and work accounts from Azure Active Directory was required toointegrate with two separate systems.</span></span>  <span data-ttu-id="5cf98-105">Hello **az Azure AD v2.0-végponttól** bevezet egy új hitelesítési API-verzió, amely lehetővé teszi mindkét típusú fiókok használatával egy egyszerű integráció toosign.</span><span class="sxs-lookup"><span data-stu-id="5cf98-105">hello **Azure AD v2.0 endpoint** introduces a new authentication API version that enables you toosign in both types of accounts using one simple integration.</span></span>  <span data-ttu-id="5cf98-106">Hello v2.0-végponttól használó alkalmazások is felhasználhat REST API-k a hello [Microsoft Graph](https://graph.microsoft.io) bármely típusú fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="5cf98-106">Apps that use hello v2.0 endpoint can also consume REST APIs from hello [Microsoft Graph](https://graph.microsoft.io) using either type of account.</span></span>

## <a name="getting-started"></a><span data-ttu-id="5cf98-107">Első lépések</span><span class="sxs-lookup"><span data-stu-id="5cf98-107">Getting Started</span></span>
<span data-ttu-id="5cf98-108">Előnyben részesített platformját a következő lista toobuild hello olyan alkalmazást válasszon a nyílt forráskódú szalagtárak & keretrendszerek használatával.</span><span class="sxs-lookup"><span data-stu-id="5cf98-108">Choose your favorite platform from hello following list toobuild an app using our open source libraries & frameworks.</span></span>  <span data-ttu-id="5cf98-109">Azt is megteheti használja az OAuth 2.0-s & OpenID Connect protokoll dokumentációja toosend & protokoll üzeneteket fogadni közvetlenül egy hitelesítési tár használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="5cf98-109">Alternatively, you can use our OAuth 2.0 & OpenID Connect protocol documentation toosend & receive protocol messages directly without using an auth library.</span></span>

<br />

[!INCLUDE [active-directory-v2-quickstart-table](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="whats-new"></a><span data-ttu-id="5cf98-110">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="5cf98-110">What's New</span></span>
<span data-ttu-id="5cf98-111">Itt hello információk hasznosak bizonyulnak a Mi & Mi nem lehetséges hello v2.0-végponttal ismertetése.</span><span class="sxs-lookup"><span data-stu-id="5cf98-111">hello information here will be useful in understanding what is & what isn't possible with hello v2.0 endpoint.</span></span>

* <span data-ttu-id="5cf98-112">További tudnivalók: hello [típusú alkalmazásokat hozhat létre hello v2.0-végponttal](active-directory-v2-flows.md).</span><span class="sxs-lookup"><span data-stu-id="5cf98-112">Learn about hello [types of apps you can build with hello v2.0 endpoint](active-directory-v2-flows.md).</span></span>
* <span data-ttu-id="5cf98-113">Hello megértéséhez [korlátozások, korlátozások és megkötések](active-directory-v2-limitations.md) hello v2.0-végponttal.</span><span class="sxs-lookup"><span data-stu-id="5cf98-113">Understand hello [limitations, restrictions, and constraints](active-directory-v2-limitations.md) with hello v2.0 endpoint.</span></span>
* <span data-ttu-id="5cf98-114">Ez az áttekintő videó hello v2.0-végpontra a kivétel:</span><span class="sxs-lookup"><span data-stu-id="5cf98-114">Check out this overview video for hello v2.0 endpoint:</span></span>

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="reference"></a><span data-ttu-id="5cf98-115">Referencia</span><span class="sxs-lookup"><span data-stu-id="5cf98-115">Reference</span></span>
<span data-ttu-id="5cf98-116">Ezek a hivatkozások akkor lehet hasznos, felderítését lehetővé tevő hello platform részletesen:</span><span class="sxs-lookup"><span data-stu-id="5cf98-116">These links will be useful for exploring hello platform in depth:</span></span>

* [<span data-ttu-id="5cf98-117">v2.0 protokoll referenciái</span><span class="sxs-lookup"><span data-stu-id="5cf98-117">v2.0 Protocol Reference</span></span>](active-directory-v2-protocols.md)
* [<span data-ttu-id="5cf98-118">v2.0 jogkivonat referenciái</span><span class="sxs-lookup"><span data-stu-id="5cf98-118">v2.0 Token Reference</span></span>](active-directory-v2-tokens.md)
* [<span data-ttu-id="5cf98-119">v2.0 kódtár – dokumentáció</span><span class="sxs-lookup"><span data-stu-id="5cf98-119">v2.0 Library Reference</span></span>](active-directory-v2-libraries.md)
* [<span data-ttu-id="5cf98-120">Hatókörök és hello v2.0-végponttól jóváhagyása</span><span class="sxs-lookup"><span data-stu-id="5cf98-120">Scopes and Consent in hello v2.0 endpoint</span></span>](active-directory-v2-scopes.md)
* [<span data-ttu-id="5cf98-121">a Microsoft Graph hello</span><span class="sxs-lookup"><span data-stu-id="5cf98-121">hello Microsoft Graph</span></span>](https://graph.microsoft.io)

## <a name="help--support"></a><span data-ttu-id="5cf98-122">Súgó és támogatás</span><span class="sxs-lookup"><span data-stu-id="5cf98-122">Help & Support</span></span>
<span data-ttu-id="5cf98-123">Ezek a hello legjobb helyen tooget segítségre fejlesztése az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5cf98-123">These are hello best places tooget help with developing on Azure Active Directory.</span></span>

* [<span data-ttu-id="5cf98-124">A Stack Overflow `azure-active-directory` és `adal` címkéi</span><span class="sxs-lookup"><span data-stu-id="5cf98-124">Stack Overflow's `azure-active-directory` and `adal` tags</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory+or+adal)
* [<span data-ttu-id="5cf98-125">Visszajelzés az Azure Active Directoryról</span><span class="sxs-lookup"><span data-stu-id="5cf98-125">Feedback on Azure Active Directory</span></span>](https://feedback.azure.com/forums/169401-azure-active-directory/category/164757-developer-experiences)


> [!NOTE]
> <span data-ttu-id="5cf98-126">Ha csak a munkahelyi és iskolai fiókok az Azure Active Directory toosign, kell kezdődnie, a [az Azure AD – fejlesztői útmutató](active-directory-developers-guide.md).</span><span class="sxs-lookup"><span data-stu-id="5cf98-126">If you only need toosign in work and school accounts from Azure Active Directory, you should start with our [Azure AD developer's guide](active-directory-developers-guide.md).</span></span>  <span data-ttu-id="5cf98-127">hello v2.0-végpontra a explicit módon kell toosign személyes Microsoft-fiókok a fejlesztőknek szól.</span><span class="sxs-lookup"><span data-stu-id="5cf98-127">hello v2.0 endpoint is intended for use by developers who explicitly need toosign in Microsoft personal accounts.</span></span>


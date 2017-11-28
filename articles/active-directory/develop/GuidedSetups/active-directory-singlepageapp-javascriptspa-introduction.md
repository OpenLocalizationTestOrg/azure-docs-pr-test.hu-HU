---
title: "Az Azure AD v2 JS SPA interaktív telepítés – bevezetés |} Microsoft Docs"
description: "Hogyan JavaScript SPA-alkalmazások Azure Active Directory-v2 végpontja hozzáférési jogkivonatok igénylő API meghívása"
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
ms.date: 06/01/2017
ms.author: andret
ms.openlocfilehash: 3d195d0d67f8f82c9450ffd93767917698addee3
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a><span data-ttu-id="511cc-103">A Microsoft Graph API meghívása a JavaScript egyetlen oldal alkalmazásból (SPA)</span><span class="sxs-lookup"><span data-stu-id="511cc-103">Call the Microsoft Graph API from a JavaScript Single Page Application (SPA)</span></span>

<span data-ttu-id="511cc-104">Ez az útmutató ismerteti, hogyan bejelentkezhet az egy JavaScript egyetlen oldal alkalmazás (SPA) a személyes, munkahelyi és iskolai fiókok szereznie egy hozzáférési jogkivonatot, és hívja meg a Microsoft Graph API vagy egyéb szükséges hozzáférési jogkivonatok az Azure Active Directory v2 végpont az API-k.</span><span class="sxs-lookup"><span data-stu-id="511cc-104">This guide demonstrates how a JavaScript Single Page Application (SPA) can sign in personal, work and school accounts, get an access token, and call the Microsoft Graph API or other APIs that require access tokens from the Azure Active Directory v2 endpoint.</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="511cc-105">Ez az útmutató működése</span><span class="sxs-lookup"><span data-stu-id="511cc-105">How this guide works</span></span>

![Ez az útmutató által generált mintaalkalmazás működése](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="511cc-107">További információ</span><span class="sxs-lookup"><span data-stu-id="511cc-107">More Information</span></span>

<span data-ttu-id="511cc-108">Ez az útmutató által létrehozott mintaalkalmazás lehetővé teszi, hogy a JavaScript SPA lekérdezni a Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="511cc-108">The sample application created by this guide enables a JavaScript SPA to query the Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="511cc-109">Ebben a forgatókönyvben után a felhasználó bejelentkezik olyan hozzáférési jogkivonatot kérése és hitelesítési fejlécéhez via HTTP-kérelmek hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="511cc-109">For this scenario, after a user signs-in, an access token is requested and added to HTTP requests via the authorization header.</span></span> <span data-ttu-id="511cc-110">Token beszerzése és -megújítás kezelése a Microsoft hitelesítési könyvtár (MSAL).</span><span class="sxs-lookup"><span data-stu-id="511cc-110">Token acquisition and renewal are handled by the Microsoft Authentication Library (MSAL).</span></span>

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a><span data-ttu-id="511cc-111">Szalagtárak</span><span class="sxs-lookup"><span data-stu-id="511cc-111">Libraries</span></span>

<span data-ttu-id="511cc-112">Ez az útmutató használja a következő könyvtárban:</span><span class="sxs-lookup"><span data-stu-id="511cc-112">This guide uses the following library:</span></span>

|<span data-ttu-id="511cc-113">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="511cc-113">Library</span></span>|<span data-ttu-id="511cc-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="511cc-114">Description</span></span>|
|---|---|
|[<span data-ttu-id="511cc-115">msal.js</span><span class="sxs-lookup"><span data-stu-id="511cc-115">msal.js</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js)|<span data-ttu-id="511cc-116">A JavaScript Preview Microsoft hitelesítési kódtár</span><span class="sxs-lookup"><span data-stu-id="511cc-116">Microsoft Authentication Library for JavaScript Preview</span></span>|

> [!NOTE]
> <span data-ttu-id="511cc-117">*msal.js* célok a *Azure Active Directory v2 végpont* -lehetővé teszi a személyes, iskolai és munkahelyi fiókokat kell jelentkezzen be, és beszerezni a jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="511cc-117">*msal.js* targets the *Azure Active Directory v2 endpoint* - which enables personal, school and work accounts to sign in and acquire tokens.</span></span> <span data-ttu-id="511cc-118">A *Azure Active Directory v2 végpont* rendelkezik [bizonyos korlátozások](..\active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="511cc-118">The *Azure Active Directory v2 endpoint* has [some limitations](..\active-directory-v2-limitations.md).</span></span> <span data-ttu-id="511cc-119">Ha érdekli, csak a munkahelyi és iskolai fiókok, *adal.js* és a *V1 végpont*.</span><span class="sxs-lookup"><span data-stu-id="511cc-119">If you are interested only in school and work accounts, use *adal.js* and the *V1 endpoint*.</span></span> <span data-ttu-id="511cc-120">Olvassa el a v1 és v2 végpontok közötti különbségek megértése a [v1-v2 összehasonlító](..\active-directory-v2-compare.md).</span><span class="sxs-lookup"><span data-stu-id="511cc-120">To understand differences between the v1 and v2 endpoints read the [v1-v2 comparison](..\active-directory-v2-compare.md).</span></span>

<!--end-collapse-->

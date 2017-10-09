---
title: "aaaAzure AD v2 JS SPA interaktív telepítés – bevezetés |} Microsoft Docs"
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
ms.openlocfilehash: 7e4250ccca837a17489a99603da167009cc2e3d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="call-hello-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a><span data-ttu-id="95aad-103">Hívás hello Microsoft Graph API a egy JavaScript egyetlen lap alkalmazás (SPA)</span><span class="sxs-lookup"><span data-stu-id="95aad-103">Call hello Microsoft Graph API from a JavaScript Single Page Application (SPA)</span></span>

<span data-ttu-id="95aad-104">Ez az útmutató ismerteti, hogyan bejelentkezhet az egy JavaScript egyetlen oldal alkalmazás (SPA) a személyes, munkahelyi és iskolai fiókok szereznie egy hozzáférési jogkivonatot, és hívja meg Microsoft Graph API hello vagy más API-k, a hozzáférési jogkivonatok igénylő hello Azure Active Directory v2 végpont.</span><span class="sxs-lookup"><span data-stu-id="95aad-104">This guide demonstrates how a JavaScript Single Page Application (SPA) can sign in personal, work and school accounts, get an access token, and call hello Microsoft Graph API or other APIs that require access tokens from hello Azure Active Directory v2 endpoint.</span></span>

### <a name="how-this-guide-works"></a><span data-ttu-id="95aad-105">Ez az útmutató működése</span><span class="sxs-lookup"><span data-stu-id="95aad-105">How this guide works</span></span>

![Ez az útmutató által generált hello mintaalkalmazás működése](media/active-directory-singlepageapp-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a><span data-ttu-id="95aad-107">További információ</span><span class="sxs-lookup"><span data-stu-id="95aad-107">More Information</span></span>

<span data-ttu-id="95aad-108">Ez az útmutató által létrehozott hello mintaalkalmazás lehetővé teszi, hogy a JavaScript SPA tooquery hello Microsoft Graph API vagy egy webes API, amely az Azure Active Directory v2 végpont jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="95aad-108">hello sample application created by this guide enables a JavaScript SPA tooquery hello Microsoft Graph API or a Web API that accepts tokens from Azure Active Directory v2 endpoint.</span></span> <span data-ttu-id="95aad-109">Ebben a forgatókönyvben követően a felhasználó bejelentkezik, olyan hozzáférési jogkivonatot kért és hozzáadott tooHTTP kérelmek keresztül hello authorization fejlécet.</span><span class="sxs-lookup"><span data-stu-id="95aad-109">For this scenario, after a user signs-in, an access token is requested and added tooHTTP requests via hello authorization header.</span></span> <span data-ttu-id="95aad-110">Token beszerzése és -megújítás hello Microsoft hitelesítési könyvtár (MSAL) kezeli.</span><span class="sxs-lookup"><span data-stu-id="95aad-110">Token acquisition and renewal are handled by hello Microsoft Authentication Library (MSAL).</span></span>

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a><span data-ttu-id="95aad-111">Szalagtárak</span><span class="sxs-lookup"><span data-stu-id="95aad-111">Libraries</span></span>

<span data-ttu-id="95aad-112">Ez az útmutató a következő könyvtár hello használja:</span><span class="sxs-lookup"><span data-stu-id="95aad-112">This guide uses hello following library:</span></span>

|<span data-ttu-id="95aad-113">Részletes ismertetés</span><span class="sxs-lookup"><span data-stu-id="95aad-113">Library</span></span>|<span data-ttu-id="95aad-114">Leírás</span><span class="sxs-lookup"><span data-stu-id="95aad-114">Description</span></span>|
|---|---|
|[<span data-ttu-id="95aad-115">msal.js</span><span class="sxs-lookup"><span data-stu-id="95aad-115">msal.js</span></span>](https://github.com/AzureAD/microsoft-authentication-library-for-js)|<span data-ttu-id="95aad-116">A JavaScript Preview Microsoft hitelesítési kódtár</span><span class="sxs-lookup"><span data-stu-id="95aad-116">Microsoft Authentication Library for JavaScript Preview</span></span>|

> [!NOTE]
> <span data-ttu-id="95aad-117">*msal.js* célok hello *Azure Active Directory v2 végpont* – amely lehetővé teszi, hogy a személyes, iskolai és munkahelyi fiókokat toosign, és beszerezni a jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="95aad-117">*msal.js* targets hello *Azure Active Directory v2 endpoint* - which enables personal, school and work accounts toosign in and acquire tokens.</span></span> <span data-ttu-id="95aad-118">Hello *Azure Active Directory v2 végpont* rendelkezik [bizonyos korlátozások](..\active-directory-v2-limitations.md).</span><span class="sxs-lookup"><span data-stu-id="95aad-118">hello *Azure Active Directory v2 endpoint* has [some limitations](..\active-directory-v2-limitations.md).</span></span> <span data-ttu-id="95aad-119">Ha érdekli, csak a munkahelyi és iskolai fiókok, *adal.js* és hello *V1 végpont*.</span><span class="sxs-lookup"><span data-stu-id="95aad-119">If you are interested only in school and work accounts, use *adal.js* and hello *V1 endpoint*.</span></span> <span data-ttu-id="95aad-120">hello v1 és v2 végpontok közötti toounderstand különbségek olvasási hello [v1-v2 összehasonlító](..\active-directory-v2-compare.md).</span><span class="sxs-lookup"><span data-stu-id="95aad-120">toounderstand differences between hello v1 and v2 endpoints read hello [v1-v2 comparison](..\active-directory-v2-compare.md).</span></span>

<!--end-collapse-->

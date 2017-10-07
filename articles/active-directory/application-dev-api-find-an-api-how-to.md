---
title: "egy adott API-t egy egyéni által fejlesztett alkalmazás szükséges aaaHow toofind |} Microsoft Docs"
description: "Hogyan tooconfigure hello engedélyek tooaccess adott API-t az egyéni szükséges fejlesztése az Azure AD-alkalmazást"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 7331129204d8b34b4ef9671749bd702f893768ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-a-specific-api-needed-for-a-custom-developed-application"></a><span data-ttu-id="6d4a3-103">Hogyan toofind egy adott API szükséges egy egyéni által fejlesztett alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6d4a3-103">How toofind a specific API needed for a custom-developed application</span></span>

<span data-ttu-id="6d4a3-104">Hozzáférés tooAPIs megkövetelése a hozzáférési hatókörök és a szerepkörök konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="6d4a3-104">Access tooAPIs require configuration of access scopes and roles.</span></span> <span data-ttu-id="6d4a3-105">Ha azt szeretné, tooexpose az erőforrás alkalmazás webes API-k tooclient alkalmazások, szükség van tooconfigure hozzáférési hatókörök és szerepkörök hello API.</span><span class="sxs-lookup"><span data-stu-id="6d4a3-105">If you want tooexpose your resource application web APIs tooclient applications, you need tooconfigure access scopes and roles for hello API.</span></span> <span data-ttu-id="6d4a3-106">Ha azt szeretné, hogy egy ügyfél alkalmazás tooaccess webes API-k, kell tooconfigure engedélyek tooaccess hello API hello app regisztrációhoz.</span><span class="sxs-lookup"><span data-stu-id="6d4a3-106">If you want a client application tooaccess a web API, you need tooconfigure permissions tooaccess hello API in hello app registration.</span></span>

## <a name="configuring-a-resource-application-tooexpose-web-apis"></a><span data-ttu-id="6d4a3-107">Egy erőforrás tooexpose webes API-k konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d4a3-107">Configuring a resource application tooexpose web APIs</span></span>

<span data-ttu-id="6d4a3-108">Amikor teszi ki a webes API-t hello API hello jeleníthető meg **API kiválasztása** engedélyek tooan app regisztrációs hozzáadásakor listában.</span><span class="sxs-lookup"><span data-stu-id="6d4a3-108">When you expose your web API, hello API be displayed in hello **Select an API** list when adding permissions tooan app registration.</span></span> <span data-ttu-id="6d4a3-109">tooadd hozzáférési hatókörök, kövesse az ismertetett hello lépéseket [tooyour erőforrás alkalmazás hozzáadása hozzáférési hatóköröket](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span><span class="sxs-lookup"><span data-stu-id="6d4a3-109">tooadd access scopes, follow hello steps outlined in [adding access scopes tooyour resource application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span></span>

## <a name="configuring-a-client-application-tooaccess-web-apis"></a><span data-ttu-id="6d4a3-110">Egy ügyfél tooaccess webes API-k konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d4a3-110">Configuring a client application tooaccess web APIs</span></span>

<span data-ttu-id="6d4a3-111">Engedélyek tooyour app regisztrációs hozzáadásakor is **API-hozzáférés hozzáadása** tooexposed webes API-k.</span><span class="sxs-lookup"><span data-stu-id="6d4a3-111">When you add permissions tooyour app registration, you can **add API access** tooexposed web APIs.</span></span> <span data-ttu-id="6d4a3-112">tooaccess webes API-khoz, hello című rész lépéseit követve [adja hozzá a hitelesítő adatokat vagy engedélyek tooaccess webes API-khoz](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span><span class="sxs-lookup"><span data-stu-id="6d4a3-112">tooaccess web APIs, follow hello steps outlined in [add credentials or permissions tooaccess web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d4a3-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d4a3-113">Next steps</span></span>

-   [<span data-ttu-id="6d4a3-114">Alkalmazások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6d4a3-114">Integrating applications with Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [<span data-ttu-id="6d4a3-115">Hello Azure Active Directory alkalmazásjegyzékének megismerése</span><span class="sxs-lookup"><span data-stu-id="6d4a3-115">Understanding hello Azure Active Directory application manifest</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)



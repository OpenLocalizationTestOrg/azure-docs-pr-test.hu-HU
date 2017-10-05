---
title: "Hol találhatók a szükséges egyéni által fejlesztett alkalmazás adott API |} Microsoft Docs"
description: "Hozzá kell férnie egy adott API-nak az egyéni engedélyek fejlesztése az Azure AD-alkalmazást"
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
ms.openlocfilehash: e0c07fd030339d025894520500d2cd948d31af45
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a><span data-ttu-id="f54b7-103">Egy adott API szükséges egyéni által fejlesztett alkalmazás megkeresése</span><span class="sxs-lookup"><span data-stu-id="f54b7-103">How to find a specific API needed for a custom-developed application</span></span>

<span data-ttu-id="f54b7-104">API-k eléréséhez szükséges hozzáférési hatókörök és a szerepkörök konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="f54b7-104">Access to APIs require configuration of access scopes and roles.</span></span> <span data-ttu-id="f54b7-105">Ha az erőforrás alkalmazás webes API-k ügyfélalkalmazások számára elérhetővé tenni kívánt, hozzáférési hatókörök és szerepkörök konfigurálása az API-t szeretné.</span><span class="sxs-lookup"><span data-stu-id="f54b7-105">If you want to expose your resource application web APIs to client applications, you need to configure access scopes and roles for the API.</span></span> <span data-ttu-id="f54b7-106">Ha azt szeretné, hogy egy webes API-k elérésére ügyfélalkalmazást, kell konfigurálni az engedélyeket, hogy az API-alkalmazás regisztrációja.</span><span class="sxs-lookup"><span data-stu-id="f54b7-106">If you want a client application to access a web API, you need to configure permissions to access the API in the app registration.</span></span>

## <a name="configuring-a-resource-application-to-expose-web-apis"></a><span data-ttu-id="f54b7-107">Egy erőforrás-alkalmazások – ezek általában a webes API-k konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f54b7-107">Configuring a resource application to expose web APIs</span></span>

<span data-ttu-id="f54b7-108">Amikor teszi ki a webes API-t, jeleníthető meg az API-t a **API kiválasztása** listára, amikor engedélyeket ad hozzá egy alkalmazás regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="f54b7-108">When you expose your web API, the API be displayed in the **Select an API** list when adding permissions to an app registration.</span></span> <span data-ttu-id="f54b7-109">A hozzáférési hatókörök hozzáadásához kövesse a témakörben ismertetett lépéseket [hozzáférési hatókörök hozzáadása az erőforrás alkalmazás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span><span class="sxs-lookup"><span data-stu-id="f54b7-109">To add access scopes, follow the steps outlined in [adding access scopes to your resource application](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-access-scopes-to-your-resource-application).</span></span>

## <a name="configuring-a-client-application-to-access-web-apis"></a><span data-ttu-id="f54b7-110">A webes API-k elérésére ügyfélalkalmazás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f54b7-110">Configuring a client application to access web APIs</span></span>

<span data-ttu-id="f54b7-111">Amikor az alkalmazás regisztrálása az engedélyeket, lehet **API-hozzáférés hozzáadása** feladatban felfedett webes API-k számára.</span><span class="sxs-lookup"><span data-stu-id="f54b7-111">When you add permissions to your app registration, you can **add API access** to exposed web APIs.</span></span> <span data-ttu-id="f54b7-112">Webes API-k eléréséhez kövesse a témakörben ismertetett lépéseket [adja hozzá a hitelesítő adatokat vagy webes API-k hozzáférési engedélye a](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span><span class="sxs-lookup"><span data-stu-id="f54b7-112">To access web APIs, follow the steps outlined in [add credentials or permissions to access web APIs](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#to-add-credentials-or-permissions-to-access-web-apis).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f54b7-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f54b7-113">Next steps</span></span>

-   [<span data-ttu-id="f54b7-114">Alkalmazások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f54b7-114">Integrating applications with Azure Active Directory</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [<span data-ttu-id="f54b7-115">Az Azure Active Directory alkalmazásjegyzékének megismerése</span><span class="sxs-lookup"><span data-stu-id="f54b7-115">Understanding the Azure Active Directory application manifest</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)



---
title: "Probléma jelentkezik be egy egyéni által fejlesztett alkalmazás |} Microsoft Docs"
description: "Az Azure ad-val kialakítása, amelyek okozza, hogy nem fog tudni bejelentkezni egy alkalmazás általános rrors"
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
ms.openlocfilehash: b0df23e040a73d18968f547eef7347f14cc577c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-an-custom-developed-application"></a><span data-ttu-id="45fb1-103">Egy egyéni által fejlesztett alkalmazás bejelentkezés problémák</span><span class="sxs-lookup"><span data-stu-id="45fb1-103">Problems signing in to an custom-developed application</span></span>

<span data-ttu-id="45fb1-104">Nincsenek számos hibát, amely az okozza, hogy nem lehet bejelentkezni az alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="45fb1-104">There are several errors that could be causing you to not be able to sign into an app.</span></span> <span data-ttu-id="45fb1-105">A legnagyobb OK személyek esetlegesen fellépő probléma van az alkalmazások konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="45fb1-105">The biggest reason people encounter this problem is misconfigured apps.</span></span>

## <a name="errors-related-to--misconfigured-apps"></a><span data-ttu-id="45fb1-106">Helytelenül konfigurált alkalmazások kapcsolatos hibák</span><span class="sxs-lookup"><span data-stu-id="45fb1-106">Errors related to  misconfigured apps</span></span>

* <span data-ttu-id="45fb1-107">Ellenőrizze a portálon mindkét a konfiguráció megfelel az alkalmazás rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="45fb1-107">Verify both the configurations in the portal match what you have in your app.</span></span> <span data-ttu-id="45fb1-108">Pontosabban hasonlítsa össze az ügyfél/alkalmazás azonosítója, a válasz URL-címek, a titkos kulcsok/Ügyfélkulcsokat és a App ID URI.</span><span class="sxs-lookup"><span data-stu-id="45fb1-108">Specifically, compare Client/Application ID, Reply URLs, Client Secrets/Keys, and App ID URI.</span></span>

* <span data-ttu-id="45fb1-109">Hasonlítsa össze a hozzáférést a kódokat, amelyek a beállított engedélyek szükségesek a kért erőforrás a **szükséges erőforrások** fülre, és ellenőrizze, hogy Ön csak erőforrás-kérelmek konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="45fb1-109">Compare the resource you’re requesting access to in code with the configured permissions in the **Required Resources** tab to make sure you only request resources you’ve configured.</span></span>

* <span data-ttu-id="45fb1-110">Lásd: [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) hasonló hibákat és problémákat.</span><span class="sxs-lookup"><span data-stu-id="45fb1-110">See [Azure AD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory) for any similar errors or problems.</span></span>

## <a name="next-steps"></a><span data-ttu-id="45fb1-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45fb1-111">Next steps</span></span>

[<span data-ttu-id="45fb1-112">Az Azure AD-fejlesztői útmutató</span><span class="sxs-lookup"><span data-stu-id="45fb1-112">Azure AD Developer Guide</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)<br>

[<span data-ttu-id="45fb1-113">Hozzájárulás és az alkalmazások az Azure AD integrálása</span><span class="sxs-lookup"><span data-stu-id="45fb1-113">Consent and Integrating Apps to Azure AD</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications>)<br>

[<span data-ttu-id="45fb1-114">Hozzájárulás és az Azure AD v2.0 Permissioning összevont alkalmazások</span><span class="sxs-lookup"><span data-stu-id="45fb1-114">Consent and Permissioning for Azure AD v2.0 converged Apps</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="45fb1-115">Az Azure AD-StackOverflow</span><span class="sxs-lookup"><span data-stu-id="45fb1-115">Azure AD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory>)

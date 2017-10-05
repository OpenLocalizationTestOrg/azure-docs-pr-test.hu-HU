---
title: "Hogyan kell megadni az engedélyeket az egyéni által fejlesztett alkalmazás |} Microsoft Docs"
description: "Hogyan engedélyezze, hogy az egyéni által fejlesztett alkalmazás használatával, az Azure AD portálon vagy az egy URL-paramétert"
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
ms.openlocfilehash: 336b945929f80e1a566f7cf71b40fd799a98c12d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-grant-permissions-to-a-custom-developed-application"></a><span data-ttu-id="f3c8b-103">Hogyan kell megadni az engedélyeket az egyéni által fejlesztett alkalmazás</span><span class="sxs-lookup"><span data-stu-id="f3c8b-103">How to grant permissions to a custom-developed application</span></span>

<span data-ttu-id="f3c8b-104">Ha szeretné, hogy a hozzájárulás megelőző jelleggel meg az alkalmazást, vagy nem beleegyezett hiba történt az alkalmazások futnak, próbálkozzon az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-104">If you want to grant consent preemptively on your app or are running into an error that you have not consented to an app, try these steps below.</span></span>

## <a name="how-to-perform-admin-consent-for-your-application"></a><span data-ttu-id="f3c8b-105">Az alkalmazás Admin hozzájárulási végrehajtása</span><span class="sxs-lookup"><span data-stu-id="f3c8b-105">How to perform Admin Consent for your application</span></span>

<span data-ttu-id="f3c8b-106">Azt a hatását, hogy a szervezete összes felhasználója számára az alkalmazásba hozzájárulás megadása.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-106">This has the effect of granting consent to the application for all users in your organization.</span></span>

1. <span data-ttu-id="f3c8b-107">Keresse meg a **App regisztrációk** panelen, egy **globális rendszergazda**, majd válassza ki az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-107">Navigate to the **App Registrations** blade as a **global administrator**, then select the app.</span></span>

2. <span data-ttu-id="f3c8b-108">Válassza ki **szükséges engedélyek**, majd végül nyomja le a **engedélyt adjon** gomb a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-108">Select **Required Permissions**, and finally hit the **Grant Permissions** button at the top of the blade.</span></span>

<span data-ttu-id="f3c8b-109">Alternatív megoldásként, hogyan hozhat létre a kérelmek *login.microsoftonline.com* a az alkalmazás configs és a hozzáfűzendő *& Rákérdezés = admin\_hozzájárulás*.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-109">Alternatively, you can construct a request to *login.microsoftonline.com* with your app configs and append on *&prompt=admin\_consent*.</span></span> <span data-ttu-id="f3c8b-110">Után rendszergazdai hitelesítő adataival bejelentkezni, az alkalmazás rendelkezik az összes felhasználó jóváhagyását.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-110">After signing in with admin credentials, the app has been granted consent for all users.</span></span>

## <a name="how-to-force-user-consent-for-your-application"></a><span data-ttu-id="f3c8b-111">Az alkalmazás a felhasználói hozzájárulás kényszerítése</span><span class="sxs-lookup"><span data-stu-id="f3c8b-111">How to force User Consent for your application</span></span>

* <span data-ttu-id="f3c8b-112">Hitelesítési kérelmek alakzatot hozzáfűzése *& Rákérdezés hozzájárulási =* igénylő felhasználók számára, hogy hozzájárulás minden alkalommal, amikor a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="f3c8b-112">Append onto auth requests *&prompt=consent* which require end users to consent each time they authenticate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f3c8b-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3c8b-113">Next steps</span></span>

[<span data-ttu-id="f3c8b-114">Hozzájárulás és AzureAD alkalmazások integrálása</span><span class="sxs-lookup"><span data-stu-id="f3c8b-114">Consent and Integrating Apps to AzureAD</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[<span data-ttu-id="f3c8b-115">Hozzájárulás és Permissioning AzureAD v2.0 az összevont alkalmazások</span><span class="sxs-lookup"><span data-stu-id="f3c8b-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="f3c8b-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="f3c8b-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

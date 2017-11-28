---
title: "aaaHow toogrant engedélyek tooa egyéni által fejlesztett alkalmazás |} Microsoft Docs"
description: "Hogyan toogrant engedélyek tooyour egyéni által fejlesztett alkalmazás használatával hello a Azure AD portálon vagy az egy URL-paramétert"
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
ms.openlocfilehash: e43a105fff60fbf912bdf4f60260f86ee289328d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogrant-permissions-tooa-custom-developed-application"></a><span data-ttu-id="ca003-103">Hogyan toogrant engedélyek tooa egyéni által fejlesztett alkalmazás</span><span class="sxs-lookup"><span data-stu-id="ca003-103">How toogrant permissions tooa custom-developed application</span></span>

<span data-ttu-id="ca003-104">Ha a kívánt toogrant hozzájárulási megelőző jelleggel az alkalmazás vagy futnak hiba történt, hogy tooan app nem beleegyezett, próbálja meg az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="ca003-104">If you want toogrant consent preemptively on your app or are running into an error that you have not consented tooan app, try these steps below.</span></span>

## <a name="how-tooperform-admin-consent-for-your-application"></a><span data-ttu-id="ca003-105">Hogyan tooperform Admin hozzájárulási az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="ca003-105">How tooperform Admin Consent for your application</span></span>

<span data-ttu-id="ca003-106">Ennek hatása hello biztosítása hozzájárulási toohello alkalmazás a szervezete összes felhasználója számára.</span><span class="sxs-lookup"><span data-stu-id="ca003-106">This has hello effect of granting consent toohello application for all users in your organization.</span></span>

1. <span data-ttu-id="ca003-107">Keresse meg a toohello **App regisztrációk** panelen, egy **globális rendszergazda**, majd válassza hello app.</span><span class="sxs-lookup"><span data-stu-id="ca003-107">Navigate toohello **App Registrations** blade as a **global administrator**, then select hello app.</span></span>

2. <span data-ttu-id="ca003-108">Válassza ki **szükséges engedélyek**, és végül kattintson a hello **engedélyt adjon** hello panel felső hello gombra.</span><span class="sxs-lookup"><span data-stu-id="ca003-108">Select **Required Permissions**, and finally hit hello **Grant Permissions** button at hello top of hello blade.</span></span>

<span data-ttu-id="ca003-109">Alternatív megoldásként, hogyan hozhat létre a kérelem túl*login.microsoftonline.com* a az alkalmazás configs és a hozzáfűzendő *& Rákérdezés = admin\_hozzájárulás*.</span><span class="sxs-lookup"><span data-stu-id="ca003-109">Alternatively, you can construct a request too*login.microsoftonline.com* with your app configs and append on *&prompt=admin\_consent*.</span></span> <span data-ttu-id="ca003-110">Után rendszergazdai hitelesítő adataival bejelentkezni, hello alkalmazás rendelkezik az összes felhasználó jóváhagyását.</span><span class="sxs-lookup"><span data-stu-id="ca003-110">After signing in with admin credentials, hello app has been granted consent for all users.</span></span>

## <a name="how-tooforce-user-consent-for-your-application"></a><span data-ttu-id="ca003-111">Hogyan tooforce felhasználói hozzájárulás az alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="ca003-111">How tooforce User Consent for your application</span></span>

* <span data-ttu-id="ca003-112">Hitelesítési kérelmek alakzatot hozzáfűzése *& Rákérdezés hozzájárulási =* minden alkalommal, amikor a hitelesítéshez a végfelhasználók tooconsent igénylő.</span><span class="sxs-lookup"><span data-stu-id="ca003-112">Append onto auth requests *&prompt=consent* which require end users tooconsent each time they authenticate.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ca003-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ca003-113">Next steps</span></span>

[<span data-ttu-id="ca003-114">Hozzájárulás és az alkalmazások integrálása tooAzureAD</span><span class="sxs-lookup"><span data-stu-id="ca003-114">Consent and Integrating Apps tooAzureAD</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[<span data-ttu-id="ca003-115">Hozzájárulás és Permissioning AzureAD v2.0 az összevont alkalmazások</span><span class="sxs-lookup"><span data-stu-id="ca003-115">Consent and Permissioning for AzureAD v2.0 converged Apps</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[<span data-ttu-id="ca003-116">AzureAD StackOverflow</span><span class="sxs-lookup"><span data-stu-id="ca003-116">AzureAD StackOverflow</span></span>](http://stackoverflow.com/questions/tagged/azure-active-directory)

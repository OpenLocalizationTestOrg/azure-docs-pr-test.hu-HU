---
title: "aaaHow toochange hello a jogkivonatok élettartama egyéni által fejlesztett alkalmazás alapértelmezés szerint |} Microsoft Docs"
description: "Hogyan tooupdate a jogkivonatok élettartama házirendek az Azure ad-val kifejlesztett alkalmazás"
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
ms.openlocfilehash: 6e1aa1f2a7c33c1f55c5fb619c618ad43cd96273
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toochange-hello-token-lifetime-defaults-for-a-custom-developed-application"></a><span data-ttu-id="5d69c-103">Hogyan egyéni által fejlesztett alkalmazás alapértelmezés szerint toochange hello a jogkivonatok élettartama</span><span class="sxs-lookup"><span data-stu-id="5d69c-103">How toochange hello token lifetime defaults for a custom-developed application</span></span>

<span data-ttu-id="5d69c-104">Prémium szintű Azure AD lehetővé teszi az alkalmazásfejlesztők és a bérlői rendszergazdák tooconfigure hello élettartama nem bizalmas ügyfelek számára kiállított jogkivonatokat.</span><span class="sxs-lookup"><span data-stu-id="5d69c-104">Azure AD Premium allows app developers and tenant admins tooconfigure hello lifetime of tokens issued for non-confidential clients.</span></span> <span data-ttu-id="5d69c-105">A jogkivonatok élettartama házirend-beállításokat a bérlői szintű vagy hello erőforrásokhoz férnek hozzá.</span><span class="sxs-lookup"><span data-stu-id="5d69c-105">Token lifetime policies are set on a tenant-wide basis or hello resources being accessed.</span></span>

 * <span data-ttu-id="5d69c-106">a jogkivonatok élettartama házirend tooset, kell toodownload hello [Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="5d69c-106">tooset a token lifetime policy, you need toodownload hello [Azure AD PowerShell Module](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>

 * <span data-ttu-id="5d69c-107">Futtassa a hello **Connect-AzureAD-megerősítése** parancs.</span><span class="sxs-lookup"><span data-stu-id="5d69c-107">Run hello **Connect-AzureAD -Confirm** command.</span></span>

 * <span data-ttu-id="5d69c-108">Íme egy példa szabályzattal, amely beállítja a hello maximális életkora egyetlen tényező frissítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="5d69c-108">Here’s an example policy that sets hello max age single factor refresh token.</span></span> <span data-ttu-id="5d69c-109">Hello-szabályzat létrehozása:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span><span class="sxs-lookup"><span data-stu-id="5d69c-109">Create hello policy: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span></span>

 * <span data-ttu-id="5d69c-110">Kivételt hello [konfigurálása a jogkivonatok élettartama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) toolearn hogyan dokumentum toocreate más egyéni.</span><span class="sxs-lookup"><span data-stu-id="5d69c-110">Checkout hello [Configuring token lifetime](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)   document toolearn how toocreate other custom.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5d69c-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5d69c-111">Next steps</span></span>
[<span data-ttu-id="5d69c-112">A jogkivonatok élettartama konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5d69c-112">Configuring Token Lifetime</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[<span data-ttu-id="5d69c-113">Az Azure AD-jogkivonat-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="5d69c-113">Azure AD Token Reference</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)


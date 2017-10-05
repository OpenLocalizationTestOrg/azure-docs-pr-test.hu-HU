---
title: "A jogkivonatok élettartama módosítása egy egyéni által fejlesztett alkalmazás alapértelmezés szerint |} Microsoft Docs"
description: "A jogkivonatok élettartama házirendek az Azure ad-val kifejlesztett alkalmazás frissítése"
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
ms.openlocfilehash: a28eacd820ed28a6470992ce86b060e886c00bcb
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a><span data-ttu-id="17e17-103">A jogkivonatok élettartama módosítása egy egyéni által fejlesztett alkalmazás alapértelmezés szerint használt érték</span><span class="sxs-lookup"><span data-stu-id="17e17-103">How to change the token lifetime defaults for a custom-developed application</span></span>

<span data-ttu-id="17e17-104">Prémium szintű Azure AD lehetővé teszi az alkalmazásfejlesztők és a bérlői rendszergazda nem bizalmas ügyfelek számára kiállított jogkivonatokat élettartama konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="17e17-104">Azure AD Premium allows app developers and tenant admins to configure the lifetime of tokens issued for non-confidential clients.</span></span> <span data-ttu-id="17e17-105">A jogkivonatok élettartama házirend-beállításokat a bérlői szintű vagy az éppen elért erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="17e17-105">Token lifetime policies are set on a tenant-wide basis or the resources being accessed.</span></span>

 * <span data-ttu-id="17e17-106">A jogkivonatok élettartama szabály beállításához le kell töltenie a [Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureADPreview).</span><span class="sxs-lookup"><span data-stu-id="17e17-106">To set a token lifetime policy, you need to download the [Azure AD PowerShell Module](https://www.powershellgallery.com/packages/AzureADPreview).</span></span>

 * <span data-ttu-id="17e17-107">Futtassa a **Connect-AzureAD-megerősítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="17e17-107">Run the **Connect-AzureAD -Confirm** command.</span></span>

 * <span data-ttu-id="17e17-108">Íme egy példa szabályzattal, amely beállítja azt a maximális életkora egyetlen tényező frissítési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="17e17-108">Here’s an example policy that sets the max age single factor refresh token.</span></span> <span data-ttu-id="17e17-109">A szabályzat létrehozása:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span><span class="sxs-lookup"><span data-stu-id="17e17-109">Create the policy: ```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```</span></span>

 * <span data-ttu-id="17e17-110">Kivételt a [konfigurálása a jogkivonatok élettartama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) dokumentum áttekintésével megismerheti, hogyan más egyéni létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="17e17-110">Checkout the [Configuring token lifetime](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)   document to learn how to create other custom.</span></span>

## <a name="next-steps"></a><span data-ttu-id="17e17-111">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="17e17-111">Next steps</span></span>
[<span data-ttu-id="17e17-112">A jogkivonatok élettartama konfigurálása</span><span class="sxs-lookup"><span data-stu-id="17e17-112">Configuring Token Lifetime</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[<span data-ttu-id="17e17-113">Az Azure AD-jogkivonat-hivatkozás</span><span class="sxs-lookup"><span data-stu-id="17e17-113">Azure AD Token Reference</span></span>](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)


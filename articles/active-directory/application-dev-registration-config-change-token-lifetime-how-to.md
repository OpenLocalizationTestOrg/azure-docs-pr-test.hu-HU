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
# <a name="how-toochange-hello-token-lifetime-defaults-for-a-custom-developed-application"></a>Hogyan egyéni által fejlesztett alkalmazás alapértelmezés szerint toochange hello a jogkivonatok élettartama

Prémium szintű Azure AD lehetővé teszi az alkalmazásfejlesztők és a bérlői rendszergazdák tooconfigure hello élettartama nem bizalmas ügyfelek számára kiállított jogkivonatokat. A jogkivonatok élettartama házirend-beállításokat a bérlői szintű vagy hello erőforrásokhoz férnek hozzá.

 * a jogkivonatok élettartama házirend tooset, kell toodownload hello [Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureADPreview).

 * Futtassa a hello **Connect-AzureAD-megerősítése** parancs.

 * Íme egy példa szabályzattal, amely beállítja a hello maximális életkora egyetlen tényező frissítési jogkivonat. Hello-szabályzat létrehozása:```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

 * Kivételt hello [konfigurálása a jogkivonatok élettartama](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes) toolearn hogyan dokumentum toocreate más egyéni.

## <a name="next-steps"></a>Következő lépések
[A jogkivonatok élettartama konfigurálása](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-configurable-token-lifetimes)<br>

[Az Azure AD-jogkivonat-hivatkozás](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims)


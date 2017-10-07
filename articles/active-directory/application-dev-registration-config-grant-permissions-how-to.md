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
# <a name="how-toogrant-permissions-tooa-custom-developed-application"></a>Hogyan toogrant engedélyek tooa egyéni által fejlesztett alkalmazás

Ha a kívánt toogrant hozzájárulási megelőző jelleggel az alkalmazás vagy futnak hiba történt, hogy tooan app nem beleegyezett, próbálja meg az alábbi lépéseket.

## <a name="how-tooperform-admin-consent-for-your-application"></a>Hogyan tooperform Admin hozzájárulási az alkalmazáshoz

Ennek hatása hello biztosítása hozzájárulási toohello alkalmazás a szervezete összes felhasználója számára.

1. Keresse meg a toohello **App regisztrációk** panelen, egy **globális rendszergazda**, majd válassza hello app.

2. Válassza ki **szükséges engedélyek**, és végül kattintson a hello **engedélyt adjon** hello panel felső hello gombra.

Alternatív megoldásként, hogyan hozhat létre a kérelem túl*login.microsoftonline.com* a az alkalmazás configs és a hozzáfűzendő *& Rákérdezés = admin\_hozzájárulás*. Után rendszergazdai hitelesítő adataival bejelentkezni, hello alkalmazás rendelkezik az összes felhasználó jóváhagyását.

## <a name="how-tooforce-user-consent-for-your-application"></a>Hogyan tooforce felhasználói hozzájárulás az alkalmazáshoz

* Hitelesítési kérelmek alakzatot hozzáfűzése *& Rákérdezés hozzájárulási =* minden alkalommal, amikor a hitelesítéshez a végfelhasználók tooconsent igénylő.

## <a name="next-steps"></a>Következő lépések

[Hozzájárulás és az alkalmazások integrálása tooAzureAD](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

[Hozzájárulás és Permissioning AzureAD v2.0 az összevont alkalmazások](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes)<br>

[AzureAD StackOverflow](http://stackoverflow.com/questions/tagged/azure-active-directory)

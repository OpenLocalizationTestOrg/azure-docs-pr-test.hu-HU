---
title: "bejelentkezés tooan alkalmazás aaaUnexpected beleegyezést kérő üzenete |} Microsoft Docs"
description: "Hogyan tootroubleshoot, amikor a felhasználó megkeresheti a jóváhagyási kérése az alkalmazás integrálva van az Azure AD, akkor nem várt"
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
ms.openlocfilehash: 32b7a5e6256faee0dcfe2e1644da3d3428812d35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-tooan-application"></a>Váratlan beleegyezést kérő üzenete tooan alkalmazás bejelentkezéskor

Számos alkalmazás, amelyekbe beépül az Azure Active Directory szükséges engedélyek toovarious a rendelés toorun. Ezeket az erőforrásokat is az Azure Active Directoryval integrált, amikor azok van szükség a hello Azure AD használatával engedélyek tooaccess hozzájárulás keretrendszer. 

Az eredmény egy beleegyezést kérő üzenete alatt jelenik meg, hello első alkalommal való használatakor egy alkalmazás, amely gyakran műveletet egyszer kell elvégezni. 

## <a name="scenarios-in-which-users-see-consent-prompts"></a>Forgatókönyvek, amelyben a felhasználók látni hozzájárulás kérdések

További útmutatást a különböző forgatókönyvekben várhatók:

* szükséges hello alkalmazás engedélyeiről hello készlete megváltozott.

* hello felhasználó, aki eredetileg átadni kívánt hozzájárult e toohello alkalmazás nem rendszergazda, és most egy másik (nem rendszergazda) felhasználó által használt hello alkalmazás hello az első alkalommal.

* hello felhasználó, aki eredetileg átadni kívánt hozzájárult e toohello alkalmazás rendszergazda volt, de azok nem volt hozzájárulás olyan hello teljes szervezet nevében.

* hello alkalmazás használ [növekményes és dinamikus hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest további engedélyek hozzájárulási kezdetben nyert után. Ez gyakran használják, amikor az alkalmazás további választható funkciók mellett alapterv működéséhez szükséges engedélyek szükségesek.

* Hozzájárulás kezdetben átadása után visszavonták.

* hello fejlesztői hello alkalmazás toorequire egy beleegyezést kérő üzenete van beállítva, minden alkalommal, amikor a rendszer (Megjegyzés: Ez nem ajánlott).

## <a name="next-steps"></a>Következő lépések

-   [Alkalmazások, engedélyek és az Azure Active Directoryban (1.0-s verziójú végpont) hozzájárulás](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [Hatókörök, engedélyek és az Azure Active Directoryban (v2.0-végponttól) hello hozzájárulás](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)



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
# <a name="unexpected-consent-prompt-when-signing-in-tooan-application"></a><span data-ttu-id="a3a3e-103">Váratlan beleegyezést kérő üzenete tooan alkalmazás bejelentkezéskor</span><span class="sxs-lookup"><span data-stu-id="a3a3e-103">Unexpected consent prompt when signing in tooan application</span></span>

<span data-ttu-id="a3a3e-104">Számos alkalmazás, amelyekbe beépül az Azure Active Directory szükséges engedélyek toovarious a rendelés toorun.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-104">Many applications that integrate with Azure Active Directory require permissions toovarious resources in order toorun.</span></span> <span data-ttu-id="a3a3e-105">Ezeket az erőforrásokat is az Azure Active Directoryval integrált, amikor azok van szükség a hello Azure AD használatával engedélyek tooaccess hozzájárulás keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-105">When these resources are also integrated with Azure Active Directory, permissions tooaccess them is requested using hello Azure AD consent framework.</span></span> 

<span data-ttu-id="a3a3e-106">Az eredmény egy beleegyezést kérő üzenete alatt jelenik meg, hello első alkalommal való használatakor egy alkalmazás, amely gyakran műveletet egyszer kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-106">This results in a consent prompt being shown hello first time an application is used, which is often a one-time operation.</span></span> 

## <a name="scenarios-in-which-users-see-consent-prompts"></a><span data-ttu-id="a3a3e-107">Forgatókönyvek, amelyben a felhasználók látni hozzájárulás kérdések</span><span class="sxs-lookup"><span data-stu-id="a3a3e-107">Scenarios in which users see consent prompts</span></span>

<span data-ttu-id="a3a3e-108">További útmutatást a különböző forgatókönyvekben várhatók:</span><span class="sxs-lookup"><span data-stu-id="a3a3e-108">Additional prompts can be expected in various scenarios:</span></span>

* <span data-ttu-id="a3a3e-109">szükséges hello alkalmazás engedélyeiről hello készlete megváltozott.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-109">hello set of permissions required by hello application have changed.</span></span>

* <span data-ttu-id="a3a3e-110">hello felhasználó, aki eredetileg átadni kívánt hozzájárult e toohello alkalmazás nem rendszergazda, és most egy másik (nem rendszergazda) felhasználó által használt hello alkalmazás hello az első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-110">hello user who originally consented toohello application was not an administrator, and now a different (non-admin) User is using hello application for hello first time.</span></span>

* <span data-ttu-id="a3a3e-111">hello felhasználó, aki eredetileg átadni kívánt hozzájárult e toohello alkalmazás rendszergazda volt, de azok nem volt hozzájárulás olyan hello teljes szervezet nevében.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-111">hello user who originally consented toohello application was an administrator, but they did not consent on-behalf of hello entire organization.</span></span>

* <span data-ttu-id="a3a3e-112">hello alkalmazás használ [növekményes és dinamikus hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest további engedélyek hozzájárulási kezdetben nyert után.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-112">hello application is using [incremental and dynamic consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) toorequest additional permissions after consent was initially granted.</span></span> <span data-ttu-id="a3a3e-113">Ez gyakran használják, amikor az alkalmazás további választható funkciók mellett alapterv működéséhez szükséges engedélyek szükségesek.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-113">This is often used when optional features of an application additional require permissions beyond those required for baseline functionality.</span></span>

* <span data-ttu-id="a3a3e-114">Hozzájárulás kezdetben átadása után visszavonták.</span><span class="sxs-lookup"><span data-stu-id="a3a3e-114">Consent was revoked after being granted initially.</span></span>

* <span data-ttu-id="a3a3e-115">hello fejlesztői hello alkalmazás toorequire egy beleegyezést kérő üzenete van beállítva, minden alkalommal, amikor a rendszer (Megjegyzés: Ez nem ajánlott).</span><span class="sxs-lookup"><span data-stu-id="a3a3e-115">hello developer has configured hello application toorequire a consent prompt every time it is used (note: this is not best practice).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3a3e-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3a3e-116">Next steps</span></span>

-   [<span data-ttu-id="a3a3e-117">Alkalmazások, engedélyek és az Azure Active Directoryban (1.0-s verziójú végpont) hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="a3a3e-117">Apps, permissions, and consent in Azure Active Directory (v1.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [<span data-ttu-id="a3a3e-118">Hatókörök, engedélyek és az Azure Active Directoryban (v2.0-végponttól) hello hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="a3a3e-118">Scopes, permissions, and consent in hello Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)



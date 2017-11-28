---
title: "Váratlan jóváhagyási kérése az alkalmazás történő bejelentkezéskor |} Microsoft Docs"
description: "Amikor egy felhasználó láthatja a jóváhagyási kérése integrálva van, akkor nem várt az Azure AD alkalmazás hibaelhárítása"
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
ms.openlocfilehash: e5b823e1251a7221f73efe6838d439f827f9665d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="unexpected-consent-prompt-when-signing-in-to-an-application"></a><span data-ttu-id="f0862-103">Váratlan beleegyezést kérő üzenete alkalmazáshoz való bejelentkezéskor</span><span class="sxs-lookup"><span data-stu-id="f0862-103">Unexpected consent prompt when signing in to an application</span></span>

<span data-ttu-id="f0862-104">Számos alkalmazás, amelyekbe beépül az Azure Active Directory futtatásához különböző erőforrásokhoz engedély szükséges.</span><span class="sxs-lookup"><span data-stu-id="f0862-104">Many applications that integrate with Azure Active Directory require permissions to various resources in order to run.</span></span> <span data-ttu-id="f0862-105">Ha ezeket az erőforrásokat is integrálva vannak az Azure Active Directoryval, azok eléréséhez engedélyek, van szükség az Azure AD hozzájárulási keretrendszer használatával.</span><span class="sxs-lookup"><span data-stu-id="f0862-105">When these resources are also integrated with Azure Active Directory, permissions to access them is requested using the Azure AD consent framework.</span></span> 

<span data-ttu-id="f0862-106">Az eredmény hozzájárulás kérése alatt jelenik meg, az első alkalommal való használatakor egy alkalmazás, amely gyakran műveletet egyszer kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="f0862-106">This results in a consent prompt being shown the first time an application is used, which is often a one-time operation.</span></span> 

## <a name="scenarios-in-which-users-see-consent-prompts"></a><span data-ttu-id="f0862-107">Forgatókönyvek, amelyben a felhasználók látni hozzájárulás kérdések</span><span class="sxs-lookup"><span data-stu-id="f0862-107">Scenarios in which users see consent prompts</span></span>

<span data-ttu-id="f0862-108">További útmutatást a különböző forgatókönyvekben várhatók:</span><span class="sxs-lookup"><span data-stu-id="f0862-108">Additional prompts can be expected in various scenarios:</span></span>

* <span data-ttu-id="f0862-109">Az alkalmazás által igényelt engedélykészletüket módosult.</span><span class="sxs-lookup"><span data-stu-id="f0862-109">The set of permissions required by the application have changed.</span></span>

* <span data-ttu-id="f0862-110">A felhasználó, aki eredetileg átadni kívánt hozzájárult e az alkalmazás nem rendszergazda, és most már egy másik (nem rendszergazda) felhasználó használ az alkalmazás első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="f0862-110">The user who originally consented to the application was not an administrator, and now a different (non-admin) User is using the application for the first time.</span></span>

* <span data-ttu-id="f0862-111">A felhasználó, aki eredetileg átadni kívánt hozzájárult e az alkalmazás egy rendszergazda, de azok nem volt hozzájárulás olyan a teljes szervezet nevében.</span><span class="sxs-lookup"><span data-stu-id="f0862-111">The user who originally consented to the application was an administrator, but they did not consent on-behalf of the entire organization.</span></span>

* <span data-ttu-id="f0862-112">Az alkalmazás által használt [növekményes és dinamikus hozzájárulási](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) után hozzájárulási kezdetben nyert további engedélyek kéréséhez.</span><span class="sxs-lookup"><span data-stu-id="f0862-112">The application is using [incremental and dynamic consent](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-compare#incremental-and-dynamic-consent) to request additional permissions after consent was initially granted.</span></span> <span data-ttu-id="f0862-113">Ez gyakran használják, amikor az alkalmazás további választható funkciók mellett alapterv működéséhez szükséges engedélyek szükségesek.</span><span class="sxs-lookup"><span data-stu-id="f0862-113">This is often used when optional features of an application additional require permissions beyond those required for baseline functionality.</span></span>

* <span data-ttu-id="f0862-114">Hozzájárulás kezdetben átadása után visszavonták.</span><span class="sxs-lookup"><span data-stu-id="f0862-114">Consent was revoked after being granted initially.</span></span>

* <span data-ttu-id="f0862-115">A fejlesztőnek az alkalmazást egy beleegyezést kérő üzenete megkövetelése minden alkalommal, amikor a rendszer konfigurált (Megjegyzés: Ez nem ajánlott).</span><span class="sxs-lookup"><span data-stu-id="f0862-115">The developer has configured the application to require a consent prompt every time it is used (note: this is not best practice).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f0862-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0862-116">Next steps</span></span>

-   [<span data-ttu-id="f0862-117">Alkalmazások, engedélyek és az Azure Active Directoryban (1.0-s verziójú végpont) hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="f0862-117">Apps, permissions, and consent in Azure Active Directory (v1.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/active-directory-apps-permissions-consent)

-   [<span data-ttu-id="f0862-118">Hatókörök, engedélyek és az Azure Active Directoryban (v2.0-végponttól) hozzájárulás</span><span class="sxs-lookup"><span data-stu-id="f0862-118">Scopes, permissions, and consent in the Azure Active Directory (v2.0 endpoint)</span></span>](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-scopes)



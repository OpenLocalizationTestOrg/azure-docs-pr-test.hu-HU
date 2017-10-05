---
title: "Az Azure Active Directory B2C: Létrehozása bérlők hibaelhárítása |} Microsoft Docs"
description: "Problémák és megoldások létrehozásához az Azure Active Directory vagy az Azure Active Directory B2C-bérlő."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 7ba4c6b2-161b-45b5-b3bd-ccb662f5d7a0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 81af4536fc223319369aff262d42149cfbf1a1e9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a><span data-ttu-id="3d2c4-103">Hibaelhárítás az Azure Active Directory vagy az Azure Active Directory B2C-bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d2c4-103">Troubleshoot creating an Azure Active Directory or Azure Active Directory B2C tenant</span></span> 

## <a name="create-an-azure-ad-tenant"></a><span data-ttu-id="3d2c4-104">Az Azure AD-bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d2c4-104">Create an Azure AD tenant</span></span>
<span data-ttu-id="3d2c4-105">Ha az Azure Active Directory (Azure AD) bérlő nem hozható létre az első kísérlet alkalmával, próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-105">If you can't create an Azure Active Directory (Azure AD) tenant on the first attempt, try again.</span></span> <span data-ttu-id="3d2c4-106">Ha a probléma továbbra is fennáll, forduljon az Azure támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-106">If the problem persists, contact Azure Support.</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="3d2c4-107">Azure AD B2C bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="3d2c4-107">Create an Azure AD B2C tenant</span></span>
<span data-ttu-id="3d2c4-108">Ha hibát tapasztal amikor Ön [hozzon létre egy Azure Active Directory B2C (Azure AD B2C) bérlő](active-directory-b2c-get-started.md), próbálja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="3d2c4-108">If you encounter issues when you [create an Azure Active Directory B2C (Azure AD B2C) tenant](active-directory-b2c-get-started.md), try the following options:</span></span>

* <span data-ttu-id="3d2c4-109">Ha az Azure AD B2C bérlő nem jelenik meg a bérlők listáját, újra megpróbálja létrehozni a bérlőt.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-109">If the Azure AD B2C tenant doesn't show up in your list of tenants, try again to create the tenant.</span></span>
* <span data-ttu-id="3d2c4-110">Az Azure AD B2C bérlő jelenik meg a bérlők listáját, és a következő hibaüzenet jelenik meg, ha a bérlő törölje és hozza létre újra:</span><span class="sxs-lookup"><span data-stu-id="3d2c4-110">If the Azure AD B2C tenant does show up in your list of tenants and you see the following  error message, delete the tenant and create it again:</span></span>

    <span data-ttu-id="3d2c4-111">"Nem tudta befejezni a B2C-bérlő"contosob2c"létrehozását.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-111">"Could not complete the creation of the B2C tenant 'contosob2c'.</span></span> <span data-ttu-id="3d2c4-112">Látogasson el a [hivatkozás](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) további útmutatást. "</span><span class="sxs-lookup"><span data-stu-id="3d2c4-112">Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."</span></span>
* <span data-ttu-id="3d2c4-113">Vannak ismert problémák törölje a meglévő Azure AD B2C-bérlő és hozza létre újból a ugyanazon a néven.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-113">There are known issues when you delete an existing Azure AD B2C tenant and re-create it by using the same domain name.</span></span> <span data-ttu-id="3d2c4-114">Amikor létrehoz egy új Azure AD B2C-bérlő, a másik tartománynevet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-114">When you create a new Azure AD B2C tenant, you must use a different domain name.</span></span>
* <span data-ttu-id="3d2c4-115">Ha ezek a megoldások nem működnek, lépjen kapcsolatba az Azure támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="3d2c4-115">If these resolutions don't work, contact Azure Support.</span></span> <span data-ttu-id="3d2c4-116">További információkért lásd: [fájl támogatási kérelmek az Azure AD B2C](active-directory-b2c-support.md).</span><span class="sxs-lookup"><span data-stu-id="3d2c4-116">For more information, see [File support requests for Azure AD B2C](active-directory-b2c-support.md).</span></span>


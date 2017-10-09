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
ms.openlocfilehash: 4bb7ec6ec67d3368a0e36c098f290f582510714a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-creating-an-azure-active-directory-or-azure-active-directory-b2c-tenant"></a><span data-ttu-id="91fa1-103">Hibaelhárítás az Azure Active Directory vagy az Azure Active Directory B2C-bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="91fa1-103">Troubleshoot creating an Azure Active Directory or Azure Active Directory B2C tenant</span></span> 

## <a name="create-an-azure-ad-tenant"></a><span data-ttu-id="91fa1-104">Az Azure AD-bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="91fa1-104">Create an Azure AD tenant</span></span>
<span data-ttu-id="91fa1-105">Ha nem hozható létre az Azure Active Directory (Azure AD) bérlő hello első kísérlet alkalmával, próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="91fa1-105">If you can't create an Azure Active Directory (Azure AD) tenant on hello first attempt, try again.</span></span> <span data-ttu-id="91fa1-106">Ha hello a probléma továbbra is fennáll, forduljon az Azure támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="91fa1-106">If hello problem persists, contact Azure Support.</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="91fa1-107">Azure AD B2C bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="91fa1-107">Create an Azure AD B2C tenant</span></span>
<span data-ttu-id="91fa1-108">Ha hibát tapasztal amikor Ön [hozzon létre egy Azure Active Directory B2C (Azure AD B2C) bérlő](active-directory-b2c-get-started.md), próbálja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="91fa1-108">If you encounter issues when you [create an Azure Active Directory B2C (Azure AD B2C) tenant](active-directory-b2c-get-started.md), try hello following options:</span></span>

* <span data-ttu-id="91fa1-109">Ha hello Azure AD B2C bérlő nem jelenik meg a bérlők listáját, próbálja meg újra toocreate hello bérlői.</span><span class="sxs-lookup"><span data-stu-id="91fa1-109">If hello Azure AD B2C tenant doesn't show up in your list of tenants, try again toocreate hello tenant.</span></span>
* <span data-ttu-id="91fa1-110">Hello Azure AD B2C bérlő jelenik meg a bérlők listáját, és hello a következő hibaüzenet jelenik meg, ha hello bérlői törölje és hozza létre újra:</span><span class="sxs-lookup"><span data-stu-id="91fa1-110">If hello Azure AD B2C tenant does show up in your list of tenants and you see hello following  error message, delete hello tenant and create it again:</span></span>

    <span data-ttu-id="91fa1-111">"Nem hajtható végre hello B2C-bérlő"contosob2c"hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="91fa1-111">"Could not complete hello creation of hello B2C tenant 'contosob2c'.</span></span> <span data-ttu-id="91fa1-112">Látogasson el a [hivatkozás](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) további útmutatást. "</span><span class="sxs-lookup"><span data-stu-id="91fa1-112">Please visit this [link](http://go.microsoft.com/fwlink/?LinkID=624192&clcid=0x409) for more guidance."</span></span>
* <span data-ttu-id="91fa1-113">Nincs ismert problémák Ha töröl egy meglévő Azure AD B2C-bérlőben és hozza létre újból hello segítségével ugyanazon a néven.</span><span class="sxs-lookup"><span data-stu-id="91fa1-113">There are known issues when you delete an existing Azure AD B2C tenant and re-create it by using hello same domain name.</span></span> <span data-ttu-id="91fa1-114">Amikor létrehoz egy új Azure AD B2C-bérlő, a másik tartománynevet kell használnia.</span><span class="sxs-lookup"><span data-stu-id="91fa1-114">When you create a new Azure AD B2C tenant, you must use a different domain name.</span></span>
* <span data-ttu-id="91fa1-115">Ha ezek a megoldások nem működnek, lépjen kapcsolatba az Azure támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="91fa1-115">If these resolutions don't work, contact Azure Support.</span></span> <span data-ttu-id="91fa1-116">További információkért lásd: [fájl támogatási kérelmek az Azure AD B2C](active-directory-b2c-support.md).</span><span class="sxs-lookup"><span data-stu-id="91fa1-116">For more information, see [File support requests for Azure AD B2C](active-directory-b2c-support.md).</span></span>


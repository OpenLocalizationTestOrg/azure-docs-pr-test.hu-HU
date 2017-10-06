---
title: "Az Azure Active Directory B2C: Váltás tooa B2C bérlő |} Microsoft Docs"
description: "Hogyan tooswitch való a Active Directory B2C hello kontextusában bérlői"
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 0eb1b198-44d3-4065-9fae-16591a8d3eae
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 4/13/2017
ms.author: parakhj
ms.openlocfilehash: 572f9ab283ecac68d284bb04fdfc98575bcf9393
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="switching-tooyour-azure-ad-b2c-tenant"></a><span data-ttu-id="d2443-103">Váltás tooyour az Azure AD B2C bérlő</span><span class="sxs-lookup"><span data-stu-id="d2443-103">Switching tooyour Azure AD B2C tenant</span></span>

<span data-ttu-id="d2443-104">Rendelés tooconfigure az Azure AD B2C az Azure AD B2C-bérlő hello környezetében toobe kell.</span><span class="sxs-lookup"><span data-stu-id="d2443-104">In order tooconfigure Azure AD B2C, you need toobe in hello context of your Azure AD B2C tenant.</span></span>

## <a name="log-into-azure-ad-b2c-tenant"></a><span data-ttu-id="d2443-105">Bejelentkezés az Azure AD B2C-bérlőre</span><span class="sxs-lookup"><span data-stu-id="d2443-105">Log into Azure AD B2C tenant</span></span>

<span data-ttu-id="d2443-106">az Azure AD B2C-bérlőben toonavigate tooyour, be kell jelentkeznie az Azure-portálon hello hello Azure AD B2C bérlő globális rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d2443-106">toonavigate tooyour Azure AD B2C tenant, you must be logged into hello Azure portal as a global administrator of hello Azure AD B2C tenant.</span></span>

1. <span data-ttu-id="d2443-107">Jelentkezzen be a hello [Azure-portálon](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2443-107">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
1. <span data-ttu-id="d2443-108">Váltás a bérlők az e-mail cím vagy a kép hello jobb felső sarokban kattintva.</span><span class="sxs-lookup"><span data-stu-id="d2443-108">Switch tenants by clicking your email address or picture in hello top-right corner.</span></span>
1. <span data-ttu-id="d2443-109">A hello `Directory` lista, amely akkor jelenik meg, hogy kívánja-e toomanage válassza hello Azure AD B2C bérlő.</span><span class="sxs-lookup"><span data-stu-id="d2443-109">In hello `Directory` list that appears, select hello Azure AD B2C tenant that you wish toomanage.</span></span>

<span data-ttu-id="d2443-110">hello Azure Portal frissíti a rendszer.</span><span class="sxs-lookup"><span data-stu-id="d2443-110">hello Azure Portal will refresh.</span></span>  <span data-ttu-id="d2443-111">Most már Ön bejelentkezett hello Azure portálon az Azure AD B2C-bérlő hello környezetében.</span><span class="sxs-lookup"><span data-stu-id="d2443-111">Now you are signed into hello Azure Portal in hello context of your Azure AD B2C tenant.</span></span>

## <a name="navigate-toohello-b2c-features-blade"></a><span data-ttu-id="d2443-112">Keresse meg a toohello B2C funkciók panelje</span><span class="sxs-lookup"><span data-stu-id="d2443-112">Navigate toohello B2C features blade</span></span>

1. <span data-ttu-id="d2443-113">Kattintson a **Tallózás** hello bal oldali navigációs.</span><span class="sxs-lookup"><span data-stu-id="d2443-113">Click **Browse** on hello left hand navigation.</span></span>
1. <span data-ttu-id="d2443-114">Kattintson a **> További szolgáltatások** majd keresse meg a `Azure AD B2C` hello bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d2443-114">Click **> More services** and then search for `Azure AD B2C` in hello left navigation pane.</span></span>  <span data-ttu-id="d2443-115">(toopin tooyour bal Kezdőpulton, kattintson az Azure AD B2C hello csillag toohello balra)</span><span class="sxs-lookup"><span data-stu-id="d2443-115">(toopin tooyour left-hand Startboard, click hello star toohello left of Azure AD B2C)</span></span>
1. <span data-ttu-id="d2443-116">Kattintson a **az Azure AD B2C** tooaccess hello B2C funkciók panelje.</span><span class="sxs-lookup"><span data-stu-id="d2443-116">Click **Azure AD B2C** tooaccess hello B2C features blade.</span></span>
   
    ![Kattintás a Tallózás tooB2C funkciók panelje](./media/active-directory-b2c-get-started/b2c-browse.png)

> [!IMPORTANT]
> <span data-ttu-id="d2443-118">Toobe hello B2C bérlő toobe képes tooaccess hello B2C funkciók panelje globális rendszergazdájának kell.</span><span class="sxs-lookup"><span data-stu-id="d2443-118">You need toobe a Global Administrator of hello B2C tenant toobe able tooaccess hello B2C features blade.</span></span> <span data-ttu-id="d2443-119">Más bérlők globális rendszergazdái vagy felhasználói nem férhetnek hozzá a panelhez.</span><span class="sxs-lookup"><span data-stu-id="d2443-119">A Global Administrator from any other tenant or a user from any tenant cannot access it.</span></span>  <span data-ttu-id="d2443-120">Átválthat tooyour B2C-bérlő hello jobb felső sarkában hello Azure-portálon hello bérlői kapcsoló használatával.</span><span class="sxs-lookup"><span data-stu-id="d2443-120">You can switch tooyour B2C tenant by using hello tenant switcher in hello top right corner of hello Azure portal.</span></span>

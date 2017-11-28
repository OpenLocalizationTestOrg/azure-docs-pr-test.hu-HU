---
title: "Az Azure Active Directory B2C: Azure Active Directory B2C-bérlő létrehozása |} Microsoft Docs"
description: "Témakör: hogyan toocreate egy Azure Active Directory B2C-bérlőben"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: patricka
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 06/07/2017
ms.author: swkrish
ms.openlocfilehash: e8b257d66c1f66ffb84f5d3d21b30b42eddcbac9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-hello-azure-portal"></a><span data-ttu-id="1ad9f-103">Hozzon létre egy Azure Active Directory B2C-bérlő hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1ad9f-103">Create an Azure Active Directory B2C tenant in hello Azure portal</span></span>

<span data-ttu-id="1ad9f-104">Sipi szerkesztését.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-104">Edited by Sipi.</span></span>

<span data-ttu-id="1ad9f-105">A gyors üzembe helyezés segítséget nyújt a Microsoft Azure Active Directory (Azure AD) B2C-bérlő létrehozása néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-105">This Quickstart helps you create a Microsoft Azure Active Directory (Azure AD) B2C tenant in just a few minutes.</span></span> <span data-ttu-id="1ad9f-106">Amikor végzett, hogy a B2C bérlő toouse B2C alkalmazások regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-106">When you're finished, you have a B2C tenant toouse for registering B2C applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1ad9f-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1ad9f-107">Prerequisites</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-tooazure"></a><span data-ttu-id="1ad9f-108">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="1ad9f-108">Log in tooAzure</span></span>

<span data-ttu-id="1ad9f-109">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1ad9f-109">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="1ad9f-110">Azure AD B2C bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ad9f-110">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="1ad9f-111">B2C funkciók nem engedélyezhetők a a meglévő bérlők számára.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-111">B2C features can't be enabled in your existing tenants.</span></span> <span data-ttu-id="1ad9f-112">Azure AD B2C-bérlő toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-112">You need toocreate an Azure AD B2C tenant.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

<span data-ttu-id="1ad9f-113">Gratulálunk, az Azure Active Directory B2C-bérlő hozott létre.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-113">Congratulations, you have created an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="1ad9f-114">Biztosan hello bérlő globális rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-114">You are a Global Administrator of hello tenant.</span></span> <span data-ttu-id="1ad9f-115">Szükség szerint más globális rendszergazdákat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-115">You can add other Global Administrators as required.</span></span> <span data-ttu-id="1ad9f-116">új tooswitch tooyour bérlői, kattintson a hello *kezelése az új bérlő hivatkozás*.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-116">tooswitch tooyour new tenant, click hello *manage your new tenant link*.</span></span>

![Az új bérlő hivatkozás kezelése](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> <span data-ttu-id="1ad9f-118">Ha azt tervezi, a B2C-bérlő éles alkalmazások toouse, hello a cikk elolvasása a [éles méretű és a B2C előzetes bérlők](active-directory-b2c-reference-tenant-type.md).</span><span class="sxs-lookup"><span data-stu-id="1ad9f-118">If you are planning toouse a B2C tenant for a production app, read hello article on [production-scale vs. preview B2C tenants](active-directory-b2c-reference-tenant-type.md).</span></span> <span data-ttu-id="1ad9f-119">Vannak, ismert problémákat, ha töröl egy meglévő B2C bérlői, és hozza létre újból hello ugyanazon a néven.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-119">There are known issues when you delete an existing B2C tenant and re-create it with hello same domain name.</span></span> <span data-ttu-id="1ad9f-120">A B2C-bérlő egy másik tartománynév toocreate van szüksége.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-120">You need toocreate a B2C tenant with a different domain name.</span></span>
>
>

## <a name="link-your-tenant-tooyour-subscription"></a><span data-ttu-id="1ad9f-121">A bérlői tooyour előfizetés hivatkozás</span><span class="sxs-lookup"><span data-stu-id="1ad9f-121">Link your tenant tooyour subscription</span></span>

<span data-ttu-id="1ad9f-122">Toolink van szüksége az Azure AD B2C bérlői tooyour Azure-előfizetés tooenable a B2C-funkciókat és használati díjat kell fizetnie.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-122">You need toolink your Azure AD B2C tenant tooyour Azure subscription tooenable all B2C functionality and pay for usage charges.</span></span> <span data-ttu-id="1ad9f-123">több, olvassa el az toolearn [Ez a cikk](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="1ad9f-123">toolearn more, read [this article](active-directory-b2c-how-to-enable-billing.md).</span></span> <span data-ttu-id="1ad9f-124">Ha nem az Azure AD B2C bérlő tooyour Azure-előfizetése, néhány funkció le van tiltva, és megjelenik egy figyelmeztető üzenet ("Nincs előfizetés csatolt toothis B2C-bérlőben vagy előfizetés hello kell a figyelmet.") a hello B2C beállítások.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-124">If you don't link your Azure AD B2C tenant tooyour Azure subscription, some functionality is blocked and, you see a warning message ("No Subscription linked toothis B2C tenant or hello Subscription needs your attention.") in hello B2C settings.</span></span> <span data-ttu-id="1ad9f-125">Fontos, hogy ezzel a lépéssel előtt az alkalmazások éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-125">It is important that you take this step before you ship your apps into production.</span></span>

## <a name="easy-access-toosettings"></a><span data-ttu-id="1ad9f-126">Könnyű hozzáférés toosettings</span><span class="sxs-lookup"><span data-stu-id="1ad9f-126">Easy access toosettings</span></span>

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

<span data-ttu-id="1ad9f-127">Emellett hello panel megadásával `Azure AD B2C` a **keresési erőforrások** hello portal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-127">You can also access hello blade by entering `Azure AD B2C` in **Search resources** at hello top of hello portal.</span></span> <span data-ttu-id="1ad9f-128">Hello eredmények listában válassza ki a **az Azure AD B2C** tooaccess hello B2C beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="1ad9f-128">In hello results list, select **Azure AD B2C** tooaccess hello B2C settings blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ad9f-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ad9f-129">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1ad9f-130">A B2C alkalmazás regisztrálása a B2C-bérlőben</span><span class="sxs-lookup"><span data-stu-id="1ad9f-130">Register your B2C application in a B2C tenant</span></span>](active-directory-b2c-app-registration.md)

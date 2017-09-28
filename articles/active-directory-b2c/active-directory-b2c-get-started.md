---
title: "Az Azure Active Directory B2C: Azure Active Directory B2C-bérlő létrehozása |} Microsoft Docs"
description: "Témakör: Azure Active Directory B2C-bérlő létrehozása"
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
ms.openlocfilehash: 8a1d4935397f59e5813afc6f04559e471187a779
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-an-azure-active-directory-b2c-tenant-in-the-azure-portal"></a><span data-ttu-id="1435f-103">Azure Active Directory B2C-bérlő létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1435f-103">Create an Azure Active Directory B2C tenant in the Azure portal</span></span>

<span data-ttu-id="1435f-104">A gyors üzembe helyezés segítséget nyújt a Microsoft Azure Active Directory (Azure AD) B2C-bérlő létrehozása néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="1435f-104">This Quickstart helps you create a Microsoft Azure Active Directory (Azure AD) B2C tenant in just a few minutes.</span></span> <span data-ttu-id="1435f-105">Amikor végzett, akkor a B2C-bérlő regisztrálása a B2C-alkalmazásokhoz használandó.</span><span class="sxs-lookup"><span data-stu-id="1435f-105">When you're finished, you have a B2C tenant to use for registering B2C applications.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1435f-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1435f-106">Prerequisites</span></span>

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

##  <a name="log-in-to-azure"></a><span data-ttu-id="1435f-107">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="1435f-107">Log in to Azure</span></span>

<span data-ttu-id="1435f-108">Jelentkezzen be az [Azure portálra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1435f-108">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="create-an-azure-ad-b2c-tenant"></a><span data-ttu-id="1435f-109">Azure AD B2C bérlő létrehozása</span><span class="sxs-lookup"><span data-stu-id="1435f-109">Create an Azure AD B2C tenant</span></span>

<span data-ttu-id="1435f-110">B2C funkciók nem engedélyezhetők a a meglévő bérlők számára.</span><span class="sxs-lookup"><span data-stu-id="1435f-110">B2C features can't be enabled in your existing tenants.</span></span> <span data-ttu-id="1435f-111">Azure AD B2C-bérlő létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="1435f-111">You need to create an Azure AD B2C tenant.</span></span>

[!INCLUDE [active-directory-b2c-create-tenant](../../includes/active-directory-b2c-create-tenant.md)]

<span data-ttu-id="1435f-112">Gratulálunk, az Azure Active Directory B2C-bérlő hozott létre.</span><span class="sxs-lookup"><span data-stu-id="1435f-112">Congratulations, you have created an Azure Active Directory B2C tenant.</span></span> <span data-ttu-id="1435f-113">A bérlő globális rendszergazdája áll.</span><span class="sxs-lookup"><span data-stu-id="1435f-113">You are a Global Administrator of the tenant.</span></span> <span data-ttu-id="1435f-114">Szükség szerint más globális rendszergazdákat is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="1435f-114">You can add other Global Administrators as required.</span></span> <span data-ttu-id="1435f-115">Az új bérlőjéhez tartozik, kattintson a *kezelése az új bérlő hivatkozás*.</span><span class="sxs-lookup"><span data-stu-id="1435f-115">To switch to your new tenant, click the *manage your new tenant link*.</span></span>

![Az új bérlő hivatkozás kezelése](./media/active-directory-b2c-get-started/manage-new-b2c-tenant-link.png)

> [!IMPORTANT]
> <span data-ttu-id="1435f-117">Ha azt tervezi, hogy a B2C-bérlő éles alkalmazások használja, olvassa el a cikk a [éles méretű és a B2C előzetes bérlők](active-directory-b2c-reference-tenant-type.md).</span><span class="sxs-lookup"><span data-stu-id="1435f-117">If you are planning to use a B2C tenant for a production app, read the article on [production-scale vs. preview B2C tenants](active-directory-b2c-reference-tenant-type.md).</span></span> <span data-ttu-id="1435f-118">Vannak ismert problémák egy meglévő B2C-bérlő törölje és hozza létre újból az azonos nevű tartomány.</span><span class="sxs-lookup"><span data-stu-id="1435f-118">There are known issues when you delete an existing B2C tenant and re-create it with the same domain name.</span></span> <span data-ttu-id="1435f-119">Egy másik tartomány nevét a B2C-bérlő létrehozásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="1435f-119">You need to create a B2C tenant with a different domain name.</span></span>
>
>

## <a name="link-your-tenant-to-your-subscription"></a><span data-ttu-id="1435f-120">A bérlő csatolása az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="1435f-120">Link your tenant to your subscription</span></span>

<span data-ttu-id="1435f-121">Az Azure AD B2C-bérlő csatolása az Azure-előfizetés engedélyezése a B2C-funkciókat és használati díjat kell fizetnie kell.</span><span class="sxs-lookup"><span data-stu-id="1435f-121">You need to link your Azure AD B2C tenant to your Azure subscription to enable all B2C functionality and pay for usage charges.</span></span> <span data-ttu-id="1435f-122">További tudnivalókért olvassa el a [Ez a cikk](active-directory-b2c-how-to-enable-billing.md).</span><span class="sxs-lookup"><span data-stu-id="1435f-122">To learn more, read [this article](active-directory-b2c-how-to-enable-billing.md).</span></span> <span data-ttu-id="1435f-123">Ha az Azure AD B2C-bérlő nem kapcsolódik az Azure-előfizetéssel, néhány funkció le van tiltva, és megjelenik egy figyelmeztető üzenet ("Nincs előfizetés csatolva a B2C-bérlő vagy az előfizetés kell a figyelmet.") a B2C beállítások.</span><span class="sxs-lookup"><span data-stu-id="1435f-123">If you don't link your Azure AD B2C tenant to your Azure subscription, some functionality is blocked and, you see a warning message ("No Subscription linked to this B2C tenant or the Subscription needs your attention.") in the B2C settings.</span></span> <span data-ttu-id="1435f-124">Fontos, hogy ezzel a lépéssel előtt az alkalmazások éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="1435f-124">It is important that you take this step before you ship your apps into production.</span></span>

## <a name="easy-access-to-settings"></a><span data-ttu-id="1435f-125">Egyszerű hozzáférést beállításai</span><span class="sxs-lookup"><span data-stu-id="1435f-125">Easy access to settings</span></span>

[!INCLUDE [active-directory-b2c-find-service-settings](../../includes/active-directory-b2c-find-service-settings.md)]

<span data-ttu-id="1435f-126">Emellett a blade megadásával `Azure AD B2C` a **keresési erőforrások** a portál felső.</span><span class="sxs-lookup"><span data-stu-id="1435f-126">You can also access the blade by entering `Azure AD B2C` in **Search resources** at the top of the portal.</span></span> <span data-ttu-id="1435f-127">Az eredmények listájában válassza **az Azure AD B2C** a B2C beállítások panel eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1435f-127">In the results list, select **Azure AD B2C** to access the B2C settings blade.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1435f-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1435f-128">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1435f-129">A B2C alkalmazás regisztrálása a B2C-bérlőben</span><span class="sxs-lookup"><span data-stu-id="1435f-129">Register your B2C application in a B2C tenant</span></span>](active-directory-b2c-app-registration.md)
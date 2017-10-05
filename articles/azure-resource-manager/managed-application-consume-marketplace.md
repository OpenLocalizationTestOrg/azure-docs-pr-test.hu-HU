---
title: "Kezelt alkalmazás Azure piactér felhasználása |} Microsoft Docs"
description: "Describeshow hozhat létre egy Azure felügyelt alkalmazást, amely elérhető a piactéren keresztül."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: baf456740bddd562391ed64d707f990c8921d710
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="consume-azure-managed-applications-in-the-marketplace"></a><span data-ttu-id="c1e30-103">Azure felhasználásához felügyelt alkalmazások a piactéren</span><span class="sxs-lookup"><span data-stu-id="c1e30-103">Consume Azure managed applications in the Marketplace</span></span>

<span data-ttu-id="c1e30-104">A bemutatott a [kezelt alkalmazás – áttekintés](managed-application-overview.md) cikk, van két olyan eset a teljes körű élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="c1e30-104">As discussed in the [Managed Application overview](managed-application-overview.md) article, there are two scenarios in the end to end experience.</span></span> <span data-ttu-id="c1e30-105">A közzétevő vagy a szállító, hozzon létre egy felügyelt alkalmazást, az ügyfelek által használható számára, akik egyik.</span><span class="sxs-lookup"><span data-stu-id="c1e30-105">One is the publisher or vendor who wants to create a managed application for use by customers.</span></span> <span data-ttu-id="c1e30-106">A második pedig a végfelhasználók az ügyfél vagy a fogyasztó a kezelt alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c1e30-106">The second is the end customer or the consumer of the managed application.</span></span> <span data-ttu-id="c1e30-107">Ez a cikk ismerteti a második forgatókönyv szerint.</span><span class="sxs-lookup"><span data-stu-id="c1e30-107">This article covers the second scenario.</span></span> <span data-ttu-id="c1e30-108">Leírja, hogyan telepíthet egy kezelt alkalmazást, a Microsoft Azure piactérről.</span><span class="sxs-lookup"><span data-stu-id="c1e30-108">It describes how you can deploy a managed application from the Microsoft Azure Marketplace.</span></span>

## <a name="create-from-the-marketplace"></a><span data-ttu-id="c1e30-109">Hozzon létre a piactérről</span><span class="sxs-lookup"><span data-stu-id="c1e30-109">Create from the Marketplace</span></span>

<span data-ttu-id="c1e30-110">A piactérről kezelt alkalmazás telepítése hasonlít bármilyen típusú erőforrások telepítése a piactéren.</span><span class="sxs-lookup"><span data-stu-id="c1e30-110">Deploying a managed application from the Marketplace is similar to deploying any type of resources from the Marketplace.</span></span> 

<span data-ttu-id="c1e30-111">Válassza a portál **+ új** , és keresse meg a telepítendő megoldás típusával.</span><span class="sxs-lookup"><span data-stu-id="c1e30-111">In the portal, select **+ New** and search for the type of solution to deploy.</span></span> <span data-ttu-id="c1e30-112">Az elérhető lehetőségek közül jelölje ki a van szüksége.</span><span class="sxs-lookup"><span data-stu-id="c1e30-112">From the available options, select the one you need.</span></span>

![keresési megoldások](./media/managed-application-consume-marketplace/search-apps.png)

<span data-ttu-id="c1e30-114">Tekintse át az összefoglalást, az alkalmazás, és válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c1e30-114">Review the summary of the application, and select **Create**.</span></span>

![kezelt alkalmazás létrehozása](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

<span data-ttu-id="c1e30-116">Más megoldás, például lehetősége lesz arra, hogy az értékek mezőkkel.</span><span class="sxs-lookup"><span data-stu-id="c1e30-116">Like any other solution, you are presented with fields to provide values for.</span></span> <span data-ttu-id="c1e30-117">Ezek a mezők változó által kezelt alkalmazás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c1e30-117">These fields vary by the type of managed application you create.</span></span> 

## <a name="view-support-information"></a><span data-ttu-id="c1e30-118">Támogatási információk megtekintése</span><span class="sxs-lookup"><span data-stu-id="c1e30-118">View support information</span></span>

<span data-ttu-id="c1e30-119">Miután a kezelt alkalmazás telepítve van, az alkalmazás támogatási információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="c1e30-119">After your managed application has deployed, view the support information for the application.</span></span> <span data-ttu-id="c1e30-120">A kezelt alkalmazás paneljén jelenik meg a támogatási információkat.</span><span class="sxs-lookup"><span data-stu-id="c1e30-120">In the managed application blade, the support information is listed.</span></span>

![Támogatás](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a><span data-ttu-id="c1e30-122">A publisher engedélyek megtekintése</span><span class="sxs-lookup"><span data-stu-id="c1e30-122">View publisher permissions</span></span>

<span data-ttu-id="c1e30-123">A gyártó, amely felügyeli az alkalmazás az erőforrásokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="c1e30-123">The vendor that manages your application is granted access to your resources.</span></span> <span data-ttu-id="c1e30-124">Ezeket az engedélyeket, jelölje ki a **engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="c1e30-124">To see those permissions, select **Authorizations**.</span></span>

![Engedélyek](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a><span data-ttu-id="c1e30-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1e30-126">Next steps</span></span>

* <span data-ttu-id="c1e30-127">További információ a kezelt alkalmazás közzététele a piactéren: [által felügyelt alkalmazások Azure piactér](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="c1e30-127">For information about publishing a managed application in the Marketplace, see [Azure Managed Applications in Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="c1e30-128">Felügyelt alkalmazások, amelyek csak a szervezet felhasználói számára elérhető közzétételét, lásd: [létrehozása és a szolgáltatási katalógus kezelt alkalmazás közzététele](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="c1e30-128">To publish managed applications that are only available to users in your organization, see [Create and publish Service Catalog Managed Application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="c1e30-129">Felügyelt alkalmazások, amelyek csak a szervezet felhasználói számára elérhető felhasználását, lásd: [szolgáltatás katalógus kezelt alkalmazás felhasználása](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="c1e30-129">To consume managed applications that are only available to users in your organization, see [Consume a Service Catalog Managed Application](managed-application-consumption.md).</span></span>
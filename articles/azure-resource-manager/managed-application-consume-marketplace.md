---
title: "Kezelt alkalmazás Azure piactér aaaConsume |} Microsoft Docs"
description: "Describeshow toocreate Azure felügyelt alkalmazás hello piactéren keresztül érhető el."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 9ae6e11a3f63eb58a9f3199364b5606a7afe5618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consume-azure-managed-applications-in-hello-marketplace"></a><span data-ttu-id="0e35f-103">Azure felhasználásához hello piactér-alkalmazások felügyelete</span><span class="sxs-lookup"><span data-stu-id="0e35f-103">Consume Azure managed applications in hello Marketplace</span></span>

<span data-ttu-id="0e35f-104">Hello leírtaknak megfelelően [kezelt alkalmazás – áttekintés](managed-application-overview.md) cikk, van két olyan eset hello end tooend élményt nyújt.</span><span class="sxs-lookup"><span data-stu-id="0e35f-104">As discussed in hello [Managed Application overview](managed-application-overview.md) article, there are two scenarios in hello end tooend experience.</span></span> <span data-ttu-id="0e35f-105">Egyik hello közzétevőt vagy szállító kívánó toocreate egy kezelt alkalmazást, az ügyfelek általi használatra.</span><span class="sxs-lookup"><span data-stu-id="0e35f-105">One is hello publisher or vendor who wants toocreate a managed application for use by customers.</span></span> <span data-ttu-id="0e35f-106">hello második hello end vevő vagy hello fogyasztói hello felügyelt alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0e35f-106">hello second is hello end customer or hello consumer of hello managed application.</span></span> <span data-ttu-id="0e35f-107">Ez a cikk ismerteti a hello második forgatókönyv szerint.</span><span class="sxs-lookup"><span data-stu-id="0e35f-107">This article covers hello second scenario.</span></span> <span data-ttu-id="0e35f-108">Leírja, hogyan telepíthet egy kezelt alkalmazást, a Microsoft Azure piactérről hello.</span><span class="sxs-lookup"><span data-stu-id="0e35f-108">It describes how you can deploy a managed application from hello Microsoft Azure Marketplace.</span></span>

## <a name="create-from-hello-marketplace"></a><span data-ttu-id="0e35f-109">Hello piactér létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e35f-109">Create from hello Marketplace</span></span>

<span data-ttu-id="0e35f-110">Erőforrások a piactér hello bármilyen hasonló toodeploying hello piactér a kezelt alkalmazás telepítése.</span><span class="sxs-lookup"><span data-stu-id="0e35f-110">Deploying a managed application from hello Marketplace is similar toodeploying any type of resources from hello Marketplace.</span></span> 

<span data-ttu-id="0e35f-111">Hello portálon, válassza ki a **+ új** , és keressen megoldást toodeploy hello típusú.</span><span class="sxs-lookup"><span data-stu-id="0e35f-111">In hello portal, select **+ New** and search for hello type of solution toodeploy.</span></span> <span data-ttu-id="0e35f-112">Hello rendelkezésre álló lehetőségeket válassza ki valamelyik kell hello.</span><span class="sxs-lookup"><span data-stu-id="0e35f-112">From hello available options, select hello one you need.</span></span>

![keresési megoldások](./media/managed-application-consume-marketplace/search-apps.png)

<span data-ttu-id="0e35f-114">Tekintse át a hello alkalmazás hello összefoglalása, és válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0e35f-114">Review hello summary of hello application, and select **Create**.</span></span>

![kezelt alkalmazás létrehozása](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

<span data-ttu-id="0e35f-116">Például a más megoldás lehetősége lesz mezők tooprovide értékeit.</span><span class="sxs-lookup"><span data-stu-id="0e35f-116">Like any other solution, you are presented with fields tooprovide values for.</span></span> <span data-ttu-id="0e35f-117">Ezek a mezők hoz létre a felügyelt alkalmazási hello típusú lehet.</span><span class="sxs-lookup"><span data-stu-id="0e35f-117">These fields vary by hello type of managed application you create.</span></span> 

## <a name="view-support-information"></a><span data-ttu-id="0e35f-118">Támogatási információk megtekintése</span><span class="sxs-lookup"><span data-stu-id="0e35f-118">View support information</span></span>

<span data-ttu-id="0e35f-119">Miután a kezelt alkalmazás telepítve van, hello alkalmazás hello támogatási információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="0e35f-119">After your managed application has deployed, view hello support information for hello application.</span></span> <span data-ttu-id="0e35f-120">Hello kezelt alkalmazás paneljén hello támogatási információkat szerepel.</span><span class="sxs-lookup"><span data-stu-id="0e35f-120">In hello managed application blade, hello support information is listed.</span></span>

![Támogatás](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a><span data-ttu-id="0e35f-122">A publisher engedélyek megtekintése</span><span class="sxs-lookup"><span data-stu-id="0e35f-122">View publisher permissions</span></span>

<span data-ttu-id="0e35f-123">amely felügyeli az alkalmazás hello szállítói kapnak tooyour erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0e35f-123">hello vendor that manages your application is granted access tooyour resources.</span></span> <span data-ttu-id="0e35f-124">toosee ezeket az engedélyeket, válassza ki **engedélyek**.</span><span class="sxs-lookup"><span data-stu-id="0e35f-124">toosee those permissions, select **Authorizations**.</span></span>

![Engedélyek](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a><span data-ttu-id="0e35f-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e35f-126">Next steps</span></span>

* <span data-ttu-id="0e35f-127">További információ a kezelt alkalmazás közzététele hello piactér: [által felügyelt alkalmazások Azure piactér](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="0e35f-127">For information about publishing a managed application in hello Marketplace, see [Azure Managed Applications in Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="0e35f-128">toopublish felügyelt alkalmazásokat, amelyek a szervezet csak elérhető toousers lásd [létrehozása és a szolgáltatási katalógus kezelt alkalmazás közzététele](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="0e35f-128">toopublish managed applications that are only available toousers in your organization, see [Create and publish Service Catalog Managed Application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="0e35f-129">tooconsume felügyelt alkalmazásokat, amelyek a szervezet csak elérhető toousers lásd [szolgáltatás katalógus kezelt alkalmazás felhasználása](managed-application-consumption.md).</span><span class="sxs-lookup"><span data-stu-id="0e35f-129">tooconsume managed applications that are only available toousers in your organization, see [Consume a Service Catalog Managed Application](managed-application-consumption.md).</span></span>

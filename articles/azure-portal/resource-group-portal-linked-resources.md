---
title: "aaaRelated és a kapcsolódó erőforrások hello csempe gyűjteménye"
description: "További információk a kapcsolódó és csatolt erőforrásokat hello csempe galériája hello Azure betekintő portálon jelennek meg."
services: azure-portal
documentationcenter: 
author: adamabdelhamed
manager: wpickett
editor: 
ms.assetid: dba96d29-f518-49c8-bfd2-f14cecb44cbf
ms.service: azure-portal
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/16/2015
ms.author: adamab
ms.openlocfilehash: c8f99be8e23dc9641ec3cd3169604b33a4b049f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="related-and-linked-resources-in-hello-tile-gallery"></a><span data-ttu-id="a8fed-103">Kapcsolódó és csatolt erőforrások hello csempe gyűjteménye</span><span class="sxs-lookup"><span data-stu-id="a8fed-103">Related and linked resources in hello tile gallery</span></span>
<span data-ttu-id="a8fed-104">hello csempe gyűjteménye lehetővé teszi egy adott erőforráshoz toofind csempéket, és húzza őket a jelenlegi panelen.</span><span class="sxs-lookup"><span data-stu-id="a8fed-104">hello tile gallery enables you toofind tiles for a particular resource and drag them onto your current blade.</span></span> <span data-ttu-id="a8fed-105">Hello csempe gyűjtemény használva létrehozhat erőforrásokat tartalmazó felügyeleti nézetek.</span><span class="sxs-lookup"><span data-stu-id="a8fed-105">Using hello tile gallery, you can create management views that span resources.</span></span> <span data-ttu-id="a8fed-106">A megadott erőforrás hello kapcsolódó erőforrások közé tartoznak a hello erőforrások az erőforráscsoportban, és minden olyan erőforrásnál, amely hozzárendeli a hello erőforrás tooor.</span><span class="sxs-lookup"><span data-stu-id="a8fed-106">For any specified resource, hello related resources include all hello resources in its resource group, and any resources that link tooor from hello resource.</span></span>

## <a name="linked-resources-in-resource-manager"></a><span data-ttu-id="a8fed-107">Az erőforrás-kezelőben kapcsolt erőforrásokban</span><span class="sxs-lookup"><span data-stu-id="a8fed-107">Linked resources in Resource Manager</span></span>
<span data-ttu-id="a8fed-108">A csatolás akkor hello erőforrás-kezelő szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="a8fed-108">Linking is a feature of hello Resource Manager.</span></span>  <span data-ttu-id="a8fed-109">Lehetővé teszi a erőforrásainak kapcsolatai toodeclare akkor is, ha azok nem találhatók hello azonos erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a8fed-109">It enables you toodeclare relationships between resources even if they do not reside in hello same resource group.</span></span> <span data-ttu-id="a8fed-110">Hello futásidejű az erőforrások, ne legyen hatással a számlázási és ne legyen hatással a szerepköralapú hozzáférés-Linking rendelkezik nincs hatással.</span><span class="sxs-lookup"><span data-stu-id="a8fed-110">Linking has no impact on hello runtime of your resources, no impact on billing, and no impact on role-based access.</span></span>  <span data-ttu-id="a8fed-111">Egyszerűen csak egy toorepresent kapcsolatainak használata, hogy az eszközök, például hello csempe gyűjtemény biztosíthat gazdag felügyeleti élmény mechanizmust is.</span><span class="sxs-lookup"><span data-stu-id="a8fed-111">It's simply a mechanism you can use toorepresent relationships so that tools like hello tile gallery can provide a rich management experience.</span></span>  <span data-ttu-id="a8fed-112">Az eszközök hello hivatkozásokkal API hello hivatkozások vizsgálják, és adjon meg egyéni kezelése, valamint észlel.</span><span class="sxs-lookup"><span data-stu-id="a8fed-112">Your tools can inspect hello links using hello links API and provide custom relationship management experiences as well.</span></span> 

## <a name="how-do-i-link-my-resources"></a><span data-ttu-id="a8fed-113">Hogyan kapcsolhatók össze az erőforrások?</span><span class="sxs-lookup"><span data-stu-id="a8fed-113">How do I link my resources?</span></span>
<span data-ttu-id="a8fed-114">Hello portálon keresztül, vagy telepítsen egy sablon Azure PowerShell vagy Azure parancssori felületen keresztül erőforrások létrehozásakor hivatkozások automatikusan létrejönnek az egyes függő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="a8fed-114">When you create resources through hello portal or by deploying a template through Azure PowerShell or Azure CLI, links are automatically created for some dependent resources.</span></span> <span data-ttu-id="a8fed-115">Erőforrások hello segítségével szoftveresen is csatolhatja [csatolt erőforrások REST API](/rest/api/resources/resourcelinks).</span><span class="sxs-lookup"><span data-stu-id="a8fed-115">You can also programmatically link resources by using hello [Linked Resources REST API](/rest/api/resources/resourcelinks).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a8fed-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a8fed-116">Next steps</span></span>
* <span data-ttu-id="a8fed-117">Ha egy bevezető toowriting Resource Manager-sablonok van szüksége, tekintse meg [sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a8fed-117">If you need an introduction toowriting Resource Manager templates, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="a8fed-118">További információ az erőforráscsoportok hello portálon keresztül használata toounderstand lásd: [Using hello Azure portál toomanage az Azure-erőforrások](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a8fed-118">toounderstand more about working with resource groups through hello portal, see [Using hello Azure portal toomanage your Azure resources](../azure-resource-manager/resource-group-portal.md).</span></span>


---
title: "aaaDefine gyermek erőforrás az Azure-sablon alapján |} Microsoft Docs"
description: "Bemutatja, hogyan tooset hello erőforrástípus és az Azure Resource Manager-sablon a gyermek-erőforrás nevét"
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: tomfitz
ms.openlocfilehash: c502c589100d7ae864d7fb01b5ba10ddfaf92592
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a><span data-ttu-id="94e8b-103">A Resource Manager-sablon nevét és típusát gyermek erőforrás beállítása</span><span class="sxs-lookup"><span data-stu-id="94e8b-103">Set name and type for child resource in Resource Manager template</span></span>
<span data-ttu-id="94e8b-104">Sablon létrehozásakor gyakran kell tooinclude kapcsolódó tooa szülő erőforrás gyermek erőforrásra.</span><span class="sxs-lookup"><span data-stu-id="94e8b-104">When creating a template, you frequently need tooinclude a child resource that is related tooa parent resource.</span></span> <span data-ttu-id="94e8b-105">A sablon tartalmazhat például egy SQL server és adatbázis.</span><span class="sxs-lookup"><span data-stu-id="94e8b-105">For example, your template may include a SQL server and a database.</span></span> <span data-ttu-id="94e8b-106">hello SQL server hello szülő erőforrás, hello adatbázis pedig hello gyermek-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="94e8b-106">hello SQL server is hello parent resource, and hello database is hello child resource.</span></span> 

<span data-ttu-id="94e8b-107">hello hello gyermek erőforrástípus formátuma:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span><span class="sxs-lookup"><span data-stu-id="94e8b-107">hello format of hello child resource type is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span></span>

<span data-ttu-id="94e8b-108">hello hello gyermek erőforrás nevének formátuma:`{parent-resource-name}/{child-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="94e8b-108">hello format of hello child resource name is: `{parent-resource-name}/{child-resource-name}`</span></span>

<span data-ttu-id="94e8b-109">Azonban adnia hello típusa és neve a sablon a menübeállításoktól függően e beágyazott hello szülő erőforrás belül, vagy a saját hello felső szintjén.</span><span class="sxs-lookup"><span data-stu-id="94e8b-109">However, you specify hello type and name in a template differently based on whether it is nested within hello parent resource, or on its own at hello top level.</span></span> <span data-ttu-id="94e8b-110">Ez a témakör bemutatja, hogyan mindkét toohandle közelít.</span><span class="sxs-lookup"><span data-stu-id="94e8b-110">This topic shows how toohandle both approaches.</span></span>

<span data-ttu-id="94e8b-111">Egy teljesen minősített hivatkozás tooa erőforrás térített hello rendelés toocombine szegmensek hello típusból név pedig nem egyszerűen hello két összefűzése.</span><span class="sxs-lookup"><span data-stu-id="94e8b-111">When constructing a fully qualified reference tooa resource, hello order toocombine segments from hello type and name  is not simply a concatenation of hello two.</span></span>  <span data-ttu-id="94e8b-112">Hello névtér után inkább sorozatát *típusnév/* párok a legkevésbé egyedi toomost adott:</span><span class="sxs-lookup"><span data-stu-id="94e8b-112">Instead, after hello namespace, use a sequence of *type/name* pairs from least specific toomost specific:</span></span>

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

<span data-ttu-id="94e8b-113">Példa:</span><span class="sxs-lookup"><span data-stu-id="94e8b-113">For example:</span></span>

<span data-ttu-id="94e8b-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`megfelelő `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` helytelen</span><span class="sxs-lookup"><span data-stu-id="94e8b-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` is correct `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is not correct</span></span>

## <a name="nested-child-resource"></a><span data-ttu-id="94e8b-115">Beágyazott gyermektevékenységének erőforrás</span><span class="sxs-lookup"><span data-stu-id="94e8b-115">Nested child resource</span></span>
<span data-ttu-id="94e8b-116">hello legegyszerűbb módja toodefine gyermek erőforrás toonest rá hello szülő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="94e8b-116">hello easiest way toodefine a child resource is toonest it within hello parent resource.</span></span> <span data-ttu-id="94e8b-117">a következő példa azt mutatja meg, egy SQL Server ágyazva egy SQL-adatbázis hello.</span><span class="sxs-lookup"><span data-stu-id="94e8b-117">hello following example shows a SQL database nested within in a SQL Server.</span></span>

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  ...
  "resources": [
    {
      "name": "exampledatabase",
      "type": "databases",
      "apiVersion": "2014-04-01",
      ...
    }
  ]
}
```

<span data-ttu-id="94e8b-118">Hello gyermek erőforrás, akkor hello típusának beállítása túl`databases` , de a teljes erőforrás típusa `Microsoft.Sql/servers/databases`.</span><span class="sxs-lookup"><span data-stu-id="94e8b-118">For hello child resource, hello type is set too`databases` but its full resource type is `Microsoft.Sql/servers/databases`.</span></span> <span data-ttu-id="94e8b-119">Nincs megadva `Microsoft.Sql/servers/` mivel feltételezzük hello szülő erőforrás típusból.</span><span class="sxs-lookup"><span data-stu-id="94e8b-119">You do not provide `Microsoft.Sql/servers/` because it is assumed from hello parent resource type.</span></span> <span data-ttu-id="94e8b-120">hello gyermek-erőforrás neve túl van-e állítva`exampledatabase` , de hello teljes nevét hello szülő nevének tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="94e8b-120">hello child resource name is set too`exampledatabase` but hello full name includes hello parent name.</span></span> <span data-ttu-id="94e8b-121">Nincs megadva `exampleserver` mivel feltételezzük hello szülő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="94e8b-121">You do not provide `exampleserver` because it is assumed from hello parent resource.</span></span>

## <a name="top-level-child-resource"></a><span data-ttu-id="94e8b-122">Legfelső szintű gyermek-erőforrás</span><span class="sxs-lookup"><span data-stu-id="94e8b-122">Top-level child resource</span></span>
<span data-ttu-id="94e8b-123">Gyermek-erőforrás hello hello felső szintjén adhat meg.</span><span class="sxs-lookup"><span data-stu-id="94e8b-123">You can define hello child resource at hello top level.</span></span> <span data-ttu-id="94e8b-124">Előfordulhat, hogy ezt a módszert használja, ha az hello szülő erőforrás nem hello azonos sablont, vagy ha szeretné, hogy toouse `copy` toocreate több gyermek-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="94e8b-124">You might use this approach if hello parent resource is not deployed in hello same template, or if want toouse `copy` toocreate multiple child resources.</span></span> <span data-ttu-id="94e8b-125">Ezt a módszert hello teljes erőforrástípust biztosít, és tartalmazza a hello szülő erőforrás nevét az hello gyermek-erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="94e8b-125">With this approach, you must provide hello full resource type, and include hello parent resource name in hello child resource name.</span></span>

```json
{
  "name": "exampleserver",
  "type": "Microsoft.Sql/servers",
  "apiVersion": "2014-04-01",
  "resources": [ 
  ],
  ...
},
{
  "name": "exampleserver/exampledatabase",
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2014-04-01",
  ...
}
```

<span data-ttu-id="94e8b-126">hello adatbázisa egy gyermek-erőforrás toohello kiszolgáló annak ellenére, hogy az azonos szinten hello sablonban hello vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="94e8b-126">hello database is a child resource toohello server even though they are defined on hello same level in hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94e8b-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94e8b-127">Next steps</span></span>
* <span data-ttu-id="94e8b-128">A fenyegetés elhárítására vonatkozó javaslatokról toocreate sablonjainak használatáról [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="94e8b-128">For recommendations about how toocreate templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>
* <span data-ttu-id="94e8b-129">Például több gyermek erőforrások létrehozásának, [erőforrások az Azure Resource Manager sablonokban több példányának telepítése](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="94e8b-129">For an example of creating multiple child resources, see [Deploy multiple instances of resources in Azure Resource Manager templates](resource-group-create-multiple.md).</span></span>

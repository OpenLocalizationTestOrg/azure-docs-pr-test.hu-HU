---
title: "Gyermek-erőforrás meghatározásához az Azure-sablon alapján |} Microsoft Docs"
description: "Bemutatja, hogyan állítsa be az erőforrás típusát és a gyermek-erőforrás nevét az Azure Resource Manager-sablonok"
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
ms.openlocfilehash: 5b6ce5526f354008eb4a697deec737876f22391f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-name-and-type-for-child-resource-in-resource-manager-template"></a><span data-ttu-id="236bb-103">A Resource Manager-sablon nevét és típusát gyermek erőforrás beállítása</span><span class="sxs-lookup"><span data-stu-id="236bb-103">Set name and type for child resource in Resource Manager template</span></span>
<span data-ttu-id="236bb-104">Sablon létrehozásakor meg gyakran meg kell adnia a gyermek erőforrása, amely a szülő erőforrás kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="236bb-104">When creating a template, you frequently need to include a child resource that is related to a parent resource.</span></span> <span data-ttu-id="236bb-105">A sablon tartalmazhat például egy SQL server és adatbázis.</span><span class="sxs-lookup"><span data-stu-id="236bb-105">For example, your template may include a SQL server and a database.</span></span> <span data-ttu-id="236bb-106">Az SQL-kiszolgáló a szülő erőforrás, az adatbázis pedig a gyermek-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="236bb-106">The SQL server is the parent resource, and the database is the child resource.</span></span> 

<span data-ttu-id="236bb-107">A gyermek erőforrástípus formátuma:`{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span><span class="sxs-lookup"><span data-stu-id="236bb-107">The format of the child resource type is: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`</span></span>

<span data-ttu-id="236bb-108">A gyermek-erőforrás neve formátuma:`{parent-resource-name}/{child-resource-name}`</span><span class="sxs-lookup"><span data-stu-id="236bb-108">The format of the child resource name is: `{parent-resource-name}/{child-resource-name}`</span></span>

<span data-ttu-id="236bb-109">Azonban adnia típusa és neve menübeállításoktól függően e beágyazott a szülő erőforrás belül, vagy a saját legfelső szintjén sablonban.</span><span class="sxs-lookup"><span data-stu-id="236bb-109">However, you specify the type and name in a template differently based on whether it is nested within the parent resource, or on its own at the top level.</span></span> <span data-ttu-id="236bb-110">Ez a témakör bemutatja, hogyan legyen kezelve mindkét megközelítés.</span><span class="sxs-lookup"><span data-stu-id="236bb-110">This topic shows how to handle both approaches.</span></span>

<span data-ttu-id="236bb-111">Amikor egy erőforrást egy teljesen minősített hivatkozást hozhat létre, típusa és neve a szegmensek egyesítése sorrendje nem egyszerűen a két összefűzése.</span><span class="sxs-lookup"><span data-stu-id="236bb-111">When constructing a fully qualified reference to a resource, the order to combine segments from the type and name  is not simply a concatenation of the two.</span></span>  <span data-ttu-id="236bb-112">A névtér után inkább sorozatát *típusnév/* a legjobban megfelel a legkevésbé egyedi párok:</span><span class="sxs-lookup"><span data-stu-id="236bb-112">Instead, after the namespace, use a sequence of *type/name* pairs from least specific to most specific:</span></span>

```json
{resource-provider-namespace}/{parent-resource-type}/{parent-resource-name}[/{child-resource-type}/{child-resource-name}]*
```

<span data-ttu-id="236bb-113">Példa:</span><span class="sxs-lookup"><span data-stu-id="236bb-113">For example:</span></span>

<span data-ttu-id="236bb-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt`megfelelő `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` helytelen</span><span class="sxs-lookup"><span data-stu-id="236bb-114">`Microsoft.Compute/virtualMachines/myVM/extensions/myExt` is correct `Microsoft.Compute/virtualMachines/extensions/myVM/myExt` is not correct</span></span>

## <a name="nested-child-resource"></a><span data-ttu-id="236bb-115">Beágyazott gyermektevékenységének erőforrás</span><span class="sxs-lookup"><span data-stu-id="236bb-115">Nested child resource</span></span>
<span data-ttu-id="236bb-116">A legegyszerűbben úgy adja meg a gyermek-erőforrás beágyazásához azt a szülő erőforrás belül.</span><span class="sxs-lookup"><span data-stu-id="236bb-116">The easiest way to define a child resource is to nest it within the parent resource.</span></span> <span data-ttu-id="236bb-117">A következő példa bemutatja egy olyan SQL Server ágyazott SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="236bb-117">The following example shows a SQL database nested within in a SQL Server.</span></span>

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

<span data-ttu-id="236bb-118">A gyermek-erőforrás, a típusuk értéke `databases` , de a teljes erőforrás típusa `Microsoft.Sql/servers/databases`.</span><span class="sxs-lookup"><span data-stu-id="236bb-118">For the child resource, the type is set to `databases` but its full resource type is `Microsoft.Sql/servers/databases`.</span></span> <span data-ttu-id="236bb-119">Nincs megadva `Microsoft.Sql/servers/` mivel feltételezzük, hogy a szülő erőforrás típusból.</span><span class="sxs-lookup"><span data-stu-id="236bb-119">You do not provide `Microsoft.Sql/servers/` because it is assumed from the parent resource type.</span></span> <span data-ttu-id="236bb-120">A gyermek-erőforrás neve legyen `exampledatabase` , de a teljes nevet tartalmazza a szülő nevének.</span><span class="sxs-lookup"><span data-stu-id="236bb-120">The child resource name is set to `exampledatabase` but the full name includes the parent name.</span></span> <span data-ttu-id="236bb-121">Nincs megadva `exampleserver` mivel feltételezzük, hogy a szülő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="236bb-121">You do not provide `exampleserver` because it is assumed from the parent resource.</span></span>

## <a name="top-level-child-resource"></a><span data-ttu-id="236bb-122">Legfelső szintű gyermek-erőforrás</span><span class="sxs-lookup"><span data-stu-id="236bb-122">Top-level child resource</span></span>
<span data-ttu-id="236bb-123">Megadhatja, hogy a gyermek-erőforrás legfelső szintjén.</span><span class="sxs-lookup"><span data-stu-id="236bb-123">You can define the child resource at the top level.</span></span> <span data-ttu-id="236bb-124">Előfordulhat, hogy ezt a módszert használja, ha az a szülő erőforrás nem ugyanazt a sablont, vagy ha szeretné használni `copy` több gyermek-erőforrás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="236bb-124">You might use this approach if the parent resource is not deployed in the same template, or if want to use `copy` to create multiple child resources.</span></span> <span data-ttu-id="236bb-125">Ezt a módszert használja adja meg a teljes erőforrás típusát, és a szülő erőforrás neve tartalmazza a gyermek-erőforrás neve.</span><span class="sxs-lookup"><span data-stu-id="236bb-125">With this approach, you must provide the full resource type, and include the parent resource name in the child resource name.</span></span>

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

<span data-ttu-id="236bb-126">Az adatbázis egy gyermek-erőforrás a kiszolgálóra, annak ellenére, hogy a sablonban ugyanazon a szinten vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="236bb-126">The database is a child resource to the server even though they are defined on the same level in the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="236bb-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="236bb-127">Next steps</span></span>
* <span data-ttu-id="236bb-128">Sablonok létrehozásával kapcsolatos ajánlások, lásd: [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](resource-manager-template-best-practices.md).</span><span class="sxs-lookup"><span data-stu-id="236bb-128">For recommendations about how to create templates, see [Best practices for creating Azure Resource Manager templates](resource-manager-template-best-practices.md).</span></span>
* <span data-ttu-id="236bb-129">Például több gyermek erőforrások létrehozásának, [erőforrások az Azure Resource Manager sablonokban több példányának telepítése](resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="236bb-129">For an example of creating multiple child resources, see [Deploy multiple instances of resources in Azure Resource Manager templates](resource-group-create-multiple.md).</span></span>

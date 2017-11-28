---
title: "egy Azure aaaDelete fürt és az erőforrások |} Microsoft Docs"
description: "Ismerje meg, hogyan toocompletely delete a Service Fabric-fürt vagy törlése hello tartalmazó csoport hello fürt, vagy szelektív módon törli az erőforrásokat."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 5c15a4184644da715cd69397f2150de86ab433ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-hello-resources-it-uses"></a><span data-ttu-id="f3498-103">Az Azure és a hello erőforrás használja a Service Fabric-fürt törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-103">Delete a Service Fabric cluster on Azure and hello resources it uses</span></span>
<span data-ttu-id="f3498-104">A Service Fabric-fürt épül fel sok más Azure-erőforrások továbbá toohello fürterőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="f3498-104">A Service Fabric cluster is made up of many other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="f3498-105">Így toocompletely a Service Fabric-fürt törlése szükség toodelete összes hello történik, az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f3498-105">So toocompletely delete a Service Fabric cluster you also need toodelete all hello resources it is made of.</span></span>
<span data-ttu-id="f3498-106">Két lehetőség közül választhat: vagy delete hello erőforráscsoport, amely a fürt hello van (amely törli a hello fürterőforrás és más erőforrások hello erőforráscsoportban) vagy a kifejezetten a hello fürterőforrás törlése és a hozzá társított erőforrásokat (de nem más erőforrások az erőforráscsoportban hello).</span><span class="sxs-lookup"><span data-stu-id="f3498-106">You have two options: Either delete hello resource group that hello cluster is in (which deletes hello cluster resource and any other resources in hello resource group) or specifically delete hello cluster resource and it's associated resources (but not other resources in hello resource group).</span></span>

> [!NOTE]
> <span data-ttu-id="f3498-107">Hello fürterőforrás törlése **nem** összes hello más erőforrások, amelyek tevődnek össze a Service Fabric-fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="f3498-107">Deleting hello cluster resource **does not** delete all hello other resources that your Service Fabric cluster is composed of.</span></span>
> 
> 

## <a name="delete-hello-entire-resource-group-rg-that-hello-service-fabric-cluster-is-in"></a><span data-ttu-id="f3498-108">Hello teljes erőforráscsoport (RG), amely a Service Fabric lévő hello törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-108">Delete hello entire resource group (RG) that hello Service Fabric cluster is in</span></span>
<span data-ttu-id="f3498-109">Ez a hello, hogy törli a fürtöt, vele együtt hello erőforráscsoporthoz társított összes hello erőforrás legegyszerűbb módja tooensure.</span><span class="sxs-lookup"><span data-stu-id="f3498-109">This is hello easiest way tooensure that you delete all hello resources associated with your cluster, including hello resource group.</span></span> <span data-ttu-id="f3498-110">Hello erőforráscsoport PowerShell használatával törölheti vagy hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="f3498-110">You can delete hello resource group using PowerShell or through hello Azure portal.</span></span> <span data-ttu-id="f3498-111">Ha az erőforráscsoport erőforráshoz kapcsolódó tooService fabric-fürt, majd törölheti egy adott erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="f3498-111">If your resource group has resources that are not related tooService fabric cluster, then you can delete specific resources.</span></span>

### <a name="delete-hello-resource-group-using-azure-powershell"></a><span data-ttu-id="f3498-112">Az Azure PowerShell hello erőforráscsoport törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-112">Delete hello resource group using Azure PowerShell</span></span>
<span data-ttu-id="f3498-113">Hello erőforráscsoport hello Azure PowerShell-parancsmagok a következő futtatásával is törli.</span><span class="sxs-lookup"><span data-stu-id="f3498-113">You can also delete hello resource group by running hello following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="f3498-114">Győződjön meg arról, hogy Azure PowerShell 1.0 vagy nagyobb telepítve van a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f3498-114">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="f3498-115">Ha nem ezt megelőzően, lépésekkel hello leírt [hogyan tooinstall és az Azure PowerShell konfigurálása.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="f3498-115">If you have not done this before, follow hello steps outlined in [How tooinstall and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="f3498-116">Nyisson meg egy PowerShell-ablakot, és futtassa a következő PS parancsmagok hello:</span><span class="sxs-lookup"><span data-stu-id="f3498-116">Open a PowerShell window and run hello following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

<span data-ttu-id="f3498-117">A kérdés tooconfirm hello törlés fog kapni, ha nem használja hello *-Force* lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f3498-117">You will get a prompt tooconfirm hello deletion if you did not use hello *-Force* option.</span></span> <span data-ttu-id="f3498-118">A megerősítő hello RG, és a benne található összes hello erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="f3498-118">On confirmation hello RG and all hello resources it contains are deleted.</span></span>

### <a name="delete-a-resource-group-in-hello-azure-portal"></a><span data-ttu-id="f3498-119">Az Azure-portálon hello erőforrás csoport törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-119">Delete a resource group in hello Azure portal</span></span>
1. <span data-ttu-id="f3498-120">Bejelentkezési toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f3498-120">Login toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f3498-121">Keresse meg a kívánt toodelete toohello Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="f3498-121">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
3. <span data-ttu-id="f3498-122">Kattintson a hello hello fürt essentials oldalon erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="f3498-122">Click on hello Resource Group name on hello cluster essentials page.</span></span>
4. <span data-ttu-id="f3498-123">Ekkor megjelenik a hello **erőforrás csoport Essentials** lap.</span><span class="sxs-lookup"><span data-stu-id="f3498-123">This brings up hello **Resource Group Essentials** page.</span></span>
5. <span data-ttu-id="f3498-124">Kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="f3498-124">Click **Delete**.</span></span>
6. <span data-ttu-id="f3498-125">Hello utasításokat kövesse, hogy hello erőforráscsoport lap toocomplete hello törlése.</span><span class="sxs-lookup"><span data-stu-id="f3498-125">Follow hello instructions on that page toocomplete hello deletion of hello resource group.</span></span>

![Erőforrás-csoport törlése][ResourceGroupDelete]

## <a name="delete-hello-cluster-resource-and-hello-resources-it-uses-but-not-other-resources-in-hello-resource-group"></a><span data-ttu-id="f3498-127">Hello fürterőforrás és hello erőforrásokat használ, de nem hello erőforráscsoport egyéb erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-127">Delete hello cluster resource and hello resources it uses, but not other resources in hello resource group</span></span>
<span data-ttu-id="f3498-128">Ha az erőforráscsoport csak olyan erőforrásokat, amelyek a Service Fabric-fürt kapcsolódó toohello toodelete szeretne, majd könnyebb toodelete hello teljes erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="f3498-128">If your resource group has only resources that are related toohello Service Fabric cluster you want toodelete, then it is easier toodelete hello entire resource group.</span></span> <span data-ttu-id="f3498-129">Ha tooselectively delete hello erőforrások egyenként az erőforráscsoportban, majd kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f3498-129">If you want tooselectively delete hello resources one-by-one in your resource group, then follow these steps.</span></span>

<span data-ttu-id="f3498-130">Ha telepítette a fürt hello portál használatával, vagy valamelyik hello Service Fabric Resource Manager-sablonok segítségével hello sablon gyűjteményből, fürt által használt hello hello erőforrások címkéjű vannak a következő két címkék hello.</span><span class="sxs-lookup"><span data-stu-id="f3498-130">If you deployed your cluster using hello portal or using one of hello Service Fabric Resource Manager templates from hello template gallery, then all hello resources that hello cluster uses are tagged with hello following two tags.</span></span> <span data-ttu-id="f3498-131">Használhatja őket erőforrások kívánt toodecide toodelete.</span><span class="sxs-lookup"><span data-stu-id="f3498-131">You can use them toodecide which resources you want toodelete.</span></span>

<span data-ttu-id="f3498-132">***1. tag:*** kulcs = fürtnév, érték = "hello fürt neve"</span><span class="sxs-lookup"><span data-stu-id="f3498-132">***Tag#1:*** Key = clusterName, Value = 'name of hello cluster'</span></span>

<span data-ttu-id="f3498-133">***2. tag:*** kulcsot resourceName, érték = = ServiceFabric</span><span class="sxs-lookup"><span data-stu-id="f3498-133">***Tag#2:*** Key = resourceName, Value = ServiceFabric</span></span>

### <a name="delete-specific-resources-in-hello-azure-portal"></a><span data-ttu-id="f3498-134">Egy adott erőforráshoz, az Azure-portálon hello törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-134">Delete specific resources in hello Azure portal</span></span>
1. <span data-ttu-id="f3498-135">Bejelentkezési toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f3498-135">Login toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f3498-136">Keresse meg a kívánt toodelete toohello Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="f3498-136">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
3. <span data-ttu-id="f3498-137">Nyissa meg túl**összes beállítás** hello essentials panelen.</span><span class="sxs-lookup"><span data-stu-id="f3498-137">Go too**All settings** on hello essentials blade.</span></span>
4. <span data-ttu-id="f3498-138">Kattintson a **címkék** alatt **erőforrás-kezelés** hello-beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="f3498-138">Click on **Tags** under **Resource Management** in hello settings blade.</span></span>
5. <span data-ttu-id="f3498-139">Kattintson az egyik hello **címkék** a hello Címkék panel tooget az adott címkével ellátott összes hello erőforrások listáját.</span><span class="sxs-lookup"><span data-stu-id="f3498-139">Click on one of hello **Tags** in hello tags blade tooget a list of all hello resources with that tag.</span></span>
   
    ![Erőforráscímkék][ResourceTags]
6. <span data-ttu-id="f3498-141">Miután címkézett erőforrások hello listája, kattintson a hello erőforrások mindegyikének, és törölje őket.</span><span class="sxs-lookup"><span data-stu-id="f3498-141">Once you have hello list of tagged resources, click on each of hello resources and delete them.</span></span>
   
    ![Címkézett erőforrások][TaggedResources]

### <a name="delete-hello-resources-using-azure-powershell"></a><span data-ttu-id="f3498-143">Az Azure PowerShell hello erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="f3498-143">Delete hello resources using Azure PowerShell</span></span>
<span data-ttu-id="f3498-144">Hello erőforrások egyenként-hello Azure PowerShell-parancsmagok a következő futtatásával törölheti.</span><span class="sxs-lookup"><span data-stu-id="f3498-144">You can delete hello resources one-by-one by running hello following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="f3498-145">Győződjön meg arról, hogy Azure PowerShell 1.0 vagy nagyobb telepítve van a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f3498-145">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="f3498-146">Ha nem ezt megelőzően, lépésekkel hello leírt [hogyan tooinstall és az Azure PowerShell konfigurálása.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="f3498-146">If you have not done this before, follow hello steps outlined in [How tooinstall and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="f3498-147">Nyisson meg egy PowerShell-ablakot, és futtassa a következő PS parancsmagok hello:</span><span class="sxs-lookup"><span data-stu-id="f3498-147">Open a PowerShell window and run hello following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="f3498-148">Az egyes hello erőforrások szeretné, hogy toodelete, futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f3498-148">For each of hello resources you want toodelete, run hello following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of hello resource group>" -Force
```

<span data-ttu-id="f3498-149">toodelete hello fürterőforrás, futtassa a következő hello:</span><span class="sxs-lookup"><span data-stu-id="f3498-149">toodelete hello cluster resource, run hello following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of hello resource group>" -Force
```

## <a name="next-steps"></a><span data-ttu-id="f3498-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3498-150">Next steps</span></span>
<span data-ttu-id="f3498-151">A következő tooalso olvasási hello fürt frissítése és a szolgáltatások particionálás megismerése:</span><span class="sxs-lookup"><span data-stu-id="f3498-151">Read hello following tooalso learn about upgrading a cluster and partitioning services:</span></span>

* [<span data-ttu-id="f3498-152">További tudnivalók a fürt frissítése</span><span class="sxs-lookup"><span data-stu-id="f3498-152">Learn about cluster upgrades</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="f3498-153">További tudnivalók a maximális méret állapotalapú szolgáltatások particionálás</span><span class="sxs-lookup"><span data-stu-id="f3498-153">Learn about partitioning stateful services for maximum scale</span></span>](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG

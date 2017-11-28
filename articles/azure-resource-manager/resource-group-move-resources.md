---
title: "Azure-erőforrások toonew előfizetés vagy az erőforrás csoport aaaMove |} Microsoft Docs"
description: "Használja az Azure Resource Manager toomove erőforrások tooa új erőforráscsoportba vagy előfizetésbe."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: tomfitz
ms.openlocfilehash: 09d35f0afbbcdc0c66779f98a982d878f0807497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-resources-toonew-resource-group-or-subscription"></a><span data-ttu-id="62ac8-103">Helyezze át az erőforrásokat toonew erőforráscsoportba vagy előfizetésbe</span><span class="sxs-lookup"><span data-stu-id="62ac8-103">Move resources toonew resource group or subscription</span></span>
<span data-ttu-id="62ac8-104">Ez a témakör bemutatja, hogyan toomove erőforrások tooeither egy új előfizetés vagy új erőforrás csoport hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="62ac8-104">This topic shows you how toomove resources tooeither a new subscription or a new resource group in hello same subscription.</span></span> <span data-ttu-id="62ac8-105">Hello portál, a PowerShell, a Azure CLI vagy a REST API toomove erőforrás hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="62ac8-105">You can use hello portal, PowerShell, Azure CLI, or hello REST API toomove resource.</span></span> <span data-ttu-id="62ac8-106">hello áthelyezési műveletek ebben a témakörben elérhető tooyou bármely Azure támogatási segítsége nélkül.</span><span class="sxs-lookup"><span data-stu-id="62ac8-106">hello move operations in this topic are available tooyou without any assistance from Azure support.</span></span>

<span data-ttu-id="62ac8-107">Erőforrások áthelyezésekor hello forrás csoport és a célcsoport hello zároltak hello művelet során.</span><span class="sxs-lookup"><span data-stu-id="62ac8-107">When moving resources, both hello source group and hello target group are locked during hello operation.</span></span> <span data-ttu-id="62ac8-108">Írási és törlési műveletek blokkolják hello erőforráscsoportok hello áthelyezés befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="62ac8-108">Write and delete operations are blocked on hello resource groups until hello move completes.</span></span> <span data-ttu-id="62ac8-109">A lakat azt jelenti, nem lehet hozzáadni, frissítenie vagy törölnie hello erőforráscsoportok erőforrásokat, de nem jelent hello erőforrások sincs rögzítve.</span><span class="sxs-lookup"><span data-stu-id="62ac8-109">This lock means you cannot add, update, or delete resources in hello resource groups, but it does not mean hello resources are frozen.</span></span> <span data-ttu-id="62ac8-110">Például ha egy SQL Server és az adatbázis tooa új erőforráscsoportot, a hello adatbázis használó alkalmazások teljesen állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="62ac8-110">For example, if you move a SQL Server and its database tooa new resource group, an application that uses hello database experiences no downtime.</span></span> <span data-ttu-id="62ac8-111">Azt is olvashat és írhat toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="62ac8-111">It can still read and write toohello database.</span></span>

<span data-ttu-id="62ac8-112">Hello hello erőforrás helye nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="62ac8-112">You cannot change hello location of hello resource.</span></span> <span data-ttu-id="62ac8-113">Egy erőforrás áthelyezése csak áthelyezi azt tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-113">Moving a resource only moves it tooa new resource group.</span></span> <span data-ttu-id="62ac8-114">Új erőforráscsoport hello rendelkezhet egy másik helyre, de nem változtatja meg, amely hello hello erőforrás helye.</span><span class="sxs-lookup"><span data-stu-id="62ac8-114">hello new resource group may have a different location, but that does not change hello location of hello resource.</span></span>

> [!NOTE]
> <span data-ttu-id="62ac8-115">Ez a cikk ismerteti, hogyan toomove erőforrásokat egy meglévő Azure fiók frissítést.</span><span class="sxs-lookup"><span data-stu-id="62ac8-115">This article describes how toomove resources within an existing Azure account offering.</span></span> <span data-ttu-id="62ac8-116">Az Azure-fiókjával (például frissítése használatalapú fizetéses toopre-fizetési) során a meglévő erőforrásokkal folytatott toowork Folytatás ajánlat lásd ténylegesen kívánja-e toochange [az Azure-előfizetés tooanother ajánlatot váltani](../billing/billing-how-to-switch-azure-offer.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-116">If you actually want toochange your Azure account offering (such as upgrading from pay-as-you-go toopre-pay) while continuing toowork with your existing resources, see [Switch your Azure subscription tooanother offer](../billing/billing-how-to-switch-azure-offer.md).</span></span>
>
>

## <a name="checklist-before-moving-resources"></a><span data-ttu-id="62ac8-117">Erőforrások áthelyezése előtt ellenőrzőlista</span><span class="sxs-lookup"><span data-stu-id="62ac8-117">Checklist before moving resources</span></span>
<span data-ttu-id="62ac8-118">Nincsenek néhány fontos lépések az tooperform erőforrás áthelyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="62ac8-118">There are some important steps tooperform before moving a resource.</span></span> <span data-ttu-id="62ac8-119">Ezen feltételek ellenőrzésével a hibák elkerülhetőek.</span><span class="sxs-lookup"><span data-stu-id="62ac8-119">By verifying these conditions, you can avoid errors.</span></span>

1. <span data-ttu-id="62ac8-120">hello forrás és cél előfizetések léteznie kell hello belül azonos [Azure Active Directory-bérlő](../active-directory/active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-120">hello source and destination subscriptions must exist within hello same [Azure Active Directory tenant](../active-directory/active-directory-howto-tenant.md).</span></span> <span data-ttu-id="62ac8-121">hogy mindkét előfizetéshez rendelkezik hello azonos toocheck bérlői azonosító, az Azure PowerShell vagy Azure CLI használata.</span><span class="sxs-lookup"><span data-stu-id="62ac8-121">toocheck that both subscriptions have hello same tenant ID, use Azure PowerShell or Azure CLI.</span></span>

  <span data-ttu-id="62ac8-122">Azure PowerShell használata:</span><span class="sxs-lookup"><span data-stu-id="62ac8-122">For Azure PowerShell, use:</span></span>

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  <span data-ttu-id="62ac8-123">Az Azure CLI 2.0 használja:</span><span class="sxs-lookup"><span data-stu-id="62ac8-123">For Azure CLI 2.0, use:</span></span>

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  <span data-ttu-id="62ac8-124">Ha hello bérlői azonosítóit hello forrás és cél előfizetések nincsenek hello azonos, megkísérelheti toochange hello directory hello az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="62ac8-124">If hello tenant IDs for hello source and destination subscriptions are not hello same, you can attempt toochange hello directory for hello subscription.</span></span> <span data-ttu-id="62ac8-125">Ez a beállítás azonban csak a rendelkezésre álló tooService rendszergazdák számára van bejelentkezve, akkor a Microsoft-fiók (nem lehet szervezeti fiókkal).</span><span class="sxs-lookup"><span data-stu-id="62ac8-125">However, this option is only available tooService Administrators who are signed in with a Microsoft account (not an organizational account).</span></span> <span data-ttu-id="62ac8-126">hello directory, a toohello napló módosítása tooattempt [klasszikus portál](https://manage.windowsazure.com/), válassza ki **beállítások**, és válassza ki a hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="62ac8-126">tooattempt changing hello directory, log in toohello [classic portal](https://manage.windowsazure.com/), and select **Settings**, and select hello subscription.</span></span> <span data-ttu-id="62ac8-127">Ha hello **könyvtár szerkesztése** ikon érhető el, válassza ki azt a toochange hello társított Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="62ac8-127">If hello **Edit Directory** icon is available, select it toochange hello associated Azure Active Directory.</span></span>

  ![címtár szerkesztése](./media/resource-group-move-resources/edit-directory.png)

  <span data-ttu-id="62ac8-129">Ha erre az ikonra nem érhető el, forduljon támogatási toomove hello erőforrások tooa új bérlőt.</span><span class="sxs-lookup"><span data-stu-id="62ac8-129">If that icon is not available, you must contact support toomove hello resources tooa new tenant.</span></span>

2. <span data-ttu-id="62ac8-130">hello szolgáltatást engedélyezni kell a hello képességét toomove erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="62ac8-130">hello service must enable hello ability toomove resources.</span></span> <span data-ttu-id="62ac8-131">Ez a témakör felsorolja a szolgáltatások lehetővé teszik az erőforrások áthelyezése, és a szolgáltatások nem engedélyezi az erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="62ac8-131">This topic lists which services enable moving resources and which services do not enable moving resources.</span></span>
3. <span data-ttu-id="62ac8-132">hello célelőfizetés mozgatásának hello erőforrás hello erőforrás-szolgáltató regisztrálva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="62ac8-132">hello destination subscription must be registered for hello resource provider of hello resource being moved.</span></span> <span data-ttu-id="62ac8-133">Ha nem, hibaüzenetet kap, egy adott hello feltüntetve **az előfizetés nincs regisztrálva az erőforrástípus**.</span><span class="sxs-lookup"><span data-stu-id="62ac8-133">If not, you receive an error stating that hello **subscription is not registered for a resource type**.</span></span> <span data-ttu-id="62ac8-134">Előforduló probléma erőforrás tooa új előfizetés áthelyezése, de az adott előfizetéshez még nem használta az adott erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="62ac8-134">You might encounter this problem when moving a resource tooa new subscription, but that subscription has never been used with that resource type.</span></span> <span data-ttu-id="62ac8-135">toolearn hogyan toocheck hello regisztrációs állapotát, és regisztrálni az erőforrás-szolgáltató, lásd: [erőforrás-szolgáltatók és típusok](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-135">toolearn how toocheck hello registration status and register resource providers, see [Resource providers and types](resource-manager-supported-services.md).</span></span>

## <a name="when-toocall-support"></a><span data-ttu-id="62ac8-136">Ha toocall támogatja</span><span class="sxs-lookup"><span data-stu-id="62ac8-136">When toocall support</span></span>
<span data-ttu-id="62ac8-137">Áthelyezheti a legtöbb erőforrást hello önkiszolgáló műveletek ebben a témakörben látható révén.</span><span class="sxs-lookup"><span data-stu-id="62ac8-137">You can move most resources through hello self-service operations shown in this topic.</span></span> <span data-ttu-id="62ac8-138">Hello önkiszolgáló műveletek használata:</span><span class="sxs-lookup"><span data-stu-id="62ac8-138">Use hello self-service operations to:</span></span>

* <span data-ttu-id="62ac8-139">Resource Manager-erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="62ac8-139">Move Resource Manager resources.</span></span>
* <span data-ttu-id="62ac8-140">Toohello szerint hagyományos erőforrás áthelyezése [klasszikus üzembe helyezési korlátozások](#classic-deployment-limitations).</span><span class="sxs-lookup"><span data-stu-id="62ac8-140">Move classic resources according toohello [classic deployment limitations](#classic-deployment-limitations).</span></span>

<span data-ttu-id="62ac8-141">Hívja a támogatást, ha szeretné:</span><span class="sxs-lookup"><span data-stu-id="62ac8-141">Call support when you need to:</span></span>

* <span data-ttu-id="62ac8-142">Helyezze át a erőforrások tooa új Azure-fiók (és az Azure Active Directory-bérlő).</span><span class="sxs-lookup"><span data-stu-id="62ac8-142">Move your resources tooa new Azure account (and Azure Active Directory tenant).</span></span>
* <span data-ttu-id="62ac8-143">Hagyományos erőforrások áthelyezéséhez, de tapasztal hello korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="62ac8-143">Move classic resources but are having trouble with hello limitations.</span></span>

## <a name="services-that-enable-move"></a><span data-ttu-id="62ac8-144">Szolgáltatások, amelyek lehetővé teszik a áthelyezése</span><span class="sxs-lookup"><span data-stu-id="62ac8-144">Services that enable move</span></span>
<span data-ttu-id="62ac8-145">Most, amelyek lehetővé teszik a mozgóátlag tooboth egy új erőforráscsoportot és az előfizetés hello szolgáltatások a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-145">For now, hello services that enable moving tooboth a new resource group and subscription are:</span></span>

* <span data-ttu-id="62ac8-146">API Management</span><span class="sxs-lookup"><span data-stu-id="62ac8-146">API Management</span></span>
* <span data-ttu-id="62ac8-147">App Service apps (webalkalmazások) – lásd: [App Service korlátozásai](#app-service-limitations)</span><span class="sxs-lookup"><span data-stu-id="62ac8-147">App Service apps (web apps) - see [App Service limitations](#app-service-limitations)</span></span>
* <span data-ttu-id="62ac8-148">Application Insights</span><span class="sxs-lookup"><span data-stu-id="62ac8-148">Application Insights</span></span>
* <span data-ttu-id="62ac8-149">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="62ac8-149">Automation</span></span>
* <span data-ttu-id="62ac8-150">Batch</span><span class="sxs-lookup"><span data-stu-id="62ac8-150">Batch</span></span>
* <span data-ttu-id="62ac8-151">A Bing Maps</span><span class="sxs-lookup"><span data-stu-id="62ac8-151">Bing Maps</span></span>
* <span data-ttu-id="62ac8-152">Tartalomkézbesítési hálózat (CDN)</span><span class="sxs-lookup"><span data-stu-id="62ac8-152">CDN</span></span>
* <span data-ttu-id="62ac8-153">A felhőalapú szolgáltatások – lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="62ac8-153">Cloud Services - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="62ac8-154">Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="62ac8-154">Cognitive Services</span></span>
* <span data-ttu-id="62ac8-155">Tartalommoderátor</span><span class="sxs-lookup"><span data-stu-id="62ac8-155">Content Moderator</span></span>
* <span data-ttu-id="62ac8-156">Data Catalog</span><span class="sxs-lookup"><span data-stu-id="62ac8-156">Data Catalog</span></span>
* <span data-ttu-id="62ac8-157">Data Factory</span><span class="sxs-lookup"><span data-stu-id="62ac8-157">Data Factory</span></span>
* <span data-ttu-id="62ac8-158">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="62ac8-158">Data Lake Analytics</span></span>
* <span data-ttu-id="62ac8-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="62ac8-159">Data Lake Store</span></span>
* <span data-ttu-id="62ac8-160">DNS</span><span class="sxs-lookup"><span data-stu-id="62ac8-160">DNS</span></span>
* <span data-ttu-id="62ac8-161">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="62ac8-161">Azure Cosmos DB</span></span>
* <span data-ttu-id="62ac8-162">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="62ac8-162">Event Hubs</span></span>
* <span data-ttu-id="62ac8-163">A HDInsight-fürtök - Lásd [HDInsight korlátozásai](#hdinsight-limitations)</span><span class="sxs-lookup"><span data-stu-id="62ac8-163">HDInsight clusters - see [HDInsight limitations](#hdinsight-limitations)</span></span>
* <span data-ttu-id="62ac8-164">IoT-központok</span><span class="sxs-lookup"><span data-stu-id="62ac8-164">IoT Hubs</span></span>
* <span data-ttu-id="62ac8-165">Key Vault</span><span class="sxs-lookup"><span data-stu-id="62ac8-165">Key Vault</span></span>
* <span data-ttu-id="62ac8-166">Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="62ac8-166">Load Balancers</span></span>
* <span data-ttu-id="62ac8-167">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="62ac8-167">Logic Apps</span></span>
* <span data-ttu-id="62ac8-168">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="62ac8-168">Machine Learning</span></span>
* <span data-ttu-id="62ac8-169">Media Services</span><span class="sxs-lookup"><span data-stu-id="62ac8-169">Media Services</span></span>
* <span data-ttu-id="62ac8-170">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="62ac8-170">Mobile Engagement</span></span>
* <span data-ttu-id="62ac8-171">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="62ac8-171">Notification Hubs</span></span>
* <span data-ttu-id="62ac8-172">Operational Insights</span><span class="sxs-lookup"><span data-stu-id="62ac8-172">Operational Insights</span></span>
* <span data-ttu-id="62ac8-173">Operations Management</span><span class="sxs-lookup"><span data-stu-id="62ac8-173">Operations Management</span></span>
* <span data-ttu-id="62ac8-174">Power BI</span><span class="sxs-lookup"><span data-stu-id="62ac8-174">Power BI</span></span>
* <span data-ttu-id="62ac8-175">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="62ac8-175">Redis Cache</span></span>
* <span data-ttu-id="62ac8-176">Scheduler</span><span class="sxs-lookup"><span data-stu-id="62ac8-176">Scheduler</span></span>
* <span data-ttu-id="62ac8-177">Keresés</span><span class="sxs-lookup"><span data-stu-id="62ac8-177">Search</span></span>
* <span data-ttu-id="62ac8-178">Kiszolgálófelügyelet</span><span class="sxs-lookup"><span data-stu-id="62ac8-178">Server Management</span></span>
* <span data-ttu-id="62ac8-179">Service Bus</span><span class="sxs-lookup"><span data-stu-id="62ac8-179">Service Bus</span></span>
* <span data-ttu-id="62ac8-180">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="62ac8-180">Service Fabric</span></span>
* <span data-ttu-id="62ac8-181">Storage</span><span class="sxs-lookup"><span data-stu-id="62ac8-181">Storage</span></span>
* <span data-ttu-id="62ac8-182">Tekintse meg a tároló (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="62ac8-182">Storage (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="62ac8-183">A Stream Analytics - feladatok nem helyezhető át, ha a Stream Analytics állapotban.</span><span class="sxs-lookup"><span data-stu-id="62ac8-183">Stream Analytics - Stream Analytics jobs cannot be moved when in running state.</span></span>
* <span data-ttu-id="62ac8-184">SQL-adatbáziskiszolgáló - hello adatbázis és a kiszolgáló kell lennie, hello ugyanabban az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="62ac8-184">SQL Database server - hello database and server must reside in hello same resource group.</span></span> <span data-ttu-id="62ac8-185">Ha egy SQL server helyezi át, az adatbázisokat is kerülnek.</span><span class="sxs-lookup"><span data-stu-id="62ac8-185">When you move a SQL server, all its databases are also moved.</span></span>
* <span data-ttu-id="62ac8-186">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="62ac8-186">Traffic Manager</span></span>
* <span data-ttu-id="62ac8-187">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="62ac8-187">Virtual Machines</span></span>
* <span data-ttu-id="62ac8-188">A virtuális gépek a Key Vault - áthelyezési toonew erőforráscsoport ugyanahhoz az előfizetéshez tárolt tanúsítvány engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="62ac8-188">Virtual Machines with certificate stored in Key Vault - Move toonew resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="62ac8-189">Virtuális gépek (klasszikus) - lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="62ac8-189">Virtual Machines (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="62ac8-190">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="62ac8-190">Virtual Machine Scale Sets</span></span>
* <span data-ttu-id="62ac8-191">Virtuális hálózatok - jelenleg, peered virtuális hálózat nem helyezhető át, amíg a Vnetben társviszony-létesítés le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="62ac8-191">Virtual Networks - Currently, a peered Virtual Network cannot be moved until VNet peering has been disabled.</span></span> <span data-ttu-id="62ac8-192">Ha le van tiltva, virtuális hálózati hello sikeresen áthelyezhető és hello Vnetben társviszony-létesítés engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="62ac8-192">Once disabled, hello Virtual Network can be moved successfully and hello VNet peering can be enabled.</span></span> <span data-ttu-id="62ac8-193">Emellett egy virtuális hálózatot nem lehet áthelyezett tooa másik előfizetést, ha hello virtuális hálózat egyetlen alhálózatának sem az erőforrás-navigációs hivatkozásokkal tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="62ac8-193">In addition, a Virtual Network cannot be moved tooa different subscription if hello Virtual Network contains any subnet with resource navigation links.</span></span> <span data-ttu-id="62ac8-194">Például a virtuális hálózati alhálózat egy erőforrás-navigációs hivatkozás rendelkezik Microsoft.Cache redis erőforrás az alhálózaton történő telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="62ac8-194">For example, a Virtual Network subnet has a resource navigation link when a Microsoft.Cache redis resource is deployed into this subnet.</span></span>
* <span data-ttu-id="62ac8-195">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="62ac8-195">VPN Gateway</span></span>


## <a name="services-that-do-not-enable-move"></a><span data-ttu-id="62ac8-196">Ne engedélyezze a move szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="62ac8-196">Services that do not enable move</span></span>
<span data-ttu-id="62ac8-197">jelenleg nem engedélyezi az erőforrás áthelyezése hello szolgáltatások a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-197">hello services that currently do not enable moving a resource are:</span></span>

* <span data-ttu-id="62ac8-198">Az AD tartományi szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="62ac8-198">AD Domain Services</span></span>
* <span data-ttu-id="62ac8-199">AD hibrid Állapotfigyelő szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="62ac8-199">AD Hybrid Health Service</span></span>
* <span data-ttu-id="62ac8-200">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="62ac8-200">Application Gateway</span></span>
* <span data-ttu-id="62ac8-201">A felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek</span><span class="sxs-lookup"><span data-stu-id="62ac8-201">Availability sets with Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="62ac8-202">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="62ac8-202">BizTalk Services</span></span>
* <span data-ttu-id="62ac8-203">Container Service</span><span class="sxs-lookup"><span data-stu-id="62ac8-203">Container Service</span></span>
* <span data-ttu-id="62ac8-204">Express Route</span><span class="sxs-lookup"><span data-stu-id="62ac8-204">Express Route</span></span>
* <span data-ttu-id="62ac8-205">DevTest Labs - áthelyezési toonew erőforráscsoport ugyanahhoz az előfizetéshez engedélyezve van, de közötti előfizetés áthelyezése nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="62ac8-205">DevTest Labs - Move toonew resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="62ac8-206">Dynamics LCS</span><span class="sxs-lookup"><span data-stu-id="62ac8-206">Dynamics LCS</span></span>
* <span data-ttu-id="62ac8-207">Felügyelt lemezekből lemezképeit</span><span class="sxs-lookup"><span data-stu-id="62ac8-207">Images created from Managed Disks</span></span>
* <span data-ttu-id="62ac8-208">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="62ac8-208">Managed Disks</span></span>
* <span data-ttu-id="62ac8-209">Felügyelt alkalmazások</span><span class="sxs-lookup"><span data-stu-id="62ac8-209">Managed Applications</span></span>
* <span data-ttu-id="62ac8-210">Recovery Services-tároló - is tegye nem hello számítási, hálózati és tárolási erőforrások áthelyezése társított hello Recovery Services tároló című [helyreállítási szolgáltatások korlátozásai](#recovery-services-limitations).</span><span class="sxs-lookup"><span data-stu-id="62ac8-210">Recovery Services vault - also do not move hello Compute, Network, and Storage resources associated with hello Recovery Services vault, see [Recovery Services limitations](#recovery-services-limitations).</span></span>
* <span data-ttu-id="62ac8-211">Biztonság</span><span class="sxs-lookup"><span data-stu-id="62ac8-211">Security</span></span>
* <span data-ttu-id="62ac8-212">Felügyelt lemezekből létrehozott pillanatfelvételek</span><span class="sxs-lookup"><span data-stu-id="62ac8-212">Snapshots created from Managed Disks</span></span>
* <span data-ttu-id="62ac8-213">StorSimple Device Manager</span><span class="sxs-lookup"><span data-stu-id="62ac8-213">StorSimple Device Manager</span></span>
* <span data-ttu-id="62ac8-214">Felügyelt lemezzel rendelkező virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="62ac8-214">Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="62ac8-215">Tekintse meg a virtuális hálózatok (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="62ac8-215">Virtual Networks (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="62ac8-216">Piactér-erőforrások - alapján létrehozott virtuális gépeken nem helyezhető át, előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="62ac8-216">Virtual Machines created from Marketplace resources - cannot be moved across subscriptions.</span></span> <span data-ttu-id="62ac8-217">Meg kell toobe ebben az előfizetésben hello platformelőfizetés és hello új előfizetés újra üzembe helyezett erőforrás</span><span class="sxs-lookup"><span data-stu-id="62ac8-217">Resource needs toobe deprovisioned in hello current subscription and deployed again in hello new subscription</span></span>

## <a name="app-service-limitations"></a><span data-ttu-id="62ac8-218">App Service korlátozásai</span><span class="sxs-lookup"><span data-stu-id="62ac8-218">App Service limitations</span></span>
<span data-ttu-id="62ac8-219">Az App Service apps használatakor csak egy App Service-csomag nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="62ac8-219">When working with App Service apps, you cannot move only an App Service plan.</span></span> <span data-ttu-id="62ac8-220">App Service apps toomove, a lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-220">toomove App Service apps, your options are:</span></span>

* <span data-ttu-id="62ac8-221">Az erőforrás csoport tooa új erőforráscsoport, amely még nincs az App Service-erőforrások helyezi át a hello App Service-csomag és egyéb App Service-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="62ac8-221">Move hello App Service plan and all other App Service resources in that resource group tooa new resource group that does not already have App Service resources.</span></span> <span data-ttu-id="62ac8-222">Ez a követelmény azt jelenti, hogy akkor is kell áthelyeznie hello App Service-erőforrások, amelyek nincsenek társítva hello App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="62ac8-222">This requirement means you must move even hello App Service resources that are not associated with hello App Service plan.</span></span>
* <span data-ttu-id="62ac8-223">Helyezze át a hello alkalmazások tooa másik erőforráscsoportban található, de megőrizni annak összes App Service-csomagokról hello eredeti erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="62ac8-223">Move hello apps tooa different resource group, but keep all App Service plans in hello original resource group.</span></span>

<span data-ttu-id="62ac8-224">hello App Service-csomag nem kell a tooreside hello ugyanazt az erőforráscsoportot a hello app toofunction hello alkalmazásként megfelelően.</span><span class="sxs-lookup"><span data-stu-id="62ac8-224">hello App Service plan does not need tooreside in hello same resource group as hello app for hello app toofunction correctly.</span></span>

<span data-ttu-id="62ac8-225">Ha például az erőforráscsoport tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="62ac8-225">For example, if your resource group contains:</span></span>

* <span data-ttu-id="62ac8-226">**webalkalmazás-a** amely társítva van **terv-a**</span><span class="sxs-lookup"><span data-stu-id="62ac8-226">**web-a** which is associated with **plan-a**</span></span>
* <span data-ttu-id="62ac8-227">**webalkalmazás-b** amely társítva van **terv-b**</span><span class="sxs-lookup"><span data-stu-id="62ac8-227">**web-b** which is associated with **plan-b**</span></span>

<span data-ttu-id="62ac8-228">A lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-228">Your options are:</span></span>

* <span data-ttu-id="62ac8-229">Helyezze át **web-a**, **terv a**, **web-b**, és **terv-b**</span><span class="sxs-lookup"><span data-stu-id="62ac8-229">Move **web-a**, **plan-a**, **web-b**, and **plan-b**</span></span>
* <span data-ttu-id="62ac8-230">Helyezze át **web-a** és **web-b**</span><span class="sxs-lookup"><span data-stu-id="62ac8-230">Move **web-a** and **web-b**</span></span>
* <span data-ttu-id="62ac8-231">Helyezze át **web-a**</span><span class="sxs-lookup"><span data-stu-id="62ac8-231">Move **web-a**</span></span>
* <span data-ttu-id="62ac8-232">Helyezze át **web-b**</span><span class="sxs-lookup"><span data-stu-id="62ac8-232">Move **web-b**</span></span>

<span data-ttu-id="62ac8-233">Minden más kombináció tartalmaz, amely így maradnak, amely nem hagyható mögött, amikor az App Service-csomag (bármilyen típusú App Service erőforrás) erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="62ac8-233">All other combinations involve leaving behind a resource type that can't be left behind when moving an App Service plan (any type of App Service resource).</span></span>

<span data-ttu-id="62ac8-234">Ha a webalkalmazás helyezkedik el, mint az App Service-csomag egy másik erőforráscsoportban található, de nem szeretnének toomove mindkét tooa új erőforráscsoportot, két lépésben hello áthelyezés kell elvégeznie.</span><span class="sxs-lookup"><span data-stu-id="62ac8-234">If your web app resides in a different resource group than its App Service plan but you want toomove both tooa new resource group, you must perform hello move in two steps.</span></span> <span data-ttu-id="62ac8-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="62ac8-235">For example:</span></span>

* <span data-ttu-id="62ac8-236">**webalkalmazás-a** található **webalkalmazás-csoport**</span><span class="sxs-lookup"><span data-stu-id="62ac8-236">**web-a** resides in **web-group**</span></span>
* <span data-ttu-id="62ac8-237">**terv a** található **terv-csoport**</span><span class="sxs-lookup"><span data-stu-id="62ac8-237">**plan-a** resides in **plan-group**</span></span>
* <span data-ttu-id="62ac8-238">Kívánt **web-a** és **terv a** a tooreside **kombinált csoport**</span><span class="sxs-lookup"><span data-stu-id="62ac8-238">You want **web-a** and **plan-a** tooreside in **combined-group**</span></span>

<span data-ttu-id="62ac8-239">Ez a áthelyezi, két külön áthelyezés műveleteinek elvégzéséhez a következő feladatütemezési hello tooaccomplish:</span><span class="sxs-lookup"><span data-stu-id="62ac8-239">tooaccomplish this move, perform two separate move operations in hello following sequence:</span></span>

1. <span data-ttu-id="62ac8-240">Helyezze át a hello **web-a** túl**terv-csoport**</span><span class="sxs-lookup"><span data-stu-id="62ac8-240">Move hello **web-a** too**plan-group**</span></span>
2. <span data-ttu-id="62ac8-241">Helyezze át **web-a** és **terv a** túl**kombinált csoport**.</span><span class="sxs-lookup"><span data-stu-id="62ac8-241">Move **web-a** and **plan-a** too**combined-group**.</span></span>

<span data-ttu-id="62ac8-242">Áthelyezheti egy App Service tanúsítvány tooa új erőforráscsoportba vagy előfizetés probléma nélkül.</span><span class="sxs-lookup"><span data-stu-id="62ac8-242">You can move an App Service Certificate tooa new resource group or subscription without any issues.</span></span> <span data-ttu-id="62ac8-243">Azonban ha a webalkalmazás beszerzett kívülről, és a feltöltött alkalmazás toohello SSL-tanúsítványt tartalmaz, törölnie kell a mozgóátlag hello webalkalmazás előtt hello tanúsítvány is.</span><span class="sxs-lookup"><span data-stu-id="62ac8-243">However, if your web app includes an SSL certificate that you purchased externally and uploaded toohello app, you must delete hello certificate before moving hello web app.</span></span> <span data-ttu-id="62ac8-244">Például végezheti el a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="62ac8-244">For example, you can perform hello following steps:</span></span>

1. <span data-ttu-id="62ac8-245">Hello webalkalmazásból hello feltöltött tanúsítvány törlése</span><span class="sxs-lookup"><span data-stu-id="62ac8-245">Delete hello uploaded certificate from hello web app</span></span>
2. <span data-ttu-id="62ac8-246">Helyezze át a hello webalkalmazás</span><span class="sxs-lookup"><span data-stu-id="62ac8-246">Move hello web app</span></span>
3. <span data-ttu-id="62ac8-247">Hello tanúsítvány toohello webalkalmazás feltöltése</span><span class="sxs-lookup"><span data-stu-id="62ac8-247">Upload hello certificate toohello web app</span></span>

## <a name="recovery-services-limitations"></a><span data-ttu-id="62ac8-248">Helyreállítási szolgáltatások korlátozásai</span><span class="sxs-lookup"><span data-stu-id="62ac8-248">Recovery Services limitations</span></span>
<span data-ttu-id="62ac8-249">Helyezze át a tárolási, hálózati, nincs engedélyezve, vagy számítási erőforrások használt vész-helyreállítási tooset az Azure Site Recovery szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="62ac8-249">Move is not enabled for Storage, Network, or Compute resources used tooset up disaster recovery with Azure Site Recovery.</span></span>

<span data-ttu-id="62ac8-250">Például tegyük fel, hogy a helyszíni gépeket tooa tárfiókja (Storage1) replikációs beállítását, és szeretné, hogy hello védett gép toocome mentése feladatátvételi tooAzure után a virtuális gép (VM1) hozzá van kapcsolva tooa virtuális hálózat (Network1).</span><span class="sxs-lookup"><span data-stu-id="62ac8-250">For example, suppose you have set up replication of your on-premises machines tooa storage account (Storage1) and want hello protected machine toocome up after failover tooAzure as a virtual machine (VM1) attached tooa virtual network (Network1).</span></span> <span data-ttu-id="62ac8-251">Nem helyezhető át, ezek az Azure erőforrásokat - Storage1, VM1, és Network1 - erőforrás között csoportok hello belül azonos előfizetés vagy előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="62ac8-251">You cannot move any of these Azure resources - Storage1, VM1, and Network1 - across resource groups within hello same subscription or across subscriptions.</span></span>

## <a name="hdinsight-limitations"></a><span data-ttu-id="62ac8-252">A HDInsight-korlátozások</span><span class="sxs-lookup"><span data-stu-id="62ac8-252">HDInsight limitations</span></span>

<span data-ttu-id="62ac8-253">Áthelyezheti a HDInsight-fürtök tooa új előfizetéshez vagy erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="62ac8-253">You can move HDInsight clusters tooa new subscription or resource group.</span></span> <span data-ttu-id="62ac8-254">Azonban hogy nem helyezhetők át előfizetések hello hálózati erőforrások csatolt toohello HDInsight-fürt (például hello virtuális hálózat, a hálózati adapter vagy a terheléselosztó).</span><span class="sxs-lookup"><span data-stu-id="62ac8-254">However, you cannot move across subscriptions hello networking resources linked toohello HDInsight cluster (such as hello virtual network, NIC, or load balancer).</span></span> <span data-ttu-id="62ac8-255">Ezenkívül tooa új erőforráscsoportot egy hálózati Adaptert, amely hello fürt csatolt tooa virtuális gép nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="62ac8-255">In addition, you cannot move tooa new resource group a NIC that is attached tooa virtual machine for hello cluster.</span></span>

<span data-ttu-id="62ac8-256">Amikor egy HDInsight fürt tooa új előfizetés helyezi át, először helyezze át más erőforrások (például hello tárfiók).</span><span class="sxs-lookup"><span data-stu-id="62ac8-256">When moving an HDInsight cluster tooa new subscription, first move other resources (like hello storage account).</span></span> <span data-ttu-id="62ac8-257">Ezután helyezze át a hello HDInsight-fürt önmagában.</span><span class="sxs-lookup"><span data-stu-id="62ac8-257">Then, move hello HDInsight cluster by itself.</span></span>

## <a name="classic-deployment-limitations"></a><span data-ttu-id="62ac8-258">Klasszikus üzembe helyezési korlátozásai</span><span class="sxs-lookup"><span data-stu-id="62ac8-258">Classic deployment limitations</span></span>
<span data-ttu-id="62ac8-259">hello hello klasszikus modellben telepített erőforrások áthelyezésére szolgáló beállítások attól függően változnak, hogy áthelyez egy előfizetés vagy tooa új előfizetés hello erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="62ac8-259">hello options for moving resources deployed through hello classic model differ based on whether you are moving hello resources within a subscription or tooa new subscription.</span></span>

### <a name="same-subscription"></a><span data-ttu-id="62ac8-260">Ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="62ac8-260">Same subscription</span></span>
<span data-ttu-id="62ac8-261">Ha erőforrások áthelyezése erőforráscsoportból egy csoport tooanother erőforrás belül hello ugyanahhoz az előfizetéshez, hello a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="62ac8-261">When moving resources from one resource group tooanother resource group within hello same subscription, hello following restrictions apply:</span></span>

* <span data-ttu-id="62ac8-262">Nem lehet áthelyezni a virtuális hálózatok (klasszikus).</span><span class="sxs-lookup"><span data-stu-id="62ac8-262">Virtual networks (classic) cannot be moved.</span></span>
* <span data-ttu-id="62ac8-263">Virtuális gépek (klasszikus) kell áthelyezni hello felhőalapú szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="62ac8-263">Virtual machines (classic) must be moved with hello cloud service.</span></span>
* <span data-ttu-id="62ac8-264">A felhőalapú szolgáltatás csak akkor helyezhető hello áthelyezés adattípust a virtuális gépek esetén.</span><span class="sxs-lookup"><span data-stu-id="62ac8-264">Cloud service can only be moved when hello move includes all its virtual machines.</span></span>
* <span data-ttu-id="62ac8-265">Egyszerre csak egy felhőalapú szolgáltatás helyezheti át.</span><span class="sxs-lookup"><span data-stu-id="62ac8-265">Only one cloud service can be moved at a time.</span></span>
* <span data-ttu-id="62ac8-266">Egyszerre csak egy (klasszikus) tárfiókot helyezheti át.</span><span class="sxs-lookup"><span data-stu-id="62ac8-266">Only one storage account (classic) can be moved at a time.</span></span>
* <span data-ttu-id="62ac8-267">(Klasszikus) tárfiókot nem lehet áthelyezni a hello egy virtuális gép vagy egy felhőalapú szolgáltatás ugyanazt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="62ac8-267">Storage account (classic) cannot be moved in hello same operation with a virtual machine or a cloud service.</span></span>

<span data-ttu-id="62ac8-268">toomove hagyományos erőforrások tooa új erőforráscsoport belül hello ugyanahhoz az előfizetéshez, használja a hello szabványos áthelyezési műveletek keresztül hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), vagy [REST API-t](#use-rest-api).</span><span class="sxs-lookup"><span data-stu-id="62ac8-268">toomove classic resources tooa new resource group within hello same subscription, use hello standard move operations through hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), or [REST API](#use-rest-api).</span></span> <span data-ttu-id="62ac8-269">Használhat hello ugyanazokat a műveleteket, mint a Resource Manager erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="62ac8-269">You use hello same operations as you use for moving Resource Manager resources.</span></span>

### <a name="new-subscription"></a><span data-ttu-id="62ac8-270">Új előfizetés</span><span class="sxs-lookup"><span data-stu-id="62ac8-270">New subscription</span></span>
<span data-ttu-id="62ac8-271">Erőforrások tooa új előfizetés áthelyezésekor hello a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="62ac8-271">When moving resources tooa new subscription, hello following restrictions apply:</span></span>

* <span data-ttu-id="62ac8-272">Minden hagyományos erőforrás hello előfizetésben át szeretné helyezni a hello ugyanazt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="62ac8-272">All classic resources in hello subscription must be moved in hello same operation.</span></span>
* <span data-ttu-id="62ac8-273">hello célként megadott előfizetés nem tartalmazhat más hagyományos erőforrások.</span><span class="sxs-lookup"><span data-stu-id="62ac8-273">hello target subscription must not contain any other classic resources.</span></span>
* <span data-ttu-id="62ac8-274">hello áthelyezése egy külön REST API-n keresztül klasszikus helyezi át a csak meg kell adniuk.</span><span class="sxs-lookup"><span data-stu-id="62ac8-274">hello move can only be requested through a separate REST API for classic moves.</span></span> <span data-ttu-id="62ac8-275">hello szabványos erőforrás-kezelő áthelyezés parancsok tooa új előfizetés hagyományos erőforrás áthelyezése nem működnek.</span><span class="sxs-lookup"><span data-stu-id="62ac8-275">hello standard Resource Manager move commands do not work when moving classic resources tooa new subscription.</span></span>

<span data-ttu-id="62ac8-276">toomove hagyományos erőforrások tooa új előfizetés, használjon hello REST műveleteinek, amelyek adott tooclassic erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="62ac8-276">toomove classic resources tooa new subscription, use hello REST operations that are specific tooclassic resources.</span></span> <span data-ttu-id="62ac8-277">toouse REST, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="62ac8-277">toouse REST, perform hello following steps:</span></span>

1. <span data-ttu-id="62ac8-278">Ellenőrizze a forrás-előfizetés hello részt vehetnek-kereszt-előfizetés helyezze át.</span><span class="sxs-lookup"><span data-stu-id="62ac8-278">Check if hello source subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="62ac8-279">Használja a következő művelet hello:</span><span class="sxs-lookup"><span data-stu-id="62ac8-279">Use hello following operation:</span></span>

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="62ac8-280">A kérelem törzsében hello a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-280">In hello request body, include:</span></span>

  ```json
  {
    "role": "source"
  }
  ```

     <span data-ttu-id="62ac8-281">hello válasz hello ellenőrzési művelet hello a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="62ac8-281">hello response for hello validation operation is in hello following format:</span></span>

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. <span data-ttu-id="62ac8-282">Jelölőnégyzet Ha hello célelőfizetés részt vehetnek-a-előfizetések közötti áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="62ac8-282">Check if hello destination subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="62ac8-283">Használja a következő művelet hello:</span><span class="sxs-lookup"><span data-stu-id="62ac8-283">Use hello following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="62ac8-284">A kérelem törzsében hello a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-284">In hello request body, include:</span></span>

  ```json
  {
    "role": "target"
  }
  ```

     <span data-ttu-id="62ac8-285">az előfizetés érvényesítése hello formátuma azonos hello rendszer hello választ.</span><span class="sxs-lookup"><span data-stu-id="62ac8-285">hello response is in hello same format as hello source subscription validation.</span></span>
3. <span data-ttu-id="62ac8-286">Ha mindkét előfizetéshez teljesíti az ellenőrző, minden hagyományos erőforrás áthelyezése egy előfizetés tooanother előfizetés a következő művelet hello:</span><span class="sxs-lookup"><span data-stu-id="62ac8-286">If both subscriptions pass validation, move all classic resources from one subscription tooanother subscription with hello following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    <span data-ttu-id="62ac8-287">A kérelem törzsében hello a következők:</span><span class="sxs-lookup"><span data-stu-id="62ac8-287">In hello request body, include:</span></span>

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

<span data-ttu-id="62ac8-288">hello művelet több percig is futhat.</span><span class="sxs-lookup"><span data-stu-id="62ac8-288">hello operation may run for several minutes.</span></span>

## <a name="use-portal"></a><span data-ttu-id="62ac8-289">A portál használatával</span><span class="sxs-lookup"><span data-stu-id="62ac8-289">Use portal</span></span>
<span data-ttu-id="62ac8-290">toomove erőforrások hello ezen erőforrásokat tartalmazó erőforráscsoport, majd válassza ki és hello **áthelyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="62ac8-290">toomove resources, select hello resource group containing those resources, and then select hello **Move** button.</span></span>

![Erőforrások áthelyezése](./media/resource-group-move-resources/select-move.png)

<span data-ttu-id="62ac8-292">Adja meg, hogy hello erőforrások tooa új erőforráscsoport vagy egy új előfizetés áthelyezni.</span><span class="sxs-lookup"><span data-stu-id="62ac8-292">Select whether you are moving hello resources tooa new resource group or a new subscription.</span></span>

<span data-ttu-id="62ac8-293">Válassza ki a hello erőforrások toomove és hello célként megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="62ac8-293">Select hello resources toomove and hello destination resource group.</span></span> <span data-ttu-id="62ac8-294">Megerősíti, hogy kell-e tooupdate parancsfájlok ezen erőforrásokat, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="62ac8-294">Acknowledge that you need tooupdate scripts for these resources and select **OK**.</span></span> <span data-ttu-id="62ac8-295">Ha hello előző lépésben kiválasztott hello Szerkesztés előfizetés ikonra, hello célelőfizetés is kell választania.</span><span class="sxs-lookup"><span data-stu-id="62ac8-295">If you selected hello edit subscription icon in hello previous step, you must also select hello destination subscription.</span></span>

![Válassza ki a cél](./media/resource-group-move-resources/select-destination.png)

<span data-ttu-id="62ac8-297">A **értesítések**, láthatja, hogy helyezze át a művelet fut. hello.</span><span class="sxs-lookup"><span data-stu-id="62ac8-297">In **Notifications**, you see that hello move operation is running.</span></span>

![Áthelyezési állapotának megjelenítése](./media/resource-group-move-resources/show-status.png)

<span data-ttu-id="62ac8-299">Amikor befejeződött, hello eredmény értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="62ac8-299">When it has completed, you are notified of hello result.</span></span>

![Áthelyezési eredmény megjelenítése](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a><span data-ttu-id="62ac8-301">A PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="62ac8-301">Use PowerShell</span></span>
<span data-ttu-id="62ac8-302">toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello `Move-AzureRmResource` parancsot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-302">toomove existing resources tooanother resource group or subscription, use hello `Move-AzureRmResource` command.</span></span>

<span data-ttu-id="62ac8-303">hello az első példában látható szövegrészt hogyan toomove egy erőforrás tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-303">hello first example shows how toomove one resource tooa new resource group.</span></span>

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

<span data-ttu-id="62ac8-304">hello hogyan példa azt mutatja meg a második toomove több erőforrások tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-304">hello second example shows how toomove multiple resources tooa new resource group.</span></span>

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

<span data-ttu-id="62ac8-305">toomove tooa új előfizetés, hello értéket tartalmazza `DestinationSubscriptionId` paraméter.</span><span class="sxs-lookup"><span data-stu-id="62ac8-305">toomove tooa new subscription, include a value for hello `DestinationSubscriptionId` parameter.</span></span>

<span data-ttu-id="62ac8-306">A rendszer felkéri, hogy szeretné-e toomove hello tooconfirm megadott erőforrások.</span><span class="sxs-lookup"><span data-stu-id="62ac8-306">You are asked tooconfirm that you want toomove hello specified resources.</span></span>

```powershell
Confirm
Are you sure you want toomove these resources toohello resource group
'/subscriptions/{guid}/resourceGroups/newRG' hello resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a><span data-ttu-id="62ac8-307">Azure parancssori felület használatával 2.0</span><span class="sxs-lookup"><span data-stu-id="62ac8-307">Use Azure CLI 2.0</span></span>
<span data-ttu-id="62ac8-308">toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello `az resource move` parancsot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-308">toomove existing resources tooanother resource group or subscription, use hello `az resource move` command.</span></span> <span data-ttu-id="62ac8-309">Adjon meg hello erőforrást hello erőforrások toomove azonosítói.</span><span class="sxs-lookup"><span data-stu-id="62ac8-309">Provide hello resource IDs of hello resources toomove.</span></span> <span data-ttu-id="62ac8-310">Erőforrás-azonosítók kaphat a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="62ac8-310">You can get resource IDs with hello following command:</span></span>

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

<span data-ttu-id="62ac8-311">hello a következő példa bemutatja, hogyan toomove tárolási fiók tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-311">hello following example shows how toomove a storage account tooa new resource group.</span></span> <span data-ttu-id="62ac8-312">A hello `--ids` paraméter, erőforrás-azonosítók toomove hello szóközökkel elválasztott listáját tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="62ac8-312">In hello `--ids` parameter, provide a space-separated list of hello resource IDs toomove.</span></span>

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

<span data-ttu-id="62ac8-313">toomove tooa új előfizetés, adja meg a hello `--destination-subscription-id` paraméter.</span><span class="sxs-lookup"><span data-stu-id="62ac8-313">toomove tooa new subscription, provide hello `--destination-subscription-id` parameter.</span></span>

## <a name="use-azure-cli-10"></a><span data-ttu-id="62ac8-314">Azure parancssori felület használatával 1.0</span><span class="sxs-lookup"><span data-stu-id="62ac8-314">Use Azure CLI 1.0</span></span>
<span data-ttu-id="62ac8-315">toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, használja a hello `azure resource move` parancsot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-315">toomove existing resources tooanother resource group or subscription, use hello `azure resource move` command.</span></span> <span data-ttu-id="62ac8-316">Adjon meg hello erőforrást hello erőforrások toomove azonosítói.</span><span class="sxs-lookup"><span data-stu-id="62ac8-316">Provide hello resource IDs of hello resources toomove.</span></span> <span data-ttu-id="62ac8-317">Erőforrás-azonosítók kaphat a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="62ac8-317">You can get resource IDs with hello following command:</span></span>

```azurecli
azure resource list -g sourceGroup --json
```

<span data-ttu-id="62ac8-318">A következő formátumban hello visszaadó:</span><span class="sxs-lookup"><span data-stu-id="62ac8-318">Which returns hello following format:</span></span>

```azurecli
[
  {
    "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
    "name": "storagedemo",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "southcentralus",
    "tags": {},
    "kind": "Storage",
    "sku": {
      "name": "Standard_RAGRS",
      "tier": "Standard"
    }
  }
]
```

<span data-ttu-id="62ac8-319">hello a következő példa bemutatja, hogyan toomove tárolási fiók tooa új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="62ac8-319">hello following example shows how toomove a storage account tooa new resource group.</span></span> <span data-ttu-id="62ac8-320">A hello `-i` paraméter, adja meg az erőforrás-azonosítók toomove hello vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="62ac8-320">In hello `-i` parameter, provide a comma-separated list of hello resource IDs toomove.</span></span>

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

<span data-ttu-id="62ac8-321">A rendszer felkéri, hogy szeretné-e toomove hello tooconfirm megadott erőforrás.</span><span class="sxs-lookup"><span data-stu-id="62ac8-321">You are asked tooconfirm that you want toomove hello specified resource.</span></span>

## <a name="use-rest-api"></a><span data-ttu-id="62ac8-322">A REST API használata</span><span class="sxs-lookup"><span data-stu-id="62ac8-322">Use REST API</span></span>
<span data-ttu-id="62ac8-323">toomove meglévő erőforrások tooanother erőforráscsoportba vagy előfizetésbe, futtassa:</span><span class="sxs-lookup"><span data-stu-id="62ac8-323">toomove existing resources tooanother resource group or subscription, run:</span></span>

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

<span data-ttu-id="62ac8-324">Hello kérelemtörzset célként megadott erőforráscsoportja hello és hello erőforrások toomove kell megadni.</span><span class="sxs-lookup"><span data-stu-id="62ac8-324">In hello request body, you specify hello target resource group and hello resources toomove.</span></span> <span data-ttu-id="62ac8-325">Hello áthelyezési REST művelet kapcsolatos további információkért lásd: [erőforrások áthelyezése](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span><span class="sxs-lookup"><span data-stu-id="62ac8-325">For more information about hello move REST operation, see [Move resources](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="62ac8-326">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62ac8-326">Next steps</span></span>
* <span data-ttu-id="62ac8-327">az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal toolearn lásd [Azure PowerShell használata a Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-327">toolearn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="62ac8-328">az előfizetés kezelésének Azure parancssori felület parancsait kapcsolatos toolearn lásd [Using hello Azure parancssori felület a Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-328">toolearn about Azure CLI commands for managing your subscription, see [Using hello Azure CLI with Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="62ac8-329">az előfizetés kezelésének portál funkciókkal kapcsolatos toolearn lásd [hello Azure portál toomanage erőforrásokat használó](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-329">toolearn about portal features for managing your subscription, see [Using hello Azure portal toomanage resources](resource-group-portal.md).</span></span>
* <span data-ttu-id="62ac8-330">a szervezet logikai tooyour erőforrások alkalmazásával kapcsolatos toolearn lásd [Using címkéket tooorganize az erőforrások](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="62ac8-330">toolearn about applying a logical organization tooyour resources, see [Using tags tooorganize your resources](resource-group-using-tags.md).</span></span>

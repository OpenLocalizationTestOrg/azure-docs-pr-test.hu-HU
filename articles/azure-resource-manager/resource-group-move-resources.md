---
title: "Azure-erőforrások áthelyezése új előfizetés vagy az erőforrás csoport |} Microsoft Docs"
description: "Azure Resource Manager segítségével az erőforrások áthelyezése egy új erőforráscsoportba vagy előfizetésbe."
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
ms.openlocfilehash: e138f80e808968ab4bf5c11cfd5fd46fe4a1bcce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a><span data-ttu-id="3a8bc-103">Erőforrások áthelyezése új erőforráscsoportba vagy előfizetésbe</span><span class="sxs-lookup"><span data-stu-id="3a8bc-103">Move resources to new resource group or subscription</span></span>
<span data-ttu-id="3a8bc-104">Ez a témakör bemutatja, hogyan kell helyeznie az erőforrásokat az új előfizetés vagy egy új erőforráscsoportot ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-104">This topic shows you how to move resources to either a new subscription or a new resource group in the same subscription.</span></span> <span data-ttu-id="3a8bc-105">A portál, PowerShell, az Azure parancssori felület vagy a REST API használatával erőforrás áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-105">You can use the portal, PowerShell, Azure CLI, or the REST API to move resource.</span></span> <span data-ttu-id="3a8bc-106">Ez a témakör az áthelyezési műveletek rendelkezésére álljanak a segítség nélkül az Azure-támogatás.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-106">The move operations in this topic are available to you without any assistance from Azure support.</span></span>

<span data-ttu-id="3a8bc-107">Ha az erőforrások áthelyezése, mind a forrás-csoport és a célcsoport zárolva van a művelet során.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-107">When moving resources, both the source group and the target group are locked during the operation.</span></span> <span data-ttu-id="3a8bc-108">Írási és törlési műveletek blokkolják az erőforráscsoportok az áthelyezés befejezéséig.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-108">Write and delete operations are blocked on the resource groups until the move completes.</span></span> <span data-ttu-id="3a8bc-109">A lakat azt jelenti, nem adja hozzá, frissítenie vagy törölnie az erőforráscsoportok erőforrásokat, de nem jelent az erőforrások sincs rögzítve.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-109">This lock means you cannot add, update, or delete resources in the resource groups, but it does not mean the resources are frozen.</span></span> <span data-ttu-id="3a8bc-110">Például ha egy SQL Server és a kapcsolódó adatbázis áthelyezése egy új erőforráscsoportot, az adatbázist használó alkalmazások teljesen állásidő nélkül.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-110">For example, if you move a SQL Server and its database to a new resource group, an application that uses the database experiences no downtime.</span></span> <span data-ttu-id="3a8bc-111">Továbbra is olvasni és írni az adatbázisba.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-111">It can still read and write to the database.</span></span>

<span data-ttu-id="3a8bc-112">Az erőforrás helye nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-112">You cannot change the location of the resource.</span></span> <span data-ttu-id="3a8bc-113">Egy erőforrás áthelyezése csak áthelyezi egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-113">Moving a resource only moves it to a new resource group.</span></span> <span data-ttu-id="3a8bc-114">Az új erőforráscsoportot egy másik helyre azonban lehet, hogy az erőforrás helye nem változik.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-114">The new resource group may have a different location, but that does not change the location of the resource.</span></span>

> [!NOTE]
> <span data-ttu-id="3a8bc-115">A cikkből megtudhatja, hogyan kívánja áthelyezni az erőforrásokat egy meglévő Azure fiók ajánlja.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-115">This article describes how to move resources within an existing Azure account offering.</span></span> <span data-ttu-id="3a8bc-116">Ha ténylegesen módosítani szeretné az Azure-fiókjával (például a frissítés a használatalapú fizetésre előre kifizetni) nyújtó miközben továbbra is a meglévő erőforrásokkal folytatott munka című [az Azure-előfizetéshez Váltás másik ajánlatra](../billing/billing-how-to-switch-azure-offer.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-116">If you actually want to change your Azure account offering (such as upgrading from pay-as-you-go to pre-pay) while continuing to work with your existing resources, see [Switch your Azure subscription to another offer](../billing/billing-how-to-switch-azure-offer.md).</span></span>
>
>

## <a name="checklist-before-moving-resources"></a><span data-ttu-id="3a8bc-117">Erőforrások áthelyezése előtt ellenőrzőlista</span><span class="sxs-lookup"><span data-stu-id="3a8bc-117">Checklist before moving resources</span></span>
<span data-ttu-id="3a8bc-118">Néhány fontos lépést végre kell hajtani az erőforrások áthelyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-118">There are some important steps to perform before moving a resource.</span></span> <span data-ttu-id="3a8bc-119">Ezen feltételek ellenőrzésével a hibák elkerülhetőek.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-119">By verifying these conditions, you can avoid errors.</span></span>

1. <span data-ttu-id="3a8bc-120">A forrás és cél előfizetések léteznie kell a belül azonos [Azure Active Directory-bérlő](../active-directory/active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-120">The source and destination subscriptions must exist within the same [Azure Active Directory tenant](../active-directory/active-directory-howto-tenant.md).</span></span> <span data-ttu-id="3a8bc-121">Ellenőrizze, hogy mindkét előfizetéshez tartozik-e az azonos Bérlőazonosító, használja az Azure PowerShell vagy az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-121">To check that both subscriptions have the same tenant ID, use Azure PowerShell or Azure CLI.</span></span>

  <span data-ttu-id="3a8bc-122">Azure PowerShell használata:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-122">For Azure PowerShell, use:</span></span>

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  <span data-ttu-id="3a8bc-123">Az Azure CLI 2.0 használja:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-123">For Azure CLI 2.0, use:</span></span>

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  <span data-ttu-id="3a8bc-124">Ha a bérlő azonosítók a forrás és cél előfizetésekhez nem egyeznek, esetleg módosítsa a könyvtárat, az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-124">If the tenant IDs for the source and destination subscriptions are not the same, you can attempt to change the directory for the subscription.</span></span> <span data-ttu-id="3a8bc-125">Ez a beállítás, szolgáltatás-rendszergazdák, akik van bejelentkezve, akkor a Microsoft-fiók (nem lehet szervezeti fiókkal) csak érhető el.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-125">However, this option is only available to Service Administrators who are signed in with a Microsoft account (not an organizational account).</span></span> <span data-ttu-id="3a8bc-126">Sikertelen bejelentkezési kísérletet könyvtárváltás, jelentkezzen be a [klasszikus portál](https://manage.windowsazure.com/), és válassza ki **beállítások**, és válassza ki az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-126">To attempt changing the directory, log in to the [classic portal](https://manage.windowsazure.com/), and select **Settings**, and select the subscription.</span></span> <span data-ttu-id="3a8bc-127">Ha a **könyvtár szerkesztése** ikon érhető el, válassza ki azt a társított Azure Active Directory módosítása.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-127">If the **Edit Directory** icon is available, select it to change the associated Azure Active Directory.</span></span>

  ![címtár szerkesztése](./media/resource-group-move-resources/edit-directory.png)

  <span data-ttu-id="3a8bc-129">Erre az ikonra nem érhető el, ha az erőforrások áthelyezése új bérlőt kell ügyfélszolgálatához.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-129">If that icon is not available, you must contact support to move the resources to a new tenant.</span></span>

2. <span data-ttu-id="3a8bc-130">A szolgáltatásnak lehetővé kell tennie az erőforrások áthelyezését.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-130">The service must enable the ability to move resources.</span></span> <span data-ttu-id="3a8bc-131">Ez a témakör felsorolja a szolgáltatások lehetővé teszik az erőforrások áthelyezése, és a szolgáltatások nem engedélyezi az erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-131">This topic lists which services enable moving resources and which services do not enable moving resources.</span></span>
3. <span data-ttu-id="3a8bc-132">A cél előfizetést regisztrálni kell az áthelyezett erőforrás erőforrás-szolgáltatóján.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-132">The destination subscription must be registered for the resource provider of the resource being moved.</span></span> <span data-ttu-id="3a8bc-133">Ha nem, hibaüzenet arról, hogy a **az előfizetés nincs regisztrálva az erőforrástípus**.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-133">If not, you receive an error stating that the **subscription is not registered for a resource type**.</span></span> <span data-ttu-id="3a8bc-134">Ez a probléma erőforrások új előfizetésre történő áthelyezésekor fordulhat elő, ha az előfizetést még nem használták az adott erőforrástípushoz.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-134">You might encounter this problem when moving a resource to a new subscription, but that subscription has never been used with that resource type.</span></span> <span data-ttu-id="3a8bc-135">A regisztrációs állapot ellenőrzésével és erőforrás-szolgáltatók regisztrálásával kapcsolatos további információ:[Erőforrás-szolgáltatók és erőforrástípusok](resource-manager-supported-services.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-135">To learn how to check the registration status and register resource providers, see [Resource providers and types](resource-manager-supported-services.md).</span></span>

## <a name="when-to-call-support"></a><span data-ttu-id="3a8bc-136">Mikor érdemes az ügyfélszolgálat</span><span class="sxs-lookup"><span data-stu-id="3a8bc-136">When to call support</span></span>
<span data-ttu-id="3a8bc-137">Áthelyezheti a legtöbb erőforrást ebben a témakörben látható önkiszolgáló műveletek révén.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-137">You can move most resources through the self-service operations shown in this topic.</span></span> <span data-ttu-id="3a8bc-138">Használja az önkiszolgáló műveletek:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-138">Use the self-service operations to:</span></span>

* <span data-ttu-id="3a8bc-139">Resource Manager-erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-139">Move Resource Manager resources.</span></span>
* <span data-ttu-id="3a8bc-140">Hagyományos erőforrások áthelyezéséhez a [klasszikus üzembe helyezési korlátozások](#classic-deployment-limitations).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-140">Move classic resources according to the [classic deployment limitations](#classic-deployment-limitations).</span></span>

<span data-ttu-id="3a8bc-141">Hívja a támogatást, ha szeretné:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-141">Call support when you need to:</span></span>

* <span data-ttu-id="3a8bc-142">Az erőforrások áthelyezése egy új Azure-fiók (és az Azure Active Directory-bérlő).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-142">Move your resources to a new Azure account (and Azure Active Directory tenant).</span></span>
* <span data-ttu-id="3a8bc-143">Hagyományos erőforrások áthelyezéséhez, de problémát tapasztal a korlátozásokkal.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-143">Move classic resources but are having trouble with the limitations.</span></span>

## <a name="services-that-enable-move"></a><span data-ttu-id="3a8bc-144">Szolgáltatások, amelyek lehetővé teszik a áthelyezése</span><span class="sxs-lookup"><span data-stu-id="3a8bc-144">Services that enable move</span></span>
<span data-ttu-id="3a8bc-145">A lépést a szolgáltatások, amelyek lehetővé teszik egy új erőforráscsoportot és az előfizetés áthelyezését a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-145">For now, the services that enable moving to both a new resource group and subscription are:</span></span>

* <span data-ttu-id="3a8bc-146">API Management</span><span class="sxs-lookup"><span data-stu-id="3a8bc-146">API Management</span></span>
* <span data-ttu-id="3a8bc-147">App Service apps (webalkalmazások) – lásd: [App Service korlátozásai](#app-service-limitations)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-147">App Service apps (web apps) - see [App Service limitations](#app-service-limitations)</span></span>
* <span data-ttu-id="3a8bc-148">Application Insights</span><span class="sxs-lookup"><span data-stu-id="3a8bc-148">Application Insights</span></span>
* <span data-ttu-id="3a8bc-149">Automatizálás</span><span class="sxs-lookup"><span data-stu-id="3a8bc-149">Automation</span></span>
* <span data-ttu-id="3a8bc-150">Batch</span><span class="sxs-lookup"><span data-stu-id="3a8bc-150">Batch</span></span>
* <span data-ttu-id="3a8bc-151">A Bing Maps</span><span class="sxs-lookup"><span data-stu-id="3a8bc-151">Bing Maps</span></span>
* <span data-ttu-id="3a8bc-152">Tartalomkézbesítési hálózat (CDN)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-152">CDN</span></span>
* <span data-ttu-id="3a8bc-153">A felhőalapú szolgáltatások – lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-153">Cloud Services - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3a8bc-154">Cognitive Services</span><span class="sxs-lookup"><span data-stu-id="3a8bc-154">Cognitive Services</span></span>
* <span data-ttu-id="3a8bc-155">Tartalommoderátor</span><span class="sxs-lookup"><span data-stu-id="3a8bc-155">Content Moderator</span></span>
* <span data-ttu-id="3a8bc-156">Data Catalog</span><span class="sxs-lookup"><span data-stu-id="3a8bc-156">Data Catalog</span></span>
* <span data-ttu-id="3a8bc-157">Data Factory</span><span class="sxs-lookup"><span data-stu-id="3a8bc-157">Data Factory</span></span>
* <span data-ttu-id="3a8bc-158">Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="3a8bc-158">Data Lake Analytics</span></span>
* <span data-ttu-id="3a8bc-159">Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="3a8bc-159">Data Lake Store</span></span>
* <span data-ttu-id="3a8bc-160">DNS</span><span class="sxs-lookup"><span data-stu-id="3a8bc-160">DNS</span></span>
* <span data-ttu-id="3a8bc-161">Azure Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="3a8bc-161">Azure Cosmos DB</span></span>
* <span data-ttu-id="3a8bc-162">Event Hubs</span><span class="sxs-lookup"><span data-stu-id="3a8bc-162">Event Hubs</span></span>
* <span data-ttu-id="3a8bc-163">A HDInsight-fürtök - Lásd [HDInsight korlátozásai](#hdinsight-limitations)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-163">HDInsight clusters - see [HDInsight limitations](#hdinsight-limitations)</span></span>
* <span data-ttu-id="3a8bc-164">IoT-központok</span><span class="sxs-lookup"><span data-stu-id="3a8bc-164">IoT Hubs</span></span>
* <span data-ttu-id="3a8bc-165">Key Vault</span><span class="sxs-lookup"><span data-stu-id="3a8bc-165">Key Vault</span></span>
* <span data-ttu-id="3a8bc-166">Terheléselosztók</span><span class="sxs-lookup"><span data-stu-id="3a8bc-166">Load Balancers</span></span>
* <span data-ttu-id="3a8bc-167">Logic Apps</span><span class="sxs-lookup"><span data-stu-id="3a8bc-167">Logic Apps</span></span>
* <span data-ttu-id="3a8bc-168">Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3a8bc-168">Machine Learning</span></span>
* <span data-ttu-id="3a8bc-169">Media Services</span><span class="sxs-lookup"><span data-stu-id="3a8bc-169">Media Services</span></span>
* <span data-ttu-id="3a8bc-170">Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="3a8bc-170">Mobile Engagement</span></span>
* <span data-ttu-id="3a8bc-171">Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="3a8bc-171">Notification Hubs</span></span>
* <span data-ttu-id="3a8bc-172">Operational Insights</span><span class="sxs-lookup"><span data-stu-id="3a8bc-172">Operational Insights</span></span>
* <span data-ttu-id="3a8bc-173">Operations Management</span><span class="sxs-lookup"><span data-stu-id="3a8bc-173">Operations Management</span></span>
* <span data-ttu-id="3a8bc-174">Power BI</span><span class="sxs-lookup"><span data-stu-id="3a8bc-174">Power BI</span></span>
* <span data-ttu-id="3a8bc-175">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="3a8bc-175">Redis Cache</span></span>
* <span data-ttu-id="3a8bc-176">Scheduler</span><span class="sxs-lookup"><span data-stu-id="3a8bc-176">Scheduler</span></span>
* <span data-ttu-id="3a8bc-177">Keresés</span><span class="sxs-lookup"><span data-stu-id="3a8bc-177">Search</span></span>
* <span data-ttu-id="3a8bc-178">Kiszolgálófelügyelet</span><span class="sxs-lookup"><span data-stu-id="3a8bc-178">Server Management</span></span>
* <span data-ttu-id="3a8bc-179">Service Bus</span><span class="sxs-lookup"><span data-stu-id="3a8bc-179">Service Bus</span></span>
* <span data-ttu-id="3a8bc-180">Service Fabric</span><span class="sxs-lookup"><span data-stu-id="3a8bc-180">Service Fabric</span></span>
* <span data-ttu-id="3a8bc-181">Storage</span><span class="sxs-lookup"><span data-stu-id="3a8bc-181">Storage</span></span>
* <span data-ttu-id="3a8bc-182">Tekintse meg a tároló (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-182">Storage (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3a8bc-183">A Stream Analytics - feladatok nem helyezhető át, ha a Stream Analytics állapotban.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-183">Stream Analytics - Stream Analytics jobs cannot be moved when in running state.</span></span>
* <span data-ttu-id="3a8bc-184">SQL adatbázis-kiszolgáló – az adatbázis és a kiszolgáló ugyanabban az erőforráscsoportban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-184">SQL Database server - The database and server must reside in the same resource group.</span></span> <span data-ttu-id="3a8bc-185">Ha egy SQL server helyezi át, az adatbázisokat is kerülnek.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-185">When you move a SQL server, all its databases are also moved.</span></span>
* <span data-ttu-id="3a8bc-186">Traffic Manager</span><span class="sxs-lookup"><span data-stu-id="3a8bc-186">Traffic Manager</span></span>
* <span data-ttu-id="3a8bc-187">Virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="3a8bc-187">Virtual Machines</span></span>
* <span data-ttu-id="3a8bc-188">A Key Vault tárolt virtuális gépek, a tanúsítvány - áthelyezése új erőforrás ugyanahhoz az előfizetéshez csoport engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-188">Virtual Machines with certificate stored in Key Vault - Move to new resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="3a8bc-189">Virtuális gépek (klasszikus) - lásd [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-189">Virtual Machines (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3a8bc-190">Virtual Machine Scale Sets</span><span class="sxs-lookup"><span data-stu-id="3a8bc-190">Virtual Machine Scale Sets</span></span>
* <span data-ttu-id="3a8bc-191">Virtuális hálózatok - jelenleg, peered virtuális hálózat nem helyezhető át, amíg a Vnetben társviszony-létesítés le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-191">Virtual Networks - Currently, a peered Virtual Network cannot be moved until VNet peering has been disabled.</span></span> <span data-ttu-id="3a8bc-192">Ha le van tiltva, a virtuális hálózat sikeresen áthelyezhető, és a Vnetben társviszony-létesítést is engedélyezhető.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-192">Once disabled, the Virtual Network can be moved successfully and the VNet peering can be enabled.</span></span> <span data-ttu-id="3a8bc-193">Emellett a virtuális hálózati nem helyezhető át egy másik előfizetést, ha a virtuális hálózat egyetlen alhálózatának sem az erőforrás-navigációs hivatkozásokkal tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-193">In addition, a Virtual Network cannot be moved to a different subscription if the Virtual Network contains any subnet with resource navigation links.</span></span> <span data-ttu-id="3a8bc-194">Például a virtuális hálózati alhálózat egy erőforrás-navigációs hivatkozás rendelkezik Microsoft.Cache redis erőforrás az alhálózaton történő telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-194">For example, a Virtual Network subnet has a resource navigation link when a Microsoft.Cache redis resource is deployed into this subnet.</span></span>
* <span data-ttu-id="3a8bc-195">VPN Gateway</span><span class="sxs-lookup"><span data-stu-id="3a8bc-195">VPN Gateway</span></span>


## <a name="services-that-do-not-enable-move"></a><span data-ttu-id="3a8bc-196">Ne engedélyezze a move szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3a8bc-196">Services that do not enable move</span></span>
<span data-ttu-id="3a8bc-197">A szolgáltatások, amelyek jelenleg nem engedélyezi az erőforrás áthelyezése a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-197">The services that currently do not enable moving a resource are:</span></span>

* <span data-ttu-id="3a8bc-198">Az AD tartományi szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="3a8bc-198">AD Domain Services</span></span>
* <span data-ttu-id="3a8bc-199">AD hibrid Állapotfigyelő szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="3a8bc-199">AD Hybrid Health Service</span></span>
* <span data-ttu-id="3a8bc-200">Application Gateway</span><span class="sxs-lookup"><span data-stu-id="3a8bc-200">Application Gateway</span></span>
* <span data-ttu-id="3a8bc-201">A felügyelt lemezzel rendelkező virtuális gépek rendelkezésre állási készletek</span><span class="sxs-lookup"><span data-stu-id="3a8bc-201">Availability sets with Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="3a8bc-202">BizTalk Services</span><span class="sxs-lookup"><span data-stu-id="3a8bc-202">BizTalk Services</span></span>
* <span data-ttu-id="3a8bc-203">Container Service</span><span class="sxs-lookup"><span data-stu-id="3a8bc-203">Container Service</span></span>
* <span data-ttu-id="3a8bc-204">Express Route</span><span class="sxs-lookup"><span data-stu-id="3a8bc-204">Express Route</span></span>
* <span data-ttu-id="3a8bc-205">DevTest Labs - helyezze át az új erőforráscsoport ugyanahhoz az előfizetéshez engedélyezve van, de több előfizetés áthelyezése nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-205">DevTest Labs - Move to new resource group in same subscription is enabled, but cross subscription move is not enabled.</span></span>
* <span data-ttu-id="3a8bc-206">Dynamics LCS</span><span class="sxs-lookup"><span data-stu-id="3a8bc-206">Dynamics LCS</span></span>
* <span data-ttu-id="3a8bc-207">Felügyelt lemezekből lemezképeit</span><span class="sxs-lookup"><span data-stu-id="3a8bc-207">Images created from Managed Disks</span></span>
* <span data-ttu-id="3a8bc-208">Felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="3a8bc-208">Managed Disks</span></span>
* <span data-ttu-id="3a8bc-209">Felügyelt alkalmazások</span><span class="sxs-lookup"><span data-stu-id="3a8bc-209">Managed Applications</span></span>
* <span data-ttu-id="3a8bc-210">Recovery Services-tároló - is do helyezi át a számítási, hálózati és tárolási erőforrásokat, a Recovery Services-tároló társított lásd [helyreállítási szolgáltatások korlátozásai](#recovery-services-limitations).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-210">Recovery Services vault - also do not move the Compute, Network, and Storage resources associated with the Recovery Services vault, see [Recovery Services limitations](#recovery-services-limitations).</span></span>
* <span data-ttu-id="3a8bc-211">Biztonság</span><span class="sxs-lookup"><span data-stu-id="3a8bc-211">Security</span></span>
* <span data-ttu-id="3a8bc-212">Felügyelt lemezekből létrehozott pillanatfelvételek</span><span class="sxs-lookup"><span data-stu-id="3a8bc-212">Snapshots created from Managed Disks</span></span>
* <span data-ttu-id="3a8bc-213">StorSimple Device Manager</span><span class="sxs-lookup"><span data-stu-id="3a8bc-213">StorSimple Device Manager</span></span>
* <span data-ttu-id="3a8bc-214">Felügyelt lemezzel rendelkező virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="3a8bc-214">Virtual Machines with Managed Disks</span></span>
* <span data-ttu-id="3a8bc-215">Tekintse meg a virtuális hálózatok (klasszikus) - [klasszikus telepítési korlátozásai](#classic-deployment-limitations)</span><span class="sxs-lookup"><span data-stu-id="3a8bc-215">Virtual Networks (classic) - see [Classic deployment limitations](#classic-deployment-limitations)</span></span>
* <span data-ttu-id="3a8bc-216">Piactér-erőforrások - alapján létrehozott virtuális gépeken nem helyezhető át, előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-216">Virtual Machines created from Marketplace resources - cannot be moved across subscriptions.</span></span> <span data-ttu-id="3a8bc-217">Az aktuális előfizetésben platformelőfizetés és az új előfizetés újra üzembe kell erőforrás</span><span class="sxs-lookup"><span data-stu-id="3a8bc-217">Resource needs to be deprovisioned in the current subscription and deployed again in the new subscription</span></span>

## <a name="app-service-limitations"></a><span data-ttu-id="3a8bc-218">App Service korlátozásai</span><span class="sxs-lookup"><span data-stu-id="3a8bc-218">App Service limitations</span></span>
<span data-ttu-id="3a8bc-219">Az App Service apps használatakor csak egy App Service-csomag nem helyezhető át.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-219">When working with App Service apps, you cannot move only an App Service plan.</span></span> <span data-ttu-id="3a8bc-220">App Service apps áthelyezéséhez a lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-220">To move App Service apps, your options are:</span></span>

* <span data-ttu-id="3a8bc-221">Helyezze át az App Service-csomag és egyéb App Service-erőforrások az erőforráscsoport egy új erőforráscsoportot, amely még nincs az App Service-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-221">Move the App Service plan and all other App Service resources in that resource group to a new resource group that does not already have App Service resources.</span></span> <span data-ttu-id="3a8bc-222">Ez a követelmény azt jelenti, hogy akkor is, amelyek nem kapcsolódnak az App Service-csomag az App Service erőforrások kell áthelyeznie.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-222">This requirement means you must move even the App Service resources that are not associated with the App Service plan.</span></span>
* <span data-ttu-id="3a8bc-223">Az alkalmazások áthelyezése egy másik erőforráscsoportban található, de az App Service-csomagokról ne az eredeti erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-223">Move the apps to a different resource group, but keep all App Service plans in the original resource group.</span></span>

<span data-ttu-id="3a8bc-224">Az App Service-csomag nem kell lennie, ugyanabban az erőforráscsoportban, az alkalmazás az alkalmazás megfelelő működéséhez.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-224">The App Service plan does not need to reside in the same resource group as the app for the app to function correctly.</span></span>

<span data-ttu-id="3a8bc-225">Ha például az erőforráscsoport tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-225">For example, if your resource group contains:</span></span>

* <span data-ttu-id="3a8bc-226">**webalkalmazás-a** amely társítva van **terv-a**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-226">**web-a** which is associated with **plan-a**</span></span>
* <span data-ttu-id="3a8bc-227">**webalkalmazás-b** amely társítva van **terv-b**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-227">**web-b** which is associated with **plan-b**</span></span>

<span data-ttu-id="3a8bc-228">A lehetőségek a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-228">Your options are:</span></span>

* <span data-ttu-id="3a8bc-229">Helyezze át **web-a**, **terv a**, **web-b**, és **terv-b**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-229">Move **web-a**, **plan-a**, **web-b**, and **plan-b**</span></span>
* <span data-ttu-id="3a8bc-230">Helyezze át **web-a** és **web-b**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-230">Move **web-a** and **web-b**</span></span>
* <span data-ttu-id="3a8bc-231">Helyezze át **web-a**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-231">Move **web-a**</span></span>
* <span data-ttu-id="3a8bc-232">Helyezze át **web-b**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-232">Move **web-b**</span></span>

<span data-ttu-id="3a8bc-233">Minden más kombináció tartalmaz, amely így maradnak, amely nem hagyható mögött, amikor az App Service-csomag (bármilyen típusú App Service erőforrás) erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-233">All other combinations involve leaving behind a resource type that can't be left behind when moving an App Service plan (any type of App Service resource).</span></span>

<span data-ttu-id="3a8bc-234">Ha a webalkalmazás helyezkedik el, mint az App Service-csomag egy másik erőforráscsoportban található, de egyaránt egy új erőforráscsoportot át szeretné helyezni, el kell végeznie az áthelyezés két lépésben.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-234">If your web app resides in a different resource group than its App Service plan but you want to move both to a new resource group, you must perform the move in two steps.</span></span> <span data-ttu-id="3a8bc-235">Példa:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-235">For example:</span></span>

* <span data-ttu-id="3a8bc-236">**webalkalmazás-a** található **webalkalmazás-csoport**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-236">**web-a** resides in **web-group**</span></span>
* <span data-ttu-id="3a8bc-237">**terv a** található **terv-csoport**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-237">**plan-a** resides in **plan-group**</span></span>
* <span data-ttu-id="3a8bc-238">Kívánt **web-a** és **terv a** lenniük, hogy **kombinált csoport**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-238">You want **web-a** and **plan-a** to reside in **combined-group**</span></span>

<span data-ttu-id="3a8bc-239">Az áthelyezés végrehajtásához két külön áthelyezési műveletet végrehajtani az alábbi sorrendben:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-239">To accomplish this move, perform two separate move operations in the following sequence:</span></span>

1. <span data-ttu-id="3a8bc-240">Helyezze át a **web-a** való **terv-csoport**</span><span class="sxs-lookup"><span data-stu-id="3a8bc-240">Move the **web-a** to **plan-group**</span></span>
2. <span data-ttu-id="3a8bc-241">Helyezze át **web-a** és **terv a** való **kombinált csoport**.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-241">Move **web-a** and **plan-a** to **combined-group**.</span></span>

<span data-ttu-id="3a8bc-242">Egy új erőforráscsoportot, vagy probléma nélkül előfizetési áthelyezheti egy App Service-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-242">You can move an App Service Certificate to a new resource group or subscription without any issues.</span></span> <span data-ttu-id="3a8bc-243">Azonban ha a webalkalmazás SSL-tanúsítványt adott beszerzett kívülről, és az alkalmazásba feltöltött tartalmaz, törölnie kell a tanúsítvány előtt a webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-243">However, if your web app includes an SSL certificate that you purchased externally and uploaded to the app, you must delete the certificate before moving the web app.</span></span> <span data-ttu-id="3a8bc-244">Például végezheti el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-244">For example, you can perform the following steps:</span></span>

1. <span data-ttu-id="3a8bc-245">A feltöltött tanúsítvány törlése a webalkalmazásról</span><span class="sxs-lookup"><span data-stu-id="3a8bc-245">Delete the uploaded certificate from the web app</span></span>
2. <span data-ttu-id="3a8bc-246">Helyezze át a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="3a8bc-246">Move the web app</span></span>
3. <span data-ttu-id="3a8bc-247">A webes alkalmazás a tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="3a8bc-247">Upload the certificate to the web app</span></span>

## <a name="recovery-services-limitations"></a><span data-ttu-id="3a8bc-248">Helyreállítási szolgáltatások korlátozásai</span><span class="sxs-lookup"><span data-stu-id="3a8bc-248">Recovery Services limitations</span></span>
<span data-ttu-id="3a8bc-249">Helyezze át a tárolási, hálózati, nincs engedélyezve, vagy számítási erőforrásokat az Azure Site Recovery vész-helyreállítási telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-249">Move is not enabled for Storage, Network, or Compute resources used to set up disaster recovery with Azure Site Recovery.</span></span>

<span data-ttu-id="3a8bc-250">Tegyük fel például, hogy állította be a tárfiók (Storage1) helyszíni gépek replikációja, és szeretné, hogy a védett gép elérni a feladatátvételt követően az Azure-ba (Network1) virtuális hálózathoz csatlakozó virtuális gépként (VM1).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-250">For example, suppose you have set up replication of your on-premises machines to a storage account (Storage1) and want the protected machine to come up after failover to Azure as a virtual machine (VM1) attached to a virtual network (Network1).</span></span> <span data-ttu-id="3a8bc-251">Nem helyezhető át - Storage1 VM1 és Network1 - e az Azure erőforrások bármelyike erőforráscsoportok egyazon előfizetésen belül vagy az előfizetések.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-251">You cannot move any of these Azure resources - Storage1, VM1, and Network1 - across resource groups within the same subscription or across subscriptions.</span></span>

## <a name="hdinsight-limitations"></a><span data-ttu-id="3a8bc-252">A HDInsight-korlátozások</span><span class="sxs-lookup"><span data-stu-id="3a8bc-252">HDInsight limitations</span></span>

<span data-ttu-id="3a8bc-253">A HDInsight-fürtök áthelyezése egy új előfizetéshez vagy erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-253">You can move HDInsight clusters to a new subscription or resource group.</span></span> <span data-ttu-id="3a8bc-254">Azonban nem helyezhető át a hálózati erőforrások (például a virtuális hálózat, a hálózati adapter vagy a terheléselosztó) a HDInsight-fürthöz kapcsolódó előfizetések között.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-254">However, you cannot move across subscriptions the networking resources linked to the HDInsight cluster (such as the virtual network, NIC, or load balancer).</span></span> <span data-ttu-id="3a8bc-255">Ezenkívül nem helyezhető át egy új erőforráscsoportot egy hálózati Adaptert, amely a fürt virtuális gép csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-255">In addition, you cannot move to a new resource group a NIC that is attached to a virtual machine for the cluster.</span></span>

<span data-ttu-id="3a8bc-256">Amikor egy új előfizetés helyezi át a HDInsight-fürtöt, először helyezze át az egyéb erőforrások (például a tárfiók).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-256">When moving an HDInsight cluster to a new subscription, first move other resources (like the storage account).</span></span> <span data-ttu-id="3a8bc-257">Ezután helyezze át a HDInsight-fürt önmagában.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-257">Then, move the HDInsight cluster by itself.</span></span>

## <a name="classic-deployment-limitations"></a><span data-ttu-id="3a8bc-258">Klasszikus üzembe helyezési korlátozásai</span><span class="sxs-lookup"><span data-stu-id="3a8bc-258">Classic deployment limitations</span></span>
<span data-ttu-id="3a8bc-259">A klasszikus modellben telepített erőforrások áthelyezésére szolgáló beállítások attól függően változnak, hogy helyez át az erőforrásokat egy előfizetésen belül vagy egy új előfizetést.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-259">The options for moving resources deployed through the classic model differ based on whether you are moving the resources within a subscription or to a new subscription.</span></span>

### <a name="same-subscription"></a><span data-ttu-id="3a8bc-260">Ugyanahhoz az előfizetéshez</span><span class="sxs-lookup"><span data-stu-id="3a8bc-260">Same subscription</span></span>
<span data-ttu-id="3a8bc-261">Erőforrások erőforráscsoportok közötti áthelyezése egy másik erőforráscsoportban egyazon előfizetésen belül, ha a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-261">When moving resources from one resource group to another resource group within the same subscription, the following restrictions apply:</span></span>

* <span data-ttu-id="3a8bc-262">Nem lehet áthelyezni a virtuális hálózatok (klasszikus).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-262">Virtual networks (classic) cannot be moved.</span></span>
* <span data-ttu-id="3a8bc-263">Virtuális gépek (klasszikus) át szeretné helyezni a felhőalapú szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-263">Virtual machines (classic) must be moved with the cloud service.</span></span>
* <span data-ttu-id="3a8bc-264">A felhőalapú szolgáltatás csak akkor helyezhető, ha az áthelyezés tartalmazza az összes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-264">Cloud service can only be moved when the move includes all its virtual machines.</span></span>
* <span data-ttu-id="3a8bc-265">Egyszerre csak egy felhőalapú szolgáltatás helyezheti át.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-265">Only one cloud service can be moved at a time.</span></span>
* <span data-ttu-id="3a8bc-266">Egyszerre csak egy (klasszikus) tárfiókot helyezheti át.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-266">Only one storage account (classic) can be moved at a time.</span></span>
* <span data-ttu-id="3a8bc-267">(Klasszikus) tárfiókot nem lehet áthelyezni a virtuális gép vagy egy felhőalapú szolgáltatás ugyanazt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-267">Storage account (classic) cannot be moved in the same operation with a virtual machine or a cloud service.</span></span>

<span data-ttu-id="3a8bc-268">Hagyományos erőforrás áthelyezése egy új erőforráscsoportot egyazon előfizetésen belül, a standard áthelyezési műveletek keresztül használja a [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), vagy [REST API-t](#use-rest-api).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-268">To move classic resources to a new resource group within the same subscription, use the standard move operations through the [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), or [REST API](#use-rest-api).</span></span> <span data-ttu-id="3a8bc-269">Használhatja ugyanazokat a műveleteket, mint a Resource Manager erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-269">You use the same operations as you use for moving Resource Manager resources.</span></span>

### <a name="new-subscription"></a><span data-ttu-id="3a8bc-270">Új előfizetés</span><span class="sxs-lookup"><span data-stu-id="3a8bc-270">New subscription</span></span>
<span data-ttu-id="3a8bc-271">Ha az erőforrások áthelyezése új előfizetés, a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-271">When moving resources to a new subscription, the following restrictions apply:</span></span>

* <span data-ttu-id="3a8bc-272">Az előfizetés az összes hagyományos erőforrások át szeretné helyezni a ugyanazt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-272">All classic resources in the subscription must be moved in the same operation.</span></span>
* <span data-ttu-id="3a8bc-273">A célként megadott előfizetés nem tartalmazhat más hagyományos erőforrások.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-273">The target subscription must not contain any other classic resources.</span></span>
* <span data-ttu-id="3a8bc-274">Az Áthelyezés egy külön REST API-n keresztül klasszikus helyezi át a csak meg kell adniuk.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-274">The move can only be requested through a separate REST API for classic moves.</span></span> <span data-ttu-id="3a8bc-275">A szabványos erőforrás-kezelő áthelyezés parancsok nem működnek a hagyományos erőforrás áthelyezése egy új előfizetést.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-275">The standard Resource Manager move commands do not work when moving classic resources to a new subscription.</span></span>

<span data-ttu-id="3a8bc-276">Hagyományos erőforrás áthelyezése egy új előfizetés, a jellemző hagyományos erőforrások REST műveleteinek használja.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-276">To move classic resources to a new subscription, use the REST operations that are specific to classic resources.</span></span> <span data-ttu-id="3a8bc-277">A többi használatához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-277">To use REST, perform the following steps:</span></span>

1. <span data-ttu-id="3a8bc-278">Ellenőrizze, hogy ha a forrás-előfizetés részt vehetnek-e egy előfizetések közötti áthelyezés.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-278">Check if the source subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="3a8bc-279">Használja a következő műveletet:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-279">Use the following operation:</span></span>

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="3a8bc-280">A kérelem törzsében szereplő a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-280">In the request body, include:</span></span>

  ```json
  {
    "role": "source"
  }
  ```

     <span data-ttu-id="3a8bc-281">A válasz az ellenőrzési művelet a következő formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-281">The response for the validation operation is in the following format:</span></span>

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. <span data-ttu-id="3a8bc-282">Ellenőrizze, hogy ha a célelőfizetés részt vehetnek-e egy előfizetések közötti áthelyezés.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-282">Check if the destination subscription can participate in a cross-subscription move.</span></span> <span data-ttu-id="3a8bc-283">Használja a következő műveletet:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-283">Use the following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     <span data-ttu-id="3a8bc-284">A kérelem törzsében szereplő a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-284">In the request body, include:</span></span>

  ```json
  {
    "role": "target"
  }
  ```

     <span data-ttu-id="3a8bc-285">A válasz van ugyanabban a formában, mint a forrás-előfizetés ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-285">The response is in the same format as the source subscription validation.</span></span>
3. <span data-ttu-id="3a8bc-286">Ha mindkét előfizetéshez teljesíti az ellenőrző, minden hagyományos erőforrás áthelyezése egy előfizetés másik előfizetéshez a következő műveletet:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-286">If both subscriptions pass validation, move all classic resources from one subscription to another subscription with the following operation:</span></span>

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    <span data-ttu-id="3a8bc-287">A kérelem törzsében szereplő a következők:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-287">In the request body, include:</span></span>

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

<span data-ttu-id="3a8bc-288">A művelet több percig is futhat.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-288">The operation may run for several minutes.</span></span>

## <a name="use-portal"></a><span data-ttu-id="3a8bc-289">A portál használatával</span><span class="sxs-lookup"><span data-stu-id="3a8bc-289">Use portal</span></span>
<span data-ttu-id="3a8bc-290">Erőforrások áthelyezéséhez jelölje ki ezeket az erőforrásokat tartalmazó erőforráscsoportot, majd a **áthelyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-290">To move resources, select the resource group containing those resources, and then select the **Move** button.</span></span>

![Erőforrások áthelyezése](./media/resource-group-move-resources/select-move.png)

<span data-ttu-id="3a8bc-292">Adja meg, hogy egy új erőforráscsoportot, vagy egy új előfizetés áthelyezni az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-292">Select whether you are moving the resources to a new resource group or a new subscription.</span></span>

<span data-ttu-id="3a8bc-293">Válassza ki az áthelyezni kívánt erőforrásokat és a célként megadott erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-293">Select the resources to move and the destination resource group.</span></span> <span data-ttu-id="3a8bc-294">Megerősíti, hogy szeretné-e frissíteni a parancsfájl-ezeket az erőforrásokat, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-294">Acknowledge that you need to update scripts for these resources and select **OK**.</span></span> <span data-ttu-id="3a8bc-295">Ha az előző lépésben kiválasztott előfizetés Szerkesztés ikonra, a célelőfizetés is kell választania.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-295">If you selected the edit subscription icon in the previous step, you must also select the destination subscription.</span></span>

![Válassza ki a cél](./media/resource-group-move-resources/select-destination.png)

<span data-ttu-id="3a8bc-297">A **értesítések**, láthatja, hogy fut-e a műveletet.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-297">In **Notifications**, you see that the move operation is running.</span></span>

![Áthelyezési állapotának megjelenítése](./media/resource-group-move-resources/show-status.png)

<span data-ttu-id="3a8bc-299">Amikor befejeződött, az eredmény értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-299">When it has completed, you are notified of the result.</span></span>

![Áthelyezési eredmény megjelenítése](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a><span data-ttu-id="3a8bc-301">A PowerShell használata</span><span class="sxs-lookup"><span data-stu-id="3a8bc-301">Use PowerShell</span></span>
<span data-ttu-id="3a8bc-302">Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a `Move-AzureRmResource` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-302">To move existing resources to another resource group or subscription, use the `Move-AzureRmResource` command.</span></span>

<span data-ttu-id="3a8bc-303">Az első példában egy erőforrás áthelyezése egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-303">The first example shows how to move one resource to a new resource group.</span></span>

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

<span data-ttu-id="3a8bc-304">A második példában több erőforrás áthelyezése egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-304">The second example shows how to move multiple resources to a new resource group.</span></span>

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

<span data-ttu-id="3a8bc-305">Helyezze át az új előfizetés, tartalmazza a értéket a `DestinationSubscriptionId` paraméter.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-305">To move to a new subscription, include a value for the `DestinationSubscriptionId` parameter.</span></span>

<span data-ttu-id="3a8bc-306">A rendszer felkéri győződjön meg arról, hogy szeretné-e a megadott erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-306">You are asked to confirm that you want to move the specified resources.</span></span>

```powershell
Confirm
Are you sure you want to move these resources to the resource group
'/subscriptions/{guid}/resourceGroups/newRG' the resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a><span data-ttu-id="3a8bc-307">Azure parancssori felület használatával 2.0</span><span class="sxs-lookup"><span data-stu-id="3a8bc-307">Use Azure CLI 2.0</span></span>
<span data-ttu-id="3a8bc-308">Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a `az resource move` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-308">To move existing resources to another resource group or subscription, use the `az resource move` command.</span></span> <span data-ttu-id="3a8bc-309">Adja meg az erőforrás-azonosítók az erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-309">Provide the resource IDs of the resources to move.</span></span> <span data-ttu-id="3a8bc-310">Erőforrás-azonosítók a következő paranccsal szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-310">You can get resource IDs with the following command:</span></span>

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

<span data-ttu-id="3a8bc-311">A következő példa bemutatja, hogyan a storage-fiók áthelyezése egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-311">The following example shows how to move a storage account to a new resource group.</span></span> <span data-ttu-id="3a8bc-312">Az a `--ids` paramétert, az erőforrás-azonosítók áthelyezése szóközökkel elválasztott listáját tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-312">In the `--ids` parameter, provide a space-separated list of the resource IDs to move.</span></span>

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

<span data-ttu-id="3a8bc-313">Helyezze át az új előfizetés, adja meg a `--destination-subscription-id` paraméter.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-313">To move to a new subscription, provide the `--destination-subscription-id` parameter.</span></span>

## <a name="use-azure-cli-10"></a><span data-ttu-id="3a8bc-314">Azure parancssori felület használatával 1.0</span><span class="sxs-lookup"><span data-stu-id="3a8bc-314">Use Azure CLI 1.0</span></span>
<span data-ttu-id="3a8bc-315">Meglévő erőforrásokat egy másik erőforráscsoportba vagy előfizetésbe történő áthelyezéséhez használja a `azure resource move` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-315">To move existing resources to another resource group or subscription, use the `azure resource move` command.</span></span> <span data-ttu-id="3a8bc-316">Adja meg az erőforrás-azonosítók az erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-316">Provide the resource IDs of the resources to move.</span></span> <span data-ttu-id="3a8bc-317">Erőforrás-azonosítók a következő paranccsal szerezheti be:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-317">You can get resource IDs with the following command:</span></span>

```azurecli
azure resource list -g sourceGroup --json
```

<span data-ttu-id="3a8bc-318">Amely adja vissza a következő formátumban:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-318">Which returns the following format:</span></span>

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

<span data-ttu-id="3a8bc-319">A következő példa bemutatja, hogyan a storage-fiók áthelyezése egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-319">The following example shows how to move a storage account to a new resource group.</span></span> <span data-ttu-id="3a8bc-320">Az a `-i` paraméter, adja meg az erőforrás-azonosítók áthelyezése vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-320">In the `-i` parameter, provide a comma-separated list of the resource IDs to move.</span></span>

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

<span data-ttu-id="3a8bc-321">Győződjön meg arról, hogy szeretné-e a megadott erőforrás áthelyezése kérni.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-321">You are asked to confirm that you want to move the specified resource.</span></span>

## <a name="use-rest-api"></a><span data-ttu-id="3a8bc-322">A REST API használata</span><span class="sxs-lookup"><span data-stu-id="3a8bc-322">Use REST API</span></span>
<span data-ttu-id="3a8bc-323">Meglévő erőforrások áthelyezése egy másik erőforráscsoportba vagy előfizetésbe, futtassa:</span><span class="sxs-lookup"><span data-stu-id="3a8bc-323">To move existing resources to another resource group or subscription, run:</span></span>

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

<span data-ttu-id="3a8bc-324">A kérelem törzsében meg a célként megadott erőforráscsoportja és az erőforrások áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="3a8bc-324">In the request body, you specify the target resource group and the resources to move.</span></span> <span data-ttu-id="3a8bc-325">Az áthelyezési REST művelet kapcsolatos további információkért lásd: [erőforrások áthelyezése](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-325">For more information about the move REST operation, see [Move resources](https://msdn.microsoft.com/library/azure/mt218710.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3a8bc-326">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3a8bc-326">Next steps</span></span>
* <span data-ttu-id="3a8bc-327">Az előfizetés kezelésére szolgáló PowerShell-parancsmagokkal kapcsolatban lásd: [Azure PowerShell használata a Resource Manager](powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-327">To learn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](powershell-azure-resource-manager.md).</span></span>
* <span data-ttu-id="3a8bc-328">Az előfizetés kezelésének Azure parancssori felület parancsait kapcsolatos további tudnivalókért lásd: [az Azure parancssori felület használatával a Resource Manager](xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-328">To learn about Azure CLI commands for managing your subscription, see [Using the Azure CLI with Resource Manager](xplat-cli-azure-resource-manager.md).</span></span>
* <span data-ttu-id="3a8bc-329">Az előfizetés kezelésének portál funkciókkal kapcsolatos további tudnivalókért lásd: [-erőforrások kezeléséhez Azure portál használatával](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-329">To learn about portal features for managing your subscription, see [Using the Azure portal to manage resources](resource-group-portal.md).</span></span>
* <span data-ttu-id="3a8bc-330">Az erőforrások logikus alkalmazásával kapcsolatos további tudnivalókért lásd: [az erőforrások rendszerezése címkék használatával](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="3a8bc-330">To learn about applying a logical organization to your resources, see [Using tags to organize your resources](resource-group-using-tags.md).</span></span>

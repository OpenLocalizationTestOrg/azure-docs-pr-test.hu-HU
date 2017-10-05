---
title: "Azure parancssori felület használatával Azure Redis Cache kezelése |} Microsoft Docs"
description: "Útmutató: az Azure parancssori felület telepítése bármilyen platformon, az Azure-fiókjába való kapcsolódáshoz használandó, és létrehozását és kezelését a Redis gyorsítótár az Azure parancssori felületen."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 964ff245-859d-4bc1-bccf-62e4b3c1169f
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: ba078a870a3998568170cc197bd6698b97b7fadb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-azure-redis-cache-using-the-azure-command-line-interface-azure-cli"></a><span data-ttu-id="b4381-103">Létrehozása és kezelése az Azure Redis Cache az Azure parancssori felület (CLI) használatával</span><span class="sxs-lookup"><span data-stu-id="b4381-103">How to create and manage Azure Redis Cache using the Azure Command-Line Interface (Azure CLI)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b4381-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b4381-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="b4381-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b4381-105">Azure CLI</span></span>](cache-manage-cli.md)
>
>

<span data-ttu-id="b4381-106">Az Azure parancssori felület kiváló módja a kezelését az Azure-infrastruktúra bármely platformra.</span><span class="sxs-lookup"><span data-stu-id="b4381-106">The Azure CLI is a great way to manage your Azure infrastructure from any platform.</span></span> <span data-ttu-id="b4381-107">Ez a cikk bemutatja, hogyan hozhatja létre és kezelheti az Azure Redis Cache példányt az Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="b4381-107">This article shows you how to create and manage your Azure Redis Cache instances using the Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="b4381-108">Ez a cikk az Azure parancssori felület korábbi verziójára vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b4381-108">This article applies to a previous version of Azure CLI.</span></span> <span data-ttu-id="b4381-109">Tekintse meg a legújabb Azure CLI 2.0 mintaparancsfájlok [Azure CLI Redis gyorsítótár minták](cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b4381-109">For the latest Azure CLI 2.0 sample scripts, see [Azure CLI Redis cache samples](cli-samples.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b4381-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b4381-110">Prerequisites</span></span>
<span data-ttu-id="b4381-111">Létrehozása és kezelése az Azure Redis Cache példány Azure CLI-vel, az alábbi lépéseket kell végrehajtania.</span><span class="sxs-lookup"><span data-stu-id="b4381-111">To create and manage Azure Redis Cache instances using Azure CLI, you must complete the following steps.</span></span>

* <span data-ttu-id="b4381-112">Azure-fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="b4381-112">You must have an Azure account.</span></span> <span data-ttu-id="b4381-113">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="b4381-113">If you don't have one, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a few moments.</span></span>
* <span data-ttu-id="b4381-114">[Az Azure parancssori felület telepítése](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="b4381-114">[Install the Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="b4381-115">Csatlakozás az Azure parancssori felület telepítése személyes Azure-fiókkal, vagy a munkahelyi vagy iskolai Azure-fiókra, és jelentkezzen be az Azure parancssori felület használatával a `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-115">Connect your Azure CLI installation with a personal Azure account, or with a work or school Azure account, and log in from the Azure CLI using the `azure login` command.</span></span> <span data-ttu-id="b4381-116">Különbségek megismeréséhez, és válassza ki, [csatlakozás Azure-előfizetéshez az Azure parancssori felület (CLI)](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="b4381-116">To understand the differences and choose, see [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="b4381-117">A következő parancsok futtatásához váltson az Azure parancssori felület Resource Manager módra futtatásával a `azure config mode arm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-117">Before running any of the following commands, switch the Azure CLI into Resource Manager mode by running the `azure config mode arm` command.</span></span> <span data-ttu-id="b4381-118">További információkért lásd: [Azure-erőforrások és csoportok kezelése az Azure parancssori felület használatával](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="b4381-118">For more information, see [Use the Azure CLI to manage Azure resources and resource groups](../xplat-cli-azure-resource-manager.md).</span></span>

## <a name="redis-cache-properties"></a><span data-ttu-id="b4381-119">Redis gyorsítótár tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b4381-119">Redis Cache properties</span></span>
<span data-ttu-id="b4381-120">Az alábbi tulajdonságokat használja a létrehozása és frissítése a Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="b4381-120">The following properties are used when creating and updating Redis Cache instances.</span></span>

| <span data-ttu-id="b4381-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b4381-121">Property</span></span> | <span data-ttu-id="b4381-122">Kapcsoló</span><span class="sxs-lookup"><span data-stu-id="b4381-122">Switch</span></span> | <span data-ttu-id="b4381-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="b4381-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b4381-124">név</span><span class="sxs-lookup"><span data-stu-id="b4381-124">name</span></span> |<span data-ttu-id="b4381-125">-n,--neve</span><span class="sxs-lookup"><span data-stu-id="b4381-125">-n, --name</span></span> |<span data-ttu-id="b4381-126">A Redis gyorsítótárt neve.</span><span class="sxs-lookup"><span data-stu-id="b4381-126">Name of the Redis Cache.</span></span> |
| <span data-ttu-id="b4381-127">erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="b4381-127">resource group</span></span> |<span data-ttu-id="b4381-128">-g,--erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="b4381-128">-g, --resource-group</span></span> |<span data-ttu-id="b4381-129">Az erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="b4381-129">Name of the Resource Group.</span></span> |
| <span data-ttu-id="b4381-130">location</span><span class="sxs-lookup"><span data-stu-id="b4381-130">location</span></span> |<span data-ttu-id="b4381-131">-l,--helye</span><span class="sxs-lookup"><span data-stu-id="b4381-131">-l, --location</span></span> |<span data-ttu-id="b4381-132">Hely gyorsítótár létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="b4381-132">Location to create cache.</span></span> |
| <span data-ttu-id="b4381-133">Méret</span><span class="sxs-lookup"><span data-stu-id="b4381-133">size</span></span> |<span data-ttu-id="b4381-134">-z, a--mérete</span><span class="sxs-lookup"><span data-stu-id="b4381-134">-z, --size</span></span> |<span data-ttu-id="b4381-135">A Redis gyorsítótár méretét.</span><span class="sxs-lookup"><span data-stu-id="b4381-135">Size of the Redis Cache.</span></span> <span data-ttu-id="b4381-136">Érvényes értékek: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span><span class="sxs-lookup"><span data-stu-id="b4381-136">Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
| <span data-ttu-id="b4381-137">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="b4381-137">sku</span></span> |<span data-ttu-id="b4381-138">-x,--termékváltozat</span><span class="sxs-lookup"><span data-stu-id="b4381-138">-x, --sku</span></span> |<span data-ttu-id="b4381-139">Redis Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="b4381-139">Redis SKU.</span></span> <span data-ttu-id="b4381-140">Egyike lehet: [alapszintű, Standard, Premium]</span><span class="sxs-lookup"><span data-stu-id="b4381-140">Should be one of : [Basic, Standard, Premium]</span></span> |
| <span data-ttu-id="b4381-141">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="b4381-141">EnableNonSslPort</span></span> |<span data-ttu-id="b4381-142">-e,--enable-nem-ssl-port</span><span class="sxs-lookup"><span data-stu-id="b4381-142">-e, --enable-non-ssl-port</span></span> |<span data-ttu-id="b4381-143">A Redis Cache EnableNonSslPort tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="b4381-143">EnableNonSslPort property of the Redis Cache.</span></span> <span data-ttu-id="b4381-144">Adja hozzá ezt a jelzőt, ha azt szeretné, hogy a gyorsítótár a nem SSL Port engedélyezéséhez</span><span class="sxs-lookup"><span data-stu-id="b4381-144">Add this flag if you want to enable the Non SSL Port for your cache</span></span> |
| <span data-ttu-id="b4381-145">Konfigurációs redis</span><span class="sxs-lookup"><span data-stu-id="b4381-145">Redis Configuration</span></span> |<span data-ttu-id="b4381-146">-c,--redis-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="b4381-146">-c, --redis-configuration</span></span> |<span data-ttu-id="b4381-147">Konfigurációs redis.</span><span class="sxs-lookup"><span data-stu-id="b4381-147">Redis Configuration.</span></span> <span data-ttu-id="b4381-148">Konfigurációs kulcsok és értékek itt egy JSON formátumú karakterláncot adjon meg.</span><span class="sxs-lookup"><span data-stu-id="b4381-148">Enter a JSON formatted string of configuration keys and values here.</span></span> <span data-ttu-id="b4381-149">Formátum: "{" ":""," ":" "}"</span><span class="sxs-lookup"><span data-stu-id="b4381-149">Format:"{"":"","":""}"</span></span> |
| <span data-ttu-id="b4381-150">Konfigurációs redis</span><span class="sxs-lookup"><span data-stu-id="b4381-150">Redis Configuration</span></span> |<span data-ttu-id="b4381-151">-f,--redis-konfiguráció-fájl</span><span class="sxs-lookup"><span data-stu-id="b4381-151">-f, --redis-configuration-file</span></span> |<span data-ttu-id="b4381-152">Konfigurációs redis.</span><span class="sxs-lookup"><span data-stu-id="b4381-152">Redis Configuration.</span></span> <span data-ttu-id="b4381-153">Konfigurációs kulcsok és értékek itt tartalmazó fájl elérési útját adja meg.</span><span class="sxs-lookup"><span data-stu-id="b4381-153">Enter the path of a file containing configuration keys and values here.</span></span> <span data-ttu-id="b4381-154">A következő fájl bejegyzés formátum: {"": "","": ""}</span><span class="sxs-lookup"><span data-stu-id="b4381-154">Format for the file entry: {"":"","":""}</span></span> |
| <span data-ttu-id="b4381-155">A shard száma</span><span class="sxs-lookup"><span data-stu-id="b4381-155">Shard Count</span></span> |<span data-ttu-id="b4381-156">-r,--shard-száma</span><span class="sxs-lookup"><span data-stu-id="b4381-156">-r, --shard-count</span></span> |<span data-ttu-id="b4381-157">Hozzon létre egy prémium szintű fürt gyorsítótár fürtözési a szilánkok száma.</span><span class="sxs-lookup"><span data-stu-id="b4381-157">Number of Shards to create on a Premium Cluster Cache with clustering.</span></span> |
| <span data-ttu-id="b4381-158">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="b4381-158">Virtual Network</span></span> |<span data-ttu-id="b4381-159">-v, – virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="b4381-159">-v, --virtual-network</span></span> |<span data-ttu-id="b4381-160">Esetén a VNETEN belül a gyorsítótárhoz, Azonosítóját adja meg a pontos ARM erőforrás a virtuális hálózati redis gyorsítótár telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4381-160">When hosting your cache in a VNET, specifies the exact ARM resource ID of the virtual network to deploy the redis cache in.</span></span> <span data-ttu-id="b4381-161">Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="b4381-161">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="b4381-162">kulcs típusa</span><span class="sxs-lookup"><span data-stu-id="b4381-162">key type</span></span> |<span data-ttu-id="b4381-163">-t,--kulcs-típus</span><span class="sxs-lookup"><span data-stu-id="b4381-163">-t, --key-type</span></span> |<span data-ttu-id="b4381-164">Kulcs megújításához típusa.</span><span class="sxs-lookup"><span data-stu-id="b4381-164">Type of key to renew.</span></span> <span data-ttu-id="b4381-165">Érvényes értékek: [elsődleges, másodlagos]</span><span class="sxs-lookup"><span data-stu-id="b4381-165">Valid values: [Primary, Secondary]</span></span> |
| <span data-ttu-id="b4381-166">StaticIP</span><span class="sxs-lookup"><span data-stu-id="b4381-166">StaticIP</span></span> |<span data-ttu-id="b4381-167">-p, – statikus ip-< ip-statikus ></span><span class="sxs-lookup"><span data-stu-id="b4381-167">-p, --static-ip <static-ip></span></span> |<span data-ttu-id="b4381-168">Esetén a VNETEN belül a gyorsítótárhoz, adja meg egy egyedi IP-cím az alhálózat, a gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="b4381-168">When hosting your cache in a VNET, specifies a unique IP address in the subnet for the cache.</span></span> <span data-ttu-id="b4381-169">Ha nem ad meg, egy választja meg az alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="b4381-169">If not provided, one is chosen for you from the subnet.</span></span> |
| <span data-ttu-id="b4381-170">Alhálózat</span><span class="sxs-lookup"><span data-stu-id="b4381-170">Subnet</span></span> |<span data-ttu-id="b4381-171">t,--alhálózati<subnet></span><span class="sxs-lookup"><span data-stu-id="b4381-171">t, --subnet <subnet></span></span> |<span data-ttu-id="b4381-172">Esetén a VNETEN belül a gyorsítótár, a neve, amelyben a gyorsítótár telepítendő alhálózat.</span><span class="sxs-lookup"><span data-stu-id="b4381-172">When hosting your cache in a VNET, specifies the name of the subnet in which to deploy the cache.</span></span> |
| <span data-ttu-id="b4381-173">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="b4381-173">VirtualNetwork</span></span> |<span data-ttu-id="b4381-174">-v rendszerben--virtuális-< virtuális hálózatok ></span><span class="sxs-lookup"><span data-stu-id="b4381-174">-v, --virtual-network <virtual-network></span></span> |<span data-ttu-id="b4381-175">Esetén a VNETEN belül a gyorsítótárhoz, Azonosítóját adja meg a pontos ARM erőforrás a virtuális hálózati redis gyorsítótár telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b4381-175">When hosting your cache in a VNET, specifies the exact ARM resource ID of the virtual network to deploy the redis cache in.</span></span> <span data-ttu-id="b4381-176">Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="b4381-176">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="b4381-177">Előfizetés</span><span class="sxs-lookup"><span data-stu-id="b4381-177">Subscription</span></span> |<span data-ttu-id="b4381-178">-s,--előfizetés</span><span class="sxs-lookup"><span data-stu-id="b4381-178">-s, --subscription</span></span> |<span data-ttu-id="b4381-179">Az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b4381-179">The subscription identifier.</span></span> |

## <a name="see-all-redis-cache-commands"></a><span data-ttu-id="b4381-180">Tekintse meg az összes Redis Cache-parancsok</span><span class="sxs-lookup"><span data-stu-id="b4381-180">See all Redis Cache commands</span></span>
<span data-ttu-id="b4381-181">Minden Redis Cache parancsot és paramétereket, használja a `azure rediscache -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-181">To see all Redis Cache commands and their parameters, use the `azure rediscache -h` command.</span></span>

    C:\>azure rediscache -h
    help:    Commands to manage your Azure Redis Cache(s)
    help:
    help:    Create a Redis Cache
    help:      rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Delete an existing Redis Cache
    help:      rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    List all Redis Caches within your Subscription or Resource Group
    help:      rediscache list [options]
    help:
    help:    Show properties of an existing Redis Cache
    help:      rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Change settings of an existing Redis Cache
    help:      rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Renew the authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a><span data-ttu-id="b4381-182">Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="b4381-182">Create a Redis Cache</span></span>
<span data-ttu-id="b4381-183">Redis gyorsítótárat létrehozni, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4381-183">To create a Redis Cache, use the following command:</span></span>

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

<span data-ttu-id="b4381-184">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache create -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-184">For more information about this command, run the `azure rediscache create -h` command.</span></span>

    C:\>azure rediscache create -h
    help:    Create a Redis Cache
    help:
    help:    Usage: rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -l, --location <location>                                Location to create cache.
    help:      -z, --size <size>                                        Size of the Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of the Redis Cache. Add this flag if you want to enable the Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here. Format for the file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards to create on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  The exact ARM resource ID of the virtual network to deploy the redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a><span data-ttu-id="b4381-185">Egy meglévő Redis Cache törlése</span><span class="sxs-lookup"><span data-stu-id="b4381-185">Delete an existing Redis Cache</span></span>
<span data-ttu-id="b4381-186">Egy Redis gyorsítótár törléséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4381-186">To delete a Redis Cache, use the following command:</span></span>

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

<span data-ttu-id="b4381-187">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache delete -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-187">For more information about this command, run the `azure rediscache delete -h` command.</span></span>

    C:\>azure rediscache delete -h
    help:    Delete an existing Redis Cache
    help:
    help:    Usage: rediscache delete [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which the cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a><span data-ttu-id="b4381-188">Minden Redis Cache-gyorsítótárak az előfizetéshez vagy erőforráscsoporthoz belül felsorolása</span><span class="sxs-lookup"><span data-stu-id="b4381-188">List all Redis Caches within your Subscription or Resource Group</span></span>
<span data-ttu-id="b4381-189">Összes Redis Cache-gyorsítótárak az előfizetéshez vagy erőforráscsoporthoz belül listájában, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4381-189">To list all Redis Caches within your Subscription or Resource Group, use the following command:</span></span>

    azure rediscache list [options]

<span data-ttu-id="b4381-190">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache list -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-190">For more information about this command, run the `azure rediscache list -h` command.</span></span>

    C:\>azure rediscache list -h
    help:    List all Redis Caches within your Subscription or Resource Group
    help:
    help:    Usage: rediscache list [options]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a><span data-ttu-id="b4381-191">Egy meglévő Redis Cache tulajdonságainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="b4381-191">Show properties of an existing Redis Cache</span></span>
<span data-ttu-id="b4381-192">Egy meglévő Redis Cache tulajdonságainak megjelenítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4381-192">To show properties of an existing Redis Cache, use the following command:</span></span>

    azure rediscache show [--name <name> --resource-group <resource-group>]

<span data-ttu-id="b4381-193">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache show -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-193">For more information about this command, run the `azure rediscache show -h` command.</span></span>

    C:\>azure rediscache show -h
    help:    Show properties of an existing Redis Cache
    help:
    help:    Usage: rediscache show [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a><span data-ttu-id="b4381-194">Egy meglévő Redis gyorsítótár beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="b4381-194">Change settings of an existing Redis Cache</span></span>
<span data-ttu-id="b4381-195">Egy meglévő Redis gyorsítótár beállításainak módosításához használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4381-195">To change settings of an existing Redis Cache, use the following command:</span></span>

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

<span data-ttu-id="b4381-196">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache set -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-196">For more information about this command, run the `azure rediscache set -h` command.</span></span>

    C:\>azure rediscache set -h
    help:    Change settings of an existing Redis Cache
    help:
    help:    Usage: rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]
    help:
    help:    Options:
    help:      -h, --help                                               output usage information
    help:      -v, --verbose                                            use verbose output
    help:      -vv                                                      more verbose with debug output
    help:      --json                                                   use json output
    help:      -n, --name <name>                                        Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of the Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter the path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-the-authentication-key-for-an-existing-redis-cache"></a><span data-ttu-id="b4381-197">Újítsa meg a hitelesítési kulcs egy meglévő Redis gyorsítótár</span><span class="sxs-lookup"><span data-stu-id="b4381-197">Renew the authentication key for an existing Redis Cache</span></span>
<span data-ttu-id="b4381-198">A hitelesítési kulcs megújításához egy meglévő Redis gyorsítótár, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b4381-198">To renew the authentication key for an existing Redis Cache, use the following command:</span></span>

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

<span data-ttu-id="b4381-199">Adja meg `Primary` vagy `Secondary` a `key-type`.</span><span class="sxs-lookup"><span data-stu-id="b4381-199">Specify `Primary` or `Secondary` for `key-type`.</span></span>

<span data-ttu-id="b4381-200">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache renew-key -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-200">For more information about this command, run the `azure rediscache renew-key -h` command.</span></span>

    C:\>azure rediscache renew-key -h
    help:    Renew the authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key to renew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a><span data-ttu-id="b4381-201">Egy meglévő Redis gyorsítótár lista elsődleges és másodlagos kulcsok</span><span class="sxs-lookup"><span data-stu-id="b4381-201">List Primary and Secondary keys of an existing Redis Cache</span></span>
<span data-ttu-id="b4381-202">Lista elsődleges és másodlagos kulcsok egy meglévő Redis gyorsítótár használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b4381-202">To list Primary and Secondary keys of an existing Redis Cache, use the following command:</span></span>

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

<span data-ttu-id="b4381-203">Ez a parancs kapcsolatos további információkért futtassa a `azure rediscache list-keys -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="b4381-203">For more information about this command, run the `azure rediscache list-keys -h` command.</span></span>

    C:\>azure rediscache list-keys -h
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:
    help:    Usage: rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of the Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of the Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      the subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

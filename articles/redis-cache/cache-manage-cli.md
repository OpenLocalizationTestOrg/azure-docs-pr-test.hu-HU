---
title: "aaaManage Azure Redis Cache-gyorsítótár Azure parancssori felületével |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello Azure CLI bármilyen platformon, hogyan toouse azt tooconnect tooyour Azure-fiókra, és hogyan toocreate és a Redis gyorsítótár hello Azure parancssori Felülettel kezelhetők."
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
ms.openlocfilehash: e8ee30090133e6b4edea93dcd13fd171e75989bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-azure-redis-cache-using-hello-azure-command-line-interface-azure-cli"></a><span data-ttu-id="55289-103">Hogyan toocreate és kezelése az Azure Redis Cache hello Azure parancssori felület (CLI) használatával</span><span class="sxs-lookup"><span data-stu-id="55289-103">How toocreate and manage Azure Redis Cache using hello Azure Command-Line Interface (Azure CLI)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="55289-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="55289-104">PowerShell</span></span>](cache-howto-manage-redis-cache-powershell.md)
> * [<span data-ttu-id="55289-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="55289-105">Azure CLI</span></span>](cache-manage-cli.md)
>
>

<span data-ttu-id="55289-106">hello Azure parancssori felület egy kiváló módja toomanage az Azure-infrastruktúra bármely platformról van.</span><span class="sxs-lookup"><span data-stu-id="55289-106">hello Azure CLI is a great way toomanage your Azure infrastructure from any platform.</span></span> <span data-ttu-id="55289-107">Ez a cikk bemutatja, hogyan toocreate és kezelése az Azure Redis Cache példány hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="55289-107">This article shows you how toocreate and manage your Azure Redis Cache instances using hello Azure CLI.</span></span>

> [!NOTE]
> <span data-ttu-id="55289-108">Ez a cikk tooa korábbi verziójának Azure parancssori felület vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="55289-108">This article applies tooa previous version of Azure CLI.</span></span> <span data-ttu-id="55289-109">Hello legújabb Azure CLI 2.0 mintaparancsfájlok, lásd: [Azure CLI Redis gyorsítótár minták](cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="55289-109">For hello latest Azure CLI 2.0 sample scripts, see [Azure CLI Redis cache samples](cli-samples.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="55289-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="55289-110">Prerequisites</span></span>
<span data-ttu-id="55289-111">toocreate és kezelése az Azure parancssori felület használatával Azure Redis Cache példányt, el kell végeznie az alábbi lépésekkel hello.</span><span class="sxs-lookup"><span data-stu-id="55289-111">toocreate and manage Azure Redis Cache instances using Azure CLI, you must complete hello following steps.</span></span>

* <span data-ttu-id="55289-112">Azure-fiókkal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="55289-112">You must have an Azure account.</span></span> <span data-ttu-id="55289-113">Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="55289-113">If you don't have one, you can create a [free account](https://azure.microsoft.com/pricing/free-trial/) in just a few moments.</span></span>
* <span data-ttu-id="55289-114">[Hello Azure parancssori felület telepítése](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="55289-114">[Install hello Azure CLI](../cli-install-nodejs.md).</span></span>
* <span data-ttu-id="55289-115">Csatlakozás az Azure parancssori felület telepítése személyes Azure-fiókkal, vagy a munkahelyi vagy iskolai Azure-fiókot, majd jelentkezzen be a hello hello használata az Azure CLI `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-115">Connect your Azure CLI installation with a personal Azure account, or with a work or school Azure account, and log in from hello Azure CLI using hello `azure login` command.</span></span> <span data-ttu-id="55289-116">toounderstand hello különbségeket, és válassza ki, lásd: [tooan Azure-előfizetés csatlakoztatja a hello Azure parancssori felület (CLI)](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="55289-116">toounderstand hello differences and choose, see [Connect tooan Azure subscription from hello Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="55289-117">Mielőtt futtatná hello a következő parancsok valamelyikét, váltson hello Azure CLI Resource Manager módra hello futtatásával `azure config mode arm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-117">Before running any of hello following commands, switch hello Azure CLI into Resource Manager mode by running hello `azure config mode arm` command.</span></span> <span data-ttu-id="55289-118">További információkért lásd: [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások](../xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="55289-118">For more information, see [Use hello Azure CLI toomanage Azure resources and resource groups](../xplat-cli-azure-resource-manager.md).</span></span>

## <a name="redis-cache-properties"></a><span data-ttu-id="55289-119">Redis gyorsítótár tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="55289-119">Redis Cache properties</span></span>
<span data-ttu-id="55289-120">hello következő tulajdonságok használata, amikor létrehozása és frissítése a Redis Cache példányt.</span><span class="sxs-lookup"><span data-stu-id="55289-120">hello following properties are used when creating and updating Redis Cache instances.</span></span>

| <span data-ttu-id="55289-121">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="55289-121">Property</span></span> | <span data-ttu-id="55289-122">Kapcsoló</span><span class="sxs-lookup"><span data-stu-id="55289-122">Switch</span></span> | <span data-ttu-id="55289-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="55289-123">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="55289-124">név</span><span class="sxs-lookup"><span data-stu-id="55289-124">name</span></span> |<span data-ttu-id="55289-125">-n,--neve</span><span class="sxs-lookup"><span data-stu-id="55289-125">-n, --name</span></span> |<span data-ttu-id="55289-126">Redis gyorsítótár hello neve.</span><span class="sxs-lookup"><span data-stu-id="55289-126">Name of hello Redis Cache.</span></span> |
| <span data-ttu-id="55289-127">erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="55289-127">resource group</span></span> |<span data-ttu-id="55289-128">-g,--erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="55289-128">-g, --resource-group</span></span> |<span data-ttu-id="55289-129">Hello erőforráscsoport neve.</span><span class="sxs-lookup"><span data-stu-id="55289-129">Name of hello Resource Group.</span></span> |
| <span data-ttu-id="55289-130">location</span><span class="sxs-lookup"><span data-stu-id="55289-130">location</span></span> |<span data-ttu-id="55289-131">-l,--helye</span><span class="sxs-lookup"><span data-stu-id="55289-131">-l, --location</span></span> |<span data-ttu-id="55289-132">Toocreate gyorsítótár helyét.</span><span class="sxs-lookup"><span data-stu-id="55289-132">Location toocreate cache.</span></span> |
| <span data-ttu-id="55289-133">Méret</span><span class="sxs-lookup"><span data-stu-id="55289-133">size</span></span> |<span data-ttu-id="55289-134">-z, a--mérete</span><span class="sxs-lookup"><span data-stu-id="55289-134">-z, --size</span></span> |<span data-ttu-id="55289-135">Hello Redis Cache-gyorsítótár méretét.</span><span class="sxs-lookup"><span data-stu-id="55289-135">Size of hello Redis Cache.</span></span> <span data-ttu-id="55289-136">Érvényes értékek: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span><span class="sxs-lookup"><span data-stu-id="55289-136">Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]</span></span> |
| <span data-ttu-id="55289-137">Termékváltozat</span><span class="sxs-lookup"><span data-stu-id="55289-137">sku</span></span> |<span data-ttu-id="55289-138">-x,--termékváltozat</span><span class="sxs-lookup"><span data-stu-id="55289-138">-x, --sku</span></span> |<span data-ttu-id="55289-139">Redis Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="55289-139">Redis SKU.</span></span> <span data-ttu-id="55289-140">Egyike lehet: [alapszintű, Standard, Premium]</span><span class="sxs-lookup"><span data-stu-id="55289-140">Should be one of : [Basic, Standard, Premium]</span></span> |
| <span data-ttu-id="55289-141">enableNonSslPort</span><span class="sxs-lookup"><span data-stu-id="55289-141">EnableNonSslPort</span></span> |<span data-ttu-id="55289-142">-e,--enable-nem-ssl-port</span><span class="sxs-lookup"><span data-stu-id="55289-142">-e, --enable-non-ssl-port</span></span> |<span data-ttu-id="55289-143">Redis gyorsítótár hello EnableNonSslPort tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="55289-143">EnableNonSslPort property of hello Redis Cache.</span></span> <span data-ttu-id="55289-144">Adja hozzá ezt a jelzőt, ha a gyorsítótár kívánt tooenable hello nem SSL Port</span><span class="sxs-lookup"><span data-stu-id="55289-144">Add this flag if you want tooenable hello Non SSL Port for your cache</span></span> |
| <span data-ttu-id="55289-145">Konfigurációs redis</span><span class="sxs-lookup"><span data-stu-id="55289-145">Redis Configuration</span></span> |<span data-ttu-id="55289-146">-c,--redis-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="55289-146">-c, --redis-configuration</span></span> |<span data-ttu-id="55289-147">Konfigurációs redis.</span><span class="sxs-lookup"><span data-stu-id="55289-147">Redis Configuration.</span></span> <span data-ttu-id="55289-148">Konfigurációs kulcsok és értékek itt egy JSON formátumú karakterláncot adjon meg.</span><span class="sxs-lookup"><span data-stu-id="55289-148">Enter a JSON formatted string of configuration keys and values here.</span></span> <span data-ttu-id="55289-149">Formátum: "{" ":""," ":" "}"</span><span class="sxs-lookup"><span data-stu-id="55289-149">Format:"{"":"","":""}"</span></span> |
| <span data-ttu-id="55289-150">Konfigurációs redis</span><span class="sxs-lookup"><span data-stu-id="55289-150">Redis Configuration</span></span> |<span data-ttu-id="55289-151">-f,--redis-konfiguráció-fájl</span><span class="sxs-lookup"><span data-stu-id="55289-151">-f, --redis-configuration-file</span></span> |<span data-ttu-id="55289-152">Konfigurációs redis.</span><span class="sxs-lookup"><span data-stu-id="55289-152">Redis Configuration.</span></span> <span data-ttu-id="55289-153">Adja meg a hello konfigurációs kulcsok és értékek itt tartalmazó fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="55289-153">Enter hello path of a file containing configuration keys and values here.</span></span> <span data-ttu-id="55289-154">Hello fájl bejegyzés formátum: {"": "","": ""}</span><span class="sxs-lookup"><span data-stu-id="55289-154">Format for hello file entry: {"":"","":""}</span></span> |
| <span data-ttu-id="55289-155">A shard száma</span><span class="sxs-lookup"><span data-stu-id="55289-155">Shard Count</span></span> |<span data-ttu-id="55289-156">-r,--shard-száma</span><span class="sxs-lookup"><span data-stu-id="55289-156">-r, --shard-count</span></span> |<span data-ttu-id="55289-157">A prémium szintű fürt gyorsítótár fürtözési szilánkok toocreate száma.</span><span class="sxs-lookup"><span data-stu-id="55289-157">Number of Shards toocreate on a Premium Cluster Cache with clustering.</span></span> |
| <span data-ttu-id="55289-158">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="55289-158">Virtual Network</span></span> |<span data-ttu-id="55289-159">-v, – virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="55289-159">-v, --virtual-network</span></span> |<span data-ttu-id="55289-160">Ha megadja a gyorsítótár a VNETEN belül üzemeltető hello pontos ARM erőforrás-azonosítója, hello virtuális hálózati toodeploy hello redis gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="55289-160">When hosting your cache in a VNET, specifies hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in.</span></span> <span data-ttu-id="55289-161">Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="55289-161">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="55289-162">kulcs típusa</span><span class="sxs-lookup"><span data-stu-id="55289-162">key type</span></span> |<span data-ttu-id="55289-163">-t,--kulcs-típus</span><span class="sxs-lookup"><span data-stu-id="55289-163">-t, --key-type</span></span> |<span data-ttu-id="55289-164">Kulcs toorenew típusú.</span><span class="sxs-lookup"><span data-stu-id="55289-164">Type of key toorenew.</span></span> <span data-ttu-id="55289-165">Érvényes értékek: [elsődleges, másodlagos]</span><span class="sxs-lookup"><span data-stu-id="55289-165">Valid values: [Primary, Secondary]</span></span> |
| <span data-ttu-id="55289-166">StaticIP</span><span class="sxs-lookup"><span data-stu-id="55289-166">StaticIP</span></span> |<span data-ttu-id="55289-167">-p, – statikus ip-< ip-statikus ></span><span class="sxs-lookup"><span data-stu-id="55289-167">-p, --static-ip <static-ip></span></span> |<span data-ttu-id="55289-168">Esetén a VNETEN belül a gyorsítótárhoz, adja meg egy egyedi IP-cím hello gyorsítótár hello IP-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="55289-168">When hosting your cache in a VNET, specifies a unique IP address in hello subnet for hello cache.</span></span> <span data-ttu-id="55289-169">Ha nem ad meg, egy választja meg hello alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="55289-169">If not provided, one is chosen for you from hello subnet.</span></span> |
| <span data-ttu-id="55289-170">Alhálózat</span><span class="sxs-lookup"><span data-stu-id="55289-170">Subnet</span></span> |<span data-ttu-id="55289-171">t,--alhálózati<subnet></span><span class="sxs-lookup"><span data-stu-id="55289-171">t, --subnet <subnet></span></span> |<span data-ttu-id="55289-172">Esetén a VNETEN belül a gyorsítótárhoz, mely toodeploy hello gyorsítótárban a hello alhálózati hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="55289-172">When hosting your cache in a VNET, specifies hello name of hello subnet in which toodeploy hello cache.</span></span> |
| <span data-ttu-id="55289-173">VirtualNetwork</span><span class="sxs-lookup"><span data-stu-id="55289-173">VirtualNetwork</span></span> |<span data-ttu-id="55289-174">-v rendszerben--virtuális-< virtuális hálózatok ></span><span class="sxs-lookup"><span data-stu-id="55289-174">-v, --virtual-network <virtual-network></span></span> |<span data-ttu-id="55289-175">Ha megadja a gyorsítótár a VNETEN belül üzemeltető hello pontos ARM erőforrás-azonosítója, hello virtuális hálózati toodeploy hello redis gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="55289-175">When hosting your cache in a VNET, specifies hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in.</span></span> <span data-ttu-id="55289-176">Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span><span class="sxs-lookup"><span data-stu-id="55289-176">Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1</span></span> |
| <span data-ttu-id="55289-177">Előfizetés</span><span class="sxs-lookup"><span data-stu-id="55289-177">Subscription</span></span> |<span data-ttu-id="55289-178">-s,--előfizetés</span><span class="sxs-lookup"><span data-stu-id="55289-178">-s, --subscription</span></span> |<span data-ttu-id="55289-179">hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="55289-179">hello subscription identifier.</span></span> |

## <a name="see-all-redis-cache-commands"></a><span data-ttu-id="55289-180">Tekintse meg az összes Redis Cache-parancsok</span><span class="sxs-lookup"><span data-stu-id="55289-180">See all Redis Cache commands</span></span>
<span data-ttu-id="55289-181">toosee minden Redis Cache parancsot és paramétereket, használja a hello `azure rediscache -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-181">toosee all Redis Cache commands and their parameters, use hello `azure rediscache -h` command.</span></span>

    C:\>azure rediscache -h
    help:    Commands toomanage your Azure Redis Cache(s)
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
    help:    Renew hello authentication key for an existing Redis Cache
    help:      rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Lists Primary and Secondary key of an existing Redis Cache
    help:      rediscache list-keys [--name <name> --resource-group <resource-group>]
    help:
    help:    Options:
    help:      -h, --help  output usage information
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="create-a-redis-cache"></a><span data-ttu-id="55289-182">Redis Cache létrehozása</span><span class="sxs-lookup"><span data-stu-id="55289-182">Create a Redis Cache</span></span>
<span data-ttu-id="55289-183">egy Redis gyorsítótárhoz toocreate hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="55289-183">toocreate a Redis Cache, use hello following command:</span></span>

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

<span data-ttu-id="55289-184">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache create -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-184">For more information about this command, run hello `azure rediscache create -h` command.</span></span>

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
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -l, --location <location>                                Location toocreate cache.
    help:      -z, --size <size>                                        Size of hello Redis Cache. Valid values: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4]
    help:      -x, --sku <sku>                                          Redis SKU. Should be one of : [Basic, Standard, Premium]
    help:      -e, --enable-non-ssl-port                                EnableNonSslPort property of hello Redis Cache. Add this flag if you want tooenable hello Non SSL Port for your cache
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here. Format:"{"<key1>":"<value1>","<key2>":"<value2>"}"
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here. Format for hello file entry: {"<key1>":"<value1>","<key2>":"<value2>"}
    help:      -r, --shard-count <shard-count>                          Number of Shards toocreate on a Premium Cluster Cache
    help:      -v, --virtual-network <virtual-network>                  hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1
    help:      -t, --subnet <subnet>                                    Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -p, --static-ip <static-ip>                              Required when deploying a redis cache inside an existing Azure Virtual Network
    help:      -s, --subscription <id>                                  hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="delete-an-existing-redis-cache"></a><span data-ttu-id="55289-185">Egy meglévő Redis Cache törlése</span><span class="sxs-lookup"><span data-stu-id="55289-185">Delete an existing Redis Cache</span></span>
<span data-ttu-id="55289-186">egy Redis gyorsítótárhoz toodelete hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="55289-186">toodelete a Redis Cache, use hello following command:</span></span>

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

<span data-ttu-id="55289-187">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache delete -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-187">For more information about this command, run hello `azure rediscache delete -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which hello cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a><span data-ttu-id="55289-188">Minden Redis Cache-gyorsítótárak az előfizetéshez vagy erőforráscsoporthoz belül felsorolása</span><span class="sxs-lookup"><span data-stu-id="55289-188">List all Redis Caches within your Subscription or Resource Group</span></span>
<span data-ttu-id="55289-189">toolist összes Redis Cache-gyorsítótárak az előfizetéshez vagy erőforráscsoporthoz, belül használja hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="55289-189">toolist all Redis Caches within your Subscription or Resource Group, use hello following command:</span></span>

    azure rediscache list [options]

<span data-ttu-id="55289-190">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache list -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-190">For more information about this command, run hello `azure rediscache list -h` command.</span></span>

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
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="show-properties-of-an-existing-redis-cache"></a><span data-ttu-id="55289-191">Egy meglévő Redis Cache tulajdonságainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="55289-191">Show properties of an existing Redis Cache</span></span>
<span data-ttu-id="55289-192">egy meglévő Redis Cache tooshow tulajdonságainak hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="55289-192">tooshow properties of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache show [--name <name> --resource-group <resource-group>]

<span data-ttu-id="55289-193">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache show -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-193">For more information about this command, run hello `azure rediscache show -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

<a name="scale"></a>

## <a name="change-settings-of-an-existing-redis-cache"></a><span data-ttu-id="55289-194">Egy meglévő Redis gyorsítótár beállításainak módosítása</span><span class="sxs-lookup"><span data-stu-id="55289-194">Change settings of an existing Redis Cache</span></span>
<span data-ttu-id="55289-195">egy meglévő Redis gyorsítótár beállításainak toochange hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="55289-195">toochange settings of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

<span data-ttu-id="55289-196">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache set -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-196">For more information about this command, run hello `azure rediscache set -h` command.</span></span>

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
    help:      -n, --name <name>                                        Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>                    Name of hello Resource Group
    help:      -c, --redis-configuration <redis-configuration>          Redis Configuration. Enter a JSON formatted string of configuration keys and values here.
    help:      -f, --redis-configuration-file <redisConfigurationFile>  Redis Configuration. Enter hello path of a file containing configuration keys and values here.
    help:      -s, --subscription <subscription>                        hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="renew-hello-authentication-key-for-an-existing-redis-cache"></a><span data-ttu-id="55289-197">Egy meglévő Redis Cache hello hitelesítési kulcs megújítása</span><span class="sxs-lookup"><span data-stu-id="55289-197">Renew hello authentication key for an existing Redis Cache</span></span>
<span data-ttu-id="55289-198">toorenew hello hitelesítési kulcs egy meglévő Redis gyorsítótár, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="55289-198">toorenew hello authentication key for an existing Redis Cache, use hello following command:</span></span>

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

<span data-ttu-id="55289-199">Adja meg `Primary` vagy `Secondary` a `key-type`.</span><span class="sxs-lookup"><span data-stu-id="55289-199">Specify `Primary` or `Secondary` for `key-type`.</span></span>

<span data-ttu-id="55289-200">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache renew-key -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-200">For more information about this command, run hello `azure rediscache renew-key -h` command.</span></span>

    C:\>azure rediscache renew-key -h
    help:    Renew hello authentication key for an existing Redis Cache
    help:
    help:    Usage: rediscache renew-key [--name <name> --resource-group <resource-group> ]
    help:
    help:    Options:
    help:      -h, --help                             output usage information
    help:      -v, --verbose                          use verbose output
    help:      -vv                                    more verbose with debug output
    help:      --json                                 use json output
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which cache exists
    help:      -t, --key-type <key-type>              type of key toorenew. Valid values are: 'Primary', 'Secondary'.
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a><span data-ttu-id="55289-201">Egy meglévő Redis gyorsítótár lista elsődleges és másodlagos kulcsok</span><span class="sxs-lookup"><span data-stu-id="55289-201">List Primary and Secondary keys of an existing Redis Cache</span></span>
<span data-ttu-id="55289-202">egy meglévő Redis gyorsítótár elsődleges és másodlagos kulcsok toolist hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="55289-202">toolist Primary and Secondary keys of an existing Redis Cache, use hello following command:</span></span>

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

<span data-ttu-id="55289-203">Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache list-keys -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="55289-203">For more information about this command, run hello `azure rediscache list-keys -h` command.</span></span>

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
    help:      -n, --name <name>                      Name of hello Redis Cache.
    help:      -g, --resource-group <resource-group>  Name of hello Resource Group under which Cache exists
    help:      -s, --subscription <subscription>      hello subscription identifier
    help:
    help:    Current Mode: arm (Azure Resource Management)

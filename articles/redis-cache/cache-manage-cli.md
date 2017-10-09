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
# <a name="how-toocreate-and-manage-azure-redis-cache-using-hello-azure-command-line-interface-azure-cli"></a>Hogyan toocreate és kezelése az Azure Redis Cache hello Azure parancssori felület (CLI) használatával
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
>
>

hello Azure parancssori felület egy kiváló módja toomanage az Azure-infrastruktúra bármely platformról van. Ez a cikk bemutatja, hogyan toocreate és kezelése az Azure Redis Cache példány hello Azure parancssori felület használatával.

> [!NOTE]
> Ez a cikk tooa korábbi verziójának Azure parancssori felület vonatkozik. Hello legújabb Azure CLI 2.0 mintaparancsfájlok, lásd: [Azure CLI Redis gyorsítótár minták](cli-samples.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
toocreate és kezelése az Azure parancssori felület használatával Azure Redis Cache példányt, el kell végeznie az alábbi lépésekkel hello.

* Azure-fiókkal kell rendelkeznie. Ha még nincs fiókja, létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/) csak néhány perc múlva.
* [Hello Azure parancssori felület telepítése](../cli-install-nodejs.md).
* Csatlakozás az Azure parancssori felület telepítése személyes Azure-fiókkal, vagy a munkahelyi vagy iskolai Azure-fiókot, majd jelentkezzen be a hello hello használata az Azure CLI `azure login` parancsot. toounderstand hello különbségeket, és válassza ki, lásd: [tooan Azure-előfizetés csatlakoztatja a hello Azure parancssori felület (CLI)](../xplat-cli-connect.md).
* Mielőtt futtatná hello a következő parancsok valamelyikét, váltson hello Azure CLI Resource Manager módra hello futtatásával `azure config mode arm` parancsot. További információkért lásd: [használja hello Azure CLI toomanage Azure erőforráscsoport-sablonok és erőforrások](../xplat-cli-azure-resource-manager.md).

## <a name="redis-cache-properties"></a>Redis gyorsítótár tulajdonságai
hello következő tulajdonságok használata, amikor létrehozása és frissítése a Redis Cache példányt.

| Tulajdonság | Kapcsoló | Leírás |
| --- | --- | --- |
| név |-n,--neve |Redis gyorsítótár hello neve. |
| erőforráscsoport |-g,--erőforráscsoport |Hello erőforráscsoport neve. |
| location |-l,--helye |Toocreate gyorsítótár helyét. |
| Méret |-z, a--mérete |Hello Redis Cache-gyorsítótár méretét. Érvényes értékek: [C0, C1, C2, C3, C4, C5, C6, P1, P2, P3, P4] |
| Termékváltozat |-x,--termékváltozat |Redis Termékváltozat. Egyike lehet: [alapszintű, Standard, Premium] |
| enableNonSslPort |-e,--enable-nem-ssl-port |Redis gyorsítótár hello EnableNonSslPort tulajdonsága. Adja hozzá ezt a jelzőt, ha a gyorsítótár kívánt tooenable hello nem SSL Port |
| Konfigurációs redis |-c,--redis-konfiguráció |Konfigurációs redis. Konfigurációs kulcsok és értékek itt egy JSON formátumú karakterláncot adjon meg. Formátum: "{" ":""," ":" "}" |
| Konfigurációs redis |-f,--redis-konfiguráció-fájl |Konfigurációs redis. Adja meg a hello konfigurációs kulcsok és értékek itt tartalmazó fájl elérési útját. Hello fájl bejegyzés formátum: {"": "","": ""} |
| A shard száma |-r,--shard-száma |A prémium szintű fürt gyorsítótár fürtözési szilánkok toocreate száma. |
| Virtual Network |-v, – virtuális hálózat |Ha megadja a gyorsítótár a VNETEN belül üzemeltető hello pontos ARM erőforrás-azonosítója, hello virtuális hálózati toodeploy hello redis gyorsítótár. Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| kulcs típusa |-t,--kulcs-típus |Kulcs toorenew típusú. Érvényes értékek: [elsődleges, másodlagos] |
| StaticIP |-p, – statikus ip-< ip-statikus > |Esetén a VNETEN belül a gyorsítótárhoz, adja meg egy egyedi IP-cím hello gyorsítótár hello IP-alhálózatot. Ha nem ad meg, egy választja meg hello alhálózatból. |
| Alhálózat |t,--alhálózati<subnet> |Esetén a VNETEN belül a gyorsítótárhoz, mely toodeploy hello gyorsítótárban a hello alhálózati hello nevét adja meg. |
| VirtualNetwork |-v rendszerben--virtuális-< virtuális hálózatok > |Ha megadja a gyorsítótár a VNETEN belül üzemeltető hello pontos ARM erőforrás-azonosítója, hello virtuális hálózati toodeploy hello redis gyorsítótár. Példa formátum: /subscriptions/{subid}/resourceGroups/{resourceGroupName}/Microsoft.ClassicNetwork/VirtualNetworks/vnet1 |
| Előfizetés |-s,--előfizetés |hello előfizetés-azonosító. |

## <a name="see-all-redis-cache-commands"></a>Tekintse meg az összes Redis Cache-parancsok
toosee minden Redis Cache parancsot és paramétereket, használja a hello `azure rediscache -h` parancsot.

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

## <a name="create-a-redis-cache"></a>Redis Cache létrehozása
egy Redis gyorsítótárhoz toocreate hello a következő parancsot használja:

    azure rediscache create [--name <name> --resource-group <resource-group> --location <location> [options]]

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache create -h` parancsot.

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

## <a name="delete-an-existing-redis-cache"></a>Egy meglévő Redis Cache törlése
egy Redis gyorsítótárhoz toodelete hello a következő parancsot használja:

    azure rediscache delete [--name <name> --resource-group <resource-group> ]

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache delete -h` parancsot.

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

## <a name="list-all-redis-caches-within-your-subscription-or-resource-group"></a>Minden Redis Cache-gyorsítótárak az előfizetéshez vagy erőforráscsoporthoz belül felsorolása
toolist összes Redis Cache-gyorsítótárak az előfizetéshez vagy erőforráscsoporthoz, belül használja hello a következő parancsot:

    azure rediscache list [options]

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache list -h` parancsot.

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

## <a name="show-properties-of-an-existing-redis-cache"></a>Egy meglévő Redis Cache tulajdonságainak megjelenítése
egy meglévő Redis Cache tooshow tulajdonságainak hello a következő parancsot használja:

    azure rediscache show [--name <name> --resource-group <resource-group>]

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache show -h` parancsot.

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

## <a name="change-settings-of-an-existing-redis-cache"></a>Egy meglévő Redis gyorsítótár beállításainak módosítása
egy meglévő Redis gyorsítótár beállításainak toochange hello a következő parancsot használja:

    azure rediscache set [--name <name> --resource-group <resource-group> --redis-configuration <redis-configuration>/--redis-configuration-file <redisConfigurationFile>]

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache set -h` parancsot.

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

## <a name="renew-hello-authentication-key-for-an-existing-redis-cache"></a>Egy meglévő Redis Cache hello hitelesítési kulcs megújítása
toorenew hello hitelesítési kulcs egy meglévő Redis gyorsítótár, a következő parancs használata hello:

    azure rediscache renew-key [--name <name> --resource-group <resource-group> --key-type <key-type>]

Adja meg `Primary` vagy `Secondary` a `key-type`.

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache renew-key -h` parancsot.

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

## <a name="list-primary-and-secondary-keys-of-an-existing-redis-cache"></a>Egy meglévő Redis gyorsítótár lista elsődleges és másodlagos kulcsok
egy meglévő Redis gyorsítótár elsődleges és másodlagos kulcsok toolist hello a következő parancsot használja:

    azure rediscache list-keys [--name <name> --resource-group <resource-group>]

Ez a parancs kapcsolatos további információkért futtassa a hello `azure rediscache list-keys -h` parancsot.

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

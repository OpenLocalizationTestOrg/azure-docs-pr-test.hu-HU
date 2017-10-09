---
title: Azure Redis Cache az Azure PowerShell aaaManage |} Microsoft Docs
description: "Megtudhatja, hogyan tooperform felügyeleti feladatokat az Azure Redis Cache Azure PowerShell használatával."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1136efe5-1e33-4d91-bb49-c8e2a6dca475
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: 1d526ce65c4bc05345cd6c3ff370211ed562cab4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-redis-cache-with-azure-powershell"></a>Azure Redis gyorsítótár Azure PowerShell kezelése
> [!div class="op_single_selector"]
> * [PowerShell](cache-howto-manage-redis-cache-powershell.md)
> * [Azure CLI](cache-manage-cli.md)
> 
> 

Ez a témakör bemutatja, hogyan tooperform gyakori feladatokat, például egy létrehozása, frissítése és az Azure Redis Cache példány méretezése hogyan tooregenerate kulcsot, és hogyan a gyorsítótárak tooview információt. Azure Redis Cache PowerShell-parancsmagok teljes listáját lásd: [Azure Redis Cache parancsmagok](https://msdn.microsoft.com/library/azure/mt634513.aspx).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

Hello klasszikus üzembe helyezési modellel kapcsolatos további információkért lásd: [Azure Resource Manager és klasszikus üzembe helyezési: üzembe helyezési modellel megértéséhez, valamint az erőforrások állapotát hello](../azure-resource-manager/resource-manager-deployment-model.md#classic-deployment-characteristics).

## <a name="prerequisites"></a>Előfeltételek
Ha már telepítette az Azure PowerShell, rendelkeznie kell Azure PowerShell 1.0.0 verzió vagy újabb. Ellenőrizheti a hello Azure PowerShell ezzel a paranccsal hello Azure PowerShell parancssorban telepített verzióját.

    Get-Module azure | format-table version


Először be kell jelentkeznie tooAzure ezzel a paranccsal.

    Login-AzureRmAccount

A Microsoft Azure-bejelentkezés hello párbeszédpanel hello e-mail címet az Azure-fiókjával, és a hozzá tartozó jelszó megadása

Ezután ha több Azure-előfizetéssel rendelkezik, meg kell tooset az Azure-előfizetéshez. az aktuális előfizetések listája toosee futtassa ezt a parancsot.

    Get-AzureRmSubscription | sort SubscriptionName | Select SubscriptionName

toospecify hello előfizetés, futtassa a következő parancs hello. A következő példa hello, hello előfizetés neve: `ContosoSubscription`.

    Select-AzureRmSubscription -SubscriptionName ContosoSubscription

A Windows PowerShell használhatja az Azure Resource Manager eszközzel, hello következőkre lesz szüksége:

* Windows PowerShell 3.0 vagy 4.0-s verzióját. a Windows Powershellt, írja be a toofind hello verziója:`$PSVersionTable` , és ellenőrizze a hello értékének `PSVersion` 3.0 vagy 4.0-s verzióját. tooinstall kompatibilitását, lásd: [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) vagy [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

tooget részletes a jelen oktatóanyag esetében használja a Get-Help parancsmagot hello látni a parancsmag súgóját.

    Get-Help <cmdlet-name> -Detailed

Hello például tooget súgóját `New-AzureRmRedisCache` parancsmag, típus:

    Get-Help New-AzureRmRedisCache -Detailed

### <a name="how-tooconnect-tooother-clouds"></a>Hogyan tooconnect tooother felhők
Alapértelmezett hello Azure környezetben az `AzureCloud`, amely jelöli hello globális Azure felhőben példány. tooconnect tooa másik példányt, használjon hello `Add-AzureRmAccount` hello parancsot `-Environment` vagy -`EnvironmentName` hello kívánt környezet vagy a környezet nevű parancssori kapcsolóval.

rendelkezésre álló környezetekben, futtassa a hello toosee hello listája `Get-AzureRmEnvironment` parancsmag.

### <a name="tooconnect-toohello-azure-government-cloud"></a>tooconnect toohello Azure Government felhő
tooconnect toohello Azure Government felhő, hello a következő parancsok egyikét használhatja.

    Add-AzureRMAccount -EnvironmentName AzureUSGovernment

vagy

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureUSGovernment)

hello Azure Government felhőbe, a gyorsítótárhoz toocreate hello alábbi helyek egyikét használhatja.

* USGov Virginia
* USGov Iowa

Hello Azure Government felhő kapcsolatos további információkért lásd: [a Microsoft Azure Government](https://azure.microsoft.com/features/gov/) és [Microsoft Azure Government – útmutató fejlesztőknek](../azure-government-developer-guide.md).

### <a name="tooconnect-toohello-azure-china-cloud"></a>tooconnect toohello Azure Kína felhő
tooconnect toohello Azure Kína felhő, hello a következő parancsok egyikét használhatja.

    Add-AzureRMAccount -EnvironmentName AzureChinaCloud

vagy

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureChinaCloud)

hello Azure Kína felhőbe, a gyorsítótárhoz toocreate hello alábbi helyek egyikét használhatja.

* Kelet-Kína
* Észak-Kína

Hello Azure Kína felhő kapcsolatos további információkért lásd: [AzureChinaCloud Azure Kínában a 21Vianet által működtetett](http://www.windowsazure.cn/).

### <a name="tooconnect-toomicrosoft-azure-germany"></a>tooconnect tooMicrosoft Azure-Németország
tooconnect tooMicrosoft németországi Azure hello a következő parancsok egyikét használhatja.

    Add-AzureRMAccount -EnvironmentName AzureGermanCloud


vagy

    Add-AzureRmAccount -Environment (Get-AzureRmEnvironment -Name AzureGermanCloud)

egy Microsoft Azure Németországban gyorsítótár toocreate hello alábbi helyek egyikét használhatja.

* Közép-Németország
* Északkelet-Németország

A Microsoft Azure Németország kapcsolatos további információkért lásd: [Microsoft Azure Németország](https://azure.microsoft.com/overview/clouds/germany/).

### <a name="properties-used-for-azure-redis-cache-powershell"></a>Azure Redis Cache PowerShell használt tulajdonságok
a következő táblázat hello tulajdonságok és a gyakran használt paraméterek létrehozása és kezelése az Azure Redis Cache példányt az Azure PowerShell leírását tartalmazza.

| Paraméter | Leírás | Alapértelmezett |
| --- | --- | --- |
| Név |Hello gyorsítótár neve | |
| Hely |Hello gyorsítótár helye | |
| erőforráscsoport-név |Az erőforráscsoport neve mely toocreate hello gyorsítótárban | |
| Méret |hello hello gyorsítótár méretét. Érvényes értékek a következők: P1, P2, P3, P4, C0, C1, C2, C3, C4, C5, C6, 250MB, 1 GB-os, 2.5 GB, 6 GB, 13 GB, 26 GB, 53 GB |1 GB |
| ShardCount |a prémium szintű gyorsítótár engedélyezve fürtözési létrehozásakor szilánkok toocreate hello száma. Érvényes értékek a következők: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 | |
| SKU |Megadja a hello hello gyorsítótár Termékváltozat. Érvényes értékek a következők: Basic, Standard, Premium |Standard |
| RedisConfiguration |Határozza meg a Redis-konfigurációs beállításokat. Minden beállítás a részletekért lásd: hello következő [RedisConfiguration tulajdonságok](#redisconfiguration-properties) tábla. | |
| enableNonSslPort |Azt jelzi, hogy engedélyezve van-e hello nem SSL port. |False (Hamis) |
| MaxMemoryPolicy |Ez a paraméter elavult – használja inkább a RedisConfiguration. | |
| StaticIP |Esetén a VNETEN belül a gyorsítótárhoz, adja meg egy egyedi IP-cím hello gyorsítótár hello IP-alhálózatot. Ha nem ad meg, egy választja meg hello alhálózatból. | |
| Alhálózat |Esetén a VNETEN belül a gyorsítótárhoz, mely toodeploy hello gyorsítótárban a hello alhálózati hello nevét adja meg. | |
| VirtualNetwork |Ha a gyorsítótár a VNETEN belül üzemeltető hello erőforrás Azonosítóját adja meg hello virtuális Hálózatot, mely toodeploy hello gyorsítótárában. | |
| keyType |Meghatározza, mely a hozzáférési kulcsot tooregenerate hívóbetűk megújításakor. Érvényes értékek a következők: elsődleges, másodlagos | |

### <a name="redisconfiguration-properties"></a>RedisConfiguration tulajdonságai
| Tulajdonság | Leírás | Árképzési szintek |
| --- | --- | --- |
| rekordadatbázis biztonsági mentés engedélyezve |E [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) engedélyezve van |Csak a prémium |
| rekordadatbázis-storage-kapcsolat-karakterlánc |kapcsolati karakterlánc toohello tárfiók hello [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) |Csak a prémium |
| biztonsági mentés-gyakori rekordadatbázis |biztonsági mentési gyakorisága hello [Redis-adatmegőrzés](cache-how-to-premium-persistence.md) |Csak a prémium |
| maxmemory fenntartott |Konfigurálja a hello [fenntartott memória](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) nem gyorsítótár folyamatok |Standard és Premium |
| maxmemory-házirend |Konfigurálja a hello [kiürítés házirend](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) hello gyorsítótár |Minden tarifacsomagok |
| értesítés-kulcstérértesítések használatával-események |Konfigurálja az [kulcstérértesítések használatával értesítések](cache-configure.md#keyspace-notifications-advanced-settings) |Standard és Premium |
| kivonat-max-ziplist-bejegyzések |Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Premium |
| kivonat-max-ziplist-értéke |Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Premium |
| set-maximális-intset-bejegyzések |Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Premium |
| zset-maximális-ziplist-bejegyzések |Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Premium |
| zset-maximális-ziplist-érték |Konfigurálja az [memóriaoptimalizálási](http://redis.io/topics/memory-optimization) kis összesített adatok esetében |Standard és Premium |
| adatbázisok |Konfigurálja az adatbázisok hello számát. Ez a tulajdonság csak a gyorsítótár létrehozásakor konfigurálható. |Standard és Premium |

## <a name="toocreate-a-redis-cache"></a>egy Redis gyorsítótárhoz toocreate
Új Azure Redis Cache példány hello használatával hozhatók létre [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag.

> [!IMPORTANT]
> hello első alkalommal hoz létre a Redis gyorsítótár hello Azure-portál használatával előfizetés hello portal regisztrálja hello `Microsoft.Cache` adott előfizetéshez tartozó névtér. Ha úgy próbálja toocreate hello először Redis gyorsítótár a PowerShell használatával előfizetés, először regisztrálnia kell a névtér a következő parancs; hello használata például más parancsmagok `New-AzureRmRedisCache` és `Get-AzureRmRedisCache` sikertelen.
> 
> `Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Cache"`
> 
> 

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `New-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help New-AzureRmRedisCache -detailed

    NAME
        New-AzureRmRedisCache

    SYNOPSIS
        Creates a new redis cache.


    SYNTAX
        New-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Location <String> [-RedisVersion <String>]
        [-Size <String>] [-Sku <String>] [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort
        <Boolean>] [-ShardCount <Integer>] [-VirtualNetwork <String>] [-Subnet <String>] [-StaticIP <String>]
        [<CommonParameters>]


    DESCRIPTION
        hello New-AzureRmRedisCache cmdlet creates a new redis cache.


    PARAMETERS
        -Name <String>
            Name of hello redis cache toocreate.

        -ResourceGroupName <String>
            Name of resource group in which toocreate hello redis cache.

        -Location <String>
            Location in which toocreate hello redis cache.

        -RedisVersion <String>
            RedisVersion is deprecated and will be removed in future release.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value, databases.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. If no value is provided, hello default value is false and the
            non-SSL port will be disabled. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        -VirtualNetwork <String>
            hello exact ARM resource ID of hello virtual network toodeploy hello redis cache in. Example format: /subscriptions/{
            subid}/resourceGroups/{resourceGroupName}/providers/Microsoft.ClassicNetwork/VirtualNetworks/{vnetName}

        -Subnet <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        -StaticIP <String>
            Required when deploying a redis cache inside an existing Azure Virtual Network.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

a gyorsítótár alapértelmezett paraméterek, futtassa a következő parancs hello toocreate.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US"

`ResourceGroupName`, `Name`, és `Location` kötelező paraméterek tartoznak, de hello rest opcionálisak, az alapértelmezett értékek szerint vannak. Hello előző parancs futtatása hoz létre egy Standard Termékváltozat Azure Redis Cache példány hello megadott nevét, helyét és erőforráscsoport, 1 GB méretű hello nem SSL port le van tiltva van.

a prémium szintű gyorsítótár toocreate P1 (6 GB - 60 GB), (13 GB - 130 GB), P2 méretet adjon meg P3 (26 GB - 260 GB), vagy P4 (53 GB - 530 GB). Fürtszolgáltatás, tooenable hello segítségével a shard számot megadnia `ShardCount` paraméter. hello alábbi példa hoz létre egy premium P1 gyorsítótár 3 szilánkok. Premium P1 gyorsítótár 6 GB-nál, és mivel azt a három szilánkok hello teljes mérete adott 18 GB (3 x 6 GB).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P1 -ShardCount 3

hello toospecify értékeinek `RedisConfiguration` paraméter, tegye a hello értékek belül `{}` kulcs/érték párok, például `@{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}`. hello alábbi példakód létrehozza szabványos 1 GB-os gyorsítótár `allkeys-random` maxmemory a beállított házirend- és kulcstérértesítések használatával értesítések `KEA`. További információkért lásd: [kulcstérértesítések használatával értesítések (Speciális beállítások)](cache-configure.md#keyspace-notifications-advanced-settings) és [memória házirendek](cache-configure.md#memory-policies).

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random", "notify-keyspace-events" = "KEA"}

<a name="databases"></a>

## <a name="tooconfigure-hello-databases-setting-during-cache-creation"></a>tooconfigure hello adatbázisok beállítása a gyorsítótár létrehozása közben
Hello `databases` beállítást csak a gyorsítótár létrehozása közben lehet megadni. hello alábbi példa létrehoz egy prémium P3 (26 GB-os) gyorsítótárat hello segítségével 48 adatbázisok [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag.

    New-AzureRmRedisCache -ResourceGroupName myGroup -Name mycache -Location "North Central US" -Sku Premium -Size P3 -RedisConfiguration @{"databases" = "48"}

További információ a hello `databases` tulajdonság, lásd: [Azure Redis Cache alapértelmezett kiszolgálókonfiguráció](cache-configure.md#default-redis-server-configuration). További létrehozásával kapcsolatos információkat hello gyorsítótár [New-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634517.aspx) parancsmag, tekintse meg az előző hello [toocreate egy Redis gyorsítótárhoz](#to-create-a-redis-cache) szakasz.

## <a name="tooupdate-a-redis-cache"></a>a Redis gyorsítótár tooupdate
Azure Redis Cache példány frissítése hello segítségével [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) parancsmag.

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Set-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Set-AzureRmRedisCache -detailed

    NAME
        Set-AzureRmRedisCache

    SYNOPSIS
        Set redis cache updatable parameters.

    SYNTAX
        Set-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Size <String>] [-Sku <String>]
        [-MaxMemoryPolicy <String>] [-RedisConfiguration <Hashtable>] [-EnableNonSslPort <Boolean>] [-ShardCount
        <Integer>] [<CommonParameters>]

    DESCRIPTION
        hello Set-AzureRmRedisCache cmdlet sets redis cache parameters.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooupdate.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -Size <String>
            Size of hello redis cache. hello default value is 1GB or C1. Possible values are P1, P2, P3, P4, C0, C1, C2, C3,
            C4, C5, C6, 250MB, 1GB, 2.5GB, 6GB, 13GB, 26GB, 53GB.

        -Sku <String>
            Sku of redis cache. hello default value is Standard. Possible values are Basic, Standard and Premium.

        -MaxMemoryPolicy <String>
            hello 'MaxMemoryPolicy' setting has been deprecated. Please use 'RedisConfiguration' setting tooset
            MaxMemoryPolicy. e.g. -RedisConfiguration @{"maxmemory-policy" = "allkeys-lru"}

        -RedisConfiguration <Hashtable>
            All Redis Configuration Settings. Few possible keys: rdb-backup-enabled, rdb-storage-connection-string,
            rdb-backup-frequency, maxmemory-reserved, maxmemory-policy, notify-keyspace-events, hash-max-ziplist-entries,
            hash-max-ziplist-value, set-max-intset-entries, zset-max-ziplist-entries, zset-max-ziplist-value.

        -EnableNonSslPort <Boolean>
            EnableNonSslPort is used by Azure Redis Cache. hello default value is null and no change will be made toothe
            currently configured value. Possible values are true and false.

        -ShardCount <Integer>
            hello number of shards toocreate on a Premium Cluster Cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

Hello `Set-AzureRmRedisCache` parancsmag lehetnek többek között használt tooupdate tulajdonságok `Size`, `Sku`, `EnableNonSslPort`, és hello `RedisConfiguration` értékeket. 

hello frissítések hello hello Redis Cache-maxmemory-házirend a következő parancs nevű myCache.

    Set-AzureRmRedisCache -ResourceGroupName "myGroup" -Name "myCache" -RedisConfiguration @{"maxmemory-policy" = "allkeys-random"}

<a name="scale"></a>

## <a name="tooscale-a-redis-cache"></a>a Redis gyorsítótár tooscale
`Set-AzureRmRedisCache`az Azure Redis cache példány amikor hello lehet használt tooscale `Size`, `Sku`, vagy `ShardCount` tulajdonság módosítását mutatjuk be. 

> [!NOTE]
> Skálázás a PowerShell használatával gyorsítótár tulajdonos toohello azonos korlátok és útmutatók a gyorsítótár, a méretezés hello Azure-portálon. Másik tarifacsomagra vált a következő korlátozások hello tooa méretezheti.
> 
> * A magasabb árképzési szint tooa, alacsonyabb árképzési szint nem lehet méretezni.
> * Nem lehet méretezni a egy **prémium** gyorsítótár le tooa **szabványos** vagy egy **alapvető** gyorsítótár.
> * Nem lehet méretezni a egy **szabványos** gyorsítótár le tooa **alapvető** gyorsítótár.
> * A méretezheti egy **alapvető** tooa gyorsítótár **szabványos** gyorsítótár, de nem módosíthatja a hello mérete: hello ugyanannyi időt vesz igénybe. Ha különböző méretű van szüksége, a későbbi skálázási művelet szükséges toohello mérete teheti meg.
> * Nem lehet méretezni a egy **alapvető** gyorsítótár közvetlenül tooa **prémium** gyorsítótár. Kell méretezni a **alapvető** túl**szabványos** egy skálázási műveletet, majd a **szabványos** túl**prémium** a későbbi skálázás a műveletet.
> * Nem lehet méretezni a nagyobb méretű le toohello **C0 csomag (250 MB)** méretét.
> 
> További információkért lásd: [hogyan tooScale Azure Redis Cache-gyorsítótár](cache-how-to-scale.md).
> 
> 

hello következő példa bemutatja, hogyan tooscale gyorsítótár nevű `myCache` tooa 2,5 GB gyorsítótár. Vegye figyelembe, hogy ez a parancs az alapszintű vagy Standard gyorsítótár is működik-e.

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Ez a parancs kiadása után hello gyorsítótár hello állapotának ad vissza (hasonló toocalling `Get-AzureRmRedisCache`). Vegye figyelembe, hogy hello `ProvisioningState` van `Scaling`.

    PS C:\> Set-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup -Size 2.5GB


    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/mygroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Scaling
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 150], [notify-keyspace-events, KEA],
                         [maxmemory-delta, 150]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : mygroup
    PrimaryKey         : ....
    SecondaryKey       : ....
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

Hello skálázás művelet befejeződése után hello `ProvisioningState` túl változik`Succeeded`. Ha egy későbbi skálázási művelet, például módosítása az alapvető tooStandard és hello méretét, majd módosítja toomake kell várnia kell addig, amíg hello korábbi művelet befejeződik, vagy egy hiba hasonló toohello következő.

    Set-AzureRmRedisCache : Conflict: hello resource '...' is not in a stable state, and is currently unable tooaccept hello update request.

## <a name="tooget-information-about-a-redis-cache"></a>a Redis gyorsítótár tooget információ
Információ a gyorsítótár hello kérheti le [Get-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634514.aspx) parancsmag.

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Get-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Get-AzureRmRedisCache -detailed

    NAME
        Get-AzureRmRedisCache

    SYNOPSIS
        Gets details about a single cache or all caches in hello specified resource group or all caches in hello current
        subscription.

    SYNTAX
        Get-AzureRmRedisCache [-Name <String>] [-ResourceGroupName <String>] [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCache cmdlet gets hello details about a cache or caches depending on input parameters. If both
        ResourceGroupName and Name parameters are provided then Get-AzureRmRedisCache will return details about the
        specific cache name provided.

        If only ResourceGroupName is provided than it will return details about all caches in hello specified resource group.

        If no parameters are given than it will return details about all caches hello current subscription.

    PARAMETERS
        -Name <String>
            hello name of hello cache. When this parameter is provided along with ResourceGroupName, Get-AzureRmRedisCache
            returns hello details for hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache or caches. If ResourceGroupName is provided with Name
            then Get-AzureRmRedisCache returns hello details of hello cache specified by Name. If only hello ResourceGroup
            parameter is provided, then details for all caches in hello resource group are returned.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

hello az aktuális előfizetést, minden gyorsítótárak tooreturn információ futtatása `Get-AzureRmRedisCache` paraméter nélkül.

    Get-AzureRmRedisCache

egy adott erőforráscsoportban található összes gyorsítótárak tooreturn információ futtatása `Get-AzureRmRedisCache` a hello `ResourceGroupName` paraméter.

    Get-AzureRmRedisCache -ResourceGroupName myGroup

tooreturn információk egy adott gyorsítótárral, futtassa `Get-AzureRmRedisCache` a hello `Name` hello nevét, valamint hello gyorsítótár hello tartalmazó paraméter `ResourceGroupName` , hogy a gyorsítótár tartalmazó hello erőforráscsoport paraméterrel.

    PS C:\> Get-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Name               : mycache
    Id                 : /subscriptions/12ad12bd-abdc-2231-a2ed-a2b8b246bbad4/resourceGroups/myGroup/providers/Mi
                         crosoft.Cache/Redis/mycache
    Location           : South Central US
    Type               : Microsoft.Cache/Redis
    HostName           : mycache.redis.cache.windows.net
    Port               : 6379
    ProvisioningState  : Succeeded
    SslPort            : 6380
    RedisConfiguration : {[maxmemory-policy, volatile-lru], [maxmemory-reserved, 62], [notify-keyspace-events, KEA],
                         [maxclients, 1000]...}
    EnableNonSslPort   : False
    RedisVersion       : 3.0
    Size               : 1GB
    Sku                : Standard
    ResourceGroupName  : myGroup
    VirtualNetwork     :
    Subnet             :
    StaticIP           :
    TenantSettings     : {}
    ShardCount         :

## <a name="tooretrieve-hello-access-keys-for-a-redis-cache"></a>tooretrieve hello a Redis gyorsítótár elérési kulcsainak
tooretrieve hello hozzáférési kulcsainak listázása a gyorsítótár, a hello használhatja [Get-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634516.aspx) parancsmag.

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Get-AzureRmRedisCacheKey`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Get-AzureRmRedisCacheKey -detailed

    NAME
        Get-AzureRmRedisCacheKey

    SYNOPSIS
        Gets hello accesskeys for hello specified redis cache.


    SYNTAX
        Get-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> [<CommonParameters>]

    DESCRIPTION
        hello Get-AzureRmRedisCacheKey cmdlet gets hello access keys for hello specified cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

a gyorsítótár, a hívás hello kulcsok tooretrieve hello `Get-AzureRmRedisCacheKey` parancsmag és a gyorsítótár hello nevű fázis hello hello gyorsítótár tartalmazó hello erőforráscsoport nevét.

    PS C:\> Get-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup

    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : ABhfB757JgjIgt785JgKH9865eifmekfnn649303JKL=

## <a name="tooregenerate-access-keys-for-your-redis-cache"></a>a Redis gyorsítótár elérési kulcsainak tooregenerate
tooregenerate hello hozzáférési kulcsainak listázása a gyorsítótár, a hello használhatja [New-AzureRmRedisCacheKey](https://msdn.microsoft.com/library/azure/mt634512.aspx) parancsmag.

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `New-AzureRmRedisCacheKey`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help New-AzureRmRedisCacheKey -detailed

    NAME
        New-AzureRmRedisCacheKey

    SYNOPSIS
        Regenerates hello access key of a redis cache.

    SYNTAX
        New-AzureRmRedisCacheKey -Name <String> -ResourceGroupName <String> -KeyType <String> [-Force] [<CommonParameters>]

    DESCRIPTION
        hello New-AzureRmRedisCacheKey cmdlet regenerate hello access key of a redis cache.

    PARAMETERS
        -Name <String>
            Name of hello redis cache.

        -ResourceGroupName <String>
            Name of hello resource group for hello cache.

        -KeyType <String>
            Specifies whether tooregenerate hello primary or secondary access key. Possible values are Primary or Secondary.

        -Force
            When hello Force parameter is provided, hello specified access key is regenerated without any confirmation prompts.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

tooregenerate hello elsődleges vagy másodlagos kulcsot a gyorsítótár, a hívás hello `New-AzureRmRedisCacheKey` parancsmag és a hello adjon át name, erőforráscsoportot, majd adja meg `Primary` vagy `Secondary` a hello `KeyType` paraméter. A következő példa hello hello másodlagos hozzáférési kulcsot a gyorsítótár újbóli létrehozása.

    PS C:\> New-AzureRmRedisCacheKey -Name myCache -ResourceGroupName myGroup -KeyType Secondary

    Confirm
    Are you sure you want tooregenerate Secondary key for redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


    PrimaryKey   : b2wdt43sfetlju4hfbryfnregrd9wgIcc6IA3zAO1lY=
    SecondaryKey : c53hj3kh4jhHjPJk8l0jji785JgKH9865eifmekfnn6=

## <a name="toodelete-a-redis-cache"></a>a Redis gyorsítótár toodelete
a Redis gyorsítótár toodelete hello használata [Remove-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634515.aspx) parancsmag.

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Remove-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Remove-AzureRmRedisCache -detailed

    NAME
        Remove-AzureRmRedisCache

    SYNOPSIS
        Remove redis cache if exists.

    SYNTAX
        Remove-AzureRmRedisCache -Name <String> -ResourceGroupName <String> [-Force] [-PassThru] [<CommonParameters>

    DESCRIPTION
        hello Remove-AzureRmRedisCache cmdlet removes a redis cache if it exists.

    PARAMETERS
        -Name <String>
            Name of hello redis cache tooremove.

        -ResourceGroupName <String>
            Name of hello resource group of hello cache tooremove.

        -Force
            When hello Force parameter is provided, hello cache is removed without any confirmation prompts.

        -PassThru
            By default Remove-AzureRmRedisCache removes hello cache and does not return any value. If hello PassThru par
            is provided then Remove-AzureRmRedisCache returns a boolean value indicating hello success of hello operatio

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

A következő példa hello, hello nevű gyorsítótár `myCache` törlődnek.

    PS C:\> Remove-AzureRmRedisCache -Name myCache -ResourceGroupName myGroup

    Confirm
    Are you sure you want tooremove redis cache 'myCache'?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): Y


## <a name="tooimport-a-redis-cache"></a>a Redis gyorsítótár tooimport
Adatok importálása az Azure Redis Cache példány hello segítségével `Import-AzureRmRedisCache` parancsmag.

> [!IMPORTANT]
> Importálási/exportálási lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza. Importálási/exportálási kapcsolatos további információkért lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Import-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Import-AzureRmRedisCache -detailed

    NAME
        Import-AzureRmRedisCache

    SYNOPSIS
        Import data from blobs tooAzure Redis Cache.


    SYNTAX
        Import-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Files <String[]> [-Format <String>] [-Force]
        [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Import-AzureRmRedisCache cmdlet imports data from hello specified blobs into Azure Redis Cache.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Files <String[]>
            SAS urls of blobs whose content should be imported into hello cache.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -Force
            When hello Force parameter is provided, import will be performed without any confirmation prompts.

        -PassThru
            By default Import-AzureRmRedisCache imports data in cache and does not return any value. If hello PassThru
            parameter is provided then Import-AzureRmRedisCache returns a boolean value indicating hello success of the
            operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


hello következő parancs importál adatokat hello blob az Azure Redis Cache hello SAS URI megadva.

    PS C:\>Import-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Files @("https://mystorageaccount.blob.core.windows.net/mycontainername/blobname?sv=2015-04-05&sr=b&sig=caIwutG2uDa0NZ8mjdNJdgOY8%2F8mhwRuGNdICU%2B0pI4%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwd") -Force

## <a name="tooexport-a-redis-cache"></a>a Redis gyorsítótár tooexport
Az Azure Redis Cache példány hello segítségével exportálhatja az adatokat `Export-AzureRmRedisCache` parancsmag.

> [!IMPORTANT]
> Importálási/exportálási lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza. Importálási/exportálási kapcsolatos további információkért lásd: [importálhat és exportálhat adatokat az Azure Redis Cache](cache-how-to-import-export-data.md).
> 
> 

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Export-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Export-AzureRmRedisCache -detailed

    NAME
        Export-AzureRmRedisCache

    SYNOPSIS
        Exports data from Azure Redis Cache tooa specified container.


    SYNTAX
        Export-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -Prefix <String> -Container <String> [-Format
        <String>] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Export-AzureRmRedisCache cmdlet exports data from Azure Redis Cache tooa specified container.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -Prefix <String>
            Prefix toouse for blob names.

        -Container <String>
            SAS url of container where data should be exported.

        -Format <String>
            Format for hello blob.  Currently "rdb" is hello only supported, with other formats expected in hello future.

        -PassThru
            By default Export-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Export-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


hello következő parancs exportál adatokat az Azure Redis Cache példány hello SAS URI azonosítója által megadott hello tárolóba.

        PS C:\>Export-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -Prefix "blobprefix"
        -Container "https://mystorageaccount.blob.core.windows.net/mycontainer?sv=2015-04-05&sr=c&sig=HezZtBZ3DURmEGDduauE7
        pvETY4kqlPI8JCNa8ATmaw%3D&st=2016-05-27T00%3A00%3A00Z&se=2016-05-28T00%3A00%3A00Z&sp=rwdl"

## <a name="tooreboot-a-redis-cache"></a>a Redis gyorsítótár tooreboot
Az Azure Redis Cache példány hello segítségével újraindíthatja `Reset-AzureRmRedisCache` parancsmag.

> [!IMPORTANT]
> Újraindítás lehetőség csak a [prémium csomagban](cache-premium-tier-intro.md) gyorsítótárazza. A gyorsítótár újraindítás kapcsolatos további információkért lásd: [felügyeleti gyorsítótár - újraindítás](cache-administration.md#reboot).
> 
> 

toosee használható paramétereket, és azok leírásait tartalmazza a listájának `Reset-AzureRmRedisCache`- ben futtassa hello következő parancsot.

    PS C:\> Get-Help Reset-AzureRmRedisCache -detailed

    NAME
        Reset-AzureRmRedisCache

    SYNOPSIS
        Reboot specified node(s) of an Azure Redis Cache instance.


    SYNTAX
        Reset-AzureRmRedisCache -Name <String> -ResourceGroupName <String> -RebootType <String> [-ShardId <Integer>]
        [-Force] [-PassThru] [<CommonParameters>]


    DESCRIPTION
        hello Reset-AzureRmRedisCache cmdlet reboots hello specified node(s) of an Azure Redis Cache instance.


    PARAMETERS
        -Name <String>
            hello name of hello cache.

        -ResourceGroupName <String>
            hello name of hello resource group that contains hello cache.

        -RebootType <String>
            Which node tooreboot. Possible values are "PrimaryNode", "SecondaryNode", "AllNodes".

        -ShardId <Integer>
            Which shard tooreboot when rebooting a premium cache with clustering enabled.

        -Force
            When hello Force parameter is provided, reset will be performed without any confirmation prompts.

        -PassThru
            By default Reset-AzureRmRedisCache does not return any value. If hello PassThru parameter is provided
            then Reset-AzureRmRedisCache returns a boolean value indicating hello success of hello operation.

        <CommonParameters>
            This cmdlet supports hello common parameters: Verbose, Debug,
            ErrorAction, ErrorVariable, WarningAction, WarningVariable,
            OutBuffer, PipelineVariable, and OutVariable. For more information, see
            about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).


hello parancsban újraindulása mindkét csomópontjának megadott hello gyorsítótár.

        PS C:\>Reset-AzureRmRedisCache -ResourceGroupName "resourceGroupName" -Name "cacheName" -RebootType "AllNodes"
        -Force


## <a name="next-steps"></a>Következő lépések
További információ a Windows PowerShell használatával az Azure-toolearn a következő erőforrások hello lásd:

* [Az Azure Redis Cache parancsmag dokumentációja az MSDN webhelyen](https://msdn.microsoft.com/library/azure/mt634513.aspx)
* [Az Azure Resource Manager parancsmagjainak](http://go.microsoft.com/fwlink/?LinkID=394765): hello Azure Resource Manager modul toouse hello használható parancsmagok megismerése.
* [Erőforrás segítségével csoportosítja toomanage az Azure-erőforrások](../azure-resource-manager/resource-group-template-deploy-portal.md): megtudhatja, hogyan toocreate és kezelheti a hello Azure-portálon az erőforráscsoportokat.
* [Az Azure blog](http://blogs.msdn.com/windowsazure): az Azure-ban új szolgáltatásainak megismerése.
* [A Windows PowerShell blog](http://blogs.msdn.com/powershell): a Windows PowerShell új szolgáltatásainak megismerése.
* ["Hey, Scripting Guy!" Blog](http://blogs.technet.com/b/heyscriptingguy/): hello Windows PowerShell-Közösség valós tippek és trükkök az beszerzése.


---
title: "aaaAzure előfizetés korlátozásai és a kvóták |} Microsoft Docs"
description: "A közös Azure-előfizetés és szolgáltatási korlátok, kvóták és megkötések listáját tartalmazza. Ide tartoznak a hogyan tooincrease korlátozza a maximális értékek együtt."
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: byvinyal
ms.openlocfilehash: a754d56124520791254ab8f1729808f0750ff222
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Az Azure-előfizetésekre és -szolgáltatásokra vonatkozó korlátozások, kvóták és megkötések
Ez a dokumentum soroljuk hello leggyakrabban használt Microsoft Azure-korlátok, kvóták néven is ismert. Ez a dokumentum jelenleg nem fedi le az összes Azure-szolgáltatásokhoz. Az idő múlásával hello lista kibontásra váró, és hello platform további toocover frissítése.

Látogasson el a [Azure díjszabása áttekintése](https://azure.microsoft.com/pricing/) további információk az Azure-beli árakról toolearn. Van, a költségek hello használatával megbecsülheti [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/) vagy hello díjszabás részleteit megjelenítő oldalon a szolgáltatás számára (például [Windows virtuális gépek](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). A tippek toohelp a költségeinek kezelése című [Azure számlázás és költség felügyeleti váratlan költségek megakadályozása](billing/billing-getting-started.md).

> [!NOTE]
> Ha azt szeretné, tooraise hello korlátot vagy a fenti hello kvóta **alapértelmezett korlát**, [nyissa meg az online támogatás ügyfélkérés díjmentesen](azure-supportability/resource-manager-core-quotas-request.md). hello korlátai nem léptethető fent hello **maximális** a következő táblák hello szereplő érték. Ha nincs **maximális** oszlopban, majd hello erőforrás nem állítható korlátokkal rendelkeznek. 
> 
> Ingyenes próba-előfizetések nem jogosultak a korlátot, vagy kvóta növeli. Ha egy ingyenes próbaverziót, frissítheti tooa [használatalapú fizetés](https://azure.microsoft.com/offers/ms-azr-0003p/) előfizetés. További információkért lásd: [Azure ingyenes próbaverzió frissítése tooPay-,-akkor-Ugrás](billing/billing-upgrade-azure-subscription.md).
> 

## <a name="limits-and-hello-azure-resource-manager"></a>Korlátozásai és hello Azure Resource Manager
Már lehetséges toocombine tooa a több Azure-erőforrások egy Azure-erőforráscsoportot. Erőforráscsoportok használatakor egyszer volt a globális korlátozza az Azure Resource Manager hello regionális szintű felügyelhető legyen. Azure erőforráscsoport-sablonok kapcsolatos további információkért lásd: [Azure Resource Manager áttekintése](azure-resource-manager/resource-group-overview.md).

Az alábbi új tábla hello korlátok lett hozzáadott tooreflect összes különbséget korlátok hello Azure Resource Manager használata esetén. Például van egy **előfizetési korlátozásait** tábla és egy **előfizetési korlátozásait - Azure Resource Manager** tábla. Ha a megadott korlát érvényes tooboth forgatókönyvek, csak látható hello első tábla. Hiányában korlátok legyenek globális minden régióban.

> [!NOTE]
> Ez megegyezik, hogy Azure erőforráscsoport-sablonok az erőforrásokra vonatkozó kvótákat /-régióban elérhető-e az előfizetés, és nem előfizetésenként, fontos tooemphasize hello szolgáltatás felügyeleti kvóták. Most használja core kvóták példaként. Ha a kvóta növeléséhez magok támogatása toorequest van szüksége, akkor toodecide hogyan szeretné, hogy melyik régióban toouse, és végezze el egy adott kérelem az Azure erőforráscsoport sok magok kvóták alapvető hello összegeket és a kívánt régiók. Ezért ha toouse 30 kell processzormag, Nyugat-Európában toorun a az alkalmazás Nyugat-Európában 30 magok kifejezetten igényeljen. Azonban Ön nem rendelkezik a core kvóta növelése más régióban – csak Nyugat-Európában hello 30-core kvóta lesz.
> <!-- -->
> Ennek eredményeképpen előfordulhat azt hasznos tooconsider dönt, hogy mi az Azure-erőforráscsoport kvóták kell toobe minden olyan egy régió tartozik, és kérelmet, amely minden régióban, amelybe a központi telepítés tervezi összeg a munkaterhelés számára. Lásd: [telepítési problémák elhárítása](resource-manager-common-deployment-errors.md) további segítséget itt találhat az aktuális kvóták adott régióban felderítéséhez.
> 
> 

## <a name="service-specific-limits"></a>Szolgáltatás-specifikus korlátok
* [Active Directory](#active-directory-limits)
* [API Management](#api-management-limits)
* [APP SERVICE](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automatizálás](#automation-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Az Azure Event rács](#azure-event-grid-limits)
* [Azure Redis Cache](#azure-redis-cache-limits)
* [Azure RemoteApp](#azure-remoteapp-limits)
* [Biztonsági mentés](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk szolgáltatások](#biztalk-services-limits)
* [TARTALOMKÉZBESÍTÉSI HÁLÓZAT (CDN)](#cdn-limits)
* [Felhőszolgáltatások](#cloud-services-limits)
* [Tárolópéldányok](#container-instances-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake analitikai szolgáltatás](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [IoT Hub](#iot-hub-limits)
* [Key Vault](#key-vault-limits)
* [Naplófájl Analytics / Operational Insights](#log-analytics-limits)
* [Médiaszolgáltatások](#media-services-limits)
* [Mobilmarketing](#mobile-engagement-limits)
* [Mobilszolgáltatások](#mobile-services-limits)
* [Figyelés](#monitor-limits)
* [Többtényezős hitelesítés](#multi-factor-authentication)
* [Hálózat](#networking-limits)
* [Hálózati figyelőt](#network-watcher-limits)
* [Értesítési központ szolgáltatás](#notification-hub-service-limits)
* [Erőforráscsoport](#resource-group-limits)
* [Scheduler](#scheduler-limits)
* [Search](#search-limits)
* [Szolgáltatásbusz](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [Tárolás](#storage-limits)
* [StorSimple rendszer](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Előfizetés](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Virtuális gépek](#virtual-machines-limits)
* [Virtual Machine Scale Sets](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Előfizetési korlátozásait
#### <a name="subscription-limits"></a>Előfizetési korlátozásait
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Előfizetési korlátozásait - Azure Resource Manager
a következő korlátozások hello alkalmazza, ha hello Azure Resource Manager és az Azure erőforráscsoport-sablonok használatával. Nem módosított rendelkező hello Azure Resource Manager korlátok alább nem láthatók. Az ilyen határidők toohello előző táblázatban tájékozódhat.

Erőforrás-kezelő kérelmekre vonatkozó korlátozások kezelésére vonatkozó információkért lásd: [sávszélesség-szabályozás erőforrás-kezelő kérelmek](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Erőforráscsoport korlátok
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Virtuális gépek korlátok
#### <a name="virtual-machine-limits"></a>Virtuális gépek korlátai
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuális gépek korlátok - Azure Resource Manager
a következő korlátozások hello alkalmazza, ha hello Azure Resource Manager és az Azure erőforráscsoport-sablonok használatával. Nem módosított rendelkező hello Azure Resource Manager korlátok alább nem láthatók. Az ilyen határidők toohello előző táblázatban tájékozódhat.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Virtuálisgép-méretezési készlet korlátai
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Tároló-példányok korlátok
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a>Hálózatkezelési korlátok
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Hálózatkezelési korlátok
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Alkalmazásátjáró korlátozza.
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Korlátozza a hálózati figyelőt
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>A TRAFFIC Manager-korlátok
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-korlátok
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Tárolási korlátai
A tárfiókok korlátai további részletekért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage/common/storage-scalability-targets.md).
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a>Storage szolgáltatásra vonatkozó korlátozások
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Virtuális gépek lemez korlátai 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Lásd: [virtuálisgép-méretek](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további részleteket.

#### <a name="managed-virtual-machine-disks"></a>Felügyelt virtuális gépek lemezei

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Nem felügyelt virtuális gépek lemezei

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Korlátozza a Storage erőforrás-szolgáltató
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a>Cloud Services korlátok
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>App Service szolgáltatásra vonatkozó korlátok
hello következő korlátozza az App Service Web Apps, a Mobile Apps, az API-alkalmazások és a Logic Apps korlátok tartalmazza.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>A Feladatütemező korlátok
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Kötegelt korlátok
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>BizTalk szolgáltatások korlátok
hello következő táblázat hello korlátok Azure Biztalk szolgáltatások.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Az Azure Cosmos DB korlátok
Azure Cosmos-adatbázis egy olyan globális méretű adatbázis, mely átviteli sebesség és tárterület lehet méretezett toohandle függetlenül az alkalmazás által igényelt. Ha Azure Cosmos DB biztosít hello méretezésének kérdése van, kérjük, küldjön e-mailek tooaskcosmosdb@microsoft.com.

### <a name="mobile-engagement-limits"></a>A Mobile Engagement korlátok
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a>Keresési korlátok
Tarifacsomagok hello kapacitás és a keresési szolgáltatás határain határozza meg. Szolgáltatásszintek:

* *Szabad* értékelési és kis fejlesztési projektek szánt más Azure-előfizetők megosztott, több-bérlős szolgáltatást.
* *Alapszintű* biztosít a számítási erőforrások dedikált kisebb léptékű termelési számítási feladatokhoz toothree replikák a munkaterhelések magas rendelkezésre állású lekérdezés mentése.
* *Standard (S1, S2, S3, S3 nagy sűrűségű)* értéke nagyobb a termelési számítási feladatokhoz. Több szinten vannak, hogy Ön egy erőforrás-konfigurációhoz, amely a legjobban illik a munkaterhelés profil hello standard csomagra.

**Előfizetésenként korlátok**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Érhető el keresési szolgáltatásonként korlátok**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

toolearn részletesebb felügyeletét, például a dokumentum mérete, a lekérdezések száma másodpercenként, kulcsok, kérések és válaszok, használati korlátait kapcsolatos további információkért lásd: [szolgáltatási korlátait, az Azure Search](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>A Media Services korlátok
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-korlátok
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>A Mobile Services korlátok
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>A figyelő korlátok
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Értesítési központ szolgáltatásra vonatkozó korlátozások
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Event Hubs korlátok
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>A Service Bus korlátok
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>Az IoT-központ korlátok
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Adat-előállító korlátozza.
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>A Data Lake Analytics korlátok
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store korlátok
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a>A Stream Analytics korlátozza.
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-korlátok
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Az Azure Event rács korlátok
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a>Az Azure RemoteApp korlátok
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>StorSimple rendszer korlátok
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>A Naplóelemzési korlátozza.
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Biztonsági mentési korlátok
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>A Site Recovery korlátai
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights korlátok
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>Korlátozza az API Management
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis Cache-korlátok
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Key Vault korlátok
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automatizálási korlátok
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>SQL-adatbázis korlátok
SQL adatbázis-korlátok, lásd: [SQL adatbázis erőforrás korlátok](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Lásd még:
[Azure korlátozását és növekszik ismertetése](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuális gépek és Felhőszolgáltatások mérete az Azure-bA](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[A Felhőszolgáltatások mérete](cloud-services/cloud-services-sizes-specs.md)


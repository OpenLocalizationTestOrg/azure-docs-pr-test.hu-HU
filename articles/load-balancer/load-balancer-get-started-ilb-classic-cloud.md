---
title: "Azure-szolgáltatásokhoz belső terheléselosztót aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó hello klasszikus üzembe helyezési modellben a PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 57966056-0f46-4f95-a295-483ca1ad135d
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: fe7975bca7bec3248626b0ad0fad6823e278ade2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Bevezetés a belső terheléselosztó (klasszikus) felhőszolgáltatásokhoz történő létrehozásába

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Felhőszolgáltatások](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](load-balancer-get-started-ilb-arm-ps.md).

## <a name="configure-internal-load-balancer-for-cloud-services"></a>Belső terheléselosztó konfigurálása a felhőszolgáltatásokhoz

A belső terheléselosztó használata virtuális gépek és felhőszolgáltatások esetén egyaránt támogatott. Belső terheléselosztó a végpont egy felhőalapú szolgáltatás, amely a regionális virtuális hálózatokon kívül létrehozott lesz csak a felhőszolgáltatáshoz hello belülről érhetők el.

belső terheléselosztó-konfiguráció hello beállítása hello létrehozása hello hello felhőalapú szolgáltatás, az első központi telepítése során, ahogy az alábbi minta hello toobe rendelkezik.

> [!IMPORTANT]
> Egy előfeltétel toorun hello lépéseket toohave hello felhő üzembe helyezése már létrehozott egy virtuális hálózathoz. Virtuális hálózat neve és az alhálózati név toocreate hello belső terheléselosztás hello kell.

### <a name="step-1"></a>1. lépés

Nyissa meg a felhő üzembe helyezése a Visual Studio hello szolgáltatás konfigurációs fájlját (.cscfg), és adja hozzá a következő szakasz toocreate hello belső terheléselosztás, a hello utolsó hello "`</Role>`" hello hálózati konfigurációs elemet.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of hello load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Adjunk hello hálózati konfigurációs fájl tooshow megjelenését hello értékeit. Hello példában feltételezzük létrehozott egy "test_vnet" meghívva egy alhálózat 10.0.0.0/24 test_subnet és egy statikus IP-cím 10.0.0.4 nevű Vnetet. hello terheléselosztó testLB lesznek elnevezve.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Hello load balancer séma kapcsolatos további információkért lásd: [Hozzáadás terheléselosztó](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>2. lépés

Hello szolgáltatás definíciós (.csdef) fájl tooadd végpontok toohello belső terheléselosztás módosítása. hello szolgáltatásdefiníciós fájl hello szerepkör példányok toohello belső terheléselosztás ad hozzá, hello néhány percet a szerepkör példánya jön létre.

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

Következő hello ugyanaz a fenti példa hello értékei adjuk hozzá hello értékek toohello szolgáltatásdefiníciós fájlban.

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

hello hálózati forgalom használ a 80-as portot a bejövő kérelmeket küldő tooworker szerepkörpéldányokat is 80-as porton hello testLB terheléselosztó segítségével elosztott terhelésű lesz.

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)


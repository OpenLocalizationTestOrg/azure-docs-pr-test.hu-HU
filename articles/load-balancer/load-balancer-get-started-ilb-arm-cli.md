---
title: "aaaCreate egy belső terheléselosztó - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a belső terheléselosztók használatával hello Azure CLI az erőforrás-kezelőben"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3aea6fdb07600f0d661ec6b8ffc784b03380a127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-by-using-hello-azure-cli"></a>Hozzon létre egy belső elosztott terhelésű hello Azure parancssori felület használatával

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-solution-by-using-hello-azure-cli"></a>Hello megoldás telepítése hello Azure parancssori felület használatával

hello lépések bemutatják, hogyan toocreate egy internetre irányuló terheléselosztó CLI Azure Resource Manager használatával. Az Azure Resource Manager az egyes erőforrások jön létre, és egyenként konfigurálni, majd tegye együtt toocreate erőforrás.

Toocreate kell, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:

* **Előtérbeli IP-konfiguráció**: a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz
* **Háttér-címkészlet**: tartalmaz, amelyek lehetővé teszik a hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC)
* **Terheléselosztási szabályok**: hello load balancer tooport hello háttér-címkészletbeli nyilvános port hozzárendelését szabályokat tartalmaz
* **Bejövő NAT-szabályok**: hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port hozzárendelését szabályokat tartalmaz
* **Vizsgálat**: tartalmaz, amelyek a virtuális gépek példánya hello háttér címkészletet használt toocheck hello rendelkezésre állási állapotát mintavételt

A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.

## <a name="set-up-cli-toouse-resource-manager"></a>Parancssori felület toouse erőforrás-kezelő beállítása

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md). Hello követésével mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooResource kezelő módra, az alábbiak szerint parancsot:

    ```azurecli
    azure config mode arm
    ```

    Várt kimenet:

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a>Belső terheléselosztó létrehozásának lépései

1. Jelentkezzen be tooAzure.

    ```azurecli
    azure login
    ```

    Amikor a rendszer erre kéri, adja meg Azure rendszerbeli hitelesítő adatait.

2. Módosítsa a hello parancs eszközök tooAzure Resource Manager módra.

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Az Azure Resource Managerben minden erőforrás egy erőforráscsoporthoz van társítva. Ha még nem tette meg, hozzon létre egy erőforráscsoportot.

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a>Hozzon létre egy belső terheléselosztó készletet

1. Hozzon létre egy belső terheléselosztót

    USA keleti régiójában hello forgatókönyv a következő, a nrprg nevű erőforráscsoport jön létre.

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > Belső terheléselosztók, például a virtuális hálózatok és virtuális hálózati alhálózat összes erőforrás kell hello ugyanabban az erőforráscsoportban, és a hello azonos régióban.

2. Hozzon létre egy hello belső terheléselosztó előtér-IP-címet.

    hello IP-cím, amelyekkel a virtuális hálózat hello alhálózati tartományba kell lennie.

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. Hello háttér-címkészlet létrehozása.

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    Miután megtörtént az előtérbeli IP-cím, valamint a háttércímkészlet definiálása, létre lehet hozni a terheléselosztási szabályokat, a bejövő NAT-szabályokat, valamint a testre szabott állapotfigyelő mintavételezőket.

4. Hozzon létre olyan terheléselosztó szabályhoz hello belső terheléselosztóhoz.

    Hello előző lépések végrehajtásával, ha hello parancs hoz létre egy terheléselosztó szabály, figyelő tooport 1433 a hello előtér-készlet és a küldő terhelésű hálózati forgalom toohello háttér címkészletet, továbbá használatával az 1433-as port.

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. Hozza létre a bejövő NAT-szabályokat.

    Bejövő NAT-szabályok olyan használt toocreate végpontok egy terheléselosztó, amely tooa adott virtuálisgép-példányt. hello előző lépést a távoli asztal két NAT-szabályok létrehozása.

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. Hozzon létre állapotfigyelő mintavételt hello terheléselosztóhoz.

    Egy állapotmintáihoz ellenőrzi az összes virtuális gép példányok toomake meg arról, hogy a hálózati forgalom elküldése lehetséges. hello virtuálisgép-példány sikertelen mintavételi ellenőrzések hello terheléselosztó törlődik, amíg újra online állapotba kerül, és a mintavétel-ellenőrzés határozza meg, hogy a rendszer kifogástalan.

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > hello Microsoft Azure platform különféle felügyeleti forgatókönyvekhez, amik egy nyilvánosan elérhető, statikus IPv4-címet használ. hello IP-cím 168.63.129.16. Ezt az IP-címet nem blokkolhatja egy tűzfal sem, mert ez nem várt viselkedést okozhat.
    > A tekintetben tooAzure belső terheléselosztás, az IP-címet használják mintavételt hello load balancer toodetermine hello állapotát a virtuális gépek egy elosztott terhelésű készlet figyelése. Ha egy hálózati biztonsági csoportot használt toorestrict forgalom tooAzure virtuális gépek egy belső elosztott terhelésű készlet vagy alkalmazott tooa virtuális hálózati alhálózat, győződjön meg arról, hogy a hálózati biztonsági szabály legyen-e adva a 168.63.129.16 tooallow forgalom.

## <a name="create-nics"></a>Hálózati adapterek létrehozása

Toocreate hálózati adapterrel kell (vagy módosíthatja a meglévőket), és rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételek menüpontban.

1. Hozzon létre egy olyan hálózati adapter nevű *lb nic1 kell*, és társíthatja hello *rdp1* NAT szabály és hello *beilb* háttér címkészletet.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    Várt kimenet:

        info:    Executing command network nic create
        + Looking up hello network interface "lb-nic1-be"
        + Looking up hello subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up hello network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. Hozzon létre egy olyan hálózati adapter nevű *lb nic2 kell*, és társíthatja hello *rdp2* NAT szabály és hello *beilb* háttér címkészletet.

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. Nevű virtuális gép létrehozása *D1*, és társíthatja hello nevű hálózati *lb nic1 kell*. A tárfiók neve *web1nrp* hello a következő parancs futtatása előtt hozza létre:

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > A betöltési terheléselosztó kell toobe a virtuális gépek hello azonos rendelkezésre állási csoportot. Használjon `azure availset create` toocreate rendelkezésre állási készlet.

4. Hozzon létre egy virtuális gépet (VM) nevű *DB2*, és társíthatja hello nevű hálózati *lb nic2 kell*. A tárfiók neve *web1nrp* hello a következő parancs futtatása előtt lett létrehozva.

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a>Terheléselosztó törlése

egy terhelés-kiegyenlítő tooremove hello a következő parancsot használja:

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)


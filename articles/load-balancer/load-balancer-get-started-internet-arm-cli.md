---
title: "az internetre irányuló aaaCreate terheléselosztó - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Internet felé néző terheléselosztót a Resource Manager használatával hello Azure parancssori felület"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: cadb5edb3b4a4e2f0813109d027eaafdc7ef7303
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-load-balancer-using-hello-azure-cli"></a>Az internet terheléselosztói hello Azure parancssori felület használatával

> [!div class="op_single_selector"]
> * [Portál](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre irányuló terheléselosztót használja a klasszikus üzembe helyezési](load-balancer-get-started-internet-classic-portal.md)

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-using-hello-azure-cli"></a>Hello Azure parancssori felület használatával hello megoldás telepítése

hello lépések bemutatják, hogyan toocreate egy internetre irányuló terheléselosztót a CLI Azure Resource Manager használatával. Az Azure Resource Manager az egyes erőforrások hozzák létre, és egyenként konfigurálni, majd hozzáfoghat toocreate erőforrás.

Kell létrehozni, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:

* Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.
* Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.
* Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.

A további információkat [Az Azure Resource Manager támogatása a terheléselosztó számára](load-balancer-arm.md) című rész tartalmazza.

## <a name="set-up-cli-toouse-resource-manager"></a>Parancssori felület toouse erőforrás-kezelő beállítása

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.

    ```azurecli
        azure config mode arm
    ```

    Várt kimenet:

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a>Hozzon létre egy virtuális hálózatot és egy nyilvános IP-cím hello előtér-IP-címtartományhoz.

1. Hozzon létre egy virtuális hálózatot (VNet) nevű *NRPVnet* hello USA keleti régiója helyen erőforráscsoport használatával nevű *NRPRG*.

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    Hozzon létre egy 10.0.0.0/24 CIDR-blokkot tartalmazó *NRPVnetSubnet* nevű alhálózatot az *NRPVnet* virtuális hálózatban.

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. Hozzon létre egy nyilvános IP-cím nevű *NRPPublicIP* DNS-nevet egy előtér-IP-készlet által használt toobe *loadbalancernrp.eastus.cloudapp.azure.com*. hello parancs az alábbi hello statikus foglalási típust használja, és 4 perc üresjárati időkorlátját.

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > hello terheléselosztó azonos teljes Tartománynevű hello tartomány címkéjének hello nyilvános IP-címet fog használni. A klasszikus üzembe helyezési, hello felhőalapú szolgáltatás, a terheléselosztó teljesen minősített tartománynév (FQDN) hello használó módosítást.
   > Ebben a példában hello FQDN-je *loadbalancernrp.eastus.cloudapp.azure.com*.

## <a name="create-a-load-balancer"></a>Terheléselosztó létrehozása

hello következő parancs létrehoz egy terhelés-kiegyenlítő nevű *NRPlb* a hello *NRPRG* hello erőforráscsoportja *USA keleti régiója* Azure-beli hely.

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a>Előtér-IP-címkészlet és háttércímkészlet létrehozása
Ez a példa bemutatja, hogyan toocreate hello előtér-IP-készlet, amely megkapja a bejövő hálózati forgalmat hello hello terheléselosztó és hello háttérbeli IP-készlet, ahol hello előtér-készlet küldi hello hálózati forgalmat.

1. Hozzon létre egy társítása hello nyilvános IP-cím létrehozott hello előző lépésben és hello terheléselosztó előtér-IP-címkészletet.

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. Állítsa be egy háttér címkészletet tooreceive bejövő forgalom használt hello előtér-IP-készletből.

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a>LB-szabályok, NAT-szabályok és mintavétel létrehozása

Ebben a példában a következő elemek hello hoz létre.

* a NAT-szabályok tootranslate minden bejövő forgalom a 22-es port 21 tooport<sup>1</sup>
* a NAT-szabályok tootranslate minden bejövő forgalom a 22-es port 23 tooport
* a load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben.
* a mintavétel szabály toocheck hello állapot nevű oldalon *HealthProbe.aspx*.

<sup>1</sup> NAT-szabályok adott virtuálisgép-példányt társított tooa hello terheléselosztó mögött. hello hálózati 21 portot a bejövő adatforgalom tooa adott virtuális gép a NAT-szabály társított 22-es porton. A NAT-szabályhoz meg kell adnia egy protokollt (UDP vagy TCP). Mindkét protokollt nem lehet toohello hozzárendelve ugyanehhez a porthoz.

1. Hello NAT-szabályok létrehozása.

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. Hozzon létre egy terheléselosztó-szabályt.

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. Hozzon létre egy állapotmintát.

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. Ellenőrizze a beállításokat.

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    Várt kimenet:

        info:    Executing command network lb show
        + Looking up hello load balancer "nrplb"
        + Looking up hello public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a>Hálózati adapterek létrehozása

Toocreate hálózati adapterrel kell (vagy módosíthatja a meglévőket), és rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételek menüpontban.

1. Hozzon létre egy hálózati adapter nevű *lb nic1 kell*, és társítsa azt az hello *rdp1* NAT szabály és hello *NRPbackendpool* háttér címkészletet.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
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

2. Hozzon létre egy hálózati adapter nevű *lb nic2 kell*, és társítsa azt az hello *rdp2* NAT szabály és hello *NRPbackendpool* háttér címkészletet.

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. Hozzon létre egy virtuális gépet (VM) nevű *web1*, és rendelje hozzá azt a hálózati adapter nevű hello *lb nic1 kell*. A tárfiók neve *web1nrp* alábbi hello parancs futtatása előtt lett létrehozva.

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > A betöltési terheléselosztó kell toobe a virtuális gépek hello azonos rendelkezésre állási csoportot. Használjon `azure availset create` toocreate rendelkezésre állási készlet.

    hello kimeneti hasonló toohello következő legyen:

        info:    Executing command vm create
        + Looking up hello VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account web1nrp
        + Looking up hello availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up hello NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in hello NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > hello tájékoztató üzenet **Ez az egy hálózati adapter nélkül konfigurált publicIP** várt hello hálózati adapter létrehozása óta hello terheléselosztóhoz csatlakozó tooInternet hello load balancer nyilvános IP-cím használatával.

    Hello óta *lb nic1 kell* hálózati adapter kapcsolódik a hello *rdp1* NAT-szabály túl kapcsolódhatnak*web1* RDP Funkciót használnak a hello terheléselosztón 3441 porton keresztül.

4. Hozzon létre egy virtuális gépet (VM) nevű *web2*, és rendelje hozzá azt a hálózati adapter nevű hello *lb nic2 kell*. A tárfiók neve *web1nrp* alábbi hello parancs futtatása előtt lett létrehozva.

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a>Meglévő terheléselosztó frissítése
Hozzáadhat egy meglévő terheléselosztóra hivatkozó szabályokat. Hello a következő példában, egy új terheléselosztási szabály kerül a meglévő terheléselosztó tooan **NRPlb**

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a>Terheléselosztó törlése
A következő parancs tooremove terheléselosztó hello használata:

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a>Következő lépések
[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-cli.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

---
title: "IPv6 - Azure CLI aaaCreate egy internetre irányuló terheléselosztót |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Internet felé néző rendelkező terheléselosztó IPv6 Azure Resource Manager használatával hello Azure parancssori felület"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 7ff75ac90d74a74e3d0c27672b36fbd955a086a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-hello-azure-cli"></a>Hozzon létre az Internet felé néző terheléselosztón az IPv6 az Azure Resource Manager hello Azure parancssori felület használatával

> [!div class="op_single_selector"]
> * [PowerShell](load-balancer-ipv6-internet-ps.md)
> * [Azure CLI](load-balancer-ipv6-internet-cli.md)
> * [Sablon](load-balancer-ipv6-internet-template.md)

Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül. hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít. Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.

## <a name="example-deployment-scenario"></a>Központi telepítési példa

hello következő diagram azt ábrázolja, hello terheléselosztási megoldás üzembe helyezéséhez sablonnal hello példa a cikkben.

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

Ebben a forgatókönyvben a következő Azure-erőforrások hello hozza létre:

* két virtuális gépek (VM)
* az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő
* egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím
* egy rendelkezésre állási csoport toothat hello két virtuális gépeket tartalmaz
* két terheléselosztási szabályok toomap hello nyilvános virtuális IP-címek toohello titkos végpontok betöltése

## <a name="deploying-hello-solution-using-hello-azure-cli"></a>Hello Azure parancssori felület használatával hello megoldás telepítése

hello lépések bemutatják, hogyan toocreate egy internetre irányuló terheléselosztót a CLI Azure Resource Manager használatával. Az Azure Resource Manager az egyes erőforrások jön létre, és egyenként konfigurálni, majd tegye együtt toocreate erőforrás.

a terheléselosztó toodeploy, létrehozása és beállítása a következő objektumok hello:

* Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.
* Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.
* Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.
* Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.

A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.

## <a name="set-up-your-cli-environment-toouse-azure-resource-manager"></a>A parancssori felület környezet toouse Azure Resource Manager beállítása

Ehhez a példához jelenleg fut hello CLI-eszközeit egy PowerShell-parancsablakot. Hello Azure PowerShell-parancsmagok nem használjuk, de a PowerShell parancsfájl-kezelési képességek tooimprove olvashatóság és újbóli használjuk.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód.

    ```azurecli
    azure config mode arm
    ```

    Várt kimenet:

        info:    New mode is arm

3. Jelentkezzen be tooAzure, és az előfizetések listáját.

    ```azurecli
    azure login
    ```

    Adja meg, amikor a rendszer kéri az Azure hitelesítő adatait.

    ```azurecli
    azure account list
    ```

    Válassza ki a kívánt hello előfizetés toouse. Jegyezze fel a hello előfizetési azonosító hello következő lépéshez.

4. Állítsa be a PowerShell változók hello parancssori felület parancsait való használatra.

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a>Hozzon létre egy erőforráscsoportot, egy adott terheléselosztóhoz, egy virtuális hálózatot és alhálózatok

1. Hozzon létre egy erőforráscsoportot

    ```azurecli
    azure group create $rgName $location
    ```

2. Terheléselosztó létrehozása

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. Hozzon létre egy virtuális hálózatot (VNet).

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    Ez a virtuális hálózat létrehozása két alhálózatból állnak.

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-hello-front-end-pool"></a>Nyilvános IP-címek hello előtér-készlet létrehozása

1. Hello PowerShell változók beállítása

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. Hozzon létre egy nyilvános IP-hello előtér-IP címkészletet.

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > hello terheléselosztó hello tartománycímke hello nyilvános IP-használja, mint a teljes Tartománynevet. A klasszikus üzembe helyezési, hello felhőszolgáltatás neve használja, mint a terheléselosztó FQDN hello módosítást.
    > Ebben a példában hello FQDN-je *contoso09152016.southcentralus.cloudapp.azure.com*.

## <a name="create-front-end-and-back-end-pools"></a>Hozzon létre az előtér- és készletek

Ebben a példában a hello előtér-IP-készlet, amely megkapja a bejövő hálózati forgalom hello hello terheléselosztón és hello háttér IP-készlet ahol hello előtér-készlet küldi hello hálózati forgalmat hoz létre.

1. Hello PowerShell változók beállítása

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. Hozzon létre egy társítása hello nyilvános IP-cím létrehozott hello előző lépésben és hello terheléselosztó előtér-IP-címkészletet.

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-hello-probe-nat-rules-and-lb-rules"></a>Hello mintavételi, a NAT-szabályok és a Terheléselosztó szabályok létrehozása

Ebben a példában a következő elemek hello hoz létre:

* a mintavétel szabály toocheck a kapcsolat tooTCP 80-as port
* a NAT-szabályok tootranslate minden bejövő forgalom a 3389-es port tooport 3389-es RDP<sup>1</sup>
* a NAT-szabályok tootranslate minden bejövő forgalom a 3389-es port 3391 tooport RDP<sup>1</sup>
* a load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben.

<sup>1</sup> NAT-szabályok adott virtuálisgép-példányt társított tooa hello terheléselosztó mögött. hello hálózati 3389-es portot a bejövő adatforgalom toohello adott virtuális gép és a társított hello NAT-szabály portja. A NAT-szabályhoz meg kell adnia egy protokollt (UDP vagy TCP). Mindkét protokollt nem lehet toohello hozzárendelve ugyanehhez a porthoz.

1. Hello PowerShell változók beállítása

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. Hello mintavétel létrehozása

    hello alábbi példakód létrehozza a TCP mintavételi modul, amely ellenőrzi a kapcsolat tooback közötti TCP 80-as port 15 másodpercenként. Azt jelöli hello háttér-erőforrás nem érhető el két egymást követő hibák után.

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. Bejövő NAT-szabályok, amelyek lehetővé teszik az RDP-kapcsolatok toohello háttér-erőforrások létrehozása

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. Hozzon létre egy terhelés-kiegyenlítő szabályokat, amelyek forgalom toodifferent háttér-portok attól függően, hogy melyik előtér kapott hello kérelem elküldése

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. Ellenőrizze a beállításokat

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    Várt kimenet:

        info:    Executing command network lb show
        info:    Looking up hello load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a>Hálózati adapterek létrehozása

Hálózati adaptert létrehozni, és rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételek menüpontban.

1. Hello PowerShell változók beállítása

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. Hozzon létre egy minden háttér hálózati Adapterhez, és adja hozzá az IPv6 konfiguráció.

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-hello-back-end-vm-resources-and-attach-each-nic"></a>Hello háttér-Virtuálisgép-erőforrások létrehozása és csatolása az egyes hálózati adapterek

virtuális gépek toocreate, rendelkeznie kell egy tárfiókot. A terheléselosztás hello virtuális gépek rendelkezésre állási csoport tagjai toobe kell. Virtuális gépek létrehozásával kapcsolatos további információkért lásd: [hozzon létre egy Azure virtuális gép PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)

1. Hello PowerShell változók beállítása

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > A példa hello felhasználónév és jelszó nyílt szövegként hello virtuális gépekhez. Megfelelő gondot kell fordítani, amikor használatával-felhasználó hitelesítő adatait a hello törölje. Egy biztonságosabb módszer a PowerShell hitelesítő adatok kezelésére, lásd: hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.

2. Hello tárolási fiók és a rendelkezésre állási készlet létrehozása

    Használhat meglévő tárfiókot hello virtuális gépek létrehozásakor. a következő parancs hello hoz létre egy új tárfiókot.

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    Ezután hozzon létre hello rendelkezésre állási csoportot.

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. Hello virtuális gépek létrehozására hello társított hálózati adapter

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-cli.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

---
title: "Azure virtuális hálózatok aaaCommon PowerShell-parancsok |} Microsoft Docs"
description: "Közös PowerShell parancsok tooget használatba a virtuális gépek virtuális hálózati és a kapcsolódó erőforrások létrehozása."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b46b78f1b7c5a0c5ba917fb48f568d871e5e8789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Azure virtuális hálózatok általános PowerShell-parancsok

Ha azt szeretné, hogy a virtuális gép toocreate, toocreate kell egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md) vagy egy meglévő virtuális hálózattal kapcsolatos tudja, melyik hello a virtuális gép lehet hozzáadni. Általában a virtuális gépek létrehozásakor is szükséges tooconsider létrehozása ebben a cikkben ismertetett hello erőforrásokat.

Lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) hello Azure PowerShell legújabb verziójának telepítése, az előfizetés kiválasztásával és tooyour fiók bejelentkezés kapcsolatos információkat.

Egyes változók akkor lehet hasznos, ha fut hello parancsok egynél több cikkben:

- $location - hello hello hálózati erőforrások helyét. Használhat [Get-AzureRmLocation](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermlocation) toofind egy [földrajzi régió](https://azure.microsoft.com/regions/) meg működik.
- $myResourceGroup - ahol hello hálózati erőforrások találhatók hello erőforráscsoport hello nevét.

## <a name="create-network-resources"></a>Hálózati erőforrások létrehozása

| Tevékenység | Parancs |
| ---- | ------- |
| Az alhálózati beállítások létrehozása |$Alhalozat_1 = [új AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" - AddressPrefix XX. X.X.X/XX<BR>$Alhalozat_2 = új AzureRmVirtualNetworkSubnetConfig-Name "mySubnet2" - AddressPrefix XX. X.X.X/XX<BR><BR>Előfordulhat, hogy egy tipikus hálózati az alhálózat egy [internetre irányuló terheléselosztót](../../load-balancer/load-balancer-internet-overview.md) és egy külön alhálózatot a egy [belső terheléselosztó](../../load-balancer/load-balancer-internal-overview.md). |
| Virtuális hálózat létrehozása |$vnet = [New-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetwork) -Name "myVNet" - ResourceGroupName $myResourceGroup-hely $location - AddressPrefix XX. X.X.X/XX-Alhalozat_2 $ $Alhalozat_1, alhálózati |
| Egy egyedi nevet a teszt |[Teszt-AzureRmDnsAvailability](https://docs.microsoft.com/powershell/module/azurerm.network/test-azurermdnsavailability) - DomainNameLabel "myDNS"-$location helye<BR><BR>Megadhatja a DNS-tartománynevet a [nyilvános IP-erőforrás](../../virtual-network/virtual-network-ip-addresses-overview-arm.md), ami létrehoz domainname.location.cloudapp.azure.com toohello nyilvános IP-cím hozzárendelése a hello Azure által kezelt DNS-kiszolgálók. hello név csak betűket, számokat és kötőjeleket tartalmazhat. hello első és utolsó karakter betű vagy szám és kell hello tartomány nevét az Azure-beli hely belül egyedieknek kell lenniük. Ha **igaz** lett visszaadva, akkor a választott név globálisan egyedi. |
| Hozzon létre egy nyilvános IP-címet |$pip = [New-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermpublicipaddress) -Name "myPublicIp" - ResourceGroupName $myResourceGroup - DomainNameLabel "myDNS"-hely $location - AllocationMethod dinamikus<BR><BR>hello nyilvános IP-cím, amelyet korábban tesztelése, és hello előtérbeli konfigurációja hello terheléselosztó által használt hello nevét használja. |
| Előtérbeli IP-konfiguráció létrehozása |$frontendIP = [New-AzureRmLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerfrontendipconfig) -Name "myFrontendIP" - PublicIpAddress $pip<BR><BR>hello előtérbeli konfigurációja magában foglalja a hello nyilvános IP-cím, amelyet korábban hozott létre a bejövő hálózati forgalmat. |
| Háttér-címkészlet létrehozása |$beAddressPool = [New-AzureRmLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerbackendaddresspoolconfig) -Name "myBackendAddressPool"<BR><BR>Itt belső címek hello háttérkiszolgáló hello a terheléselosztó, amely egy hálózati adapteren keresztül érhetők el. |
| A mintavétel létrehozása |$healthProbe = [New-AzureRmLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerprobeconfig) -Name "myProbe" - RequestPath "HealthProbe.aspx"-protokoll http – 80-as Port - IntervalInSeconds 15 - ProbeCount 2<BR><BR>Virtuális gépek példánya hello háttércímkészletet állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása tartalmazza. |
| Terheléselosztási szabály létrehozása |$lbRule = [New-AzureRmLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerruleconfig) -név HTTP - FrontendIpConfiguration $frontendIP - BackendAddressPool $beAddressPool-$healthProbe mintavételi-protokoll Tcp - FrontendPort 80 - BackendPort 80<BR><BR>Rendelje hozzá a nyilvános port hello terheléselosztón hello háttércímkészletet tooa port szabályokat tartalmaz. |
| Egy bejövő NAT-szabály létrehozása |$inboundNATRule = [New-AzureRmLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancerinboundnatruleconfig) -Name "myInboundRule1" - FrontendIpConfiguration $frontendIP-protokoll TCP - FrontendPort 3441 - BackendPort 3389-es<BR><BR>Egy nyilvános port hello terhelés terheléselosztó tooa porton egy adott virtuális gép hello háttércímkészletet a leképezési szabályokat tartalmaz. |
| Terheléselosztó létrehozása |$loadBalancer = [New-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermloadbalancer) - ResourceGroupName $myResourceGroup-Name "myLoadBalancer"-hely $location - FrontendIpConfiguration $frontendIP - InboundNatRule $inboundNATRule - LoadBalancingRule $ lbRule - BackendAddressPool $beAddressPool-$healthProbe mintavétel |
| A hálózati illesztő létrehozása |$nic1 = [New-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermnetworkinterface) - ResourceGroupName $myResourceGroup-Name "myNIC" - hely $location - privateipaddress tulajdonságot XX. X.X.X-alhálózat $Alhalozat_2 - LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] - LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Egy adott hálózati csatoló hello nyilvános IP-cím és a korábban létrehozott virtuális hálózati alhálózat létrehozása. |

## <a name="get-information-about-network-resources"></a>Hálózati erőforrás adatainak beolvasása

| Tevékenység | Parancs |
| ---- | ------- |
| Virtuális hálózatok listája |[Get-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetwork) - ResourceGroupName $myResourceGroup<BR><BR>Felsorolja az összes hello virtuális hálózatok hello erőforráscsoportban. |
| A virtuális hálózat adatainak megjelenítése |Get-AzureRmVirtualNetwork-Name "myVNet" - ResourceGroupName $myResourceGroup |
| Egy virtuális hálózat alhálózatai lista |Get-AzureRmVirtualNetwork-Name "myVNet" - ResourceGroupName $myResourceGroup &#124; Válassza ki az alhálózatot |
| Egy alhálózat adatainak beolvasása |[Get-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworksubnetconfig) -Name "mySubnet1" - VirtualNetwork $vnet<BR><BR>A megadott virtuális hálózati hello hello alhálózati információ lekérése. hello $vnet érték a Get-AzureRmVirtualNetwork által visszaadott hello az objektumot határozza meg. |
| IP-címeinek listáját |[Get-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermpublicipaddress) - ResourceGroupName $myResourceGroup<BR><BR>Nyilvános IP-címek hello hello erőforráscsoportban listája. |
| Lista terheléselosztók |[Get-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermloadbalancer) - ResourceGroupName $myResourceGroup<BR><BR>Felsorolja az összes hello terheléselosztók hello erőforráscsoportban. |
| Hálózati illesztők listája |[Get-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterface) - ResourceGroupName $myResourceGroup<BR><BR>Hello erőforráscsoport összes hello hálózati illesztők listája. |
| Egy adott hálózati csatoló adatainak beolvasása |Get-AzureRmNetworkInterface-Name "myNIC" - ResourceGroupName $myResourceGroup<BR><BR>Egy adott hálózati illesztőn információ lekérése. |
| A hálózati illesztő IP-konfigurációja hello beolvasása |[Get-AzureRmNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkinterfaceipconfig) -Name "myNICIP" - Kezdőértékről $nic<BR><BR>Hello IP-konfiguráció hello megadott hálózati adapter információ lekérése. hello $nic érték a Get-AzureRmNetworkInterface által visszaadott hello az objektumot határozza meg. |

## <a name="manage-network-resources"></a>Hálózati erőforrások kezelése

| Tevékenység | Parancs |
| ---- | ------- |
| Egy alhálózat tooa virtuális hálózat hozzáadása |[Adja hozzá AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig) - AddressPrefix XX. X.X.X/XX-Name "mySubnet1" - VirtualNetwork $vnet<BR><BR>Egy alhálózati tooan meglévő virtuális hálózat hozzáadása. hello $vnet érték a Get-AzureRmVirtualNetwork által visszaadott hello az objektumot határozza meg. |
| A virtuális hálózat törlése |[Remove-AzureRmVirtualNetwork](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermvirtualnetwork) -Name "myVNet" - ResourceGroupName $myResourceGroup<BR><BR>Hello megadott virtuális hálózati eltávolítása hello erőforráscsoportot. |
| A hálózati illesztő törlése |[Remove-AzureRmNetworkInterface](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermnetworkinterface) -Name "myNIC" - ResourceGroupName $myResourceGroup<BR><BR>Hello megadott hálózati adapter eltávolítása hello erőforráscsoportot. |
| Terheléselosztó törlése |[Remove-AzureRmLoadBalancer](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermloadbalancer) -Name "myLoadBalancer" - ResourceGroupName $myResourceGroup<BR><BR>Hello eltávolítja a megadott terheléselosztó hello erőforráscsoportból. |
| A nyilvános IP-cím törlése |[Remove-AzureRmPublicIpAddress](https://docs.microsoft.com/powershell/module/azurerm.network/remove-azurermpublicipaddress)-Name "myIPAddress" - ResourceGroupName $myResourceGroup<BR><BR>Eltávolítja a hello megadott hello erőforráscsoportból nyilvános IP-cím. |

## <a name="next-steps"></a>Következő lépések
* Akkor használja hello hálózati adaptert, most kell létrehozni, [hozzon létre egy virtuális Gépet](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* További tudnivalók: hogyan zajlik [több hálózati adapterrel rendelkező virtuális gép létrehozása](../../virtual-network/virtual-networks-multiple-nics.md).


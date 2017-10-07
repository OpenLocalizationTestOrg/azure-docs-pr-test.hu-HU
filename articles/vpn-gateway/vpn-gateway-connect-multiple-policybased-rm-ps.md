---
title: "Csatlakozás az Azure VPN átjárók toomultiple a helyi csoportházirend-alapú VPN-eszközök: Azure Resource Manager: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan konfigurálása Azure útvonalalapú VPN gateway toomultiple házirendalapú VPN-eszközök Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/27/2017
ms.author: yushwang
ms.openlocfilehash: 866c78d96305207106a66cc3300c355e4b6bfbb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-vpn-gateways-toomultiple-on-premises-policy-based-vpn-devices-using-powershell"></a>Csatlakozás Azure VPN-átjárók toomultiple helyszíni házirendalapú VPN eszközök PowerShell használatával

Ez a cikk segítséget nyújt az Azure útválasztó-alapú VPN átjáró tooconnect toomultiple a helyi csoportházirend-alapú VPN-eszközök egyéni IPsec/IKE-házirendek S2S VPN-kapcsolatok számát, ami konfigurálásához.

## <a name="about-policy-based-and-route-based-vpn-gateways"></a>Házirend- és útvonal-alapú VPN gatewayek kapcsolatban

Házirend - *és* útvonalalapú VPN-eszközök nem egyezik a hello IPsec forgalom választók kapcsolat beállításának módját:

* **Csoportházirend-alapú** VPN-eszközök használata mindkét hálózatok toodefine hogyan forgalom titkosítása/visszafejtése IPsec-alagutakon keresztül előtagjait hello kombinációi. Hálózaticsomag-szűrés végző tűzfal eszközök általában épül. IPsec-alagút titkosítási és visszafejtési kerülnek toohello hálózaticsomag-szűrés és a program.
* **Útválasztó-alapú** VPN-eszközök használata bármilyen közöttiként (helyettesítő) forgalmat választók, és lehetővé teszik útválasztás/továbbító táblák közvetlen forgalom toodifferent IPsec-alagutak. Ha minden IPsec-alagutat van modellezve a hálózati adaptert vagy VTI (virtuális alagút-illesztő) útválasztó platformokon általában be van építve.

hello alábbi ábrák jelöljön ki két modell hello:

### <a name="policy-based-vpn-example"></a>A házirendalapú VPN – példa
![Csoportházirend-alapú](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedmultisite.png)

### <a name="route-based-vpn-example"></a>Útvonalalapú VPN – példa
![útválasztó-alapú](./media/vpn-gateway-connect-multiple-policybased-rm-ps/routebasedmultisite.png)

### <a name="azure-support-for-policy-based-vpn"></a>Azure házirendalapú VPN-támogatás
Jelenleg Azure támogatja-e a VPN-átjárók mindkét módnál: útvonalalapú VPN-átjárók és a csoportházirend-alapú VPN gatewayek. A beépített különböző platformokon belső, a különböző specifikációihoz eredmények:

|                          | **PolicyBased VPN-átjáró** | **RouteBased VPN-átjáró**               |
| ---                      | ---                         | ---                                      |
| **Az Azure átjáró-Termékváltozat**    | Basic                       | Basic, Standard, a HighPerformance         |
| **IKE-verzió**          | IKEv1                       | IKEv2                                    |
| **Max. S2S-kapcsolatok** | **1**                       | Basic vagy Standard: 10<br> HighPerformance: 30 |
|                          |                             |                                          |

Az egyéni IPsec/IKE házirend hello, most már beállíthatja Azure útvonalalapú VPN átjárók toouse előtag-alapú forgalom választók kapcsolóval "**PolicyBasedTrafficSelectors**", tooconnect tooon helyi csoportházirend-alapú VPN-eszközök. Ez a funkció lehetővé teszi az Azure virtuális hálózat tooconnect és VPN-átjáró toomultiple a helyszíni házirendalapú VPN/tűzfal eszközök, hello aktuális Azure házirendalapú VPN gatewayek eltávolítása a hello egyetlen kapcsolathoz megadott korlátot.

> [!IMPORTANT]
> 1. tooenable a kapcsolatot, a helyi csoportházirend-alapú VPN-eszközök támogatnia kell a **IKEv2** tooconnect toohello Azure VPN gatewayek útválasztó-alapú. Ellenőrizze a VPN-eszköz specifikációja.
> 2. a mechanizmus a házirendalapú VPN-eszközök keresztül csatlakozó hello helyszíni hálózatokhoz csak kapcsolódhatnak toohello Azure virtuális hálózat; **azok nem tranzit tooother a helyszíni hálózatokban vagy keresztül virtuális hálózatok ugyanazon Azure VPN gatewaynél hello**.
> 3. hello konfigurációs beállítás hello egyéni IPsec/IKE kapcsolatkezelési házirendet részét képezi. Ha hello csoportházirend-alapú forgalom választó lehetőséget választja, meg kell adnia a teljes házirend hello (a IPsec/IKE titkosítás és sértetlenség algoritmusokról, a kulcs szintjeiről és a biztonsági Társítások élettartama).

a következő diagram hello jeleníti meg, miért tranzit útválasztást Azure VPN-átjárón keresztül nem működik együtt a házirend alapú beállítás hello:

![Csoportházirend-alapú átvitel közben](./media/vpn-gateway-connect-multiple-policybased-rm-ps/policybasedtransit.png)

Hello ábrán is látható, hello Azure VPN gateway hello virtuális hálózati tooeach hello a helyszíni hálózat előtagjai, de nem hello kereszt-kapcsolat előtagok a forgalom választók van. Például a helyszíni hely 2, 3 hely és hely 4 egyes kommunikálhatnak tooVNet1 rendre, de hello Azure VPN gateway tooeach más keresztül nem tud csatlakozni. hello látható hello-elosztó, amely nem szerepel a hello Azure VPN gateway ilyen konfiguráció forgalom választók.

## <a name="configure-policy-based-traffic-selectors-on-a-connection"></a>Csoportházirend-alapú forgalom választók a kapcsolat konfigurálása

hello Ez a cikk kövesse az utasításokat hello azonos példában ismertetett módon [konfigurálása IPsec/IKE házirend S2S és VNet – VNet kapcsolatokhoz](vpn-gateway-ipsecikepolicy-rm-powershell.md) tooestablish S2S VPN-kapcsolatot. Ez a következő diagram hello jelenik meg:

![s2s-házirend](./media/vpn-gateway-connect-multiple-policybased-rm-ps/s2spolicypb.png)

munkafolyamat tooenable Ez az összekapcsolhatóság hello:
1. Hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró a létesítmények közötti kapcsolat létrehozása
2. IPsec/IKE-házirend létrehozása
3. Hello házirend alkalmazása, ha S2S vagy VNet – VNet-kapcsolatot, és **hello csoportházirend-alapú forgalom választók engedélyezése** hello kapcsolaton
4. Ha hello kapcsolatot hozott létre, alkalmazása vagy hello házirend tooan meglévő kapcsolat frissítése

## <a name="enable-policy-based-traffic-selectors-on-a-connection"></a>Csoportházirend-alapú forgalom választók kapcsolat engedélyezése

Győződjön meg arról, hogy befejeződött [3. rész hello konfigurálása IPsec/IKE házirend cikk](vpn-gateway-ipsecikepolicy-rm-powershell.md) az ebben a szakaszban. a következő példában hello hello azonos paraméterek és lépéseket:

### <a name="step-1---create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>1. lépés – hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása

#### <a name="1-declare-your-variables--connect-tooyour-subscription"></a>1. Deklarálja a változókat, és csatlakoztathatja tooyour előfizetés
Ehhez a gyakorlathoz először is deklarálni kell a változókat. Lehet, hogy tooreplace hello értékeket a saját üzemi konfigurálásakor.

```powershell
$Sub1          = "<YourSubscriptionName>"
$RG1           = "TestPolicyRG1"
$Location1     = "East US 2"
$VNetName1     = "TestVNet1"
$FESubName1    = "FrontEnd"
$BESubName1    = "Backend"
$GWSubName1    = "GatewaySubnet"
$VNetPrefix11  = "10.11.0.0/16"
$VNetPrefix12  = "10.12.0.0/16"
$FESubPrefix1  = "10.11.0.0/24"
$BESubPrefix1  = "10.12.0.0/24"
$GWSubPrefix1  = "10.12.255.0/27"
$DNS1          = "8.8.8.8"
$GWName1       = "VNet1GW"
$GW1IPName1    = "VNet1GWIP1"
$GW1IPconf1    = "gw1ipconf1"
$Connection16  = "VNet1toSite6"

$LNGName6      = "Site6"
$LNGPrefix61   = "10.61.0.0/16"
$LNGPrefix62   = "10.62.0.0/16"
$LNGIP6        = "131.107.72.22"
```
Erőforrás-kezelő parancsmagok toouse hello váltson át tooPowerShell mód. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók. A következő minta toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="2-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>2. Hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása
a következő példa hello hello virtuális hálózatot, három alhálózatot, és hello VPN-átjáró TestVNet1 hoz létre. És az értékeket, fontos, hogy mindig nevezze el az átjáró alhálózatának kifejezetten a "GatewaySubnet". Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.

```powershell
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName1 -AddressPrefix $FESubPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName1 -AddressPrefix $BESubPrefix1
$gwsub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName1 -AddressPrefix $GWSubPrefix1

New-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNetPrefix11,$VNetPrefix12 -Subnet $fesub1,$besub1,$gwsub1

$gw1pip1    = New-AzureRmPublicIpAddress -Name $GW1IPName1 -ResourceGroupName $RG1 -Location $Location1 -AllocationMethod Dynamic
$vnet1      = Get-AzureRmVirtualNetwork -Name $VNetName1 -ResourceGroupName $RG1
$subnet1    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet1
$gw1ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW1IPconf1 -Subnet $subnet1 -PublicIpAddress $gw1pip1

New-AzureRmVirtualNetworkGateway -Name $GWName1 -ResourceGroupName $RG1 -Location $Location1 -IpConfigurations $gw1ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance

New-AzureRmLocalNetworkGateway -Name $LNGName6 -ResourceGroupName $RG1 -Location $Location1 -GatewayIpAddress $LNGIP6 -AddressPrefix $LNGPrefix61,$LNGPrefix62
```

### <a name="step-2---create-a-s2s-vpn-connection-with-an-ipsecike-policy"></a>2. lépés - az IPsec/IKE házirendjével S2S VPN-kapcsolat létrehozása

#### <a name="1-create-an-ipsecike-policy"></a>1. IPsec/IKE-házirend létrehozása

> [!IMPORTANT]
> Rendelés tooenable hello kapcsolat "UsePolicyBasedTrafficSelectors" kapcsolót egy IPsec-/ h.rend toocreate van szüksége.

hello alábbi példa házirendet hoz létre IPsec/IKE ezeket az algoritmusokat és paraméterek:
* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA-256, PFS24, 2048KB & SA élettartam 3600 másodperc

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-policy-based-traffic-selectors-and-ipsecike-policy"></a>2. Hello S2S VPN-kapcsolat létrehozása a csoportházirend-alapú forgalom választók és IPsec/IKE házirend
S2S VPN-kapcsolat létrehozásához, és hello előző lépésben létrehozott hello IPsec/IKE házirend alkalmazása. Ügyeljen a hello további paraméter "-UsePolicyBasedTrafficSelectors $True" lehetővé teszi a csoportházirend-alapú forgalom választók hello kapcsolaton.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -UsePolicyBasedTrafficSelectors $True -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Hello lépések végrehajtását követően hello S2S VPN-kapcsolat lesz hello IPsec/IKE házirend lett meghatározva, és engedélyezheti a házirend-alapú forgalom választók hello kapcsolaton. Ismételje meg az azonos lépések tooadd további kapcsolatok tooadditional a helyi csoportházirend-alapú VPN-eszközök a hello hello ugyanazon Azure VPN gatewaynél.

## <a name="update-policy-based-traffic-selectors-for-a-connection"></a>Csoportházirend-alapú forgalom választók kapcsolat frissítése
hello utolsó szakasza bemutatja, hogyan tooupdate hello csoportházirend-alapú forgalom választók lehetőséget egy meglévő S2S VPN-kapcsolathoz.

### <a name="1-get-hello-connection"></a>1. Hello-kapcsolat lekérdezése
Hello kapcsolati erőforrást beolvasni.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
```

### <a name="2-check-hello-policy-based-traffic-selectors-option"></a>2. Jelölje be a házirend-alapú forgalom hello választók beállítást
hello következő sor látható használja-e hello csoportházirend-alapú forgalom választók hello kapcsolathoz:

```powershell
$connection6.UsePolicyBasedTrafficSelectors
```

Ha hello sort adja vissza "**igaz**", majd csoportházirend-alapú forgalom választók hello kapcsolat van konfigurálva; egyéb esetben ad vissza "**hamis**."

### <a name="3-update-hello-policy-based-traffic-selectors-on-a-connection"></a>3. Hello csoportházirend-alapú forgalom választók a kapcsolat frissítése
Miután beszerezte hello kapcsolati erőforrást, engedélyezheti vagy hello tiltható le.

#### <a name="disable-usepolicybasedtrafficselectors"></a>Tiltsa le a UsePolicyBasedTrafficSelectors
hello következő példa letiltja hello csoportházirend-alapú forgalom választók beállítást, de hagyja változatlanul IPsec/IKE házirend hello:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $False
```

#### <a name="enable-usepolicybasedtrafficselectors"></a>UsePolicyBasedTrafficSelectors engedélyezése
hello következő példa engedélyezi hello csoportházirend-alapú forgalom választók, de hagyja változatlanul IPsec/IKE házirend hello:

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -UsePolicyBasedTrafficSelectors $True
```

## <a name="next-steps"></a>Következő lépések
Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Emellett nézze át az [konfigurálása IPsec/IKE házirend S2S VPN- és VNet – VNet kapcsolatokhoz](vpn-gateway-ipsecikepolicy-rm-powershell.md) egyéni IPsec/IKE-házirendekkel kapcsolatos további részletekért.

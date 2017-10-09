---
title: "S2S VPN- és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása: Azure Resource Manager: PowerShell |} Microsoft Docs"
description: "Ez a cikk bemutatja, hogyan S2S és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása az Azure VPN Gatewayek Azure Resource Manager és a PowerShell használatával."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager
ms.assetid: 238cd9b3-f1ce-4341-b18e-7390935604fa
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: yushwang
ms.openlocfilehash: f8d2e29276efdec7071f2aa0d463b1abd64a5253
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ipsecike-policy-for-s2s-vpn-or-vnet-to-vnet-connections"></a>S2S VPN- és VNet – VNet kapcsolatokhoz IPsec/IKE-házirend konfigurálása

Ez a cikk bemutatja, hogyan hello lépéseket tooconfigure IPsec/h.rend hello Resource Manager üzembe helyezési modellben és a PowerShell használatával telephelyek közötti VPN- és VNet – VNet kapcsolatokhoz.

## <a name="about"></a>Az Azure VPN gatewayek IPsec és az internetes KULCSCSERE házirend paraméterek
Standard IPsec és IKE protokoll titkosítási algoritmusok számos különböző kombinációkban támogatja. Tekintse meg a túl[kriptográfiai követelményeiről és az Azure VPN gatewayek](vpn-gateway-about-compliance-crypto.md) toosee hogyan Ez segítheti a létesítmények közötti és VNet – VNet-kapcsolatot a megfelelőségi és biztonsági követelmények kielégítéséhez.

Ez a cikk utasításokat toocreate biztosít, és IPsec/IKE-szabályzat beállítása és tooa új vagy meglévő kapcsolat alkalmazni:

* [1. rész - munkafolyamat toocreate és IPsec/IKE házirendjének beállítása](#workflow)
* [2. rész - támogatott titkosítási algoritmusok és a kulcs szintjeiről](#params)
* [3. rész – hozzon létre egy új S2S VPN-kapcsolat IPsec/IKE-házirend](#crossprem)
* [Rész 4 – hozzon létre egy új VNet – VNet-kapcsolatot IPsec/IKE-házirend](#vnet2vnet)
* [5 - rész kezelése (létrehozása, hozzáadása, eltávolítása) kapcsolat IPsec/IKE-házirend](#managepolicy)

> [!IMPORTANT]
> 1. Vegye figyelembe, hogy IPsec/IKE házirend csak akkor működik a hello gateway SKU-n a következő:
>    * ***VpnGw1, VpnGw2, VpnGw3*** (útválasztó-alapú)
>    * ***Standard*** és ***HighPerformance*** (útválasztó-alapú)
> 2. Egy adott kapcsolathoz csak ***egy*** házirendet adhat meg.
> 3. Meg kell adnia a internetes KULCSCSERE (alapmód) és a IPsec (gyorsmódú) algoritmusok és a paraméterek. A részleges házirend-megadás nem engedélyezett.
> 4. Vegye fel a kapcsolatot a VPN szállító műszaki tooensure hello házirend a helyszíni VPN-eszközök esetén támogatott. S2S vagy a VNet – VNet kapcsolatokhoz nem tud Ha hello házirendek nem kompatibilisek.

## <a name ="workflow"></a>1. rész - munkafolyamat toocreate és IPsec/IKE házirendjének beállítása
Ez a szakasz ismerteti a hello munkafolyamat toocreate és frissítés IPsec/IKE házirend S2S VPN- vagy a VNet – VNet használ:
1. Virtuális hálózat és VPN-átjáró létrehozása
2. Helyi hálózati átjáró a helyi kapcsolat vagy egy másik virtuális hálózati közötti és VNet – VNet-kapcsolatot az átjáró létrehozása
3. Hozzon létre egy IPsec/IKE házirendet a kijelölt algoritmusok és paraméterek
4. (Az IPsec vagy VNet2VNet) kapcsolat létrehozása a hello IPsec/IKE házirend
5. Frissítés/hozzáadása egy meglévő kapcsolat az IPsec/IKE házirend

jelen cikkben lévő utasítások hello segít beállítása és konfigurálása IPsec/IKE házirendek hello ábrán látható módon:

![IPSec-ike-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)

## <a name ="params"></a>2. rész - támogatott titkosítási algoritmusok és a kulcs szintjeiről

hello alábbi táblázat hello támogatott titkosítási algoritmusok és a kulcs szintjeiről konfigurálható hello ügyfelek:

| **IPsec/IKEv2**  | **Beállítások**    |
| ---  | --- 
| IKEv2-titkosítás | AES256, AES192, AES128, DES3, DES  
| IKEv2-integritás  | SHA384, MD5, SHA1, SHA256  |
| DH-csoport         | DHGroup24, ECP384, ECP256, DHGroup14, DHGroup2048, DHGroup2, DHGroup1, None |
| IPsec-titkosítás | GCMAES256, GCMAES192, GCMAES128, AES256, AES192, AES128, DES3, DES, Nincs    |
| IPsec-integritás  | GCMASE256, GCMAES192, GCMAES128, SHA-256, SHA1, MD5 |
| PFS-csoport        | PFS24, ECP384, ECP256, PFS2048, PFS2, PFS1, Nincs 
| Gyorsmódú biztonsági társítás élettartama   | (**Nem kötelező**: alapértelmezett értékek vannak használt Ha nincs megadva)<br>Másodperc (egész szám; **min. 300**/alapértelmezett érték: 27000 másodperc)<br>KB (egész szám; **min. 1024**/alapértelmezett érték: 102400000 KB)   |
| Forgalomválasztó | UsePolicyBasedTrafficSelectors ** ($True vagy $False; **Nem kötelező**, alapértelmezett $False Ha nincs megadva)    |
|  |  |

> [!IMPORTANT]
> 1. **GCMAES mint IPsec titkosítási algoritmus használata esetén ki kell választania azonos GCMAES algoritmus és a kulcshossz hello IPSec-integritásának; például mind a GCMAES128 használatával**
> 2. IKEv2 alapmódú biztonsági Társítás élettartama hello Azure VPN gatewayek a 28 800 másodperc van rögzítve.
> 3. "UsePolicyBasedTrafficSelectors" beállítás túl$ True kapcsolaton konfigurálja hello Azure VPN gateway tooconnect toopolicy-alapú VPN tűzfal a helyszínen. Ha engedélyezi az PolicyBasedTrafficSelectors, kell-e a VPN-eszköz van hello meghatározott összes kiegészítve a helyi hálózati (helyi hálózati átjáró) előtagok hello Azure-beli virtuális hálózat előtagokat, és a megfelelő forgalmat választók tooensure nem minden elem közöttiként. Például ha a helyszíni hálózati előtagok 10.1.0.0/16 és 10.2.0.0/16, és a virtuális hálózati előtagok 192.168.0.0/16 és 172.16.0.0/16, kell a következő forgalmat választók toospecify hello:
>    * 10.1.0.0/16 <====> 192.168.0.0/16
>    * 10.1.0.0/16 <====> 172.16.0.0/16
>    * 10.2.0.0/16 <====> 192.168.0.0/16
>    * 10.2.0.0/16 <====> 172.16.0.0/16

Csoportházirend-alapú forgalom választók kapcsolatos további információkért lásd: [csatlakozás több helyszíni házirendalapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md).

a következő táblázat hello hello egyéni házirend által támogatott megfelelő Diffie-Hellman csoport hello:

| **Diffie-Hellman csoport**  | **DH-csoport**              | **PFS-csoport** | **A kulcs hossza** |
| --- | --- | --- | --- |
| 1                         | DHGroup1                 | PFS1         | 768 bites MODP   |
| 2                         | DHGroup2                 | PFS2         | 1024 bites MODP  |
| 14                        | DHGroup14<br>DHGroup2048 | PFS2048      | 2048 bites MODP  |
| 19                        | ECP256                   | ECP256       | 256 bites ECP    |
| 20                        | ECP384                   | ECP284       | 384 bites ECP    |
| 24                        | DHGroup24                | PFS24        | 2048 bites MODP  |

Tekintse meg a túl[RFC3526](https://tools.ietf.org/html/rfc3526) és [RFC5114](https://tools.ietf.org/html/rfc5114) további részleteket.

## <a name ="crossprem"></a>3. rész – hozzon létre egy új S2S VPN-kapcsolat IPsec/IKE-házirend

Ez a szakasz végigvezeti hello S2S VPN-kapcsolat létrehozása az IPsec/IKE házirendjével. hello lépések hello kapcsolat létrehozása hello ábrán látható módon:

![s2s-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/s2spolicy.png)

Lásd: [S2S VPN-kapcsolatot](vpn-gateway-create-site-to-site-rm-powershell.md) részletesebb lépésenkénti S2S VPN-kapcsolat létrehozásához.

### <a name="before"></a>Előkészületek

* Győződjön meg arról, hogy rendelkezik Azure-előfizetéssel. Ha még nincs Azure-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), vagy regisztrálhat egy [ingyenes fiókot](https://azure.microsoft.com/pricing/free-trial/).
* Hello Azure Resource Manager PowerShell-parancsmagjainak telepítése. Lásd: [áttekintés az Azure PowerShell](/powershell/azure/overview) hello PowerShell-parancsmagok telepítéséről további információt.

### <a name="createvnet1"></a>1. lépés – hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása

#### <a name="1-declare-your-variables"></a>1. A változók deklarálása

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

#### <a name="2-connect-tooyour-subscription-and-create-a-new-resource-group"></a>2. Csatlakozás tooyour előfizetés, és hozzon létre egy új erőforráscsoportot

Váltson át tooPowerShell mód toouse hello erőforrás-kezelő parancsmagokat. További információ: [A Windows PowerShell használata a Resource Managerrel](../powershell-azure-resource-manager.md).

Nyissa meg a PowerShell-konzolt, és csatlakozzon a tooyour fiók. A következő minta toohelp csatlakozás hello használata:

```powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName $Sub1
New-AzureRmResourceGroup -Name $RG1 -Location $Location1
```

#### <a name="3-create-hello-virtual-network-vpn-gateway-and-local-network-gateway"></a>3. Hello virtuális hálózat, a VPN-átjáró és a helyi hálózati átjáró létrehozása

a következő minta hello hello virtuális hálózat, TestVNet1, hoz létre három alhálózatok és hello VPN-átjáró. Az értékek behelyettesítésekor fontos, hogy az átjáróalhálózat neve mindenképp GatewaySubnet legyen. Ha ezt másként nevezi el, az átjáró létrehozása meghiúsul.

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

### <a name="s2sconnection"></a>2. lépés - az IPsec/IKE házirendjével S2S VPN-kapcsolat létrehozása

#### <a name="1-create-an-ipsecike-policy"></a>1. IPsec/IKE-házirend létrehozása

a következő mintaparancsfájl hello házirendet hoz létre IPsec/IKE hello algoritmusok és a paraméterek a következő:

* IKEv2: AES256, SHA384 DHGroup24
* IPsec: AES256, SHA-256, PFS24, 2048KB & SA élettartama 7200 másodperc

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption AES256 -IpsecIntegrity SHA256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

Ha GCMAES használja az IPsec, használnia kell hello azonos GCMAES algoritmus és a kulcshossz IPsec titkosításhoz és integritását, például:

* IKEv2: AES256, SHA384 DHGroup24
* IPsec: **GCMAES256, GCMAES256**, PFS24, 2048 KB & SA élettartama 7200 másodperc

```powershell
$ipsecpolicy6 = New-AzureRmIpsecPolicy -IkeEncryption AES256 -IkeIntegrity SHA384 -DhGroup DHGroup24 -IpsecEncryption GCMAES256 -IpsecIntegrity GCMAES256 -PfsGroup PFS24 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 2048
```

#### <a name="2-create-hello-s2s-vpn-connection-with-hello-ipsecike-policy"></a>2. IPsec-/ h.rend hello hello S2S VPN-kapcsolat létrehozása

S2S VPN-kapcsolat létrehozásához, és a korábban létrehozott hello IPsec/IKE házirend alkalmazása.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$lng6 = Get-AzureRmLocalNetworkGateway  -Name $LNGName6 -ResourceGroupName $RG1

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -LocalNetworkGateway2 $lng6 -Location $Location1 -ConnectionType IPsec -IpsecPolicies $ipsecpolicy6 -SharedKey 'AzureA1b2C3'
```

Opcionálisan hozzáadhat "-UsePolicyBasedTrafficSelectors $True" toohello kapcsolat létrehozása parancsmag tooenable Azure VPN gateway tooconnect toopolicy-alapú VPN-eszközök a helyszínen, fent leírt módon.

> [!IMPORTANT]
> Miután egy IPsec-/ h.rend kapcsolat van megadva, hello Azure VPN gateway csak elküldi vagy hello IPsec/IKE javaslat a megadott titkosítási algoritmusok és a kulcs szintjeiről a adott kapcsolat elfogadása. Győződjön meg arról, a helyszíni VPN-eszköz kapcsolat hello használ, vagy fogadja hello pontos házirend kombináció, ellenkező esetben nem főkiszolgálójával hello S2S VPN-alagúton.


## <a name ="vnet2vnet"></a>Rész 4 – hozzon létre egy új VNet – VNet-kapcsolatot IPsec/IKE-házirend

hello VNet – VNet kapcsolat létrehozásának olyan IPsec/IKE házirend lépésekre hasonló toothat egy S2S VPN-kapcsolat. hello következő minta parancsfájlokat hello kapcsolat létrehozása hello ábrán látható módon:

![v2v-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/v2vpolicy.png)

Lásd: [VNet – VNet-kapcsolatot](vpn-gateway-vnet-vnet-rm-ps.md) részletes lépéseket a VNet – VNet-kapcsolat létrehozásához. Meg kell adnia a [3. rész](#crossprem) toocreate hello VPN Gateway és TestVNet1 konfigurálni.

### <a name="createvnet2"></a>1. lépés – hello második virtuális hálózat és a VPN-átjáró létrehozása

#### <a name="1-declare-your-variables"></a>1. A változók deklarálása

Lehet, hogy tooreplace hello értékeket hasonlíthatja hello megjeleníteni kívánt toouse a konfigurációhoz.

```powershell
$RG2          = "TestPolicyRG2"
$Location2    = "East US 2"
$VNetName2    = "TestVNet2"
$FESubName2   = "FrontEnd"
$BESubName2   = "Backend"
$GWSubName2   = "GatewaySubnet"
$VNetPrefix21 = "10.21.0.0/16"
$VNetPrefix22 = "10.22.0.0/16"
$FESubPrefix2 = "10.21.0.0/24"
$BESubPrefix2 = "10.22.0.0/24"
$GWSubPrefix2 = "10.22.255.0/27"
$DNS2         = "8.8.8.8"
$GWName2      = "VNet2GW"
$GW2IPName1   = "VNet2GWIP1"
$GW2IPconf1   = "gw2ipconf1"
$Connection21 = "VNet2toVNet1"
$Connection12 = "VNet1toVNet2"
```

#### <a name="2-create-hello-second-virtual-network-and-vpn-gateway-in-hello-new-resource-group"></a>2. Hello második virtuális hálózat és a VPN-átjáró hello új erőforráscsoport létrehozása

```powershell
New-AzureRmResourceGroup -Name $RG2 -Location $Location2

$fesub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubName2 -AddressPrefix $FESubPrefix2
$besub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubName2 -AddressPrefix $BESubPrefix2
$gwsub2 = New-AzureRmVirtualNetworkSubnetConfig -Name $GWSubName2 -AddressPrefix $GWSubPrefix2

New-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2 -Location $Location2 -AddressPrefix $VNetPrefix21,$VNetPrefix22 -Subnet $fesub2,$besub2,$gwsub2

$gw2pip1    = New-AzureRmPublicIpAddress -Name $GW2IPName1 -ResourceGroupName $RG2 -Location $Location2 -AllocationMethod Dynamic
$vnet2      = Get-AzureRmVirtualNetwork -Name $VNetName2 -ResourceGroupName $RG2
$subnet2    = Get-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet" -VirtualNetwork $vnet2
$gw2ipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GW2IPconf1 -Subnet $subnet2 -PublicIpAddress $gw2pip1

New-AzureRmVirtualNetworkGateway -Name $GWName2 -ResourceGroupName $RG2 -Location $Location2 -IpConfigurations $gw2ipconf1 -GatewayType Vpn -VpnType RouteBased -GatewaySku HighPerformance
```

### <a name="step-2---create-a-vnet-tovnet-connection-with-hello-ipsecike-policy"></a>2. lépés - a VNet-toVNet kapcsolat létrehozása a hello IPsec/IKE-házirend

Hasonló toohello S2S VPN-kapcsolat IPsec/IKE-házirend létrehozása, akkor alkalmazza a toopolicy toohello új kapcsolatot.

#### <a name="1-create-an-ipsecike-policy"></a>1. IPsec/IKE-házirend létrehozása

a következő mintaparancsfájl hello különböző IPsec/IKE-házirendet hoz hello algoritmusok és a paraméterek a következő:
* IKEv2: Az AES128, SHA1, DHGroup14
* IPsec: GCMAES128, GCMAES128, PFS14, SA élettartama 7200 másodperc & 4096KB

```powershell
$ipsecpolicy2 = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup PFS14 -SALifeTimeSeconds 7200 -SADataSizeKilobytes 4096
```

#### <a name="2-create-vnet-to-vnet-connections-with-hello-ipsecike-policy"></a>2. VNet – VNet kapcsolatokhoz hello IPsec/IKE-házirend létrehozása

VNet – VNet-kapcsolatot, és létrehozott hello IPsec/IKE házirend alkalmazása. Ebben a példában mindkét átjárók vannak hello ugyanahhoz az előfizetéshez. Így lehetséges toocreate, és mindkét-kapcsolatok konfigurálása hello hello ugyanazon IPsec/IKE házirend ugyanazon PowerShell-munkamenetben.

```powershell
$vnet1gw = Get-AzureRmVirtualNetworkGateway -Name $GWName1  -ResourceGroupName $RG1
$vnet2gw = Get-AzureRmVirtualNetworkGateway -Name $GWName2  -ResourceGroupName $RG2

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection12 -ResourceGroupName $RG1 -VirtualNetworkGateway1 $vnet1gw -VirtualNetworkGateway2 $vnet2gw -Location $Location1 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'

New-AzureRmVirtualNetworkGatewayConnection -Name $Connection21 -ResourceGroupName $RG2 -VirtualNetworkGateway1 $vnet2gw -VirtualNetworkGateway2 $vnet1gw -Location $Location2 -ConnectionType Vnet2Vnet -IpsecPolicies $ipsecpolicy2 -SharedKey 'AzureA1b2C3'
```

> [!IMPORTANT]
> Miután egy IPsec-/ h.rend kapcsolat van megadva, hello Azure VPN gateway csak elküldi vagy hello IPsec/IKE javaslat a megadott titkosítási algoritmusok és a kulcs szintjeiről a adott kapcsolat elfogadása. Győződjön meg arról, hogy hello IPsec-házirendek a mindkét kapcsolatok vannak hello azonos, ellenkező esetben a VNet – VNet-kapcsolatot nem fogja létrehozni.

A lépések elvégzése után hello kapcsolatot néhány perc múlva, és a következő hálózati topológia látható módon hello kezdete hello kell:

![IPSec-ike-házirend](./media/vpn-gateway-ipsecikepolicy-rm-powershell/ipsecikepolicy.png)


## <a name ="managepolicy"></a>Rész 5 - kapcsolat frissítés IPsec/IKE-házirend

hello utolsó szakasza bemutatja, hogyan toomanage IPsec/h.rend létező S2S vagy VNet – VNet kapcsolat. az alábbi hello a gyakorlatban végigvezeti hello műveleteket a kapcsolatot a következő:

1. Hello IPsec/IKE házirend kapcsolódási megjelenítése
2. Adja hozzá vagy hello IPsec/IKE házirend tooa kapcsolat frissítése
3. A kapcsolat hello IPsec/IKE házirend eltávolítása

hello ugyanazokat a lépéseket alkalmazása tooboth S2S és VNet – VNet kapcsolatokhoz.

> [!IMPORTANT]
> IPsec-/ h.rend támogatott *szabványos* és *HighPerformance* csak VPN-átjárók útválasztó-alapú. Alapszintű átjáró hello SKU vagy hello házirendalapú VPN-átjáró nem működik.

#### <a name="1-show-hello-ipsecike-policy-of-a-connection"></a>1. Hello IPsec/IKE házirend kapcsolódási megjelenítése

hello a következő példa bemutatja, hogyan tooget hello kapcsolaton konfigurált IPsec/IKE-szabályzatot. hello parancsfájlok továbbra is a fenti hello gyakorlatokat.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

hello utolsó parancs hello hello kapcsolaton konfigurált aktuális IPsec/IKE házirend sorolja fel, ha van ilyen. a következő minta kimenet hello hello kapcsolat van:

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : AES256
IpsecIntegrity      : SHA256
IkeEncryption       : AES256
IkeIntegrity        : SHA384
DhGroup             : DHGroup24
PfsGroup            : PFS24
```

Ha nincs konfigurált IPsec/IKE házirend, hello parancs (PS > $connection6.policy) egy üres visszatérési lekérdezi. IPsec/IKE hello kapcsolat nincs konfigurálva, azonban, hogy nincs-e egyéni IPsec/IKE házirend nem jelenti. hello tényleges kapcsolat a helyszíni VPN-eszköz és a hello Azure VPN gateway egyezteti hello alapértelmezett házirendet használja.

#### <a name="2-add-or-update-an-ipsecike-policy-for-a-connection"></a>2. A kapcsolat egy IPsec/IKE szabályzat hozzáadásakor vagy módosításakor

hello lépések tooadd egy új házirendet, vagy a kapcsolat egy meglévő házirenddel is frissítés hello ugyanaz: hozzon létre egy új szabályzatot, akkor alkalmazza az új házirend toohello hello kapcsolat.

```powershell
$RG1          = "TestPolicyRG1"
$Connection16 = "VNet1toSite6"
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$newpolicy6   = New-AzureRmIpsecPolicy -IkeEncryption AES128 -IkeIntegrity SHA1 -DhGroup DHGroup14 -IpsecEncryption GCMAES128 -IpsecIntegrity GCMAES128 -PfsGroup None -SALifeTimeSeconds 3600 -SADataSizeKilobytes 2048

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6
```

tooenable "UsePolicyBasedTrafficSelectors" Ha tooan csatlakozás a helyi csoportházirend-alapú VPN-eszköz hozzáadása hello "-UsePolicyBaseTrafficSelectors" paraméter toohello parancsmagot, vagy állítsa be túl$ False toodisable hello lehetőséget:

```powershell
Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6 -IpsecPolicies $newpolicy6 -UsePolicyBasedTrafficSelectors $True
```

Hello kapcsolat kaphat újra toocheck hello házirend frissítése után.

```powershell
$connection6  = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1
$connection6.IpsecPolicies
```

Hello kimenete hello utolsó sora, ahogy az alábbi példa hello kell megjelennie:

```powershell
SALifeTimeSeconds   : 3600
SADataSizeKilobytes : 2048
IpsecEncryption     : GCMAES128
IpsecIntegrity      : GCMAES128
IkeEncryption       : AES128
IkeIntegrity        : SHA1
DhGroup             : DHGroup14--
PfsGroup            : None
```

#### <a name="3-remove-an-ipsecike-policy-from-a-connection"></a>3. Távolítsa el az IPsec/IKE házirendet a kapcsolatot

Kapcsolat hello egyéni házirendet eltávolítja, ha hello Azure VPN gateway visszaállítja-e a háttérben toohello [alapértelmezett IPsec/IKE javaslatok listájának](vpn-gateway-about-vpn-devices.md) és újbóli a egyeztetést végez, a helyszíni VPN-eszközön újra.

```powershell
$RG1           = "TestPolicyRG1"
$Connection16  = "VNet1toSite6"
$connection6   = Get-AzureRmVirtualNetworkGatewayConnection -Name $Connection16 -ResourceGroupName $RG1

$currentpolicy = $connection6.IpsecPolicies[0]
$connection6.IpsecPolicies.Remove($currentpolicy)

Set-AzureRmVirtualNetworkGatewayConnection -VirtualNetworkGatewayConnection $connection6
```

Használhatja ugyanazon parancsfájl toocheck hello hello házirend hello kapcsolatról el lett távolítva.

## <a name="next-steps"></a>Következő lépések

Lásd: [csatlakozás több helyszíni házirendalapú VPN-eszközök](vpn-gateway-connect-multiple-policybased-rm-ps.md) csoportházirend-alapú forgalom választók kapcsolatos további részletekért.

Ha a kapcsolat befejeződött, a virtuális gépek tooyour virtuális hálózatok is hozzáadhat. A lépésekért lásd: [Virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

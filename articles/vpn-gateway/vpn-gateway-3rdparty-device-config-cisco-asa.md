---
title: "Mintakonfiguráció - Cisco ASA eszköz csatlakoztatása az Azure VPN gatewayek |} Microsoft Docs"
description: "Ez a cikk a Cisco ASA eszköz csatlakoztatása az Azure VPN gatewayek minta konfigurációját tartalmazza."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.openlocfilehash: 10466b8928e2cd687f7961a2956b6d60823b82be
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Mintakonfiguráció: Cisco ASA (IKEv2/nem BGP-) eszköz
Ez a cikk ismerteti az Azure VPN gatewayek Cisco ASA eszközök minta konfigurációkat.

## <a name="device-at-a-glance"></a>Egy pillanat alatt eszköz

|                        |                                   |
| ---                    | ---                               |
| Eszköz gyártója          | Cisco                             |
| Eszközmodell           | ASA (adaptív biztonsági készülék) |
| Célverzió         | 8.4+                              |
| Tesztelt modellt           | 5505 ASA                          |
| Tesztelt verziója         | 9.2                               |
| IKE-verzió            | **IKEv2**                         |
| BGP                    | **Nem**                            |
| Az Azure VPN-átjáró típusa | **Útválasztó-alapú** VPN-átjáró       |
|                        |                                   |

> [!NOTE]
> 1. Az alábbi konfigurációs Cisco ASA eszköz csatlakozik az Azure **útválasztó-alapú** házirenddel egyéni IPsec/IKE "UserPolicyBasedTrafficSelectors" beállítást, a VPN-átjáró [Ez a cikk](vpn-gateway-connect-multiple-policybased-rm-ps.md).
> 2. ASA eszközök használatára van szükség **IKEv2** a hozzáférési lista-konfigurációkat, nem VTI-alapú.
> 3. Tekintse meg a VPN szállító műszaki győződjön meg arról, a házirend a helyszíni VPN-eszközök esetén támogatott.

## <a name="vpn-device-requirements"></a>VPN-eszköz követelményei
Az Azure VPN gatewayek szabványos IPsec/IKE protokoll csomagok használatával szeretne létrehozni a S2S VPN-alagutat. Tekintse meg [kapcsolatos VPN-eszközök](vpn-gateway-about-vpn-devices.md) részletes IPsec/IKE protokoll paraméterek és alapértelmezett titkosítási algoritmusok az Azure VPN gatewayek. Opcionálisan megadhat a titkosítási algoritmusok és egy adott kapcsolat kulcs szintjeiről pontos kombinációja a leírtak szerint [titkosítási követelményekkel kapcsolatos](vpn-gateway-about-compliance-crypto.md). Ha olyan titkosítási algoritmusok és a kulcs szintjeiről egyedi kombinációja, ellenőrizze, hogy a VPN-eszközök használatát a vonatkozó előírásokat.

## <a name="single-vpn-tunnel"></a>Egy VPN-alagút
Ez a topológia áll az Azure VPN gateway és a helyszíni VPN-eszköz között egyetlen S2S VPN-alagúton. A VPN-alagúton keresztül nem kötelező beállítani a BGP Protokollt.

![az egyalagutas](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Tekintse meg [egyalagutas telepítő](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) hozhat létre az Azure-konfigurációjának részletes, lépésenkénti útmutatást.

### <a name="network-and-vpn-gateway-information"></a>Hálózati és a VPN-átjáró adatai
Ez a szakasz a paraméterek listázása az alábbi minta.

| **A paraméter**                | **Érték**                    |
| ---                          | ---                          |
| VNet-címelőtagjait        | 10.11.0.0/16<br>10.12.0.0/16 |
| Az Azure VPN gateway IP-címet         | Azure_Gateway_Public_IP      |
| A helyszíni címelőtagokat | 10.51.0.0/16<br>10.52.0.0/16 |
| A helyszíni VPN-eszköz IP-címet    | OnPrem_Device_Public_IP     |
| * VNet BGP ASN                | 65010                        |
| * Az azure BGP-Társgép IP-           | 10.12.255.30                 |
| * A helyszíni BGP ASN         | 65050                        |
| * A helyszíni BGP-Társgép IP-     | 10.52.255.254                |
|                              |                              |

* (*) Választható paraméterek: a BGP csak.

### <a name="ipsecike-policy--parameters"></a>IPsec-/ h.rend & paraméterek

Az alábbi táblázat az IPsec vagy internetes KULCSCSERE-algoritmusok és a mintában használt paraméterek. Győződjön meg arról, hogy a fent felsorolt összes algoritmusok a VPN-eszköz modellek és belsővezérlőprogram-verziók által támogatott VPN műszaki további részleteket.

| **IPsec/IKEv2**  | **Érték**                            |
| ---              | ---                                  |
| IKEv2-titkosítás | AES256                               |
| IKEv2-integritás  | SHA384                               |
| DH-csoport         | DHGroup24                            |
| IPsec-titkosítás | AES256                               |
| IPsec-integritás  | SHA1                                 |
| PFS-csoport        | PFS24                                |
| Gyorsmódú biztonsági társítás élettartama   | 7200 másodperc                         |
| Forgalomválasztó | UsePolicyBasedTrafficSelectors $True |
| Előre megosztott kulcs   | PreSharedKey                         |
|                  |                                      |

- (*) Néhány eszközön IPSec-integritásának "null", ha a GCM-AES IPsec titkosítási algoritmusként használt kell lennie.

### <a name="device-notes"></a>Eszköz megjegyzések

>[!NOTE]
>
> 1. IKEv2 támogatás szükséges ASA 8.4 verzió vagy újabb verzió.
> 2. (Túl csoport 5) nagyobb DH és a PFS csoport támogatási ASA verziója szükséges 9.x.
> 3. IPsec-titkosítást az SHA-256, SHA-384-et, SHA-512 támogatási AES-GCM- és IPSec-integritási ASA verziója szükséges 9.x újabb ASA hardverre; 5505 ASA, 5510, 5520, 5540, 5550, amelyek 5580 **nem** támogatott. (Ellenőrizze a szállító specifikációk megerősítéséhez.)
>


### <a name="sample-device-configurations"></a>A minta eszköz-konfigurációk
Az alábbi parancsfájl egy minta-konfigurációt a topológia és a fenti paraméterek alapján biztosít. Az S2S VPN-alagút konfiguráció a következő részből áll:

1. Felületek & útvonalak
2. Hozzáférés-listák
3. IKE-házirend és a paraméterek (1. fázis vagy alapmódú)
4. IPSec-házirend és a paraméterek (2. szakasza vagy gyorsmódú)
5. Más paramétereket (TCP MSS hántoló, stb.)

>[!IMPORTANT] 
>Győződjön meg arról, hogy az alább felsorolt, a további konfigurálás elvégzésére, és cserélje le a helyőrzőket a tényleges értékek:
> 
> - Kapcsolat az illesztők kívül és belül is konfigurációja
> - A vállalaton belüli és személyes és a külső/nyilvános hálózatokhoz útvonalak
> - Biztosítsa, hogy az összes nevét és házirend számok egyedi az eszközön
> - Győződjön meg arról, a titkosítási algoritmusok támogatottak az eszközön
> - Cserélje le a következő helyen tartozó felhasználók számára a tényleges értékek
>   - A kapcsolat nevét kívül: "külső"
>   - Azure_Gateway_Public_IP
>   - OnPrem_Device_Public_IP
>   - IKE Pre_Shared_Key
>   - Virtuális hálózat és a helyi hálózati átjáró nevek (VNetName, LNGName)
>   - Virtuális hálózat és a helyszíni hálózati címelőtagokat
>   - Megfelelő netmasks

#### <a name="sample-configuration"></a>Mintakonfiguráció

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Egyszerű hibakeresési parancsok

Az alábbiakban néhány ASA parancsok hibakeresési célra:

1. Az IPsec és az internetes KULCSCSERE SA megjelenítése
    - "megjelenítése titkosítási ipsec biztonsági társítás"
    - "titkosítási megjelenítése ikev2 rendszergazdai (SA)"
2. Belépés hibakeresési mód – ez kaphat nagyon zajos a konzolon
    - "hibakeresési titkosítási ikev2 platform <level>"
    - "titkosítási ikev2 protokoll debug <level>"
3. Lista aktuális konfigurációk
    - "futtatása show" – aktuális konfigurációk láthatók az eszközön; a konfigurációs listát meghatározott részeit a különféle alárendelt parancsok is használhatja. Például "futtatása megjelenítése titkosítási", "Futtatás megjelenítése access-list", "Futtatás megjelenítése alagút-csoport" stb.


## <a name="next-steps"></a>Következő lépések
A létesítmények közötti és a virtuális hálózatok közötti aktív-aktív kapcsolatok konfigurálásának lépéseit lásd: [Aktív-aktív VPN Gateway-átjárók konfigurálása létesítmények közötti és virtuális hálózatok közötti kapcsolatokhoz](vpn-gateway-activeactive-rm-powershell.md).


---
title: "aaaSample konfigurációs - tooAzure VPN-átjárók kapcsolódó Cisco ASA eszközhöz |} Microsoft Docs"
description: "Ez a cikk a Cisco ASA eszköznek tooAzure VPN-átjárók minta konfigurációját tartalmazza."
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
ms.openlocfilehash: dad13e02afe8dad2379db750eb09602e08e8ea99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Mintakonfiguráció: Cisco ASA (IKEv2/nem BGP-) eszköz
Ez a cikk ismerteti a minta konfigurációk tooAzure VPN-átjárók csatlakozó Cisco ASA eszközök.

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
> 1. az alábbi hello konfigurációs csatlakozik egy Cisco ASA eszköz tooan Azure **útválasztó-alapú** házirenddel egyéni IPsec/IKE "UserPolicyBasedTrafficSelectors" beállítást, a VPN-átjáró [Ez a cikk](vpn-gateway-connect-multiple-policybased-rm-ps.md).
> 2. Szükségel ASA eszközök toouse **IKEv2** a hozzáférési lista-konfigurációkat, nem VTI-alapú.
> 3. Tekintse át a a VPN-eszköz szállító specifikációnak tooensure hello házirend a helyszíni VPN-eszközök esetén támogatott.

## <a name="vpn-device-requirements"></a>VPN-eszköz követelményei
Az Azure VPN gatewayek szabványos IPsec/IKE protokoll csomagok tooestablish S2S VPN-alagutat használja. Tekintse meg a túl[kapcsolatos VPN-eszközök](vpn-gateway-about-vpn-devices.md) hello részletes IPsec/IKE protokoll paraméterek és az alapértelmezett titkosítási algoritmusok az Azure VPN gatewayek. Opcionálisan megadhat titkosítási algoritmusok és egy adott kapcsolat kulcs szintjeiről pontos kombinációja hello a leírtak szerint [titkosítási követelményekkel kapcsolatos](vpn-gateway-about-compliance-crypto.md). Ha olyan titkosítási algoritmusok és a kulcs szintjeiről egyedi kombinációja, ellenőrizze, hogy használja a VPN-eszközök hello vonatkozó előírásokat.

## <a name="single-vpn-tunnel"></a>Egy VPN-alagút
Ez a topológia áll az Azure VPN gateway és a helyszíni VPN-eszköz között egyetlen S2S VPN-alagúton. Konfigurálhatja BGP hello VPN-alagúton keresztül.

![az egyalagutas](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Tekintse meg a túl[egyalagutas telepítő](vpn-gateway-3rdparty-device-config-overview.md#singletunnel) részletes, lépésenkénti toobuild hello Azure-konfigurációjának.

### <a name="network-and-vpn-gateway-information"></a>Hálózati és a VPN-átjáró adatai
Ez a szakasz felsorolása hello hello paramétereinek ezt a mintát.

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

hello az alábbi táblázat hello IPsec/internetes KULCSCSERE-algoritmusok és hello mintában használt paraméterek. Tekintse meg a VPN eszköz specifikációk toomake meg arról, hogy a fent felsorolt összes algoritmusok a VPN-eszköz modellek és belsővezérlőprogram-verziók által támogatott.

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
> 3. IPsec-titkosítást az SHA-256, SHA-384-et, SHA-512 támogatási AES-GCM- és IPSec-integritási ASA verziója szükséges 9.x újabb ASA hardverre; 5505 ASA, 5510, 5520, 5540, 5550, amelyek 5580 **nem** támogatott. (Ellenőrizze hello szállító specifikációk tooconfirm.)
>


### <a name="sample-device-configurations"></a>A minta eszköz-konfigurációk
az alábbi parancsfájl hello hello topológia alapján minta konfigurációt és a fent felsorolt paramétereket biztosít. hello S2S VPN-alagút konfiguráció hello a következő részből áll:

1. Felületek & útvonalak
2. Hozzáférés-listák
3. IKE-házirend és a paraméterek (1. fázis vagy alapmódú)
4. IPSec-házirend és a paraméterek (2. szakasza vagy gyorsmódú)
5. Más paramétereket (TCP MSS hántoló, stb.)

>[!IMPORTANT] 
>Ellenőrizze, hogy végezze el az alább felsorolt hello további konfigurációs és hello helyőrzőket cserélje le a tényleges értékek hello:
> 
> - Kapcsolat az illesztők kívül és belül is konfigurációja
> - A vállalaton belüli és személyes és a külső/nyilvános hálózatokhoz útvonalak
> - Biztosítsa, hogy az összes nevét és házirend számok egyedi hello eszközön
> - Győződjön meg arról, titkosítási algoritmusok hello támogatottak az eszközön
> - Cserélje le a következő helyen tulajdonosainak hello tényleges értékeket hello
>   - A kapcsolat nevét kívül: "külső"
>   - Azure_Gateway_Public_IP
>   - OnPrem_Device_Public_IP
>   - IKE Pre_Shared_Key
>   - Virtuális hálózat és a helyi hálózati átjáró nevek (VNetName, LNGName)
>   - Virtuális hálózat és a helyszíni hálózati címelőtagokat
>   - Megfelelő netmasks

#### <a name="sample-configuration"></a>Mintakonfiguráció

```
! Sample ASA configuration for connecting tooAzure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace hello following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - hello Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with hello actual nexthop IP address
!
! (*) Must be unique names in hello device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on hello outside interface or vlan
!     > <PrivateIPAddress> on hello inside interface or vlan; e.g., 10.51.0.1/24
!     > Route tooconnect too<Azure_Gateway_Public_IP> address
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
!       (1) Allow S2S VPN tunnels between hello ASA and hello Azure gateway public IP address
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
!     > Object group that corresponding toohello <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines hello on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify hello access-list between hello Azure VNet and your on-premises network.
!       This access list defines hello IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between hello on-premises network and Azure VNet
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
!       - Make sure hello policy number is not used
!       - integrity and prf must be hello same
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
!       - This sample uses "Azure-<VNetName>-map" as hello crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned tooyour outside interface, you must use
!         hello same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses hello access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses hello proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS too1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Egyszerű hibakeresési parancsok

Az alábbiakban néhány ASA parancsok hibakeresési célra:

1. Megjelenítése IPsec és az internetes KULCSCSERE SA hello
    - "megjelenítése titkosítási ipsec biztonsági társítás"
    - "titkosítási megjelenítése ikev2 rendszergazdai (SA)"
2. Belépés hibakeresési mód – hello konzolon ez kérheti le nagyon zajos
    - "hibakeresési titkosítási ikev2 platform <level>"
    - "titkosítási ikev2 protokoll debug <level>"
3. Lista aktuális konfigurációk
    - "a megjelenítése futtatási" - látható hello aktuális konfigurációk hello eszközön; használható különböző alárendelt parancsok toolist meghatározott részeit hello konfigurációs hello. Például "futtatása megjelenítése titkosítási", "Futtatás megjelenítése access-list", "Futtatás megjelenítése alagút-csoport" stb.


## <a name="next-steps"></a>Következő lépések
Lásd: [aktív-aktív VPN-átjárók konfigurálása a létesítmények közötti és VNet – VNet kapcsolatokhoz](vpn-gateway-activeactive-rm-powershell.md) lépéseit tooconfigure aktív-aktív létesítmények közötti és VNet – VNet kapcsolatokhoz.


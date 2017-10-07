---
title: "aaaExpressRoute ügyfél útválasztó konfigurációs minták |} Microsoft Docs"
description: "Ezen a lapon útválasztó config minták biztosít a Cisco és a Juniper útválasztó."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: 564826bc-017a-4683-a385-37c9fa814948
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: 5c91f24e6082e01c3e8df91b4fcfda46a6c29fa8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-routing"></a>Útválasztó-konfigurálási tooset minták fel, és útválasztási kezelése
Ezen a lapon Cisco IOS-XE és a Juniper MX adatsorozat útválasztók felületet és útválasztási konfigurációs minták biztosít. Ezek tervezett toobe minták csak tájékoztatásra szolgálnak, és nem kell használni, mert a. Dolgozhat a szállító toocome a megfelelő konfigurációival a hálózathoz. 

> [!IMPORTANT]
> Ezen a lapon minta értéke tervezett toobe tisztán vonatkozó útmutatást. Kérje a gyártója által biztosított értékesítés / technikai csapat és a hálózati csoport toocome be a megfelelő konfigurációk toomeet igényeinek. A Microsoft nem fogja támogatni a problémák kapcsolódó tooconfigurations ezen a lapon szerepel. Támogatási kérdéseivel forduljon az eszköz gyártója.
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a>Útválasztó-kapcsolatokon MTU-beállítása és TCP MSS beállításai
* hello ExpressRoute kapcsolat MTU-beállítása hello érték 1500, amely hello alapértelmezett MTU-beállítása az Ethernet-adapter útválasztó. Az útválasztó alapértelmezés szerint van egy másik MTU-beállítása, hacsak nincs nincs szükség toospecify értéket hello útválasztó illesztő.
* Az Azure VPN Gateway eltérően hello TCP MSS ExpressRoute-kör nem kell megadott toobe.

Útválasztó-konfiguráció minták az alábbi tooall esetében érvényesek. Felülvizsgálati [ExpressRoute-társviszony](expressroute-circuit-peerings.md) és [ExpressRoute útválasztási követelmények](expressroute-routing.md) útválasztási olvashat.


## <a name="cisco-ios-xe-based-routers"></a>Cisco IOS-XE-alapú útválasztók
Ebben a szakaszban hello minták bármely útválasztóját hello IOS-XE operációsrendszer-család érvényes.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. És alárendelt kapcsolatain konfigurálása
Szüksége lesz egy sub felületet / társviszony-létesítés minden útválasztót, csatlakozás tooMicrosoft. Egy sub felületet azonosítható a VLAN-azonosító vagy halmozott két virtuális helyi hálózati azonosítókat és az IP-címet.

**Dot1Q felületdefiníció**

Ez a minta nyújt hello alárendelt felületdefiníció egy alárendelt felület az egyetlen VLAN-azonosítót. hello VLAN-azonosító minden társviszony-létesítés egyedi. az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

**QinQ felületdefiníció**

Ez a minta alárendelt felületdefiníció hello két VLAN-azonosítók egy alárendelt felületet biztosít. külső VLAN-azonosító (s-címke), ha a használt marad hello hello azonos közötti összes hello esetében. hello belső a VLAN-azonosító (c-címke) társviszony-létesítés minden egyedi. az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a>2. EBGP munkamenetek beállítása
Be kell állítania egy BGP-munkamenetet a Microsoft az a minden társviszony-létesítéshez. hello minta az alábbi lehetővé teszi a BGP-munkamenetet a Microsoft toosetup. Ha a sub felület használt IPv4-cím hello a.b.c.d volt, akkor hello IP-cím hello BGP szomszéd (Microsoft) a.b.c.d+1 lesz. utolsó oktettje hello hello BGP szomszéd IPv4-cím mindig lesz páros szám.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a>3. Hello BGP keresztüli meghirdetett előtagok toobe beállítása
Az útválasztó tooadvertise válassza előtagok tooMicrosoft konfigurálhatja. Így az alábbi példa használatával hello teheti meg.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a>4. Útvonal-leképezések
Útvonal-leképezések is használhat, és előtag a hálózaton történő propagálása toofilter előtagok sorolja fel. Hello mintát tooaccomplish hello feladat alatt is használhatja. Győződjön meg arról, hogy rendelkezik a megfelelő előtaggal listák beállítása.

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
      neighbor <IP#2_used_by_Azure> route-map <MS_Prefixes_Inbound> in
     exit-address-family
    !
    route-map <MS_Prefixes_Inbound> permit 10
     match ip address prefix-list <MS_Prefixes>
    !


## <a name="juniper-mx-series-routers"></a>Juniper MX adatsorozat útválasztók
Ebben a szakaszban hello minták Juniper MX adatsorozat útválasztókkal érvényes.

### <a name="1-configuring-interfaces-and-sub-interfaces"></a>1. És alárendelt kapcsolatain konfigurálása

**Dot1Q felületdefiníció**

Ez a minta nyújt hello alárendelt felületdefiníció egy alárendelt felület az egyetlen VLAN-azonosítót. hello VLAN-azonosító minden társviszony-létesítés egyedi. az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.

    interfaces {
        vlan-tagging;
        <Interface_Number> {
            unit <Number> {
                vlan-id <VLAN_ID>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }
            }
        }
    }


**QinQ felületdefiníció**

Ez a minta alárendelt felületdefiníció hello két VLAN-azonosítók egy alárendelt felületet biztosít. külső VLAN-azonosító (s-címke), ha a használt marad hello hello azonos közötti összes hello esetében. hello belső a VLAN-azonosító (c-címke) társviszony-létesítés minden egyedi. az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.

    interfaces {
        <Interface_Number> {
            flexible-vlan-tagging;
            unit <Number> {
                vlan-tags outer <S-tag> inner <C-tag>;
                family inet {
                    address <IPv4_Address/Subnet_Mask>;
                }                           
            }                               
        }                                   
    }                           

### <a name="2-setting-up-ebgp-sessions"></a>2. EBGP munkamenetek beállítása
Be kell állítania egy BGP-munkamenetet a Microsoft az a minden társviszony-létesítéshez. hello minta az alábbi lehetővé teszi a BGP-munkamenetet a Microsoft toosetup. Ha a sub felület használt IPv4-cím hello a.b.c.d volt, akkor hello IP-cím hello BGP szomszéd (Microsoft) a.b.c.d+1 lesz. utolsó oktettje hello hello BGP szomszéd IPv4-cím mindig lesz páros szám.

    routing-options {
        autonomous-system <Customer_ASN>;
    }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a>3. Hello BGP keresztüli meghirdetett előtagok toobe beállítása
Az útválasztó tooadvertise válassza előtagok tooMicrosoft konfigurálhatja. Így az alábbi példa használatával hello teheti meg.

    policy-options {
        policy-statement <Policy_Name> {
            term 1 {
                from protocol OSPF;
        route-filter <Prefix_to_be_advertised/Subnet_Mask> exact;
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }


### <a name="4-route-maps"></a>4. Útvonal-leképezések
Útvonal-leképezések is használhat, és előtag a hálózaton történő propagálása toofilter előtagok sorolja fel. Hello mintát tooaccomplish hello feladat alatt is használhatja. Győződjön meg arról, hogy rendelkezik a megfelelő előtaggal listák beállítása.

    policy-options {
        prefix-list MS_Prefixes {
            <IP_Prefix_1/Subnet_Mask>;
            <IP_Prefix_2/Subnet_Mask>;
        }
        policy-statement <MS_Prefixes_Inbound> {
            term 1 {
                from {
        prefix-list MS_Prefixes;
                }
                then {
                    accept;
                }
            }
        }
    }
    protocols {
        bgp { 
            group <Group_Name> { 
                export <Policy_Name>
                import <MS_Prefixes_Inbound>
                peer-as 12076;              
                neighbor <IP#2_used_by_Azure>;
            }                               
        }                                   
    }

## <a name="next-steps"></a>Következő lépések
Lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) további részleteket.


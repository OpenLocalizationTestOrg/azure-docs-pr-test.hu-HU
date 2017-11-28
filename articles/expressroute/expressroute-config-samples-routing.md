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
# <a name="router-configuration-samples-tooset-up-and-manage-routing"></a><span data-ttu-id="3732a-103">Útválasztó-konfigurálási tooset minták fel, és útválasztási kezelése</span><span class="sxs-lookup"><span data-stu-id="3732a-103">Router configuration samples tooset up and manage routing</span></span>
<span data-ttu-id="3732a-104">Ezen a lapon Cisco IOS-XE és a Juniper MX adatsorozat útválasztók felületet és útválasztási konfigurációs minták biztosít.</span><span class="sxs-lookup"><span data-stu-id="3732a-104">This page provides interface and routing configuration samples for Cisco IOS-XE and Juniper MX series routers.</span></span> <span data-ttu-id="3732a-105">Ezek tervezett toobe minták csak tájékoztatásra szolgálnak, és nem kell használni, mert a.</span><span class="sxs-lookup"><span data-stu-id="3732a-105">These are intended toobe samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="3732a-106">Dolgozhat a szállító toocome a megfelelő konfigurációival a hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="3732a-106">You can work with your vendor toocome up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="3732a-107">Ezen a lapon minta értéke tervezett toobe tisztán vonatkozó útmutatást.</span><span class="sxs-lookup"><span data-stu-id="3732a-107">Samples in this page are intended toobe purely for guidance.</span></span> <span data-ttu-id="3732a-108">Kérje a gyártója által biztosított értékesítés / technikai csapat és a hálózati csoport toocome be a megfelelő konfigurációk toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="3732a-108">You must work with your vendor's sales / technical team and your networking team toocome up with appropriate configurations toomeet your needs.</span></span> <span data-ttu-id="3732a-109">A Microsoft nem fogja támogatni a problémák kapcsolódó tooconfigurations ezen a lapon szerepel.</span><span class="sxs-lookup"><span data-stu-id="3732a-109">Microsoft will not support issues related tooconfigurations listed in this page.</span></span> <span data-ttu-id="3732a-110">Támogatási kérdéseivel forduljon az eszköz gyártója.</span><span class="sxs-lookup"><span data-stu-id="3732a-110">You must contact your device vendor for support issues.</span></span>
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a><span data-ttu-id="3732a-111">Útválasztó-kapcsolatokon MTU-beállítása és TCP MSS beállításai</span><span class="sxs-lookup"><span data-stu-id="3732a-111">MTU and TCP MSS settings on router interfaces</span></span>
* <span data-ttu-id="3732a-112">hello ExpressRoute kapcsolat MTU-beállítása hello érték 1500, amely hello alapértelmezett MTU-beállítása az Ethernet-adapter útválasztó.</span><span class="sxs-lookup"><span data-stu-id="3732a-112">hello MTU for hello ExpressRoute interface is 1500, which is hello typical default MTU for an Ethernet interface on a router.</span></span> <span data-ttu-id="3732a-113">Az útválasztó alapértelmezés szerint van egy másik MTU-beállítása, hacsak nincs nincs szükség toospecify értéket hello útválasztó illesztő.</span><span class="sxs-lookup"><span data-stu-id="3732a-113">Unless your router has a different MTU by default, there is no need toospecify a value on hello router interface.</span></span>
* <span data-ttu-id="3732a-114">Az Azure VPN Gateway eltérően hello TCP MSS ExpressRoute-kör nem kell megadott toobe.</span><span class="sxs-lookup"><span data-stu-id="3732a-114">Unlike an Azure VPN Gateway, hello TCP MSS for an ExpressRoute circuit does not need toobe specified.</span></span>

<span data-ttu-id="3732a-115">Útválasztó-konfiguráció minták az alábbi tooall esetében érvényesek.</span><span class="sxs-lookup"><span data-stu-id="3732a-115">Router configuration samples below apply tooall peerings.</span></span> <span data-ttu-id="3732a-116">Felülvizsgálati [ExpressRoute-társviszony](expressroute-circuit-peerings.md) és [ExpressRoute útválasztási követelmények](expressroute-routing.md) útválasztási olvashat.</span><span class="sxs-lookup"><span data-stu-id="3732a-116">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute routing requirements](expressroute-routing.md) for more details on routing.</span></span>


## <a name="cisco-ios-xe-based-routers"></a><span data-ttu-id="3732a-117">Cisco IOS-XE-alapú útválasztók</span><span class="sxs-lookup"><span data-stu-id="3732a-117">Cisco IOS-XE based routers</span></span>
<span data-ttu-id="3732a-118">Ebben a szakaszban hello minták bármely útválasztóját hello IOS-XE operációsrendszer-család érvényes.</span><span class="sxs-lookup"><span data-stu-id="3732a-118">hello samples in this section apply for any router running hello IOS-XE OS family.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="3732a-119">1. És alárendelt kapcsolatain konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3732a-119">1. Configuring interfaces and sub-interfaces</span></span>
<span data-ttu-id="3732a-120">Szüksége lesz egy sub felületet / társviszony-létesítés minden útválasztót, csatlakozás tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="3732a-120">You will require a sub interface per peering in every router you connect tooMicrosoft.</span></span> <span data-ttu-id="3732a-121">Egy sub felületet azonosítható a VLAN-azonosító vagy halmozott két virtuális helyi hálózati azonosítókat és az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="3732a-121">A sub interface can be identified with a VLAN ID or a stacked pair of VLAN IDs and an IP address.</span></span>

<span data-ttu-id="3732a-122">**Dot1Q felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="3732a-122">**Dot1Q interface definition**</span></span>

<span data-ttu-id="3732a-123">Ez a minta nyújt hello alárendelt felületdefiníció egy alárendelt felület az egyetlen VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3732a-123">This sample provides hello sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="3732a-124">hello VLAN-azonosító minden társviszony-létesítés egyedi.</span><span class="sxs-lookup"><span data-stu-id="3732a-124">hello VLAN ID is unique per peering.</span></span> <span data-ttu-id="3732a-125">az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="3732a-125">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

<span data-ttu-id="3732a-126">**QinQ felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="3732a-126">**QinQ interface definition**</span></span>

<span data-ttu-id="3732a-127">Ez a minta alárendelt felületdefiníció hello két VLAN-azonosítók egy alárendelt felületet biztosít.</span><span class="sxs-lookup"><span data-stu-id="3732a-127">This sample provides hello sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="3732a-128">külső VLAN-azonosító (s-címke), ha a használt marad hello hello azonos közötti összes hello esetében.</span><span class="sxs-lookup"><span data-stu-id="3732a-128">hello outer VLAN ID (s-tag), if used remains hello same across all hello peerings.</span></span> <span data-ttu-id="3732a-129">hello belső a VLAN-azonosító (c-címke) társviszony-létesítés minden egyedi.</span><span class="sxs-lookup"><span data-stu-id="3732a-129">hello inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="3732a-130">az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="3732a-130">hello last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="3732a-131">2. EBGP munkamenetek beállítása</span><span class="sxs-lookup"><span data-stu-id="3732a-131">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="3732a-132">Be kell állítania egy BGP-munkamenetet a Microsoft az a minden társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="3732a-132">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="3732a-133">hello minta az alábbi lehetővé teszi a BGP-munkamenetet a Microsoft toosetup.</span><span class="sxs-lookup"><span data-stu-id="3732a-133">hello sample below enables you toosetup a BGP session with Microsoft.</span></span> <span data-ttu-id="3732a-134">Ha a sub felület használt IPv4-cím hello a.b.c.d volt, akkor hello IP-cím hello BGP szomszéd (Microsoft) a.b.c.d+1 lesz.</span><span class="sxs-lookup"><span data-stu-id="3732a-134">If hello IPv4 address you used for your sub interface was a.b.c.d, hello IP address of hello BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="3732a-135">utolsó oktettje hello hello BGP szomszéd IPv4-cím mindig lesz páros szám.</span><span class="sxs-lookup"><span data-stu-id="3732a-135">hello last octet of hello BGP neighbor's IPv4 address will always be an even number.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a><span data-ttu-id="3732a-136">3. Hello BGP keresztüli meghirdetett előtagok toobe beállítása</span><span class="sxs-lookup"><span data-stu-id="3732a-136">3. Setting up prefixes toobe advertised over hello BGP session</span></span>
<span data-ttu-id="3732a-137">Az útválasztó tooadvertise válassza előtagok tooMicrosoft konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="3732a-137">You can configure your router tooadvertise select prefixes tooMicrosoft.</span></span> <span data-ttu-id="3732a-138">Így az alábbi példa használatával hello teheti meg.</span><span class="sxs-lookup"><span data-stu-id="3732a-138">You can do so using hello sample below.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a><span data-ttu-id="3732a-139">4. Útvonal-leképezések</span><span class="sxs-lookup"><span data-stu-id="3732a-139">4. Route maps</span></span>
<span data-ttu-id="3732a-140">Útvonal-leképezések is használhat, és előtag a hálózaton történő propagálása toofilter előtagok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="3732a-140">You can use route-maps and prefix lists toofilter prefixes propagated into your network.</span></span> <span data-ttu-id="3732a-141">Hello mintát tooaccomplish hello feladat alatt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3732a-141">You can use hello sample below tooaccomplish hello task.</span></span> <span data-ttu-id="3732a-142">Győződjön meg arról, hogy rendelkezik a megfelelő előtaggal listák beállítása.</span><span class="sxs-lookup"><span data-stu-id="3732a-142">Ensure that you have appropriate prefix lists setup.</span></span>

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


## <a name="juniper-mx-series-routers"></a><span data-ttu-id="3732a-143">Juniper MX adatsorozat útválasztók</span><span class="sxs-lookup"><span data-stu-id="3732a-143">Juniper MX series routers</span></span>
<span data-ttu-id="3732a-144">Ebben a szakaszban hello minták Juniper MX adatsorozat útválasztókkal érvényes.</span><span class="sxs-lookup"><span data-stu-id="3732a-144">hello samples in this section apply for any Juniper MX series routers.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="3732a-145">1. És alárendelt kapcsolatain konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3732a-145">1. Configuring interfaces and sub-interfaces</span></span>

<span data-ttu-id="3732a-146">**Dot1Q felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="3732a-146">**Dot1Q interface definition**</span></span>

<span data-ttu-id="3732a-147">Ez a minta nyújt hello alárendelt felületdefiníció egy alárendelt felület az egyetlen VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="3732a-147">This sample provides hello sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="3732a-148">hello VLAN-azonosító minden társviszony-létesítés egyedi.</span><span class="sxs-lookup"><span data-stu-id="3732a-148">hello VLAN ID is unique per peering.</span></span> <span data-ttu-id="3732a-149">az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="3732a-149">hello last octet of your IPv4 address will always be an odd number.</span></span>

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


<span data-ttu-id="3732a-150">**QinQ felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="3732a-150">**QinQ interface definition**</span></span>

<span data-ttu-id="3732a-151">Ez a minta alárendelt felületdefiníció hello két VLAN-azonosítók egy alárendelt felületet biztosít.</span><span class="sxs-lookup"><span data-stu-id="3732a-151">This sample provides hello sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="3732a-152">külső VLAN-azonosító (s-címke), ha a használt marad hello hello azonos közötti összes hello esetében.</span><span class="sxs-lookup"><span data-stu-id="3732a-152">hello outer VLAN ID (s-tag), if used remains hello same across all hello peerings.</span></span> <span data-ttu-id="3732a-153">hello belső a VLAN-azonosító (c-címke) társviszony-létesítés minden egyedi.</span><span class="sxs-lookup"><span data-stu-id="3732a-153">hello inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="3732a-154">az IPv4-cím utolsó oktettje hello mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="3732a-154">hello last octet of your IPv4 address will always be an odd number.</span></span>

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

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="3732a-155">2. EBGP munkamenetek beállítása</span><span class="sxs-lookup"><span data-stu-id="3732a-155">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="3732a-156">Be kell állítania egy BGP-munkamenetet a Microsoft az a minden társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="3732a-156">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="3732a-157">hello minta az alábbi lehetővé teszi a BGP-munkamenetet a Microsoft toosetup.</span><span class="sxs-lookup"><span data-stu-id="3732a-157">hello sample below enables you toosetup a BGP session with Microsoft.</span></span> <span data-ttu-id="3732a-158">Ha a sub felület használt IPv4-cím hello a.b.c.d volt, akkor hello IP-cím hello BGP szomszéd (Microsoft) a.b.c.d+1 lesz.</span><span class="sxs-lookup"><span data-stu-id="3732a-158">If hello IPv4 address you used for your sub interface was a.b.c.d, hello IP address of hello BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="3732a-159">utolsó oktettje hello hello BGP szomszéd IPv4-cím mindig lesz páros szám.</span><span class="sxs-lookup"><span data-stu-id="3732a-159">hello last octet of hello BGP neighbor's IPv4 address will always be an even number.</span></span>

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

### <a name="3-setting-up-prefixes-toobe-advertised-over-hello-bgp-session"></a><span data-ttu-id="3732a-160">3. Hello BGP keresztüli meghirdetett előtagok toobe beállítása</span><span class="sxs-lookup"><span data-stu-id="3732a-160">3. Setting up prefixes toobe advertised over hello BGP session</span></span>
<span data-ttu-id="3732a-161">Az útválasztó tooadvertise válassza előtagok tooMicrosoft konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="3732a-161">You can configure your router tooadvertise select prefixes tooMicrosoft.</span></span> <span data-ttu-id="3732a-162">Így az alábbi példa használatával hello teheti meg.</span><span class="sxs-lookup"><span data-stu-id="3732a-162">You can do so using hello sample below.</span></span>

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


### <a name="4-route-maps"></a><span data-ttu-id="3732a-163">4. Útvonal-leképezések</span><span class="sxs-lookup"><span data-stu-id="3732a-163">4. Route maps</span></span>
<span data-ttu-id="3732a-164">Útvonal-leképezések is használhat, és előtag a hálózaton történő propagálása toofilter előtagok sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="3732a-164">You can use route-maps and prefix lists toofilter prefixes propagated into your network.</span></span> <span data-ttu-id="3732a-165">Hello mintát tooaccomplish hello feladat alatt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3732a-165">You can use hello sample below tooaccomplish hello task.</span></span> <span data-ttu-id="3732a-166">Győződjön meg arról, hogy rendelkezik a megfelelő előtaggal listák beállítása.</span><span class="sxs-lookup"><span data-stu-id="3732a-166">Ensure that you have appropriate prefix lists setup.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="3732a-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3732a-167">Next Steps</span></span>
<span data-ttu-id="3732a-168">Lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="3732a-168">See hello [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>


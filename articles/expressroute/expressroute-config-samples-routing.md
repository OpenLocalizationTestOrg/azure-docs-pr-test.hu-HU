---
title: "ExpressRoute ügyfél útválasztó konfigurációs minták |} Microsoft Docs"
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
ms.openlocfilehash: 032e584dc5abf59e9e3e8d80673b402f1fbf721b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="router-configuration-samples-to-set-up-and-manage-routing"></a><span data-ttu-id="f6539-103">Útválasztó beállítása és kezelése az útválasztási konfigurációs minták</span><span class="sxs-lookup"><span data-stu-id="f6539-103">Router configuration samples to set up and manage routing</span></span>
<span data-ttu-id="f6539-104">Ezen a lapon Cisco IOS-XE és a Juniper MX adatsorozat útválasztók felületet és útválasztási konfigurációs minták biztosít.</span><span class="sxs-lookup"><span data-stu-id="f6539-104">This page provides interface and routing configuration samples for Cisco IOS-XE and Juniper MX series routers.</span></span> <span data-ttu-id="f6539-105">Ezek a minták csak útmutatót kell, és nem kell használni, mert a.</span><span class="sxs-lookup"><span data-stu-id="f6539-105">These are intended to be samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="f6539-106">A gyártó, így kapja meg a hálózat megfelelő konfigurációi dolgozhat.</span><span class="sxs-lookup"><span data-stu-id="f6539-106">You can work with your vendor to come up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="f6539-107">Ezen a lapon minták célja, hogy pusztán az útmutatást kell.</span><span class="sxs-lookup"><span data-stu-id="f6539-107">Samples in this page are intended to be purely for guidance.</span></span> <span data-ttu-id="f6539-108">A gyártója által biztosított értékesítés / műszaki adapterek és a hálózati így kapja meg az igényeinek megfelelő konfigurációt kell dolgozni.</span><span class="sxs-lookup"><span data-stu-id="f6539-108">You must work with your vendor's sales / technical team and your networking team to come up with appropriate configurations to meet your needs.</span></span> <span data-ttu-id="f6539-109">A Microsoft nem támogatja a beállításokat ezen a lapon szereplő kapcsolatos problémákat.</span><span class="sxs-lookup"><span data-stu-id="f6539-109">Microsoft will not support issues related to configurations listed in this page.</span></span> <span data-ttu-id="f6539-110">Támogatási kérdéseivel forduljon az eszköz gyártója.</span><span class="sxs-lookup"><span data-stu-id="f6539-110">You must contact your device vendor for support issues.</span></span>
> 
> 

## <a name="mtu-and-tcp-mss-settings-on-router-interfaces"></a><span data-ttu-id="f6539-111">Útválasztó-kapcsolatokon MTU-beállítása és TCP MSS beállításai</span><span class="sxs-lookup"><span data-stu-id="f6539-111">MTU and TCP MSS settings on router interfaces</span></span>
* <span data-ttu-id="f6539-112">A MTU-beállítása, a ExpressRoute kapcsolat 1500, a tipikus alapértelmezett MTU-beállítása az Ethernet-adapter egy útválasztón.</span><span class="sxs-lookup"><span data-stu-id="f6539-112">The MTU for the ExpressRoute interface is 1500, which is the typical default MTU for an Ethernet interface on a router.</span></span> <span data-ttu-id="f6539-113">Az útválasztó alapértelmezés szerint van egy másik MTU-beállítása, hacsak nincs szükség az érték határozza meg az útválasztó-illesztő.</span><span class="sxs-lookup"><span data-stu-id="f6539-113">Unless your router has a different MTU by default, there is no need to specify a value on the router interface.</span></span>
* <span data-ttu-id="f6539-114">Az Azure VPN Gateway eltérően a TCP MSS ExpressRoute-kör nem kell megadni.</span><span class="sxs-lookup"><span data-stu-id="f6539-114">Unlike an Azure VPN Gateway, the TCP MSS for an ExpressRoute circuit does not need to be specified.</span></span>

<span data-ttu-id="f6539-115">Útválasztó-konfiguráció minták az alábbi összes esetében érvényesek.</span><span class="sxs-lookup"><span data-stu-id="f6539-115">Router configuration samples below apply to all peerings.</span></span> <span data-ttu-id="f6539-116">Felülvizsgálati [ExpressRoute-társviszony](expressroute-circuit-peerings.md) és [ExpressRoute útválasztási követelmények](expressroute-routing.md) útválasztási olvashat.</span><span class="sxs-lookup"><span data-stu-id="f6539-116">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute routing requirements](expressroute-routing.md) for more details on routing.</span></span>


## <a name="cisco-ios-xe-based-routers"></a><span data-ttu-id="f6539-117">Cisco IOS-XE-alapú útválasztók</span><span class="sxs-lookup"><span data-stu-id="f6539-117">Cisco IOS-XE based routers</span></span>
<span data-ttu-id="f6539-118">Ebben a szakaszban a minták bármely útválasztóját az IOS-XE operációsrendszer-család érvényes.</span><span class="sxs-lookup"><span data-stu-id="f6539-118">The samples in this section apply for any router running the IOS-XE OS family.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="f6539-119">1. És alárendelt kapcsolatain konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6539-119">1. Configuring interfaces and sub-interfaces</span></span>
<span data-ttu-id="f6539-120">Szüksége lesz egy sub felületet / minden útválasztót csatlakozhat a Microsoft társviszony-létesítés.</span><span class="sxs-lookup"><span data-stu-id="f6539-120">You will require a sub interface per peering in every router you connect to Microsoft.</span></span> <span data-ttu-id="f6539-121">Egy sub felületet azonosítható a VLAN-azonosító vagy halmozott két virtuális helyi hálózati azonosítókat és az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="f6539-121">A sub interface can be identified with a VLAN ID or a stacked pair of VLAN IDs and an IP address.</span></span>

<span data-ttu-id="f6539-122">**Dot1Q felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="f6539-122">**Dot1Q interface definition**</span></span>

<span data-ttu-id="f6539-123">Ez a minta biztosítja a alárendelt felületdefiníció egy alárendelt felület az egyetlen VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="f6539-123">This sample provides the sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="f6539-124">A VLAN-azonosító minden társviszony-létesítés egyedi.</span><span class="sxs-lookup"><span data-stu-id="f6539-124">The VLAN ID is unique per peering.</span></span> <span data-ttu-id="f6539-125">Az IPv4-cím utolsó oktettje mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="f6539-125">The last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <VLAN_ID>
     ip address <IPv4_Address><Subnet_Mask>

<span data-ttu-id="f6539-126">**QinQ felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="f6539-126">**QinQ interface definition**</span></span>

<span data-ttu-id="f6539-127">Ez a minta biztosítja a alárendelt felületdefiníció egy alárendelt felület egy két virtuális helyi hálózati azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="f6539-127">This sample provides the sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="f6539-128">A külső VLAN-azonosító (s-címke), ha változatlan marad, a társviszony között.</span><span class="sxs-lookup"><span data-stu-id="f6539-128">The outer VLAN ID (s-tag), if used remains the same across all the peerings.</span></span> <span data-ttu-id="f6539-129">A belső VLAN-azonosító (c-címke) társviszony-létesítés minden egyedi.</span><span class="sxs-lookup"><span data-stu-id="f6539-129">The inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="f6539-130">Az IPv4-cím utolsó oktettje mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="f6539-130">The last octet of your IPv4 address will always be an odd number.</span></span>

    interface GigabitEthernet<Interface_Number>.<Number>
     encapsulation dot1Q <s-tag> seconddot1Q <c-tag>
     ip address <IPv4_Address><Subnet_Mask>

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="f6539-131">2. EBGP munkamenetek beállítása</span><span class="sxs-lookup"><span data-stu-id="f6539-131">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="f6539-132">Be kell állítania egy BGP-munkamenetet a Microsoft az a minden társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="f6539-132">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="f6539-133">Az alábbi minta egy BGP-munkamenetet a Microsoft telepítési teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="f6539-133">The sample below enables you to setup a BGP session with Microsoft.</span></span> <span data-ttu-id="f6539-134">Ha a sub felület használt IPv4-cím volt a.b.c.d, az IP-címet a BGP szomszéd (Microsoft) nem a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="f6539-134">If the IPv4 address you used for your sub interface was a.b.c.d, the IP address of the BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="f6539-135">A BGP szomszéd IPv4-cím utolsó oktettje mindig lesz páros szám.</span><span class="sxs-lookup"><span data-stu-id="f6539-135">The last octet of the BGP neighbor's IPv4 address will always be an even number.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
     neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a><span data-ttu-id="f6539-136">3. A BGP-munkameneten keresztül hirdetését előtagok beállítása</span><span class="sxs-lookup"><span data-stu-id="f6539-136">3. Setting up prefixes to be advertised over the BGP session</span></span>
<span data-ttu-id="f6539-137">Az útválasztót Microsoft válassza előtagok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f6539-137">You can configure your router to advertise select prefixes to Microsoft.</span></span> <span data-ttu-id="f6539-138">Ehhez használja az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="f6539-138">You can do so using the sample below.</span></span>

    router bgp <Customer_ASN>
     bgp log-neighbor-changes
     neighbor <IP#2_used_by_Azure> remote-as 12076
     !        
     address-family ipv4
      network <Prefix_to_be_advertised> mask <Subnet_mask>
      neighbor <IP#2_used_by_Azure> activate
     exit-address-family
    !

### <a name="4-route-maps"></a><span data-ttu-id="f6539-139">4. Útvonal-leképezések</span><span class="sxs-lookup"><span data-stu-id="f6539-139">4. Route maps</span></span>
<span data-ttu-id="f6539-140">Útvonal-leképezések is használhat, és előtag sorolja fel, a szűrő-előtagok a hálózaton történő propagálása.</span><span class="sxs-lookup"><span data-stu-id="f6539-140">You can use route-maps and prefix lists to filter prefixes propagated into your network.</span></span> <span data-ttu-id="f6539-141">Az alábbi minta segítségével a feladatnak.</span><span class="sxs-lookup"><span data-stu-id="f6539-141">You can use the sample below to accomplish the task.</span></span> <span data-ttu-id="f6539-142">Győződjön meg arról, hogy rendelkezik a megfelelő előtaggal listák beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6539-142">Ensure that you have appropriate prefix lists setup.</span></span>

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


## <a name="juniper-mx-series-routers"></a><span data-ttu-id="f6539-143">Juniper MX adatsorozat útválasztók</span><span class="sxs-lookup"><span data-stu-id="f6539-143">Juniper MX series routers</span></span>
<span data-ttu-id="f6539-144">Ebben a szakaszban a minták Juniper MX adatsorozat útválasztókkal érvényes.</span><span class="sxs-lookup"><span data-stu-id="f6539-144">The samples in this section apply for any Juniper MX series routers.</span></span>

### <a name="1-configuring-interfaces-and-sub-interfaces"></a><span data-ttu-id="f6539-145">1. És alárendelt kapcsolatain konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f6539-145">1. Configuring interfaces and sub-interfaces</span></span>

<span data-ttu-id="f6539-146">**Dot1Q felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="f6539-146">**Dot1Q interface definition**</span></span>

<span data-ttu-id="f6539-147">Ez a minta biztosítja a alárendelt felületdefiníció egy alárendelt felület az egyetlen VLAN-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="f6539-147">This sample provides the sub-interface definition for a sub-interface with a single VLAN ID.</span></span> <span data-ttu-id="f6539-148">A VLAN-azonosító minden társviszony-létesítés egyedi.</span><span class="sxs-lookup"><span data-stu-id="f6539-148">The VLAN ID is unique per peering.</span></span> <span data-ttu-id="f6539-149">Az IPv4-cím utolsó oktettje mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="f6539-149">The last octet of your IPv4 address will always be an odd number.</span></span>

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


<span data-ttu-id="f6539-150">**QinQ felületdefiníció**</span><span class="sxs-lookup"><span data-stu-id="f6539-150">**QinQ interface definition**</span></span>

<span data-ttu-id="f6539-151">Ez a minta biztosítja a alárendelt felületdefiníció egy alárendelt felület egy két virtuális helyi hálózati azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="f6539-151">This sample provides the sub-interface definition for a sub-interface with a two VLAN IDs.</span></span> <span data-ttu-id="f6539-152">A külső VLAN-azonosító (s-címke), ha változatlan marad, a társviszony között.</span><span class="sxs-lookup"><span data-stu-id="f6539-152">The outer VLAN ID (s-tag), if used remains the same across all the peerings.</span></span> <span data-ttu-id="f6539-153">A belső VLAN-azonosító (c-címke) társviszony-létesítés minden egyedi.</span><span class="sxs-lookup"><span data-stu-id="f6539-153">The inner VLAN ID (c-tag) is unique per peering.</span></span> <span data-ttu-id="f6539-154">Az IPv4-cím utolsó oktettje mindig lesz páratlan szám.</span><span class="sxs-lookup"><span data-stu-id="f6539-154">The last octet of your IPv4 address will always be an odd number.</span></span>

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

### <a name="2-setting-up-ebgp-sessions"></a><span data-ttu-id="f6539-155">2. EBGP munkamenetek beállítása</span><span class="sxs-lookup"><span data-stu-id="f6539-155">2. Setting up eBGP sessions</span></span>
<span data-ttu-id="f6539-156">Be kell állítania egy BGP-munkamenetet a Microsoft az a minden társviszony-létesítéshez.</span><span class="sxs-lookup"><span data-stu-id="f6539-156">You must setup a BGP session with Microsoft for every peering.</span></span> <span data-ttu-id="f6539-157">Az alábbi minta egy BGP-munkamenetet a Microsoft telepítési teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="f6539-157">The sample below enables you to setup a BGP session with Microsoft.</span></span> <span data-ttu-id="f6539-158">Ha a sub felület használt IPv4-cím volt a.b.c.d, az IP-címet a BGP szomszéd (Microsoft) nem a.b.c.d+1.</span><span class="sxs-lookup"><span data-stu-id="f6539-158">If the IPv4 address you used for your sub interface was a.b.c.d, the IP address of the BGP neighbor (Microsoft) will be a.b.c.d+1.</span></span> <span data-ttu-id="f6539-159">A BGP szomszéd IPv4-cím utolsó oktettje mindig lesz páros szám.</span><span class="sxs-lookup"><span data-stu-id="f6539-159">The last octet of the BGP neighbor's IPv4 address will always be an even number.</span></span>

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

### <a name="3-setting-up-prefixes-to-be-advertised-over-the-bgp-session"></a><span data-ttu-id="f6539-160">3. A BGP-munkameneten keresztül hirdetését előtagok beállítása</span><span class="sxs-lookup"><span data-stu-id="f6539-160">3. Setting up prefixes to be advertised over the BGP session</span></span>
<span data-ttu-id="f6539-161">Az útválasztót Microsoft válassza előtagok hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="f6539-161">You can configure your router to advertise select prefixes to Microsoft.</span></span> <span data-ttu-id="f6539-162">Ehhez használja az alábbi minta.</span><span class="sxs-lookup"><span data-stu-id="f6539-162">You can do so using the sample below.</span></span>

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


### <a name="4-route-maps"></a><span data-ttu-id="f6539-163">4. Útvonal-leképezések</span><span class="sxs-lookup"><span data-stu-id="f6539-163">4. Route maps</span></span>
<span data-ttu-id="f6539-164">Útvonal-leképezések is használhat, és előtag sorolja fel, a szűrő-előtagok a hálózaton történő propagálása.</span><span class="sxs-lookup"><span data-stu-id="f6539-164">You can use route-maps and prefix lists to filter prefixes propagated into your network.</span></span> <span data-ttu-id="f6539-165">Az alábbi minta segítségével a feladatnak.</span><span class="sxs-lookup"><span data-stu-id="f6539-165">You can use the sample below to accomplish the task.</span></span> <span data-ttu-id="f6539-166">Győződjön meg arról, hogy rendelkezik a megfelelő előtaggal listák beállítása.</span><span class="sxs-lookup"><span data-stu-id="f6539-166">Ensure that you have appropriate prefix lists setup.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="f6539-167">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f6539-167">Next Steps</span></span>
<span data-ttu-id="f6539-168">További részletek: [ExpressRoute FAQ](expressroute-faqs.md) (ExpressRoute – gyakori kérdések).</span><span class="sxs-lookup"><span data-stu-id="f6539-168">See the [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>


---
title: "aaaExpressRoute ügyfél útválasztó konfigurációs minták |} Microsoft Docs"
description: "Ezen a lapon útválasztó-konfiguráció minták biztosít a Cisco és a Juniper útválasztó."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: d6ea716f-d5ee-4a61-92b0-640d6e7d6974
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
ms.openlocfilehash: b5faca0666bda6173e54abb0b6560d5f8bf8bfc2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="router-configuration-samples-tooset-up-and-manage-nat"></a><span data-ttu-id="c1351-103">Útválasztó-konfigurálási tooset minták fel és kezelheti a NAT</span><span class="sxs-lookup"><span data-stu-id="c1351-103">Router configuration samples tooset up and manage NAT</span></span>
<span data-ttu-id="c1351-104">Ezen a lapon NAT konfigurációs minták biztosít a Cisco ASA és a Juniper SRX adatsorozat útválasztó.</span><span class="sxs-lookup"><span data-stu-id="c1351-104">This page provides NAT configuration samples for Cisco ASA and Juniper SRX series routers.</span></span> <span data-ttu-id="c1351-105">Ezek tervezett toobe minták csak tájékoztatásra szolgálnak, és nem kell használni, mert a.</span><span class="sxs-lookup"><span data-stu-id="c1351-105">These are intended toobe samples for guidance only and must not be used as is.</span></span> <span data-ttu-id="c1351-106">Dolgozhat a szállító toocome a megfelelő konfigurációival a hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="c1351-106">You can work with your vendor toocome up with appropriate configurations for your network.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c1351-107">Ezen a lapon minta értéke tervezett toobe tisztán vonatkozó útmutatást.</span><span class="sxs-lookup"><span data-stu-id="c1351-107">Samples in this page are intended toobe purely for guidance.</span></span> <span data-ttu-id="c1351-108">Kérje a gyártója által biztosított értékesítés / technikai csapat és a hálózati csoport toocome be a megfelelő konfigurációk toomeet igényeinek.</span><span class="sxs-lookup"><span data-stu-id="c1351-108">You must work with your vendor's sales / technical team and your networking team toocome up with appropriate configurations toomeet your needs.</span></span> <span data-ttu-id="c1351-109">A Microsoft nem fogja támogatni a problémák kapcsolódó tooconfigurations ezen a lapon szerepel.</span><span class="sxs-lookup"><span data-stu-id="c1351-109">Microsoft will not support issues related tooconfigurations listed in this page.</span></span> <span data-ttu-id="c1351-110">Támogatási kérdéseivel forduljon az eszköz gyártója.</span><span class="sxs-lookup"><span data-stu-id="c1351-110">You must contact your device vendor for support issues.</span></span>
> 
> 

* <span data-ttu-id="c1351-111">Útválasztó-konfiguráció minták az alábbi tooAzure nyilvános és a Microsoft társviszony alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="c1351-111">Router configuration samples below apply tooAzure Public and Microsoft peerings.</span></span> <span data-ttu-id="c1351-112">Az Azure magánhálózati társviszony-létesítés nem NAT kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="c1351-112">You must not configure NAT for Azure private peering.</span></span> <span data-ttu-id="c1351-113">Felülvizsgálati [ExpressRoute-társviszony](expressroute-circuit-peerings.md) és [ExpressRoute NAT követelmények](expressroute-nat.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="c1351-113">Review [ExpressRoute peerings](expressroute-circuit-peerings.md) and [ExpressRoute NAT requirements](expressroute-nat.md) for more details.</span></span>

* <span data-ttu-id="c1351-114">Hálózati Címfordítás IP-címkészleteket külön kell használnia az kapcsolat toohello internetes és az ExpressRoute.</span><span class="sxs-lookup"><span data-stu-id="c1351-114">You MUST use separate NAT IP pools for connectivity toohello internet and ExpressRoute.</span></span> <span data-ttu-id="c1351-115">Azonos hálózati Címfordítás IP-készlet között hello segítségével hello internet, és ExpressRoute aszimmetrikus az Útválasztás és a kapcsolat megszakadása eredményez.</span><span class="sxs-lookup"><span data-stu-id="c1351-115">Using hello same NAT IP pool across hello internet and ExpressRoute will result in asymmetric routing and loss of connectivity.</span></span>


## <a name="cisco-asa-firewalls"></a><span data-ttu-id="c1351-116">Cisco ASA tűzfal</span><span class="sxs-lookup"><span data-stu-id="c1351-116">Cisco ASA firewalls</span></span>
### <a name="pat-configuration-for-traffic-from-customer-network-toomicrosoft"></a><span data-ttu-id="c1351-117">Az ügyfél hálózati tooMicrosoft forgalom PAT konfigurációja</span><span class="sxs-lookup"><span data-stu-id="c1351-117">PAT configuration for traffic from customer network tooMicrosoft</span></span>
    object network MSFT-PAT
      range <SNAT-START-IP> <SNAT-END-IP>


    object-group network MSFT-Range
      network-object <IP> <Subnet_Mask>

    object-group network on-prem-range-1
      network-object <IP> <Subnet-Mask>

    object-group network on-prem-range-2
      network-object <IP> <Subnet-Mask>

    object-group network on-prem
      network-object object on-prem-range-1
      network-object object on-prem-range-2

    nat (outside,inside) source dynamic on-prem pat-pool MSFT-PAT destination static MSFT-Range MSFT-Range

### <a name="pat-configuration-for-traffic-from-microsoft-toocustomer-network"></a><span data-ttu-id="c1351-118">Microsoft toocustomer hálózati forgalmát PAT konfigurációja</span><span class="sxs-lookup"><span data-stu-id="c1351-118">PAT configuration for traffic from Microsoft toocustomer network</span></span>

<span data-ttu-id="c1351-119">**Felületek és iránya:**</span><span class="sxs-lookup"><span data-stu-id="c1351-119">**Interfaces and Direction:**</span></span>

    Source Interface (where hello traffic enters hello ASA): inside
    Destination Interface (where hello traffic exits hello ASA): outside

<span data-ttu-id="c1351-120">**A konfiguráció:**</span><span class="sxs-lookup"><span data-stu-id="c1351-120">**Configuration:**</span></span>

<span data-ttu-id="c1351-121">NAT-készlete:</span><span class="sxs-lookup"><span data-stu-id="c1351-121">NAT Pool:</span></span>

    object network outbound-PAT
        host <NAT-IP>

<span data-ttu-id="c1351-122">Célkiszolgáló:</span><span class="sxs-lookup"><span data-stu-id="c1351-122">Target Server:</span></span>

    object network Customer-Network
        network-object <IP> <Subnet-Mask>

<span data-ttu-id="c1351-123">Az ügyfél IP-címhez objektum-csoport</span><span class="sxs-lookup"><span data-stu-id="c1351-123">Object Group for Customer IP Addresses</span></span>

    object-group network MSFT-Network-1
        network-object <MSFT-IP> <Subnet-Mask>

    object-group network MSFT-PAT-Networks
        network-object object MSFT-Network-1

<span data-ttu-id="c1351-124">NAT-parancsok:</span><span class="sxs-lookup"><span data-stu-id="c1351-124">NAT Commands:</span></span>

    nat (inside,outside) source dynamic MSFT-PAT-Networks pat-pool outbound-PAT destination static Customer-Network Customer-Network


## <a name="juniper-srx-series-routers"></a><span data-ttu-id="c1351-125">Juniper SRX adatsorozat útválasztók</span><span class="sxs-lookup"><span data-stu-id="c1351-125">Juniper SRX series routers</span></span>
### <a name="1-create-redundant-ethernet-interfaces-for-hello-cluster"></a><span data-ttu-id="c1351-126">1. Redundáns Ethernet-illesztőkhöz használható hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1351-126">1. Create redundant Ethernet interfaces for hello cluster</span></span>
    interfaces {
        reth0 {
            description "tooInternal Network";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 1;
            }
            unit 100 {
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
        reth1 {
            description "tooMicrosoft via Edge Router";
            vlan-tagging;
            redundant-ether-options {
                redundancy-group 2;
            }
            unit 100 {
                description "tooMicrosoft via Edge Router";
                vlan-id 100;
                family inet {
                    address <IP-Address/Subnet-mask>;
                }
            }
        }
    }


### <a name="2-create-two-security-zones"></a><span data-ttu-id="c1351-127">2. Hozzon létre két biztonsági zónák</span><span class="sxs-lookup"><span data-stu-id="c1351-127">2. Create two security zones</span></span>
* <span data-ttu-id="c1351-128">Megbízhatósági kapcsolat a belső hálózathoz és zónát Untrust külső hálózat felé néző peremhálózati útválasztók</span><span class="sxs-lookup"><span data-stu-id="c1351-128">Trust Zone for internal network and Untrust Zone for external network facing Edge Routers</span></span>
* <span data-ttu-id="c1351-129">Rendelje hozzá a megfelelő adapterek toohello zónák</span><span class="sxs-lookup"><span data-stu-id="c1351-129">Assign appropriate interfaces toohello zones</span></span>
* <span data-ttu-id="c1351-130">Hello interfészeken-szolgáltatások engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c1351-130">Allow services on hello interfaces</span></span>

    <span data-ttu-id="c1351-131">biztonsági {zónák {biztonsági-zóna megbízhatósági {állomás bejövő-forgalom {-rendszerszolgáltatások {ping;                   } {bgp; protokollok                   {reth0.100;}} felületek               }} biztonsági-zóna Untrust {állomás bejövő-forgalom {-rendszerszolgáltatások {ping;                   } {bgp; protokollok                   {reth1.100;}} felületek               }           }       }   }</span><span class="sxs-lookup"><span data-stu-id="c1351-131">security {       zones {           security-zone Trust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth0.100;               }           }           security-zone Untrust {               host-inbound-traffic {                   system-services {                       ping;                   }                   protocols {                       bgp;                   }               }               interfaces {                   reth1.100;               }           }       }   }</span></span>


### <a name="3-create-security-policies-between-zones"></a><span data-ttu-id="c1351-132">3. Biztonsági házirendek közötti zónák létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1351-132">3. Create security policies between zones</span></span>
    security {
        policies {
            from-zone Trust to-zone Untrust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
            from-zone Untrust to-zone Trust {
                policy allow-any {
                    match {
                        source-address any;
                        destination-address any;
                        application any;
                    }
                    then {
                        permit;
                    }
                }
            }
        }
    }


### <a name="4-configure-nat-policies"></a><span data-ttu-id="c1351-133">4. NAT-szabályzatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1351-133">4. Configure NAT policies</span></span>
* <span data-ttu-id="c1351-134">Hozzon létre két NAT-készletek.</span><span class="sxs-lookup"><span data-stu-id="c1351-134">Create two NAT pools.</span></span> <span data-ttu-id="c1351-135">Egy lesz használt tooNAT forgalom kimenő tooMicrosoft és más Microsoft toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="c1351-135">One will be used tooNAT traffic outbound tooMicrosoft and other from Microsoft toohello customer.</span></span>
* <span data-ttu-id="c1351-136">TooNAT hello megfelelő forgalmi szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1351-136">Create rules tooNAT hello respective traffic</span></span>
  
       security {
           nat {
               source {
                   pool SNAT-To-ExpressRoute {
                       routing-instance {
                           External-ExpressRoute;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   pool SNAT-From-ExpressRoute {
                       routing-instance {
                           Internal;
                       }
                       address {
                           <NAT-IP-address/Subnet-mask>;
                       }
                   }
                   rule-set Outbound_NAT {
                       from routing-instance Internal;
                       toorouting-instance External-ExpressRoute;
                       rule SNAT-Out {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-To-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
                   rule-set Inbound-NAT {
                       from routing-instance External-ExpressRoute;
                       toorouting-instance Internal;
                       rule SNAT-In {
                           match {
                               source-address 0.0.0.0/0;
                           }
                           then {
                               source-nat {
                                   pool {
                                       SNAT-From-ExpressRoute;
                                   }
                               }
                           }
                       }
                   }
               }
           }
       }

### <a name="5-configure-bgp-tooadvertise-selective-prefixes-in-each-direction"></a><span data-ttu-id="c1351-137">5. Mindkét irányban BGP tooadvertise szelektív előtagok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c1351-137">5. Configure BGP tooadvertise selective prefixes in each direction</span></span>
<span data-ttu-id="c1351-138">Tekintse meg a toosamples [útválasztás konfigurációs minták ](expressroute-config-samples-routing.md) lap.</span><span class="sxs-lookup"><span data-stu-id="c1351-138">Refer toosamples in [Routing configuration samples ](expressroute-config-samples-routing.md) page.</span></span>

### <a name="6-create-policies"></a><span data-ttu-id="c1351-139">6. Házirendek létrehozása</span><span class="sxs-lookup"><span data-stu-id="c1351-139">6. Create policies</span></span>
    routing-options {
                  autonomous-system <Customer-ASN>;
    }
    policy-options {
        prefix-list Microsoft-Prefixes {
            <IP-Address/Subnet-Mask;
            <IP-Address/Subnet-Mask;
        }
        prefix-list private-ranges {
            10.0.0.0/8;
            172.16.0.0/12;
            192.168.0.0/16;
            100.64.0.0/10;
        }
        policy-statement Advertise-NAT-Pools {
            from {
                protocol static;
                route-filter <NAT-Pool-Address/Subnet-mask> prefix-length-range /32-/32;
            }
            then accept;
        }
        policy-statement Accept-from-Microsoft {
            term 1 {
                from {
                    instance External-ExpressRoute;
                    prefix-list-filter Microsoft-Prefixes orlonger;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
        policy-statement Accept-from-Internal {
            term no-private {
                from {
                    instance Internal;
                    prefix-list-filter private-ranges orlonger;
                }
                then reject;
            }
            term bgp {
                from {
                    instance Internal;
                    protocol bgp;
                }
                then accept;
            }
            term deny {
                then reject;
            }
        }
    }
    routing-instances {
        Internal {
            instance-type virtual-router;
            interface reth0.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Microsoft;
            }
            protocols {
                bgp {
                    group customer {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-ASN-1>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
        External-ExpressRoute {
            instance-type virtual-router;
            interface reth1.100;
            routing-options {
                static {
                    route <NAT-Pool-IP-Address/Subnet-mask> discard;
                }
                instance-import Accept-from-Internal;
            }
            protocols {
                bgp {
                    group edge-router {
                        export <Advertise-NAT-Pools>;
                        peer-as <Customer-Public-ASN>;
                        neighbor <BGP-Neighbor-IP-Address>;
                    }
                }
            }
        }
    }

## <a name="next-steps"></a><span data-ttu-id="c1351-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c1351-140">Next steps</span></span>
<span data-ttu-id="c1351-141">Lásd: hello [ExpressRoute – gyakori kérdések](expressroute-faqs.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="c1351-141">See hello [ExpressRoute FAQ](expressroute-faqs.md) for more details.</span></span>


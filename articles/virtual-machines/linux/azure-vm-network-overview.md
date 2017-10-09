---
title: "aaaAzure és a Linux virtuális gép hálózati áttekintése |} Microsoft Docs"
description: "Azure- és Linux rendszerű virtuális hálózatok áttekintése."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: b5420e35-0463-4456-9803-6142db150f2e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/25/2016
ms.author: v-livech
ms.openlocfilehash: c3de2dc583c62160e10c0e97e96fef49b9eaffbf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux-vm-network-overview"></a>Azure és a Linux virtuális gép hálózati – áttekintés
## <a name="virtual-networks"></a>Virtuális hálózatok
Egy Azure virtuális hálózatot (VNet) a saját hálózati hello felhőben megjelenítése. Hello Azure felhőben dedikált tooyour előfizetés logikai elkülönítése is. Hello IP-címblokkok, a DNS-beállítások, a biztonsági házirendek és a útvonaltáblák ezen a hálózaton belül teljes mértékben irányíthatja. Emellett tovább a vnetet alhálózatokra, és indítsa el az Azure infrastruktúra-szolgáltatási virtuális gépeket (VM) és/vagy felhőszolgáltatásokat (PaaS szerepkörpéldányok). Emellett hello virtuális hálózati tooyour a helyszíni hálózat hello kapcsolati lehetőségek, amely az Azure-ban egyikével csatlakoztathatja. Lényegében kiterjesztheti a hálózati tooAzure, amelyek hello előnye, hogy a vállalati méretű Azure által az IP-címblokkok teljes körű vezérlést.

* [Virtual Network áttekintése](../../virtual-network/virtual-networks-overview.md)

egy virtuális hálózat használatával toocreate hello Azure CLI-t, lépjen ide toofollow hello lépésein végighaladva.

* [Hogyan egy virtuális hálózat használatával toocreate hello Azure parancssori felület](../../virtual-network/virtual-networks-create-vnet-arm-cli.md)

## <a name="network-security-groups"></a>Network Security Groups (Hálózati biztonsági csoportok)
Hálózati biztonsági csoport (NSG) tartalmaz, amelyek engedélyezik vagy megtagadják a hálózati forgalom tooyour Virtuálisgép-példány egy virtuális hálózati hozzáférés-vezérlési lista (ACL) szabályok listáját. Az NSG-ket alhálózatokhoz vagy az alhálózaton belüli virtuálisgép-példányokhoz lehet hozzárendelni. Amikor egy NSG-t hozzárendelnek egy alhálózathoz, hello ACL szabályok érvényessé válnak tooall hello Virtuálisgép-példány alhálózaton. Ezenkívül forgalom tooan adott virtuális Gépre korlátozható egy NSG társításával további közvetlenül a virtuális gép toothat.

* [Mi az a hálózati biztonsági csoport (NSG)?](../../virtual-network/virtual-networks-nsg.md)
* [Hogyan toocreate NSG-ket a hello Azure parancssori felület](../../virtual-network/virtual-networks-create-nsg-arm-cli.md)

## <a name="user-defined-routes"></a>Felhasználó által megadott útvonalak
Amikor az Azure virtuális gépek (VM) tooa virtuális hálózathoz (VNet), megfigyelheti, hogy hello virtuális gépek automatikusan legyenek-e egymással képes toocommunicate hello hálózaton keresztül. Az átjáró toospecify nem kell annak ellenére, hogy a virtuális gépek hello külön alhálózatokon vannak. hello is igaz hello virtuális gépek toohello közti kommunikációhoz nyilvános interneten, és még akkor is, tooyour a helyszíni hálózatra is, ha egy hibrid kapcsolat az Azure tooyour saját datacenter jelen.

* [Mi a felhasználó által megadott útvonal és az IP-továbbítás?](../../virtual-network/virtual-networks-udr-overview.md)
* [Hozzon létre egy UDR hello Azure parancssori felület](../../virtual-network/virtual-network-create-udr-arm-cli.md)

## <a name="associating-a-fqdn-tooyour-linux-vm"></a>Egy teljesen minősített Tartományneve tooyour Linux virtuális gép társítása
Amikor létrehoz egy virtuális gép (VM) hello Azure-portálon hello Resource Manager üzembe helyezési modellben egy nyilvános IP-cím használatával hello virtuálisgép-erőforrást automatikusan létrejön. Az IP cím tooremotely hozzáférés hello virtuális gép használja. Bár hello portál nem hoz létre egy teljesen minősített tartománynevét, vagy teljesen minősített Tartománynevét, alapértelmezés szerint, egy hello virtuális gép létrehozása után is hozzáadhat.

* [Hozzon létre egy teljesen minősített tartománynevét hello Azure-portálon](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="network-interfaces"></a>Hálózati illesztők
A hálózati illesztő (NIC) a virtuális gép (VM) és az alapjául szolgáló szoftver hálózati hello hello összekapcsolása egy. Ez a cikk ismerteti a hálózati adaptert, és felhasználásukról hello Azure Resource Manager üzembe helyezési modellben.

* [Virtuális hálózati adapterek](../../virtual-network/virtual-network-network-interface.md)

## <a name="virtual-nics-and-dns-labeling"></a>Virtuális hálózati adaptert, és a DNS-címkézés
Ha egy kiszolgálót, hogy állandó kell toobe, de ez a kiszolgáló szarvasmarha a rendszer, és van bontva, és központilag telepített gyakran, érdemes toouse DNS szolgáltatást a hálózati toopersist hello neve hello VNET címkézését.  Az útmutató a következő hello be egy tartósan nevű hálózati Adaptert egy statikus IP-fog állítania.

* [Hozzon létre egy teljes Linux környezetet hello Azure parancssori felület használatával](create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network-gateways"></a>Virtuális hálózati átjárók
A virtuális hálózati átjáró használatos toosend hálózati forgalmat egy Azure virtuális hálózatot és a helyszíni helyek közötti, valamint az Azure (VNet – VNet) virtuális hálózatok között. VPN-átjáró konfigurálásakor létre kell hoznia és konfigurálnia kell egy virtuális hálózati átjárót és egy virtuális hálózati átjárókapcsolatot.

* [Információk a VPN Gateway-ről](../../vpn-gateway/vpn-gateway-about-vpngateways.md)

## <a name="internal-load-balancing"></a>Belső terheléselosztás
Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül. hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít. Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.

* [Egy belső terheléselosztói hello Azure parancssori felület használatával](../../load-balancer/load-balancer-get-started-internet-arm-cli.md)


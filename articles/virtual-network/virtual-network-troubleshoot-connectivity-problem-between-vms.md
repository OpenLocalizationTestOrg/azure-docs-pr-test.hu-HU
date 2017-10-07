---
title: "Azure virtuális gépek közötti aaaTroubleshooting kapcsolódási problémái vannak |} Microsoft Docs"
description: "Ismerje meg, hogyan tooTroubleshoot hello Azure virtuális gépek közötti kapcsolódási problémái vannak."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: ee0819178dcbee2bf529a495758e6c33f49e085e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a>Azure virtuális gépek közötti kapcsolati problémák elhárítása

Azure virtuális gépek közötti kapcsolódási problémák merülhetnek fel. Ez a cikk ismerteti a hibaelhárítási lépéseket toohelp a probléma megoldásához. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Jelenség

Egy Azure virtuális gép nem csatlakoztatható tooanother Azure virtuális Gépen.

## <a name="troubleshooting-guidance"></a>Hibaelhárítási útmutató 

1. [Ellenőrizze, hogy ha a hálózati adapter helytelenül van konfigurálva](#step-1-check-if-nic-is-misconfigured)
2. [Ha hálózati forgalmát blokkolja NSG-t vagy UDR ellenőrzése](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [Ha hálózati forgalmát blokkolja a virtuális gép tűzfal ellenőrzése](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [Ellenőrizze, hogy virtuális gép alkalmazás vagy szolgáltatás hello portot figyel](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [Ellenőrizze, hogy hello probléma okozza-e SNAT](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [Ellenőrizze, hogy forgalmat blokkolja az ACL-ek hello klasszikus virtuális gép](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [Ellenőrizze, hogy hello végpont kerül létrehozásra az hello klasszikus virtuális gép](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [Nem lehet tooconnect tooa VM hálózati megosztáson](#step-8-unable-to-connect-to-a-vm-network-share)
9. [Régión Vnet-kapcsolatot](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a>Hibaelhárítási lépések

Kövesse a lépéseket tootroubleshoot hello probléma. Ellenőrizze, hogy hello a probléma nem oldódik lépések után. 

### <a name="step-1-check-if-nic-is-misconfigured"></a>1. lépés: Ellenőrizze, hogy ha a hálózati adapter helytelenül van konfigurálva

Hajtsa végre a [hogyan tooreset a hálózati kapcsolat az Azure virtuális gépekhez Windows](../virtual-machines/windows/reset-network-interface.md). 

Ha a probléma hello hello hálózati illesztőt (NIC) módosítása után következik be, kövesse az alábbi lépéseket:

**A hálózati adapter Mulit virtuális gépek**

1. Adja hozzá a hálózati adaptert.
2. Javítsa ki a hello problémákat hello hibás, vagy távolítsa el a hálózati adapter hibás hálózati adaptert.  Majd readd hello hálózati adaptert.

További információkért lásd: [Hozzáadás hálózati illesztők tooor távolítsa el a virtuális gépek](virtual-network-network-interface-vm.md).

**Single-NIC VM** 

- [Telepítse újra a Windows virtuális gép](../virtual-machines/windows/redeploy-to-new-node.md)
- [Telepítse újra a Linux virtuális gép](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a>2. lépés: Ellenőrizze, hogy ha hálózati forgalmát blokkolja NSG-t vagy UDR

Használatára [hálózati figyelő IP folyamata ellenőrizze](../network-watcher/network-watcher-ip-flow-verify-overview.md) és [NSG folyamata naplózás](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, ha a hálózati biztonsági csoport vagy felhasználói útvonalat, amely ütközik az adatforgalmat.

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a>3. lépés: Ellenőrizze, hogy ha hálózati forgalmát blokkolja a virtuális gép tűzfal

Tiltsa le a hello tűzfal, és tesztelje a hello eredménye. Hello probléma megoldódott, ha a tűzfal hello hello-beállítások érvényesítése, és engedélyezze újra.

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-hello-port"></a>4. lépés: Ellenőrizze, hogy virtuális gép alkalmazás vagy szolgáltatás hello portot figyel

Hogy a virtuális gép alkalmazás vagy szolgáltatás hello porton figyel a következő módszerek toocheck hello egyikét használhatja

- A következő futtatási hello toocheck parancsokat, hogy hello server adott portot figyel.

**Windows rendszerű virtuális gép**

    netstat –ano

**Linux rendszerű virtuális gép**

    netstat -l

- Futtassa a hello **Telnet** hello VM tooitself tootest hello port parancs. Hello teszt sikertelen, alkalmazás vagy szolgáltatás nincs beállított toolisten hello porton.

### <a name="step-5-check-whether-hello-problem-is-caused-by-snat"></a>5. lépés: Ellenőrizze, hogy hello probléma okozza-e SNAT

Bizonyos esetekben hello VM mögé kerül, amely az Azure-on kívüli erőforrások Load balance megoldást. Ezekben az esetekben ha időszakos kapcsolattal problémák hello probléma okozhatja [SNAT port Erőforrásfogyás](../load-balancer/load-balancer-outbound-connections.md). tooresolve hello probléma, az egyes virtuális gépekhez, amely VIP (vagy a klasszikus ILPIP) létrehozása hello terheléselosztó mögött, és az NSG-t vagy a hozzáférés-vezérlési lista biztonságáról. 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-hello-classic-vm"></a>6. lépés: Ellenőrizze hogy forgalmat blokkolja az ACL-ek hello klasszikus virtuális gép

Hozzáférés-vezérlési Listában lehetővé teszi a hello képességét tooselectively engedély, vagy letilthatja a forgalmat a virtuális gép végpont. További információkért lásd: [kezelése hello ACL a végpont](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

### <a name="step-7-check-whether-hello-endpoint-is-created-for-hello-classic-vm"></a>7. lépés: Ellenőrizze, hogy hello végpont kerül létrehozásra az hello klasszikus virtuális gép

Az Azure-ban hello klasszikus üzembe helyezési modellel létrehozott összes virtuális gép automatikusan kommunikálhatnak más virtuális gépekkel hello a magánhálózat-csatornán keresztül azonos felhőalapú szolgáltatás, vagy a virtuális hálózat. Azonban más virtuális hálózatokon lévő számítógépek számára szükséges végpontok toodirect hello bejövő hálózati forgalom tooa virtuális gép. További információkért lásd: [hogyan mentése végpontok tooset](../virtual-machines/windows/classic/setup-endpoints.md).

### <a name="step-8-unable-tooconnect-tooa-vm-network-share"></a>8. lépés: Nem tooconnect tooa VM hálózati megosztáson

Ha nem tooconnect tooa VM hálózati megosztást, hello probléma okozhatja hello VM hálózati adapter nem érhető el. toodelete nem érhető el a hálózati adapterek hello című [hogyan toodelete hello nem érhető el a hálózati adapterek](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)

### <a name="step-9-inter-vnet-connectivity"></a>9. lépés: Virtuális közötti Vnet-kapcsolatot

Használatára [hálózati figyelő IP folyamata ellenőrizze](../network-watcher/network-watcher-ip-flow-verify-overview.md) és [NSG folyamata naplózás](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify, ha a hálózati biztonsági csoport vagy felhasználói útvonalat, amely ütközik az adatforgalmat. Az Inter-virtuális hálózat konfigurációt is ellenőrizheti [Itt](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).

### <a name="need-help-contact-support"></a>Segítség Forduljon a támogatási szolgálathoz.
Ha további segítségre van, [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) a probléma megoldódik gyorsan tooget.

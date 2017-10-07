---
title: az Azure Load Balancer IPv6 aaaOverview |} Microsoft Docs
description: "IPv6-támogatás az Azure terheléselosztó és a virtuális gépek elosztott terhelésű ismertetése."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: 6a1d583f-a305-40fd-a94b-fa42e1943bbb
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2016
ms.author: kumud
ms.openlocfilehash: 5b203f77d86cc1ad455f4ebb297097aef46b658d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-ipv6-for-azure-load-balancer"></a>IPv6-alapú, az Azure Load Balancer áttekintése

Az Internet felé néző terheléselosztók üzembe helyezhetők IPv6-címet. Ezenkívül tooIPv4 való kapcsolat esetén, ez lehetővé teszi a következő képességeket hello:

* Natív végpont IPv6-kapcsolatot nyilvános internetes ügyfelek és az Azure virtuális gépek (VM) közötti hello terheléselosztó keresztül.
* Natív végpont IPv6-alapú kimenő kapcsolatok virtuális gépek és a nyilvános Internet IPv6-kompatibilis ügyfelek között.

a következő kép hello hello IPv6 funkció az Azure Load Balancer mutatja be.

![Azure Load Balancer IPv6](./media/load-balancer-ipv6-overview/load-balancer-ipv6.png)

Amennyiben a telepített, egy IPv4- vagy IPv6-kompatibilis internetes ügyfél hello nyilvános IPv4- vagy IPv6-címek (vagy állomásnevek) hello Azure internetre irányuló terheléselosztót a kommunikációt. hello load balancer útvonalak hello IPv6 csomagok toohello saját IPv6-címek hello virtuális gépek hálózati címfordítás (NAT) használ. hello IPv6-alapú internetes ügyfél nem tud közvetlenül kommunikálni hello hello virtuális gépek IPv6-cím.

## <a name="features"></a>Szolgáltatások

Azure Resource Manager használatával telepített virtuális gépek natív IPv6-támogatást biztosít:

1. IPv6-alapú szolgáltatások terhelésű hello Internet IPv6-alapú ügyfelek
2. Natív IPv6 és IPv4-alapú végpontok ("kettős halmozott") virtuális gépeken
3. Bejövő és kimenő által kezdeményezett natív IPv6-kapcsolatok
4. Támogatott protokollok-pl.: TCP, UDP és HTTP (S) engedélyezése számos különféle szolgáltatás architektúrák

## <a name="benefits"></a>Előnyök

Ez a funkció lehetővé teszi, hogy a fő előnyei a következő hello:

* Felel meg, hogy új alkalmazásokat kell-e elérhető csak tooIPv6 ügyfelek igénylő kormányzati szabályzat
* Enable mobil és az eszközök internetes hálózata (IOT) fejlesztők toouse kettős halmozott (IPv4 + IPv6) Azure virtuális gépek tooaddress hello növekvő mobile & IOT piacok

## <a name="details-and-limitations"></a>Részletek és korlátozások

Részletek

* hello Azure DNS-szolgáltatás mind az IPv4 és IPv6 AAAA neve rekordok tartalmazza, és mindkét rekordok hello terheléselosztóhoz válaszol. hello ügyfél úgy dönt, hogy mely cím (IPv4 vagy IPv6) toocommunicate rendelkező.
* Amikor egy virtuális Gépet egy kapcsolat tooa nyilvános Internet IPv6-csatlakozóval csatlakoztatott eszközök kezdeményez, hello VM IPv6-cím forrása a lefordított (NAT) toohello nyilvános IPv6-cím hello terheléselosztó hálózati cím.
* Hello Linux operációs rendszert futtató virtuális gépek konfigurált tooreceive DHCP IPv6-alapú IP-címnek kell lennie. Már van Azure-katalógus hello hello Linux lemezképeket számos konfigurált toosupport IPv6 módosítás nélkül. További információkért lásd: [DHCPv6 konfigurálása Linux virtuális gépekhez](load-balancer-ipv6-for-linux.md)
* Ha úgy dönt, a terheléselosztóval mintavételi állapotfigyelő toouse egy IPv4-alapú mintavétel létrehozása és hello IPv4 és IPv6-végpontot használni. Ha hello szolgáltatást a virtuális gép leáll, a hello IPv4, mind az IPv6-végpontot kiveszik elforgatás.

Korlátozások

* IPv6 terheléselosztási szabályok hello Azure-portál nem adható hozzá. hello szabályok csak hello sablon, CLI-t, a PowerShell segítségével hozhatók létre.
* Előfordulhat, hogy nem frissít meglévő virtuális gépek toouse IPv6-címeket. Új virtuális gépeket kell telepítenie.
* Egy IPv6-cím rendelhetők hozzá az egyes virtuális gépek tooa egyetlen hálózati adapterrel.
* hello nyilvános IPv6-címek tooa virtuális gép nem lehet hozzárendelni. Csak hozzárendelhető tooa terheléselosztóhoz.
* A nyilvános IPv6-címek hello névkeresési DNS nem tudja konfigurálni.
* virtuális gépek hello hello IPv6-címekkel nem lehetnek tagjai az Azure-Felhőszolgáltatásban. Azok csatlakoztatott tooan Azure Virtual Network (VNet) és az IPv4-címét keresztül kommunikálnak egymással.
* Saját IPv6-címek egy erőforráscsoportot az egyes virtuális gépek telepíthetők, de nem telepíthető az egy erőforráscsoport keresztül méretezési készlet.
* Azure virtuális gépek IPv6 tooother virtuális gépeket, más Azure-szolgáltatások vagy a helyszíni eszközök keresztül nem tud csatlakozni. Csak kommunikálhatnak hello Azure terheléselosztó IPv6-kapcsolaton keresztül. Azonban amelyekkel kommunikálhatnak IPv4 használatával ezek más erőforrások.
* Hálózati biztonsági csoport (NSG) a védelem IPv4 protokoll kettős verem (IPv4 + IPv6) telepítések esetén támogatott. Az NSG-k toohello IPv6 végpont nem vonatkoznak.
* a virtuális gép nincs hello hello IPv6 végpont kitett közvetlenül toohello internet. Egy terheléselosztó mögött van. Csak a megadott hello load balancer szabályok hello portok IPv6-kapcsolaton keresztül érhetők el.
* Módosítása hello IdleTimeout paraméter, az IPv6 **jelenleg nem támogatott**. hello alapértelmezett érték négy perc.
* Módosítása hello loadDistributionMethod paraméter, az IPv6 **jelenleg nem támogatott**.
* IPv6-alapú IP-címek fenntartott (ahol IP = static) vannak **jelenleg nem támogatott**.

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan toodeploy IPv6 terheléselosztó.

* [IPv6 régiónként rendelkezésre állása](https://go.microsoft.com/fwlink/?linkid=828357)
* [Központi telepítése egy terheléselosztó IPv6-sablon használatával](load-balancer-ipv6-internet-template.md)
* [A terheléselosztó és az IPv6 az Azure PowerShell telepítése](load-balancer-ipv6-internet-ps.md)
* [Egy Azure CLI-vel rendelkező IPv6-alapú terheléselosztó telepítése](load-balancer-ipv6-internet-cli.md)

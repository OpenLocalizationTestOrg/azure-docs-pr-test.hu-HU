---
title: "aaaVPN átjáró klasszikus tooResource kezelő áttelepítésének |} Microsoft Docs"
description: "Ezen a lapon hello VPN Gateway klasszikus tooResource kezelő áttelepítésének áttekintése látható."
documentationcenter: na
services: vpn-gateway
author: amsriva
manager: rossort
editor: amsriva
ms.assetid: caa8eb19-825a-4031-8b49-18fbf3ebc04e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/02/2017
ms.author: amsriva
ms.openlocfilehash: c1d84bf5315224c5c8faac53d7957b496ed205ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vpn-gateway-classic-tooresource-manager-migration"></a>VPN-átjáró klasszikus tooResource kezelő áttelepítése
VPN-átjáróként is áttelepíthetők a klasszikus tooResource Manager üzembe helyezési modellben. További tudnivalók Azure Resource Manager [szolgáltatásait és előnyeit](../azure-resource-manager/resource-group-overview.md). Ez a cikk azt részletességi hogyan toomigrate a klasszikus üzembe helyezés toonewer erőforrás-kezelő alapú modell. 

VPN-átjárók települnek a klasszikus tooResource Manager VNet-áttelepítés részeként. Ez az áttelepítés egyszerre csak egy virtuális hálózat végezhető el. Nincs olyan eszközök, vagy Előfeltételek toomigration tekintetében további követelmény. Áttelepítési lépések azonos tooexisting VNet áttelepítési és leírása a következő [IaaS erőforrások áttelepítési lap](../virtual-machines/windows/migration-classic-resource-manager-ps.md). Nincs állásidő nélkül adatok elérési útja az áttelepítés során, és így meglévő alkalmazások folytatódna a helyszíni kapcsolat megszakadása nélkül toofunction az áttelepítés során. hello társított hello VPN-átjáró nyilvános IP-cím nem változik meg hello áttelepítési folyamat során. Ez azt jelenti, hogy nem kell tooreconfigure a helyszíni útválasztót hello áttelepítés befejeződése után.  

hello modell erőforrás-kezelő nem azonos a klasszikus modellt, és virtuális hálózati átjárók, a helyi hálózati átjáró és a kapcsolat erőforrások tevődik össze. Ezek képviselik hello VPN-átjáró magát, helyszíni címtér és két hello közötti kapcsolatot képviselő helyi webhely hello. Áttelepítés befejeződése után az átjáró nem klasszikus modellben érhető el, és a Resource Manager modellt használja a virtuális hálózati átjárók, a helyi hálózati átjáró és a kapcsolat objektumok összes felügyeleti műveletet kell végrehajtani.

## <a name="supported-scenarios"></a>Támogatott helyzetek
VPN-kapcsolat leggyakoribb forgatókönyve klasszikus tooResource kezelő áttelepítésének tartoznak. hello támogatott forgatókönyvek például-

* A csatlakozási pont toosite
* Hely toosite kapcsolatot a VPN-átjáró csatlakoztatva tooon helyszíni hely
* Virtuális hálózat tooVNet összekapcsolását két Vnetek VPN-átjárók használatával
* Több Vnetek csatlakoztatott toosame a helyi helye
* Többhelyes kapcsolat
* A kényszerített bújtatás Vnetek engedélyezve

Nem támogatott forgatókönyvek például a-  

* Az ExpressRoute-átjáró és a VPN-átjáró virtuális hálózat jelenleg nem támogatott.
* Átvitel forgatókönyvek csatlakoztatott tooon helyszíni kiszolgálók esetén a Virtuálisgép-bővítmények. VPN-kapcsolat korlátozások átvitel közben részleteket lejjebb olvashatja.

> [!NOTE]
> A klasszikus modellt CIDR-ellenőrzés az erőforrás-kezelő modell több mint egy hello szigorú. Áttelepítése előtt győződjön meg arról, hogy megfelelnek-e megadott klasszikus címtartományai toovalid CIDR formátumban hello áttelepítésének megkezdése előtt. CIDR érvényesíteni lehessen bármely közös CIDR érvényesség-ellenőrzők használatával. A virtuális hálózat vagy érvénytelen CIDR tartományok áttelepítésekor a helyi telephely eredményezne hibaállapotban van.
> 
> 

## <a name="vnet-toovnet-connectivity-migration"></a>Virtuális hálózat tooVNet kapcsolat áttelepítése
A klasszikus virtuális hálózat tooVNet kapcsolat értük hozzon létre egy helyi VNet csatlakozó hello ábrázolása. Az ügyfelek volt szükséges toocreate két helyi telephelyre amely hello két Vnetek, amelyekre szükség toobe kapcsolódik egymáshoz. Ezek akkor két Vnetek hello csatlakoztatott toohello megfelelő Vnetek IPsec alagút tooestablish közötti kapcsolat használatával. Ez a modell kezelhetőségi kihívást rendelkezik, mivel egy virtuális hálózat tartomány cím módosításai is kezelhetők hello megfelelő helyi ábrázolás. A Resource Manager modellt ezt a megoldást már nem szükséges. hello két Vnetek közvetlenül elérhető "Vnet2Vnet" kapcsolattípus használata a kapcsolati erőforrást hello közötti kapcsolat. 

![Képernyőkép a virtuális hálózat tooVNet áttelepítési.](./media/vpn-gateway-migration/migration1.png)

Virtuális hálózat az áttelepítés során azt észleli, amelyek csatlakoztatott entitás hello toocurrent virtuális hálózat VPN-átjáró két VNet között, és győződjön meg arról, hogy mindkét Vnetek áttelepítésének befejezése után már nem mutatunk be két helyi telephelyre hello képviselő más virtuális hálózat. hello Klasszikus modell két VPN-átjárók, a két helyi telephelyre és a közöttük két kapcsolatok átalakított tooResource Manager modell két VPN-átjárók és Vnet2Vnet típusú két kapcsolat.

## <a name="transit-vpn-connectivity"></a>Átvitel VPN-kapcsolat
VPN-átjárók konfigurálhatja a topológia úgy, hogy a helyi kapcsolat egy vnet csatlakozzon a virtuális hálózat, amely közvetlenül csatlakoztatott tooon helyszíni tooanother érhető el. Ez az átvitel VPN-kapcsolat csatlakoztatott tooon helyszíni erőforrások keresztül átvitel toohello VPN-átjáró, amely közvetlenül csatlakoztatott tooon-hez csatlakoztatott virtuális első VNet-példányok esetén. tooachieve ezt a konfigurációt a klasszikus üzembe helyezési modellel, a helyi webhely, amely rendelkezik összesített értéket képviselő mindkét hello csatlakoztatott virtuális hálózat és a helyszíni címtér előtagok toocreate kellene. A representational helyi webhely van, majd csatlakoztatott toohello VNet tooachieve tranzit kapcsolat. A klasszikus modellt is rendelkezik hasonló kezelhetőségi kihívás, mivel bármi is módosul a helyszíni-címtartományban is fenn kell tartani a virtuális hálózat és a helyszíni hello összesítésére képviselő hello helyi helyen. A támogatott erőforrás-kezelő átjáró BGP-támogatás bevezetése kezelhetőségi egyszerűbbé teszi a óta hello csatlakoztatott átjáró megtanulhassa útvonalakat a helyszíni tooprefixes manuális módosítása nélkül.

![Képernyőkép a tranzit útválasztás forgatókönyve.](./media/vpn-gateway-migration/migration2.png)

Mivel azt átalakításához VNet tooVNet kapcsolatot anélkül, hogy a helyi telephely a hello átvitel forgatókönyv helyszíni kapcsolata megszakad, amely közvetve csatlakoztatott tooon helyi vnet. hello a kapcsolat megszakadása kivédhető, az alábbi két módon hello áttelepítés befejeződött - 

* A BGP engedélyezéséhez a VPN-átjárók, egymáshoz kapcsolódó és tooon helyszíni. Kapcsolat nélkül a konfigurációs változásokat BGP engedélyezése állítja vissza, mert útvonalak megtanulta, és meghirdetett VNet-átjárók között. Vegye figyelembe, hogy BGP beállítás csak akkor érhető el, a Standard és magasabb SKU.
* Az érintett virtuális hálózat toohello helyi hálózati átjáró helyszíni hely képviselő explicit kapcsolatot létrehozni. Ez akkor is szükséges hello helyi útválasztó toocreate konfigurációjának módosítása, és hello IPsec-alagutat konfigurálhatja.

## <a name="next-steps"></a>Következő lépések
VPN-átjáró áttelepítési támogatás megismerését követően nyissa meg túl[IaaS-erőforrásokra, a klasszikus tooResource kezelő áttelepítésének platform által támogatott](../virtual-machines/windows/migration-classic-resource-manager-ps.md) tooget elindult.


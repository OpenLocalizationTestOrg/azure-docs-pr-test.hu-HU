---
title: "egy Azure virtuális hálózatot érintő Azure szolgáltatás megszűnésének hello esetén aaaWhat toodo |} Microsoft Docs"
description: "Ismerje meg, milyen toodo egy Azure virtuális hálózatot érintő Azure szolgáltatás megszűnésének hello esetén."
services: virtual-network
documentationcenter: 
author: NarayanAnnamalai
manager: jefco
editor: 
ms.assetid: ad260ab9-d873-43b3-8896-f9a1db9858a5
ms.service: virtual-network
ms.workload: virtual-network
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: narayan;aglick
ms.openlocfilehash: db022d2a042d255cf8ec6afb68cd8436aeecfe08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-network--business-continuity"></a>Virtuális hálózat – az üzletmenet folytonossága
## <a name="overview"></a>Áttekintés
Egy virtuális hálózatot (VNet) a hálózati hello felhő logikai reprezentációjává. A saját privát IP cím tárhely- és a szegmens hello hálózati alhálózatokra toodefine lehetővé teszi. A megbízhatósági kapcsolat határán toohost a számítási erőforrásokat, például az Azure virtuális gépek és Felhőszolgáltatások (webes/munkavégző szerepkörök) funkcionál Vnetek. Egy VNet lehetővé teszi, hogy a közvetlen saját integrációs csomaggal folytatott kommunikációhoz az üzemeltetett hello erőforrások között. Egy virtuális hálózatot is csatolt tooan a helyi hálózaton keresztül, például a VPN-átjáró vagy ExpressRoute hello hibrid lehetőségek közül.

A virtuális hálózat egy régiót hello hatókörén belül jön létre. Ugyanazt a címtartományt, két különböző régiókban Vnetek hozhat létre (azaz Velünk East és Velünk nyugati, de nem csatlakoztassa őket tooone egy másik közvetlenül). 

## <a name="business-continuity"></a>Üzleti folyamatok fenntarthatósága
Előfordulhat, hogy több különböző módon, hogy az alkalmazás sikerült-e működni. Egy adott régió sikerült teljesen vágható tooa természeti katasztrófa vagy részleges katasztrófa miatt több eszközök/szolgáltatások tooa hibája miatt. hello VNet szolgáltatás hello gyakorolt eltér a helyzetekben.

**K: Mit kell tennie egy kimaradás tooan teljes terület hello eseményben? például ha egy régiót teljesen levágási tooa természeti katasztrófa miatt? Mi történik, hogy a virtuális hálózatok üzemeltetett hello régióban toohello?**

V: hello virtuális hálózati és hello erőforrások hello érintett terület marad; hello szolgáltatás megszűnésének hello ideje alatt nem érhető el.

![Egyszerű virtuális hálózati diagramja](./media/virtual-network-disaster-recovery-guidance/vnet.png)

**K: milyen lehetőségeket toodo ugyanannak a virtuális hálózatnak egy másik régióban hozza létre újból hello?**

V: virtual Network (VNet) viszonylag könnyű erőforrás. Hívhat meg Azure API-k toocreate egy Vnetet az hello azonos címterének egy másik régióban. toore-hello létrehozása toomake API-hívások toore telepítette, ugyanabban a környezetben, amely szerepel hello érintett régió-a Cloud Services (webes/munkavégző szerepkörök) és az volt a virtuális gépek telepítéséhez. VPN-átjáró, ingyenesen tooyour a helyi hálózati csatlakozás, ha (például egy hibrid telepítésben) a helyi kapcsolat toospin is rendelkezik majd.

a virtuális hálózat létrehozásának a lépései hello találhatók [Itt](virtual-networks-create-vnet-arm-pportal.md). 

**K: egy replikát készít a virtuális hálózat egy adott régióban lehet hozza létre újra egy másik régióban időben?**

A: Igen két Vnetek hello segítségével azonos privát IP-címterületeken és a korábbi két különböző régiókban erőforrások time is létrehozhat. Ha az ügyfél internetre irányuló szolgáltatások a virtuális hálózat hello lett üzemeltet, a Traffic Manager toogeo-útvonal forgalmát toohello régiót, amelyben aktív volt beállította. Azonban az ügyfél nem tud csatlakozni a két Vnetek hello az azonos címe terület tootheir a helyszíni hálózat útválasztási problémák okozna. Egy olyan vészhelyzet esetén hello időpontja és egy Vnetet egy régióban megszűnését, az ügyfél képes kapcsolódni hello más VNet hello elérhető régióban egyező cím terület tootheir a helyszíni hálózattal.

a virtuális hálózat létrehozásának a lépései hello találhatók [Itt](virtual-networks-create-vnet-arm-pportal.md).


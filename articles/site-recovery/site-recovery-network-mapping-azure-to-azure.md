---
title: "az Azure Site Recovery két Azure-régiók közötti aaaNetwork leképezési |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja hello replikációjának, feladatátvételének és helyreállításának virtuális gépek és fizikai kiszolgálók. További információk a feladatátvételi tooAzure vagy egy másodlagos adatközpontba."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 4f80c44e3f94eaf446bc01a7041d91fe34aa78d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-mapping-between-two-azure-regions"></a>Két Azure-régiók közötti hálózatleképezés


Ez a cikk ismerteti, hogyan toomap Azure virtuális hálózatok az két Azure-régiók egymással. A hálózatleképezés biztosítja, hogy a replikált virtuális gép létrehozásakor hello cél Azure-régiót jön létre, hello virtuális hálózathoz csatlakoztatott toovirtual hálózati hello forrás virtuális gép.  

## <a name="prerequisites"></a>Előfeltételek
Mielőtt leképez hálózatok mindenképpen, létrehozott [Azure virtuális hálózataihoz](../virtual-network/virtual-networks-overview.md) mindkét forrás és cél Azure-régiók.

## <a name="map-networks"></a>Hálózatok leképezése

toomap egy Azure-régiót tooanother virtuális hálózat egy másik régióban, nyissa meg tooSite Recovery-infrastruktúra az Azure virtuális hálózat -> Hálózatleképezés (az Azure Virtual Machines), és hozzon létre egy hálózatra való leképezés.

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Az alábbi példában a virtuális gép Kelet-Ázsia régióban fut, és folyamatban van a hello replikálja a tooSoutheast Ázsia.

Válassza ki a forrás és cél hálózati hello, és kattintson az OK toocreate a hálózatra való leképezést a Kelet-Ázsia tooSoutheast Ázsia.

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Hello azonos dolog toocreate a hálózatra való leképezést a Délkelet-Ázsia tooEast Ázsia.  
![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Ha engedélyezve van a replikáció hálózati

Ha hálózatra való leképezés nem történik meg, amikor egy virtuális gép hello első alkalommal űrlap egy Azure-régiót tooanother replikál, akkor választhatja célhálózat hello részeként ugyanazt a folyamatot. A Site Recovery forrás régió tootarget régióban, és a cél régió toosource régió a kijelölés alapján hoz létre a hálózatok leképezését.   

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Alapértelmezés szerint a Site Recovery egy hálózatot hoz létre, amely azonos toohello Forráshálózat hello cél régióban, és adja hozzá a(z)-asr "hello forrás hálózat utótag toohello neve. Testreszabás kattintva választhat egy már létrehozott hálózati.

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Hello hálózatra való leképezés már megtörtént, ha replikáció során hello cél virtuális hálózat nem módosítható. toochange, meglévő hálózatleképezés módosítása.  

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Ha módosít egy hálózatra való leképezést a terület-1 tooregion-2, ellenőrizze, hogy a terület-2 tooregion-1, valamint a hello hálózatleképezés módosítása.
>
>


## <a name="subnet-selection"></a>Alhálózat kiválasztása
Alhálózati hello cél virtuális gép kiválasztása hello hello alhálózati hello forrás virtuális gép neve alapján történik. Ha hello alhálózatának azonos nevet hello forrás virtuális gép hello célhálózat, elérhető, amely, amely hello cél virtuális gép van kiválasztva, majd. Ha nem található hello alhálózat azonos név hello célhálózat, majd betűrendben az első alhálózat van kiválasztva, mert a cél alhálózathoz hello. Ez az alhálózat tooCompute és hello virtuális gép hálózati beállításainak megnyitásával módosíthatja.

![Alhálózat módosítása](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-cím

Az egyes hello hálózati illesztő hello cél virtuális gép IP-cím van kiválasztva az alábbiak szerint:

### <a name="dhcp"></a>DHCP
Ha hello hálózati illesztő hello forrás virtuális gép nem használja a DHCP, majd hello cél virtuális gép hálózati adapter is értéke DHCP.

### <a name="static-ip"></a>Statikus IP-cím
Ha hello hálózati illesztő hello forrás virtuális gép statikus IP-címet használ, majd hello cél virtuális gép hálózati adapter is toouse statikus IP-cím van beállítva. Statikus IP-cím van kiválasztva az alábbiak szerint:

#### <a name="same-address-space"></a>Ugyanazt a címtartományt

Ha hello forrás alhálózat és a cél alhálózathoz hello hello azonos címtartományon, cél IP-címet ugyanaz, mint a hello IP hello hálózati adapter hello forrás virtuális gép be. Ha ugyanazon IP-cím nem érhető el, majd néhány más elérhető IP értéke hello cél IP-címet.

#### <a name="different-address-space"></a>Különböző címterület

Ha hello forrás alhálózati és a cél alhálózathoz hello eltérő, cél IP-címet legyen az bármilyen elérhető IP-hello cél alhálózaton.

Minden hálózati illesztőn hello cél IP-címet tooCompute és hello virtuális gép hálózati beállításainak megnyitásával módosíthatja.

## <a name="next-steps"></a>Következő lépések

- További tudnivalók [hálózati útmutatót az Azure virtuális gépek replikálása](site-recovery-azure-to-azure-networking-guidance.md).

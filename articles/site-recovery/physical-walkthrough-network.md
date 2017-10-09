---
title: "a fizikai kiszolgáló replikációs tooAzure hálózati aaaPlan |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek hálózati tooAzure fizikai kiszolgálók replikálása esetén szükséges tervezési"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 71db2435-b5ce-4263-83f6-093d10d1d4e1
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: e2ca2db2a1cb58ca5468d4ee2b0406f29ff09479
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-physical-server-replication-tooazure"></a>4. lépés: A fizikai kiszolgáló replikációs tooAzure hálózatkezelés tervezése

Ez a cikk összegzi a tervezési szempontok, ha replikálása a helyszíni fizikai kiszolgálók tooAzure hello használata hálózati [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.

Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="connect-tooreplica-azure-vms"></a>Csatlakozás tooreplica Azure virtuális gépeken

A replikációs és feladatátvételi stratégiát megtervezésekor fontos kérdések hello egyik hogyan tooconnect toohello Azure virtuális gép a feladatátvételt követően. Számos több lehetősége a replika Azure virtuális gépek hálózati stratégia tervezésekor.

- **Különböző IP-cím**: kiválaszthatja toouse egy másik IP-címtartomány a hello replikált Azure Virtuálisgép-hálózatot. Ebben a forgatókönyvben a hello gép új IP-cím lekérése a után feladatátvétel és a DNS-frissítés szükséges.
- **Használjon hello azonos IP-cím**: érdemes toouse hello azonos IP-címtartomány, mint a helyszíni elsődleges hely, hello Azure-hálózatot a feladatátvételt követően. Egyszerűbbé teszi a ugyanazon IP-címeket való tartása hello hello helyreállítási csökkentésével kapcsolatos probléma hálózati feladatátvételt követően. Azonban ha tooAzure replikál, szüksége lesz tooupdate útvonalak hello új hellyel hello IP-címek a feladatátvételt követően.

## <a name="retain-ip-addresses"></a>Tartsa meg az IP-címek

A Site Recovery biztosít hello funkció tooretain rögzített IP-címek, amikor tooAzure egy alhálózat feladatátvételi feladatátvétele.
Alhálózati feladatátvétellel egy bizonyos alhálózat jelen webhely 1 vagy 2. hely, de soha nem mindkét helyen egyszerre. Rendelés toomaintain hello IP-címterének hello esemény a feladatátvétel, a programozott módon el rendezése hello útválasztó infrastruktúra toomove hello alhálózatok az egyik hely tooanother. A feladatátvételi hello alhálózatok áthelyezése az hello társított védett virtuális gépek. hello fő hátránya, hogy a hello esetre, ha nem, akkor toomove hello teljes alhálózattal rendelkezik.

### <a name="failover-example"></a>Feladatátvétel – példa

Egy példa a feladatátvevő tooAzure vizsgáljuk meg.

- Egy ficticious vállalati, a Woodgrove Bank üzemeltető üzleti alkalmazások helyszíni infrastruktúra van. A mobilalkalmazás Azure üzemelteti.
- Hello a helyszíni peremhálózati hálózati és hello Azure-beli virtuális hálózat közötti pont-pont (VPN) kapcsolatot biztosít a Woodgrove Bank virtuális gépeket az Azure és a helyszíni kiszolgálók közötti kapcsolatot.
- A VPN azt jelenti, hogy a vállalati virtuális hálózat az Azure-ban hello jelenik meg a helyszíni hálózat kiterjesztése.
- Woodgrove toouse Site Recovery tooreplicate helyszíni munkaterhelések tooAzure szeretne.
 - Woodgrove toodeal az alkalmazások és konfigurációk, amely IP-címek kódolt függ, és így kell tooretain IP-címek az alkalmazásuk kezelésére feladatátvételi tooAzure után van.
 - Woodgrove rendelkezik hozzárendelt IP-címek tartomány 172.16.1.0/24 172.16.2.0/24 tooits erőforrásaihoz, az Azure-ban.


A Woodgrove toobe képes tooreplicate a kiszolgálók tooAzure, miközben megtartja hello IP-címek ez milyen hello vállalatának szüksége toodo:

1. Hozzon létre egy Azure virtuális hálózatra. Hello kiterjesztése a helyszíni hálózat, így az alkalmazások zökkenőmentesen feladatátvétel kell tenni.
2. Azure lehetővé teszi, hogy Ön tooadd pont-pont VPN-kapcsolatot, továbbá toopoint-hely kapcsolatot toohello létrehozott virtuális hálózatokat az Azure-ban.
3. Hello pont-pont kapcsolat beállításakor a hello Azure hálózati, csak akkor, ha hello IP-címtartomány eltér a helyi IP-címtartomány hello irányítani tudja forgalom toohello helyszíni hely (helyi hálózati).
    - Ennek az az oka Azure felhőbe archivált alhálózatok nem támogatja. Ezért ha alhálózati 192.168.1.0/24 helyi, nem adhat hozzá egy helyi hálózati 192.168.1.0/24 hello Azure-hálózatot a.
    - Ez elvárható, hiszen az Azure nem tudja, hogy nincs aktív gépek hello alhálózaton van, és hello alhálózaton csak a vész-helyreállítási kerül létrehozásra.
    - toobe képes toocorrectly hálózati forgalom kívül egy Azure-hálózat hello alhálózatai hello és hello helyi-hálózat nem ütköznek.

![Alhálózati feladatátvétel előtt](./media/physical-walkthrough-network/network-design7.png)

#### <a name="before-failover"></a>Feladatátvétel előtt

1. Hozzon létre egy további hálózati (például a helyreállítási hálózati). Ez az hello hálózati amely átadja a virtuális gépek jönnek létre.
2. egy feladatátvétel után a virtuálisgép-tulajdonságokat hello őrzi meg, amely az IP-cím a gép hello tooensure > **konfigurálása**, adja meg a kiszolgáló hello azonos IP-cím a helyszíni rendelkezik, és kattintson a hello **mentése**.
3. Hello gép átadja a, ha az Azure Site Recovery rendeli a megadott IP-cím tooit hello.
4. Miután a feladatátvétel eseményindító aktiválódik, és hello virtuális gépeket hoz létre az Azure-ban szükséges hello IP-címmel, toohello hálózati használatával csatlakoztathatja a [tooVnet kapcsolatcsoporttal](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Ez a művelet parancsfájlalapú lehet.
5. Útvonalak toobe megfelelően módosítják, tooreflect kell, hogy a 192.168.1.0/24 tooAzure most már át lett helyezve.

    ![Alhálózati feladatátvételt követően](./media/physical-walkthrough-network/network-design9.png)

#### <a name="after-failover"></a>A feladatátvételt követően

Ha a fenti módon még nem rendelkezik az Azure-hálózatot, létrehozhat egy-helyek közti VPN-kapcsolat az elsődleges hely és az Azure, a feladatátvételt követően.

## <a name="change-ip-addresses"></a>IP-címek módosítása

Ez [blogbejegyzés](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) azt ismerteti, hogyan tooset be hello Azure hálózati infrastruktúra, ha már nincs szükség tooretain IP-címek a feladatátvételt követően. Ez a kezdődik-e az alkalmazás leírása, néz ki, hogyan tooset a hálózatkezelés a helyszíni és az Azure-ban, és folyamat végén a feladatátvételeket a futtatásával kapcsolatos információkat.  

## <a name="next-steps"></a>Következő lépések

Nyissa meg túl[5. lépés: Azure előkészítése](physical-walkthrough-prepare-azure.md)

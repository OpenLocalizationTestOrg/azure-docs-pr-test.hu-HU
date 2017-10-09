---
title: "Linux virtuális gépek irányelvek aaaAzure |} Microsoft Docs"
description: "További tudnivalók: hello legfontosabb tervezési és megvalósítási vonatkozó irányelveket az Azure Linux virtuális gépek telepítése"
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 740767d7-9a40-407b-93e7-c29dd976ffd7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.openlocfilehash: 0f779f791005441b6b16e106a3dc5a095bb33034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-guidelines-for-linux"></a>Azure virtuális gépeken Linux irányelveiről
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk az Azure környezetben szükséges tervezési lépéseket létrehozására és kezelésére a virtuális gépek (VM) ismertetése hello összpontosít.

## <a name="implementation-guidelines-for-vms"></a>Virtuális gépek implementációs segédlet
Döntéseket:

* Hány virtuális gépek szükség van a különböző alkalmazásrétegek és az infrastruktúra összetevőinek?
* Milyen CPU és memória-erőforrásokat kell vállalatnak követnie, minden virtuális gép, és melyek hello tárhellyel kapcsolatos követelmények?

Feladatok:

* Adja meg a virtuális gépeknek szüksége van az alkalmazás és hello erőforrások hello hello munkaterhelések.
* Hello erőforrás iránti igények kielégítése érdekében az egyes virtuális gépek hello megfelelő virtuális gép mérete és a tárolási típusú igazítása.
* Az erőforráscsoport hello külön rétegekhez és az infrastruktúra összetevőinek meghatározása.
* A virtuális gép elnevezési konvenciójának definiálása.
* Hozzon létre a virtuális gépek hello Azure CLI, webes portál, vagy a Resource Manager-sablonok használatával.

## <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
Hello fő erőforrások az Azure környezetben valamelyike valószínűleg virtuális gépeket. Ehhez az erőforráshoz, amelyiken futtatja a alkalmazások, adatbázisok, hitelesítési szolgáltatások, stb.

Fontos toounderstand hello [másik Virtuálisgép-méretek](sizes.md) toocorrectly méretet környezettől teljesítmény és a költséghatékonyság szempontjából. Ha a virtuális gépek nem rendelkezik elegendő Processzormagok vagy a memória, függetlenül attól, milyen mértékben tervezték és fejlesztett romlik a teljesítmény az alkalmazás. Felülvizsgálati hello javasolt a munkaterhelések minden egyes Virtuálisgép-sorozat kiindulási pontként, eldöntheti, melyik méret VM toouse az egyes összetevők a infrastruktúrában. Is [módosítsa a virtuális gép méretét hello](change-vm-size.md) telepítést követően.

Tárolási kulcsfontosságú szerepet játszik a virtuális gép teljesítményét. Standard szintű tárolást, rendszeres forgó lemezeket használó, vagy a prémium szintű storage használata magas i/o-munkaterhelések és SSD-lemezeket használó csúcsteljesítmény. Mint hello Virtuálisgép-méretet, a hiba vannak költség szempontok tooselecting hello adathordozóra. Hello olvasható [tárolási infrastruktúra irányelvek cikk](infrastructure-storage-solutions-guidelines.md) toounderstand hogyan toodesign megfelelő tár megadása a virtuális gépek optimális teljesítményét.

## <a name="resource-groups"></a>Erőforráscsoportok
Összetevők, például virtuális gépek amelyek logikailag egy csoportba az egyszerű kezelés és a karbantartás használatával [Azure erőforráscsoportok](../../azure-resource-manager/resource-group-overview.md). Erőforráscsoportok segítségével létrehozására, kezelésére és egy adott alkalmazás alkotó összes hello-erőforrások figyelése. Is megvalósíthatja [szerepköralapú hozzáférés-vezérlést](../../active-directory/role-based-access-control-what-is.md) toogrant hozzáférés tooothers belül a csapat tooonly hello erőforrások van szükségük. Az erőforráscsoportok és szerepkör-hozzárendelések idő tooplan igénybe vehet. Tervezése és megvalósítása erőforráscsoportok különböző szempontok tooactually vannak, ezért meg arról, hogy tooread hello [erőforrás csoportok irányelvek cikk](infrastructure-resource-groups-guidelines.md) toounderstand legjobb toobuild ki a virtuális gépek.

## <a name="templates"></a>Sablonok
Sablonok deklaratív JSON-fájlokat, toocreate által meghatározott hozhat létre a virtuális gépek. Sablonok általában is fel kell építenie hello szükséges tárolási, a hálózatkezelés, a hálózati adapterek, a IP-címzés, együtt hello virtuális stb. A fejlesztési és tesztelési célokra tooeasily replikálja az éles környezetekben sablonok toocreate egységes, reprodukálható környezetekben használható, és ez fordítva is igaz. További tudnivalók [kialakításához és -sablonokkal](../../azure-resource-manager/resource-group-overview.md#template-deployment) toounderstand hogyan használhatók a létrehozása és telepítése a virtuális gépek.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]


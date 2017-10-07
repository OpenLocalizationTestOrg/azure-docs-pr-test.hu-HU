---
title: "Virtuális gépek irányelvek aaaAzure |} Microsoft Docs"
description: "Tudnivalók: hello legfontosabb tervezési és megvalósítási Windows virtuális gépek telepítése az Azure"
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f932e65a-437b-48b0-8d70-f61ded8ce1c6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.openlocfilehash: d2c8043cd5829b54a5d57e56533122e42021797d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machines-guidelines-for-windows"></a>A Windows Azure virtuális gépek irányelvek
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

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
* Hozzon létre a virtuális gépek hello Azure PowerShell, a webes portál, vagy a Resource Manager-sablonok használatával.

## <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
Hello fő erőforrások az Azure környezetben valamelyike valószínűleg virtuális gépeket. Ehhez az erőforráshoz, amelyiken futtatja a alkalmazások, adatbázisok, hitelesítési szolgáltatások, stb.

Fontos toounderstand hello [másik Virtuálisgép-méretek](sizes.md) toocorrectly méretet környezettől teljesítmény és a költséghatékonyság szempontjából. Ha a virtuális gépek nem rendelkezik elegendő Processzormagok vagy a memória, függetlenül attól, milyen mértékben tervezték és fejlesztett romlik a teljesítmény az alkalmazás. Felülvizsgálati hello javasolt a munkaterhelések minden egyes Virtuálisgép-sorozat kiindulási pontként, eldöntheti, melyik méret VM toouse az egyes összetevők a infrastruktúrában. Is [módosítsa a virtuális gép méretét hello](resize-vm.md) telepítést követően.

Tárolási kulcsfontosságú szerepet játszik a virtuális gép teljesítményét. Standard szintű tárolást, rendszeres forgó lemezeket használó, vagy a prémium szintű storage nagy i/o-munkaterhelések és csúcsteljesítmény, SSD lemezeket használó használható. Mint hello Virtuálisgép-méretet, a hiba vannak költség szempontok tooselecting hello adathordozóra. Hello olvasható [tárolási infrastruktúra irányelvek cikk](infrastructure-storage-solutions-guidelines.md) toounderstand hogyan toodesign megfelelő tár megadása a virtuális gépek optimális teljesítményét.

## <a name="resource-groups"></a>Erőforráscsoportok
Összetevők, például virtuális gépek amelyek logikailag egy csoportba az egyszerű kezelés és a karbantartás használatával [Azure erőforráscsoportok](../../azure-resource-manager/resource-group-overview.md). Erőforráscsoportok segítségével létrehozására, kezelésére és egy adott alkalmazás alkotó összes hello-erőforrások figyelése. Is megvalósíthatja [szerepköralapú hozzáférés-vezérlést](../../active-directory/role-based-access-control-what-is.md) toogrant hozzáférés tooothers belül a csapat tooonly hello erőforrások van szükségük. Az erőforráscsoportok és szerepkör-hozzárendelések idő tooplan igénybe vehet. Tervezése és megvalósítása erőforráscsoportok különböző szempontok tooactually vannak, ezért meg arról, hogy tooread hello [erőforrás csoportok irányelvek cikk](infrastructure-resource-groups-guidelines.md) toounderstand legjobb toobuild ki a virtuális gépek.

## <a name="templates"></a>Sablonok
Sablonok deklaratív JSON-fájlokat, toocreate által meghatározott hozhat létre a virtuális gépek. Sablonok általában is fel kell építenie hello szükséges tárolási, a hálózatkezelés, a hálózati adapterek, a IP-címzés, együtt hello virtuális stb. Sablonok toocreate egységes, reprodukálható környezetekben inkább fejlesztési és tesztelési célokra tooeasily replikálja az éles környezetben, és ez fordítva is igaz. További tudnivalók [kialakításához és -sablonokkal](../../azure-resource-manager/resource-group-overview.md#template-deployment) toounderstand hogyan használhatók a létrehozása és telepítése a virtuális gépek.

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


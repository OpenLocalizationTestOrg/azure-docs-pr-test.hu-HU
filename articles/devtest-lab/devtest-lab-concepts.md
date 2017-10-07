---
title: aaaDevTest Labs fogalmak |} Microsoft Docs
description: "Ismerje meg a DevTest Labs hello alapvető fogalmait, és azt teheti, hogy könnyen toocreate, hogyan kezelheti és figyelheti az Azure virtuális gépek"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 105919e8-3617-4ce3-a29f-a289fa608fb2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: d9f1d948002c4d3121e5bdd4e65eb8b54cd91f9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="devtest-labs-concepts"></a>DevTest Labs-fogalmak
## <a name="overview"></a>Áttekintés
a következő lista hello alapfogalmakat DevTest Labs és definícióit tartalmazza:

## <a name="labs"></a>Tesztkörnyezetek
Egy laboratóriumi környezet hello infrastruktúra, amely magában foglalja egy csoportot az erőforrások, például a virtuális gépek (VM), amely lehetővé teszi, hogy jobb kezelése ezeket az erőforrásokat korlátozásai és a kvóták megadásával.

## <a name="virtual-machine"></a>Virtuális gép
Egy Azure virtuális gép egyike a számos különböző típusú [igény szerinti, méretezhető számítási erőforrások](https://docs.microsoft.com/azure/app-service-web/choose-web-site-cloud-service-vm) , amely az Azure biztosít. Az Azure virtuális gépek biztosít anélkül, hogy toobuy biztosítják a virtualizálás rugalmasságát hello és karbantartása hello fizikai hardveren futó, bár továbbra is szükséges toomaintain VM hello bizonyos feladatokat, például a konfigurálását, javítását és hello szoftverek telepítése amelyen fut rajta.

[A Windows Azure virtuális gépek – áttekintés](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-overview) által biztosított előtt érdemes megfontolni kapcsolatos információk létrehozni egy virtuális Gépet, hogyan hoz létre, és hogyan kezelheti.

## <a name="claimable-vm"></a>Claimable VM
Egy Azure Claimable a virtuális gép egy virtuális gépre, amelyik a labor engedélyekkel rendelkező felhasználó által használható. A tesztkörnyezet rendszergazdája készítse elő a virtuális gépek az adott alap képek és összetevők, és mentse őket megosztott tooa készlet. Egy lab-felhasználó akkor is jogcím egy működő virtuális gép hello készletből, ha szükségük van egy adott adott konfigurációval.

Egy virtuális Gépet, amely claimable tooany adott felhasználó kezdetben nincs hozzárendelve, de fog megjelenni a "Claimable virtual machines" minden felhasználó partnereinek listájához. Miután a felhasználó által a virtuális gépek igényelnek, magasabbra állítani tootheir "A virtuális gépnek" területen, és már nem claimable, amelyet semmilyen más felhasználó.

## <a name="environment"></a>Környezet
A DevTest Labs szolgáltatásban a környezet tooa Azure-erőforrások gyűjteménye, egy tesztkörnyezetben hivatkozik. [Ebben a blogbejegyzésben](https://blogs.msdn.microsoft.com/devtestlab/2016/11/16/connect-2016-news-for-azure-devtest-labs-azure-resource-manager-template-based-environments-vm-auto-shutdown-and-more/) ismerteti hogyan toocreate virtuális Gépre kiterjedő környezetekben az Azure Resource Manager-sablonok alapján.

## <a name="base-images"></a>Alap lemezképek
Alap képek VM-rendszerképek a hello eszközök és beállítások előtelepített és konfigurált tooquickly hozzon létre egy virtuális Gépet. Megadhat egy virtuális gép válassza háttérszínnek. egy meglévő talál, és vegye fel az összetevő tooinstall a teszt ügynök. Is majd mentés hello létesített VM, így hello talál használható minden létesítésére irányuló tooreinstall hello teszt ügynök nélkül hello VM base.

## <a name="artifacts"></a>Összetevők
Az összetevők használt toodeploy, és állítsa be az alkalmazását, a virtuális gép kiépítése után. Az összetevők lehetnek:

* Az eszközök, amelyet a Virtuálisgép - hello például ügynökök, a Fiddler és a Visual Studio tooinstall.
* A Virtuálisgép - hello toorun kívánt például egy tárház klónozása műveleteket.
* Az alkalmazások, amelyet az tootest.

Összetevőket [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) JSON-fájlok, melyek tooperform telepítési utasításokat és konfiguráció alkalmazásához.

## <a name="artifact-repositories"></a>Összetevő adattárak
Összetevő adattárak git tárházak találhatók, ahol a összetevők be van jelölve a rendszer. Összetevő tárolóhelyekkel a szervezet újbóli engedélyezése és a megosztási toomultiple labs lehet hozzáadni.

## <a name="formulas"></a>Képletek
Képletek, továbbá toobase lemezképek, a virtuális gép gyors kiépítése egy olyan mechanizmus biztosítása. A DevTest Labs szolgáltatásban egy képlete alapértelmezett tulajdonság használt értékek toocreate VM labor listáját.
A képletekkel, azonos beállítása tulajdonságai – például az alapjául szolgáló lemezképhez, a virtuális gép mérete, a virtuális hálózat és a összetevők - hello rendelkező virtuális gépek hozhatók létre kellene toospecify azokat a tulajdonságokat, minden alkalommal, amikor nélkül. Hello alapértelmezett értékek egy képletet a virtuális gép létrehozásakor használható- vagy módosítani.

## <a name="policies"></a>Házirendek
A házirendek segítenek a költség, a laborban vezérlése. Például létrehozhat egy házirendet tooautomatically, állítsa le a virtuális gépek a meghatározott ütemezés szerint.

## <a name="caps"></a>CAPS
Caps egy mechanizmus toominimize pazarlás a tesztkörnyezetben. Például beállíthatja a virtuális gépek is létrehozható, felhasználónként, vagy egy tesztkörnyezetben cap toorestrict hello száma.

## <a name="security-levels"></a>Biztonsági szint
Biztonsági hozzáférést által átruházásához hozzáférés-vezérlés (RBAC) határozza meg. toounderstand hogyan férhetnek hozzá a működését, hanem toounderstand hello különbségei engedélyt, a szerepkör és egy hatókör RBAC által definiált konfigurációjának kialakításához.

* Hozzáférés - engedély egy meghatározott hozzáférési tooa bizonyos művelet (pl. írásvédett tooall virtuális gépeken).
* Szerepkör - szerepkör csoportosítva, és hozzárendelt tooa felhasználó engedélyekkel. Például hello *előfizetés tulajdonosa* szerepkör rendelkezik egy előfizetésen belül tooall erőforrások eléréséhez.
* Hatókör - hatókör belül egy Azure-erőforrás, például egy erőforráscsoport, egyetlen labor vagy hello teljes előfizetés hello hierarchiája egy szint.

A DevTest Labs hello hatókörbe szerepkörök toodefine felhasználói engedélyek két típusa van: a tesztkörnyezet tulajdonosa és lab-felhasználó.

* Tesztlabor - A tesztkörnyezet tulajdonosa a tulajdonosnak hello laboron belüli tooany erőforrások eléréséhez. Ezért a tesztkörnyezet tulajdonosa is házirendek módosíthatók, olvasási és írási bármely virtuális gépeket, hello virtuális hálózat módosítása, és így tovább.
* Lab-felhasználó - lab-felhasználó összes lab-erőforrások, például a virtuális gépek, a házirendek és a virtuális hálózatok, megtekinthetik, de nem módosíthatja a házirendeket, vagy más felhasználók által létrehozott bármely virtuális gépeket.

toosee hogyan toocreate egyéni szerepkörök a DevTest Labs szolgáltatásban, tekintse meg a toohello cikk [felhasználói engedélyek toospecific labor házirendek](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Mivel a hatókörök hierarchikus, amikor egy felhasználó jogosult bizonyos hatókörre, ezeket az engedélyeket minden alacsonyabb szintű hatókörből lefedett automatikusan kapnak. Például ha a felhasználó az előfizetés tulajdonosa toohello szerepkör van hozzárendelve, majd rendelkeznek tooall az Azure-erőforrások előfizetés, többek között az összes virtuális gép, az összes virtuális hálózatot és az összes labs. Ezért egy előfizetés tulajdonosa automatikusan örökli a tesztkörnyezet tulajdonosa hello szerepe. Ellenkező hello azonban nem értéke true. A tesztkörnyezet tulajdonosa mint hello előfizetés szintje alacsonyabb hatókör hozzáférés tooa labor rendelkezik. A tesztkörnyezet tulajdonosa, ezért nem lesz képes toosee virtuális gépek vagy virtuális hálózatok, vagy minden olyan erőforrásnál, amely hello labor kívül esnek.

## <a name="azure-resource-manager-templates"></a>Azure Resource Manager-sablonok
Azure Resource Manager-sablonok, amelyek lehetővé teszik, hogy a jelen cikkben ismertetett fogalmakat konfigurálható hello hello infrastruktúra-vagy konfiguráció az Azure-megoldás és ismételten telepítése konzisztens lesz.

[Hello struktúra és az Azure Resource Manager-sablonok szintaxisát](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates#template-format) hello különböző szakaszban sablon érhető el az Azure Resource Manager sablon és hello tulajdonságok hello szerkezete ismerteti.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Következő lépések
[Labor létrehozása a DevTest Labs szolgáltatásban](devtest-lab-create-lab.md)

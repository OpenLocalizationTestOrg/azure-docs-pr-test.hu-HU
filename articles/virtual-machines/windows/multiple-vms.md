---
title: "Több virtuális gép létrehozása |} Microsoft Docs"
description: "Több virtuális gép létrehozása a Windows beállításai"
services: virtual-machines-windows
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: dfc1d1bb-a47d-4d7c-9fd2-f12050baacab
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/25/2016
ms.author: guybo
ms.openlocfilehash: 5e96805f8880a30a5fc8779d8f07addb6d068c09
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-multiple-azure-virtual-machines"></a>Több Azure virtuális gép létrehozása
Nem kell létrehozni nagy számú hasonló virtuális gépek (VM) több forgatókönyv áll rendelkezésre. Például nagy teljesítményű számítástechnikai (HPC), a nagyméretű adatok elemzése, méretezhető és gyakran állapotmentes középső rétegbeli vagy háttéradatbázis kiszolgálókat (például a webkiszolgálók), és elosztott adatbázisok.

Ez a cikk ismerteti, amelyek több virtuális gép létrehozása az Azure-ban rendelkezésre álló lehetőségeket. Ezek a lehetőségek az egyszerű esetekben, ahol létre manuálisan a virtuális gépek több túlmutató. Sok virtuális gép létrehozásához a folyamatok, amelyek általában nem méretezhető is, ha több mint néhány olyan virtuális gépek létrehozásához szükséges.

Számos hasonló virtuális gépek létrehozásának egyik módja, hogy használja az Azure Resource Manager szerkezet a *erőforrás hurkok*.

## <a name="resource-loops"></a>Erőforrás hurkok
Erőforrás hurkok az Azure Resource Manager sablonokban szintaktikai rövid tulajdonságnak. Erőforrás hurkok hurkot hasonló módon konfigurált erőforrások olyan készletét hozhatja létre. Erőforrás hurkok használatával több storage-fiókok, hálózati adapterek vagy virtuális gépek létrehozása. Erőforrás hurkok kapcsolatos további információkért tekintse meg a [hozzon létre virtuális gépek rendelkezésre állási készletek használata erőforrás hurkok](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>A skála kihívásai
Bár erőforrás hurkok könnyebben kimenő léptékű felhőalapú infrastruktúra összeállítása és tömörebb sablonok létrehozására, bizonyos kihívásokkal maradnak. Például erőforrás hurkot 100 virtuális gépek létrehozására használhatja, ha szüksége hálózati illesztő tartományvezérlők (NIC) a megfelelő virtuális gépek és tárfiókok összefüggéseket. Mivel valószínű, hogy a storage-fiókok száma eltér a virtuális gépek számát, akkor kell különböző erőforrás hurok méretű túl foglalkozik. Ezek solvable problémákat, de összetettségét jelentősen növeli a skála.

További kihívást következik be, amikor olyan infrastruktúra, amely rugalmasan méretezi van szüksége. Például előfordulhat, hogy szeretné az automatikus skálázás infrastruktúra, amely automatikusan növeli vagy csökkenti a virtuális gépek számát munkaterhelés válaszként. Virtuális gépek nem ad meg egy számot (kibővítési és a skála) eltérő integrált mechanizmus. Ha eltávolítja a virtuális gépek méretezési is nehézkes magas rendelkezésre állásának biztosítása azáltal, hogy meg arról, hogy a virtuális gépek frissítés és a tartalék tartományok közötti elosztását.

Végezetül erőforrás hurkot használatakor több hívások erőforrások létrehozásához lépjen az alatta üzemelő fizikai infrastruktúráéval. Ha több hívások hasonló erőforrások hoz létre, Azure rendelkezik tovább fejlesztheti a tervezési és telepítési megbízhatóságának és teljesítményének optimalizálásához implicit lehetőséget. Ez akkor, ha *virtuálisgép-méretezési csoportok* származnak.

## <a name="virtual-machine-scale-sets"></a>Virtuálisgép-méretezési csoportok
Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás üzembe helyezését, és az azonos virtuális gépek kezelésére. Minden virtuális gép azonos, a Virtuálisgép-méretezési készletek könnyen méretezése és ki konfigurálva. Egyszerűen módosítani a virtuális gépek számát a készlet. Beállíthatja úgy is automatikus skálázásra igényeit kiszolgálni, a munkaterhelés alapuló Virtuálisgép-méretezési készlet.

Méretezési és számítási erőforrásokat igénylő alkalmazásokhoz a skálázási műveletek implicit módon hiba és a frissítési tartományok közötti elosztását.

Helyett használatával történik, például a hálózati adapterek és a virtuális gépek több erőforrást, a Virtuálisgép-méretezési készlet hálózati, tárolási, virtuális gép és központilag konfigurálható bővítmény tulajdonságai rendelkezik.

Megismerkedhet a Virtuálisgép-méretezési készlet, tekintse meg a [virtuálisgép-méretezési beállítja, termékoldala](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Részletes információkat, navigáljon a [virtuálisgép-skálázási készletekben dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).


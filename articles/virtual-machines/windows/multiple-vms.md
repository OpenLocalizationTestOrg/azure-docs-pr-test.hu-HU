---
title: "aaaCreate több virtuális gép |} Microsoft Docs"
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
ms.openlocfilehash: 37729fabd91049744f6bd94c55221f5c1c65d527
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-multiple-azure-virtual-machines"></a>Több Azure virtuális gép létrehozása
Nem kell toocreate nagy számú hasonló virtuális gépek (VM) több forgatókönyv áll rendelkezésre. Például nagy teljesítményű számítástechnikai (HPC), a nagyméretű adatok elemzése, méretezhető és gyakran állapotmentes középső rétegbeli vagy háttéradatbázis kiszolgálókat (például a webkiszolgálók), és elosztott adatbázisok.

Ez a cikk ismerteti, amelyek hello elérhető lehetőségek toocreate több virtuális gép az Azure-ban. Ezek a beállítások hello egyszerű esetekben, ahol létre manuálisan a virtuális gépek több túlmutató. toocreate hello folyamatok, amelyek általában sok virtuális gép nem méretezhető is, ha néhány olyan virtuális gépek nagyobb toocreate van szüksége.

Sok hasonló virtuális gép toouse hello Azure Resource Manager szerkezetet az egyirányú toocreate *erőforrás hurkok*.

## <a name="resource-loops"></a>Erőforrás hurkok
Erőforrás hurkok az Azure Resource Manager sablonokban szintaktikai rövid tulajdonságnak. Erőforrás hurkok hurkot hasonló módon konfigurált erőforrások olyan készletét hozhatja létre. Több storage-fiókok, hálózati illesztőkre vagy virtuális gépek erőforrás hurkok toocreate használható. Erőforrás hurkok kapcsolatos további információkért tekintse meg túl[hozzon létre virtuális gépek rendelkezésre állási készletek használata erőforrás hurkok](https://azure.microsoft.com/documentation/templates/201-vm-copy-index-loops/).

## <a name="challenges-of-scale"></a>A skála kihívásai
Bár erőforrás hurkok tegye egyszerűbbé toobuild ki a felhőalapú infrastruktúrák a méretezési és tömörebb sablonok létrehozására, bizonyos kihívásokkal maradnak. Például ha egy erőforrás hurok toocreate 100 virtuális gépek használatához szüksége toocorrelate hálózati illesztő tartományvezérlőket (NIC) a megfelelő virtuális gépek és tárfiókok. Mivel a virtuális gépek száma hello valószínűleg toobe tárfiókok hello száma eltér, akkor kell toodeal különböző erőforrás hurok mérete, a túl. Ezek solvable problémákat, de hello összetettsége jelentősen növeli a skála.

További kihívást következik be, amikor olyan infrastruktúra, amely rugalmasan méretezi van szüksége. Például előfordulhat, hogy szeretné az automatikus skálázás infrastruktúra, amely automatikusan növeli vagy csökkenti a hello válasz tooworkload a virtuális gépek száma. Virtuális gépek nem ad meg egy integrált mechanizmus toovary (kibővítési és a skála) száma. Eltávolítja a virtuális gépek méretezési esetén nehéz tooguarantee magas rendelkezésre állású azáltal, hogy meg arról, hogy a virtuális gépek frissítés és a tartalék tartományok közötti elosztását.

Végezetül erőforrás hurkot használatakor több hívások toocreate erőforrások toohello alapjául szolgáló háló lépjen. Hasonló erőforrások létrehozásakor a több hívások Azure egy implicit lehetőség tooimprove esetén ez a kialakítás rendelkezik, és telepítési megbízhatóságának és teljesítményének optimalizálásához. Ez akkor, ha *virtuálisgép-méretezési csoportok* származnak.

## <a name="virtual-machine-scale-sets"></a>Virtuálisgép-méretezési csoportok
Virtuálisgép-méretezési csoportok olyan Azure számítási erőforrás toodeploy és az azonos virtuális gépek kezelésére. A konfigurált összes virtuális gép hello azonos, Virtuálisgép-méretezési készlet a könnyen tooscale és kibővítési. Egyszerűen módosítani hello hello beállítása a virtuális gépek száma. VM skálázási készletek tooautoscale hello munkaterhelés hello igények alapján is konfigurálhatja.

Tooscale számítási erőforrásokat, és a skála igénylő alkalmazásokhoz műveletek implicit módon hiba és a frissítési tartományok közötti elosztását.

Helyett használatával történik, például a hálózati adapterek és a virtuális gépek több erőforrást, a Virtuálisgép-méretezési készlet hálózati, tárolási, virtuális gép és központilag konfigurálható bővítmény tulajdonságai rendelkezik.

Egy bevezető tooVM skálázási készletekben, részletek toohello [virtuálisgép-méretezési beállítja, termékoldala](https://azure.microsoft.com/services/virtual-machine-scale-sets/). Részletes információkat, nyissa meg toohello [virtuálisgép-skálázási készletekben dokumentáció](https://azure.microsoft.com/documentation/services/virtual-machine-scale-sets/).


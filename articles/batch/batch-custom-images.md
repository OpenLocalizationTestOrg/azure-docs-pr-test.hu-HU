---
title: "az egyéni lemezképek készletbe aaaProvision Azure Batch |} Microsoft Docs"
description: "Létrehozhat egy kötegben, egy egyéni lemezkép tooprovision a készlet számítási csomópontok hello szoftvert tartalmazó és az alkalmazáshoz szükséges adatokat. Egyéni lemezkép tooconfigure számítási csomópontok toorun a Batch-alkalmazások hatékonyan."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 5cb698ee90f7d3ec9ffe69fa4dc602132c3f7569
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-custom-image-toocreate-a-pool-of-virtual-machines"></a>Egy egyéni lemezkép toocreate virtuális gépek készletét használja

Az Azure Batch egyes virtuális gépek létrehozásakor meg kell adnia egy virtuális gép (VM) lemezképfájlt, hello operációs rendszer biztosít minden számítási csomópont hello készletben. Virtuális gépek készletét hozhat létre, vagy egy Azure piactér lemezkép használatával, vagy egy egyéni előkészítette a Virtuálismerevlemez-kép megadásával. Ha ad meg egy egyéni lemezképet, befolyásolni hogyan hello operációs rendszer minden számítási csomópont ellátott hello időpontban van konfigurálva. Az egyéni lemezképet is használható alkalmazások és referenciaadatok hello elérhető számítási csomópont, amint ki van építve.

Egyéni lemezkép használatával is idő takarítható meg a számítási csomópontok a készlet első kész toorun a Batch számítási feladathoz. Mindig Azure piactér képet, és rendelkezik a kiépítése után telepíthet szoftvereket minden számítási csomópont, lehet, hogy ez a megközelítés egyéni lemezkép használatával-nél kevésbé hatékonyak. 

Egyes okok toouse egy egyéni helyzetnek konfigurált lemezképben kell:

- **Hello operációs rendszer konfigurálása** semmiféle speciális beállítást hello operációs rendszer hello egyéni lemezkép hajtható végre. 
- **Nagy alkalmazásokat telepíteni.** Egyéni alkalmazást is telepíthet hatékonyabb, mint az kiépítése után telepíteni őket minden számítási csomóponton.
- **Jelentős mennyiségű adatot másolni.** Ha hello adatok másolt toohello egyéni lemezképet, csak egyszer, ahelyett, hogy tooeach számítási csomópont, időt és sávszélességet takaríthat másolt toobe van szüksége.
- **Indítsa újra a virtuális gép hello hello beállítási folyamata során.** Újraindítás hello VM időigényes folyamat lehet, különösen akkor, ha a számítási csomópontok száma.

## <a name="prerequisites"></a>Előfeltételek

- **Batch-fiók létrehozása hello felhasználói előfizetési tárolókészlet foglalási üzemmóddal.** toouse egy egyéni lemezkép tooprovision virtuálisgép-készletek, a Batch-fiók létrehozása a felhasználói előfizetési hello [tárolókészlet foglalási mód](batch-api-basics.md#pool-allocation-mode). Ebben a módban a kötegelt készletek hello fiókot tartalmazó hello előfizetéssé foglal le. Lásd: hello [fiók](batch-api-basics.md#account) szakasz [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md) hello tárolókészlet foglalási mód beállításáról a Batch-fiók létrehozásakor.

- **Egy Azure Storage-fiók.** toocreate egyéni lemezkép használatával virtuális gépek készletét, szabványos, általános célú Azure Storage-fiók szükséges a hello azonos előfizetésbe és azonos térségbe. Ha egy egyéni lemezképet hoz létre egy Azure virtuális Gépen, akkor hello kép toohello tárfiók ahol hello virtuális gép operációsrendszer-lemez helyezkedik el, és nincs szükség külön tárfiókot toocreate fogja másolni. 
    
## <a name="prepare-a-custom-image"></a>Egyéni lemezkép előkészítése

tooprepare egyéni lemezkép kötegelt való használatra, kell generalize Linux vagy a Windows meglévő telepítését. Normalizálása egy operációs rendszer telepítése eltávolítja a számítógép-specifikus adatait. hello eredménye egy olyanra, amely a más számítógépeken és a virtuális gépek telepíthető.  

> [!IMPORTANT]
> Kötegelt jelenleg nem támogatja a Azure használatával kezelt lemezképek tooprovision készlet. hello egyéni lemezképet használ egy készletet kell tárolni az Azure Storage tooprovision. 
>
> Az egyéni lemezképet előkészítésekor tartsa szem előtt tartva hello pontok a következő:
> - Győződjön meg arról, hogy hello alap operációsrendszer-lemezképek használhatja a kötegelt készletek does tooprovision nem rendelkezik olyan előre telepített Azure-bővítményeket, például az egyéni parancsprogramok futtatására szolgáló bővítmény hello. Ha hello kép egy előre telepített bővítményt tartalmazza, Azure hello VM telepítése problémák fordulhatnak elő.
> - Győződjön meg arról, hogy hello alap operációsrendszer-lemezképek, megadására, használ hello alapértelmezett ideiglenes meghajtón, mert hello kötegelt csomópont ügynök jelenleg vár hello alapértelmezett ideiglenes meghajtón.
>
>

Egyéni lemezképként használhat meglévő előkészített Windows vagy Linux képet. Például ha akarja toouse egy helyi lemezképet, majd töltse fel a hello kép tooan Azure Storage-fiók, amely hello azonos előfizetésével és régiójával, a kötegelt fiók használatával [AzCopy](../storage/storage-use-azcopy.md) vagy egy másik feltöltése.

Egyéni lemezkép előkészítheti egy új vagy meglévő Azure virtuális gépről is. Ha egy új virtuális Gépet hoz létre, Azure piactér lemezkép használata az egyéni lemezképet hello alapjául szolgáló lemezképhez, és majd testreszabása. toocustomize hello alapjául szolgáló lemezképhez, hozzon létre egy Azure virtuális Gépet, és adja hozzá az alkalmazások és adatok tooit. Majd generalize hello VM tooserve, az egyéni lemezképet, és mentse tooAzure tároló. 

egy Azure virtuális gép, egy egyéni lemezkép tooprepare kövesse az alábbi lépéseket:

1. Hozzon létre egy **nem felügyelt** Azure virtuális gép Azure Piactéri lemezképből. Az Azure piactéren olyan képek mindkét [Windows](../virtual-machines/windows/quick-create-portal.md) és [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    3. lépésében hello virtuális gép létrehozási folyamata, győződjön meg arról, hogy kiválasztotta **nem** a **tárolási: használata felügyelt lemezek** lehetőséget. Is figyelembe hello a tárfióknév hello virtuális gép operációsrendszer-lemez, mivel ezt a tárfiókot is, ahol Azure menti az egyéni lemezképet:

    ![Hozzon létre egy nem felügyelt virtuális gép és a Megjegyzés hello tárfiók neve](media/batch-custom-images/vm-create-storage.png)
 
2. Végezze el a virtuális gép létrehozásának folyamatán hello, és várjon, amíg az Azure által lefoglalt toobe. Íme egy képet jeleníti meg a virtuális gépek hello hello fut, az Azure-portálon:

    ![Hozzon létre egy virtuális gép egy Piactéri rendszerképből](media/batch-custom-images/vm-status-running.png)

3. Hello virtuális gép fut, egyszer tooit RDP (a Windows) vagy (a Linux) SSH keresztül csatlakozni. Minden szükséges szoftvert telepíteni, illetve másolni kívánt adatokat, majd generalize hello VM. Kövesse az ismertetett lépések hello [Generalize hello VM](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Hello lépésekkel túl[tooAzure PowerShell-e jelentkezni](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). tooinstall Azure PowerShell, lásd: [áttekintés az Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. A következő lépésekkel hello túl[Deallocate hello virtuális gép és a set hello állapot toogeneralized](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    Az Azure-portálon hello figyelje meg, hogy virtuális gép felszabadítása hello:

    ![Győződjön meg arról, hogy virtuális gép felszabadítása hello](media/batch-custom-images/vm-status-deallocated.png)

6.  Hozzon létre és mentsen hello méretű kép tooAzure tárolási hello segítségével [mentés-AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) PowerShell-parancsmagot. Útmutatás alapján hello ismertetett [hello kép létrehozása](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    hello virtuális gép képének mentési toohello Azure Storage-fiók létrehozása hello virtuális gép létrehozása után ez az eljárás 1. lépésben ismertetett módon. hello mentés-AzureRmVMImage parancsmag menti hello kép toohello **rendszer** storage-fiókhoz tartozó tárolóban. Hello `-DestinationContainername` paraméternév hello belül egy virtuális könyvtárat **rendszer** tároló. Hello `-VHDNamePrefix` paraméterrel hello blob neve előtag. Az előtag kötőjel $a toohello blob neve. 

    Tegyük fel, hogy Ön mentés-AzureRmVMImage hívja meg a következő paraméterek hello:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    hello eredményül kapott képének mentési toohello helyét és a blob neve itt látható:

    ![A rendszer tárolóhoz mentett VHD-fájl helyét](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Az Azure nem felügyelt virtuális gép több tárfiókok különböző célokra hoz létre. Ha nem Megjegyzés: Ha hello hello OS lemez hello hello a tároló neve virtuális gép létrejött, majd kapcsolódó hello tárolási fiókot, amellyel Keresett tartalmaz hello **rendszer** tároló. Haladjon végig hello **rendszer** tároló toofind hello egyéni lemezkép megadott hello értékek használ hello **mentés-AzureRmVMImage** parancsot.

## <a name="create-a-pool-from-a-custom-image-in-hello-portal"></a>Egy egyéni lemezképből hello portálon készlet létrehozása

Ha mentette az egyéni lemezképet, és ismeri a helyét, a Batch-készlet erről a képről is létrehozhat. Kövesse ezeket a lépéseket toocreate készlet hello Azure-portálon:

1. Keresse meg a Batch-fiók tooyour hello Azure-portálon. Ennek a fióknak kell lennie a hello azonos előfizetésbe és azonos térségbe hello tárolóként fiók tartalmazó hello egyéni lemezképet. 
2. A hello **beállítások** hello balra, jelölje be hello ablak **készletek** menüpont.
3. A hello **készletek** ablakban, a select hello **Hozzáadás** parancsot.
4. A hello **a készlet hozzáadása** ablakban válassza ki **egyéni lemezkép (Linux és Windows)** a hello **képtípus** legördülő menüből. hello portál megjeleníti hello **egyéni lemezkép** kiválasztása. Keresse meg a toohello tárfiókot, ahol az egyéni lemezképet, és válassza ki a kívánt virtuális merevlemez hello tárolóból hello. 
5. Jelölje be hello megfelelő **Publisher/ajánlat/Sku** az egyéni virtuális merevlemez.
6. Adja meg a beállításokat, beleértve a hello hello fennmaradó szükséges **csomópont méretének**, **céloz dedikált csomópontok**, és **alacsony prioritású csomópont**, valamint minden szükséges, választható beállítások.

    Például a Microsoft Windows Server Datacenter 2016 egyéni kép hello **a készlet hozzáadása** ablak megjelenik, amint az itt látható:

    ![Egyéni Windows-lemezkép a készlet hozzáadása](media/batch-custom-images/add-pool-custom-image.png)
  
toocheck e egy meglévő készlet alapuló egyéni lemezképet, tekintse meg a hello **operációs rendszer** hello erőforrás összefoglaló szakaszában hello tulajdonság **készlet** ablak. Hello készlet egyéni lemezkép alapján készült, ha túl van-e beállítva**egyéni Virtuálisgép-lemezkép**.

A készlethez társított összes egyéni virtuális merevlemezek hello készlet megjelenő **tulajdonságok** ablak.
 
## <a name="next-steps"></a>Következő lépések

- Kötegelt, áttekintéséért lásd: [Develop nagyméretű párhuzamos számítási solutions a kötegelt](batch-api-basics.md).
- Batch-fiók létrehozásával kapcsolatos további információkért lásd: [Batch-fiók létrehozása az Azure-portálon hello](batch-account-create-portal.md).

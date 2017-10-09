---
title: "a nagy Azure virtuálisgép-méretezési csoportok aaaWorking |} Microsoft Docs"
description: "Mit kell tooknow toouse nagyméretű Azure virtuális gép skálázása beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 2/7/2017
ms.author: guybo
ms.openlocfilehash: a39aab25925d7fc50763f0a20148b1f2213b492f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="working-with-large-virtual-machine-scale-sets"></a>Nagyméretű virtuálisgép-méretezési csoportok használata
Mostantól létrehozhat Azure [virtuálisgép-méretezési csoportok](/azure/virtual-machine-scale-sets/) too1, 000 virtuális gép mentése kapacitással. A jelen dokumentum egy _nagy virtuálisgép-méretezési csoport_ egy méretezési készletben képes mint 100 virtuális gépek toogreater skálázás típusúként van definiálva. Ezt a képességet a méretezési csoport egyik tulajdonsága adja meg (_singlePlacementGroup=False_). 

Nagy méretű bizonyos elemeinek állítja be, például a terhelést és a tartalék tartományok eltérően viselkednek tooa szabványos méretezési készlet. Ez a dokumentum nagy méretű méretezési csoportok hello jellemzőit ismerteti, és leírja, milyen kell tooknow toosuccessfully használhatja azokat az alkalmazásokban. 

Nagy léptékű felhőalapú infrastruktúra üzembe helyezéséhez egy általánosan használt megközelítés olyan toocreate _skálázási egységek_, például hozzon létre több virtuális gépek méretezési készletek több Vnetek és a storage-fiókok. Ez a megközelítés biztosítja egyszerűbb felügyeleti képest toosingle virtuális gépek, és hasznos a számos olyan alkalmazás, különösképpen más rakatolható összetevők, például a virtuális hálózatok és a végpontok igénylő több méretezési egységeket. Ha az alkalmazás egy nagy fürtön azonban, célszerű egyetlen méretezési beállítása a too1, 000 virtuális gépek egyszerűbb toodeploy. A példaforgatókönyvek központosított big data-alapú üzemelő példányokat, vagy a munkavégző csomópontok nagy készleteinek egyszerű kezelését igénylő számítási grideket tartalmaznak. Virtuálisgép-méretezési készlet együtt [adatlemezt csatolni](virtual-machine-scale-sets-attached-disks.md), nagy méretű méretezési csoportok lehetővé teszik a toodeploy egy skálázható infrastruktúrája magok ezer és petabájt is, mint egyetlen műveletben.

## <a name="placement-groups"></a>Elhelyezési csoportok 
Milyen részekből egy _nagy_ méretezési készletben különleges nincs hello virtuális gépeinek számával, hanem hello száma _elhelyezési csoportok_ tartalmaz. Elhelyezési csoport olyan szerkezet hasonló tooan Azure rendelkezésre állási, a saját tartalék tartományok és a frissítési tartományok. A méretezési csoport alapértelmezés szerint egy legfeljebb 100 virtuális gép méretű elhelyezési csoportból áll. Ha egy méretezési nevű _singlePlacementGroup_ értéke too_false_, hello méretezési több elhelyezési csoport állhat, és 0 – 1000 tartománnyal rendelkező virtuális gépeket. Ha az alapértelmezett értéke toohello beállítása _igaz_, a méretezési egyetlen elhelyezési csoport áll, és 0 – 100 tartománnyal rendelkező virtuális gépeket.

## <a name="checklist-for-using-large-scale-sets"></a>Ellenőrzőlista a nagyméretű méretezési csoportok használatához
toodecide, hogy az alkalmazás képes hatékony felhasználása nagy méretű méretezési csoportok, fontolja meg a követelményeknek hello:

- A nagyméretű méretezési csoportokhoz az Azure Managed Disks szükséges. Azokhoz a méretezési csoportokhoz, amelyeket nem a Managed Disksszel hoztak létre, több tárfiókra van szükség (egyre minden 20 virtuális géphez). Nagy méretű méretezési csoportok kizárólag a storage-fiókok korlátozza a storage management terhelés, és tooavoid hello kockázatát előfizetés rendszert futtató felügyelt lemezek tooreduce tervezett toowork. Nem kezelt lemezek használja, a méretezési esetén korlátozott too100 virtuális gépek.
- Méretezési készlet létrehozása az Azure piactéren elérhető rendszerkép too1, 000 virtuális gépek költenie.
- Egyéni képek (VM-lemezképekkel hoz létre, és töltse fel saját maga) alapján létrehozott méretezési csoportok jelenleg legfeljebb too100 virtuális gépeket.
- Réteg-4 terheléselosztás hello Azure terheléselosztó és még nem támogatott a méretezési készlet több elhelyezési csoport alkotja. Azure Load Balancer győződjön meg arról, hogy hello méretezési készletben toouse hello szüksége van konfigurált toouse egyetlen elhelyezési csoport, amely hello alapértelmezett beállítás.
- Az összes méretezési csoportok réteg-7 terheléselosztás és hello Azure Application Gateway esetén támogatott.
- A méretezési van definiálva, egyetlen alhálózattal – ellenőrizze, hogy az alhálózat rendelkezik egy megfelelő méretű címtartománnyal kell hello virtuális gépek. Alapértelmezés szerint olyan méretezési overprovisions (hoz létre további virtuális gépek központi telepítéskor vagy kiterjesztése, amely nem a van szó) tooimprove telepítési megbízhatóságát és teljesítményét. Lehetővé teszi egy cím lemezterület 20 % nagyobb, mint a virtuális gépek azt tervezi, hogy tooscale hello száma.
- Ha azt tervezi, toodeploy sok virtuális gép, a számítási core kvótakorlát esetleg toobe nőtt.
- A tartalék tartományok és a frissítési tartományok csak az elhelyezési csoporton belül konzisztensek. Ez az architektúra nem változtatja meg a hello teljes méretű rendelkezésre állási csoportban, virtuális gépek különböző fizikai hardver egyenlően elosztott, akkor viszont azt jelenti, hogy ha két virtuális gépek vannak a másik hardverekhez, tooguarantee kell gondoskodjon arról, hogy azok különböző hiba a tartományok hello elhelyezési ugyanabban a csoportban. Tartalék tartomány és elhelyezési Csoportazonosító látható hello _nézet példány_ a skála beállítása a virtuális gép. A Virtuálisgép-méretezési csoport példányait tartalmazó nézetet hello megtekintheti a hello [Azure erőforrás-kezelő](https://resources.azure.com/).


## <a name="creating-a-large-scale-set"></a>Nagyméretű méretezési csoport létrehozása
Amikor létrehoz egy méretezési hello Azure-portálon a készletben, engedélyezheti annak tooscale toomultiple elhelyezési csoportok által hello beállítása _korlát tooa egyetlen elhelyezési csoport_ beállítás too_False_ a hello _alapjai_ panelen. Ez a beállítás set too_False_ megadhatja egy _száma példány_ too1, 000 magasabb érték esetén a.

![](./media/virtual-machine-scale-sets-placement-groups/portal-large-scale.png)

Létrehozhat egy nagy Virtuálisgép-méretezési hello segítségével [Azure CLI](https://github.com/Azure/azure-cli) _az vmss létrehozása_ parancsot. Ez a parancs beállítja az intelligens alapértelmezett beállításokat, például az alhálózat méretét hello alapján _példányszám_ argumentum:

```bash
az group create -l southcentralus -n biginfra
az vmss create -g biginfra -n bigvmss --image ubuntults --instance-count 1000
```
Vegye figyelembe, hogy hello _vmss létrehozása_ parancs bizonyos konfigurációs értékeket alapértelmezés szerint, ha nem adja meg azokat. toosee hello elérhető beállításokat lehet felülbírálni, próbálja meg:
```bash
az vmss create --help
```

Ha állítja be az Azure Resource Manager-sablon létrehozása nagy méretű hoz létre, győződjön meg arról hello sablon Azure felügyelt lemezek alapuló méretezési készletet hoz létre. Beállíthatja a hello _singlePlacementGroup_ tulajdonság too_false_ a hello _tulajdonságok_ hello szakasza _Microsoft.Compute/virtualMAchineScaleSets_ erőforrás. hello következő JSON-töredéket mutatja egy méretezési sablon beállítása, beleértve a hello 1000 Virtuálisgép-kapacitást és hello hello elejére _"singlePlacementGroup": hamis_ beállítást:
```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "location": "australiaeast",
  "name": "bigvmss",
  "sku": {
    "name": "Standard_DS1_v2",
    "tier": "Standard",
    "capacity": 1000
  },
  "properties": {
    "singlePlacementGroup": false,
    "upgradePolicy": {
      "mode": "Automatic"
    }
```
Túl nagy méretű átfogó példát a sablon beállítása tudnivalókat[https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json](https://github.com/gbowerman/azure-myriad/blob/master/bigtest/bigbottle.json).

## <a name="converting-an-existing-scale-set-toospan-multiple-placement-groups"></a>Egy meglévő méretezési konvertálása beállítása toospan több elhelyezési csoportok
egy meglévő Virtuálisgép-méretezési készlet képes mint 100 virtuális gépek toomore skálázás toomake, kell toochange hello _singplePlacementGroup_ tulajdonság too_false_ hello méretezési modell beállítása. Tesztelheti a hello e tulajdonság módosítása [Azure erőforrás-kezelő](https://resources.azure.com/). Méretezési készlet, jelölje be található _szerkesztése_ , és módosítsa a hello _singlePlacementGroup_ tulajdonság. Ha nem látja ezt a tulajdonságot, akkor lehetséges, hogy látja hello méretezési készletben hello Microsoft.Compute API egy régebbi verziója.

>[!NOTE] 
Módosíthatja a skála állítható be egy egyetlen elhelyezési csoport egyetlen (hello alapértelmezett viselkedés) tooa több elhelyezési csoportok támogató támogató, de hello megfordítva már nem lehet konvertálni. Ezért tudja, hogy hello tulajdonságok nagy méretű készlet átalakítás előtt. Különösen ellenőrizze, hogy nem kell hello Azure terheléselosztó a terheléselosztási réteg-4.



---
title: "a Linux virtuális gépek az Azure-ban a parancssori felület 2.0 kép aaaCapture |} Microsoft Docs"
description: "Rögzítsen egy rendszerképet az Azure virtuális gép toouse hello Azure CLI 2.0 használatával tömeges telepítésekhez."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9558332a86186b282775097428df462709373012
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-image-of-a-virtual-machine-or-vhd"></a>Hogyan toocreate egy virtuális géphez vagy virtuális merevlemez képe

<!-- generalize, image - extended version of hello tutorial-->

toocreate több példányát az Azure, a virtuális gép (VM) toouse rögzítsen egy rendszerképet virtuális gép hello, vagy az operációs rendszer virtuális merevlemez hello. toocreate kép, meg kell, törölje a személyes fiók adatait, így a biztonságosabb toodeploy többször. Az alábbi lépésekkel hello meg kiosztásának megszüntetése a meglévő virtuális, felszabadítása és lemezkép létrehozása. A kép toocreate virtuális gépek közötti bármely erőforráscsoport használhatja az előfizetésen belül.

Ha szeretné, hogy a meglévő Linux virtuális gép egy másolatát toocreate biztonsági mentését vagy hibakeresés, vagy egy helyszíni virtuális gép speciális Linux virtuális merevlemez feltöltése, lásd: [feltöltése és a Linux virtuális gép létrehozása az egyéni lemezképet](upload-vhd.md).  

Is **csomagoló** toocreate az egyéni konfiguráció. Csomagoló használatával kapcsolatos további információkért lásd: [hogyan toouse csomagoló toocreate Linux virtuális gép az Azure-ban lemezképek](build-image-with-packer.md).


## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy teljesülnek-e a következő előfeltételek hello:

* Egy hello Resource Manager üzembe helyezési modellel felügyelt lemezekkel létrehozott Azure virtuális Gépre van szüksége. Ha még nem hozott létre egy Linux virtuális Gépet, használhatja a hello [portal](quick-create-portal.md), hello [Azure CLI](quick-create-cli.md), vagy [Resource Manager-sablonok](create-ssh-secured-vm-from-template.md). Virtuális gép szükség szerint hello konfigurálása. Például [adatok lemezek hozzáadása a](add-disk.md), frissítések és alkalmazások telepítéséhez. 

* Szükség toohave hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve és tooan Azure-fiókot használó rögzítheti [az bejelentkezési](/cli/azure/#login).

## <a name="quick-commands"></a>Gyors parancsok

Egy egyszerűsített verziója, tesztelési, ez a témakör értékelése és megismerése az virtuális gépeket az Azure-ban, olvassa el [hozzon létre egy Azure virtuális gép hello parancssori felület használatával egyéni lemezkép](tutorial-custom-images.md).


## <a name="step-1-deprovision-hello-vm"></a>1. lépés: Virtuális gép hello kiosztásának megszüntetése
Ön kiosztásának megszüntetése hello virtuális gépet a hello Azure Virtuálisgép-ügynök, toodelete gép meghatározott fájlokat és adatokat. Használjon hello `waagent` hello parancsot *-deprovision + felhasználói* paraméter a Linux virtuális gép – forrásként. További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](../windows/agent-user-guide.md).

1. Csatlakozás tooyour Linux virtuális gép SSH-ügyfél.
2. Hello SSH ablakban írja be a következő parancs hello:
   
    ```bash
    sudo waagent -deprovision+user
    ```
<br>
   > [!NOTE]
   > A parancs egy virtuális gépen, hogy szeretné-e képként toocapture csak fut. Hello lemezkép törlődik a bizalmas információk, vagy alkalmas terjesztési nem garantálja. Hello *+ felhasználói* paraméter is eltávolítja a hello utolsó kiépített felhasználói fiókot. Ha azt szeretné, hogy a virtuális gép hello tookeep fiók hitelesítő adatait, használja *-deprovision* tooleave hello felhasználói fiók helyen.
 
3. Típus **y** toocontinue. Hozzáadhat hello **-force** paraméter tooavoid a megerősítési lépés.
4. Hello parancs után írja be a **kilépéshez**. Ez a lépés hello SSH-ügyfél bezárása után.

## <a name="step-2-create-vm-image"></a>2. lépés: A Virtuálisgép-lemezkép létrehozása
Általános hello Azure CLI 2.0 toomark hello virtuális gép használja, és hello lemezképének. Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők *myResourceGroup*, *myVnet*, és *myVM*.

1. Felszabadítani a virtuális Gépet, amely az Ön platformelőfizetés hello [az virtuális gép felszabadítása](/cli//azure/vm#deallocate). hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:
   
    ```azurecli
    az vm deallocate \
      --resource-group myResourceGroup \
      --name myVM
    ```

2. Megjelölés hello módon rendelkező általánosított virtuális gép [az vm generalize](/cli//azure/vm#generalize). Példa jelek hello hello VM nevű következő hello *myVM* nevű hello erőforráscsoportban *myResourceGroup* , általánosítva van:
   
    ```azurecli
    az vm generalize \
      --resource-group myResourceGroup \
      --name myVM
    ```

3. Most létrehoz egy képet hello VM erőforrás [az lemezkép létrehozása](/cli//azure/image#create). hello alábbi példakód létrehozza nevű kép *myImage* nevű hello erőforráscsoportban *myResourceGroup* nevű VM erőforrás hello segítségével *myVM*:
   
    ```azurecli
    az image create \
      --resource-group myResourceGroup \
      --name myImage --source myVM
    ```
   
   > [!NOTE]
   > hello lemezkép létrehozása hello azonos erőforráscsoporthoz tartozik, mint a forrás virtuális gép. Bármely erőforráscsoport virtuális gépeket hozhat létre a lemezképből az előfizetésen belül. Felügyeleti szempontból Kezdésként toocreate egy adott erőforráscsoportban található a Virtuálisgép-erőforrások és a képeket.

## <a name="step-3-create-a-vm-from-hello-captured-image"></a>3. lépés: Virtuális gép létrehozása hello rögzített lemezképből
Hello lemezkép segítségével létrehozott virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create). hello alábbi példakód létrehozza a virtuális gépek nevű *myVMDeployed* nevű hello lemezképből *myImage*:

```azurecli
az vm create \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --image myImage\
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```

### <a name="creating-hello-vm-in-another-resource-group"></a>Egy másik erőforráscsoportban található hello virtuális gép létrehozása 

Bármely erőforráscsoport lemezkép virtuális gépeket hozhat létre az előfizetésen belül. egy virtuális Gépet egy másik erőforráscsoportban található, mint hello lemezkép toocreate adja meg a hello teljes erőforrás azonosítója tooyour lemezképet. Használjon [az képlistában](/cli/azure/image#list) tooview lemezképek listáját. hello hasonló toohello a következő példa a kimenetre:

```json
"id": "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage",
   "location": "westus",
   "name": "myImage",
```

hello alábbi példában [az virtuális gép létrehozása](/cli/azure/vm#create) toocreate egy virtuális Gépet egy másik erőforráscsoportban található, mint hello forrás lemezkép hello képerőforrás-azonosító megadásával:

```azurecli
az vm create \
   --resource-group myOtherResourceGroup \
   --name myOtherVMDeployed \
   --image "/subscriptions/guid/resourceGroups/MYRESOURCEGROUP/providers/Microsoft.Compute/images/myImage" \
   --admin-username azureuser \
   --ssh-key-value ~/.ssh/id_rsa.pub
```


## <a name="step-4-verify-hello-deployment"></a>4. lépés: Hello telepítés ellenőrzése

Most SSH toohello virtuális gép tooverify hello üzembe helyezési és kezdő használatával létrehozott új virtuális gép hello. ssh, tooconnect található hello IP-cím vagy FQDN-jét a virtuális Gépet a [az vm megjelenítése](/cli/azure/vm#show):

```azurecli
az vm show \
   --resource-group myResourceGroup \
   --name myVMDeployed \
   --show-details
```

## <a name="next-steps"></a>Következő lépések
A forrás Virtuálisgép-lemezkép létrehozhat több virtuális géphez. Ha toomake módosítások tooyour kép lesz szüksége: 

- Hozzon létre egy virtuális Gépet a lemezképből.
- Győződjön meg a szükséges frissítések vagy a konfigurációs módosításokat.
- Hajtsa végre a hello lépések újra toodeprovision, felszabadítani, generalize és lemezkép létrehozása.
- Az új kép használata a jövőben a központi telepítésekre. Ha szükséges, hello eredeti lemezkép törlése.

A virtuális gépek hello CLI kezeléséről további információkért lásd: [Azure CLI 2.0](/cli/azure/overview).

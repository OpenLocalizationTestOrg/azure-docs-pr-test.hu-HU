---
title: "egyéni VM képeket aaaCreate hello Azure parancssori Felülettel |} Microsoft Docs"
description: "Útmutató – hozzon létre egy egyéni Virtuálisgép-lemezkép hello Azure parancssori felület használatával."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/21/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 217a993c0c1d48939b74108ac6c5f7a1a619416c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-hello-cli"></a>Hozzon létre egy Azure virtuális gép hello parancssori felület használatával egyéni képe

Egyéni lemezképek piactéren elérhető rendszerkép hasonló, de Ön hozza létre őket. Egyéni lemezképek lehet például az alkalmazások, alkalmazás, és más operációs rendszer konfigurációjában kerüli használt toobootstrap konfigurációkat. Ebben az oktatóanyagban létrehoz egy Azure virtuális gép saját egyéni rendszerképét. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Kiosztásának megszüntetése és virtuális gépek generalize
> * Egyéni lemezkép létrehozása
> * Virtuális gép létrehozása egy egyéni lemezképből
> * Az előfizetésében szereplő összes hello lemezképek felsorolása
> * Lemezkép törlése


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="before-you-begin"></a>Előkészületek

hello lépéseket részletesen tootake egy meglévő virtuális Gépre, és azt újra felhasználható egyéni lemezképet, hogy kapcsolja használatát toocreate új Virtuálisgép-példányok.

Ebben az oktatóanyagban toocomplete hello példában rendelkeznie kell egy meglévő virtuális gépet. Ha szükséges, ez [parancsfájl minta](../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md) hozhat létre egyet. Ha feldolgozása révén hello oktatóanyag, a csere hello erőforráscsoport és a virtuális gép nevét, ha szükséges.

## <a name="create-a-custom-image"></a>Egyéni lemezkép létrehozása

toocreate egy virtuális gép lemezképét, meg kell tooprepare hello VM megszüntetés, felszabadítása és hello forrás virtuális gép általánosítva van, majd jelölést. Egyszer hello a virtuális gép előkészített, hozhat létre kép.

### <a name="deprovision-hello-vm"></a>Virtuális gép hello kiosztásának megszüntetése 

Megszüntetés használatúvá hello VM számítógép-specifikus adatok eltávolításával. Ez általánosítása révén lehetséges toodeploy sok virtuális gép egyetlen lemezképéről. Megszüntetés, során hello állomásnév alaphelyzetbe áll túl*localhost.localdomain*. SSH-állomások kulcsait, a névkiszolgáló-konfigurációk, a gyökér szintű jelszó és a gyorsítótárazott DHCP-bérletek is törlődnek.

virtuális gép, toodeprovision hello hello Azure Virtuálisgép-ügynök (waagent) használja. hello Azure Virtuálisgép-ügynök hello virtuális gép telepítve van, és kezeli az üzembe helyezési és hello Azure Fabric Controller való interakció. További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](agent-user-guide.md).

Csatlakozás a virtuális gép tooyour SSH és futtatási hello parancs toodeprovision hello VM használatával. A hello `+user` argumentum, hello utolsó kiépített felhasználói fiók és minden társított adatok is törlődnek. Hello példa IP-cím cserélje le a virtuális gép hello nyilvános IP-címe.

SSH toohello virtuális gép.
```bash
ssh azureuser@52.174.34.95
```
Deprovision hello virtuális gép.

```bash
sudo waagent -deprovision+user -force
```
Zárja be a hello SSH-munkamenetet.

```bash
exit
```

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a>Felszabadítani, és jelölje be a virtuális gép általánosítva, hello

toocreate kép, hello virtuális gép felszabadítása toobe kell. Hello VM használatával felszabadítani [az virtuális gép felszabadítása](/cli//azure/vm#deallocate). 
   
```azurecli-interactive 
az vm deallocate --resource-group myResourceGroup --name myVM
```

Végezetül beállítani hello VM állapotának hello rendelkező általánosított [az vm generalize](/cli//azure/vm#generalize) , hello Azure platformon tudja hello VM már általánosítva lett. Kép csak létrehozhat egy általánosított virtuális Gépet.
   
```azurecli-interactive 
az vm generalize --resource-group myResourceGroup --name myVM
```

### <a name="create-hello-image"></a>Hello lemezkép létrehozása

Most használatával hozhat létre virtuális gép hello képe [az lemezkép létrehozása](/cli//azure/image#create). hello alábbi példakód létrehozza nevű kép *myImage* nevű VM *myVM*.
   
```azurecli-interactive 
az image create \
    --resource-group myResourceGroup \
    --name myImage \
    --source myVM
```
 
## <a name="create-vms-from-hello-image"></a>Hozzon létre a virtuális gépek hello lemezképből

Most, hogy egy lemezképet, létrehozhat egy vagy több új virtuális gépek hello lemezkép használatával [az virtuális gép létrehozása](/cli/azure/vm#create). hello alábbi példakód létrehozza a virtuális gépek nevű *myVMfromImage* nevű hello lemezképből *myImage*.

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVMfromImage \
    --image myImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

## <a name="image-management"></a>Lemezkép-kezelési 

Az alábbiakban néhány olyan gyakori lemezkép-kezelési feladatok és hogyan toocomplete őket hello Azure parancssori felület használatával.

Lista összes lemezkép nevű táblázatos formátumban.

```azurecli-interactive 
az image list \
  --resource-group myResourceGroup
```

Lemezkép törlése. Ez a példa törlések hello nevű kép *myOldImage* a hello *myResourceGroup*.

```azurecli-interactive 
az image delete \
    --name myOldImage \
    --resource-group myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban létre egyéni Virtuálisgép-lemezképet. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Kiosztásának megszüntetése és virtuális gépek generalize
> * Egyéni lemezkép létrehozása
> * Virtuális gép létrehozása egy egyéni lemezképből
> * Az előfizetésében szereplő összes hello lemezképek felsorolása
> * Lemezkép törlése

Előzetes toohello oktatóanyag következő toolearn kapcsolatos magas rendelkezésre állású virtuális gépeket.

> [!div class="nextstepaction"]
> [Hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).


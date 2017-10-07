---
title: "aaaDifferent módon toocreate egy Linux virtuális Gépet az Azure-ban |} A Microsoft Azure"
description: "Ismerje meg, hogy hello különböző módokon toocreate egy Linux virtuális gépet az Azure, beleértve a hivatkozások tootools és az egyes módszerek oktatóanyagok."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 250e92c063c87a8c1279097dc2264777d95478d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-linux-vm"></a>Különböző módokon toocreate Linux virtuális gép
Hello beleszólása van az Azure toocreate Linux virtuális gép (VM) eszközök és a munkafolyamatok kényelmes tooyou. Ez a cikk ezeket különbségek és a Linux virtuális gépeken, beleértve az Azure CLI 2.0 hello létrehozására vonatkozó példákat foglalja össze. Megtekintheti többek között a hello létrehozási lehetőségek [Azure CLI 1.0](creation-choices-nodejs.md).

Hello [Azure CLI 2.0](/cli/azure/install-az-cli2) érhető el több platformon keresztül az npm-csomagot, a megadott distro csomagok és a Docker-tároló. Hello a környezetnek leginkább megfelelő build telepítése, és jelentkezzen be tooan Azure-fiók használatával [az bejelentkezés](/cli/azure/#login)

* [Hozzon létre egy Linux virtuális gép hello Azure CLI 2.0](quick-create-cli.md)
  
  * Hozzon létre egy *myResourceGroup* nevű erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * A virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) nevű *myVM* legújabb hello segítségével *UbuntuLTS* rendszerképet, az SSH-kulcsok létrehozása, ha még nem léteznek a *~/.ssh*:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Linux virtuális gép létrehozása Azure-sablon alapján](create-ssh-secured-vm-from-template.md)
  
  * hello alábbi példában [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) toocreate egy virtuális Gépet a Githubon tárolt sablonból:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Linux virtuális gép létrehozása és testreszabása a cloud-init paranccsal](tutorial-automate-vm-deployment.md)

* [Elosztott terhelésű és magas rendelkezésre állású alkalmazás létrehozása több Linux virtuális gépen](tutorial-load-balancer.md)


## <a name="azure-portal"></a>Azure Portal
Hello [Azure-portálon](https://portal.azure.com) lehetővé teszi a tooquickly virtuális gép létrehozása, mivel nincs szükség a rendszeren tooinstall. Hello Azure portál toocreate hello VM használja:

* [Linux virtuális gép létrehozása Azure-portálon hello](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>Választható operációs rendszerek és rendszerképek
Virtuális gép létrehozásakor ki kell választania egy operációs rendszer kívánt toorun hello alapuló rendszerképet. Az Azure és a partnerei számos rendszerképet kínálnak, amelyek némelyike előre telepített alkalmazásokat és eszközöket tartalmaz. Vagy a feltöltött egyik saját rendszerképét (lásd: [szakasz következő hello](#use-your-own-image)).

### <a name="azure-images"></a>Azure-rendszerképek
Használjon hello [az virtuálisgép-lemezkép](/cli/azure/vm/image) elérhető közzétevő, distro kiadás és buildek toosee parancsokat.

Az elérhető közzétevők listázása:

```azurecli
az vm image list-publishers --location eastus
```

Egy adott közzétevő elérhető termékeinek (ajánlatainak) listázása:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Egy adott ajánlat elérhető termékváltozatainak (disztribúciós kiadásainak) listázása:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Egy adott kiadás összes elérhető rendszerképének listázása:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Keresse meg, és elérhető rendszerkép használatával további példákért lásd [keresse meg és válassza ki azokat az Azure CLI hello Azure virtuális gép lemezképeket](cli-ps-findimage.md).

Hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsában aliasok vannak használhatja tooquickly hozzáférés hello gyakori disztribúciókkal és a legújabb kiadást. Használjon olyan aliasneveket gyakran gyorsabb, mint hello közzétevő, az ajánlat, SKU és verzió megadása minden alkalommal, amikor a virtuális gép létrehozása:

| Alias | Közzétevő | Ajánlat | SKU | Verzió |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |legújabb |
| CoreOS |CoreOS |CoreOS |Stable |legújabb |
| Debian |credativ |Debian |8 |legújabb |
| openSUSE |SUSE |openSUSE |13.2 |legújabb |
| RHEL |Redhat |RHEL |7.2 |legújabb |
| SLES |SLES |SLES |12-SP1 |legújabb |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |legújabb |

### <a name="use-your-own-image"></a>Saját rendszerkép használata
Ha speciális egyéni beállításokra van szüksége, használhat egy meglévő Azure virtuális gépen alapuló rendszerképet a virtuális gép rögzítésével. Emellett feltölthet egy helyszínen létrehozott rendszerképet is. További információ a támogatott disztribúciókkal és hogyan toouse a saját lemezképek: hello következő cikkeket:

* [Azure által támogatott disztribúciók](endorsed-distros.md)
* [Nem támogatott disztribúciókkal kapcsolatos tudnivalók](create-upload-generic.md)
* [Hogyan toocreate az egy meglévő Azure virtuális gép lemezképének](tutorial-custom-images.md).
  
  * Gyors üzembe helyezési példa parancsok toocreate az egy meglévő Azure virtuális gép lemezképét:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Következő lépések
* Hozzon létre egy Linux virtuális gép hello [CLI](quick-create-cli.md), a hello [portal](quick-create-portal.md), vagy egy [Azure Resource Manager sablon](../windows/cli-deploy-templates.md).
* A Linux virtuális gép létrehozása után [ismerje meg az Azure lemezeket és tárolót](tutorial-manage-disks.md).
* Gyors lépések túl[jelszó vagy SSH-kulcsok alaphelyzetbe és felhasználók kezeléséhez](using-vmaccess-extension.md).

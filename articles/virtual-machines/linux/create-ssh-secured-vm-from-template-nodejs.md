---
title: "Linux virtuális gépet egy Azure-sablon az Azure CLI 1.0 aaaCreate |} Microsoft Docs"
description: "Linux virtuális gép létrehozása az Azure hello Azure CLI 1.0 és az Azure Resource Manager-sablon használatával."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b694cc8247a8431b7ef4b24cc7dc2b4cdb9660ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-vm-using-hello-azure-cli-10-an-azure-resource-manager-template"></a>Hogyan toocreate a Linux virtuális gép hello Azure CLI 1.0 Azure Resource Manager-sablonok
Ez a cikk bemutatja, hogyan tooquickly hello Azure CLI 1.0 és az Azure Resource Manager-sablon használatával Linux virtuális gép telepítése. hello cikk van szükség:

* egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),
* Hello [Azure CLI 1.0](../../cli-install-nodejs.md) bejelentkezett `azure login`.
* az Azure parancssori felület hello *kell* Azure Resource Manager módra `azure config mode arm`.

A Linux Virtuálisgép-sablonok hello segítségével is gyorsan telepíthet [Azure-portálon](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#quick-command-summary) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](create-ssh-secured-vm-from-template.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="quick-command-summary"></a>Gyors parancsösszegzés
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Részletes bemutató
A sablonok segítségével toocreate virtuális gépek Azure-on, amelyet az toocustomize során hello indítási, beállítások, például felhasználónevekkel és állomásnevekkel beállításokkal. Ebben a cikkben elindítunk egy Ubuntu virtuális gépet használó Azure-sablont, valamint egy hálózati biztonsági csoportot (NSG-t), amelynek a 22-es portja nyitva van az SSH számára.

Az Azure Resource Manager-sablonok JSON-fájlok, amelyek egyszerű, egyszeri feladatokhoz használhatók, például egy Ubuntu virtuális gép indításához, mint ebben a cikkben.  Azure-sablonok is használt tooconstruct összetett Azure-konfigurációjának olyan teljes környezetek tesztelési, fejlesztési és éles telepítési verem.

## <a name="create-hello-linux-vm"></a>Hello Linux virtuális gép létrehozása
Hogyan kód példa azt mutatja meg a következő hello toocall `azure group create` toocreate erőforrás csoport és az SSH által biztonságossá tett Linux virtuális gép egyidejű hello használatával [Azure Resource Manager sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Ne feledje, hogy a példában toouse nevet, amely egyedi tooyour környezetben van szüksége. Ez a példa *myResourceGroup* hello az erőforráscsoport neve, mint és *myVM* hello virtuális gép neve.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

hello kimenete a következő kimeneti blokk hello kell hasonlítania:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for hello following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

A példában a virtuális gépek hello telepített `--template-uri` paraméter.  Is letöltheti vagy létrehozhat egy sablont helyben és hello sablon használatával hello átadni `--template-file` argumentumként elérési toohello sablonfájlt paraméterrel. az Azure parancssori felület hello kéri hello paraméterek hello sablonhoz szükséges.

## <a name="next-steps"></a>Következő lépések
Keresési hello [sablontárban](https://azure.microsoft.com/documentation/templates/) toodiscover milyen alkalmazás-keretrendszerek toodeploy tovább.


---
title: "aaaCreate Azure Linux virtuális gép sablon alapján |} Microsoft Docs"
description: "Hogyan toouse hello Azure CLI 2.0 toocreate Linux virtuális gép Resource Manager sablon alapján"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4b8c4103cccbae13c679ff2a2cac928a0e8e809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Hogyan toocreate egy Linux virtuális gép Azure Resource Manager-sablonok
Ez a cikk bemutatja, hogyan telepítsék központilag az tooquickly egy Linux virtuális gép (VM) az Azure Resource Manager-sablonok és hello Azure CLI 2.0. Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Sablonok – áttekintés
Az Azure Resource Manager-sablonok JSON-fájlokat, amelyek meghatározzák a hello infrastruktúráját és konfigurációját az Azure-megoldás. A sablonok segítségével a megoldás a teljes életciklusa során ismételten üzembe helyezhető, és az erőforrások üzembe helyezése biztosan konzisztens lesz. További információ az hello sablont, és hogyan hozható létre, hello formátuma toolearn lásd: [az első Azure Resource Manager-sablon létrehozása](../../azure-resource-manager/resource-manager-create-first-template.md). tooview hello JSON-szintaxis erőforrások esetében, tekintse meg [meghatározhatja az erőforrásokat az Azure Resource Manager sablonokban](/azure/templates/).


## <a name="create-resource-group"></a>Erőforráscsoport létrehozása
Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupVM* a hello *eastus* régió:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása
hello alábbi példa létrehoz egy virtuális gép [Azure Resource Manager sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) rendelkező [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create). Adja meg a saját nyilvános SSH-kulcs, például a hello tartalmát hello értékének *~/.ssh/id_rsa.pub*. Ha egy SSH-kulcspárral toocreate van szüksége, tekintse meg [hogyan toocreate és -felhasználási SSH kulcs pár Linux virtuális gépek Azure-ban](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

Ebben a példában a Githubon tárolt sablon adott meg. Is letöltheti vagy -sablon létrehozása és hello hello helyi elérési utat adjon meg azonos `--template-file` paraméter.

tooSSH tooyour VM, hello nyilvános IP-cím az beszerzése [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show):

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

Ezek közül SSH tooyour VM szokásos módon:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Következő lépések
Ebben a példában létrehozott egy alapszintű Linux virtuális Gépet. További Resource Manager-sablonok, amelyek tartalmazzák az alkalmazás-keretrendszerek számára, vagy hozzon létre összetettebb környezetben, tallózással hello [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/).

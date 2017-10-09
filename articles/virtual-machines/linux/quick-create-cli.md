---
title: "gyors üzembe helyezés – aaaAzure hozzon létre virtuális gép CLI |} Microsoft Docs"
description: "Ismerje meg gyorsan, toocreate virtuális gépek hello Azure parancssori felület."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/14/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 0d9c686e2f4c7339b29b8c43cd1dd9ee56d7dc6a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-hello-azure-cli"></a>Hozzon létre egy Linux virtuális gép hello Azure parancssori felület

hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez az útmutató adatokat hello Azure CLI toodeploy Ubuntu server operációs rendszert futtató virtuális gépek használata. Miután hello kiszolgáló van telepítve, az SSH-kapcsolat jön létre, és egy NGINX-webkiszolgálón telepítve van.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. 

hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* és SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye. toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.  

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg. Jegyezze fel a hello `publicIpAddress`. Ez a cím használt tooaccess hello virtuális gép.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>A 80-as port megnyitása a webes adatforgalom számára 

Alapértelmezés szerint kizárólag SSH-kapcsolatok engedélyezettek az Azure-ban üzembe helyezett, Linux rendszerű virtuális gépeken. Ha a virtuális gép lesz egy webkiszolgáló toobe, kell tooopen hello Internet a 80-as porton. Használjon hello [az vm-port megnyitása](/cli/azure/vm#open-port) parancs tooopen hello port szükséges.  
 
 ```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>Bejelentkezés a virtuális gépre SSH-val

Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet. Győződjön meg arról, hogy tooreplace  *<publicIpAddress>*  hello megfelelő nyilvános IP-címet a virtuális gép.  A fenti példában az IP-cím a következő volt: *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>Az NGINX telepítése

Használjon hello alábbi parancsfájl tooupdate csomag források bash, és hello legújabb NGINX csomag telepítése. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-hello-nginx-welcome-page"></a>Hello NGINX üdvözlőlap megtekintése

Telepített NGINX és a virtuális gépre, az Internet - hello most nyissa meg a 80-as port az a választott tooview hello alapértelmezett NGINX üdvözlőlap webböngésző is használhatja. Lehet, hogy toouse hello *publicIpAddress* toovisit hello alapértelmezett oldal fent részletes ismertetését lásd. 

![Alapértelmezett NGINX-webhely](./media/quick-create-cli/nginx.png) 


## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót. További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Linux virtuális gépek továbbra is.


> [!div class="nextstepaction"]
> [Azure-beli Linux rendszerű virtuális gépek – oktatóanyag](./tutorial-manage-vm.md)

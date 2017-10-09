---
title: "gyors üzembe helyezés – aaaAzure létrehozása a Windows virtuális gép parancssori Felülettel |} Microsoft Docs"
description: "A Windows hello Azure CLI virtuális gépek gyors megtudhatja, toocreate."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 029bdecec219b12b80b958ceeedda214f1b13149
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-cli"></a>Hozzon létre egy Windows rendszerű virtuális gép hello Azure parancssori felület

hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez az útmutató adatokat hello Azure CLI toodeploy Windows Server 2016 rendszert futtató virtuális gépek használata. Központi telepítés befejezése után azt csatlakoztassa toohello a kiszolgálót, és telepítse az IIS.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal. Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal. 

hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*. Ez a példa *azureuser* egy rendszergazda felhasználó neve és *myPassword12* hello jelszóként. Ezen értékek toosomething megfelelő tooyour környezet frissítése. Ezeket az értékeket a kapcsolat létrehozásakor hello virtuális géppel van szükség.

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg. Jegyezze fel a hello `publicIpAaddress`. Ez a cím használt tooaccess hello virtuális gép.

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>A 80-as port megnyitása a webes adatforgalom számára 

Alapértelmezés szerint csak az RDP-kapcsolatok tooWindows virtuális gépek Azure szolgáltatásba telepített engedélyezettek. Ha a virtuális gép lesz egy webkiszolgáló toobe, kell tooopen hello Internet a 80-as porton. Használjon hello [az vm-port megnyitása](/cli/azure/vm#open-port) parancs tooopen hello port szükséges.  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-toovirtual-machine"></a>Csatlakoztassa a gépet toovirtual

Használjon hello következő parancsot a toocreate egy távoli asztali munkamenet hello virtuális géppel. Hello IP-cím cserélje le a virtuális gép hello nyilvános IP-címe. Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a>Az IIS telepítése a PowerShell-lel

Most, hogy az Azure virtuális gép toohello már bejelentkezett, használjon PowerShell tooinstall IIS egysoros, és hello helyi tűzfal szabály tooallow webes forgalom engedélyezése. Nyisson meg egy PowerShell-parancssorba, és futtassa a következő parancs hello:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a>Nézet hello IIS-kezdőlap

A telepített IIS-t, és most nyissa meg a virtuális gép hello Internet a 80-as porton az a choice tooview hello alapértelmezett IIS üdvözlőlap webböngésző is használhatja. Lehet, hogy toouse hello nyilvános IP-cím feletti toovisit hello alapértelmezett oldal részletes ismertetését lásd. 

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Az erőforrások eltávolítása

Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót. További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Windows virtuális gépek továbbra is.

> [!div class="nextstepaction"]
> [Windowsos virtuális gépek az Azure-ban – oktatóanyagok](./tutorial-manage-vm.md)

---
title: "aaaCreate virtuális gépeket a DevTest Labs szolgáltatásban Azure parancssori felülettel |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure DevTest Labs toocreate virtuális gépeket az Azure CLI 2.0"
services: devtest-lab,virtual-machines
documentationcenter: na
author: lisawong19
manager: douge
editor: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: liwong
ms.openlocfilehash: 98ea3aa7b2489bff61971573aaf584984cd811e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-hello-azure-cli"></a>Hozzon létre és virtuális gépek kezelése a DevTest Labs hello Azure parancssori felület használatával
A gyors üzembe helyezési végigvezeti keresztül létrehozása, elindítása, csatlakozás, frissítése és törléséről a fejlesztési számítógépén a tesztkörnyezetben. 

Előzetes teendők

* Ha a labor nem lett létrehozva, található utasításokat [Itt](devtest-lab-create-lab.md).

* [CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli). toostart, futtassa az bejelentkezési toocreate Azure kapcsolatot. 

## <a name="create-and-verify-hello-virtual-machine"></a>Hozzon létre, és ellenőrizze a virtuális gép hello 
Hozzon létre egy virtuális gép egy Piactéri rendszerképből az ssh hitelesítés.
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> PUT hello **labor tartozó erőforráscsoport** hello--erőforráscsoport paraméter neve.
>

Toocreate a virtuális gépek képlet, használja hello – a képlet paraméter [az tesztlabor virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/lab/vm#create).


Győződjön meg arról, hogy hello VM érhető el.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand 'properties($expand=ComputeVm,NetworkInterface)' --query '{status: computeVm.statuses[0].displayStatus, fqdn: fqdn, ipAddress: networkInterface.publicIpAddress}'
```
```json
{
  "fqdn": "lisalabvm.southcentralus.cloudapp.azure.com",
  "ipAddress": "13.85.228.112",
  "status": "Provisioning succeeded"
}
```

## <a name="start-and-connect-toohello-virtual-machine"></a>Indítsa el, és csatlakoztassa toohello virtuális gépet
Indítsa el a virtuális Gépet.
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> PUT hello **labor tartozó erőforráscsoport** hello--erőforráscsoport paraméter neve.
>

Csatlakozás a virtuális gép tooa: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) vagy [távoli asztal](../virtual-machines/windows/connect-logon.md).
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-hello-virtual-machine"></a>Hello virtuális gép frissítése
Az összetevők tooa VM alkalmazni.
```azurecli
az lab vm apply-artifacts --lab-name  sampleLabName --name sampleVMName  --resource-group sampleResourceGroup  --artifacts @/artifacts.json
```

```json
[
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-java",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-install-nodejs",
    "parameters": []
  },
  {
    "artifactId": "/artifactSources/public repo/artifacts/linux-apt-package",
    "parameters": [
      {
        "name": "packages",
        "value": "abcd"
      },
      {
        "name": "update",
        "value": "true"
      },
      {
        "name": "options",
        "value": ""
      }
    ]
  } 
]
```

Lista összetevők hello tesztkörnyezet érhető el.
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-hello-virtual-machine"></a>Állítsa le és törölje a virtuális gép hello    
Állítsa le a virtuális Gépet.
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

A virtuális gép törlése.
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
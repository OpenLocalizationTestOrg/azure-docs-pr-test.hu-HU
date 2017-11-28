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
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-hello-azure-cli"></a><span data-ttu-id="0e277-103">Hozzon létre és virtuális gépek kezelése a DevTest Labs hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="0e277-103">Create and manage virtual machines with DevTest Labs using hello Azure CLI</span></span>
<span data-ttu-id="0e277-104">A gyors üzembe helyezési végigvezeti keresztül létrehozása, elindítása, csatlakozás, frissítése és törléséről a fejlesztési számítógépén a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0e277-104">This quick start will guide you through creating, starting, connecting, updating and cleaning up a development machine in your lab.</span></span> 

<span data-ttu-id="0e277-105">Előzetes teendők</span><span class="sxs-lookup"><span data-stu-id="0e277-105">Before you begin:</span></span>

* <span data-ttu-id="0e277-106">Ha a labor nem lett létrehozva, található utasításokat [Itt](devtest-lab-create-lab.md).</span><span class="sxs-lookup"><span data-stu-id="0e277-106">If a lab has not been created, instructions can be found [here](devtest-lab-create-lab.md).</span></span>

* <span data-ttu-id="0e277-107">[CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="0e277-107">[Install CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="0e277-108">toostart, futtassa az bejelentkezési toocreate Azure kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="0e277-108">toostart, run az login toocreate a connection with Azure.</span></span> 

## <a name="create-and-verify-hello-virtual-machine"></a><span data-ttu-id="0e277-109">Hozzon létre, és ellenőrizze a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="0e277-109">Create and verify hello virtual machine</span></span> 
<span data-ttu-id="0e277-110">Hozzon létre egy virtuális gép egy Piactéri rendszerképből az ssh hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="0e277-110">Create a VM from a marketplace image with ssh authentication.</span></span>
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> <span data-ttu-id="0e277-111">PUT hello **labor tartozó erőforráscsoport** hello--erőforráscsoport paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="0e277-111">Put hello **lab's resource group** name in hello --resource-group parameter.</span></span>
>

<span data-ttu-id="0e277-112">Toocreate a virtuális gépek képlet, használja hello – a képlet paraméter [az tesztlabor virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/lab/vm#create).</span><span class="sxs-lookup"><span data-stu-id="0e277-112">If you want toocreate a VM using a formula, use hello --formula parameter in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#create).</span></span>


<span data-ttu-id="0e277-113">Győződjön meg arról, hogy hello VM érhető el.</span><span class="sxs-lookup"><span data-stu-id="0e277-113">Verify that hello VM is available.</span></span>
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

## <a name="start-and-connect-toohello-virtual-machine"></a><span data-ttu-id="0e277-114">Indítsa el, és csatlakoztassa toohello virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="0e277-114">Start and connect toohello virtual machine</span></span>
<span data-ttu-id="0e277-115">Indítsa el a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="0e277-115">Start a VM.</span></span>
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> <span data-ttu-id="0e277-116">PUT hello **labor tartozó erőforráscsoport** hello--erőforráscsoport paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="0e277-116">Put hello **lab's resource group** name in hello --resource-group parameter.</span></span>
>

<span data-ttu-id="0e277-117">Csatlakozás a virtuális gép tooa: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) vagy [távoli asztal](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="0e277-117">Connect tooa VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) or [Remote Desktop](../virtual-machines/windows/connect-logon.md).</span></span>
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-hello-virtual-machine"></a><span data-ttu-id="0e277-118">Hello virtuális gép frissítése</span><span class="sxs-lookup"><span data-stu-id="0e277-118">Update hello virtual machine</span></span>
<span data-ttu-id="0e277-119">Az összetevők tooa VM alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="0e277-119">Apply artifacts tooa VM.</span></span>
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

<span data-ttu-id="0e277-120">Lista összetevők hello tesztkörnyezet érhető el.</span><span class="sxs-lookup"><span data-stu-id="0e277-120">List artifacts available in hello lab.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-hello-virtual-machine"></a><span data-ttu-id="0e277-121">Állítsa le és törölje a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="0e277-121">Stop and delete hello virtual machine</span></span>    
<span data-ttu-id="0e277-122">Állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="0e277-122">Stop a VM.</span></span>
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

<span data-ttu-id="0e277-123">A virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="0e277-123">Delete a VM.</span></span>
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
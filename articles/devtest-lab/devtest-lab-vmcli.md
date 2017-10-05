---
title: "A DevTest Labs szolgáltatásban az Azure parancssori felület a virtuális gépek létrehozására és kezelésére |} Microsoft Docs"
description: "Útmutató: Azure DevTest Labs segítségével az Azure CLI 2.0 virtuális gépek létrehozására és kezelésére"
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
ms.openlocfilehash: 42b0448c1bcdfa909715abd5075353d63cab8389
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-virtual-machines-with-devtest-labs-using-the-azure-cli"></a><span data-ttu-id="16768-103">Hozzon létre és virtuális gépek kezelése a DevTest Labs szolgáltatásban az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="16768-103">Create and manage virtual machines with DevTest Labs using the Azure CLI</span></span>
<span data-ttu-id="16768-104">A gyors üzembe helyezési végigvezeti keresztül létrehozása, elindítása, csatlakozás, frissítése és törléséről a fejlesztési számítógépén a tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="16768-104">This quick start will guide you through creating, starting, connecting, updating and cleaning up a development machine in your lab.</span></span> 

<span data-ttu-id="16768-105">Előzetes teendők</span><span class="sxs-lookup"><span data-stu-id="16768-105">Before you begin:</span></span>

* <span data-ttu-id="16768-106">Ha a labor nem lett létrehozva, található utasításokat [Itt](devtest-lab-create-lab.md).</span><span class="sxs-lookup"><span data-stu-id="16768-106">If a lab has not been created, instructions can be found [here](devtest-lab-create-lab.md).</span></span>

* <span data-ttu-id="16768-107">[CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="16768-107">[Install CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span> <span data-ttu-id="16768-108">Indításához futtassa az bejelentkezés az Azure VPN-kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="16768-108">To start, run az login to create a connection with Azure.</span></span> 

## <a name="create-and-verify-the-virtual-machine"></a><span data-ttu-id="16768-109">Hozzon létre, és ellenőrizze a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="16768-109">Create and verify the virtual machine</span></span> 
<span data-ttu-id="16768-110">Hozzon létre egy virtuális gép egy Piactéri rendszerképből az ssh hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="16768-110">Create a VM from a marketplace image with ssh authentication.</span></span>
```azurecli
az lab vm create --lab-name sampleLabName --resource-group sampleLabResourceGroup --name sampleVMName --image "Ubuntu Server 16.04 LTS" --image-type gallery --size Standard_DS1_v2 --authentication-type  ssh --generate-ssh-keys --ip-configuration public 
```
> [!NOTE]
> <span data-ttu-id="16768-111">Helyezze a **labor tartozó erőforráscsoport** paraméterben--erőforráscsoport név.</span><span class="sxs-lookup"><span data-stu-id="16768-111">Put the **lab's resource group** name in the --resource-group parameter.</span></span>
>

<span data-ttu-id="16768-112">Ha szeretne létrehozni egy virtuális gép használatával egy képlet, használja a--képlet paraméter [az tesztlabor virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/lab/vm#create).</span><span class="sxs-lookup"><span data-stu-id="16768-112">If you want to create a VM using a formula, use the --formula parameter in [az lab vm create](https://docs.microsoft.com/cli/azure/lab/vm#create).</span></span>


<span data-ttu-id="16768-113">Győződjön meg arról, hogy rendelkezésre áll-e a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="16768-113">Verify that the VM is available.</span></span>
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

## <a name="start-and-connect-to-the-virtual-machine"></a><span data-ttu-id="16768-114">Indítsa el, és csatlakozzon a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="16768-114">Start and connect to the virtual machine</span></span>
<span data-ttu-id="16768-115">Indítsa el a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="16768-115">Start a VM.</span></span>
```azurecli
az lab vm start --lab-name sampleLabName --name sampleVMName --resource-group sampleLabResourceGroup
```
> [!NOTE]
> <span data-ttu-id="16768-116">Helyezze a **labor tartozó erőforráscsoport** paraméterben--erőforráscsoport név.</span><span class="sxs-lookup"><span data-stu-id="16768-116">Put the **lab's resource group** name in the --resource-group parameter.</span></span>
>

<span data-ttu-id="16768-117">Csatlakoztassa a virtuális Gépet: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) vagy [távoli asztal](../virtual-machines/windows/connect-logon.md).</span><span class="sxs-lookup"><span data-stu-id="16768-117">Connect to a VM: [SSH](../virtual-machines/linux/mac-create-ssh-keys.md) or [Remote Desktop](../virtual-machines/windows/connect-logon.md).</span></span>
```bash
ssh userName@ipAddressOrfqdn 
```

## <a name="update-the-virtual-machine"></a><span data-ttu-id="16768-118">A virtuális gép frissítése</span><span class="sxs-lookup"><span data-stu-id="16768-118">Update the virtual machine</span></span>
<span data-ttu-id="16768-119">A virtuális gépek összetevők vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="16768-119">Apply artifacts to a VM.</span></span>
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

<span data-ttu-id="16768-120">Lista összetevők a tesztkörnyezet érhető el.</span><span class="sxs-lookup"><span data-stu-id="16768-120">List artifacts available in the lab.</span></span>
```azurecli
az lab vm show --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup --expand "properties(\$expand=artifacts)" --query 'artifacts[].{artifactId: artifactId, status: status}'
```
```json
{
  "artifactId": "/subscriptions/abcdeftgh1213123/resourceGroups/lisalab123RG822645/providers/Microsoft.DevTestLab/labs/lisalab123/artifactSources/public repo/artifacts/linux-install-nodejs",
  "status": "Succeeded"
}
```

## <a name="stop-and-delete-the-virtual-machine"></a><span data-ttu-id="16768-121">Állítsa le és törölje a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="16768-121">Stop and delete the virtual machine</span></span>    
<span data-ttu-id="16768-122">Állítsa le a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="16768-122">Stop a VM.</span></span>
```azurecli
az lab vm stop --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

<span data-ttu-id="16768-123">A virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="16768-123">Delete a VM.</span></span>
```azurecli
az lab vm delete --lab-name sampleLabName --name sampleVMName --resource-group sampleResourceGroup
```

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]
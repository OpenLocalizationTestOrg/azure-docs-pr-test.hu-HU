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
# <a name="create-a-windows-virtual-machine-with-hello-azure-cli"></a><span data-ttu-id="16fc8-103">Hozzon létre egy Windows rendszerű virtuális gép hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="16fc8-103">Create a Windows virtual machine with hello Azure CLI</span></span>

<span data-ttu-id="16fc8-104">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="16fc8-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="16fc8-105">Ez az útmutató adatokat hello Azure CLI toodeploy Windows Server 2016 rendszert futtató virtuális gépek használata.</span><span class="sxs-lookup"><span data-stu-id="16fc8-105">This guide details using hello Azure CLI toodeploy a virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="16fc8-106">Központi telepítés befejezése után azt csatlakoztassa toohello a kiszolgálót, és telepítse az IIS.</span><span class="sxs-lookup"><span data-stu-id="16fc8-106">Once deployment is complete, we connect toohello server and install IIS.</span></span>

<span data-ttu-id="16fc8-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="16fc8-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="16fc8-108">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="16fc8-108">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="16fc8-109">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="16fc8-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="16fc8-110">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="16fc8-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="16fc8-111">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="16fc8-111">Create a resource group</span></span>

<span data-ttu-id="16fc8-112">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="16fc8-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="16fc8-113">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="16fc8-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="16fc8-114">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="16fc8-114">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="16fc8-115">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="16fc8-115">Create virtual machine</span></span>

<span data-ttu-id="16fc8-116">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="16fc8-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> 

<span data-ttu-id="16fc8-117">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*.</span><span class="sxs-lookup"><span data-stu-id="16fc8-117">hello following example creates a VM named *myVM*.</span></span> <span data-ttu-id="16fc8-118">Ez a példa *azureuser* egy rendszergazda felhasználó neve és *myPassword12* hello jelszóként.</span><span class="sxs-lookup"><span data-stu-id="16fc8-118">This example uses *azureuser* for an administrative user name and *myPassword12* as hello password.</span></span> <span data-ttu-id="16fc8-119">Ezen értékek toosomething megfelelő tooyour környezet frissítése.</span><span class="sxs-lookup"><span data-stu-id="16fc8-119">Update these values toosomething appropriate tooyour environment.</span></span> <span data-ttu-id="16fc8-120">Ezeket az értékeket a kapcsolat létrehozásakor hello virtuális géppel van szükség.</span><span class="sxs-lookup"><span data-stu-id="16fc8-120">These values are needed when creating a connection with hello virtual machine.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

<span data-ttu-id="16fc8-121">Hello virtuális gép létrehozásakor a hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="16fc8-121">When hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="16fc8-122">Jegyezze fel a hello `publicIpAaddress`.</span><span class="sxs-lookup"><span data-stu-id="16fc8-122">Take note of hello `publicIpAaddress`.</span></span> <span data-ttu-id="16fc8-123">Ez a cím használt tooaccess hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="16fc8-123">This address is used tooaccess hello VM.</span></span>

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

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="16fc8-124">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="16fc8-124">Open port 80 for web traffic</span></span> 

<span data-ttu-id="16fc8-125">Alapértelmezés szerint csak az RDP-kapcsolatok tooWindows virtuális gépek Azure szolgáltatásba telepített engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="16fc8-125">By default only RDP connections are allowed in tooWindows virtual machines deployed in Azure.</span></span> <span data-ttu-id="16fc8-126">Ha a virtuális gép lesz egy webkiszolgáló toobe, kell tooopen hello Internet a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="16fc8-126">If this VM is going toobe a webserver, you need tooopen port 80 from hello Internet.</span></span> <span data-ttu-id="16fc8-127">Használjon hello [az vm-port megnyitása](/cli/azure/vm#open-port) parancs tooopen hello port szükséges.</span><span class="sxs-lookup"><span data-stu-id="16fc8-127">Use hello [az vm open-port](/cli/azure/vm#open-port) command tooopen hello desired port.</span></span>  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-toovirtual-machine"></a><span data-ttu-id="16fc8-128">Csatlakoztassa a gépet toovirtual</span><span class="sxs-lookup"><span data-stu-id="16fc8-128">Connect toovirtual machine</span></span>

<span data-ttu-id="16fc8-129">Használjon hello következő parancsot a toocreate egy távoli asztali munkamenet hello virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="16fc8-129">Use hello following command toocreate a remote desktop session with hello virtual machine.</span></span> <span data-ttu-id="16fc8-130">Hello IP-cím cserélje le a virtuális gép hello nyilvános IP-címe.</span><span class="sxs-lookup"><span data-stu-id="16fc8-130">Replace hello IP address with hello public IP address of your virtual machine.</span></span> <span data-ttu-id="16fc8-131">Amikor a rendszer kéri, adja meg a hello virtuális gép létrehozásakor használt hello hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="16fc8-131">When prompted, enter hello credentials used when creating hello virtual machine.</span></span>

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a><span data-ttu-id="16fc8-132">Az IIS telepítése a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="16fc8-132">Install IIS using PowerShell</span></span>

<span data-ttu-id="16fc8-133">Most, hogy az Azure virtuális gép toohello már bejelentkezett, használjon PowerShell tooinstall IIS egysoros, és hello helyi tűzfal szabály tooallow webes forgalom engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="16fc8-133">Now that you have logged in toohello Azure VM, you can use a single line of PowerShell tooinstall IIS and enable hello local firewall rule tooallow web traffic.</span></span> <span data-ttu-id="16fc8-134">Nyisson meg egy PowerShell-parancssorba, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="16fc8-134">Open a PowerShell prompt and run hello following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="16fc8-135">Nézet hello IIS-kezdőlap</span><span class="sxs-lookup"><span data-stu-id="16fc8-135">View hello IIS welcome page</span></span>

<span data-ttu-id="16fc8-136">A telepített IIS-t, és most nyissa meg a virtuális gép hello Internet a 80-as porton az a choice tooview hello alapértelmezett IIS üdvözlőlap webböngésző is használhatja.</span><span class="sxs-lookup"><span data-stu-id="16fc8-136">With IIS installed and port 80 now open on your VM from hello Internet, you can use a web browser of your choice tooview hello default IIS welcome page.</span></span> <span data-ttu-id="16fc8-137">Lehet, hogy toouse hello nyilvános IP-cím feletti toovisit hello alapértelmezett oldal részletes ismertetését lásd.</span><span class="sxs-lookup"><span data-stu-id="16fc8-137">Be sure toouse hello public IP address you documented above toovisit hello default page.</span></span> 

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="16fc8-139">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="16fc8-139">Clean up resources</span></span>

<span data-ttu-id="16fc8-140">Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="16fc8-140">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="16fc8-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="16fc8-141">Next steps</span></span>

<span data-ttu-id="16fc8-142">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="16fc8-142">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="16fc8-143">További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Windows virtuális gépek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="16fc8-143">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="16fc8-144">Windowsos virtuális gépek az Azure-ban – oktatóanyagok</span><span class="sxs-lookup"><span data-stu-id="16fc8-144">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)

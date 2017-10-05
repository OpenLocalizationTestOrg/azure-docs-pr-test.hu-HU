---
title: "Azure gyors üzembe helyezés – Windows VM CLI létrehozása | Microsoft Docs"
description: "Gyorsan megismerheti a Windows rendszerű virtuális gép Azure parancssori felülettel való létrehozásának módját."
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
ms.openlocfilehash: fcb2f1389b3434d0d2e3145217e54ceb2326b969
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-windows-virtual-machine-with-the-azure-cli"></a><span data-ttu-id="f93ad-103">Windowsos virtuális gép létrehozása az Azure parancssori felülettel</span><span class="sxs-lookup"><span data-stu-id="f93ad-103">Create a Windows virtual machine with the Azure CLI</span></span>

<span data-ttu-id="f93ad-104">Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="f93ad-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="f93ad-105">Ez az útmutató részletesen bemutatja, hogyan lehet egy Windows Server 2016-ot futtató virtuális gépet az Azure CLI-vel üzembe helyezni.</span><span class="sxs-lookup"><span data-stu-id="f93ad-105">This guide details using the Azure CLI to deploy a virtual machine running Windows Server 2016.</span></span> <span data-ttu-id="f93ad-106">Az üzembe helyezés végeztével csatlakozunk a kiszolgálóhoz, és telepítjük az IIS-t.</span><span class="sxs-lookup"><span data-stu-id="f93ad-106">Once deployment is complete, we connect to the server and install IIS.</span></span>

<span data-ttu-id="f93ad-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="f93ad-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="f93ad-108">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a gyorsútmutatóhoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="f93ad-108">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="f93ad-109">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="f93ad-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="f93ad-110">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="f93ad-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 


## <a name="create-a-resource-group"></a><span data-ttu-id="f93ad-111">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="f93ad-111">Create a resource group</span></span>

<span data-ttu-id="f93ad-112">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f93ad-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="f93ad-113">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="f93ad-113">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="f93ad-114">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot az *eastus* helyen.</span><span class="sxs-lookup"><span data-stu-id="f93ad-114">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="f93ad-115">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f93ad-115">Create virtual machine</span></span>

<span data-ttu-id="f93ad-116">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="f93ad-116">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> 

<span data-ttu-id="f93ad-117">Az alábbi példában egy *myVM* nevű virtuális gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f93ad-117">The following example creates a VM named *myVM*.</span></span> <span data-ttu-id="f93ad-118">Ez a példa az *azureuser* rendszergazdanevet és a *myPassword12* jelszót használja.</span><span class="sxs-lookup"><span data-stu-id="f93ad-118">This example uses *azureuser* for an administrative user name and *myPassword12* as the password.</span></span> <span data-ttu-id="f93ad-119">Az értékeket módosítsa a környezetének megfelelően.</span><span class="sxs-lookup"><span data-stu-id="f93ad-119">Update these values to something appropriate to your environment.</span></span> <span data-ttu-id="f93ad-120">Ezekre az értékekre akkor van szükség, amikor kapcsolódik a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="f93ad-120">These values are needed when creating a connection with the virtual machine.</span></span>

```azurecli-interactive 
az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --admin-username azureuser --admin-password myPassword12
```

<span data-ttu-id="f93ad-121">A virtuális gép létrehozása után az Azure CLI az alábbi példához hasonló információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="f93ad-121">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="f93ad-122">Jegyezze fel a `publicIpAaddress` értékét.</span><span class="sxs-lookup"><span data-stu-id="f93ad-122">Take note of the `publicIpAaddress`.</span></span> <span data-ttu-id="f93ad-123">Ez a cím használható a virtuális gép eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="f93ad-123">This address is used to access the VM.</span></span>

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

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="f93ad-124">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="f93ad-124">Open port 80 for web traffic</span></span> 

<span data-ttu-id="f93ad-125">Alapértelmezés szerint kizárólag RDP-kapcsolatok engedélyezettek az Azure-ban üzembe helyezett, Windows rendszerű virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f93ad-125">By default only RDP connections are allowed in to Windows virtual machines deployed in Azure.</span></span> <span data-ttu-id="f93ad-126">Ha ez a virtuális gép webkiszolgáló lesz, meg kell nyitnia a 80-as portot az internet irányából.</span><span class="sxs-lookup"><span data-stu-id="f93ad-126">If this VM is going to be a webserver, you need to open port 80 from the Internet.</span></span> <span data-ttu-id="f93ad-127">A kívánt port megnyitásához használja az [az vm open-port](/cli/azure/vm#open-port) parancsot.</span><span class="sxs-lookup"><span data-stu-id="f93ad-127">Use the [az vm open-port](/cli/azure/vm#open-port) command to open the desired port.</span></span>  
 
 ```azurecli-interactive  
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```


## <a name="connect-to-virtual-machine"></a><span data-ttu-id="f93ad-128">Csatlakozás virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="f93ad-128">Connect to virtual machine</span></span>

<span data-ttu-id="f93ad-129">Használja az alábbi parancsot egy távoli asztali kapcsolat létrehozásához a virtuális géppel.</span><span class="sxs-lookup"><span data-stu-id="f93ad-129">Use the following command to create a remote desktop session with the virtual machine.</span></span> <span data-ttu-id="f93ad-130">Cserélje le az IP-címet a virtuális gépe nyilvános IP-címére.</span><span class="sxs-lookup"><span data-stu-id="f93ad-130">Replace the IP address with the public IP address of your virtual machine.</span></span> <span data-ttu-id="f93ad-131">Ha a rendszer erre kéri, adja meg a virtuális gép létrehozásakor használt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="f93ad-131">When prompted, enter the credentials used when creating the virtual machine.</span></span>

```bash 
mstsc /v:<Public IP Address>
```

## <a name="install-iis-using-powershell"></a><span data-ttu-id="f93ad-132">Az IIS telepítése a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="f93ad-132">Install IIS using PowerShell</span></span>

<span data-ttu-id="f93ad-133">Miután bejelentkezett az Azure-beli virtuális gépre, egyetlen PowerShell-utasítással telepítheti az IIS-t, és engedélyezheti, hogy a helyi tűzfalszabály átengedje a webforgalmat.</span><span class="sxs-lookup"><span data-stu-id="f93ad-133">Now that you have logged in to the Azure VM, you can use a single line of PowerShell to install IIS and enable the local firewall rule to allow web traffic.</span></span> <span data-ttu-id="f93ad-134">Nyisson meg egy PowerShell-parancssort, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="f93ad-134">Open a PowerShell prompt and run the following command:</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

## <a name="view-the-iis-welcome-page"></a><span data-ttu-id="f93ad-135">Az IIS kezdőlapjának megtekintése</span><span class="sxs-lookup"><span data-stu-id="f93ad-135">View the IIS welcome page</span></span>

<span data-ttu-id="f93ad-136">Miután az IIS telepítve lett, és a 80-as port meg van nyitva a virtuális gépen az internet irányából, egy tetszőleges böngésző használatával megtekintheti az alapértelmezett IIS-kezdőlapot.</span><span class="sxs-lookup"><span data-stu-id="f93ad-136">With IIS installed and port 80 now open on your VM from the Internet, you can use a web browser of your choice to view the default IIS welcome page.</span></span> <span data-ttu-id="f93ad-137">Ügyeljen arra, hogy az alapértelmezett oldalt a fentebb dokumentált nyilvános IP-cím használatával keresse fel.</span><span class="sxs-lookup"><span data-stu-id="f93ad-137">Be sure to use the public IP address you documented above to visit the default page.</span></span> 

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="f93ad-139">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="f93ad-139">Clean up resources</span></span>

<span data-ttu-id="f93ad-140">Ha már nincs rá szükség, a [az group delete](/cli/azure/group#delete) paranccsal eltávolítható az erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrás.</span><span class="sxs-lookup"><span data-stu-id="f93ad-140">When no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="f93ad-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f93ad-141">Next steps</span></span>

<span data-ttu-id="f93ad-142">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="f93ad-142">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="f93ad-143">Ha bővebb információra van szüksége az Azure-alapú virtuális gépekkel kapcsolatban, lépjen tovább a Windows rendszerű virtuális gépekről szóló oktatóanyagra.</span><span class="sxs-lookup"><span data-stu-id="f93ad-143">To learn more about Azure virtual machines, continue to the tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="f93ad-144">Windowsos virtuális gépek az Azure-ban – oktatóanyagok</span><span class="sxs-lookup"><span data-stu-id="f93ad-144">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)

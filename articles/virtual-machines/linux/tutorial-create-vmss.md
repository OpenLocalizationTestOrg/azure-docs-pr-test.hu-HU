---
title: "Hozzon létre egy virtuálisgép-méretezési csoportok Linux az Azure-ban |} Microsoft Docs"
description: "A Linux virtuális gépet egy virtuálisgép-méretezési csoport segítségével egy magas rendelkezésre állású alkalmazás létrehozását és telepítését"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.openlocfilehash: 2b8d519e11f70eda164bd8f6e131a3989f242ab0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a><span data-ttu-id="35d92-103">Hozzon létre egy virtuálisgép-méretezési és magas rendelkezésre állású Linux alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="35d92-103">Create a Virtual Machine Scale Set and deploy a highly available app on Linux</span></span>
<span data-ttu-id="35d92-104">A virtuálisgép-méretezési csoport lehetővé teszi, telepítéséhez és kezeléséhez azonos, az automatikus skálázást virtuális gépek halmazát jelenti.</span><span class="sxs-lookup"><span data-stu-id="35d92-104">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="35d92-105">A méretezési csoportban lévő virtuális gépek száma manuálisan méretezheti, vagy az automatikus skálázás CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján szabályok megadása.</span><span class="sxs-lookup"><span data-stu-id="35d92-105">You can scale the number of VMs in the scale set manually, or define rules to autoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="35d92-106">Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="35d92-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="35d92-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="35d92-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="35d92-108">Felhő inicializálás segítségével méretezési-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="35d92-108">Use cloud-init to create an app to scale</span></span>
> * <span data-ttu-id="35d92-109">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="35d92-109">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="35d92-110">Növeli vagy csökkenti a méretezési csoportban lévő példányok száma</span><span class="sxs-lookup"><span data-stu-id="35d92-110">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="35d92-111">Kapcsolatinformáció méretezési készlet példányok megtekintése</span><span class="sxs-lookup"><span data-stu-id="35d92-111">View connection info for scale set instances</span></span>
> * <span data-ttu-id="35d92-112">Méretezési csoportban lévő adatok lemez használata</span><span class="sxs-lookup"><span data-stu-id="35d92-112">Use data disks in a scale set</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="35d92-113">Telepítése és a parancssori felület helyileg használata mellett dönt, ha ez az oktatóanyag van szükség, hogy futnak-e az Azure parancssori felület 2.0.4 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="35d92-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="35d92-114">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="35d92-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="35d92-115">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="35d92-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="scale-set-overview"></a><span data-ttu-id="35d92-116">Méretezési készlet – áttekintés</span><span class="sxs-lookup"><span data-stu-id="35d92-116">Scale Set overview</span></span>
<span data-ttu-id="35d92-117">A virtuálisgép-méretezési csoport lehetővé teszi, telepítéséhez és kezeléséhez azonos, az automatikus skálázást virtuális gépek halmazát jelenti.</span><span class="sxs-lookup"><span data-stu-id="35d92-117">A virtual machine scale set allows you to deploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="35d92-118">Méretezési csoportok az azonos összetevőket használnak, mint az előző oktatóanyag megismerte [hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="35d92-118">Scale sets use the same components as you learned about in the previous tutorial to [Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="35d92-119">Állítsa be, és logikai hiba és a frissítési tartományok között elosztott rendelkezésre állási méretezési csoportban lévő virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="35d92-119">VMs in a scale set are created in an availability set and distributed across logic fault and update domains.</span></span>

<span data-ttu-id="35d92-120">Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="35d92-120">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="35d92-121">Megadhatja az automatikus skálázási szabályok, hogy hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor a méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="35d92-121">You define autoscale rules to control how and when VMs are added or removed from the scale set.</span></span> <span data-ttu-id="35d92-122">Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="35d92-122">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="35d92-123">Skálázási készletekben legfeljebb 1000 virtuális gépek támogatása, ha az Azure platformon lemezképet használ.</span><span class="sxs-lookup"><span data-stu-id="35d92-123">Scale sets support up to 1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="35d92-124">A termelési számítási feladatokhoz, érdemes lehet [hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="35d92-124">For production workloads, you may wish to [Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="35d92-125">Legfeljebb 100 virtuális gépek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="35d92-125">You can create up to 100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-to-scale"></a><span data-ttu-id="35d92-126">Méretezési-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="35d92-126">Create an app to scale</span></span>
<span data-ttu-id="35d92-127">Az éles környezetben való használathoz, érdemes lehet [hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md) , amely tartalmazza az alkalmazás telepítését és konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="35d92-127">For production use, you may wish to [Create a custom VM image](tutorial-custom-images.md) that includes your application installed and configured.</span></span> <span data-ttu-id="35d92-128">A jelen oktatóanyag esetében lehetővé teszi, hogy gyorsan megtekintheti a méretezési művelet a készletben lévő első rendszerindító virtuális gépek testreszabása.</span><span class="sxs-lookup"><span data-stu-id="35d92-128">For this tutorial, lets customize the VMs on first boot to quickly see a scale set in action.</span></span>

<span data-ttu-id="35d92-129">Az előző oktatóanyagban megtanulta [első indításakor Linux virtuális gépek testreszabása](tutorial-automate-vm-deployment.md) a felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="35d92-129">In a previous tutorial, you learned [How to customize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) with cloud-init.</span></span> <span data-ttu-id="35d92-130">Az azonos felhő inicializálás konfigurációs fájl segítségével NGINX telepítheti és futtathatja egy egyszerű "Hello, World" Node.js alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="35d92-130">You can use the same cloud-init configuration file to install NGINX and run a simple 'Hello World' Node.js app.</span></span> 

<span data-ttu-id="35d92-131">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* , majd illessze be a következő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="35d92-131">In your current shell, create a file named *cloud-init.txt* and paste the following configuration.</span></span> <span data-ttu-id="35d92-132">A felhő rendszerhéj nem a helyi számítógépen hozzon létre például a fájlt.</span><span class="sxs-lookup"><span data-stu-id="35d92-132">For example, create the file in the Cloud Shell not on your local machine.</span></span> <span data-ttu-id="35d92-133">Adja meg `sensible-editor cloud-init.txt` hozza létre a fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="35d92-133">Enter `sensible-editor cloud-init.txt` to create the file and see a list of available editors.</span></span> <span data-ttu-id="35d92-134">Győződjön meg arról, hogy az egész felhő inicializálás fájl megfelelően lett lemásolva különösen az első sor:</span><span class="sxs-lookup"><span data-stu-id="35d92-134">Make sure that the whole cloud-init file is copied correctly, especially the first line:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```


## <a name="create-a-scale-set"></a><span data-ttu-id="35d92-135">Méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="35d92-135">Create a scale set</span></span>
<span data-ttu-id="35d92-136">A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="35d92-136">Before you can create a scale set, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="35d92-137">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupScaleSet* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="35d92-137">The following example creates a resource group named *myResourceGroupScaleSet* in the *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

<span data-ttu-id="35d92-138">Most hozzon létre egy virtuálisgép-méretezési állítható be [az vmss létrehozása](/cli/azure/vmss#create).</span><span class="sxs-lookup"><span data-stu-id="35d92-138">Now create a virtual machine scale set with [az vmss create](/cli/azure/vmss#create).</span></span> <span data-ttu-id="35d92-139">Az alábbi példakód létrehozza a méretezési készletben elnevezett *myScaleSet*, a felhő inicializálás fájlt használja a virtuális gép testreszabása és SSH-kulcsokat generál, ha azok nem léteznek:</span><span class="sxs-lookup"><span data-stu-id="35d92-139">The following example creates a scale set named *myScaleSet*, uses the cloud-init file to customize the VM, and generates SSH keys if they do not exist:</span></span>

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

<span data-ttu-id="35d92-140">Hozza létre és konfigurálja a méretezési készlet erőforrások és a virtuális gépek néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="35d92-140">It takes a few minutes to create and configure all the scale set resources and VMs.</span></span> <span data-ttu-id="35d92-141">Nincsenek háttérfeladatok, hogy végezze el az Azure parancssori felület visszatér a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="35d92-141">There are background tasks that continue to run after the Azure CLI returns you to the prompt.</span></span> <span data-ttu-id="35d92-142">Elképzelhető, hogy egy másik néhány percet, mielőtt hozzáférhetne az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="35d92-142">It may be another couple of minutes before you can access the app.</span></span>


## <a name="allow-web-traffic"></a><span data-ttu-id="35d92-143">Webalkalmazás-kezelési forgalom engedélyezése</span><span class="sxs-lookup"><span data-stu-id="35d92-143">Allow web traffic</span></span>
<span data-ttu-id="35d92-144">A terheléselosztó a virtuálisgép-méretezési csoport részeként automatikusan lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="35d92-144">A load balancer was created automatically as part of the virtual machine scale set.</span></span> <span data-ttu-id="35d92-145">A load balancer egy meghatározott virtuális gépeket használ a load balancer szabályok készletét forgalom elosztása.</span><span class="sxs-lookup"><span data-stu-id="35d92-145">The load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="35d92-146">Load balancer fogalmak és a következő oktatóanyag konfigurálásával kapcsolatos részletesebb [hogyan virtuális gépek terhelést elosztani az Azure-ban](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="35d92-146">You can learn more about load balancer concepts and configuration in the next tutorial, [How to load balance virtual machines in Azure](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="35d92-147">A webes alkalmazás eléréséhez forgalom engedélyezéséhez hozzon létre egy szabály [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="35d92-147">To allow traffic to reach the web app, create a rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="35d92-148">Az alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRuleWeb*:</span><span class="sxs-lookup"><span data-stu-id="35d92-148">The following example creates a rule named *myLoadBalancerRuleWeb*:</span></span>

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a><span data-ttu-id="35d92-149">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="35d92-149">Test your app</span></span>
<span data-ttu-id="35d92-150">Az Node.js alkalmazás megtekintése a weben, szerezze be a terheléselosztó a nyilvános IP-címe [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="35d92-150">To see your Node.js app on the web, obtain the public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="35d92-151">Az alábbi példa beolvassa az IP-címek *myScaleSetLBPublicIP* hozza létre a méretezési részeként:</span><span class="sxs-lookup"><span data-stu-id="35d92-151">The following example obtains the IP address for *myScaleSetLBPublicIP* created as part of the scale set:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="35d92-152">Adja meg a nyilvános IP-címet egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="35d92-152">Enter the public IP address in to a web browser.</span></span> <span data-ttu-id="35d92-153">Az alkalmazás megjelenik, beleértve az állomásnevet, a virtuális gép, amelyek a terheléselosztó felé irányuló forgalom:</span><span class="sxs-lookup"><span data-stu-id="35d92-153">The app is displayed, including the hostname of the VM that the load balancer distributed traffic to:</span></span>

![Futó Node.js-alkalmazás](./media/tutorial-create-vmss/running-nodejs-app.png)

<span data-ttu-id="35d92-155">Tekintse meg a méretezési készletben működés közben, akkor is kényszerített frissítési a webböngészőt a terheléselosztó forgalom szét az alkalmazást futtató összes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="35d92-155">To see the scale set in action, you can force-refresh your web browser to see the load balancer distribute traffic across all the VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="35d92-156">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="35d92-156">Management tasks</span></span>
<span data-ttu-id="35d92-157">A méretezési életciklusa során szükség lehet egy vagy több felügyeleti feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="35d92-157">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="35d92-158">Emellett érdemes lehet különböző életciklus-feladatokat automatizáló parancsfájlokat hozhatnak létre.</span><span class="sxs-lookup"><span data-stu-id="35d92-158">Additionally, you may want to create scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="35d92-159">Az Azure CLI 2.0 e feladatok elvégzéséhez gyors lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="35d92-159">The Azure CLI 2.0 provides a quick way to do those tasks.</span></span> <span data-ttu-id="35d92-160">Az alábbiakban néhány gyakori feladatot.</span><span class="sxs-lookup"><span data-stu-id="35d92-160">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="35d92-161">Nézet virtuális gépek méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="35d92-161">View VMs in a scale set</span></span>
<span data-ttu-id="35d92-162">A méretezési csoportban lévő rendszert futtató virtuális gépek listájának megtekintéséhez használja [az vmss-példányokat](/cli/azure/vmss#list-instances) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="35d92-162">To view a list of VMs running in your scale set, use [az vmss list-instances](/cli/azure/vmss#list-instances) as follows:</span></span>

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

<span data-ttu-id="35d92-163">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="35d92-163">The output is similar to the following example:</span></span>

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="35d92-164">Növeli vagy csökkenti a Virtuálisgép-példányok</span><span class="sxs-lookup"><span data-stu-id="35d92-164">Increase or decrease VM instances</span></span>
<span data-ttu-id="35d92-165">Már van egy méretezési csoportban lévő példányok száma, használja a [az vmss megjelenítése](/cli/azure/vmss#show) és a lekérdezés *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="35d92-165">To see the number of instances you currently have in a scale set, use [az vmss show](/cli/azure/vmss#show) and query on *sku.capacity*:</span></span>

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

<span data-ttu-id="35d92-166">Ezután manuálisan növeléséhez vagy csökkentéséhez tegye a következőket a méretezési készletben rendelkező virtuális gépek [az vmss méretezési](/cli/azure/vmss#scale).</span><span class="sxs-lookup"><span data-stu-id="35d92-166">You can then manually increase or decrease the number of virtual machines in the scale set with [az vmss scale](/cli/azure/vmss#scale).</span></span> <span data-ttu-id="35d92-167">Az alábbi példában a virtuális gépek számát beállítja a méretezés beállítása *5*:</span><span class="sxs-lookup"><span data-stu-id="35d92-167">The following example sets the number of VMs in your scale set to *5*:</span></span>

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

<span data-ttu-id="35d92-168">Automatikus skálázási szabályok segítségével meghatározhatja, hogyan kell felfelé vagy lefelé a virtuális gépek számát a méretezési iránti igény miatt például a hálózati forgalom vagy a CPU-használat készletben méretezése.</span><span class="sxs-lookup"><span data-stu-id="35d92-168">Autoscale rules let you define how to scale up or down the number of VMs in your scale set in response to demand such as network traffic or CPU usage.</span></span> <span data-ttu-id="35d92-169">Ezek a szabályok jelenleg az Azure CLI 2.0 nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="35d92-169">Currently, these rules cannot be set in Azure CLI 2.0.</span></span> <span data-ttu-id="35d92-170">Használja a [Azure-portálon](https://portal.azure.com) automatikus skálázás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="35d92-170">Use the [Azure portal](https://portal.azure.com) to configure autoscale.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="35d92-171">Kapcsolat-adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="35d92-171">Get connection info</span></span>
<span data-ttu-id="35d92-172">A virtuális gépek a méretezési csoportok kapcsolati információ beszerzéséhez használja [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info).</span><span class="sxs-lookup"><span data-stu-id="35d92-172">To obtain connection information about the VMs in your scale sets, use [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info).</span></span> <span data-ttu-id="35d92-173">Ez a parancs kimenetében a nyilvános IP-cím és port az egyes virtuális gépekhez, amely lehetővé teszi, hogy csatlakozzon SSH:</span><span class="sxs-lookup"><span data-stu-id="35d92-173">This command outputs the public IP address and port for each VM that allows you to connect with SSH:</span></span>

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a><span data-ttu-id="35d92-174">Az adatlemezek használata méretezési csoportok</span><span class="sxs-lookup"><span data-stu-id="35d92-174">Use data disks with scale sets</span></span>
<span data-ttu-id="35d92-175">Hozzon létre, és adatlemezek használata méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="35d92-175">You can create and use data disks with scale sets.</span></span> <span data-ttu-id="35d92-176">Egy korábbi oktatóanyagban megtanulta, hogyan [kezelése Azure lemezek](tutorial-manage-disks.md) , amely ismerteti az ajánlott eljárásokról és a teljesítménnyel kapcsolatos fejlesztések az operációsrendszer-lemezképet, hanem az adatlemezek alkalmazások készítéséhez.</span><span class="sxs-lookup"><span data-stu-id="35d92-176">In a previous tutorial, you learned how to [Manage Azure disks](tutorial-manage-disks.md) that outlines the best practices and performance improvements for building apps on data disks rather than the OS disk.</span></span>

### <a name="create-scale-set-with-data-disks"></a><span data-ttu-id="35d92-177">Adatlemezekkel rendelkező méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="35d92-177">Create scale set with data disks</span></span>
<span data-ttu-id="35d92-178">Hozzon létre egy méretezési és adatlemezt csatolni, adja hozzá a `--data-disk-sizes-gb` paramétert a [az vmss létrehozása](/cli/azure/vmss#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="35d92-178">To create a scale set and attach data disks, add the `--data-disk-sizes-gb` parameter to the [az vmss create](/cli/azure/vmss#create) command.</span></span> <span data-ttu-id="35d92-179">Az alábbi példakód létrehozza a skála állítható be *50*Gb adatlemezek csatolva minden példány:</span><span class="sxs-lookup"><span data-stu-id="35d92-179">The following example creates a scale set with *50*Gb data disks attached to each instance:</span></span>

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

<span data-ttu-id="35d92-180">Ha példányok egy méretezési eltávolították, a mellékelt adatok lemezek is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="35d92-180">When instances are removed from a scale set, any attached data disks are also removed.</span></span>

### <a name="add-data-disks"></a><span data-ttu-id="35d92-181">Adatlemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="35d92-181">Add data disks</span></span>
<span data-ttu-id="35d92-182">A méretezési csoportban lévő példányok adhat hozzá adatlemezt, [az vmss lemez csatolása](/cli/azure/vmss/disk#attach).</span><span class="sxs-lookup"><span data-stu-id="35d92-182">To add a data disk to instances in your scale set, use [az vmss disk attach](/cli/azure/vmss/disk#attach).</span></span> <span data-ttu-id="35d92-183">A következő példa egy *50*-példányokhoz Gb lemezterület:</span><span class="sxs-lookup"><span data-stu-id="35d92-183">The following example adds a *50*Gb disk to each instance:</span></span>

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a><span data-ttu-id="35d92-184">Adatlemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="35d92-184">Detach data disks</span></span>
<span data-ttu-id="35d92-185">A méretezési csoportban lévő példányokhoz adatlemezt eltávolításához használja [az vmss lemez leválasztása](/cli/azure/vmss/disk#detach).</span><span class="sxs-lookup"><span data-stu-id="35d92-185">To remove a data disk to instances in your scale set, use [az vmss disk detach](/cli/azure/vmss/disk#detach).</span></span> <span data-ttu-id="35d92-186">A következő példában eltávolítjuk a LUN azonosítójú adatlemeze *2* az egyes példányok:</span><span class="sxs-lookup"><span data-stu-id="35d92-186">The following example removes the data disk at LUN *2* from each instance:</span></span>

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a><span data-ttu-id="35d92-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35d92-187">Next steps</span></span>
<span data-ttu-id="35d92-188">Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="35d92-188">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="35d92-189">Megtudta, hogyan, hogy:</span><span class="sxs-lookup"><span data-stu-id="35d92-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="35d92-190">Felhő inicializálás segítségével méretezési-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="35d92-190">Use cloud-init to create an app to scale</span></span>
> * <span data-ttu-id="35d92-191">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="35d92-191">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="35d92-192">Növeli vagy csökkenti a méretezési csoportban lévő példányok száma</span><span class="sxs-lookup"><span data-stu-id="35d92-192">Increase or decrease the number of instances in a scale set</span></span>
> * <span data-ttu-id="35d92-193">Kapcsolatinformáció méretezési készlet példányok megtekintése</span><span class="sxs-lookup"><span data-stu-id="35d92-193">View connection info for scale set instances</span></span>
> * <span data-ttu-id="35d92-194">Méretezési csoportban lévő adatok lemez használata</span><span class="sxs-lookup"><span data-stu-id="35d92-194">Use data disks in a scale set</span></span>

<span data-ttu-id="35d92-195">A következő oktatóanyag további információt a hálózati terheléselosztást a virtuális gépek fogalmak továbblépés.</span><span class="sxs-lookup"><span data-stu-id="35d92-195">Advance to the next tutorial to learn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="35d92-196">Virtuális gépek terhelést elosztani</span><span class="sxs-lookup"><span data-stu-id="35d92-196">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
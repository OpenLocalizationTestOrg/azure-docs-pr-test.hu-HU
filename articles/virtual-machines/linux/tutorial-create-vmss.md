---
title: "egy Azure Linux virtuálisgép-méretezési csoportok aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: 00dd81043f9be46ef2dc6dfe97eefdb20944ee13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a><span data-ttu-id="1ed73-103">Hozzon létre egy virtuálisgép-méretezési és magas rendelkezésre állású Linux alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="1ed73-103">Create a Virtual Machine Scale Set and deploy a highly available app on Linux</span></span>
<span data-ttu-id="1ed73-104">A virtuálisgép-méretezési csoport toodeploy lehetővé teszi, és az azonos, az automatikus skálázást virtuális gépek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="1ed73-104">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="1ed73-105">Hello hello méretezési csoportban lévő virtuális gépek száma manuálisan méretezhető, vagy a szabályok tooautoscale CPU kihasználtsága, a memória igény szerint vagy a hálózati forgalom alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="1ed73-105">You can scale hello number of VMs in hello scale set manually, or define rules tooautoscale based on CPU usage, memory demand, or network traffic.</span></span> <span data-ttu-id="1ed73-106">Ebben az oktatóanyagban telepít egy virtuálisgép-méretezési beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="1ed73-106">In this tutorial, you deploy a virtual machine scale set in Azure.</span></span> <span data-ttu-id="1ed73-107">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="1ed73-107">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1ed73-108">Egy alkalmazás tooscale felhő inicializálás toocreate használata</span><span class="sxs-lookup"><span data-stu-id="1ed73-108">Use cloud-init toocreate an app tooscale</span></span>
> * <span data-ttu-id="1ed73-109">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="1ed73-109">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="1ed73-110">Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma</span><span class="sxs-lookup"><span data-stu-id="1ed73-110">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="1ed73-111">Kapcsolatinformáció méretezési készlet példányok megtekintése</span><span class="sxs-lookup"><span data-stu-id="1ed73-111">View connection info for scale set instances</span></span>
> * <span data-ttu-id="1ed73-112">Méretezési csoportban lévő adatok lemez használata</span><span class="sxs-lookup"><span data-stu-id="1ed73-112">Use data disks in a scale set</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="1ed73-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1ed73-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="1ed73-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="1ed73-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="1ed73-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="1ed73-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="scale-set-overview"></a><span data-ttu-id="1ed73-116">Méretezési készlet – áttekintés</span><span class="sxs-lookup"><span data-stu-id="1ed73-116">Scale Set overview</span></span>
<span data-ttu-id="1ed73-117">A virtuálisgép-méretezési csoport toodeploy lehetővé teszi, és az azonos, az automatikus skálázást virtuális gépek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="1ed73-117">A virtual machine scale set allows you toodeploy and manage a set of identical, auto-scaling virtual machines.</span></span> <span data-ttu-id="1ed73-118">Skálázási készletekben használata hello azonos összetevők, megismerte hello előző oktatóprogram túl[hozzon létre magas rendelkezésre állású virtuális gépek](tutorial-availability-sets.md).</span><span class="sxs-lookup"><span data-stu-id="1ed73-118">Scale sets use hello same components as you learned about in hello previous tutorial too[Create highly available VMs](tutorial-availability-sets.md).</span></span> <span data-ttu-id="1ed73-119">Állítsa be, és logikai hiba és a frissítési tartományok között elosztott rendelkezésre állási méretezési csoportban lévő virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="1ed73-119">VMs in a scale set are created in an availability set and distributed across logic fault and update domains.</span></span>

<span data-ttu-id="1ed73-120">Virtuális gépek méretezési csoportban lévő igény szerint jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="1ed73-120">VMs are created as needed in a scale set.</span></span> <span data-ttu-id="1ed73-121">Megadhatja az automatikus skálázási szabályok toocontrol hogyan és mikor virtuális gépek hozzáadásakor vagy eltávolításakor hello méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="1ed73-121">You define autoscale rules toocontrol how and when VMs are added or removed from hello scale set.</span></span> <span data-ttu-id="1ed73-122">Ezek a szabályok alapján metrikák például CPU-terhelést, a memória használata vagy a hálózati forgalmat is elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="1ed73-122">These rules can trigger based on metrics such as CPU load, memory usage, or network traffic.</span></span>

<span data-ttu-id="1ed73-123">Skálázási készletekben támogatási too1, 000 virtuális gépeken, ha az Azure platformon lemezképet használ fel.</span><span class="sxs-lookup"><span data-stu-id="1ed73-123">Scale sets support up too1,000 VMs when you use an Azure platform image.</span></span> <span data-ttu-id="1ed73-124">A termelési számítási feladatokhoz, Kezdésként túl[hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="1ed73-124">For production workloads, you may wish too[Create a custom VM image](tutorial-custom-images.md).</span></span> <span data-ttu-id="1ed73-125">Too100 virtuális gépeinek egy méretezési állítható be, ha egyéni lemezkép használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="1ed73-125">You can create up too100 VMs in a scale set when using a custom image.</span></span>


## <a name="create-an-app-tooscale"></a><span data-ttu-id="1ed73-126">Hozzon létre egy alkalmazást tooscale</span><span class="sxs-lookup"><span data-stu-id="1ed73-126">Create an app tooscale</span></span>
<span data-ttu-id="1ed73-127">Az éles környezetben való használathoz, Kezdésként túl[hozzon létre egy egyéni Virtuálisgép-lemezkép](tutorial-custom-images.md) , amely tartalmazza az alkalmazás telepítését és konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="1ed73-127">For production use, you may wish too[Create a custom VM image](tutorial-custom-images.md) that includes your application installed and configured.</span></span> <span data-ttu-id="1ed73-128">A jelen oktatóanyag esetében lehetővé teszi, hogy első rendszerindítási tooquickly futó virtuális gépek, tekintse meg a méretezési művelet készletben hello testreszabása.</span><span class="sxs-lookup"><span data-stu-id="1ed73-128">For this tutorial, lets customize hello VMs on first boot tooquickly see a scale set in action.</span></span>

<span data-ttu-id="1ed73-129">Az előző oktatóanyagban megtanulta [hogyan toocustomize egy Linux virtuális gép első indításakor](tutorial-automate-vm-deployment.md) a felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="1ed73-129">In a previous tutorial, you learned [How toocustomize a Linux virtual machine on first boot](tutorial-automate-vm-deployment.md) with cloud-init.</span></span> <span data-ttu-id="1ed73-130">Használhatja ugyanazon felhő inicializálás konfigurációs fájl tooinstall NGINX hello és egy egyszerű "Hello World" Node.js-alkalmazás futtatása.</span><span class="sxs-lookup"><span data-stu-id="1ed73-130">You can use hello same cloud-init configuration file tooinstall NGINX and run a simple 'Hello World' Node.js app.</span></span> 

<span data-ttu-id="1ed73-131">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="1ed73-131">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="1ed73-132">A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="1ed73-132">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="1ed73-133">Adja meg `sensible-editor cloud-init.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1ed73-133">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="1ed73-134">Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:</span><span class="sxs-lookup"><span data-stu-id="1ed73-134">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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


## <a name="create-a-scale-set"></a><span data-ttu-id="1ed73-135">Méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ed73-135">Create a scale set</span></span>
<span data-ttu-id="1ed73-136">A méretezési csoport létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="1ed73-136">Before you can create a scale set, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="1ed73-137">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupScaleSet* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="1ed73-137">hello following example creates a resource group named *myResourceGroupScaleSet* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

<span data-ttu-id="1ed73-138">Most hozzon létre egy virtuálisgép-méretezési állítható be [az vmss létrehozása](/cli/azure/vmss#create).</span><span class="sxs-lookup"><span data-stu-id="1ed73-138">Now create a virtual machine scale set with [az vmss create](/cli/azure/vmss#create).</span></span> <span data-ttu-id="1ed73-139">hello alábbi példa létrehoz egy méretezési készletben elnevezett *myScaleSet*, hello felhő inicializálás fájl toocustomize hello virtuális gép használja, és SSH-kulcsokat generál, ha azok nem léteznek:</span><span class="sxs-lookup"><span data-stu-id="1ed73-139">hello following example creates a scale set named *myScaleSet*, uses hello cloud-init file toocustomize hello VM, and generates SSH keys if they do not exist:</span></span>

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

<span data-ttu-id="1ed73-140">Néhány perc toocreate vesz igénybe, és hello méretezési készlet erőforrások és a virtuális gépek konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="1ed73-140">It takes a few minutes toocreate and configure all hello scale set resources and VMs.</span></span> <span data-ttu-id="1ed73-141">Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés.</span><span class="sxs-lookup"><span data-stu-id="1ed73-141">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="1ed73-142">Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1ed73-142">It may be another couple of minutes before you can access hello app.</span></span>


## <a name="allow-web-traffic"></a><span data-ttu-id="1ed73-143">Webalkalmazás-kezelési forgalom engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1ed73-143">Allow web traffic</span></span>
<span data-ttu-id="1ed73-144">A terheléselosztó hello virtuálisgép-méretezési csoport részeként automatikusan lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="1ed73-144">A load balancer was created automatically as part of hello virtual machine scale set.</span></span> <span data-ttu-id="1ed73-145">hello terheléselosztó elosztása forgalom meghatározott virtuális gépeket használ a load balancer szabályok készlete.</span><span class="sxs-lookup"><span data-stu-id="1ed73-145">hello load balancer distributes traffic across a set of defined VMs using load balancer rules.</span></span> <span data-ttu-id="1ed73-146">Load balancer fogalmakat és hello következő oktatóanyagban konfigurálásával kapcsolatos részletesebb [hogyan tooload elosztása a virtuális gépek Azure-ban](tutorial-load-balancer.md).</span><span class="sxs-lookup"><span data-stu-id="1ed73-146">You can learn more about load balancer concepts and configuration in hello next tutorial, [How tooload balance virtual machines in Azure](tutorial-load-balancer.md).</span></span>

<span data-ttu-id="1ed73-147">tooallow forgalom tooreach hello webes alkalmazás, hozzon létre egy szabályt a [az hálózati terheléselosztó szabály létrehozása](/cli/azure/network/lb/rule#create).</span><span class="sxs-lookup"><span data-stu-id="1ed73-147">tooallow traffic tooreach hello web app, create a rule with [az network lb rule create](/cli/azure/network/lb/rule#create).</span></span> <span data-ttu-id="1ed73-148">hello alábbi példa létrehoz egy nevű szabályt *myLoadBalancerRuleWeb*:</span><span class="sxs-lookup"><span data-stu-id="1ed73-148">hello following example creates a rule named *myLoadBalancerRuleWeb*:</span></span>

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

## <a name="test-your-app"></a><span data-ttu-id="1ed73-149">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="1ed73-149">Test your app</span></span>
<span data-ttu-id="1ed73-150">toosee az beszerzése hello nyilvános IP-címét a terheléselosztót, a Node.js-alkalmazás hello weben [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show).</span><span class="sxs-lookup"><span data-stu-id="1ed73-150">toosee your Node.js app on hello web, obtain hello public IP address of your load balancer with [az network public-ip show](/cli/azure/network/public-ip#show).</span></span> <span data-ttu-id="1ed73-151">hello alábbi példa beszerzi hello IP-címet *myScaleSetLBPublicIP* hello méretezési részeként létrehozott:</span><span class="sxs-lookup"><span data-stu-id="1ed73-151">hello following example obtains hello IP address for *myScaleSetLBPublicIP* created as part of hello scale set:</span></span>

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="1ed73-152">Adja meg a nyilvános IP-cím hello tooa webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="1ed73-152">Enter hello public IP address in tooa web browser.</span></span> <span data-ttu-id="1ed73-153">hello alkalmazásokról, beleértve az adott hello VM betöltése elosztott terheléselosztó felé irányuló forgalom hello hello állomásnevét:</span><span class="sxs-lookup"><span data-stu-id="1ed73-153">hello app is displayed, including hello hostname of hello VM that hello load balancer distributed traffic to:</span></span>

![Futó Node.js-alkalmazás](./media/tutorial-create-vmss/running-nodejs-app.png)

<span data-ttu-id="1ed73-155">toosee hello méretezési készletben működés közben, akkor is kényszerített frissítési a webes böngésző toosee hello terhelését terheléselosztó forgalom szét az alkalmazást futtató összes hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="1ed73-155">toosee hello scale set in action, you can force-refresh your web browser toosee hello load balancer distribute traffic across all hello VMs running your app.</span></span>


## <a name="management-tasks"></a><span data-ttu-id="1ed73-156">Felügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="1ed73-156">Management tasks</span></span>
<span data-ttu-id="1ed73-157">Hello méretezési hello életciklusa során szükség lehet a toorun egy vagy több felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="1ed73-157">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="1ed73-158">Emellett érdemes lehet toocreate olyan parancsfájlok, amelyek különböző életciklus-feladatok automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="1ed73-158">Additionally, you may want toocreate scripts that automate various lifecycle-tasks.</span></span> <span data-ttu-id="1ed73-159">hello Azure CLI 2.0 biztosít egy gyorsan toodo ezeket a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="1ed73-159">hello Azure CLI 2.0 provides a quick way toodo those tasks.</span></span> <span data-ttu-id="1ed73-160">Az alábbiakban néhány gyakori feladatot.</span><span class="sxs-lookup"><span data-stu-id="1ed73-160">Here are a few common tasks.</span></span>

### <a name="view-vms-in-a-scale-set"></a><span data-ttu-id="1ed73-161">Nézet virtuális gépek méretezési csoportban lévő</span><span class="sxs-lookup"><span data-stu-id="1ed73-161">View VMs in a scale set</span></span>
<span data-ttu-id="1ed73-162">tooview a skála futó virtuális gépek listájának megadásához használja [az vmss-példányokat](/cli/azure/vmss#list-instances) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="1ed73-162">tooview a list of VMs running in your scale set, use [az vmss list-instances](/cli/azure/vmss#list-instances) as follows:</span></span>

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

<span data-ttu-id="1ed73-163">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="1ed73-163">hello output is similar toohello following example:</span></span>

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a><span data-ttu-id="1ed73-164">Növeli vagy csökkenti a Virtuálisgép-példányok</span><span class="sxs-lookup"><span data-stu-id="1ed73-164">Increase or decrease VM instances</span></span>
<span data-ttu-id="1ed73-165">példányok száma toosee hello jelenleg egy méretezési állította, használjon [az vmss megjelenítése](/cli/azure/vmss#show) és a lekérdezés *sku.capacity*:</span><span class="sxs-lookup"><span data-stu-id="1ed73-165">toosee hello number of instances you currently have in a scale set, use [az vmss show](/cli/azure/vmss#show) and query on *sku.capacity*:</span></span>

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

<span data-ttu-id="1ed73-166">Majd manuálisan növelhető és csökkenthető hello méretezési a készletben lévő virtuális gépek száma hello [az vmss méretezési](/cli/azure/vmss#scale).</span><span class="sxs-lookup"><span data-stu-id="1ed73-166">You can then manually increase or decrease hello number of virtual machines in hello scale set with [az vmss scale](/cli/azure/vmss#scale).</span></span> <span data-ttu-id="1ed73-167">hello alábbi mintakód hello virtuális gépek száma a méretezési készletben túl a*5*:</span><span class="sxs-lookup"><span data-stu-id="1ed73-167">hello following example sets hello number of VMs in your scale set too*5*:</span></span>

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```

<span data-ttu-id="1ed73-168">Automatikus skálázási szabályok segítségével határozza meg, hogyan tooscale felfelé vagy lefelé hello virtuális gépek számát a skála állítsa be a válasz toodemand például a hálózati forgalom vagy a CPU-használat.</span><span class="sxs-lookup"><span data-stu-id="1ed73-168">Autoscale rules let you define how tooscale up or down hello number of VMs in your scale set in response toodemand such as network traffic or CPU usage.</span></span> <span data-ttu-id="1ed73-169">Ezek a szabályok jelenleg az Azure CLI 2.0 nem állítható be.</span><span class="sxs-lookup"><span data-stu-id="1ed73-169">Currently, these rules cannot be set in Azure CLI 2.0.</span></span> <span data-ttu-id="1ed73-170">Használjon hello [Azure-portálon](https://portal.azure.com) tooconfigure automatikus skálázási.</span><span class="sxs-lookup"><span data-stu-id="1ed73-170">Use hello [Azure portal](https://portal.azure.com) tooconfigure autoscale.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="1ed73-171">Kapcsolat-adatok beolvasása</span><span class="sxs-lookup"><span data-stu-id="1ed73-171">Get connection info</span></span>
<span data-ttu-id="1ed73-172">tooobtain kapcsolatadatok készül hello virtuális gépek a méretezési készletben, használja [az vmss lista--kapcsolat-példányadatait](/cli/azure/vmss#list-instance-connection-info).</span><span class="sxs-lookup"><span data-stu-id="1ed73-172">tooobtain connection information about hello VMs in your scale sets, use [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info).</span></span> <span data-ttu-id="1ed73-173">Ez a parancs kimenetében hello nyilvános IP-cím és port az egyes virtuális gépekhez, amely lehetővé teszi az SSH tooconnect:</span><span class="sxs-lookup"><span data-stu-id="1ed73-173">This command outputs hello public IP address and port for each VM that allows you tooconnect with SSH:</span></span>

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a><span data-ttu-id="1ed73-174">Az adatlemezek használata méretezési csoportok</span><span class="sxs-lookup"><span data-stu-id="1ed73-174">Use data disks with scale sets</span></span>
<span data-ttu-id="1ed73-175">Hozzon létre, és adatlemezek használata méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="1ed73-175">You can create and use data disks with scale sets.</span></span> <span data-ttu-id="1ed73-176">Egy korábbi oktatóanyagban megtanulta, hogyan túl[kezelése Azure lemezek](tutorial-manage-disks.md) , hogy a körvonal hello ajánlott eljárásokról és a teljesítménnyel kapcsolatos fejlesztések az alkalmazások támaszkodva hello operációsrendszer-lemez helyett adatlemezek.</span><span class="sxs-lookup"><span data-stu-id="1ed73-176">In a previous tutorial, you learned how too[Manage Azure disks](tutorial-manage-disks.md) that outlines hello best practices and performance improvements for building apps on data disks rather than hello OS disk.</span></span>

### <a name="create-scale-set-with-data-disks"></a><span data-ttu-id="1ed73-177">Adatlemezekkel rendelkező méretezési készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="1ed73-177">Create scale set with data disks</span></span>
<span data-ttu-id="1ed73-178">toocreate terjedő skálán állítsa be, és csatlakoztassa az adatlemezek hozzáadása hello `--data-disk-sizes-gb` paraméter toohello [az vmss létrehozása](/cli/azure/vmss#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="1ed73-178">toocreate a scale set and attach data disks, add hello `--data-disk-sizes-gb` parameter toohello [az vmss create](/cli/azure/vmss#create) command.</span></span> <span data-ttu-id="1ed73-179">hello alábbi példa létrehoz egy méretezési a készletben *50*Gb adatlemezt csatolni tooeach példány:</span><span class="sxs-lookup"><span data-stu-id="1ed73-179">hello following example creates a scale set with *50*Gb data disks attached tooeach instance:</span></span>

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

<span data-ttu-id="1ed73-180">Ha példányok egy méretezési eltávolították, a mellékelt adatok lemezek is törlődnek.</span><span class="sxs-lookup"><span data-stu-id="1ed73-180">When instances are removed from a scale set, any attached data disks are also removed.</span></span>

### <a name="add-data-disks"></a><span data-ttu-id="1ed73-181">Adatlemez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1ed73-181">Add data disks</span></span>
<span data-ttu-id="1ed73-182">tooadd egy adatok lemez tooinstances a méretezési megadásához használja [az vmss lemez csatolása](/cli/azure/vmss/disk#attach).</span><span class="sxs-lookup"><span data-stu-id="1ed73-182">tooadd a data disk tooinstances in your scale set, use [az vmss disk attach](/cli/azure/vmss/disk#attach).</span></span> <span data-ttu-id="1ed73-183">hello következő példakóddal felveheti egy *50*Gb szabad tooeach példány:</span><span class="sxs-lookup"><span data-stu-id="1ed73-183">hello following example adds a *50*Gb disk tooeach instance:</span></span>

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a><span data-ttu-id="1ed73-184">Adatlemez leválasztása</span><span class="sxs-lookup"><span data-stu-id="1ed73-184">Detach data disks</span></span>
<span data-ttu-id="1ed73-185">tooremove egy adatok lemez tooinstances a méretezési megadásához használja [az vmss lemez leválasztása](/cli/azure/vmss/disk#detach).</span><span class="sxs-lookup"><span data-stu-id="1ed73-185">tooremove a data disk tooinstances in your scale set, use [az vmss disk detach](/cli/azure/vmss/disk#detach).</span></span> <span data-ttu-id="1ed73-186">hello következő példában eltávolítjuk hello adatlemez LUN azonosítójú *2* az egyes példányok:</span><span class="sxs-lookup"><span data-stu-id="1ed73-186">hello following example removes hello data disk at LUN *2* from each instance:</span></span>

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a><span data-ttu-id="1ed73-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ed73-187">Next steps</span></span>
<span data-ttu-id="1ed73-188">Ebben az oktatóanyagban létre egy virtuálisgép-méretezési készlet.</span><span class="sxs-lookup"><span data-stu-id="1ed73-188">In this tutorial, you created a virtual machine scale set.</span></span> <span data-ttu-id="1ed73-189">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="1ed73-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1ed73-190">Egy alkalmazás tooscale felhő inicializálás toocreate használata</span><span class="sxs-lookup"><span data-stu-id="1ed73-190">Use cloud-init toocreate an app tooscale</span></span>
> * <span data-ttu-id="1ed73-191">Hozzon létre egy virtuálisgép-méretezési csoport</span><span class="sxs-lookup"><span data-stu-id="1ed73-191">Create a virtual machine scale set</span></span>
> * <span data-ttu-id="1ed73-192">Növeli vagy csökkenti a méretezési csoportban lévő példányok hello száma</span><span class="sxs-lookup"><span data-stu-id="1ed73-192">Increase or decrease hello number of instances in a scale set</span></span>
> * <span data-ttu-id="1ed73-193">Kapcsolatinformáció méretezési készlet példányok megtekintése</span><span class="sxs-lookup"><span data-stu-id="1ed73-193">View connection info for scale set instances</span></span>
> * <span data-ttu-id="1ed73-194">Méretezési csoportban lévő adatok lemez használata</span><span class="sxs-lookup"><span data-stu-id="1ed73-194">Use data disks in a scale set</span></span>

<span data-ttu-id="1ed73-195">Előzetes toohello következő útmutató toolearn bővebben a hálózati terheléselosztást a virtuális gépek fogalmakat.</span><span class="sxs-lookup"><span data-stu-id="1ed73-195">Advance toohello next tutorial toolearn more about load balancing concepts for virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1ed73-196">Virtuális gépek terhelést elosztani</span><span class="sxs-lookup"><span data-stu-id="1ed73-196">Load balance virtual machines</span></span>](tutorial-load-balancer.md)
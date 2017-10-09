---
title: "Linux virtuális gép első indításakor az Azure-ban aaaCustomize |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse felhő inicializálás és a Key Vault toocustomze Linux virtuális gépek hello először indulnak az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 280189723ac0205226f9c0068bd605da13249ace
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocustomize-a-linux-virtual-machine-on-first-boot"></a><span data-ttu-id="5b1ee-103">Hogyan toocustomize egy Linux virtuális gép első rendszerindítás</span><span class="sxs-lookup"><span data-stu-id="5b1ee-103">How toocustomize a Linux virtual machine on first boot</span></span>
<span data-ttu-id="5b1ee-104">Egy korábbi oktatóanyagban, megtudta, hogyan tooSSH tooa virtuális gép (VM) és NGINX manuálisan telepítenie.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-104">In a previous tutorial, you learned how tooSSH tooa virtual machine (VM) and manually install NGINX.</span></span> <span data-ttu-id="5b1ee-105">virtuális gépek toocreate gyorsan és egységes módon, valamilyen automatizált művelettel általában van szükség.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-105">toocreate VMs in a quick and consistent manner, some form of automation is typically desired.</span></span> <span data-ttu-id="5b1ee-106">Egy általános módszer toocustomize egy virtuális gép első indításakor toouse [felhő inicializálás](https://cloudinit.readthedocs.io).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-106">A common approach toocustomize a VM on first boot is toouse [cloud-init](https://cloudinit.readthedocs.io).</span></span> <span data-ttu-id="5b1ee-107">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b1ee-108">Felhő inicializálás konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-108">Create a cloud-init config file</span></span>
> * <span data-ttu-id="5b1ee-109">A felhő inicializálás fájlt használó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-109">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="5b1ee-110">Hello virtuális gép létrehozása után egy futó Node.js-alkalmazás megtekintése</span><span class="sxs-lookup"><span data-stu-id="5b1ee-110">View a running Node.js app after hello VM is created</span></span>
> * <span data-ttu-id="5b1ee-111">Key Vault toosecurely tároló-tanúsítványokat használ</span><span class="sxs-lookup"><span data-stu-id="5b1ee-111">Use Key Vault toosecurely store certificates</span></span>
> * <span data-ttu-id="5b1ee-112">Automatizálhatja a biztonságos telepítéseket NGINX felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="5b1ee-112">Automate secure deployments of NGINX with cloud-init</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="5b1ee-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="5b1ee-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="5b1ee-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  



## <a name="cloud-init-overview"></a><span data-ttu-id="5b1ee-116">Felhő inicializálás áttekintése</span><span class="sxs-lookup"><span data-stu-id="5b1ee-116">Cloud-init overview</span></span>
<span data-ttu-id="5b1ee-117">[Felhő inicializálás](https://cloudinit.readthedocs.io) megegyezik egy széles körben használt megközelítés toocustomize Linux virtuális gép elinduló hello az első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-117">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach toocustomize a Linux VM as it boots for hello first time.</span></span> <span data-ttu-id="5b1ee-118">Felhő inicializálás tooinstall csomagot, és fájlokat, vagy tooconfigure felhasználók és biztonsági írása.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-118">You can use cloud-init tooinstall packages and write files, or tooconfigure users and security.</span></span> <span data-ttu-id="5b1ee-119">Felhő inicializálás hello rendszerindítási folyamat során fut, mert nincsenek további lépések nem és az ügynökök tooapply a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-119">As cloud-init runs during hello initial boot process, there are no additional steps or required agents tooapply your configuration.</span></span>

<span data-ttu-id="5b1ee-120">Felhő inicializálás terjesztéseket is használható.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-120">Cloud-init also works across distributions.</span></span> <span data-ttu-id="5b1ee-121">Például, hogy ne használjon **apt-get-telepítés** vagy **yum telepítése** tooinstall csomagot.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-121">For example, you don't use **apt-get install** or **yum install** tooinstall a package.</span></span> <span data-ttu-id="5b1ee-122">Helyette megadhatja csomagok tooinstall listáját.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-122">Instead you can define a list of packages tooinstall.</span></span> <span data-ttu-id="5b1ee-123">Felhő inicializálás hello natív csomag felügyeleti eszköz automatikusan választ hello distro használ.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-123">Cloud-init automatically uses hello native package management tool for hello distro you select.</span></span>

<span data-ttu-id="5b1ee-124">Folyamatban, a partnerek tooget felhő inicializálás része azokkal, valamint, hogy ezek biztosítanak tooAzure hello képek működik.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-124">We are working with our partners tooget cloud-init included and working in hello images that they provide tooAzure.</span></span> <span data-ttu-id="5b1ee-125">a következő táblázat hello hello aktuális felhő inicializálás elérhetőségét a Azure platformon képek ismerteti:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-125">hello following table outlines hello current cloud-init availability on Azure platform images:</span></span>

| <span data-ttu-id="5b1ee-126">Alias</span><span class="sxs-lookup"><span data-stu-id="5b1ee-126">Alias</span></span> | <span data-ttu-id="5b1ee-127">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="5b1ee-127">Publisher</span></span> | <span data-ttu-id="5b1ee-128">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="5b1ee-128">Offer</span></span> | <span data-ttu-id="5b1ee-129">SKU</span><span class="sxs-lookup"><span data-stu-id="5b1ee-129">SKU</span></span> | <span data-ttu-id="5b1ee-130">Verzió</span><span class="sxs-lookup"><span data-stu-id="5b1ee-130">Version</span></span> |
|:--- |:--- |:--- |:--- |:--- |:--- |
| <span data-ttu-id="5b1ee-131">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="5b1ee-131">UbuntuLTS</span></span> |<span data-ttu-id="5b1ee-132">Canonical</span><span class="sxs-lookup"><span data-stu-id="5b1ee-132">Canonical</span></span> |<span data-ttu-id="5b1ee-133">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="5b1ee-133">UbuntuServer</span></span> |<span data-ttu-id="5b1ee-134">16.04-ES LTS VERZIÓ</span><span class="sxs-lookup"><span data-stu-id="5b1ee-134">16.04-LTS</span></span> |<span data-ttu-id="5b1ee-135">legújabb</span><span class="sxs-lookup"><span data-stu-id="5b1ee-135">latest</span></span> |
| <span data-ttu-id="5b1ee-136">UbuntuLTS</span><span class="sxs-lookup"><span data-stu-id="5b1ee-136">UbuntuLTS</span></span> |<span data-ttu-id="5b1ee-137">Canonical</span><span class="sxs-lookup"><span data-stu-id="5b1ee-137">Canonical</span></span> |<span data-ttu-id="5b1ee-138">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="5b1ee-138">UbuntuServer</span></span> |<span data-ttu-id="5b1ee-139">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="5b1ee-139">14.04.5-LTS</span></span> |<span data-ttu-id="5b1ee-140">legújabb</span><span class="sxs-lookup"><span data-stu-id="5b1ee-140">latest</span></span> |
| <span data-ttu-id="5b1ee-141">CoreOS</span><span class="sxs-lookup"><span data-stu-id="5b1ee-141">CoreOS</span></span> |<span data-ttu-id="5b1ee-142">CoreOS</span><span class="sxs-lookup"><span data-stu-id="5b1ee-142">CoreOS</span></span> |<span data-ttu-id="5b1ee-143">CoreOS</span><span class="sxs-lookup"><span data-stu-id="5b1ee-143">CoreOS</span></span> |<span data-ttu-id="5b1ee-144">Stable</span><span class="sxs-lookup"><span data-stu-id="5b1ee-144">Stable</span></span> |<span data-ttu-id="5b1ee-145">legújabb</span><span class="sxs-lookup"><span data-stu-id="5b1ee-145">latest</span></span> |


## <a name="create-cloud-init-config-file"></a><span data-ttu-id="5b1ee-146">Felhő inicializálás konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-146">Create cloud-init config file</span></span>
<span data-ttu-id="5b1ee-147">toosee felhő inicializálás a művelet hozzon létre egy virtuális Gépet, amely NGINX telepíti, és egy egyszerű "Hello, World" Node.js-alkalmazást futtat.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-147">toosee cloud-init in action, create a VM that installs NGINX and runs a simple 'Hello World' Node.js app.</span></span> <span data-ttu-id="5b1ee-148">hello inicializálás felhőalapú konfiguráció a következő szükséges hello csomagok telepíti, létrehoz egy Node.js-alkalmazást, majd inicializálása és hello alkalmazás elindítja.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-148">hello following cloud-init configuration installs hello required packages, creates a Node.js app, then initialize and starts hello app.</span></span>

<span data-ttu-id="5b1ee-149">Hozzon létre egy fájlt az aktuális rendszerhéjban *felhő-init.txt* és a Beillesztés hello a következő konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-149">In your current shell, create a file named *cloud-init.txt* and paste hello following configuration.</span></span> <span data-ttu-id="5b1ee-150">A felhő rendszerhéj hello nem a helyi számítógépen hozzon létre például hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-150">For example, create hello file in hello Cloud Shell not on your local machine.</span></span> <span data-ttu-id="5b1ee-151">A szerkesztő kívánja használata.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-151">You can use any editor you wish.</span></span> <span data-ttu-id="5b1ee-152">Adja meg `sensible-editor cloud-init.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-152">Enter `sensible-editor cloud-init.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="5b1ee-153">Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-153">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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

<span data-ttu-id="5b1ee-154">Felhő inicializálás konfigurációs beállításokkal kapcsolatos további információkért lásd: [felhő inicializálás config példák](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-154">For more information about cloud-init configuration options, see [cloud-init config examples](https://cloudinit.readthedocs.io/en/latest/topics/examples.html).</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="5b1ee-155">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-155">Create virtual machine</span></span>
<span data-ttu-id="5b1ee-156">A virtuális gépek létrehozása előtt hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-156">Before you can create a VM, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="5b1ee-157">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupAutomate* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-157">hello following example creates a resource group named *myResourceGroupAutomate* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupAutomate --location eastus
```

<span data-ttu-id="5b1ee-158">Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-158">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="5b1ee-159">Használjon hello `--custom-data` paraméter toopass a felhő inicializálás konfigurációs fájlban.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-159">Use hello `--custom-data` parameter toopass in your cloud-init config file.</span></span> <span data-ttu-id="5b1ee-160">Adja meg a teljes elérési útja toohello hello *felhő-init.txt* config, ha a jelenlegi munkakönyvtár kívül hello fájlt mentette.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-160">Provide hello full path toohello *cloud-init.txt* config if you saved hello file outside of your present working directory.</span></span> <span data-ttu-id="5b1ee-161">hello alábbi példakód létrehozza a virtuális gépek nevű *myAutomatedVM*:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-161">hello following example creates a VM named *myAutomatedVM*:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init.txt
```

<span data-ttu-id="5b1ee-162">Hello VM toobe hozott létre, néhány percet vesz igénybe hello csomagok tooinstall és hello app toostart.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-162">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="5b1ee-163">Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-163">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="5b1ee-164">Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-164">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="5b1ee-165">Hello virtuális gép létrehozása, jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-165">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="5b1ee-166">Ez a cím használt tooaccess hello Node.js-alkalmazás webböngésző segítségével.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-166">This address is used tooaccess hello Node.js app via a web browser.</span></span>

<span data-ttu-id="5b1ee-167">webes forgalom tooreach a virtuális Gépet, nyissa meg 80-as porton az hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="5b1ee-167">tooallow web traffic tooreach your VM, open port 80 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port --port 80 --resource-group myResourceGroupAutomate --name myVM
```

## <a name="test-web-app"></a><span data-ttu-id="5b1ee-168">Vizsgálati a webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="5b1ee-168">Test web app</span></span>
<span data-ttu-id="5b1ee-169">Most nyisson meg egy webböngészőt, és írja be *http://<publicIpAddress>*  hello címsorába.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-169">Now you can open a web browser and enter *http://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="5b1ee-170">Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-170">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="5b1ee-171">A Node.js-alkalmazás a következő példa hello hasonlóan jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-171">Your Node.js app is displayed as in hello following example:</span></span>

![Futó NGINX-hely megtekintése](./media/tutorial-automate-vm-deployment/nginx.png)


## <a name="inject-certificates-from-key-vault"></a><span data-ttu-id="5b1ee-173">A tanúsítványokat a Key Vault beszúrása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-173">Inject certificates from Key Vault</span></span>
<span data-ttu-id="5b1ee-174">A választható szakasz bemutatja, hogyan lehet biztonságosan tanúsítványok tárolása az Azure Key Vault és szúrjon azokat a Virtuálisgép-telepítéshez hello során.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-174">This optional section shows how you can securely store certificates in Azure Key Vault and inject them during hello VM deployment.</span></span> <span data-ttu-id="5b1ee-175">Ahelyett, hogy az egyéni lemezkép hello tanúsítványokat tartalmazó bővíthetőség-a, ez a folyamat biztosítja, hogy hello legfrissebb tanúsítványok vannak olyanok tooa virtuális gép első indításakor.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-175">Rather than using a custom image that includes hello certificates baked-in, this process ensures that hello most up-to-date certificates are injected tooa VM on first boot.</span></span> <span data-ttu-id="5b1ee-176">Hello során hello tanúsítvány soha nem hagyja hello Azure platformon, vagy fel van fedve egy parancsfájl, a parancssori előzmények vagy a sablonban.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-176">During hello process, hello certificate never leaves hello Azure platform or is exposed in a script, command-line history, or template.</span></span>

<span data-ttu-id="5b1ee-177">Az Azure Key Vault megvédi a titkosítási kulcsok és titkos kulcsokat, például a tanúsítványok vagy jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-177">Azure Key Vault safeguards cryptographic keys and secrets, such as certificates or passwords.</span></span> <span data-ttu-id="5b1ee-178">Key Vault segítségével teheti gördülékennyé hello kulcskezelési folyamatot, és lehetővé teszi a hozzáférést, és az adatok titkosításához használt kulcsok toomaintain irányítását.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-178">Key Vault helps streamline hello key management process and enables you toomaintain control of keys that access and encrypt your data.</span></span> <span data-ttu-id="5b1ee-179">Ebben a forgatókönyvben vezet be az egyes Key Vault fogalmak toocreate és a tanúsítvány használata, azonban a nem teljes körű áttekintést hogyan toouse kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-179">This scenario introduces some Key Vault concepts toocreate and use a certificate, though is not an exhaustive overview on how toouse Key Vault.</span></span>

<span data-ttu-id="5b1ee-180">hello lépések bemutatják, hogyan zajlik:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-180">hello following steps show how you can:</span></span>

- <span data-ttu-id="5b1ee-181">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-181">Create an Azure Key Vault</span></span>
- <span data-ttu-id="5b1ee-182">Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése</span><span class="sxs-lookup"><span data-stu-id="5b1ee-182">Generate or upload a certificate toohello Key Vault</span></span>
- <span data-ttu-id="5b1ee-183">A virtuális gép tooa hello tanúsítvány tooinject a titkos kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-183">Create a secret from hello certificate tooinject in tooa VM</span></span>
- <span data-ttu-id="5b1ee-184">Hozzon létre egy virtuális Gépet, és hello tanúsítvány beszúrása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-184">Create a VM and inject hello certificate</span></span>

### <a name="create-an-azure-key-vault"></a><span data-ttu-id="5b1ee-185">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-185">Create an Azure Key Vault</span></span>
<span data-ttu-id="5b1ee-186">Először hozzon létre egy rendelkező Key Vaultból [az keyvault létrehozása](/cli/azure/keyvault#create) és engedélyezi azt használja a virtuális gépek telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-186">First, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="5b1ee-187">Minden Key Vault szükséges egy egyedi nevet, és minden kisbetű kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-187">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="5b1ee-188">Cserélje le *mykeyvault* a következő példa a saját egyedi névvel Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-188">Replace *mykeyvault* in hello following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=mykeyvault
az keyvault create \
    --resource-group myResourceGroupAutomate \
    --name $keyvault_name \
    --enabled-for-deployment
```

### <a name="generate-certificate-and-store-in-key-vault"></a><span data-ttu-id="5b1ee-189">Tanúsítvány jön létre, és tárolja a Key Vault</span><span class="sxs-lookup"><span data-stu-id="5b1ee-189">Generate certificate and store in Key Vault</span></span>
<span data-ttu-id="5b1ee-190">Az éles környezetben való használathoz, importálnia kell a egy érvényes tanúsítvány megbízható-szolgáltató által aláírt [az keyvault tanúsítvány importálása](/cli/azure/keyvault/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-190">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/keyvault/certificate#import).</span></span> <span data-ttu-id="5b1ee-191">Ebben az oktatóanyagban hello következő példa bemutatja, hogyan hozhat létre egy önaláírt tanúsítványt [az keyvault tanúsítvány létrehozása](/cli/azure/keyvault/certificate#create) használó hello alapértelmezett tanúsítási szabályzat:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-191">For this tutorial, hello following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/keyvault/certificate#create) that uses hello default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```


### <a name="prepare-certificate-for-use-with-vm"></a><span data-ttu-id="5b1ee-192">Készítse elő a tanúsítvány használható a virtuális Géphez</span><span class="sxs-lookup"><span data-stu-id="5b1ee-192">Prepare certificate for use with VM</span></span>
<span data-ttu-id="5b1ee-193">toouse hello tanúsítvány során hello VM folyamatot létrehozni, szerezze be a tanúsítvány hello azonosítója [az keyvault lista-verzióját](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-193">toouse hello certificate during hello VM create process, obtain hello ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="5b1ee-194">virtuális gép hello hello tanúsítványt kell az egy bizonyos formátum tooinject azt a rendszerindító, így átalakítása hello tanúsítvány [az vm formátum-titkos](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-194">hello VM needs hello certificate in a certain format tooinject it on boot, so convert hello certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="5b1ee-195">a következő példa hello rendeli hozzá ezen parancsok toovariables könnyű használatra a hello hello kimenete lépéseket:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-195">hello following example assigns hello output of these commands toovariables for ease of use in hello next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```


### <a name="create-cloud-init-config-toosecure-nginx"></a><span data-ttu-id="5b1ee-196">Felhő inicializálás config toosecure NGINX létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-196">Create cloud-init config toosecure NGINX</span></span>
<span data-ttu-id="5b1ee-197">Ha hoz létre egy virtuális Gépet, a tanúsítványok és kulcsok tárolódnak védett hello */var/lib/waagent/* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-197">When you create a VM, certificates and keys are stored in hello protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="5b1ee-198">tooautomate hozzáadását hello tanúsítvány toohello VM és NGINX konfigurálása egy frissített felhő inicializálás config hello előző példa is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-198">tooautomate adding hello certificate toohello VM and configuring NGINX, you can use an updated cloud-init config from hello previous example.</span></span>

<span data-ttu-id="5b1ee-199">Hozzon létre egy fájlt *felhő-init-secured.txt* és a Beillesztés hello a következő konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-199">Create a file named *cloud-init-secured.txt* and paste hello following configuration.</span></span> <span data-ttu-id="5b1ee-200">Újra Ha felhő rendszerhéj hello használatához hozzon létre hello felhő inicializálás konfigurációs fájl vannak, és nem a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-200">Again, if you use hello Cloud Shell, create hello cloud-init config file there and not on your local machine.</span></span> <span data-ttu-id="5b1ee-201">Használjon `sensible-editor cloud-init-secured.txt` toocreate hello fájlt, és elérhető szerkesztők listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-201">Use `sensible-editor cloud-init-secured.txt` toocreate hello file and see a list of available editors.</span></span> <span data-ttu-id="5b1ee-202">Győződjön meg arról, hogy hello egész felhő inicializálás fájl megfelelően lett lemásolva, különösen az első sor hello:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-202">Make sure that hello whole cloud-init file is copied correctly, especially hello first line:</span></span>

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
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
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
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-secure-vm"></a><span data-ttu-id="5b1ee-203">Biztonságos virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-203">Create secure VM</span></span>
<span data-ttu-id="5b1ee-204">Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="5b1ee-204">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="5b1ee-205">hello tanúsítványának adatait a Key Vault beszúrta a hello `--secrets` paraméter.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-205">hello certificate data is injected from Key Vault with hello `--secrets` parameter.</span></span> <span data-ttu-id="5b1ee-206">Hello előző példához hasonlóan is át hello felhő inicializálás config a hello `--custom-data` paraméter:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-206">As in hello previous example, you also pass in hello cloud-init config with hello `--custom-data` parameter:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-secured.txt \
    --secrets "$vm_secret"
```

<span data-ttu-id="5b1ee-207">Hello VM toobe hozott létre, néhány percet vesz igénybe hello csomagok tooinstall és hello app toostart.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-207">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="5b1ee-208">Nincsenek háttérfeladatok, hogy toorun után hello Azure CLI-t adja vissza, akkor toohello kérdés.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-208">There are background tasks that continue toorun after hello Azure CLI returns you toohello prompt.</span></span> <span data-ttu-id="5b1ee-209">Elképzelhető, hogy egy másik néhány perc alatt hello alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-209">It may be another couple of minutes before you can access hello app.</span></span> <span data-ttu-id="5b1ee-210">Hello virtuális gép létrehozása, jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-210">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="5b1ee-211">Ez a cím használt tooaccess hello Node.js-alkalmazás webböngésző segítségével.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-211">This address is used tooaccess hello Node.js app via a web browser.</span></span>

<span data-ttu-id="5b1ee-212">biztonságos webes forgalom tooreach a virtuális Gépet, nyissa meg 443-as portot a hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="5b1ee-212">tooallow secure web traffic tooreach your VM, open port 443 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupAutomate \
    --name myVMSecured \
    --port 443
```

### <a name="test-secure-web-app"></a><span data-ttu-id="5b1ee-213">Biztonságos webes alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="5b1ee-213">Test secure web app</span></span>
<span data-ttu-id="5b1ee-214">Most nyisson meg egy webböngészőt, és írja be *https://<publicIpAddress>*  hello címsorába.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-214">Now you can open a web browser and enter *https://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="5b1ee-215">Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-215">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="5b1ee-216">Fogadja el a hello biztonsági figyelmeztetést, ha egy önaláírt tanúsítványt használt:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-216">Accept hello security warning if you used a self-signed certificate:</span></span>

![Fogadja el a webes böngésző biztonsági figyelmeztetés](./media/tutorial-automate-vm-deployment/browser-warning.png)

<span data-ttu-id="5b1ee-218">A biztonságos NGINX hely és a Node.js alkalmazás ezután megjelenik, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-218">Your secured NGINX site and Node.js app is then displayed as in hello following example:</span></span>

![Futó biztonságos NGINX webhely megtekintése](./media/tutorial-automate-vm-deployment/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="5b1ee-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5b1ee-220">Next steps</span></span>
<span data-ttu-id="5b1ee-221">Ebben az oktatóanyagban a virtuális gépek a felhő inicializálás első rendszerindítás konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-221">In this tutorial, you configured VMs on first boot with cloud-init.</span></span> <span data-ttu-id="5b1ee-222">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="5b1ee-222">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5b1ee-223">Felhő inicializálás konfigurációs fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-223">Create a cloud-init config file</span></span>
> * <span data-ttu-id="5b1ee-224">A felhő inicializálás fájlt használó virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-224">Create a VM that uses a cloud-init file</span></span>
> * <span data-ttu-id="5b1ee-225">Hello virtuális gép létrehozása után egy futó Node.js-alkalmazás megtekintése</span><span class="sxs-lookup"><span data-stu-id="5b1ee-225">View a running Node.js app after hello VM is created</span></span>
> * <span data-ttu-id="5b1ee-226">Key Vault toosecurely tároló-tanúsítványokat használ</span><span class="sxs-lookup"><span data-stu-id="5b1ee-226">Use Key Vault toosecurely store certificates</span></span>
> * <span data-ttu-id="5b1ee-227">Automatizálhatja a biztonságos telepítéseket NGINX felhő inicializálás</span><span class="sxs-lookup"><span data-stu-id="5b1ee-227">Automate secure deployments of NGINX with cloud-init</span></span>

<span data-ttu-id="5b1ee-228">Hogyan előzetes toohello következő útmutató toolearn toocreate egyéni Virtuálisgép-lemezképek.</span><span class="sxs-lookup"><span data-stu-id="5b1ee-228">Advance toohello next tutorial toolearn how toocreate custom VM images.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="5b1ee-229">Egyéni virtuálisgép-rendszerképek létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b1ee-229">Create custom VM images</span></span>](./tutorial-custom-images.md)

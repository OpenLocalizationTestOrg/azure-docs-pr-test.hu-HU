---
title: "a webkiszolgáló SSL-tanúsítványok az Azure-ban aaaSecure |} Microsoft Docs"
description: "Ismerje meg, hogyan toosecure hello NGINX webkiszolgáló SSL-tanúsítványok a Linux virtuális gép az Azure-ban"
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
ms.date: 07/17/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d3a62d77ac05c9aa2a44356b7c8e44cb485b81aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-web-server-with-ssl-certificates-on-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="64c27-103">Biztonságos Linux virtuális gépre az Azure-ban SSL-tanúsítványokkal egy webkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="64c27-103">Secure a web server with SSL certificates on a Linux virtual machine in Azure</span></span>
<span data-ttu-id="64c27-104">toosecure webkiszolgálók, a Secure Sockets később (SSL) tanúsítvány lehet tooencrypt webes forgalom használt.</span><span class="sxs-lookup"><span data-stu-id="64c27-104">toosecure web servers, a Secure Sockets Later (SSL) certificate can be used tooencrypt web traffic.</span></span> <span data-ttu-id="64c27-105">Ezek SSL-tanúsítványokat tárolhatja az Azure Key Vault, és a tanúsítványok tooLinux virtuális gépek (VM) biztonságos telepítéshez engedélyezése az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="64c27-105">These SSL certificates can be stored in Azure Key Vault, and allow secure deployments of certificates tooLinux virtual machines (VMs) in Azure.</span></span> <span data-ttu-id="64c27-106">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="64c27-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="64c27-107">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="64c27-107">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="64c27-108">Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése</span><span class="sxs-lookup"><span data-stu-id="64c27-108">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="64c27-109">Hozzon létre egy virtuális Gépet, és hello NGINX-webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="64c27-109">Create a VM and install hello NGINX web server</span></span>
> * <span data-ttu-id="64c27-110">Hello tanúsítvány behelyezése hello VM és SSL-kötést NGINX konfigurálása</span><span class="sxs-lookup"><span data-stu-id="64c27-110">Inject hello certificate into hello VM and configure NGINX with an SSL binding</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="64c27-111">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="64c27-111">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="64c27-112">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="64c27-112">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="64c27-113">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="64c27-113">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>  


## <a name="overview"></a><span data-ttu-id="64c27-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="64c27-114">Overview</span></span>
<span data-ttu-id="64c27-115">Az Azure Key Vault megvédi a titkosítási kulcsok és titkos kulcsok, ilyen tanúsítványokat vagy jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="64c27-115">Azure Key Vault safeguards cryptographic keys and secrets, such certificates or passwords.</span></span> <span data-ttu-id="64c27-116">Key Vault segítségével teheti gördülékennyé hello tanúsítvány felügyeleti folyamatot, és lehetővé teszi a kulcsok ezeknek a tanúsítványoknak elérő toomaintain irányítását.</span><span class="sxs-lookup"><span data-stu-id="64c27-116">Key Vault helps streamline hello certificate management process and enables you toomaintain control of keys that access those certificates.</span></span> <span data-ttu-id="64c27-117">Hozzon létre egy önaláírt tanúsítványt Key Vault belül, vagy töltsön fel egy meglévő, a megbízható tanúsítványt, amely már Ön a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="64c27-117">You can create a self-signed certificate inside Key Vault, or upload an existing, trusted certificate that you already own.</span></span>

<span data-ttu-id="64c27-118">Ahelyett, hogy az egyéni Virtuálisgép-lemezkép tanúsítványokat tartalmaz bővíthetőség a tanúsítványok behelyezése egy futó virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="64c27-118">Rather than using a custom VM image that includes certificates baked-in, you inject certificates into a running VM.</span></span> <span data-ttu-id="64c27-119">Ez a folyamat biztosítja, hogy naprakész tanúsítványok hello telepítve vannak a webkiszolgáló üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="64c27-119">This process ensures that hello most up-to-date certificates are installed on a web server during deployment.</span></span> <span data-ttu-id="64c27-120">Újítsa meg, vagy cserélje le a tanúsítványt, ha még nincs toocreate egy új egyéni Virtuálisgép-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="64c27-120">If you renew or replace a certificate, you don't also have toocreate a new custom VM image.</span></span> <span data-ttu-id="64c27-121">hello legújabb tanúsítványok automatikusan vannak olyanok, további virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="64c27-121">hello latest certificates are automatically injected as you create additional VMs.</span></span> <span data-ttu-id="64c27-122">Hello teljes folyamat során hello tanúsítványok soha nem hagyja hello Azure platformon, vagy egy parancsfájl, a parancssori előzmények vagy a sablon ki vannak téve.</span><span class="sxs-lookup"><span data-stu-id="64c27-122">During hello whole process, hello certificates never leave hello Azure platform or are exposed in a script, command-line history, or template.</span></span>


## <a name="create-an-azure-key-vault"></a><span data-ttu-id="64c27-123">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="64c27-123">Create an Azure Key Vault</span></span>
<span data-ttu-id="64c27-124">Tanúsítványok és a kulcstároló létrehozásához, hozzon létre egy erőforráscsoportot, a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="64c27-124">Before you can create a Key Vault and certificates, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="64c27-125">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupSecureWeb* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="64c27-125">hello following example creates a resource group named *myResourceGroupSecureWeb* in hello *eastus* location:</span></span>

```azurecli-interactive 
az group create --name myResourceGroupSecureWeb --location eastus
```

<span data-ttu-id="64c27-126">Ezután hozzon létre egy rendelkező Key Vaultból [az keyvault létrehozása](/cli/azure/keyvault#create) és engedélyezi azt használja a virtuális gépek telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="64c27-126">Next, create a Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable it for use when you deploy a VM.</span></span> <span data-ttu-id="64c27-127">Minden Key Vault szükséges egy egyedi nevet, és minden kisbetű kell lennie.</span><span class="sxs-lookup"><span data-stu-id="64c27-127">Each Key Vault requires a unique name, and should be all lower case.</span></span> <span data-ttu-id="64c27-128">Cserélje le  *<mykeyvault>*  a következő példa a saját egyedi névvel Key Vault hello:</span><span class="sxs-lookup"><span data-stu-id="64c27-128">Replace *<mykeyvault>* in hello following example with your own unique Key Vault name:</span></span>

```azurecli-interactive 
keyvault_name=<mykeyvault>
az keyvault create \
    --resource-group myResourceGroupSecureWeb \
    --name $keyvault_name \
    --enabled-for-deployment
```

## <a name="generate-a-certificate-and-store-in-key-vault"></a><span data-ttu-id="64c27-129">Egy tanúsítvány jön létre, és tárolja a Key Vault</span><span class="sxs-lookup"><span data-stu-id="64c27-129">Generate a certificate and store in Key Vault</span></span>
<span data-ttu-id="64c27-130">Az éles környezetben való használathoz, importálnia kell a egy érvényes tanúsítvány megbízható-szolgáltató által aláírt [az keyvault tanúsítvány importálása](/cli/azure/certificate#import).</span><span class="sxs-lookup"><span data-stu-id="64c27-130">For production use, you should import a valid certificate signed by trusted provider with [az keyvault certificate import](/cli/azure/certificate#import).</span></span> <span data-ttu-id="64c27-131">Ebben az oktatóanyagban hello következő példa bemutatja, hogyan hozhat létre egy önaláírt tanúsítványt [az keyvault tanúsítvány létrehozása](/cli/azure/certificate#create) használó hello alapértelmezett tanúsítási szabályzat:</span><span class="sxs-lookup"><span data-stu-id="64c27-131">For this tutorial, hello following example shows how you can generate a self-signed certificate with [az keyvault certificate create](/cli/azure/certificate#create) that uses hello default certificate policy:</span></span>

```azurecli-interactive 
az keyvault certificate create \
    --vault-name $keyvault_name \
    --name mycert \
    --policy "$(az keyvault certificate get-default-policy)"
```

### <a name="prepare-a-certificate-for-use-with-a-vm"></a><span data-ttu-id="64c27-132">Készítse elő a tanúsítványt egy virtuális Géphez való használatra</span><span class="sxs-lookup"><span data-stu-id="64c27-132">Prepare a certificate for use with a VM</span></span>
<span data-ttu-id="64c27-133">toouse hello tanúsítvány során hello VM folyamatot létrehozni, szerezze be a tanúsítvány hello azonosítója [az keyvault lista-verzióját](/cli/azure/keyvault/secret#list-versions).</span><span class="sxs-lookup"><span data-stu-id="64c27-133">toouse hello certificate during hello VM create process, obtain hello ID of your certificate with [az keyvault secret list-versions](/cli/azure/keyvault/secret#list-versions).</span></span> <span data-ttu-id="64c27-134">Átalakítás hello tanúsítvány [az vm formátum-secret](/cli/azure/vm#format-secret).</span><span class="sxs-lookup"><span data-stu-id="64c27-134">Convert hello certificate with [az vm format-secret](/cli/azure/vm#format-secret).</span></span> <span data-ttu-id="64c27-135">a következő példa hello rendeli hozzá ezen parancsok toovariables könnyű használatra a hello hello kimenete lépéseket:</span><span class="sxs-lookup"><span data-stu-id="64c27-135">hello following example assigns hello output of these commands toovariables for ease of use in hello next steps:</span></span>

```azurecli-interactive 
secret=$(az keyvault secret list-versions \
          --vault-name $keyvault_name \
          --name mycert \
          --query "[?attributes.enabled].id" --output tsv)
vm_secret=$(az vm format-secret --secret "$secret")
```

### <a name="create-a-cloud-init-config-toosecure-nginx"></a><span data-ttu-id="64c27-136">Hozzon létre egy felhő-init config toosecure NGINX</span><span class="sxs-lookup"><span data-stu-id="64c27-136">Create a cloud-init config toosecure NGINX</span></span>
<span data-ttu-id="64c27-137">[Felhő inicializálás](https://cloudinit.readthedocs.io) megegyezik egy széles körben használt megközelítés toocustomize Linux virtuális gép elinduló hello az első alkalommal.</span><span class="sxs-lookup"><span data-stu-id="64c27-137">[Cloud-init](https://cloudinit.readthedocs.io) is a widely used approach toocustomize a Linux VM as it boots for hello first time.</span></span> <span data-ttu-id="64c27-138">Felhő inicializálás tooinstall csomagot, és fájlokat, vagy tooconfigure felhasználók és biztonsági írása.</span><span class="sxs-lookup"><span data-stu-id="64c27-138">You can use cloud-init tooinstall packages and write files, or tooconfigure users and security.</span></span> <span data-ttu-id="64c27-139">Felhő inicializálás hello rendszerindítási folyamat során fut, mert nincsenek további lépések nem és az ügynökök tooapply a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="64c27-139">As cloud-init runs during hello initial boot process, there are no additional steps or required agents tooapply your configuration.</span></span>

<span data-ttu-id="64c27-140">Ha hoz létre egy virtuális Gépet, a tanúsítványok és kulcsok tárolódnak védett hello */var/lib/waagent/* könyvtár.</span><span class="sxs-lookup"><span data-stu-id="64c27-140">When you create a VM, certificates and keys are stored in hello protected */var/lib/waagent/* directory.</span></span> <span data-ttu-id="64c27-141">tooautomate hello tanúsítvány hozzáadása toohello virtuális gép és konfigurálás hello webkiszolgáló, használja a felhő inicializálás.</span><span class="sxs-lookup"><span data-stu-id="64c27-141">tooautomate adding hello certificate toohello VM and configuring hello web server, use cloud-init.</span></span> <span data-ttu-id="64c27-142">Ebben a példában azt telepítse és konfigurálja hello NGINX-webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="64c27-142">In this example, we install and configure hello NGINX web server.</span></span> <span data-ttu-id="64c27-143">Használhatja a hello azonos tooinstall feldolgozni, és konfigurálja az Apache.</span><span class="sxs-lookup"><span data-stu-id="64c27-143">You can use hello same process tooinstall and configure Apache.</span></span> 

<span data-ttu-id="64c27-144">Hozzon létre egy fájlt *felhő-init-webalkalmazás-server.txt* és a következő konfigurációs Beillesztés hello:</span><span class="sxs-lookup"><span data-stu-id="64c27-144">Create a file named *cloud-init-web-server.txt* and paste hello following configuration:</span></span>

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 443 ssl;
        ssl_certificate /etc/nginx/ssl/mycert.cert;
        ssl_certificate_key /etc/nginx/ssl/mycert.prv;
      }
runcmd:
  - secretsname=$(find /var/lib/waagent/ -name "*.prv" | cut -c -57)
  - mkdir /etc/nginx/ssl
  - cp $secretsname.crt /etc/nginx/ssl/mycert.cert
  - cp $secretsname.prv /etc/nginx/ssl/mycert.prv
  - service nginx restart
```

### <a name="create-a-secure-vm"></a><span data-ttu-id="64c27-145">Biztonságos virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="64c27-145">Create a secure VM</span></span>
<span data-ttu-id="64c27-146">Most létrehozza a virtuális gép és [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="64c27-146">Now create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="64c27-147">hello tanúsítványának adatait a Key Vault beszúrta a hello `--secrets` paraméter.</span><span class="sxs-lookup"><span data-stu-id="64c27-147">hello certificate data is injected from Key Vault with hello `--secrets` parameter.</span></span> <span data-ttu-id="64c27-148">Adja meg a hello felhő inicializálás konfigurációban a hello `--custom-data` paraméter:</span><span class="sxs-lookup"><span data-stu-id="64c27-148">You pass in hello cloud-init config with hello `--custom-data` parameter:</span></span>

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --custom-data cloud-init-web-server.txt \
    --secrets "$vm_secret"
```

<span data-ttu-id="64c27-149">Hello VM toobe hozott létre, néhány percet vesz igénybe hello csomagok tooinstall és hello app toostart.</span><span class="sxs-lookup"><span data-stu-id="64c27-149">It takes a few minutes for hello VM toobe created, hello packages tooinstall, and hello app toostart.</span></span> <span data-ttu-id="64c27-150">Hello virtuális gép létrehozása, jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített.</span><span class="sxs-lookup"><span data-stu-id="64c27-150">When hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="64c27-151">Ez a cím használt tooaccess van a webhely egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="64c27-151">This address is used tooaccess your site in a web browser.</span></span>

<span data-ttu-id="64c27-152">biztonságos webes forgalom tooreach a virtuális Gépet, nyissa meg 443-as portot a hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="64c27-152">tooallow secure web traffic tooreach your VM, open port 443 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli-interactive 
az vm open-port \
    --resource-group myResourceGroupSecureWeb \
    --name myVM \
    --port 443
```


### <a name="test-hello-secure-web-app"></a><span data-ttu-id="64c27-153">Teszt hello biztonságos webes alkalmazás</span><span class="sxs-lookup"><span data-stu-id="64c27-153">Test hello secure web app</span></span>
<span data-ttu-id="64c27-154">Most nyisson meg egy webböngészőt, és írja be *https://<publicIpAddress>*  hello címsorába.</span><span class="sxs-lookup"><span data-stu-id="64c27-154">Now you can open a web browser and enter *https://<publicIpAddress>* in hello address bar.</span></span> <span data-ttu-id="64c27-155">Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="64c27-155">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="64c27-156">Fogadja el a hello biztonsági figyelmeztetést, ha egy önaláírt tanúsítványt használt:</span><span class="sxs-lookup"><span data-stu-id="64c27-156">Accept hello security warning if you used a self-signed certificate:</span></span>

![Fogadja el a webes böngésző biztonsági figyelmeztetés](./media/tutorial-secure-web-server/browser-warning.png)

<span data-ttu-id="64c27-158">A biztonságos NGINX webhely ezután megjelenik, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="64c27-158">Your secured NGINX site is then displayed as in hello following example:</span></span>

![Futó biztonságos NGINX webhely megtekintése](./media/tutorial-secure-web-server/secured-nginx.png)


## <a name="next-steps"></a><span data-ttu-id="64c27-160">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="64c27-160">Next steps</span></span>

<span data-ttu-id="64c27-161">Ebben az oktatóanyagban az NGINX webkiszolgáló az Azure Key Vault tárolt SSL-tanúsítvánnyal védett.</span><span class="sxs-lookup"><span data-stu-id="64c27-161">In this tutorial, you secured an NGINX web server with an SSL certificate stored in Azure Key Vault.</span></span> <span data-ttu-id="64c27-162">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="64c27-162">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="64c27-163">Egy Azure-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="64c27-163">Create an Azure Key Vault</span></span>
> * <span data-ttu-id="64c27-164">Hozza létre, vagy egy tanúsítvány toohello Key Vault feltöltése</span><span class="sxs-lookup"><span data-stu-id="64c27-164">Generate or upload a certificate toohello Key Vault</span></span>
> * <span data-ttu-id="64c27-165">Hozzon létre egy virtuális Gépet, és hello NGINX-webkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="64c27-165">Create a VM and install hello NGINX web server</span></span>
> * <span data-ttu-id="64c27-166">Hello tanúsítvány behelyezése hello VM és SSL-kötést NGINX konfigurálása</span><span class="sxs-lookup"><span data-stu-id="64c27-166">Inject hello certificate into hello VM and configure NGINX with an SSL binding</span></span>

<span data-ttu-id="64c27-167">Kövesse a hivatkozást toosee előzetesen elkészített virtuálisgép-parancsprogram minták.</span><span class="sxs-lookup"><span data-stu-id="64c27-167">Follow this link toosee pre-built virtual machine script samples.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="64c27-168">Windows virtuális gép parancsfájl minták</span><span class="sxs-lookup"><span data-stu-id="64c27-168">Windows virtual machine script samples</span></span>](./cli-samples.md)
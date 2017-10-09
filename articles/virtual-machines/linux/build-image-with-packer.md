---
title: "aaaHow toocreate Linux Azure VM-rendszerképek a csomagoló |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse csomagoló toocreate képek a Linux virtuális gépek Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/18/2017
ms.author: iainfou
ms.openlocfilehash: 5990598859e73efac477884bc8de5fd5138bf6e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-packer-toocreate-linux-virtual-machine-images-in-azure"></a><span data-ttu-id="c818a-103">Hogyan toouse csomagoló toocreate Linux virtuális gép lemezképek az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="c818a-103">How toouse Packer toocreate Linux virtual machine images in Azure</span></span>
<span data-ttu-id="c818a-104">Minden virtuális gép (VM) az Azure-ban, amely meghatározza a Linux-disztribúció hello és operációsrendszer-verzió lemezkép jön létre.</span><span class="sxs-lookup"><span data-stu-id="c818a-104">Each virtual machine (VM) in Azure is created from an image that defines hello Linux distribution and OS version.</span></span> <span data-ttu-id="c818a-105">Lemezképek előre telepített alkalmazások és konfigurációk tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="c818a-105">Images can include pre-installed applications and configurations.</span></span> <span data-ttu-id="c818a-106">hello Azure piactér lehetővé teszi számos első és harmadik fél rendszerképet leggyakoribb disztribúcióiról, valamint az alkalmazás környezetekben, vagy létrehozhat saját szabott egyéni lemezképek tooyour igényeinek.</span><span class="sxs-lookup"><span data-stu-id="c818a-106">hello Azure Marketplace provides many first and third-party images for most common distributions and application environments, or you can create your own custom images tailored tooyour needs.</span></span> <span data-ttu-id="c818a-107">Ez a cikk részletesen hogyan toouse hello forrás eszköz megnyitásához [csomagoló](https://www.packer.io/) toodefine és -buildek egyéni lemezképek az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c818a-107">This article details how toouse hello open source tool [Packer](https://www.packer.io/) toodefine and build custom images in Azure.</span></span>


## <a name="create-azure-resource-group"></a><span data-ttu-id="c818a-108">Azure erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="c818a-108">Create Azure resource group</span></span>
<span data-ttu-id="c818a-109">Hello létrehozási folyamat során a csomagoló ideiglenes Azure-erőforrások létrehozza a, hello forrás Virtuálisgép-buildekről nyújtanak.</span><span class="sxs-lookup"><span data-stu-id="c818a-109">During hello build process, Packer creates temporary Azure resources as it builds hello source VM.</span></span> <span data-ttu-id="c818a-110">amely a forrás virtuális gép képként használatra toocapture, meg kell adnia egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="c818a-110">toocapture that source VM for use as an image, you must define a resource group.</span></span> <span data-ttu-id="c818a-111">hello hello csomagoló felépítési folyamat kimenetét tárolja Ez az erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="c818a-111">hello output from hello Packer build process is stored in this resource group.</span></span>

<span data-ttu-id="c818a-112">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="c818a-112">Create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="c818a-113">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="c818a-113">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a><span data-ttu-id="c818a-114">Az Azure hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="c818a-114">Create Azure credentials</span></span>
<span data-ttu-id="c818a-115">Csomagoló egyszerű szolgáltatás használatával végzi a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c818a-115">Packer authenticates with Azure using a service principal.</span></span> <span data-ttu-id="c818a-116">Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a csomagoló használható.</span><span class="sxs-lookup"><span data-stu-id="c818a-116">An Azure service principal is a security identity that you can use with apps, services, and automation tools like Packer.</span></span> <span data-ttu-id="c818a-117">Szabályozza, és adja meg hello engedélyek, mivel toowhat műveletek hello szolgáltatás egyszerű hajthat végre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c818a-117">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span>

<span data-ttu-id="c818a-118">Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és kimeneti hello hitelesítő adatait, amelyet a csomagoló:</span><span class="sxs-lookup"><span data-stu-id="c818a-118">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that Packer needs:</span></span>

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

<span data-ttu-id="c818a-119">A parancsok megelőző hello hello kimeneti példát a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="c818a-119">An example of hello output from hello preceding commands is as follows:</span></span>

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

<span data-ttu-id="c818a-120">tooauthenticate tooAzure is szükség van az azonosító az Azure-előfizetéshez tooobtain [az fiók megjelenítése](/cli/azure/account#show):</span><span class="sxs-lookup"><span data-stu-id="c818a-120">tooauthenticate tooAzure, you also need tooobtain your Azure subscription ID with [az account show](/cli/azure/account#show):</span></span>

```azurecli
az account show --query [id] --output tsv
```

<span data-ttu-id="c818a-121">Ez a két parancs kimenetét hello hello következő lépésben használhatja.</span><span class="sxs-lookup"><span data-stu-id="c818a-121">You use hello output from these two commands in hello next step.</span></span>


## <a name="define-packer-template"></a><span data-ttu-id="c818a-122">Adja meg a csomagoló sablon</span><span class="sxs-lookup"><span data-stu-id="c818a-122">Define Packer template</span></span>
<span data-ttu-id="c818a-123">toobuild képek létrehozása sablon JSON-fájlként.</span><span class="sxs-lookup"><span data-stu-id="c818a-123">toobuild images, you create a template as a JSON file.</span></span> <span data-ttu-id="c818a-124">Hello sablonban szerkesztők megadhatja, és végrehajtott tényleges hello provisioners létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="c818a-124">In hello template, you define builders and provisioners that carry out hello actual build process.</span></span> <span data-ttu-id="c818a-125">Csomagoló rendelkezik egy [Azure webhelykiépítőt](https://www.packer.io/docs/builders/azure.html) , amely lehetővé teszi, hogy toodefine Azure erőforrások, például hello szolgáltatás egyszerű hitelesítő adatait az előző lépésben hello létrehozott.</span><span class="sxs-lookup"><span data-stu-id="c818a-125">Packer has a [provisioner for Azure](https://www.packer.io/docs/builders/azure.html) that allows you toodefine Azure resources, such as hello service principal credentials created in hello preceding step.</span></span>

<span data-ttu-id="c818a-126">Hozzon létre egy fájlt *ubuntu.json* és a Beillesztés hello a következő tartalmat.</span><span class="sxs-lookup"><span data-stu-id="c818a-126">Create a file named *ubuntu.json* and paste hello following content.</span></span> <span data-ttu-id="c818a-127">Adja meg a saját értékeit hello következő:</span><span class="sxs-lookup"><span data-stu-id="c818a-127">Enter your own values for hello following:</span></span>

| <span data-ttu-id="c818a-128">Paraméter</span><span class="sxs-lookup"><span data-stu-id="c818a-128">Parameter</span></span>                           | <span data-ttu-id="c818a-129">Ha tooobtain</span><span class="sxs-lookup"><span data-stu-id="c818a-129">Where tooobtain</span></span> |
|-------------------------------------|----------------------------------------------------|
| <span data-ttu-id="c818a-130">*client_id*</span><span class="sxs-lookup"><span data-stu-id="c818a-130">*client_id*</span></span>                         | <span data-ttu-id="c818a-131">Első sor kimenetét `az ad sp` parancs - *appId*</span><span class="sxs-lookup"><span data-stu-id="c818a-131">First line of output from `az ad sp` create command - *appId*</span></span> |
| <span data-ttu-id="c818a-132">*client_secret*</span><span class="sxs-lookup"><span data-stu-id="c818a-132">*client_secret*</span></span>                     | <span data-ttu-id="c818a-133">A második sor kimenetét `az ad sp` parancs - *jelszó*</span><span class="sxs-lookup"><span data-stu-id="c818a-133">Second line of output from `az ad sp` create command - *password*</span></span> |
| <span data-ttu-id="c818a-134">*tenant_id*</span><span class="sxs-lookup"><span data-stu-id="c818a-134">*tenant_id*</span></span>                         | <span data-ttu-id="c818a-135">A kimenet a harmadik sorban `az ad sp` parancs - *bérlői*</span><span class="sxs-lookup"><span data-stu-id="c818a-135">Third line of output from `az ad sp` create command - *tenant*</span></span> |
| <span data-ttu-id="c818a-136">*ELŐFIZETÉS_AZONOSÍTÓJA*</span><span class="sxs-lookup"><span data-stu-id="c818a-136">*subscription_id*</span></span>                   | <span data-ttu-id="c818a-137">A kimeneti `az account show` parancs</span><span class="sxs-lookup"><span data-stu-id="c818a-137">Output from `az account show` command</span></span> |
| <span data-ttu-id="c818a-138">*managed_image_resource_group_name*</span><span class="sxs-lookup"><span data-stu-id="c818a-138">*managed_image_resource_group_name*</span></span> | <span data-ttu-id="c818a-139">Hello első lépésben létrehozott erőforráscsoport nevét</span><span class="sxs-lookup"><span data-stu-id="c818a-139">Name of resource group you created in hello first step</span></span> |
| <span data-ttu-id="c818a-140">*managed_image_name*</span><span class="sxs-lookup"><span data-stu-id="c818a-140">*managed_image_name*</span></span>                | <span data-ttu-id="c818a-141">A létrehozott hello felügyelt lemezképet neve</span><span class="sxs-lookup"><span data-stu-id="c818a-141">Name for hello managed disk image that is created</span></span> |


```json
{
  "builders": [{
    "type": "azure-arm",

    "client_id": "f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
    "client_secret": "0e760437-bf34-4aad-9f8d-870be799c55d",
    "tenant_id": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "subscription_id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx",

    "managed_image_resource_group_name": "myResourceGroup",
    "managed_image_name": "myPackerImage",

    "os_type": "Linux",
    "image_publisher": "Canonical",
    "image_offer": "UbuntuServer",
    "image_sku": "16.04-LTS",

    "azure_tags": {
        "dept": "Engineering",
        "task": "Image deployment"
    },

    "location": "East US",
    "vm_size": "Standard_DS2_v2"
  }],
  "provisioners": [{
    "execute_command": "chmod +x {{ .Path }}; {{ .Vars }} sudo -E sh '{{ .Path }}'",
    "inline": [
      "apt-get update",
      "apt-get upgrade -y",
      "apt-get -y install nginx",

      "/usr/sbin/waagent -force -deprovision+user && export HISTSIZE=0 && sync"
    ],
    "inline_shebang": "/bin/sh -x",
    "type": "shell"
  }]
}
```

<span data-ttu-id="c818a-142">Ez a sablon Ubuntu 16.04 LTS képet alkot, NGINX telepíti, majd hello VM deprovisions.</span><span class="sxs-lookup"><span data-stu-id="c818a-142">This template builds an Ubuntu 16.04 LTS image, installs NGINX, then deprovisions hello VM.</span></span>

> [!NOTE]
> <span data-ttu-id="c818a-143">Bontsa ki a sablon tooprovision hitelesítő adatait, ha úgy dönt, hogy deprovisions hello Azure ügynök tooread hello webhelykiépítőt parancs `-deprovision` helyett `deprovision+user`.</span><span class="sxs-lookup"><span data-stu-id="c818a-143">If you expand on this template tooprovision user credentials, adjust hello provisioner command that deprovisions hello Azure agent tooread `-deprovision` rather than `deprovision+user`.</span></span>
> <span data-ttu-id="c818a-144">Hello `+user` jelző eltávolítja az összes felhasználói fiók hello forrás virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c818a-144">hello `+user` flag removes all user accounts from hello source VM.</span></span>


## <a name="build-packer-image"></a><span data-ttu-id="c818a-145">Csomagoló lemezkép</span><span class="sxs-lookup"><span data-stu-id="c818a-145">Build Packer image</span></span>
<span data-ttu-id="c818a-146">Ha még nincs telepítve a helyi számítógépre csomagoló [hello csomagoló telepítési utasításokat követve](https://www.packer.io/docs/install/index.html).</span><span class="sxs-lookup"><span data-stu-id="c818a-146">If you don't already have Packer installed on your local machine, [follow hello Packer installation instructions](https://www.packer.io/docs/install/index.html).</span></span>

<span data-ttu-id="c818a-147">Megadásával hozhat létre hello kép a csomagoló sablonfájl az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c818a-147">Build hello image by specifying your Packer template file as follows:</span></span>

```bash
./packer build ubuntu.json
```

<span data-ttu-id="c818a-148">A parancsok megelőző hello hello kimeneti példát a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="c818a-148">An example of hello output from hello preceding commands is as follows:</span></span>

```bash
azure-arm output will be in this color.

==> azure-arm: Running builder ...
    azure-arm: Creating Azure Resource Manager (ARM) client ...
==> azure-arm: Creating resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Location          : ‘East US’
==> azure-arm:  -> Tags              :
==> azure-arm:  ->> dept : Engineering
==> azure-arm:  ->> task : Image deployment
==> azure-arm: Validating deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Deploying deployment template ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> DeploymentName    : ‘pkrdpswtxmqm7ly’
==> azure-arm: Getting hello VM’s IP address ...
==> azure-arm:  -> ResourceGroupName   : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> PublicIPAddressName : ‘packerPublicIP’
==> azure-arm:  -> NicName             : ‘packerNic’
==> azure-arm:  -> Network Connection  : ‘PublicEndpoint’
==> azure-arm:  -> IP Address          : ‘40.76.218.147’
==> azure-arm: Waiting for SSH toobecome available...
==> azure-arm: Connected tooSSH!
==> azure-arm: Provisioning with shell script: /var/folders/h1/ymh5bdx15wgdn5hvgj1wc0zh0000gn/T/packer-shell868574263
    azure-arm: WARNING! hello waagent service will be stopped.
    azure-arm: WARNING! Cached DHCP leases will be deleted.
    azure-arm: WARNING! root password will be disabled. You will not be able toologin as root.
    azure-arm: WARNING! /etc/resolvconf/resolv.conf.d/tail and /etc/resolvconf/resolv.conf.d/original will be deleted.
    azure-arm: WARNING! packer account and entire home directory will be deleted.
==> azure-arm: Querying hello machine’s properties ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Managed OS Disk   : ‘/subscriptions/guid/resourceGroups/packer-Resource-Group-swtxmqm7ly/providers/Microsoft.Compute/disks/osdisk’
==> azure-arm: Powering off machine ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> ComputeName       : ‘pkrvmswtxmqm7ly’
==> azure-arm: Capturing image ...
==> azure-arm:  -> Compute ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm:  -> Compute Name              : ‘pkrvmswtxmqm7ly’
==> azure-arm:  -> Compute Location          : ‘East US’
==> azure-arm:  -> Image ResourceGroupName   : ‘myResourceGroup’
==> azure-arm:  -> Image Name                : ‘myPackerImage’
==> azure-arm:  -> Image Location            : ‘eastus’
==> azure-arm: Deleting resource group ...
==> azure-arm:  -> ResourceGroupName : ‘packer-Resource-Group-swtxmqm7ly’
==> azure-arm: Deleting hello temporary OS disk ...
==> azure-arm:  -> OS Disk : skipping, managed disk was used...
Build ‘azure-arm’ finished.

==> Builds finished. hello artifacts of successful builds are:
--> azure-arm: Azure.ResourceManagement.VMImage:

ManagedImageResourceGroupName: myResourceGroup
ManagedImageName: myPackerImage
ManagedImageLocation: eastus
```


## <a name="create-vm-from-azure-image"></a><span data-ttu-id="c818a-149">Kép: Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c818a-149">Create VM from Azure Image</span></span>
<span data-ttu-id="c818a-150">Mostantól létrehozhat egy virtuális Gépet a lemezkép [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="c818a-150">You can now create a VM from your Image with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c818a-151">Adja meg a lemezkép hello segítségével létrehozott hello `--image` paraméter.</span><span class="sxs-lookup"><span data-stu-id="c818a-151">Specify hello Image you created with hello `--image` parameter.</span></span> <span data-ttu-id="c818a-152">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* a *myPackerImage* és az SSH-kulcsokat generál, ha még nem léteznek:</span><span class="sxs-lookup"><span data-stu-id="c818a-152">hello following example creates a VM named *myVM* from *myPackerImage* and generates SSH keys if they do not already exist:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="c818a-153">Néhány perc toocreate hello VM vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c818a-153">It takes a few minutes toocreate hello VM.</span></span> <span data-ttu-id="c818a-154">Hello virtuális gép létrehozása után jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített.</span><span class="sxs-lookup"><span data-stu-id="c818a-154">Once hello VM has been created, take note of hello `publicIpAddress` displayed by hello Azure CLI.</span></span> <span data-ttu-id="c818a-155">Ez a cím használt tooaccess hello NGINX hely webböngésző segítségével.</span><span class="sxs-lookup"><span data-stu-id="c818a-155">This address is used tooaccess hello NGINX site via a web browser.</span></span>

<span data-ttu-id="c818a-156">webes forgalom tooreach a virtuális Gépet, nyissa meg 80-as porton az hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):</span><span class="sxs-lookup"><span data-stu-id="c818a-156">tooallow web traffic tooreach your VM, open port 80 from hello Internet with [az vm open-port](/cli/azure/vm#open-port):</span></span>

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a><span data-ttu-id="c818a-157">Virtuális gép és NGINX tesztelése</span><span class="sxs-lookup"><span data-stu-id="c818a-157">Test VM and NGINX</span></span>
<span data-ttu-id="c818a-158">Most nyisson meg egy webböngészőt, és írja be `http://publicIpAddress` hello címsorába.</span><span class="sxs-lookup"><span data-stu-id="c818a-158">Now you can open a web browser and enter `http://publicIpAddress` in hello address bar.</span></span> <span data-ttu-id="c818a-159">Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c818a-159">Provide your own public IP address from hello VM create process.</span></span> <span data-ttu-id="c818a-160">ahogy a következő példa hello hello alapértelmezett NGINX lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="c818a-160">hello default NGINX page is displayed as in hello following example:</span></span>

![Alapértelmezett NGINX-webhely](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a><span data-ttu-id="c818a-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c818a-162">Next steps</span></span>
<span data-ttu-id="c818a-163">Ebben a példában használt csomagoló toocreate Virtuálisgép-lemezkép NGINX már telepítve.</span><span class="sxs-lookup"><span data-stu-id="c818a-163">In this example, you used Packer toocreate a VM image with NGINX already installed.</span></span> <span data-ttu-id="c818a-164">A Virtuálisgép-lemezkép meglévő központi telepítési munkafolyamatai, például az alkalmazás tooVMs hello Ansible, Chef vagy Puppet lemezkép alapján létrehozott toodeploy együtt használható.</span><span class="sxs-lookup"><span data-stu-id="c818a-164">You can use this VM image alongside existing deployment workflows, such as toodeploy your app tooVMs created from hello Image with Ansible, Chef, or Puppet.</span></span>

<span data-ttu-id="c818a-165">További példa csomagoló sablonokat más Linux disztribúciókkal, lásd: [a GitHub-tárház](https://github.com/hashicorp/packer/tree/master/examples/azure).</span><span class="sxs-lookup"><span data-stu-id="c818a-165">For additional example Packer templates for other Linux distros, see [this GitHub repo](https://github.com/hashicorp/packer/tree/master/examples/azure).</span></span>
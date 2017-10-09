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
# <a name="how-toouse-packer-toocreate-linux-virtual-machine-images-in-azure"></a>Hogyan toouse csomagoló toocreate Linux virtuális gép lemezképek az Azure-ban
Minden virtuális gép (VM) az Azure-ban, amely meghatározza a Linux-disztribúció hello és operációsrendszer-verzió lemezkép jön létre. Lemezképek előre telepített alkalmazások és konfigurációk tartalmazhatnak. hello Azure piactér lehetővé teszi számos első és harmadik fél rendszerképet leggyakoribb disztribúcióiról, valamint az alkalmazás környezetekben, vagy létrehozhat saját szabott egyéni lemezképek tooyour igényeinek. Ez a cikk részletesen hogyan toouse hello forrás eszköz megnyitásához [csomagoló](https://www.packer.io/) toodefine és -buildek egyéni lemezképek az Azure-ban.


## <a name="create-azure-resource-group"></a>Azure erőforráscsoport létrehozása
Hello létrehozási folyamat során a csomagoló ideiglenes Azure-erőforrások létrehozza a, hello forrás Virtuálisgép-buildekről nyújtanak. amely a forrás virtuális gép képként használatra toocapture, meg kell adnia egy erőforráscsoportot. hello hello csomagoló felépítési folyamat kimenetét tárolja Ez az erőforráscsoport.

Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal. hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:

```azurecli
az group create -n myResourceGroup -l eastus
```


## <a name="create-azure-credentials"></a>Az Azure hitelesítő adatok létrehozása
Csomagoló egyszerű szolgáltatás használatával végzi a hitelesítést. Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a csomagoló használható. Szabályozza, és adja meg hello engedélyek, mivel toowhat műveletek hello szolgáltatás egyszerű hajthat végre az Azure-ban.

Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és kimeneti hello hitelesítő adatait, amelyet a csomagoló:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

A parancsok megelőző hello hello kimeneti példát a következőképpen történik:

```azurecli
"f5b6a5cf-fbdf-4a9f-b3b8-3c2cd00225a4",
"0e760437-bf34-4aad-9f8d-870be799c55d",
"72f988bf-86f1-41af-91ab-2d7cd011db47"
```

tooauthenticate tooAzure is szükség van az azonosító az Azure-előfizetéshez tooobtain [az fiók megjelenítése](/cli/azure/account#show):

```azurecli
az account show --query [id] --output tsv
```

Ez a két parancs kimenetét hello hello következő lépésben használhatja.


## <a name="define-packer-template"></a>Adja meg a csomagoló sablon
toobuild képek létrehozása sablon JSON-fájlként. Hello sablonban szerkesztők megadhatja, és végrehajtott tényleges hello provisioners létrehozása folyamatban. Csomagoló rendelkezik egy [Azure webhelykiépítőt](https://www.packer.io/docs/builders/azure.html) , amely lehetővé teszi, hogy toodefine Azure erőforrások, például hello szolgáltatás egyszerű hitelesítő adatait az előző lépésben hello létrehozott.

Hozzon létre egy fájlt *ubuntu.json* és a Beillesztés hello a következő tartalmat. Adja meg a saját értékeit hello következő:

| Paraméter                           | Ha tooobtain |
|-------------------------------------|----------------------------------------------------|
| *client_id*                         | Első sor kimenetét `az ad sp` parancs - *appId* |
| *client_secret*                     | A második sor kimenetét `az ad sp` parancs - *jelszó* |
| *tenant_id*                         | A kimenet a harmadik sorban `az ad sp` parancs - *bérlői* |
| *ELŐFIZETÉS_AZONOSÍTÓJA*                   | A kimeneti `az account show` parancs |
| *managed_image_resource_group_name* | Hello első lépésben létrehozott erőforráscsoport nevét |
| *managed_image_name*                | A létrehozott hello felügyelt lemezképet neve |


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

Ez a sablon Ubuntu 16.04 LTS képet alkot, NGINX telepíti, majd hello VM deprovisions.

> [!NOTE]
> Bontsa ki a sablon tooprovision hitelesítő adatait, ha úgy dönt, hogy deprovisions hello Azure ügynök tooread hello webhelykiépítőt parancs `-deprovision` helyett `deprovision+user`.
> Hello `+user` jelző eltávolítja az összes felhasználói fiók hello forrás virtuális gép.


## <a name="build-packer-image"></a>Csomagoló lemezkép
Ha még nincs telepítve a helyi számítógépre csomagoló [hello csomagoló telepítési utasításokat követve](https://www.packer.io/docs/install/index.html).

Megadásával hozhat létre hello kép a csomagoló sablonfájl az alábbiak szerint:

```bash
./packer build ubuntu.json
```

A parancsok megelőző hello hello kimeneti példát a következőképpen történik:

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


## <a name="create-vm-from-azure-image"></a>Kép: Azure virtuális gép létrehozása
Mostantól létrehozhat egy virtuális Gépet a lemezkép [az virtuális gép létrehozása](/cli/azure/vm#create). Adja meg a lemezkép hello segítségével létrehozott hello `--image` paraméter. hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* a *myPackerImage* és az SSH-kulcsokat generál, ha még nem léteznek:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image myPackerImage \
    --admin-username azureuser \
    --generate-ssh-keys
```

Néhány perc toocreate hello VM vesz igénybe. Hello virtuális gép létrehozása után jegyezze fel a hello `publicIpAddress` hello Azure CLI által megjelenített. Ez a cím használt tooaccess hello NGINX hely webböngésző segítségével.

webes forgalom tooreach a virtuális Gépet, nyissa meg 80-as porton az hello Internet rendelkező tooallow [az vm-port megnyitása](/cli/azure/vm#open-port):

```azurecli
az vm open-port \
    --resource-group myResourceGroup \
    --name myVM \
    --port 80
```

## <a name="test-vm-and-nginx"></a>Virtuális gép és NGINX tesztelése
Most nyisson meg egy webböngészőt, és írja be `http://publicIpAddress` hello címsorába. Adja meg a saját nyilvános IP-címet a virtuális gép hello folyamatot létrehozni. ahogy a következő példa hello hello alapértelmezett NGINX lap jelenik meg:

![Alapértelmezett NGINX-webhely](./media/build-image-with-packer/nginx.png) 


## <a name="next-steps"></a>Következő lépések
Ebben a példában használt csomagoló toocreate Virtuálisgép-lemezkép NGINX már telepítve. A Virtuálisgép-lemezkép meglévő központi telepítési munkafolyamatai, például az alkalmazás tooVMs hello Ansible, Chef vagy Puppet lemezkép alapján létrehozott toodeploy együtt használható.

További példa csomagoló sablonokat más Linux disztribúciókkal, lásd: [a GitHub-tárház](https://github.com/hashicorp/packer/tree/master/examples/azure).
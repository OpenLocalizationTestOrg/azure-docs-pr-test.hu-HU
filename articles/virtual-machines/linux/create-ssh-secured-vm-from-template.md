---
title: "aaaCreate Azure Linux virtuális gép sablon alapján |} Microsoft Docs"
description: "Hogyan toouse hello Azure CLI 2.0 toocreate Linux virtuális gép Resource Manager sablon alapján"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4b8c4103cccbae13c679ff2a2cac928a0e8e809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-with-azure-resource-manager-templates"></a><span data-ttu-id="356fe-103">Hogyan toocreate egy Linux virtuális gép Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="356fe-103">How toocreate a Linux virtual machine with Azure Resource Manager templates</span></span>
<span data-ttu-id="356fe-104">Ez a cikk bemutatja, hogyan telepítsék központilag az tooquickly egy Linux virtuális gép (VM) az Azure Resource Manager-sablonok és hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="356fe-104">This article shows you how tooquickly deploy a Linux virtual machine (VM) with Azure Resource Manager templates and hello Azure CLI 2.0.</span></span> <span data-ttu-id="356fe-105">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="356fe-105">You can also perform these steps with hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).</span></span>


## <a name="templates-overview"></a><span data-ttu-id="356fe-106">Sablonok – áttekintés</span><span class="sxs-lookup"><span data-stu-id="356fe-106">Templates overview</span></span>
<span data-ttu-id="356fe-107">Az Azure Resource Manager-sablonok JSON-fájlokat, amelyek meghatározzák a hello infrastruktúráját és konfigurációját az Azure-megoldás.</span><span class="sxs-lookup"><span data-stu-id="356fe-107">Azure Resource Manager templates are JSON files that define hello infrastructure and configuration of your Azure solution.</span></span> <span data-ttu-id="356fe-108">A sablonok segítségével a megoldás a teljes életciklusa során ismételten üzembe helyezhető, és az erőforrások üzembe helyezése biztosan konzisztens lesz.</span><span class="sxs-lookup"><span data-stu-id="356fe-108">By using a template, you can repeatedly deploy your solution throughout its lifecycle and have confidence your resources are deployed in a consistent state.</span></span> <span data-ttu-id="356fe-109">További információ az hello sablont, és hogyan hozható létre, hello formátuma toolearn lásd: [az első Azure Resource Manager-sablon létrehozása](../../azure-resource-manager/resource-manager-create-first-template.md).</span><span class="sxs-lookup"><span data-stu-id="356fe-109">toolearn more about hello format of hello template and how you construct it, see [Create your first Azure Resource Manager template](../../azure-resource-manager/resource-manager-create-first-template.md).</span></span> <span data-ttu-id="356fe-110">tooview hello JSON-szintaxis erőforrások esetében, tekintse meg [meghatározhatja az erőforrásokat az Azure Resource Manager sablonokban](/azure/templates/).</span><span class="sxs-lookup"><span data-stu-id="356fe-110">tooview hello JSON syntax for resources types, see [Define resources in Azure Resource Manager templates](/azure/templates/).</span></span>


## <a name="create-resource-group"></a><span data-ttu-id="356fe-111">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="356fe-111">Create resource group</span></span>
<span data-ttu-id="356fe-112">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="356fe-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="356fe-113">Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni.</span><span class="sxs-lookup"><span data-stu-id="356fe-113">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="356fe-114">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupVM* a hello *eastus* régió:</span><span class="sxs-lookup"><span data-stu-id="356fe-114">hello following example creates a resource group named *myResourceGroupVM* in hello *eastus* region:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a><span data-ttu-id="356fe-115">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="356fe-115">Create virtual machine</span></span>
<span data-ttu-id="356fe-116">hello alábbi példa létrehoz egy virtuális gép [Azure Resource Manager sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) rendelkező [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create).</span><span class="sxs-lookup"><span data-stu-id="356fe-116">hello following example creates a VM from [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) with [az group deployment create](/cli/azure/group/deployment#create).</span></span> <span data-ttu-id="356fe-117">Adja meg a saját nyilvános SSH-kulcs, például a hello tartalmát hello értékének *~/.ssh/id_rsa.pub*.</span><span class="sxs-lookup"><span data-stu-id="356fe-117">Provide hello value of your own SSH public key, such as hello contents of *~/.ssh/id_rsa.pub*.</span></span> <span data-ttu-id="356fe-118">Ha egy SSH-kulcspárral toocreate van szüksége, tekintse meg [hogyan toocreate és -felhasználási SSH kulcs pár Linux virtuális gépek Azure-ban](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="356fe-118">If you need toocreate an SSH key pair, see [How toocreate and use an SSH key pair for Linux VMs in Azure](mac-create-ssh-keys.md).</span></span>

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

<span data-ttu-id="356fe-119">Ebben a példában a Githubon tárolt sablon adott meg.</span><span class="sxs-lookup"><span data-stu-id="356fe-119">In this example, you specified a template stored in GitHub.</span></span> <span data-ttu-id="356fe-120">Is letöltheti vagy -sablon létrehozása és hello hello helyi elérési utat adjon meg azonos `--template-file` paraméter.</span><span class="sxs-lookup"><span data-stu-id="356fe-120">You can also download or create a template and specify hello local path with hello same `--template-file` parameter.</span></span>

<span data-ttu-id="356fe-121">tooSSH tooyour VM, hello nyilvános IP-cím az beszerzése [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show):</span><span class="sxs-lookup"><span data-stu-id="356fe-121">tooSSH tooyour VM, obtain hello public IP address with [az network public-ip show](/cli/azure/network/public-ip#show):</span></span>

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

<span data-ttu-id="356fe-122">Ezek közül SSH tooyour VM szokásos módon:</span><span class="sxs-lookup"><span data-stu-id="356fe-122">You can then SSH tooyour VM as normal:</span></span>

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a><span data-ttu-id="356fe-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="356fe-123">Next steps</span></span>
<span data-ttu-id="356fe-124">Ebben a példában létrehozott egy alapszintű Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="356fe-124">In this example, you created a basic Linux VM.</span></span> <span data-ttu-id="356fe-125">További Resource Manager-sablonok, amelyek tartalmazzák az alkalmazás-keretrendszerek számára, vagy hozzon létre összetettebb környezetben, tallózással hello [Azure gyors üzembe helyezés sablontárban](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="356fe-125">For more Resource Manager templates that include application frameworks or create more complex environments, browse hello [Azure quickstart templates gallery](https://azure.microsoft.com/documentation/templates/).</span></span>

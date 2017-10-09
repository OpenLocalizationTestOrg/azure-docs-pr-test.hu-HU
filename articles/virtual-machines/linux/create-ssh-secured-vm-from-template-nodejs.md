---
title: "Linux virtuális gépet egy Azure-sablon az Azure CLI 1.0 aaaCreate |} Microsoft Docs"
description: "Linux virtuális gép létrehozása az Azure hello Azure CLI 1.0 és az Azure Resource Manager-sablon használatával."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b694cc8247a8431b7ef4b24cc7dc2b4cdb9660ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-vm-using-hello-azure-cli-10-an-azure-resource-manager-template"></a><span data-ttu-id="383db-103">Hogyan toocreate a Linux virtuális gép hello Azure CLI 1.0 Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="383db-103">How toocreate a Linux VM using hello Azure CLI 1.0 an Azure Resource Manager template</span></span>
<span data-ttu-id="383db-104">Ez a cikk bemutatja, hogyan tooquickly hello Azure CLI 1.0 és az Azure Resource Manager-sablon használatával Linux virtuális gép telepítése.</span><span class="sxs-lookup"><span data-stu-id="383db-104">This article shows you how tooquickly deploy a Linux Virtual Machine using hello Azure CLI 1.0 and an Azure Resource Manager template.</span></span> <span data-ttu-id="383db-105">hello cikk van szükség:</span><span class="sxs-lookup"><span data-stu-id="383db-105">hello article requires:</span></span>

* <span data-ttu-id="383db-106">egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="383db-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="383db-107">Hello [Azure CLI 1.0](../../cli-install-nodejs.md) bejelentkezett `azure login`.</span><span class="sxs-lookup"><span data-stu-id="383db-107">hello [Azure CLI 1.0](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="383db-108">az Azure parancssori felület hello *kell* Azure Resource Manager módra `azure config mode arm`.</span><span class="sxs-lookup"><span data-stu-id="383db-108">hello Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

<span data-ttu-id="383db-109">A Linux Virtuálisgép-sablonok hello segítségével is gyorsan telepíthet [Azure-portálon](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="383db-109">You can also quickly deploy a Linux VM template by using hello [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="383db-110">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="383db-110">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="383db-111">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="383db-111">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="383db-112">[Az Azure CLI 1.0](#quick-command-summary) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="383db-112">[Azure CLI 1.0](#quick-command-summary) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="383db-113">[Az Azure CLI 2.0](create-ssh-secured-vm-from-template.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="383db-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="383db-114">Gyors parancsösszegzés</span><span class="sxs-lookup"><span data-stu-id="383db-114">Quick Command Summary</span></span>
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="383db-115">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="383db-115">Detailed Walkthrough</span></span>
<span data-ttu-id="383db-116">A sablonok segítségével toocreate virtuális gépek Azure-on, amelyet az toocustomize során hello indítási, beállítások, például felhasználónevekkel és állomásnevekkel beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="383db-116">Templates allow you toocreate VMs on Azure with settings that you want toocustomize during hello launch, settings like usernames and hostnames.</span></span> <span data-ttu-id="383db-117">Ebben a cikkben elindítunk egy Ubuntu virtuális gépet használó Azure-sablont, valamint egy hálózati biztonsági csoportot (NSG-t), amelynek a 22-es portja nyitva van az SSH számára.</span><span class="sxs-lookup"><span data-stu-id="383db-117">For this article, we are launching an Azure template utilizing an Ubuntu VM along with a network security group (NSG) with port 22 open for SSH.</span></span>

<span data-ttu-id="383db-118">Az Azure Resource Manager-sablonok JSON-fájlok, amelyek egyszerű, egyszeri feladatokhoz használhatók, például egy Ubuntu virtuális gép indításához, mint ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="383db-118">Azure Resource Manager templates are JSON files that can be used for simple one-off tasks like launching an Ubuntu VM as done in this article.</span></span>  <span data-ttu-id="383db-119">Azure-sablonok is használt tooconstruct összetett Azure-konfigurációjának olyan teljes környezetek tesztelési, fejlesztési és éles telepítési verem.</span><span class="sxs-lookup"><span data-stu-id="383db-119">Azure Templates can also be used tooconstruct complex Azure configurations of entire environments like a testing, dev, or production deployment stack.</span></span>

## <a name="create-hello-linux-vm"></a><span data-ttu-id="383db-120">Hello Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="383db-120">Create hello Linux VM</span></span>
<span data-ttu-id="383db-121">Hogyan kód példa azt mutatja meg a következő hello toocall `azure group create` toocreate erőforrás csoport és az SSH által biztonságossá tett Linux virtuális gép egyidejű hello használatával [Azure Resource Manager sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="383db-121">hello following code example shows how toocall `azure group create` toocreate a resource group and deploy an SSH-secured Linux VM at hello same time using [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span></span> <span data-ttu-id="383db-122">Ne feledje, hogy a példában toouse nevet, amely egyedi tooyour környezetben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="383db-122">Remember that in your example you need toouse names that are unique tooyour environment.</span></span> <span data-ttu-id="383db-123">Ez a példa *myResourceGroup* hello az erőforráscsoport neve, mint és *myVM* hello virtuális gép neve.</span><span class="sxs-lookup"><span data-stu-id="383db-123">This example uses *myResourceGroup* as hello resource group name, and *myVM* as hello VM name.</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

<span data-ttu-id="383db-124">hello kimenete a következő kimeneti blokk hello kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="383db-124">hello output should look like hello following output block:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for hello following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

<span data-ttu-id="383db-125">A példában a virtuális gépek hello telepített `--template-uri` paraméter.</span><span class="sxs-lookup"><span data-stu-id="383db-125">That example deployed a VM using hello `--template-uri` parameter.</span></span>  <span data-ttu-id="383db-126">Is letöltheti vagy létrehozhat egy sablont helyben és hello sablon használatával hello átadni `--template-file` argumentumként elérési toohello sablonfájlt paraméterrel.</span><span class="sxs-lookup"><span data-stu-id="383db-126">You can also download or create a template locally and pass hello template using hello `--template-file` parameter with a path toohello template file as an argument.</span></span> <span data-ttu-id="383db-127">az Azure parancssori felület hello kéri hello paraméterek hello sablonhoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="383db-127">hello Azure CLI prompts you for hello parameters required by hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="383db-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="383db-128">Next steps</span></span>
<span data-ttu-id="383db-129">Keresési hello [sablontárban](https://azure.microsoft.com/documentation/templates/) toodiscover milyen alkalmazás-keretrendszerek toodeploy tovább.</span><span class="sxs-lookup"><span data-stu-id="383db-129">Search hello [templates gallery](https://azure.microsoft.com/documentation/templates/) toodiscover what app frameworks toodeploy next.</span></span>


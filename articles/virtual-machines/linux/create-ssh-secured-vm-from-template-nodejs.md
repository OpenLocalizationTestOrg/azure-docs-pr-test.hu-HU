---
title: "Linux virtuális gép létrehozása Azure-sablon alapján az Azure CLI 1.0 |} Microsoft Docs"
description: "Linux virtuális gép létrehozása az Azure-ban az Azure CLI 1.0 és az Azure Resource Manager-sablon használatával."
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
ms.openlocfilehash: 33d4aaa78fcdf3bd9e2e236606f2d3049f464a8a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-vm-using-the-azure-cli-10-an-azure-resource-manager-template"></a><span data-ttu-id="92942-103">Linux virtuális gépet az Azure CLI 1.0 Azure Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="92942-103">How to create a Linux VM using the Azure CLI 1.0 an Azure Resource Manager template</span></span>
<span data-ttu-id="92942-104">Ez a cikk bemutatja, hogyan helyezhet üzembe gyorsan Linux virtuális gépek az Azure CLI 1.0 és az Azure Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="92942-104">This article shows you how to quickly deploy a Linux Virtual Machine using the Azure CLI 1.0 and an Azure Resource Manager template.</span></span> <span data-ttu-id="92942-105">A cikkben foglaltak végrehajtásához szükség van:</span><span class="sxs-lookup"><span data-stu-id="92942-105">The article requires:</span></span>

* <span data-ttu-id="92942-106">egy Azure-fiókra ([ingyenes próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/)),</span><span class="sxs-lookup"><span data-stu-id="92942-106">an Azure account ([get a free trial](https://azure.microsoft.com/pricing/free-trial/)).</span></span>
* <span data-ttu-id="92942-107">a [Azure CLI 1.0](../../cli-install-nodejs.md) bejelentkezett `azure login`.</span><span class="sxs-lookup"><span data-stu-id="92942-107">the [Azure CLI 1.0](../../cli-install-nodejs.md) logged in with `azure login`.</span></span>
* <span data-ttu-id="92942-108">Az Azure parancssori felületnek `azure config mode arm` Azure Resource Manager módban *kell lennie*.</span><span class="sxs-lookup"><span data-stu-id="92942-108">the Azure CLI *must be in* Azure Resource Manager mode `azure config mode arm`.</span></span>

<span data-ttu-id="92942-109">Az [Azure Portallal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) is gyorsan üzembe helyezhet Linux rendszerű virtuálisgép-sablonokat.</span><span class="sxs-lookup"><span data-stu-id="92942-109">You can also quickly deploy a Linux VM template by using the [Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="92942-110">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="92942-110">CLI versions to complete the task</span></span>
<span data-ttu-id="92942-111">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="92942-111">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="92942-112">[Az Azure CLI 1.0](#quick-command-summary) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="92942-112">[Azure CLI 1.0](#quick-command-summary) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="92942-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="92942-113">[Azure CLI 2.0](create-ssh-secured-vm-from-template.md) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-command-summary"></a><span data-ttu-id="92942-114">Gyors parancsösszegzés</span><span class="sxs-lookup"><span data-stu-id="92942-114">Quick Command Summary</span></span>
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="92942-115">Részletes bemutató</span><span class="sxs-lookup"><span data-stu-id="92942-115">Detailed Walkthrough</span></span>
<span data-ttu-id="92942-116">A sablonokkal virtuális gépeket hozhat létre az Azure-ban az indításkor testre szabni kívánt beállításokkal, például felhasználónevekkel és állomásnevekkel.</span><span class="sxs-lookup"><span data-stu-id="92942-116">Templates allow you to create VMs on Azure with settings that you want to customize during the launch, settings like usernames and hostnames.</span></span> <span data-ttu-id="92942-117">Ebben a cikkben elindítunk egy Ubuntu virtuális gépet használó Azure-sablont, valamint egy hálózati biztonsági csoportot (NSG-t), amelynek a 22-es portja nyitva van az SSH számára.</span><span class="sxs-lookup"><span data-stu-id="92942-117">For this article, we are launching an Azure template utilizing an Ubuntu VM along with a network security group (NSG) with port 22 open for SSH.</span></span>

<span data-ttu-id="92942-118">Az Azure Resource Manager-sablonok JSON-fájlok, amelyek egyszerű, egyszeri feladatokhoz használhatók, például egy Ubuntu virtuális gép indításához, mint ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="92942-118">Azure Resource Manager templates are JSON files that can be used for simple one-off tasks like launching an Ubuntu VM as done in this article.</span></span>  <span data-ttu-id="92942-119">Az Azure-sablonokkal teljes környezetek összetett Azure-konfigurációi is létrehozhatók, például a tesztelési, fejlesztési és éles üzembe helyezési vermek.</span><span class="sxs-lookup"><span data-stu-id="92942-119">Azure Templates can also be used to construct complex Azure configurations of entire environments like a testing, dev, or production deployment stack.</span></span>

## <a name="create-the-linux-vm"></a><span data-ttu-id="92942-120">A Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="92942-120">Create the Linux VM</span></span>
<span data-ttu-id="92942-121">Az alábbi kódpélda az `azure group create` parancs hívását mutatja be egy erőforráscsoport és az SSH által biztonságossá tett Linux virtuális gép egyidejű létrehozásához [ezen Azure Resource Manager-sablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) használatával.</span><span class="sxs-lookup"><span data-stu-id="92942-121">The following code example shows how to call `azure group create` to create a resource group and deploy an SSH-secured Linux VM at the same time using [this Azure Resource Manager template](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json).</span></span> <span data-ttu-id="92942-122">Ne feledje, hogy a példában a környezet egyedi neveit kell használni.</span><span class="sxs-lookup"><span data-stu-id="92942-122">Remember that in your example you need to use names that are unique to your environment.</span></span> <span data-ttu-id="92942-123">Ez a példa *myResourceGroup* az erőforráscsoport neve, mint és *myVM* Virtuálisgép-nevet.</span><span class="sxs-lookup"><span data-stu-id="92942-123">This example uses *myResourceGroup* as the resource group name, and *myVM* as the VM name.</span></span>

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

<span data-ttu-id="92942-124">A kimenetnek az alábbi kimeneti blokkhoz hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="92942-124">The output should look like the following output block:</span></span>

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for the following parameters
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

<span data-ttu-id="92942-125">A példa a `--template-uri` paraméter használatával helyezett üzembe egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="92942-125">That example deployed a VM using the `--template-uri` parameter.</span></span>  <span data-ttu-id="92942-126">Emellett letölthet vagy létrehozhat egy sablont helyben, majd átadhatja a sablont a `--template-file` paraméterrel a sablonfájl elérési útját használva argumentumként.</span><span class="sxs-lookup"><span data-stu-id="92942-126">You can also download or create a template locally and pass the template using the `--template-file` parameter with a path to the template file as an argument.</span></span> <span data-ttu-id="92942-127">Az Azure parancssori felület felszólítja a sablonhoz szükséges paraméterek megadására.</span><span class="sxs-lookup"><span data-stu-id="92942-127">The Azure CLI prompts you for the parameters required by the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="92942-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="92942-128">Next steps</span></span>
<span data-ttu-id="92942-129">A [sablontárban](https://azure.microsoft.com/documentation/templates/) indított kereséssel derítheti ki, hogy mely alkalmazás-keretrendszert helyezze üzembe következőként.</span><span class="sxs-lookup"><span data-stu-id="92942-129">Search the [templates gallery](https://azure.microsoft.com/documentation/templates/) to discover what app frameworks to deploy next.</span></span>


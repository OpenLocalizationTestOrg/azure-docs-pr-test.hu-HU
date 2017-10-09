---
title: "Linux virtuális gépek Azure-ban aaaRedeploy |} Microsoft Docs"
description: "Hogyan tooredeploy Linux virtuális gépek Azure toomitigate SSH-kapcsolat ad ki."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 9adfd1b11f262d362133366b2bba5e69c70c9b82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-linux-virtual-machine-toonew-azure-node"></a><span data-ttu-id="58309-103">Telepítse újra a Linux virtuális gép toonew Azure csomópont</span><span class="sxs-lookup"><span data-stu-id="58309-103">Redeploy Linux virtual machine toonew Azure node</span></span>
<span data-ttu-id="58309-104">Ha Ön szembesülhetnek SSH hibaelhárítási problémák vagy alkalmazás tooa Linux virtuális gép (VM) elérni az Azure-ban, újratelepíteni a virtuális gép hello segíthet.</span><span class="sxs-lookup"><span data-stu-id="58309-104">If you face difficulties troubleshooting SSH or application access tooa Linux virtual machine (VM) in Azure, redeploying hello VM may help.</span></span> <span data-ttu-id="58309-105">A virtuális gép újbóli telepítésének, hello VM tooa új csomópont belül hello Azure-infrastruktúra helyezi át, és majd bekapcsolja azt vissza.</span><span class="sxs-lookup"><span data-stu-id="58309-105">When you redeploy a VM, it moves hello VM tooa new node within hello Azure infrastructure and then powers it back on.</span></span> <span data-ttu-id="58309-106">A konfigurációs beállításokat és a kapcsolódó erőforrások jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="58309-106">All your configuration options and associated resources are retained.</span></span> <span data-ttu-id="58309-107">Ez a cikk bemutatja, hogyan tooredeploy a virtuális gépet az Azure CLI vagy hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="58309-107">This article shows you how tooredeploy a VM using Azure CLI or hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="58309-108">Miután a virtuális gép újbóli telepítésének, hello mennyiségű ideiglenes lemezes elvesztését, és dinamikus IP-címek társított virtuális hálózati illesztő frissítése.</span><span class="sxs-lookup"><span data-stu-id="58309-108">After you redeploy a VM, hello temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 

<span data-ttu-id="58309-109">A virtuális gépek hello a következő lehetőségek egyikének használatával központilag telepítheti.</span><span class="sxs-lookup"><span data-stu-id="58309-109">You can redeploy a VM using one of hello following options.</span></span> <span data-ttu-id="58309-110">Csak kell toochoose egy beállítás tooredeploy a virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="58309-110">You only need toochoose one option tooredeploy your VM:</span></span>

- [<span data-ttu-id="58309-111">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="58309-111">Azure CLI 2.0</span></span>](#azure-cli-20)
- [<span data-ttu-id="58309-112">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="58309-112">Azure CLI 1.0</span></span>](#azure-cli-10)
- [<span data-ttu-id="58309-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="58309-113">Azure portal</span></span>](#using-azure-portal)

## <a name="use-hello-azure-cli-20"></a><span data-ttu-id="58309-114">Azure CLI 2.0 hello használata</span><span class="sxs-lookup"><span data-stu-id="58309-114">Use hello Azure CLI 2.0</span></span>
<span data-ttu-id="58309-115">Legutóbbi telepítés hello [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="58309-115">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="58309-116">Telepítse újra a virtuális Gépet a [az vm helyezze üzembe újra](/cli/azure/vm#redeploy).</span><span class="sxs-lookup"><span data-stu-id="58309-116">Redeploy your VM with [az vm redeploy](/cli/azure/vm#redeploy).</span></span> <span data-ttu-id="58309-117">a következő példa redeploys hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="58309-117">hello following example redeploys hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-hello-azure-cli-10"></a><span data-ttu-id="58309-118">Hello Azure CLI 1.0 használata</span><span class="sxs-lookup"><span data-stu-id="58309-118">Use hello Azure CLI 1.0</span></span>
<span data-ttu-id="58309-119">Hello telepítése [Azure CLI legújabb 1.0](../../cli-install-nodejs.md), jelentkezzen be Azure-fiók tooan, és győződjön meg arról, hogy éppen erőforrás-kezelő módban (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="58309-119">Install hello [latest Azure CLI 1.0](../../cli-install-nodejs.md), log in tooan Azure account, and make sure that you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="58309-120">a következő példa redeploys hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="58309-120">hello following example redeploys hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="58309-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58309-121">Next steps</span></span>
<span data-ttu-id="58309-122">Csatlakozás a virtuális gép tooyour problémát tapasztal, ha található segítséget [SSH-kapcsolatok hibáinak elhárítása](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [részletes hibaelhárítási SSH](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58309-122">If you are having issues connecting tooyour VM, you can find specific help on [troubleshooting SSH connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [detailed SSH troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="58309-123">Ha nem fér hozzá a virtuális gép futó alkalmazást, is olvasható [problémák elhárítása alkalmazás](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="58309-123">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


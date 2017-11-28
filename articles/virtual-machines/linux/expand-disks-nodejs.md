---
title: "a Linux virtuális gép lemez az operációs rendszer aaaExpand hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan tooexpand hello hello Azure CLI 1.0 és hello Resource Manager üzembe helyezési modellben Linux virtuális gép virtuális merevlemezére operációs rendszer"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 0db78c0b86b48b2c5358611e11bb0b7ad781a559
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expand-os-disk-on-a-linux-vm-using-hello-azure-cli-with-hello-azure-cli-10"></a><span data-ttu-id="5e2cc-103">Bontsa ki a hello Azure CLI használata hello Azure CLI 1.0 Linux virtuális gép operációsrendszer-lemez</span><span class="sxs-lookup"><span data-stu-id="5e2cc-103">Expand OS disk on a Linux VM using hello Azure CLI with hello Azure CLI 1.0</span></span>
<span data-ttu-id="5e2cc-104">hello alapértelmezett virtuális merevlemez hello operációs rendszer mérete általában 30 GB Linux virtuális gépre (VM) az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-104">hello default virtual hard disk size for hello operating system (OS) is typically 30 GB on a Linux virtual machine (VM) in Azure.</span></span> <span data-ttu-id="5e2cc-105">Is [adatok lemezek hozzáadása a](add-disk.md) tooprovide további tárhelyet, de az is előfordulhat, hogy kívánja tooexpand hello operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-105">You can [add data disks](add-disk.md) tooprovide for additional storage space, but you may also wish tooexpand hello OS disk.</span></span> <span data-ttu-id="5e2cc-106">Ez a cikk részletesen, hogyan tooexpand hello az operációs rendszer nem kezelt lemezek használata hello Azure CLI 1.0 Linux virtuális lemez.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-106">This article details how tooexpand hello OS disk for a Linux VM using unmanaged disks with hello Azure CLI 1.0.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="5e2cc-107">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="5e2cc-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="5e2cc-108">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="5e2cc-109">[Az Azure CLI 1.0](#prerequisites) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="5e2cc-109">[Azure CLI 1.0](#prerequisites) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="5e2cc-110">[Az Azure CLI 2.0](expand-disks.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="5e2cc-110">[Azure CLI 2.0](expand-disks.md) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e2cc-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5e2cc-111">Prerequisites</span></span>
<span data-ttu-id="5e2cc-112">Hello kell [Azure CLI legújabb 1.0](../../cli-install-nodejs.md) telepítve, és bejelentkezett a tooan [Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) használatával hello Resource Manager módra az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-112">You need hello [latest Azure CLI 1.0](../../cli-install-nodejs.md) installed and logged in tooan [Azure account](https://azure.microsoft.com/pricing/free-trial/) using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="5e2cc-113">Hello következő mintákat, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-113">In hello following samples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5e2cc-114">Példa paraméter nevek a következők *myResourceGroup* és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-114">Example parameter names include *myResourceGroup* and *myVM*.</span></span>

## <a name="expand-os-disk"></a><span data-ttu-id="5e2cc-115">Operációsrendszer-lemez kibontása</span><span class="sxs-lookup"><span data-stu-id="5e2cc-115">Expand OS disk</span></span>

1. <span data-ttu-id="5e2cc-116">A virtuális merevlemezeken műveletek nem hajthatók végre a virtuális gép hello futtatása.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-116">Operations on virtual hard disks cannot be performed with hello VM running.</span></span> <span data-ttu-id="5e2cc-117">hello alábbi példa leáll, és felszabadítja a hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-117">hello following example stops and deallocates hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --name myVM
    ```

    > [!NOTE]
    > <span data-ttu-id="5e2cc-118">`azure vm stop`nem mentesíti hello számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-118">`azure vm stop` does not release hello compute resources.</span></span> <span data-ttu-id="5e2cc-119">toorelease számítási erőforrásokat, használja a `azure vm deallocate`.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-119">toorelease compute resources, use `azure vm deallocate`.</span></span> <span data-ttu-id="5e2cc-120">virtuális gép hello tooexpand hello virtuális merevlemezt kell felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-120">hello VM must be deallocated tooexpand hello virtual hard disk.</span></span>

2. <span data-ttu-id="5e2cc-121">Hello segítségével a nem felügyelt hello operációsrendszer-lemez mérete hello frissítése `azure vm set` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-121">Update hello size of hello unmanaged OS disk using hello `azure vm set` command.</span></span> <span data-ttu-id="5e2cc-122">a következő példa frissítések hello hello nevű virtuális gép *myVM* nevű hello erőforráscsoportban *myResourceGroup* toobe *50* GB:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-122">hello following example updates hello VM named *myVM* in hello resource group named *myResourceGroup* toobe *50* GB:</span></span>

    ```azurecli
    azure vm set \
        --resource-group myResourceGroup \
        --name myVM \
        --new-os-disk-size 50
    ```

3. <span data-ttu-id="5e2cc-123">Indítsa el a virtuális Gépet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-123">Start your VM as follows:</span></span>

    ```azurecli
    azure vm start --resource-group myResourceGroup --name myVM
    ```

4. <span data-ttu-id="5e2cc-124">SSH tooyour VM hello megfelelő hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-124">SSH tooyour VM with hello appropriate credentials.</span></span> <span data-ttu-id="5e2cc-125">tooverify hello operációsrendszer-lemez át lett méretezve, használja a `df -h`.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-125">tooverify hello OS disk has been resized, use `df -h`.</span></span> <span data-ttu-id="5e2cc-126">a következő egy példa a kimenetre hello hello elsődleges partíció jeleníti meg (*/dev/sda1*) 50 GB-os áll:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-126">hello following example output shows hello primary partition (*/dev/sda1*) is now 50 GB:</span></span>

    ```bash
    Filesystem      Size  Used Avail Use% Mounted on
    udev            1.7G     0  1.7G   0% /dev
    tmpfs           344M  5.0M  340M   2% /run
    /dev/sda1        49G  1.3G   48G   3% /
    ```

## <a name="next-steps"></a><span data-ttu-id="5e2cc-127">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e2cc-127">Next steps</span></span>
<span data-ttu-id="5e2cc-128">Ha szeretne további tárhely, akkor is [adatok lemezek tooa Linux virtuális gép hozzáadása](add-disk.md).</span><span class="sxs-lookup"><span data-stu-id="5e2cc-128">If you need additional storage, you also [add data disks tooa Linux VM](add-disk.md).</span></span> <span data-ttu-id="5e2cc-129">Lemez titkosításával kapcsolatos további információkért lásd: [Linux virtuális gép titkosítása lemezein hello Azure CLI](encrypt-disks.md).</span><span class="sxs-lookup"><span data-stu-id="5e2cc-129">For more information about disk encryption, see [Encrypt disks on a Linux VM using hello Azure CLI](encrypt-disks.md).</span></span>

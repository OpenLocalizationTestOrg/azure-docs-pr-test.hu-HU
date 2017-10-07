---
title: "a Linux virtuális gépek Azure-ban nem felügyelt aaaConvert lemezek toomanaged lemezek - Azure felügyelt lemezek |} Microsoft Docs"
description: "Hogyan tooconvert egy Linux virtuális gép nem felügyelt lemezek toomanaged lemezek hello Resource Manager üzembe helyezési modellel Azure CLI 2.0 használatával"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 1b94da11deab46f344e28ab4491cf220506b6347
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-linux-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a><span data-ttu-id="b14b7-103">Egy Linux virtuális gép átalakítása nem felügyelt lemezek toomanaged lemezekből</span><span class="sxs-lookup"><span data-stu-id="b14b7-103">Convert a Linux virtual machine from unmanaged disks toomanaged disks</span></span>

<span data-ttu-id="b14b7-104">Ha meglévő Linux virtuális gépek (VM), a nem felügyelt lemezeket használó, hello virtuális gépek felügyelt toouse lemezek keresztül hello átválthat [Azure felügyelt lemezek](../windows/managed-disks-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b14b7-104">If you have existing Linux virtual machines (VMs) that use unmanaged disks, you can convert hello VMs toouse managed disks through hello [Azure Managed Disks](../windows/managed-disks-overview.md) service.</span></span> <span data-ttu-id="b14b7-105">Ez a folyamat hello operációsrendszer-lemez és a mellékelt adatok lemezzel alakítja át.</span><span class="sxs-lookup"><span data-stu-id="b14b7-105">This process converts both hello OS disk and any attached data disks.</span></span>

<span data-ttu-id="b14b7-106">Ez a cikk bemutatja, hogyan használatával virtuális gépek tooconvert hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="b14b7-106">This article shows you how tooconvert VMs by using hello Azure CLI.</span></span> <span data-ttu-id="b14b7-107">Ha tooinstall kell, vagy frissíteni, lásd: [Azure CLI 2.0 telepítése](/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b14b7-107">If you need tooinstall or upgrade it, see [Install Azure CLI 2.0](/cli/azure/install-azure-cli).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="b14b7-108">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="b14b7-108">Before you begin</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]


## <a name="convert-single-instance-vms"></a><span data-ttu-id="b14b7-109">Egypéldányos virtuális gépek átalakítása</span><span class="sxs-lookup"><span data-stu-id="b14b7-109">Convert single-instance VMs</span></span>
<span data-ttu-id="b14b7-110">Ez a szakasz ismerteti, hogyan tooconvert egypéldányos Azure virtuális gépek nem felügyelt kódból felügyelt lemezek, toomanaged lemezek.</span><span class="sxs-lookup"><span data-stu-id="b14b7-110">This section covers how tooconvert single-instance Azure VMs from unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="b14b7-111">(Ha a virtuális gépek rendelkezésre állási csoportba, lásd: hello következő szakaszt.) A folyamat tooconvert hello virtuális gépek nem felügyelt prémium (SSD) lemezeket felügyelt toopremium lemezek vagy (HDD) szabvány a nem felügyelt lemezek felügyelt toostandard lemezek is használhatók.</span><span class="sxs-lookup"><span data-stu-id="b14b7-111">(If your VMs are in an availability set, see hello next section.) You can use this process tooconvert hello VMs from premium (SSD) unmanaged disks toopremium managed disks, or from standard (HDD) unmanaged disks toostandard managed disks.</span></span>

1. <span data-ttu-id="b14b7-112">Hello virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="b14b7-112">Deallocate hello VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="b14b7-113">hello alábbi példa felszabadítja a hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b14b7-113">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

2. <span data-ttu-id="b14b7-114">Alakítsa át a hello VM toomanaged lemezeket használatával [az virtuális gép átalakítása](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="b14b7-114">Convert hello VM toomanaged disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="b14b7-115">a következő folyamat konvertálja hello hello nevű virtuális gép `myVM`hello operációsrendszer-lemez és bármely adatlemezek is beleértve:</span><span class="sxs-lookup"><span data-stu-id="b14b7-115">hello following process converts hello VM named `myVM`, including hello OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="b14b7-116">Hello VM indítása után hello átalakítás toomanaged lemezek használatával [az vm indítása](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="b14b7-116">Start hello VM after hello conversion toomanaged disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="b14b7-117">a következő példában elindul hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="b14b7-117">hello following example starts hello VM named `myVM` in hello resource group named `myResourceGroup`.</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="b14b7-118">Alakítsa át a virtuális gépek rendelkezésre állási csoportba</span><span class="sxs-lookup"><span data-stu-id="b14b7-118">Convert VMs in an availability set</span></span>

<span data-ttu-id="b14b7-119">Ha hello virtuális gépeket, amelyet a rendelkezésre állási csoportok tooconvert toomanaged lemezek vannak, akkor először kell tooconvert hello rendelkezésre állási készlet tooa felügyelt rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="b14b7-119">If hello VMs that you want tooconvert toomanaged disks are in an availability set, you first need tooconvert hello availability set tooa managed availability set.</span></span>

<span data-ttu-id="b14b7-120">Hello rendelkezésre állási csoport virtuális gépeinek felszabadítása kell, mielőtt hello rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="b14b7-120">All VMs in hello availability set must be deallocated before you convert hello availability set.</span></span> <span data-ttu-id="b14b7-121">Az összes virtuális gépek toomanaged lemez után hello rendelkezésre állási beállítása maga terv tooconvert lett felügyelt konvertált tooa rendelkezésre állási csoport.</span><span class="sxs-lookup"><span data-stu-id="b14b7-121">Plan tooconvert all VMs toomanaged disks after hello availability set itself has been converted tooa managed availability set.</span></span> <span data-ttu-id="b14b7-122">Ezután indítsa el az összes hello virtuális gép, és folytatja a normál.</span><span class="sxs-lookup"><span data-stu-id="b14b7-122">Then, start all hello VMs and continue operating as normal.</span></span>

1. <span data-ttu-id="b14b7-123">Minden virtuális gép rendelkezésre állási készlet használatával listájában [az virtuális gép rendelkezésre állási-készlet lista](/cli/azure/vm/availability-set#list).</span><span class="sxs-lookup"><span data-stu-id="b14b7-123">List all VMs in an availability set by using [az vm availability-set list](/cli/azure/vm/availability-set#list).</span></span> <span data-ttu-id="b14b7-124">hello alábbi példa felsorolja az összes virtuális gépek hello a rendelkezésre állási csoportban elnevezett `myAvailabilitySet` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b14b7-124">hello following example lists all VMs in hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set show \
        --resource-group myResourceGroup \
        --name myAvailabilitySet \
        --query [virtualMachines[*].id] \
        --output table
    ```

2. <span data-ttu-id="b14b7-125">Minden hello virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="b14b7-125">Deallocate all hello VMs by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="b14b7-126">hello alábbi példa felszabadítja a hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b14b7-126">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    ```

3. <span data-ttu-id="b14b7-127">Hello rendelkezésre állási csoport használatával alakítsa át [az a virtuális gép rendelkezésre állási-készlet konvertálás](/cli/azure/vm/availability-set#convert).</span><span class="sxs-lookup"><span data-stu-id="b14b7-127">Convert hello availability set by using [az vm availability-set convert](/cli/azure/vm/availability-set#convert).</span></span> <span data-ttu-id="b14b7-128">hello alábbi példa konvertál hello rendelkezésre állási csoport elnevezett `myAvailabilitySet` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b14b7-128">hello following example converts hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm availability-set convert \
        --resource-group myResourceGroup \
        --name myAvailabilitySet
    ```

4. <span data-ttu-id="b14b7-129">Alakítsa át az összes hello virtuális gépek toomanaged lemez használatával [az virtuális gép átalakítása](/cli/azure/vm#convert).</span><span class="sxs-lookup"><span data-stu-id="b14b7-129">Convert all hello VMs toomanaged disks by using [az vm convert](/cli/azure/vm#convert).</span></span> <span data-ttu-id="b14b7-130">a következő folyamat konvertálja hello hello nevű virtuális gép `myVM`hello operációsrendszer-lemez és bármely adatlemezek is beleértve:</span><span class="sxs-lookup"><span data-stu-id="b14b7-130">hello following process converts hello VM named `myVM`, including hello OS disk and any data disks:</span></span>

    ```azurecli
    az vm convert --resource-group myResourceGroup --name myVM
    ```

5. <span data-ttu-id="b14b7-131">Indítsa el a hello virtuális gépeinek után hello átalakítás toomanaged lemezek segítségével [az vm indítása](/cli/azure/vm#start).</span><span class="sxs-lookup"><span data-stu-id="b14b7-131">Start all hello VMs after hello conversion toomanaged disks by using [az vm start](/cli/azure/vm#start).</span></span> <span data-ttu-id="b14b7-132">a következő példában elindul hello hello nevű virtuális gép `myVM` nevű hello erőforráscsoportban `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="b14b7-132">hello following example starts hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span>

    ```azurecli
    az vm start --resource-group myResourceGroup --name myVM
    ```

## <a name="next-steps"></a><span data-ttu-id="b14b7-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b14b7-133">Next steps</span></span>
<span data-ttu-id="b14b7-134">További információ a tárolási lehetőségek közül választhat: [Azure felügyelt lemezekhez – áttekintés](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b14b7-134">For more information about storage options, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span>

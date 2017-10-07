---
title: "virtuális gépek (klasszikus) - Azure CLI 1.0 aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek (klasszikus) hello Azure parancssori felület (CLI) 1.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 417a57181bcf5c2e6101bf3bdf63fc94ebc99df5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-cli-10"></a><span data-ttu-id="f3e31-103">Egy virtuális gép (klasszikus) Azure CLI 1.0 hello saját IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f3e31-103">Configure private IP addresses for a virtual machine (Classic) using hello Azure CLI 1.0</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="f3e31-104">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="f3e31-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="f3e31-105">Emellett [hello Resource Manager üzembe helyezési modellel statikus magánhálózati IP-cím kezelése](virtual-networks-static-private-ip-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f3e31-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-cli.md).</span></span>

<span data-ttu-id="f3e31-106">minta hello Azure CLI-t az alábbi parancsok már létrehozott egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="f3e31-106">hello sample Azure CLI commands below expect a simple environment already created.</span></span> <span data-ttu-id="f3e31-107">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="f3e31-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="f3e31-108">Hogyan toospecify egy statikus magánhálózati IP-virtuális gép létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="f3e31-108">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="f3e31-109">toocreate nevű új virtuális gép *DNS01* új felhőszolgáltatásban nevű *TestService* a fenti hello forgatókönyv alapján, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f3e31-109">toocreate a new VM named *DNS01* in a new cloud service named *TestService* based on hello scenario above, follow these steps:</span></span>

1. <span data-ttu-id="f3e31-110">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="f3e31-110">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="f3e31-111">Futtassa a hello **azure-szolgáltatás létrehozása** toocreate hello felhőszolgáltatás parancsot.</span><span class="sxs-lookup"><span data-stu-id="f3e31-111">Run hello **azure service create** command toocreate hello cloud service.</span></span>
   
        azure service create TestService --location uscentral
   
    <span data-ttu-id="f3e31-112">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f3e31-112">Expected output:</span></span>
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. <span data-ttu-id="f3e31-113">Futtassa a hello **azure virtuális gép létrehozása** parancs toocreate hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f3e31-113">Run hello **azure create vm** command toocreate hello VM.</span></span> <span data-ttu-id="f3e31-114">Figyelje meg, hogy egy statikus magánhálózati IP-cím hello értéke.</span><span class="sxs-lookup"><span data-stu-id="f3e31-114">Notice hello value for a static private IP address.</span></span> <span data-ttu-id="f3e31-115">hello kimenet után látható hello lista hello paramétereket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f3e31-115">hello list shown after hello output explains hello parameters used.</span></span>
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    <span data-ttu-id="f3e31-116">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f3e31-116">Expected output:</span></span>
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting too"Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * <span data-ttu-id="f3e31-117">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-117">**-l (or --location)**.</span></span> <span data-ttu-id="f3e31-118">Azure-régió, ahol a virtuális gép hello létrejön.</span><span class="sxs-lookup"><span data-stu-id="f3e31-118">Azure region where hello VM will be created.</span></span> <span data-ttu-id="f3e31-119">A mi esetünkben *centralus*.</span><span class="sxs-lookup"><span data-stu-id="f3e31-119">For our scenario, *centralus*.</span></span>
   * <span data-ttu-id="f3e31-120">**-n (vagy--virtuálisgép-név)**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-120">**-n (or --vm-name)**.</span></span> <span data-ttu-id="f3e31-121">Hello VM toobe létrehozott neve.</span><span class="sxs-lookup"><span data-stu-id="f3e31-121">Name of hello VM toobe created.</span></span>
   * <span data-ttu-id="f3e31-122">**-l (vagy--virtuális hálózat neve)**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-122">**-w (or --virtual-network-name)**.</span></span> <span data-ttu-id="f3e31-123">Hello ahol létrejön a virtuális gép hello VNet neve.</span><span class="sxs-lookup"><span data-stu-id="f3e31-123">Name of hello VNet where hello VM will be created.</span></span> 
   * <span data-ttu-id="f3e31-124">**-S (vagy--statikus ip-)**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-124">**-S (or --static-ip)**.</span></span> <span data-ttu-id="f3e31-125">Statikus magánhálózati IP-címet hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="f3e31-125">Static private IP address for hello VM.</span></span>
   * <span data-ttu-id="f3e31-126">**TestService**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-126">**TestService**.</span></span> <span data-ttu-id="f3e31-127">Ahol létrejön a virtuális gép hello hello felhőalapú szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="f3e31-127">Name of hello cloud service where hello VM will be created.</span></span>
   * <span data-ttu-id="f3e31-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-128">**bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**.</span></span> <span data-ttu-id="f3e31-129">Kép toocreate hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="f3e31-129">Image used toocreate hello VM.</span></span>
   * <span data-ttu-id="f3e31-130">**adminuser**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-130">**adminuser**.</span></span> <span data-ttu-id="f3e31-131">Hello Windows virtuális gép helyi rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="f3e31-131">Local administrator for hello Windows VM.</span></span>
   * <span data-ttu-id="f3e31-132">**AdminP@ssw0rd**.</span><span class="sxs-lookup"><span data-stu-id="f3e31-132">**AdminP@ssw0rd**.</span></span> <span data-ttu-id="f3e31-133">Hello Windows virtuális gép helyi rendszergazda jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f3e31-133">Local administrator password for hello Windows VM.</span></span>

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="f3e31-134">Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="f3e31-134">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="f3e31-135">tooview hello statikus magánhálózati IP-címe cím VM létre hello parancsfájl fenti, Futtatás a következő parancs az Azure parancssori felület hello hello adatait, és tekintse meg az hello értéke *hálózati StaticIP*:</span><span class="sxs-lookup"><span data-stu-id="f3e31-135">tooview hello static private IP address information for hello VM created with hello script above, run hello following Azure CLI command and observe hello value for *Network StaticIP*:</span></span>

    azure vm static-ip show DNS01

<span data-ttu-id="f3e31-136">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f3e31-136">Expected output:</span></span>

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="f3e31-137">Hogyan tooremove egy statikus magánhálózati IP-VM</span><span class="sxs-lookup"><span data-stu-id="f3e31-137">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="f3e31-138">tooremove hello statikus magánhálózati IP-cím toohello VM hello parancsfájlban felett, a következő Azure CLI-parancs futtatása hello hozzáadva:</span><span class="sxs-lookup"><span data-stu-id="f3e31-138">tooremove hello static private IP address added toohello VM in hello script above, run hello following Azure CLI command:</span></span>

    azure vm static-ip remove DNS01

<span data-ttu-id="f3e31-139">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f3e31-139">Expected output:</span></span>

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-tooadd-a-static-private-ip-tooan-existing-vm"></a><span data-ttu-id="f3e31-140">Hogyan tooadd statikus magánhálózati IP-tooan meglévő virtuális gép</span><span class="sxs-lookup"><span data-stu-id="f3e31-140">How tooadd a static private IP tooan existing VM</span></span>
<span data-ttu-id="f3e31-141">a statikus magánhálózati IP cím toohello runt a fenti hello parancsfájl a következő parancs használatával létrehozott virtuális gép tooadd:</span><span class="sxs-lookup"><span data-stu-id="f3e31-141">tooadd a static private IP address toohello VM created using hello script above, runt he following command:</span></span>

    azure vm static-ip set DNS01 192.168.1.101

<span data-ttu-id="f3e31-142">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f3e31-142">Expected output:</span></span>

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a><span data-ttu-id="f3e31-143">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3e31-143">Next steps</span></span>
* <span data-ttu-id="f3e31-144">További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="f3e31-144">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="f3e31-145">További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="f3e31-145">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="f3e31-146">Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="f3e31-146">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>


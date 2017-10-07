---
title: "aaaUnderstand megosztott IP-címek a Azure DevTest Labs szolgáltatásban |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure DevTest Labs használja a megosztott IP címek toominimize hello nyilvános IP címek szükséges tooaccess a labor virtuális gépeken."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 8756410117a9d550d567d372174bf1ea96703c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a><span data-ttu-id="3f1c8-103">Megosztott IP-címek az Azure DevTest Labs ismertetése</span><span class="sxs-lookup"><span data-stu-id="3f1c8-103">Understand shared IP addresses in Azure DevTest Labs</span></span>

<span data-ttu-id="3f1c8-104">Az Azure DevTest Labs segítségével tesztlabor virtuális gépek megosztás hello azonos nyilvános IP-cím toominimize hello száma szükséges tooaccess nyilvános IP címek a egyedi labor virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-104">Azure DevTest Labs lets lab VMs share hello same public IP address toominimize hello number of public IP addresses required tooaccess your individual lab VMs.</span></span>  <span data-ttu-id="3f1c8-105">Ez a cikk ismerteti, hogyan megosztott IP-címek munkahelyi és a kapcsolódó beállítási lehetőségekre.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-105">This article describes how shared IPs work and their related configuration options.</span></span>

## <a name="shared-ip-setting"></a><span data-ttu-id="3f1c8-106">A megosztott IP-beállítása</span><span class="sxs-lookup"><span data-stu-id="3f1c8-106">Shared IP setting</span></span>

<span data-ttu-id="3f1c8-107">Ha a labor létrehozása a virtuális hálózati alhálózat helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-107">When you create a lab, it resides in a subnet of a virtual network.</span></span>  <span data-ttu-id="3f1c8-108">Alapértelmezés szerint ez az alhálózat jön létre **engedélyezése megosztott nyilvános IP-cím** túl beállítása*Igen*.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-108">By default, this subnet is created with **Enable shared public IP** set too*Yes*.</span></span>  <span data-ttu-id="3f1c8-109">Ez a konfiguráció egyetlen nyilvános IP-cím hello teljes alhálózathoz tartozó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-109">This configuration creates one public IP address for hello entire subnet.</span></span>  <span data-ttu-id="3f1c8-110">Virtuális hálózatok és alhálózatok konfigurálásával kapcsolatos további információkért lásd: [virtuális hálózat konfigurálása az Azure DevTest Labs](devtest-lab-configure-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="3f1c8-110">For more information about configuring virtual networks and subnets, see [Configure a virtual network in Azure DevTest Labs](devtest-lab-configure-vnet.md).</span></span>

![Új labor alhálózat](media/devtest-lab-shared-ip/lab-subnet.png)

<span data-ttu-id="3f1c8-112">Meglévő labs, engedélyezheti a beállítás kiválasztásával **konfigurációs és házirendek > virtuális hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-112">For existing labs, you can enable this option by selecting **Configuration and policies > Virtual Networks**.</span></span> <span data-ttu-id="3f1c8-113">Ezt követően válassza ki a virtuális hálózat hello listából, és válassza **engedélyezése megosztott nyilvános IP-cím** a kijelölt alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-113">Then, select a virtual network from hello list and choose **ENABLE SHARED PUBLIC IP** for a selected subnet.</span></span> <span data-ttu-id="3f1c8-114">Letilthatja ezt a beállítást, a laborban is, ha nem szeretné, hogy a nyilvános IP-cím tooshare tesztlabor virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-114">You can also disable this option in any lab if you don't want tooshare a public IP address across lab VMs.</span></span>

<span data-ttu-id="3f1c8-115">A virtuális gépek a labor alapértelmezett toousing létre egy megosztott IP-cím.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-115">Any VMs created in this lab default toousing a shared IP.</span></span>  <span data-ttu-id="3f1c8-116">Hello virtuális gép létrehozásakor a beállítás figyelhető-e meg hello **speciális beállítások** részen **IP-címkonfigurációt**.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-116">When creating hello VM, this setting can be observed in hello **Advanced settings** blade under **IP address configuration**.</span></span>

![Új virtuális gép](media/devtest-lab-shared-ip/new-vm.png)

- <span data-ttu-id="3f1c8-118">**Megosztott:** létrehozandó virtuális gépeinek **megosztott** bekerülnek egy erőforráscsoportban (RG).</span><span class="sxs-lookup"><span data-stu-id="3f1c8-118">**Shared:** All VMs created as **Shared** are placed into one resource group (RG).</span></span> <span data-ttu-id="3f1c8-119">Egy egyetlen IP-cím hozzá van rendelve, az adott Entitáshoz és hello RG virtuális gépeinek fogja használni az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-119">A single IP address is assigned for that RG and all VMs in hello RG will use that IP address.</span></span>
- <span data-ttu-id="3f1c8-120">**Nyilvános:** minden virtuális gép létrehozása a saját IP-címmel rendelkezik, és a saját erőforráscsoportban jön létre.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-120">**Public:** Every VM you create has its own IP address and is created in its own resource group.</span></span>
- <span data-ttu-id="3f1c8-121">**Saját:** minden virtuális Géphez létrejön egy privát IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-121">**Private:** Every VM you create uses a private IP address.</span></span> <span data-ttu-id="3f1c8-122">Csak akkor tudja tooconnect toothis VM hello közvetlenül az interneten a távoli asztal.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-122">You will not be able tooconnect toothis VM directly from hello internet with Remote Desktop.</span></span>

<span data-ttu-id="3f1c8-123">Virtuális gép és a megosztott IP engedélyezve toohello alhálózati ad hozzá, amikor a DevTest Labs automatikusan hello VM tooa terheléselosztó hozzáadása, és hozzárendeli a hello nyilvános IP-cím, az TCP-portszámot továbbítási toohello hello virtuális gép RDP-portjához.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-123">Whenever a VM with shared IP enabled is added toohello subnet, DevTest Labs automatically adds hello VM tooa load balancer and assigns a TCP port number on hello public IP address, forwarding toohello RDP port on hello VM.</span></span>  

## <a name="using-hello-shared-ip"></a><span data-ttu-id="3f1c8-124">IP megosztott hello használata</span><span class="sxs-lookup"><span data-stu-id="3f1c8-124">Using hello shared IP</span></span>

- <span data-ttu-id="3f1c8-125">**Linux-felhasználók:** hello IP-címét vagy teljesen minősített tartománynevét, majd egy kettőspontot a virtuális gép SSH-toohello hello port követ.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-125">**Linux users:** SSH toohello VM by using hello IP address or fully qualified domain name, followed by a colon, followed by hello port.</span></span> <span data-ttu-id="3f1c8-126">A hello alábbi rendszerképek közül, hogy hello RDP például cím a virtuális gép tooconnect toohello `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-126">For example, in hello image below, hello RDP address tooconnect toohello VM is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span></span>

  ![Virtuális gép – példa](media/devtest-lab-shared-ip/vm-info.png)

- <span data-ttu-id="3f1c8-128">**Windows-felhasználók:** válassza hello **Connect** hello Azure portál toodownload gombra egy előre megadott RDP-fájlt, és hozzáférési hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="3f1c8-128">**Windows users:** Select hello **Connect** button on hello Azure portal toodownload a pre-configured RDP file and access hello VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f1c8-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3f1c8-129">Next steps</span></span>

* [<span data-ttu-id="3f1c8-130">Labor házirendeket definiálhat az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="3f1c8-130">Define lab policies in Azure DevTest Labs</span></span>](devtest-lab-set-lab-policy.md)
* [<span data-ttu-id="3f1c8-131">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f1c8-131">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md)






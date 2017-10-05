---
title: "Megosztott IP-címek az Azure DevTest Labs megértése |} Microsoft Docs"
description: "Ismerje meg, hogyan Azure DevTest Labs megosztott IP-címeket használó minimalizálása érdekében a nyilvános IP-címek, a labor virtuális gépek eléréséhez szükséges."
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
ms.openlocfilehash: 9f6e1980bf5ea5b41da98a135d89f1c5159921a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a><span data-ttu-id="0cc46-103">Megosztott IP-címek az Azure DevTest Labs ismertetése</span><span class="sxs-lookup"><span data-stu-id="0cc46-103">Understand shared IP addresses in Azure DevTest Labs</span></span>

<span data-ttu-id="0cc46-104">Az Azure DevTest Labs lehetővé teszi, hogy a labor virtuális gépeken megosztani az azonos nyilvános IP-címet a virtuális gépek egyes labor eléréséhez szükséges nyilvános IP-címek számának minimálisra csökkentése.</span><span class="sxs-lookup"><span data-stu-id="0cc46-104">Azure DevTest Labs lets lab VMs share the same public IP address to minimize the number of public IP addresses required to access your individual lab VMs.</span></span>  <span data-ttu-id="0cc46-105">Ez a cikk ismerteti, hogyan megosztott IP-címek munkahelyi és a kapcsolódó beállítási lehetőségekre.</span><span class="sxs-lookup"><span data-stu-id="0cc46-105">This article describes how shared IPs work and their related configuration options.</span></span>

## <a name="shared-ip-setting"></a><span data-ttu-id="0cc46-106">A megosztott IP-beállítása</span><span class="sxs-lookup"><span data-stu-id="0cc46-106">Shared IP setting</span></span>

<span data-ttu-id="0cc46-107">Ha a labor létrehozása a virtuális hálózati alhálózat helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="0cc46-107">When you create a lab, it resides in a subnet of a virtual network.</span></span>  <span data-ttu-id="0cc46-108">Alapértelmezés szerint ez az alhálózat jön létre **engedélyezése megosztott nyilvános IP-cím** beállítása *Igen*.</span><span class="sxs-lookup"><span data-stu-id="0cc46-108">By default, this subnet is created with **Enable shared public IP** set to *Yes*.</span></span>  <span data-ttu-id="0cc46-109">Ez a konfiguráció egyetlen nyilvános IP-cím, a teljes alhálózathoz tartozó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0cc46-109">This configuration creates one public IP address for the entire subnet.</span></span>  <span data-ttu-id="0cc46-110">Virtuális hálózatok és alhálózatok konfigurálásával kapcsolatos további információkért lásd: [virtuális hálózat konfigurálása az Azure DevTest Labs](devtest-lab-configure-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="0cc46-110">For more information about configuring virtual networks and subnets, see [Configure a virtual network in Azure DevTest Labs](devtest-lab-configure-vnet.md).</span></span>

![Új labor alhálózat](media/devtest-lab-shared-ip/lab-subnet.png)

<span data-ttu-id="0cc46-112">Meglévő labs, engedélyezheti a beállítás kiválasztásával **konfigurációs és házirendek > virtuális hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="0cc46-112">For existing labs, you can enable this option by selecting **Configuration and policies > Virtual Networks**.</span></span> <span data-ttu-id="0cc46-113">Ezután egy virtuális hálózatot válasszon a listából, és válassza a **engedélyezése megosztott nyilvános IP-cím** a kijelölt alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="0cc46-113">Then, select a virtual network from the list and choose **ENABLE SHARED PUBLIC IP** for a selected subnet.</span></span> <span data-ttu-id="0cc46-114">Letilthatja ezt a beállítást, a laborban is, ha nem szeretné a nyilvános IP-cím megosztani a labor virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="0cc46-114">You can also disable this option in any lab if you don't want to share a public IP address across lab VMs.</span></span>

<span data-ttu-id="0cc46-115">Bármely a labor beállítást, hogy egy megosztott IP-cím használatával létrehozott virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="0cc46-115">Any VMs created in this lab default to using a shared IP.</span></span>  <span data-ttu-id="0cc46-116">A virtuális gép létrehozásakor a beállítás figyelhető meg a **speciális beállítások** részen **IP-címkonfigurációt**.</span><span class="sxs-lookup"><span data-stu-id="0cc46-116">When creating the VM, this setting can be observed in the **Advanced settings** blade under **IP address configuration**.</span></span>

![Új virtuális gép](media/devtest-lab-shared-ip/new-vm.png)

- <span data-ttu-id="0cc46-118">**Megosztott:** létrehozandó virtuális gépeinek **megosztott** bekerülnek egy erőforráscsoportban (RG).</span><span class="sxs-lookup"><span data-stu-id="0cc46-118">**Shared:** All VMs created as **Shared** are placed into one resource group (RG).</span></span> <span data-ttu-id="0cc46-119">Egy egyetlen IP-cím hozzá van rendelve, az adott Entitáshoz és a RG virtuális gépeinek fogja használni az IP-címet.</span><span class="sxs-lookup"><span data-stu-id="0cc46-119">A single IP address is assigned for that RG and all VMs in the RG will use that IP address.</span></span>
- <span data-ttu-id="0cc46-120">**Nyilvános:** minden virtuális gép létrehozása a saját IP-címmel rendelkezik, és a saját erőforráscsoportban jön létre.</span><span class="sxs-lookup"><span data-stu-id="0cc46-120">**Public:** Every VM you create has its own IP address and is created in its own resource group.</span></span>
- <span data-ttu-id="0cc46-121">**Saját:** minden virtuális Géphez létrejön egy privát IP-címet használja.</span><span class="sxs-lookup"><span data-stu-id="0cc46-121">**Private:** Every VM you create uses a private IP address.</span></span> <span data-ttu-id="0cc46-122">Nem lehet csatlakozni a virtuális gép közvetlenül a távoli asztal az internetről.</span><span class="sxs-lookup"><span data-stu-id="0cc46-122">You will not be able to connect to this VM directly from the internet with Remote Desktop.</span></span>

<span data-ttu-id="0cc46-123">Virtuális gép megosztott IP engedélyezve és az alhálózati ad hozzá, amikor a DevTest Labs automatikusan hozzáadja a virtuális gép egy terheléselosztó és rendeli hozzá a nyilvános IP-cím, az TCP-portszámot, a virtuális gép RDP-portjára továbbítja.</span><span class="sxs-lookup"><span data-stu-id="0cc46-123">Whenever a VM with shared IP enabled is added to the subnet, DevTest Labs automatically adds the VM to a load balancer and assigns a TCP port number on the public IP address, forwarding to the RDP port on the VM.</span></span>  

## <a name="using-the-shared-ip"></a><span data-ttu-id="0cc46-124">A megosztott IP használatával</span><span class="sxs-lookup"><span data-stu-id="0cc46-124">Using the shared IP</span></span>

- <span data-ttu-id="0cc46-125">**Linux-felhasználók:** SSH-kapcsolatot a virtuális Gépet, az IP-címét vagy teljesen minősített tartománynevét, majd egy kettőspontot, a port követ.</span><span class="sxs-lookup"><span data-stu-id="0cc46-125">**Linux users:** SSH to the VM by using the IP address or fully qualified domain name, followed by a colon, followed by the port.</span></span> <span data-ttu-id="0cc46-126">Például az alábbi képen a RDP csatlakozni a virtuális gép címe `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span><span class="sxs-lookup"><span data-stu-id="0cc46-126">For example, in the image below, the RDP address to connect to the VM is `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.</span></span>

  ![Virtuális gép – példa](media/devtest-lab-shared-ip/vm-info.png)

- <span data-ttu-id="0cc46-128">**Windows-felhasználók:** válassza ki a **Connect** gomb az Azure portál egy előre konfigurált RDP-fájl letöltésére, és a virtuális gép eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0cc46-128">**Windows users:** Select the **Connect** button on the Azure portal to download a pre-configured RDP file and access the VM.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0cc46-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0cc46-129">Next steps</span></span>

* [<span data-ttu-id="0cc46-130">Labor házirendeket definiálhat az Azure DevTest Labs szolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="0cc46-130">Define lab policies in Azure DevTest Labs</span></span>](devtest-lab-set-lab-policy.md)
* [<span data-ttu-id="0cc46-131">A Azure DevTest Labs szolgáltatásban virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0cc46-131">Configure a virtual network in Azure DevTest Labs</span></span>](devtest-lab-configure-vnet.md)






---
title: "magánhálózati IP-címek aaaConfigure virtuális gépek (klasszikus) – az Azure portálon |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek (klasszikus) hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: df4bfa6768fc9e66db89785b633ffdb0274dbc46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-hello-azure-portal"></a><span data-ttu-id="24ef4-103">Egy virtuális gép (klasszikus) Azure-portálon hello saját IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="24ef4-103">Configure private IP addresses for a virtual machine (Classic) using hello Azure portal</span></span>

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="24ef4-104">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="24ef4-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="24ef4-105">Emellett [hello Resource Manager üzembe helyezési modellel statikus magánhálózati IP-cím kezelése](virtual-networks-static-private-ip-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="24ef4-105">You can also [manage a static private IP address in hello Resource Manager deployment model](virtual-networks-static-private-ip-arm-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="24ef4-106">hello minta lépéseket már létrehozott egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="24ef4-106">hello sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="24ef4-107">Ha ebben a dokumentumban megjelenített szeretné toorun hello lépéseket, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="24ef4-107">If you want toorun hello steps as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-classic-pportal.md).</span></span>

## <a name="how-toospecify-a-static-private-ip-address-when-creating-a-vm"></a><span data-ttu-id="24ef4-108">Hogyan toospecify egy statikus magánhálózati IP-virtuális gép létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="24ef4-108">How toospecify a static private IP address when creating a VM</span></span>
<span data-ttu-id="24ef4-109">toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="24ef4-109">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet* with a static private IP of *192.168.1.101*, follow hello steps below:</span></span>

1. <span data-ttu-id="24ef4-110">Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="24ef4-110">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="24ef4-111">Kattintson a **új** > **számítási** > **Windows Server 2012 R2 Datacenter**, figyelje meg, hogy hello **telepítésimodellkiválasztása** lista már bemutatja **klasszikus**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="24ef4-111">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that hello **Select a deployment model** list already shows **Classic**, and then click **Create**.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. <span data-ttu-id="24ef4-113">A hello **hozzon létre virtuális gép** panelen adja meg a létrehozott hello VM toobe hello nevét (*DNS01* a mi esetünkben), hello helyi rendszergazda fiókot és jelszót.</span><span class="sxs-lookup"><span data-stu-id="24ef4-113">In hello **Create VM** blade, enter hello name of hello VM toobe created (*DNS01* in our scenario), hello local administrator account, and password.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. <span data-ttu-id="24ef4-115">Kattintson a **opcionális konfigurációs** > **hálózati** > **virtuális hálózati**, és kattintson a **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="24ef4-115">Click **Optional Configuration** > **Network** > **Virtual Network**, and then click **TestVNet**.</span></span> <span data-ttu-id="24ef4-116">Ha **TestVNet** nem érhető el, ellenőrizze, hogy azok be hello *USA középső RÉGIÓJA* helyét, és ez a cikk hello elején leírt tesztkörnyezet hello hozott létre.</span><span class="sxs-lookup"><span data-stu-id="24ef4-116">If **TestVNet** is not available, make sure you are using hello *Central US* location and have created hello test environment described at hello beginning of this article.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. <span data-ttu-id="24ef4-118">A hello **hálózati** panelen ellenőrizze kijelölt meg arról, hogy hello alhálózat van *előtér*, majd kattintson a **IP-címek**, a **IP-cím hozzárendelése** kattintson **statikus**, majd adja meg *192.168.1.101* a **IP-cím** az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="24ef4-118">In hello **Network** blade, make sure hello subnet currently selected is *FrontEnd*, then click **IP addresses**, under **IP address assignment** click **Static**, and then enter *192.168.1.101* for **IP Address** as seen below.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. <span data-ttu-id="24ef4-120">Kattintson a **OK** a hello **IP-címek** panelen kattintson a **OK** a hello **hálózati** panelen, majd kattintson **OK**a hello **opcionális konfigurációs** panelen.</span><span class="sxs-lookup"><span data-stu-id="24ef4-120">Click **OK** in hello **IP addresses** blade, then click **OK** in hello **Network** blade, and click **OK** in hello **Optional config** blade.</span></span>
7. <span data-ttu-id="24ef4-121">A hello **hozzon létre virtuális gép** panelen kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="24ef4-121">In hello **Create VM** blade, click **Create**.</span></span> <span data-ttu-id="24ef4-122">Értesítés hello alábbi megjelenített mozaik az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="24ef4-122">Notice hello tile below displayed in your dashboard.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="24ef4-124">Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="24ef4-124">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="24ef4-125">tooview hello statikus magánhálózati IP-címadatok hello hello lépéseket, a létrehozott virtuális gép hello lépésekkel hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="24ef4-125">tooview hello static private IP address information for hello VM created with hello steps above, execute hello steps below.</span></span>

1. <span data-ttu-id="24ef4-126">Hello Azure Azure portálon kattintson **összes TALLÓZÁSA** > **virtuális gépek (klasszikus)** > **DNS01**  >   **Minden beállítás** > **IP-címek** , és figyelje meg, hello IP-cím hozzárendelése és IP-cím, az alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="24ef4-126">From hello Azure Azure portal, click **BROWSE ALL** > **Virtual machines (classic)** > **DNS01** > **All settings** > **IP addresses** and notice hello IP address assignment and IP address as seen below.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="24ef4-128">Hogyan tooremove egy statikus magánhálózati IP-VM</span><span class="sxs-lookup"><span data-stu-id="24ef4-128">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="24ef4-129">tooremove hello statikus magánhálózati IP-cím a virtuális gép, a fenti létrehozott hello hajtsa végre az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="24ef4-129">tooremove hello static private IP address from hello VM created above, follow hello steps below.</span></span>

1. <span data-ttu-id="24ef4-130">A hello **IP-címek** a fent látható panelen kattintson **dinamikus** toohello jobbra **IP-cím hozzárendelése**, majd kattintson **mentése**, majd Kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="24ef4-130">From hello **IP addresses** blade shown above, click **Dynamic** toohello right of **IP address assignment**, then click **Save**, and then click **Yes**.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="24ef4-132">Hogyan tooadd statikus magánhálózati IP-címe kezelje a meglévő virtuális gép tooan</span><span class="sxs-lookup"><span data-stu-id="24ef4-132">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="24ef4-133">a statikus magánhálózati IP cím toohello hello fenti lépések használatával létrehozott virtuális gép tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="24ef4-133">tooadd a static private IP address toohello VM created using hello steps above, follow hello steps below:</span></span>

1. <span data-ttu-id="24ef4-134">A hello **IP-címek** panelen látható a fenti kattintson **statikus** jobbra toohello **IP-cím hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="24ef4-134">From hello **IP addresses** blade shown above, click **Static** toohello right of **IP address assignment**.</span></span>
2. <span data-ttu-id="24ef4-135">Típus *192.168.1.101* a **IP-cím**, majd kattintson a **mentése**, és kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="24ef4-135">Type *192.168.1.101* for **IP address**, then click **Save**, and then click **Yes**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="24ef4-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="24ef4-136">Next steps</span></span>
* <span data-ttu-id="24ef4-137">További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="24ef4-137">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="24ef4-138">További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="24ef4-138">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="24ef4-139">Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="24ef4-139">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>


---
title: "virtuális gépek - Azure-portálon aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek hello Azure-portálon."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 474161303cdf8cb98e16ffd7cef6b74debdbc49a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-portal"></a><span data-ttu-id="435c9-103">Hello Azure-portál használatával virtuális gépek magánhálózati IP-címek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="435c9-103">Configure private IP addresses for a virtual machine using hello Azure portal</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="435c9-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="435c9-104">Azure portal</span></span>](virtual-networks-static-private-ip-arm-pportal.md)
> * [<span data-ttu-id="435c9-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="435c9-105">PowerShell</span></span>](virtual-networks-static-private-ip-arm-ps.md)
> * [<span data-ttu-id="435c9-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="435c9-106">Azure CLI</span></span>](virtual-networks-static-private-ip-arm-cli.md)
> * [<span data-ttu-id="435c9-107">(Klasszikus) Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="435c9-107">Azure portal (Classic)</span></span>](virtual-networks-static-private-ip-classic-pportal.md)
> * [<span data-ttu-id="435c9-108">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="435c9-108">PowerShell (Classic)</span></span>](virtual-networks-static-private-ip-classic-ps.md)
> * [<span data-ttu-id="435c9-109">Az Azure CLI (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="435c9-109">Azure CLI (Classic)</span></span>](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="435c9-110">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="435c9-110">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="435c9-111">Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="435c9-111">You can also [manage static private IP address in hello classic deployment model](virtual-networks-static-private-ip-classic-pportal.md).</span></span>

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

<span data-ttu-id="435c9-112">hello minta lépéseket már létrehozott egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="435c9-112">hello sample steps below expect a simple environment already created.</span></span> <span data-ttu-id="435c9-113">Ha ebben a dokumentumban megjelenített szeretné toorun hello lépéseket, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="435c9-113">If you want toorun hello steps as they are displayed in this document, first build hello test environment described in [create a vnet](virtual-networks-create-vnet-arm-pportal.md).</span></span>

## <a name="how-toocreate-a-vm-for-testing-static-private-ip-addresses"></a><span data-ttu-id="435c9-114">Hogyan toocreate statikus magánhálózati IP-címe tesztelési virtuális gép címek</span><span class="sxs-lookup"><span data-stu-id="435c9-114">How toocreate a VM for testing static private IP addresses</span></span>
<span data-ttu-id="435c9-115">Nem be hozzá statikus magánhálózati IP-cím hello létrehozásakor a virtuális gépek erőforrás-kezelő a telepítési módban hello hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="435c9-115">You cannot set a static private IP address during hello creation of a VM in hello Resource Manager deployment mode by using hello Azure portal.</span></span> <span data-ttu-id="435c9-116">Először létre kell hoznia a virtuális gép hello, illeszti be a magánhálózati IP-toobe statikus.</span><span class="sxs-lookup"><span data-stu-id="435c9-116">You must create hello VM first, tehn set its private IP toobe static.</span></span>

<span data-ttu-id="435c9-117">toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet*, kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="435c9-117">toocreate a VM named *DNS01* in hello *FrontEnd* subnet of a VNet named *TestVNet*, follow hello steps below.</span></span>

1. <span data-ttu-id="435c9-118">Egy böngészőből keresse meg a toohttp://portal.azure.com, és ha szükséges, jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="435c9-118">From a browser, navigate toohttp://portal.azure.com and, if necessary, sign in with your Azure account.</span></span>
2. <span data-ttu-id="435c9-119">Kattintson a **új** > **számítási** > **Windows Server 2012 R2 Datacenter**, figyelje meg, hogy hello **telepítésimodellkiválasztása** lista már bemutatja **erőforrás-kezelő**, és kattintson a **létrehozása**, hello az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="435c9-119">Click **NEW** > **Compute** > **Windows Server 2012 R2 Datacenter**, notice that hello **Select a deployment model** list already shows **Resource Manager**, and then click **Create**, as seen in hello figure below.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. <span data-ttu-id="435c9-121">A hello **alapjai** panelen adja meg a létrehozott hello VM toobe hello nevét (*DNS01* a mi esetünkben), hello helyi rendszergazda fiókot és jelszót, hello az alábbi ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="435c9-121">In hello **Basics** blade, enter hello name of hello VM toobe created (*DNS01* in our scenario), hello local administrator account, and password, as seen in hello figure below.</span></span>
   
    ![Alapvető beállítások panel](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. <span data-ttu-id="435c9-123">Győződjön meg arról, hogy hello **hely** kiválasztott *USA középső RÉGIÓJA*, kattintson a **válasszon meglévő** alatt **erőforráscsoport**, majd kattintson az **Erőforráscsoport** újra, majd kattintson a *TestRG*, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="435c9-123">Make sure hello **Location** selected is *Central US*, then click **Select existing** under **Resource group**, then click **Resource group** again, then click *TestRG*, and then click **OK**.</span></span>
   
    ![Alapvető beállítások panel](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. <span data-ttu-id="435c9-125">A hello **méret kiválasztása** panelen válassza **A1 szabványos**, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="435c9-125">In hello **Choose a size** blade, select **A1 Standard**, and then click **Select**.</span></span>
   
    ![Válassza ki a méret panelen](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. <span data-ttu-id="435c9-127">Az a **beállítások** panelen, győződjön meg arról, hogy hello következő tulajdonságai vannak beállítva az alábbi hello értékekkel van beállítva, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="435c9-127">In teh **Settings** blade, make sure hello following properties are set are set with hello values below, and then click **OK**.</span></span>
   
    <span data-ttu-id="435c9-128">-**A tárfiók**: *vnetstorage*</span><span class="sxs-lookup"><span data-stu-id="435c9-128">-**Storage account**: *vnetstorage*</span></span>
   
   * <span data-ttu-id="435c9-129">**Hálózati**: *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="435c9-129">**Network**: *TestVNet*</span></span>
   * <span data-ttu-id="435c9-130">**Alhálózati**: *előtér*</span><span class="sxs-lookup"><span data-stu-id="435c9-130">**Subnet**: *FrontEnd*</span></span>
     
     ![Válassza ki a méret panelen](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. <span data-ttu-id="435c9-132">A hello **összegzés** panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="435c9-132">In hello **Summary** blade, click **OK**.</span></span> <span data-ttu-id="435c9-133">Értesítés hello alábbi megjelenített mozaik az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="435c9-133">Notice hello tile below displayed in your dashboard.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-tooretrieve-static-private-ip-address-information-for-a-vm"></a><span data-ttu-id="435c9-135">Hogyan tooretrieve statikus magánhálózati IP-címe címadatok a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="435c9-135">How tooretrieve static private IP address information for a VM</span></span>
<span data-ttu-id="435c9-136">tooview hello statikus magánhálózati IP-címadatok hello hello lépéseket, a létrehozott virtuális gép hello lépésekkel hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="435c9-136">tooview hello static private IP address information for hello VM created with hello steps above, execute hello steps below.</span></span>

1. <span data-ttu-id="435c9-137">Hello Azure Azure portálon kattintson **összes TALLÓZÁSA** > **virtuális gépek** > **DNS01** > **összes beállítások** > **hálózati illesztőt** hello felsorolt egyetlen hálózati adapteren majd.</span><span class="sxs-lookup"><span data-stu-id="435c9-137">From hello Azure Azure portal, click **BROWSE ALL** > **Virtual machines** > **DNS01** > **All settings** > **Network interfaces** and then click on hello only network interface listed.</span></span>
   
    ![Virtuális gép csempe telepítése](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. <span data-ttu-id="435c9-139">A hello **hálózati illesztő** panelen kattintson a **összes beállítás** > **IP-címek** és a hirdetmény hello **hozzárendelés** és **IP-cím** értékeket.</span><span class="sxs-lookup"><span data-stu-id="435c9-139">In hello **Network interface** blade, click **All settings** > **IP addresses** and notice hello **Assignment** and **IP address** values.</span></span>
   
    ![Virtuális gép csempe telepítése](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-tooadd-a-static-private-ip-address-tooan-existing-vm"></a><span data-ttu-id="435c9-141">Hogyan tooadd statikus magánhálózati IP-címe kezelje a meglévő virtuális gép tooan</span><span class="sxs-lookup"><span data-stu-id="435c9-141">How tooadd a static private IP address tooan existing VM</span></span>
<span data-ttu-id="435c9-142">a statikus magánhálózati IP cím toohello hello fenti lépések használatával létrehozott virtuális gép tooadd kövesse hello alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="435c9-142">tooadd a static private IP address toohello VM created using hello steps above, follow hello steps below:</span></span>

1. <span data-ttu-id="435c9-143">A hello **IP-címek** kattintson a panel látható a fenti **statikus** alatt **hozzárendelés**.</span><span class="sxs-lookup"><span data-stu-id="435c9-143">From hello **IP addresses** blade shown above, click **Static** under **Assignment**.</span></span>
2. <span data-ttu-id="435c9-144">Típus *192.168.1.101* a **IP-cím**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="435c9-144">Type *192.168.1.101* for **IP address**, and then click **Save**.</span></span>
   
    ![Virtuális gép létrehozása az Azure-portálon](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> <span data-ttu-id="435c9-146">Ha kattintás után **mentése** bizonyára észrevette, hogy hello hozzárendelés továbbra is túl beállítása**dinamikus**, az azt jelenti, hogy a beírt hello IP-cím már használatban van.</span><span class="sxs-lookup"><span data-stu-id="435c9-146">If after clicking **Save** you notice that hello assignment is still set too**Dynamic**, it means that hello IP address you typed is already in use.</span></span> <span data-ttu-id="435c9-147">Próbálja meg egy másik IP-címet.</span><span class="sxs-lookup"><span data-stu-id="435c9-147">Try a different IP address.</span></span>
> 
> 

## <a name="how-tooremove-a-static-private-ip-address-from-a-vm"></a><span data-ttu-id="435c9-148">Hogyan tooremove egy statikus magánhálózati IP-VM</span><span class="sxs-lookup"><span data-stu-id="435c9-148">How tooremove a static private IP address from a VM</span></span>
<span data-ttu-id="435c9-149">tooremove hello statikus magánhálózati IP-cím a virtuális gép, a fenti létrehozott hello hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="435c9-149">tooremove hello static private IP address from hello VM created above, complete hello following step:</span></span>

<span data-ttu-id="435c9-150">A hello **IP-címek** panelen látható a fenti kattintson **dinamikus** alatt **hozzárendelés**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="435c9-150">From hello **IP addresses** blade shown above, click **Dynamic** under **Assignment**, and then click **Save**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="435c9-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="435c9-151">Next steps</span></span>
* <span data-ttu-id="435c9-152">További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="435c9-152">Learn about [reserved public IP](virtual-networks-reserved-public-ip.md) addresses.</span></span>
* <span data-ttu-id="435c9-153">További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.</span><span class="sxs-lookup"><span data-stu-id="435c9-153">Learn about [instance-level public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) addresses.</span></span>
* <span data-ttu-id="435c9-154">Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span><span class="sxs-lookup"><span data-stu-id="435c9-154">Consult hello [Reserved IP REST APIs](https://msdn.microsoft.com/library/azure/dn722420.aspx).</span></span>


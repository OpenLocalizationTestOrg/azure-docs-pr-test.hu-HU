---
title: "gyors üzembe helyezés – aaaAzure Windows virtuális gép portál létrehozása |} Microsoft Docs"
description: "Azure gyors üzembe helyezés – Windows virtuális gép létrehozása a Portalon"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5646ad51244db6d214c0121d1f7cc45c59f9a78b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-portal"></a><span data-ttu-id="94fa3-103">Hozzon létre egy Windows rendszerű virtuális gép hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="94fa3-103">Create a Windows virtual machine with hello Azure portal</span></span>

<span data-ttu-id="94fa3-104">Az Azure virtuális gépek hello Azure-portálon keresztül is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="94fa3-104">Azure virtual machines can be created through hello Azure portal.</span></span> <span data-ttu-id="94fa3-105">Ez a módszer egy böngészőalapú felhasználói felületet biztosít a virtuális gépek, valamint az összes kapcsolódó erőforrás létrehozásához és konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="94fa3-105">This method provides a browser-based user interface for creating and configuring virtual machines and all related resources.</span></span> <span data-ttu-id="94fa3-106">A gyors üzembe helyezés lépéseit egy virtuális gépet hoz létre, és egy webkiszolgáló hello VM telepíti.</span><span class="sxs-lookup"><span data-stu-id="94fa3-106">This Quickstart steps through creating a virtual machine and installing a webserver on hello VM.</span></span>

<span data-ttu-id="94fa3-107">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="94fa3-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="94fa3-108">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="94fa3-108">Log in tooAzure</span></span>

<span data-ttu-id="94fa3-109">Jelentkezzen be toohello http://portal.azure.com: az Azure portál.</span><span class="sxs-lookup"><span data-stu-id="94fa3-109">Log in toohello Azure portal at http://portal.azure.com.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="94fa3-110">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="94fa3-110">Create virtual machine</span></span>

1. <span data-ttu-id="94fa3-111">Kattintson a hello **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="94fa3-111">Click hello **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

2. <span data-ttu-id="94fa3-112">Válassza a **Számítás**, majd a **Windows Server 2016 Datacenter** elemet.</span><span class="sxs-lookup"><span data-stu-id="94fa3-112">Select **Compute**, and then select **Windows Server 2016 Datacenter**.</span></span> 

3. <span data-ttu-id="94fa3-113">Adja meg a hello virtuális gép adatait.</span><span class="sxs-lookup"><span data-stu-id="94fa3-113">Enter hello virtual machine information.</span></span> <span data-ttu-id="94fa3-114">hello felhasználónevét és jelszavát, az itt megadott használt toolog toohello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="94fa3-114">hello user name and password entered here is used toolog in toohello virtual machine.</span></span> <span data-ttu-id="94fa3-115">Amikor végzett, kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="94fa3-115">When complete, click **OK**.</span></span>

    ![Adja meg a virtuális Gépre vonatkozó alapvető adatok hello portál panel](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. <span data-ttu-id="94fa3-117">Virtuális gép hello kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="94fa3-117">Select a size for hello VM.</span></span> <span data-ttu-id="94fa3-118">toosee további méretét, válasszon **összes** , vagy módosítsa a hello **lemez típusát támogatott** szűrő.</span><span class="sxs-lookup"><span data-stu-id="94fa3-118">toosee more sizes, select **View all** or change hello **Supported disk type** filter.</span></span> 

    ![Képernyőkép a virtuális gépek méreteivel](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. <span data-ttu-id="94fa3-120">A hello-beállítások panelen hello alapértelmezett tartani, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-120">On hello settings blade, keep hello defaults and click **OK**.</span></span>

6. <span data-ttu-id="94fa3-121">Hello összefoglaló lapon kattintson **Ok** toostart hello virtuálisgép-telepítést.</span><span class="sxs-lookup"><span data-stu-id="94fa3-121">On hello summary page, click **Ok** toostart hello virtual machine deployment.</span></span>

7. <span data-ttu-id="94fa3-122">virtuális gép hello Azure portál Irányítópultjára rögzített toohello lesz.</span><span class="sxs-lookup"><span data-stu-id="94fa3-122">hello VM will be pinned toohello Azure portal dashboard.</span></span> <span data-ttu-id="94fa3-123">Hello központi telepítés befejezése után automatikusan megnyílik hello VM összefoglaló panel.</span><span class="sxs-lookup"><span data-stu-id="94fa3-123">Once hello deployment has completed, hello VM summary blade automatically opens.</span></span>


## <a name="connect-toovirtual-machine"></a><span data-ttu-id="94fa3-124">Csatlakoztassa a gépet toovirtual</span><span class="sxs-lookup"><span data-stu-id="94fa3-124">Connect toovirtual machine</span></span>

<span data-ttu-id="94fa3-125">Távoli asztali kapcsolat toohello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="94fa3-125">Create a remote desktop connection toohello virtual machine.</span></span>

1. <span data-ttu-id="94fa3-126">Kattintson a hello **Connect** hello virtuális gép Tulajdonságok gombra.</span><span class="sxs-lookup"><span data-stu-id="94fa3-126">Click hello **Connect** button on hello virtual machine properties.</span></span> <span data-ttu-id="94fa3-127">A rendszer létrehoz és letölt egy Remote Desktop Protocol-fájlt (.rdp-fájlt).</span><span class="sxs-lookup"><span data-stu-id="94fa3-127">A Remote Desktop Protocol file (.rdp file) is created and downloaded.</span></span>

    ![Portál – 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. <span data-ttu-id="94fa3-129">virtuális gép, nyissa meg hello tooconnect tooyour letöltött RDP-fájlt.</span><span class="sxs-lookup"><span data-stu-id="94fa3-129">tooconnect tooyour VM, open hello downloaded RDP file.</span></span> <span data-ttu-id="94fa3-130">Ha a rendszer kéri, akkor kattintson a **Csatlakozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="94fa3-130">If prompted, click **Connect**.</span></span> <span data-ttu-id="94fa3-131">A Mac, ez például RDP-ügyfelet kell [távoli asztali ügyfél](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) hello Mac App Store áruházból származó.</span><span class="sxs-lookup"><span data-stu-id="94fa3-131">On a Mac, you need an RDP client such as this [Remote Desktop Client](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) from hello Mac App Store.</span></span>

3. <span data-ttu-id="94fa3-132">Adja meg a hello felhasználónevet és jelszót hello virtuális gép létrehozásakor megadott, majd kattintson a **Ok**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-132">Enter hello user name and password you specified when creating hello virtual machine, then click **Ok**.</span></span>

4. <span data-ttu-id="94fa3-133">Hello bejelentkezési folyamat során a tanúsítványra vonatkozó figyelmeztetést is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="94fa3-133">You may receive a certificate warning during hello sign-in process.</span></span> <span data-ttu-id="94fa3-134">Kattintson a **Igen** vagy **Folytatás** tooproceed hello kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="94fa3-134">Click **Yes** or **Continue** tooproceed with hello connection.</span></span>


## <a name="install-iis-using-powershell"></a><span data-ttu-id="94fa3-135">Az IIS telepítése a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="94fa3-135">Install IIS using PowerShell</span></span>

<span data-ttu-id="94fa3-136">Hello virtuális gépen indítson el egy PowerShell-munkamenetet, és futtassa a következő parancs tooinstall IIS hello.</span><span class="sxs-lookup"><span data-stu-id="94fa3-136">On hello virtual machine, start a PowerShell session and run hello following command tooinstall IIS.</span></span>

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

<span data-ttu-id="94fa3-137">Lépjen ki a hello RDP-munkamenetet, és adja vissza hello virtuális gép tulajdonságok hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="94fa3-137">When done, exit hello RDP session and return hello VM properties in hello Azure portal.</span></span>

## <a name="open-port-80-for-web-traffic"></a><span data-ttu-id="94fa3-138">A 80-as port megnyitása a webes adatforgalom számára</span><span class="sxs-lookup"><span data-stu-id="94fa3-138">Open port 80 for web traffic</span></span> 

<span data-ttu-id="94fa3-139">A hálózati biztonsági csoport (NSG) feladata a bejövő és kimenő forgalom védelme.</span><span class="sxs-lookup"><span data-stu-id="94fa3-139">A Network security group (NSG) secures inbound and outbound traffic.</span></span> <span data-ttu-id="94fa3-140">Ha egy virtuális gép létrehozása az Azure-portálon hello, egy bejövő forgalomra vonatkozó szabály a 3389-es porton az RDP-kapcsolatok jön létre.</span><span class="sxs-lookup"><span data-stu-id="94fa3-140">When a VM is created from hello Azure portal, an inbound rule is created on port 3389 for RDP connections.</span></span> <span data-ttu-id="94fa3-141">A virtuális gép egy webkiszolgáló üzemelteti, mert egy NSG-szabály a 80-as port számára létrehozott toobe kell.</span><span class="sxs-lookup"><span data-stu-id="94fa3-141">Because this VM hosts a webserver, an NSG rule needs toobe created for port 80.</span></span>

1. <span data-ttu-id="94fa3-142">Hello virtuális gépre, kattintson a hello hello neve **erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-142">On hello virtual machine, click hello name of hello **Resource group**.</span></span>
2. <span data-ttu-id="94fa3-143">Jelölje be hello **hálózati biztonsági csoport**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-143">Select hello **network security group**.</span></span> <span data-ttu-id="94fa3-144">hello NSG használatával azonosíthatók hello **típus** oszlop.</span><span class="sxs-lookup"><span data-stu-id="94fa3-144">hello NSG can be identified using hello **Type** column.</span></span> 
3. <span data-ttu-id="94fa3-145">Kattintson a bal oldali menüben hello, a beállítások **bejövő biztonsági szabályok**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-145">On hello left-hand menu, under settings, click **Inbound security rules**.</span></span>
4. <span data-ttu-id="94fa3-146">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="94fa3-146">Click on **Add**.</span></span>
5. <span data-ttu-id="94fa3-147">A **Név** mezőbe írja be a **http** karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="94fa3-147">In **Name**, type **http**.</span></span> <span data-ttu-id="94fa3-148">Győződjön meg arról, hogy **porttartomány** too80 beállítása és **művelet** értéke túl**engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-148">Make sure **Port range** is set too80 and **Action** is set too**Allow**.</span></span> 
6. <span data-ttu-id="94fa3-149">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="94fa3-149">Click **OK**.</span></span>


## <a name="view-hello-iis-welcome-page"></a><span data-ttu-id="94fa3-150">Nézet hello IIS-kezdőlap</span><span class="sxs-lookup"><span data-stu-id="94fa3-150">View hello IIS welcome page</span></span>

<span data-ttu-id="94fa3-151">Az IIS-kiszolgálón telepített, és a port 80 nyissa meg a virtuális gép tooyour, hello webkiszolgáló most már elérhető az hello internet.</span><span class="sxs-lookup"><span data-stu-id="94fa3-151">With IIS installed, and port 80 open tooyour VM, hello webserver can now be accessed from hello internet.</span></span> <span data-ttu-id="94fa3-152">Nyisson meg egy webböngészőt, és adja meg a virtuális gép hello hello nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="94fa3-152">Open a web browser, and enter hello public IP address of hello VM.</span></span> <span data-ttu-id="94fa3-153">hello VM panel az Azure-portálon hello hello nyilvános IP-cím található.</span><span class="sxs-lookup"><span data-stu-id="94fa3-153">hello public IP address can be found on hello VM blade in hello Azure portal.</span></span>

![Alapértelmezett IIS-webhely](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a><span data-ttu-id="94fa3-155">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="94fa3-155">Clean up resources</span></span>

<span data-ttu-id="94fa3-156">Ha már nincs szükség, törölje a hello erőforráscsoport, a virtuális gép és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="94fa3-156">When no longer needed, delete hello resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="94fa3-157">toodo Igen, jelölje ki hello erőforráscsoport hello virtuális gépek paneljét, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="94fa3-157">toodo so, select hello resource group from hello virtual machine blade and click **Delete**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94fa3-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94fa3-158">Next steps</span></span>

<span data-ttu-id="94fa3-159">Ebben a rövid útmutatóban üzembe helyezett egy egyszerű virtuális gépet, egy hálózati biztonsági csoport szabályát, valamint telepített egy webkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="94fa3-159">In this quick start, you’ve deployed a simple virtual machine, a network security group rule, and installed a web server.</span></span> <span data-ttu-id="94fa3-160">További információ az Azure virtuális gépeken, toolearn toohello oktatóanyag Windows virtuális gépek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="94fa3-160">toolearn more about Azure virtual machines, continue toohello tutorial for Windows VMs.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="94fa3-161">Windowsos virtuális gépek az Azure-ban – oktatóanyagok</span><span class="sxs-lookup"><span data-stu-id="94fa3-161">Azure Windows virtual machine tutorials</span></span>](./tutorial-manage-vm.md)
